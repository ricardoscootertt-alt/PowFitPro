<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro - Plataforma Profissional de Treinos</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            /* Tema Padrão Masculino (Escuro Fitness) */
            --bg-body: #0f172a;
            --bg-card: #1e293b;
            --text-main: #f8fafc;
            --text-muted: #94a3b8;
            --border-color: #334155;
            --primary: #3b82f6;
            --primary-hover: #2563eb;
            --input-bg: #0f172a;
            --input-border: #475569;
        }

        [data-theme="Feminino"] {
            /* Tema Feminino (Rosa Moderno Elegante) */
            --bg-body: #fdf2f8;
            --bg-card: #ffffff;
            --text-main: #1e293b;
            --text-muted: #64748b;
            --border-color: #fbcfe8;
            --primary: #ec4899;
            --primary-hover: #db2777;
            --input-bg: #f8fafc;
            --input-border: #f1f5f9;
        }

        body { font-family: 'Inter', sans-serif; background-color: var(--bg-body); color: var(--text-main); transition: background-color 0.3s ease, color 0.3s ease; }
        .card { background-color: var(--bg-card); border: 1px solid var(--border-color); transition: background-color 0.3s ease, border-color 0.3s ease; }
        .input-field { background-color: var(--input-bg); border: 1px solid var(--input-border); color: var(--text-main); }
        .input-field:focus { outline: none; border-color: var(--primary); box-shadow: 0 0 0 1px var(--primary); }
        .btn-primary { background-color: var(--primary); color: white; transition: background-color 0.2s; }
        .btn-primary:hover { background-color: var(--primary-hover); }

        /* Oculta área de impressão na tela */
        #print-area, #print-report-area { display: none; }

        /* Estilos de Impressão (Estilo Planilha) */
        @media print {
            body { background: white !important; color: black !important; margin: 0; padding: 0; }
            #login-screen, #app-container, #history-modal, #exercise-modal, #report-modal, .no-print { display: none !important; }
            
            #print-area, #print-report-area {
                display: block !important;
                padding: 10mm 15mm;
                font-family: 'Arial', sans-serif;
                font-size: 10px;
            }
            @page { size: A4; margin: 0; }
            
            /* Cabeçalho Profissional */
            .print-header { border-bottom: 2px solid #000; padding-bottom: 8px; margin-bottom: 12px; }
            .print-title { font-size: 18px; font-weight: bold; margin: 0; text-transform: uppercase; text-align: center;}
            .prof-info { font-size: 12px; margin-top: 5px; text-align: center; font-weight: bold;}
            
            /* Grid Aluno */
            .print-grid { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 4px; margin-bottom: 15px; border: 1px solid #000; padding: 8px; background: #fff; }
            .print-grid div { font-size: 11px; }
            
            /* Diretrizes Automáticas */
            .print-guidelines { border: 1px solid #000; padding: 8px; margin-bottom: 15px; font-size: 9.5px; }
            .print-guidelines h4 { margin: 0 0 4px 0; font-size: 11px; text-transform: uppercase; background: #eee; padding: 3px;}
            .print-guidelines ul { margin: 0; padding-left: 15px; }
            .print-guidelines li { margin-bottom: 3px; }

            /* Tabelas Estilo Planilha */
            .print-workout { margin-bottom: 15px; page-break-inside: avoid; }
            .print-workout h3 { background: #e0e0e0; border: 1px solid #000; border-bottom: none; padding: 4px 8px; margin: 0; font-size: 12px; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            table { width: 100%; border-collapse: collapse; margin-bottom: 0; border: 1px solid #000; }
            th, td { border: 1px solid #000; padding: 4px 6px; text-align: left; font-size: 10px; }
            th { background-color: #f0f0f0; font-weight: bold; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            
            /* Recomendações e Textos Legais */
            .print-footer-section { margin-top: 15px; border: 1px solid #000; padding: 8px; font-size: 9px; page-break-inside: avoid;}
            .legal-text { font-size: 8px; color: #333; text-align: justify; margin-top: 5px; font-style: italic;}

            /* Relatório Layout */
            .report-title { font-size: 20px; text-align: center; font-weight: bold; margin-bottom: 20px; border-bottom: 2px solid #000; padding-bottom: 10px;}
            .report-summary { font-size: 14px; margin-bottom: 20px; }
            .report-table th { background: #333; color: #fff; }
        }

        /* Scrollbar */
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
        
        #loading-overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.8); z-index: 9999; display: flex;
            align-items: center; justify-content: center; color: white; flex-direction: column;
        }
    </style>
</head>
<body data-theme="Masculino" class="min-h-screen pb-20">

    <!-- OVERLAY DE CARREGAMENTO -->
    <div id="loading-overlay">
        <i class="fas fa-circle-notch fa-spin text-4xl mb-4 text-primary"></i>
        <h2 class="text-xl font-bold">Conectando PowFit Pro...</h2>
    </div>

    <!-- TELA DE LOGIN -->
    <div id="login-screen" class="hidden min-h-screen flex items-center justify-center p-4">
        <div class="card p-8 rounded-2xl shadow-2xl max-w-md w-full text-center">
            <i class="fas fa-dumbbell text-6xl text-primary mb-4"></i>
            <h1 class="text-3xl font-bold mb-2">PowFit Pro</h1>
            <p class="text-sm opacity-70 mb-8">Plataforma Profissional em Nuvem</p>
            
            <button onclick="window.loginGoogle()" class="w-full bg-white text-gray-800 font-bold py-3 px-4 rounded-lg shadow border border-gray-300 hover:bg-gray-100 transition flex items-center justify-center gap-3">
                <img src="https://upload.wikimedia.org/wikipedia/commons/5/53/Google_%22G%22_Logo.svg" alt="Google" class="w-5 h-5">
                Entrar com Conta Google
            </button>
            <p class="text-xs opacity-50 mt-6">Ao acessar, seu histórico ficará salvo na nuvem da sua rede de trabalho.</p>
        </div>
    </div>

    <!-- ÁREA PRINCIPAL DO SISTEMA -->
    <div id="app-container" class="hidden max-w-7xl mx-auto p-4 sm:p-6 lg:p-8">
        
        <!-- Header Principal -->
        <div class="flex flex-col md:flex-row justify-between items-start md:items-center mb-8 gap-4 bg-black bg-opacity-20 p-4 rounded-xl border border-gray-500 border-opacity-20">
            <div class="flex items-center gap-3">
                <img src="https://ui-avatars.com/api/?name=User&background=3b82f6&color=fff" id="user-avatar" class="w-12 h-12 rounded-full border-2 border-primary">
                <div>
                    <h1 class="text-xl font-bold tracking-tight" id="user-display-name">Carregando...</h1>
                    <p class="text-xs text-primary font-medium" id="user-network-display">PowFit Pro Network</p>
                </div>
            </div>
            <div class="flex flex-wrap gap-2">
                <button onclick="window.openReport()" class="bg-indigo-600 hover:bg-indigo-700 text-white px-4 py-2 rounded-lg font-medium shadow flex items-center gap-2 transition text-sm">
                    <i class="fas fa-chart-bar"></i> Relatório da Unidade
                </button>
                <button onclick="window.openHistory()" class="bg-gray-700 hover:bg-gray-600 text-white px-4 py-2 rounded-lg font-medium shadow flex items-center gap-2 transition text-sm">
                    <i class="fas fa-cloud"></i> Histórico em Nuvem
                </button>
                <button onclick="window.logoutGoogle()" class="bg-red-500 hover:bg-red-600 text-white px-4 py-2 rounded-lg font-medium shadow flex items-center gap-2 transition text-sm">
                    <i class="fas fa-sign-out-alt"></i> Sair
                </button>
            </div>
        </div>

        <div class="grid grid-cols-1 xl:grid-cols-12 gap-6">
            
            <!-- COLUNA ESQUERDA: Configurações -->
            <div class="xl:col-span-4 space-y-6">
                
                <!-- Card: Profissional -->
                <div class="card rounded-xl p-5 shadow-sm">
                    <div class="flex justify-between items-center border-b border-opacity-20 pb-2 mb-4" style="border-color: var(--border-color)">
                        <h2 class="text-lg font-semibold flex items-center gap-2">
                            <i class="fas fa-id-badge text-primary"></i> Meu Perfil Profissional
                        </h2>
                        <button onclick="window.saveProfile()" class="text-xs bg-primary text-white px-2 py-1 rounded hover:bg-primary-hover">Salvar Perfil</button>
                    </div>
                    
                    <div class="space-y-4">
                        <div class="grid grid-cols-2 gap-3">
                            <div>
                                <label class="block text-xs font-medium mb-1">Rede</label>
                                <input type="text" id="prof-network" class="input-field w-full rounded-lg px-2 py-1.5 text-sm" placeholder="Ex: PowFit">
                            </div>
                            <div>
                                <label class="block text-xs font-medium mb-1">Unidade</label>
                                <input type="text" id="prof-unit" class="input-field w-full rounded-lg px-2 py-1.5 text-sm" placeholder="Ex: Vila Flor">
                            </div>
                        </div>
                        <div class="grid grid-cols-2 gap-3">
                            <div class="col-span-2">
                                <label class="block text-xs font-medium mb-1">Equipe</label>
                                <input type="text" id="prof-team" class="input-field w-full rounded-lg px-2 py-1.5 text-sm" placeholder="Nome da Equipe">
                            </div>
                        </div>
                        <div>
                            <label class="block text-xs font-medium mb-1">Categoria de Atuação</label>
                            <select id="prof-type" onchange="window.toggleProfType()" class="input-field w-full rounded-lg px-2 py-1.5 text-sm font-medium text-primary">
                                <option value="PEF">Profissional de Educação Física</option>
                                <option value="TE">Treinador Esportivo</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-xs font-medium mb-1">Nome do Membro na Ficha</label>
                            <input type="text" id="prof-name" class="input-field w-full rounded-lg px-2 py-1.5 text-sm" placeholder="Seu nome">
                        </div>
                        <div class="grid grid-cols-3 gap-3">
                            <div>
                                <label class="block text-xs font-medium mb-1">CPF</label>
                                <input type="text" id="prof-cpf" class="input-field w-full rounded-lg px-2 py-1.5 text-xs" placeholder="000.000.000-00">
                            </div>
                            <div class="cref-field">
                                <label class="block text-xs font-medium mb-1">CREF</label>
                                <input type="text" id="prof-cref" class="input-field w-full rounded-lg px-2 py-1.5 text-xs" placeholder="000000">
                            </div>
                            <div class="cref-field">
                                <label class="block text-xs font-medium mb-1">UF (Estado)</label>
                                <input type="text" id="prof-state" class="input-field w-full rounded-lg px-2 py-1.5 text-xs" placeholder="RN">
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Card: Dados do Aluno -->
                <div class="card rounded-xl p-5 shadow-sm">
                    <div class="flex justify-between items-center mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <h2 class="text-lg font-semibold flex items-center gap-2">
                            <i class="fas fa-user text-primary"></i> Dados do Aluno
                        </h2>
                        <div id="imc-display"></div>
                    </div>
                    
                    <div class="space-y-4">
                        <div>
                            <label class="block text-sm font-medium mb-1">Nome do Aluno</label>
                            <input type="text" id="stu-name" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Nome do aluno">
                        </div>
                        <div class="grid grid-cols-3 gap-3">
                            <div>
                                <label class="block text-sm font-medium mb-1">Idade</label>
                                <input type="number" id="stu-age" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Anos">
                            </div>
                            <div>
                                <label class="block text-sm font-medium mb-1">Peso (kg)</label>
                                <input type="number" id="stu-weight" oninput="window.calculateIMC()" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="kg">
                            </div>
                            <div>
                                <label class="block text-sm font-medium mb-1">Altura (m)</label>
                                <input type="number" id="stu-height" step="0.01" oninput="window.calculateIMC()" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="m">
                            </div>
                        </div>
                        <div class="grid grid-cols-2 gap-3">
                            <div>
                                <label class="block text-sm font-medium mb-1">Gênero</label>
                                <select id="stu-gender" onchange="window.changeTheme()" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                    <option value="Masculino">Masculino</option>
                                    <option value="Feminino">Feminino</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-sm font-medium mb-1">Nível</label>
                                <select id="stu-level" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                    <option>Iniciante</option>
                                    <option>Intermediário</option>
                                    <option>Avançado</option>
                                </select>
                            </div>
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-1">Objetivo</label>
                            <select id="stu-objective" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                <option value="Emagrecimento">Emagrecimento</option>
                                <option value="Hipertrofia">Hipertrofia</option>
                                <option value="Definição">Definição</option>
                                <option value="Condicionamento">Condicionamento</option>
                                <option value="Resistência">Resistência</option>
                                <option value="Força">Força</option>
                                <option value="Reabilitação">Reabilitação</option>
                                <option value="Saúde geral">Saúde geral</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-1">Frequência</label>
                            <select id="stu-freq" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                <option>3 dias</option>
                                <option>5 dias</option>
                                <option>6 dias</option>
                                <option>Personalizado</option>
                            </select>
                        </div>
                    </div>
                </div>

                <!-- Card: Estado de Saúde -->
                <div class="card rounded-xl p-5 shadow-sm">
                    <h2 class="text-lg font-semibold mb-2 flex items-center gap-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <i class="fas fa-notes-medical text-primary"></i> Estado de Saúde
                    </h2>
                    <p class="text-[11px] opacity-70 mb-3 leading-tight">Diretrizes geradas com base no seu tipo profissional.</p>
                    <div class="grid grid-cols-2 gap-2 text-xs" id="health-container"></div>
                </div>

            </div>

            <!-- COLUNA DIREITA: Montagem dos Treinos -->
            <div class="xl:col-span-8 space-y-6">
                
                <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center bg-opacity-10 p-4 rounded-xl card border-dashed border-2 gap-4">
                    <div>
                        <h2 class="text-xl font-bold flex items-center gap-2">
                            <i class="fas fa-clipboard-list text-primary"></i> Ficha de Treino
                        </h2>
                        <div class="flex gap-2 mt-2">
                            <select id="stu-validity" class="input-field rounded px-2 py-1 text-xs">
                                <option>Validade: 15 dias</option>
                                <option selected>Validade: 30 dias</option>
                                <option>Validade: 60 dias</option>
                                <option>Validade: 90 dias</option>
                            </select>
                        </div>
                    </div>
                    <div class="flex gap-2 w-full sm:w-auto">
                        <button onclick="window.addWorkout()" class="btn-primary px-4 py-2 rounded-lg text-sm font-medium flex items-center gap-2 flex-1 justify-center">
                            <i class="fas fa-plus"></i> Dia de Treino
                        </button>
                        <button onclick="window.generateAndSavePrint()" class="bg-green-600 hover:bg-green-700 text-white px-6 py-2 rounded-lg text-sm font-bold flex items-center gap-2 flex-1 justify-center shadow-lg">
                            <i class="fas fa-save"></i> Imprimir e Salvar
                        </button>
                    </div>
                </div>

                <!-- Container de Treinos -->
                <div id="workouts-container" class="space-y-6"></div>

                <!-- Recomendações Livres -->
                <div class="card rounded-xl p-5 shadow-sm">
                    <h2 class="text-md font-semibold mb-2"><i class="fas fa-pen text-primary"></i> Recomendações Livres (Opcional)</h2>
                    <textarea id="stu-recs" rows="3" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Hidratação, descanso, cuidados específicos..."></textarea>
                </div>

            </div>
        </div>
    </div>

    <!-- MODAL DE EXERCÍCIOS -->
    <div id="exercise-modal" class="fixed inset-0 bg-black bg-opacity-70 backdrop-blur-sm hidden z-[60] flex items-center justify-center p-2 sm:p-4">
        <div class="card w-full max-w-5xl rounded-xl shadow-2xl flex flex-col h-[90vh]">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold"><i class="fas fa-search text-primary mr-2"></i> Adicionar Exercício</h3>
                <button onclick="window.closeModal()" class="text-gray-400 hover:text-red-500 transition-colors text-2xl leading-none">&times;</button>
            </div>
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <div class="w-full md:w-1/4 border-r overflow-y-auto p-2 space-y-1" style="border-color: var(--border-color)" id="modal-categories"></div>
                <div class="w-full md:w-3/4 overflow-y-auto p-3 bg-black bg-opacity-5" id="modal-exercises"></div>
            </div>
        </div>
    </div>

    <!-- MODAL DE HISTÓRICO (NUVEM) -->
    <div id="history-modal" class="fixed inset-0 bg-black bg-opacity-70 backdrop-blur-sm hidden z-[70] flex items-center justify-center p-2 sm:p-4">
        <div class="card w-full max-w-3xl rounded-xl shadow-2xl flex flex-col max-h-[90vh]">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold"><i class="fas fa-cloud text-primary mr-2"></i> Histórico em Nuvem</h3>
                <button onclick="window.closeHistory()" class="text-gray-400 hover:text-red-500 transition-colors text-2xl leading-none">&times;</button>
            </div>
            <div class="p-4 overflow-y-auto flex-1 space-y-3" id="history-list"></div>
        </div>
    </div>

    <!-- MODAL RELATÓRIO MENSAL -->
    <div id="report-modal" class="fixed inset-0 bg-black bg-opacity-70 backdrop-blur-sm hidden z-[70] flex items-center justify-center p-2 sm:p-4">
        <div class="card w-full max-w-2xl rounded-xl shadow-2xl flex flex-col max-h-[90vh]">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold"><i class="fas fa-chart-line text-primary mr-2"></i> Relatório de Unidade</h3>
                <button onclick="window.closeReport()" class="text-gray-400 hover:text-red-500 transition-colors text-2xl leading-none">&times;</button>
            </div>
            <div class="p-6 overflow-y-auto flex-1">
                <p class="text-sm mb-4">Gere a contagem de fichas prescritas pela sua Unidade na Rede.</p>
                <div class="grid grid-cols-2 gap-4 mb-6">
                    <div>
                        <label class="block text-xs font-medium mb-1">Mês/Ano</label>
                        <input type="month" id="report-month" class="input-field w-full rounded px-3 py-2 text-sm">
                    </div>
                    <div class="flex items-end">
                        <button onclick="window.generateReportData()" class="btn-primary w-full py-2 rounded font-bold">Pesquisar na Nuvem</button>
                    </div>
                </div>
                <div id="report-result" class="bg-black bg-opacity-20 p-4 rounded border border-gray-600 hidden text-center">
                    <h4 class="text-xl font-bold text-primary mb-2" id="res-unit">Unidade Vila Flor</h4>
                    <p class="text-sm mb-1">Total de Fichas Geradas no Mês:</p>
                    <p class="text-4xl font-bold" id="res-count">0</p>
                    <button onclick="window.printReport()" class="mt-4 bg-gray-200 text-black px-4 py-2 rounded text-sm font-bold shadow hover:bg-white"><i class="fas fa-print"></i> Imprimir Relatório</button>
                </div>
            </div>
        </div>
    </div>

    <!-- ========================================== -->
    <!-- ÁREAS DE IMPRESSÃO (Ocultas na tela)       -->
    <!-- ========================================== -->
    <div id="print-area"></div>
    <div id="print-report-area"></div>

    <!-- MÓDULO JAVASCRIPT: Firebase & Lógica -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-app.js";
        import { getAuth, signInWithPopup, GoogleAuthProvider, signOut, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, addDoc, query, where, getDocs, deleteDoc, serverTimestamp } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-firestore.js";

        // Configuração Firebase (Fornecida por você)
        const firebaseConfig = {
            apiKey: "AIzaSyBIPqTYYkG5vHr57CndPCmxUeACncNAobM",
            authDomain: "powfitpro-582df.firebaseapp.com",
            projectId: "powfitpro-582df",
            storageBucket: "powfitpro-582df.firebasestorage.app",
            messagingSenderId: "1080284745812",
            appId: "1:1080284745812:web:56729b17fcdd3d44aaddc1",
            measurementId: "G-GVPV7SSBTB"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const provider = new GoogleAuthProvider();

        let currentUser = null;

        // --- BANCOS DE DADOS LOCAIS ---
        const healthDataPEF = {
            "🟢 Saudável": "Manter treinos regulares 3–5x por semana, combinando musculação e cardio. Foco em evolução e constância.",
            "⚪ Sedentário": "Iniciar com treinos leves 2–3x por semana. Priorizar adaptação, técnica e evitar excesso de carga.",
            "🟡 Sobrepeso": "Treinar 3–5x por semana com foco em gasto calórico. Combinar musculação e cardio moderado.",
            "🔴 Obesidade": "Iniciar com exercícios de baixo impacto. Evolução gradual, priorizando saúde e segurança.",
            "⚖️ Baixo peso": "Foco em musculação para ganho de massa. Treinos moderados com alimentação adequada.",
            "🍬 Diabetes": "Treinos regulares moderados. Monitorar glicemia e evitar treinos em jejum.",
            "❤️ Hipertensão": "Treinos moderados, evitar prender a respiração. Priorizar controle da intensidade.",
            "🔵 Hipotensão": "Evitar mudanças bruscas. Manter hidratação e intensidade leve a moderada.",
            "💔 Problemas cardíacos": "Treinos leves com liberação médica. Monitorar frequência cardíaca.",
            "🦴 Problemas articulares": "Evitar impacto. Priorizar exercícios controlados e máquinas.",
            "🫁 Problemas respiratórios": "Treinos leves a moderados com progressão gradual. Atenção à respiração.",
            "⚠️ Lesões": "Adaptar exercícios. Evitar dor e focar na recuperação.",
            "🤰 Gestante": "Treinos leves a moderados, sem impacto ou risco. Foco em mobilidade e bem-estar.",
            "🤱 Lactante": "Treinar normalmente com atenção à hidratação e energia.",
            "👴 Idoso": "Foco em força, equilíbrio e mobilidade. Intensidade moderada com segurança."
        };

        const healthDataTE = {
            "🟢 Saudável": "Manter treinos regulares de 3–5x por semana, combinando musculação e cardio. Descanso e boa alimentação contribuem para resultados.",
            "⚪ Sedentário": "Início gradual, treinos leves e adaptação do corpo. Prioridade é constância, execução correta e evitar excesso de carga.",
            "🟡 Sobrepeso": "Musculação + cardio para melhora do condicionamento e composição corporal. Progressão deve respeitar a individualidade.",
            "🔴 Obesidade": "Iniciar com exercícios de menor impacto e progressão gradual. Segurança, mobilidade e constância são prioridades.",
            "⚖️ Baixo peso": "Musculação para ganho de massa, associada a alimentação e descanso. Priorizar evolução progressiva.",
            "🍬 Diabetes": "Manter acompanhamento médico regular. Em casos de tontura ou alteração de glicemia, interromper e procurar orientação.",
            "❤️ Hipertensão": "Acompanhamento médico regular. Evitar prender a respiração (Manobra de Valsalva) e controlar intensidade.",
            "🔵 Hipotensão": "Evitar mudanças bruscas de posição. Manter hidratação e respeitar intensidade para evitar mal-estar.",
            "💔 Problemas cardíacos": "Prática apenas com liberação médica. Respeitar limites, com controle de intensidade.",
            "🦴 Problemas articulares": "Exercícios com menor impacto e maior controle de movimento. Acompanhamento ajuda na segurança.",
            "🫁 Problemas respiratórios": "Progressão gradual respeitando capacidade respiratória. Interromper em caso de falta de ar excessiva.",
            "⚠️ Lesões": "Adaptação deve respeitar limitação e evitar dor. Seguir orientação profissional antes da prática.",
            "🤰 Gestante": "Prática com liberação médica. Foco na segurança, mobilidade e bem-estar, evitando impacto e risco.",
            "🤱 Lactante": "Atividade mantida normalmente, respeitando recuperação, hidratação e alimentação.",
            "👴 Idoso": "Musculação contribui para força, equilíbrio e mobilidade. Respeitar limitações, com intensidade moderada."
        };

        const objectiveData = {
            "Emagrecimento": "Foco em déficit calórico com treinos mistos de força (manter massa magra) e aeróbicos (maior gasto).",
            "Hipertrofia": "Prioridade na progressão de carga e volume adequado. Essencial superávit calórico e descanso.",
            "Definição": "Manutenção de massa muscular enquanto reduz o percentual de gordura. Atenção estrita à dieta.",
            "Condicionamento": "Treinos com menor tempo de intervalo, circuitos e alta integração cardiopulmonar.",
            "Resistência": "Séries mais longas, cadência controlada e aprimoramento da capacidade muscular.",
            "Força": "Cargas altas, baixas repetições e intervalos de descanso maiores.",
            "Reabilitação": "Treino focado em fortalecimento específico, mobilidade e controle motor. Respeitar limites.",
            "Saúde geral": "Equilíbrio entre força, cardio e flexibilidade. O principal objetivo é a constância e bem-estar."
        };

        const dbCategories = {
            "🔥 PEITO": ["Supino Reto", "Supino Inclinado", "Supino Inclinado com Halteres", "Supino Fechado com Halteres", "Cross Over", "Cross Over Alto", "Cross Over Baixo", "Crucifixo Reto", "Crucifixo Inclinado com Halteres", "Crucifixo na Máquina", "Peck Fly", "Peck Fly Unilateral", "Pullover", "Flexão de Braço", "Flexão com Pés Elevados", "Flexão Explosiva"],
            "🦍 COSTAS": ["Puxada Alta", "Puxada de Frente Supinada", "Pulldown", "Remada Aberta", "Remada Baixa", "Remada Curvada", "Remada Curvada Supinada", "Remada Cavalinho (T-Bar)", "Serrote", "Facepull (puxada de cima para baixo)", "Puxada Unilateral", "Remada Unilateral", "Remada no Cross", "Encolhimento (Trapézio)"],
            "🦵 PERNAS": ["Agachamento Livre", "Agachamento Taça", "Agachamento no Smith", "Agachamento com passada lateral", "Squat", "Hack Machine", "Leg 45°", "Leg 90°", "Agachamento Sumô", "Agachamento Sissy (Livre)", "Afundo", "Recuo", "Avanço", "Passada", "Búlgaro", "Step-up", "Levantamento Terra", "Levantamento Terra Romeno", "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", "Cadeira Flexora", "Elevação Pélvica no Banco", "Elevação Pélvica no Chão", "Elevação Pélvica Unilateral no Chão", "Extensão de Quadril (Glúteo Máximo)", "Extensão Cruzada (Glúteo Médio)", "Coice", "Cachorrinho", "Cadeira Extensora", "Adução", "Abdução", "Abdução Inclinada", "Flexão Nórdica", "Flexão Nórdica Invertida", "Panturrilha Livre", "Panturrilha no Leg Press", "Panturrilha Banco", "Panturrilha Squat", "Panturrilha Unilateral"],
            "💪 BRAÇOS": ["(Bíceps) Rosca Direta", "(Bíceps) Rosca Alternada", "(Bíceps) Rosca 21", "(Bíceps) Rosca Scott Barra W", "(Bíceps) Rosca Scott Unilateral", "(Bíceps) Rosca Scott com Halteres", "(Bíceps) Rosca Martelo", "(Bíceps) Rosca Cross", "(Bíceps) Rosca Inversa", "(Bíceps) Rosca 45°", "(Tríceps) Pulley Unilateral", "(Tríceps) Pulley Barra", "(Tríceps) Pulley Corda", "(Tríceps) Pulley Pegada Inversa", "(Tríceps) Francês na Corda", "(Tríceps) Francês com Halter", "(Tríceps) Francês Unilateral", "(Tríceps) Cruzado Polia Dupla", "(Tríceps) Coice Unilateral", "(Tríceps) Arremesso", "(Tríceps) Testa", "(Tríceps) Mergulho no Banco"],
            "🪨 OMBROS": ["Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", "Elevação Lateral Unilateral Cross", "Elevação Lateral Sentado", "Desenvolvimento com Halteres", "Desenvolvimento com Barra", "Arnold Press", "Elevação Borboleta", "Crucifixo Inverso Sentado com Halteres", "Crucifixo Inverso na Polia", "Crucifixo Inverso Unilateral na Polia", "Facepull (puxada reta)", "Remada Alta", "Encolhimento (Trapézio)"],
            "🧠 ABDÔMEN": ["Infra com Elevação de Perna", "Abdominal Supra", "Abdominal Remador", "Abdominal Bicicleta", "Abdominal Twister com Peso", "Prancha", "Prancha Lateral", "Trituração de Cabos em Pé", "Isometria na parede", "Abdominal isometrico"],
            "🫀 CARDIO": ["Bicicleta 10 Minutos", "Bicicleta 15 Minutos", "Bicicleta 20 Minutos", "Esteira 10 Minutos", "Esteira 15 Minutos", "Esteira 20 Minutos", "Pular Corda"]
        };

        const dbTechniques = ["Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", "Negativa", "Isometria", "Parciais", "Pirâmide"];
        const daysOfWeek = ["SEGUNDA-FEIRA", "TERÇA-FEIRA", "QUARTA-FEIRA", "QUINTA-FEIRA", "SEXTA-FEIRA", "SÁBADO", "DOMINGO"];

        let state = {
            currentFichaId: null,
            workouts: [],
            activeModalWorkoutId: null,
            activeCategory: "🔥 PEITO"
        };

        // --- AUTH ---
        onAuthStateChanged(auth, async (user) => {
            const overlay = document.getElementById('loading-overlay');
            if (user) {
                currentUser = user;
                document.getElementById('login-screen').classList.add('hidden');
                document.getElementById('app-container').classList.remove('hidden');
                document.getElementById('user-avatar').src = user.photoURL || `https://ui-avatars.com/api/?name=${user.displayName}&background=3b82f6&color=fff`;
                document.getElementById('user-display-name').innerText = user.displayName;
                
                await loadUserProfile();
                if(state.workouts.length === 0) window.addWorkout(); // Add first day if empty
            } else {
                currentUser = null;
                document.getElementById('login-screen').classList.remove('hidden');
                document.getElementById('app-container').classList.add('hidden');
            }
            overlay.style.display = 'none';
        });

        window.loginGoogle = () => signInWithPopup(auth, provider);
        window.logoutGoogle = () => signOut(auth);

        // --- PROFILE FIRESTORE ---
        async function loadUserProfile() {
            try {
                const docRef = doc(db, "users", currentUser.uid);
                const docSnap = await getDoc(docRef);
                if (docSnap.exists()) {
                    const data = docSnap.data();
                    document.getElementById('prof-network').value = data.network || '';
                    document.getElementById('prof-unit').value = data.unit || '';
                    document.getElementById('prof-team').value = data.team || '';
                    document.getElementById('prof-type').value = data.type || 'PEF';
                    document.getElementById('prof-name').value = data.name || currentUser.displayName;
                    document.getElementById('prof-cpf').value = data.cpf || '';
                    document.getElementById('prof-cref').value = data.cref || '';
                    document.getElementById('prof-state').value = data.state || '';
                    
                    document.getElementById('user-network-display').innerText = data.unit ? `Unidade ${data.unit}` : 'Defina sua unidade';
                } else {
                    document.getElementById('prof-name').value = currentUser.displayName;
                }
                window.toggleProfType();
            } catch(e) { console.error("Erro perfil:", e); }
        }

        window.saveProfile = async () => {
            try {
                await setDoc(doc(db, "users", currentUser.uid), {
                    network: document.getElementById('prof-network').value,
                    unit: document.getElementById('prof-unit').value,
                    team: document.getElementById('prof-team').value,
                    type: document.getElementById('prof-type').value,
                    name: document.getElementById('prof-name').value,
                    cpf: document.getElementById('prof-cpf').value,
                    cref: document.getElementById('prof-cref').value,
                    state: document.getElementById('prof-state').value,
                });
                document.getElementById('user-network-display').innerText = `Unidade ${document.getElementById('prof-unit').value}`;
                alert("Perfil profissional salvo na nuvem!");
            } catch(e) { alert("Erro ao salvar perfil. Verifique permissões."); console.error(e); }
        };

        // --- UI & LOGIC ---
        window.toggleProfType = () => {
            const type = document.getElementById('prof-type').value;
            const fields = document.querySelectorAll('.cref-field');
            if(type === 'TE') {
                fields.forEach(f => f.classList.add('hidden'));
                window.renderHealthOptions(healthDataTE);
            } else {
                fields.forEach(f => f.classList.remove('hidden'));
                window.renderHealthOptions(healthDataPEF);
            }
        };

        window.changeTheme = () => document.body.setAttribute('data-theme', document.getElementById('stu-gender').value);

        window.calculateIMC = () => {
            const w = parseFloat(document.getElementById('stu-weight').value);
            const h = parseFloat(document.getElementById('stu-height').value);
            const display = document.getElementById('imc-display');
            if (w > 0 && h > 0) {
                const imc = (w / (h * h)).toFixed(1);
                let classif = imc < 18.5 ? "Abaixo" : imc < 24.9 ? "Normal" : imc < 29.9 ? "Sobrepeso" : "Obesidade";
                display.innerHTML = `<span class="bg-primary bg-opacity-20 text-primary px-3 py-1 rounded text-xs font-bold border border-primary border-opacity-30">IMC: ${imc} (${classif})</span>`;
            } else display.innerHTML = '';
        };

        window.renderHealthOptions = (dataObj) => {
            const selected = Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value);
            document.getElementById('health-container').innerHTML = Object.keys(dataObj).map(opt => `
                <label class="flex items-start space-x-2 cursor-pointer p-1 rounded hover:bg-black hover:bg-opacity-5">
                    <input type="checkbox" value="${opt}" ${selected.includes(opt)?'checked':''} class="health-cb mt-0.5 w-3.5 h-3.5 text-primary">
                    <span class="font-medium">${opt}</span>
                </label>`).join('');
        };

        const generateId = () => Math.random().toString(36).substr(2, 9);

        window.addWorkout = () => {
            state.workouts.push({ id: generateId(), title: `TREINO ${daysOfWeek[state.workouts.length % 7] || 'EXTRA'}`, exercises: [] });
            window.renderWorkouts();
        };

        window.removeWorkout = (id) => { if(confirm("Excluir treino?")) { state.workouts = state.workouts.filter(w => w.id !== id); window.renderWorkouts(); } };
        window.duplicateWorkout = (id) => {
            const w = state.workouts.find(x => x.id === id);
            if(w) { state.workouts.push({ ...JSON.parse(JSON.stringify(w)), id: generateId(), title: w.title + " (Cópia)" }); window.renderWorkouts(); }
        };
        window.updateWorkoutTitle = (id, val) => { const w = state.workouts.find(x => x.id === id); if(w) w.title = val; };

        window.removeExercise = (wId, idx) => { state.workouts.find(w => w.id === wId).exercises.splice(idx, 1); window.renderWorkouts(); };
        window.moveExercise = (wId, idx, dir) => {
            const arr = state.workouts.find(w => w.id === wId).exercises;
            if (dir==='up' && idx>0) [arr[idx], arr[idx-1]] = [arr[idx-1], arr[idx]];
            if (dir==='down' && idx<arr.length-1) [arr[idx], arr[idx+1]] = [arr[idx+1], arr[idx]];
            window.renderWorkouts();
        };
        window.updateExercise = (wId, idx, field, val) => { state.workouts.find(w => w.id === wId).exercises[idx][field] = val; };

        window.renderWorkouts = () => {
            document.getElementById('workouts-container').innerHTML = state.workouts.map(workout => `
                <div class="card rounded-xl overflow-hidden shadow-sm">
                    <div class="p-3 border-b flex justify-between bg-black bg-opacity-5" style="border-color: var(--border-color);">
                        <input type="text" value="${workout.title}" onchange="window.updateWorkoutTitle('${workout.id}', this.value)" class="input-field bg-transparent font-bold text-sm w-1/2 px-2 py-1 rounded uppercase">
                        <div class="flex gap-1">
                            <button onclick="window.duplicateWorkout('${workout.id}')" class="text-xs p-1.5 rounded hover:bg-black hover:bg-opacity-10"><i class="fas fa-copy"></i></button>
                            <button onclick="window.removeWorkout('${workout.id}')" class="text-xs p-1.5 text-red-500 rounded hover:bg-red-500 hover:text-white"><i class="fas fa-trash"></i></button>
                        </div>
                    </div>
                    <div class="p-0">
                        ${workout.exercises.length===0 ? `<div class="text-center p-4 text-xs opacity-50 italic">Sem exercícios.</div>` : 
                        workout.exercises.map((ex, idx) => `
                        <div class="flex flex-col sm:flex-row gap-2 p-2 items-start sm:items-center border-b last:border-0 border-opacity-10" style="border-color: var(--border-color)">
                            <div class="flex gap-1 hidden sm:flex">
                                <button onclick="window.moveExercise('${workout.id}', ${idx}, 'up')" class="text-[10px] p-1"><i class="fas fa-chevron-up"></i></button>
                                <button onclick="window.moveExercise('${workout.id}', ${idx}, 'down')" class="text-[10px] p-1"><i class="fas fa-chevron-down"></i></button>
                            </div>
                            <div class="flex-1 font-medium w-full sm:w-auto">
                                <div class="text-[9px] opacity-60 uppercase">${ex.category}</div>
                                <div class="text-xs">${ex.name}</div>
                            </div>
                            <div class="flex flex-wrap gap-1 w-full sm:w-auto">
                                <input type="text" value="${ex.sets}" onchange="window.updateExercise('${workout.id}', ${idx}, 'sets', this.value)" class="input-field w-12 rounded px-1 text-xs text-center" placeholder="Séries">
                                <span class="self-center opacity-50 text-xs">x</span>
                                <input type="text" value="${ex.reps}" onchange="window.updateExercise('${workout.id}', ${idx}, 'reps', this.value)" class="input-field w-16 rounded px-1 text-xs text-center" placeholder="Reps">
                                <select onchange="window.updateExercise('${workout.id}', ${idx}, 'technique', this.value)" class="input-field w-24 rounded px-1 text-[10px]">
                                    ${dbTechniques.map(t => `<option value="${t}" ${ex.technique===t?'selected':''}>${t}</option>`).join('')}
                                </select>
                                <input type="text" value="${ex.obs}" onchange="window.updateExercise('${workout.id}', ${idx}, 'obs', this.value)" class="input-field flex-1 sm:w-32 rounded px-1 text-xs" placeholder="Obs...">
                            </div>
                            <div class="flex items-center w-full sm:w-auto justify-end">
                                <button onclick="window.removeExercise('${workout.id}', ${idx})" class="text-red-500 p-1"><i class="fas fa-trash"></i></button>
                            </div>
                        </div>`).join('')}
                    </div>
                    <div class="p-2 border-t" style="border-color: var(--border-color)">
                        <button onclick="window.openModal('${workout.id}')" class="w-full text-primary py-1.5 rounded-lg text-xs font-semibold hover:bg-primary hover:bg-opacity-10 transition">
                            + ADICIONAR EXERCÍCIO
                        </button>
                    </div>
                </div>`).join('');
        };

        // --- MODAL DE EXERCÍCIOS ---
        window.openModal = (id) => { state.activeModalWorkoutId = id; document.getElementById('exercise-modal').classList.remove('hidden'); window.renderModalCategories(); window.renderModalExercises(); };
        window.closeModal = () => { document.getElementById('exercise-modal').classList.add('hidden'); };
        window.setModalCategory = (cat) => { state.activeCategory = cat; window.renderModalCategories(); window.renderModalExercises(); };
        
        window.renderModalCategories = () => {
            document.getElementById('modal-categories').innerHTML = Object.keys(dbCategories).map(cat => `
                <button onclick="window.setModalCategory('${cat}')" class="w-full text-left px-2 py-2 rounded-lg text-xs font-medium ${state.activeCategory === cat ? 'bg-primary text-white' : 'hover:bg-black hover:bg-opacity-10'}">${cat}</button>
            `).join('');
        };
        
        window.renderModalExercises = () => {
            document.getElementById('modal-exercises').innerHTML = `<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-2">` + 
                dbCategories[state.activeCategory].map(ex => `<button onclick="window.addEx('${ex}')" class="card p-2 rounded-lg text-left text-xs font-medium border hover:border-primary transition"><span>${ex}</span></button>`).join('') + `</div>`;
        };
        
        window.addEx = (ex) => {
            const w = state.workouts.find(x => x.id === state.activeModalWorkoutId);
            if(w) {
                const isC = state.activeCategory === "🫀 CARDIO";
                w.exercises.push({ category: state.activeCategory, name: ex, sets: isC ? '1' : '3', reps: isC ? '-' : '10 a 12', technique: 'Nenhuma', obs: '' });
                window.renderWorkouts();
                document.getElementById('modal-exercises').style.opacity = '0.5'; setTimeout(() => document.getElementById('modal-exercises').style.opacity = '1', 100);
            }
        };

        // --- FIREBASE HISTORY & PRINT ---
        window.getDataObject = () => {
            const today = new Date();
            const monthYear = `${today.getFullYear()}-${(today.getMonth()+1).toString().padStart(2,'0')}`;
            return {
                uid: currentUser.uid,
                unitName: document.getElementById('prof-unit').value || 'S/N',
                monthYear: monthYear,
                timestamp: serverTimestamp(),
                createdAtDisplay: today.toLocaleDateString('pt-BR') + ' ' + today.toLocaleTimeString('pt-BR', {hour: '2-digit', minute:'2-digit'}),
                
                profType: document.getElementById('prof-type').value,
                profName: document.getElementById('prof-name').value,
                profCref: document.getElementById('prof-cref').value,
                profState: document.getElementById('prof-state').value,
                
                stuName: document.getElementById('stu-name').value || 'Aluno(a)',
                stuAge: document.getElementById('stu-age').value,
                stuWeight: document.getElementById('stu-weight').value,
                stuHeight: document.getElementById('stu-height').value,
                stuGender: document.getElementById('stu-gender').value,
                stuLevel: document.getElementById('stu-level').value,
                stuObjective: document.getElementById('stu-objective').value,
                stuFreq: document.getElementById('stu-freq').value,
                stuValidity: document.getElementById('stu-validity').value,
                stuRecs: document.getElementById('stu-recs').value,
                health: Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value),
                workouts: state.workouts
            };
        };

        window.generateAndSavePrint = async () => {
            const overlay = document.getElementById('loading-overlay');
            overlay.style.display = 'flex';
            overlay.querySelector('h2').innerText = "Salvando na Nuvem...";
            
            try {
                // Salvar Perfil Auto
                await window.saveProfile();
                
                // Salvar Ficha no Firestore
                const docData = window.getDataObject();
                let docId = state.currentFichaId;
                if(docId) {
                    await setDoc(doc(db, "fichas", docId), docData);
                } else {
                    const docRef = await addDoc(collection(db, "fichas"), docData);
                    state.currentFichaId = docRef.id;
                }

                // Montar HTML de Impressão
                const d = window.getDataObject();
                const imcStr = (d.stuWeight>0 && d.stuHeight>0) ? (d.stuWeight / (d.stuHeight * d.stuHeight)).toFixed(1) : "-";
                const isPEF = d.profType === 'PEF';
                const healthDict = isPEF ? healthDataPEF : healthDataTE;
                const crefText = isPEF ? `CREF: ${d.profCref} - Estado: ${d.profState}` : '';
                const legalText = isPEF ? 
                    `⚠️ OBSERVAÇÃO LEGAL – PROFISSIONAL DE EDUCAÇÃO FÍSICA: Conforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º estabelece que compete ao profissional coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos especializados nas áreas de atividades físicas e do desporto.` : 
                    `⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO: Conforme a Lei nº 14.597/2023 (Lei Geral do Esporte), Art. 75, a profissão de treinador esportivo é reconhecida e regulada no Brasil... As recomendações desta ficha possuem caráter informativo e orientativo, voltadas ao treinamento esportivo e prática de musculação. Este material não substitui avaliação médica ou acompanhamento de profissionais da saúde. Em casos de dores, lesões, doenças, gestação ou qualquer condição específica, recomenda-se procurar um profissional habilitado antes de iniciar ou continuar os treinos.`;

                let html = `
                    <div class="print-header">
                        <h1 class="print-title">Planilha de Treinamento - ${d.stuObjective}</h1>
                        <div class="prof-info">Prescrição feita por: ${d.profName} (${isPEF?'Profissional de Educação Física':'Treinador Esportivo'})<br>${crefText}</div>
                    </div>
                    <div class="print-grid">
                        <div><strong>Aluno:</strong> ${d.stuName}</div><div><strong>Idade:</strong> ${d.stuAge || '-'} anos</div><div><strong>Gênero:</strong> ${d.stuGender}</div>
                        <div><strong>Peso:</strong> ${d.stuWeight || '-'} kg</div><div><strong>Altura:</strong> ${d.stuHeight || '-'} m</div><div><strong>IMC:</strong> ${imcStr}</div>
                        <div><strong>Nível:</strong> ${d.stuLevel}</div><div><strong>Frequência:</strong> ${d.stuFreq}</div><div><strong>Validade:</strong> ${d.stuValidity.replace('Validade: ', '')}</div>
                    </div>`;

                if (objectiveData[d.stuObjective] || d.health.length > 0) {
                    html += `<div class="print-guidelines"><h4>Diretrizes Automáticas de Perfil</h4><ul>`;
                    if(objectiveData[d.stuObjective]) html += `<li><strong>Objetivo (${d.stuObjective}):</strong> ${objectiveData[d.stuObjective]}</li>`;
                    d.health.forEach(k => { if(healthDict[k]) html += `<li><strong>Saúde (${k.replace(/[\u{1F300}-\u{1F9FF}\u{2600}-\u{26FF}\u{2700}-\u{27BF}\u{FE0F}]/gu, '').trim()}):</strong> ${healthDict[k]}</li>`; });
                    html += `</ul></div>`;
                }

                d.workouts.forEach(w => {
                    html += `<div class="print-workout"><h3>${w.title}</h3><table><thead><tr><th style="width: 35%">Exercício</th><th style="width: 8%; text-align:center;">Séries</th><th style="width: 12%; text-align:center;">Reps</th><th style="width: 15%">Técnica</th><th style="width: 30%">Observações</th></tr></thead><tbody>`;
                    if (w.exercises.length === 0) html += `<tr><td colspan="5" style="text-align:center; font-style:italic;">Sem exercícios</td></tr>`;
                    else w.exercises.forEach(ex => { html += `<tr><td><strong>${ex.name}</strong></td><td style="text-align:center;">${ex.sets}</td><td style="text-align:center;">${ex.reps}</td><td>${ex.technique!=='Nenhuma'?ex.technique:'-'}</td><td>${ex.obs||'-'}</td></tr>`; });
                    html += `</tbody></table></div>`;
                });

                html += `<div class="print-footer-section">`;
                if(d.stuRecs.trim()) html += `<strong>Recomendações Específicas do Profissional:</strong><p style="white-space: pre-wrap; margin: 3px 0 10px 0;">${d.stuRecs}</p>`;
                html += `<div class="legal-text">${legalText}</div><div style="text-align:center; margin-top: 15px; font-size: 8px;">PowFit Pro Cloud - Gerado em ${d.createdAtDisplay}</div></div>`;

                document.getElementById('print-area').innerHTML = html;
                overlay.style.display = 'none';
                
                // Abre impressão
                setTimeout(() => { window.print(); }, 300);

            } catch (e) {
                overlay.style.display = 'none';
                alert("Erro ao salvar no Firebase. Verifique suas regras no console Firestore.");
                console.error(e);
            }
        };

        // --- MODAL HISTORY (NUVEM) ---
        window.openHistory = async () => {
            document.getElementById('history-modal').classList.remove('hidden');
            const list = document.getElementById('history-list');
            list.innerHTML = '<div class="text-center py-5"><i class="fas fa-spinner fa-spin text-primary text-2xl"></i> Buscando na nuvem...</div>';
            
            try {
                const q = query(collection(db, "fichas"), where("uid", "==", currentUser.uid));
                const querySnapshot = await getDocs(q);
                let docs = [];
                querySnapshot.forEach(doc => docs.push({ id: doc.id, ...doc.data() }));
                
                // Sort by timestamp desc locally since we might lack composite indexes
                docs.sort((a,b) => (b.timestamp?.seconds || 0) - (a.timestamp?.seconds || 0));

                if(docs.length === 0) { list.innerHTML = `<div class="text-center opacity-50 py-10">Nenhuma ficha na nuvem.</div>`; return; }

                list.innerHTML = docs.map(h => `
                    <div class="card p-3 rounded-lg flex justify-between items-center border border-gray-500 border-opacity-20" id="hist-${h.id}">
                        <div><div class="font-bold text-sm">${h.stuName}</div><div class="text-[10px] opacity-70">${h.createdAtDisplay} | Obj: ${h.stuObjective}</div></div>
                        <div class="flex gap-1">
                            <button onclick="window.loadHistory('${h.id}')" class="bg-primary text-white px-2 py-1 rounded text-xs font-bold">CARREGAR</button>
                            <button onclick="window.deleteHistory('${h.id}')" class="bg-red-500 text-white px-2 py-1 rounded text-xs font-bold"><i class="fas fa-trash"></i></button>
                        </div>
                    </div>`).join('');
            } catch (e) {
                list.innerHTML = `<div class="text-center text-red-500 py-10">Erro ao carregar dados.</div>`;
                console.error(e);
            }
        };
        window.closeHistory = () => document.getElementById('history-modal').classList.add('hidden');
        
        window.loadHistory = async (id) => {
            window.closeHistory();
            const overlay = document.getElementById('loading-overlay'); overlay.style.display = 'flex';
            try {
                const docSnap = await getDoc(doc(db, "fichas", id));
                if (docSnap.exists()) {
                    const data = docSnap.data();
                    state.currentFichaId = id;
                    document.getElementById('stu-name').value = data.stuName || '';
                    document.getElementById('stu-age').value = data.stuAge || '';
                    document.getElementById('stu-weight').value = data.stuWeight || '';
                    document.getElementById('stu-height').value = data.stuHeight || '';
                    document.getElementById('stu-gender').value = data.stuGender || 'Masculino';
                    document.getElementById('stu-level').value = data.stuLevel || 'Iniciante';
                    document.getElementById('stu-objective').value = data.stuObjective || 'Emagrecimento';
                    document.getElementById('stu-freq').value = data.stuFreq || '3 dias';
                    document.getElementById('stu-validity').value = data.stuValidity || 'Validade: 30 dias';
                    document.getElementById('stu-recs').value = data.stuRecs || '';
                    
                    window.changeTheme();
                    window.calculateIMC();

                    document.querySelectorAll('.health-cb').forEach(cb => { cb.checked = (data.health || []).includes(cb.value); });
                    state.workouts = data.workouts || [];
                    window.renderWorkouts();
                }
            } catch(e) { console.error(e); }
            overlay.style.display = 'none';
        };

        window.deleteHistory = async (id) => {
            if(confirm("Excluir esta ficha permanentemente da nuvem?")) {
                try {
                    await deleteDoc(doc(db, "fichas", id));
                    document.getElementById(`hist-${id}`).remove();
                    if(state.currentFichaId === id) state.currentFichaId = null;
                } catch(e) { alert("Erro ao excluir."); console.error(e); }
            }
        };

        // --- RELATÓRIO DE UNIDADE ---
        window.openReport = () => {
            document.getElementById('report-modal').classList.remove('hidden');
            const d = new Date();
            document.getElementById('report-month').value = `${d.getFullYear()}-${(d.getMonth()+1).toString().padStart(2,'0')}`;
            document.getElementById('report-result').classList.add('hidden');
        };
        window.closeReport = () => { document.getElementById('report-modal').classList.add('hidden'); };

        window.generateReportData = async () => {
            const monthVal = document.getElementById('report-month').value;
            const unitVal = document.getElementById('prof-unit').value;
            if(!unitVal) return alert("Defina o nome da sua Unidade no Perfil primeiro.");
            if(!monthVal) return alert("Selecione um mês.");

            document.getElementById('report-result').classList.add('hidden');
            
            try {
                // Busca todas as fichas dessa unidade. Em apps enormes, precisa de index, mas aqui tratamos via JS a data.
                const q = query(collection(db, "fichas"), where("unitName", "==", unitVal));
                const snap = await getDocs(q);
                let count = 0;
                snap.forEach(doc => { if(doc.data().monthYear === monthVal) count++; });

                document.getElementById('res-unit').innerText = `Unidade ${unitVal}`;
                document.getElementById('res-count').innerText = count;
                document.getElementById('report-result').classList.remove('hidden');
            } catch(e) {
                alert("Erro ao gerar relatório. Verifique conexões."); console.error(e);
            }
        };

        window.printReport = () => {
            const unit = document.getElementById('prof-unit').value;
            const month = document.getElementById('report-month').value;
            const count = document.getElementById('res-count').innerText;
            const net = document.getElementById('prof-network').value || 'Sem Rede';

            const html = `
                <div class="report-title">Relatório de Prescrições PowFit Pro</div>
                <div class="report-summary">
                    <strong>Rede:</strong> ${net} <br>
                    <strong>Unidade:</strong> ${unit} <br>
                    <strong>Mês/Ano de Referência:</strong> ${month} <br>
                    <strong>Total de Fichas Geradas:</strong> ${count}
                </div>
                <div style="font-size: 10px; text-align: center; margin-top: 50px;">
                    Gerado automaticamente pelo sistema em ${new Date().toLocaleDateString('pt-BR')} - PowFit Pro Cloud
                </div>
            `;
            
            const pArea = document.getElementById('print-area');
            const rArea = document.getElementById('print-report-area');
            
            pArea.style.display = 'none';
            rArea.innerHTML = html;
            
            setTimeout(() => { window.print(); rArea.innerHTML=''; pArea.style.display=''; }, 200);
        };

    </script>
</body>
</html>
