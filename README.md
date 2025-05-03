<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Transferência entre Filiais</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f4f4f4;
      padding: 20px;
    }
    .container {
      max-width: 600px;
      margin: auto;
      background: white;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    h2 {
      text-align: center;
      color: #333;
    }
    label {
      display: block;
      margin-top: 15px;
      font-weight: bold;
    }
    select, textarea, input[type="text"] {
      width: 100%;
      padding: 10px;
      margin-top: 5px;
      border: 1px solid #ccc;
      border-radius: 5px;
      box-sizing: border-box;
    }
    .buttons {
      display: flex;
      justify-content: space-between;
      margin-top: 20px;
    }
    button {
      padding: 10px 20px;
      border: none;
      border-radius: 5px;
      color: white;
      cursor: pointer;
      font-weight: bold;
    }
    .limpar {
      background-color: #4CAF50;
    }
    .enviar {
      background-color: #2196F3;
    }
    .mensagem {
      margin-top: 15px;
      text-align: center;
      font-weight: bold;
      color: green;
    }
  </style>
</head>
<body>
  <div class="container">
    <h2>TRANSFERÊNCIA ENTRE FILIAIS</h2>
    <form id="formTransferencia">
      <label for="filialOrigem">Filial Origem *</label>
      <select id="filialOrigem" name="filialOrigem" required>
        <option value="">Selecione</option>
        <option value="Loja 1">Loja 1</option>
        <option value="Loja 2">Loja 2</option>
        <option value="Loja 3">Loja 3</option>
      </select>

      <label for="filialDestino">Filial Destino *</label>
      <select id="filialDestino" name="filialDestino" required>
        <option value="">Selecione</option>
        <option value="Loja 1">Loja 1</option>
        <option value="Loja 2">Loja 2</option>
        <option value="Loja 3">Loja 3</option>
      </select>

      <label for="mercadorias">Mercadorias que estão saindo *</label>
      <textarea id="mercadorias" name="mercadorias" rows="4" required></textarea>

      <label for="numeroTransferencia">Número da Transferência</label>
      <input type="text" id="numeroTransferencia" name="numeroTransferencia" readonly />

      <div class="buttons">
        <button type="reset" class="limpar">Limpar formulário</button>
        <button type="submit" class="enviar">Enviar</button>
      </div>

      <div id="mensagem" class="mensagem"></div>
    </form>
  </div>

  <script>
    // Geração do número da transferência ao carregar a página
    document.addEventListener("DOMContentLoaded", function () {
      gerarNumeroTransferencia();
    });

    function gerarNumeroTransferencia() {
      const campoNumero = document.getElementById("numeroTransferencia");
      const agora = new Date();
      const numero = `TRF-${agora.getFullYear()}${(agora.getMonth() + 1).toString().padStart(2, "0")}${agora.getDate().toString().padStart(2, "0")}-${Math.floor(Math.random() * 9000 + 1000)}`;
      campoNumero.value = numero;
    }

    // Submissão do formulário
    document.getElementById("formTransferencia").addEventListener("submit", function (e) {
      e.preventDefault(); // Impede envio real

      // Simular envio e mostrar mensagem de sucesso
      document.getElementById("mensagem").textContent = "Formulário enviado com sucesso!";
      
      // Aguarda 2 segundos, limpa e regenera número
      setTimeout(() => {
        this.reset();
        gerarNumeroTransferencia();
        document.getElementById("mensagem").textContent = "";
      }, 2000);
    });
  </script>
</body>
</html>
