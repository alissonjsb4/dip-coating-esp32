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
mosquitto_pub -h <broker> -t 'dipcoat/button/mover_1000_passos/command' -m 'PRESS'
```

Procedimento de medição: marcar o carro, comandar um número conhecido de passos, cronometrar
e medir o deslocamento com régua. Repetir para 25, 50, 100, 200 e 400 passos/s.

## Hardware

| Função | Componente |
|---|---|
| Controlador | ESP32 DevKit |
| Driver de passo | A4988 ou compatível step/dir (DRV8825, TMC2209) |
| Motor | NEMA 17 |
| Eixo | Atuador linear |
| Interface | RepRapDiscount Full Graphic Smart Controller (ST7920 + encoder + buzzer) |
| Segurança | Chave de fim de curso |

Mapeamento de pinos ainda não definido. Os pinos em `bancada-motor.yaml` são provisórios e
serão fixados depois do teste de bancada.

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

## Licença

MIT, ver `LICENSE`. O projeto original do Clube do Hardware não declara licença.
