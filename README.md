<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro - Plataforma Profissional de Treinos (Cloud)</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            /* Tema Masculino (Escuro Fitness) */
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

        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg-body);
            color: var(--text-main);
            transition: background-color 0.3s ease, color 0.3s ease;
        }

        .card {
            background-color: var(--bg-card);
            border: 1px solid var(--border-color);
            transition: background-color 0.3s ease, border-color 0.3s ease;
        }

        .input-field {
            background-color: var(--input-bg);
            border: 1px solid var(--input-border);
            color: var(--text-main);
        }
        
        .input-field:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 1px var(--primary);
        }

        .btn-primary { background-color: var(--primary); color: white; transition: background-color 0.2s; }
        .btn-primary:hover { background-color: var(--primary-hover); }

        #print-area { display: none; }

        /* Estilos de Impressão (Estilo Planilha) */
        @media print {
            body { background: white !important; color: black !important; margin: 0; padding: 0; }
            #app-container, #history-modal, #exercise-modal, .no-print { display: none !important; }
            #print-area {
                display: block !important;
                padding: 10mm 15mm;
                font-family: 'Arial', sans-serif;
                font-size: 10px;
            }
            @page { size: A4; margin: 0; }
            
            .print-header { border-bottom: 2px solid #000; padding-bottom: 8px; margin-bottom: 12px; }
            .print-title { font-size: 18px; font-weight: bold; margin: 0; text-transform: uppercase; text-align: center;}
            .prof-info { font-size: 11px; margin-top: 5px; text-align: center; font-weight: bold; line-height: 1.4;}
            
            .print-grid {
                display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 4px;
                margin-bottom: 15px; border: 1px solid #000; padding: 8px; background: #fff;
            }
            .print-grid div { font-size: 11px; }
            
            .print-guidelines { border: 1px solid #000; padding: 8px; margin-bottom: 15px; font-size: 9.5px; }
            .print-guidelines h4 { margin: 0 0 4px 0; font-size: 11px; text-transform: uppercase; background: #eee; padding: 3px;}
            .print-guidelines ul { margin: 0; padding-left: 15px; }
            .print-guidelines li { margin-bottom: 3px; }

            .print-workout { margin-bottom: 15px; page-break-inside: avoid; }
            .print-workout h3 {
                background: #e0e0e0; border: 1px solid #000; border-bottom: none;
                padding: 4px 8px; margin: 0; font-size: 12px; text-transform: uppercase;
                -webkit-print-color-adjust: exact; color-adjust: exact;
            }
            table { width: 100%; border-collapse: collapse; margin-bottom: 0; border: 1px solid #000; }
            th, td { border: 1px solid #000; padding: 4px 6px; text-align: left; font-size: 10px; }
            th { background-color: #f0f0f0; font-weight: bold; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            
            .print-footer-section { margin-top: 15px; border: 1px solid #000; padding: 8px; font-size: 9px; page-break-inside: avoid;}
            .legal-text { font-size: 8px; color: #333; text-align: justify; margin-top: 8px; font-style: italic; border-top: 1px dashed #ccc; padding-top: 5px;}
        }

        /* Loading Spinner */
        .loader {
            border: 3px solid rgba(255,255,255,0.3); border-radius: 50%;
            border-top: 3px solid #fff; width: 20px; height: 20px;
            animation: spin 1s linear infinite; display: none;
        }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }

        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
    </style>
</head>
<body data-theme="Masculino" class="min-h-screen pb-20">

    <!-- ÁREA PRINCIPAL DO SISTEMA -->
    <div id="app-container" class="max-w-7xl mx-auto p-4 sm:p-6 lg:p-8">
        
        <!-- Header & Auth -->
        <div class="flex flex-col lg:flex-row justify-between items-center mb-8 gap-4 bg-black bg-opacity-20 p-4 rounded-xl border border-opacity-20" style="border-color: var(--border-color)">
            <div class="flex items-center gap-3">
                <i class="fas fa-dumbbell text-3xl text-primary"></i>
                <div>
                    <h1 class="text-2xl font-bold tracking-tight">PowFit Pro</h1>
                    <p class="text-xs text-primary font-medium tracking-widest uppercase">Cloud System</p>
                </div>
            </div>

            <!-- Login / Logout UI -->
            <div id="auth-ui" class="flex items-center gap-4">
                <div id="user-info" class="hidden items-center gap-3">
                    <img id="user-photo" src="" class="w-10 h-10 rounded-full border-2 border-primary object-cover">
                    <div class="hidden sm:block">
                        <p class="text-sm font-bold" id="user-name"></p>
                        <button onclick="window.logout()" class="text-xs text-red-400 hover:text-red-300 transition">Sair da conta</button>
                    </div>
                </div>
                <button id="login-btn" onclick="window.login()" class="bg-white text-gray-800 hover:bg-gray-100 px-5 py-2.5 rounded-lg font-bold shadow flex items-center gap-2 transition">
                    <img src="https://upload.wikimedia.org/wikipedia/commons/c/c1/Google_%22G%22_logo.svg" class="w-4 h-4">
                    Entrar com Google
                </button>
                <div id="app-actions" class="hidden gap-2 flex-wrap justify-center">
                    <button onclick="window.openHistory()" class="bg-gray-700 hover:bg-gray-600 text-white px-4 py-2.5 rounded-lg font-medium shadow flex items-center gap-2 transition">
                        <i class="fas fa-cloud"></i> Salvos na Nuvem
                    </button>
                    <button onclick="window.saveAndPrint()" class="btn-primary px-6 py-2.5 rounded-lg font-medium shadow-lg flex items-center gap-2 relative">
                        <span id="btn-text"><i class="fas fa-save"></i> <i class="fas fa-print"></i> Salvar & Imprimir</span>
                        <div id="save-loader" class="loader absolute left-1/2 top-1/2 transform -translate-x-1/2 -translate-y-1/2"></div>
                    </button>
                </div>
            </div>
        </div>

        <div id="main-content" class="grid grid-cols-1 xl:grid-cols-12 gap-6 opacity-50 pointer-events-none transition-opacity duration-300">
            
            <!-- Aviso se deslogado -->
            <div id="login-warning" class="xl:col-span-12 bg-red-500 bg-opacity-10 border border-red-500 text-red-500 p-4 rounded-xl text-center font-medium">
                ⚠️ Faça login com o Google para montar, salvar e imprimir as fichas.
            </div>

            <!-- COLUNA ESQUERDA: Configurações -->
            <div class="xl:col-span-4 space-y-6">
                
                <!-- Card: Profissional -->
                <div class="card rounded-xl p-5 shadow-sm">
                    <h2 class="text-lg font-semibold mb-4 flex items-center gap-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <i class="fas fa-id-badge text-primary"></i> Dados do Profissional
                    </h2>
                    <div class="space-y-4">
                        <div>
                            <label class="block text-sm font-medium mb-1">Área de Atuação</label>
                            <select id="prof-type" onchange="window.toggleProfType()" class="input-field w-full rounded-lg px-3 py-2 text-sm font-medium">
                                <option value="PEF">Profissional de Educação Física</option>
                                <option value="TE">Treinador Esportivo</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-1">Nome</label>
                            <input type="text" id="prof-name" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Seu nome completo">
                        </div>
                        <div class="grid grid-cols-2 gap-3" id="cref-container">
                            <div>
                                <label class="block text-sm font-medium mb-1">CREF</label>
                                <input type="text" id="prof-cref" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: 000000">
                            </div>
                            <div>
                                <label class="block text-sm font-medium mb-1">Estado</label>
                                <input type="text" id="prof-state" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: SP">
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
                            <label class="block text-sm font-medium mb-1">Nome Completo</label>
                            <input type="text" id="stu-name" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Nome do aluno">
                        </div>
                        <div class="grid grid-cols-3 gap-3">
                            <div>
                                <label class="block text-sm font-medium mb-1">Idade</label>
                                <input type="number" id="stu-age" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Anos">
                            </div>
                            <div>
                                <label class="block text-sm font-medium mb-1">Peso(kg)</label>
                                <input type="number" id="stu-weight" oninput="window.calculateIMC()" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="kg">
                            </div>
                            <div>
                                <label class="block text-sm font-medium mb-1">Alt(m)</label>
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
                    <p class="text-[11px] opacity-70 mb-3 leading-tight">Diretrizes automáticas baseadas na atuação profissional.</p>
                    <div class="grid grid-cols-2 gap-2 text-xs" id="health-container">
                        <!-- Gerado via JS -->
                    </div>
                </div>

            </div>

            <!-- COLUNA DIREITA: Montagem dos Treinos -->
            <div class="xl:col-span-8 space-y-6">
                
                <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center bg-opacity-10 p-4 rounded-xl card border-dashed border-2 gap-4">
                    <div>
                        <h2 class="text-xl font-bold flex items-center gap-2">
                            <i class="fas fa-clipboard-list text-primary"></i> Montagem da Ficha
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
                    <button onclick="window.addWorkout()" class="btn-primary px-4 py-2 rounded-lg text-sm font-medium flex items-center gap-2 w-full sm:w-auto justify-center">
                        <i class="fas fa-plus"></i> Adicionar Dia de Treino
                    </button>
                </div>

                <!-- Container de Treinos -->
                <div id="workouts-container" class="space-y-6">
                    <!-- Treinos JS -->
                </div>

                <!-- Recomendações Livres -->
                <div class="card rounded-xl p-5 shadow-sm">
                    <h2 class="text-md font-semibold mb-2"><i class="fas fa-pen text-primary"></i> Recomendações Livres (Opcional)</h2>
                    <textarea id="stu-recs" rows="3" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Hidratação, descanso, cuidados específicos..."></textarea>
                </div>

            </div>
        </div>
    </div>

    <!-- MODAL DE EXERCÍCIOS -->
    <div id="exercise-modal" class="fixed inset-0 bg-black bg-opacity-80 backdrop-blur-sm hidden z-[60] flex items-center justify-center p-2 sm:p-4">
        <div class="card w-full max-w-6xl rounded-xl shadow-2xl flex flex-col h-[90vh]">
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
    <div id="history-modal" class="fixed inset-0 bg-black bg-opacity-80 backdrop-blur-sm hidden z-[70] flex items-center justify-center p-2 sm:p-4">
        <div class="card w-full max-w-4xl rounded-xl shadow-2xl flex flex-col h-[85vh]">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold flex items-center gap-2"><i class="fas fa-cloud text-primary"></i> Suas Fichas na Nuvem</h3>
                <button onclick="window.closeHistory()" class="text-gray-400 hover:text-red-500 transition-colors text-2xl leading-none">&times;</button>
            </div>
            <div class="p-4 overflow-y-auto flex-1 space-y-3 relative" id="history-list">
                <div class="text-center py-10 opacity-60">Carregando fichas da nuvem...</div>
            </div>
        </div>
    </div>

    <!-- ÁREA DE IMPRESSÃO -->
    <div id="print-area"></div>

    <!-- SCRIPT TIPO MÓDULO (FIREBASE + LOGICA) -->
    <script type="module">
        // Importações do Firebase v11
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInWithPopup, GoogleAuthProvider, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, addDoc, getDocs, doc, deleteDoc, updateDoc, query, orderBy } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Configuração Firebase (Fornecida pelo Usuário)
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

        // Variáveis de Estado
        let currentUser = null;
        let globalState = {
            currentDocId: null, // ID do doc no Firestore
            workouts: [],
            activeModalWorkoutId: null,
            activeCategory: "🔥 PEITO"
        };

        // --- BANCO DE DADOS (TEXTOS E EXERCÍCIOS) ---
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
            "🟢 Saudável": "Manter treinos regulares de 3–5x por semana, combinando musculação e atividades cardiovasculares. A constância e boa alimentação contribuem para resultados.",
            "⚪ Sedentário": "Início gradual, com treinos leves e foco na adaptação do corpo. A prioridade é desenvolver constância, técnica e evitar excesso de carga.",
            "🟡 Sobrepeso": "A musculação associada ao cardio contribui para melhora do condicionamento. A progressão deve respeitar a individualidade.",
            "🔴 Obesidade": "Iniciar com exercícios de menor impacto e progressão gradual. Segurança, mobilidade e constância são prioridades.",
            "⚖️ Baixo peso": "A musculação auxilia no ganho de massa. O treino deve priorizar evolução progressiva e recuperação muscular.",
            "🍬 Diabetes": "Pessoas com diabetes devem manter acompanhamento médico. Em casos de tontura ou alteração de glicemia, o treino deve ser interrompido.",
            "❤️ Hipertensão": "Manter acompanhamento médico regular. Durante o treino, é importante evitar prender a respiração e controlar a intensidade.",
            "🔵 Hipotensão": "Evitar mudanças bruscas de posição. Manter boa hidratação e respeitar a intensidade adequada.",
            "💔 Problemas cardíacos": "Prática apenas com liberação e acompanhamento médico. Treino deve respeitar limites com controle de intensidade.",
            "🦴 Problemas articulares": "Exercícios com menor impacto e maior controle são indicados. O acompanhamento profissional ajuda na segurança.",
            "🫁 Problemas respiratórios": "Progressão gradual respeitando a capacidade individual. Em caso de falta de ar, o exercício deve ser interrompido.",
            "⚠️ Lesões": "A adaptação deve respeitar a limitação existente e evitar dor. Seguir orientação profissional antes da prática.",
            "🤰 Gestante": "Prática com liberação médica e acompanhamento adequado. Foco na segurança, mobilidade e bem-estar, sem risco.",
            "🤱 Lactante": "Atividade mantida normalmente, respeitando a recuperação, hidratação e alimentação.",
            "👴 Idoso": "Foco em força, equilíbrio, mobilidade e autonomia. Treino deve respeitar limitações, com segurança."
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
            "🔥 PEITO": [
                "Supino Reto", "Supino Inclinado", "Supino Inclinado com Halteres", "Supino Fechado com Halteres", 
                "Cross Over", "Cross Over Alto", "Cross Over Baixo", "Crucifixo Reto", "Crucifixo Inclinado com Halteres", 
                "Crucifixo na Máquina", "Peck Fly", "Peck Fly Unilateral", "Pullover", 
                "Flexão de Braço", "Flexão com Pés Elevados", "Flexão Explosiva"
            ],
            "🦍 COSTAS": [
                "Puxada Alta", "Puxada de Frente Supinada", "Pulldown", "Remada Aberta", "Remada Baixa", 
                "Remada Curvada", "Remada Curvada Supinada", "Remada Cavalinho (T-Bar)", "Remada Unilateral", "Serrote", 
                "Facepull (puxada de cima para baixo)", "Encolhimento (Trapézio)"
            ],
            "🦵 PERNAS": [
                "Agachamento Livre", "Agachamento Taça", "Agachamento com Barra", "Agachamento no Smith", "Agachamento com passada lateral", 
                "Squat", "Hack Machine", "Leg 45°", "Leg 90°", "Agachamento Sumô", "Agachamento Sissy (Livre)", 
                "Afundo", "Recuo", "Avanço", "Passada", "Búlgaro", "Step-up", "Levantamento Terra", "Levantamento Terra Romeno", 
                "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", "Cadeira Flexora", "Elevação Pélvica no Banco", 
                "Elevação Pélvica no Chão", "Elevação Pélvica Unilateral no Chão", "Extensão de Quadril (Glúteo Máximo)", 
                "Extensão Cruzada (Glúteo Médio)", "Coice", "Cachorrinho", "Cadeira Extensora", "Adução", "Abdução", 
                "Abdução Inclinada", "Flexão Nórdica", "Flexão Nórdica Invertida", "Panturrilha Livre", "Panturrilha no Leg Press", 
                "Panturrilha Banco", "Panturrilha Squat", "Panturrilha Unilateral"
            ],
            "💪 BRAÇOS": [
                "(Bíceps) Rosca Direta", "(Bíceps) Rosca Alternada", "(Bíceps) Rosca 21", "(Bíceps) Rosca Scott Barra W", 
                "(Bíceps) Rosca Scott Unilateral", "(Bíceps) Rosca Scott com Halteres", "(Bíceps) Rosca Martelo", "(Bíceps) Rosca Cross", 
                "(Bíceps) Rosca Concentrada", "(Bíceps) Rosca Inversa", "(Bíceps) Rosca 45°",
                "(Tríceps) Pulley Unilateral", "(Tríceps) Pulley Barra", "(Tríceps) Pulley Corda", "(Tríceps) Pulley Pegada Inversa", 
                "(Tríceps) Francês na Corda", "(Tríceps) Francês com Halter", "(Tríceps) Francês Unilateral", "(Tríceps) Cruzado Polia Dupla", 
                "(Tríceps) Coice Unilateral", "(Tríceps) Arremesso", "(Tríceps) Testa", "(Tríceps) Mergulho no Banco"
            ],
            "🪨 OMBROS": [
                "Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", "Elevação Lateral Unilateral Cross", 
                "Elevação Lateral Sentado", "Desenvolvimento com Halteres", "Desenvolvimento com Barra", "Arnold Press", 
                "Elevação Borboleta", "Crucifixo Inverso Sentado com Halteres", "Crucifixo Inverso na Polia", 
                "Crucifixo Inverso Unilateral na Polia", "Facepull (puxada reta)", "Remada Alta", "Encolhimento (Trapézio)"
            ],
            "🧠 ABDÔMEN": [
                "Infra com Elevação de Perna", "Abdominal Supra", "Abdominal Remador", "Abdominal Bicicleta", 
                "Abdominal Twister com Peso", "Prancha", "Prancha Lateral", "Trituração de Cabos em Pé", "Isometria na parede", "Abdominal isometrico"
            ],
            "🫀 CARDIO": [
                "Bicicleta 10 Minutos", "Bicicleta 15 Minutos", "Bicicleta 20 Minutos",
                "Esteira 10 Minutos", "Esteira 15 Minutos", "Esteira 20 Minutos",
                "Pular Corda"
            ]
        };

        const dbTechniques = [
            "Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", 
            "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", 
            "Negativa", "Isometria", "Parciais", "Pirâmide"
        ];

        const daysOfWeek = ["SEGUNDA-FEIRA", "TERÇA-FEIRA", "QUARTA-FEIRA", "QUINTA-FEIRA", "SEXTA-FEIRA", "SÁBADO", "DOMINGO"];

        // --- AUTENTICAÇÃO E INICIALIZAÇÃO ---
        onAuthStateChanged(auth, (user) => {
            currentUser = user;
            const loginBtn = document.getElementById('login-btn');
            const userInfo = document.getElementById('user-info');
            const appActions = document.getElementById('app-actions');
            const mainContent = document.getElementById('main-content');
            const loginWarning = document.getElementById('login-warning');

            if (user) {
                document.getElementById('user-name').innerText = user.displayName;
                document.getElementById('user-photo').src = user.photoURL;
                
                loginBtn.classList.add('hidden');
                userInfo.classList.remove('hidden');
                userInfo.classList.add('flex');
                appActions.classList.remove('hidden');
                appActions.classList.add('flex');
                
                mainContent.classList.remove('opacity-50', 'pointer-events-none');
                loginWarning.classList.add('hidden');
                
                // Pré-preenche o nome do profissional se estiver vazio
                if(!document.getElementById('prof-name').value) {
                    document.getElementById('prof-name').value = user.displayName;
                }
            } else {
                loginBtn.classList.remove('hidden');
                userInfo.classList.add('hidden');
                userInfo.classList.remove('flex');
                appActions.classList.add('hidden');
                appActions.classList.remove('flex');

                mainContent.classList.add('opacity-50', 'pointer-events-none');
                loginWarning.classList.remove('hidden');
            }
        });

        window.login = async () => {
            try { await signInWithPopup(auth, provider); } 
            catch (error) { alert("Erro ao fazer login. Tente novamente."); console.error(error); }
        };

        window.logout = async () => {
            try { await signOut(auth); resetForm(); } 
            catch (error) { console.error(error); }
        };

        // --- FUNÇÕES DE UI E CÁLCULOS ---
        window.toggleProfType = () => {
            const type = document.getElementById('prof-type').value;
            const crefContainer = document.getElementById('cref-container');
            
            if(type === 'TE') {
                crefContainer.classList.add('hidden');
                renderHealthOptions(healthDataTE);
            } else {
                crefContainer.classList.remove('hidden');
                renderHealthOptions(healthDataPEF);
            }
        };

        window.changeTheme = () => {
            const gender = document.getElementById('stu-gender').value;
            document.body.setAttribute('data-theme', gender);
        };

        window.calculateIMC = () => {
            const w = parseFloat(document.getElementById('stu-weight').value);
            const h = parseFloat(document.getElementById('stu-height').value);
            const display = document.getElementById('imc-display');

            if (w > 0 && h > 0) {
                const imc = (w / (h * h)).toFixed(1);
                let classif = imc < 18.5 ? "Abaixo" : imc < 24.9 ? "Normal" : imc < 29.9 ? "Sobrepeso" : "Obesidade";
                display.innerHTML = `<span class="bg-primary bg-opacity-20 text-primary px-3 py-1 rounded-full text-xs font-bold border border-primary border-opacity-30">IMC: ${imc} (${classif})</span>`;
            } else { display.innerHTML = ''; }
        };

        function renderHealthOptions(dataObj) {
            const container = document.getElementById('health-container');
            const selected = Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value);
            
            container.innerHTML = Object.keys(dataObj).map(opt => {
                const isChecked = selected.includes(opt) ? 'checked' : '';
                return `
                <label class="flex items-start space-x-2 cursor-pointer p-1.5 rounded hover:bg-black hover:bg-opacity-5 transition border border-transparent hover:border-gray-500 hover:border-opacity-20">
                    <input type="checkbox" value="${opt}" ${isChecked} class="health-cb mt-0.5 rounded border-gray-400 text-primary focus:ring-primary w-3.5 h-3.5">
                    <span class="font-medium">${opt}</span>
                </label>`
            }).join('');
        }

        // --- LÓGICA DE TREINOS ---
        function getNextDayName() {
            const currentCount = globalState.workouts.length;
            if(currentCount < 7) return `TREINO ${daysOfWeek[currentCount]}`;
            return `NOVO TREINO ${currentCount + 1}`;
        }

        window.addWorkout = () => {
            globalState.workouts.push({ id: Math.random().toString(36).substr(2, 9), title: getNextDayName(), exercises: [] });
            renderWorkouts();
        };

        window.duplicateWorkout = (id) => {
            const workout = globalState.workouts.find(w => w.id === id);
            if(workout) {
                const newWorkout = JSON.parse(JSON.stringify(workout));
                newWorkout.id = Math.random().toString(36).substr(2, 9);
                newWorkout.title = newWorkout.title + " (Cópia)";
                globalState.workouts.push(newWorkout);
                renderWorkouts();
            }
        };

        window.removeWorkout = (id) => {
            if(confirm("Excluir este dia de treino?")) {
                globalState.workouts = globalState.workouts.filter(w => w.id !== id);
                renderWorkouts();
            }
        };

        window.updateWorkoutTitle = (id, newTitle) => {
            const workout = globalState.workouts.find(w => w.id === id);
            if(workout) workout.title = newTitle;
        };

        // --- LÓGICA DE EXERCÍCIOS ---
        window.removeExercise = (workoutId, exIndex) => {
            const workout = globalState.workouts.find(w => w.id === workoutId);
            workout.exercises.splice(exIndex, 1);
            renderWorkouts();
        };

        window.moveExercise = (workoutId, exIndex, direction) => {
            const workout = globalState.workouts.find(w => w.id === workoutId);
            if (direction === 'up' && exIndex > 0) {
                [workout.exercises[exIndex], workout.exercises[exIndex - 1]] = [workout.exercises[exIndex - 1], workout.exercises[exIndex]];
            } else if (direction === 'down' && exIndex < workout.exercises.length - 1) {
                [workout.exercises[exIndex], workout.exercises[exIndex + 1]] = [workout.exercises[exIndex + 1], workout.exercises[exIndex]];
            }
            renderWorkouts();
        };

        window.updateExercise = (workoutId, exIndex, field, value) => {
            const workout = globalState.workouts.find(w => w.id === workoutId);
            if(workout && workout.exercises[exIndex]) { workout.exercises[exIndex][field] = value; }
        };

        function renderWorkouts() {
            const container = document.getElementById('workouts-container');
            container.innerHTML = '';

            globalState.workouts.forEach(workout => {
                let exercisesHtml = '';
                if (workout.exercises.length === 0) {
                    exercisesHtml = `<div class="text-center p-4 text-xs opacity-50 italic">Sem exercícios cadastrados.</div>`;
                } else {
                    exercisesHtml = workout.exercises.map((ex, idx) => `
                        <div class="flex flex-col sm:flex-row gap-2 p-2 items-start sm:items-center border-b last:border-0 border-opacity-10" style="border-color: var(--border-color)">
                            <div class="flex gap-1 hidden sm:flex">
                                <button onclick="window.moveExercise('${workout.id}', ${idx}, 'up')" class="text-[10px] p-1 rounded hover:bg-black hover:bg-opacity-10"><i class="fas fa-chevron-up"></i></button>
                                <button onclick="window.moveExercise('${workout.id}', ${idx}, 'down')" class="text-[10px] p-1 rounded hover:bg-black hover:bg-opacity-10"><i class="fas fa-chevron-down"></i></button>
                            </div>
                            <div class="flex-1 font-medium w-full sm:w-auto">
                                <div class="text-[9px] opacity-60 uppercase tracking-wider">${ex.category}</div>
                                <div class="text-xs">${ex.name}</div>
                            </div>
                            <div class="flex flex-wrap gap-1 w-full sm:w-auto">
                                <input type="text" value="${ex.sets}" onchange="window.updateExercise('${workout.id}', ${idx}, 'sets', this.value)" class="input-field w-12 rounded px-1 py-1 text-xs text-center" placeholder="Séries">
                                <span class="self-center opacity-50 text-xs">x</span>
                                <input type="text" value="${ex.reps}" onchange="window.updateExercise('${workout.id}', ${idx}, 'reps', this.value)" class="input-field w-16 rounded px-1 py-1 text-xs text-center" placeholder="Reps">
                                <select onchange="window.updateExercise('${workout.id}', ${idx}, 'technique', this.value)" class="input-field w-24 rounded px-1 py-1 text-[10px]">
                                    ${dbTechniques.map(t => `<option value="${t}" ${ex.technique === t ? 'selected' : ''}>${t}</option>`).join('')}
                                </select>
                                <input type="text" value="${ex.obs}" onchange="window.updateExercise('${workout.id}', ${idx}, 'obs', this.value)" class="input-field flex-1 sm:w-32 rounded px-1 py-1 text-xs" placeholder="Obs...">
                            </div>
                            <div class="flex items-center gap-1 w-full sm:w-auto justify-end mt-1 sm:mt-0">
                                <button onclick="window.removeExercise('${workout.id}', ${idx})" class="text-red-500 hover:text-red-700 p-1 rounded transition text-sm">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                    `).join('');
                }

                container.insertAdjacentHTML('beforeend', `
                    <div class="card rounded-xl overflow-hidden shadow-sm">
                        <div class="p-3 border-b flex justify-between items-center bg-black bg-opacity-5" style="border-color: var(--border-color);">
                            <input type="text" value="${workout.title}" onchange="window.updateWorkoutTitle('${workout.id}', this.value)" class="input-field bg-transparent font-bold text-sm w-1/2 px-2 py-1 rounded uppercase border-transparent focus:border-primary">
                            <div class="flex gap-1">
                                <button onclick="window.duplicateWorkout('${workout.id}')" class="text-xs p-1.5 rounded hover:bg-black hover:bg-opacity-10 transition" title="Duplicar">
                                    <i class="fas fa-copy"></i>
                                </button>
                                <button onclick="window.removeWorkout('${workout.id}')" class="text-xs p-1.5 text-red-500 rounded hover:bg-red-500 hover:text-white transition" title="Excluir">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                        <div class="p-0">${exercisesHtml}</div>
                        <div class="p-2 border-t" style="border-color: var(--border-color)">
                            <button onclick="window.openModal('${workout.id}')" class="w-full text-primary py-1.5 rounded-lg text-xs font-semibold hover:bg-primary hover:bg-opacity-10 transition">
                                + ADICIONAR EXERCÍCIO
                            </button>
                        </div>
                    </div>
                `);
            });
        }

        // --- MODAL DE EXERCÍCIOS ---
        window.openModal = (workoutId) => {
            globalState.activeModalWorkoutId = workoutId;
            document.getElementById('exercise-modal').classList.remove('hidden');
            window.renderModalCategories();
            window.renderModalExercises();
        };
        window.closeModal = () => { document.getElementById('exercise-modal').classList.add('hidden'); globalState.activeModalWorkoutId = null; };
        
        window.renderModalCategories = () => {
            document.getElementById('modal-categories').innerHTML = Object.keys(dbCategories).map(cat => `
                <button onclick="window.setModalCategory('${cat}')" class="w-full text-left px-2 py-2 rounded-lg text-xs font-medium transition ${globalState.activeCategory === cat ? 'bg-primary text-white' : 'hover:bg-black hover:bg-opacity-10'}">
                    ${cat}
                </button>
            `).join('');
        };
        
        window.setModalCategory = (cat) => {
            globalState.activeCategory = cat;
            window.renderModalCategories();
            window.renderModalExercises();
        };
        
        window.renderModalExercises = () => {
            const exercises = dbCategories[globalState.activeCategory];
            document.getElementById('modal-exercises').innerHTML = `
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-2">
                    ${exercises.map(ex => {
                        const isCardio = globalState.activeCategory === "🫀 CARDIO";
                        return `<button onclick="window.addExerciseToWorkout('${ex}', ${isCardio})" class="card p-2 rounded-lg text-left text-xs font-medium border hover:border-primary transition flex justify-between items-center group">
                            <span>${ex}</span><i class="fas fa-plus opacity-0 group-hover:opacity-100 transition text-primary"></i>
                        </button>`
                    }).join('')}
                </div>
            `;
        };
        
        window.addExerciseToWorkout = (exName, isCardio) => {
            const workout = globalState.workouts.find(w => w.id === globalState.activeModalWorkoutId);
            if(workout) {
                workout.exercises.push({ category: globalState.activeCategory, name: exName, sets: isCardio ? '1' : '3', reps: isCardio ? '-' : '10 a 12', technique: 'Nenhuma', obs: '' });
                renderWorkouts();
                const container = document.getElementById('modal-exercises');
                container.style.opacity = '0.5';
                setTimeout(() => container.style.opacity = '1', 100);
            }
        };

        // --- SISTEMA NUVEM (FIRESTORE) E IMPRESSÃO ---
        function getFormData() {
            return {
                timestamp: new Date().getTime(),
                dateString: new Date().toLocaleDateString('pt-BR') + ' ' + new Date().toLocaleTimeString('pt-BR', {hour: '2-digit', minute:'2-digit'}),
                studentName: document.getElementById('stu-name').value || 'Sem Nome',
                profType: document.getElementById('prof-type').value,
                profName: document.getElementById('prof-name').value,
                profCref: document.getElementById('prof-cref').value,
                profState: document.getElementById('prof-state').value,
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
                workouts: globalState.workouts
            };
        }

        function resetForm() {
            globalState.currentDocId = null;
            document.getElementById('stu-name').value = '';
            document.getElementById('stu-age').value = '';
            document.getElementById('stu-weight').value = '';
            document.getElementById('stu-height').value = '';
            document.getElementById('stu-recs').value = '';
            document.querySelectorAll('.health-cb').forEach(cb => cb.checked = false);
            globalState.workouts = [];
            window.addWorkout(); // Adiciona 1 treino vazio
            document.getElementById('imc-display').innerHTML = '';
        }

        window.saveAndPrint = async () => {
            if(!currentUser) { alert("Você precisa fazer login para salvar e imprimir."); return; }
            
            const btnText = document.getElementById('btn-text');
            const loader = document.getElementById('save-loader');
            btnText.style.opacity = "0"; loader.style.display = "block";

            try {
                const data = getFormData();
                const userRef = collection(db, 'users', currentUser.uid, 'fichas');
                
                if (globalState.currentDocId) {
                    await updateDoc(doc(db, 'users', currentUser.uid, 'fichas', globalState.currentDocId), data);
                } else {
                    const docRef = await addDoc(userRef, data);
                    globalState.currentDocId = docRef.id;
                }
                
                // Gera a impressão após o save ter sucesso
                buildPrintHTML(data);
                setTimeout(() => window.print(), 500);

            } catch (e) {
                console.error("Erro ao salvar no Firestore: ", e);
                alert("Erro ao salvar na nuvem. Verifique sua conexão.");
            } finally {
                btnText.style.opacity = "1"; loader.style.display = "none";
            }
        };

        window.openHistory = async () => {
            if(!currentUser) return;
            document.getElementById('history-modal').classList.remove('hidden');
            const list = document.getElementById('history-list');
            list.innerHTML = `<div class="text-center py-10 opacity-60"><div class="loader inline-block border-primary border-t-transparent w-6 h-6"></div> Buscando fichas...</div>`;
            
            try {
                const q = query(collection(db, 'users', currentUser.uid, 'fichas'), orderBy('timestamp', 'desc'));
                const querySnapshot = await getDocs(q);
                
                if(querySnapshot.empty) {
                    list.innerHTML = `<div class="text-center opacity-50 py-10">Nenhuma ficha salva na sua conta.</div>`;
                    return;
                }

                let html = '';
                querySnapshot.forEach((docSnap) => {
                    const h = docSnap.data();
                    html += `
                        <div class="card p-4 rounded-lg flex justify-between items-center border border-gray-500 border-opacity-20" id="hist-${docSnap.id}">
                            <div>
                                <div class="font-bold text-sm text-primary">${h.studentName}</div>
                                <div class="text-xs opacity-70">Salvo: ${h.dateString} | Obj: ${h.stuObjective}</div>
                            </div>
                            <div class="flex gap-2">
                                <button onclick="window.loadHistoryItem('${docSnap.id}')" class="bg-primary hover:bg-blue-600 text-white px-3 py-1.5 rounded text-xs font-semibold shadow">CARREGAR</button>
                                <button onclick="window.deleteHistoryItem('${docSnap.id}')" class="bg-red-500 hover:bg-red-600 text-white px-3 py-1.5 rounded text-xs font-semibold shadow"><i class="fas fa-trash"></i></button>
                            </div>
                        </div>`;
                });
                list.innerHTML = html;
            } catch (e) {
                console.error(e);
                list.innerHTML = `<div class="text-center text-red-500 py-10">Erro ao buscar histórico. Tente novamente.</div>`;
            }
        };

        window.closeHistory = () => { document.getElementById('history-modal').classList.add('hidden'); };

        window.loadHistoryItem = async (docId) => {
            try {
                const q = query(collection(db, 'users', currentUser.uid, 'fichas'));
                const querySnapshot = await getDocs(q);
                let item = null;
                querySnapshot.forEach(d => { if(d.id === docId) item = d.data(); });
                
                if(!item) return;

                globalState.currentDocId = docId;
                
                document.getElementById('prof-type').value = item.profType || 'PEF';
                window.toggleProfType(); 
                document.getElementById('prof-name').value = item.profName || '';
                document.getElementById('prof-cref').value = item.profCref || '';
                document.getElementById('prof-state').value = item.profState || '';
                document.getElementById('stu-name').value = item.studentName || '';
                document.getElementById('stu-age').value = item.stuAge || '';
                document.getElementById('stu-weight').value = item.stuWeight || '';
                document.getElementById('stu-height').value = item.stuHeight || '';
                document.getElementById('stu-gender').value = item.stuGender || 'Masculino';
                document.getElementById('stu-level').value = item.stuLevel || 'Iniciante';
                document.getElementById('stu-objective').value = item.stuObjective || 'Emagrecimento';
                document.getElementById('stu-freq').value = item.stuFreq || '3 dias';
                document.getElementById('stu-validity').value = item.stuValidity || 'Validade: 30 dias';
                document.getElementById('stu-recs').value = item.stuRecs || '';
                
                window.changeTheme();
                window.calculateIMC();

                document.querySelectorAll('.health-cb').forEach(cb => { cb.checked = (item.health || []).includes(cb.value); });
                globalState.workouts = item.workouts || [];
                renderWorkouts();
                window.closeHistory();
                
            } catch(e) { console.error(e); alert("Erro ao carregar ficha."); }
        };

        window.deleteHistoryItem = async (docId) => {
            if(confirm("Excluir definitivamente esta ficha da nuvem?")) {
                try {
                    await deleteDoc(doc(db, 'users', currentUser.uid, 'fichas', docId));
                    document.getElementById(`hist-${docId}`).remove();
                    if(globalState.currentDocId === docId) globalState.currentDocId = null;
                } catch(e) { console.error(e); alert("Erro ao excluir."); }
            }
        };

        // --- CONSTRUTOR DE IMPRESSÃO ---
        function buildPrintHTML(d) {
            let imcStr = "-";
            const w = parseFloat(d.stuWeight);
            const h = parseFloat(d.stuHeight);
            if(w>0 && h>0) imcStr = (w / (h * h)).toFixed(1);

            const objRecommendation = objectiveData[d.stuObjective] || "";
            const isPEF = d.profType === 'PEF';
            const healthSourceDict = isPEF ? healthDataPEF : healthDataTE;
            
            const profLabel = isPEF ? 'Profissional de Educação Física' : 'Treinador Esportivo';
            const crefText = isPEF ? `CREF: ${d.profCref} - Estado: ${d.profState}` : '';
            
            let legalTextPEF = `⚠️ OBSERVAÇÃO LEGAL – PROFISSIONAL DE EDUCAÇÃO FÍSICA: Conforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º estabelece que compete ao profissional coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos especializados.`;
            
            let legalTextTE = `⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO: Conforme a Lei nº 14.597/2023 (Lei Geral do Esporte), Art. 75, a profissão de treinador esportivo é reconhecida e regulada no Brasil. A atuação possui finalidade orientativa e não substitui avaliação médica, diagnóstico clínico ou acompanhamento de profissionais da saúde. Em casos de doenças, lesões, gestação, ou condição de saúde, recomenda-se avaliação prévia por profissional habilitado antes da prática. O treinamento proposto respeita os limites individuais, priorizando segurança, dentro da atuação permitida por lei.`;

            let html = `
                <div class="print-header">
                    <h1 class="print-title">Planilha de Treinamento - ${d.stuObjective}</h1>
                    <div class="prof-info">
                        Prescrição feita por: ${d.profName} (${profLabel})<br>
                        ${crefText}
                    </div>
                </div>

                <div class="print-grid">
                    <div><strong>Aluno:</strong> ${d.studentName}</div>
                    <div><strong>Idade:</strong> ${d.stuAge || '-'} anos</div>
                    <div><strong>Gênero:</strong> ${d.stuGender}</div>
                    <div><strong>Peso:</strong> ${d.stuWeight || '-'} kg</div>
                    <div><strong>Altura:</strong> ${d.stuHeight || '-'} m</div>
                    <div><strong>IMC:</strong> ${imcStr}</div>
                    <div><strong>Nível:</strong> ${d.stuLevel}</div>
                    <div><strong>Frequência:</strong> ${d.stuFreq}</div>
                    <div><strong>Validade:</strong> ${d.stuValidity.replace('Validade: ', '')}</div>
                </div>
            `;

            if (objRecommendation || d.health.length > 0) {
                html += `<div class="print-guidelines"><h4>Diretrizes Automáticas de Perfil</h4><ul>`;
                if(objRecommendation) html += `<li><strong>Objetivo (${d.stuObjective}):</strong> ${objRecommendation}</li>`;
                d.health.forEach(healthKey => {
                    if(healthSourceDict[healthKey]) {
                        const cleanKey = healthKey.replace(/[\u{1F300}-\u{1F9FF}\u{2600}-\u{26FF}\u{2700}-\u{27BF}\u{FE0F}]/gu, '').trim();
                        html += `<li><strong>Saúde (${cleanKey}):</strong> ${healthSourceDict[healthKey]}</li>`;
                    }
                });
                html += `</ul></div>`;
            }

            d.workouts.forEach(w => {
                html += `
                    <div class="print-workout">
                        <h3>${w.title}</h3>
                        <table>
                            <thead>
                                <tr>
                                    <th style="width: 35%">Exercício</th>
                                    <th style="width: 8%; text-align:center;">Séries</th>
                                    <th style="width: 12%; text-align:center;">Reps</th>
                                    <th style="width: 15%">Técnica</th>
                                    <th style="width: 30%">Observações</th>
                                </tr>
                            </thead>
                            <tbody>
                `;
                
                if (w.exercises.length === 0) { html += `<tr><td colspan="5" style="text-align:center; font-style:italic; padding:10px;">Sem exercícios</td></tr>`; } 
                else {
                    w.exercises.forEach(ex => {
                        html += `
                            <tr>
                                <td><strong>${ex.name}</strong></td>
                                <td style="text-align:center;">${ex.sets}</td>
                                <td style="text-align:center;">${ex.reps}</td>
                                <td>${ex.technique !== 'Nenhuma' ? ex.technique : '-'}</td>
                                <td>${ex.obs || '-'}</td>
                            </tr>`;
                    });
                }
                html += `</tbody></table></div>`;
            });

            html += `<div class="print-footer-section">`;
            if(d.stuRecs.trim()) {
                html += `<strong>Recomendações Manuais:</strong><br><p style="white-space: pre-wrap; margin: 3px 0 10px 0;">${d.stuRecs}</p>`;
            }
            html += `
                <div class="legal-text">${isPEF ? legalTextPEF : legalTextTE}</div>
                <div style="text-align:center; margin-top: 15px; font-size: 8px;">Ficha gerada em ${d.dateString} - PowFit Pro Cloud</div>
            </div>`;

            document.getElementById('print-area').innerHTML = html;
        }

        // Init App Defaults
        window.toggleProfType();
        window.addWorkout();

    </script>
</body>
</html>
