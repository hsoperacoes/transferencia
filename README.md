<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Transferência entre Lojas</title>
  <style>
    /* (os estilos permanecem os mesmos, sem alterações) */
    .form-container {
      width: 80%;
      max-width: 600px;
      margin: 0 auto;
      font-family: Arial, sans-serif;
      padding: 20px;
      border: 1px solid #ddd;
      border-radius: 8px;
    }

    .form-header { text-align: center; margin-bottom: 20px; }
    .form-title { font-size: 24px; font-weight: bold; }
    .question-container { margin-bottom: 15px; }
    .question-title { font-size: 16px; margin-bottom: 5px; }
    .required-star { color: red; }
    select, textarea, input {
      width: 100%; padding: 8px; margin: 5px 0;
      border: 1px solid #ddd; border-radius: 4px;
    }
    .submit-buttons {
      display: flex; justify-content: space-between; margin-top: 20px;
    }
    .clear-button {
      background-color: #f44336; color: white; border: none;
      padding: 10px; cursor: pointer; border-radius: 4px;
    }
    .submit-button {
      background-color: #4CAF50; color: white; border: none;
      padding: 10px; cursor: pointer; border-radius: 4px;
    }
    #success-message, #error-message {
      text-align: center; margin-top: 20px; font-size: 16px;
    }
    #success-message { color: green; display: none; }
    #error-message { color: red; display: none; }
    #numero-transferencia {
      margin-top: 20px; font-size: 18px; display: none;
    }
    .loading-overlay {
      position: fixed; top: 0; left: 0; width: 100%; height: 100%;
      background-color: rgba(0, 0, 0, 0.7); display: none;
      justify-content: center; align-items: center; z-index: 999;
    }
    .loading-content {
      text-align: center; color: white;
    }
    .spinner {
      border: 4px solid #f3f3f3;
      border-top: 4px solid #3498db;
      border-radius: 50%;
      width: 50px;
      height: 50px;
      animation: spin 2s linear infinite;
    }
    @keyframes spin {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
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
        <div class="question-title">Enviar por email <span class="required-star">*</span></div>
        <input type="email" name="email" id="email" required readonly>
      </div>

      <div class="question-container">
        <div class="question-title">FILIAL ORIGEM <span class="required-star">*</span></div>
        <select name="filialOrigem" id="filial-origem" required onchange="atualizarEmail()">
          <option value="" disabled selected>Selecione</option>
          <option value="AATUR">AATUR</option>
          <option value="FLORIANO">FLORIANO</option>
          <option value="JOTA">JOTA</option>
          <option value="MOGA">MOGA</option>
          <option value="PONTO">PONTO</option>
          <option value="JA">JA</option>
          <option value="JE">JE</option>
        </select>
      </div>

      <div class="question-container">
        <div class="question-title">FILIAL DESTINO <span class="required-star">*</span></div>
        <select name="filialDestino" id="filial-destino" required>
          <option value="" disabled selected>Selecione</option>
          <option value="AATUR">AATUR</option>
          <option value="FLORIANO">FLORIANO</option>
          <option value="JOTA">JOTA</option>
          <option value="MOGA">MOGA</option>
          <option value="PONTO">PONTO</option>
          <option value="JA">JA</option>
          <option value="JE">JE</option>
        </select>
      </div>

      <div class="question-container">
        <div class="question-title">MERCADORIAS QUE ESTÃO SAINDO <span class="required-star">*</span></div>
        <textarea name="mercadorias" id="mercadorias" required placeholder="Descreva detalhadamente as mercadorias que estão sendo transferidas"></textarea>
      </div>

      <div id="success-message">Formulário enviado com sucesso!</div>
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
        AATUR: "hs.operacoes.loja@gmail.com",
        FLORIANO: "hs.operacoes.loja@gmail.com",
        JOTA: "hs.operacoes.loja@gmail.com",
        MOGA: "hs.operacoes.loja@gmail.com",
        PONTO: "hs.operacoes.loja@gmail.com",
        JA: "hs.operacoes.loja@gmail.com",
        JE: "hs.operacoes.loja@gmail.com"
      };
      document.getElementById('email').value = emailPorFilial[filialOrigem] || "";

      Array.from(filialDestinoSelect.options).forEach(option => {
        option.disabled = option.value === filialOrigem;
      });

      if (filialDestinoSelect.value === filialOrigem) {
        filialDestinoSelect.value = "";
      }
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
        headers: {
          "Content-Type": "application/x-www-form-urlencoded"
        },
        body: data
      })
      .then(response => response.json())
      .then(responseData => {
        document.getElementById('loading-overlay').style.display = 'none';

        if (responseData.success) {
          mostrarMensagemSucesso();
          exibirNumeroTransferencia(responseData.numeroTransferencia);
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
  </script>
</body>
</html>
