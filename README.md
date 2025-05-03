<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Transferência entre Lojas</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f4f4f4;
      margin: 0;
      padding: 0;
    }
    .form-container {
      width: 50%;
      margin: 50px auto;
      background-color: white;
      padding: 20px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }
    .form-header {
      text-align: center;
      margin-bottom: 20px;
    }
    .form-title {
      font-size: 24px;
      color: #333;
    }
    .question-container {
      margin-bottom: 15px;
    }
    .question-title {
      font-size: 16px;
      margin-bottom: 5px;
    }
    .required-star {
      color: red;
    }
    input[type="email"], input[type="text"], select, textarea {
      width: 100%;
      padding: 10px;
      margin: 5px 0 20px 0;
      border: 1px solid #ccc;
      border-radius: 4px;
      box-sizing: border-box;
    }
    textarea {
      min-height: 100px;
      resize: vertical;
    }
    .submit-buttons {
      text-align: center;
      margin-top: 30px;
    }
    .submit-button, .clear-button {
      padding: 12px 25px;
      margin: 0 10px;
      border: none;
      border-radius: 4px;
      background-color: #4CAF50;
      color: white;
      font-size: 16px;
      cursor: pointer;
      transition: background-color 0.3s;
    }
    .clear-button {
      background-color: #f44336;
    }
    .submit-button:hover {
      background-color: #45a049;
    }
    .clear-button:hover {
      background-color: #d32f2f;
    }
    #success-message {
      color: #2e7d32;
      font-weight: bold;
      text-align: center;
      display: none;
      margin: 20px 0;
      padding: 15px;
      background-color: #e8f5e9;
      border-radius: 4px;
      border-left: 5px solid #2e7d32;
    }
    #error-message {
      color: #c62828;
      font-weight: bold;
      text-align: center;
      display: none;
      margin: 20px 0;
      padding: 15px;
      background-color: #ffebee;
      border-radius: 4px;
      border-left: 5px solid #c62828;
    }
    .loading-overlay {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(0,0,0,0.5);
      z-index: 1000;
      justify-content: center;
      align-items: center;
    }
    .loading-content {
      background-color: white;
      padding: 30px;
      border-radius: 8px;
      text-align: center;
    }
    .spinner {
      border: 5px solid #f3f3f3;
      border-top: 5px solid #4CAF50;
      border-radius: 50%;
      width: 50px;
      height: 50px;
      animation: spin 1s linear infinite;
      margin: 0 auto 20px;
    }
    @keyframes spin {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }
    .hidden {
      display: none;
    }
  </style>
</head>
<body>
  <div class="form-container">
    <div class="form-header">
      <h1 class="form-title">TRANSFERÊNCIA ENTRE LOJAS</h1>
    </div>

    <form id="transfer-form">
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

      <!-- Número da Transferência -->
      <div class="question-container">
        <div class="question-title">NÚMERO DA TRANSFERÊNCIA</div>
        <input type="text" id="numero-transferencia" name="numeroTransferencia" readonly>
      </div>

      <!-- Mensagens de feedback -->
      <div id="success-message">
        <i class="fas fa-check-circle"></i> Formulário enviado com sucesso!
      </div>
      <div id="error-message"></div>

      <div class="submit-buttons">
        <button type="reset" class="clear-button">Limpar formulário</button>
        <button type="submit" class="submit-button" id="submit-button">Enviar</button>
      </div>
    </form>
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
    // Função para atualizar o email com base na filial selecionada
    function atualizarEmail() {
      const filialOrigem = document.getElementById('filial-origem').value;
      const filialDestinoSelect = document.getElementById('filial-destino');
      
      // Mapeamento de emails por filial
      const emailPorFilial = {
        AATUR: "hs.operacoes.loja@gmail.com",
        FLORIANO: "hs.operacoes.loja@gmail.com",
        JOTA: "hs.operacoes.loja@gmail.com",
        MOGA: "hs.operacoes.loja@gmail.com",
        PONTO: "hs.operacoes.loja@gmail.com",
        JA: "hs.operacoes.loja@gmail.com",
        JE: "hs.operacoes.loja@gmail.com"
      };
      
      // Atualiza o campo de email
      document.getElementById('email').value = emailPorFilial[filialOrigem] || "";
      
      // Desabilita a filial de origem na seleção de destino
      Array.from(filialDestinoSelect.options).forEach(option => {
        option.disabled = option.value === filialOrigem;
      });
      
      // Reseta a seleção se for a mesma filial
      if (filialDestinoSelect.value === filialOrigem) {
        filialDestinoSelect.value = "";
      }
    }

    // Quando o DOM estiver completamente carregado
    document.addEventListener('DOMContentLoaded', function() {
      // Carrega o número inicial da transferência
      carregarNumeroTransferencia();
      
      // Configura o evento de submit do formulário
      document.getElementById('transfer-form').addEventListener('submit', function(event) {
        event.preventDefault();
        enviarFormulario();
      });
      
      // Configura o evento de change para a filial de origem
      document.getElementById('filial-origem').addEventListener('change', atualizarEmail);
    });

    // Função para carregar o número da transferência
    function carregarNumeroTransferencia() {
      google.script.run
        .withSuccessHandler(function(numeroAtual) {
          document.getElementById('numero-transferencia').value = numeroAtual;
        })
        .withFailureHandler(function(error) {
          console.error("Erro ao obter número:", error);
          document.getElementById('numero-transferencia').value = "TRF-001";
          mostrarMensagemErro("Erro ao carregar número da transferência. Usando número padrão.");
        })
        .obterNumeroTransferencia();
    }

    // Função para enviar o formulário
    function enviarFormulario() {
      const form = document.getElementById('transfer-form');
      const submitButton = document.getElementById('submit-button');
      
      // Validação básica
      if (!form.checkValidity()) {
        mostrarMensagemErro("Por favor, preencha todos os campos obrigatórios!");
        return;
      }
      
      // Verifica se origem e destino são diferentes
      const origem = document.getElementById('filial-origem').value;
      const destino = document.getElementById('filial-destino').value;
      
      if (origem === destino) {
        mostrarMensagemErro("A filial de origem e destino devem ser diferentes!");
        return;
      }

      // Prepara os dados do formulário
      const dados = {
        email: form.email.value,
        filialOrigem: origem,
        filialDestino: destino,
        mercadorias: form.mercadorias.value,
        numeroTransferencia: form.numeroTransferencia.value
      };

      // Mostra o loading
      mostrarLoading(true);
      submitButton.disabled = true;

      // Envia os dados para o Google Apps Script
      google.script.run
        .withSuccessHandler(function(novoNumero) {
          mostrarMensagemSucesso();
          document.getElementById('numero-transferencia').value = novoNumero;
          
          // Esconde o loading após 1 segundo
          setTimeout(function() {
            mostrarLoading(false);
            submitButton.disabled = false;
          }, 1000);
        })
        .withFailureHandler(function(error) {
          console.error("Erro ao enviar formulário:", error);
          mostrarMensagemErro("Erro ao enviar formulário: " + error.message);
          mostrarLoading(false);
          submitButton.disabled = false;
        })
        .processarFormulario(dados);
    }

    // Função para mostrar/ocultar o loading
    function mostrarLoading(mostrar) {
      document.getElementById('loading-overlay').style.display = mostrar ? 'flex' : 'none';
    }

    // Função para mostrar mensagem de sucesso
    function mostrarMensagemSucesso() {
      const successMsg = document.getElementById('success-message');
      successMsg.style.display = 'block';
      
      // Esconde a mensagem após 5 segundos
      setTimeout(function() {
        successMsg.style.display = 'none';
      }, 5000);
    }

    // Função para mostrar mensagem de erro
    function mostrarMensagemErro(mensagem) {
      const errorMsg = document.getElementById('error-message');
      errorMsg.textContent = mensagem;
      errorMsg.style.display = 'block';
      
      // Esconde a mensagem após 5 segundos
      setTimeout(function() {
        errorMsg.style.display = 'none';
      }, 5000);
    }
  </script>
</body>
</html>
