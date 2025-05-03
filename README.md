<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TRANSFERÊNCIA ENTRE LOJAS</title>
    <style>
        /* Layout anterior restaurado */
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
    </style>
</head>
<body>
    <div class="form-container">
        <div class="form-header">
            <h1 class="form-title">TRANSFERÊNCIA ENTRE LOJAS</h1>
        </div>

        <form action="https://script.google.com/macros/s/AKfycbxu_jVaotWytMOQh4UCZetFZFOxgk5ePrOkaviDd-qKNPiu2_8BjCaNczAVZzaDwAbj/exec" method="POST">
            <!-- Campo de e-mail -->
            <div class="question-container">
                <div class="question-title">Enviar por email <span class="required-star">*</span></div>
                <input type="email" name="email" id="email" required readonly>
            </div>

            <!-- Filial Origem -->
            <div class="question-container">
                <div class="question-title">FILIAL ORIGEM <span class="required-star">*</span></div>
                <select name="filial-origem" id="filial-origem" required onchange="atualizarEmail()">
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
                <select name="filial-destino" required>
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
                <textarea name="mercadorias" required placeholder="Sua resposta"></textarea>
            </div>

            <!-- Número da Transferência -->
            <div class="question-container">
                <div class="question-title">NÚMERO DA TRANSFERÊNCIA</div>
                <input type="text" id="num-transferencia" disabled>
            </div>

            <div class="submit-buttons">
                <button type="reset" class="clear-button">Limpar formulário</button>
                <button type="submit" class="submit-button">Enviar</button>
            </div>
        </form>
    </div>

    <script>
        function atualizarEmail() {
            const emailInput = document.getElementById('email');
            const filialOrigem = document.getElementById('filial-origem').value;

            // Lógica de e-mail automático com base na filial (padrão para todos no momento)
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
        }

        // Função para carregar o número da transferência quando o formulário for carregado
        function carregarNumeroTransferencia() {
            fetch("/getTransferNumber")
                .then(response => response.json())
                .then(data => {
                    document.getElementById("num-transferencia").value = data.numTransferencia;
                });
        }

        // Chama a função ao carregar a página
        window.onload = carregarNumeroTransferencia;
    </script>
</body>
</html>
