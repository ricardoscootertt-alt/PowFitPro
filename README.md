<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro - Plataforma Profissional</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg-body: #0f172a; --bg-card: #1e293b; --text-main: #f8fafc; --text-muted: #94a3b8;
            --border-color: #334155; --primary: #3b82f6; --primary-hover: #2563eb;
            --input-bg: #0f172a; --input-border: #475569;
        }

        [data-theme="Feminino"] {
            --bg-body: #fdf2f8; --bg-card: #ffffff; --text-main: #1e293b; --text-muted: #64748b;
            --border-color: #fbcfe8; --primary: #ec4899; --primary-hover: #db2777;
            --input-bg: #f8fafc; --input-border: #f1f5f9;
        }

        body { font-family: 'Inter', sans-serif; background-color: var(--bg-body); color: var(--text-main); transition: all 0.3s ease; }
        .card { background-color: var(--bg-card); border: 1px solid var(--border-color); }
        .input-field { background-color: var(--input-bg); border: 1px solid var(--input-border); color: var(--text-main); }
        .input-field:focus { outline: none; border-color: var(--primary); box-shadow: 0 0 0 1px var(--primary); }
        .btn-primary { background-color: var(--primary); color: white; transition: 0.2s; }
        .btn-primary:hover { background-color: var(--primary-hover); }
        .text-primary { color: var(--primary); }
        
        /* Utilitários de UI */
        .hidden { display: none !important; }
        .loader { border: 3px solid rgba(255,255,255,0.3); border-radius: 50%; border-top: 3px solid white; width: 20px; height: 20px; animation: spin 1s linear infinite; }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }

        /* Scrollbar */
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }

        /* Estilos de Impressão (Planilha Profissional A4) */
        #print-area { display: none; }
        @media print {
            body { background: white !important; color: black !important; margin: 0; padding: 0; }
            #app-container, .modal, .no-print { display: none !important; }
            #print-area { display: block !important; padding: 10mm; font-family: 'Arial', sans-serif; font-size: 10px; }
            @page { size: A4; margin: 0; }
            
            .print-header { border-bottom: 2px solid #000; padding-bottom: 5px; margin-bottom: 10px; text-align: center; }
            .print-title { font-size: 16px; font-weight: bold; text-transform: uppercase; margin: 0;}
            .prof-info { font-size: 11px; margin-top: 3px; font-weight: bold; }
            
            .print-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 2px; margin-bottom: 10px; border: 1px solid #000; padding: 5px; }
            .print-grid div { font-size: 10px; }
            
            .print-guidelines { border: 1px solid #000; padding: 5px; margin-bottom: 10px; font-size: 9px; }
            .print-guidelines h4 { margin: 0 0 2px 0; font-size: 10px; text-transform: uppercase; background: #eee; padding: 2px; }
            .print-guidelines ul { margin: 0; padding-left: 15px; }
            .print-guidelines li { margin-bottom: 2px; }

            .print-workout { margin-bottom: 10px; page-break-inside: avoid; }
            .print-workout h3 { background: #e0e0e0; border: 1px solid #000; border-bottom: none; padding: 3px 5px; margin: 0; font-size: 11px; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            table { width: 100%; border-collapse: collapse; border: 1px solid #000; }
            th, td { border: 1px solid #000; padding: 3px 4px; text-align: left; font-size: 9px; }
            th { background-color: #f0f0f0; font-weight: bold; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            
            .print-footer-section { margin-top: 10px; border: 1px solid #000; padding: 5px; font-size: 9px; page-break-inside: avoid; }
            .legal-text { font-size: 8px; color: #333; text-align: justify; margin-top: 5px; font-style: italic; }
        }
    </style>
</head>
<body data-theme="Masculino">

    <!-- OVERLAY DE CARREGAMENTO GLOBAL -->
    <div id="global-loader" class="fixed inset-0 bg-black bg-opacity-80 z-[100] flex flex-col items-center justify-center hidden">
        <div class="loader mb-4 w-10 h-10 border-4"></div>
        <p class="text-white font-medium" id="loader-text">Carregando sistema...</p>
    </div>

    <!-- TELA DE LOGIN -->
    <div id="login-screen" class="min-h-screen flex items-center justify-center p-4">
        <div class="card p-8 rounded-2xl shadow-2xl max-w-md w-full text-center">
            <i class="fas fa-dumbbell text-5xl text-primary mb-4"></i>
            <h1 class="text-3xl font-bold mb-2">PowFit Pro</h1>
            <p class="opacity-70 mb-8 text-sm">Plataforma Profissional para Gestão de Treinos e Franquias</p>
            <button id="btn-login" class="w-full btn-primary py-3 rounded-lg font-bold flex items-center justify-center gap-2">
                <i class="fab fa-google"></i> Entrar com Google
            </button>
            <p class="text-xs opacity-50 mt-4">Acesso exclusivo para profissionais.</p>
        </div>
    </div>

    <!-- TELA DE SETUP DA REDE (Primeiro Acesso) -->
    <div id="setup-screen" class="min-h-screen flex items-center justify-center p-4 hidden">
        <div class="card p-8 rounded-2xl shadow-2xl max-w-lg w-full">
            <h2 class="text-2xl font-bold mb-2"><i class="fas fa-building text-primary"></i> Configuração da Rede</h2>
            <p class="opacity-70 mb-6 text-sm">Crie sua estrutura principal. Você poderá adicionar mais unidades depois.</p>
            
            <div class="space-y-4">
                <div>
                    <label class="block text-sm font-medium mb-1">Nome da Rede (Sua Marca)</label>
                    <input type="text" id="setup-network-name" class="input-field w-full rounded-lg px-3 py-2" placeholder="Ex: Power Fitness">
                </div>
                <div>
                    <label class="block text-sm font-medium mb-1">Nome da 1ª Unidade</label>
                    <input type="text" id="setup-unit-name" class="input-field w-full rounded-lg px-3 py-2" placeholder="Ex: Matriz Centro">
                </div>
                <button id="btn-save-setup" class="w-full btn-primary py-3 rounded-lg font-bold mt-4">
                    Concluir Setup Inicial
                </button>
            </div>
        </div>
    </div>

    <!-- APP PRINCIPAL -->
    <div id="app-screen" class="hidden min-h-screen pb-20">
        <!-- Barra de Navegação Superior (Gestão) -->
        <nav class="bg-black bg-opacity-20 border-b border-opacity-20 backdrop-blur-md sticky top-0 z-40" style="border-color: var(--border-color)">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-3 flex flex-wrap justify-between items-center gap-4">
                <div class="flex items-center gap-3">
                    <i class="fas fa-dumbbell text-2xl text-primary"></i>
                    <div>
                        <h1 class="font-bold leading-tight" id="nav-network-name">Rede</h1>
                        <p class="text-[10px] opacity-70 uppercase tracking-widest">Painel de Gestão</p>
                    </div>
                </div>
                
                <div class="flex items-center gap-2 w-full sm:w-auto">
                    <!-- Seletores Hierárquicos -->
                    <select id="nav-select-unit" class="input-field rounded px-2 py-1.5 text-xs font-medium max-w-[150px]">
                        <!-- Populated via JS -->
                    </select>
                    <select id="nav-select-member" class="input-field rounded px-2 py-1.5 text-xs font-medium max-w-[150px]">
                        <!-- Populated via JS -->
                    </select>
                    
                    <div class="h-6 w-px bg-gray-500 bg-opacity-30 mx-1 hidden sm:block"></div>
                    
                    <!-- Ações Rápidas -->
                    <button onclick="openModal('modal-manage')" class="text-xs px-3 py-1.5 rounded hover:bg-black hover:bg-opacity-20 transition" title="Gerenciar Equipe/Unidades">
                        <i class="fas fa-users-cog"></i> <span class="hidden md:inline">Gerenciar</span>
                    </button>
                    <button onclick="openReports()" class="text-xs px-3 py-1.5 rounded hover:bg-black hover:bg-opacity-20 transition" title="Relatórios">
                        <i class="fas fa-chart-bar"></i> <span class="hidden md:inline">Relatórios</span>
                    </button>
                    <button id="btn-logout" class="text-xs px-3 py-1.5 rounded text-red-400 hover:bg-red-500 hover:bg-opacity-10 transition">
                        <i class="fas fa-sign-out-alt"></i> Sair
                    </button>
                </div>
            </div>
        </nav>

        <!-- Container Principal -->
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-6">
            
            <div class="flex justify-between items-center mb-6">
                <h2 class="text-xl font-bold"><i class="fas fa-clipboard-list text-primary"></i> Criador de Fichas</h2>
                <div class="flex gap-2">
                    <button onclick="openHistory()" class="bg-gray-700 hover:bg-gray-600 text-white px-4 py-2 rounded-lg text-sm font-medium shadow flex items-center gap-2 transition">
                        <i class="fas fa-cloud"></i> Fichas Salvas
                    </button>
                    <button onclick="generatePrint()" class="btn-primary px-5 py-2 rounded-lg text-sm font-bold shadow-lg flex items-center gap-2">
                        <i class="fas fa-save"></i> Salvar & Imprimir
                    </button>
                </div>
            </div>

            <div class="grid grid-cols-1 xl:grid-cols-12 gap-6">
                <!-- COLUNA ESQUERDA: Dados -->
                <div class="xl:col-span-4 space-y-6">
                    <!-- Alerta de Seleção de Profissional -->
                    <div id="alert-no-member" class="bg-red-500 bg-opacity-10 border border-red-500 text-red-400 p-3 rounded-lg text-sm hidden">
                        <i class="fas fa-exclamation-triangle"></i> Selecione um Treinador no menu superior antes de criar a ficha.
                    </div>

                    <!-- Dados do Aluno -->
                    <div class="card rounded-xl p-5 shadow-sm">
                        <div class="flex justify-between items-center mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                            <h3 class="font-semibold flex items-center gap-2"><i class="fas fa-user text-primary"></i> Aluno</h3>
                            <div id="imc-display"></div>
                        </div>
                        <div class="space-y-4">
                            <div>
                                <label class="block text-xs font-medium mb-1">Nome</label>
                                <input type="text" id="stu-name" class="input-field w-full rounded p-2 text-sm" placeholder="Nome completo">
                            </div>
                            <div class="grid grid-cols-3 gap-2">
                                <div>
                                    <label class="block text-xs font-medium mb-1">Idade</label>
                                    <input type="number" id="stu-age" class="input-field w-full rounded p-2 text-sm" placeholder="Anos">
                                </div>
                                <div>
                                    <label class="block text-xs font-medium mb-1">Peso(kg)</label>
                                    <input type="number" id="stu-weight" oninput="calculateIMC()" class="input-field w-full rounded p-2 text-sm">
                                </div>
                                <div>
                                    <label class="block text-xs font-medium mb-1">Altura(m)</label>
                                    <input type="number" id="stu-height" step="0.01" oninput="calculateIMC()" class="input-field w-full rounded p-2 text-sm">
                                </div>
                            </div>
                            <div class="grid grid-cols-2 gap-2">
                                <div>
                                    <label class="block text-xs font-medium mb-1">Gênero</label>
                                    <select id="stu-gender" onchange="changeTheme()" class="input-field w-full rounded p-2 text-sm">
                                        <option value="Masculino">Masculino</option>
                                        <option value="Feminino">Feminino</option>
                                    </select>
                                </div>
                                <div>
                                    <label class="block text-xs font-medium mb-1">Nível</label>
                                    <select id="stu-level" class="input-field w-full rounded p-2 text-sm">
                                        <option>Iniciante</option><option>Intermediário</option><option>Avançado</option>
                                    </select>
                                </div>
                            </div>
                            <div>
                                <label class="block text-xs font-medium mb-1">Objetivo (Gera recomendação)</label>
                                <select id="stu-objective" class="input-field w-full rounded p-2 text-sm">
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
                        </div>
                    </div>

                    <!-- Estado de Saúde -->
                    <div class="card rounded-xl p-5 shadow-sm">
                        <h3 class="font-semibold mb-1 flex items-center gap-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                            <i class="fas fa-notes-medical text-primary"></i> Estado de Saúde
                        </h3>
                        <p class="text-[10px] opacity-70 mb-3">As diretrizes mudam com base no tipo do profissional selecionado (PEF ou TE).</p>
                        <div class="grid grid-cols-2 gap-1 text-[11px]" id="health-container">
                            <!-- JS popula baseado no membro selecionado -->
                        </div>
                    </div>

                    <!-- Recomendações e Configs -->
                    <div class="card rounded-xl p-5 shadow-sm space-y-4">
                        <div class="grid grid-cols-2 gap-2">
                            <div>
                                <label class="block text-xs font-medium mb-1">Frequência</label>
                                <select id="stu-freq" class="input-field w-full rounded p-2 text-sm">
                                    <option>3 dias</option><option>5 dias</option><option>6 dias</option><option>Personalizado</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs font-medium mb-1">Validade</label>
                                <select id="stu-validity" class="input-field w-full rounded p-2 text-sm">
                                    <option>15 dias</option><option selected>30 dias</option><option>60 dias</option><option>90 dias</option>
                                </select>
                            </div>
                        </div>
                        <div>
                            <label class="block text-xs font-medium mb-1">Recomendações Livres</label>
                            <textarea id="stu-recs" rows="3" class="input-field w-full rounded p-2 text-sm" placeholder="Hidratação, descanso..."></textarea>
                        </div>
                    </div>
                </div>

                <!-- COLUNA DIREITA: Treinos -->
                <div class="xl:col-span-8 space-y-4">
                    <div class="flex justify-between items-center p-3 rounded-xl bg-primary bg-opacity-10 border border-primary border-opacity-20">
                        <span class="text-sm font-medium"><i class="fas fa-info-circle"></i> O sistema não sugere treinos. A montagem é totalmente manual.</span>
                        <button onclick="addWorkout()" class="btn-primary px-3 py-1.5 rounded text-xs font-bold">
                            + Dia de Treino
                        </button>
                    </div>

                    <div id="workouts-container" class="space-y-4">
                        <!-- Treinos populados via JS -->
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- MODAL: GERENCIAMENTO DE REDE E EQUIPE -->
    <div id="modal-manage" class="modal fixed inset-0 bg-black bg-opacity-70 backdrop-blur-sm hidden z-[60] flex items-center justify-center p-4">
        <div class="card w-full max-w-3xl rounded-xl shadow-2xl flex flex-col max-h-[90vh]">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold"><i class="fas fa-sitemap text-primary mr-2"></i> Gerenciar Unidades e Equipe</h3>
                <button onclick="closeModal('modal-manage')" class="text-gray-400 hover:text-white text-2xl">&times;</button>
            </div>
            <div class="p-4 overflow-y-auto flex-1 grid grid-cols-1 md:grid-cols-2 gap-6">
                
                <!-- Nova Unidade -->
                <div class="space-y-3 p-4 bg-black bg-opacity-10 rounded-lg">
                    <h4 class="font-semibold text-sm border-b border-gray-600 pb-1">Adicionar Nova Unidade</h4>
                    <input type="text" id="new-unit-name" class="input-field w-full rounded p-2 text-sm" placeholder="Nome da Unidade">
                    <button onclick="addNewUnit()" class="btn-primary w-full py-2 rounded text-sm font-medium">Salvar Unidade</button>
                </div>

                <!-- Novo Membro (Treinador) -->
                <div class="space-y-3 p-4 bg-black bg-opacity-10 rounded-lg">
                    <h4 class="font-semibold text-sm border-b border-gray-600 pb-1">Cadastrar Profissional</h4>
                    <select id="new-member-unit" class="input-field w-full rounded p-2 text-sm">
                        <!-- Unidades -->
                    </select>
                    <select id="new-member-type" onchange="toggleNewMemberCref()" class="input-field w-full rounded p-2 text-sm">
                        <option value="PEF">Profissional de Educação Física (PEF)</option>
                        <option value="TE">Treinador Esportivo (TE)</option>
                    </select>
                    <input type="text" id="new-member-name" class="input-field w-full rounded p-2 text-sm" placeholder="Nome do Profissional">
                    <div id="new-member-cref-container" class="grid grid-cols-2 gap-2">
                        <input type="text" id="new-member-cref" class="input-field w-full rounded p-2 text-sm" placeholder="CREF">
                        <input type="text" id="new-member-state" class="input-field w-full rounded p-2 text-sm" placeholder="UF">
                    </div>
                    <button onclick="addNewMember()" class="btn-primary w-full py-2 rounded text-sm font-medium">Salvar Profissional</button>
                </div>

            </div>
        </div>
    </div>

    <!-- MODAL: EXERCÍCIOS -->
    <div id="modal-exercises" class="modal fixed inset-0 bg-black bg-opacity-70 backdrop-blur-sm hidden z-[60] flex items-center justify-center p-2 sm:p-4">
        <div class="card w-full max-w-5xl rounded-xl shadow-2xl flex flex-col h-[90vh]">
            <div class="p-3 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-md font-bold"><i class="fas fa-search text-primary mr-2"></i> Adicionar Exercício</h3>
                <button onclick="closeModal('modal-exercises')" class="text-gray-400 hover:text-white text-2xl">&times;</button>
            </div>
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <div class="w-full md:w-1/4 border-r overflow-y-auto p-2 space-y-1" style="border-color: var(--border-color)" id="modal-cats"></div>
                <div class="w-full md:w-3/4 overflow-y-auto p-3 bg-black bg-opacity-5" id="modal-ex-list"></div>
            </div>
        </div>
    </div>

    <!-- MODAL: HISTÓRICO E RELATÓRIOS -->
    <div id="modal-history" class="modal fixed inset-0 bg-black bg-opacity-70 backdrop-blur-sm hidden z-[60] flex items-center justify-center p-4">
        <div class="card w-full max-w-4xl rounded-xl shadow-2xl flex flex-col max-h-[90vh]">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold" id="history-title"><i class="fas fa-cloud text-primary mr-2"></i> Fichas Salvas (Rede)</h3>
                <div class="flex gap-2">
                    <button onclick="loadReportMode()" id="btn-toggle-report" class="bg-gray-700 px-3 py-1 text-xs rounded">Modo Relatório</button>
                    <button onclick="closeModal('modal-history')" class="text-gray-400 hover:text-white text-2xl">&times;</button>
                </div>
            </div>
            <div class="p-4 overflow-y-auto flex-1" id="history-content">
                <!-- Populado JS -->
            </div>
        </div>
    </div>

    <!-- ÁREA DE IMPRESSÃO (Oculta na tela) -->
    <div id="print-area"></div>

    <!-- FIREBASE E LÓGICA DA APLICAÇÃO -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-app.js";
        import { getAuth, signInWithPopup, GoogleAuthProvider, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, addDoc, query, where, getDocs, orderBy, deleteDoc } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-firestore.js";

        // --- 1. CONFIGURAÇÃO FIREBASE ---
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

        // --- 2. DADOS ESTÁTICOS (REGRAS E EXERCÍCIOS) ---
        const D = {
            healthPEF: { "🟢 Saudável": "Manter treinos regulares 3–5x por semana, combinando musculação e cardio.", "⚪ Sedentário": "Iniciar treinos leves 2–3x. Priorizar adaptação.", "🟡 Sobrepeso": "Treinar 3–5x foco em gasto calórico.", "🔴 Obesidade": "Exercícios de baixo impacto. Evolução gradual.", "⚖️ Baixo peso": "Foco em musculação para ganho de massa.", "🍬 Diabetes": "Monitorar glicemia. Evitar jejum.", "❤️ Hipertensão": "Evitar prender respiração. Controle de intensidade.", "🔵 Hipotensão": "Evitar mudanças bruscas. Hidratação.", "💔 Problemas cardíacos": "Liberação médica essencial. Monitorar FC.", "🦴 Problemas articulares": "Evitar impacto.", "🫁 Problemas respiratórios": "Progressão gradual.", "⚠️ Lesões": "Adaptar exercícios. Foco recuperação.", "🤰 Gestante": "Liberação médica. Sem impacto.", "🤱 Lactante": "Treino normal com hidratação.", "👴 Idoso": "Força, equilíbrio e mobilidade." },
            healthTE: { "🟢 Saudável": "Manter constância 3-5x para melhores resultados esportivos.", "⚪ Sedentário": "Início gradual. Foco na execução motora correta.", "🟡 Sobrepeso": "Treino progressivo respeitando nível de adaptação.", "🔴 Obesidade": "Segurança e mobilidade são prioridades.", "⚖️ Baixo peso": "Treino focado em evolução progressiva.", "🍬 Diabetes": "Acompanhamento médico. Interromper em mal-estar.", "❤️ Hipertensão": "Evitar Manobra de Valsalva. Respeitar limites.", "🔵 Hipotensão": "Cuidado nas trocas de posições.", "💔 Problemas cardíacos": "Apenas com liberação médica.", "🦴 Problemas articulares": "Movimentos controlados recomendados.", "🫁 Problemas respiratórios": "Respeitar capacidade aeróbica.", "⚠️ Lesões": "Seguir orientação profissional. Evitar dor.", "🤰 Gestante": "Foco em segurança e bem-estar (com aval médico).", "🤱 Lactante": "Respeitar recuperação.", "👴 Idoso": "Intensidade moderada. Segurança garantida." },
            objectives: { "Emagrecimento": "Déficit calórico + Força e Cardio.", "Hipertrofia": "Progressão de carga e superávit calórico.", "Definição": "Manutenção muscular e redução de gordura.", "Condicionamento": "Treinos integrados e circuitos.", "Resistência": "Séries longas e cadência controlada.", "Força": "Cargas altas, reps baixas, descansos longos.", "Reabilitação": "Fortalecimento específico.", "Saúde geral": "Equilíbrio e constância." },
            categories: {
                "🔥 PEITO": ["Supino Reto", "Supino Inclinado", "Supino com Halteres", "Supino Fechado com Halteres", "Cross Over", "Cross Over Alto", "Cross Over Baixo", "Crucifixo Reto", "Crucifixo Inclinado", "Crucifixo na Máquina", "Peck Fly", "Peck Fly Unilateral", "Pullover", "Flexão de Braço", "Flexão com Pés Elevados", "Flexão Explosiva"],
                "🦍 COSTAS": ["Puxada Alta", "Puxada Unilateral", "Pulldown", "Remada Aberta", "Remada Baixa", "Remada Curvada", "Remada Curvada na Polia", "Remada Unilateral", "Remada Cavalinho (T-Bar)", "Remada no Cross", "Serrote", "Facepull (puxada de cima para baixo)", "Encolhimento (Trapézio)"],
                "🦵 PERNAS": ["Agachamento Livre", "Agachamento Taça", "Agachamento com Barra", "Agachamento no Smith", "Squat", "Hack Machine", "Leg 45°", "Leg 90°", "Agachamento Sumô", "Agachamento Sissy (Livre)", "Afundo", "Recuo", "Avanço", "Passada", "Búlgaro", "Step-up", "Levantamento Terra", "Levantamento Terra Romeno", "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", "Cadeira Flexora", "Elevação Pélvica no Banco", "Elevação Pélvica no Chão", "Elevação Pélvica Unilateral no Chão", "Extensão de Quadril (Glúteo Máximo)", "Extensão Cruzada (Glúteo Médio)", "Abdução no Cross (Glúteo Médio + Mínimo)", "Coice", "Cachorrinho", "Caranguejo", "Cadeira Extensora", "Adução", "Abdução", "Cadeira Abdutora Inclinada", "Flexão Nórdica", "Flexão Nórdica Invertida", "Panturrilha em Pé (Máquina)", "Panturrilha Livre", "Panturrilha no Leg Press", "Panturrilha Banco", "Panturrilha Squat", "Panturrilha Unilateral"],
                "💪 BRAÇOS": ["(Bíceps) Rosca Direta", "(Bíceps) Rosca Alternada", "(Bíceps) Rosca 21", "(Bíceps) Rosca Scott Barra W", "(Bíceps) Rosca Scott Unilateral", "(Bíceps) Rosca Scott com Halteres", "(Bíceps) Rosca Martelo", "(Bíceps) Rosca Cross", "(Bíceps) Rosca Concentrada", "(Bíceps) Rosca Inversa", "(Bíceps) Rosca 45°", "(Tríceps) Pulley Unilateral", "(Tríceps) Pulley Barra", "(Tríceps) Pulley Corda", "(Tríceps) Pulley Pegada Inversa", "(Tríceps) Francês na Corda", "(Tríceps) Francês com Halter", "(Tríceps) Francês Unilateral", "(Tríceps) Cruzado Polia Dupla", "(Tríceps) Coice Unilateral", "(Tríceps) Arremesso", "(Tríceps) Testa", "(Tríceps) Mergulho no Banco"],
                "🪨 OMBROS": ["Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", "Elevação Lateral na Polia", "Elevação Lateral Sentado", "Desenvolvimento com Halteres", "Desenvolvimento com Barra", "Arnold Press", "Elevação Borboleta", "Crucifixo Inverso Sentado com Halteres", "Crucifixo Inverso na Polia", "Crucifixo Inverso Unilateral na Polia", "Facepull (puxada reta)", "Remada Alta"],
                "🧠 ABDÔMEN": ["Infra com Elevação de Perna", "Abdominal Supra", "Abdominal Remador", "Abdominal Bicicleta", "Abdominal Twister com Peso", "Prancha", "Prancha Lateral", "Trituração de Cabos em Pé", "Isometria na parede", "Abdominal isometrico"],
                "🫀 CARDIO": ["Bicicleta - 10 Min", "Bicicleta - 15 Min", "Bicicleta - 20 Min", "Esteira - 10 Min", "Esteira - 15 Min", "Esteira - 20 Min", "Pular Corda - 10 Min", "Pular Corda - 15 Min", "Pular Corda - 20 Min"]
            },
            techniques: ["Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", "Negativa", "Isometria", "Parciais", "Pirâmide"],
            days: ["SEGUNDA-FEIRA", "TERÇA-FEIRA", "QUARTA-FEIRA", "QUINTA-FEIRA", "SEXTA-FEIRA", "SÁBADO", "DOMINGO"],
            legalPEF: "⚠️ OBSERVAÇÃO LEGAL – PROFISSIONAL DE EDUCAÇÃO FÍSICA: Conforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º estabelece que compete ao profissional coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos.",
            legalTE: "⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO: Conforme a Lei nº 14.597/2023 (Lei Geral do Esporte), Art. 75, a profissão de treinador esportivo é reconhecida e regulada no Brasil. As recomendações desta ficha possuem caráter informativo e orientativo. Não substitui avaliação médica. Em casos de condições específicas de saúde, recomenda-se avaliação médica prévia. O treinamento prioriza segurança e execução correta."
        };

        // --- 3. ESTADO GLOBAL ---
        window.STATE = {
            user: null, network: null, members: [], units: [], currentFichaId: null,
            workouts: [], activeWorkoutId: null, activeCategory: "🔥 PEITO"
        };

        // --- 4. FUNÇÕES DE UTILIDADE E UI ---
        const $ = id => document.getElementById(id);
        const showLoader = (text="Carregando...") => { $('loader-text').innerText = text; $('global-loader').classList.remove('hidden'); };
        const hideLoader = () => $('global-loader').classList.add('hidden');
        const genId = () => Math.random().toString(36).substr(2, 9);
        
        window.openModal = id => $(id).classList.remove('hidden');
        window.closeModal = id => $(id).classList.add('hidden');
        window.toggleNewMemberCref = () => {
            const t = $('new-member-type').value;
            $('new-member-cref-container').style.display = t === 'TE' ? 'none' : 'grid';
        };
        window.changeTheme = () => document.body.setAttribute('data-theme', $('stu-gender').value);
        window.calculateIMC = () => {
            const w = parseFloat($('stu-weight').value), h = parseFloat($('stu-height').value);
            if(w>0 && h>0) {
                const imc = (w/(h*h)).toFixed(1);
                let cls = imc<18.5?"Abaixo":imc<24.9?"Normal":imc<29.9?"Sobrepeso":"Obesidade";
                $('imc-display').innerHTML = `<span class="bg-primary bg-opacity-20 text-primary px-2 py-1 rounded text-xs">IMC: ${imc} (${cls})</span>`;
            } else $('imc-display').innerHTML = '';
        };

        // --- 5. AUTENTICAÇÃO E SETUP DE REDE ---
        onAuthStateChanged(auth, async (user) => {
            if (user) {
                STATE.user = user;
                $('login-screen').classList.add('hidden');
                showLoader("Carregando sua Rede...");
                await loadNetworkData();
                hideLoader();
            } else {
                $('login-screen').classList.remove('hidden');
                $('setup-screen').classList.add('hidden');
                $('app-screen').classList.add('hidden');
            }
        });

        $('btn-login').onclick = () => signInWithPopup(auth, new GoogleAuthProvider()).catch(e=>alert("Erro login: "+e.message));
        $('btn-logout').onclick = () => signOut(auth);

        async function loadNetworkData() {
            try {
                const netDoc = await getDoc(doc(db, "networks", STATE.user.uid));
                if (!netDoc.exists()) {
                    $('setup-screen').classList.remove('hidden');
                } else {
                    STATE.network = netDoc.data();
                    STATE.units = STATE.network.units || [];
                    
                    // Fetch Members
                    const memQ = query(collection(db, "members"), where("networkId", "==", STATE.user.uid));
                    const memSnap = await getDocs(memQ);
                    STATE.members = memSnap.docs.map(d => ({id: d.id, ...d.data()}));
                    
                    initAppUI();
                }
            } catch (e) { alert("Erro ao carregar rede: " + e.message); }
        }

        $('btn-save-setup').onclick = async () => {
            const netName = $('setup-network-name').value.trim();
            const unitName = $('setup-unit-name').value.trim();
            if(!netName || !unitName) return alert("Preencha ambos os campos.");
            
            showLoader("Criando estrutura...");
            const initUnit = { id: genId(), name: unitName };
            await setDoc(doc(db, "networks", STATE.user.uid), {
                name: netName, ownerEmail: STATE.user.email, units: [initUnit], createdAt: new Date()
            });
            $('setup-screen').classList.add('hidden');
            await loadNetworkData();
            hideLoader();
        };

        // --- 6. GERENCIAMENTO DA EQUIPE (MODAL) ---
        window.addNewUnit = async () => {
            const name = $('new-unit-name').value.trim();
            if(!name) return;
            showLoader("Salvando...");
            const newUnit = { id: genId(), name };
            STATE.units.push(newUnit);
            await setDoc(doc(db, "networks", STATE.user.uid), { units: STATE.units }, {merge: true});
            $('new-unit-name').value = '';
            updateNavSelects();
            hideLoader();
        };

        window.addNewMember = async () => {
            const name = $('new-member-name').value.trim();
            const type = $('new-member-type').value;
            const unitId = $('new-member-unit').value;
            const cref = type === 'PEF' ? $('new-member-cref').value.trim() : '';
            const stateUf = type === 'PEF' ? $('new-member-state').value.trim() : '';
            
            if(!name || !unitId) return alert("Nome e Unidade obrigatórios.");
            if(type === 'PEF' && (!cref || !stateUf)) return alert("CREF e Estado obrigatórios para PEF.");

            showLoader("Salvando profissional...");
            const memberData = { networkId: STATE.user.uid, unitId, name, type, cref, state: stateUf, active: true };
            const docRef = await addDoc(collection(db, "members"), memberData);
            STATE.members.push({id: docRef.id, ...memberData});
            
            $('new-member-name').value = ''; $('new-member-cref').value = ''; $('new-member-state').value = '';
            updateNavSelects();
            hideLoader();
            closeModal('modal-manage');
        };

        // --- 7. LÓGICA PRINCIPAL DA APLICAÇÃO ---
        function initAppUI() {
            $('app-screen').classList.remove('hidden');
            $('nav-network-name').innerText = STATE.network.name;
            updateNavSelects();
            $('nav-select-member').addEventListener('change', updateHealthOptions);
            if(STATE.workouts.length === 0) window.addWorkout(); // Adiciona 1 dia default
        }

        function updateNavSelects() {
            const sUnit = $('nav-select-unit'); const sMem = $('nav-select-member'); const sNewMemUnit = $('new-member-unit');
            sUnit.innerHTML = STATE.units.map(u => `<option value="${u.id}">${u.name}</option>`).join('');
            sNewMemUnit.innerHTML = sUnit.innerHTML;

            const updateMembers = () => {
                const uId = sUnit.value;
                const unitMembers = STATE.members.filter(m => m.unitId === uId);
                if(unitMembers.length === 0) {
                    sMem.innerHTML = `<option value="">-- Sem profissionais --</option>`;
                    $('alert-no-member').classList.remove('hidden');
                } else {
                    sMem.innerHTML = unitMembers.map(m => `<option value="${m.id}">${m.name} (${m.type})</option>`).join('');
                    $('alert-no-member').classList.add('hidden');
                }
                updateHealthOptions();
            };
            sUnit.addEventListener('change', updateMembers);
            updateMembers(); // Trigger inicial
        }

        function updateHealthOptions() {
            const memId = $('nav-select-member').value;
            const member = STATE.members.find(m => m.id === memId);
            const dataObj = (member && member.type === 'TE') ? D.healthTE : D.healthPEF;
            
            const cont = $('health-container');
            const selected = Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value);
            
            cont.innerHTML = Object.keys(dataObj).map(opt => {
                const checked = selected.includes(opt) ? 'checked' : '';
                return `<label class="flex items-start space-x-1 cursor-pointer hover:bg-black hover:bg-opacity-10 p-1 rounded">
                    <input type="checkbox" value="${opt}" ${checked} class="health-cb mt-0.5 rounded border-gray-400">
                    <span class="truncate" title="${dataObj[opt]}">${opt}</span>
                </label>`;
            }).join('');
        }

        // --- 8. GERENCIADOR DE TREINOS (DIAS E EXERCÍCIOS) ---
        window.addWorkout = () => {
            const title = STATE.workouts.length < 7 ? `TREINO ${D.days[STATE.workouts.length]}` : `NOVO TREINO ${STATE.workouts.length+1}`;
            STATE.workouts.push({ id: genId(), title, exercises: [] });
            renderWorkouts();
        };

        window.duplicateWorkout = (id) => {
            const w = STATE.workouts.find(x => x.id === id);
            if(w) { STATE.workouts.push({ ...JSON.parse(JSON.stringify(w)), id: genId(), title: w.title+"(Cópia)" }); renderWorkouts(); }
        };

        window.removeWorkout = (id) => {
            if(confirm("Excluir este dia?")) { STATE.workouts = STATE.workouts.filter(w => w.id !== id); renderWorkouts(); }
        };

        window.updateWorkoutTitle = (id, val) => { const w = STATE.workouts.find(x => x.id === id); if(w) w.title = val; };

        // Manipulação de Exercícios
        window.removeEx = (wId, idx) => { STATE.workouts.find(w=>w.id===wId).exercises.splice(idx,1); renderWorkouts(); };
        window.moveEx = (wId, idx, dir) => {
            const arr = STATE.workouts.find(w=>w.id===wId).exercises;
            if(dir==='up' && idx>0) { [arr[idx], arr[idx-1]] = [arr[idx-1], arr[idx]]; }
            if(dir==='down' && idx<arr.length-1) { [arr[idx], arr[idx+1]] = [arr[idx+1], arr[idx]]; }
            renderWorkouts();
        };
        window.updateEx = (wId, idx, fld, val) => { STATE.workouts.find(w=>w.id===wId).exercises[idx][fld] = val; };

        function renderWorkouts() {
            const container = $('workouts-container'); container.innerHTML = '';
            STATE.workouts.forEach(w => {
                let exs = w.exercises.length === 0 ? `<div class="text-center p-3 text-xs opacity-50">Vazio</div>` : w.exercises.map((ex, i) => `
                    <div class="flex gap-2 p-1 border-b border-opacity-10 items-center text-xs" style="border-color:var(--border-color)">
                        <div class="flex flex-col"><button onclick="moveEx('${w.id}',${i},'up')"><i class="fas fa-caret-up"></i></button><button onclick="moveEx('${w.id}',${i},'down')"><i class="fas fa-caret-down"></i></button></div>
                        <div class="w-1/3 truncate"><span class="opacity-50 text-[9px] uppercase block leading-none">${ex.category}</span><strong title="${ex.name}">${ex.name}</strong></div>
                        <input type="text" value="${ex.sets}" onchange="updateEx('${w.id}',${i},'sets',this.value)" class="input-field w-8 text-center rounded p-1" title="Séries">x
                        <input type="text" value="${ex.reps}" onchange="updateEx('${w.id}',${i},'reps',this.value)" class="input-field w-12 text-center rounded p-1" title="Reps">
                        <select onchange="updateEx('${w.id}',${i},'technique',this.value)" class="input-field w-20 rounded p-1 text-[10px]">
                            ${D.techniques.map(t=>`<option ${ex.technique===t?'selected':''}>${t}</option>`).join('')}
                        </select>
                        <input type="text" value="${ex.obs}" onchange="updateEx('${w.id}',${i},'obs',this.value)" class="input-field flex-1 rounded p-1" placeholder="Obs">
                        <button onclick="removeEx('${w.id}',${i})" class="text-red-500 hover:text-red-400 p-1"><i class="fas fa-times"></i></button>
                    </div>
                `).join('');

                container.innerHTML += `
                    <div class="card rounded-lg overflow-hidden border border-gray-600 border-opacity-30">
                        <div class="p-2 bg-black bg-opacity-20 flex justify-between items-center">
                            <input type="text" value="${w.title}" onchange="updateWorkoutTitle('${w.id}',this.value)" class="input-field font-bold text-sm bg-transparent px-1 w-1/2">
                            <div><button onclick="duplicateWorkout('${w.id}')" class="px-2"><i class="fas fa-copy"></i></button><button onclick="removeWorkout('${w.id}')" class="px-2 text-red-500"><i class="fas fa-trash"></i></button></div>
                        </div>
                        <div class="p-1">${exs}</div>
                        <button onclick="openExModal('${w.id}')" class="w-full text-center py-2 text-primary text-xs font-bold hover:bg-black hover:bg-opacity-10">+ ADD EXERCÍCIO</button>
                    </div>`;
            });
        }

        // Modal de Exercícios
        window.openExModal = wId => { STATE.activeWorkoutId = wId; openModal('modal-exercises'); renderExCats(); renderExList(); };
        const renderExCats = () => $('modal-cats').innerHTML = Object.keys(D.categories).map(c=>`<button onclick="setExCat('${c}')" class="w-full text-left p-2 rounded text-xs ${STATE.activeCategory===c?'bg-primary text-white':'hover:bg-black hover:bg-opacity-20'}">${c}</button>`).join('');
        window.setExCat = c => { STATE.activeCategory = c; renderExCats(); renderExList(); };
        const renderExList = () => {
            const isCardio = STATE.activeCategory === "🫀 CARDIO";
            $('modal-ex-list').innerHTML = `<div class="grid grid-cols-2 gap-2">${D.categories[STATE.activeCategory].map(ex=>`<button onclick="addEx('${ex}',${isCardio})" class="card p-2 rounded text-xs text-left hover:border-primary transition">${ex}</button>`).join('')}</div>`;
        };
        window.addEx = (name, isCardio) => {
            STATE.workouts.find(w=>w.id===STATE.activeWorkoutId).exercises.push({ category: STATE.activeCategory, name, sets: isCardio?'1':'3', reps: isCardio?'-':'10 a 12', technique: 'Nenhuma', obs: '' });
            renderWorkouts();
        };

        // --- 9. SALVAR NO FIREBASE & IMPRIMIR (PLANILHA A4) ---
        window.generatePrint = async () => {
            const memberId = $('nav-select-member').value;
            if(!memberId) return alert("Selecione o profissional que está prescrevendo (Menu Superior).");
            const member = STATE.members.find(m => m.id === memberId);
            const unit = STATE.units.find(u => u.id === $('nav-select-unit').value);

            // Coleta de Dados
            const w = parseFloat($('stu-weight').value), h = parseFloat($('stu-height').value);
            const imcStr = (w>0 && h>0) ? (w/(h*h)).toFixed(1) : "-";
            const health = Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value);
            const data = {
                networkId: STATE.user.uid, unitId: unit.id, memberId: member.id,
                profName: member.name, profType: member.type, profCref: member.cref || '', profState: member.state || '',
                studentName: $('stu-name').value || 'Sem Nome', stuAge: $('stu-age').value, stuWeight: $('stu-weight').value, stuHeight: $('stu-height').value,
                stuGender: $('stu-gender').value, stuLevel: $('stu-level').value, stuObjective: $('stu-objective').value,
                stuFreq: $('stu-freq').value, stuValidity: $('stu-validity').value, stuRecs: $('stu-recs').value,
                health, workouts: STATE.workouts,
                createdAt: new Date(), updatedAt: new Date()
            };

            showLoader("Salvando na Nuvem...");
            try {
                if(STATE.currentFichaId) { await setDoc(doc(db, "workouts", STATE.currentFichaId), data, {merge: true}); } 
                else { const ref = await addDoc(collection(db, "workouts"), data); STATE.currentFichaId = ref.id; }
                hideLoader();
                
                // --- Geração do HTML de Impressão ---
                const isPEF = member.type === 'PEF';
                const healthDict = isPEF ? D.healthPEF : D.healthTE;
                const legalTxt = isPEF ? D.legalPEF : D.legalTE;
                const profLabel = isPEF ? 'Profissional de Educação Física' : 'Treinador Esportivo';
                const crefHtml = isPEF ? `<br>CREF: ${member.cref} - ${member.state}` : '';

                let html = `
                    <div class="print-header">
                        <h1 class="print-title">PLANILHA DE TREINAMENTO</h1>
                        <div class="prof-info">Prescrição: ${member.name} (${profLabel})${crefHtml} | Unidade: ${unit.name}</div>
                    </div>
                    <div class="print-grid">
                        <div><strong>Aluno:</strong> ${data.studentName}</div><div><strong>Idade:</strong> ${data.stuAge||'-'}</div><div><strong>Gênero:</strong> ${data.stuGender}</div>
                        <div><strong>Peso:</strong> ${data.stuWeight||'-'}kg</div><div><strong>Altura:</strong> ${data.stuHeight||'-'}m</div><div><strong>IMC:</strong> ${imcStr}</div>
                        <div><strong>Objetivo:</strong> ${data.stuObjective}</div><div><strong>Freq:</strong> ${data.stuFreq}</div><div><strong>Validade:</strong> ${data.stuValidity}</div>
                    </div>`;

                if(D.objectives[data.stuObjective] || health.length > 0) {
                    html += `<div class="print-guidelines"><h4>Diretrizes de Perfil</h4><ul>`;
                    if(D.objectives[data.stuObjective]) html += `<li><strong>${data.stuObjective}:</strong> ${D.objectives[data.stuObjective]}</li>`;
                    health.forEach(k => { if(healthDict[k]) html += `<li><strong>${k.replace(/[^\w\sÀ-ú]/g,'').trim()}:</strong> ${healthDict[k]}</li>`; });
                    html += `</ul></div>`;
                }

                data.workouts.forEach(wk => {
                    html += `<div class="print-workout"><h3>${wk.title}</h3><table>
                        <thead><tr><th style="width:35%">Exercício</th><th style="width:8%">Séries</th><th style="width:12%">Reps</th><th style="width:15%">Técnica</th><th style="width:30%">Obs</th></tr></thead><tbody>`;
                    if(wk.exercises.length===0) html += `<tr><td colspan="5" style="text-align:center;font-style:italic">Sem exercícios</td></tr>`;
                    else wk.exercises.forEach(ex => html += `<tr><td><strong>${ex.name}</strong></td><td>${ex.sets}</td><td>${ex.reps}</td><td>${ex.technique!=='Nenhuma'?ex.technique:'-'}</td><td>${ex.obs||'-'}</td></tr>`);
                    html += `</tbody></table></div>`;
                });

                html += `<div class="print-footer-section">`;
                if(data.stuRecs) html += `<strong>Recomendações Específicas:</strong><p style="white-space:pre-wrap; margin:2px 0 5px 0">${data.stuRecs}</p>`;
                html += `<div class="legal-text">${legalTxt}</div><div style="text-align:center; margin-top:10px; font-size:7px;">Gerado por PowFit Pro - ${new Date().toLocaleDateString('pt-BR')}</div></div>`;

                $('print-area').innerHTML = html;
                setTimeout(() => window.print(), 300);

            } catch(e) { hideLoader(); alert("Erro ao salvar: " + e.message); }
        };

        // --- 10. HISTÓRICO E RELATÓRIOS ---
        window.openHistory = async () => {
            showLoader("Buscando fichas...");
            openModal('modal-history');
            const q = query(collection(db, "workouts"), where("networkId", "==", STATE.user.uid), orderBy("createdAt", "desc"));
            try {
                const snap = await getDocs(q);
                window.HISTORY_DATA = snap.docs.map(d => ({id: d.id, ...d.data()}));
                renderHistoryList();
            } catch(e) { alert("Erro ao carregar histórico. Verifique se os índices do Firestore estão configurados caso o erro persista (apenas um recarregamento pode resolver). Msg: "+e.message); }
            hideLoader();
        };

        function renderHistoryList() {
            $('history-title').innerHTML = '<i class="fas fa-cloud text-primary mr-2"></i> Fichas Salvas (Rede)';
            $('btn-toggle-report').innerText = "Ver Relatório";
            const cont = $('history-content');
            if(window.HISTORY_DATA.length === 0) return cont.innerHTML = `<p class="opacity-50 text-center py-8">Nenhuma ficha salva na nuvem.</p>`;
            
            cont.innerHTML = window.HISTORY_DATA.map(h => {
                const date = h.createdAt?.toDate ? h.createdAt.toDate().toLocaleDateString('pt-BR') : 'Recente';
                const isExpired = checkValidade(h.createdAt?.toDate(), h.stuValidity);
                const mem = STATE.members.find(m=>m.id===h.memberId)?.name || 'Desconhecido';
                
                return `<div class="p-3 mb-2 bg-black bg-opacity-20 rounded flex justify-between items-center border ${isExpired?'border-yellow-500':'border-transparent'}">
                    <div>
                        <strong class="block">${h.studentName}</strong>
                        <span class="text-xs opacity-70">Data: ${date} | Por: ${mem}</span>
                        ${isExpired ? `<span class="text-[10px] text-yellow-500 ml-2 border border-yellow-500 px-1 rounded">VENCIDA</span>` : ''}
                    </div>
                    <div class="flex gap-2">
                        <button onclick="loadFicha('${h.id}')" class="btn-primary text-[10px] px-2 py-1 rounded">CARREGAR</button>
                        <button onclick="deleteFicha('${h.id}')" class="text-red-500 text-xs px-2"><i class="fas fa-trash"></i></button>
                    </div>
                </div>`;
            }).join('');
        }

        function checkValidade(dateObj, validadeStr) {
            if(!dateObj) return false;
            const days = parseInt(validadeStr);
            if(isNaN(days)) return false;
            const diffTime = Math.abs(new Date() - dateObj);
            const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24)); 
            return diffDays > days;
        }

        window.loadFicha = id => {
            const f = window.HISTORY_DATA.find(x => x.id === id); if(!f) return;
            STATE.currentFichaId = f.id;
            $('nav-select-unit').value = f.unitId;
            updateNavSelects(); // Atualiza membros
            setTimeout(() => {
                $('nav-select-member').value = f.memberId;
                updateHealthOptions();
                
                $('stu-name').value = f.studentName||''; $('stu-age').value = f.stuAge||''; $('stu-weight').value = f.stuWeight||'';
                $('stu-height').value = f.stuHeight||''; $('stu-gender').value = f.stuGender||'Masculino';
                $('stu-level').value = f.stuLevel||'Iniciante'; $('stu-objective').value = f.stuObjective||'Emagrecimento';
                $('stu-freq').value = f.stuFreq||'3 dias'; $('stu-validity').value = f.stuValidity||'30 dias'; $('stu-recs').value = f.stuRecs||'';
                
                document.querySelectorAll('.health-cb').forEach(cb => cb.checked = (f.health||[]).includes(cb.value));
                STATE.workouts = f.workouts || [];
                renderWorkouts(); calculateIMC(); changeTheme();
                closeModal('modal-history');
            }, 100);
        };

        window.deleteFicha = async id => {
            if(confirm("Excluir definitivamente da nuvem?")) {
                showLoader("Excluindo...");
                await deleteDoc(doc(db, "workouts", id));
                window.HISTORY_DATA = window.HISTORY_DATA.filter(x => x.id !== id);
                if(STATE.currentFichaId === id) STATE.currentFichaId = null;
                renderHistoryList();
                hideLoader();
            }
        };

        // Relatório de Produtividade
        window.openReports = () => { window.openHistory(); setTimeout(window.loadReportMode, 500); };
        window.loadReportMode = () => {
            const btn = $('btn-toggle-report');
            if(btn.innerText === "Voltar p/ Fichas") { renderHistoryList(); return; }
            
            btn.innerText = "Voltar p/ Fichas";
            $('history-title').innerHTML = '<i class="fas fa-chart-bar text-primary mr-2"></i> Produtividade da Rede';
            
            // Agrupamento
            const counts = {};
            window.HISTORY_DATA.forEach(w => {
                const k = `${w.unitId}_${w.memberId}`;
                counts[k] = (counts[k]||0) + 1;
            });

            let html = `<div class="space-y-4">`;
            STATE.units.forEach(u => {
                let unitHtml = `<div class="card p-3 rounded border border-gray-600"><h4 class="font-bold text-primary border-b border-gray-700 pb-1 mb-2">${u.name}</h4>`;
                const mems = STATE.members.filter(m => m.unitId === u.id);
                let hasData = false;
                mems.forEach(m => {
                    const total = counts[`${u.id}_${m.id}`] || 0;
                    if(total > 0) { unitHtml += `<div class="flex justify-between text-sm py-1"><span>${m.name} (${m.type})</span><span class="font-bold">${total} fichas</span></div>`; hasData = true; }
                });
                if(!hasData) unitHtml += `<p class="text-xs opacity-50">Nenhuma ficha produzida.</p>`;
                unitHtml += `</div>`;
                html += unitHtml;
            });
            $('history-content').innerHTML = html + `</div>`;
        };

    </script>
</body>
</html>
