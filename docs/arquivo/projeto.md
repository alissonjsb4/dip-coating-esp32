# O projeto, explicado

Documento de entrada para quem está chegando na equipe. O README é resumo técnico; este
aqui explica o porquê.

## O problema

Perda pós-colheita. Fruta estraga entre a colheita e o consumo, e no Ceará isso pesa porque
a região exporta manga, melón e banana. Uma das formas consagradas de atrasar a deterioração
é aplicar um **revestimento comestível** na casca: quitosana, alginato, ceras. O filme
modifica a troca de gases e a perda de água, então a fruta respira mais devagar, murcha
menos e mofa depois.

O método padrão de laboratório para aplicar esse revestimento é **imersão**: mergulha a
fruta na solução, retira e deixa secar.

## Onde está o furo

Quando se mergulha na mão, cada fruta sai com uma espessura de filme diferente. E a
espessura importa: pela relação de Landau-Levich, ela escala com a **velocidade de retirada
elevada a 2/3**. Retirar rápido deixa filme grosso, retirar devagar deixa fino.

Ou seja, o experimento tem uma variável que decide o resultado e que ninguém controla.

## O que a gente constrói

Duas coisas, que juntas fecham um sistema.

**1. A máquina de dip coating.** Um eixo motorizado que mergulha e retira a fruta com
velocidade programável, de 1 a 200 mm/min, com tempo de imersão e número de ciclos
configuráveis. É o que fixa a variável que estava solta.

**2. A câmara instrumentada.** Uma caixa onde a fruta revestida fica guardada, com sensores
medindo continuamente o que acontece com ela ao longo dos dias.

Os dois publicam por MQTT no mesmo broker. Um modelo de inteligência computacional consome
esses dados e prevê deterioração.

## Por que isso é um projeto e não dois

Sem a máquina, a câmara mede fruta com revestimento inconsistente e o dado não compara.
Sem a câmara, a máquina reveste bonito e ninguém sabe se adiantou alguma coisa. O valor está
no par.

## Arquitetura

```
                    ┌──────────────────────┐
                    │  Nó 1: máquina       │
                    │  ESP32 + A4988       │
                    │  motor, LCD, encoder │
                    └──────────┬───────────┘
                               │ velocidade, posição, ciclo
                               ▼
                        ┌─────────────┐        ┌──────────────────┐
                        │  Broker     │◄───────│  Nó 2: câmara    │
                        │  MQTT       │        │  ESP32 + sensores│
                        └──────┬──────┘        │  massa, T, UR    │
                               │               └──────────────────┘
                               ▼
                  ┌────────────────────────┐
                  │  Banco + modelo de CI  │
                  │  previsão de           │
                  │  deterioração          │
                  └────────────────────────┘
```

Dois nós e não um porque a câmara fica ligada por semanas seguidas e a máquina é usada por
minutos. Se fossem o mesmo, regravar o firmware da máquina derrubaria um ensaio em curso.

## As duas disciplinas

O mesmo sistema atende duas cadeiras, com entregáveis distintos. **Isso precisa ser
confirmado com a professora**, que é a mesma nas duas.

| | Internet das Coisas | TI0179 Projeto Integrador |
|---|---|---|
| Professora | Atslands Rocha | Atslands Rocha e Michela Mulas |
| Foco | telemetria, MQTT, dashboard, ciclo, interface | dataset, modelo de CI, validação, arquitetura |
| Entregas | v0 a v4, quinzenais | M1 a M6 |
| Peça do sistema | os dois nós e a comunicação | o modelo que consome os dados |

O trabalho de Banco de Dados **não** faz parte disso. Aquele é outro domínio, outra equipe,
e segue separado.

## Calendário e a decisão que ele impõe

Ensaio de vida de prateleira leva de uma a três semanas por lote. De setembro a dezembro dá
para quatro a seis lotes.

Consequência: **o lote 1 é mergulhado à mão, em setembro, antes da máquina existir.** Isso
destrava a coleta de dados sem depender do hardware ficar pronto. Os lotes seguintes usam a
máquina, e a comparação entre o lote manual e os automáticos vira resultado em vez de
pressuposto.

Quem for cuidar do lado experimental pode começar na primeira semana. Não espera hardware.

## Estado e próximo passo

Nada validado em hardware ainda. O próximo passo é o ensaio de velocidade descrito em
`ensaio-velocidade.md`: motor e driver na bancada, medindo velocidade real contra a pedida.
É o único risco que pode obrigar a trocar o firmware inteiro, então vem antes de tudo.

## Onde está o quê

| Arquivo | Conteúdo |
|---|---|
| `README.md` | resumo técnico, pinos, como rodar |
| `docs/projeto.md` | este documento |
| `docs/sensores.md` | o que cada sensor mede e por quê |
| `docs/datasets.md` | bases de dados públicas e a nossa |
| `docs/materiais.md` | lista de materiais e onde comprar |
| `docs/passo-a-passo.md` | guia de montagem, na ordem certa |
| `docs/pos-colheita.md` | desenho do experimento |
| `docs/ensaio-velocidade.md` | procedimento do teste de bancada |
| `docs/mecanica.md` | peças 3D a fazer |
| `docs/montagem.md` | boas práticas de ligação |
| `firmware/` | configurações ESPHome |
