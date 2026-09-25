### Modelagem de um sistema de gestão de informações para uma organização de pequeno porte

---

## Metadados

- Davi Ramos de Oliveira - RGM 049576216
- Gabriel Silva Candelo - RGM 48050938
- John Antoni Rodrigo Quispe - RGM 49450492
- Leon Correia da Silva - RGM 048036374
- Pablo Lorran Ferreira da Silva - RGM 48146978

---

## 1. Caracterização da Organização

- **Pizzaria Santoluppo:**
- **Contexto e porte:** *Uma empresa com fins lucrativos do ramo alimentício, com uma operação envolvendo aproximadamente 6 colaboradores, com um volume de 25 pedidos por dia.*
- **Problemas e necessidades identificados:** *qual é a "crise operacional" — o que está desorganizado hoje (planilhas soltas, papel, falta de controle de estoque/doações/cadastros, etc.)?*
- **Justificativa da escolha:** *A escolha se deu pelo fácil acesso a toda estrutura necessária para o projeto, desde acesso ao local até a quantidade real de dados que são coletados diariamente*
- **Evidências da organização:** *Redes Sociais: Instagram https://www.instagram.com/santoluppopizzaria/ - Endereço: Av. Sapopemba 1333 Vila Reg. Feijó CEP:03345-001 - Contato: Telefone(11) 2028-8486 - WhatsApp (11) 995585599*

---

## 2. Processos de Negócio

- **Principais processos mapeados:**
*Gestão de Pedidos (Venda): O fluxo central inicia quando o cliente faz contato via WhatsApp, telefone ou balcão. O atendente identifica o cliente (ou realiza um novo cadastro), anota os itens do pedido com suas especificações (ex: metades, remoção de ingredientes, bordas) e confirma o endereço e a forma de entrega.*

*Produção (Cozinha): A comanda gerada pelo pedido é enviada à área de preparo. A equipe da cozinha utiliza os insumos disponíveis no estoque diário para montar e assar as pizzas conforme as especificações exigidas na venda.*

*Logística de Entrega e Retirada: Com o produto finalizado, o pedido é encaminhado para a expedição. Se for delivery, é alocado para a rota de um motoboy; se for retirada, é entregue diretamente ao cliente no balcão.*

*Fechamento e Pagamento: O pagamento é processado e vinculado ao pedido, seja no balcão (cartão/dinheiro), via motoboy (maquininha na entrega) ou remotamente (PIX), finalizando o ciclo de atendimento ao cliente.*

*Controle de Estoque e Compras (Processo Paralelo): Independentemente das vendas do momento, o volume de ingredientes é monitorado. Quando um insumo atinge o estoque mínimo de segurança, a gestão aciona o processo de compras junto aos fornecedores para reabastecer a pizzaria antes do próximo turno de pico.*

- [Fluxograma do Banco de Dados (Básico)](docs/fluxograma-basico-projetosantoluppo.png)

---

## 3. Requisitos do Sistema

### 3.1 Requisitos Funcionais

*RF01 (Gestão de Clientes): O sistema deve permitir o registo, atualização e consulta dos dados dos clientes, armazenando obrigatoriamente o nome, telefone e o endereço desdobrado em rua, bairro e código postal (CEP).*
*RF02 (Controle de Pedidos): O sistema deve permitir a abertura de novas comandas, vinculando cada pedido a um cliente de forma mandatória e registando a data, hora e o tipo de entrega (Delivery ou Retirada).*
*RF03 (Gestão de Itens e Cardápio): O sistema deve permitir a inclusão de múltiplos produtos num mesmo pedido, distinguindo entre pizzas (com atributos específicos de sabor e tamanho) e bebidas, e registando o preço de cada item.*
*RF04 (Faturação e Pagamentos): O sistema deve associar estritamente cada pedido finalizado a uma operação de pagamento, calculando o valor total e exigindo a seleção de um método de pagamento a partir de um catálogo padronizado e pré-definido.*

### 3.2 Requisitos Não Funcionais

*RNF01 (Desempenho): O sistema deve processar e registar novas inserções na base de dados (novos pedidos) em menos de 3 segundos, assegurando a agilidade necessária para suportar o fluxo de chamadas e mensagens durante os picos de atendimento (ex.: sextas-feiras à noite).*
a
*RNF02 (Usabilidade): A interface de introdução de dados deve ser intuitiva e de rápida aprendizagem, permitindo que qualquer um dos 6 a 8 funcionários da equipa consiga registar comandas no sistema sem necessidade de formação técnica extensa.*

*RNF03 (Segurança e Privacidade): O sistema deve garantir a proteção dos dados pessoais armazenados (nome, morada e telefone dos clientes), restringindo a sua extração em massa e garantindo o acesso apenas a utilizadores autorizados no contexto do serviço logístico.*

*(RNF04 (Disponibilidade): A arquitetura da base de dados deve ser fiável e manter a integridade transacional das comandas, impedindo a perda de registos de pedidos ativos ou pagamentos em caso de falhas temporárias de energia ou de rede.)*

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
*(vale 10% — Dimensão Procedimental - Segue o modelo do arquivo 02-03g_Exemplo_Dicionario_Dados.pdf)*

Para cada entidade identificada, liste:

| Atributo | Descrição | Regra de negócio associada |
|----------|-----------|------------------------------|
| *nome do atributo* | *o que ele representa* | *se houver alguma regra (obrigatoriedade, valores possíveis, etc.)* |

*Mantenha o dicionário organizado e padronizado (mesmo formato de tabela para todas as entidades).*

**Atenção à privacidade:** se forem usados exemplos de valores para ilustrar os atributos, esses exemplos devem ser **fictícios** — não utilize dados reais de clientes, fiéis, beneficiários, doadores ou funcionários da organização (nomes, CPFs, contatos etc.), mesmo que tenham sido observados durante a pesquisa de campo. Os exemplos devem apenas ser **coerentes com as operações reais** observadas.

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

Diagrama anexado: [Diagrama Entidade-Relacionamento](docs/DER-PIZZARIA.pdf)

---

## 8. Justificativa Técnica
*(vale 7,5% — sozinho, é o subcritério de maior peso dentro da Dimensão Conceitual)*

*Explique e defenda as decisões de abstração e modelagem tomadas: por que essas entidades, esses atributos, esses relacionamentos e essas cardinalidades — e não outras alternativas possíveis?*

---

## 9. Uso de Inteligência Artificial
*(documentação obrigatória — não é opcional se o grupo usou IA em qualquer etapa: pesquisa, escrita, organização de ideias ou revisão de texto)*

Se o grupo usou alguma ferramenta de IA (ChatGPT, Claude, Gemini, Perplexity etc.) em qualquer parte do trabalho, registre **para cada uso relevante**:

| Item | O que registrar |
|------|------------------|
| **Ferramenta e etapa** | Qual IA foi usada e em qual parte do trabalho (ex.: pesquisa sobre o setor da organização, redação do README, organização dos requisitos, revisão ortográfica/gramatical). |
| **Motivação** | Por que o grupo recorreu à IA nesse ponto específico. |
| **Prompt(s) utilizados** | Texto exato (ou muito próximo) do que foi perguntado/pedido à IA. |
| **Resposta recebida** | Resumo ou trecho relevante da resposta da IA. |
| **Fontes consultadas e verificadas** | Se a IA citou fontes/dados, quais foram checadas pelo grupo e como (ex.: comparação com o que foi observado na visita de campo). |
| **Trechos rejeitados ou corrigidos** | O que da resposta da IA foi descartado, editado ou corrigido manualmente, e por quê. |
| **Justificativa da escolha final** | Por que o grupo manteve, adaptou ou rejeitou o que a IA sugeriu. |
| **Reflexão crítica** | Limites, vieses ou erros identificados no uso da IA nessa etapa (ex.: informação desatualizada, alucinação, generalização incorreta sobre o tipo de organização). |

*Se o grupo não usou nenhuma ferramenta de IA, declare isso explicitamente nesta seção.*
