# Montagem e boas práticas de ligação

Máquina que vibra e motor de passo que chaveia ampères não convivem com jumper em
protoboard. As regras abaixo estão em ordem de retorno.

## 1. Potência nunca passa pela protoboard

Trilha de protoboard aguenta cerca de 1 A e o contato arcaria com a comutação do motor.

- VMOT, GND de potência e as quatro fases do motor: borne a parafuso ou fio soldado.
- Só STEP, DIR, EN e SLEEP podem passar por conector de sinal. São microampères.

## 2. Placa perfurada em vez de protoboard

Meio-termo entre protoboard (frágil) e PCB própria (que foi descartada neste semestre):
uma placa perfurada soldada com barra de pinos fêmea para o ESP32 e para o módulo do
driver, e bornes para motor e alimentação. Custa cerca de R$ 20 e elimina a maior parte
das falhas intermitentes.

## 3. Capacitor e limite de corrente antes de ligar

- Eletrolítico de 100 µF entre VMOT e GND, com pernas curtas, junto do driver. Sem ele o
  pico de comutação mata A4988 com facilidade.
- Ajustar o Vref pelo trimpot antes de girar o motor. No A4988,
  `I_max = Vref / (8 × Rs)`. Conferir o valor de Rs impresso na placa, que costuma ser
  0,1 Ω ou 0,05 Ω. Com Rs = 0,1 Ω, `Vref = 0,8 × I`.
- Para dip coating a carga é leve e a velocidade é baixa. Começar em torno de 0,5 A e só
  subir se perder passo.
- Dissipador no A4988.

## 4. Nunca desconectar o motor com o driver energizado

É a causa número um de A4988 queimado. Desligar a fonte antes de mexer em qualquer fio de
fase.

## 5. Ruído

- Torcer cada par de fios de bobina do motor entre si.
- Passar os fios de sinal longe dos de potência. Se precisarem se cruzar, cruzar a 90°.
- Terra em estrela: todos os terras se encontram num ponto, de preferência na fonte, e não
  em cascata pela trilha da protoboard. Terra compartilhado e fino é o que produz reset do
  ESP32 e passo falso.

## 6. Conector certo em cada lugar

- Motor: JST-XH de 4 vias, que é o padrão de impressora 3D.
- LCD RepRapDiscount: os conectores EXP1 e EXP2 são IDC 2x5. Vai precisar de adaptador
  IDC para Dupont ou de crimpar um cabo. Não dá para espetar jumper solto neles.
- Onde for desconectar com frequência, usar conector com capa, não pino Dupont avulso.

## 7. Fim de curso normalmente fechado

Ligar a chave em NC. Assim um fio partido é lido como acionado e a máquina para, em vez de
falhar silenciosamente em parar. Fio rompido é o modo de falha realista numa peça que se
move.

## 8. Alívio de tração e identificação

- Tudo que se move tem o cabo ancorado no frame com abraçadeira, com folga de serviço no
  carro. O conector nunca segura o peso do cabo.
- Etiquetar as duas pontas de cada fio com fita crepe. Custa dois minutos e economiza horas.

## 9. Ordem de energização no primeiro teste

1. Pull-up de 10k entre EN e VCC já soldado.
2. Motor conectado, fonte desligada.
3. Ajustar Vref com a fonte ligada e o motor parado.
4. Só então gravar o firmware.
