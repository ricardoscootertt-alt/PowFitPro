<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro - Plataforma de Treinos</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');

        :root {
            --bg-color: #111827; /* gray-900 */
            --surface-color: #1f2937; /* gray-800 */
            --text-main: #f3f4f6; /* gray-100 */
            --text-muted: #9ca3af; /* gray-400 */
            --primary: #3b82f6; /* blue-500 */
            --primary-hover: #2563eb; /* blue-600 */
            --border-color: #374151; /* gray-700 */
            --input-bg: #374151;
        }

        body.theme-female {
            --bg-color: #fdf2f8; /* pink-50 */
            --surface-color: #ffffff;
            --text-main: #1f2937; /* gray-800 */
            --text-muted: #6b7280; /* gray-500 */
            --primary: #ec4899; /* pink-500 */
            --primary-hover: #db2777; /* pink-600 */
            --border-color: #fbcfe8; /* pink-200 */
            --input-bg: #f9fafb;
        }

        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg-color);
            color: var(--text-main);
            transition: all 0.3s ease;
        }

        .surface { background-color: var(--surface-color); border-color: var(--border-color); }
        .text-main { color: var(--text-main); }
        .text-muted { color: var(--text-muted); }
        .btn-primary { background-color: var(--primary); color: white; }
        .btn-primary:hover { background-color: var(--primary-hover); }
        .border-theme { border-color: var(--border-color); }
        .input-theme { 
            background-color: var(--input-bg); 
            color: var(--text-main); 
            border: 1px solid var(--border-color);
        }
        .input-theme:focus { outline: none; border-color: var(--primary); ring: 2px var(--primary); }

        /* Custom Scrollbar */
        ::-webkit-scrollbar { width: 8px; height: 8px; }
        ::-webkit-scrollbar-track { background: var(--bg-color); }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: var(--primary); }

        /* Print Styles */
        @media print {
            body { background-color: white !important; color: black !important; }
            .no-print { display: none !important; }
            .print-only { display: block !important; }
            #print-area { display: block !important; padding: 0; }
            .page-break { page-break-before: always; }
            .avoid-break { page-break-inside: avoid; }
            table { width: 100%; border-collapse: collapse; }
            th, td { border: 1px solid #ddd; padding: 8px; text-align: left; }
            th { background-color: #f3f4f6 !important; -webkit-print-color-adjust: exact; }
            .legal-footer { font-size: 10px; color: #555; margin-top: 20px; text-align: justify; border-top: 1px solid #ccc; padding-top: 10px; }
        }

        .print-only { display: none; }
        
        /* Toast */
        #toast-container { position: fixed; bottom: 20px; right: 20px; z-index: 9999; }
        .toast { padding: 12px 24px; border-radius: 8px; margin-top: 10px; color: white; opacity: 0; transform: translateY(20px); transition: all 0.3s ease; box-shadow: 0 4px 6px rgba(0,0,0,0.1); }
        .toast.show { opacity: 1; transform: translateY(0); }
        .toast.success { background-color: #10b981; }
        .toast.error { background-color: #ef4444; }
        .toast.info { background-color: var(--primary); }

        .tab-active { border-bottom: 2px solid var(--primary); color: var(--primary); }
    </style>
</head>
<body class="min-h-screen flex flex-col">

    <!-- Toast Notifications -->
    <div id="toast-container"></div>

    <!-- Login Screen -->
    <div id="login-screen" class="flex-1 flex items-center justify-center p-4">
        <div class="surface p-8 rounded-2xl shadow-xl max-w-md w-full text-center border border-theme">
            <div class="text-5xl text-primary mb-4"><i class="fas fa-dumbbell"></i></div>
            <h1 class="text-3xl font-bold mb-2 text-main">PowFit Pro</h1>
            <p class="text-muted mb-8">Plataforma Profissional de Gestão e Montagem de Treinos.</p>
            <button id="btn-login" class="w-full bg-white text-gray-800 font-semibold py-3 px-4 rounded-lg shadow-md hover:bg-gray-100 flex items-center justify-center gap-3 transition">
                <img src="https://www.svgrepo.com/show/475656/google-color.svg" alt="Google" class="w-6 h-6">
                Entrar com Google
            </button>
        </div>
    </div>

    <!-- Main Dashboard -->
    <div id="dashboard-screen" class="hidden flex-1 flex flex-col h-screen overflow-hidden">
        
        <!-- Navbar -->
        <header class="surface border-b border-theme p-4 flex justify-between items-center no-print">
            <div class="flex items-center gap-3">
                <i class="fas fa-dumbbell text-primary text-2xl"></i>
                <span class="text-xl font-bold text-main">PowFit Pro</span>
            </div>
            <nav class="hidden md:flex gap-6">
                <button onclick="switchView('builder')" class="nav-btn text-main hover:text-primary transition font-medium" data-target="builder">Nova Ficha</button>
                <button onclick="switchView('network')" class="nav-btn text-muted hover:text-primary transition font-medium" data-target="network">Gestão de Rede</button>
                <button onclick="switchView('reports')" class="nav-btn text-muted hover:text-primary transition font-medium" data-target="reports">Relatórios</button>
            </nav>
            <div class="flex items-center gap-4">
                <div class="text-right hidden sm:block">
                    <p id="user-name-display" class="text-sm font-semibold text-main"></p>
                    <p class="text-xs text-muted">Administrador</p>
                </div>
                <button id="btn-logout" class="text-red-500 hover:text-red-400 p-2"><i class="fas fa-sign-out-alt"></i></button>
            </div>
        </header>

        <!-- Main Content Area -->
        <main class="flex-1 overflow-y-auto p-4 md:p-8 no-print pb-24">
            
            <!-- 1. Gestão de Rede View -->
            <div id="view-network" class="hidden max-w-5xl mx-auto space-y-6">
                <h2 class="text-2xl font-bold mb-6 border-b border-theme pb-2 text-main"><i class="fas fa-network-wired mr-2"></i> Gestão da Rede e Equipe</h2>
                
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <!-- Config Rede e Unidades -->
                    <div class="surface p-6 rounded-xl border border-theme">
                        <h3 class="text-lg font-semibold mb-4 text-main">1. Unidades da Rede</h3>
                        <form id="form-unit" class="flex gap-2 mb-4">
                            <input type="text" id="unit-name" placeholder="Nome da Unidade (Ex: Matriz Centro)" class="input-theme flex-1 p-2 rounded-lg text-sm" required>
                            <button type="submit" class="btn-primary px-4 py-2 rounded-lg text-sm font-medium">Adicionar</button>
                        </form>
                        <ul id="units-list" class="space-y-2 mt-4 max-h-48 overflow-y-auto pr-2">
                            <!-- Units injected here -->
                        </ul>
                    </div>

                    <!-- Add Member -->
                    <div class="surface p-6 rounded-xl border border-theme">
                        <h3 class="text-lg font-semibold mb-4 text-main">2. Cadastrar Membro (Equipe)</h3>
                        <form id="form-member" class="space-y-3">
                            <select id="member-unit" class="input-theme w-full p-2 rounded-lg text-sm" required>
                                <option value="">Selecione a Unidade...</option>
                            </select>
                            <input type="text" id="member-name" placeholder="Nome Completo" class="input-theme w-full p-2 rounded-lg text-sm" required>
                            <div class="grid grid-cols-2 gap-3">
                                <input type="text" id="member-cpf" placeholder="CPF" class="input-theme w-full p-2 rounded-lg text-sm" required>
                                <input type="text" id="member-uf" placeholder="Estado (UF)" class="input-theme w-full p-2 rounded-lg text-sm uppercase" maxlength="2" required>
                            </div>
                            <select id="member-role" class="input-theme w-full p-2 rounded-lg text-sm" required onchange="toggleCrefField()">
                                <option value="">Selecione a Categoria...</option>
                                <option value="PEF">Profissional de Educação Física (PEF)</option>
                                <option value="TE">Treinador Esportivo (TE)</option>
                            </select>
                            <input type="text" id="member-cref" placeholder="Número do CREF" class="input-theme w-full p-2 rounded-lg text-sm hidden">
                            <button type="submit" class="btn-primary w-full py-2 rounded-lg text-sm font-medium mt-2">Cadastrar Membro</button>
                        </form>
                    </div>
                </div>

                <!-- Members List -->
                <div class="surface p-6 rounded-xl border border-theme mt-6">
                    <h3 class="text-lg font-semibold mb-4 text-main">Equipe Cadastrada</h3>
                    <div class="overflow-x-auto">
                        <table class="w-full text-sm text-left">
                            <thead class="text-xs text-muted uppercase bg-black/10">
                                <tr>
                                    <th class="px-4 py-3 rounded-tl-lg">Nome</th>
                                    <th class="px-4 py-3">Unidade</th>
                                    <th class="px-4 py-3">Categoria</th>
                                    <th class="px-4 py-3 rounded-tr-lg">Ações</th>
                                </tr>
                            </thead>
                            <tbody id="members-table-body">
                                <!-- Members injected here -->
                            </tbody>
                        </table>
                    </div>
                </div>
                
                <!-- Custom Exercises Config -->
                <div class="surface p-6 rounded-xl border border-theme mt-6">
                    <h3 class="text-lg font-semibold mb-4 text-main"><i class="fas fa-plus-circle mr-2"></i> Banco de Exercícios Customizados</h3>
                    <form id="form-custom-exercise" class="flex flex-wrap gap-3 mb-4">
                        <select id="custom-ex-muscle" class="input-theme p-2 rounded-lg text-sm flex-1 min-w-[150px]" required>
                            <option value="">Grupo Muscular...</option>
                            <option value="PEITO">Peito</option><option value="COSTAS">Costas</option>
                            <option value="PERNAS">Pernas</option><option value="BRACOS_BICEPS">Bíceps</option>
                            <option value="BRACOS_TRICEPS">Tríceps</option><option value="OMBROS">Ombros</option>
                            <option value="ABDOMEN">Abdômen</option><option value="CARDIO">Cardio</option>
                        </select>
                        <input type="text" id="custom-ex-name" placeholder="Nome do Exercício" class="input-theme p-2 rounded-lg text-sm flex-[2] min-w-[200px]" required>
                        <button type="submit" class="btn-primary px-4 py-2 rounded-lg text-sm font-medium">Adicionar Exercício</button>
                    </form>
                    <div id="custom-exercises-list" class="flex flex-wrap gap-2 mt-4">
                        <!-- Custom badges injected here -->
                    </div>
                </div>
            </div>

            <!-- 2. Builder View (Prancheta) -->
            <div id="view-builder" class="max-w-6xl mx-auto space-y-6">
                <!-- Header Actions -->
                <div class="flex flex-wrap gap-4 items-center justify-between surface p-4 rounded-xl border border-theme sticky top-0 z-10 shadow-lg">
                    <div class="flex-1 min-w-[250px]">
                        <label class="block text-xs text-muted mb-1">Profissional Responsável pela Ficha</label>
                        <select id="builder-member-select" class="input-theme w-full p-2 rounded-lg text-sm font-medium" required onchange="renderHealthOptions()">
                            <option value="">Selecione seu perfil...</option>
                        </select>
                    </div>
                    <div class="flex gap-2">
                        <button onclick="saveAndPrint()" class="bg-green-600 hover:bg-green-700 text-white px-5 py-2 rounded-lg font-medium shadow flex items-center gap-2 transition">
                            <i class="fas fa-print"></i> Salvar e Imprimir
                        </button>
                        <button onclick="clearBuilder()" class="bg-red-500/20 text-red-500 hover:bg-red-500 hover:text-white px-4 py-2 rounded-lg font-medium transition">
                            Limpar
                        </button>
                    </div>
                </div>

                <!-- Client Info -->
                <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                    <div class="lg:col-span-2 surface p-6 rounded-xl border border-theme">
                        <h3 class="text-lg font-semibold mb-4 border-b border-theme pb-2 text-main"><i class="fas fa-user mr-2"></i> Dados do Aluno</h3>
                        <div class="grid grid-cols-2 sm:grid-cols-4 gap-4">
                            <div class="col-span-2 sm:col-span-4">
                                <label class="block text-xs text-muted mb-1">Nome Completo</label>
                                <input type="text" id="client-name" class="input-theme w-full p-2 rounded-lg text-sm" placeholder="Nome do aluno">
                            </div>
                            <div>
                                <label class="block text-xs text-muted mb-1">Idade</label>
                                <input type="number" id="client-age" class="input-theme w-full p-2 rounded-lg text-sm">
                            </div>
                            <div>
                                <label class="block text-xs text-muted mb-1">Gênero</label>
                                <select id="client-gender" class="input-theme w-full p-2 rounded-lg text-sm" onchange="toggleTheme()">
                                    <option value="M">Masculino</option>
                                    <option value="F">Feminino</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs text-muted mb-1">Peso (kg)</label>
                                <input type="number" id="client-weight" step="0.1" class="input-theme w-full p-2 rounded-lg text-sm" oninput="calculateBMI()">
                            </div>
                            <div>
                                <label class="block text-xs text-muted mb-1">Altura (cm)</label>
                                <input type="number" id="client-height" class="input-theme w-full p-2 rounded-lg text-sm" oninput="calculateBMI()">
                            </div>
                        </div>
                        
                        <div class="mt-4 p-3 rounded-lg bg-black/20 border border-theme flex items-center justify-between">
                            <span class="text-sm text-muted">IMC Calculado: <strong id="bmi-value" class="text-main text-lg ml-2">--</strong></span>
                            <span id="bmi-status" class="text-sm font-medium px-3 py-1 rounded-full bg-gray-500/20 text-gray-400">Aguardando dados</span>
                        </div>

                        <div class="grid grid-cols-1 sm:grid-cols-3 gap-4 mt-4">
                            <div>
                                <label class="block text-xs text-muted mb-1">Nível</label>
                                <select id="client-level" class="input-theme w-full p-2 rounded-lg text-sm">
                                    <option>Iniciante</option><option>Intermediário</option><option>Avançado</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs text-muted mb-1">Frequência</label>
                                <select id="client-freq" class="input-theme w-full p-2 rounded-lg text-sm">
                                    <option>3 dias</option><option>5 dias</option><option>6 dias</option><option>Personalizado</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs text-muted mb-1">Validade da Ficha</label>
                                <select id="client-validity" class="input-theme w-full p-2 rounded-lg text-sm">
                                    <option value="15">15 dias</option><option value="30" selected>30 dias</option>
                                    <option value="60">60 dias</option><option value="90">90 dias</option>
                                </select>
                            </div>
                        </div>

                        <div class="mt-4">
                            <label class="block text-xs text-muted mb-1">Objetivo Principal</label>
                            <select id="client-objective" class="input-theme w-full p-2 rounded-lg text-sm mb-2" onchange="updateObjectiveText()">
                                <option value="Emagrecimento">Emagrecimento</option>
                                <option value="Hipertrofia">Hipertrofia</option>
                                <option value="Definição">Definição</option>
                                <option value="Condicionamento">Condicionamento</option>
                                <option value="Resistência">Resistência</option>
                                <option value="Força">Força</option>
                                <option value="Reabilitação">Reabilitação</option>
                                <option value="Saúde geral">Saúde geral</option>
                            </select>
                            <p id="objective-help" class="text-xs text-muted italic bg-black/10 p-2 rounded border border-theme">Selecione um objetivo para ver os detalhes.</p>
                        </div>
                    </div>

                    <!-- Health Status -->
                    <div class="surface p-6 rounded-xl border border-theme h-full overflow-hidden flex flex-col">
                        <h3 class="text-lg font-semibold mb-4 border-b border-theme pb-2 text-main"><i class="fas fa-heartbeat mr-2"></i> Estado de Saúde</h3>
                        <p class="text-xs text-muted mb-3 italic">Selecione para adicionar orientações à impressão. (As opções mudam conforme sua categoria profissional).</p>
                        <div id="health-options-container" class="space-y-2 overflow-y-auto flex-1 pr-2 text-sm">
                            <!-- Injected dynamically based on PEF/TE -->
                            <p class="text-center text-muted mt-10">Selecione o Profissional Responsável acima primeiro.</p>
                        </div>
                    </div>
                </div>

                <!-- Workout Builder Area -->
                <div class="surface rounded-xl border border-theme overflow-hidden">
                    <div class="bg-black/20 border-b border-theme p-2 flex gap-2 overflow-x-auto" id="workout-tabs-container">
                        <!-- Tabs injected here -->
                    </div>
                    
                    <div class="p-4 md:p-6" id="workout-content-area">
                        <!-- Content injected here -->
                    </div>
                </div>

                <!-- General Recommendations -->
                <div class="surface p-6 rounded-xl border border-theme">
                    <h3 class="text-lg font-semibold mb-2 text-main"><i class="fas fa-comment-medical mr-2"></i> Recomendações Gerais do Profissional</h3>
                    <textarea id="general-notes" class="input-theme w-full p-3 rounded-lg text-sm min-h-[100px]" placeholder="Ex: Manter hidratação alta, aquecer 10 min na esteira antes do treino..."></textarea>
                </div>
            </div>

            <!-- 3. Reports View -->
            <div id="view-reports" class="hidden max-w-5xl mx-auto space-y-6">
                <h2 class="text-2xl font-bold mb-6 border-b border-theme pb-2 text-main"><i class="fas fa-chart-bar mr-2"></i> Relatório de Produtividade</h2>
                <div class="surface p-6 rounded-xl border border-theme">
                    <div class="flex gap-4 mb-6">
                        <select id="report-month" class="input-theme p-2 rounded-lg text-sm flex-1">
                            <!-- Populated by JS -->
                        </select>
                        <select id="report-unit" class="input-theme p-2 rounded-lg text-sm flex-1">
                            <option value="ALL">Todas as Unidades</option>
                        </select>
                        <button onclick="generateReport()" class="btn-primary px-4 py-2 rounded-lg text-sm font-medium">Gerar Relatório</button>
                        <button onclick="window.print()" class="bg-gray-600 hover:bg-gray-700 text-white px-4 py-2 rounded-lg text-sm font-medium"><i class="fas fa-print"></i></button>
                    </div>
                    
                    <div id="report-results" class="hidden">
                        <h3 class="text-lg font-bold mb-4 text-center" id="report-title">Resumo Mensal</h3>
                        <div class="overflow-x-auto">
                            <table class="w-full text-sm text-left border border-theme">
                                <thead class="text-xs text-muted uppercase bg-black/10">
                                    <tr>
                                        <th class="px-4 py-3 border-b border-theme">Membro / Profissional</th>
                                        <th class="px-4 py-3 border-b border-theme">Unidade</th>
                                        <th class="px-4 py-3 border-b border-theme text-center">Fichas Criadas</th>
                                    </tr>
                                </thead>
                                <tbody id="report-table-body">
                                    <!-- Results injected -->
                                </tbody>
                                <tfoot>
                                    <tr class="bg-black/20 font-bold">
                                        <td colspan="2" class="px-4 py-3 text-right">TOTAL GERAL:</td>
                                        <td id="report-total" class="px-4 py-3 text-center text-primary text-lg">0</td>
                                    </tr>
                                </tfoot>
                            </table>
                        </div>
                    </div>
                </div>
            </div>

        </main>
    </div>

    <!-- Hidden Print Area -->
    <div id="print-area" class="print-only bg-white text-black p-8 font-sans w-full max-w-[21cm] mx-auto hidden">
        <div id="print-content"></div>
    </div>

    <!-- Mobile Nav Bar (Bottom) -->
    <nav class="md:hidden fixed bottom-0 left-0 w-full surface border-t border-theme flex justify-around p-3 pb-safe z-50 no-print">
        <button onclick="switchView('builder')" class="flex flex-col items-center text-muted hover:text-primary nav-btn" data-target="builder">
            <i class="fas fa-clipboard-list text-xl mb-1"></i><span class="text-[10px]">Ficha</span>
        </button>
        <button onclick="switchView('network')" class="flex flex-col items-center text-muted hover:text-primary nav-btn" data-target="network">
            <i class="fas fa-users text-xl mb-1"></i><span class="text-[10px]">Equipe</span>
        </button>
        <button onclick="switchView('reports')" class="flex flex-col items-center text-muted hover:text-primary nav-btn" data-target="reports">
            <i class="fas fa-chart-pie text-xl mb-1"></i><span class="text-[10px]">Relatórios</span>
        </button>
    </nav>

    <!-- CORE LOGIC & FIREBASE -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getAuth, signInWithPopup, GoogleAuthProvider, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
        import { getFirestore, collection, doc, setDoc, addDoc, onSnapshot, query, deleteDoc, serverTimestamp, getDocs, where } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        // ==========================================
        // 1. CONFIGURAÇÃO E DADOS ESTÁTICOS
        // ==========================================
        const firebaseConfig = {
            apiKey: "AIzaSyD-iuERHA_x1f69smJXh7b8wTLO1zTadIU",
            authDomain: "powfitpro-d8577.firebaseapp.com",
            projectId: "powfitpro-d8577",
            storageBucket: "powfitpro-d8577.firebasestorage.app",
            messagingSenderId: "651381183985",
            appId: "1:651381183985:web:9a425692372a9d67d9e050"
        };
        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const APP_ID = "powfitpro"; // Prefix to satisfy strict artifact rules safely

        // Databases
        const DB_EXERCISES = {
            "PEITO": ["Supino Reto", "Supino Inclinado", "Supino Inclinado com Halteres", "Supino Fechado com Halteres", "Cross Over", "Cross Over Alto", "Cross Over Baixo", "Crucifixo Reto", "Crucifixo Inclinado com Halteres", "Crucifixo na Máquina", "Peck Fly", "Peck Fly Unilateral", "Pullover", "Flexão de Braço", "Flexão com Pés Elevados", "Flexão Explosiva"],
            "COSTAS": ["Puxada Alta", "Puxada de Frente Supinada", "Pulldown", "Remada Aberta", "Remada Baixa", "Remada Curvada", "Remada Curvada Supinada", "Remada Cavalinho (T-Bar)", "Serrote", "Facepull (puxada de cima para baixo)"],
            "PERNAS": ["Agachamento Livre", "Agachamento Taça", "Agachamento no Smith", "Agachamento com passada lateral", "Squat", "Hack Machine", "Leg 45°", "Leg 90°", "Agachamento Sumô", "Agachamento Sissy (Livre)", "Afundo", "Recuo", "Avanço", "Passada", "Búlgaro", "Step-up", "Levantamento Terra", "Levantamento Terra Romeno", "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", "Cadeira Flexora", "Elevação Pélvica no Banco", "Elevação Pélvica no Chão", "Elevação Pélvica Unilateral no Chão", "Extensão de Quadril (Glúteo Máximo)", "Extensão Cruzada (Glúteo Médio)", "Coice", "Cachorrinho", "Cadeira Extensora", "Adução", "Abdução", "Abdução Inclinada", "Flexão Nórdica", "Flexão Nórdica Invertida", "Panturrilha Livre", "Panturrilha no Leg Press", "Panturrilha Banco", "Panturrilha Squat", "Panturrilha Unilateral"],
            "BRACOS_BICEPS": ["Rosca Direta", "Rosca Alternada", "Rosca 21", "Rosca Scott Barra W", "Rosca Scott Unilateral", "Rosca Scott com Halteres", "Rosca Martelo", "Rosca Cross", "Rosca Inversa", "Rosca 45°"],
            "BRACOS_TRICEPS": ["Triceps Pulley Unilateral", "Tríceps Pulley Barra", "Tríceps Pulley Corda", "Tríceps Pulley Pegada Inversa", "Tríceps Francês na Corda", "Tríceps Francês com Halter", "Tríceps Francês Unilateral", "Tríceps Cruzado Polia Dupla", "Tríceps Coice Unilateral", "Tríceps Arremesso", "Tríceps Testa", "Mergulho no Banco"],
            "OMBROS": ["Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", "Elevação Lateral Unilateral Cross", "Elevação Lateral Sentado", "Desenvolvimento com Halteres", "Desenvolvimento com Barra", "Arnold Press", "Elevação Borboleta", "Crucifixo Inverso Sentado com Halteres", "Crucifixo Inverso na Polia", "Crucifixo Inverso Unilateral na Polia", "Facepull (puxada reta)", "Remada Alta", "Encolhimento (Trapézio)"],
            "ABDOMEN": ["Infra com Elevação de Perna", "Abdominal Supra", "Abdominal Remador", "Abdominal Bicicleta", "Abdominal Twister com Peso", "Prancha", "Prancha Lateral", "Trituração de Cabos em Pé", "Isometria na parede"],
            "CARDIO": ["Bicicleta 10 Minutos", "Bicicleta 15 Minutos", "Bicicleta 20 Minutos", "Esteira 10 Minutos", "Esteira 15 Minutos", "Esteira 20 Minutos", "Pular Corda"]
        };

        const TECHNIQUES = ["Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", "Negativa", "Isometria", "Parciais", "Pirâmide"];

        const OBJECTIVES_INFO = {
            "Emagrecimento": "Foco em déficit calórico com treinos mistos de força (manter massa magra) e aeróbicos (maior gasto).",
            "Hipertrofia": "Prioridade na progressão de carga e volume adequado. Essencial superávit calórico e descanso.",
            "Definição": "Manutenção de massa muscular enquanto reduz o percentual de gordura. Atenção estrita à dieta.",
            "Condicionamento": "Treinos com menor tempo de intervalo, circuitos e alta integração cardiopulmonar.",
            "Resistência": "Séries mais longas, cadência controlada e aprimoramento da capacidade muscular.",
            "Força": "Cargas altas, baixas repetições e intervalos de descanso maiores.",
            "Reabilitação": "Treino focado em fortalecimento específico, mobilidade e controle motor. Respeitar limites.",
            "Saúde geral": "Equilíbrio entre força, cardio e flexibilidade. O principal objetivo é a constância e bem-estar."
        };

        const HEALTH_STATUS = [
            { id: "saudavel", label: "🟢 Saudável" }, { id: "sedentario", label: "⚪ Sedentário" },
            { id: "sobrepeso", label: "🟡 Sobrepeso" }, { id: "obesidade", label: "🔴 Obesidade" },
            { id: "baixopeso", label: "⚖️ Baixo peso" }, { id: "diabetes", label: "🍬 Diabetes" },
            { id: "hipertensao", label: "❤️ Hipertensão" }, { id: "hipotensao", label: "🔵 Hipotensão" },
            { id: "cardiaco", label: "💔 Problemas cardíacos" }, { id: "articular", label: "🦴 Problemas articulares" },
            { id: "respiratorio", label: "🫁 Problemas respiratórios" }, { id: "lesao", label: "⚠️ Lesões" },
            { id: "gestante", label: "🤰 Gestante" }, { id: "lactante", label: "🤱 Lactante" },
            { id: "idoso", label: "👴 Idoso" }
        ];

        const HEALTH_INFO = {
            "PEF": {
                "saudavel": "Manter treinos regulares 3–5x por semana, combinando musculação e cardio. Foco em evolução e constância.",
                "sedentario": "Iniciar com treinos leves 2–3x por semana. Priorizar adaptação, técnica e evitar excesso de carga.",
                "sobrepeso": "Treinar 3–5x por semana com foco em gasto calórico. Combinar musculação e cardio moderado.",
                "obesidade": "Iniciar com exercícios de baixo impacto. Evolução gradual, priorizando saúde e segurança.",
                "baixopeso": "Foco em musculação para ganho de massa. Treinos moderados com alimentação adequada.",
                "diabetes": "Treinos regulares moderados. Monitorar glicemia e evitar treinos em jejum.",
                "hipertensao": "Treinos moderados, evitar prender a respiração. Priorizar controle da intensidade.",
                "hipotensao": "Evitar mudanças bruscas. Manter hidratação e intensidade leve a moderada.",
                "cardiaco": "Treinos leves com liberação médica. Monitorar frequência cardíaca.",
                "articular": "Evitar impacto. Priorizar exercícios controlados e máquinas.",
                "respiratorio": "Treinos leves a moderados com progressão gradual. Atenção à respiração.",
                "lesao": "Adaptar exercícios. Evitar dor e focar na recuperação.",
                "gestante": "Treinos leves a moderados, sem impacto ou risco. Foco em mobilidade e bem-estar.",
                "lactante": "Treinar normalmente com atenção à hidratação e energia.",
                "idoso": "Foco em força, equilíbrio e mobilidade. Intensidade moderada com segurança."
            },
            "TE": {
                "saudavel": "Manter treinos regulares de 3–5x por semana, combinando musculação e atividades cardiovasculares. A constância, descanso adequado e boa alimentação contribuem para melhores resultados.",
                "sedentario": "O início deve ser gradual, com treinos leves e foco na adaptação do corpo. A prioridade é desenvolver constância, aprender a execução correta e evitar excesso de carga.",
                "sobrepeso": "A prática regular de musculação associada ao cardio pode contribuir para melhora do condicionamento físico e composição corporal. A progressão deve respeitar a individualidade e o nível de adaptação.",
                "obesidade": "É recomendado iniciar com exercícios de menor impacto e progressão gradual. A segurança, mobilidade e constância são prioridades durante o processo.",
                "baixopeso": "A musculação pode auxiliar no ganho de massa muscular quando associada a alimentação adequada e descanso. O treino deve priorizar evolução progressiva e recuperação muscular.",
                "diabetes": "Pessoas com diabetes devem manter acompanhamento médico regular antes e durante a prática de exercícios. A musculação pode auxiliar na rotina de atividade física quando liberada por profissional de saúde. Em casos de tontura, mal-estar ou alteração de glicemia, o treino deve ser interrompido.",
                "hipertensao": "Pessoas com hipertensão devem manter acompanhamento médico regular e respeitar as orientações profissionais. Durante o treino, é importante evitar prender a respiração e controlar a intensidade.",
                "hipotensao": "Pessoas com hipotensão devem evitar mudanças bruscas de posição durante o treino. Manter boa hidratação e respeitar a intensidade adequada ajuda a reduzir episódios de mal-estar.",
                "cardiaco": "A prática de exercícios deve ocorrer apenas com liberação e acompanhamento médico. O treino deve respeitar limites individuais, com controle de intensidade e atenção aos sinais do corpo.",
                "articular": "Exercícios com menor impacto e maior controle de movimento costumam ser mais indicados. O acompanhamento profissional e a adaptação dos exercícios ajudam na segurança.",
                "respiratorio": "A progressão do treino deve ser gradual e respeitar a capacidade respiratória individual. Em caso de falta de ar excessiva ou desconforto, o exercício deve ser interrompido.",
                "lesao": "A adaptação dos exercícios deve respeitar a limitação existente e evitar dor durante a execução. Em situações de lesão, é importante seguir orientação profissional adequada.",
                "gestante": "A prática de exercícios deve ocorrer com liberação médica e acompanhamento adequado. O foco deve ser segurança, mobilidade e bem-estar, evitando impacto excessivo e situações de risco.",
                "lactante": "A atividade física pode ser mantida normalmente, respeitando a recuperação individual e mantendo boa hidratação e alimentação adequada.",
                "idoso": "A musculação pode contribuir para força, equilíbrio, mobilidade e autonomia. O treino deve respeitar limitações individuais, com intensidade moderada e foco na segurança."
            }
        };

        const LEGAL_TEXTS = {
            "PEF": "⚠️ OBSERVAÇÃO LEGAL – PROFISSIONAL DE EDUCAÇÃO FÍSICA\nConforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º da mesma lei estabelece que compete ao profissional de Educação Física coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos especializados nas áreas de atividades físicas e do desporto.",
            "TE": "⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO\nConforme a Lei nº 14.597/2023 (Lei Geral do Esporte), Art. 75, a profissão de treinador esportivo é reconhecida e regulada no Brasil, com atuação de caráter técnico e esportivo voltada à preparação, supervisão, orientação e acompanhamento de treinos físicos e esportivos. A atuação possui finalidade orientativa e não substitui avaliação médica, diagnóstico clínico, prescrição medicamentosa ou acompanhamento de profissionais da saúde. Em casos de doenças, lesões, gestação, limitações físicas, uso de medicações ou qualquer condição específica de saúde, recomenda-se avaliação prévia por profissional habilitado antes do início ou continuidade da prática de exercícios. O treinamento proposto respeita os limites individuais, priorizando segurança, execução correta e evolução progressiva, dentro da atuação técnica e esportiva permitida por lei. As recomendações desta ficha possuem caráter informativo e orientativo, voltadas ao treinamento esportivo e prática de musculação."
        };

        // ==========================================
        // 2. ESTADO GLOBAL (STATE)
        // ==========================================
        let currentUser = null;
        let units = [];
        let members = [];
        let customExercises = [];
        let unsubscribes = [];

        // Estrutura atual da Ficha que está sendo criada
        let builderState = {
            tabs: [{ id: generateId(), name: "Treino A", exercises: [] }],
            activeTabId: null
        };
        builderState.activeTabId = builderState.tabs[0].id;

        // Utilitários globais expondo no window para os event listeners do HTML
        window.switchView = switchView;
        window.toggleCrefField = toggleCrefField;
        window.calculateBMI = calculateBMI;
        window.toggleTheme = toggleTheme;
        window.updateObjectiveText = updateObjectiveText;
        window.renderHealthOptions = renderHealthOptions;
        window.generateReport = generateReport;
        window.saveAndPrint = saveAndPrint;
        window.clearBuilder = initBuilderState;
        
        // Builder Actions
        window.addTab = addTab;
        window.setActiveTab = setActiveTab;
        window.renameTab = renameTab;
        window.removeTab = removeTab;
        window.addExerciseRow = addExerciseRow;
        window.removeExerciseRow = removeExerciseRow;
        window.updateExerciseRow = updateExerciseRow;
        window.populateExerciseDropdown = populateExerciseDropdown;
        window.moveRowUp = moveRowUp;
        window.moveRowDown = moveRowDown;
        window.deleteCustomExercise = deleteCustomExercise;

        // ==========================================
        // 3. UI HELPERS
        // ==========================================
        function showToast(message, type = 'info') {
            const container = document.getElementById('toast-container');
            const toast = document.createElement('div');
            toast.className = `toast ${type}`;
            toast.innerText = message;
            container.appendChild(toast);
            
            setTimeout(() => toast.classList.add('show'), 10);
            setTimeout(() => {
                toast.classList.remove('show');
                setTimeout(() => toast.remove(), 300);
            }, 3000);
        }

        function switchView(viewName) {
            document.querySelectorAll('main > div').forEach(el => el.classList.add('hidden'));
            document.getElementById(`view-${viewName}`).classList.remove('hidden');
            
            document.querySelectorAll('.nav-btn').forEach(btn => {
                if(btn.dataset.target === viewName) {
                    btn.classList.add('text-primary'); btn.classList.remove('text-muted');
                } else {
                    btn.classList.remove('text-primary'); btn.classList.add('text-muted');
                }
            });
            
            if(viewName === 'reports') populateReportsDropdowns();
        }

        function generateId() {
            return Math.random().toString(36).substr(2, 9);
        }

        function toggleTheme() {
            const gender = document.getElementById('client-gender').value;
            if (gender === 'F') document.body.classList.add('theme-female');
            else document.body.classList.remove('theme-female');
        }

        function updateObjectiveText() {
            const obj = document.getElementById('client-objective').value;
            document.getElementById('objective-help').innerText = OBJECTIVES_INFO[obj] || '';
        }

        function calculateBMI() {
            const w = parseFloat(document.getElementById('client-weight').value);
            const h = parseFloat(document.getElementById('client-height').value);
            const valSpan = document.getElementById('bmi-value');
            const statSpan = document.getElementById('bmi-status');

            if (w > 0 && h > 0) {
                const imc = w / ((h/100) * (h/100));
                valSpan.innerText = imc.toFixed(1);
                
                let text = "", colorClass = "";
                if (imc < 18.5) { text = "Abaixo do peso"; colorClass = "bg-yellow-500/20 text-yellow-500"; }
                else if (imc < 25) { text = "Peso normal"; colorClass = "bg-green-500/20 text-green-500"; }
                else if (imc < 30) { text = "Sobrepeso"; colorClass = "bg-orange-500/20 text-orange-500"; }
                else if (imc < 35) { text = "Obesidade Grau I"; colorClass = "bg-red-500/20 text-red-500"; }
                else if (imc < 40) { text = "Obesidade Grau II"; colorClass = "bg-red-600/20 text-red-600"; }
                else { text = "Obesidade Grau III"; colorClass = "bg-red-700/20 text-red-700"; }
                
                statSpan.innerText = text;
                statSpan.className = `text-sm font-medium px-3 py-1 rounded-full ${colorClass}`;
            } else {
                valSpan.innerText = "--";
                statSpan.innerText = "Aguardando dados";
                statSpan.className = "text-sm font-medium px-3 py-1 rounded-full bg-gray-500/20 text-gray-400";
            }
        }

        // ==========================================
        // 4. AUTHENTICATION & FIREBASE INIT
        // ==========================================
        document.getElementById('btn-login').addEventListener('click', () => {
            const provider = new GoogleAuthProvider();
            signInWithPopup(auth, provider).catch(error => showToast("Erro no login: " + error.message, 'error'));
        });

        document.getElementById('btn-logout').addEventListener('click', () => {
            signOut(auth);
        });

        onAuthStateChanged(auth, (user) => {
            if (user) {
                currentUser = user;
                document.getElementById('login-screen').classList.add('hidden');
                document.getElementById('dashboard-screen').classList.remove('hidden');
                document.getElementById('dashboard-screen').classList.add('flex');
                document.getElementById('user-name-display').innerText = user.displayName;
                
                loadFirebaseData();
                switchView('builder');
                initBuilderState();
            } else {
                currentUser = null;
                unsubscribes.forEach(unsub => unsub());
                unsubscribes = [];
                document.getElementById('login-screen').classList.remove('hidden');
                document.getElementById('dashboard-screen').classList.add('hidden');
                document.getElementById('dashboard-screen').classList.remove('flex');
            }
        });

        function getBasePath(colName) {
            // Using strict artifacts paths as required by the system prompt to avoid permission errors
            return collection(db, 'artifacts', APP_ID, 'users', currentUser.uid, colName);
        }

        function loadFirebaseData() {
            // 1. Unidades
            unsubscribes.push(onSnapshot(getBasePath('units'), (snapshot) => {
                units = snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
                renderUnitsList();
                updateUnitSelects();
            }, (error) => showToast("Erro ao carregar unidades", "error")));

            // 2. Membros
            unsubscribes.push(onSnapshot(getBasePath('members'), (snapshot) => {
                members = snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
                renderMembersList();
                updateMemberSelects();
            }, (error) => showToast("Erro ao carregar membros", "error")));

            // 3. Exercícios Customizados
            unsubscribes.push(onSnapshot(getBasePath('custom_exercises'), (snapshot) => {
                customExercises = snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
                renderCustomExercises();
                // If we are in the builder, a re-render of dropdowns might be needed, 
                // but usually user does this before building. We keep it simple.
            }, (error) => showToast("Erro ao carregar exercícios customizados", "error")));
        }

        // ==========================================
        // 5. GESTÃO DE REDE (UNIDADES E MEMBROS)
        // ==========================================
        document.getElementById('form-unit').addEventListener('submit', async (e) => {
            e.preventDefault();
            const name = document.getElementById('unit-name').value.trim();
            if(!name) return;
            try {
                await addDoc(getBasePath('units'), { name, createdAt: serverTimestamp() });
                document.getElementById('unit-name').value = '';
                showToast("Unidade adicionada!", "success");
            } catch (e) { showToast("Erro ao salvar", "error"); }
        });

        function renderUnitsList() {
            const list = document.getElementById('units-list');
            list.innerHTML = units.length ? '' : '<p class="text-sm text-muted">Nenhuma unidade cadastrada.</p>';
            units.forEach(u => {
                list.innerHTML += `<li class="p-2 bg-black/20 rounded flex justify-between items-center text-sm border border-theme">
                    ${u.name} <button onclick="deleteDoc(doc(db, 'artifacts', '${APP_ID}', 'users', '${currentUser.uid}', 'units', '${u.id}'))" class="text-red-500 hover:text-red-400"><i class="fas fa-trash"></i></button>
                </li>`;
            });
        }

        function updateUnitSelects() {
            const selects = [document.getElementById('member-unit'), document.getElementById('report-unit')];
            selects.forEach(select => {
                if(!select) return;
                const isReport = select.id === 'report-unit';
                select.innerHTML = isReport ? '<option value="ALL">Todas as Unidades</option>' : '<option value="">Selecione a Unidade...</option>';
                units.forEach(u => select.innerHTML += `<option value="${u.id}">${u.name}</option>`);
            });
        }

        function toggleCrefField() {
            const role = document.getElementById('member-role').value;
            const cref = document.getElementById('member-cref');
            if(role === 'PEF') {
                cref.classList.remove('hidden'); cref.required = true;
            } else {
                cref.classList.add('hidden'); cref.required = false; cref.value = '';
            }
        }

        document.getElementById('form-member').addEventListener('submit', async (e) => {
            e.preventDefault();
            const data = {
                unitId: document.getElementById('member-unit').value,
                name: document.getElementById('member-name').value.trim(),
                cpf: document.getElementById('member-cpf').value.trim(),
                uf: document.getElementById('member-uf').value.trim().toUpperCase(),
                role: document.getElementById('member-role').value,
                cref: document.getElementById('member-cref').value.trim(),
                createdAt: serverTimestamp()
            };
            try {
                await addDoc(getBasePath('members'), data);
                e.target.reset(); toggleCrefField();
                showToast("Membro cadastrado!", "success");
            } catch (e) { showToast("Erro ao salvar membro", "error"); }
        });

        function renderMembersList() {
            const tbody = document.getElementById('members-table-body');
            tbody.innerHTML = '';
            members.forEach(m => {
                const unitName = units.find(u => u.id === m.unitId)?.name || 'Desconhecida';
                const roleBadge = m.role === 'PEF' ? `<span class="bg-blue-500/20 text-blue-400 px-2 py-1 rounded text-xs">PEF ${m.cref ? '('+m.cref+')' : ''}</span>` : `<span class="bg-green-500/20 text-green-400 px-2 py-1 rounded text-xs">TE</span>`;
                tbody.innerHTML += `
                    <tr class="border-b border-theme hover:bg-black/10">
                        <td class="px-4 py-2 font-medium">${m.name}</td>
                        <td class="px-4 py-2 text-muted">${unitName}</td>
                        <td class="px-4 py-2">${roleBadge}</td>
                        <td class="px-4 py-2">
                            <button onclick="deleteDoc(doc(db, 'artifacts', '${APP_ID}', 'users', '${currentUser.uid}', 'members', '${m.id}'))" class="text-red-500 hover:text-red-400"><i class="fas fa-trash"></i></button>
                        </td>
                    </tr>
                `;
            });
        }

        function updateMemberSelects() {
            const select = document.getElementById('builder-member-select');
            const currentVal = select.value;
            select.innerHTML = '<option value="">Selecione seu perfil...</option>';
            
            // Agrupar por Unidade
            units.forEach(u => {
                const unitMembers = members.filter(m => m.unitId === u.id);
                if(unitMembers.length > 0) {
                    const group = document.createElement('optgroup');
                    group.label = u.name;
                    unitMembers.forEach(m => {
                        const opt = document.createElement('option');
                        opt.value = m.id; opt.innerText = `${m.name} (${m.role})`;
                        group.appendChild(opt);
                    });
                    select.appendChild(group);
                }
            });
            if (currentVal && members.find(m => m.id === currentVal)) select.value = currentVal;
            renderHealthOptions(); // Update options if category changed
        }

        // ==========================================
        // 6. EXERCÍCIOS CUSTOMIZADOS
        // ==========================================
        document.getElementById('form-custom-exercise').addEventListener('submit', async (e) => {
            e.preventDefault();
            const muscle = document.getElementById('custom-ex-muscle').value;
            const name = document.getElementById('custom-ex-name').value.trim();
            if(!muscle || !name) return;

            try {
                await addDoc(getBasePath('custom_exercises'), { muscle, name, createdAt: serverTimestamp() });
                document.getElementById('custom-ex-name').value = '';
                showToast("Exercício adicionado!", "success");
            } catch (e) { showToast("Erro", "error"); }
        });

        function renderCustomExercises() {
            const container = document.getElementById('custom-exercises-list');
            container.innerHTML = customExercises.length ? '' : '<p class="text-xs text-muted">Nenhum adicionado.</p>';
            customExercises.forEach(ex => {
                container.innerHTML += `
                    <span class="inline-flex items-center gap-2 px-3 py-1 rounded-full bg-primary/20 text-primary text-xs border border-primary/30">
                        ${ex.name} <button onclick="deleteCustomExercise('${ex.id}')" class="hover:text-white"><i class="fas fa-times"></i></button>
                    </span>
                `;
            });
        }

        async function deleteCustomExercise(id) {
            if(confirm("Remover este exercício do seu banco de dados?")) {
                await deleteDoc(doc(db, 'artifacts', APP_ID, 'users', currentUser.uid, 'custom_exercises', id));
            }
        }

        function getFullExerciseList(muscleGroup) {
            const base = DB_EXERCISES[muscleGroup] || [];
            const custom = customExercises.filter(ex => ex.muscle === muscleGroup).map(ex => ex.name);
            return [...base, ...custom].sort();
        }

        // ==========================================
        // 7. BUILDER (PRANCHETA DIGITAL)
        // ==========================================
        function initBuilderState() {
            builderState = { tabs: [{ id: generateId(), name: "Treino A", exercises: [] }] };
            builderState.activeTabId = builderState.tabs[0].id;
            
            // Clear inputs
            document.getElementById('client-name').value = '';
            document.getElementById('client-age').value = '';
            document.getElementById('client-weight').value = '';
            document.getElementById('client-height').value = '';
            document.getElementById('general-notes').value = '';
            document.querySelectorAll('#health-options-container input[type="checkbox"]').forEach(cb => cb.checked = false);
            calculateBMI();
            renderBuilder();
        }

        function renderHealthOptions() {
            const container = document.getElementById('health-options-container');
            const memberId = document.getElementById('builder-member-select').value;
            const member = members.find(m => m.id === memberId);
            
            if(!member) {
                container.innerHTML = '<p class="text-center text-muted mt-10">Selecione o Profissional Responsável acima primeiro.</p>';
                return;
            }

            // Guardar seleções atuais para não perder ao trocar de aba/renderizar
            const selected = Array.from(container.querySelectorAll('input:checked')).map(cb => cb.value);

            let html = '';
            HEALTH_STATUS.forEach(status => {
                const isChecked = selected.includes(status.id) ? 'checked' : '';
                html += `
                    <label class="flex items-start gap-3 p-2 hover:bg-black/10 rounded cursor-pointer transition border border-transparent hover:border-theme">
                        <input type="checkbox" value="${status.id}" class="mt-1 rounded text-primary focus:ring-primary bg-input-theme border-theme" ${isChecked}>
                        <div>
                            <span class="block font-medium">${status.label}</span>
                            <span class="block text-xs text-muted leading-tight">${HEALTH_INFO[member.role][status.id]}</span>
                        </div>
                    </label>
                `;
            });
            container.innerHTML = html;
        }

        function renderBuilder() {
            renderTabs();
            renderActiveTabContent();
        }

        function renderTabs() {
            const container = document.getElementById('workout-tabs-container');
            let html = '';
            builderState.tabs.forEach((tab, index) => {
                const isActive = tab.id === builderState.activeTabId;
                const activeClass = isActive ? 'bg-surface-color text-primary border-t-2 border-t-primary font-bold shadow-sm' : 'text-muted hover:bg-black/20';
                
                html += `
                    <div class="flex items-center group relative cursor-pointer px-4 py-2 rounded-t-lg transition ${activeClass}" style="${isActive ? 'background-color: var(--surface-color)' : ''}">
                        <span onclick="setActiveTab('${tab.id}')" class="mr-3 whitespace-nowrap">${tab.name}</span>
                        ${isActive ? `<button onclick="renameTab('${tab.id}')" class="text-xs text-muted hover:text-primary mr-2"><i class="fas fa-edit"></i></button>` : ''}
                        ${builderState.tabs.length > 1 ? `<button onclick="removeTab('${tab.id}')" class="text-xs text-red-400 hover:text-red-500 opacity-0 group-hover:opacity-100 transition"><i class="fas fa-times"></i></button>` : ''}
                    </div>
                `;
            });
            html += `<button onclick="addTab()" class="px-4 py-2 text-muted hover:text-main rounded-t-lg flex items-center gap-1 transition"><i class="fas fa-plus"></i> Novo</button>`;
            container.innerHTML = html;
        }

        function addTab() {
            const newId = generateId();
            const defaultName = `Treino ${String.fromCharCode(65 + builderState.tabs.length)}`; // Treino B, C, etc.
            builderState.tabs.push({ id: newId, name: defaultName, exercises: [] });
            builderState.activeTabId = newId;
            renderBuilder();
        }

        function setActiveTab(id) { builderState.activeTabId = id; renderBuilder(); }
        
        function renameTab(id) {
            const tab = builderState.tabs.find(t => t.id === id);
            const newName = prompt("Novo nome para a aba:", tab.name);
            if(newName && newName.trim()) { tab.name = newName.trim(); renderTabs(); }
        }

        function removeTab(id) {
            if(builderState.tabs.length <= 1) return;
            if(confirm("Excluir esta aba de treino?")) {
                builderState.tabs = builderState.tabs.filter(t => t.id !== id);
                if(builderState.activeTabId === id) builderState.activeTabId = builderState.tabs[0].id;
                renderBuilder();
            }
        }

        function renderActiveTabContent() {
            const container = document.getElementById('workout-content-area');
            const tab = builderState.tabs.find(t => t.id === builderState.activeTabId);
            if(!tab) return;

            let html = `
                <div class="overflow-x-auto pb-4">
                    <table class="w-full text-sm text-left border-collapse min-w-[800px]">
                        <thead class="text-xs text-muted uppercase bg-black/10 border-b border-theme">
                            <tr>
                                <th class="p-3 w-10 text-center">#</th>
                                <th class="p-3 w-48">Grupo / Exercício</th>
                                <th class="p-3 w-20 text-center">Séries</th>
                                <th class="p-3 w-24 text-center">Reps</th>
                                <th class="p-3 w-32 text-center">Técnica</th>
                                <th class="p-3">Observações / Descanso</th>
                                <th class="p-3 w-16 text-center">Ações</th>
                            </tr>
                        </thead>
                        <tbody class="divide-y divide-gray-700/50">
            `;

            if(tab.exercises.length === 0) {
                html += `<tr><td colspan="7" class="p-8 text-center text-muted italic">Nenhum exercício adicionado. Clique no botão abaixo para iniciar.</td></tr>`;
            } else {
                tab.exercises.forEach((ex, idx) => {
                    html += `
                        <tr class="hover:bg-black/5 transition group">
                            <td class="p-2 text-center text-muted font-mono">${idx + 1}</td>
                            <td class="p-2">
                                <select class="input-theme w-full text-xs p-1.5 rounded mb-1 border-theme" onchange="updateExerciseRow('${tab.id}', '${ex.id}', 'muscle', this.value); populateExerciseDropdown('${tab.id}', '${ex.id}')" id="muscle-${ex.id}">
                                    <option value="">Grupo...</option>
                                    <option value="PEITO" ${ex.muscle==='PEITO'?'selected':''}>Peito</option>
                                    <option value="COSTAS" ${ex.muscle==='COSTAS'?'selected':''}>Costas</option>
                                    <option value="PERNAS" ${ex.muscle==='PERNAS'?'selected':''}>Pernas</option>
                                    <option value="BRACOS_BICEPS" ${ex.muscle==='BRACOS_BICEPS'?'selected':''}>Bíceps</option>
                                    <option value="BRACOS_TRICEPS" ${ex.muscle==='BRACOS_TRICEPS'?'selected':''}>Tríceps</option>
                                    <option value="OMBROS" ${ex.muscle==='OMBROS'?'selected':''}>Ombros</option>
                                    <option value="ABDOMEN" ${ex.muscle==='ABDOMEN'?'selected':''}>Abdômen</option>
                                    <option value="CARDIO" ${ex.muscle==='CARDIO'?'selected':''}>Cardio</option>
                                </select>
                                <select class="input-theme w-full text-xs p-1.5 rounded border-theme font-medium" onchange="updateExerciseRow('${tab.id}', '${ex.id}', 'name', this.value)" id="name-${ex.id}">
                                    <!-- Injected by JS -->
                                </select>
                            </td>
                            <td class="p-2 text-center"><input type="text" class="input-theme w-full text-center text-sm p-1.5 rounded border-theme" value="${ex.sets}" onchange="updateExerciseRow('${tab.id}', '${ex.id}', 'sets', this.value)"></td>
                            <td class="p-2 text-center"><input type="text" class="input-theme w-full text-center text-sm p-1.5 rounded border-theme" value="${ex.reps}" onchange="updateExerciseRow('${tab.id}', '${ex.id}', 'reps', this.value)"></td>
                            <td class="p-2 text-center">
                                <select class="input-theme w-full text-xs p-1.5 rounded border-theme" onchange="updateExerciseRow('${tab.id}', '${ex.id}', 'technique', this.value)">
                                    ${TECHNIQUES.map(t => `<option value="${t}" ${ex.technique === t ? 'selected' : ''}>${t}</option>`).join('')}
                                </select>
                            </td>
                            <td class="p-2"><input type="text" class="input-theme w-full text-xs p-1.5 rounded border-theme" value="${ex.notes}" placeholder="Ex: 60s descanso" onchange="updateExerciseRow('${tab.id}', '${ex.id}', 'notes', this.value)"></td>
                            <td class="p-2 text-center flex flex-col items-center justify-center gap-1 opacity-20 group-hover:opacity-100 transition">
                                <div class="flex gap-1">
                                    <button onclick="moveRowUp('${tab.id}', ${idx})" class="text-xs text-muted hover:text-white bg-black/30 p-1 rounded"><i class="fas fa-chevron-up"></i></button>
                                    <button onclick="moveRowDown('${tab.id}', ${idx})" class="text-xs text-muted hover:text-white bg-black/30 p-1 rounded"><i class="fas fa-chevron-down"></i></button>
                                </div>
                                <button onclick="removeExerciseRow('${tab.id}', '${ex.id}')" class="text-xs text-red-500 hover:text-white hover:bg-red-500 bg-red-500/10 p-1 rounded w-full"><i class="fas fa-trash"></i></button>
                            </td>
                        </tr>
                    `;
                });
            }

            html += `
                        </tbody>
                    </table>
                </div>
                <div class="mt-4 flex justify-center">
                    <button onclick="addExerciseRow('${tab.id}')" class="border border-dashed border-theme text-primary hover:bg-primary/10 px-6 py-2 rounded-lg text-sm font-medium transition flex items-center gap-2">
                        <i class="fas fa-plus"></i> Adicionar Exercício
                    </button>
                </div>
            `;
            container.innerHTML = html;

            // Populate the dropdowns for existing rows
            tab.exercises.forEach(ex => {
                if(ex.muscle) populateExerciseDropdown(tab.id, ex.id, ex.name);
            });
        }

        function addExerciseRow(tabId) {
            const tab = builderState.tabs.find(t => t.id === tabId);
            tab.exercises.push({ id: generateId(), muscle: '', name: '', sets: '3', reps: '10-12', technique: 'Nenhuma', notes: '' });
            renderActiveTabContent();
        }

        function updateExerciseRow(tabId, exId, field, value) {
            const tab = builderState.tabs.find(t => t.id === tabId);
            const ex = tab.exercises.find(e => e.id === exId);
            if(ex) ex[field] = value;
        }

        function removeExerciseRow(tabId, exId) {
            const tab = builderState.tabs.find(t => t.id === tabId);
            tab.exercises = tab.exercises.filter(e => e.id !== exId);
            renderActiveTabContent();
        }

        function moveRowUp(tabId, idx) {
            if(idx === 0) return;
            const tab = builderState.tabs.find(t => t.id === tabId);
            const temp = tab.exercises[idx];
            tab.exercises[idx] = tab.exercises[idx-1];
            tab.exercises[idx-1] = temp;
            renderActiveTabContent();
        }

        function moveRowDown(tabId, idx) {
            const tab = builderState.tabs.find(t => t.id === tabId);
            if(idx === tab.exercises.length - 1) return;
            const temp = tab.exercises[idx];
            tab.exercises[idx] = tab.exercises[idx+1];
            tab.exercises[idx+1] = temp;
            renderActiveTabContent();
        }

        function populateExerciseDropdown(tabId, exId, selectedVal = '') {
            const tab = builderState.tabs.find(t => t.id === tabId);
            const ex = tab.exercises.find(e => e.id === exId);
            const selectMuscle = document.getElementById(`muscle-${exId}`);
            const selectName = document.getElementById(`name-${exId}`);
            if(!selectMuscle || !selectName) return;

            const muscle = selectMuscle.value;
            selectName.innerHTML = '<option value="">Selecione...</option>';
            if(muscle) {
                const list = getFullExerciseList(muscle);
                list.forEach(name => {
                    const isSelected = (name === selectedVal || name === ex.name) ? 'selected' : '';
                    selectName.innerHTML += `<option value="${name}" ${isSelected}>${name}</option>`;
                });
            }
            // Auto update state
            if(!selectedVal && selectName.options.length > 1) {
                ex.name = selectName.options[1].value; // Auto select first valid
                selectName.value = ex.name;
            }
        }

        // ==========================================
        // 8. IMPRESSÃO E SALVAMENTO
        // ==========================================
        async function saveAndPrint() {
            const memberId = document.getElementById('builder-member-select').value;
            if(!memberId) { showToast("Selecione o Profissional Responsável!", "error"); return; }
            
            const member = members.find(m => m.id === memberId);
            const unit = units.find(u => u.id === member.unitId);

            // Coletar Dados do Aluno
            const clientData = {
                name: document.getElementById('client-name').value || "Não informado",
                age: document.getElementById('client-age').value || "--",
                gender: document.getElementById('client-gender').value,
                weight: document.getElementById('client-weight').value || "--",
                height: document.getElementById('client-height').value || "--",
                level: document.getElementById('client-level').value,
                freq: document.getElementById('client-freq').value,
                validity: document.getElementById('client-validity').value,
                objective: document.getElementById('client-objective').value
            };

            const selectedHealthIds = Array.from(document.querySelectorAll('#health-options-container input:checked')).map(cb => cb.value);
            const healthItems = selectedHealthIds.map(id => ({
                label: HEALTH_STATUS.find(s => s.id === id).label,
                text: HEALTH_INFO[member.role][id]
            }));

            const payload = {
                memberId: member.id,
                unitId: unit ? unit.id : null,
                createdAt: serverTimestamp(),
                client: clientData,
                health: healthItems,
                notes: document.getElementById('general-notes').value,
                tabs: builderState.tabs,
                month: new Date().toISOString().slice(0, 7) // YYYY-MM
            };

            // Save to Firestore
            try {
                await addDoc(getBasePath('workouts'), payload);
                showToast("Ficha salva no histórico!", "success");
            } catch (error) {
                console.error(error);
                showToast("Erro ao salvar histórico, imprimindo mesmo assim...", "error");
            }

            // Generate Print Layout
            generatePrintLayout(member, unit, clientData, healthItems, payload.notes, payload.tabs);
            
            // Wait for DOM update then print
            setTimeout(() => { window.print(); }, 500);
        }

        function generatePrintLayout(member, unit, client, healthItems, notes, tabs) {
            const printArea = document.getElementById('print-content');
            const today = new Date();
            const dateStr = today.toLocaleDateString('pt-BR');
            const validUntil = new Date(today);
            validUntil.setDate(validUntil.getDate() + parseInt(client.validity));
            const validStr = validUntil.toLocaleDateString('pt-BR');

            const themeColor = client.gender === 'F' ? '#db2777' : '#2563eb';
            
            let html = `
                <div style="border-bottom: 2px solid ${themeColor}; padding-bottom: 10px; margin-bottom: 20px; display: flex; justify-content: space-between; align-items: flex-end;">
                    <div>
                        <h1 style="margin:0; font-size: 24px; color: ${themeColor}; font-weight: bold;">FICHA DE TREINAMENTO</h1>
                        <p style="margin: 5px 0 0 0; font-size: 14px; font-weight: bold;">Unidade: ${unit ? unit.name : 'Matriz'}</p>
                    </div>
                    <div style="text-align: right; font-size: 12px; line-height: 1.4;">
                        <p style="margin:0"><strong>Profissional:</strong> ${member.name}</p>
                        <p style="margin:0"><strong>Categoria:</strong> ${member.role === 'PEF' ? 'Profissional de Educação Física' : 'Treinador Esportivo'}</p>
                        ${member.cref ? `<p style="margin:0"><strong>CREF:</strong> ${member.cref}</p>` : ''}
                        <p style="margin:0"><strong>Data:</strong> ${dateStr}</p>
                    </div>
                </div>

                <div style="background-color: #f8f9fa; border: 1px solid #ddd; padding: 15px; border-radius: 8px; margin-bottom: 20px; font-size: 13px;">
                    <h3 style="margin-top:0; font-size: 14px; border-bottom: 1px solid #ddd; padding-bottom: 5px; color: ${themeColor}">DADOS DO ALUNO</h3>
                    <div style="display: flex; flex-wrap: wrap; gap: 15px;">
                        <div style="flex: 1 1 200px;"><strong>Nome:</strong> ${client.name}</div>
                        <div style="flex: 1 1 100px;"><strong>Idade:</strong> ${client.age} anos</div>
                        <div style="flex: 1 1 100px;"><strong>Peso/Altura:</strong> ${client.weight}kg / ${client.height}cm</div>
                        <div style="flex: 1 1 150px;"><strong>Nível:</strong> ${client.level}</div>
                        <div style="flex: 1 1 150px;"><strong>Frequência:</strong> ${client.freq}</div>
                    </div>
                    <div style="margin-top: 10px;">
                        <strong>Objetivo Principal:</strong> ${client.objective} - <span style="font-style:italic">${OBJECTIVES_INFO[client.objective]}</span>
                    </div>
                    <div style="margin-top: 10px; color: #b91c1c; font-weight: bold;">
                        Válido até: ${validStr} (${client.validity} dias)
                    </div>
                </div>
            `;

            if(healthItems.length > 0) {
                html += `
                <div style="margin-bottom: 20px; font-size: 12px; border: 1px solid #ddd; border-left: 4px solid #f59e0b; padding: 10px; border-radius: 4px;">
                    <strong style="display:block; margin-bottom: 5px;">ESTADO DE SAÚDE E PRESCRIÇÃO:</strong>
                    <ul style="margin: 0; padding-left: 20px;">
                        ${healthItems.map(h => `<li style="margin-bottom: 4px;"><strong>${h.label}:</strong> ${h.text}</li>`).join('')}
                    </ul>
                </div>`;
            }

            // Workouts
            tabs.forEach(tab => {
                if(tab.exercises.length === 0) return;
                html += `
                <div class="avoid-break" style="margin-bottom: 25px;">
                    <h3 style="background-color: ${themeColor}; color: white; padding: 8px 12px; margin: 0 0 10px 0; font-size: 16px; border-radius: 4px;">${tab.name}</h3>
                    <table style="width: 100%; font-size: 12px;">
                        <thead>
                            <tr>
                                <th style="width: 5%; text-align: center;">#</th>
                                <th style="width: 40%;">Exercício</th>
                                <th style="width: 10%; text-align: center;">Séries</th>
                                <th style="width: 10%; text-align: center;">Reps</th>
                                <th style="width: 15%; text-align: center;">Técnica</th>
                                <th style="width: 20%;">Obs / Descanso</th>
                            </tr>
                        </thead>
                        <tbody>
                            ${tab.exercises.map((ex, idx) => `
                                <tr>
                                    <td style="text-align: center;">${idx + 1}</td>
                                    <td style="font-weight: bold;">${ex.name}</td>
                                    <td style="text-align: center;">${ex.sets}</td>
                                    <td style="text-align: center;">${ex.reps}</td>
                                    <td style="text-align: center;">${ex.technique !== 'Nenhuma' ? ex.technique : '-'}</td>
                                    <td>${ex.notes || '-'}</td>
                                </tr>
                            `).join('')}
                        </tbody>
                    </table>
                </div>`;
            });

            if(notes.trim()) {
                html += `
                <div class="avoid-break" style="margin-top: 20px; font-size: 12px; border: 1px dashed #ccc; padding: 10px;">
                    <strong>OBSERVAÇÕES GERAIS DO PROFISSIONAL:</strong><br>
                    <div style="white-space: pre-wrap; margin-top: 5px;">${notes}</div>
                </div>`;
            }

            // Legal Footer
            html += `
                <div class="legal-footer avoid-break">
                    ${LEGAL_TEXTS[member.role]}
                </div>
            `;

            printArea.innerHTML = html;
        }

        // ==========================================
        // 9. RELATÓRIOS
        // ==========================================
        function populateReportsDropdowns() {
            const monthSelect = document.getElementById('report-month');
            monthSelect.innerHTML = '';
            // Generate last 6 months
            for(let i=0; i<6; i++) {
                const d = new Date();
                d.setMonth(d.getMonth() - i);
                const val = d.toISOString().slice(0, 7); // YYYY-MM
                const label = d.toLocaleDateString('pt-BR', { month: 'long', year: 'numeric' });
                monthSelect.innerHTML += `<option value="${val}">${label.charAt(0).toUpperCase() + label.slice(1)}</option>`;
            }
        }

        async function generateReport() {
            const month = document.getElementById('report-month').value;
            const unitId = document.getElementById('report-unit').value;
            
            try {
                // Fetch workouts for the user
                const q = query(getBasePath('workouts'), where("month", "==", month));
                const snapshot = await getDocs(q);
                let allWorkouts = snapshot.docs.map(d => d.data());

                if (unitId !== 'ALL') {
                    allWorkouts = allWorkouts.filter(w => w.unitId === unitId);
                }

                // Aggregate by Member
                const counts = {};
                allWorkouts.forEach(w => {
                    counts[w.memberId] = (counts[w.memberId] || 0) + 1;
                });

                const tbody = document.getElementById('report-table-body');
                tbody.innerHTML = '';
                let total = 0;

                Object.keys(counts).forEach(memberId => {
                    const member = members.find(m => m.id === memberId) || { name: 'Deletado/Desconhecido', unitId: 'null' };
                    const unitName = units.find(u => u.id === member.unitId)?.name || '-';
                    const c = counts[memberId];
                    total += c;

                    tbody.innerHTML += `
                        <tr class="border-b border-theme hover:bg-black/5">
                            <td class="px-4 py-3 font-medium">${member.name}</td>
                            <td class="px-4 py-3">${unitName}</td>
                            <td class="px-4 py-3 text-center font-bold text-primary">${c}</td>
                        </tr>
                    `;
                });

                if(Object.keys(counts).length === 0) {
                    tbody.innerHTML = `<tr><td colspan="3" class="px-4 py-4 text-center text-muted">Nenhuma ficha criada no período.</td></tr>`;
                }

                document.getElementById('report-total').innerText = total;
                document.getElementById('report-results').classList.remove('hidden');

            } catch (error) {
                console.error(error);
                showToast("Erro ao gerar relatório", "error");
            }
        }

    </script>
</body>
</html>
