# Ensaio de conservação pós-colheita

Aplicação agrícola do sistema. Revestimento comestível aplicado por imersão com velocidade
de retirada controlada, e acompanhamento da fruta revestida ao longo dos dias numa câmara
instrumentada.

## Por que a máquina precisa existir

Mergulhar fruta na mão dá espessura de filme diferente a cada amostra, e a espessura do
filme escala com a velocidade de retirada elevada a 2/3 (Landau-Levich). Em experimento de
pós-colheita a espessura é a variável que se quer controlar e normalmente não se controla.
A máquina fixa essa variável. É a resposta para "por que não faz na mão".

## Variáveis

**Independentes (o que se controla)**
- Velocidade de retirada, mm/min. É o fator principal
- Formulação do revestimento: base amido ou base gelatina, e concentração
- Número de imersões
- Temperatura e umidade de estocagem

**Dependentes (o que se mede)**
- Perda de massa acumulada, contínua pela célula de carga. Métrica primária
- Taxa respiratória, via acúmulo de CO2 na câmara fechada, se houver sensor
- Cor e deterioração visível, por foto diária em posição e iluminação fixas
- Firmeza, se houver penetrômetro no laboratório. Caso contrário, descartar

**Controle**
- Fruta sem revestimento, mesmo lote, mesma câmara

## Sensores

| Grandeza | Sensor | Estimativa | Prioridade |
|---|---|---|---|
| Massa | Célula de carga 1 kg + HX711 | R$ 30-45 | essencial |
| Temperatura e umidade | DHT22 (ou SHT31, mais preciso) | R$ 25-40 | essencial |
| CO2 | MH-Z19B NDIR | R$ 150-200 | eleva muito, mas é opcional |
| Imagem | Celular em suporte fixo | R$ 0 | essencial |

A célula de carga é o item de melhor retorno: transforma seis pesagens manuais por fruta em
milhares de amostras de série temporal. É o que torna o dataset próprio viável.

O sensor de CO2 é o único caro, e é o que dá fundamento fisiológico ao trabalho, porque o
mecanismo do revestimento é justamente modificar a troca gasosa. **Perguntar antes de
comprar**: a Célula IoT, o LESC ou o laboratório de alimentos podem ter um.

## Câmara

Caixa de isopor com os sensores dentro. Temperatura estável, custo baixo, e vira parte do
sistema em vez de problema. Uma câmara por tratamento, ou uma câmara com as frutas
identificadas e uma célula de carga por posição.

## Calendário

Ensaio de vida de prateleira leva de uma a três semanas por lote. De setembro a dezembro dá
para quatro a seis lotes. Consequência prática:

**O lote 1 é mergulhado à mão, em setembro, antes da máquina ficar pronta.** Isso destrava
a coleta de dados e as fases iniciais do Projeto Integrador sem depender do hardware. Os
lotes seguintes usam a máquina e aí entra a comparação de reprodutibilidade, que por si só
já é resultado.

## Fruta

Escolher entre manga, melão ou banana. Critérios: disponível no Ceará o ano todo, custo
baixo, e janela de amadurecimento compatível com o calendário. Banana é a mais rápida e a
mais barata, então é a melhor para o lote 1.

## Formulações

O laboratório não fornece os insumos, mas todos são adquiríveis por fora. As professoras
liberaram formulação alternativa para protótipo, o que garante o piso; a quitosana entra como
alvo principal porque é o padrão da literatura e tem ação antifúngica.

| Formulação | Insumo | Onde | Estimativa |
|---|---|---|---|
| Quitosana | quitosana em pó + ácido acético + glicerina | **Polymar, Fortaleza** | amostra, ou R$ 40-80 |
| Alginato | alginato de sódio + cloreto de cálcio | gastronomia molecular | R$ 50-65 |
| Base amido | amido de milho gelatinizado + glicerina | supermercado e farmácia | R$ 10 |
| Base gelatina | gelatina incolor sem sabor + glicerina | supermercado | R$ 5 |
| Base lipídica | cera de carnaúba em grau alimentício | a confirmar | a confirmar |

A [Polymar](https://www.polymar.com.br/) fabrica quitosana em Fortaleza, na Rua Manoel Arruda
980, Barroso, a partir de carapaça de crustáceo da pesca cearense. Pedir amostra é o primeiro
movimento, porque é o item de maior prazo de resposta. Saída imediata se travar: quitosana em
cápsula de suplemento, R$ 40 por 60 g de pó grau alimentício.

A cera de carnaúba é a de maior interesse porque consta da lista de lipídios da literatura e
é produto do Ceará. Só entra se aparecer em grau alimentício; a de uso em polimento costuma
levar solvente e está fora.

**Ressalva a registrar no relatório:** amido e gelatina não têm a ação antifúngica da
quitosana. A métrica principal continua sendo perda de massa, que responde a qualquer filme
barreira. Se a quitosana entrar, a comparação entre ela e as formulações sem ação antifúngica
vira um resultado a mais, em vez de uma limitação.

## Massa de filme depositado

Para validar Landau-Levich sem depender da fruta: revestir lâminas de vidro em velocidades
diferentes, pesar antes e depois, e levantar a curva de massa depositada contra velocidade.

O filme numa lâmina pesa dezenas de miligramas, o que **não** é resolvível pela célula de
carga de 1 kg da câmara. Usar uma balança de precisão de 0,01 g, que custa R$ 25 a 40.
Esse ensaio é barato, rápido, independe do calendário biológico e transforma a premissa
teórica em resultado medido.
