<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Transferência entre Lojas</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f5f5f5;
      margin: 0;
      padding: 0;
    }

    .form-container {
      background-color: #ffffff;
      width: 80%;
      margin: 50px auto;
      padding: 30px;
      border-radius: 10px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    }

    .form-header {
      text-align: center;
      margin-bottom: 20px;
    }

    .form-title {
      font-size: 24px;
      font-weight: bold;
      color: #333;
    }

    .form-section {
      margin-bottom: 20px;
    }

    .form-section label {
      font-size: 16px;
      color: #333;
      display: block;
      margin-bottom: 8px;
    }

    .form-section input[type="text"],
    .form-section input[type="email"],
    .form-section textarea {
      width: 100%;
      padding: 10px;
      font-size: 16px;
      border: 1px solid #ccc;
      border-radius: 5px;
    }

    .submit-buttons {
      display: flex;
      justify-content: space-between;
    }

    .submit-buttons button {
      padding: 10px 20px;
      font-size: 16px;
      background-color: #4CAF50;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }

    .submit-buttons button:hover {
      background-color: #45a049;
    }

    .clear-button {
      background-color: #f44336;
    }

    #success-message {
      color: green;
      font-size: 16px;
      text-align: center;
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <div class="form-container">
    <div class="form-header">
      <h1 class="form-title">TRANSFERÊNCIA ENTRE LOJAS</h1>
    </div>

    <form id="transfer-form">
      <div class="form-section">
        <label for="numero-transferencia">Número da Transferência</label>
        <input type="text" id="numero-transferencia" name="numeroTransferencia" readonly>
      </div>

      <div class="form-section">
        <label for="filial-origem">Filial de Origem</label>
        <input type="text" id="filial-origem" name="filialOrigem" required>
      </div>

      <div class="form-section">
        <label for="filial-destino">Filial de Destino</label>
        <input type="text" id="filial-destino" name="filialDestino" required>
      </div>

      <div class="form-section">
        <label for="mercadorias">Mercadorias</label>
        <textarea id="mercadorias" name="mercadorias" rows="4" required></textarea>
      </div>

      <div id="success-message" style="display: none;">Formulário enviado com sucesso!</div>

      <div class="submit-buttons">
        <button type="reset" class="clear-button">Limpar formulário</button>
        <button type="submit" class="submit-button">Enviar</button>
      </div>
    </form>
  </div>

  <script>
    // Função para carregar o número de transferência
    window.onload = function () {
      google.script.run.withSuccessHandler(function(numeroAtual) {
        document.getElementById('numero-transferencia').value = numeroAtual;
      }).getNextTransferNumber();
    };

    // Envio do formulário
    document.getElementById('transfer-form').addEventListener('submit', function(event) {
      event.preventDefault();
      console.log("Botão de envio pressionado!"); // Log para verificar se o evento é acionado

      const form = event.target;
      const dados = {
        email: form.email.value,
        filialOrigem: form.filialOrigem.value,
        filialDestino: form.filialDestino.value,
        mercadorias: form.mercadorias.value,
        numeroTransferencia: form.numeroTransferencia.value
      };

      console.log("Dados do formulário", dados); // Log para ver os dados antes de enviar

      google.script.run.withSuccessHandler(function(novoNumero) {
        console.log("Dados processados com sucesso!");
        document.getElementById("success-message").style.display = 'block';
        setTimeout(function() {
          document.getElementById("success-message").style.display = 'none';
          form.reset();
          document.getElementById('numero-transferencia').value = novoNumero;
        }, 2000);
      }).processarFormulario(dados);
    });
  </script>
</body>
</html>
