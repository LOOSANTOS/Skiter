<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema de Análise de Projeto</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.7.1/jspdf.plugin.autotable.min.js"></script>
    <style>
        :root {
            --primary-color: #2E8B57;
            --primary-color-hover: #3a9a66;
            --primary-color-dark: #2d5e47;
            --primary-color-dark-hover: #26503d;
            --primary-color-rgb: 46, 139, 87;
            --primary-color-dark-rgb: 45, 94, 71;
            
            --background-light: #f8fafb;
            --card-light: #ffffff;
            --text-primary: #2d3748;
            --text-secondary: #718096;
            --border-light: #e2e8f0;
            --shadow-light: 0 6px 18px rgba(0, 0, 0, 0.06);
            
            --background-dark: #1a202c;
            --card-dark: #2d3748;
            --text-dark-primary: #e2e8f0;
            --text-dark-secondary: #a0aec0;
            --border-dark: #4a5568;
            --shadow-dark: 0 6px 18px rgba(0, 0, 0, 0.2);
            
            --glass-bg: rgba(255, 255, 255, 0.08);
            --glass-border: rgba(255, 255, 255, 0.1);
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Segoe UI', -apple-system, BlinkMacSystemFont, 'Helvetica Neue', sans-serif;
            background-color: var(--background-light);
            color: var(--text-primary);
            display: flex;
            flex-direction: column;
            min-height: 100vh;
            transition: background-color 0.3s ease-in-out, color 0.3s ease-in-out;
            line-height: 1.6;
        }

        /* Header moderno com efeito de vidro */
        header {
            background: linear-gradient(135deg, var(--primary-color) 0%, #1e6d4e 100%);
            color: white;
            padding: 25px 30px;
            text-align: center;
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            align-items: center;
            position: relative;
            box-shadow: var(--shadow-light);
            backdrop-filter: blur(10px);
            border-bottom: 1px solid var(--glass-border);
        }

        header h1 {
            width: 100%;
            margin: 0 0 15px 0;
            font-size: 2.3rem;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 700;
            letter-spacing: -0.5px;
            text-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }

        header h1 img {
            height: 42px;
            margin-right: 15px;
            vertical-align: middle;
            filter: drop-shadow(0 2px 4px rgba(0,0,0,0.15));
        }

        #userNameDisplay {
            width: 100%;
            font-size: 1.15rem;
            margin-bottom: 20px;
            color: rgba(255, 255, 255, 0.92);
            font-weight: 400;
            letter-spacing: 0.3px;
        }

        /* Grupo de botões com efeito de vidro */
        .button-group {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 16px;
            width: 100%;
            margin-bottom: 15px;
        }

        .button-group:last-child {
            margin-bottom: 0;
        }

        .button-group button {
            flex-grow: 1;
            max-width: 250px;
            padding: 14px 20px;
            font-size: 1.05rem;
            background: var(--glass-bg);
            border: 1px solid var(--glass-border);
            color: white;
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
            font-weight: 500;
            letter-spacing: 0.5px;
            backdrop-filter: blur(4px);
        }

        .button-group button i {
            margin-right: 10px;
            font-size: 1.2em;
        }

        .button-group button:hover {
            background: rgba(255, 255, 255, 0.2);
            transform: translateY(-3px);
            box-shadow: 0 6px 15px rgba(0, 0, 0, 0.15);
        }

        /* Botões de tema de cor */
        .color-theme-buttons {
            margin-top: 15px;
        }

        .color-theme-buttons button {
            width: 125px;
            height: 48px;
            padding: 0;
            font-size: 0.95rem;
            background: var(--glass-bg);
            border: 1px solid var(--glass-border);
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 3px 8px rgba(0, 0, 0, 0.1);
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
            backdrop-filter: blur(4px);
        }

        .color-theme-buttons button:hover {
            background: rgba(255, 255, 255, 0.2);
            transform: translateY(-3px);
            box-shadow: 0 5px 12px rgba(0, 0, 0, 0.15);
        }

        .color-theme-buttons button i {
            margin-right: 6px;
        }

        /* Botões de exportação */
        .export-buttons {
            position: absolute;
            top: 25px;
            right: 25px;
            display: flex;
            gap: 12px;
            width: auto;
            margin-bottom: 0;
            flex-wrap: nowrap;
        }

        .export-buttons button {
            width: 46px;
            height: 46px;
            padding: 0;
            font-size: 0.8rem;
            background: var(--glass-bg);
            border: 1px solid var(--glass-border);
            color: rgba(255, 255, 255, 0.95);
            border-radius: 10px;
            transition: all 0.2s ease-in-out;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 600;
            box-shadow: 0 3px 8px rgba(0, 0, 0, 0.1);
            cursor: pointer;
            backdrop-filter: blur(4px);
        }

        .export-buttons button:hover {
            background: rgba(255, 255, 255, 0.25);
            color: white;
            transform: scale(1.1);
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.15);
        }

        /* Relógio moderno */
        #dateTimeDisplay {
            position: absolute;
            top: 25px;
            left: 25px;
            color: rgba(255, 255, 255, 0.95);
            font-weight: 400;
            text-align: left;
            display: flex;
            flex-direction: column;
            gap: 4px;
            line-height: 1.3;
            z-index: 10;
        }

        #clock {
            display: flex;
            align-items: center;
            font-size: 1.25rem;
            font-weight: 600;
            letter-spacing: 1px;
            text-shadow: 0 2px 4px rgba(0,0,0,0.15);
        }

        #clock .separator {
            margin: 0 2px;
            animation: blink 1s infinite;
        }

        @keyframes blink {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.5; }
        }

        #currentDate {
            font-size: 0.95rem;
            opacity: 0.9;
            margin-top: 4px;
            letter-spacing: 0.5px;
        }

        /* Conteúdo principal */
        main {
            padding: 30px;
            flex-grow: 1;
            max-width: 1400px;
            margin: 0 auto;
            width: 100%;
            box-sizing: border-box;
        }

        .form-section, .table-section {
            margin-bottom: 30px;
            background-color: var(--card-light);
            padding: 30px;
            border-radius: 16px;
            box-shadow: var(--shadow-light);
            transition: all 0.3s ease-in-out;
            border: 1px solid var(--border-light);
        }

        .form-section h2, .table-section h2 {
            margin-top: 0;
            margin-bottom: 25px;
            color: var(--primary-color);
            font-weight: 600;
            font-size: 1.7rem;
            padding-bottom: 15px;
            border-bottom: 1px solid var(--border-light);
        }

        /* Inputs modernos */
        input, textarea {
            width: 100%;
            padding: 14px 16px;
            margin-top: 8px;
            margin-bottom: 20px;
            border: 1px solid var(--border-light);
            border-radius: 10px;
            box-sizing: border-box;
            transition: all 0.3s ease-in-out;
            font-size: 1rem;
            background-color: #f9fbfc;
            font-family: inherit;
        }

        input:focus, textarea:focus {
            border-color: var(--primary-color);
            outline: none;
            box-shadow: 0 0 0 3px rgba(var(--primary-color-rgb), 0.1);
            background-color: white;
        }

        /* Botões gerais */
        button {
            background: linear-gradient(135deg, var(--primary-color) 0%, var(--primary-color-hover) 100%);
            color: white;
            padding: 15px 25px;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
            width: 100%;
            margin-bottom: 15px;
            font-weight: 600;
            letter-spacing: 0.5px;
            font-size: 1.05rem;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }

        button:last-child {
            margin-bottom: 0;
        }

        button:hover {
            transform: translateY(-3px);
            box-shadow: 0 6px 15px rgba(0, 0, 0, 0.15);
        }

        /* Tabela moderna */
        table {
            width: 100%;
            border-collapse: separate;
            border-spacing: 0 14px;
            display: block;
            overflow-x: auto;
            white-space: nowrap;
        }

        table thead, table tbody tr {
            display: table;
            width: 100%;
            table-layout: fixed;
        }

        th, td {
            border: 1px solid var(--border-light);
            padding: 16px;
            text-align: left;
            white-space: normal;
            word-wrap: break-word;
            transition: all 0.25s ease-in-out;
        }

        th {
            background-color: #f5f8fa;
            font-weight: 600;
            color: var(--text-secondary);
            font-size: 0.95rem;
            letter-spacing: 0.3px;
        }

        tbody tr {
            background-color: white;
            box-shadow: 0 2px 8px rgba(0,0,0,0.03);
            border-radius: 12px;
            overflow: hidden;
            transition: all 0.3s ease;
        }

        tbody tr:hover {
            box-shadow: 0 6px 15px rgba(0,0,0,0.08);
            transform: translateY(-3px);
        }

        tbody tr td:first-child { 
            border-left: 1px solid var(--border-light); 
            border-top-left-radius: 12px; 
            border-bottom-left-radius: 12px; 
        }

        tbody tr td:last-child { 
            border-right: 1px solid var(--border-light); 
            border-top-right-radius: 12px; 
            border-bottom-right-radius: 12px; 
        }

        tbody tr td { 
            border-top: none; 
            border-bottom: none; 
            vertical-align: middle;
        }

        /* Botões de ícone */
        .icon-button {
            background: none;
            border: none;
            cursor: pointer;
            color: var(--primary-color);
            font-size: 1.4rem;
            transition: all 0.2s ease-in-out;
            margin: 0 8px;
            padding: 10px;
            width: auto;
            border-radius: 50%;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            background-color: rgba(var(--primary-color-rgb), 0.1);
        }

        .icon-button:hover {
            color: var(--primary-color-hover);
            transform: scale(1.15);
            background-color: rgba(var(--primary-color-rgb), 0.2);
        }

        /* Barra de progresso */
        .progress-bar {
            width: 100%;
            background-color: #edf2f7;
            border-radius: 10px;
            overflow: hidden;
            margin-top: 20px;
            height: 8px;
            position: relative;
        }

        .progress-bar div {
            height: 100%;
            background: linear-gradient(90deg, var(--primary-color), #3a9a66);
            width: 0;
            transition: width 0.5s ease-out, background-color 0.3s ease-in-out;
            border-radius: 10px;
            position: relative;
            overflow: hidden;
        }

        .progress-bar div::after {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 50%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.4), transparent);
            animation: shimmer 1.5s infinite;
        }

        @keyframes shimmer {
            100% {
                left: 150%;
            }
        }

        /* Radio buttons personalizados */
        .status-radio-group {
            display: flex;
            flex-direction: column;
            gap: 12px;
            align-items: flex-start;
        }

        .status-radio-group label {
            display: flex;
            align-items: center;
            margin: 0;
            cursor: pointer;
            position: relative;
            padding-left: 32px;
            color: inherit;
            font-size: 0.95rem;
        }

        .status-radio-group input[type="radio"] {
            position: absolute;
            opacity: 0;
            cursor: pointer;
            height: 0;
            width: 0;
        }

        .status-radio-group .radio-custom {
            position: absolute;
            left: 0;
            top: 1px;
            height: 22px;
            width: 22px;
            background-color: white;
            border: 2px solid #cbd5e0;
            border-radius: 50%;
            transition: all 0.2s ease-in-out;
        }

        .status-radio-group .radio-custom:after {
            content: "";
            position: absolute;
            display: none;
            left: 5px;
            top: 5px;
            width: 10px;
            height: 10px;
            border-radius: 50%;
            background: white;
        }

        .status-radio-group input[type="radio"]:checked + .radio-custom:after {
            display: block;
        }

        .status-radio-group input[type="radio"]:checked + .radio-custom {
            background-color: var(--primary-color);
            border-color: var(--primary-color);
        }

        .status-radio-group label:hover .radio-custom {
            border-color: var(--primary-color);
        }

        .status-radio-group input[type="radio"]:checked + .radio-custom:hover {
            background-color: var(--primary-color-hover);
            border-color: var(--primary-color-hover);
        }

        /* Tema escuro */
        body.dark-mode {
            background-color: var(--background-dark);
            color: var(--text-dark-primary);
        }

        body.dark-mode header {
            background: linear-gradient(135deg, var(--primary-color-dark) 0%, #1d4a37 100%);
            box-shadow: var(--shadow-dark);
        }

        body.dark-mode .form-section,
        body.dark-mode .table-section {
            background-color: var(--card-dark);
            color: var(--text-dark-primary);
            box-shadow: var(--shadow-dark);
            border: 1px solid var(--border-dark);
        }

        body.dark-mode .form-section h2, 
        body.dark-mode .table-section h2 {
            color: var(--primary-color-dark-hover);
            border-bottom: 1px solid var(--border-dark);
        }

        body.dark-mode input,
        body.dark-mode textarea {
            background-color: #374151;
            color: var(--text-dark-primary);
            border: 1px solid var(--border-dark);
        }

        body.dark-mode input:focus, body.dark-mode textarea:focus {
            border-color: var(--primary-color-dark-hover);
            box-shadow: 0 0 0 3px rgba(var(--primary-color-dark-rgb), 0.2);
        }

        body.dark-mode button {
            background: linear-gradient(135deg, var(--primary-color-dark) 0%, var(--primary-color-dark-hover) 100%);
            color: #eee;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
        }

        body.dark-mode .icon-button {
            color: #a8e6a8;
        }

        body.dark-mode .icon-button:hover {
            color: #8fd48f;
            background-color: rgba(var(--primary-color-dark-rgb), 0.2);
        }

        body.dark-mode #userNameDisplay {
            color: rgba(255, 255, 255, 0.9);
        }

        body.dark-mode th, body.dark-mode td {
            border-color: var(--border-dark);
        }

        body.dark-mode th {
            background-color: #2d3748;
            color: var(--text-dark-secondary);
        }

        body.dark-mode tbody tr {
            background-color: #2d3748;
            box-shadow: 0 2px 8px rgba(0,0,0,0.2);
        }

        body.dark-mode .progress-bar {
            background-color: #2d3748;
        }

        /* Modal moderno */
        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto;
            background-color: rgba(0,0,0,0.5);
            justify-content: center;
            align-items: center;
            backdrop-filter: blur(5px);
        }

        .modal-content {
            background-color: var(--card-light);
            margin: 20px;
            padding: 35px;
            border: none;
            width: 90%;
            max-width: 550px;
            border-radius: 16px;
            box-shadow: 0 15px 35px rgba(0,0,0,0.25);
            animation: fadeInScale 0.4s cubic-bezier(0.18, 0.89, 0.32, 1.28) forwards;
        }

        @keyframes fadeInScale {
            0% { opacity: 0; transform: scale(0.9) translateY(20px); }
            100% { opacity: 1; transform: scale(1) translateY(0); }
        }

        .modal-content h2 {
            margin-top: 0;
            color: var(--primary-color);
            font-size: 1.7rem;
            text-align: center;
            margin-bottom: 25px;
            padding-bottom: 15px;
            border-bottom: 1px solid var(--border-light);
        }

        .modal-content label, .modal-content input, .modal-content textarea {
            display: block;
            margin-bottom: 15px;
        }

        .modal-content button {
            margin-right: 15px;
            margin-top: 15px;
            width: auto;
            display: inline-block;
            min-width: 130px;
        }

        body.dark-mode .modal-content {
            background-color: var(--card-dark);
            color: var(--text-dark-primary);
            box-shadow: 0 15px 35px rgba(0,0,0,0.4);
        }

        body.dark-mode .modal-content h2 {
            color: var(--primary-color-dark);
            border-bottom: 1px solid var(--border-dark);
        }

        /* Rodapé moderno */
        footer {
            text-align: center;
            padding: 25px;
            margin-top: 40px;
            background-color: #edf2f7;
            color: #4a5568;
            font-size: 0.95rem;
            border-top: 1px solid var(--border-light);
            transition: all 0.3s ease-in-out;
            font-weight: 400;
        }

        body.dark-mode footer {
            background-color: #2d3748;
            color: var(--text-dark-secondary);
            border-top: 1px solid var(--border-dark);
        }

        /* Classes de tema de cor */
        body.green-theme {
            --primary-color: #2E8B57;
            --primary-color-hover: #3a9a66;
            --primary-color-dark: #2d5e47;
            --primary-color-dark-hover: #26503d;
            --primary-color-rgb: 46, 139, 87;
            --primary-color-dark-rgb: 45, 94, 71;
        }

        body.blue-theme {
            --primary-color: #4682B4;
            --primary-color-hover: #528fc0;
            --primary-color-dark: #3b6b90;
            --primary-color-dark-hover: #325a7a;
            --primary-color-rgb: 70, 130, 180;
            --primary-color-dark-rgb: 59, 107, 144;
        }

        body.orange-theme {
            --primary-color: #BDB76B;
            --primary-color-hover: #c9c378;
            --primary-color-dark: #8F8A50;
            --primary-color-dark-hover: #7A7642;
            --primary-color-rgb: 189, 183, 107;
            --primary-color-dark-rgb: 143, 138, 80;
        }

        body.grey-theme {
            --primary-color: #8F8F8F;
            --primary-color-hover: #9d9d9d;
            --primary-color-dark: #656565;
            --primary-color-dark-hover: #505050;
            --primary-color-rgb: 143, 143, 143;
            --primary-color-dark-rgb: 101, 101, 101;
        }

        /* Responsividade */
        @media (max-width: 768px) {
            main {
                padding: 20px;
            }
            
            .form-section, .table-section {
                padding: 20px;
            }
            
            header {
                padding: 20px 15px;
            }
            
            header h1 {
                font-size: 1.9rem;
            }
            
            header h1 img {
                height: 34px;
                margin-bottom: 10px;
            }
            
            #userNameDisplay {
                font-size: 1.05rem;
                margin-bottom: 15px;
            }
            
            .button-group {
                gap: 12px;
            }
            
            .button-group button {
                max-width: none;
                width: 100%;
                margin: 0 0 10px 0;
                font-size: 0.95rem;
                padding: 12px 15px;
            }
            
            .color-theme-buttons button {
                width: 110px;
                height: 44px;
                font-size: 0.85rem;
            }
            
            .export-buttons {
                position: static;
                width: 100%;
                justify-content: center;
                margin-top: 10px;
                margin-bottom: 15px;
                gap: 12px;
            }
            
            .export-buttons button {
                width: 90px;
                height: 40px;
                font-size: 0.85rem;
            }
            
            #dateTimeDisplay {
                position: static;
                text-align: center;
                width: 100%;
                margin-top: 10px;
                margin-bottom: 10px;
            }
            
            #clock {
                justify-content: center;
                font-size: 1.15rem;
            }
            
            #currentDate {
                font-size: 0.9rem;
            }
            
            tbody tr {
                margin-bottom: 15px;
                box-shadow: 0 2px 8px rgba(0,0,0,0.08);
            }
            
            td {
                padding-left: 50%;
                padding-top: 12px;
                padding-bottom: 12px;
            }
            
            td:before {
                left: 12px;
                width: 45%;
                font-weight: 600;
            }
            
            .icon-button {
                margin: 0 12px;
                font-size: 1.3rem;
                padding: 8px;
            }
            
            .status-radio-group {
                flex-direction: row;
                gap: 18px;
                justify-content: center;
                width: 100%;
            }
            
            .status-radio-group label {
                padding-left: 28px;
                font-size: 0.9rem;
            }
            
            .status-radio-group .radio-custom {
                height: 20px;
                width: 20px;
            }
        }

        @media (max-width: 480px) {
            header h1 {
                font-size: 1.6rem;
            }
            
            .button-group button {
                font-size: 0.9rem;
            }
            
            .color-theme-buttons button {
                width: 100px;
                font-size: 0.8rem;
            }
            
            .export-buttons {
                gap: 10px;
            }
            
            .export-buttons button {
                width: 80px;
                height: 38px;
                font-size: 0.8rem;
            }
            
            .modal-content {
                padding: 25px;
            }
            
            .modal-content button {
                width: 100%;
                margin-right: 0;
                margin-bottom: 12px;
            }
            
            .status-radio-group {
                flex-direction: column;
                gap: 10px;
            }
        }
    </style>
</head>
<body>

    <header>
        <div id="dateTimeDisplay">
            <div id="clock">
                <span id="hours">00</span>
                <span class="separator">:</span>
                <span id="minutes">00</span>
                <span class="separator">:</span>
                <span id="seconds">00</span>
            </div>
            <div id="currentDate">00/00/0000</div>
        </div>
        <h1>
            <img src="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath fill='%23ffffff' d='M19,3H14.82C14.4,1.84 13.3,1 12,1C10.7,1 9.6,1.84 9.18,3H5A2,2 0 0,0 3,5V19A2,2 0 0,0 5,21H19A2,2 0 0,0 21,19V5A2,2 0 0,0 19,3M12,3A1,1 0 0,1 13,4A1,1 0 0,1 12,5A1,1 0 0,1 11,4A1,1 0 0,1 12,3M7,7H17V5H19V19H5V5H7V7M7,9V11H9V9H7M11,9V11H13V9H11M15,9V11H17V9H15M7,13V15H9V13H7M11,13V15H13V13H11M15,13V15H17V13H15Z'/%3E%3C/svg%3E" alt="Símbolo de Arquitetura">
            Sistema de Análise de Projeto
        </h1>
        <span id="userNameDisplay"></span>

        <div class="button-group">
            <button onclick="generatePdfReport()"><i class="fas fa-file-pdf"></i> Gerar Relatório</button>
            <button onclick="toggleTheme()"><i class="fas fa-moon"></i> Alternar Tema</button>
        </div>

        <div class="button-group color-theme-buttons">
            <button onclick="changeColorTheme('green')"><i class="fas fa-leaf"></i> Verde</button>
            <button onclick="changeColorTheme('blue')"><i class="fas fa-tint"></i> Azul</button>
            <button onclick="changeColorTheme('orange')"><i class="fas fa-palette"></i> Pastel</button>
            <button onclick="changeColorTheme('grey')"><i class"fas fa-grip-lines"></i> Pastel II</button>
        </div>

        <div class="export-buttons">
            <button onclick="exportJSON()">JSON</button>
            <button onclick="exportCSV()">CSV</button>
        </div>
    </header>

    <main>
        <section class="form-section">
            <h2>Adicionar Novo Projeto</h2>
            <form id="projectForm">
                <label for="userName">Analista:</label>
                <input type="text" id="userName" name="userName" placeholder="Digite seu nome">

                <label for="nome">Número do Processo:</label>
                <input type="text" id="nome" name="nome" placeholder="Ex: 12345" maxlength="6">

                <label for="descricao">Descrição das exigências:</label>
                <textarea id="descricao" name="descricao" rows="4" placeholder="Detalhes das exigências do projeto"></textarea>

                <button type="button" onclick="addProject()">Salvar</button>
                <div class="progress-bar" id="progressBar"><div></div></div>
            </form>
        </section>

        <section class="table-section">
            <h2>Projetos Cadastrados</h2>
            <table id="projectTable">
                <thead>
                    <tr>
                        <th>Número do Processo</th>
                        <th>Descrição</th>
                        <th>Data</th>
                        <th>Status</th>
                        <th>Ações</th>
                    </tr>
                </thead>
                <tbody>
                </tbody>
            </table>
        </section>
    </main>

    <div id="editModal" class="modal">
        <div class="modal-content">
            <h2>Editar Projeto</h2>
            <label for="editNome">Número do Processo:</label>
            <input type="text" id="editNome" maxlength="6">
            <label for="editDescricao">Descrição das exigências:</label>
            <textarea id="editDescricao" rows="4"></textarea>
            <div style="display: flex; justify-content: center; margin-top: 20px;">
                <button onclick="saveEditedProject()">Salvar Edição</button>
                <button onclick="closeEditModal()" style="background: #f0f2f5; color: #333; margin-left: 15px;">Cancelar</button>
            </div>
        </div>
    </div>

    <footer>
        <p>Sistema de Análise de Projeto | <span id="systemVersionFooter"></span> | <span id="trademarkFooter"></span></p>
    </footer>

    <script>
        let currentEditRow = null;

        const SYSTEM_VERSION = "v1.0.0";
        const TRADEMARK = "Loosantos™  © 2025 Todos os direitos reservados.";

        function generateUniqueId() {
            return 'proj_' + Date.now().toString() + Math.random().toString(36).substr(2, 9);
        }

        function createProjectRow(project) {
            const table = document.getElementById('projectTable').getElementsByTagName('tbody')[0];
            const newRow = table.insertRow(0);
            newRow.setAttribute('data-id', project.id);

            const nameCell = newRow.insertCell(0);
            const descCell = newRow.insertCell(1);
            const dateCell = newRow.insertCell(2);
            const statusCell = newRow.insertCell(3);
            const actionsCell = newRow.insertCell(4);

            nameCell.textContent = project.name;
            nameCell.setAttribute('data-label', 'Nº Processo:');

            descCell.textContent = project.description;
            descCell.setAttribute('data-label', 'Descrição:');

            dateCell.textContent = project.date;
            dateCell.setAttribute('data-label', 'Data:');

            statusCell.setAttribute('data-label', 'Status:');
            const statusGroupDiv = document.createElement('div');
            statusGroupDiv.classList.add('status-radio-group');

            function createRadioButton(value, text, projectId, currentStatus) {
                const label = document.createElement('label');
                const radio = document.createElement('input');
                radio.type = 'radio';
                radio.name = `status-${projectId}`;
                radio.value = value;
                radio.checked = (currentStatus === value);
                radio.onchange = () => saveStatusChange(projectId, value);

                const customSpan = document.createElement('span');
                customSpan.classList.add('radio-custom');

                label.appendChild(radio);
                label.appendChild(customSpan);
                label.appendChild(document.createTextNode(text));
                return label;
            }

            statusGroupDiv.appendChild(createRadioButton('Aprovado', ' Aprovado', project.id, project.status));
            statusGroupDiv.appendChild(createRadioButton('Exigencia', ' Exigência', project.id, project.status));

            statusCell.appendChild(statusGroupDiv);

            actionsCell.innerHTML = `<button class="icon-button" onclick="editProject(this)"><i class="fas fa-edit"></i></button>
                                     <button class="icon-button" onclick="deleteProject(this)"><i class="fas fa-trash"></i></button>`;
        }

        function saveStatusChange(projectId, newStatus) {
            let projects = JSON.parse(localStorage.getItem('projects')) || [];
            const projectIndex = projects.findIndex(p => p.id === projectId);
            if (projectIndex !== -1) {
                projects[projectIndex].status = newStatus;
                localStorage.setItem('projects', JSON.stringify(projects));
            }
        }

        function addProject() {
            const userNameInput = document.getElementById('userName');
            const nomeInput = document.getElementById('nome');
            const descricaoInput = document.getElementById('descricao');

            const userName = userNameInput.value.trim();
            const nome = nomeInput.value.trim();
            const descricao = descricaoInput.value.trim();
            const date = new Date().toLocaleDateString('pt-BR');

            if (!userName || !nome || !descricao) {
                alert('Por favor, preencha todos os campos: Seu Nome, Número do Processo e Descrição das exigências.');
                return;
            }

            if (!/^\d{1,6}$/.test(nome)) {
                alert('O "Número do Processo" deve conter apenas números e ter no máximo 6 dígitos.');
                return;
            }

            const project = {
                id: generateUniqueId(),
                name: nome,
                description: descricao,
                date: date,
                status: ''
            };

            createProjectRow(project);
            saveProjectsToLocalStorage();
           
            nomeInput.value = '';
            descricaoInput.value = '';
           
            document.getElementById('userNameDisplay').textContent = `Bem-vindo(a), ${userName}!`;
            updateProgressBar(100);
            setTimeout(() => updateProgressBar(0), 1000);
        }

        function editProject(button) {
            currentEditRow = button.parentElement.parentElement;
            const nameCell = currentEditRow.cells[0];
            const descCell = currentEditRow.cells[1];

            document.getElementById('editNome').value = nameCell.textContent;
            document.getElementById('editDescricao').value = descCell.textContent;
            document.getElementById('editModal').style.display = 'flex';
            // Foco no campo de edição do nome
            document.getElementById('editNome').focus();
        }

        function saveEditedProject() {
            if (currentEditRow) {
                const newName = document.getElementById('editNome').value.trim();
                const newDesc = document.getElementById('editDescricao').value.trim();
                const projectId = currentEditRow.getAttribute('data-id');

                if (!newName || !newDesc) {
                    alert('Por favor, preencha todos os campos para editar.');
                    return;
                }
                if (!/^\d{1,6}$/.test(newName)) {
                    alert('O "Número do Processo" deve conter apenas números e ter no máximo 6 dígitos.');
                    return;
                }

                currentEditRow.cells[0].textContent = newName;
                currentEditRow.cells[1].textContent = newDesc;

                let projects = JSON.parse(localStorage.getItem('projects')) || [];
                const projectIndex = projects.findIndex(p => p.id === projectId);
                if (projectIndex !== -1) {
                    projects[projectIndex].name = newName;
                    projects[projectIndex].description = newDesc;
                    localStorage.setItem('projects', JSON.stringify(projects));
                }
                closeEditModal();
            }
        }

        function closeEditModal() {
            document.getElementById('editModal').style.display = 'none';
            currentEditRow = null;
        }

        function deleteProject(button) {
            if (confirm('Tem certeza que deseja excluir este projeto?')) {
                const row = button.parentElement.parentElement;
                const projectId = row.getAttribute('data-id');
                row.remove();

                let projects = JSON.parse(localStorage.getItem('projects')) || [];
                projects = projects.filter(p => p.id !== projectId);
                localStorage.setItem('projects', JSON.stringify(projects));
            }
        }

        function updateProgressBar(value) {
            const progressBar = document.getElementById('progressBar').firstElementChild;
            progressBar.style.width = value + '%';
        }

        function saveProjectsToLocalStorage() {
            const table = document.getElementById('projectTable').getElementsByTagName('tbody')[0];
            const rows = table.rows;
            const projects = [];

            for (let i = 0; i < rows.length; i++) {
                const id = rows[i].getAttribute('data-id');
                const name = rows[i].cells[0].textContent;
                const description = rows[i].cells[1].textContent;
                const date = rows[i].cells[2].textContent;

                const statusRadios = rows[i].querySelectorAll(`input[name="status-${id}"]:checked`);
                let status = statusRadios.length > 0 ? statusRadios[0].value : '';

                projects.push({ id, name, description, date, status });
            }

            localStorage.setItem('projects', JSON.stringify(projects.reverse()));

            const userName = document.getElementById('userName').value.trim();
            if (userName) {
                localStorage.setItem('currentUserName', userName);
            }
        }

        function loadProjectsFromLocalStorage() {
            const tableBody = document.getElementById('projectTable').getElementsByTagName('tbody')[0];
            tableBody.innerHTML = '';

            const projects = JSON.parse(localStorage.getItem('projects')) || [];
            projects.forEach(project => {
                createProjectRow(project);
            });

            const savedUserName = localStorage.getItem('currentUserName');
            if (savedUserName) {
                document.getElementById('userName').value = savedUserName;
                document.getElementById('userNameDisplay').textContent = `Bem-vindo(a), ${savedUserName}!`;
            } else {
                document.getElementById('userNameDisplay').textContent = `Bem-vindo(a)!`;
            }
        }

        function toggleTheme() {
            document.body.classList.toggle('dark-mode');
            localStorage.setItem('theme', document.body.classList.contains('dark-mode') ? 'dark' : 'light');
        }

        function changeColorTheme(color) {
            const body = document.body;
            body.classList.remove('green-theme', 'blue-theme', 'orange-theme', 'grey-theme');
            body.classList.add(`${color}-theme`);
            localStorage.setItem('colorTheme', color);

            let primaryColorRgb, primaryColorDarkRgb;
            switch(color) {
                case 'green':
                    primaryColorRgb = '46, 139, 87';
                    primaryColorDarkRgb = '45, 94, 71';
                    break;
                case 'blue':
                    primaryColorRgb = '70, 130, 180';
                    primaryColorDarkRgb = '59, 107, 144';
                    break;
                case 'orange':
                    primaryColorRgb = '189, 183, 107';
                    primaryColorDarkRgb = '143, 138, 80';
                    break;
                case 'grey':
                    primaryColorRgb = '143, 143, 143';
                    primaryColorDarkRgb = '101, 101, 101';
                    break;
                default:
                    primaryColorRgb = '46, 139, 87';
                    primaryColorDarkRgb = '45, 94, 71';
            }
            document.documentElement.style.setProperty('--primary-color-rgb', primaryColorRgb);
            document.documentElement.style.setProperty('--primary-color-dark-rgb', primaryColorDarkRgb);
        }

        function exportJSON() {
            const projects = JSON.parse(localStorage.getItem('projects')) || [];
            const dataStr = JSON.stringify(projects, null, 2);
            const blob = new Blob([dataStr], { type: 'application/json' });
            const url = URL.createObjectURL(blob);

            const link = document.createElement('a');
            link.href = url;
            link.download = 'projetos.json';
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            URL.revokeObjectURL(url);
            alert('Dados exportados para projetos.json!');
        }

        function exportCSV() {
            const projects = JSON.parse(localStorage.getItem('projects')) || [];
            let csv = 'ID;Numero do Processo;Descricao;Data;Status\n';

            projects.forEach(p => {
                csv += `"${p.id}";"${p.name}";"${p.description.replace(/"/g, '""')}";"${p.date}";"${p.status}"\n`;
            });

            const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
            const url = URL.createObjectURL(blob);

            const link = document.createElement('a');
            link.href = url;
            link.download = 'projetos.csv';
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            URL.revokeObjectURL(url);
            alert('Dados exportados para projetos.csv!');
        }

        async function generatePdfReport() {
            const { jsPDF } = window.jspdf;
            const doc = new jsPDF();

            const projects = JSON.parse(localStorage.getItem('projects')) || [];

            const totalProjects = projects.length;
            const userName = localStorage.getItem('currentUserName') || "Usuário";

            let approvedCount = 0;
            let exigencyCount = 0;
            projects.forEach(p => {
                if (p.status === 'Aprovado') {
                    approvedCount++;
                } else if (p.status === 'Exigencia') {
                    exigencyCount++;
                }
            });

            // Tenta obter a cor primária do tema
            let primaryColorRgb = [46, 139, 87]; // padrão verde
            try {
                const primaryColor = getComputedStyle(document.body).getPropertyValue('--primary-color');
                if (primaryColor) {
                    const rgbArray = primaryColor.match(/\d+/g);
                    if (rgbArray && rgbArray.length >= 3) {
                        primaryColorRgb = rgbArray.slice(0, 3).map(Number);
                    }
                }
            } catch (e) {
                console.error("Erro ao obter a cor primária", e);
            }

            doc.setFont('helvetica');
            doc.setTextColor(0, 0, 0); // texto preto

            doc.setFontSize(22);
            doc.text("Relatório de Análise de Projetos", 105, 20, { align: "center" });

            doc.setFontSize(10);
            doc.text(`Usuário: ${userName}`, 14, 30);
            doc.text(`Data do Relatório: ${new Date().toLocaleDateString('pt-BR')}`, 14, 35);
           
            doc.setFontSize(14);
            doc.text("Resumo:", 14, 50);
            doc.setFontSize(12);
            doc.text(`Total de Projetos Analisados: ${totalProjects}`, 14, 60);
            doc.text(`Projetos Aprovados: ${approvedCount}`, 14, 67);
            doc.text(`Projetos com Exigência: ${exigencyCount}`, 14, 74);

            const tableColumn = ["Nº Processo", "Descrição", "Data", "Status"];
            const tableRows = projects.map(p => [p.name, p.description, p.date, p.status || 'Pendente']);

            doc.autoTable({
                startY: 85,
                head: [tableColumn],
                body: tableRows,
                theme: 'striped',
                headStyles: {
                    fillColor: primaryColorRgb,
                    textColor: [255, 255, 255],
                    fontStyle: 'bold'
                },
                styles: {
                    fontSize: 10,
                    cellPadding: 3,
                    valign: 'middle',
                    overflow: 'linebreak'
                },
                columnStyles: {
                    0: { cellWidth: 30 },
                    1: { cellWidth: 'auto' },
                    2: { cellWidth: 25 },
                    3: { cellWidth: 25 }
                },
                didDrawPage: function (data) {
                    const pageHeight = doc.internal.pageSize.height;
                    const pageWidth = doc.internal.pageSize.width;
                    const margin = data.settings.margin.left;
                   
                    doc.setFontSize(9);
                    doc.setTextColor(100);

                    const centerText = `${TRADEMARK} | Versão do Sistema: ${SYSTEM_VERSION}`;
                    doc.text(centerText, pageWidth / 2, pageHeight - 10, { align: "center" });

                    doc.text(`Página ${data.pageNumber} de ${doc.internal.getNumberOfPages()}`, pageWidth - margin, pageHeight - 10, { align: "right" });
                }
            });

            doc.save('relatorio_projetos.pdf');
            alert('Relatório PDF gerado com sucesso!');
        }

        // Função para atualizar o relógio e a data
        function updateDateTime() {
            const now = new Date();
            const hours = String(now.getHours()).padStart(2, '0');
            const minutes = String(now.getMinutes()).padStart(2, '0');
            const seconds = String(now.getSeconds()).padStart(2, '0');
            
            const day = String(now.getDate()).padStart(2, '0');
            const month = String(now.getMonth() + 1).padStart(2, '0');
            const year = now.getFullYear();
            
            document.getElementById('hours').textContent = hours;
            document.getElementById('minutes').textContent = minutes;
            document.getElementById('seconds').textContent = seconds;
            document.getElementById('currentDate').textContent = `${day}/${month}/${year}`;
        }

        document.addEventListener('DOMContentLoaded', () => {
            loadProjectsFromLocalStorage();
            const savedTheme = localStorage.getItem('theme');
            if (savedTheme === 'dark') {
                document.body.classList.add('dark-mode');
            }

            const savedColorTheme = localStorage.getItem('colorTheme');
            if (savedColorTheme) {
                changeColorTheme(savedColorTheme);
            } else {
                changeColorTheme('green');
            }

            document.getElementById('systemVersionFooter').textContent = SYSTEM_VERSION;
            document.getElementById('trademarkFooter').textContent = TRADEMARK;

            updateDateTime();
            setInterval(updateDateTime, 1000);
        });
    </script>
</body>
</html>
