# 🛒 Refinando um Projeto Conceitual de Banco de Dados — E-Commerce

## 📋 Descrição do Projeto

Este repositório contém o **esquema conceitual refinado** de um banco de dados para um cenário de **E-Commerce**, desenvolvido como parte da **Formação SQL Database Specialist** da [DIO (Digital Innovation One)](https://www.dio.me/).

O objetivo principal é modelar um sistema de e-commerce completo, aplicando conceitos avançados de modelagem de dados, incluindo **especialização/generalização (EER)**, **relacionamentos N:M** e **restrições de integridade**. O modelo foi refinado a partir de um esquema base, incorporando os seguintes aprimoramentos:

- **Cliente PJ e PF:** Uma conta pode ser de Pessoa Jurídica (PJ) ou Pessoa Física (PF), mas **não** ambas simultaneamente — modelado via **especialização disjunta e total**.
- **Pagamento:** Um cliente pode ter cadastrado **mais de uma forma de pagamento** (cartão de crédito, boleto, PIX, etc.).
- **Entrega:** Cada pedido possui informações de entrega com **status** e **código de rastreio**.

---

## 🏗️ Esquema Conceitual — Entidades e Atributos

### 1. Cliente (Superclasse)

| Atributo | Tipo | Descrição |
|---|---|---|
| `idCliente` | INT (PK) | Identificador único do cliente |
| `Email` | VARCHAR | Endereço de e-mail do cliente |
| `Endereco` | VARCHAR | Endereço completo para correspondência |
| `Telefone` | VARCHAR | Telefone de contato |
| `DataCadastro` | DATE | Data de criação da conta |

> **Nota:** A entidade `Cliente` é uma **superclasse** com especialização **total e disjunta** (`d`), ou seja, todo cliente **obrigatoriamente** pertence a uma das subclasses abaixo, e **não pode** pertencer a ambas ao mesmo tempo.

### 2. ClientePF (Pessoa Física — Subclasse de Cliente)

| Atributo | Tipo | Descrição |
|---|---|---|
| `CPF` | CHAR(11) | Cadastro de Pessoa Física (único) |
| `Nome` | VARCHAR | Primeiro nome |
| `Sobrenome` | VARCHAR | Sobrenome |
| `DataNascimento` | DATE | Data de nascimento |

### 3. ClientePJ (Pessoa Jurídica — Subclasse de Cliente)

| Atributo | Tipo | Descrição |
|---|---|---|
| `CNPJ` | CHAR(14) | Cadastro Nacional de Pessoa Jurídica (único) |
| `RazaoSocial` | VARCHAR | Razão social da empresa |
| `NomeFantasia` | VARCHAR | Nome fantasia |
| `InscricaoEstadual` | VARCHAR | Inscrição estadual |

### 4. Produto

| Atributo | Tipo | Descrição |
|---|---|---|
| `idProduto` | INT (PK) | Identificador único do produto |
| `NomeProduto` | VARCHAR | Nome do produto |
| `Descricao` | TEXT | Descrição detalhada |
| `Categoria` | VARCHAR | Categoria do produto |
| `Valor` | DECIMAL | Preço unitário |

### 5. Pedido

| Atributo | Tipo | Descrição |
|---|---|---|
| `idPedido` | INT (PK) | Identificador único do pedido |
| `DataPedido` | DATETIME | Data e hora da realização do pedido |
| `StatusPedido` | ENUM | Status atual: Pendente, Confirmado, Enviado, Entregue, Cancelado |
| `ValorFrete` | DECIMAL | Valor do frete |
| `ValorTotal` | DECIMAL | Valor total do pedido (itens + frete) |

### 6. ItemPedido (Entidade Associativa)

| Atributo | Tipo | Descrição |
|---|---|---|
| `idPedido` | INT (FK) | Referência ao pedido |
| `idProduto` | INT (FK) | Referência ao produto |
| `Quantidade` | INT | Quantidade do produto no pedido |
| `PrecoUnitario` | DECIMAL | Preço unitário no momento da compra |

### 7. Pagamento

| Atributo | Tipo | Descrição |
|---|---|---|
| `idPagamento` | INT (PK) | Identificador único da forma de pagamento |
| `idCliente` | INT (FK) | Referência ao cliente proprietário |
| `TipoPagamento` | ENUM | Tipo: Cartão de Crédito, Cartão de Débito, Boleto, PIX, Transferência |
| `NumeroCartao` | VARCHAR | Número do cartão (quando aplicável) |
| `Validade` | DATE | Data de validade do cartão |
| `Bandeira` | VARCHAR | Bandeira do cartão (Visa, Mastercard, etc.) |
| `ChavePIX` | VARCHAR | Chave PIX (quando aplicável) |

> **Nota:** Um cliente pode cadastrar **múltiplas formas de pagamento** (relacionamento 1:N entre Cliente e Pagamento). Cada pedido referencia **uma** forma de pagamento previamente cadastrada.

### 8. Entrega

| Atributo | Tipo | Descrição |
|---|---|---|
| `idEntrega` | INT (PK) | Identificador único da entrega |
| `idPedido` | INT (FK) | Referência ao pedido |
| `StatusEntrega` | ENUM | Status: Pendente, Em Separação, Enviado, Em Trânsito, Saiu para Entrega, Entregue, Devolvido |
| `CodigoRastreio` | VARCHAR | Código de rastreamento (gerado após envio) |
| `Transportadora` | VARCHAR | Nome da transportadora |
| `DataEnvio` | DATE | Data de envio |
| `DataEstimada` | DATE | Data estimada de entrega |
| `DataEntrega` | DATE | Data efetiva de entrega |

### 9. Estoque

| Atributo | Tipo | Descrição |
|---|---|---|
| `idEstoque` | INT (PK) | Identificador do local de estoque |
| `LocalEstoque` | VARCHAR | Localização do estoque (cidade/estado) |

### 10. ProdutoEstoque (Entidade Associativa)

| Atributo | Tipo | Descrição |
|---|---|---|
| `idProduto` | INT (FK) | Referência ao produto |
| `idEstoque` | INT (FK) | Referência ao estoque |
| `Quantidade` | INT | Quantidade disponível naquele estoque |

### 11. Fornecedor

| Atributo | Tipo | Descrição |
|---|---|---|
| `idFornecedor` | INT (PK) | Identificador único do fornecedor |
| `RazaoSocial` | VARCHAR | Razão social |
| `CNPJ` | CHAR(14) | CNPJ do fornecedor |
| `Telefone` | VARCHAR | Telefone de contato |

### 12. Vendedor (Terceiro/Marketplace)

| Atributo | Tipo | Descrição |
|---|---|---|
| `idVendedor` | INT (PK) | Identificador único do vendedor |
| `RazaoSocial` | VARCHAR | Razão social |
| `CNPJ` | CHAR(14) | CNPJ do vendedor |
| `Localizacao` | VARCHAR | Localização do vendedor |

---

## 🔗 Relacionamentos

| Relacionamento | Cardinalidade | Descrição |
|---|---|---|
| Cliente → Pedido | 1:N | Um cliente realiza vários pedidos |
| Cliente → Pagamento | 1:N | Um cliente possui várias formas de pagamento |
| Pedido → ItemPedido | 1:N | Um pedido contém vários itens |
| ItemPedido → Produto | N:1 | Cada item referencia um produto |
| Pedido → Pagamento | N:1 | Cada pedido utiliza uma forma de pagamento |
| Pedido → Entrega | 1:1 | Cada pedido possui uma entrega associada |
| Produto ↔ Estoque | N:M | Um produto pode estar em vários estoques (via ProdutoEstoque) |
| Fornecedor ↔ Produto | N:M | Um fornecedor fornece vários produtos e vice-versa |
| Vendedor ↔ Produto | N:M | Um vendedor vende vários produtos na plataforma |

---

## 🧬 Especialização/Generalização (EER)

```
                    ┌──────────────┐
                    │   CLIENTE    │
                    │──────────────│
                    │ idCliente PK │
                    │ Email        │
                    │ Endereco     │
                    │ Telefone     │
                    │ DataCadastro │
                    └──────┬───────┘
                           │
                      ─────┴─────
                     │  d  (total) │
                      ─────┬─────
                     ╱            ╲
              ┌─────┴──────┐  ┌────┴───────┐
              │ CLIENTE_PF │  │ CLIENTE_PJ │
              │────────────│  │────────────│
              │ CPF        │  │ CNPJ       │
              │ Nome       │  │ RazaoSocial│
              │ Sobrenome  │  │ NomeFantas.│
              │ DataNasc.  │  │ InscEstadu.│
              └────────────┘  └────────────┘
```

- **Tipo de especialização:** Total e Disjunta (`d`)
- **Significado:** Todo cliente deve ser obrigatoriamente PF **ou** PJ, nunca ambos, e não pode existir cliente sem classificação.

---

## 💳 Modelo de Pagamento

```
  ┌──────────────┐        ┌────────────────┐        ┌──────────────┐
  │   CLIENTE    │ 1    N │   PAGAMENTO    │ 1    N │    PEDIDO    │
  │              ├────────┤                ├────────┤              │
  │              │ possui │ TipoPagamento  │utiliza │              │
  └──────────────┘        │ NumeroCartao   │        └──────────────┘
                          │ Validade       │
                          │ Bandeira       │
                          │ ChavePIX       │
                          └────────────────┘
```

- Um cliente pode cadastrar **N** formas de pagamento.
- Cada pedido seleciona **uma** forma de pagamento dentre as cadastradas.

---

## 🚚 Modelo de Entrega

```
  ┌──────────────┐        ┌─────────────────┐
  │    PEDIDO    │ 1    1 │    ENTREGA      │
  │              ├────────┤                 │
  │              │  tem   │ StatusEntrega   │
  └──────────────┘        │ CodigoRastreio  │
                          │ Transportadora  │
                          │ DataEnvio       │
                          │ DataEstimada    │
                          │ DataEntrega     │
                          └─────────────────┘
```

- Cada pedido possui **exatamente uma** entrega associada (1:1).
- O código de rastreio é gerado **após o envio** pela transportadora.
- Status possíveis: `Pendente` → `Em Separação` → `Enviado` → `Em Trânsito` → `Saiu para Entrega` → `Entregue` (ou `Devolvido`).

---

## 🛠️ Ferramentas Utilizadas

- **MySQL Workbench** — Modelagem do diagrama ER
- **Draw.io (diagrams.net)** — Ferramenta alternativa de design
- **GitHub** — Versionamento e documentação do projeto

---

## 📂 Estrutura do Repositório

```
ecommerce-conceitual-db/
├── README.md          # Documentação completa do projeto (este arquivo)
└── (diagrama ER)      # Diagrama conceitual do banco de dados
```

---

## 🎓 Contexto Acadêmico

Este projeto foi desenvolvido como **Desafio de Projeto** da **Formação SQL Database Specialist** oferecida pela **DIO (Digital Innovation One)**. O desafio consiste em refinar o modelo conceitual de um banco de dados de E-Commerce, aplicando os conceitos de:

- Modelagem Entidade-Relacionamento (ER)
- Modelo Entidade-Relacionamento Estendido (EER)
- Especialização e Generalização
- Restrições de integridade
- Cardinalidades e participações

---

## 📝 Licença

Este projeto é de uso educacional e foi desenvolvido para fins de aprendizado na plataforma DIO.

---

> **Autor:** Gabriel Demetrios Lafis  
> **Formação:** SQL Database Specialist — DIO  
> **Data:** Fevereiro de 2026
