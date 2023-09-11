# projeto_logico_bd
Desafio DIO

Esquema Lógico do Banco de Dados
O esquema lógico do banco de dados para o cenário de e-commerce pode ser representado da seguinte forma:

Tabelas Principais:

Cliente (ClienteID, Nome, Email, Tipo (PJ ou PF))
Chave Primária: ClienteID
Pedido (PedidoID, ClienteID, DataPedido)
Chave Primária: PedidoID
Chave Estrangeira: ClienteID (referenciando Cliente)
Produto (ProdutoID, Nome, Preço)
Chave Primária: ProdutoID
Fornecedor (FornecedorID, Nome)
Chave Primária: FornecedorID
Estoque (ProdutoID, FornecedorID, Quantidade)
Chave Primária: (ProdutoID, FornecedorID)
Chave Estrangeira: ProdutoID (referenciando Produto), FornecedorID (referenciando Fornecedor)
FormaPagamento (FormaPagamentoID, Nome)
Chave Primária: FormaPagamentoID
Pagamento (PagamentoID, PedidoID, FormaPagamentoID, Valor)
Chave Primária: PagamentoID
Chave Estrangeira: PedidoID (referenciando Pedido), FormaPagamentoID (referenciando FormaPagamento)
Entrega (EntregaID, PedidoID, Status, CodigoRastreio)
Chave Primária: EntregaID
Chave Estrangeira: PedidoID (referenciando Pedido)
Tabelas de Relacionamento:

PedidoProduto (PedidoID, ProdutoID, Quantidade)
Chave Primária: (PedidoID, ProdutoID)
Chave Estrangeira: PedidoID (referenciando Pedido), ProdutoID (referenciando Produto)
Agora que temos o esquema lógico, vou criar algumas consultas SQL com base nos requisitos mencionados.

Consultas SQL

1) Quantos pedidos foram feitos por cada cliente?
    SELECT c.Nome AS Cliente, COUNT(p.PedidoID) AS TotalPedidos
    FROM Cliente c
    LEFT JOIN Pedido p ON c.ClienteID = p.ClienteID
    GROUP BY c.ClienteID;
   
2) Algum vendedor também é fornecedor?
   SELECT DISTINCT v.Nome AS Vendedor, f.Nome AS Fornecedor
   FROM Cliente v
   INNER JOIN Fornecedor f ON v.Nome = f.Nome;

3) Relação de produtos fornecedores e estoques:
   SELECT p.Nome AS Produto, f.Nome AS Fornecedor, e.Quantidade AS Estoque
   FROM Produto p
   INNER JOIN Estoque e ON p.ProdutoID = e.ProdutoID
   INNER JOIN Fornecedor f ON e.FornecedorID = f.FornecedorID;

4) Relação de nomes dos fornecedores e nomes dos produtos:
   SELECT f.Nome AS Fornecedor, GROUP_CONCAT(p.Nome) AS ProdutosFornecidos
   FROM Fornecedor f
   INNER JOIN Estoque e ON f.FornecedorID = e.FornecedorID
   INNER JOIN Produto p ON e.ProdutoID = p.ProdutoID
   GROUP BY f.Nome;
