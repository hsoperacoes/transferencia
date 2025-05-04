<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Transferência entre Lojas</title>
  <style>
    /* Estilos permanecem os mesmos */
  </style>
</head>
<body>
  <div class="form-container">
    <div class="form-header">
      <h1 class="form-title">TRANSFERÊNCIA ENTRE LOJAS</h1>
    </div>

    <form id="transfer-form" method="POST">
      <!-- Campo de e-mail -->
      <div class="question-container">
        <div class="question-title">Enviar por email <span class="required-star">*</span></div>
        <input type="email" name="email" id="email" required readonly>
      </div>

      <!-- Filial Origem -->
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

      <!-- Filial Destino -->
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

      <!-- Mercadorias -->
      <div class="question-container">
        <div class="question-title">MERCADORIAS QUE ESTÃO SAINDO <span class="required-star">*</span></div>
        <textarea name="mercadorias" id="mercadorias" required placeholder="Descreva detalhadamente as mercadorias que estão sendo transferidas"></textarea>
      </div>

      <!-- Mensagens de feedback -->
      <div id="success-message">Formulário enviado com sucesso!</div>
      <div id="error-message"></div>

      <div class="submit-buttons">
        <button type="reset" class="clear-button">Limpar formulário</button>
        <button type="submit" class="submit-button" id="submit-button">Enviar</button>
      </div>
    </form>

    <!-- Número da Transferência -->
    <div id="numero-transferencia">
      Número da transferência: <strong id="transfer-id"></strong>
    </div>
  </div>

  <!-- Loading Overlay -->
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

    document.addEventListener('DOMContentLoaded', function() {
      document.getElementById('transfer-form').addEventListener('submit', function(event) {
        event.preventDefault();
        enviarFormulario();
      });

      document.getElementById('filial-origem').addEventListener('change', atualizarEmail);
    });

    function enviarFormulario() {
      const form = document.getElementById('transfer-form');
      const submitButton = document.getElementById('submit-button');

      if (!form.checkValidity()) {
        mostrarMensagemErro("Por favor, preencha todos os campos obrigatórios!");
        return;
      }

      const formData = new FormData(form);
      const data = new URLSearchParams(formData).toString();

      // Exibe overlay de carregamento
      document.getElementById('loading-overlay').style.display = 'flex';

      // Envia dados via fetch
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
        mostrarMensagemSucesso();
        exibirNumeroTransferencia();
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

    function exibirNumeroTransferencia() {
      const transferId = Math.floor(Math.random() * 100000); // Simula um número de transferência
      document.getElementById('numero-transferencia').style.display = 'block';
      document.getElementById('transfer-id').textContent = transferId;
    }
  </script>
</body>
</html>
