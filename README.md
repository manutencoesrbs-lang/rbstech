# rbstech
Sistema de orçamentos RBS TECH
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <title>RBS TECH - Dashboard</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <h1>RBS TECH - Dashboard</h1>
    <nav>
        <a href="clientes.html">Clientes</a>
        <a href="materiais.html">Materiais</a>
        <a href="orcamentos.html">Orçamentos</a>
    </nav>
    <div id="welcome">
        <p>Bem-vindo, Rodrigo!</p>
    </div>
</body>
</html>
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <title>Clientes - RBS TECH</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <h1>Clientes</h1>
    <form id="clienteForm">
        <input type="text" id="nome" placeholder="Nome" required>
        <input type="text" id="telefone" placeholder="Telefone">
        <button type="submit">Adicionar Cliente</button>
    </form>
    <ul id="listaClientes"></ul>

    <script>
        async function carregarClientes() {
            const res = await fetch('/clientes');
            const data = await res.json();
            const lista = document.getElementById('listaClientes');
            lista.innerHTML = '';
            data.forEach((c, i) => {
                lista.innerHTML += `<li>${c.nome} - ${c.telefone}</li>`;
            });
        }

        document.getElementById('clienteForm').addEventListener('submit', async (e) => {
            e.preventDefault();
            const nome = document.getElementById('nome').value;
            const telefone = document.getElementById('telefone').value;
            await fetch('/clientes', {
                method: 'POST',
                headers: {'Content-Type':'application/json'},
                body: JSON.stringify({nome, telefone})
            });
            document.getElementById('nome').value = '';
            document.getElementById('telefone').value = '';
            carregarClientes();
        });

        carregarClientes();
    </script>
</body>
</html>
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <title>Materiais - RBS TECH</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <h1>Materiais</h1>
    <form id="materialForm">
        <input type="text" id="nome" placeholder="Nome do material" required>
        <input type="number" id="quantidade" placeholder="Quantidade" required>
        <input type="number" step="0.01" id="preco" placeholder="Preço unitário" required>
        <button type="submit">Adicionar Material</button>
    </form>
    <ul id="listaMateriais"></ul>

    <script>
        async function carregarMateriais() {
            const res = await fetch('/materiais');
            const data = await res.json();
            const lista = document.getElementById('listaMateriais');
            lista.innerHTML = '';
            data.forEach((m, i) => {
                lista.innerHTML += `<li>${m.nome} - ${m.quantidade} unidades - R$ ${m.preco.toFixed(2)}</li>`;
            });
        }

        document.getElementById('materialForm').addEventListener('submit', async (e) => {
            e.preventDefault();
            const nome = document.getElementById('nome').value;
            const quantidade = parseInt(document.getElementById('quantidade').value);
            const preco = parseFloat(document.getElementById('preco').value);
            await fetch('/materiais', {
                method: 'POST',
                headers: {'Content-Type':'application/json'},
                body: JSON.stringify({nome, quantidade, preco})
            });
            document.getElementById('nome').value = '';
            document.getElementById('quantidade').value = '';
            document.getElementById('preco').value = '';
            carregarMateriais();
        });

        carregarMateriais();
    </script>
</body>
</html>
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <title>Orçamentos - RBS TECH</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <h1>Orçamentos</h1>
    <form id="orcamentoForm">
        <input type="text" id="cliente" placeholder="Nome do Cliente" required>
        <input type="text" id="itens" placeholder="Itens (nome:quantidade:preco separados por ,)" required>
        <button type="submit">Gerar Orçamento</button>
    </form>
    <ul id="listaOrcamentos"></ul>

    <script>
        async function carregarOrcamentos() {
            const res = await fetch('/orcamentos');
            const data = await res.json();
            const lista = document.getElementById('listaOrcamentos');
            lista.innerHTML = '';
            data.forEach((o, i) => {
                lista.innerHTML += `<li>${o.cliente} - Total: R$ ${o.total.toFixed(2)}</li>`;
            });
        }

        document.getElementById('orcamentoForm').addEventListener('submit', async (e) => {
            e.preventDefault();
            const cliente = document.getElementById('cliente').value;
            const itensTexto = document.getElementById('itens').value;
            let total = 0;
            const itensArray = itensTexto.split(',').map(item => {
                const [nome, quantidade, preco] = item.split(':');
                const qtd = parseFloat(quantidade);
                const prc = parseFloat(preco);
                total += qtd * prc;
                return {nome, quantidade: qtd, preco: prc};
            });
            await fetch('/orcamentos', {
                method: 'POST',
                headers: {'Content-Type':'application/json'},
                body: JSON.stringify({cliente, itens: itensArray, total})
            });
            document.getElementById('cliente').value = '';
            document.getElementById('itens').value = '';
            carregarOrcamentos();
        });

        carregarOrcamentos();
    </script>
</body>
</html>
body {
    font-family: Arial, sans-serif;
    margin: 20px;
}

h1 {
    color: #2c3e50;
}

form input, form button {
    margin: 5px;
    padding: 5px;
}

nav a {
    margin-right: 10px;
    text-decoration: none;
    color: blue;
}
