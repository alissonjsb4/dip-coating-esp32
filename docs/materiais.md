# Lista de materiais

Estimativas de mercado brasileiro em agosto de 2026, não cotações. Confirmar antes de comprar.

## Já disponível

| Item | Origem |
|---|---|
| Fonte de alimentação | Clube do Hardware |
| Atuador linear OpenBuilds | Clube do Hardware |
| Motor de passo NEMA 17 | Clube do Hardware |
| RepRapDiscount Full Graphic Smart Controller (ST7920 + encoder) | Clube do Hardware |
| ESP32-DevKitC V4 | próprio |
| ESP32 WROOM-32 | próprio |
| Ferro de solda e estanho | laboratório |
| Impressora 3D | laboratório |

## Eletrônica a comprar

| Item | Qtd | Estimativa | Para quê |
|---|---|---|---|
| Driver A4988 (ou DRV8825/TMC2209 em step/dir) | 1 | R$ 15-40 | aciona o motor |
| Dissipador para o driver | 1 | R$ 5 | driver esquenta |
| Capacitor eletrolítico 100 µF/50 V | 1 | R$ 2 | protege o driver do pico de comutação |
| Resistor 10 kΩ | 2 | R$ 1 | pull-up do EN, pull-up do fim de curso |
| Célula de carga 1 kg + módulo HX711 | 1 | R$ 30-45 | perda de massa contínua |
| DHT22 (ou SHT31) | 1 | R$ 25-40 | temperatura e umidade da câmara |
| BH1750 | 1 | R$ 15 | luminosidade, feature do dataset |
| Chave de fim de curso mecânica | 1 | R$ 5-10 | origem e segurança |
| Placa perfurada 10x15 cm | 2 | R$ 12 | uma por nó |
| Barra de pinos fêmea | 4 | R$ 8 | encaixe do ESP32 e do driver |
| Borne KRE 2 vias | 4 | R$ 8 | motor e alimentação |
| Fio rígido e flexível, jumpers | — | R$ 25 | ligações |
| Barra de pinos macho 2x5 | 2 | R$ 4 | adaptador para EXP1/EXP2 do LCD |

**Subtotal: R$ 155 a 215.**

Onde: [AutoCore Robótica](https://www.autocorerobotica.com.br/) (Fortaleza, retirada em mãos,
tem A4988 em estoque), Smart Kits, e Eletrônica Circuito na Rua Pedro Pereira 857, Centro,
para componente avulso.

## Instrumentação de medida

| Item | Estimativa | Para quê |
|---|---|---|
| Balança de precisão 0,01 g | R$ 25-40 | massa de filme em lâmina de vidro |
| Paquímetro | R$ 30 (ou do laboratório) | medir peças antes de modelar |
| Régua metálica e cronômetro | R$ 10 (ou celular) | ensaio de velocidade |

## Câmara

| Item | Estimativa |
|---|---|
| Caixa de isopor média | R$ 25-35 |
| Suporte impresso da célula de carga | filamento |
| Pratinho para apoiar a fruta | filamento ou tampa de pote |
| Tripé ou suporte de celular | R$ 20 (ou improvisado) |

## Revestimento

### Quitosana, e onde conseguir em Fortaleza

**A [Polymar](https://www.polymar.com.br/) fabrica quitosana em Fortaleza**, na Rua Manoel
Arruda, 980, Barroso. Extrai de carapaça de crustáceo, rejeito da indústria pesqueira do
litoral cearense, e vende quitosana em pó. É fornecedor nacional e exporta.

Isso resolve o insumo e dá uma linha boa ao trabalho: quitosana produzida em Fortaleza, a
partir de resíduo da pesca do Ceará, aplicada em fruta do Ceará.

Duas abordagens, nessa ordem:

1. **Contato direto com a Polymar**, apresentando o projeto e pedindo amostra. Empresa local
   costuma atender projeto de graduação de universidade pública, e 100 g resolvem o semestre
   inteiro. Custo possível: zero.
2. **Compra**, se não houver amostra. Quitosana em pó grau alimentício é vendida por
   distribuidores como Mercadão Natural e App Pharma, e por farmácia de manipulação.

**Saída imediata, se as duas travarem:** quitosana em cápsula, vendida como suplemento em
farmácia comum. Um frasco de 500 mg com 120 cápsulas dá 60 g de pó, custa em torno de
R$ 40 e é grau alimentício. Abre a cápsula e usa o pó. Não é elegante, mas funciona e está
disponível hoje.

### Demais insumos

| Item | Onde | Estimativa |
|---|---|---|
| Ácido acético (vinagre branco serve) | supermercado | R$ 5 |
| Glicerina bidestilada | farmácia | R$ 10 |
| Alginato de sódio 100 g | lojas de gastronomia molecular | R$ 30-45 |
| Cloreto de cálcio | gastronomia molecular | R$ 20 |
| Amido de milho | supermercado | R$ 6 |
| Gelatina incolor sem sabor | supermercado | R$ 5 |

Quitosana dissolve em meio ácido, não em água pura. Por isso o ácido acético. Alginato
precisa de cloreto de cálcio para reticular. A glicerina é plastificante e entra em todas as
formulações, para o filme não trincar ao secar.

## Consumíveis de bancada

| Item | Estimativa |
|---|---|
| Béquer 500 ml ou pote de vidro de boca larga | R$ 20 |
| Lâminas de vidro para microscopia, caixa | R$ 15 |
| Luvas descartáveis | R$ 15 |
| Seringa ou pipeta graduada | R$ 5 |
| Jacaré grande ou clipe, para porta-amostra | R$ 10 |
| Papel toalha, potes, etiquetas | R$ 20 |

## Fruta

Banana no lote 1: mais barata, amadurece rápido e tem disponibilidade o ano todo. Uns R$ 15
por lote de 12 unidades, contando controle.

## Total

| Bloco | Estimativa |
|---|---|
| Eletrônica | R$ 155-215 |
| Instrumentação | R$ 55-80 |
| Câmara | R$ 45-55 |
| Revestimento | R$ 55-135, ou menos com amostra da Polymar |
| Consumíveis | R$ 85 |
| Fruta, por lote | R$ 15 |

**Faixa total: R$ 400 a 570**, dividido entre até três pessoas e distribuído ao longo do
semestre.

O item de maior alavancagem continua sendo a célula de carga, a R$ 40. É o que transforma
seis pesagens manuais por fruta em milhares de amostras.
