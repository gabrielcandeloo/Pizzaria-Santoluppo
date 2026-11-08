# Levantamento — Pizzaria Santoluppo

Documento organizado pelo grupo a partir das duas visitas à pizzaria e das respostas do responsável enviadas depois da correção da Entrega 1. Não é transcrição: as falas foram reorganizadas por tema.

## 1. Visitas realizadas

| Visita | Entrevistador(es) | Apoio | Data | Objetivo |
| :---- | :---- | :---- | :---- | :---- |
| 1ª — levantamento inicial | Leon Correia da Silva | Davi Ramos (gravação) e John Antoni (fotos) | 28/09/2026 | visão geral da operação e das dores do atendimento. |
| 2ª — complementação pós-correção | Pablo Lorran | 29/10/2026 | cardápio, cancelamentos, fornecedores, estoque, caixa, expedição, dias e volume, taxa de entrega, meio a meio, horário, fornecedores, registros. |

- **Entrevistado:** Antônio, proprietário da Pizzaria Santoluppo.
- **Local:** Av. Sapopemba, 1333 — Vila Reg. Feijó, São Paulo — SP, CEP 03345-001.
- **Comprovação:** [foto dos membros na entrevista](../docs/membros-entrevista.jpeg) e [frente da pizzaria](../docs/Pizzaria-Frente.jpeg).

## 2. Porte e funcionamento

- Cerca de 6 colaboradores.
- Volume médio de 150 pedidos por semana (cerca de 25 por dia de funcionamento).
- Funciona de terça a domingo a partir das 18h: até 23h de terça a quinta e aos domingos; até 00h às sextas e sábados. Fecha às segundas.
- Pedidos chegam por telefone, WhatsApp e balcão. Saem por entrega (motoboy) ou retirada no balcão. Não há consumo no salão.
- 3 fornecedores recorrentes: um conhecido do dono (caixas de pizza), Coca-Cola (refrigerantes) e PMG (mercadoria geral).

## 3. O que foi levantado, por tema

### 3.1 Atendimento e clientes

- O atendimento é feito por uma pessoa, porque muitos clientes têm dúvidas ou pedem alteração de ingredientes; o dono tem receio de que automatizar o atendimento gere confusão.
- O telefone é a chave de busca do cliente: ao digitá-lo, o cadastro aparece.
- Se o telefone mudou, o endereço é confirmado na hora. Se só o endereço mudou e o cliente não avisou, o pedido vai ao endereço antigo; o motoboy não encontra ninguém, avisa a pizzaria e só então o cliente é contatado (origem do **P1**).
- Em picos (ex.: sexta às 20h) os pedidos de telefone são anotados na hora e as conversas do WhatsApp ficam gravadas, o que permite revisar se algum pedido ficou para trás.

### 3.2 Cardápio

- Sabores em ordem alfabética, cada um com um código numérico fixo que nunca se repete (ex.: 58 = mussarela). O código é usado para lançar o pedido e facilita a impressão.
- Cadastro de produtos e alteração de preços são feitos em um sistema à parte, separado em pizzas, brotos, bebidas e refrigerantes (origem do **P2**).
- Pizza meio a meio: o item tem dois sabores e é cobrado o valor do mais caro.

### 3.3 Pedido, cozinha e expedição

- O pedido registra cliente, tipo (entrega ou retirada), endereço (entrega) e, por item, código, quantidade e observação para a cozinha.
- Comandas de entrega e de retirada são separadas fisicamente na expedição.
- Taxa de entrega: até 3 km não há cobrança; acima de 3 km, taxa fixa de R$ 5,00. A pizzaria não entrega acima de 8 km.
- A pizzaria não registra os motoboys nem qual pedido saiu com qual motoboy.

### 3.4 Pagamento e caixa

- A forma de pagamento é perguntada e anotada no pedido. Dinheiro: pergunta-se se precisa de troco. PIX: envia-se a chave e o cliente manda o comprovante antes de o motoboy sair. Cartão: o motoboy leva a maquininha.
- Cada pedido tem uma única forma de pagamento; não existe pagamento dividido.
- No fim do expediente, o caixa é apurado somando à mão três fontes: cartões, dinheiro e PIX (origem do **P3**).

### 3.5 Cancelamentos e trocas

- O motivo do cancelamento é anotado ao cancelar e revisado no fim do expediente; o pedido cancelado continua no histórico.
- Na maioria dos casos o prato ainda não tinha sido preparado, então não há devolução de estoque.
- Não existe um registro único dos cancelamentos que permita contar e classificar os motivos (origem do **P4**).
- Pedido que sai errado é trocado e o prejuízo é da pizzaria; a troca não é registrada.

### 3.6 Fornecedores e insumos

- Fornecedores poucos e especializados; Coca-Cola e PMG enviam a mercadoria à pizzaria (a PMG raramente).
- Contato e tipo de fornecimento não ficam registrados no sistema (origem do **P5**).
- O próprio dono repõe os ingredientes. Não há controle de quantidade em estoque nem registro de compra/recebimento.
- Quando um ingrediente acaba no meio do expediente, os produtos que o usam saem do cardápio.

### 3.7 Obrigações externas

- Documento fiscal emitido por aplicativo da Receita Federal, fora do sistema.
- Não há registros sanitários (temperaturas, validades, lotes).

## 4. O que mudou em relação à Entrega 1

| Tema | Entrega 1 | Após nova visita / respostas do responsável |
| :---- | :---- | :---- |
| P1 | lógica invertida (telefone x endereço) | corrigida: o problema é o endereço que muda sem aviso |
| Fornecedores e insumos | não modelados | Fornecedor e Insumo com indicador de disponibilidade, sem quantidade |
| Taxa de entrega | não levantada | até 3 km grátis; R$ 5,00 acima de 3 km; limite de 8 km |
| Meio a meio | não levantado | dois sabores por item, cobra o mais caro |
| Horário e volume | não levantados | terça a domingo; ~150 pedidos/semana |
| Pagamento | não confirmado se podia ser dividido | confirmado: uma forma por pedido |
| Motoboys | — | confirmado que não há registro |
