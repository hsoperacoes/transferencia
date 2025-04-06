<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TRANSFERÊNCIA ENTRE LOJAS</title>
    <style>
        body {
            font-family: 'Google Sans', Roboto, Arial, sans-serif;
            color: #202124;
            max-width: 640px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f8f9fa;
        }
        .form-header {
            background-color: white;
            border: 1px solid #dadce0;
            border-radius: 8px;
            padding: 24px;
            margin-bottom: 12px;
        }
        .form-title {
            font-size: 24px;
            font-weight: 500;
            margin-bottom: 20px;
            color: #202124;
        }
        .account-info {
            font-size: 14px;
            color: #5f6368;
            margin-bottom: 20px;
            padding-bottom: 20px;
            border-bottom: 1px solid #dadce0;
        }
        .required-note {
            font-size: 14px;
            color: #5f6368;
            margin-bottom: 20px;
        }
        .required-star {
            color: #d93025;
        }
        .question-container {
            background-color: white;
            border: 1px solid #dadce0;
            border-radius: 8px;
            padding: 24px;
            margin-bottom: 12px;
        }
        .question-title {
            font-size: 16px;
            font-weight: 500;
            margin-bottom: 8px;
        }
        .email-option {
            font-size: 14px;
            color: #5f6368;
            margin-top: 10px;
        }
        input[type="email"], textarea, select {
            width: 100%;
            padding: 10px;
            border: 1px solid #dadce0;
            border-radius: 4px;
            font-size: 14px;
            margin-top: 4px;
            box-sizing: border-box;
        }
        select {
            height: 40px;
        }
        textarea {
            min-height: 100px;
            resize: vertical;
        }
        .submit-buttons {
            display: flex;
            justify-content: flex-end;
            margin-top: 24px;
            gap: 8px;
        }
        .submit-button {
            background-color: #1a73e8;
            color: white;
            border: none;
            padding: 10px 24px;
            border-radius: 4px;
            font-size: 14px;
            font-weight: 500;
            cursor: pointer;
        }
        .submit-button:hover {
            background-color: #1765cc;
            box-shadow: 0 1px 2px 0 rgba(26,115,233,0.45);
        }
        .clear-button {
            background-color: transparent;
            color: #1a73e8;
            border: 1px solid #dadce0;
            padding: 10px 24px;
            border-radius: 4px;
            font-size: 14px;
            font-weight: 500;
            cursor: pointer;
        }
        .clear-button:hover {
            background-color: #f8f9fa;
        }
    </style>
</head>
<body>
    <div class="form-container">
        <div class="form-header">
            <h1 class="form-title">TRANSFERÊNCIA ENTRE LOJAS</h1>
            <div class="account-info">
                Na aparcacosa.lujaj@gmail.com
                <a href="#" style="color: #1a73e8; text-decoration: none; margin-left: 10px;">Mudar de conta</a>
            </div>
            <div class="required-note">
                <span class="required-star">*</span> Indica uma pergunta obrigatória.
            </div>
        </div>

        <form action="#" method="POST">
            <!-- Campo de e-mail -->
            <div class="question-container">
                <div class="question-title">Enviar por email <span class="required-star">*</span></div>
                <input type="email" name="email" required>
                <div class="email-option">
                    <input type="checkbox" id="register-email" name="register-email" checked>
                    <label for="register-email">Registrar na aparcacosa.lujaj@gmail.com como o e-mail a ser incluído na minha resposta</label>
                </div>
            </div>

            <!-- Filial Origem -->
            <div class="question-container">
                <div class="question-title">FILIAL ORIGEM <span class="required-star">*</span></div>
                <select name="filial-origem" required>
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

            <div class="submit-buttons">
                <button type="reset" class="clear-button">Limpar formulário</button>
                <button type="submit" class="submit-button">Enviar</button>
            </div>
        </form>
    </div>
</body>
</html>
