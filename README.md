# dip-coating-esp32

Controlador de uma máquina de dip coating (revestimento por imersão) de eixo único, rodando
ESPHome em ESP32, com telemetria e comando por MQTT.

Deriva do projeto [ClubeDoHardware/dip-coating-cdh](https://github.com/ClubeDoHardware/dip-coating-cdh),
da UFC, que usa STM32 em PCB própria. Esta versão troca o STM32 por ESP32 em placa de
desenvolvimento para validar movimento, interface e ciclo sem depender da montagem da PCB.

## Estado

Nada validado em hardware ainda. O primeiro teste pendente é o de velocidade, descrito em
`## Uso`.

| Item | Estado |
|---|---|
| Motor girando com A4988 sob ESPHome | não testado |
| Velocidade pedida contra velocidade medida | não medido |
| LCD ST7920 respondendo em 3,3 V | não testado |
| Encoder e menu | não iniciado |
| Ciclo completo com MQTT | não iniciado |

## Uso

Teste de bancada. Mede a velocidade real do motor contra a pedida, que é o parâmetro
fisicamente crítico em dip coating.

```
pip install esphome
cp firmware/secrets.yaml.example firmware/secrets.yaml   # editar wifi e broker
esphome run firmware/bancada-motor.yaml
```

Com o firmware rodando, no broker MQTT:

```
mosquitto_sub -h <broker> -t 'dipcoat/#' -v
mosquitto_pub -h <broker> -t 'dipcoat/number/passos_por_segundo/command' -m '200'
mosquitto_pub -h <broker> -t 'dipcoat/number/passos_do_ensaio/command' -m '2000'
mosquitto_pub -h <broker> -t 'dipcoat/button/mover/command' -m 'PRESS'
mosquitto_pub -h <broker> -t 'dipcoat/button/voltar/command' -m 'PRESS'
```

Procedimento de medição: marcar o carro, comandar um número conhecido de passos, cronometrar
e medir o deslocamento com régua. Repetir para 25, 50, 100, 200 e 400 passos/s.

## Hardware

| Função | Componente |
|---|---|
| Controlador | ESP32-DevKitC V4 (WROOM-32). Uma placa WROOM-32 genérica serve de reserva |
| Driver de passo | A4988 ou compatível step/dir (DRV8825, TMC2209) |
| Motor | NEMA 17 |
| Eixo | Atuador linear |
| Interface | RepRapDiscount Full Graphic Smart Controller (ST7920 + encoder + buzzer) |
| Segurança | Chave de fim de curso |

## Pinos

Escolhidos fora dos strapping pins do ESP32 (GPIO0, 2, 5, 12, 15), fora dos pinos de flash
(GPIO6 a 11) e fora da UART0 (GPIO1, 3). Os pinos de entrada apenas (GPIO34 a 39) não são
usados para saída.

| Sinal | GPIO | Observação |
|---|---|---|
| STEP | 26 | |
| DIR | 27 | |
| SLEEP | 25 | |
| EN | 33 | ativo em nível baixo, precisa de pull-up externo |
| LCD SCLK | 18 | reservado, VSPI |
| LCD MOSI | 23 | reservado, VSPI |
| LCD CS | 22 | reservado |
| Encoder A | 32 | reservado |
| Encoder B | 21 | reservado |
| Encoder botão | 19 | reservado |
| Fim de curso | 4 | reservado, pull-up interno |

O EN do A4988 é ativo em nível baixo e o GPIO fica em alta impedância durante o boot. Sem um
pull-up de 10k entre EN e VCC, o driver pode energizar a bobina antes do firmware subir.

Alimentação do motor vem da fonte direto no VMOT do driver, nunca do regulador da placa, com
terra comum entre fonte e ESP32. O A4988 aceita lógica de 3 a 5,5 V, então os 3,3 V do ESP32
acionam STEP e DIR sem conversor.

## Notas

- ESP32 no lugar do STM32: sem PCB própria, o único motivo do STM32 era a placa do clube. Um
  ESP32 aciona motor, LCD, encoder e fim de curso e ainda tem rádio, o que dispensa inventar
  uma ponte UART entre dois microcontroladores.
- ESPHome no lugar de firmware próprio: `stepper` (a4988), `sensor/rotary_encoder`,
  `display/st7920` e `display_menu` já cobrem toda a lista de drivers a escrever do projeto
  original, e MQTT é nativo.
- FluidNC foi considerado e descartado: resolve o movimento bem e tem WebUI, mas não fala MQTT
  nativo e integrar LCD e encoder custa mais que no ESPHome.
- Teste de velocidade antes de qualquer outra coisa: o `stepper` do ESPHome é pensado para
  posicionamento, não para velocidade constante controlada. Se a velocidade real não seguir a
  pedida, o stack inteiro muda, e isso precisa ser descoberto no início e não no fim.
- `sleep_pin` em vez de `enable_pin`: é o nome que o componente a4988 do ESPHome usa. O EN do
  driver é acionado separado, por GPIO comum.
- Faixa de operação alvo de 1 a 200 mm/min, herdada do projeto original. É regime de baixa
  velocidade, onde a limitação conhecida de frequência do a4988 no ESPHome
  ([esphome/issues#3297](https://github.com/esphome/issues/issues/3297)) tende a não atrapalhar.
- LCD alimentado em 5 V com lógica de 3,3 V do ESP32: funciona na maioria dos relatos, mas é
  risco conhecido. Se não responder, entra level shifter.
- Pinos escolhidos fora dos strapping pins e com GPIO18 e 23 reservados para o SPI do LCD,
  para o mapeamento não precisar mudar quando a interface entrar.
- Pull-up externo no EN do driver: sem ele o pino fica em alta impedância durante o boot e o
  A4988, que habilita em nível baixo, pode energizar a bobina antes do firmware subir.

## Licença

MIT, ver `LICENSE`. O projeto original do Clube do Hardware não declara licença.
