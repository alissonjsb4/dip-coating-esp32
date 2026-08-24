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

## Tabela

| passos/s | passos | tempo esperado (s) | tempo medido (s) | deslocamento (mm) | mm/min real |
|---|---|---|---|---|---|
| 25 | 1000 | 40,0 | | | |
| 50 | 1000 | 20,0 | | | |
| 100 | 1000 | 10,0 | | | |
| 200 | 2000 | 10,0 | | | |
| 400 | 4000 | 10,0 | | | |

## Criterio

Se o tempo medido divergir do esperado em mais de 5 por cento em qualquer linha, o stepper do
ESPHome nao serve para controle de velocidade e o firmware muda para PlatformIO com Arduino.

## Resultado

Pendente.
