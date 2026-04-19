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

        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg-body);
            color: var(--text-main);
            transition: background-color 0.3s ease, color 0.3s ease;
        }

        .card { background-color: var(--bg-card); border: 1px solid var(--border-color); }
        .input-field { background-color: var(--input-bg); border: 1px solid var(--input-border); color: var(--text-main); }
        .input-field:focus { outline: none; border-color: var(--primary); box-shadow: 0 0 0 1px var(--primary); }
        .btn-primary { background-color: var(--primary); color: white; transition: background-color 0.2s; }
        .btn-primary:hover { background-color: var(--primary-hover); }

        /* Utilitários de Tela */
        .screen { display: none; }
        .screen.active { display: block; }

        /* ÁREA DE IMPRESSÃO (Oculta na tela) */
        #print-area { display: none; }

        @media print {
            body { background: white !important; color: black !important; margin: 0; padding: 0; }
            #app-screens, #exercise-modal { display: none !important; }
            #print-area { display: block !important; padding: 10mm 15mm; font-family: 'Arial', sans-serif; font-size: 10px; }
            @page { size: A4; margin: 0; }
            
            .print-header { border-bottom: 2px solid #000; padding-bottom: 8px; margin-bottom: 12px; }
            .print-title { font-size: 18px; font-weight: bold; margin: 0; text-align: center; text-transform: uppercase;}
            .prof-info { font-size: 11px; margin-top: 5px; text-align: center; font-weight: bold;}
            
            .print-grid { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 4px; margin-bottom: 12px; border: 1px solid #000; padding: 8px; background: #fff; }
            .print-grid div { font-size: 10.5px; }
            
            .print-guidelines { border: 1px solid #000; padding: 8px; margin-bottom: 12px; font-size: 9.5px; }
            .print-guidelines h4 { margin: 0 0 4px 0; font-size: 10.5px; text-transform: uppercase; background: #eee; padding: 3px; }
            .print-guidelines ul { margin: 0; padding-left: 15px; }
            .print-guidelines li { margin-bottom: 2px; }

            .print-workout { margin-bottom: 12px; page-break-inside: avoid; }
            .print-workout h3 { background: #e0e0e0; border: 1px solid #000; border-bottom: none; padding: 4px 8px; margin: 0; font-size: 11px; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            table { width: 100%; border-collapse: collapse; margin-bottom: 0; border: 1px solid #000; }
            th, td { border: 1px solid #000; padding: 4px 6px; text-align: left; font-size: 10px; }
            th { background-color: #f0f0f0; font-weight: bold; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            
            .print-footer-section { margin-top: 15px; border: 1px solid #000; padding: 8px; font-size: 9px; page-break-inside: avoid; }
            .legal-text { font-size: 8px; color: #333; text-align: justify; margin-top: 5px; font-style: italic; }
        }

        /* Scrollbar */
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
    </style>
</head>
<body data-theme="Masculino" class="min-h-screen pb-20">

    <div id="app-screens">
        <!-- TELA 1: LOGIN -->
        <div id="screen-login" class="screen flex items-center justify-center min-h-screen p-4">
            <div class="card p-8 rounded-2xl shadow-xl max-w-md w-full text-center">
                <i class="fas fa-dumbbell text-5xl text-primary mb-4"></i>
                <h1 class="text-3xl font-bold mb-2">PowFit Pro</h1>
                <p class="text-sm opacity-70 mb-8">Acesso à nuvem para Profissionais e Treinadores.</p>
                <button onclick="window.appLogin()" class="w-full btn-primary py-3 rounded-lg font-bold flex items-center justify-center gap-3 text-lg">
                    <i class="fab fa-google"></i> Entrar com Google
                </button>
            </div>
        </div>

        <!-- TELA 2: SETUP INICIAL (Rede/Franquia) -->
        <div id="screen-setup" class="screen max-w-3xl mx-auto p-4 sm:p-8 pt-12">
            <div class="card p-6 sm:p-10 rounded-2xl shadow-xl">
                <h2 class="text-2xl font-bold mb-2"><i class="fas fa-building text-primary"></i> Configuração da Conta</h2>
                <p class="text-sm opacity-70 mb-8">Defina os dados da sua Rede e Perfil Profissional para iniciar.</p>
                
                <div class="space-y-6">
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4 border-b border-opacity-20 pb-6" style="border-color: var(--border-color)">
                        <div class="col-span-full font-semibold text-primary">🏢 Estrutura da Rede</div>
                        <div>
                            <label class="block text-sm font-medium mb-1">Nome da Rede</label>
                            <input type="text" id="setup-rede" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: Power Fitness">
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-1">Qtd Total de Unidades</label>
                            <input type="number" id="setup-units" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: 5">
                        </div>
                        <div class="col-span-full">
                            <label class="block text-sm font-medium mb-1">Nome da Unidade Atual</label>
                            <input type="text" id="setup-unit-name" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: Unidade Centro">
                        </div>
                    </div>

                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div class="col-span-full font-semibold text-primary">👤 Dados do Profissional</div>
                        <div class="col-span-full">
                            <label class="block text-sm font-medium mb-1">Nome Completo</label>
                            <input type="text" id="setup-name" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-1">CPF</label>
                            <input type="text" id="setup-cpf" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="000.000.000-00">
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-1">Categoria de Atuação</label>
                            <select id="setup-role" onchange="window.toggleSetupRole()" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                <option value="PEF">Profissional de Educação Física</option>
                                <option value="TE">Treinador Esportivo</option>
                            </select>
                        </div>
                        <div id="setup-cref-container">
                            <label class="block text-sm font-medium mb-1">CREF</label>
                            <input type="text" id="setup-cref" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: 000000">
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-1">Estado (UF)</label>
                            <input type="text" id="setup-state" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: SP" maxlength="2">
                        </div>
                    </div>

                    <button onclick="window.saveSetup()" class="w-full btn-primary py-3 rounded-lg font-bold mt-4">Concluir Configuração</button>
                </div>
            </div>
        </div>

        <!-- TELA 3: DASHBOARD -->
        <div id="screen-dashboard" class="screen max-w-7xl mx-auto p-4 sm:p-6 lg:p-8">
            <div class="flex justify-between items-center mb-8">
                <div>
                    <h1 class="text-2xl font-bold flex items-center gap-2"><i class="fas fa-dumbbell text-primary"></i> PowFit Pro</h1>
                    <p class="text-sm opacity-70" id="dash-subtitle">Rede X - Unidade Y</p>
                </div>
                <button onclick="window.appLogout()" class="bg-red-500 hover:bg-red-600 text-white px-4 py-2 rounded-lg text-sm font-medium transition">
                    <i class="fas fa-sign-out-alt"></i> Sair
                </button>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
                <div class="card p-6 rounded-xl flex items-center justify-between shadow">
                    <div>
                        <p class="text-sm opacity-70">Fichas no Mês (Unidade)</p>
                        <h3 class="text-3xl font-bold text-primary" id="dash-month-count">0</h3>
                    </div>
                    <i class="fas fa-file-invoice text-4xl opacity-20"></i>
                </div>
                <div class="card p-6 rounded-xl flex items-center justify-between shadow">
                    <div>
                        <p class="text-sm opacity-70">Total de Fichas (Sua Conta)</p>
                        <h3 class="text-3xl font-bold" id="dash-total-count">0</h3>
                    </div>
                    <i class="fas fa-database text-4xl opacity-20"></i>
                </div>
                <button onclick="window.openBuilder(null)" class="btn-primary rounded-xl p-6 shadow-lg flex flex-col items-center justify-center gap-2 hover:scale-[1.02] transition transform">
                    <i class="fas fa-plus-circle text-3xl"></i>
                    <span class="font-bold">Criar Nova Ficha</span>
                </button>
            </div>

            <h2 class="text-xl font-bold mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">Histórico de Fichas (Nuvem)</h2>
            <div class="grid grid-cols-1 gap-3" id="dash-history-list">
                <div class="text-center opacity-50 py-10">Carregando dados da nuvem...</div>
            </div>
        </div>

        <!-- TELA 4: BUILDER (Montagem da Ficha) -->
        <div id="screen-builder" class="screen max-w-7xl mx-auto p-4 sm:p-6 lg:p-8">
            
            <div class="flex flex-col sm:flex-row justify-between items-center mb-6 gap-4 border-b border-opacity-20 pb-4" style="border-color: var(--border-color)">
                <button onclick="window.showScreen('screen-dashboard')" class="text-sm hover:text-primary transition flex items-center gap-2">
                    <i class="fas fa-arrow-left"></i> Voltar ao Dashboard
                </button>
                <button onclick="window.saveAndPrint()" class="btn-primary px-6 py-2.5 rounded-lg font-medium shadow flex items-center gap-2">
                    <i class="fas fa-cloud-upload-alt"></i> Salvar na Nuvem & Imprimir
                </button>
            </div>

            <div class="grid grid-cols-1 xl:grid-cols-12 gap-6">
                <!-- Coluna Esquerda: Aluno e Saúde -->
                <div class="xl:col-span-4 space-y-6">
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
                                    <input type="number" id="stu-age" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                </div>
                                <div>
                                    <label class="block text-sm font-medium mb-1">Peso (kg)</label>
                                    <input type="number" id="stu-weight" oninput="window.calcIMC()" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                </div>
                                <div>
                                    <label class="block text-sm font-medium mb-1">Altura (m)</label>
                                    <input type="number" id="stu-height" step="0.01" oninput="window.calcIMC()" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                </div>
                            </div>
                            <div class="grid grid-cols-2 gap-3">
                                <div>
                                    <label class="block text-sm font-medium mb-1">Gênero</label>
                                    <select id="stu-gender" onchange="window.updateTheme()" class="input-field w-full rounded-lg px-3 py-2 text-sm">
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

                    <div class="card rounded-xl p-5 shadow-sm">
                        <h2 class="text-lg font-semibold mb-2 flex items-center gap-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                            <i class="fas fa-notes-medical text-primary"></i> Estado de Saúde
                        </h2>
                        <div class="grid grid-cols-2 gap-2 text-xs" id="health-container"></div>
                    </div>
                </div>

                <!-- Coluna Direita: Treinos -->
                <div class="xl:col-span-8 space-y-6">
                    <div class="flex justify-between items-center bg-opacity-10 p-4 rounded-xl card border-dashed border-2 gap-4">
                        <div>
                            <h2 class="text-xl font-bold flex items-center gap-2"><i class="fas fa-clipboard-list text-primary"></i> Montagem Livre</h2>
                            <div class="mt-2">
                                <select id="stu-validity" class="input-field rounded px-2 py-1 text-xs">
                                    <option value="15">Validade: 15 dias</option>
                                    <option value="30" selected>Validade: 30 dias</option>
                                    <option value="60">Validade: 60 dias</option>
                                    <option value="90">Validade: 90 dias</option>
                                </select>
                            </div>
                        </div>
                        <button onclick="window.addWorkoutDay()" class="btn-primary px-4 py-2 rounded-lg text-sm font-medium">
                            <i class="fas fa-plus"></i> Novo Dia
                        </button>
                    </div>

                    <div id="workouts-container" class="space-y-4"></div>

                    <div class="card rounded-xl p-5 shadow-sm">
                        <h2 class="text-md font-semibold mb-2"><i class="fas fa-pen text-primary"></i> Recomendações Livres</h2>
                        <textarea id="stu-recs" rows="3" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Hidratação, descanso..."></textarea>
                    </div>
                </div>
            </div>
        </div>

        <!-- MODAL DE EXERCÍCIOS -->
        <div id="exercise-modal" class="fixed inset-0 bg-black bg-opacity-70 backdrop-blur-sm hidden z-[60] flex items-center justify-center p-2 sm:p-4">
            <div class="card w-full max-w-5xl rounded-xl shadow-2xl flex flex-col h-[90vh]">
                <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                    <h3 class="text-lg font-bold"><i class="fas fa-search text-primary mr-2"></i> Adicionar Exercício</h3>
                    <button onclick="window.closeModal()" class="text-gray-400 hover:text-red-500 text-2xl leading-none">&times;</button>
                </div>
                <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                    <div class="w-full md:w-1/4 border-r overflow-y-auto p-2 space-y-1" style="border-color: var(--border-color)" id="modal-categories"></div>
                    <div class="w-full md:w-3/4 overflow-y-auto p-3 bg-black bg-opacity-5" id="modal-exercises"></div>
                </div>
            </div>
        </div>
    </div>

    <!-- ÁREA DE IMPRESSÃO -->
    <div id="print-area"></div>

    <!-- SCRIPT PRINCIPAL (MÓDULO ES6 - FIREBASE) -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-app.js";
        import { getAuth, signInWithPopup, GoogleAuthProvider, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, addDoc, query, where, getDocs, orderBy, deleteDoc } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-firestore.js";

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

        // --- DADOS DA APLICAÇÃO ---
        const healthPEF = {
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

        const healthTE = {
            "🟢 Saudável": "Manter treinos regulares de 3–5x por semana, combinando musculação e atividades cardiovasculares. A constância, descanso adequado e boa alimentação contribuem para melhores resultados.",
            "⚪ Sedentário": "O início deve ser gradual, com treinos leves e foco na adaptação do corpo. A prioridade é desenvolver constância, aprender a execução correta e evitar excesso de carga.",
            "🟡 Sobrepeso": "A prática regular de musculação associada ao cardio pode contribuir para melhora do condicionamento físico e composição corporal. A progressão deve respeitar a individualidade e o nível de adaptação.",
            "🔴 Obesidade": "É recomendado iniciar com exercícios de menor impacto e progressão gradual. A segurança, mobilidade e constância são prioridades durante o processo.",
            "⚖️ Baixo peso": "A musculação pode auxiliar no ganho de massa muscular quando associada a alimentação adequada e descanso. O treino deve priorizar evolução progressiva e recuperação muscular.",
            "🍬 Diabetes": "Pessoas com diabetes devem manter acompanhamento médico regular antes e durante a prática de exercícios. A musculação pode auxiliar na rotina de atividade física quando liberada por profissional de saúde. Em casos de tontura, mal-estar ou alteração de glicemia, o treino deve ser interrompido e o aluno deve procurar orientação médica.",
            "❤️ Hipertensão": "Pessoas com hipertensão devem manter acompanhamento médico regular e respeitar as orientações profissionais. Durante o treino, é importante evitar prender a respiração e controlar a intensidade dos exercícios.",
            "🔵 Hipotensão": "Pessoas com hipotensão devem evitar mudanças bruscas de posição durante o treino. Manter boa hidratação e respeitar a intensidade adequada ajuda a reduzir episódios de mal-estar.",
            "💔 Problemas cardíacos": "A prática de exercícios deve ocorrer apenas com liberação e acompanhamento médico. O treino deve respeitar limites individuais, com controle de intensidade e atenção aos sinais do corpo.",
            "🦴 Problemas articulares": "Exercícios com menor impacto e maior controle de movimento costumam ser mais indicados. O acompanhamento profissional e a adaptação dos exercícios ajudam na segurança durante o treino.",
            "🫁 Problemas respiratórios": "A progressão do treino deve ser gradual e respeitar a capacidade respiratória individual. Em caso de falta de ar excessiva ou desconforto, o exercício deve ser interrompido.",
            "⚠️ Lesões": "A adaptação dos exercícios deve respeitar a limitação existente e evitar dor durante a execução. Em situações de lesão, é importante seguir orientação profissional adequada antes da prática.",
            "🤰 Gestante": "A prática de exercícios deve ocorrer com liberação médica e acompanhamento adequado. O foco deve ser segurança, mobilidade e bem-estar, evitando impacto excessivo e situações de risco.",
            "🤱 Lactante": "A atividade física pode ser mantida normalmente, respeitando a recuperação individual e mantendo boa hidratação e alimentação adequada.",
            "👴 Idoso": "A musculação pode contribuir para força, equilíbrio, mobilidade e autonomia. O treino deve respeitar limitações individuais, com intensidade moderada e foco na segurança."
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
            "🦍 COSTAS": ["Puxada Alta", "Puxada de Frente Supinada", "Pulldown", "Remada Aberta", "Remada Baixa", "Remada Curvada", "Remada Curvada Supinada", "Remada Cavalinho (T-Bar)", "Serrote", "Facepull (puxada de cima para baixo)"],
            "🦵 PERNAS": ["Agachamento Livre", "Agachamento Taça", "Agachamento no Smith", "Agachamento com passada lateral", "Squat", "Hack Machine", "Leg 45°", "Leg 90°", "Agachamento Sumô", "Agachamento Sissy (Livre)", "Afundo", "Recuo", "Avanço", "Passada", "Búlgaro", "Step-up", "Levantamento Terra", "Levantamento Terra Romeno", "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", "Cadeira Flexora", "Elevação Pélvica no Banco", "Elevação Pélvica no Chão", "Elevação Pélvica Unilateral no Chão", "Extensão de Quadril (Glúteo Máximo)", "Extensão Cruzada (Glúteo Médio)", "Coice", "Cachorrinho", "Cadeira Extensora", "Adução", "Abdução", "Abdução Inclinada", "Flexão Nórdica", "Flexão Nórdica Invertida", "Panturrilha Livre", "Panturrilha no Leg Press", "Panturrilha Banco", "Panturrilha Squat", "Panturrilha Unilateral"],
            "💪 BRAÇOS (Bíceps)": ["Rosca Direta", "Rosca Alternada", "Rosca 21", "Rosca Scott Barra W", "Rosca Scott Unilateral", "Rosca Scott com Halteres", "Rosca Martelo", "Rosca Cross", "Rosca Inversa", "Rosca 45°"],
            "💪 BRAÇOS (Tríceps)": ["Triceps Pulley Unilateral", "Tríceps Pulley Barra", "Tríceps Pulley Corda", "Tríceps Pulley Pegada Inversa", "Tríceps Francês na Corda", "Tríceps Francês com Halter", "Tríceps Francês Unilateral", "Tríceps Cruzado Polia Dupla", "Tríceps Coice Unilateral", "Tríceps Arremesso", "Tríceps Testa", "Mergulho no Banco"],
            "🪨 OMBROS": ["Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", "Elevação Lateral Unilateral Cross", "Elevação Lateral Sentado", "Desenvolvimento com Halteres", "Desenvolvimento com Barra", "Arnold Press", "Elevação Borboleta", "Crucifixo Inverso Sentado com Halteres", "Crucifixo Inverso na Polia", "Crucifixo Inverso Unilateral na Polia", "Facepull (puxada reta)", "Remada Alta", "Encolhimento (Trapézio)"],
            "🧠 ABDÔMEN": ["Infra com Elevação de Perna", "Abdominal Supra", "Abdominal Remador", "Abdominal Bicicleta", "Abdominal Twister com Peso", "Prancha", "Prancha Lateral", "Trituração de Cabos em Pé", "Isometria na parede"],
            "🫀 CARDIO": ["Bicicleta 10 Minutos", "Bicicleta 15 Minutos", "Bicicleta 20 Minutos", "Esteira 10 Minutos", "Esteira 15 Minutos", "Esteira 20 Minutos", "Pular Corda"]
        };

        const dbTechniques = ["Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", "Negativa", "Isometria", "Parciais", "Pirâmide"];
        const daysOfWeek = ["SEGUNDA-FEIRA", "TERÇA-FEIRA", "QUARTA-FEIRA", "QUINTA-FEIRA", "SEXTA-FEIRA", "SÁBADO", "DOMINGO"];

        // --- STATE ---
        let appState = {
            user: null,
            userData: null,
            editingDocId: null, // Para edição
            currentWorkouts: [],
            activeModalDayId: null,
            activeCategory: "🔥 PEITO"
        };

        // --- GLOBAL FUNCTIONS EXPOSED TO HTML ---
        window.showScreen = (screenId) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
            document.getElementById(screenId).classList.add('active');
        };

        window.appLogin = () => { signInWithPopup(auth, provider); };
        window.appLogout = () => { signOut(auth); };

        window.toggleSetupRole = () => {
            const role = document.getElementById('setup-role').value;
            document.getElementById('setup-cref-container').style.display = role === 'TE' ? 'none' : 'block';
        };

        window.saveSetup = async () => {
            if (!appState.user) return;
            const data = {
                redeName: document.getElementById('setup-rede').value || 'Rede Padrão',
                totalUnits: document.getElementById('setup-units').value || '1',
                unitName: document.getElementById('setup-unit-name').value || 'Unidade Principal',
                name: document.getElementById('setup-name').value,
                cpf: document.getElementById('setup-cpf').value,
                role: document.getElementById('setup-role').value,
                cref: document.getElementById('setup-cref').value,
                state: document.getElementById('setup-state').value,
            };
            await setDoc(doc(db, "users", appState.user.uid), data);
            appState.userData = data;
            loadDashboard();
        };

        window.updateTheme = () => {
            const g = document.getElementById('stu-gender').value;
            document.body.setAttribute('data-theme', g);
        };

        window.calcIMC = () => {
            const w = parseFloat(document.getElementById('stu-weight').value);
            const h = parseFloat(document.getElementById('stu-height').value);
            const display = document.getElementById('imc-display');
            if (w > 0 && h > 0) {
                const imc = (w / (h * h)).toFixed(1);
                let cls = imc < 18.5 ? "Abaixo" : imc < 24.9 ? "Normal" : imc < 29.9 ? "Sobrepeso" : "Obesidade";
                display.innerHTML = `<span class="bg-primary bg-opacity-20 text-primary px-3 py-1 rounded-full text-xs font-bold border border-primary border-opacity-30">IMC: ${imc} (${cls})</span>`;
            } else display.innerHTML = '';
        };

        const renderHealthCheckboxes = () => {
            const dict = appState.userData?.role === 'TE' ? healthTE : healthPEF;
            const container = document.getElementById('health-container');
            container.innerHTML = Object.keys(dict).map(opt => `
                <label class="flex items-start space-x-2 cursor-pointer p-1 rounded hover:bg-black hover:bg-opacity-5">
                    <input type="checkbox" value="${opt}" class="health-cb mt-0.5 rounded border-gray-400 text-primary focus:ring-primary w-3.5 h-3.5">
                    <span class="font-medium">${opt}</span>
                </label>`).join('');
        };

        window.openBuilder = (docId = null) => {
            appState.editingDocId = docId;
            appState.currentWorkouts = [];
            document.querySelectorAll('.health-cb').forEach(cb => cb.checked = false);
            
            if (docId === null) {
                // Nova Ficha Limpa
                document.getElementById('stu-name').value = '';
                document.getElementById('stu-age').value = '';
                document.getElementById('stu-weight').value = '';
                document.getElementById('stu-height').value = '';
                document.getElementById('stu-recs').value = '';
                window.calcIMC();
                window.addWorkoutDay(); // Add pelo menos a Segunda
            } else {
                // Lógica de carregar do Firestore será chamada aqui (veja abaixo em loadHistoryItem)
            }
            window.showScreen('screen-builder');
        };

        window.addWorkoutDay = () => {
            const count = appState.currentWorkouts.length;
            const title = count < 7 ? `TREINO ${daysOfWeek[count]}` : `NOVO TREINO ${count + 1}`;
            appState.currentWorkouts.push({ id: Math.random().toString(36).substr(2, 9), title, exercises: [] });
            renderWorkoutsUI();
        };

        window.removeWorkoutDay = (id) => {
            if(confirm('Remover este dia?')) {
                appState.currentWorkouts = appState.currentWorkouts.filter(w => w.id !== id);
                renderWorkoutsUI();
            }
        };
        
        window.updateWorkoutTitle = (id, val) => {
            const w = appState.currentWorkouts.find(w => w.id === id);
            if(w) w.title = val;
        };

        window.removeExercise = (wId, idx) => {
            appState.currentWorkouts.find(w => w.id === wId).exercises.splice(idx, 1);
            renderWorkoutsUI();
        };

        window.moveExercise = (wId, idx, dir) => {
            const exList = appState.currentWorkouts.find(w => w.id === wId).exercises;
            if (dir === 'up' && idx > 0) {
                [exList[idx], exList[idx-1]] = [exList[idx-1], exList[idx]];
            } else if (dir === 'down' && idx < exList.length - 1) {
                [exList[idx], exList[idx+1]] = [exList[idx+1], exList[idx]];
            }
            renderWorkoutsUI();
        };

        window.updateExercise = (wId, idx, field, val) => {
            appState.currentWorkouts.find(w => w.id === wId).exercises[idx][field] = val;
        };

        const renderWorkoutsUI = () => {
            const container = document.getElementById('workouts-container');
            container.innerHTML = appState.currentWorkouts.map(w => {
                const exHtml = w.exercises.length === 0 ? `<div class="p-3 text-center text-xs opacity-50">Sem exercícios</div>` : w.exercises.map((ex, idx) => `
                    <div class="flex flex-col sm:flex-row gap-2 p-2 items-start sm:items-center border-b border-opacity-10" style="border-color: var(--border-color)">
                        <div class="flex gap-1 hidden sm:flex">
                            <button onclick="window.moveExercise('${w.id}', ${idx}, 'up')" class="p-1 hover:bg-black hover:bg-opacity-10 rounded text-[10px]"><i class="fas fa-chevron-up"></i></button>
                            <button onclick="window.moveExercise('${w.id}', ${idx}, 'down')" class="p-1 hover:bg-black hover:bg-opacity-10 rounded text-[10px]"><i class="fas fa-chevron-down"></i></button>
                        </div>
                        <div class="flex-1 font-medium w-full sm:w-auto">
                            <div class="text-[9px] opacity-60 uppercase">${ex.category}</div>
                            <div class="text-xs">${ex.name}</div>
                        </div>
                        <div class="flex flex-wrap gap-1 w-full sm:w-auto">
                            <input type="text" value="${ex.sets}" onchange="window.updateExercise('${w.id}', ${idx}, 'sets', this.value)" class="input-field w-12 rounded px-1 py-1 text-xs text-center" placeholder="Séries">
                            <span class="opacity-50 text-xs self-center">x</span>
                            <input type="text" value="${ex.reps}" onchange="window.updateExercise('${w.id}', ${idx}, 'reps', this.value)" class="input-field w-16 rounded px-1 py-1 text-xs text-center" placeholder="Reps">
                            <select onchange="window.updateExercise('${w.id}', ${idx}, 'technique', this.value)" class="input-field w-24 rounded px-1 py-1 text-[10px]">
                                ${dbTechniques.map(t => `<option value="${t}" ${ex.technique === t ? 'selected' : ''}>${t}</option>`).join('')}
                            </select>
                            <input type="text" value="${ex.obs}" onchange="window.updateExercise('${w.id}', ${idx}, 'obs', this.value)" class="input-field flex-1 sm:w-32 rounded px-1 py-1 text-xs" placeholder="Obs...">
                        </div>
                        <button onclick="window.removeExercise('${w.id}', ${idx})" class="text-red-500 p-1"><i class="fas fa-trash text-sm"></i></button>
                    </div>
                `).join('');

                return `
                <div class="card rounded-xl overflow-hidden shadow-sm">
                    <div class="p-3 border-b flex justify-between bg-black bg-opacity-5" style="border-color: var(--border-color)">
                        <input type="text" value="${w.title}" onchange="window.updateWorkoutTitle('${w.id}', this.value)" class="input-field bg-transparent font-bold text-sm w-1/2 uppercase">
                        <button onclick="window.removeWorkoutDay('${w.id}')" class="text-red-500 text-xs p-1"><i class="fas fa-trash"></i></button>
                    </div>
                    <div>${exHtml}</div>
                    <div class="p-2 border-t" style="border-color: var(--border-color)">
                        <button onclick="window.openExModal('${w.id}')" class="w-full text-primary py-1 text-xs font-bold hover:bg-primary hover:bg-opacity-10 rounded">
                            + ADICIONAR EXERCÍCIO
                        </button>
                    </div>
                </div>`;
            }).join('');
        };

        window.openExModal = (wId) => {
            appState.activeModalDayId = wId;
            document.getElementById('exercise-modal').classList.remove('hidden');
            renderCategories();
            renderExercisesList();
        };
        window.closeModal = () => document.getElementById('exercise-modal').classList.add('hidden');

        const renderCategories = () => {
            document.getElementById('modal-categories').innerHTML = Object.keys(dbCategories).map(cat => `
                <button onclick="window.setCat('${cat}')" class="w-full text-left px-2 py-2 rounded-lg text-xs font-medium ${appState.activeCategory === cat ? 'bg-primary text-white' : 'hover:bg-black hover:bg-opacity-10'}">${cat}</button>
            `).join('');
        };
        window.setCat = (cat) => { appState.activeCategory = cat; renderCategories(); renderExercisesList(); };
        
        const renderExercisesList = () => {
            const isCardio = appState.activeCategory === "🫀 CARDIO";
            document.getElementById('modal-exercises').innerHTML = `
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-2">
                    ${dbCategories[appState.activeCategory].map(ex => `
                        <button onclick="window.addEx('${ex}', ${isCardio})" class="card p-2 rounded-lg text-left text-xs font-medium border hover:border-primary flex justify-between group">
                            <span>${ex}</span><i class="fas fa-plus opacity-0 group-hover:opacity-100 text-primary"></i>
                        </button>
                    `).join('')}
                </div>`;
        };
        window.addEx = (name, isCardio) => {
            appState.currentWorkouts.find(w => w.id === appState.activeModalDayId).exercises.push({
                category: appState.activeCategory, name, sets: isCardio ? '1' : '3', reps: isCardio ? '-' : '10 a 12', technique: 'Nenhuma', obs: ''
            });
            renderWorkoutsUI();
        };

        // --- CLOUD LOGIC (DASHBOARD & SAVE) ---
        const loadDashboard = async () => {
            document.getElementById('dash-subtitle').innerText = `${appState.userData.redeName} - ${appState.userData.unitName}`;
            renderHealthCheckboxes();
            window.showScreen('screen-dashboard');

            // Fetch History from cloud
            const q = query(collection(db, `users/${appState.user.uid}/workouts`), orderBy("timestamp", "desc"));
            const snaps = await getDocs(q);
            
            let total = snaps.size;
            let monthCount = 0;
            const currentMonth = new Date().getMonth();

            const historyHtml = snaps.docs.map(docSnap => {
                const data = docSnap.data();
                const dDate = data.timestamp ? new Date(data.timestamp) : new Date();
                if(dDate.getMonth() === currentMonth) monthCount++;

                // Lógica de Selo Vencido
                const creationTime = dDate.getTime();
                const validityDays = parseInt(data.validity || '30');
                const isExpired = (new Date().getTime() - creationTime) > (validityDays * 24 * 60 * 60 * 1000);
                const statusBadge = isExpired 
                    ? `<span class="bg-red-500 text-white text-[10px] px-2 py-0.5 rounded ml-2">VENCIDA</span>` 
                    : `<span class="bg-green-500 text-white text-[10px] px-2 py-0.5 rounded ml-2">ATIVA</span>`;

                return `
                <div class="card p-4 rounded-xl flex justify-between items-center shadow-sm">
                    <div>
                        <div class="font-bold text-sm">${data.studentName} ${statusBadge}</div>
                        <div class="text-[11px] opacity-70">Criado em: ${dDate.toLocaleDateString()} | Objetivo: ${data.objective} | ${data.validity} dias</div>
                    </div>
                    <div class="flex gap-2">
                        <button onclick="window.loadFromCloud('${docSnap.id}')" class="btn-primary px-3 py-1.5 rounded text-xs font-semibold">CARREGAR</button>
                        <button onclick="window.deleteFromCloud('${docSnap.id}')" class="bg-red-500 hover:bg-red-600 text-white px-3 py-1.5 rounded text-xs font-semibold"><i class="fas fa-trash"></i></button>
                    </div>
                </div>`;
            }).join('');

            document.getElementById('dash-total-count').innerText = total;
            document.getElementById('dash-month-count').innerText = monthCount;
            document.getElementById('dash-history-list').innerHTML = total > 0 ? historyHtml : `<div class="text-center opacity-50 py-10">Nenhuma ficha salva na nuvem.</div>`;
        };

        window.loadFromCloud = async (docId) => {
            const docSnap = await getDoc(doc(db, `users/${appState.user.uid}/workouts`, docId));
            if(docSnap.exists()) {
                const d = docSnap.data();
                appState.editingDocId = docId;
                
                document.getElementById('stu-name').value = d.studentName || '';
                document.getElementById('stu-age').value = d.age || '';
                document.getElementById('stu-weight').value = d.weight || '';
                document.getElementById('stu-height').value = d.height || '';
                document.getElementById('stu-gender').value = d.gender || 'Masculino';
                document.getElementById('stu-level').value = d.level || 'Iniciante';
                document.getElementById('stu-objective').value = d.objective || 'Emagrecimento';
                document.getElementById('stu-freq').value = d.freq || '3 dias';
                document.getElementById('stu-validity').value = d.validity || '30';
                document.getElementById('stu-recs').value = d.recs || '';
                
                window.updateTheme();
                window.calcIMC();

                document.querySelectorAll('.health-cb').forEach(cb => { cb.checked = (d.health || []).includes(cb.value); });
                
                appState.currentWorkouts = d.workouts || [];
                renderWorkoutsUI();
                window.showScreen('screen-builder');
            }
        };

        window.deleteFromCloud = async (docId) => {
            if(confirm('Excluir esta ficha permanentemente da nuvem?')) {
                await deleteDoc(doc(db, `users/${appState.user.uid}/workouts`, docId));
                loadDashboard();
            }
        };

        window.saveAndPrint = async () => {
            const formData = {
                studentName: document.getElementById('stu-name').value || 'Sem Nome',
                age: document.getElementById('stu-age').value,
                weight: document.getElementById('stu-weight').value,
                height: document.getElementById('stu-height').value,
                gender: document.getElementById('stu-gender').value,
                level: document.getElementById('stu-level').value,
                objective: document.getElementById('stu-objective').value,
                freq: document.getElementById('stu-freq').value,
                validity: document.getElementById('stu-validity').value,
                recs: document.getElementById('stu-recs').value,
                health: Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value),
                workouts: appState.currentWorkouts,
                timestamp: Date.now() // For sorting in cloud
            };

            // Save to Firestore
            const collectionRef = collection(db, `users/${appState.user.uid}/workouts`);
            if (appState.editingDocId) {
                await setDoc(doc(db, `users/${appState.user.uid}/workouts`, appState.editingDocId), formData);
            } else {
                const newDoc = await addDoc(collectionRef, formData);
                appState.editingDocId = newDoc.id; // Atualiza ID para não duplicar se imprimir dnv
            }

            // --- BUILD PRINT HTML ---
            const isPEF = appState.userData.role === 'PEF';
            const healthSourceDict = isPEF ? healthPEF : healthTE;
            const w = parseFloat(formData.weight);
            const h = parseFloat(formData.height);
            const imcStr = (w>0 && h>0) ? (w / (h*h)).toFixed(1) : "-";
            const objRec = objectiveData[formData.objective] || "";

            let legalText = isPEF 
                ? `⚠️ OBSERVAÇÃO LEGAL – PROFISSIONAL DE EDUCAÇÃO FÍSICA: Conforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º estabelece que compete ao profissional coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos especializados nas áreas de atividades físicas e do desporto.`
                : `⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO: Conforme a Lei nº 14.597/2023 (Lei Geral do Esporte), Art. 75, a profissão de treinador esportivo é reconhecida e regulada no Brasil, com atuação de caráter técnico e esportivo. A atuação possui finalidade orientativa e não substitui avaliação médica. Em casos de doenças, lesões, gestação, ou condição de saúde, recomenda-se avaliação médica antes do início da prática. O treinamento proposto respeita limites individuais. As recomendações possuem caráter informativo voltadas ao treinamento esportivo.`;

            let html = `
                <div class="print-header">
                    <h1 class="print-title">PLANILHA DE TREINAMENTO - ${formData.objective}</h1>
                    <div class="prof-info">
                        Prescrição feita por: ${appState.userData.name} (${isPEF ? 'Profissional de Educação Física' : 'Treinador Esportivo'})
                        ${isPEF ? `<br>CREF: ${appState.userData.cref} - ${appState.userData.state}` : ''}
                    </div>
                </div>

                <div class="print-grid">
                    <div><strong>Aluno(a):</strong> ${formData.studentName}</div>
                    <div><strong>Idade:</strong> ${formData.age || '-'} anos</div>
                    <div><strong>Gênero:</strong> ${formData.gender}</div>
                    <div><strong>Peso:</strong> ${formData.weight || '-'} kg</div>
                    <div><strong>Altura:</strong> ${formData.height || '-'} m</div>
                    <div><strong>IMC:</strong> ${imcStr}</div>
                    <div><strong>Nível:</strong> ${formData.level}</div>
                    <div><strong>Frequência:</strong> ${formData.freq}</div>
                    <div><strong>Validade:</strong> ${formData.validity} dias</div>
                </div>`;

            if (objRec || formData.health.length > 0) {
                html += `<div class="print-guidelines"><h4>Diretrizes Automáticas</h4><ul>`;
                if(objRec) html += `<li><strong>Objetivo (${formData.objective}):</strong> ${objRec}</li>`;
                formData.health.forEach(k => {
                    if(healthSourceDict[k]) html += `<li><strong>Saúde (${k.replace(/[\u{1F300}-\u{1F9FF}\u{2600}-\u{26FF}\u{2700}-\u{27BF}\u{FE0F}]/gu, '').trim()}):</strong> ${healthSourceDict[k]}</li>`;
                });
                html += `</ul></div>`;
            }

            formData.workouts.forEach(w => {
                html += `<div class="print-workout"><h3>${w.title}</h3><table>
                    <thead><tr>
                        <th style="width:35%">Exercício</th><th style="width:8%;text-align:center">Séries</th><th style="width:12%;text-align:center">Reps</th><th style="width:15%">Técnica</th><th style="width:30%">Observações</th>
                    </tr></thead><tbody>`;
                
                if (w.exercises.length === 0) html += `<tr><td colspan="5" style="text-align:center; font-style:italic;">Sem exercícios</td></tr>`;
                else w.exercises.forEach(ex => {
                    html += `<tr>
                        <td><strong>${ex.name}</strong></td><td style="text-align:center">${ex.sets}</td><td style="text-align:center">${ex.reps}</td>
                        <td>${ex.technique !== 'Nenhuma' ? ex.technique : '-'}</td><td>${ex.obs || '-'}</td>
                    </tr>`;
                });
                html += `</tbody></table></div>`;
            });

            html += `
                <div class="print-footer-section">
                    ${formData.recs.trim() ? `<strong>Recomendações Manuais:</strong><br><p style="margin: 3px 0 10px 0; white-space:pre-wrap;">${formData.recs}</p>` : ''}
                    <div class="legal-text">${legalText}</div>
                    <div style="text-align:center; margin-top: 10px; font-size: 8px;">PowFit Pro - Ficha gerada em ${new Date().toLocaleString()} | Unidade: ${appState.userData.unitName} (${appState.userData.redeName})</div>
                </div>`;

            document.getElementById('print-area').innerHTML = html;
            setTimeout(() => { window.print(); }, 500);
        };

        // --- AUTH LISTENER ---
        onAuthStateChanged(auth, async (user) => {
            if (user) {
                appState.user = user;
                const docSnap = await getDoc(doc(db, "users", user.uid));
                if (docSnap.exists()) {
                    appState.userData = docSnap.data();
                    loadDashboard();
                } else {
                    // Preenche nome do Google se disponivel
                    document.getElementById('setup-name').value = user.displayName || '';
                    window.showScreen('screen-setup');
                }
            } else {
                appState = { user: null, userData: null, editingDocId: null, currentWorkouts: [], activeModalDayId: null, activeCategory: "🔥 PEITO" };
                window.showScreen('screen-login');
            }
        });

    </script>
</body>
</html>
