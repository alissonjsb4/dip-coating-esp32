# Guia de montagem, passo a passo

Ordem deliberada: **a câmara vem antes da máquina.** A câmara fica pronta numa tarde e começa
a coletar dado em setembro, com o lote mergulhado à mão. A máquina leva semanas. Quem
inverter a ordem chega em dezembro com dois lotes em vez de cinco.

Ler `montagem.md` antes de encostar em fio.

---

## Fase 0 — Antes de comprar

1. Levantar o estoque do Clube do Hardware. Resistor, capacitor, placa perfurada, barra de
   pinos, borne e jacaré costumam estar em gaveta.
2. Abrir as propostas de orçamento de 11/08 (AutoCore, Smart Kits, Mercado Livre) e ver o que
   já está cotado ou comprado.
3. Medir com paquímetro: furação do atuador, curso útil, flange e eixo do motor, placa do LCD.
   As peças impressas saem dessas medidas, não do modelo antigo.
4. Confirmar o **passo do fuso** do atuador. Define passos por milímetro e todo o ensaio de
   velocidade. Se for TR8x8, são 8 mm por volta.
5. Contatar a Polymar sobre amostra de quitosana. É o item com maior prazo de resposta, então
   é o primeiro a disparar.

---

## Fase 1 — Câmara (uma tarde)

### 1.1 Suporte da célula de carga

Imprimir duas chapinhas e parafusar a célula em balanço entre elas: a base fixa embaixo, o
prato da fruta em cima. É o padrão de montagem de célula de carga tipo barra. A célula precisa
poder fletir, então nenhuma das pontas pode encostar em nada além dos parafusos.

Base rígida, presa no fundo da caixa. Vibração vira ruído.

### 1.2 Ligação

Célula de carga no HX711 pelos quatro fios coloridos, seguindo a serigrafia do módulo.
HX711 no ESP32: VCC em 3,3 V, GND, DT e SCK em dois GPIO livres. DHT22 e BH1750 em seguida,
o BH1750 no barramento I2C.

Tudo em placa perfurada soldada, com barra de pinos fêmea para o ESP32. Nada de protoboard,
porque a câmara vai ficar ligada semanas.

### 1.3 Firmware

Config ESPHome com `hx711`, `dht` e `bh1750`, publicando por MQTT. Intervalo de amostragem de
1 minuto é suficiente e não enche o banco à toa.

### 1.4 Calibração da célula

1. Com o prato vazio, zerar (tara).
2. Pôr uma massa conhecida. Moeda serve: moeda de 1 real pesa 7 g, a de 50 centavos pesa 7,81 g.
   Melhor ainda é usar a balança de precisão para pesar um objeto e usá-lo como padrão.
3. Ajustar o fator de calibração até a leitura bater.
4. Repetir com uma segunda massa para conferir linearidade.

### 1.5 Ensaio em branco

Antes de pôr fruta, deixar a câmara rodando 48 horas com um objeto inerte de massa parecida,
tipo um pote com água fechado. Isso mede a **deriva** do sistema: quanto a leitura muda sozinha
por causa de temperatura e do próprio sensor.

Sem esse branco, você não sabe separar perda de massa da fruta de deriva do instrumento. É a
etapa que a maioria pula e depois não consegue defender o resultado.

### 1.6 Posição da foto

Marcar com fita no chão e na parede onde o celular fica. Mesma distância, mesmo ângulo, mesma
iluminação, toda vez. Foto diária.

---

## Fase 2 — Bancada do motor

Não precisa da mecânica montada. Motor solto na mesa.

### 2.1 Ligação

1. Soldar barra de pinos fêmea na placa perfurada para o ESP32 e para o A4988.
2. **Pull-up de 10 kΩ entre EN e VCC.** O EN é ativo em nível baixo e o GPIO fica em alta
   impedância no boot; sem o resistor o motor pode dar um tranco antes do firmware subir.
3. Capacitor de 100 µF entre VMOT e GND, pernas curtas, junto do driver.
4. Bobinas do motor em borne a parafuso. Nunca em protoboard.
5. VMOT direto da fonte. Terra comum entre fonte e ESP32.
6. STEP, DIR, SLEEP e EN nos GPIO 26, 27, 25 e 33.

### 2.2 Ajuste do Vref

Com a fonte ligada e o motor parado, ajustar o trimpot do driver.

`I_max = Vref / (8 × Rs)`. Conferir o Rs impresso na placa, que costuma ser 0,1 Ω ou 0,05 Ω.
Com Rs = 0,1 Ω, `Vref = 0,8 × I`. Começar em 0,5 A, ou seja, Vref = 0,4 V.

Dissipador colado no driver.

**Nunca desconectar o motor com o driver energizado.** É a causa número um de A4988 queimado.

### 2.3 Ensaio de velocidade

`esphome run firmware/bancada-motor.yaml` e seguir `ensaio-velocidade.md`.

Este é o ponto de decisão do projeto. Se a velocidade real não seguir a comandada na taxa
necessária, muda o micropasso ou muda o firmware. Descobrir isso agora custa uma tarde;
descobrir em outubro custa o semestre.

---

## Fase 3 — Mecânica

Só depois do ensaio de velocidade aprovar.

1. Modelar as peças novas a partir das medidas da Fase 0. São quatro ou cinco, todas
   parafusadas no perfil com T-nut: bandeja de eletrônica com acesso ao USB, suporte do LCD,
   suporte da amostra no gantry plate, base do recipiente e suporte do fim de curso.
2. Imprimir em PLA, exceto o que ficar perto do béquer, que vai em PETG.
3. Montar o motor no atuador, acoplar o fuso.
4. Instalar a chave de fim de curso no topo do curso, **ligada em NC**.
5. Amarrar cabo no perfil com folga de serviço no carro.

Regra: **nada de eletrônica acima do recipiente.** Vapor e respingo.

### 3.1 Calibração de passos por milímetro

Marcar o carro, comandar um número conhecido de passos, medir o deslocamento com régua.
Repetir três vezes. Esse número converte passos em mm/min e é o que valida a especificação de
1 a 200 mm/min.

---

## Fase 4 — Interface

1. Soldar barra de pinos macho 2x5 em pedaço de placa perfurada e abrir os sinais de EXP1 e
   EXP2 do LCD. Os conectores são IDC e não aceitam jumper solto.
2. Ligar o LCD no VSPI: SCLK no 18, MOSI no 23, CS no 22. Encoder nos GPIO 32, 21 e 19.
3. **Testar o LCD antes de confiar.** A placa é de 5 V e o ESP32 é de 3,3 V. Funciona na
   maioria dos relatos, mas se não responder entra level shifter.
4. Config ESPHome com `display/st7920`, `sensor/rotary_encoder` e `display_menu`.
5. Máquina de estados do ciclo em `script`: desce, imerge pelo tempo configurado, sobe na
   velocidade configurada, aguarda o tempo suspenso, repete N vezes. Estado de emergência
   monitorando o fim de curso.

---

## Fase 5 — Revestimento e ensaio

### 5.1 Preparo das soluções

**Quitosana.** 1 a 2 g de quitosana em pó para 100 ml de solução de ácido acético a 1%
(vinagre branco diluído 1:4 em água serve). Agitar até dissolver, o que leva tempo. Adicionar
glicerina a 0,5%, como plastificante. Filtrar em pano fino ou peneira.

**Alginato.** 1 a 2 g de alginato para 100 ml de água. Aplicar e depois borrifar solução de
cloreto de cálcio a 1% para reticular.

**Amido.** 2 a 3 g de amido de milho para 100 ml de água, aquecer até gelatinizar, deixar
esfriar. Glicerina a 0,5%.

**Gelatina.** Conforme a embalagem, mais glicerina a 0,5%.

Preparar sempre no mesmo dia do uso, e registrar concentração e lote.

### 5.2 Curva de massa de filme

Antes da fruta, com lâmina de vidro:

1. Pesar a lâmina limpa e seca na balança de precisão.
2. Revestir na máquina, com velocidade conhecida.
3. Secar completamente e pesar de novo.
4. Repetir para 10, 25, 50, 100 e 200 mm/min, três lâminas por velocidade.

A curva de massa depositada contra velocidade valida Landau-Levich, que é a premissa que
sustenta o projeto inteiro. Não depende de fruta nem de calendário biológico, e sai numa tarde.

### 5.3 Ensaio de prateleira

1. Comprar fruta do mesmo lote, mesmo estágio de maturação, tamanho parecido.
2. Lavar, secar, pesar e etiquetar cada uma.
3. Separar em grupos: controle sem revestimento, e um grupo por combinação de formulação e
   velocidade.
4. Revestir, secar e colocar na câmara. Uma fruta fica na célula de carga; as outras são
   pesadas na mão uma vez por dia.
5. Foto diária, na posição marcada.
6. Registrar tudo numa planilha com identificador de fruta, lote e tratamento.

O **lote 1 vai mergulhado à mão**, em setembro, antes da máquina. Ele é ao mesmo tempo o
destravamento da coleta e o termo de comparação de variância contra os lotes automatizados.

---

## Resumo da ordem

| Fase | Depende de | Prazo |
|---|---|---|
| 0 Compras e medidas | nada | primeira semana |
| 1 Câmara | célula de carga chegar | uma tarde |
| 2 Bancada do motor | driver chegar | uma tarde |
| 3 Mecânica | Fase 2 aprovar | duas semanas |
| 4 Interface | Fase 3 | uma semana |
| 5 Ensaios | Fase 1 para o lote 1 | contínuo desde setembro |

Fases 1 e 2 são independentes e podem correr em paralelo, com pessoas diferentes.
