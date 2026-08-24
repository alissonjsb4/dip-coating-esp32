# Sensores: o que cada um mede e por que ele está aqui

Nenhum sensor entra por ser legal. Cada um responde uma pergunta do experimento.

## Nó 2, a câmara

### Célula de carga 5 kg + HX711 — massa

**Mede:** a massa da fruta, continuamente.

**Por que:** perda de massa é a **métrica primária** de conservação pós-colheita. Fruta perde
água por transpiração através da casca, e o revestimento comestível existe justamente para
frear isso. Se o revestimento funciona, a curva de perda de massa da fruta revestida fica
abaixo da curva do controle. É o resultado central do trabalho.

**Por que não pesar na mão:** pesagem diária dá seis pontos por fruta em uma semana. A célula
de carga amostrando de minuto em minuto dá milhares. É a diferença entre ter e não ter série
temporal para treinar modelo. **É o item de melhor retorno do projeto inteiro.**

**Como funciona:** a célula de carga é uma barra de alumínio com extensômetros colados,
ligados em ponte de Wheatstone. Deformar a barra desbalanceia a ponte e gera uma diferença de
tensão de poucos milivolts. O HX711 é um conversor A/D de 24 bits feito para esse sinal, com
amplificador embutido. Fala com o ESP32 por dois fios (clock e dados).

**Cuidados:** precisa de calibração com massa conhecida, e deriva com temperatura. Como a
câmara também mede temperatura, dá para corrigir. Montar a célula fixa numa base rígida,
senão vibração vira ruído.

**Estimativa:** R$ 30-45

### DHT22 (ou SHT31) — temperatura e umidade relativa

**Mede:** temperatura e umidade do ar dentro da câmara.

**Por que:** as duas são **variáveis de confusão**. Fruta perde mais massa em ar seco e
estraga mais rápido em ar quente. Sem medir, não dá para saber se a diferença entre dois
tratamentos veio do revestimento ou de a câmara ter esquentado. Medir é o que permite afirmar
que o efeito é do revestimento.

Também são duas das features do dataset público que a gente vai usar.

**DHT22 contra SHT31:** o DHT22 é mais barato e suficiente. O SHT31 é mais preciso, fala I2C
e não trava. Se o orçamento deixar, SHT31.

**Estimativa:** R$ 25-40

### BH1750 — luz

**Mede:** intensidade luminosa em lux.

**Por que:** luz acelera amadurecimento e degrada pigmento. Mas o motivo principal aqui é
outro: **luminosidade é uma das features do dataset público** que a gente escolheu, e custa
R$ 15. Ter o sensor evita ter que descartar essa coluna na hora de levar o modelo para o
hardware.

**Estimativa:** R$ 15

### MH-Z19B — CO2 (opcional)

**Mede:** concentração de CO2 dentro da câmara fechada.

**Por que seria bom:** o acúmulo de CO2 numa câmara fechada é proxy direto da **taxa
respiratória** da fruta. E respiração é exatamente o que o revestimento modifica: o filme
funciona como barreira à troca gasosa. Ou seja, o CO2 mede o mecanismo, e não só o efeito.

**Por que é opcional:** custa R$ 150-200, mais que todos os outros somados.

**Decisão tomada:** ficar sem, e transformar isso em método. Treina o modelo com e sem a
coluna de CO2, mede a queda de performance e deploya o modelo reduzido. Ver `datasets.md`.

**Antes de comprar:** perguntar na Célula IoT, no LESC e no laboratório de alimentos se
alguém tem um emprestado.

**Estimativa:** R$ 150-200

### Câmera — celular em suporte fixo

**Mede:** cor e deterioração visível.

**Por que:** manchas, escurecimento e mofo não aparecem na balança. Foto diária, sempre na
mesma posição e com a mesma iluminação, dá rótulo temporal de graça e serve de referência
visual para o dado numérico.

**Por que celular e não ESP32-CAM:** qualidade muito melhor, custo zero, e a foto é uma por
dia, então automatizar não compensa.

## Nó 1, a máquina

### Chave de fim de curso

**Mede:** se o carro chegou ao topo do curso.

**Por que:** referência de origem e segurança. Sem ela o motor tenta passar do fim mecânico e
perde passo ou força a estrutura.

**Ligar em NC, normalmente fechada.** Assim um fio partido é lido como acionada e a máquina
para, em vez de falhar em silêncio na hora de parar. Numa peça que se move, fio rompido é o
modo de falha realista.

### Encoder EC11 e LCD ST7920

**Para quê:** operar a máquina sem PC. O operador ajusta velocidade, tempo de imersão e número
de ciclos girando o encoder e lendo o LCD. Requisito de uso real em laboratório, onde ninguém
quer abrir terminal para revestir uma fruta.

### Telemetria do motor

Não é sensor físico, é dado que o firmware publica: velocidade comandada, posição em passos,
estado do ciclo. Serve para provar que a velocidade de retirada foi a que se pediu, que é o
fundamento inteiro do trabalho.

## Resumo de custo

| Item | Estimativa | Prioridade |
|---|---|---|
| Célula de carga 5 kg + HX711 | R$ 30-45 | essencial |
| DHT22 | R$ 25-40 | essencial |
| BH1750 | R$ 15 | recomendado |
| Chave de fim de curso | R$ 5-10 | essencial |
| Celular como câmera | R$ 0 | essencial |
| MH-Z19B (CO2) | R$ 150-200 | opcional |

Essencial mais recomendado: **R$ 75 a 110**. Tudo disponível presencialmente em Fortaleza.
