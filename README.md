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

        body { font-family: 'Inter', sans-serif; background-color: var(--bg-body); color: var(--text-main); transition: all 0.3s ease; }
        .card { background-color: var(--bg-card); border: 1px solid var(--border-color); }
        .input-field { background-color: var(--input-bg); border: 1px solid var(--input-border); color: var(--text-main); }
        .input-field:focus { outline: none; border-color: var(--primary); box-shadow: 0 0 0 1px var(--primary); }
        .btn-primary { background-color: var(--primary); color: white; transition: background-color 0.2s; }
        .btn-primary:hover { background-color: var(--primary-hover); }
        
        .view-section { display: none; }
        .view-section.active { display: block; }

        /* Estilos de Impressão (Planilha A4) */
        #print-area { display: none; }
        @media print {
            body { background: white !important; color: black !important; margin: 0; padding: 0; }
            #app-container, .no-print, .view-section { display: none !important; }
            #print-area { display: block !important; padding: 10mm 15mm; font-family: 'Arial', sans-serif; font-size: 10px; }
            @page { size: A4; margin: 0; }
            
            .print-header { border-bottom: 2px solid #000; padding-bottom: 8px; margin-bottom: 12px; }
            .print-title { font-size: 18px; font-weight: bold; margin: 0; text-transform: uppercase; text-align: center;}
            .prof-info { font-size: 12px; margin-top: 5px; text-align: center; font-weight: bold;}
            
            .print-grid { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 4px; margin-bottom: 15px; border: 1px solid #000; padding: 8px; }
            .print-grid div { font-size: 11px; }
            
            .print-guidelines { border: 1px solid #000; padding: 8px; margin-bottom: 15px; font-size: 9.5px; }
            .print-guidelines h4 { margin: 0 0 4px 0; font-size: 11px; text-transform: uppercase; background: #eee; padding: 3px;}
            .print-guidelines ul { margin: 0; padding-left: 15px; }
            
            .print-workout { margin-bottom: 15px; page-break-inside: avoid; }
            .print-workout h3 { background: #e0e0e0; border: 1px solid #000; border-bottom: none; padding: 4px 8px; margin: 0; font-size: 12px; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            table { width: 100%; border-collapse: collapse; border: 1px solid #000; }
            th, td { border: 1px solid #000; padding: 4px 6px; text-align: left; font-size: 10px; }
            th { background-color: #f0f0f0; font-weight: bold; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            
            .print-footer-section { margin-top: 15px; border: 1px solid #000; padding: 8px; font-size: 9px; page-break-inside: avoid;}
            .legal-text { font-size: 8px; color: #333; text-align: justify; margin-top: 5px; font-style: italic;}
        }

        /* Relatório Mensal Impressão */
        .report-table { width: 100%; border-collapse: collapse; margin-top: 20px; color: var(--text-main); }
        .report-table th, .report-table td { border: 1px solid var(--border-color); padding: 8px; text-align: left; }
        .report-table th { background-color: var(--bg-card); font-weight: bold; }
        @media print {
            .report-table { color: black; }
            .report-table th, .report-table td { border-color: black; }
            .report-table th { background-color: #eee !important; -webkit-print-color-adjust: exact; }
            #report-view-content { display: block !important; padding: 20mm; }
        }

        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
    </style>
</head>
<body data-theme="Masculino" class="min-h-screen">

    <div id="app-container" class="max-w-7xl mx-auto p-4 sm:p-6 lg:p-8">

        <!-- ========================================== -->
        <!-- VIEW: LOGIN -->
        <!-- ========================================== -->
        <div id="view-login" class="view-section active flex items-center justify-center min-h-[80vh]">
            <div class="card p-8 rounded-2xl shadow-xl text-center max-w-md w-full">
                <i class="fas fa-dumbbell text-5xl text-primary mb-4"></i>
                <h1 class="text-3xl font-bold mb-2">PowFit Pro</h1>
                <p class="text-sm opacity-70 mb-8">Plataforma Profissional de Prescrição. Faça login para acessar sua rede e histórico na nuvem.</p>
                <button id="btn-login" class="w-full bg-white text-gray-800 font-bold py-3 px-4 rounded-lg shadow border border-gray-200 hover:bg-gray-50 transition flex items-center justify-center gap-3">
                    <img src="https://www.svgrepo.com/show/475656/google-color.svg" class="w-5 h-5" alt="Google">
                    Entrar com Google
                </button>
            </div>
        </div>

        <!-- ========================================== -->
        <!-- VIEW: SETUP REDE/UNIDADES/MEMBROS -->
        <!-- ========================================== -->
        <div id="view-setup" class="view-section">
            <div class="mb-8">
                <h2 class="text-2xl font-bold">Configuração da Rede</h2>
                <p class="opacity-70 text-sm">Cadastre sua rede, unidades e profissionais.</p>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <!-- Cadastro da Rede -->
                <div class="card p-6 rounded-xl">
                    <h3 class="text-lg font-bold mb-4 border-b border-opacity-20 pb-2 border-gray-500">1. Dados da Rede</h3>
                    <div class="space-y-4">
                        <div>
                            <label class="block text-sm mb-1">Nome da Rede (ex: Power Fitness)</label>
                            <input type="text" id="setup-net-name" class="input-field w-full rounded p-2 text-sm">
                        </div>
                    </div>
                </div>

                <!-- Cadastro de Unidades -->
                <div class="card p-6 rounded-xl">
                    <h3 class="text-lg font-bold mb-4 border-b border-opacity-20 pb-2 border-gray-500">2. Unidades</h3>
                    <div class="flex gap-2 mb-4">
                        <input type="text" id="setup-unit-name" class="input-field flex-1 rounded p-2 text-sm" placeholder="Nome da Unidade (ex: Sibaúma)">
                        <button onclick="addUnit()" class="btn-primary px-4 rounded font-bold"><i class="fas fa-plus"></i></button>
                    </div>
                    <ul id="setup-units-list" class="space-y-2 text-sm"></ul>
                </div>

                <!-- Cadastro de Membros -->
                <div class="card p-6 rounded-xl md:col-span-2">
                    <h3 class="text-lg font-bold mb-4 border-b border-opacity-20 pb-2 border-gray-500">3. Equipe / Membros</h3>
                    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-5 gap-3 mb-4">
                        <input type="text" id="mem-name" class="input-field rounded p-2 text-sm" placeholder="Nome do Profissional">
                        <select id="mem-cat" onchange="toggleMemCref()" class="input-field rounded p-2 text-sm">
                            <option value="PEF">Prof. Educação Física</option>
                            <option value="TE">Treinador Esportivo</option>
                        </select>
                        <input type="text" id="mem-cpf" class="input-field rounded p-2 text-sm" placeholder="CPF">
                        <div class="flex gap-2">
                            <input type="text" id="mem-cref" class="input-field rounded p-2 text-sm w-2/3" placeholder="CREF">
                            <input type="text" id="mem-state" class="input-field rounded p-2 text-sm w-1/3" placeholder="UF">
                        </div>
                        <select id="mem-unit" class="input-field rounded p-2 text-sm"></select>
                    </div>
                    <button onclick="addMember()" class="btn-primary px-4 py-2 rounded text-sm font-bold w-full sm:w-auto mb-4">Adicionar Membro</button>
                    
                    <div class="overflow-x-auto">
                        <table class="w-full text-left text-sm border-collapse">
                            <thead><tr class="border-b border-opacity-20 border-gray-500"><th class="p-2">Nome</th><th class="p-2">Categoria</th><th class="p-2">CREF</th><th class="p-2">Unidade</th><th class="p-2">Ação</th></tr></thead>
                            <tbody id="setup-members-list"></tbody>
                        </table>
                    </div>
                </div>
            </div>

            <div class="mt-8 flex justify-end">
                <button onclick="saveSetupAndGoToDashboard()" class="btn-primary px-8 py-3 rounded-xl font-bold text-lg shadow-lg">Salvar Configurações e Entrar</button>
            </div>
        </div>

        <!-- ========================================== -->
        <!-- VIEW: DASHBOARD -->
        <!-- ========================================== -->
        <div id="view-dashboard" class="view-section">
            <div class="flex flex-col md:flex-row justify-between items-start md:items-center mb-8 gap-4">
                <div>
                    <h1 class="text-2xl font-bold tracking-tight" id="dash-net-name">Rede</h1>
                    <p class="text-sm opacity-70">Painel de Controle e Histórico</p>
                </div>
                <div class="flex flex-wrap gap-2">
                    <button onclick="openNewFichaModal()" class="btn-primary px-4 py-2 rounded-lg font-medium shadow flex items-center gap-2">
                        <i class="fas fa-plus"></i> Nova Ficha
                    </button>
                    <button onclick="showReportView()" class="bg-gray-700 hover:bg-gray-600 text-white px-4 py-2 rounded-lg font-medium shadow flex items-center gap-2 transition">
                        <i class="fas fa-chart-bar"></i> Relatórios
                    </button>
                    <button onclick="editSetup()" class="bg-gray-700 hover:bg-gray-600 text-white px-4 py-2 rounded-lg font-medium shadow flex items-center gap-2 transition">
                        <i class="fas fa-cog"></i> Configurar Rede
                    </button>
                    <button id="btn-logout" class="bg-red-500 hover:bg-red-600 text-white px-4 py-2 rounded-lg font-medium shadow transition">Sair</button>
                </div>
            </div>

            <!-- Lista de Fichas Recentes -->
            <div class="card rounded-xl shadow-sm overflow-hidden">
                <div class="p-4 border-b border-opacity-20 border-gray-500 bg-black bg-opacity-10 flex justify-between items-center">
                    <h2 class="font-bold text-lg"><i class="fas fa-folder-open text-primary mr-2"></i> Histórico Geral de Fichas</h2>
                    <input type="text" id="search-ficha" onkeyup="renderFichasList()" placeholder="Buscar aluno..." class="input-field rounded-full px-4 py-1 text-sm w-48 sm:w-64">
                </div>
                <div class="p-0 overflow-x-auto">
                    <table class="w-full text-left text-sm">
                        <thead class="bg-black bg-opacity-5">
                            <tr>
                                <th class="p-4 font-semibold">Aluno</th>
                                <th class="p-4 font-semibold">Profissional</th>
                                <th class="p-4 font-semibold">Unidade</th>
                                <th class="p-4 font-semibold">Data Criação</th>
                                <th class="p-4 font-semibold">Status</th>
                                <th class="p-4 font-semibold text-right">Ações</th>
                            </tr>
                        </thead>
                        <tbody id="dash-fichas-list" class="divide-y divide-gray-500 divide-opacity-20">
                            <!-- Fichas JS -->
                        </tbody>
                    </table>
                </div>
            </div>
        </div>

        <!-- ========================================== -->
        <!-- VIEW: RELATÓRIOS -->
        <!-- ========================================== -->
        <div id="view-report" class="view-section">
            <div class="flex justify-between items-center mb-6 no-print">
                <button onclick="showView('dashboard')" class="text-sm font-medium hover:text-primary"><i class="fas fa-arrow-left"></i> Voltar ao Painel</button>
                <button onclick="window.print()" class="btn-primary px-4 py-2 rounded-lg shadow"><i class="fas fa-print"></i> Imprimir Relatório</button>
            </div>
            
            <div id="report-view-content" class="card p-6 rounded-xl">
                <div class="text-center mb-6 border-b pb-4 border-gray-500 border-opacity-20">
                    <h2 class="text-2xl font-bold" id="report-net-name">Relatório</h2>
                    <p class="text-sm opacity-70">Produção Mensal por Unidade e Membro</p>
                </div>

                <div class="flex gap-4 mb-6 no-print justify-center">
                    <input type="month" id="report-month" onchange="generateReport()" class="input-field rounded p-2">
                </div>

                <div id="report-content-area"></div>
            </div>
        </div>

        <!-- ========================================== -->
        <!-- VIEW: EDITOR (CRIAR FICHA) -->
        <!-- ========================================== -->
        <div id="view-editor" class="view-section">
            
            <div class="flex justify-between items-center mb-6">
                <button onclick="showView('dashboard')" class="text-sm font-medium hover:text-primary"><i class="fas fa-arrow-left"></i> Voltar sem salvar</button>
                <div class="flex gap-2">
                    <button onclick="saveFicha(false)" class="bg-gray-700 hover:bg-gray-600 text-white px-4 py-2 rounded-lg font-medium shadow transition">Salvar</button>
                    <button onclick="saveFicha(true)" class="btn-primary px-4 py-2 rounded-lg font-medium shadow flex items-center gap-2"><i class="fas fa-print"></i> Salvar & Imprimir</button>
                </div>
            </div>

            <!-- Header do Editor indicando quem está logado/selecionado -->
            <div class="card p-4 rounded-xl mb-6 bg-opacity-20 flex flex-col sm:flex-row items-center gap-4 border-primary">
                <div class="w-12 h-12 rounded-full bg-primary flex items-center justify-center text-white text-xl font-bold">
                    <i class="fas fa-user-tie"></i>
                </div>
                <div>
                    <p class="text-xs opacity-70">Prescrevendo como:</p>
                    <h3 class="font-bold text-lg" id="active-member-name">-</h3>
                    <p class="text-xs" id="active-member-info">-</p>
                </div>
            </div>

            <div class="grid grid-cols-1 xl:grid-cols-12 gap-6">
                <!-- Esquerda: Aluno -->
                <div class="xl:col-span-4 space-y-6">
                    <div class="card rounded-xl p-5 shadow-sm">
                        <div class="flex justify-between items-center mb-4 border-b border-opacity-20 pb-2 border-gray-500">
                            <h2 class="text-lg font-semibold"><i class="fas fa-user text-primary mr-2"></i> Dados do Aluno</h2>
                            <div id="imc-display"></div>
                        </div>
                        <div class="space-y-4">
                            <input type="text" id="stu-name" class="input-field w-full rounded p-2 text-sm" placeholder="Nome do aluno">
                            <div class="grid grid-cols-3 gap-2">
                                <input type="number" id="stu-age" class="input-field rounded p-2 text-sm" placeholder="Idade">
                                <input type="number" id="stu-weight" oninput="calculateIMC()" class="input-field rounded p-2 text-sm" placeholder="Peso (kg)">
                                <input type="number" id="stu-height" step="0.01" oninput="calculateIMC()" class="input-field rounded p-2 text-sm" placeholder="Alt. (m)">
                            </div>
                            <div class="grid grid-cols-2 gap-2">
                                <select id="stu-gender" onchange="changeTheme()" class="input-field rounded p-2 text-sm">
                                    <option value="Masculino">Masculino</option>
                                    <option value="Feminino">Feminino</option>
                                </select>
                                <select id="stu-level" class="input-field rounded p-2 text-sm">
                                    <option>Iniciante</option>
                                    <option>Intermediário</option>
                                    <option>Avançado</option>
                                </select>
                            </div>
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
                            <select id="stu-freq" class="input-field w-full rounded p-2 text-sm">
                                <option>3 dias</option>
                                <option>5 dias</option>
                                <option>6 dias</option>
                                <option>Personalizado</option>
                            </select>
                        </div>
                    </div>

                    <div class="card rounded-xl p-5 shadow-sm">
                        <h2 class="text-lg font-semibold mb-2 border-b border-opacity-20 pb-2 border-gray-500"><i class="fas fa-notes-medical text-primary mr-2"></i> Estado de Saúde</h2>
                        <div class="grid grid-cols-2 gap-2 text-xs" id="health-container"></div>
                    </div>
                </div>

                <!-- Direita: Treinos -->
                <div class="xl:col-span-8 space-y-6">
                    <div class="flex justify-between items-center bg-opacity-10 p-4 rounded-xl card border-dashed border-2">
                        <div class="flex gap-2 items-center">
                            <h2 class="text-xl font-bold"><i class="fas fa-list text-primary mr-2"></i> Montagem</h2>
                            <select id="stu-validity" class="input-field rounded px-2 py-1 text-xs">
                                <option>Validade: 15 dias</option><option selected>Validade: 30 dias</option>
                                <option>Validade: 60 dias</option><option>Validade: 90 dias</option>
                            </select>
                        </div>
                        <button onclick="addWorkoutDay()" class="btn-primary px-3 py-1.5 rounded text-sm font-medium flex items-center gap-2">
                            <i class="fas fa-plus"></i> Dia
                        </button>
                    </div>

                    <div id="workouts-container" class="space-y-4"></div>

                    <div class="card rounded-xl p-4 shadow-sm">
                        <h2 class="text-sm font-semibold mb-2">Recomendações Manuais Livres</h2>
                        <textarea id="stu-recs" rows="2" class="input-field w-full rounded p-2 text-sm" placeholder="Ex: Cuidado com a hidratação..."></textarea>
                    </div>
                </div>
            </div>
        </div>

    </div>

    <!-- MODALS -->
    <!-- Modal: Selecionar Membro para criar ficha -->
    <div id="modal-select-member" class="fixed inset-0 bg-black bg-opacity-80 backdrop-blur-sm hidden z-50 flex items-center justify-center p-4">
        <div class="card w-full max-w-md rounded-xl p-6 shadow-2xl">
            <h3 class="text-xl font-bold mb-4">Quem está prescrevendo?</h3>
            <p class="text-sm opacity-70 mb-4">Selecione o membro da equipe para associar a esta ficha.</p>
            <select id="select-active-member" class="input-field w-full rounded p-3 mb-6"></select>
            <div class="flex justify-end gap-2">
                <button onclick="document.getElementById('modal-select-member').classList.add('hidden')" class="px-4 py-2 rounded text-sm opacity-70 hover:bg-gray-700">Cancelar</button>
                <button onclick="startNewFicha()" class="btn-primary px-4 py-2 rounded font-bold">Continuar</button>
            </div>
        </div>
    </div>

    <!-- Modal: Banco de Exercícios -->
    <div id="exercise-modal" class="fixed inset-0 bg-black bg-opacity-80 backdrop-blur-sm hidden z-50 flex items-center justify-center p-2 sm:p-4">
        <div class="card w-full max-w-5xl rounded-xl shadow-2xl flex flex-col h-[90vh]">
            <div class="p-4 border-b flex justify-between items-center border-gray-500 border-opacity-20">
                <h3 class="text-lg font-bold"><i class="fas fa-search text-primary mr-2"></i> Adicionar Exercício</h3>
                <button onclick="document.getElementById('exercise-modal').classList.add('hidden')" class="text-gray-400 hover:text-red-500 text-2xl leading-none">&times;</button>
            </div>
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <div class="w-full md:w-1/4 border-r border-gray-500 border-opacity-20 overflow-y-auto p-2 space-y-1" id="modal-categories"></div>
                <div class="w-full md:w-3/4 overflow-y-auto p-3 bg-black bg-opacity-5" id="modal-exercises"></div>
            </div>
        </div>
    </div>

    <!-- ÁREA DE IMPRESSÃO (Oculta) -->
    <div id="print-area"></div>


    <!-- FIREBASE & LOGIC -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-app.js";
        import { getAuth, GoogleAuthProvider, signInWithPopup, signOut, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-auth.js";
        import { getFirestore, collection, doc, setDoc, getDoc, getDocs, deleteDoc } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-firestore.js";

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

        // --- GLOBAL STATE ---
        window.appState = {
            user: null,
            network: null,
            units: [],
            members: [],
            fichas: [],
            activeMember: null, // Membro selecionado para criar ficha
            currentFicha: null, // Ficha sendo editada ({ id, workouts: [], ... })
            activeCategory: "🔥 PEITO",
            activeWorkoutIdModal: null
        };

        // --- DADOS FIXOS ---
        const healthDataPEF = {
            "🟢 Saudável": "Manter treinos regulares 3–5x por semana. Foco em evolução e constância.",
            "⚪ Sedentário": "Iniciar com treinos leves 2–3x por semana. Priorizar adaptação.",
            "🟡 Sobrepeso": "Treinar 3–5x por semana com foco em gasto calórico.",
            "🔴 Obesidade": "Iniciar com exercícios de baixo impacto. Evolução gradual.",
            "⚖️ Baixo peso": "Foco em musculação para ganho de massa. Treinos moderados.",
            "🍬 Diabetes": "Treinos regulares moderados. Monitorar glicemia.",
            "❤️ Hipertensão": "Treinos moderados, evitar prender a respiração.",
            "🔵 Hipotensão": "Evitar mudanças bruscas. Manter hidratação.",
            "💔 Problemas cardíacos": "Treinos leves com liberação médica.",
            "🦴 Problemas articulares": "Evitar impacto. Priorizar exercícios controlados.",
            "🫁 Problemas respiratórios": "Progressão gradual. Atenção à respiração.",
            "⚠️ Lesões": "Adaptar exercícios. Evitar dor e focar na recuperação.",
            "🤰 Gestante": "Treinos leves a moderados. Foco em mobilidade.",
            "🤱 Lactante": "Treinar normalmente com atenção à hidratação.",
            "👴 Idoso": "Foco em força, equilíbrio e mobilidade."
        };

        const healthDataTE = {
            "🟢 Saudável": "Treinos de 3–5x/sem. A constância, descanso e boa alimentação contribuem para resultados.",
            "⚪ Sedentário": "Início gradual. Prioridade em desenvolver constância e execução correta.",
            "🟡 Sobrepeso": "Musculação + cardio para condicionamento. Respeitar individualidade.",
            "🔴 Obesidade": "Início com menor impacto. Segurança, mobilidade e constância.",
            "⚖️ Baixo peso": "Musculação auxiliando no ganho de massa, priorizar recuperação.",
            "🍬 Diabetes": "Acompanhamento médico. Em caso de mal-estar, interromper o treino.",
            "❤️ Hipertensão": "Evitar Manobra de Valsalva (prender respiração) e controlar intensidade.",
            "🔵 Hipotensão": "Evitar mudanças bruscas de posição. Manter hidratação.",
            "💔 Problemas cardíacos": "Prática apenas com liberação médica. Controle estrito de intensidade.",
            "🦴 Problemas articulares": "Menor impacto e maior controle de movimento.",
            "🫁 Problemas respiratórios": "Respeitar capacidade respiratória. Interromper se houver falta de ar.",
            "⚠️ Lesões": "Adaptar exercícios para evitar dor. Seguir orientação profissional.",
            "🤰 Gestante": "Liberação médica obrigatória. Foco em segurança e evitar impacto.",
            "🤱 Lactante": "Atividade mantida, respeitando hidratação e recuperação.",
            "👴 Idoso": "Intensidade moderada. Foco na segurança, mobilidade e autonomia."
        };

        const objectiveData = {
            "Emagrecimento": "Foco em déficit calórico com treinos mistos de força e aeróbicos.",
            "Hipertrofia": "Prioridade na progressão de carga e volume adequado.",
            "Definição": "Manutenção de massa muscular com foco na dieta.",
            "Condicionamento": "Treinos com menor intervalo e alta integração cardiopulmonar.",
            "Resistência": "Séries longas, cadência controlada.",
            "Força": "Cargas altas, baixas repetições e intervalos maiores.",
            "Reabilitação": "Treino focado em fortalecimento específico. Respeitar limites.",
            "Saúde geral": "Equilíbrio entre força, cardio e flexibilidade."
        };

        const dbCategories = {
            "🔥 PEITO": ["Supino Reto", "Supino Inclinado", "Supino Inclinado com Halteres", "Supino Fechado com Halteres", "Cross Over", "Cross Over Alto", "Cross Over Baixo", "Crucifixo Reto", "Crucifixo Inclinado com Halteres", "Crucifixo na Máquina", "Peck Fly", "Peck Fly Unilateral", "Pullover", "Flexão de Braço", "Flexão com Pés Elevados", "Flexão Explosiva"],
            "🦍 COSTAS": ["Puxada Alta", "Puxada de Frente Supinada", "Pulldown", "Remada Aberta", "Remada Baixa", "Remada Curvada", "Remada Curvada Supinada", "Remada Cavalinho (T-Bar)", "Serrote", "Facepull (puxada de cima para baixo)"],
            "🦵 PERNAS": ["Agachamento Livre", "Agachamento Taça", "Agachamento no Smith", "Agachamento com passada lateral", "Squat", "Hack Machine", "Leg 45°", "Leg 90°", "Agachamento Sumô", "Agachamento Sissy (Livre)", "Afundo", "Recuo", "Avanço", "Passada", "Búlgaro", "Step-up", "Levantamento Terra", "Levantamento Terra Romeno", "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", "Cadeira Flexora", "Elevação Pélvica no Banco", "Elevação Pélvica no Chão", "Elevação Pélvica Unilateral no Chão", "Extensão de Quadril (Glúteo Máximo)", "Extensão Cruzada (Glúteo Médio)", "Coice", "Cachorrinho", "Cadeira Extensora", "Adução", "Abdução", "Abdução Inclinada", "Flexão Nórdica", "Flexão Nórdica Invertida", "Panturrilha Livre", "Panturrilha no Leg Press", "Panturrilha Banco", "Panturrilha Squat", "Panturrilha Unilateral"],
            "💪 BRAÇOS": ["(Bíceps) Rosca Direta", "(Bíceps) Rosca Alternada", "(Bíceps) Rosca 21", "(Bíceps) Rosca Scott Barra W", "(Bíceps) Rosca Scott Unilateral", "(Bíceps) Rosca Scott com Halteres", "(Bíceps) Rosca Martelo", "(Bíceps) Rosca Cross", "(Bíceps) Rosca Inversa", "(Bíceps) Rosca 45°", "(Tríceps) Pulley Unilateral", "(Tríceps) Pulley Barra", "(Tríceps) Pulley Corda", "(Tríceps) Pulley Pegada Inversa", "(Tríceps) Francês na Corda", "(Tríceps) Francês com Halter", "(Tríceps) Francês Unilateral", "(Tríceps) Cruzado Polia Dupla", "(Tríceps) Coice Unilateral", "(Tríceps) Arremesso", "(Tríceps) Testa", "(Tríceps) Mergulho no Banco"],
            "🪨 OMBROS": ["Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", "Elevação Lateral Unilateral Cross", "Elevação Lateral Sentado", "Desenvolvimento com Halteres", "Desenvolvimento com Barra", "Arnold Press", "Elevação Borboleta", "Crucifixo Inverso Sentado com Halteres", "Crucifixo Inverso na Polia", "Crucifixo Inverso Unilateral na Polia", "Facepull (puxada reta)", "Remada Alta", "Encolhimento (Trapézio)"],
            "🧠 ABDÔMEN": ["Infra com Elevação de Perna", "Abdominal Supra", "Abdominal Remador", "Abdominal Bicicleta", "Abdominal Twister com Peso", "Prancha", "Prancha Lateral", "Trituração de Cabos em Pé", "Isometria na parede"],
            "🫀 CARDIO": ["Bicicleta 10 Minutos", "Bicicleta 15 Minutos", "Bicicleta 20 Minutos", "Esteira 10 Minutos", "Esteira 15 Minutos", "Esteira 20 Minutos", "Pular Corda"]
        };
        const dbTechniques = ["Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", "Negativa", "Isometria", "Parciais", "Pirâmide"];
        const daysOfWeek = ["SEGUNDA-FEIRA", "TERÇA-FEIRA", "QUARTA-FEIRA", "QUINTA-FEIRA", "SEXTA-FEIRA", "SÁBADO", "DOMINGO"];

        function genId() { return Math.random().toString(36).substr(2, 9); }

        // --- AUTH & ROUTING ---
        document.getElementById('btn-login').addEventListener('click', () => {
            signInWithPopup(auth, provider).catch(error => alert("Erro ao logar: " + error.message));
        });
        document.getElementById('btn-logout').addEventListener('click', () => { signOut(auth); });

        onAuthStateChanged(auth, async (user) => {
            if (user) {
                appState.user = user;
                await loadCloudData();
            } else {
                appState.user = null;
                showView('view-login');
            }
        });

        window.showView = function(viewId) {
            document.querySelectorAll('.view-section').forEach(el => el.classList.remove('active'));
            document.getElementById(viewId).classList.add('active');
        }

        async function loadCloudData() {
            const uid = appState.user.uid;
            
            // Verifica Rede
            const netDoc = await getDoc(doc(db, "users", uid, "network", "info"));
            if (!netDoc.exists()) {
                showView('view-setup');
                return;
            }
            appState.network = netDoc.data();
            document.getElementById('dash-net-name').innerText = appState.network.name;

            // Carrega Unidades
            const unitsSnap = await getDocs(collection(db, "users", uid, "units"));
            appState.units = unitsSnap.docs.map(d => ({id: d.id, ...d.data()}));
            
            // Carrega Membros
            const membersSnap = await getDocs(collection(db, "users", uid, "members"));
            appState.members = membersSnap.docs.map(d => ({id: d.id, ...d.data()}));

            // Carrega Fichas
            const fichasSnap = await getDocs(collection(db, "users", uid, "fichas"));
            appState.fichas = fichasSnap.docs.map(d => ({id: d.id, ...d.data()}));

            if(appState.units.length === 0 || appState.members.length === 0) {
                populateSetupForm();
                showView('view-setup');
            } else {
                renderFichasList();
                showView('view-dashboard');
            }
        }

        // --- SETUP VIEW LOGIC ---
        window.editSetup = function() {
            populateSetupForm();
            showView('view-setup');
        }

        function populateSetupForm() {
            if(appState.network) document.getElementById('setup-net-name').value = appState.network.name;
            renderSetupUnits();
            renderSetupMembers();
            
            // Preenche select de unidades no cadastro de membro
            const unitSelect = document.getElementById('mem-unit');
            unitSelect.innerHTML = appState.units.map(u => `<option value="${u.id}">${u.name}</option>`).join('');
        }

        window.addUnit = function() {
            const name = document.getElementById('setup-unit-name').value.trim();
            if(!name) return;
            appState.units.push({ id: genId(), name: name });
            document.getElementById('setup-unit-name').value = '';
            populateSetupForm(); // Atualiza listagens
        }
        
        window.removeUnit = function(id) {
            appState.units = appState.units.filter(u => u.id !== id);
            appState.members = appState.members.filter(m => m.unitId !== id); // Remove membros atrelados
            populateSetupForm();
        }

        function renderSetupUnits() {
            document.getElementById('setup-units-list').innerHTML = appState.units.map(u => `
                <li class="flex justify-between items-center bg-black bg-opacity-10 p-2 rounded">
                    ${u.name} <button onclick="removeUnit('${u.id}')" class="text-red-500"><i class="fas fa-trash"></i></button>
                </li>
            `).join('');
        }

        window.toggleMemCref = function() {
            const isTE = document.getElementById('mem-cat').value === 'TE';
            document.getElementById('mem-cref').parentElement.style.display = isTE ? 'none' : 'flex';
        }

        window.addMember = function() {
            const name = document.getElementById('mem-name').value.trim();
            const cat = document.getElementById('mem-cat').value;
            const cpf = document.getElementById('mem-cpf').value;
            const unitId = document.getElementById('mem-unit').value;
            const cref = document.getElementById('mem-cref').value;
            const state = document.getElementById('mem-state').value;

            if(!name || !unitId) { alert("Nome e Unidade são obrigatórios."); return; }

            appState.members.push({ id: genId(), name, cat, cpf, unitId, cref, state });
            document.getElementById('mem-name').value = '';
            document.getElementById('mem-cpf').value = '';
            document.getElementById('mem-cref').value = '';
            populateSetupForm();
        }

        window.removeMember = function(id) {
            appState.members = appState.members.filter(m => m.id !== id);
            populateSetupForm();
        }

        function renderSetupMembers() {
            document.getElementById('setup-members-list').innerHTML = appState.members.map(m => {
                const unit = appState.units.find(u => u.id === m.unitId)?.name || 'Desconhecida';
                const catStr = m.cat === 'PEF' ? 'Ed. Física' : 'T. Esportivo';
                return `<tr class="border-b border-opacity-10 border-gray-500">
                    <td class="p-2">${m.name}</td>
                    <td class="p-2">${catStr}</td>
                    <td class="p-2">${m.cat === 'PEF' ? m.cref : '-'}</td>
                    <td class="p-2">${unit}</td>
                    <td class="p-2"><button onclick="removeMember('${m.id}')" class="text-red-500"><i class="fas fa-trash"></i></button></td>
                </tr>`;
            }).join('');
        }

        window.saveSetupAndGoToDashboard = async function() {
            const netName = document.getElementById('setup-net-name').value.trim();
            if(!netName || appState.units.length === 0 || appState.members.length === 0) {
                alert("Preencha o nome da rede, adicione pelo menos 1 unidade e 1 membro."); return;
            }

            const uid = appState.user.uid;
            
            // Salvar Rede
            appState.network = { name: netName };
            await setDoc(doc(db, "users", uid, "network", "info"), appState.network);

            // Substituir Unidades (apaga antigas, salva novas - simplificado p/ este exemplo)
            const unitsRef = collection(db, "users", uid, "units");
            const oldUnits = await getDocs(unitsRef);
            oldUnits.forEach(async (d) => await deleteDoc(d.ref));
            for(let u of appState.units) await setDoc(doc(db, "users", uid, "units", u.id), u);

            // Substituir Membros
            const memsRef = collection(db, "users", uid, "members");
            const oldMems = await getDocs(memsRef);
            oldMems.forEach(async (d) => await deleteDoc(d.ref));
            for(let m of appState.members) await setDoc(doc(db, "users", uid, "members", m.id), m);

            document.getElementById('dash-net-name').innerText = netName;
            renderFichasList();
            showView('view-dashboard');
        }

        // --- DASHBOARD LOGIC ---
        window.renderFichasList = function() {
            const searchTerm = document.getElementById('search-ficha').value.toLowerCase();
            const list = document.getElementById('dash-fichas-list');
            
            // Sort by date desc
            let sorted = [...appState.fichas].sort((a,b) => new Date(b.createdAt) - new Date(a.createdAt));
            
            if(searchTerm) {
                sorted = sorted.filter(f => f.stuName.toLowerCase().includes(searchTerm));
            }

            if(sorted.length === 0) {
                list.innerHTML = `<tr><td colspan="6" class="p-6 text-center opacity-50">Nenhuma ficha encontrada.</td></tr>`;
                return;
            }

            list.innerHTML = sorted.map(f => {
                const mem = appState.members.find(m => m.id === f.memberId) || { name: 'Desconhecido' };
                const unit = appState.units.find(u => u.id === f.unitId) || { name: 'Desconhecida' };
                
                // Calculo de Validade
                const createdDate = new Date(f.createdAt);
                const validityDays = parseInt(f.validity.match(/\d+/)[0]);
                const expireDate = new Date(createdDate.getTime() + (validityDays * 24 * 60 * 60 * 1000));
                const isExpired = new Date() > expireDate;
                
                const statusBadge = isExpired 
                    ? `<span class="bg-red-500 bg-opacity-20 text-red-500 px-2 py-1 rounded text-xs font-bold">⚠️ Vencida</span>`
                    : `<span class="bg-green-500 bg-opacity-20 text-green-500 px-2 py-1 rounded text-xs font-bold">🟢 Válida</span>`;

                return `
                <tr class="hover:bg-black hover:bg-opacity-5">
                    <td class="p-4 font-bold">${f.stuName}</td>
                    <td class="p-4 text-xs">${mem.name}</td>
                    <td class="p-4 text-xs">${unit.name}</td>
                    <td class="p-4 text-xs">${createdDate.toLocaleDateString('pt-BR')}</td>
                    <td class="p-4">${statusBadge}</td>
                    <td class="p-4 text-right">
                        <button onclick="editFicha('${f.id}')" class="text-primary hover:text-white bg-primary bg-opacity-10 hover:bg-opacity-100 px-3 py-1 rounded transition text-xs font-bold mr-2">Abrir</button>
                        <button onclick="deleteFicha('${f.id}')" class="text-red-500 hover:text-red-700 transition text-sm"><i class="fas fa-trash"></i></button>
                    </td>
                </tr>`;
            }).join('');
        }

        window.deleteFicha = async function(id) {
            if(!confirm("Excluir definitivamente esta ficha?")) return;
            const uid = appState.user.uid;
            await deleteDoc(doc(db, "users", uid, "fichas", id));
            appState.fichas = appState.fichas.filter(f => f.id !== id);
            renderFichasList();
        }

        // --- RELATORIO MENSAL ---
        window.showReportView = function() {
            // Set current month as default
            const now = new Date();
            const monthStr = `${now.getFullYear()}-${String(now.getMonth() + 1).padStart(2, '0')}`;
            document.getElementById('report-month').value = monthStr;
            document.getElementById('report-net-name').innerText = `Relatório - ${appState.network.name}`;
            generateReport();
            showView('view-report');
        }

        window.generateReport = function() {
            const monthVal = document.getElementById('report-month').value; // YYYY-MM
            if(!monthVal) return;
            
            const [year, month] = monthVal.split('-');
            
            // Filtrar fichas do mês selecionado
            const monthFichas = appState.fichas.filter(f => {
                const fd = new Date(f.createdAt);
                return fd.getFullYear() == year && (fd.getMonth() + 1) == month;
            });

            // Agrupar por Unidade -> Membro -> Contagem
            let reportData = {};
            appState.units.forEach(u => reportData[u.id] = { name: u.name, members: {} });
            appState.members.forEach(m => {
                if(reportData[m.unitId]) {
                    reportData[m.unitId].members[m.id] = { name: m.name, cat: m.cat, count: 0 };
                }
            });

            monthFichas.forEach(f => {
                if(reportData[f.unitId] && reportData[f.unitId].members[f.memberId]) {
                    reportData[f.unitId].members[f.memberId].count++;
                }
            });

            // Gerar HTML do relatório
            let html = ``;
            let totalGeral = 0;

            for(const unitId in reportData) {
                const u = reportData[unitId];
                let unitTotal = 0;
                let rows = '';
                
                for(const memId in u.members) {
                    const m = u.members[memId];
                    if(m.count > 0 || true) { // Mostra todos mesmo zerados
                        rows += `<tr><td>${m.name}</td><td>${m.cat === 'PEF' ? 'Ed. Física' : 'T. Esportivo'}</td><td class="font-bold text-center">${m.count}</td></tr>`;
                        unitTotal += m.count;
                    }
                }

                html += `
                    <h3 class="font-bold text-lg mt-6 bg-primary text-white p-2 rounded-t-lg">Unidade: ${u.name} (Total: ${unitTotal})</h3>
                    <table class="report-table mb-6">
                        <thead><tr><th>Profissional / Membro</th><th>Categoria</th><th class="text-center">Fichas Geradas</th></tr></thead>
                        <tbody>${rows}</tbody>
                    </table>
                `;
                totalGeral += unitTotal;
            }

            html += `<div class="text-xl font-bold text-center p-4 border-t-2 border-dashed border-gray-500 mt-8">TOTAL GERAL DA REDE: ${totalGeral} FICHAS</div>`;
            document.getElementById('report-content-area').innerHTML = html;
        }

        // --- EDITOR (NOVA FICHA) ---
        window.openNewFichaModal = function() {
            const select = document.getElementById('select-active-member');
            select.innerHTML = appState.members.map(m => {
                const unitName = appState.units.find(u => u.id === m.unitId)?.name || '';
                return `<option value="${m.id}">${m.name} (${unitName})</option>`;
            }).join('');
            document.getElementById('modal-select-member').classList.remove('hidden');
        }

        window.startNewFicha = function() {
            const memId = document.getElementById('select-active-member').value;
            appState.activeMember = appState.members.find(m => m.id === memId);
            document.getElementById('modal-select-member').classList.add('hidden');
            
            // Init Ficha Object
            appState.currentFicha = {
                id: genId(),
                workouts: [{ id: genId(), title: "TREINO " + daysOfWeek[0], exercises: [] }]
            };
            
            setupEditorView();
            showView('view-editor');
        }

        window.editFicha = function(fichaId) {
            const f = appState.fichas.find(x => x.id === fichaId);
            appState.activeMember = appState.members.find(m => m.id === f.memberId);
            appState.currentFicha = JSON.parse(JSON.stringify(f)); // Deep copy
            setupEditorView();
            showView('view-editor');
        }

        function setupEditorView() {
            const m = appState.activeMember;
            const u = appState.units.find(un => un.id === m.unitId);
            document.getElementById('active-member-name').innerText = m.name;
            document.getElementById('active-member-info').innerText = `${m.cat === 'PEF' ? 'Prof. Ed. Física' : 'Treinador Esportivo'} | Unidade: ${u ? u.name : ''}`;

            // Popula os dados do aluno se for edição
            const f = appState.currentFicha;
            document.getElementById('stu-name').value = f.stuName || '';
            document.getElementById('stu-age').value = f.stuAge || '';
            document.getElementById('stu-weight').value = f.stuWeight || '';
            document.getElementById('stu-height').value = f.stuHeight || '';
            document.getElementById('stu-gender').value = f.stuGender || 'Masculino';
            document.getElementById('stu-level').value = f.stuLevel || 'Iniciante';
            document.getElementById('stu-objective').value = f.stuObjective || 'Emagrecimento';
            document.getElementById('stu-freq').value = f.stuFreq || '3 dias';
            document.getElementById('stu-validity').value = f.validity || 'Validade: 30 dias';
            document.getElementById('stu-recs').value = f.recs || '';

            changeTheme();
            calculateIMC();

            // Set health guidelines based on member category
            const healthDict = m.cat === 'PEF' ? healthDataPEF : healthDataTE;
            const container = document.getElementById('health-container');
            const selectedHealth = f.health || [];
            
            container.innerHTML = Object.keys(healthDict).map(opt => {
                const isChecked = selectedHealth.includes(opt) ? 'checked' : '';
                return `
                <label class="flex items-start space-x-2 cursor-pointer p-1 rounded hover:bg-black hover:bg-opacity-5 transition border border-transparent hover:border-gray-500 hover:border-opacity-20">
                    <input type="checkbox" value="${opt}" ${isChecked} class="health-cb mt-0.5 rounded border-gray-400 text-primary w-3.5 h-3.5">
                    <span class="font-medium">${opt}</span>
                </label>`
            }).join('');

            renderWorkoutsEditor();
        }

        window.calculateIMC = function() {
            const w = parseFloat(document.getElementById('stu-weight').value);
            const h = parseFloat(document.getElementById('stu-height').value);
            const display = document.getElementById('imc-display');
            if (w > 0 && h > 0) {
                const imc = (w / (h * h)).toFixed(1);
                display.innerHTML = `<span class="bg-primary bg-opacity-20 text-primary px-3 py-1 rounded-full text-xs font-bold border border-primary border-opacity-30">IMC: ${imc}</span>`;
            } else display.innerHTML = '';
        }
        window.changeTheme = function() {
            document.body.setAttribute('data-theme', document.getElementById('stu-gender').value);
        }

        // Editor Workout Logic
        window.addWorkoutDay = function() {
            const count = appState.currentFicha.workouts.length;
            const title = count < 7 ? `TREINO ${daysOfWeek[count]}` : `NOVO TREINO ${count+1}`;
            appState.currentFicha.workouts.push({ id: genId(), title: title, exercises: [] });
            renderWorkoutsEditor();
        }
        window.duplicateWorkout = function(id) {
            const w = appState.currentFicha.workouts.find(x => x.id === id);
            if(w) {
                let copy = JSON.parse(JSON.stringify(w));
                copy.id = genId(); copy.title += " (Cópia)";
                appState.currentFicha.workouts.push(copy);
                renderWorkoutsEditor();
            }
        }
        window.removeWorkout = function(id) {
            if(confirm("Remover este dia?")) {
                appState.currentFicha.workouts = appState.currentFicha.workouts.filter(x => x.id !== id);
                renderWorkoutsEditor();
            }
        }
        window.updateWorkoutTitle = function(id, val) {
            const w = appState.currentFicha.workouts.find(x => x.id === id);
            if(w) w.title = val;
        }

        function renderWorkoutsEditor() {
            const container = document.getElementById('workouts-container');
            container.innerHTML = appState.currentFicha.workouts.map(w => {
                let exHtml = w.exercises.length === 0 ? `<div class="p-3 text-center text-xs opacity-50 italic">Sem exercícios</div>` : '';
                w.exercises.forEach((ex, idx) => {
                    exHtml += `
                    <div class="flex flex-col sm:flex-row gap-2 p-2 border-b border-opacity-10 border-gray-500 items-start sm:items-center">
                        <div class="hidden sm:flex flex-col gap-1">
                            <button onclick="moveEx('${w.id}', ${idx}, -1)" class="text-[9px] p-1"><i class="fas fa-chevron-up"></i></button>
                            <button onclick="moveEx('${w.id}', ${idx}, 1)" class="text-[9px] p-1"><i class="fas fa-chevron-down"></i></button>
                        </div>
                        <div class="flex-1">
                            <div class="text-[9px] opacity-60 uppercase">${ex.category}</div>
                            <div class="text-sm font-medium">${ex.name}</div>
                        </div>
                        <div class="flex flex-wrap gap-1 items-center">
                            <input type="text" value="${ex.sets}" onchange="updEx('${w.id}', ${idx}, 'sets', this.value)" class="input-field w-10 text-center text-xs p-1 rounded" placeholder="S">
                            <span class="text-xs opacity-50">x</span>
                            <input type="text" value="${ex.reps}" onchange="updEx('${w.id}', ${idx}, 'reps', this.value)" class="input-field w-14 text-center text-xs p-1 rounded" placeholder="R">
                            <select onchange="updEx('${w.id}', ${idx}, 'technique', this.value)" class="input-field w-20 text-[10px] p-1 rounded">
                                ${dbTechniques.map(t => `<option ${ex.technique===t?'selected':''}>${t}</option>`).join('')}
                            </select>
                            <input type="text" value="${ex.obs}" onchange="updEx('${w.id}', ${idx}, 'obs', this.value)" class="input-field w-full sm:w-32 text-xs p-1 rounded" placeholder="Obs">
                        </div>
                        <button onclick="remEx('${w.id}', ${idx})" class="text-red-500 p-1 text-sm"><i class="fas fa-trash"></i></button>
                    </div>`;
                });

                return `
                <div class="card rounded-xl overflow-hidden shadow-sm">
                    <div class="p-3 border-b border-opacity-20 border-gray-500 bg-black bg-opacity-5 flex justify-between items-center">
                        <input type="text" value="${w.title}" onchange="updateWorkoutTitle('${w.id}', this.value)" class="input-field bg-transparent font-bold w-1/2 p-1 rounded uppercase">
                        <div class="flex gap-1">
                            <button onclick="duplicateWorkout('${w.id}')" class="p-1.5"><i class="fas fa-copy"></i></button>
                            <button onclick="removeWorkout('${w.id}')" class="p-1.5 text-red-500"><i class="fas fa-trash"></i></button>
                        </div>
                    </div>
                    <div>${exHtml}</div>
                    <div class="p-2 border-t border-opacity-20 border-gray-500">
                        <button onclick="openExModal('${w.id}')" class="w-full text-primary font-bold text-xs p-1 hover:bg-primary hover:bg-opacity-10 rounded transition">+ EXERCÍCIO</button>
                    </div>
                </div>`;
            }).join('');
        }

        // Ex Logic
        window.moveEx = function(wId, idx, dir) {
            const w = appState.currentFicha.workouts.find(x => x.id === wId);
            if(!w) return;
            const target = idx + dir;
            if(target >= 0 && target < w.exercises.length) {
                [w.exercises[idx], w.exercises[target]] = [w.exercises[target], w.exercises[idx]];
                renderWorkoutsEditor();
            }
        }
        window.updEx = function(wId, idx, field, val) {
            const w = appState.currentFicha.workouts.find(x => x.id === wId);
            if(w) w.exercises[idx][field] = val;
        }
        window.remEx = function(wId, idx) {
            const w = appState.currentFicha.workouts.find(x => x.id === wId);
            if(w) { w.exercises.splice(idx, 1); renderWorkoutsEditor(); }
        }

        // Ex Modal
        window.openExModal = function(wId) {
            appState.activeWorkoutIdModal = wId;
            document.getElementById('exercise-modal').classList.remove('hidden');
            renderCatModal(); renderExsModal();
        }
        function renderCatModal() {
            document.getElementById('modal-categories').innerHTML = Object.keys(dbCategories).map(c => `
                <button onclick="setCatModal('${c}')" class="w-full text-left p-2 rounded text-xs font-bold ${appState.activeCategory===c ? 'bg-primary text-white':'hover:bg-black hover:bg-opacity-10'}">${c}</button>
            `).join('');
        }
        window.setCatModal = function(c) { appState.activeCategory = c; renderCatModal(); renderExsModal(); }
        function renderExsModal() {
            const exs = dbCategories[appState.activeCategory];
            const isCardio = appState.activeCategory === "🫀 CARDIO";
            document.getElementById('modal-exercises').innerHTML = `<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-2">` + 
                exs.map(ex => `<button onclick="addEx('${ex}', ${isCardio})" class="card p-2 rounded text-left text-xs font-medium border hover:border-primary transition">${ex}</button>`).join('') + `</div>`;
        }
        window.addEx = function(name, isCardio) {
            const w = appState.currentFicha.workouts.find(x => x.id === appState.activeWorkoutIdModal);
            if(w) {
                w.exercises.push({ category: appState.activeCategory, name, sets: isCardio?'1':'3', reps: isCardio?'-':'10 a 12', technique: 'Nenhuma', obs: '' });
                renderWorkoutsEditor();
            }
        }

        // --- SALVAR E IMPRIMIR ---
        window.saveFicha = async function(doPrint) {
            const f = appState.currentFicha;
            f.stuName = document.getElementById('stu-name').value || 'Sem Nome';
            f.stuAge = document.getElementById('stu-age').value;
            f.stuWeight = document.getElementById('stu-weight').value;
            f.stuHeight = document.getElementById('stu-height').value;
            f.stuGender = document.getElementById('stu-gender').value;
            f.stuLevel = document.getElementById('stu-level').value;
            f.stuObjective = document.getElementById('stu-objective').value;
            f.stuFreq = document.getElementById('stu-freq').value;
            f.validity = document.getElementById('stu-validity').value;
            f.recs = document.getElementById('stu-recs').value;
            f.health = Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value);
            
            f.memberId = appState.activeMember.id;
            f.unitId = appState.activeMember.unitId;
            f.createdAt = f.createdAt || new Date().toISOString();

            // Salva no Firebase
            const uid = appState.user.uid;
            await setDoc(doc(db, "users", uid, "fichas", f.id), f);

            // Atualiza estado local
            const existingIdx = appState.fichas.findIndex(x => x.id === f.id);
            if(existingIdx >= 0) appState.fichas[existingIdx] = f;
            else appState.fichas.push(f);

            if(doPrint) executePrintPlanilha();
            else {
                renderFichasList();
                showView('view-dashboard');
            }
        }

        function executePrintPlanilha() {
            const f = appState.currentFicha;
            const m = appState.activeMember;
            const isPEF = m.cat === 'PEF';
            const dict = isPEF ? healthDataPEF : healthDataTE;

            let imcStr = "-";
            const w = parseFloat(f.stuWeight); const h = parseFloat(f.stuHeight);
            if(w>0 && h>0) imcStr = (w / (h * h)).toFixed(1);

            let html = `
                <div class="print-header">
                    <h1 class="print-title">Planilha de Treinamento - ${f.stuObjective}</h1>
                    <div class="prof-info">
                        Prescrição feita por: ${m.name} (${isPEF ? 'Prof. de Ed. Física' : 'Treinador Esportivo'})<br>
                        ${isPEF ? `CREF: ${m.cref} - ${m.state}` : ''}
                    </div>
                </div>

                <div class="print-grid">
                    <div><strong>Aluno:</strong> ${f.stuName}</div>
                    <div><strong>Idade:</strong> ${f.stuAge || '-'} anos</div>
                    <div><strong>Gênero:</strong> ${f.stuGender}</div>
                    <div><strong>Peso:</strong> ${f.stuWeight || '-'} kg</div>
                    <div><strong>Altura:</strong> ${f.stuHeight || '-'} m</div>
                    <div><strong>IMC:</strong> ${imcStr}</div>
                    <div><strong>Nível:</strong> ${f.stuLevel}</div>
                    <div><strong>Frequência:</strong> ${f.stuFreq}</div>
                    <div><strong>Validade:</strong> ${f.validity.replace('Validade: ','')}</div>
                </div>`;

            const objRec = objectiveData[f.stuObjective] || '';
            if(objRec || f.health.length > 0) {
                html += `<div class="print-guidelines"><h4>Diretrizes Automáticas de Perfil</h4><ul>`;
                if(objRec) html += `<li><strong>Objetivo (${f.stuObjective}):</strong> ${objRec}</li>`;
                f.health.forEach(k => {
                    if(dict[k]) html += `<li><strong>Saúde (${k.replace(/[\u{1F300}-\u{1F9FF}\u{2600}-\u{26FF}\u{2700}-\u{27BF}\u{FE0F}]/gu, '').trim()}):</strong> ${dict[k]}</li>`;
                });
                html += `</ul></div>`;
            }

            f.workouts.forEach(wk => {
                html += `<div class="print-workout"><h3>${wk.title}</h3><table>
                    <thead><tr><th style="width:35%">Exercício</th><th style="width:8%;text-align:center;">Séries</th><th style="width:12%;text-align:center;">Reps</th><th style="width:15%">Técnica</th><th style="width:30%">Observações</th></tr></thead><tbody>`;
                if(wk.exercises.length === 0) html += `<tr><td colspan="5" style="text-align:center;font-style:italic;">Sem exercícios</td></tr>`;
                else {
                    wk.exercises.forEach(ex => {
                        html += `<tr><td><strong>${ex.name}</strong></td><td style="text-align:center;">${ex.sets}</td><td style="text-align:center;">${ex.reps}</td><td>${ex.technique!=='Nenhuma'?ex.technique:'-'}</td><td>${ex.obs||'-'}</td></tr>`;
                    });
                }
                html += `</tbody></table></div>`;
            });

            html += `<div class="print-footer-section">`;
            if(f.recs) html += `<strong>Recomendações Manuais:</strong><br><p style="white-space:pre-wrap; margin-top:2px;">${f.recs}</p>`;
            
            const txtPEF = "⚠️ OBSERVAÇÃO LEGAL – PROFISSIONAL DE EDUCAÇÃO FÍSICA: Conforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º estabelece que compete ao profissional coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos especializados nas áreas de atividades físicas e do desporto.";
            const txtTE = "⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO: Conforme a Lei nº 14.597/2023 (Lei Geral do Esporte), Art. 75, a profissão de treinador esportivo é reconhecida e regulada no Brasil. As recomendações desta ficha possuem caráter informativo e orientativo. Este material não substitui avaliação médica ou acompanhamento de profissionais da saúde. Em casos de dores, lesões ou doenças, recomenda-se procurar um profissional habilitado.";
            
            html += `<div class="legal-text">${isPEF ? txtPEF : txtTE}</div>
                     <div style="text-align:center; margin-top: 10px; font-size: 8px;">Ficha gerada em ${new Date(f.createdAt).toLocaleDateString('pt-BR')} - PowFit Pro</div></div>`;

            document.getElementById('print-area').innerHTML = html;
            setTimeout(() => { 
                window.print();
                // Return to dashboard after printing
                renderFichasList();
                showView('view-dashboard');
            }, 300);
        }

    </script>
</body>
</html>
