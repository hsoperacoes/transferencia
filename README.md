<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Transferência entre Lojas</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
      background-color: #f4f4f4;
    }

    .form-container {
      width: 100%;
      max-width: 600px;
      margin: 50px auto;
      padding: 20px;
      background-color: #fff;
      border-radius: 8px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    }

    .form-header {
      text-align: center;
      margin-bottom: 30px;
    }

    .form-title {
      font-size: 24px;
      font-weight: bold;
      color: #333;
    }

    .question-container {
      margin-bottom: 20px;
    }

    .question-title {
      font-size: 16px;
      font-weight: bold;
      color: #333;
    }

    .required-star {
      color: red;
    }

    input[type="email"],
    select,
    textarea,
    input[type="text"] {
      width: 100%;
      padding: 10px;
      margin-top: 8px;
      border: 1px solid #ddd;
      border-radius: 4px;
      font-size: 14px;
    }

    textarea {
      height: 100px;
    }

    .submit-buttons {
      display: flex;
      justify-content: space-between;
      margin-top: 20px;
    }

    .submit-button,
    .clear-button {
      padding: 10px 20px;
      font-size: 14px;
      background-color: #007bff;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }

    .submit-button:hover,
    .clear-button:hover {
      background-color: #0056b3;
    }

    #success-message, #error-message {
      display: none;
      padding: 10px;
      margin-top: 20px;
      border-radius: 4px;
      text-align: center;
    }

    #success-message {
      background-color: #28a745;
      color: white;
    }

    #error-message {
      background-color: #dc3545;
      color: white;
    }

    .loading-overlay {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: rgba(0, 0, 0, 0.5);
      color: white;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      z-index: 1000;
    }

    .spinner {
      border: 4px solid transparent;
      border-top: 4px solid #fff;
      border-radius: 50%;
      width: 40px;
      height: 40px;
      animation: spin 2s linear infinite;
    }

    @keyframes spin {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }

    #numero-transferencia {
      display: none;
      margin-top: 20px;
      font-size: 18px;
      text-align: center;
    }

    .barcode-count {
      font-size: 16px;
      margin-top: 10px;
      font-weight: bold;
    }

    .count-number {
      color: red;
      font-weight: bold;
    }
  </style>
</head>
<body>
  <div class="form-container">
    <div class="form-header">
      <h1 class="form-title">TRANSFERÊNCIA ENTRE LOJAS</h1>
    </div>

    <form id="transfer-form">
      <div class="question-container">
        <div class="question-title">Email da filial de origem:(Preenchimento automatico) <span class="required-star">*</span></div>
        <input type="email" name="email" id="email" required readonly>
      </div>

      <div class="question-container">
        <div class="question-title">FILIAL ORIGEM <span class="required-star">*</span></div>
        <select name="filialOrigem" id="filial-origem" required onchange="atualizarEmail()">
          <option value="" disabled selected>Selecione</option>
          <option value="ARTUR">ARTUR</option>
          <option value="FLORIANO">FLORIANO</option>
          <option value="JOTA">JOTA</option>
          <option value="MODA">MODA</option>
          <option value="PONTO">PONTO</option>
          <option value="JA">JA</option>
          <option value="JE">JE</option>
        </select>
      </div>

      <div class="question-container">
        <div class="question-title">FILIAL DESTINO <span class="required-star">*</span></div>
        <select name="filialDestino" id="filial-destino" required>
          <option value="" disabled selected>Selecione</option>
          <option value="ARTUR">ARTUR</option>
          <option value="FLORIANO">FLORIANO</option>
          <option value="JOTA">JOTA</option>
          <option value="MODA">MODA</option>
          <option value="PONTO">PONTO</option>
          <option value="JA">JA</option>
          <option value="JE">JE</option>
        </select>
      </div>

      <div class="question-container">
        <div class="question-title">MERCADORIAS QUE ESTÃO SAINDO <span class="required-star">*</span></div>
        <textarea name="mercadorias" id="mercadorias" required placeholder="Inclua aqui os códigos de barras." oninput="contarLinhas()"></textarea>
      </div>

      <div id="barcode-count" class="barcode-count">Total de itens: <span id="total-itens" class="count-number">0</span></div>

      <div id="success-message">Transferência enviada com sucesso!</div>
      <div id="error-message"></div>

      <div class="submit-buttons">
        <button type="reset" class="clear-button">Limpar formulário</button>
        <button type="submit" class="submit-button" id="submit-button">Enviar</button>
      </div>
    </form>

    <div id="numero-transferencia">
      Número da transferência: <strong id="transfer-id"></strong>
    </div>
  </div>

  <div class="loading-overlay" id="loading-overlay">
    <div class="loading-content">
      <div class="spinner"></div>
      <h3>Processando sua transferência...</h3>
      <p>Aguarde enquanto enviamos os dados.</p>
    </div>
  </div>

  <script>
    function atualizarEmail() {
      const filialOrigem = document.getElementById('filial-origem').value;
      const filialDestinoSelect = document.getElementById('filial-destino');
      const emailPorFilial = {
        ARTUR: "heringarturmachado@gmail.com",
        FLORIANO: "hs.uberlandia.floriano@gmail.com",
        JOTA: "brunohenzo09@gmail.com",
        MODA: "dlaire28@gmail.com",
        PONTO: "soniameiry@gmail.com",
        JA: "jaugustocoliveira@terra.com.br",
        JE: "jeoliveira1966@gmail.com"
      };
      document.getElementById('email').value = emailPorFilial[filialOrigem] || "";

      Array.from(filialDestinoSelect.options).forEach(option => {
        option.disabled = option.value === filialOrigem;
      });

      if (filialDestinoSelect.value === filialOrigem) {
        filialDestinoSelect.value = "";
      }
    }

    function contarLinhas() {
      const mercadorias = document.getElementById('mercadorias').value;
      const linhas = mercadorias.split('\n').filter(linha => linha.trim() !== '');
      document.getElementById('total-itens').textContent = linhas.length;
    }

    document.addEventListener('DOMContentLoaded', function () {
      document.getElementById('transfer-form').addEventListener('submit', function (event) {
        event.preventDefault();
        enviarFormulario();
      });

      document.getElementById('filial-origem').addEventListener('change', atualizarEmail);
    });

    function enviarFormulario() {
      const form = document.getElementById('transfer-form');
      if (!form.checkValidity()) {
        mostrarMensagemErro("Por favor, preencha todos os campos obrigatórios!");
        return;
      }

      const formData = new FormData(form);
      const data = new URLSearchParams(formData).toString();

      document.getElementById('loading-overlay').style.display = 'flex';

      fetch("https://script.google.com/macros/s/AKfycbxu_jVaotWytMOQh4UCZetFZFOxgk5ePrOkaviDd-qKNPiu2_8BjCaNczAVZzaDwAbj/exec", {
        method: "POST",
        headers: { "Content-Type": "application/x-www-form-urlencoded" },
        body: data
      })
      .then(response => response.json())
      .then(responseData => {
        document.getElementById('loading-overlay').style.display = 'none';

        if (responseData.numeroTransferencia) {
          mostrarMensagemSucesso();
          exibirNumeroTransferencia(responseData.numeroTransferencia);
          setTimeout(limparFormulario, 5000);
        } else {
          mostrarMensagemErro("Erro ao enviar: Resposta inválida do servidor.");
        }
      })
      .catch(error => {
        document.getElementById('loading-overlay').style.display = 'none';
        mostrarMensagemErro("Erro ao enviar o formulário. Tente novamente.");
      });
    }

    function mostrarMensagemSucesso() {
      document.getElementById('success-message').style.display = 'block';
      document.getElementById('error-message').style.display = 'none';
    }

    function mostrarMensagemErro(mensagem) {
      document.getElementById('error-message').innerHTML = mensagem;
      document.getElementById('error-message').style.display = 'block';
      document.getElementById('success-message').style.display = 'none';
    }

    function exibirNumeroTransferencia(numero) {
      document.getElementById('numero-transferencia').style.display = 'block';
      document.getElementById('transfer-id').textContent = numero;
    }

    function limparFormulario() {
      document.getElementById('transfer-form').reset();
      document.getElementById('barcode-count').style.display = 'block';
      document.getElementById('numero-transferencia').style.display = 'none';
      document.getElementById('total-itens').textContent = '0';
      contarLinhas(); // Força atualização
    }
  </script>
</body>
</html>
