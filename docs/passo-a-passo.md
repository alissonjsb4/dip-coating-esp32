# Guia de montagem, passo a passo

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

## Resumo da ordem

| Fase | Depende de | Prazo |
|---|---|---|
| 0 Compras e medidas | nada | primeira semana |
| 2 Bancada do motor | driver chegar | uma tarde |
| 3 Mecânica | Fase 2 aprovar | duas semanas |
| 4 Interface | Fase 3 | uma semana |

A Fase 2 é o ponto de decisão. Nada de mecânica antes dela aprovar.

O ensaio de massa de filme em lâmina de vidro, que levanta a curva de espessura contra
velocidade, está descrito em `arquivo/pos-colheita.md`.
