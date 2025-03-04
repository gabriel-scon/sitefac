<!DOCTYPE html>
<html lang="pt-BR">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Pedido Online</title>
    <style>
      body {
        font-family: Arial, sans-serif;
        text-align: center;
        padding: 20px;
        background: linear-gradient(135deg, #5200b1 0%, #000000 100%);
        color: #fff;
        height: 100vh;
      }

      h2 {
        color: #fff;
        margin-bottom: 20px;
        font-size: 2.5em;
        font-weight: bold;
      }

      form {
        max-width: 500px;
        margin: auto;
        padding: 20px;
        background: #240377;
        box-shadow: 0 2px 15px rgba(0, 0, 0, 0.1);
        border-radius: 15px;
      }

      label {
        font-weight: bold;
        display: block;
        margin: 10px 0 5px;
        color: #ffffff;
      }

      input,
      select,
      button {
        width: 100%;
        padding: 12px;
        margin: 5px 0 15px;
        border: 1px solid #ccc;
        border-radius: 5px;
        box-sizing: border-box;
        font-size: 1.1em;
      }

      button {
        background-color: #00000083;
        border: none;
        color: white;
        font-size: 16px;
        cursor: pointer;
        padding: 12px;
        border-radius: 5px;
        transition: background-color 0.3s ease-in-out;
      }

      button:hover {
        background-color: #ea00ff;
      }

      button:active {
        background-color: #ea00ff;
      }

      input[readonly] {
        background: #f5f5f5;
        font-weight: bold;
        color: #888;
      }

      .produto-container {
        position: relative;
        margin-bottom: 15px;
      }

      .produto-container img {
        width: 30px; /* Reduzi o tamanho da imagem */
        position: absolute;
        left: 10px;
        top: 50%;
        transform: translateY(-50%);
      }

      .produto-select {
        width: 100%;
        padding: 12px;
        margin: 5px 0 15px;
        border: 1px solid #ccc;
        border-radius: 5px;
        font-size: 1.1em;
        padding-left: 45px; /* Ajuste para o espaço da imagem */
        background-color: #fff;
        text-align: left;
        height: 50px; /* Ajustei a altura do campo para ficar alinhado com a imagem */
      }

      .total-container {
        margin-top: 15px;
        font-size: 1.3em;
        font-weight: bold;
        color: #ea00ff;
      }
    </style>
  </head>
  <body>
    <h2>Faça seu Pedido</h2>
    <form id="pedidoForm">
      <label>Facção Compradora:</label>
      <input
        type="text"
        id="faccao"
        placeholder="Nome da facção / Número"
        required
      />

      <label>Produto:</label>
      <div class="produto-container">
        <img
          id="produtoImg"
          src="https://images.vexels.com/media/users/3/156800/isolated/preview/645abae8c53b50a8034ec59bfdb92116-icone-de-colete-salva-vidas.png"
          alt="Produto"
        />
        <select
          id="produto"
          class="produto-select"
          required
          onchange="atualizarImagem()"
        >
          <option
            value="COLETE"
            data-img="https://images.vexels.com/media/users/3/156800/isolated/preview/645abae8c53b50a8034ec59bfdb92116-icone-de-colete-salva-vidas.png"
          >
            Colete
          </option>
          <option
            value="C4"
            data-img="https://cdn-icons-png.flaticon.com/512/4613/4613379.png"
          >
            C4
          </option>
          <option
            value="ALGEMA"
            data-img="https://cdn-icons-png.flaticon.com/512/2765/2765259.png"
          >
            Algema
          </option>
          <option
            value="ATTACHS"
            data-img="https://www.thinlinesanctuary.com/cdn/shop/products/glockattachments_d0820114-2fdf-4650-bb1b-81234757c7b8.png?v=1657104268"
          >
            Attachs
          </option>
        </select>
      </div>

      <label>Quantidade:</label>
      <input
        type="number"
        id="quantidade"
        min="1"
        required
        onchange="atualizarTotal()"
      />

      <label>Parceria:</label>
      <select id="parceria" required onchange="atualizarTotal()">
        <option value="com">Com Parceria</option>
        <option value="sem">Sem Parceria</option>
      </select>

      <div class="total-container" id="totalContainer">
        Total a Pagar: R$ 0,00
      </div>

      <button type="submit">Enviar Pedido</button>
    </form>

    <script>
      const precos = {
        COLETE: { com: 6000, sem: 7000 },
        C4: { com: 1000, sem: 1500 },
        ALGEMA: { com: 2500, sem: 3000 },
        ATTACHS: { com: 4000, sem: 5000 },
      };

      function atualizarImagem() {
        const select = document.getElementById("produto");
        const img = document.getElementById("produtoImg");
        const selectedOption = select.options[select.selectedIndex];
        img.src = selectedOption.getAttribute("data-img");
        atualizarTotal();
      }

      function atualizarTotal() {
        const produto = document.getElementById("produto").value;
        const quantidade =
          parseInt(document.getElementById("quantidade").value) || 0;
        const parceria = document.getElementById("parceria").value;
        const valorUnitario = precos[produto][parceria];
        const total = quantidade * valorUnitario;
        document.getElementById(
          "totalContainer"
        ).textContent = `Total a Pagar: R$ ${total.toFixed(2)}`;
      }

      document
        .getElementById("pedidoForm")
        .addEventListener("submit", function (event) {
          event.preventDefault();

          const faccao = document.getElementById("faccao").value;
          const produto = document.getElementById("produto").value;
          const quantidade = parseInt(
            document.getElementById("quantidade").value
          );
          const parceria = document.getElementById("parceria").value;
          const valorUnitario = precos[produto][parceria];
          const total = quantidade * valorUnitario;
          const imgSrc = document.getElementById("produtoImg").src;

          const webhookURL =
            "https://discord.com/api/webhooks/1346544790358982656/E9UAi16nw3Z5CVxs-cspKCVRfSH2IChLICUFHogtZyVCJaPoumkld81-YEKZYcFy_QBA";

          const pedido = {
            content: `📦 **Novo Pedido** 📦\n📣 Facção: ${faccao}\n📬 Produto: ${produto}\n🧮 Quantidade: ${quantidade}\n💰 Valor Unitário: R$ ${valorUnitario}\n💵 Total: R$ ${total}`,
            embeds: [
              {
                image: {
                  url: imgSrc,
                },
              },
            ],
          };

          fetch(webhookURL, {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify(pedido),
          })
            .then(() => {
              alert("Pedido enviado com sucesso!");
              document.getElementById("pedidoForm").reset();
              document.getElementById("produtoImg").src =
                "https://images.vexels.com/media/users/3/156800/isolated/preview/645abae8c53b50a8034ec59bfdb92116-icone-de-colete-salva-vidas.png";
              document.getElementById("totalContainer").textContent =
                "Total a Pagar: R$ 0,00";
            })
            .catch(() => alert("Erro ao enviar o pedido."));
        });

      document.addEventListener("DOMContentLoaded", atualizarTotal);
    </script>
  </body>
</html>
