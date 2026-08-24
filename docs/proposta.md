# Proposta de projeto

**Título.** Sistema IoT para aplicação controlada de revestimento comestível e monitoramento
de conservação pós-colheita

**Disciplinas.** Internet das Coisas (2026.2) e TI0179 Projeto Integrado em Inteligência
Computacional e IoT
**Instituição.** Departamento de Engenharia de Teleinformática, UFC
**Data.** Agosto de 2026

## 1. Problema

Perda pós-colheita é a fração da produção agrícola que se deteriora entre a colheita e o
consumo. No Ceará o problema tem peso econômico direto, porque a região produz e exporta
manga, melão e banana.

Uma das técnicas consagradas para atrasar a deterioração é a aplicação de **revestimento
comestível** sobre a casca, com polissacarídeos como quitosana e alginato. O filme atua como
barreira: reduz a transpiração, portanto a perda de água, e modifica a troca gasosa,
portanto a taxa respiratória e a velocidade de amadurecimento.

O método padrão de aplicação em bancada é a imersão, conhecida como dip coating.

## 2. Lacuna

Na prática laboratorial a imersão é feita manualmente. Isso deixa livre uma variável que
determina o resultado: a **velocidade de retirada**.

Pela relação de Landau-Levich, a espessura do filme depositado escala com a velocidade de
retirada elevada a 2/3. Retirada manual produz velocidade diferente a cada amostra, logo
espessura diferente, logo variância que se soma ao efeito do tratamento e o mascara.

Equipamentos comerciais de dip coating com velocidade controlada existem, mas custam alguns
milhares de reais, o que os torna inacessíveis para laboratório de graduação.

## 3. Objetivo geral

Desenvolver um sistema IoT de baixo custo que aplique revestimento comestível com velocidade
de retirada controlada e acompanhe, por sensoriamento contínuo, a conservação da fruta
tratada, empregando um modelo de inteligência computacional para prever deterioração.

## 4. Objetivos específicos

1. Construir um eixo motorizado de imersão com velocidade programável de 1 a 200 mm/min,
   com tempo de imersão e número de ciclos configuráveis
2. Verificar experimentalmente a fidelidade entre velocidade comandada e velocidade real
3. Instrumentar uma câmara de armazenamento com medição contínua de massa, temperatura,
   umidade e luminosidade
4. Integrar os dois nós por MQTT, com persistência e painel de acompanhamento
5. Treinar e validar um modelo de inteligência computacional para previsão de deterioração,
   a partir de base pública e da base própria
6. Avaliar o custo de performance da ausência do sensor de CO2, para viabilizar a implantação
   em hardware restrito
7. Comparar a variância de perda de massa entre lotes revestidos manualmente e pela máquina

## 5. Justificativa

O sistema ataca a lacuna descrita fixando a variável solta, e o faz com hardware de
prateleira. O custo estimado de sensoriamento adicional fica abaixo de R$ 120, contra
milhares de reais de um equipamento comercial.

Do ponto de vista das disciplinas, o projeto percorre a cadeia completa que o Projeto
Integrado propõe: parte de um problema real e aberto, define um conjunto de dados, constrói e
valida um modelo, projeta a arquitetura IoT, ergue o protótipo físico e valida a integração.

## 6. Arquitetura

Dois nós ESP32 independentes, ambos rodando ESPHome e publicando em um broker MQTT comum.

**Nó 1, máquina de revestimento.** ESP32-DevKitC V4, driver A4988, motor de passo NEMA 17
sobre atuador linear, interface local com display ST7920 e encoder rotativo, chave de fim de
curso em contato normalmente fechado. Publica velocidade comandada, posição e estado do ciclo.

**Nó 2, câmara de armazenamento.** ESP32 WROOM-32, célula de carga de 5 kg com HX711,
sensor de temperatura e umidade, sensor de luminosidade. Publica série temporal contínua.

A separação em dois nós é deliberada: a câmara opera por semanas ininterruptas enquanto a
máquina é usada por minutos, e um nó único faria a regravação de firmware interromper ensaios
em andamento.

## 7. Metodologia

**Etapa 1, verificação do acionamento.** Ensaio de bancada comparando velocidade comandada e
velocidade medida, em varredura de taxa de passo e nos regimes de passo cheio, 1/8 e 1/16. O
resultado define o micropasso de operação e valida ou substitui o firmware.

**Etapa 2, modelagem inicial.** Exploração e modelagem sobre base pública, com definição de
linha de base, seleção de modelo e análise de erro.

**Etapa 3, protótipo.** Montagem dos dois nós, integração por MQTT e persistência.

**Etapa 4, ensaios de conservação.** Lotes de fruta divididos em tratamentos, com variação de
velocidade de retirada e formulação, e grupo de controle sem revestimento. Acompanhamento por
perda de massa contínua, condições ambientais e registro fotográfico diário.

O primeiro lote é aplicado manualmente, em setembro, antes da conclusão da máquina. Isso
inicia a coleta sem dependência do hardware e, além disso, gera o termo de comparação para o
objetivo específico 7.

**Etapa 5, validação.** Reavaliação do modelo sobre a base própria e verificação da hipótese
de que o revestimento aplicado com velocidade controlada reduz a perda de massa e a variância
entre amostras.

## 8. Base de dados

**Pública.** Multi-Parameter Dataset for Machine Learning Based Fruit Spoilage Prediction in
an IoT-Enabled Cold Storage System, Mendeley Data, DOI 10.17632/czz68d9fwj.1, licença
CC BY 4.0. 10.996 registros com temperatura, umidade, luminosidade, CO2 e rótulo binário de
deterioração, cobrindo laranja, abacaxi, banana e tomate.

**Própria.** Gerada pela câmara, acrescenta perda de massa contínua e a descrição do
tratamento aplicado, que a base pública não possui. A base pública responde se dadas condições
ambientais deterioram a fruta; a própria responde se um revestimento, aplicado a uma dada
velocidade, adia a deterioração.

## 9. Resultados esperados

1. Máquina de imersão funcional com velocidade verificada experimentalmente
2. Curvas de perda de massa por tratamento, com grupo de controle
3. Modelo de previsão de deterioração validado, com análise de ablação do sensor de CO2
4. Comparação de variância entre aplicação manual e automatizada
5. Repositório público com firmware, documentação, dados e peças

## 10. Recursos

**Disponíveis.** Fonte de alimentação, atuador linear, motor de passo, display com encoder,
duas placas ESP32, acesso a solda e a impressora 3D.

**A adquirir.** Driver de passo, célula de carga com HX711, sensor de temperatura e umidade,
sensor de luminosidade, chave de fim de curso e material de montagem. Estimativa abaixo de
R$ 200.

**A confirmar.** Fornecimento de quitosana e alginato de sódio pelo laboratório, e empréstimo
de sensor de CO2.

## 11. Riscos e mitigação

| Risco | Mitigação |
|---|---|
| Acionamento não sustentar velocidade constante na taxa necessária | Ensaio de bancada na primeira semana; reduzir micropasso ou trocar o firmware |
| Atraso na montagem mecânica | Primeiro lote aplicado manualmente, sem dependência do hardware |
| Janela curta para ensaios de prateleira | Banana no primeiro lote, por amadurecimento rápido e custo baixo |
| Ausência do sensor de CO2 | Ablação da feature, com o custo de performance quantificado |
| Deriva térmica da célula de carga | Correção pela temperatura, medida na mesma câmara |

## 12. Repositório

https://github.com/alissonjsb4/dip-coating-esp32

Firmware, documentação, dados e registro de decisões. A parte mecânica deriva do projeto
ClubeDoHardware/dip-coating-cdh, da UFC, reimplementado sobre ESP32 para dispensar a montagem
da placa dedicada.
