# Gerar o arquivo zip mais uma vez e garantir que está acessível
import zipfile

zip_path = "/mnt/data/brasil-distribuidora-site.zip"

html_code = """<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Brasil Distribuidora</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background-color: #121212;
      color: white;
    }
    header {
      background-color: #1f1f1f;
      padding: 20px;
      text-align: center;
    }
    h1 {
      margin: 0;
    }
    .container {
      padding: 20px;
    }
    .produtos {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 20px;
    }
    .produto {
      background-color: #222;
      border-radius: 10px;
      padding: 15px;
      text-align: center;
    }
    .produto img {
      max-width: 100%;
      border-radius: 8px;
    }
    .produto h2 {
      margin: 10px 0 5px;
    }
    .produto p {
      margin: 0 0 10px;
    }
    .produto button {
      background-color: #00c853;
      color: white;
      border: none;
      padding: 10px;
      width: 100%;
      font-size: 14px;
      border-radius: 6px;
      cursor: pointer;
    }
    .produto button:hover {
      background-color: #00e676;
    }
    .carrinho {
      margin-top: 30px;
      background-color: #1e1e1e;
      padding: 20px;
      border-radius: 10px;
    }
    input, textarea {
      width: 100%;
      padding: 10px;
      margin-top: 10px;
      border: none;
      border-radius: 5px;
      background-color: #333;
      color: white;
    }
    button.finalizar {
      background-color: #ffd600;
      color: black;
      font-weight: bold;
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <header>
    <h1>Brasil Distribuidora</h1>
    <p>Bebidas geladas na sua porta!</p>
  </header>

  <div class="container">
    <div class="produtos">
      <div class="produto">
        <img src="https://images.unsplash.com/photo-1600431521340-491eca880813" alt="Skol" />
        <h2>Skol Lata 350ml</h2>
        <p>R$ 3,50</p>
        <button onclick="adicionarItem('Skol Lata 350ml', 3.50)">Adicionar</button>
      </div>
      <div class="produto">
        <img src="https://images.unsplash.com/photo-1614361751013-c47f1ebf1cf9" alt="Coca-Cola" />
        <h2>Coca-Cola 2L</h2>
        <p>R$ 8,00</p>
        <button onclick="adicionarItem('Coca-Cola 2L', 8.00)">Adicionar</button>
      </div>
      <div class="produto">
        <img src="https://images.unsplash.com/photo-1582152627354-ffb8b1be3c2f" alt="Smirnoff" />
        <h2>Vodka Smirnoff 1L</h2>
        <p>R$ 29,90</p>
        <button onclick="adicionarItem('Vodka Smirnoff 1L', 29.90)">Adicionar</button>
      </div>
    </div>

    <div class="carrinho">
      <h3>Itens do Pedido:</h3>
      <ul id="listaCarrinho"></ul>
      <p><strong>Total: R$ <span id="total">0.00</span></strong></p>

      <h3>Seu nome:</h3>
      <input type="text" id="nome" placeholder="Digite seu nome" />

      <h3>Endereço para entrega:</h3>
      <textarea id="endereco" rows="3" placeholder="Rua, número, bairro..."></textarea>

      <button class="finalizar" onclick="enviarWhatsApp()">Finalizar Pedido no WhatsApp</button>
    </div>
  </div>

  <script>
    let carrinho = [];

    function adicionarItem(nome, preco) {
      carrinho.push({ nome, preco });
      atualizarCarrinho();
    }

    function atualizarCarrinho() {
      const lista = document.getElementById('listaCarrinho');
      lista.innerHTML = '';
      let total = 0;

      carrinho.forEach(item => {
        const li = document.createElement('li');
        li.textContent = `${item.nome} - R$ ${item.preco.toFixed(2)}`;
        lista.appendChild(li);
        total += item.preco;
      });

      document.getElementById('total').textContent = total.toFixed(2);
    }

    function enviarWhatsApp() {
      const nome = document.getElementById('nome').value;
      const endereco = document.getElementById('endereco').value;
      if (!nome || !endereco || carrinho.length === 0) {
        alert('Preencha seu nome, endereço e selecione pelo menos 1 produto.');
        return;
      }

      let mensagem = `Olá! Meu nome é ${nome} e gostaria de fazer um pedido:\\n`;
      carrinho.forEach(item => {
        mensagem += `- ${item.nome} - R$ ${item.preco.toFixed(2)}\\n`;
      });
      const total = carrinho.reduce((acc, item) => acc + item.preco, 0);
      mensagem += `\\nTotal: R$ ${total.toFixed(2)}\\nEndereço: ${endereco}`;

      const numero = '5538999030706';
      const link = `https://wa.me/${numero}?text=${encodeURIComponent(mensagem)}`;
      window.open(link, '_blank');
    }
  </script>
</body>
</html>"""

with zipfile.ZipFile(zip_path, 'w') as zipf:
    zipf.writestr("index.html", html_code)

zip_path
