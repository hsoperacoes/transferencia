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
    }
    .submit-buttons {
      text-align: center;
    }
    .submit-button, .clear-button {
      padding: 10px 20px;
      margin: 5px;
      border: none;
      background-color: #4CAF50;
      color: white;
      font-size: 16px;
      cursor: pointer;
    }
    .submit-button:hover, .clear-button:hover {
      background-color: #45a049;
    }
    #success-message {
      color: green;
      font-weight: bold;
      text-align: center;
      display: none;
      margin: 20px 0;
      padding: 10px;
      background-color: #e8f5e9;
      border-radius: 4px;
    }
    #error-message {
      color: red;
      font-weight: bold;
      text-align: center;
      display: none;
      margin: 20px 0;
      padding: 10px;
      background-color: #ffebee;
      border-radius: 4px;
    }
    .loading {
      display: none;
      text-align: center;
      margin: 20px 0;
    }
    .loading-spinner {
      border: 4px solid #f3f3f3;
      border-top: 4px solid #4CAF50;
      border-radius: 50%;
      width: 30px;
      height: 30px;
      animation: spin 1s linear infinite;
      margin: 0 auto;
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
        <textarea name="mercadorias" id="mercadorias" required placeholder="Descreva as mercadorias que estão sendo transferidas"></textarea>
      </div>

      <!-- Número da Transferência -->
      <div class="question-container">
        <div class="question-title">NÚMERO DA TRANSFERÊNCIA</div>
        <input type="text" id="numero-transferencia" name="numeroTransferencia" readonly>
      </div>

      <div id="success-message">Formulário enviado com sucesso!</div>
      <div id="error-message"></div>
      
      <div class="loading">
        <div class="loading-spinner"></div>
        <p>Enviando dados, por favor aguarde...</p>
      </div>

      <div class="submit-buttons">
        <button type="reset" class="clear-button">Limpar formulário</button>
        <button type="submit" class="submit-button" id="submit-button">Enviar</button>
      </div>
    </form>
  </div>

  <script>
    function atualizarEmail() {
      const emailInput = document.getElementById('email');
      const filialOrigem = document.getElementById('filial-origem').value;
      const filialDestinoSelect = document.getElementById('filial-destino');

      // Atualiza email com base na filial de origem
      const emailPorFilial = {
        AATUR: "hs.operacoes.loja@gmail.com",
        FLORIANO: "hs.operacoes.loja@gmail.com",
        JOTA: "hs.operacoes.loja@gmail.com",
        MOGA: "hs.operacoes.loja@gmail.com",
        PONTO: "hs.operacoes.loja@gmail.com",
        JA: "hs.operacoes.loja@gmail.com",
        JE: "hs.operacoes.loja@gmail.com"
      };

      emailInput.value = emailPorFilial[filialOrigem] || "";
      
      // Desabilita a filial de origem na seleção de destino
      Array.from(filialDestinoSelect.options).forEach(option => {
        option.disabled = option.value === filialOrigem;
      });
      
      // Reseta a seleção se for a mesma filial
      if (filialDestinoSelect.value === filialOrigem) {
        filialDestinoSelect.value = "";
      }
    }

    window.onload = function() {
      // Carrega o número da transferência
      google.script.run
        .withSuccessHandler(function(numeroAtual) {
          document.getElementById('numero-transferencia').value = numeroAtual;
        })
        .withFailureHandler(function(error) {
          console.error("Erro ao obter número:", error);
          document.getElementById('numero-transferencia').value = "TRF-001";
          document.getElementById('error-message').textContent = "Erro ao carregar número da transferência. Use TRF-001 como fallback.";
          document.getElementById('error-message').style.display = 'block';
          setTimeout(() => {
            document.getElementById('error-message').style.display = 'none';
          }, 5000);
        })
        .obterNumeroTransferencia();
        
      // Configura o evento de change para a filial de origem
      document.getElementById('filial-origem').addEventListener('change', atualizarEmail);
    };

    document.getElementById('transfer-form').addEventListener('submit', function(event) {
      event.preventDefault();
      
      // Validação básica
      if (!this.checkValidity()) {
        document.getElementById('error-message').textContent = "Por favor, preencha todos os campos obrigatórios!";
        document.getElementById('error-message').style.display = 'block';
        setTimeout(() => {
          document.getElementById('error-message').style.display = 'none';
        }, 5000);
        return;
      }
      
      // Verifica se origem e destino são diferentes
      const origem = document.getElementById('filial-origem').value;
      const destino = document.getElementById('filial-destino').value;
      
      if (origem === destino) {
        document.getElementById('error-message').textContent = "A filial de origem e destino devem ser diferentes!";
        document.getElementById('error-message').style.display = 'block';
        setTimeout(() => {
          document.getElementById('error-message').style.display = 'none';
        }, 5000);
        return;
      }

      // Mostra loading
      document.querySelector('.loading').style.display = 'block';
      document.getElementById('submit-button').disabled = true;

      const form = event.target;
      const dados = {
        email: form.email.value,
        filialOrigem: form.filialOrigem.value,
        filialDestino: form.filialDestino.value,
        mercadorias: form.mercadorias.value,
        numeroTransferencia: form.numeroTransferencia.value
      };

      // Envia os dados
      google.script.run
        .withSuccessHandler(function(novoNumero) {
          document.getElementById("success-message").style.display = 'block';
          document.querySelector('.loading').style.display = 'none';
          
          setTimeout(function() {
            document.getElementById("success-message").style.display = 'none';
            form.reset();
            document.getElementById('numero-transferencia').value = novoNumero;
            document.getElementById('submit-button').disabled = false;
            // Mantém o email preenchido se a filial for selecionada novamente
            if (document.getElementById('filial-origem').value) {
              atualizarEmail();
            }
          }, 3000);
        })
        .withFailureHandler(function(error) {
          console.error("Erro ao enviar:", error);
          document.querySelector('.loading').style.display = 'none';
          document.getElementById('submit-button').disabled = false;
          
          document.getElementById('error-message').textContent = "Erro ao enviar formulário: " + error.message;
          document.getElementById('error-message').style.display = 'block';
          setTimeout(() => {
            document.getElementById('error-message').style.display = 'none';
          }, 5000);
        })
        .processarFormulario(dados);
    });
  </script>
</body>
</html>
