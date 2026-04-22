<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<title> PETSHOP MARKET</title>

<style>
body {
    background: linear-gradient(135deg, #43534b);
    font-family: 'Segoe UI', sans-serif;
    text-align: center;
    color: #444;
}

.container {
    background: white;
    width: 80%;
    margin: 20px auto;
    padding: 20px;
    border-radius: 15px;
    box-shadow: 0 0 15px rgba(82, 199, 214, 0.7);
}

h2 {
    color: #ff69b4;
}

input {
    padding: 8px;
    margin: 5px;
    border-radius: 8px;
    border: 1px solid #66ff00;
}

button {
    background: #ffb6c1;
    color: white;
    border: none;
    padding: 10px;
    border-radius: 10px;
    cursor: pointer;
    transition: 0.3s;
}

button:hover {
    background: #73f37e;
}

.lista {
    text-align: left;
    margin-top: 20px;
}

.item-produto {
    padding: 10px;
    border-bottom: 1px solid #eee;
}

.btn-excluir {
    background: #ff8fab;
    font-size: 12px;
}

.btn-excluir:hover {
    background: #ff4d6d;
}
</style>
</head>

<body>

<div class="container">
    <h2> PETSHOP </h2>

    <input type="text" id="nomeProduto" placeholder="Nome do item">
    <input type="text" id="categoriaProduto" placeholder="Categoria">
    <input type="number" id="precoProduto" placeholder="Preço">
    <input type="number" id="estoqueProduto" placeholder="Estoque">
    <br>

    <button onclick="cadastrarProduto()">Adicionar </button>

    <div class="lista" id="listaProdutos"></div>
</div>

<script>
// =======================
// PRODUTO
// =======================
class Produto {
    constructor(id, nome, categoria, preco, estoque) {
        this.id = id;
        this.nome = nome;
        this.categoria = categoria;
        this.preco = preco;
        this.estoque = estoque;
    }
}

// =======================
// SISTEMA
// =======================
class Sistema {
    constructor() {
        this.produtos = [];
    }

    adicionar(produto) {
        this.produtos.push(produto);
    }

    listar() {
        return this.produtos;
    }

    excluir(id) {
        this.produtos = this.produtos.filter(p => p.id !== id);
    }
}

// =======================
// UI
// =======================
class UI {
    constructor(sistema) {
        this.sistema = sistema;
    }

    getDados() {
        return {
            nome: document.getElementById("nomeProduto").value,
            categoria: document.getElementById("categoriaProduto").value,
            preco: document.getElementById("precoProduto").value,
            estoque: document.getElementById("estoqueProduto").value
        };
    }

    limpar() {
        document.getElementById("nomeProduto").value = "";
        document.getElementById("categoriaProduto").value = "";
        document.getElementById("precoProduto").value = "";
        document.getElementById("estoqueProduto").value = "";
    }

    render() {
        let html = "<h3>🛍️ Itens do Estoque </h3>";

        this.sistema.listar().forEach(p => {
            html += `
                <div class="item-produto">
                     ${p.nome} | R$ ${p.preco} | Estoque: ${p.estoque}
                    <button class="btn-excluir" onclick="app.deletar(${p.id})">Remover</button>
                </div>
            `;
        });

        document.getElementById("listaProdutos").innerHTML = html;
    }
}

// =======================
// APP
// =======================
class App {
    constructor() {
        this.sistema = new Sistema();
        this.ui = new UI(this.sistema);

        this.seed();
        this.ui.render();
    }

    seed() {
        const dados = [
            {id:1,nome:"Coleira Pequena",categoria:"Acessório",preco:30,estoque:10},
            {id:2,nome:"Fêmur de Boi",categoria:"Alimento",preco:25,estoque:20}
        ];

        dados.forEach(d => {
            this.sistema.adicionar(new Produto(
                d.id, d.nome, d.categoria, d.preco, d.estoque
            ));
        });
    }

    cadastrar() {
        const d = this.ui.getDados();

        if (!d.nome) {
            alert("Digite o nome ");
            return;
        }

        const novo = new Produto(
            Date.now(),
            d.nome,
            d.categoria,
            d.preco,
            d.estoque
        );

        this.sistema.adicionar(novo);
        this.ui.render();
        this.ui.limpar();

        alert("Item adicionado ");
    }

    deletar(id) {
        if (confirm("Remover item?")) {
            this.sistema.excluir(id);
            this.ui.render();
        }
    }
}

const app = new App();

function cadastrarProduto() {
    app.cadastrar();
}
</script>

</body>
</html>
