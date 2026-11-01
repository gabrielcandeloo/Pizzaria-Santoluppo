### Modelagem de um sistema de gestão de informações para uma organização de pequeno porte

---

## Metadados

- Davi Ramos de Oliveira - RGM 49576216
- Gabriel Silva Candelo - RGM 48050938
- John Antoni Rodrigo Quispe - RGM 49450492
- Leon Correia da Silva - RGM 48036374
- Pablo Lorran Ferreira da Silva - RGM 48146978

---

## 1. Caracterização da Organização

- **Pizzaria Santoluppo:**
- **Contexto e porte:** *Uma empresa com fins lucrativos do ramo alimentício, com uma operação envolvendo aproximadamente 6 colaboradores, com um volume de 25 pedidos por dia.*
- **Problemas e necessidades identificados:** *Um ponto a mencionar é a ausência de um fornecedor, os funcionários repõem o estaque mediante a falta de determinado item, realizando compras em um estabelecimento próximo. Para a realização da entrega ao cliente, o entregador verificar o número de telefone se for o mesmo não houve alteração no endereço, mas se o número de telefone for mudado há a possiblidade da entregar ser realizada no local errado.*
- **Justificativa da escolha:** *A escolha se deu pelo fácil acesso a toda estrutura necessária para o projeto, desde acesso ao local até a quantidade real de dados que são coletados diariamente*
- **Evidências da organização:** *Redes Sociais: Instagram https://www.instagram.com/santoluppopizzaria/ - Endereço: Av. Sapopemba 1333 Vila Reg. Feijó CEP:03345-001 - Contato: Telefone(11) 2028-8486 - WhatsApp (11) 995585599 - Fotos no local: [Frente da Pizzaria](docs/Pizzaria-Frente.jpeg) - [Membros presentes: Pablo Lorran, Davi Ramos, Leon Correia e John Antoni](docs/membros-entrevista.jpeg)*

---

## 2. Processos de Negócio

**Principais processos mapeados:**

*Gestão de Cardápio: Os sabores são organizados em ordem alfabética, cada um com uma numeração própria que nunca se repete (ex: 58 para mussarela). O cadastro de novos produtos e a alteração de preços são feitos em um sistema à parte, onde os itens são classificados em pizzas, brotos, bebidas ou refrigerantes. O cadastro armazena o nome do produto, o valor monetário, a categoria, o código numérico identificador único e o indicador de disponibilidade, usado para retirar o item do cardápio quando falta ingrediente.*

*Fluxo de Pedidos: O atendimento (telefone, WhatsApp ou balcão) é feito por uma pessoa, pois muitos clientes têm dúvidas ou querem alterar ingredientes. Todo pedido é de entrega ou de retirada; não há consumo no salão e, portanto, não há número de mesa. Ao digitar o telefone, o cadastro do cliente aparece; se o telefone mudou, o endereço é confirmado. O pedido associa o cliente, o tipo (entrega ou retirada), o endereço de entrega (rua, número, bairro e CEP) nos pedidos de entrega e, para cada item, o código do produto, a quantidade e a observação para a cozinha. Na pizza meio a meio, o item registra dois sabores e é cobrado o valor do mais caro. Na entrega, a distância até o endereço define a taxa: até 3 km não há cobrança; acima disso, cobra-se R$ 1,00 por km excedente. O código numérico facilita a impressão do pedido. A pizzaria só atende no horário de funcionamento.*

*Produção e Expedição: O pedido impresso segue para a cozinha com os itens, as quantidades e as observações. Pronto, é despachado ao motoboy (entrega) ou entregue no balcão (retirada). A pizzaria não mantém registro dos motoboys nem da distribuição dos pedidos entre eles. Se o motoboy não encontra ninguém no endereço, avisa a pizzaria, que contata o cliente para confirmar o novo endereço; o modelo registra essa ocorrência no pedido.*

*Pagamento: A forma de pagamento é perguntada na hora do pedido e anotada. Dinheiro: pergunta-se se o cliente precisa de troco. Pix: envia-se a chave da empresa e o cliente manda o comprovante antes de o motoboy sair. Cartão: o motoboy leva a maquininha. Cada pedido tem uma única forma de pagamento; não existe pagamento dividido. O valor do pagamento é a soma dos itens e da taxa de entrega.*

*Cancelamento de Pedidos: A desistência de uma venda exige o registro do identificador do pedido cancelado acompanhado de um atributo de texto livre obrigatório contendo o motivo do cancelamento, sem disparar gatilhos matemáticos de devolução de estoque, já que, na maioria dos casos, o prato ainda não havia sido preparado. O motivo é informado ao iniciar o cancelamento e revisado ao final do expediente; o pedido cancelado permanece no histórico. Quando um pedido sai errado, a pizzaria arca com o prejuízo da troca, e essa troca não é registrada.*

*Fechamento de Caixa: A apuração financeira diária cruza o valor total dos pedidos expedidos e os agrupa de acordo com o dado categórico da forma de pagamento registrada no momento da venda (cartão, dinheiro e PIX). A soma dos três totais é o valor vendido na noite.*

*Cadastro de Fornecedores e Reposição de Insumos: Os fornecedores são poucos e cada um atende uma parte: um conhecido do dono para as caixas de pizza, a Coca-Cola para alguns refrigerantes e a PMG para mercadoria geral (pedida com entrega apenas raramente). A Coca-Cola e a PMG enviam a mercadoria à pizzaria. O cadastro armazena o nome, o tipo de fornecimento (embalagens, bebidas ou mercadoria geral), o telefone de contato e se o fornecedor envia a mercadoria. O dono repõe os ingredientes pessoalmente; quando um insumo acaba, ele é marcado como indisponível e os produtos que o utilizam são retirados do cardápio.*

**Justificativas de processos ausentes:**

*Estoque Físico por quantidade: O abatimento de ingredientes grama a grama não é executado nativamente pela organização, pois o controle é simples: o próprio dono repõe os ingredientes. O trâmite é substituído pela verificação visual do operador, exigindo que o modelo contemple apenas o indicador de disponibilidade do insumo e do produto, sem quantidade em estoque.*

*Pedido de Compra e Recebimento de Mercadoria: A organização não mantém registro de compras nem de recebimento de mercadoria: o dono repõe os ingredientes pessoalmente e a mercadoria enviada pela Coca-Cola e pela PMG não gera registro no sistema. Por isso o modelo inclui o cadastro de FORNECEDOR, mas não as entidades COMPRA e RECEBIMENTO.*

*Cadastro de Motoboys e controle de entregas por entregador: Os motoboys não possuem registros na pizzaria. Por isso o modelo não tem entidade MOTOBOY nem ENTREGA; a logística é representada pelo tipo e pelo status do pedido.*

*Consumo no salão (mesas): A pizzaria atende apenas entrega e retirada, sem consumo no local; por isso o modelo não tem mesa nem comanda de salão.*

*Troca de pedido errado: A troca ocorre e o prejuízo é da pizzaria, mas não é registrada; o modelo não a contempla.*

*Emissão de documento fiscal: É feita por aplicativo disponibilizado pela Receita Federal, externo ao banco de dados; o modelo não armazena dados fiscais.*

*Controle sanitário: A organização não mantém registros sanitários; o modelo não armazena temperaturas, validades ou lotes.*

*Escala de funcionários: Não é gerida no banco de dados e não foi indicada pelo responsável como necessidade de informação do sistema; por isso o modelo não tem entidade FUNCIONARIO.*

---

## 3. Requisitos do Sistema

### 3.1 Requisitos Funcionais

*RF01 (Gestão de Clientes): O sistema deve permitir o registro, atualização e consulta dos dados dos clientes, armazenando obrigatoriamente o nome, telefone e o endereço desdobrado em rua, bairro e código postal (CEP).*

*RF02 (Controle de Pedidos): O sistema deve permitir a abertura de novas comandas, vinculando cada pedido a um cliente de forma mandatória e registrando a data, hora e o tipo de entrega (Delivery ou Retirada).*

*RF03 (Gestão de Itens e Cardápio): O sistema deve permitir a inclusão de múltiplos produtos num mesmo pedido, distinguindo entre pizzas (com atributos específicos de sabor e tamanho) e bebidas, e registrando o preço de cada item.*

*RF04 (Faturação e Pagamentos): O sistema deve associar estritamente cada pedido finalizado a uma operação de pagamento, calculando o valor total e exigindo a seleção de um método de pagamento a partir de um catálogo padronizado e pré-definido.*

### 3.2 Requisitos Não Funcionais

*RNF01 (Desempenho): O sistema deve processar e registrar novas inserções na base de dados (novos pedidos) em menos de 3 segundos, assegurando a agilidade necessária para suportar o fluxo de chamadas e mensagens durante os picos de atendimento (ex.: sextas-feiras à noite).*

*RNF02 (Usabilidade): A interface de introdução de dados deve ser intuitiva e de rápida aprendizagem, permitindo que qualquer um dos 6 a 8 funcionários da equipa consiga registrar comandas no sistema sem necessidade de formação técnica extensa.*

*RNF03 (Segurança e Privacidade): O sistema deve garantir a proteção dos dados pessoais armazenados (nome, morada e telefone dos clientes), restringindo a sua extração em massa e garantindo o acesso apenas a utilizadores autorizados no contexto do serviço logístico.*

*(RNF04 (Disponibilidade): A arquitetura da base de dados deve ser fiável e manter a integridade transacional das comandas, impedindo a perda de registros de pedidos ativos ou pagamentos em caso de falhas temporárias de energia ou de rede.)*

---

## 4. Regras de Negócio

- **Regras operacionais:**

*RN01: O lançamento de produtos no pedido exige a utilização de um código numérico identificador único predefinido no cardápio (ex: 58 para mussarela).*

*RN02: O trâmite de cancelamento requer o preenchimento obrigatório do motivo, sendo vedada a devolução automatizada de insumos ao estoque físico.*

*RN03: O fechamento diário do caixa exige a totalização financeira segmentada estritamente por três métodos de pagamento: dinheiro, PIX e cartões.*

*RN04: A organização não executa baixa de estoque por quantidade; o controle de falta de ingredientes se restringe ao indicador de disponibilidade do insumo. Quando um insumo é marcado como indisponível, os produtos que o utilizam são marcados como indisponíveis no cardápio.*

*RN05: O pedido cancelado não é excluído; permanece no histórico com o motivo informado.*

*RN06: Todo pedido possui exatamente um pagamento, com uma única forma escolhida entre dinheiro, PIX e cartão; não existe pagamento dividido.*

*RN07: Quando a forma de pagamento é dinheiro, deve ser registrado se o cliente precisa de troco.*

*RN08: Pedido pago por PIX só é despachado após o recebimento do comprovante.*

*RN09: Se o telefone informado não consta no cadastro, o endereço deve ser confirmado com o cliente antes do despacho.*

*RN10: Quando o motoboy não encontra ninguém no endereço, o pedido recebe a ocorrência "endereço não localizado" e o cliente é contatado; o endereço confirmado é gravado e o anterior é mantido no histórico.*

*RN11: Todo fornecedor cadastrado possui nome, tipo de fornecimento e telefone de contato.*

*RN12: Entregas até 3 km não têm taxa; acima de 3 km, cobra-se R$ 1,00 por km excedente. Pedidos de retirada não têm taxa.*

*RN13: Na pizza meio a meio, o item registra dois sabores e é cobrado o valor do sabor mais caro.*

*RN14: A pizzaria só registra pedidos dentro do horário de funcionamento: terça a quinta e domingo, das 18h às 23h; sexta e sábado, das 18h às 00h; fechada às segundas.*

*RN15: Pedido do tipo entrega exige um endereço de entrega; pedido do tipo retirada não tem endereço.*

*RN16: O endereço de entrega de um pedido deve pertencer ao cliente que fez o pedido.*

- **Restrições organizacionais e legais:**

*Restrição Organizacional 01 (Adequação à LGPD): O armazenamento dos dados de clientes (nome, telefone e endereço) está submetido à Lei Geral de Proteção de Dados (Lei nº 13.709/2018). O banco de dados deve respeitar o princípio da minimização, garantindo que apenas as informações estritamente necessárias para a finalidade de execução da entrega sejam coletadas e processadas, vedando o uso indevido desses registros.*

*Restrição Organizacional 02 (Normas sanitárias): Como serviço de alimentação no município de São Paulo, a pizzaria está sujeita à RDC nº 216/2004 da Anvisa, à Portaria CVS nº 5/2013 do Estado de São Paulo e à regulamentação sanitária municipal. A organização não mantém registros sanitários; por isso o modelo não armazena dados de controle sanitário (temperaturas, validades, lotes).*

*Restrição Organizacional 03 (Documento fiscal): A emissão do documento fiscal é feita por aplicativo disponibilizado pela Receita Federal, externo ao banco de dados. Por ser um processo de terceiros, o modelo não armazena dados fiscais.*

- **Regras prováveis do setor não incluídas:**

*Tempo prometido de entrega: não foi levantado como regra pelo responsável, e o modelo não armazena o horário de saída nem de entrega do pedido.*

---

## 5. Dicionário de Dados Conceitual (Preliminar)

- Dicionário anexado: [Dicionário de Dados](docs/dicionario-de-dados.html)

---

## 6. Modelagem Conceitual (Entidades, Atributos, Relacionamentos)

- **Entidades reconhecidas:**

*Cliente: Armazena os dados pessoais dos consumidores. A sua existência no banco é obrigatória para manter o histórico de consumo e vincular os despachos logísticos.*

*Endereço: Entidade independente criada para resolver o antigo atributo composto do cliente, evitando duplicidade e permitindo o armazenamento de múltiplos locais de entrega e do histórico de endereços de cada cliente.*

*Pedido: É a entidade núcleo do modelo. Ela agrupa os dados transacionais de cada venda, guardando o momento exato da compra, o tipo (entrega ou retirada), o status de entrega, a distância e a taxa de entrega e a justificativa em casos de cancelamento.*

*Produto: Catálogo unificado que consolida todos os itens à venda (pizzas, brotos, bebidas e refrigerantes), diferenciados por categoria.*

*Item_Pedido (Associativa): Entidade de intersecção física que resolve a cardinalidade N:N entre Pedido e Produto, essencial para registrar quantidades específicas, a observação para a cozinha e resguardar o preço histórico praticado no momento da venda, evitando redundância incontrolável. Na pizza meio a meio, relaciona-se uma segunda vez com Produto para registrar o segundo sabor.*

*Pagamento: Representa a consolidação financeira gerada pela operação comercial finalizada.*

*Formas de Pagamentos: Entidade de domínio (catálogo) que restringe os métodos aceitos (Dinheiro, PIX, Cartões) para o fechamento de caixa diário.*

*Fornecedor: Cadastro dos poucos fornecedores da pizzaria (caixas de pizza, refrigerantes e mercadoria geral), com o tipo de fornecimento, o contato e a informação de quem envia mercadoria.*

*Insumo: Cadastro simples dos ingredientes e embalagens, sem controle de quantidade, que permite identificar quais produtos devem sair do cardápio quando um insumo acaba.*

- **Atributos e classificações:**

*Cliente: Contém o atributo identificador (Chave Primária) Código, e os atributos simples Nome e Telefone.*

*Endereço: Contém o atributo identificador (Chave Primária) Código, e os atributos simples Rua, Número e Bairro (obrigatórios) e CEP (opcional).*

*Pedido: Contém o atributo identificador Número, e os atributos simples Data_e_Hora, Tipo (entrega ou retirada), Status_Entrega (inclui a ocorrência "endereço não localizado"), Distancia_Km e Taxa_Entrega (somente entrega) e Cancelamento (motivo em texto livre, opcional).*

*Produto: Contém o atributo identificador Código (numérico decorado pela operação), e os atributos simples Sabor (nome comercial), Categoria, Tamanho (opcional), Preço e Disponível (indicador).*

*Item_Pedido: Contém os atributos simples obrigatórios Quantidade e Preço_Unitario, e o atributo opcional Observação.*

*Pagamento: Contém o atributo identificador Código e os atributos simples Valor_Total (itens mais taxa de entrega), Troco_Necessário (somente dinheiro) e Comprovante_Recebido (somente PIX).*

*Formas de Pagamentos: Contém o atributo identificador Código e o atributo simples Descricao_Metodo.*

*Fornecedor: Contém o atributo identificador Código e os atributos simples Nome, Tipo_Fornecimento, Telefone e Envia_Mercadoria (indicador).*

*Insumo: Contém o atributo identificador Código e os atributos simples Nome, Tipo (ingrediente ou embalagem) e Disponível (indicador).*

- **Relacionamentos pertinentes:**

*Cliente (Possui) Endereço: Vínculo 1:N. Um cliente pode cadastrar vários endereços ao longo do tempo (0,n), mas cada endereço registrado pertence àquele cliente (1,1).*

*Cliente (Faz) Pedido: Vínculo 1:N. Cada registro de pedido pertence estritamente a um único cliente (1,1), enquanto um cliente recorrente acumula múltiplos pedidos (0,n).*

*Pedido (Entrega em) Endereço: Vínculo 1:N. Um pedido de entrega é despachado para exatamente um endereço e um pedido de retirada não tem endereço (0,1); um endereço pode receber vários pedidos (0,n). Esse vínculo permite saber para onde cada pedido foi e medir as entregas frustradas por endereço desatualizado (P1).*

*Pedido (Contém) Produto: Vínculo N:N gerador da entidade associativa Item_Pedido. Um pedido engloba um ou mais produtos (1,n), e um produto figura em vários pedidos (0,n), exigindo o registro da quantidade, do valor unitário e da observação daquela transação.*

*Item_Pedido (Segundo Sabor) Produto: Vínculo N:1 opcional. Um item de pizza meio a meio aponta para um segundo produto (0,1); um produto pode ser segundo sabor em vários itens (0,n).*

*Pedido (Gera) Pagamento: Vínculo 1:1. Cada pedido gera o acoplamento obrigatório com uma operação de pagamento financeiro (1,1).*

*Pagamento (É quitado por) Formas de Pagamentos: Vínculo N:1. Cada pagamento possui exatamente uma forma (1,1), registrada na hora do pedido, e uma forma quita vários pagamentos (0,n).*

*Fornecedor (Fornece) Insumo: Vínculo 1:N. Um fornecedor pode atender vários insumos (0,n) e cada insumo tem no máximo um fornecedor habitual (0,1), pois a reposição pode ocorrer fora dos fornecedores cadastrados.*

*Insumo (Compõe) Produto: Vínculo N:N. Um insumo pode ser usado em vários produtos (0,n) e um produto pode usar vários insumos (0,n); produtos como refrigerantes podem não ter insumos registrados.*

- **Restrições e políticas organizacionais aplicadas ao modelo:**

*Identidade Exigida (LGPD): O sistema não processa pedidos anônimos. A restrição de cardinalidade (1,1) entre Pedido e Cliente força a identificação, mas o armazenamento de Nome e Telefone é estritamente limitado à finalidade logística de execução do delivery, submetido ao princípio da minimização.*

*Controle de Estoque Visual: A organização não executa abatimento automatizado de estoque por quantidade. A falta de insumos é tratada mediante a marcação manual do insumo e dos produtos afetados como indisponíveis, dispensando gatilhos matemáticos no banco.*

*Lastro Financeiro: O processamento final exige compromisso financeiro. A cardinalidade mandatória (1,1) impede que o banco acomode vendas concluídas "órfãs" de faturamento.*

*Pagamento Único: Cada pedido é quitado por uma única forma de pagamento; não existe pagamento dividido.*

---

## 7. Diagrama Entidade-Relacionamento (DER)

Diagrama revisado (notação brModelo, criado no brModelo Web): [DER revisado](docs/DER-PIZZARIA-revisado.jpg)

Versão da Entrega 1 (histórico): [Diagrama Entidade-Relacionamento](docs/DER-PIZZARIA_page-0001.jpg)

---

## 8. Justificativa Técnica

O modelo de dados foi concebido para refletir a operação enxuta da Pizzaria Santoluppo, priorizando a integridade das informações e a velocidade durante os picos de atendimento. As decisões de abstração evitaram a super-complexidade, focando na normalização do que é essencial para o fluxo de negócio do balcão à cozinha.

*   **Catálogo Único de Produtos com Categoria:** Pizzas, brotos, bebidas e refrigerantes ficam em uma única entidade `PRODUTO`, diferenciados por categoria. A justificativa é operacional e técnica: o cardápio da pizzaria usa uma numeração única e nunca repetida (`CD_PRODUTO`, ex.: 58 para mussarela), e o `ITEM_PEDIDO` precisa referenciar uma única tabela. Separar pizza e bebida exigiria um relacionamento ternário entre pedido, pizza e bebida, quando cada item é uma coisa ou outra. O custo são colunas opcionais (`DS_TAMANHO` não se aplica a bebidas), controladas por restrição.
*   **Pizza Meio a Meio com Segundo Sabor Opcional:** Como é cobrado o valor do sabor mais caro, o item registra os dois sabores e um único preço unitário. O segundo sabor é uma segunda referência opcional de `ITEM_PEDIDO` a `PRODUTO` (`CD_PRODUTO_2`), o que evita duplicar o item e mantém o preço cobrado no próprio item. Por isso o item é identificado pelo pedido e por um número sequencial (`ID_PEDIDO`, `NR_ITEM`), e não pelo par pedido e produto.
*   **Entidade de Domínio para Formas de Pagamento:** Em vez de registrar o método de acerto como texto livre na comanda, optou-se por criar a entidade `FORMAS_PAGAMENTOS`. Essa escolha elimina erros de digitação (ex.: variações como "Pix", "PIX", "P I X") e padroniza as opções transacionais.
*   **Uma Forma de Pagamento por Pagamento (1,1):** O responsável confirma que não existe pagamento dividido: a forma é perguntada e anotada na hora do pedido. Por isso cada pagamento aponta para uma única forma, o que dispensa tabela associativa e simplifica o fechamento de caixa por forma.
*   **Cardinalidade Obrigatória entre Cliente e Pedido (1:N com restrição 1,1):** A regra de negócio de "Identidade Exigida" impede o lançamento de pedidos anônimos. A cardinalidade mandatória garante que nenhum pedido exista sem o seu respectivo cliente, o que é crucial para viabilizar o despacho logístico de *Delivery* com base no endereço (`DS_RUA`, `DS_NUMERO`, `NM_BAIRRO`, `CD_CEP`).
*   **Endereço como Entidade Própria e Vínculo com o Pedido:** O endereço é separado do cliente para que um cliente possa ter mais de um endereço e para manter o histórico dos endereços usados. O pedido de entrega aponta para o endereço usado (`ID_ENDERECO`, opcional na retirada), o que sustenta o problema P1: quando o motoboy não encontra ninguém, o novo endereço é gravado sem apagar o anterior e é possível contar as entregas frustradas por endereço.
*   **Taxa de Entrega no Pedido:** A taxa depende da distância da entrega (até 3 km sem cobrança; R$ 1,00 por km excedente). `QT_DISTANCIA_KM` e `VL_TAXA_ENTREGA` ficam no `PEDIDO` porque descrevem a entrega daquele pedido, e o `VL_TOTAL` do pagamento passa a somar itens e taxa. Guardar a distância permite auditar se a taxa cobrada segue a regra.
*   **Lastro Financeiro Estrito (Pedido 1:1 Pagamento):** Optou-se por uma relação estrita de 1:1 entre a venda e o faturamento. Dada a realidade de uma equipe reduzida (cerca de 6 colaboradores), o modelo assegura um fechamento de caixa sem dependências de aprovações gerenciais complexas, impedindo a existência de comandas "órfãs" sem prestação de contas.
*   **Conversão de Processos em Atributos:** Com base na terceira forma normal e na revisão arquitetural, evitou-se o erro de transformar etapas operacionais (verbos) em tabelas isoladas. A logística foi abstraída para os atributos `TP_PEDIDO` (entrega ou retirada) e `DS_ENTREGA` (status, incluindo "endereço não localizado") dentro da entidade núcleo `PEDIDO`, centralizando a transação sem poluir o Diagrama Entidade-Relacionamento. Pela mesma razão não há entidade de motoboy, que a pizzaria não registra.
*   **Fornecedor e Insumo sem Controle de Quantidade:** Os fornecedores são poucos e o dono repõe os ingredientes, sem baixa por gramatura. Por isso `INSUMO` guarda apenas o indicador de disponibilidade e o vínculo com os produtos que o utilizam, e `FORNECEDOR` guarda o contato, o tipo de fornecimento e se envia mercadoria. As entidades de compra e recebimento ficaram de fora porque a organização não registra esses processos (ver Seção 2).
---

## 9. Uso de Inteligência Artificial

### | Uso 1 Revisão técnica e correção do Diagrama Entidade-Relacionamento (DER) |
|------|------------------|
- **Ferramenta e etapa**
*Gemini: Utilizado na etapa de Modelagem Conceitual (Seções 6, 7 e 8) para revisão do Diagrama Entidade-Relacionamento desenhado no software brModelo.* |
- **Motivação** 
*Precisávamos validar se o diagrama construído possuía erros de lógica, redundâncias ou violações das regras formais de banco de dados antes da entrega final.* |
- **Prompt(s) utilizados** 
*"Me ajude a corrigir os erros antes de realizar o readme então, me explique de forma simples e objetiva do que mudar e por que."* |
- **Resposta recebida** 
*A IA apontou falhas críticas de normalização, como a criação de entidades de ação ("Checkout"), a alocação incorreta de atributos ("Endereço" ligado a "Delivery" em vez de "Cliente") e o uso de dados literais como estrutura (atributos "Broto" e "Cerveja" em vez de um genérico "Tamanho" ou "Nome"). A IA orientou a exclusão das entidades fantasmas e a unificação dos relacionamentos na entidade "Pedido".* |
- **Fontes consultadas e verificadas** 
*As correções sugeridas pela IA foram validadas pelo grupo através do cruzamento com os materiais didáticos da disciplina, especificamente os conceitos de não transformar processos (verbos) em entidades (substantivos) e a exigência de identificadores únicos.* |
- **Trechos rejeitados ou corrigidos** 
*A aplicação no brModelo foi feita de forma 100% manual pelo grupo, adaptando as sugestões visuais da IA para a interface da ferramenta (como a realocação das linhas de cardinalidade dos losangos "Faz" e "Contém").* |
- **Justificativa da escolha final** 
*As orientações da IA foram acatadas porque enxugaram o modelo, garantindo a independência da tecnologia e alinhando o diagrama à 3ª Forma Normal na abstração conceitual, facilitando a futura implementação em SQL.* |
- **Reflexão crítica** 
*A IA foi muito eficaz na revisão de regras de negócio lógicas, mas possui a limitação de não poder editar o diagrama visualmente. Tivemos que descrever ou exportar imagens sucessivas para que ela pudesse "enxergar" as conexões, o que exige um bom domínio da nossa parte para aplicar as correções no software.* |

---

### | Uso 2 - Estruturação dos Requisitos, Regras de Negócio e Descrição do Repositório |
|------|------------------|
- **Ferramenta e etapa** 
*Gemini: Utilizado nas etapas de planejamento de levantamento de dados, redação e aperfeiçoamento dos Processos de Negócio, Requisitos e Regras (Seções 2, 3 e 4) e elaboração da Introdução do GitHub.* |
- **Motivação** 
*Auxiliar a transformar o fluxograma inicial e as observações empíricas colhidas na pizzaria em um formato de texto técnico e estruturado (linguagem de Engenharia de Software).* |
- **Prompt(s) utilizados** 
*"Monte uma lista de perguntas que podem ser feitas na entrevista... Simplifique as explicações." e "Seguindo o markdown que te enviei, estou na seção ##3. Precisamos de ajuda no preenchimento, transforme *texto* em uma linguagem mais técnica e formal."* |
- **Resposta recebida** 
*A IA gerou um roteiro de entrevista prático focado nas "dores" da pizzaria (como o caos nos pedidos de sexta-feira) e, posteriormente, redigiu os Requisitos Funcionais (ex: RF01 - Gestão de Clientes), Requisitos Não Funcionais e Regras de Negócio (ex: Obrigatoriedade de vínculo financeiro) baseados nas cardinalidades do nosso DER.* |
- **Fontes consultadas e verificadas** 
*O conteúdo das regras geradas foi confrontado com a realidade da pizzaria analisada pelo membro do grupo que trabalha no local e a entrevista realizada pelos membros, garantindo que os gargalos propostos correspondiam ao mundo real.* |
- **Trechos rejeitados ou corrigidos** 
*Durante a geração de contexto, a IA "alucinou" afirmando que os alunos eram de semestres avançados e que possuíam conhecimentos de especialistas,  corrigimos ela para nossa realidade  atual e a mantemos com uma linguagem técnica mas explicativa conosco. Auxiliando no melhor entendimento de suas ações.* |
- **Justificativa da escolha final** 
*A estruturação técnica do texto foi mantida pois traduziu perfeitamente o comportamento do fluxograma, do modelo conceitual e dos textos bases construído pelo grupo, com jargões adequados à disciplina.* |
- **Reflexão crítica** 
*A alucinação da IA ao tentar adivinhar ao nos tratar como especialistas com anos de experiência serviu como um excelente lembrete de que o texto gerado não pode ser apenas "copiado e colado"  e que causaria obviamente uma enorme confusão e nos tiraria o aprendizado que o projeto proporciona. É necessário ler criticamente e corrigir a máquina para que o documento reflita exatamente a realidade do grupo.* |

---

### | Uso 3 - Correção do README da Entrega 1, análise de inconsistências e instruções para o DER e o dicionário |

- **Ferramenta e etapa**
*Claude (Anthropic): utilizado na transição para a Entrega 2, na revisão do README corrigido da Entrega 1 (Seções 1, 2, 3, 4, 6, 7 e 8), na comparação com a entrevista, o dicionário de dados e a Issue #26, e na elaboração de instruções para atualizar o DER e o dicionário.*
- **Motivação**
*Prazo curto e necessidade de alinhar o README às exigências do esqueleto da Entrega 2 e aos apontamentos do professor, sem perder o que já estava feito.*
- **Prompt(s) utilizados**
*"Siga rigorosamente as regras descritas [no esqueleto da Entrega 2] ... Se faltar informações ... me pergunte antes de produzir qualquer coisa"; "Corrija então com base nos dados que você tem do nosso projeto, as seções do README corrigido que você citou"; "Corrija tudo que ficou em aberto, e me instrua a corrigir o dicionário e o DER", seguido das respostas do responsável (dias e volume, taxa de entrega, meio a meio, horário, fornecedores, registros).*
- **Resposta recebida**
*A IA apontou contradições entre o README, a entrevista e o dicionário (ex.: P1 com a lógica do endereço invertida, catálogo de produtos separado na Seção 8 e unificado na Seção 6, atributo de disponibilidade ausente no dicionário), reescreveu as seções afetadas e propôs ajustes de modelagem: Fornecedor, Insumo, vínculo Pedido-Endereço, segundo sabor, taxa de entrega.*
- **Fontes consultadas e verificadas**
*Issue #26 do professor, esqueleto da Entrega 2, entrevistas, README e dicionário do grupo, e respostas do responsável. A IA localizou na web a RDC nº 216/2004 (Anvisa) e a Portaria CVS nº 5/2013 (SP). `[PREENCHER: o grupo conferiu os dois textos em fonte oficial — indicar a fonte]`*
- **Trechos rejeitados ou corrigidos**
*A IA deixou como pendentes os pontos que só o responsável poderia confirmar, em vez de inventar respostas; as pendências foram preenchidas com as respostas dele. A assunção de pagamento único por pedido foi confirmada pelo responsável. A regra de taxa foi redigida como R$ 1,00 por km excedente aos 3 km, interpretação da IA sobre o relato.*
- **Justificativa da escolha final**
*As correções foram mantidas porque eliminam contradições que se propagariam para o modelo lógico, a normalização e o script SQL, e porque cada regra ficou ligada a um dado informado pelo responsável.*
- **Reflexão crítica**
*A IA só enxerga os arquivos enviados e depende das respostas do responsável: sem elas, parte do levantamento ficaria presa em suposições. A revisão mostrou que texto gerado por IA na Entrega 1 (Gemini) tinha sido aceito sem conferência com a entrevista, o que gerou contradições. Toda saída da IA precisa ser checada contra a realidade da pizzaria.*
