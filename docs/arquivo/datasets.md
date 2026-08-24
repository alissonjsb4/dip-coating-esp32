# Bases de dados

Duas fontes: uma pública, que destrava as fases iniciais sem depender de hardware, e a nossa,
gerada pela câmara.

## Pública, escolhida

**Multi-Parameter Dataset for Machine Learning Based Fruit Spoilage Prediction in an
IoT-Enabled Cold Storage System**
Nshekanabo Marius e Stanley Mugisha, Soroti University, 29/09/2025.
[data.mendeley.com/datasets/czz68d9fwj/1](https://data.mendeley.com/datasets/czz68d9fwj/1)
DOI 10.17632/czz68d9fwj.1 · Licença CC BY 4.0

| Coluna | Tipo |
|---|---|
| Fruit | categórica (laranja, abacaxi, banana, tomate) |
| Temperatura | °C |
| Umidade | % |
| Luminosidade | lux |
| CO2 | ppm |
| Class | binária, Good ou Bad. **Alvo** |

**10.996 linhas.**

**Por que essa:** é praticamente o nosso problema. Armazenamento refrigerado instrumentado
por IoT, prevendo deterioração a partir de variáveis ambientais. Inclui banana, que é a fruta
do lote 1. E as features batem com os sensores que a gente vai ter, com uma exceção.

## A exceção do CO2, e o que fazer com ela

O dataset tem CO2 e a gente decidiu não comprar o sensor, que custa mais que todos os outros
somados.

Isso não é problema, é a análise. O plano:

1. Treinar o modelo com todas as features. Baseline.
2. Treinar sem a coluna de CO2. **Ablação.**
3. Medir a queda de performance.
4. Deployar o modelo reduzido, que é o que o hardware consegue alimentar.

Isso responde diretamente a pergunta que a disciplina coloca no material: a questão de
engenharia não é maximizar acurácia, é qual modelo é adequado para rodar num dispositivo IoT
limitado. Quantificar o custo de abrir mão de um sensor caro é resultado, não desculpa.

Se aparecer um MH-Z19B emprestado, a ablação continua valendo e vira comparação com dado real.

## Nossa, gerada pela câmara

O dataset público prevê deterioração a partir do **ambiente**. O nosso acrescenta duas coisas
que ele não tem:

- **Perda de massa contínua**, pela célula de carga
- **O tratamento**: qual revestimento, qual velocidade de retirada, quantas imersões

Ou seja, o público responde "essas condições estragam a fruta?". O nosso responde "esse
revestimento, aplicado nessa velocidade, adia a deterioração?". É extensão, não cópia.

Estrutura por amostra:

| Campo | Origem |
|---|---|
| id da fruta, lote, tratamento, controle | anotação |
| revestimento e concentração | anotação |
| velocidade de retirada, mm/min | telemetria do nó 1 |
| número de imersões, tempo de imersão | telemetria do nó 1 |
| massa, série temporal | célula de carga |
| temperatura, umidade, luz, série temporal | nó 2 |
| foto diária | celular |
| rótulo de deterioração por dia | anotação visual |

## Imagem, se for pelo caminho visual

Se o time quiser classificação de deterioração por foto em vez de tabular:

- [Banana Ripeness Classification Dataset](https://www.kaggle.com/datasets/shahriar26s/banana-ripeness-classification-dataset) — 13 mil imagens
- [Fruit Ripeness: Unripe, Ripe, and Rotten](https://www.kaggle.com/datasets/leftin/fruit-ripeness-unripe-ripe-and-rotten)
- [Fresh and Rotten Fruits Dataset](https://data.mendeley.com/datasets/bdd69gyhv8/1) — Mendeley, 16 classes

**Ressalva:** modelo de imagem é mais pesado e o material da disciplina insiste em modelo que
rode em dispositivo limitado. Tabular resolve com random forest e cabe no ESP32. Imagem
empurra para a nuvem. Se for usar imagem, que seja complemento e não a espinha dorsal.
