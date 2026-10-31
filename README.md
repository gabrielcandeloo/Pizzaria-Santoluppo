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
*Identidade Obrigatória: Um pedido só pode ser iniciado no sistema se houver um cliente identificado e vinculado (refletido na cardinalidade 1,1 do diagrama).*

*Lastro Financeiro: O processamento da comanda exige um vínculo obrigatório com um registro de pagamento (cardinalidade 1,1 entre Pedido e Pagamento), impedindo que existam pedidos sem prestação de contas no caixa.*

*Meios de Pagamento: A modalidade de acerto só pode ser escolhida a partir do catálogo pré-definido na entidade de domínio "Formas de Pagamentos".*

- **Restrições organizacionais:**
  
*Validação Logística: Se o atributo Tipo_Entrega for assinalado como "Delivery", o sistema assume como obrigatório o preenchimento do endereço composto (Rua, Bairro e CEP) do cliente para viabilizar o despacho.*

*Perfil de Operação Enxuta: Dado que a pizzaria opera com uma equipe de 6 a 8 funcionários, o modelo de negócio assume que não há departamentos isolados. O fluxo deve ser contínuo do balcão à cozinha sem depender de aprovações gerenciais em múltiplas etapas.*

---

## 5. Dicionário de Dados Conceitual (Preliminar)

- Dicionário anexado: [Dicionário de Dados](docs/dicionario-de-dados.html)

---

## 6. Modelagem Conceitual (Entidades, Atributos, Relacionamentos)

- **Entidades reconhecidas:**
*Cliente: Armazena os dados dos consumidores. A sua existência no banco é obrigatória para registrar os envolvidos nas transações, manter o histórico de consumo e viabilizar o despacho logístico.*

*Pedido: É a entidade núcleo do modelo. Ela agrupa os dados transacionais de cada venda efetivada, guardando o momento exato da compra e a modalidade de entrega (delivery ou retirada) sem a necessidade de duplicar lógicas de cadastro.*

*Pizza e Bebidas: Representam os catálogos independentes de produtos. A segregação em duas entidades se justifica pois as pizzas (de fabricação própria) exigem propriedades distintas (como tamanho e variação de sabor) em relação às bebidas, que são apenas itens padronizados de revenda.*

*Pagamento: Representa o registro financeiro gerado pela operação comercial.*

*Formas de Pagamentos: Entidade de domínio (catálogo). Garante que a escolha do método de pagamento (Pix, Crédito, etc.) seja feita a partir de uma lista padronizada, eliminando erros de digitação livre no sistema.*

- **Atributos e classificações:**
*Cliente: Contém o atributo identificador (Chave Primária) Cod_Cliente, os atributos simples Nome e Telefone, e o atributo composto Endereço — o qual desdobra-se nos sub-atributos simples Rua, Bairro e CEP.*

*Pedido: Contém o atributo identificador ID_Pedido e os atributos simples data_hora e Tipo_Entrega.*

*Pizza: Contém o atributo identificador ID_Pizza e os atributos simples Sabor, Tamanho e Preco.*

*Bebidas: Contém o atributo identificador ID_Bebidas e os atributos simples Nome_Bebida e Preco.*

*Pagamento: Contém o atributo identificador ID_pagamento e o atributo simples valor_total.*

*Formas de Pagamentos: Contém o atributo identificador ID_FormaPagto e o atributo simples Descricao_Metodo.*

- **Relacionamentos pertinentes:**
*Cliente (Faz) Pedido: Estabelece um vínculo 1:N. Cada registro de pedido pertence de modo restrito a um único cliente (1,1), ao passo que um cliente recorrente pode acumular múltiplos pedidos na base histórica (0,n).*

*Pedido (Contém) Pizza / Bebidas: Estabelece vínculos do tipo N:N. Um pedido comercial engloba um ou múltiplos produtos do cardápio, e esses mesmos itens podem figurar em infinitos pedidos distintos (1,n e 0,n).*

*Pedido (Faz) Pagamento: Estabelece um vínculo estrito 1:1. Cada comanda gera a obrigatoriedade de acoplamento com exatamente uma operação de pagamento financeiro.*

*Pagamento (Decisão) Formas de Pagamentos: Associa o montante faturado à modalidade transacional selecionada pelo consumidor, extraída do catálogo.*

- **Restrições e políticas organizacionais aplicadas ao modelo:**
*Identidade Exigida: O sistema impede o lançamento de pedidos anônimos. A restrição de cardinalidade (1,1) entre Pedido e Cliente determina que não se processa uma venda sem a prévia identificação do indivíduo.*

*Lastro Financeiro: O processamento final exige o compromisso financeiro. A cardinalidade mandatória (1,1) na relação Pedido-Pagamento restringe o banco de dados de acomodar vendas "órfãs", exigindo que a entrada do pedido dispare um evento de valor associado na camada de faturamento.*

---

## 7. Diagrama Entidade-Relacionamento (DER)

Diagrama anexado: [Diagrama Entidade-Relacionamento](docs/DER-PIZZARIA_page-0001.jpg)

---

## 8. Justificativa Técnica

O modelo de dados foi concebido para refletir a operação enxuta da Pizzaria Santoluppo, priorizando a integridade das informações e a velocidade durante os picos de atendimento. As decisões de abstração evitaram a super-complexidade, focando na normalização do que é essencial para o fluxo de negócio do balcão à cozinha.

*   **Separação entre Pizzas e Bebidas (Catálogos Independentes):** A decisão de segregar produtos justifica-se pela natureza distinta dos itens. As pizzas exigem atributos de fabricação própria e variação (`DS_SABOR`, `TP_TAMANHO`), enquanto as bebidas são itens padronizados de revenda que requerem apenas nome e preço. Agrupá-las numa única entidade "Produto" geraria anomalias e campos nulos desnecessários, ferindo as boas práticas de modelagem.
*   **Entidade de Domínio para Formas de Pagamento:** Em vez de registrar o método de acerto como texto livre na comanda, optou-se por criar a entidade `FORMAS_DE_PAGAMENTOS`. Essa escolha elimina erros de digitação (ex.: variações como "Pix", "PIX", "P I X") e padroniza as opções transacionais.
*   **Cardinalidade Obrigatória entre Cliente e Pedido (1:N com restrição 1,1):** A regra de negócio de "Identidade Exigida" impede o lançamento de pedidos anónimos. A cardinalidade mandatória garante que nenhum pedido exista sem o seu respetivo cliente, o que é crucial para viabilizar o despacho logístico de *Delivery* com base no endereço composto (`DS_RUA`, `DS_BAIRRO`, `CD_CEP`).
*   **Lastro Financeiro Estrito (Pedido 1:1 Pagamento):** Optou-se por uma relação estrita de 1:1 entre a venda e o faturamento. Dada a realidade de uma equipe reduzida (6 a 8 funcionários), o modelo assegura um fecho de caixa sem dependências de aprovações gerenciais complexas, impedindo a existência de comandas "órfãs" sem prestação de contas.
*   **Conversão de Processos em Atributos:** Com base na terceira forma normal e na revisão arquitetural, evitou-se o erro de transformar etapas operacionais (verbos) em tabelas isoladas. A logística foi abstraída com eficiência para o atributo estruturado `TP_RETIRADA | TP_DELIVERY` dentro da entidade núcleo `Pedido`, centralizando a transação sem poluir o Diagrama Entidade-Relacionamento.

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
