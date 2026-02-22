# Projeto Conceitual de Banco de Dados - E-Commerce

## Descricao do Projeto

Este projeto faz parte da **Formacao SQL Database Specialist** da DIO. O objetivo e refinar o modelo conceitual de banco de dados para um cenario de **E-Commerce**, acrescentando os seguintes pontos:

- **Cliente PJ e PF**: Uma conta pode ser Pessoa Juridica (PJ) ou Pessoa Fisica (PF), mas nao ambas simultaneamente.
- **Pagamento**: Um cliente pode ter cadastrado mais de uma forma de pagamento.
- **Entrega**: Possui status e codigo de rastreio.

## Esquema Conceitual - Entidades e Relacionamentos

### Entidades Principais

| Entidade | Descricao | Atributos Principais |
|---|---|---|
| **Cliente** | Superclasse (especializacao total e disjunta) | idCliente, Email, DataCadastro |
| **ClientePF** | Pessoa Fisica (subclasse de Cliente) | CPF, Nome, Sobrenome, DataNascimento |
| **ClientePJ** | Pessoa Juridica (subclasse de Cliente) | CNPJ, RazaoSocial, NomeFantasia |
| **Produto** | Itens disponiveis para venda | idProduto, Descricao, Categoria, Valor |
| **Pedido** | Registro de compra do cliente | idPedido, DataPedido, StatusPedido, Frete |
| **ItemPedido** | Produtos dentro de um pedido | Quantidade, Preco |
| **Estoque** | Local de armazenamento | idEstoque, Local, Quantidade |
| **Fornecedor** | Empresa que fornece produtos | idFornecedor, RazaoSocial, CNPJ |
| **Vendedor** | Terceiros que vendem na plataforma | idVendedor, RazaoSocial, CNPJ |
| **Pagamento** | Forma de pagamento cadastrada | idPagamento, TipoPagamento, NumCartao, Validade, Bandeira |
| **Entrega** | Informacoes de entrega do pedido | idEntrega, StatusEntrega, CodigoRastreio, DataEstimada |

### Relacionamentos

- **Cliente** realiza **Pedido** (1:N)
- **Pedido** contem **ItemPedido** (1:N)
- **ItemPedido** referencia **Produto** (N:1)
- **Produto** esta em **Estoque** (N:M)
- **Fornecedor** fornece **Produto** (N:M)
- **Vendedor** vende **Produto** (N:M)
- **Cliente** possui **Pagamento** (1:N - multiplas formas de pagamento)
- **Pedido** utiliza **Pagamento** (N:1)
- **Pedido** possui **Entrega** (1:1)

### Especializacao/Generalizacao

- **Cliente** e uma superclasse com especializacao **total e disjunta**:
  - **ClientePF** (Pessoa Fisica) - identificado por CPF
  - **ClientePJ** (Pessoa Juridica) - identificado por CNPJ
  - Uma conta NAO pode ser PJ e PF ao mesmo tempo (disjoint constraint, d)

### Restricoes de Pagamento

- Um cliente pode ter **N formas de pagamento** cadastradas (cartao de credito, boleto, PIX, etc.)
- Cada pedido referencia **uma forma de pagamento** escolhida pelo cliente
- Tipos suportados: Cartao de Credito, Cartao de Debito, Boleto, PIX, Transferencia

### Restricoes de Entrega

- Cada pedido possui **uma entrega** associada
- Status possiveis: Pendente, Em Separacao, Enviado, Em Transito, Entregue, Devolvido
- Codigo de rastreio gerado apos envio

## Ferramentas Utilizadas

- **MySQL Workbench** / **Draw.io** para modelagem do diagrama ER
- **GitHub** para versionamento

## Como Visualizar

O diagrama ER conceitual esta descrito neste README com as entidades, atributos e relacionamentos detalhados acima.

---
*Projeto desenvolvido como parte da Formacao SQL Database Specialist - DIO*
