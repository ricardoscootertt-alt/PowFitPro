<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro - Master Edition</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            /* Tema Padrão (Masculino/Escuro) */
            --bg-body: #0f172a; --bg-card: #1e293b; --text-main: #f8fafc; --text-muted: #94a3b8;
            --border-color: #334155; --primary: #3b82f6; --primary-hover: #2563eb;
            --input-bg: #0f172a; --input-border: #475569;
        }
        [data-theme="Feminino"] {
            /* Tema Feminino (Rosa) */
            --bg-body: #fdf2f8; --bg-card: #ffffff; --text-main: #1e293b; --text-muted: #64748b;
            --border-color: #fbcfe8; --primary: #ec4899; --primary-hover: #db2777;
            --input-bg: #f8fafc; --input-border: #f1f5f9;
        }
        body { font-family: 'Inter', sans-serif; background-color: var(--bg-body); color: var(--text-main); transition: all 0.3s ease; }
        .card { background-color: var(--bg-card); border: 1px solid var(--border-color); }
        .input-field { background-color: var(--input-bg); border: 1px solid var(--input-border); color: var(--text-main); }
        .input-field:focus { outline: none; border-color: var(--primary); box-shadow: 0 0 0 1px var(--primary); }
        .btn-primary { background-color: var(--primary); color: white; transition: background-color 0.2s; }
        .btn-primary:hover { background-color: var(--primary-hover); }
        
        /* Modais, Views e Scrollbar */
        .view-section { display: none; }
        .view-section.active { display: block; }
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
        
        /* Esconde área de impressão na tela */
        #print-area { display: none; }

        /* IMPRESSÃO (ESTILO PLANILHA A4) */
        @media print {
            body { background: white !important; color: black !important; margin: 0; padding: 0; }
            #app-container, #login-view, #dashboard-view, .no-print, .modal { display: none !important; }
            #print-area { display: block !important; padding: 10mm 15mm; font-family: 'Arial', sans-serif; font-size: 10px; }
            @page { size: A4; margin: 0; }
            .print-header { border-bottom: 2px solid #000; padding-bottom: 5px; margin-bottom: 10px; text-align: center; }
            .print-title { font-size: 16px; font-weight: bold; margin: 0; text-transform: uppercase; }
            .prof-info { font-size: 11px; margin-top: 5px; font-weight: bold; }
            .print-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 4px; margin-bottom: 10px; border: 1px solid #000; padding: 6px; }
            .print-grid div { font-size: 10px; border-bottom: 1px dotted #ccc; padding-bottom: 2px;}
            .print-guidelines { border: 1px solid #000; padding: 6px; margin-bottom: 10px; font-size: 9px; }
            .print-guidelines h4 { margin: 0 0 4px 0; font-size: 10px; text-transform: uppercase; background: #eee; padding: 3px; }
            .print-guidelines ul { margin: 0; padding-left: 15px; }
            .print-workout { margin-bottom: 10px; page-break-inside: avoid; }
            .print-workout h3 { background: #e0e0e0; border: 1px solid #000; border-bottom: none; padding: 4px; margin: 0; font-size: 11px; text-transform: uppercase; -webkit-print-color-adjust: exact; }
            table { width: 100%; border-collapse: collapse; margin-bottom: 0; border: 1px solid #000; }
            th, td { border: 1px solid #000; padding: 4px; text-align: left; font-size: 10px; vertical-align: middle; }
            th { background-color: #f0f0f0; font-weight: bold; text-transform: uppercase; -webkit-print-color-adjust: exact; }
            .ex-img { width: 40px; height: 40px; object-fit: cover; border-radius: 4px; margin-right: 5px; display: inline-block; vertical-align: middle;}
            .print-footer-section { margin-top: 15px; border: 1px solid #000; padding: 6px; font-size: 9px; page-break-inside: avoid; }
            .legal-text { font-size: 8px; color: #333; text-align: justify; margin-top: 5px; font-weight: bold; }
            .ex-name-container { display: flex; align-items: center; }
        }
    </style>
</head>
<body data-theme="Masculino">

    <!-- 1. TELA DE LOGIN -->
    <div id="login-view" class="view-section active min-h-screen flex items-center justify-center p-4">
        <div class="card p-8 rounded-2xl shadow-2xl max-w-md w-full text-center">
            <i class="fas fa-dumbbell text-5xl text-primary mb-4"></i>
            <h1 class="text-3xl font-bold mb-2">PowFit Pro</h1>
            <p class="text-sm opacity-70 mb-8">Plataforma Master de Prescrição</p>
            <button id="btn-login" class="w-full bg-white text-gray-800 hover:bg-gray-100 font-bold py-3 px-4 rounded-lg flex items-center justify-center gap-3 shadow transition">
                <img src="https://www.svgrepo.com/show/475656/google-color.svg" class="w-5 h-5" alt="Google">
                Entrar com Google
            </button>
            <div id="login-loader" class="hidden mt-4 text-primary"><i class="fas fa-circle-notch fa-spin text-2xl"></i></div>
        </div>
    </div>

    <!-- 2. DASHBOARD DE REDE E MEMBROS -->
    <div id="dashboard-view" class="view-section min-h-screen p-4 sm:p-8 max-w-6xl mx-auto">
        <div class="flex justify-between items-center mb-8">
            <div>
                <h1 class="text-2xl font-bold"><i class="fas fa-network-wired text-primary"></i> Dashboard da Rede</h1>
                <p class="text-sm opacity-70">Gerencie Unidades, Membros e Relatórios</p>
            </div>
            <button id="btn-logout" class="bg-red-500 hover:bg-red-600 text-white px-4 py-2 rounded-lg font-medium shadow text-sm">Sair</button>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <!-- Gerenciamento -->
            <div class="space-y-6">
                <!-- Add Unidade -->
                <div class="card p-5 rounded-xl">
                    <h2 class="text-lg font-bold mb-3">1. Cadastrar Unidade</h2>
                    <div class="flex gap-2">
                        <input type="text" id="dash-new-unit" class="input-field flex-1 rounded-lg px-3 text-sm" placeholder="Ex: PowerFit Centro">
                        <button id="btn-add-unit" class="btn-primary px-4 py-2 rounded-lg text-sm font-bold">Adicionar</button>
                    </div>
                </div>
                
                <!-- Add Membro -->
                <div class="card p-5 rounded-xl">
                    <h2 class="text-lg font-bold mb-3">2. Cadastrar Membro (Profissional)</h2>
                    <div class="space-y-3">
                        <select id="dash-sel-unit" class="input-field w-full rounded-lg px-3 py-2 text-sm"><option value="">Selecione a Unidade...</option></select>
                        <input type="text" id="dash-m-name" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Nome do Profissional">
                        <input type="text" id="dash-m-cpf" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="CPF">
                        <div class="grid grid-cols-2 gap-2">
                            <select id="dash-m-cat" class="input-field rounded-lg px-3 py-2 text-sm" onchange="toggleDashCref()">
                                <option value="PEF">Prof. de Ed. Física</option>
                                <option value="TE">Treinador Esportivo</option>
                            </select>
                            <input type="text" id="dash-m-uf" class="input-field rounded-lg px-3 py-2 text-sm" placeholder="UF (Ex: SP)">
                        </div>
                        <input type="text" id="dash-m-cref" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="CREF (Apenas números)">
                        <button id="btn-add-member" class="btn-primary w-full py-2 rounded-lg text-sm font-bold">Cadastrar Membro</button>
                    </div>
                </div>
            </div>

            <!-- Listagem e Acesso -->
            <div class="card p-5 rounded-xl flex flex-col">
                <h2 class="text-lg font-bold mb-3 flex justify-between items-center">
                    <span>3. Acessar Sistema</span>
                    <button id="btn-report" class="text-xs bg-gray-700 text-white px-3 py-1 rounded hover:bg-gray-600"><i class="fas fa-chart-bar"></i> Relatório de Produtividade</button>
                </h2>
                <p class="text-xs opacity-70 mb-4">Selecione o seu perfil para gerar fichas no seu nome.</p>
                <div id="network-tree" class="flex-1 overflow-y-auto space-y-2 max-h-[400px]">
                    <!-- Renderizado via JS -->
                    <div class="text-center opacity-50 text-sm py-4">Carregando unidades...</div>
                </div>
            </div>
        </div>
    </div>

    <!-- 3. PLATAFORMA DE PRESCRIÇÃO (APP) -->
    <div id="app-view" class="view-section min-h-screen pb-20">
        <div id="app-container" class="max-w-7xl mx-auto p-4 sm:p-6 lg:p-8">
            <!-- Header App -->
            <div class="flex flex-col sm:flex-row justify-between items-center mb-6 gap-4">
                <div>
                    <h1 class="text-xl font-bold tracking-tight">PowFit Pro <span class="text-sm font-normal text-primary px-2 py-1 bg-primary bg-opacity-10 rounded ml-2" id="app-active-member">...</span></h1>
                </div>
                <div class="flex gap-2">
                    <button onclick="openHistory()" class="bg-gray-700 hover:bg-gray-600 text-white px-3 py-2 rounded-lg font-medium shadow flex items-center gap-2 text-sm transition">
                        <i class="fas fa-cloud-download-alt"></i> Histórico
                    </button>
                    <button onclick="generatePrintAndSave()" class="btn-primary px-4 py-2 rounded-lg font-bold shadow-lg flex items-center gap-2 text-sm">
                        <i class="fas fa-save"></i> <i class="fas fa-print"></i> Salvar & Imprimir
                    </button>
                    <button onclick="backToDashboard()" class="bg-red-500 hover:bg-red-600 text-white px-3 py-2 rounded-lg text-sm"><i class="fas fa-sign-out-alt"></i></button>
                </div>
            </div>

            <div class="grid grid-cols-1 xl:grid-cols-12 gap-6">
                <!-- Coluna Esquerda: Aluno e Saúde -->
                <div class="xl:col-span-4 space-y-5">
                    <div class="card rounded-xl p-4 shadow-sm">
                        <div class="flex justify-between items-center mb-3 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                            <h2 class="text-md font-semibold"><i class="fas fa-user text-primary"></i> Aluno</h2>
                            <div id="imc-display"></div>
                        </div>
                        <div class="space-y-3">
                            <input type="text" id="stu-name" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Nome Completo">
                            <div class="grid grid-cols-3 gap-2">
                                <input type="number" id="stu-age" class="input-field rounded-lg px-2 py-2 text-sm" placeholder="Idade">
                                <input type="number" id="stu-weight" oninput="calculateIMC()" class="input-field rounded-lg px-2 py-2 text-sm" placeholder="Peso(kg)">
                                <input type="number" id="stu-height" step="0.01" oninput="calculateIMC()" class="input-field rounded-lg px-2 py-2 text-sm" placeholder="Alt.(m)">
                            </div>
                            <div class="grid grid-cols-2 gap-2">
                                <select id="stu-gender" onchange="changeTheme()" class="input-field rounded-lg px-2 py-2 text-sm">
                                    <option value="Masculino">Masculino</option><option value="Feminino">Feminino</option>
                                </select>
                                <select id="stu-level" class="input-field rounded-lg px-2 py-2 text-sm">
                                    <option>Iniciante</option><option>Intermediário</option><option>Avançado</option>
                                </select>
                            </div>
                            <div class="grid grid-cols-2 gap-2">
                                <select id="stu-freq" class="input-field rounded-lg px-2 py-2 text-sm">
                                    <option>3 dias</option><option>5 dias</option><option>6 dias</option><option>Personalizado</option>
                                </select>
                                <select id="stu-validity" class="input-field rounded-lg px-2 py-2 text-sm">
                                    <option>15 dias</option><option selected>30 dias</option><option>60 dias</option><option>90 dias</option>
                                </select>
                            </div>
                            <select id="stu-objective" onchange="updateObjText()" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                <option value="Emagrecimento">Emagrecimento</option><option value="Hipertrofia">Hipertrofia</option>
                                <option value="Definição">Definição</option><option value="Condicionamento">Condicionamento</option>
                                <option value="Resistência">Resistência</option><option value="Força">Força</option>
                                <option value="Reabilitação">Reabilitação</option><option value="Saúde geral">Saúde geral</option>
                            </select>
                            <p id="obj-info" class="text-[10px] opacity-70 italic p-2 bg-black bg-opacity-10 rounded"></p>
                        </div>
                    </div>

                    <div class="card rounded-xl p-4 shadow-sm">
                        <h2 class="text-md font-semibold mb-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)"><i class="fas fa-heartbeat text-primary"></i> Estado de Saúde</h2>
                        <div class="grid grid-cols-2 gap-2 text-[11px]" id="health-container"></div>
                    </div>
                </div>

                <!-- Coluna Direita: Treinos -->
                <div class="xl:col-span-8 space-y-5">
                    <div class="flex justify-between items-center bg-opacity-10 p-3 rounded-xl card border-dashed border-2">
                        <h2 class="text-lg font-bold"><i class="fas fa-list-alt text-primary"></i> Montagem</h2>
                        <button onclick="addWorkout()" class="btn-primary px-3 py-1.5 rounded-lg text-sm font-medium"><i class="fas fa-plus"></i> Dia</button>
                    </div>
                    <div id="workouts-container" class="space-y-4"></div>
                    
                    <div class="card rounded-xl p-4 shadow-sm">
                        <h2 class="text-md font-semibold mb-2"><i class="fas fa-comment-medical text-primary"></i> Recomendações Profissionais</h2>
                        <textarea id="stu-recs" rows="3" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Hidratação, descanso, cuidados..."></textarea>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- MODAL DE EXERCÍCIOS E CADASTRO -->
    <div id="exercise-modal" class="fixed inset-0 bg-black bg-opacity-80 backdrop-blur-sm hidden z-[60] flex items-center justify-center p-2">
        <div class="card w-full max-w-6xl rounded-xl shadow-2xl flex flex-col h-[95vh] border border-gray-600">
            <div class="p-4 border-b flex justify-between items-center bg-black bg-opacity-20" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold"><i class="fas fa-search text-primary mr-2"></i> Banco de Exercícios</h3>
                <button onclick="closeModal()" class="text-gray-400 hover:text-red-500 text-2xl">&times;</button>
            </div>
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <div class="w-full md:w-1/4 border-r overflow-y-auto p-2" style="border-color: var(--border-color)" id="modal-categories"></div>
                <div class="w-full md:w-3/4 flex flex-col bg-black bg-opacity-5">
                    
                    <!-- Painel Adicionar Novo -->
                    <div class="p-3 border-b flex flex-wrap gap-2 items-end bg-black bg-opacity-10" style="border-color: var(--border-color)">
                        <div class="flex-1 min-w-[200px]">
                            <label class="block text-[10px] mb-1 font-bold">Novo Exercício nesta categoria:</label>
                            <input type="text" id="new-ex-name" class="input-field w-full rounded px-2 py-1.5 text-sm" placeholder="Nome do exercício">
                        </div>
                        <div>
                            <label class="block text-[10px] mb-1 font-bold">Imagem (Opcional):</label>
                            <input type="file" id="new-ex-img" accept="image/*" class="text-xs w-48">
                        </div>
                        <button onclick="saveCustomExercise()" class="btn-primary px-4 py-1.5 rounded text-sm font-bold"><i class="fas fa-save"></i> Salvar na Nuvem</button>
                    </div>

                    <!-- Lista de Exercícios -->
                    <div class="flex-1 overflow-y-auto p-3" id="modal-exercises"></div>
                </div>
            </div>
        </div>
    </div>

    <!-- MODAL DE HISTÓRICO -->
    <div id="history-modal" class="fixed inset-0 bg-black bg-opacity-80 backdrop-blur-sm hidden z-[70] flex items-center justify-center p-2">
        <div class="card w-full max-w-4xl rounded-xl flex flex-col max-h-[90vh]">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold"><i class="fas fa-cloud text-primary mr-2"></i> Histórico na Nuvem (Sua Unidade)</h3>
                <button onclick="closeHistory()" class="text-gray-400 hover:text-red-500 text-2xl">&times;</button>
            </div>
            <div class="p-4 overflow-y-auto flex-1 space-y-2" id="history-list"></div>
        </div>
    </div>

    <!-- MODAL RELATÓRIO DE PRODUTIVIDADE -->
    <div id="report-modal" class="fixed inset-0 bg-black bg-opacity-80 backdrop-blur-sm hidden z-[80] flex items-center justify-center p-2">
        <div class="card w-full max-w-3xl rounded-xl flex flex-col max-h-[90vh]">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold"><i class="fas fa-chart-line text-primary mr-2"></i> Relatório Mensal</h3>
                <button onclick="document.getElementById('report-modal').classList.add('hidden')" class="text-gray-400 hover:text-red-500 text-2xl">&times;</button>
            </div>
            <div class="p-4 flex gap-2 border-b" style="border-color: var(--border-color)">
                <select id="report-month" class="input-field rounded px-2 py-1 text-sm"></select>
                <select id="report-unit" class="input-field rounded px-2 py-1 text-sm"></select>
                <button onclick="generateReport()" class="btn-primary px-3 py-1 rounded text-sm font-bold">Gerar</button>
            </div>
            <div class="p-4 overflow-y-auto flex-1" id="report-content"></div>
        </div>
    </div>

    <div id="print-area"></div>

    <!-- TELA DE LOADING GLOBAL -->
    <div id="global-loader" class="fixed inset-0 bg-black bg-opacity-90 z-[100] hidden flex-col items-center justify-center text-white">
        <i class="fas fa-spinner fa-spin text-5xl text-primary mb-4"></i>
        <h2 class="text-xl font-bold">Sincronizando com a Nuvem...</h2>
    </div>

    <!-- JAVASCRIPT MASTER MODULE -->
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

        // --- DADOS ESTÁTICOS BASE ---
        const healthDataPEF = {
            "🟢 Saudável": "Manter treinos regulares 3–5x por semana, combinando musculação e cardio.",
            "⚪ Sedentário": "Iniciar com treinos leves 2–3x por semana. Priorizar adaptação.",
            "🟡 Sobrepeso": "Treinar 3–5x por semana com foco em gasto calórico.",
            "🔴 Obesidade": "Iniciar com exercícios de baixo impacto. Evolução gradual.",
            "⚖️ Baixo peso": "Foco em musculação para ganho de massa.",
            "🍬 Diabetes": "Treinos regulares moderados. Monitorar glicemia.",
            "❤️ Hipertensão": "Treinos moderados, evitar prender a respiração.",
            "🔵 Hipotensão": "Evitar mudanças bruscas. Manter hidratação.",
            "💔 Problemas cardíacos": "Treinos leves com liberação médica.",
            "🦴 Problemas articulares": "Evitar impacto. Priorizar exercícios controlados.",
            "🫁 Problemas respiratórios": "Treinos leves a moderados. Atenção à respiração.",
            "⚠️ Lesões": "Adaptar exercícios. Evitar dor e focar na recuperação.",
            "🤰 Gestante": "Treinos leves a moderados, sem impacto. Liberação médica.",
            "🤱 Lactante": "Treinar normalmente com atenção à hidratação.",
            "👴 Idoso": "Foco em força, equilíbrio e mobilidade."
        };

        const healthDataTE = {
            "🟢 Saudável": "Manter treinos regulares de 3–5x por semana, combinando musculação e cardio.",
            "⚪ Sedentário": "Início gradual, com treinos leves e foco na adaptação.",
            "🟡 Sobrepeso": "A prática regular de musculação e cardio contribui para a composição corporal.",
            "🔴 Obesidade": "Início com exercícios de menor impacto e progressão gradual.",
            "⚖️ Baixo peso": "Musculação para ganho de massa, priorizando evolução progressiva.",
            "🍬 Diabetes": "Acompanhamento médico regular. Interromper em caso de mal-estar.",
            "❤️ Hipertensão": "Evitar manobra de Valsalva e controlar intensidade.",
            "🔵 Hipotensão": "Evitar mudanças bruscas de posição.",
            "💔 Problemas cardíacos": "Prática apenas com liberação médica e controle rigoroso.",
            "🦴 Problemas articulares": "Menor impacto e maior controle de movimento.",
            "🫁 Problemas respiratórios": "Progressão gradual. Interromper em falta de ar.",
            "⚠️ Lesões": "Respeitar limitação. Seguir orientação técnica.",
            "🤰 Gestante": "Apenas com liberação médica e sem risco de impacto.",
            "🤱 Lactante": "Atividade mantida com atenção à recuperação e hidratação.",
            "👴 Idoso": "Contribuição para força e autonomia. Intensidade moderada."
        };

        const objectiveData = {
            "Emagrecimento": "Foco em déficit calórico com treinos mistos (força + aeróbico).",
            "Hipertrofia": "Prioridade na progressão de carga e superávit calórico.",
            "Definição": "Manutenção muscular reduzindo % de gordura.",
            "Condicionamento": "Circuitos e alta integração cardiopulmonar.",
            "Resistência": "Séries mais longas, cadência controlada.",
            "Força": "Cargas altas, baixas repetições e intervalos longos.",
            "Reabilitação": "Fortalecimento específico e controle motor.",
            "Saúde geral": "Equilíbrio entre força, cardio e flexibilidade."
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

        // --- ESTADO GLOBAL DA APLICAÇÃO ---
        window.state = {
            currentUser: null,
            networkUnits: [],
            networkMembers: [],
            activeMember: null, // Perfil que está montando a ficha
            customExercises: [], // Carregados da nuvem {id, category, name, imageBase64}
            workouts: [],
            activeModalWorkoutId: null,
            activeCategory: "🔥 PEITO",
            currentFichaId: null
        };

        // --- FUNÇÕES UTILITÁRIAS ---
        const showLoader = () => document.getElementById('global-loader').classList.remove('hidden');
        const hideLoader = () => document.getElementById('global-loader').classList.add('hidden');
        const generateId = () => Math.random().toString(36).substr(2, 9);
        const switchView = (viewId) => {
            document.querySelectorAll('.view-section').forEach(el => el.classList.remove('active'));
            document.getElementById(viewId).classList.add('active');
        };

        // --- COMPRESSÃO DE IMAGEM (CANVAS) ---
        function compressImage(file, callback) {
            const reader = new FileReader();
            reader.readAsDataURL(file);
            reader.onload = event => {
                const img = new Image();
                img.src = event.target.result;
                img.onload = () => {
                    const canvas = document.createElement('canvas');
                    const MAX_WIDTH = 150; // Bem pequeno, só para o ícone da planilha
                    const scaleSize = MAX_WIDTH / img.width;
                    canvas.width = MAX_WIDTH;
                    canvas.height = img.height * scaleSize;
                    const ctx = canvas.getContext('2d');
                    ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
                    callback(canvas.toDataURL('image/jpeg', 0.6));
                }
            }
        }

        // --- AUTENTICAÇÃO ---
        document.getElementById('btn-login').addEventListener('click', async () => {
            document.getElementById('login-loader').classList.remove('hidden');
            try {
                await signInWithPopup(auth, new GoogleAuthProvider());
            } catch (error) {
                alert("Erro ao logar: " + error.message);
                document.getElementById('login-loader').classList.add('hidden');
            }
        });

        document.getElementById('btn-logout').addEventListener('click', () => signOut(auth));

        onAuthStateChanged(auth, async (user) => {
            if (user) {
                window.state.currentUser = user;
                switchView('dashboard-view');
                await loadNetworkData();
            } else {
                window.state.currentUser = null;
                document.getElementById('login-loader').classList.add('hidden');
                switchView('login-view');
            }
        });

        // --- DASHBOARD / REDE ---
        window.toggleDashCref = () => {
            const cat = document.getElementById('dash-m-cat').value;
            document.getElementById('dash-m-cref').style.display = (cat === 'TE') ? 'none' : 'block';
        };

        async function loadNetworkData() {
            showLoader();
            const uid = window.state.currentUser.uid;
            
            // Carregar Unidades (Criadas pelo usuário logado)
            const qUnits = query(collection(db, "units"), where("ownerUid", "==", uid));
            const snapUnits = await getDocs(qUnits);
            window.state.networkUnits = snapUnits.docs.map(d => ({id: d.id, ...d.data()}));

            // Carregar Membros
            const qMembers = query(collection(db, "members"), where("ownerUid", "==", uid));
            const snapMembers = await getDocs(qMembers);
            window.state.networkMembers = snapMembers.docs.map(d => ({id: d.id, ...d.data()}));

            // Preencher Select de Unidades no Cadastro de Membro
            const selUnit = document.getElementById('dash-sel-unit');
            selUnit.innerHTML = '<option value="">Selecione a Unidade...</option>' + 
                window.state.networkUnits.map(u => `<option value="${u.id}">${u.name}</option>`).join('');

            renderNetworkTree();
            hideLoader();
        }

        document.getElementById('btn-add-unit').addEventListener('click', async () => {
            const name = document.getElementById('dash-new-unit').value.trim();
            if(!name) return alert("Digite o nome da unidade.");
            showLoader();
            await addDoc(collection(db, "units"), { ownerUid: window.state.currentUser.uid, name });
            document.getElementById('dash-new-unit').value = '';
            await loadNetworkData();
        });

        document.getElementById('btn-add-member').addEventListener('click', async () => {
            const unitId = document.getElementById('dash-sel-unit').value;
            const name = document.getElementById('dash-m-name').value.trim();
            const cpf = document.getElementById('dash-m-cpf').value.trim();
            const cat = document.getElementById('dash-m-cat').value;
            const uf = document.getElementById('dash-m-uf').value.trim();
            const cref = document.getElementById('dash-m-cref').value.trim();

            if(!unitId || !name || !cpf || !uf) return alert("Preencha todos os campos obrigatórios.");
            if(cat === 'PEF' && !cref) return alert("CREF é obrigatório para Profissional de Educação Física.");

            showLoader();
            await addDoc(collection(db, "members"), {
                ownerUid: window.state.currentUser.uid,
                unitId, name, cpf, category: cat, uf, cref: (cat==='PEF'?cref:'')
            });
            document.getElementById('dash-m-name').value = '';
            document.getElementById('dash-m-cpf').value = '';
            document.getElementById('dash-m-cref').value = '';
            await loadNetworkData();
        });

        function renderNetworkTree() {
            const tree = document.getElementById('network-tree');
            tree.innerHTML = '';
            if(window.state.networkUnits.length === 0) {
                tree.innerHTML = '<p class="text-sm opacity-50">Nenhuma unidade cadastrada.</p>';
                return;
            }

            window.state.networkUnits.forEach(u => {
                const members = window.state.networkMembers.filter(m => m.unitId === u.id);
                const memHtml = members.map(m => `
                    <div class="flex justify-between items-center bg-black bg-opacity-20 p-2 rounded mt-1 ml-4 border border-gray-600">
                        <div>
                            <span class="font-bold text-sm text-primary">${m.name}</span> <span class="text-[10px] opacity-70">(${m.category})</span>
                        </div>
                        <button onclick="enterApp('${m.id}')" class="bg-primary hover:bg-blue-600 text-white px-3 py-1 rounded text-xs font-bold shadow">
                            Acessar Fichas <i class="fas fa-arrow-right ml-1"></i>
                        </button>
                    </div>
                `).join('');

                tree.innerHTML += `
                    <div class="card p-3 rounded bg-black bg-opacity-10 border border-gray-600">
                        <h3 class="font-bold text-md"><i class="fas fa-building text-gray-400 mr-2"></i> ${u.name}</h3>
                        ${memHtml || '<p class="text-xs opacity-50 ml-6 mt-1">Nenhum membro nesta unidade.</p>'}
                    </div>
                `;
            });
        }

        // --- APP VIEW INIT ---
        window.enterApp = async (memberId) => {
            window.state.activeMember = window.state.networkMembers.find(m => m.id === memberId);
            document.getElementById('app-active-member').innerText = `${window.state.activeMember.name} | ${window.state.activeMember.category}`;
            
            // Configurar textos e lógicas baseadas no PEF ou TE
            renderHealthCheckboxes();
            
            // Limpar ficha atual e adicionar Segunda-Feira
            window.state.workouts = [{ id: generateId(), title: "TREINO SEGUNDA-FEIRA", exercises: [] }];
            window.state.currentFichaId = null;
            renderWorkoutsApp();
            
            // Carregar Custom Exercises da Unidade
            await loadCustomExercises();
            
            switchView('app-view');
        };

        window.backToDashboard = () => {
            if(confirm("Sair sem salvar?")) switchView('dashboard-view');
        };

        // --- LÓGICA DO APP (ALUNO, SAÚDE) ---
        window.changeTheme = () => {
            document.body.setAttribute('data-theme', document.getElementById('stu-gender').value);
        };

        window.calculateIMC = () => {
            const w = parseFloat(document.getElementById('stu-weight').value);
            const h = parseFloat(document.getElementById('stu-height').value);
            const display = document.getElementById('imc-display');
            if (w > 0 && h > 0) {
                const imc = (w / (h * h)).toFixed(1);
                let cls = imc < 18.5 ? "Baixo" : imc < 24.9 ? "Normal" : imc < 29.9 ? "Sobrepeso" : "Obesidade";
                display.innerHTML = `<span class="bg-primary text-white px-2 py-0.5 rounded text-[10px] font-bold">IMC: ${imc} (${cls})</span>`;
            } else display.innerHTML = '';
        };

        window.updateObjText = () => {
            const obj = document.getElementById('stu-objective').value;
            document.getElementById('obj-info').innerText = objectiveData[obj] || '';
        };

        function renderHealthCheckboxes() {
            const container = document.getElementById('health-container');
            const dataObj = window.state.activeMember.category === 'PEF' ? healthDataPEF : healthDataTE;
            container.innerHTML = Object.keys(dataObj).map(opt => `
                <label class="flex items-start space-x-1 cursor-pointer">
                    <input type="checkbox" value="${opt}" class="health-cb mt-0.5 w-3 h-3 text-primary">
                    <span class="font-medium">${opt}</span>
                </label>
            `).join('');
            window.updateObjText(); // Força update do obj também
        }

        // --- LÓGICA DE MONTAGEM DE TREINOS ---
        window.addWorkout = () => {
            const c = window.state.workouts.length;
            window.state.workouts.push({ id: generateId(), title: c < 7 ? `TREINO ${daysOfWeek[c]}` : `NOVO TREINO ${c+1}`, exercises: [] });
            renderWorkoutsApp();
        };

        window.duplicateWorkout = (id) => {
            const w = window.state.workouts.find(w => w.id === id);
            if(w) {
                const copy = JSON.parse(JSON.stringify(w));
                copy.id = generateId(); copy.title += " (Cópia)";
                window.state.workouts.push(copy); renderWorkoutsApp();
            }
        };

        window.removeWorkout = (id) => {
            if(confirm("Excluir dia?")) { window.state.workouts = window.state.workouts.filter(w => w.id !== id); renderWorkoutsApp(); }
        };

        window.updateWorkoutTitle = (id, val) => { window.state.workouts.find(w => w.id === id).title = val; };
        
        window.removeExercise = (wId, exIdx) => { window.state.workouts.find(w => w.id === wId).exercises.splice(exIdx, 1); renderWorkoutsApp(); };
        window.moveExercise = (wId, idx, dir) => {
            const arr = window.state.workouts.find(w => w.id === wId).exercises;
            if (dir === 'up' && idx > 0) { const t = arr[idx]; arr[idx] = arr[idx-1]; arr[idx-1] = t; }
            else if (dir === 'down' && idx < arr.length-1) { const t = arr[idx]; arr[idx] = arr[idx+1]; arr[idx+1] = t; }
            renderWorkoutsApp();
        };
        window.updateExercise = (wId, idx, field, val) => { window.state.workouts.find(w => w.id === wId).exercises[idx][field] = val; };

        function renderWorkoutsApp() {
            const cont = document.getElementById('workouts-container');
            cont.innerHTML = window.state.workouts.map(w => `
                <div class="card rounded-xl overflow-hidden shadow">
                    <div class="p-2 border-b flex justify-between bg-black bg-opacity-20" style="border-color: var(--border-color)">
                        <input type="text" value="${w.title}" onchange="updateWorkoutTitle('${w.id}', this.value)" class="input-field bg-transparent font-bold text-sm w-1/2 uppercase">
                        <div class="flex gap-1">
                            <button onclick="duplicateWorkout('${w.id}')" class="text-xs p-1 text-gray-400 hover:text-white"><i class="fas fa-copy"></i></button>
                            <button onclick="removeWorkout('${w.id}')" class="text-xs p-1 text-red-500 hover:text-white"><i class="fas fa-trash"></i></button>
                        </div>
                    </div>
                    <div class="p-0">
                        ${w.exercises.length === 0 ? '<div class="text-center p-3 text-xs opacity-50">Sem exercícios.</div>' : w.exercises.map((ex, idx) => `
                            <div class="flex flex-wrap sm:flex-nowrap gap-2 p-2 items-center border-b border-opacity-10" style="border-color: var(--border-color)">
                                <div class="flex flex-col gap-1 w-6">
                                    <button onclick="moveExercise('${w.id}', ${idx}, 'up')" class="text-[10px] hover:text-primary"><i class="fas fa-chevron-up"></i></button>
                                    <button onclick="moveExercise('${w.id}', ${idx}, 'down')" class="text-[10px] hover:text-primary"><i class="fas fa-chevron-down"></i></button>
                                </div>
                                ${ex.img ? `<img src="${ex.img}" class="w-8 h-8 rounded object-cover">` : ''}
                                <div class="flex-1 w-full sm:w-auto">
                                    <div class="text-[9px] opacity-60 uppercase">${ex.category}</div>
                                    <div class="text-xs font-bold leading-tight">${ex.name}</div>
                                </div>
                                <div class="flex gap-1 items-center">
                                    <input type="text" value="${ex.sets}" onchange="updateExercise('${w.id}', ${idx}, 'sets', this.value)" class="input-field w-8 text-center text-xs p-1" placeholder="S"> x
                                    <input type="text" value="${ex.reps}" onchange="updateExercise('${w.id}', ${idx}, 'reps', this.value)" class="input-field w-12 text-center text-xs p-1" placeholder="R">
                                    <select onchange="updateExercise('${w.id}', ${idx}, 'technique', this.value)" class="input-field text-[10px] p-1 w-20">
                                        ${dbTechniques.map(t => `<option ${ex.technique===t?'selected':''}>${t}</option>`).join('')}
                                    </select>
                                    <input type="text" value="${ex.obs}" onchange="updateExercise('${w.id}', ${idx}, 'obs', this.value)" class="input-field w-24 text-xs p-1" placeholder="Obs">
                                    <button onclick="removeExercise('${w.id}', ${idx})" class="text-red-500 p-1"><i class="fas fa-times"></i></button>
                                </div>
                            </div>
                        `).join('')}
                    </div>
                    <button onclick="openModal('${w.id}')" class="w-full text-primary py-2 text-xs font-bold hover:bg-primary hover:bg-opacity-10">+ ADD EXERCÍCIO AQUI</button>
                </div>
            `).join('');
        }

        // --- BANCO DE EXERCÍCIOS E CUSTOMIZAÇÃO NA NUVEM ---
        async function loadCustomExercises() {
            const q = query(collection(db, "custom_exercises"), where("unitId", "==", window.state.activeMember.unitId));
            const snap = await getDocs(q);
            window.state.customExercises = snap.docs.map(d => ({id: d.id, ...d.data()}));
        }

        window.openModal = (wId) => { window.state.activeModalWorkoutId = wId; document.getElementById('exercise-modal').classList.remove('hidden'); renderModalCategories(); renderModalExercises(); };
        window.closeModal = () => { document.getElementById('exercise-modal').classList.add('hidden'); };
        window.setModalCategory = (cat) => { window.state.activeCategory = cat; renderModalCategories(); renderModalExercises(); };
        
        function renderModalCategories() {
            document.getElementById('modal-categories').innerHTML = Object.keys(dbCategories).map(cat => `
                <button onclick="setModalCategory('${cat}')" class="w-full text-left px-2 py-2 rounded text-xs font-bold ${window.state.activeCategory===cat?'bg-primary text-white':'hover:bg-white hover:bg-opacity-10'}">${cat}</button>
            `).join('');
        }

        function renderModalExercises() {
            // Merge defaults and customs
            const defaults = dbCategories[window.state.activeCategory].map(name => ({name, img: null}));
            const customs = window.state.customExercises.filter(c => c.category === window.state.activeCategory);
            const all = [...defaults, ...customs];

            document.getElementById('modal-exercises').innerHTML = `
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-2">
                    ${all.map(ex => {
                        const imgTag = ex.img ? `<img src="${ex.img}" class="w-8 h-8 rounded object-cover mr-2">` : `<i class="fas fa-dumbbell opacity-30 mr-2"></i>`;
                        // Escape quotes for onclick
                        const safeName = ex.name.replace(/'/g, "\\'");
                        const safeImg = ex.img ? ex.img.replace(/'/g, "\\'") : '';
                        return `
                        <button onclick="addExerciseToWorkout('${safeName}', '${safeImg}')" class="card p-2 rounded text-left text-xs font-bold border hover:border-primary flex items-center group">
                            ${imgTag}
                            <span class="flex-1">${ex.name}</span>
                            <i class="fas fa-plus opacity-0 group-hover:opacity-100 text-primary"></i>
                        </button>`
                    }).join('')}
                </div>
            `;
        }

        window.saveCustomExercise = async () => {
            const name = document.getElementById('new-ex-name').value.trim();
            const fileInput = document.getElementById('new-ex-img');
            if(!name) return alert("Digite o nome.");

            showLoader();
            const cat = window.state.activeCategory;
            const unitId = window.state.activeMember.unitId;

            const saveToDb = async (base64Img = null) => {
                await addDoc(collection(db, "custom_exercises"), { unitId, category: cat, name, img: base64Img });
                await loadCustomExercises();
                document.getElementById('new-ex-name').value = '';
                fileInput.value = '';
                renderModalExercises();
                hideLoader();
            };

            if(fileInput.files && fileInput.files[0]) {
                compressImage(fileInput.files[0], saveToDb);
            } else {
                saveToDb();
            }
        };

        window.addExerciseToWorkout = (name, img) => {
            const w = window.state.workouts.find(w => w.id === window.state.activeModalWorkoutId);
            if(w) {
                w.exercises.push({ category: window.state.activeCategory, name, img: img || null, sets: '3', reps: '10 a 12', technique: 'Nenhuma', obs: '' });
                renderWorkoutsApp();
            }
        };

        // --- SALVAR NA NUVEM, HISTÓRICO E IMPRIMIR ---
        function getFormData() {
            return {
                studentName: document.getElementById('stu-name').value || 'Sem Nome',
                stuAge: document.getElementById('stu-age').value,
                stuWeight: document.getElementById('stu-weight').value,
                stuHeight: document.getElementById('stu-height').value,
                stuGender: document.getElementById('stu-gender').value,
                stuLevel: document.getElementById('stu-level').value,
                stuObjective: document.getElementById('stu-objective').value,
                stuFreq: document.getElementById('stu-freq').value,
                stuValidity: document.getElementById('stu-validity').value,
                stuRecs: document.getElementById('stu-recs').value,
                health: Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value)
            };
        }

        window.generatePrintAndSave = async () => {
            showLoader();
            const m = window.state.activeMember;
            const data = getFormData();
            const dateStr = new Date().toLocaleDateString('pt-BR');
            const monthYear = new Date().toISOString().substring(0, 7); // YYYY-MM para relatórios

            const payload = {
                memberId: m.id,
                unitId: m.unitId,
                date: dateStr,
                monthYear: monthYear,
                timestamp: Date.now(),
                ...data,
                workouts: window.state.workouts
            };

            try {
                if(window.state.currentFichaId) {
                    await setDoc(doc(db, "workouts", window.state.currentFichaId), payload);
                } else {
                    const docRef = await addDoc(collection(db, "workouts"), payload);
                    window.state.currentFichaId = docRef.id;
                }
                
                hideLoader();
                executePrintHTML(payload, m);
            } catch (e) {
                hideLoader();
                alert("Erro ao salvar: " + e.message);
            }
        };

        window.openHistory = async () => {
            showLoader();
            const q = query(collection(db, "workouts"), where("unitId", "==", window.state.activeMember.unitId), orderBy("timestamp", "desc"));
            const snap = await getDocs(q);
            const list = document.getElementById('history-list');
            list.innerHTML = '';
            
            if(snap.empty) {
                list.innerHTML = `<p class="text-center opacity-50">Nenhuma ficha salva nesta unidade.</p>`;
            } else {
                snap.forEach(doc => {
                    const d = doc.data();
                    // Checagem de validade visual (simplificada para demonstração)
                    const isOld = (Date.now() - d.timestamp) > (30 * 24 * 60 * 60 * 1000); 
                    list.innerHTML += `
                        <div class="card p-3 rounded border border-gray-600 flex justify-between items-center ${isOld ? 'opacity-60' : ''}">
                            <div>
                                <div class="font-bold text-sm">${d.studentName} ${isOld ? '<span class="text-[9px] bg-red-500 text-white px-1 rounded ml-1">Expirada</span>' : ''}</div>
                                <div class="text-[10px] opacity-70">${d.date} | ${d.stuObjective} | Val: ${d.stuValidity}</div>
                            </div>
                            <div class="flex gap-2">
                                <button onclick="loadHistoryItem('${doc.id}')" class="btn-primary px-2 py-1 rounded text-[10px] font-bold">CARREGAR</button>
                                <button onclick="deleteHistoryItem('${doc.id}')" class="bg-red-500 text-white px-2 py-1 rounded text-[10px]"><i class="fas fa-trash"></i></button>
                            </div>
                        </div>
                    `;
                });
            }
            hideLoader();
            document.getElementById('history-modal').classList.remove('hidden');
        };

        window.closeHistory = () => document.getElementById('history-modal').classList.add('hidden');

        window.loadHistoryItem = async (id) => {
            showLoader();
            const docSnap = await getDoc(doc(db, "workouts", id));
            if(docSnap.exists()) {
                const item = docSnap.data();
                window.state.currentFichaId = id;
                
                document.getElementById('stu-name').value = item.studentName || '';
                document.getElementById('stu-age').value = item.stuAge || '';
                document.getElementById('stu-weight').value = item.stuWeight || '';
                document.getElementById('stu-height').value = item.stuHeight || '';
                document.getElementById('stu-gender').value = item.stuGender || 'Masculino';
                document.getElementById('stu-level').value = item.stuLevel || 'Iniciante';
                document.getElementById('stu-objective').value = item.stuObjective || 'Emagrecimento';
                document.getElementById('stu-freq').value = item.stuFreq || '3 dias';
                document.getElementById('stu-validity').value = item.stuValidity || '30 dias';
                document.getElementById('stu-recs').value = item.stuRecs || '';
                
                window.changeTheme();
                window.calculateIMC();
                window.updateObjText();

                document.querySelectorAll('.health-cb').forEach(cb => cb.checked = (item.health || []).includes(cb.value));

                window.state.workouts = item.workouts || [];
                renderWorkoutsApp();
                closeHistory();
            }
            hideLoader();
        };

        window.deleteHistoryItem = async (id) => {
            if(confirm("Excluir permanentemente da nuvem?")) {
                await deleteDoc(doc(db, "workouts", id));
                if(window.state.currentFichaId === id) window.state.currentFichaId = null;
                openHistory();
            }
        };

        // --- RELATÓRIO DE PRODUTIVIDADE ---
        document.getElementById('btn-report').addEventListener('click', () => {
            const mSel = document.getElementById('report-month');
            const uSel = document.getElementById('report-unit');
            
            // Gerar ultimos 6 meses
            let mHtml = '';
            for(let i=0; i<6; i++) {
                let d = new Date(); d.setMonth(d.getMonth()-i);
                let val = d.toISOString().substring(0,7);
                mHtml += `<option value="${val}">${val}</option>`;
            }
            mSel.innerHTML = mHtml;
            uSel.innerHTML = '<option value="ALL">Todas as Unidades</option>' + window.state.networkUnits.map(u => `<option value="${u.id}">${u.name}</option>`).join('');
            
            document.getElementById('report-content').innerHTML = '<p class="text-sm opacity-50 text-center mt-10">Clique em Gerar.</p>';
            document.getElementById('report-modal').classList.remove('hidden');
        });

        window.generateReport = async () => {
            showLoader();
            const month = document.getElementById('report-month').value;
            const unitId = document.getElementById('report-unit').value;
            
            let q;
            if(unitId === 'ALL') {
                // Simplificação: Filtra no client se pegar todos, ideal é ter índice.
                q = query(collection(db, "workouts"), where("monthYear", "==", month));
            } else {
                q = query(collection(db, "workouts"), where("monthYear", "==", month), where("unitId", "==", unitId));
            }

            const snap = await getDocs(q);
            const results = {}; // key: memberId, val: count
            
            snap.forEach(d => {
                const mid = d.data().memberId;
                results[mid] = (results[mid] || 0) + 1;
            });

            let html = `<table class="w-full text-sm mt-4"><tr class="bg-gray-800 text-white"><th class="p-2">Profissional</th><th class="p-2">Unidade</th><th class="p-2 text-center">Fichas Geradas</th></tr>`;
            
            let total = 0;
            Object.keys(results).forEach(mid => {
                const mem = window.state.networkMembers.find(m => m.id === mid);
                if(mem) {
                    const unitName = window.state.networkUnits.find(u => u.id === mem.unitId)?.name || 'N/A';
                    html += `<tr class="border-b border-gray-700"><td class="p-2 font-bold">${mem.name}</td><td class="p-2 opacity-70">${unitName}</td><td class="p-2 text-center text-primary font-bold">${results[mid]}</td></tr>`;
                    total += results[mid];
                }
            });

            html += `<tr class="bg-gray-800 text-white"><th colspan="2" class="p-2 text-right">TOTAL GERAL NO MÊS:</th><th class="p-2 text-center text-xl text-primary">${total}</th></tr></table>`;
            
            html += `<button onclick="window.print()" class="mt-6 btn-primary w-full py-2 rounded font-bold"><i class="fas fa-print"></i> Imprimir Relatório</button>`;

            document.getElementById('report-content').innerHTML = html;
            hideLoader();
        };


        // --- IMPRESSÃO A4 PLANILHA ---
        function executePrintHTML(d, m) {
            let imcStr = "-";
            if(d.stuWeight && d.stuHeight) imcStr = (d.stuWeight / (d.stuHeight * d.stuHeight)).toFixed(1);
            
            const objTxt = objectiveData[d.stuObjective] || "";
            const hData = m.category === 'PEF' ? healthDataPEF : healthDataTE;
            
            const profLabel = m.category === 'PEF' ? 'Profissional de Educação Física' : 'Treinador Esportivo';
            const crefText = m.category === 'PEF' ? `CREF: ${m.cref} - Estado: ${m.uf}` : `Estado: ${m.uf}`;
            
            const legalTextPEF = `⚠️ OBSERVAÇÃO LEGAL – PROFISSIONAL DE EDUCAÇÃO FÍSICA: Conforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º estabelece que compete ao profissional coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos especializados nas áreas de atividades físicas e do desporto.`;
            const legalTextTE = `⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO: Conforme a Lei nº 14.597/2023 (Lei Geral do Esporte), Art. 75, a profissão de treinador esportivo é reconhecida e regulada no Brasil, com atuação de caráter técnico e esportivo voltada à preparação, supervisão, orientação e acompanhamento de treinos. A atuação possui finalidade orientativa e não substitui avaliação médica. Em casos de doenças ou lesões, recomenda-se avaliação prévia por profissional habilitado. O treinamento proposto respeita os limites individuais, priorizando segurança.`;

            let html = `
                <div class="print-header">
                    <h1 class="print-title">PLANILHA DE TREINAMENTO - ${d.stuObjective}</h1>
                    <div class="prof-info">Prescrição feita por: ${m.name} (${profLabel}) | ${crefText}</div>
                </div>

                <div class="print-grid">
                    <div><strong>Aluno:</strong> ${d.studentName}</div>
                    <div><strong>Idade:</strong> ${d.stuAge} anos</div>
                    <div><strong>Gênero:</strong> ${d.stuGender}</div>
                    <div><strong>Validade:</strong> ${d.stuValidity.replace('Validade: ','')}</div>
                    
                    <div><strong>Peso:</strong> ${d.stuWeight} kg</div>
                    <div><strong>Altura:</strong> ${d.stuHeight} m</div>
                    <div><strong>IMC:</strong> ${imcStr}</div>
                    <div><strong>Nível:</strong> ${d.stuLevel} | <strong>Freq:</strong> ${d.stuFreq}</div>
                </div>
            `;

            if (objTxt || d.health.length > 0) {
                html += `<div class="print-guidelines"><h4>Diretrizes de Perfil</h4><ul>`;
                if(objTxt) html += `<li><strong>${d.stuObjective}:</strong> ${objTxt}</li>`;
                d.health.forEach(h => {
                    const cleanH = h.replace(/[\u{1F300}-\u{1F9FF}\u{2600}-\u{26FF}\u{2700}-\u{27BF}\u{FE0F}]/gu, '').trim();
                    if(hData[h]) html += `<li><strong>${cleanH}:</strong> ${hData[h]}</li>`;
                });
                html += `</ul></div>`;
            }

            d.workouts.forEach(w => {
                html += `<div class="print-workout"><h3>${w.title}</h3><table>
                            <thead><tr>
                                <th style="width:40%">Exercício</th><th style="width:8%; text-align:center;">Séries</th>
                                <th style="width:12%; text-align:center;">Reps</th><th style="width:15%">Técnica</th><th style="width:25%">Observações</th>
                            </tr></thead><tbody>`;
                if (w.exercises.length === 0) html += `<tr><td colspan="5" style="text-align:center;">Sem exercícios</td></tr>`;
                else {
                    w.exercises.forEach(ex => {
                        const imgTag = ex.img ? `<img src="${ex.img}" class="ex-img">` : ``;
                        html += `<tr>
                            <td><div class="ex-name-container">${imgTag}<strong>${ex.name}</strong></div></td>
                            <td style="text-align:center;">${ex.sets}</td><td style="text-align:center;">${ex.reps}</td>
                            <td>${ex.technique==='Nenhuma'?'-':ex.technique}</td><td>${ex.obs||'-'}</td>
                        </tr>`;
                    });
                }
                html += `</tbody></table></div>`;
            });

            html += `<div class="print-footer-section">`;
            if(d.stuRecs.trim()) html += `<strong>Recomendações do Profissional:</strong><p style="white-space: pre-wrap; margin:2px 0 8px 0;">${d.stuRecs}</p>`;
            html += `<div class="legal-text">${m.category==='PEF'?legalTextPEF:legalTextTE}</div>
                     <div style="text-align:center; margin-top:10px; font-size:7px;">Gerado em ${d.date} - PowFit Pro Master</div>
                     </div>`;

            document.getElementById('print-area').innerHTML = html;
            setTimeout(() => { window.print(); }, 400); // Atraso leve para renderizar imagens Base64
        }
    </script>
</body>
</html>
