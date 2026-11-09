# Entrega 2 — Modelo Lógico, Implementação SQL e Apresentação Final

### Modelagem de um sistema de gestão de informações para uma organização de pequeno porte — Pizzaria Santoluppo

---

## 1. Relatório Técnico Final

### Integrantes

| Nome completo do aluno | RGM | Usuário do GitHub |
| :---- | :---- | :---- |
| Davi Ramos de Oliveira | 49576216 | daviramosdeoliveira67-eng |
| Gabriel Silva Candelo | 48050938 | gabrielcandeloo |
| John Antoni Rodrigo Quispe | 49450492 | John-anToni |
| Leon Correia da Silva | 48036374 | leon-verse |
| Pablo Lorran Ferreira da Silva | 48146978 | Lorran57 |

&nbsp;

### 1.1 Revisão do Modelo Conceitual

#### Caracterização da organização

- **Nome e natureza da organização:** Pizzaria Santoluppo — empresa privada, com fins lucrativos, do ramo de alimentação (pizzaria de entrega e retirada).

- **Contexto e porte:** cerca de 6 colaboradores, volume médio de 150 pedidos por semana (cerca de 25 por dia de funcionamento) e 3 fornecedores recorrentes (caixas de pizza, refrigerantes e mercadoria geral). Funciona de terça a domingo, a partir das 18h: até 23h de terça a quinta e aos domingos, e até 00h às sextas e sábados; fecha às segundas. Os pedidos chegam por telefone, WhatsApp e balcão e saem por entrega (motoboy) ou retirada; não há consumo no salão.

- **Problemas e necessidades identificados:**
  - **P1 — Endereço desatualizado e entrega frustrada:** ao digitar o telefone, o sistema exibe o cadastro do cliente. Se o telefone mudou, o endereço é confirmado na hora; porém, se apenas o endereço mudou e o cliente não avisou, o pedido é despachado ao endereço antigo. O motoboy não encontra ninguém, avisa a pizzaria e só então o cliente é contatado para confirmar o novo endereço. O modelo precisa registrar essa ocorrência no pedido e manter o histórico de endereços do cliente, para medir quantas entregas falham por endereço desatualizado.
  - **P2 — Desintegração de cardápio e pedidos:** o cadastro de novos produtos e as alterações de preço são feitos em um sistema à parte. A operação usa códigos numéricos decorados ou tabelas externas (ex.: código 58 para mussarela) para lançar o pedido, com risco de divergência entre o cardápio cadastrado e os códigos e preços lançados no pedido.
  - **P3 — Fechamento de caixa fragmentado:** a apuração no fim do expediente exige somar à mão três fontes (cartões, dinheiro e PIX), o que atrasa a consolidação diária das vendas.
  - **P4 — Rastreabilidade de cancelamentos:** os motivos de cancelamento são anotados, mas não ficam num registro único ligado ao pedido; não é possível contar cancelamentos por período nem classificá-los por motivo sem refazer a apuração à mão.
  - **P5 — Fornecedores sem cadastro de contato e fornecimento:** cada fornecedor atende uma parte (caixas de pizza; refrigerantes da Coca-Cola; mercadoria geral da PMG), e a Coca-Cola e a PMG enviam a mercadoria. Contato, tipo de fornecimento e quem envia mercadoria não ficam registrados, o que dificulta saber a quem recorrer para repor cada item.

- **Escopo do sistema:** o banco de dados cobre a operação inteira da pizzaria: o catálogo unificado de produtos (pizzas, brotos, bebidas e refrigerantes) com disponibilidade; o cadastro de insumos e fornecedores; o atendimento (identificação do cliente pelo telefone e histórico de endereços); o pedido (itens, pizza meio a meio, taxa de entrega por distância, entrega frustrada e cancelamento); o pagamento (forma única por pedido, troco e comprovante PIX); e o fechamento de caixa diário por forma de pagamento.

- **Justificativa da escolha:** acesso direto ao local, ao proprietário e ao volume real de operação, e um ganho visível para a pizzaria ao unificar cardápio, pedidos e caixa num único banco.

- **Evidências da organização:**
  - Redes sociais: [Instagram](https://www.instagram.com/santoluppopizzaria/)
  - Endereço: Av. Sapopemba, 1333 — Vila Reg. Feijó — CEP 03345-001. Telefone (11) 2028-8486 — WhatsApp (11) 99558-5599
  - Foto no local: [Frente da pizzaria](docs/Pizzaria-Frente.jpeg)
  - Google Maps: [Pizzaria Santoluppo](https://maps.app.goo.gl/FwuzXXwQoKZwRVKD6)

- **Responsável pela organização:** Antônio (proprietário) — telefone (11) 99575-9922

#### Relato da entrevista

- **Quem:** entrevistado Antônio, proprietário. Entrevistadores: Leon Correia da Silva e Pablo Lorran. Gravação e fotos: Davi Ramos e John Antoni.
- **Quando e onde:** 1ª visita em 28/09/2026; 2ª visita (pós-correção) em `[PREENCHER: data]` — ambas na própria pizzaria, Av. Sapopemba, 1333, Vila Reg. Feijó, São Paulo — SP. Entrevista presencial com o proprietário, com gravação de áudio e fotos.
- **Comprovação:** [membros presentes — Pablo Lorran, Davi Ramos, Leon Correia e John Antoni](docs/membros-entrevista.jpeg)
- **Levantamento completo:** [Levantamento](docs/levantamento.md)

**Resumo do levantamento.** Na primeira visita foram levantadas as dores do atendimento: o pico de sexta à noite, o uso do telefone como chave do cadastro, a entrega que vai ao endereço antigo quando o cliente muda sem avisar (P1) e o registro da forma de pagamento no pedido (troco, comprovante PIX, maquininha). Depois da correção da Entrega 1, a segunda visita e as respostas do responsável confirmaram o cardápio por código numérico mantido em sistema à parte (P2), o fechamento de caixa somando três fontes à mão (P3), o motivo de cancelamento anotado sem registro único (P4), os três fornecedores e a reposição feita pelo dono, sem controle de quantidade (P5). Também foram confirmados: horário e volume, a regra da taxa de entrega (até 3 km grátis, R$ 5,00 acima disso, limite de 8 km), a pizza meio a meio cobrada pelo sabor mais caro, a forma de pagamento única por pedido e a ausência de registro de motoboys. Mudou em relação à Entrega 1: a lógica do P1 foi corrigida e entraram no modelo Fornecedor, Insumo, a taxa de entrega e o segundo sabor.

#### Processos de negócio

| Processo | Como funciona na Santoluppo |
| :---- | :---- |
| Gestão de cardápio | Cada produto tem código numérico único e fixo (ex.: 58 = mussarela), nome, categoria (pizza, broto, bebida, refrigerante), tamanho quando houver, preço e indicador de disponibilidade. Hoje é feito em sistema à parte (P2). |
| Atendimento e registro do pedido | Atendente busca o cliente pelo telefone, confirma o endereço quando o telefone é novo, registra tipo (entrega ou retirada), endereço (entrega) e os itens por código, com quantidade e observação. Meio a meio registra dois sabores e cobra o mais caro. Entrega até 3 km é grátis; acima de 3 km paga R$ 5,00; não há entrega acima de 8 km. Só no horário de funcionamento. |
| Produção e expedição | Comanda impressa vai para a cozinha; pronta, sai com o motoboy ou é entregue no balcão. Se o motoboy não encontra ninguém, a pizzaria contata o cliente e grava o novo endereço (P1). |
| Pagamento | Uma forma por pedido: dinheiro (com ou sem troco), PIX (comprovante antes da saída) ou cartão (maquininha com o motoboy). Valor = itens + taxa de entrega. |
| Cancelamento | Motivo obrigatório; o pedido cancelado fica no histórico; sem devolução de estoque (P4). |
| Fechamento de caixa | Soma dos pedidos não cancelados do dia, separada por forma de pagamento (P3). |
| Fornecedores e insumos | Cadastro de fornecedores com tipo, telefone e se envia mercadoria; insumos com indicador de disponibilidade; quando um insumo acaba, os produtos que o usam saem do cardápio (P5). |

**Processos comuns em pizzarias que ficaram fora do modelo:**
- **Controle de estoque por quantidade:** o dono repõe os ingredientes e confere visualmente; o modelo guarda só a disponibilidade do insumo.
- **Compra e recebimento de mercadoria:** não são registrados; por isso há FORNECEDOR, mas não COMPRA nem RECEBIMENTO.
- **Cadastro de motoboys e atribuição de entregas:** a pizzaria não registra; por isso não há MOTOBOY nem ENTREGA.
- **Consumo no salão (mesas, comandas):** não existe na operação.
- **Troca de pedido errado:** acontece, mas não é registrada.
- **Emissão fiscal:** feita por aplicativo da Receita Federal, fora do banco.
- **Controle sanitário:** não há registros sanitários na organização.
- **Escala de funcionários:** não é gerida em sistema e não foi apontada como necessidade.

#### Requisitos

**Requisitos funcionais**

| Código | Requisito | Problema |
| :---- | :---- | :---- |
| RF01 | Cadastrar produtos com código numérico único, nome, categoria (pizza, broto, bebida, refrigerante), tamanho (quando houver), preço e indicador de disponibilidade. | P2 |
| RF02 | Registrar pedidos com cliente, data e hora, tipo (entrega ou retirada), endereço de entrega (só na entrega) e, para cada item, código do produto, quantidade, preço cobrado e observação para a cozinha. | P2 |
| RF03 | Registrar o cancelamento do pedido com motivo obrigatório, mantendo o pedido no histórico. | P4 |
| RF04 | Emitir o fechamento de caixa de um dia, com o total vendido por forma de pagamento (dinheiro, PIX, cartão) e o total geral, sem os pedidos cancelados. | P3 |
| RF05 | Registrar uma única forma de pagamento por pedido e o valor total (itens + taxa); se dinheiro, se precisa de troco; se PIX, se o comprovante foi recebido. | P3 |
| RF06 | Registrar no pedido a ocorrência de endereço não localizado e cadastrar o novo endereço do cliente, mantendo o anterior no histórico como inativo. | P1 |
| RF07 | Cadastrar fornecedores com nome, tipo de fornecimento (embalagens, bebidas, mercadoria geral), telefone e indicador de envio de mercadoria. | P5 |
| RF08 | Cadastrar insumos com nome, tipo (ingrediente ou embalagem), disponibilidade, fornecedor habitual (quando houver) e os produtos que os usam; ao marcar um insumo como indisponível, listar os produtos afetados. | P5 |
| RF09 | Registrar, na entrega, a distância em km e a taxa (grátis até 3 km; R$ 5,00 acima de 3 km; entrega limitada a 8 km) e, no item meio a meio, o segundo sabor, cobrando o mais caro. | — |
| RF10 | Emitir relatório de cancelamentos por período e por motivo. | P4 |
| RF11 | Emitir relatório de entregas frustradas por endereço desatualizado, por período. | P1 |
| RF12 | Listar os itens de pedido cujo preço cobrado diverge do preço de cardápio. | P2 |

**Requisitos não funcionais**

| Código | Requisito |
| :---- | :---- |
| RNF01 (Usabilidade/Desempenho) | Lançar itens no pedido pelo código numérico do produto, com busca pela chave primária. |
| RNF02 (Disponibilidade) | O banco deve funcionar localmente durante todo o expediente, sem depender de internet. |
| RNF03 (Segurança) | Apenas usuários autorizados do SGBD podem alterar preços e registrar cancelamentos; o atendente não exclui pedidos (controle por `GRANT` do MySQL). |
| RNF04 (Confiabilidade) | Garantir transações ACID (InnoDB), para que pedido, itens e pagamento sejam gravados juntos ou não sejam gravados. |
| RNF05 (Privacidade — LGPD) | Coletar do cliente só nome, telefone e endereço, necessários à entrega. |

**Requisitos comuns no setor que ficaram de fora:** pedido online / cardápio digital (a pizzaria atende por pessoa, por escolha do dono); programa de fidelidade (não existe); rastreamento do motoboy (motoboys não são registrados); emissão de nota fiscal integrada (feita por aplicativo da Receita); baixa automática de estoque por receita (não há controle de quantidade).

#### Regras de negócio

**Regras operacionais**

| Código | Regra |
| :---- | :---- |
| RN01 | Todo produto lançado no pedido é identificado pelo código numérico único do cardápio (ex.: 58 = mussarela). |
| RN02 | O cancelamento exige motivo; não há devolução automática de insumos. |
| RN03 | O fechamento de caixa totaliza os pedidos por três formas de pagamento: dinheiro, PIX e cartão. |
| RN04 | Não há baixa de estoque por quantidade; quando um insumo fica indisponível, os produtos que o usam ficam indisponíveis no cardápio. |
| RN05 | Pedido cancelado não é excluído; fica no histórico com o motivo. |
| RN06 | Todo pedido tem exatamente um pagamento, com uma única forma; não há pagamento dividido. |
| RN07 | Se a forma é dinheiro, registra-se se o cliente precisa de troco; nas outras formas, não. |
| RN08 | Pedido pago por PIX só é despachado após o recebimento do comprovante. |
| RN09 | Telefone que não consta no cadastro exige confirmação do endereço antes do despacho; o telefone identifica um único cliente. |
| RN10 | Quando o motoboy não encontra ninguém, o pedido recebe a ocorrência "endereço não localizado", o novo endereço é gravado e o anterior fica no histórico como inativo. |
| RN11 | Todo fornecedor tem nome, tipo de fornecimento e telefone. |
| RN12 | Entrega até 3 km não paga taxa; acima de 3 km paga taxa fixa de R$ 5,00. Retirada não tem taxa. |
| RN19 | A pizzaria não entrega acima de 8 km; nesse caso o pedido só pode ser de retirada. |
| RN13 | Na meio a meio, o item tem dois sabores diferentes e é cobrado o valor do mais caro. |
| RN14 | Pedidos só são registrados no horário de funcionamento: terça a quinta e domingo, 18h–23h; sexta e sábado, 18h–00h; fechado às segundas. |
| RN15 | Pedido de entrega exige endereço; pedido de retirada não tem endereço. |
| RN16 | O endereço de entrega de um pedido pertence ao cliente que fez o pedido. |
| RN17 | O valor do pagamento é a soma dos itens (quantidade × preço cobrado) mais a taxa de entrega. |
| RN18 | Produto indisponível não pode ser lançado em pedido novo. |

**Restrições legais e organizacionais**
- **LGPD (Lei nº 13.709/2018):** os dados do cliente (nome, telefone, endereço) são usados só para a entrega, pelo princípio da minimização.
- **Normas sanitárias (RDC Anvisa nº 216/2004; Portaria CVS nº 5/2013):** a pizzaria está sujeita a elas, mas não mantém registros sanitários; o modelo não guarda temperatura, validade ou lote.
- **Documento fiscal:** emitido por aplicativo da Receita Federal, fora do banco.

**Regra comum no setor que ficou de fora:** tempo prometido de entrega — não foi levantado como regra e o modelo não guarda horário de saída nem de entrega.

#### DER conceitual revisado

- Arquivo: [docs/DER-PIZZARIA-revisado.jpeg](docs/DER-PIZZARIA-revisado_page-0001.jpg) — criado no **brModelo Web** (app.brmodeloweb.com).
- Versão da Entrega 1 (histórico): [docs/DER-PIZZARIA_page-0001.jpg](docs/DER-PIZZARIA_page-0001.jpg)

#### Caso A — apontamentos da correção da Entrega 1

| Apontamento da correção da E1 | O que mudou no trabalho | Por quê |
| :---- | :---- | :---- |
| 3.2 — o README não relata com quem, quando e como foi a entrevista; o responsável não aparece na foto | Relato com entrevistado, entrevistadores, datas e forma da entrevista, telefone do responsável e levantamento completo em `levantamento/levantamento.md` | o professor precisa poder confirmar o contato com a organização |
| 4 — "Problemas e necessidades" ficou com o texto do esqueleto | Problemas P1 a P5 descritos a partir da entrevista | a modelagem precisa partir das dores reais da pizzaria |
| 5 — processos com 50% de cobertura: faltavam cardápio, cancelamento, fornecedores, recebimento de mercadoria e fechamento de caixa; compras e estoque numa frase | Incluídos gestão de cardápio, cancelamento, fechamento de caixa e fornecedores/insumos; compra, recebimento e estoque por quantidade justificados como processos que a pizzaria não registra | confirmado na 2ª visita: o dono repõe os ingredientes pessoalmente, sem registro |
| 6 — requisitos e regras derivados das cardinalidades do DER, não da operação; estoque e compras sem requisito | Requisitos reescritos a partir dos processos, cada RF ligado a um problema (P1–P5); RF07 e RF08 para fornecedores e insumos | requisito descreve o que a operação precisa, não o desenho do diagrama |
| 7 — poucos requisitos para os processos descritos | 12 RF e 5 RNF | cobrir todos os processos e problemas |
| 8 — regras com 38% de cobertura; sem taxa de entrega, meio a meio, horário, LGPD, sanitária e fiscal | RN01–RN19 com taxa e limite de entrega, meio a meio, horário, endereço do pedido; restrições LGPD, sanitárias e fiscais; tempo de entrega justificado como não incluído | regras levantadas com o responsável; cancelamento após o início do preparo `[PENDENTE: confirmar com o Antônio]` |
| 9 — dicionário só com tabela de atributos | Dicionário com relacionamentos e cardinalidades, definição das entidades, fluxo de dados, convenções, notação formal e LGPD `[PENDENTE: collation, versão, leitura das expressões, índices por entidade, log de acesso e acesso por operação]` | itens exigidos pelo esqueleto |
| 10 — `Contém` associativa sem atributos e ternária PEDIDO–PIZZA–BEBIDAS | PIZZA e BEBIDAS unidas em PRODUTO com categoria; Item_pedido com quantidade, preço unitário e observação, ligando só PEDIDO e PRODUTO | cada item é um único produto e precisa guardar quantidade e preço cobrado |
| 10 — sem INSUMO e FORNECEDOR, embora o processo exista | Entidades INSUMO e FORNECEDOR com os relacionamentos "Fornece" e "Compõe" | sustentam o P5 e a regra de disponibilidade (RN04) |
| 10 — `Decisão` com (0,n): um pagamento poderia ter várias formas | Relacionamento renomeado para "Quitado por", com (1,1) do lado de FORMAS_PAGAMENTOS | a pizzaria confirmou uma forma por pedido (RN06) |
| 10 — endereço composto em CLIENTE impede mais de um endereço | ENDERECO virou entidade, ligada a CLIENTE ("Possui") e a PEDIDO ("Entrega em") | histórico de endereços e medição das entregas frustradas (P1) |
| COMPRA, MOTOBOY/ENTREGA e FUNCIONÁRIO sugeridas como desejáveis | Mantidas fora, com justificativa em Processos de negócio | a pizzaria não registra compras, motoboys nem escala |

&nbsp;

### 1.2 Conversão do Modelo Conceitual para o Modelo Lógico

A conversão seguiu as regras de mapeamento do modelo ER para o relacional. Os nomes das colunas seguem as convenções do [dicionário de dados](docs/dicionario-de-dados.html) (`ID_` identificador, `CD_` código, `NM_` nome, `DS_` descrição, `TP_` tipo, `QT_` quantidade, `VL_` valor, `IN_` indicador, `DT_` data).

- **Entidades → tabelas:** CLIENTE, ENDERECO, PEDIDO, PRODUTO, PAGAMENTO, FORMAS_PAGAMENTOS, FORNECEDOR e INSUMO viraram tabelas. A entidade associativa Item_pedido virou a tabela ITEM_PEDIDO. O relacionamento N:N "Compõe" (Insumo–Produto) virou a tabela PRODUTO_INSUMO.

- **Atributos → colunas (tipos do MySQL):**
  - textos (nomes, telefones, logradouros, status, categorias): `VARCHAR` com tamanho definido no dicionário; motivo de cancelamento: `TEXT`;
  - valores monetários (preço, preço cobrado, taxa, valor total): `DECIMAL(10,2)` — valor exato com duas casas; `FLOAT` foi descartado porque arredonda em binário (ex.: 0,1 + 0,2 ≠ 0,3) e geraria diferença de centavos no fechamento de caixa;
  - distância: `DECIMAL(5,2)`; quantidades e identificadores: `INT`;
  - indicadores (disponível, troco, comprovante, envia mercadoria, entrega frustrada, endereço ativo): `BOOLEAN`;
  - data e hora do pedido e do cadastro do endereço: `DATETIME`.

- **Relacionamentos 1:N** — a chave primária do lado 1 vira chave estrangeira no lado N:
  - CLIENTE (1) — ENDERECO (N) "Possui": `ENDERECO.ID_CLIENTE` → CLIENTE. É isso que guarda o histórico de endereços de cada cliente.
  - CLIENTE (1) — PEDIDO (N) "Faz": `PEDIDO.ID_CLIENTE` → CLIENTE.
  - ENDERECO (1) — PEDIDO (N) "Entrega em": `PEDIDO.ID_ENDERECO` → ENDERECO, opcional (nula na retirada).
  - FORMAS_PAGAMENTOS (1) — PAGAMENTO (N) "Quitado por": `PAGAMENTO.ID_FORMA` → FORMAS_PAGAMENTOS.
  - FORNECEDOR (1) — INSUMO (N) "Fornece": `INSUMO.ID_FORNECEDOR` → FORNECEDOR, opcional (insumo pode não ter fornecedor habitual).
  - PRODUTO (1) — ITEM_PEDIDO (N) "Segundo_sabor": `ITEM_PEDIDO.CD_PRODUTO_2` → PRODUTO, opcional. É uma segunda FK para a mesma tabela, com nome diferente da primeira (`CD_PRODUTO`).

- **Relacionamento 1:1** — PEDIDO (1) — PAGAMENTO (1) "Gera": `PAGAMENTO.ID_PEDIDO` → PEDIDO com restrição `UNIQUE`. A FK fica no pagamento porque ele depende do pedido; o `UNIQUE` impede um segundo pagamento para o mesmo pedido (RN06).

- **Relacionamentos N:N** — viram tabela própria com as duas chaves:
  - PEDIDO (N) — PRODUTO (N) "Contém" → ITEM_PEDIDO, com FKs `ID_PEDIDO` e `CD_PRODUTO` e os atributos do relacionamento (quantidade, preço cobrado, observação). A chave primária é (`ID_PEDIDO`, `NR_ITEM`), e não (`ID_PEDIDO`, `CD_PRODUTO`), porque o mesmo produto pode aparecer duas vezes no pedido com observações diferentes (ex.: uma mussarela sem cebola e outra normal) ou como primeiro sabor de duas meio a meio diferentes.
  - INSUMO (N) — PRODUTO (N) "Compõe" → PRODUTO_INSUMO, com chave primária composta (`CD_PRODUTO`, `ID_INSUMO`), ambas FKs.

- **Generalização/especialização:** não foi usada. Pizzas, brotos, bebidas e refrigerantes compartilham os mesmos atributos (código, nome, preço, disponibilidade); só o tamanho é exclusivo das pizzas. Uma especialização geraria subtabelas quase vazias e obrigaria o ITEM_PEDIDO a apontar para mais de uma tabela. O tipo foi resolvido pelo atributo `TP_CATEGORIA` em PRODUTO.

- **Tabelas resultantes (10):** CLIENTE, ENDERECO, PRODUTO, FORMAS_PAGAMENTOS, FORNECEDOR, INSUMO, PRODUTO_INSUMO, PEDIDO, ITEM_PEDIDO e PAGAMENTO. O diagrama está na seção 2.

### 1.3 Definição de Chaves Primárias e Estrangeiras

| Tabela | Chave primária | Chaves estrangeiras |
| :---- | :---- | :---- |
| CLIENTE | `ID_CLIENTE` | — |
| ENDERECO | `ID_ENDERECO` | `ID_CLIENTE` → CLIENTE |
| PRODUTO | `CD_PRODUTO` (código do cardápio, sem auto incremento) | — |
| FORMAS_PAGAMENTOS | `ID_FORMA` | — |
| FORNECEDOR | `ID_FORNECEDOR` | — |
| INSUMO | `ID_INSUMO` | `ID_FORNECEDOR` → FORNECEDOR (aceita nulo) |
| PRODUTO_INSUMO | (`CD_PRODUTO`, `ID_INSUMO`) | `CD_PRODUTO` → PRODUTO; `ID_INSUMO` → INSUMO |
| PEDIDO | `ID_PEDIDO` | `ID_CLIENTE` → CLIENTE; (`ID_ENDERECO`, `ID_CLIENTE`) → ENDERECO (`ID_ENDERECO`, `ID_CLIENTE`) (aceita nulo) |
| ITEM_PEDIDO | (`ID_PEDIDO`, `NR_ITEM`) | `ID_PEDIDO` → PEDIDO; `CD_PRODUTO` → PRODUTO; `CD_PRODUTO_2` → PRODUTO (aceita nulo) |
| PAGAMENTO | `ID_PAGAMENTO` | `ID_PEDIDO` → PEDIDO (`UNIQUE`); `ID_FORMA` → FORMAS_PAGAMENTOS |

A FK do endereço no pedido é composta (`ID_ENDERECO`, `ID_CLIENTE`) para que o banco recuse um pedido entregue no endereço de outro cliente (RN16). Para isso, ENDERECO recebe `UNIQUE (ID_ENDERECO, ID_CLIENTE)`. Na retirada, `ID_ENDERECO` fica nulo e o MySQL não verifica a FK composta.

### 1.4 Processo de Normalização (1FN, 2FN e 3FN)

- **1FN — sem atributos multivalorados nem grupos repetitivos.**
  - O cliente pode ter vários endereços ao longo do tempo. Guardar "endereço" como coluna de CLIENTE obrigaria a sobrescrever o antigo (perdendo o histórico do P1) ou a criar colunas repetidas (endereço1, endereço2…). O endereço foi para a tabela ENDERECO, uma linha por endereço.
  - Um pedido tem vários itens. Em vez de colunas produto1, qtd1, produto2, qtd2…, cada item é uma linha de ITEM_PEDIDO.
  - Um produto usa vários insumos. A lista de insumos virou a tabela PRODUTO_INSUMO, uma linha por par produto–insumo.
  - O endereço também foi decomposto em colunas atômicas (rua, número, bairro, CEP), para que seja possível agrupar entregas por bairro.

- **2FN — sem dependência parcial de chave composta.** Só ITEM_PEDIDO e PRODUTO_INSUMO têm chave composta.
  - Em ITEM_PEDIDO (`ID_PEDIDO`, `NR_ITEM`), um esboço inicial guardava o nome do produto e a data do pedido no item. O nome depende só de `CD_PRODUTO` e a data depende só de `ID_PEDIDO` — parte da chave. Ficaram em PRODUTO e PEDIDO. No item restaram quantidade, preço cobrado e observação, que dependem do item inteiro.
  - PRODUTO_INSUMO não tem atributos além da chave, então já está em 2FN.

- **3FN — sem dependência transitiva.**
  - A descrição da forma de pagamento ("Dinheiro", "PIX", "Cartão") depende do código da forma, não do pagamento (`ID_PAGAMENTO → ID_FORMA → DS_METODO`). Foi para FORMAS_PAGAMENTOS, o que também evita grafias diferentes ("Pix", "PIX") que quebrariam o fechamento de caixa (P3).
  - Nome e telefone do cliente no pedido dependeriam do cliente (`ID_PEDIDO → ID_CLIENTE → NM_CLIENTE`); ficam só em CLIENTE.
  - Telefone e tipo do fornecedor no insumo dependeriam do fornecedor (`ID_INSUMO → ID_FORNECEDOR → DS_TELEFONE`); ficam só em FORNECEDOR.

- **Redundâncias mantidas de propósito (e como são controladas):**
  - `ITEM_PEDIDO.VL_UNITARIO` repete o preço do produto no momento da venda. Não é dependência transitiva: é outro fato (o preço cobrado naquele dia). Sem ele, uma mudança de preço no cardápio alteraria o faturamento de pedidos antigos. A divergência entre os dois é exatamente o que o P2 quer medir.
  - `PEDIDO.VL_TAXA_ENTREGA` pode ser calculada a partir da distância, e `PAGAMENTO.VL_TOTAL` a partir dos itens e da taxa. Foram mantidos porque são o valor efetivamente cobrado do cliente e conferido no caixa. A taxa é amarrada à distância por `CHECK` (RN12) e o total é conferido por consulta de auditoria (RN17).

- **Justificativa do processo:** a 1FN era necessária por causa do histórico de endereços (P1) e dos itens do pedido; a 2FN, por causa das chaves compostas das duas tabelas associativas; a 3FN, porque o fechamento de caixa (P3) e o contato com fornecedores (P5) dependem de que cada informação esteja num lugar só.

### 1.5 Justificativas Técnicas das Decisões

**Chaves primárias**
- `CD_PRODUTO` é chave natural: é o código que a equipe já decorou e imprime na comanda (RN01). Não usa auto incremento, para que o código 58 continue sendo a mussarela.
- As demais tabelas usam chave substituta inteira (`ID_...`, `AUTO_INCREMENT`). Telefone não serve de chave do cliente porque muda (P1 começa justamente na troca de telefone); ele é `UNIQUE`, mas pode ser alterado sem quebrar os pedidos antigos.
- ITEM_PEDIDO usa (`ID_PEDIDO`, `NR_ITEM`) — ver seção 1.2.
- PRODUTO_INSUMO usa a combinação das duas FKs, porque o mesmo insumo não se repete no mesmo produto.

**Chaves estrangeiras** — todas com `ON DELETE RESTRICT`: nenhum cliente, produto ou forma de pagamento usado em pedido pode ser apagado, o que preserva o histórico (RN05). As FKs opcionais (`ID_ENDERECO`, `CD_PRODUTO_2`, `ID_FORNECEDOR`) aceitam nulo porque o relacionamento tem cardinalidade mínima 0.

**Restrições**

| Restrição | Tabela | Regra |
| :---- | :---- | :---- |
| `DS_TELEFONE UNIQUE NOT NULL` | CLIENTE | RN09 — o telefone localiza um único cliente |
| `IN_ATIVO DEFAULT TRUE` e `DT_CADASTRO` | ENDERECO | RN10 — o endereço antigo é desativado, não apagado |
| `CHECK (TP_CATEGORIA IN ('PIZZA','BROTO','BEBIDA','REFRIGERANTE'))` | PRODUTO | RF01 |
| `CHECK (VL_PRECO > 0)` | PRODUTO | RF01 |
| `IN_DISPONIVEL NOT NULL DEFAULT TRUE` | PRODUTO, INSUMO | RN04 |
| `DS_METODO UNIQUE` | FORMAS_PAGAMENTOS | RN03 — só três formas, sem duplicidade |
| `CHECK (TP_FORNECIMENTO IN ('EMBALAGENS','BEBIDAS','MERCADORIA GERAL'))`, `DS_TELEFONE NOT NULL` | FORNECEDOR | RN11 |
| `CHECK (TP_PEDIDO IN ('ENTREGA','RETIRADA'))` | PEDIDO | RN15 |
| `CHECK` entrega ⇒ endereço e distância preenchidos; retirada ⇒ sem endereço, sem distância e taxa 0 | PEDIDO | RN15 |
| `CHECK` distância ≤ 3 ⇒ taxa 0; 3 < distância ≤ 8 ⇒ taxa 5,00 | PEDIDO | RN12, RN19 |
| FK composta (`ID_ENDERECO`, `ID_CLIENTE`) | PEDIDO | RN16 |
| `CHECK` sobre dia da semana e hora de `DT_HORA` | PEDIDO | RN14 |
| `IN_ENTREGA_FRUSTRADA NOT NULL DEFAULT FALSE` | PEDIDO | RN10 / P1 — a ocorrência não some quando o status muda para "ENTREGUE" |
| `CHECK` status "CANCELADO" ⇔ motivo preenchido | PEDIDO | RN02, RN05 |
| `CHECK (QT_ITEM > 0)`, `CHECK (VL_UNITARIO > 0)` | ITEM_PEDIDO | RF02 |
| `CHECK (CD_PRODUTO_2 IS NULL OR CD_PRODUTO_2 <> CD_PRODUTO)` | ITEM_PEDIDO | RN13 — dois sabores diferentes |
| `ID_PEDIDO UNIQUE NOT NULL` | PAGAMENTO | RN06 |

Regras que dependem de dados de outra tabela não podem ser `CHECK` no MySQL e são conferidas por consultas de auditoria: troco só em dinheiro (RN07), comprovante em PIX (RN08), preço do meio a meio (RN13), total do pagamento (RN17), produto indisponível em pedido (RN18) e pedido sem pagamento ou sem itens (RN06).

**Decisões arquiteturais**
- **Catálogo único de produtos**, diferenciado por categoria (ver 1.2).
- **Pagamento separado do pedido** apesar do 1:1: os atributos de pagamento (troco, comprovante, forma) são de outra natureza que os de entrega, e o fechamento de caixa (P3) consulta só PAGAMENTO e FORMAS_PAGAMENTOS.
- **Cancelamento como atributo do pedido**, não como tabela: cada pedido é cancelado no máximo uma vez e o motivo é um texto. Com o status "CANCELADO" e o motivo obrigatório (RN02), uma consulta agrupa cancelamentos por período e motivo (P4) sem tabela adicional.
- **Entrega frustrada como indicador separado do status:** se fosse só um valor de status, ele seria sobrescrito quando o pedido finalmente fosse entregue, e o P1 deixaria de ser mensurável.
- **Sem motoboy, compra ou estoque por quantidade**, porque a organização não registra esses processos (ver Processos de negócio).
- **MySQL 8 com InnoDB**, para ter FKs, `CHECK` (a partir da 8.0.16) e transações ACID (RNF04).

### 1.6 Proposta de Arquitetura Analítica para BI e IA

Os indicadores abaixo foram escolhidos entre os KPIs usuais de pizzarias de entrega e filtrados pelo que o banco consegue calcular. As metas são **propostas do grupo**, a serem confirmadas com o responsável. Todos os números citados vêm da massa de dados fictícia (seção 4), período de 22/09 a 18/10/2026.

**Informações estratégicas que o banco passa a oferecer**
- **Quanto entrou em cada dia, separado por forma de pagamento**, sem soma à mão no fim do expediente (P3).
- **Por que os pedidos são cancelados e quanto isso custa.** Na massa, 26 cancelamentos (4,4% dos pedidos) deixaram de faturar cerca de R$ 2.450; o motivo mais comum é a demora (P4).
- **Quantas entregas voltam por endereço desatualizado e de quais clientes** (P1).
- **Se os preços lançados batem com o cardápio depois de um reajuste** (P2).
- **O que está em falta, quais produtos saíram do cardápio por isso e a quem ligar** (P5).
- **Quais produtos sustentam a receita, em que horários o movimento se concentra e quantos clientes voltam a comprar**: base para promoções, escala de atendente e de motoboys e compras.

**Indicadores (KPIs)**

| KPI | Fórmula | Período | Meta proposta | Consulta |
| :---- | :---- | :---- | :---- | :---- |
| Faturamento diário por forma de pagamento | soma de `VL_TOTAL` dos pedidos não cancelados, por dia e por forma | dia | — (acompanhamento) | G01, G02 |
| Ticket médio | faturamento ÷ pedidos não cancelados | semana | ≥ R$ 100,00 | G03 |
| Taxa de cancelamento | pedidos cancelados ÷ pedidos registrados | semana | < 3% | G04 |
| Taxa de entregas frustradas | entregas com `IN_ENTREGA_FRUSTRADA` ÷ pedidos de entrega | semana | < 1% | G06 |
| Divergência de preço | itens com preço ≠ cardápio ÷ itens lançados desde o último reajuste | semana | 0% | G08 |
| Participação na receita (curva ABC) | receita do produto ÷ receita total, acumulada em ordem decrescente | período | — (classificação A/B/C) | G09 |
| Pedidos por hora | pedidos na faixa horária ÷ número de dias daquele dia da semana | período | — (dimensionamento) | G10 |
| Disponibilidade do cardápio | produtos disponíveis ÷ produtos cadastrados, por categoria | momento da consulta | ≥ 95% | G12 |
| Entregas com taxa e receita de taxa | entregas acima de 3 km ÷ entregas; soma de `VL_TAXA_ENTREGA` | semana | — (acompanhamento) | G14 |
| Taxa de recompra | clientes com 2 ou mais pedidos ÷ clientes cadastrados | período | — (acompanhamento) | G15 |

**Potencial dos dados para BI**
- **Fatos e dimensões já separados.** PEDIDO, ITEM_PEDIDO e PAGAMENTO são os fatos (o que aconteceu, com valor e data). CLIENTE, ENDERECO, PRODUTO, FORMAS_PAGAMENTOS, INSUMO e FORNECEDOR são as dimensões (por onde se agrupa). Isso permite ligar o MySQL diretamente ao Power BI ou ao Looker Studio e montar um modelo estrela sem reestruturar o banco.
- **Painéis sugeridos:**
  1. *Caixa do dia* (G01, G02), para o fechamento;
  2. *Semana* (G03, G04, G06, G14), para comparar semanas com as metas;
  3. *Cardápio e insumos* (G09, G11, G12, G13), para compras e promoções;
  4. *Movimento* (G10), em mapa de calor dia × hora.
- **Granularidade suficiente:** `DT_HORA` com minuto, item a item, com o preço cobrado na hora. Dá para recortar por dia, semana, hora, produto, bairro e forma de pagamento.

**Aplicações analíticas e de IA**
- **Previsão de demanda por dia e hora.** Com o histórico de `DT_HORA` (G10), um modelo de séries temporais estima quantos pedidos virão na próxima sexta às 20h, para decidir quantos motoboys chamar e quanta massa preparar.
- **Alerta de risco de entrega frustrada.** A partir de clientes com endereço antigo, tempo desde o cadastro do endereço e histórico de frustradas (S03, G07), o sistema avisa o atendente para confirmar o endereço antes de despachar (P1).
- **Sugestão de combinação no atendimento.** Itens que aparecem juntos no mesmo pedido (ITEM_PEDIDO) e sabores combinados no meio a meio (`CD_PRODUTO_2`) indicam sugestões como "quem pede Calabresa costuma levar Coca-Cola 2 L".
- **Clientes em risco de abandono.** Clientes que compravam com frequência e pararam (G15) podem receber uma mensagem de retorno pelo WhatsApp.
- **Previsão de falta de insumo.** Ainda **não é possível**: o banco não guarda quantidade em estoque (decisão da seção 1.5). Seria o próximo passo, se a pizzaria passar a registrar compras.

**Rastreabilidade**

| Problema ou KPI | Tabelas envolvidas | Consulta no script |
| :---- | :---- | :---- |
| P1 — endereço desatualizado e entrega frustrada | PEDIDO, ENDERECO, CLIENTE | G06, G07 (e S03, A10) |
| P2 — cardápio e pedidos desintegrados | ITEM_PEDIDO, PRODUTO, PEDIDO | G08 (e A06, A07) |
| P3 — fechamento de caixa fragmentado | PAGAMENTO, FORMAS_PAGAMENTOS, PEDIDO | G01, G02 |
| P4 — rastreabilidade de cancelamentos | PEDIDO, PAGAMENTO | G04, G05 |
| P5 — fornecedores sem cadastro | FORNECEDOR, INSUMO, PRODUTO_INSUMO, PRODUTO | G11, G12, G13 |
| KPI faturamento diário por forma | PAGAMENTO, FORMAS_PAGAMENTOS, PEDIDO | G01, G02 |
| KPI ticket médio | PEDIDO, PAGAMENTO | G03 |
| KPI taxa de cancelamento | PEDIDO | G04 |
| KPI taxa de entregas frustradas | PEDIDO | G06 |
| KPI divergência de preço | ITEM_PEDIDO, PRODUTO, PEDIDO | G08 |
| KPI participação na receita (ABC) | ITEM_PEDIDO, PRODUTO, PEDIDO | G09 |
| KPI pedidos por hora | PEDIDO | G10 |
| KPI disponibilidade do cardápio | PRODUTO | G12 |
| KPI entregas com taxa e receita de taxa | PEDIDO | G14 |
| KPI taxa de recompra | CLIENTE, PEDIDO | G15 |

---

## 2. Modelo Lógico Relacional

- Arquivo: [docs/modelo-logico.png](docs/modelo-logico.png) — criado no **brModelo Web** (app.brmodeloweb.com), a partir do DER conceitual revisado.
- 10 tabelas, 11 chaves estrangeiras e 11 ligações. Restrições `NOT NULL`, `UNIQUE`, `CHECK` e `DEFAULT` não aparecem no diagrama; estão no [dicionário de dados](docs/dicionario-de-dados.html), na seção 1.5 e no [script](docs/script.sql).

**Diferenças entre o diagrama e o script, por limitação da ferramenta**

O modelo lógico do brModelo Web tem duas limitações que afetam este diagrama:

1. **Não há o tipo `DECIMAL` na lista de tipos.** As colunas de valor aparecem como `FLOAT` no diagrama, mas no script são `DECIMAL`, que guarda o valor exato. `FLOAT` arredonda em binário e geraria diferença de centavos no fechamento de caixa.

| Coluna | No diagrama | No script |
| :---- | :---- | :---- |
| PRODUTO.VL_PRECO | FLOAT | DECIMAL(10,2) |
| ITEM_PEDIDO.VL_UNITARIO | FLOAT | DECIMAL(10,2) |
| PEDIDO.VL_TAXA_ENTREGA | FLOAT | DECIMAL(10,2) |
| PAGAMENTO.VL_TOTAL | FLOAT | DECIMAL(10,2) |
| PEDIDO.QT_DISTANCIA_KM | FLOAT | DECIMAL(5,2) |

2. **Toda ligação é desenhada automaticamente como (1,1) no lado da tabela referenciada e (0,n) no lado da chave estrangeira**, sem opção de editar. As cardinalidades reais, que vêm do DER conceitual e estão implementadas no script, são estas:

| Tabela referenciada | Tabela com a FK | Coluna FK | Cardinalidade real | Como o script garante |
| :---- | :---- | :---- | :---- | :---- |
| CLIENTE | ENDERECO | ID_CLIENTE | (1,1) — (0,n) | igual ao diagrama |
| CLIENTE | PEDIDO | ID_CLIENTE | (1,1) — (0,n) | igual ao diagrama |
| ENDERECO | PEDIDO | ID_ENDERECO | **(0,1)** — (0,n) | FK aceita nulo (pedido de retirada) |
| PEDIDO | ITEM_PEDIDO | ID_PEDIDO | (1,1) — **(1,n)** | todo pedido tem ao menos um item; conferido pela auditoria A02 |
| PRODUTO | ITEM_PEDIDO | CD_PRODUTO | (1,1) — (0,n) | igual ao diagrama |
| PRODUTO | ITEM_PEDIDO | CD_PRODUTO_2 | **(0,1)** — (0,n) | FK aceita nulo (item sem meio a meio) |
| PEDIDO | PAGAMENTO | ID_PEDIDO | (1,1) — **(1,1)** | `UNIQUE` em `PAGAMENTO.ID_PEDIDO`; pedido sem pagamento é conferido pela auditoria A01 |
| FORMAS_PAGAMENTOS | PAGAMENTO | ID_FORMA | (1,1) — (0,n) | igual ao diagrama |
| FORNECEDOR | INSUMO | ID_FORNECEDOR | **(0,1)** — (0,n) | FK aceita nulo (insumo comprado direto pelo dono) |
| PRODUTO | PRODUTO_INSUMO | CD_PRODUTO | (1,1) — (0,n) | igual ao diagrama |
| INSUMO | PRODUTO_INSUMO | ID_INSUMO | (1,1) — (0,n) | igual ao diagrama |

As linhas em negrito são as que o diagrama não consegue mostrar.

---

## 3. Script SQL Completo

- Arquivo: [docs/script.sql](docs/script.sql) — **MySQL 8.0** (testado na versão 8.0.46), executa do zero em banco vazio e pode ser executado de novo (começa com `DROP DATABASE IF EXISTS`).
- Conteúdo, nesta ordem: `CREATE DATABASE` → `CREATE TABLE` com PK, FK, `NOT NULL`, `UNIQUE`, `CHECK` e `DEFAULT` → `INSERT` da massa de dados → `SELECT` com `JOIN` → `UPDATE` → `DELETE` → consultas gerenciais (`G01`…) → consultas de auditoria (`A01`…).
- **Teste:** executado do zero duas vezes seguidas com o "safe update mode" ligado (o mesmo do MySQL Workbench), sem erros. A única mensagem que não é tique verde é o aviso 1008 do `DROP DATABASE IF EXISTS` na primeira execução, porque o banco ainda não existia.
- **Parâmetros:** o início da seção 5 define `@INICIO`, `@FIM`, `@DIA_CAIXA` e `@DT_ULTIMO_REAJUSTE`. Basta trocar as datas ali para rodar as consultas para outro período ou outro dia de caixa.

| Seção | Códigos | O que faz |
| :---- | :---- | :---- |
| 5 — SELECT com JOIN | S01 a S04 | comanda completa do pedido (7 tabelas); busca do cliente pelo telefone (RN09); histórico de endereços (P1); composição dos produtos com fornecedor (RF08) |
| 6 — UPDATE | U01 a U04 | entrega frustrada em transação (RN10); insumo acabou e produtos saem do cardápio (RN04); cancelamento com motivo (RN02); troca de telefone |
| 7 — DELETE | D01, D02 | insumo retirado de uma receita; exclusão de cliente sem pedidos a pedido dele (LGPD) |
| 8 — Gerenciais | G01 a G15 | ver a tabela de rastreabilidade na seção 1.6 |
| 9 — Auditoria | A01 a A10 | tabela abaixo |

Nas seções 6 e 7 há dois comandos **comentados** que mostram as restrições em ação: cancelar sem motivo (o `CHECK` recusa, erro 3819) e apagar cliente com pedidos (a FK recusa, erro 1451). Para demonstrar, basta descomentá-los e executar.

**Consultas de auditoria** — cada uma lista o que viola a regra; resultado vazio significa que a regra está sendo respeitada.

| Código | Regra verificada | Resultado na massa de dados |
| :---- | :---- | :---- |
| A01 | RN06 — todo pedido tem pagamento | pedidos 41 e 576 |
| A02 | RN06 / RF02 — todo pedido tem itens | pedido 41 |
| A03 | RN07 — troco só no dinheiro | pedido 459 (cartão com troco) |
| A04 | RN08 — PIX só sai com comprovante | pedidos 342 e 343 |
| A05 | RN17 — total = itens + taxa | pedidos 398 e 408 (taxa esquecida) |
| A06 | RN13 — meio a meio pelo sabor mais caro | pedido 331 |
| A07 | RN01 / P2 — preço lançado = cardápio | pedidos 308, 311, 322 e 324 (preço anterior ao reajuste) |
| A08 | RN18 — produto indisponível em pedido aberto | pedido 593 (Sprite lata) |
| A09 | RN04 — produto disponível com insumo em falta | pizza Escarola |
| A10 | RN10 — um único endereço ativo por cliente | vazio (regra respeitada, inclusive depois do U01) |

As demais regras (RN02, RN05, RN09, RN12, RN14, RN15, RN16, RN19) não têm consulta de auditoria porque são garantidas por `CHECK`, `UNIQUE` ou FK: o banco recusa o dado errado no momento da gravação.

**Limitação conhecida:** o banco guarda só o preço atual do produto, não o histórico de preços. Por isso, A06, A07 e G08 comparam o preço lançado com o cardápio apenas a partir da data do último reajuste (`@DT_ULTIMO_REAJUSTE`).

---

## 4. Massa de Dados

A massa de dados está no próprio [script](docs/script.sql), na seção 4. É **fictícia**: nenhum nome, telefone ou endereço é de cliente real. Os nomes de fornecedores reais (Coca-Cola e PMG) foram mantidos porque são empresas citadas pelo responsável, não pessoas; os telefones deles são fictícios.

**Período:** 4 semanas, de terça 22/09/2026 a domingo 18/10/2026, só em dias e horários de funcionamento (RN14). Quatro semanas permitem comparar semanas entre si e cobrem a virada de setembro para outubro.

| Tabela | Registros | Justificativa pelo volume real |
| :---- | ----: | :---- |
| PEDIDO | 594 | ~150 pedidos/semana × 4 semanas, distribuídos como na operação: ~17 na terça e quarta, ~21 na quinta, ~23 no domingo, ~35 na sexta e no sábado; picos entre 19h e 21h |
| ITEM_PEDIDO | 1.325 | média de 2,2 itens por pedido: 1 a 3 pizzas e, em cerca de 60% dos pedidos, uma ou duas bebidas; 216 itens meio a meio |
| PAGAMENTO | 592 | um por pedido (RN06), inclusive cancelados; PIX 46%, cartão 33%, dinheiro 20%; ticket médio ≈ R$ 100 |
| CLIENTE | 160 | clientes recorrentes de um bairro: um mês de 594 pedidos concentrado em ~135 clientes que compraram; 25 cadastrados não compraram no período |
| ENDERECO | 132 | um por cliente de entrega; 22 clientes têm dois (o antigo inativo): 14 mudaram sem avisar e 8 avisaram; 35 clientes só retiram e não têm endereço |
| PRODUTO | 34 | cardápio: 19 pizzas em ordem alfabética com código fixo (58 = Mussarela), 6 brotos, 3 bebidas, 6 refrigerantes |
| INSUMO | 41 | ingredientes, embalagens e bebidas usados pelos produtos |
| PRODUTO_INSUMO | 173 | composição de cada produto |
| FORNECEDOR | 3 | os três fornecedores levantados |
| FORMAS_PAGAMENTOS | 3 | dinheiro, PIX e cartão (RN03) |

**Situações cobertas, para que as consultas tenham o que mostrar:**
- **P1:** 14 entregas frustradas, todas apontando para o endereço antigo (agora inativo) do cliente; uma delas terminou cancelada.
- **P2:** reajuste de preço da Calabresa (R$ 54 → R$ 56) e da Coca-Cola 2 L (R$ 14 → R$ 15) em 06/10; itens antigos guardam o preço da época e 4 itens posteriores foram lançados com o preço antigo.
- **P3:** pagamentos nas três formas em todos os dias.
- **P4:** 25 cancelamentos com 7 motivos diferentes, em pedidos de entrega e de retirada.
- **P5:** insumos com e sem fornecedor; Palmito, Sprite lata e Escarola em falta, com os produtos correspondentes indisponíveis.
- **Situações em aberto:** 3 pedidos do último dia ainda em preparo ou a caminho, usados nos exemplos de `UPDATE`.
- **Inconsistências inseridas de propósito**, listadas no cabeçalho da seção 4 do script, para que as consultas de auditoria mostrem resultado: troco em pagamento com cartão (RN07), PIX entregue sem comprovante (RN08), meio a meio cobrado pelo sabor mais barato (RN13), total sem a taxa (RN17), produto indisponível em pedido aberto (RN18), pedido sem pagamento e pedido sem itens (RN06), produto disponível com insumo em falta (RN04).

**Geração:** a massa foi gerada por um script Python escrito com o Claude (ver seção 6), com semente aleatória fixa — rodar de novo gera os mesmos dados. As regras do negócio foram aplicadas na geração; as restrições do banco (`CHECK`, FK) recusariam qualquer linha fora delas.

---

## 5. Repositório GitHub

- Relatório: este `README.md`
- Levantamento: [levantamento/levantamento.md](docs/levantamento.md)
- DER conceitual: [docs/DER-PIZZARIA-revisado](docs/DER-PIZZARIA-revisado_page-0001.jpg)
- Dicionário de dados: [docs/dicionario-de-dados.html](docs/dicionario-de-dados.html)
- README da Entrega 1 (histórico): [docs/READMEentrega1.md](docs/READMEentrega1.md)

---

## 6. Uso de Inteligência Artificial

Usos 1 e 2: Entrega 1 (Gemini). Uso 3: transição para a Entrega 2. Usos 4 a 7: Entrega 2.

### Uso 1 — Revisão técnica e correção do Diagrama Entidade-Relacionamento (DER)
- **Ferramenta e etapa**
*Gemini: Utilizado na etapa de Modelagem Conceitual (Seções 6, 7 e 8) para revisão do Diagrama Entidade-Relacionamento desenhado no software brModelo.*
- **Motivação**
*Precisávamos validar se o diagrama construído possuía erros de lógica, redundâncias ou violações das regras formais de banco de dados antes da entrega final.*
- **Prompt(s) utilizados**
*"Me ajude a corrigir os erros antes de realizar o readme então, me explique de forma simples e objetiva do que mudar e por que."*
- **Resposta recebida**
*A IA apontou falhas críticas de normalização, como a criação de entidades de ação ("Checkout"), a alocação incorreta de atributos ("Endereço" ligado a "Delivery" em vez de "Cliente") e o uso de dados literais como estrutura (atributos "Broto" e "Cerveja" em vez de um genérico "Tamanho" ou "Nome"). A IA orientou a exclusão das entidades fantasmas e a unificação dos relacionamentos na entidade "Pedido".*
- **Fontes consultadas e verificadas**
*As correções sugeridas pela IA foram validadas pelo grupo através do cruzamento com os materiais didáticos da disciplina, especificamente os conceitos de não transformar processos (verbos) em entidades (substantivos) e a exigência de identificadores únicos.*
- **Trechos rejeitados ou corrigidos**
*A aplicação no brModelo foi feita de forma 100% manual pelo grupo, adaptando as sugestões visuais da IA para a interface da ferramenta (como a realocação das linhas de cardinalidade dos losangos "Faz" e "Contém").*
- **Justificativa da escolha final**
*As orientações da IA foram acatadas porque enxugaram o modelo, garantindo a independência da tecnologia e alinhando o diagrama à 3ª Forma Normal na abstração conceitual, facilitando a futura implementação em SQL.*
- **Reflexão crítica**
*A IA foi muito eficaz na revisão de regras de negócio lógicas, mas possui a limitação de não poder editar o diagrama visualmente. Tivemos que descrever ou exportar imagens sucessivas para que ela pudesse "enxergar" as conexões, o que exige um bom domínio da nossa parte para aplicar as correções no software.*
---

### Uso 2 — Estruturação dos Requisitos, Regras de Negócio e Descrição do Repositório
- **Ferramenta e etapa**
*Gemini: Utilizado nas etapas de planejamento de levantamento de dados, redação e aperfeiçoamento dos Processos de Negócio, Requisitos e Regras (Seções 2, 3 e 4) e elaboração da Introdução do GitHub.*
- **Motivação**
*Auxiliar a transformar o fluxograma inicial e as observações empíricas colhidas na pizzaria em um formato de texto técnico e estruturado (linguagem de Engenharia de Software).*
- **Prompt(s) utilizados**
*"Monte uma lista de perguntas que podem ser feitas na entrevista... Simplifique as explicações." e "Seguindo o markdown que te enviei, estou na seção ##3. Precisamos de ajuda no preenchimento, transforme *texto* em uma linguagem mais técnica e formal."*
- **Resposta recebida**
*A IA gerou um roteiro de entrevista prático focado nas "dores" da pizzaria (como o caos nos pedidos de sexta-feira) e, posteriormente, redigiu os Requisitos Funcionais (ex: RF01 - Gestão de Clientes), Requisitos Não Funcionais e Regras de Negócio (ex: Obrigatoriedade de vínculo financeiro) baseados nas cardinalidades do nosso DER.*
- **Fontes consultadas e verificadas**
*O conteúdo das regras geradas foi confrontado com a realidade da pizzaria analisada pelo membro do grupo que trabalha no local e a entrevista realizada pelos membros, garantindo que os gargalos propostos correspondiam ao mundo real.*
- **Trechos rejeitados ou corrigidos**
*Durante a geração de contexto, a IA "alucinou" afirmando que os alunos eram de semestres avançados e que possuíam conhecimentos de especialistas,  corrigimos ela para nossa realidade  atual e a mantemos com uma linguagem técnica mas explicativa conosco. Auxiliando no melhor entendimento de suas ações.*
- **Justificativa da escolha final**
*A estruturação técnica do texto foi mantida pois traduziu perfeitamente o comportamento do fluxograma, do modelo conceitual e dos textos bases construído pelo grupo, com jargões adequados à disciplina.*
- **Reflexão crítica**
*A alucinação da IA ao tentar adivinhar ao nos tratar como especialistas com anos de experiência serviu como um excelente lembrete de que o texto gerado não pode ser apenas "copiado e colado"  e que causaria obviamente uma enorme confusão e nos tiraria o aprendizado que o projeto proporciona. É necessário ler criticamente e corrigir a máquina para que o documento reflita exatamente a realidade do grupo.*
---

### Uso 3 — Correção do README da Entrega 1, análise de inconsistências e instruções para o DER e o dicionário
- **Ferramenta e etapa**
*Claude (Anthropic): utilizado na transição para a Entrega 2, na revisão do README corrigido da Entrega 1 (Seções 1, 2, 3, 4, 6, 7 e 8), na comparação com a entrevista, o dicionário de dados e a Issue #26, e na elaboração de instruções para atualizar o DER e o dicionário.*
- **Motivação**
*Prazo curto e necessidade de alinhar o README às exigências do esqueleto da Entrega 2 e aos apontamentos do professor, sem perder o que já estava feito.*
- **Prompt(s) utilizados**
*"Siga rigorosamente as regras descritas [no esqueleto da Entrega 2] ... Se faltar informações ... me pergunte antes de produzir qualquer coisa"; "Corrija então com base nos dados que você tem do nosso projeto, as seções do README corrigido que você citou"; "Corrija tudo que ficou em aberto, e me instrua a corrigir o dicionário e o DER", seguido das respostas do responsável (dias e volume, taxa de entrega, meio a meio, horário, fornecedores, registros).*
- **Resposta recebida**
*A IA apontou contradições entre o README, a entrevista e o dicionário (ex.: P1 com a lógica do endereço invertida, catálogo de produtos separado na Seção 8 e unificado na Seção 6, atributo de disponibilidade ausente no dicionário), reescreveu as seções afetadas e propôs ajustes de modelagem: Fornecedor, Insumo, vínculo Pedido-Endereço, segundo sabor, taxa de entrega.*
- **Fontes consultadas e verificadas**
*Issue #26 do professor, esqueleto da Entrega 2, entrevistas, README e dicionário do grupo, e respostas do responsável. A IA localizou na web a RDC nº 216/2004 (Anvisa) e a Portaria CVS nº 5/2013 (SP).*
- **Trechos rejeitados ou corrigidos**
*A IA deixou como pendentes os pontos que só o responsável poderia confirmar, em vez de inventar respostas; as pendências foram preenchidas com as respostas dele. A assunção de pagamento único por pedido foi confirmada pelo responsável. A regra de taxa foi redigida como R$ 1,00 por km excedente aos 3 km, interpretação da IA sobre o relato; depois o responsável informou a regra real (R$ 5,00 fixos acima de 3 km, entrega até 8 km), e o texto foi corrigido (ver Uso 4).*
- **Justificativa da escolha final**
*As correções foram mantidas porque eliminam contradições que se propagariam para o modelo lógico, a normalização e o script SQL, e porque cada regra ficou ligada a um dado informado pelo responsável.*
- **Reflexão crítica**
*A IA só enxerga os arquivos enviados e depende das respostas do responsável: sem elas, parte do levantamento ficaria presa em suposições. A revisão mostrou que texto gerado por IA na Entrega 1 (Gemini) tinha sido aceito sem conferência com a entrevista, o que gerou contradições. Toda saída da IA precisa ser checada contra a realidade da pizzaria.*

---

### Uso 4 — Preenchimento do relatório (seções 1.1 a 1.5), levantamento e resposta à correção da Entrega 1

- **Ferramenta e etapa**
*Claude (Anthropic), modelo Claude Opus 5.5, em sessão com acesso ao repositório do grupo. Usado na revisão do modelo conceitual (seção 1.1: caracterização, relato da entrevista, processos, requisitos, regras e a tabela do Caso A), na conversão para o lógico, nas chaves, na normalização e nas justificativas técnicas (seções 1.2 a 1.5), e na organização do arquivo `levantamento/levantamento.md`.*
- **Motivação**
*Prazo curto para a 2° entrega e um rascunho das seções 1.2 e 1.3 que o grupo não sabia se estava correto. Também precisávamos responder item por item à correção da Entrega 1, que teve nota 0,723 de 1,60.*
- **Prompt(s) utilizados**
*Instruções fixas do projeto, configuradas antes da conversa: "Precisamos realizar uma modelagem quase completa de banco de dados para a Pizzaria Santoluppo [...]. O contexto anexado é o esqueleto que precisamos preencher para a 2ª entrega [...]. Faça uma varredura completa no nosso projeto no GitHub [...] e nos ajude a preencher todos os requisitos da 2ª entrega. [...] Trabalhe em cima do que já temos."*

  *Instruções de comportamento da IA, configuradas pelo grupo: "Você é meu conselheiro, não meu assistente. Seu trabalho é acertar, não concordar. [...] Se a minha ideia tiver uma falha, uma lacuna ou uma suposição arriscada, diga isso na primeira frase. [...] Marque sua confiança nas afirmações principais: [Certo] para evidência sólida, [Provável] para inferência forte, [Chute] quando estiver preenchendo lacunas. [...] Quando eu estiver errado, use esta estrutura: 'Discordo porque [motivo]. O que eu faria no lugar: [alternativa]. O risco do seu caminho é [desvantagem específica]'."*

  *Prompts da conversa: "Vamos dar início ao preenchimento do arquivo anexado no contexto; nosso projeto está no repositório [link] para sua leitura." Em seguida, a correção da Entrega 1 colada na íntegra, com os dados confirmados pelo responsável: "A 1ª entrevista foi em 28/09; telefone do Sr. Antônio: (11) 99575-9922. Acima de 3 km a taxa é R$ 5,00; o máximo é 8 km."*
- **Resposta recebida**
*A IA leu o README e o dicionário da E1, o relato das entrevistas e o DER revisado, e apontou erros no rascunho do grupo antes de preencher:*
  - *`FLOAT` para valores em dinheiro;*
  - *falta da chave do cliente no endereço, o que impedia o histórico de endereços;*
  - *falta da tabela do relacionamento N:N entre Produto e Insumo;*
  - *P1 impossível de medir, porque "endereço não localizado" era só um valor de status, sobrescrito quando o pedido era entregue.*

  *Em seguida, redigiu as seções 1.1 a 1.5:*
  - *12 requisitos funcionais, cada um ligado a um problema;*
  - *19 regras de negócio;*
  - *a tabela do Caso A com cada apontamento da correção;*
  - *a normalização com exemplos do próprio modelo.*

  *Também propôs incluir `IN_ENTREGA_FRUSTRADA` no pedido e `IN_ATIVO`/`DT_CADASTRO` no endereço.*
- **Fontes consultadas e verificadas**
*Correção da E1, esqueleto da Entrega 2, entrevistas e respostas do responsável. As regras sobre o funcionamento da pizzaria (horário, taxa, meio a meio, forma única de pagamento) vieram do responsável, e não da IA. As normas sanitárias citadas são as do Uso 3.*
- **Trechos rejeitados ou corrigidos**
  - *A IA tinha escrito a regra de taxa como "R$ 1,00 por km excedente", herdada do Uso 3. O responsável informou a regra real (R$ 5,00 fixos acima de 3 km e limite de 8 km), e a IA reescreveu RN12, criou a RN19 e transformou a taxa em `CHECK` no banco.*
  - *A IA apontou que a data de 27/09 não podia ser a da segunda visita, já que a primeira foi em 28/09, e deixou o campo para o grupo preencher em vez de inventar uma data.*
  - *A regra de cancelamento depois do início do preparo, cobrada pelo professor, ficou marcada como pendente, porque só o responsável pode confirmá-la.*
- **Justificativa da escolha final**
*As correções foram mantidas porque cada uma tem consequência concreta: com `FLOAT`, o fechamento de caixa teria diferença de centavos; sem a FK do cliente no endereço, o P1 não teria histórico; sem a tabela N:N, o P5 não teria modelo.*
- **Reflexão crítica**
*Configuramos a IA para apontar falhas antes de concordar, e foi isso que expôs os erros do rascunho. Mesmo assim, a IA errou ao manter a regra de taxa do Uso 3 sem questioná-la; só a resposta do responsável corrigiu. Regra de negócio precisa vir da organização, não de interpretação da IA.*

---

### Uso 5 — Correção do DER conceitual e do modelo lógico no brModelo

- **Ferramenta e etapa**
*Claude (Anthropic), na correção do DER conceitual (seção 1.1) e na conferência do modelo lógico (seção 2).*
- **Motivação**
*O DER da E1 perdeu 0,200 por associativa sem atributos e por faltar Insumo e Fornecedor. O grupo precisava de instruções que outro integrante pudesse seguir sozinho no brModelo.*
- **Prompt(s) utilizados**
*"Explique como corrigir o DER e o modelo lógico, de forma que outro integrante consiga finalizar sozinho." Depois, a cada nova versão, o print do diagrama com a pergunta "Este DER atende o seu guia?"; o mesmo para o modelo lógico. Por fim, a limitação da ferramenta: "O brModelo não tem o tipo DECIMAL, e as cardinalidades das ligações do modelo lógico vêm num padrão fixo que não conseguimos modificar."*
- **Resposta recebida**
*A IA escreveu um arquivo com três partes:*
  - *tabela "hoje × corrigir para" de cada relacionamento;*
  - *as 10 tabelas e as 11 ligações do modelo lógico;*
  - *as alterações no dicionário.*

  *A cada print, conferiu cardinalidades, identificadores e atributos, e listou o que faltava. No modelo lógico, apontou chaves estrangeiras na tabela errada (`idPedido` em Endereço, `ID_INSUMO` em Produto e em Fornecedor) e colunas faltando (`ID_ENDERECO` em Pedido, `CD_PRODUTO` em Item_pedido).*
- **Fontes consultadas e verificadas**
*Notação do brModelo descrita no esqueleto da Entrega 2. A leitura das cardinalidades foi conferida contra a correção da E1: o professor aceitou o relacionamento "Faz" e cobrou o (1,1) do lado das formas de pagamento.*
- **Trechos rejeitados ou corrigidos**
  - *Uma instrução da IA ("apaguem o losango Contém e as duas linhas dele") era ambígua e levou o grupo a apagar também a ligação entre Item_pedido e Produto. A IA identificou o próprio erro no print seguinte e corrigiu a instrução.*
  - *Um dos prints era de uma versão antiga do arquivo. A IA comparou com o print anterior e mostrou a regressão.*
  - *O brModelo não oferece `DECIMAL` e não permite editar a cardinalidade das ligações do modelo lógico. Em vez de forçar o diagrama, a IA propôs documentar as diferenças na seção 2 do README, com duas tabelas: tipos do diagrama × tipos do script, e cardinalidades reais.*
- **Justificativa da escolha final**
*Documentar a limitação da ferramenta é mais honesto do que deixar o diagrama dizer uma coisa e o script fazer outra sem explicação.*
- **Reflexão crítica**
*A IA não edita o diagrama; o trabalho dependeu de prints e de alguém aplicar cada correção à mão, e foram necessárias quatro rodadas no DER. A IA também errou numa instrução. Conferir cada versão contra uma lista fechada (o guia) foi o que impediu que erros passassem.*

---

### Uso 6 — Geração da massa de dados

- **Ferramenta e etapa**
*Claude (Anthropic), na geração da massa de dados (seção 4 do script).*
- **Motivação**
*O esqueleto exige uma massa de dados fictícia, coerente com o volume real e com variedade suficiente para que as consultas gerenciais e de auditoria tenham o que mostrar. Fazer centenas de `INSERT` à mão era inviável no prazo.*
- **Prompt(s) utilizados**
*"Pode começar com a massa de dados [...] e nos instrua também na instalação do MySQL e na execução do script." Na instalação, correções com base nos problemas encontrados e, depois, o resultado da execução para conferência.*
- **Resposta recebida**
*A IA escreveu um gerador em Python, com semente aleatória fixa (rodar de novo gera os mesmos dados), que produz os `INSERT` a partir dos números da pizzaria:*
  - *~150 pedidos por semana, distribuídos de terça a domingo, com picos na sexta e no sábado entre 19h e 21h;*
  - *cardápio com o código 58 para a mussarela;*
  - *taxa de entrega de R$ 5,00 acima de 3 km e limite de 8 km;*
  - *um reajuste de preço em 06/10.*

  *Também inseriu de propósito 14 erros de registro, para que as auditorias A01 a A09 tenham o que encontrar. A IA carregou a massa num MySQL 8.0.46 com todas as constraints ligadas, para garantir que nenhuma linha violasse as regras. Depois, deu o passo a passo de instalação do MySQL no Windows.*
- **Fontes consultadas e verificadas**
  - *Volumes: seção "Contexto e porte" (6 colaboradores, 150 pedidos por semana, horário e dias de funcionamento).*
  - *Instalação: página oficial de downloads do MySQL para Windows ([dev.mysql.com/downloads/windows](https://dev.mysql.com/downloads/windows/)), consultada pela IA para confirmar que o MySQL Installer só existe até a série 8.0.*
  - *O grupo rodou o script no MySQL Workbench e conferiu a contagem de registros de cada tabela contra a informada pela IA (160 clientes, 594 pedidos, 1.325 itens, 592 pagamentos).*
- **Trechos rejeitados ou corrigidos**
  - *Na primeira versão, pedidos de retirada eram cancelados com o motivo "demora na entrega", o que não faz sentido. O motivo passou a ser "demora para ficar pronto".*
  - *O erro proposital do meio a meio tinha caído num pedido anterior ao reajuste de preços, em que a auditoria não consegue compará-lo. Foi movido para depois.*
  - *O número de clientes sem compra no período estava errado no comentário do script (15, quando o correto era 25) e foi corrigido.*
  - *Na instalação, o Workbench não encontrou o servidor porque a etapa de configuração do MySQL Server não tinha sido feita; a IA orientou refazer a configuração pelo MySQL Installer.*
- **Justificativa da escolha final**
*A massa foi mantida porque segue as regras reais da pizzaria e porque o próprio banco recusaria qualquer linha fora delas. Os erros propositais estão listados no cabeçalho da seção 4 do script, para não serem confundidos com descuido.*
- **Reflexão crítica**
*Dado gerado por IA parece realista mesmo quando é incoerente (cancelamento "pela demora na entrega" em pedido de retirada), e esse erro passou pelas constraints. Só a leitura dos resultados encontrou o problema. Os nomes e telefones são inventados; os nomes reais de fornecedores (Coca-Cola e PMG) foram mantidos porque são empresas citadas pelo responsável, mas os telefones deles também são fictícios.*

---

### Uso 7 — Consultas SQL (seções 5 a 9 do script) e arquitetura analítica (seção 1.6)

- **Ferramenta e etapa**
*Claude (Anthropic), na escrita das consultas `SELECT` com `JOIN`, `UPDATE`, `DELETE`, gerenciais (G01–G15) e de auditoria (A01–A10), e na seção 1.6 (KPIs, BI, IA e rastreabilidade).*
- **Motivação**
*O script vale 12% e a seção 1.6 vale 7,5%. O esqueleto exige pelo menos uma consulta por problema e por KPI e uma auditoria citando cada regra, o que o grupo não conseguiria escrever e testar a tempo.*
- **Prompt(s) utilizados**
*Continuação da conversa do Uso 6. Depois de o grupo confirmar a carga ("Os números batem e não encontrei nenhum erro no caminho"), a IA seguiu para as seções 5 a 9 do script e a seção 1.6, conforme o plano que tinha proposto ("Sigo agora com as consultas e a seção 1.6 de KPIs?"), usando como requisito o esqueleto da Entrega 2: uma consulta por problema e por KPI, e uma auditoria citando cada regra.*
- **Resposta recebida**
*A IA escreveu as consultas e executou o script inteiro duas vezes, do zero, no MySQL 8.0.46 com o "safe update mode" ligado (o mesmo do Workbench), sem erros. Também:*
  - *conferiu que cada auditoria encontra exatamente os erros propositais da massa e que a A10 volta vazia;*
  - *redigiu a seção 1.6 com 10 KPIs (fórmula, período, meta e consulta) e a tabela de rastreabilidade problema/KPI → tabelas → consulta.*
- **Fontes consultadas e verificadas**
*Os KPIs seguem indicadores comuns de pizzarias de entrega (ticket médio, taxa de cancelamento, curva ABC, pedidos por hora), filtrados pelo que o banco consegue calcular. As metas (ticket médio ≥ R$ 100, cancelamento < 3%, entregas frustradas < 1%, disponibilidade do cardápio ≥ 95%) são **propostas do grupo, sem fonte externa**, e ainda precisam ser confirmadas com o responsável. Os resultados de cada consulta foram conferidos executando o script.*
- **Trechos rejeitados ou corrigidos**
  - *Para conferir preços (P2) seria preciso o histórico de preços dos produtos. Uma nova coluna de data de reajuste foi considerada e rejeitada, porque obrigaria a alterar de novo o DER, o modelo lógico e o dicionário a um dia da entrega. A conferência passou a valer só a partir do último reajuste, e isso ficou declarado como limitação na seção 3.*
  - *Os exemplos de erro (cancelar sem motivo, apagar cliente com pedidos) ficaram comentados no script, para não interromper a execução; são descomentados só na demonstração.*
- **Justificativa da escolha final**
*Cada consulta responde a uma pergunta de gestão escrita em português no cabeçalho e foi conferida com o resultado real. A limitação do histórico de preços foi assumida em vez de escondida.*
- **Reflexão crítica**
*A IA escreve SQL correto mais rápido do que o grupo, mas isso cria o risco de apresentarmos consultas que não sabemos explicar. Por isso, cada integrante deve executar e entender as consultas que vai mostrar na apresentação. Outro limite: os resultados vêm de dados fictícios. Ter taxa de cancelamento acima da meta demonstra a consulta, não a realidade da pizzaria.*

---

## 7. Apresentação Corporativa Final

*Anexado em: [Slides Apresentação Final](docs/Pizzaria Santoluppo — Pitch Final.pptx).
