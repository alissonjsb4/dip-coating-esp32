# Mecânica e alterações no modelo 3D

O modelo do projeto original está em `model/` do repo do Clube do Hardware. Foi desenhado
em torno de uma PCB própria que não será montada neste semestre. As peças abaixo substituem
essa premissa.

## O que já existe

- Atuador linear OpenBuilds: perfil V-slot, fuso trapezoidal, gantry plate sobre rodas de
  POM, NEMA 17 numa extremidade
- Corner brackets do frame
- Encoder EC11
- RepRapDiscount Full Graphic Smart Controller

## Princípio de escopo

Não redesenhar a máquina. Projetar peças que **parafusam no perfil existente com T-nut**,
sem exigir alteração do frame. Se uma peça obriga a furar ou cortar o perfil, o desenho
está errado.

## Peças a fazer

### 1. Bandeja de eletrônica

Uma peça só, que segura a placa perfurada inteira (ESP32 e driver plugados nela). Não fazer
suporte separado para cada módulo.

- Acesso livre ao conector USB do ESP32 para gravação
- Presa na lateral ou atrás do frame
- Furação de ventilação sob o driver, que esquenta

### 2. Suporte do LCD

O RepRapDiscount é uma placa grande, com furação própria. Montar inclinado para leitura em
pé, e nunca sobre o recipiente.

### 3. Suporte da amostra no gantry plate

Segura o jacaré. Gantry plate OpenBuilds tem furação padronizada, então a peça parafusa no
que já existe.

Regra de projeto: **curto e rígido**. Qualquer balanço aqui vira variação de espessura do
filme, porque a retirada precisa ser suave. Braço longo em balanço é o erro clássico.

### 4. Base do recipiente

A peça que o projeto original não tem e faz falta. O béquer precisa de posição repetível
sob a amostra, senão cada corrida tem alinhamento diferente e o dado não compara.

Um rebaixo ou anel de encaixe na base, dimensionado para o béquer que o laboratório usa.

### 5. Suporte da chave de fim de curso

Pequeno, no perfil, acionando no topo do curso. Serve de referência de origem.

### 6. Presilhas de cabo

Duas ou três no perfil, com folga de serviço no carro. Corrente porta-cabo é exagero para
um eixo só.

## Regras que valem para todas as peças

- **Nada de eletrônica acima do recipiente.** Vapor de solvente e respingo. Eletrônica vai
  para o lado ou para trás, nunca por cima.
- **Material:** PLA serve no frame. Perto do recipiente, PETG, que resiste melhor a
  solvente. Nada impresso entra em contato prolongado com a solução.
- **Medir antes de desenhar.** As peças novas saem das medidas reais das peças em mãos, com
  paquímetro, e não do modelo, que nunca foi impresso nem conferido contra o que o clube
  comprou.

## Dados a confirmar

| Dado | Por que importa | Estado |
|---|---|---|
| Passo do fuso | Converte passos em mm. Define a faixa de velocidade e o micropasso | pendente |
| Curso útil do atuador | Define profundidade de imersão possível | pendente |
| Modelo do NEMA 17 | Corrente nominal, para ajustar o Vref | pendente |
| Tensão e corrente da fonte | A4988 aceita até 35 V. Define a tensão do capacitor do VMOT | pendente |
| Béquer usado no laboratório | Dimensiona a base | pendente |
