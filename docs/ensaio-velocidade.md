# Ensaio de velocidade

Objetivo: verificar se a velocidade real do carro segue a velocidade comandada, na faixa de
operacao de dip coating.

## Montagem

ESP32, driver A4988 e motor NEMA 17 na bancada, alimentados pela fonte do projeto. Motor
acoplado ao atuador linear. Sem LCD, sem encoder.

## Procedimento

Para cada velocidade da tabela:

1. Marcar a posicao inicial do carro.
2. Publicar a velocidade em `dipcoat/number/passos_por_segundo/command`.
3. Publicar o numero de passos em `dipcoat/number/passos_do_ensaio/command`.
4. Acionar `dipcoat/button/mover/command` e cronometrar do inicio ao fim do movimento.
5. Medir o deslocamento com regua.
6. Voltar com `dipcoat/button/voltar/command`.

Tres repeticoes por velocidade.

## Faixa necessaria

Com fuso TR8x8 (8 mm por volta) e NEMA 17 de 200 passos por volta, a faixa alvo de
1 a 200 mm/min exige:

| Micropasso | passos/mm | 1 mm/min | 200 mm/min |
|---|---|---|---|
| cheio | 25 | 0,4 passos/s | 83 passos/s |
| 1/8 | 200 | 3,3 passos/s | 667 passos/s |
| 1/16 | 400 | 6,7 passos/s | 1333 passos/s |

CONFIRMAR o passo do fuso antes do ensaio. Se nao for TR8x8, refazer a tabela.

O `stepper` do ESPHome e temporizado em software. A regiao de 1000 passos/s para cima e
onde ele pode nao sustentar a taxa. Achar esse teto e o objetivo do ensaio.

## Tabela

| passos/s | passos | tempo esperado (s) | tempo medido (s) | deslocamento (mm) | mm/min real |
|---|---|---|---|---|---|
| 50 | 500 | 10,0 | | | |
| 100 | 1000 | 10,0 | | | |
| 200 | 2000 | 10,0 | | | |
| 400 | 4000 | 10,0 | | | |
| 800 | 8000 | 10,0 | | | |
| 1333 | 13330 | 10,0 | | | |
| 2000 | 20000 | 10,0 | | | |

Rodar em passo cheio primeiro. Depois repetir em 1/8 e 1/16 mudando MS1, MS2 e MS3.

## Criterio

Se o tempo medido divergir do esperado em mais de 5 por cento, aquela taxa esta acima do teto do ESPHome.
Se o teto ficar acima de 1333 passos/s, o projeto roda em 1/16 de passo. Se ficar entre 667
e 1333, roda em 1/8. Se ficar abaixo de 667, usa passo cheio ou o firmware muda para
PlatformIO com Arduino.

## Resultado

Pendente.
