<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro - Plataforma Profissional</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* CSS Variables for Dynamic Theming */
        :root {
            --bg-color: #111827; /* Gray 900 */
            --panel-bg: #1f2937; /* Gray 800 */
            --text-main: #f9fafb; /* Gray 50 */
            --text-muted: #9ca3af; /* Gray 400 */
            --accent: #3b82f6; /* Blue 500 */
            --accent-hover: #2563eb; /* Blue 600 */
            --border-color: #374151; /* Gray 700 */
            --input-bg: #374151;
        }

        /* Female Theme Override */
        body.theme-female {
            --bg-color: #fdf2f8; /* Pink 50 */
            --panel-bg: #ffffff; /* White */
            --text-main: #1f2937; /* Gray 800 */
            --text-muted: #6b7280; /* Gray 500 */
            --accent: #ec4899; /* Pink 500 */
            --accent-hover: #db2777; /* Pink 600 */
            --border-color: #f3f4f6; /* Gray 100 */
            --input-bg: #f9fafb;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-main);
            transition: background-color 0.3s, color 0.3s;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        .panel { background-color: var(--panel-bg); border-color: var(--border-color); }
        .input-field { 
            background-color: var(--input-bg); 
            color: var(--text-main); 
            border-color: var(--border-color); 
        }
        .btn-primary { background-color: var(--accent); color: white; }
        .btn-primary:hover { background-color: var(--accent-hover); }
        .text-accent { color: var(--accent); }
        .border-accent { border-color: var(--accent); }

        /* Print Styles */
        @media print {
            body { background: white; color: black; font-size: 12px; }
            #app-ui, #login-ui { display: none !important; }
            #print-ui { display: block !important; }
            .print-page-break { page-break-before: always; }
            .avoid-break { page-break-inside: avoid; }
            table { width: 100%; border-collapse: collapse; margin-bottom: 1rem; }
            th, td { border: 1px solid #ccc; padding: 6px; text-align: left; }
            th { background-color: #f3f4f6; -webkit-print-color-adjust: exact; }
            h1, h2, h3 { color: #111827; margin-bottom: 0.5rem; }
            .header-banner { background-color: #111827; color: white; padding: 10px; text-align: center; -webkit-print-color-adjust: exact; }
            .legal-footer { font-size: 10px; color: #6b7280; margin-top: 20px; border-top: 1px solid #ccc; padding-top: 10px; text-align: justify; }
        }
        
        #print-ui { display: none; }
    </style>
</head>
<body class="min-h-screen">

    <!-- LOGIN UI -->
    <div id="login-ui" class="flex items-center justify-center min-h-screen flex-col p-4">
        <div class="panel p-8 rounded-2xl shadow-2xl max-w-md w-full text-center border">
            <h1 class="text-4xl font-black italic mb-2 tracking-tighter text-accent">POWFIT PRO</h1>
            <p class="text-muted mb-8 text-sm">Plataforma Profissional de Prescrição</p>
            <button id="btn-login" class="btn-primary w-full py-3 rounded-lg font-bold flex items-center justify-center gap-2 transition-all">
                <svg class="w-5 h-5" viewBox="0 0 24 24" fill="currentColor"><path d="M12.545,10.239v3.821h5.445c-0.712,2.315-2.647,3.972-5.445,3.972c-3.332,0-6.033-2.701-6.033-6.032s2.701-6.032,6.033-6.032c1.498,0,2.866,0.549,3.921,1.453l2.814-2.814C17.503,2.988,15.139,2,12.545,2C7.021,2,2.543,6.477,2.543,12s4.478,10,10.002,10c8.396,0,10.249-7.85,9.426-11.761H12.545z"/></svg>
                Entrar com Google
            </button>
            <p id="login-error" class="text-red-500 mt-4 text-sm hidden"></p>
        </div>
    </div>

    <!-- MAIN APP UI -->
    <div id="app-ui" class="hidden flex flex-col md:flex-row min-h-screen">
        <!-- Sidebar -->
        <aside class="panel w-full md:w-64 border-r md:min-h-screen flex flex-col">
            <div class="p-4 border-b border-color">
                <h1 class="text-2xl font-black italic tracking-tighter text-accent">POWFIT PRO</h1>
                <p id="user-email-display" class="text-xs text-muted mt-1 truncate"></p>
            </div>
            <nav class="flex-1 p-4 flex flex-col gap-2">
                <button onclick="switchView('dashboard')" class="text-left px-4 py-2 rounded-lg hover:bg-black/10 dark:hover:bg-white/10 font-medium transition-colors" id="nav-dashboard">🏢 Gestão / Rede</button>
                <button onclick="switchView('builder')" class="text-left px-4 py-2 rounded-lg hover:bg-black/10 dark:hover:bg-white/10 font-medium transition-colors" id="nav-builder">🏋️ Montar Treino</button>
                <button onclick="switchView('reports')" class="text-left px-4 py-2 rounded-lg hover:bg-black/10 dark:hover:bg-white/10 font-medium transition-colors" id="nav-reports">📊 Produtividade</button>
            </nav>
            <div class="p-4 border-t border-color">
                <button id="btn-logout" class="text-sm text-red-500 hover:text-red-400 font-bold w-full text-left">Sair do Sistema</button>
            </div>
        </aside>

        <!-- Main Content -->
        <main class="flex-1 p-4 md:p-8 overflow-y-auto h-screen">
            
            <!-- VIEW: DASHBOARD -->
            <div id="view-dashboard" class="hidden space-y-6">
                <div>
                    <h2 class="text-3xl font-bold mb-1">Gestão da Rede</h2>
                    <p class="text-muted text-sm">Gerencie suas unidades e equipe de treinadores.</p>
                </div>
                
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                    <!-- Unidades -->
                    <div class="panel p-6 rounded-xl border">
                        <h3 class="text-xl font-bold mb-4 flex justify-between items-center">
                            Unidades
                            <button onclick="toggleModal('modal-unit')" class="btn-primary text-sm px-3 py-1 rounded">+ Nova</button>
                        </h3>
                        <div id="units-list" class="space-y-2 max-h-64 overflow-y-auto"></div>
                    </div>

                    <!-- Membros -->
                    <div class="panel p-6 rounded-xl border">
                        <h3 class="text-xl font-bold mb-4 flex justify-between items-center">
                            Equipe (Membros)
                            <button onclick="toggleModal('modal-member')" class="btn-primary text-sm px-3 py-1 rounded">+ Novo</button>
                        </h3>
                        <select id="filter-unit-members" class="input-field w-full p-2 rounded mb-4 border" onchange="renderMembers()">
                            <option value="">Todas as unidades...</option>
                        </select>
                        <div id="members-list" class="space-y-2 max-h-64 overflow-y-auto"></div>
                    </div>
                </div>
            </div>

            <!-- VIEW: RELATORIOS -->
            <div id="view-reports" class="hidden space-y-6">
                <div>
                    <h2 class="text-3xl font-bold mb-1">Produtividade</h2>
                    <p class="text-muted text-sm">Fichas criadas por unidade e treinador.</p>
                </div>
                <div class="panel p-6 rounded-xl border">
                    <div class="flex gap-4 mb-6">
                        <input type="month" id="report-month" class="input-field p-2 rounded border">
                        <select id="report-unit" class="input-field p-2 rounded border">
                            <option value="">Todas as unidades</option>
                        </select>
                        <button onclick="generateReport()" class="btn-primary px-4 py-2 rounded font-bold">Gerar Relatório</button>
                        <button onclick="printReport()" class="bg-gray-600 text-white px-4 py-2 rounded font-bold hover:bg-gray-500">🖨️ Imprimir</button>
                    </div>
                    <div id="report-results" class="space-y-4"></div>
                </div>
            </div>

            <!-- VIEW: BUILDER -->
            <div id="view-builder" class="hidden space-y-6">
                <div class="flex justify-between items-end">
                    <div>
                        <h2 class="text-3xl font-bold mb-1">Nova Ficha de Treino</h2>
                        <p class="text-muted text-sm">Prancheta digital profissional. Você no controle.</p>
                    </div>
                    <button onclick="saveAndPrint()" class="btn-primary px-6 py-3 rounded-lg font-bold shadow-lg flex items-center gap-2 text-lg">
                        🖨️ Salvar e Imprimir
                    </button>
                </div>

                <!-- Responsável -->
                <div class="panel p-6 rounded-xl border border-accent border-l-4">
                    <h3 class="font-bold mb-3">1. Profissional Responsável (Equipe)</h3>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div>
                            <label class="block text-xs font-bold mb-1 text-muted">Selecione sua Unidade</label>
                            <select id="builder-unit" class="input-field w-full p-2 rounded border" onchange="updateBuilderMembers()"></select>
                        </div>
                        <div>
                            <label class="block text-xs font-bold mb-1 text-muted">Selecione seu Perfil</label>
                            <select id="builder-member" class="input-field w-full p-2 rounded border"></select>
                        </div>
                    </div>
                </div>

                <!-- Dados do Aluno -->
                <div class="panel p-6 rounded-xl border">
                    <h3 class="font-bold mb-3">2. Dados do Aluno</h3>
                    <div class="grid grid-cols-1 md:grid-cols-4 gap-4 mb-4">
                        <div class="md:col-span-2">
                            <label class="block text-xs font-bold mb-1 text-muted">Nome do Aluno</label>
                            <input type="text" id="client-name" class="input-field w-full p-2 rounded border" placeholder="Ex: João Silva">
                        </div>
                        <div>
                            <label class="block text-xs font-bold mb-1 text-muted">Gênero</label>
                            <select id="client-gender" class="input-field w-full p-2 rounded border" onchange="toggleTheme()">
                                <option value="Masculino">Masculino</option>
                                <option value="Feminino">Feminino</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-xs font-bold mb-1 text-muted">Idade</label>
                            <input type="number" id="client-age" class="input-field w-full p-2 rounded border">
                        </div>
                        <div>
                            <label class="block text-xs font-bold mb-1 text-muted">Peso (kg)</label>
                            <input type="number" step="0.1" id="client-weight" class="input-field w-full p-2 rounded border" oninput="calculateBMI()">
                        </div>
                        <div>
                            <label class="block text-xs font-bold mb-1 text-muted">Altura (m)</label>
                            <input type="number" step="0.01" id="client-height" class="input-field w-full p-2 rounded border" oninput="calculateBMI()">
                        </div>
                        <div class="md:col-span-2 flex flex-col justify-end">
                            <p class="text-sm font-bold text-accent" id="bmi-result">IMC: --</p>
                        </div>
                    </div>
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                        <div>
                            <label class="block text-xs font-bold mb-1 text-muted">Nível</label>
                            <select id="client-level" class="input-field w-full p-2 rounded border">
                                <option>Iniciante</option><option>Intermediário</option><option>Avançado</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-xs font-bold mb-1 text-muted">Frequência</label>
                            <select id="client-freq" class="input-field w-full p-2 rounded border">
                                <option>3 dias</option><option>5 dias</option><option>6 dias</option><option>Personalizado</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-xs font-bold mb-1 text-muted">Objetivo</label>
                            <select id="client-obj" class="input-field w-full p-2 rounded border">
                                <option>Emagrecimento</option><option>Hipertrofia</option><option>Definição</option>
                                <option>Condicionamento</option><option>Resistência</option><option>Força</option>
                                <option>Reabilitação</option><option>Saúde geral</option>
                            </select>
                        </div>
                    </div>
                </div>

                <!-- Estado de Saúde -->
                <div class="panel p-6 rounded-xl border">
                    <h3 class="font-bold mb-3">3. Estado de Saúde (Múltipla Seleção)</h3>
                    <div class="grid grid-cols-2 md:grid-cols-4 gap-2 text-sm" id="health-status-container">
                        <!-- Genereted via JS -->
                    </div>
                </div>

                <!-- Montagem do Treino -->
                <div class="panel p-6 rounded-xl border">
                    <div class="flex justify-between items-center mb-4">
                        <h3 class="font-bold text-xl">4. Montagem da Ficha</h3>
                        <div class="flex gap-2 items-center">
                            <label class="text-sm font-bold">Validade:</label>
                            <select id="routine-validity" class="input-field p-2 rounded border text-sm">
                                <option value="15">15 dias</option>
                                <option value="30" selected>30 dias</option>
                                <option value="60">60 dias</option>
                                <option value="90">90 dias</option>
                            </select>
                        </div>
                    </div>

                    <!-- Tabs Header -->
                    <div class="flex gap-2 border-b border-color mb-4 overflow-x-auto pb-2" id="tabs-header">
                        <!-- Generated via JS -->
                    </div>
                    
                    <div class="mb-4">
                        <button onclick="addTab()" class="text-sm text-accent hover:underline font-bold">+ Adicionar Aba (Ex: Treino C)</button>
                        <button onclick="removeCurrentTab()" class="text-sm text-red-500 hover:underline font-bold ml-4">🗑️ Remover Aba Atual</button>
                    </div>

                    <!-- Tabs Content -->
                    <div id="tabs-content" class="overflow-x-auto">
                        <!-- Active tab table will be rendered here -->
                    </div>
                    
                    <div class="mt-4 flex gap-4">
                        <button onclick="addExerciseRow()" class="btn-primary px-4 py-2 rounded font-bold">+ Adicionar Exercício</button>
                        <button onclick="toggleModal('modal-custom-ex')" class="bg-gray-600 text-white px-4 py-2 rounded font-bold hover:bg-gray-500 text-sm">Adicionar Exercício Customizado</button>
                    </div>
                </div>

                <!-- Recomendações -->
                <div class="panel p-6 rounded-xl border">
                    <h3 class="font-bold mb-3">5. Recomendações Profissionais / Observações</h3>
                    <textarea id="routine-obs" class="input-field w-full p-3 rounded border h-24" placeholder="Escreva sobre hidratação, cadência, descanso, métodos adicionais..."></textarea>
                </div>
            </div>

        </main>
    </div>

    <!-- MODALS -->
    <!-- Modal: Add Unit -->
    <div id="modal-unit" class="hidden fixed inset-0 bg-black/50 flex items-center justify-center z-50 p-4">
        <div class="panel p-6 rounded-xl border max-w-sm w-full">
            <h3 class="text-xl font-bold mb-4">Nova Unidade</h3>
            <input type="text" id="unit-name" class="input-field w-full p-2 rounded border mb-4" placeholder="Nome da Unidade">
            <div class="flex justify-end gap-2">
                <button onclick="toggleModal('modal-unit')" class="px-4 py-2 rounded text-muted hover:bg-black/10">Cancelar</button>
                <button onclick="saveUnit()" class="btn-primary px-4 py-2 rounded font-bold">Salvar</button>
            </div>
        </div>
    </div>

    <!-- Modal: Add Member -->
    <div id="modal-member" class="hidden fixed inset-0 bg-black/50 flex items-center justify-center z-50 p-4">
        <div class="panel p-6 rounded-xl border max-w-md w-full">
            <h3 class="text-xl font-bold mb-4">Novo Membro da Equipe</h3>
            <div class="space-y-3">
                <input type="text" id="mem-name" class="input-field w-full p-2 rounded border" placeholder="Nome Completo">
                <input type="text" id="mem-cpf" class="input-field w-full p-2 rounded border" placeholder="CPF">
                <select id="mem-unit" class="input-field w-full p-2 rounded border"></select>
                <input type="text" id="mem-uf" class="input-field w-full p-2 rounded border" placeholder="Estado (UF) - Ex: SP">
                <select id="mem-cat" class="input-field w-full p-2 rounded border" onchange="toggleCrefField()">
                    <option value="PEF">Profissional de Educação Física (PEF)</option>
                    <option value="TE">Treinador Esportivo (TE)</option>
                </select>
                <input type="text" id="mem-cref" class="input-field w-full p-2 rounded border" placeholder="CREF (Apenas números/UF)">
            </div>
            <div class="flex justify-end gap-2 mt-4">
                <button onclick="toggleModal('modal-member')" class="px-4 py-2 rounded text-muted hover:bg-black/10">Cancelar</button>
                <button onclick="saveMember()" class="btn-primary px-4 py-2 rounded font-bold">Salvar</button>
            </div>
        </div>
    </div>

    <!-- Modal: Custom Exercise -->
    <div id="modal-custom-ex" class="hidden fixed inset-0 bg-black/50 flex items-center justify-center z-50 p-4">
        <div class="panel p-6 rounded-xl border max-w-sm w-full">
            <h3 class="text-xl font-bold mb-4">Criar Exercício Customizado</h3>
            <div class="space-y-3">
                <label class="block text-xs font-bold text-muted">Grupo Muscular</label>
                <select id="custom-ex-group" class="input-field w-full p-2 rounded border">
                    <option value="PEITO">Peito</option><option value="COSTAS">Costas</option>
                    <option value="PERNAS">Pernas</option><option value="BRAÇOS (BÍCEPS)">Braços (Bíceps)</option>
                    <option value="BRAÇOS (TRÍCEPS)">Braços (Tríceps)</option><option value="OMBROS">Ombros</option>
                    <option value="ABDÔMEN">Abdômen</option><option value="CARDIO">Cardio</option>
                </select>
                <input type="text" id="custom-ex-name" class="input-field w-full p-2 rounded border" placeholder="Nome do Exercício">
            </div>
            <div class="flex justify-end gap-2 mt-4">
                <button onclick="toggleModal('modal-custom-ex')" class="px-4 py-2 rounded text-muted hover:bg-black/10">Cancelar</button>
                <button onclick="saveCustomExercise()" class="btn-primary px-4 py-2 rounded font-bold">Salvar</button>
            </div>
        </div>
    </div>

    <!-- PRINT CONTAINER -->
    <div id="print-ui"></div>

    <!-- SCRIPT (Firebase, Logic, Data) -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInWithPopup, GoogleAuthProvider, signInWithCustomToken, signInAnonymously, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, doc, setDoc, getDoc, addDoc, onSnapshot, deleteDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Firebase Config Setup
        let firebaseConfig;
        try {
            firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : {
                apiKey: "AIzaSyD-iuERHA_x1f69smJXh7b8wTLO1zTadIU",
                authDomain: "powfitpro-d8577.firebaseapp.com",
                projectId: "powfitpro-d8577",
                storageBucket: "powfitpro-d8577.firebasestorage.app",
                messagingSenderId: "651381183985",
                appId: "1:651381183985:web:9a425692372a9d67d9e050"
            };
        } catch(e) {
            console.error("Config erro", e);
        }

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'powfit-default';

        // Globals
        let currentUser = null;
        let unsubscribes = [];
        let state = {
            units: [],
            members: [],
            customExercises: [],
            routines: [], // for reports
            workoutTabs: [{ id: 'A', name: 'Treino A', rows: [] }],
            activeTabId: 'A'
        };

        // --- STATIC DATA ---
        const EXERCISES_DB = {
            "PEITO": ["Supino Reto", "Supino Inclinado", "Supino Inclinado com Halteres", "Supino Fechado com Halteres", "Cross Over", "Cross Over Alto", "Cross Over Baixo", "Crucifixo Reto", "Crucifixo Inclinado com Halteres", "Crucifixo na Máquina", "Peck Fly", "Peck Fly Unilateral", "Pullover", "Flexão de Braço", "Flexão com Pés Elevados", "Flexão Explosiva"],
            "COSTAS": ["Puxada Alta", "Puxada de Frente Supinada", "Pulldown", "Remada Aberta", "Remada Baixa", "Remada Curvada", "Remada Curvada Supinada", "Remada Cavalinho (T-Bar)", "Serrote", "Facepull (puxada de cima para baixo)"],
            "PERNAS": ["Agachamento Livre", "Agachamento Taça", "Agachamento no Smith", "Agachamento com passada lateral", "Squat", "Hack Machine", "Leg 45°", "Leg 90°", "Agachamento Sumô", "Agachamento Sissy (Livre)", "Afundo", "Recuo", "Avanço", "Passada", "Búlgaro", "Step-up", "Levantamento Terra", "Levantamento Terra Romeno", "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", "Cadeira Flexora", "Elevação Pélvica no Banco", "Elevação Pélvica no Chão", "Elevação Pélvica Unilateral no Chão", "Extensão de Quadril (Glúteo Máximo)", "Extensão Cruzada (Glúteo Médio)", "Coice", "Cachorrinho", "Cadeira Extensora", "Adução", "Abdução", "Abdução Inclinada", "Flexão Nórdica", "Flexão Nórdica Invertida", "Panturrilha Livre", "Panturrilha no Leg Press", "Panturrilha Banco", "Panturrilha Squat", "Panturrilha Unilateral"],
            "BRAÇOS (BÍCEPS)": ["Rosca Direta", "Rosca Alternada", "Rosca 21", "Rosca Scott Barra W", "Rosca Scott Unilateral", "Rosca Scott com Halteres", "Rosca Martelo", "Rosca Cross", "Rosca Inversa", "Rosca 45°"],
            "BRAÇOS (TRÍCEPS)": ["Triceps Pulley Unilateral", "Tríceps Pulley Barra", "Tríceps Pulley Corda", "Tríceps Pulley Pegada Inversa", "Tríceps Francês na Corda", "Tríceps Francês com Halter", "Tríceps Francês Unilateral", "Tríceps Cruzado Polia Dupla", "Tríceps Coice Unilateral", "Tríceps Arremesso", "Tríceps Testa", "Mergulho no Banco"],
            "OMBROS": ["Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", "Elevação Lateral Unilateral Cross", "Elevação Lateral Sentado", "Desenvolvimento com Halteres", "Desenvolvimento com Barra", "Arnold Press", "Elevação Borboleta", "Crucifixo Inverso Sentado com Halteres", "Crucifixo Inverso na Polia", "Crucifixo Inverso Unilateral na Polia", "Facepull (puxada reta)", "Remada Alta", "Encolhimento (Trapézio)"],
            "ABDÔMEN": ["Infra com Elevação de Perna", "Abdominal Supra", "Abdominal Remador", "Abdominal Bicicleta", "Abdominal Twister com Peso", "Prancha", "Prancha Lateral", "Trituração de Cabos em Pé", "Isometria na parede"],
            "CARDIO": ["Bicicleta 10 Minutos", "Bicicleta 15 Minutos", "Bicicleta 20 Minutos", "Esteira 10 Minutos", "Esteira 15 Minutos", "Esteira 20 Minutos", "Pular Corda"]
        };

        const TECHNIQUES = ["Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", "Negativa", "Isometria", "Parciais", "Pirâmide"];

        const HEALTH_OPTIONS = ["🟢 Saudável", "⚪ Sedentário", "🟡 Sobrepeso", "🔴 Obesidade", "⚖️ Baixo peso", "🍬 Diabetes", "❤️ Hipertensão", "🔵 Hipotensão", "💔 Problemas cardíacos", "🦴 Problemas articulares", "🫁 Problemas respiratórios", "⚠️ Lesões", "🤰 Gestante", "🤱 Lactante", "👴 Idoso"];

        const TEXTS_OBJ = {
            "Emagrecimento": "Foco em déficit calórico com treinos mistos de força (manter massa magra) e aeróbicos (maior gasto).",
            "Hipertrofia": "Prioridade na progressão de carga e volume adequado. Essencial superávit calórico e descanso.",
            "Definição": "Manutenção de massa muscular enquanto reduz o percentual de gordura. Atenção estrita à dieta.",
            "Condicionamento": "Treinos com menor tempo de intervalo, circuitos e alta integração cardiopulmonar.",
            "Resistência": "Séries mais longas, cadência controlada e aprimoramento da capacidade muscular.",
            "Força": "Cargas altas, baixas repetições e intervalos de descanso maiores.",
            "Reabilitação": "Treino focado em fortalecimento específico, mobilidade e controle motor. Respeitar limites.",
            "Saúde geral": "Equilíbrio entre força, cardio e flexibilidade. O principal objetivo é a constância e bem-estar."
        };

        const TEXTS_HEALTH_PEF = {
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

        const TEXTS_HEALTH_TE = {
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

        const LEGAL_PEF = "⚠️ OBSERVAÇÃO LEGAL – PROFISSIONAL DE EDUCAÇÃO FÍSICA<br>Conforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º da mesma lei estabelece que compete ao profissional de Educação Física coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos especializados nas áreas de atividades físicas e do desporto.";
        const LEGAL_TE = "⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO<br>Conforme a Lei nº 14.597/2023 (Lei Geral do Esporte), Art. 75, a profissão de treinador esportivo é reconhecida e regulada no Brasil, com atuação de caráter técnico e esportivo voltada à preparação, supervisão, orientação e acompanhamento de treinos físicos e esportivos. A atuação possui finalidade orientativa e não substitui avaliação médica, diagnóstico clínico, prescrição medicamentosa ou acompanhamento de profissionais da saúde. Em casos de doenças, lesões, gestação, limitações físicas, uso de medicações ou qualquer condição específica de saúde, recomenda-se avaliação prévia por profissional habilitado antes do início ou continuidade da prática de exercícios. O treinamento proposto respeita os limites individuais, priorizando segurança, execução correta e evolução progressiva, dentro da atuação técnica e esportiva permitida por lei. As recomendações desta ficha possuem caráter informativo e orientativo, voltadas ao treinamento esportivo e prática de musculação.";

        // --- AUTH LOGIC ---
        document.getElementById('btn-login').addEventListener('click', async () => {
            const errEl = document.getElementById('login-error');
            try {
                if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                    await signInWithCustomToken(auth, __initial_auth_token);
                } else {
                    const provider = new GoogleAuthProvider();
                    await signInWithPopup(auth, provider);
                }
            } catch (error) {
                console.error("Login erro", error);
                errEl.innerText = "Erro ao fazer login. Tentando acesso anônimo...";
                errEl.classList.remove('hidden');
                try { await signInAnonymously(auth); } catch(e) {}
            }
        });

        document.getElementById('btn-logout').addEventListener('click', () => signOut(auth));

        onAuthStateChanged(auth, user => {
            currentUser = user;
            if (user) {
                document.getElementById('login-ui').classList.add('hidden');
                document.getElementById('app-ui').classList.remove('hidden');
                document.getElementById('user-email-display').innerText = user.email || 'Usuário Logado';
                initDataListeners();
                switchView('dashboard');
                initHealthCheckboxes();
            } else {
                document.getElementById('login-ui').classList.remove('hidden');
                document.getElementById('app-ui').classList.add('hidden');
                clearListeners();
            }
        });

        // --- DATABASE LOGIC ---
        const getPath = (coll) => collection(db, 'artifacts', appId, 'users', currentUser.uid, coll);

        function initDataListeners() {
            clearListeners();
            
            // Units
            unsubscribes.push(onSnapshot(getPath('units'), snap => {
                state.units = snap.docs.map(d => ({id: d.id, ...d.data()}));
                renderUnits();
                populateUnitSelects();
            }, e => console.error(e)));

            // Members
            unsubscribes.push(onSnapshot(getPath('members'), snap => {
                state.members = snap.docs.map(d => ({id: d.id, ...d.data()}));
                renderMembers();
            }, e => console.error(e)));

            // Custom Exercises
            unsubscribes.push(onSnapshot(getPath('custom_exercises'), snap => {
                state.customExercises = snap.docs.map(d => ({id: d.id, ...d.data()}));
                if(state.activeTabId) renderTable(); // re-render table to show new exercises
            }, e => console.error(e)));

            // Routines (for reports)
            unsubscribes.push(onSnapshot(getPath('routines'), snap => {
                state.routines = snap.docs.map(d => ({id: d.id, ...d.data()}));
            }, e => console.error(e)));
        }

        function clearListeners() {
            unsubscribes.forEach(unsub => unsub());
            unsubscribes = [];
        }

        // --- UI NAVIGATION ---
        window.switchView = function(view) {
            document.getElementById('view-dashboard').classList.add('hidden');
            document.getElementById('view-builder').classList.add('hidden');
            document.getElementById('view-reports').classList.add('hidden');
            
            document.getElementById(`view-${view}`).classList.remove('hidden');
            
            ['dashboard', 'builder', 'reports'].forEach(v => {
                document.getElementById(`nav-${v}`).classList.remove('text-accent', 'bg-black/10', 'dark:bg-white/10');
            });
            document.getElementById(`nav-${view}`).classList.add('text-accent', 'bg-black/10', 'dark:bg-white/10');

            if(view === 'builder') {
                renderTabs();
                renderTable();
            }
        }

        window.toggleModal = function(id) {
            const el = document.getElementById(id);
            if(el.classList.contains('hidden')) el.classList.remove('hidden');
            else el.classList.add('hidden');
        }

        // --- DASHBOARD LOGIC ---
        window.saveUnit = async function() {
            const name = document.getElementById('unit-name').value.trim();
            if(!name) return;
            await addDoc(getPath('units'), { name, createdAt: new Date().toISOString() });
            document.getElementById('unit-name').value = '';
            toggleModal('modal-unit');
        }

        function renderUnits() {
            const c = document.getElementById('units-list');
            c.innerHTML = state.units.map(u => `<div class="p-3 bg-black/5 dark:bg-white/5 rounded flex justify-between"><span>${u.name}</span> <button onclick="deleteDocItem('units', '${u.id}')" class="text-red-500 text-sm">Excluir</button></div>`).join('');
        }

        function populateUnitSelects() {
            const opts = '<option value="">Selecione...</option>' + state.units.map(u => `<option value="${u.id}">${u.name}</option>`).join('');
            document.getElementById('mem-unit').innerHTML = opts;
            document.getElementById('filter-unit-members').innerHTML = '<option value="">Todas as unidades...</option>' + state.units.map(u => `<option value="${u.id}">${u.name}</option>`).join('');
            document.getElementById('builder-unit').innerHTML = opts;
            document.getElementById('report-unit').innerHTML = '<option value="">Todas as unidades</option>' + state.units.map(u => `<option value="${u.id}">${u.name}</option>`).join('');
        }

        window.toggleCrefField = function() {
            const cat = document.getElementById('mem-cat').value;
            document.getElementById('mem-cref').style.display = cat === 'PEF' ? 'block' : 'none';
        }

        window.saveMember = async function() {
            const name = document.getElementById('mem-name').value.trim();
            const cpf = document.getElementById('mem-cpf').value.trim();
            const unitId = document.getElementById('mem-unit').value;
            const uf = document.getElementById('mem-uf').value.trim();
            const cat = document.getElementById('mem-cat').value;
            const cref = document.getElementById('mem-cref').value.trim();

            if(!name || !unitId) return alert('Nome e Unidade são obrigatórios');
            
            await addDoc(getPath('members'), { name, cpf, unitId, uf, category: cat, cref: cat === 'PEF' ? cref : '', createdAt: new Date().toISOString() });
            
            document.getElementById('mem-name').value = '';
            document.getElementById('mem-cpf').value = '';
            document.getElementById('mem-cref').value = '';
            toggleModal('modal-member');
        }

        window.renderMembers = function() {
            const filterId = document.getElementById('filter-unit-members').value;
            const c = document.getElementById('members-list');
            let filtered = state.members;
            if(filterId) filtered = filtered.filter(m => m.unitId === filterId);

            c.innerHTML = filtered.map(m => {
                const unitName = state.units.find(u => u.id === m.unitId)?.name || 'Sem unidade';
                return `<div class="p-3 bg-black/5 dark:bg-white/5 rounded flex justify-between items-center">
                    <div><div class="font-bold">${m.name} <span class="text-xs font-normal bg-accent text-white px-1 rounded ml-1">${m.category}</span></div><div class="text-xs text-muted">${unitName}</div></div>
                    <button onclick="deleteDocItem('members', '${m.id}')" class="text-red-500 text-sm">Excluir</button>
                </div>`;
            }).join('');
        }

        window.deleteDocItem = async function(collectionName, id) {
            await deleteDoc(doc(db, 'artifacts', appId, 'users', currentUser.uid, collectionName, id));
        }

        // --- REPORTS LOGIC ---
        window.generateReport = function() {
            const monthStr = document.getElementById('report-month').value; // YYYY-MM
            const unitFilter = document.getElementById('report-unit').value;
            
            let filteredRoutines = state.routines;
            if (monthStr) {
                filteredRoutines = filteredRoutines.filter(r => r.createdAt && r.createdAt.startsWith(monthStr));
            }
            if (unitFilter) {
                filteredRoutines = filteredRoutines.filter(r => {
                    const m = state.members.find(mem => mem.id === r.memberId);
                    return m && m.unitId === unitFilter;
                });
            }

            // Group by Member
            let reportData = {};
            filteredRoutines.forEach(r => {
                const m = state.members.find(mem => mem.id === r.memberId);
                const memberName = m ? m.name : 'Desconhecido';
                const unitName = m ? (state.units.find(u => u.id === m.unitId)?.name || 'Desconhecida') : 'Desconhecida';
                
                const key = `${unitName} - ${memberName}`;
                if(!reportData[key]) reportData[key] = 0;
                reportData[key]++;
            });

            const resDiv = document.getElementById('report-results');
            resDiv.innerHTML = `<h4 class="font-bold">Total Geral: ${filteredRoutines.length} fichas</h4>`;
            
            let html = '<table class="w-full text-left mt-2"><thead><tr><th>Unidade - Membro</th><th>Fichas Criadas</th></tr></thead><tbody>';
            for(const [key, count] of Object.entries(reportData)) {
                html += `<tr><td class="p-2 border-b border-color">${key}</td><td class="p-2 border-b border-color font-bold text-accent">${count}</td></tr>`;
            }
            html += '</tbody></table>';
            resDiv.innerHTML += html;
        }

        window.printReport = function() {
            const reportContent = document.getElementById('report-results').innerHTML;
            const printUi = document.getElementById('print-ui');
            const month = document.getElementById('report-month').value || 'Todo o período';
            
            printUi.innerHTML = `
                <div class="header-banner">
                    <h1 style="color:white; margin:0">POWFIT PRO - RELATÓRIO DE PRODUTIVIDADE</h1>
                    <p style="margin:5px 0 0 0">Período: ${month}</p>
                </div>
                <div style="padding: 20px;">${reportContent}</div>
            `;
            window.print();
        }

        // --- BUILDER LOGIC ---
        window.updateBuilderMembers = function() {
            const unitId = document.getElementById('builder-unit').value;
            const mSelect = document.getElementById('builder-member');
            mSelect.innerHTML = '<option value="">Selecione você...</option>' + 
                state.members.filter(m => m.unitId === unitId).map(m => `<option value="${m.id}">${m.name} (${m.category})</option>`).join('');
        }

        window.toggleTheme = function() {
            const gender = document.getElementById('client-gender').value;
            if(gender === 'Feminino') document.body.classList.add('theme-female');
            else document.body.classList.remove('theme-female');
        }

        window.calculateBMI = function() {
            const w = parseFloat(document.getElementById('client-weight').value);
            const h = parseFloat(document.getElementById('client-height').value);
            if(w && h && h > 0) {
                const imc = w / (h * h);
                let sit = "";
                if(imc < 18.5) sit = "Abaixo do peso";
                else if(imc < 25) sit = "Peso normal";
                else if(imc < 30) sit = "Sobrepeso";
                else if(imc < 35) sit = "Obesidade Grau I";
                else if(imc < 40) sit = "Obesidade Grau II";
                else sit = "Obesidade Grau III";
                document.getElementById('bmi-result').innerText = `IMC: ${imc.toFixed(1)} (${sit})`;
            } else {
                document.getElementById('bmi-result').innerText = `IMC: --`;
            }
        }

        function initHealthCheckboxes() {
            const c = document.getElementById('health-status-container');
            c.innerHTML = HEALTH_OPTIONS.map(opt => `
                <label class="flex items-center gap-1 cursor-pointer">
                    <input type="checkbox" value="${opt}" class="health-cb rounded bg-transparent border-color">
                    <span class="truncate">${opt}</span>
                </label>
            `).join('');
        }

        window.addTab = function() {
            const nextLetter = String.fromCharCode(65 + state.workoutTabs.length);
            const newTab = { id: nextLetter, name: `Treino ${nextLetter}`, rows: [] };
            state.workoutTabs.push(newTab);
            state.activeTabId = newTab.id;
            renderTabs();
            renderTable();
        }

        window.removeCurrentTab = function() {
            if(state.workoutTabs.length <= 1) return;
            state.workoutTabs = state.workoutTabs.filter(t => t.id !== state.activeTabId);
            state.activeTabId = state.workoutTabs[0].id;
            // Rename remaining tabs
            state.workoutTabs.forEach((t, i) => {
                t.id = String.fromCharCode(65 + i);
                t.name = `Treino ${t.id}`;
            });
            renderTabs();
            renderTable();
        }

        window.switchTab = function(id) {
            state.activeTabId = id;
            renderTabs();
            renderTable();
        }

        function renderTabs() {
            const c = document.getElementById('tabs-header');
            c.innerHTML = state.workoutTabs.map(t => `
                <button onclick="switchTab('${t.id}')" class="px-4 py-2 rounded-t-lg font-bold border-b-4 whitespace-nowrap transition-colors ${t.id === state.activeTabId ? 'border-accent text-accent bg-black/5 dark:bg-white/5' : 'border-transparent text-muted hover:bg-black/5 dark:hover:bg-white/5'}">
                    ${t.name}
                </button>
            `).join('');
        }

        window.addExerciseRow = function() {
            const tab = state.workoutTabs.find(t => t.id === state.activeTabId);
            tab.rows.push({ group: '', exercise: '', sets: '', reps: '', tech: 'Nenhuma', obs: '' });
            renderTable();
        }

        window.removeExerciseRow = function(index) {
            const tab = state.workoutTabs.find(t => t.id === state.activeTabId);
            tab.rows.splice(index, 1);
            renderTable();
        }

        window.updateRow = function(index, field, value) {
            const tab = state.workoutTabs.find(t => t.id === state.activeTabId);
            tab.rows[index][field] = value;
            if(field === 'group') {
                tab.rows[index].exercise = ''; // reset exercise if group changes
                renderTable(); // re-render to update exercise dropdown
            }
        }

        function getExercisesForGroup(group) {
            if(!group) return [];
            const standard = EXERCISES_DB[group] || [];
            const custom = state.customExercises.filter(c => c.group === group).map(c => `* ${c.name}`);
            return [...standard, ...custom];
        }

        window.renderTable = function() {
            const tab = state.workoutTabs.find(t => t.id === state.activeTabId);
            const c = document.getElementById('tabs-content');
            
            if(tab.rows.length === 0) {
                c.innerHTML = `<div class="text-center p-8 text-muted border-dashed border-2 border-color rounded">Nenhum exercício neste treino. Adicione abaixo.</div>`;
                return;
            }

            const techOptions = TECHNIQUES.map(t => `<option value="${t}">${t}</option>`).join('');

            let html = `
                <table class="w-full text-sm min-w-[800px]">
                    <thead class="text-left border-b border-color">
                        <tr>
                            <th class="p-2 w-1/6">Grupo Muscular</th>
                            <th class="p-2 w-2/6">Exercício</th>
                            <th class="p-2 w-1/12">Séries</th>
                            <th class="p-2 w-1/12">Reps</th>
                            <th class="p-2 w-1/6">Técnica</th>
                            <th class="p-2 w-1/6">Obs / Descanso</th>
                            <th class="p-2">Ação</th>
                        </tr>
                    </thead>
                    <tbody>
            `;

            tab.rows.forEach((row, i) => {
                const groupOpts = '<option value="">Selecione...</option>' + Object.keys(EXERCISES_DB).map(g => `<option value="${g}" ${row.group===g?'selected':''}>${g}</option>`).join('');
                
                let exOpts = '<option value="">Selecione o Grupo primeiro</option>';
                if(row.group) {
                    exOpts = '<option value="">Selecione...</option>' + getExercisesForGroup(row.group).map(ex => `<option value="${ex}" ${row.exercise===ex?'selected':''}>${ex}</option>`).join('');
                }

                html += `
                    <tr class="border-b border-color border-dashed hover:bg-black/5 dark:hover:bg-white/5 transition-colors">
                        <td class="p-1"><select class="input-field w-full p-2 rounded border text-xs" onchange="updateRow(${i}, 'group', this.value)">${groupOpts}</select></td>
                        <td class="p-1"><select class="input-field w-full p-2 rounded border text-xs font-bold" onchange="updateRow(${i}, 'exercise', this.value)">${exOpts}</select></td>
                        <td class="p-1"><input type="text" class="input-field w-full p-2 rounded border text-xs text-center font-bold" value="${row.sets}" onchange="updateRow(${i}, 'sets', this.value)"></td>
                        <td class="p-1"><input type="text" class="input-field w-full p-2 rounded border text-xs text-center font-bold" value="${row.reps}" onchange="updateRow(${i}, 'reps', this.value)"></td>
                        <td class="p-1">
                            <select class="input-field w-full p-2 rounded border text-xs" onchange="updateRow(${i}, 'tech', this.value)">
                                ${TECHNIQUES.map(t => `<option value="${t}" ${row.tech===t?'selected':''}>${t}</option>`).join('')}
                            </select>
                        </td>
                        <td class="p-1"><input type="text" class="input-field w-full p-2 rounded border text-xs" value="${row.obs}" onchange="updateRow(${i}, 'obs', this.value)"></td>
                        <td class="p-1 text-center"><button onclick="removeExerciseRow(${i})" class="text-red-500 hover:text-red-700 font-bold px-2 py-1 bg-red-100 rounded text-xs">X</button></td>
                    </tr>
                `;
            });

            html += `</tbody></table>`;
            c.innerHTML = html;
        }

        window.saveCustomExercise = async function() {
            const group = document.getElementById('custom-ex-group').value;
            const name = document.getElementById('custom-ex-name').value.trim();
            if(!name) return;
            
            await addDoc(getPath('custom_exercises'), { group, name, createdAt: new Date().toISOString() });
            
            document.getElementById('custom-ex-name').value = '';
            toggleModal('modal-custom-ex');
        }

        // --- PRINT AND SAVE LOGIC ---
        window.saveAndPrint = async function() {
            const memberId = document.getElementById('builder-member').value;
            if(!memberId) return alert("Selecione seu Perfil de Profissional na etapa 1 antes de imprimir.");
            const clientName = document.getElementById('client-name').value;
            if(!clientName) return alert("Informe o Nome do Aluno.");

            const member = state.members.find(m => m.id === memberId);
            const unit = state.units.find(u => u.id === member.unitId);

            // Coletar Saúde
            const healthCbs = document.querySelectorAll('.health-cb:checked');
            const healthStatuses = Array.from(healthCbs).map(cb => cb.value);

            const routineData = {
                memberId,
                unitId: member.unitId,
                clientName,
                clientAge: document.getElementById('client-age').value,
                clientWeight: document.getElementById('client-weight').value,
                clientHeight: document.getElementById('client-height').value,
                clientGender: document.getElementById('client-gender').value,
                level: document.getElementById('client-level').value,
                frequency: document.getElementById('client-freq').value,
                objective: document.getElementById('client-obj').value,
                healthStatuses,
                validityDays: document.getElementById('routine-validity').value,
                obs: document.getElementById('routine-obs').value,
                tabs: state.workoutTabs,
                createdAt: new Date().toISOString()
            };

            // Salvar no Firebase
            try {
                await addDoc(getPath('routines'), routineData);
            } catch(e) {
                console.error("Erro ao salvar:", e);
            }

            // Gerar Tela de Impressão
            generatePrintView(routineData, member, unit);
            window.print();
        }

        function generatePrintView(data, member, unit) {
            const printUi = document.getElementById('print-ui');
            const d = new Date();
            const dateStr = d.toLocaleDateString('pt-BR');
            d.setDate(d.getDate() + parseInt(data.validityDays));
            const validStr = d.toLocaleDateString('pt-BR');

            // Header
            let html = `
                <div class="header-banner">
                    <h1 style="color:white; margin:0; font-style: italic;">POWFIT PRO</h1>
                    <p style="margin:2px 0 0 0; font-size:14px;">PROGRAMA DE TREINAMENTO PERSONALIZADO</p>
                </div>
                
                <div style="display:flex; justify-content:space-between; margin-top:15px; border-bottom: 2px solid #111827; padding-bottom:10px;">
                    <div style="width: 48%;">
                        <h3 style="margin:0 0 5px 0;">DADOS DO ALUNO</h3>
                        <p style="margin:2px 0;"><strong>Nome:</strong> ${data.clientName}</p>
                        <p style="margin:2px 0;"><strong>Idade:</strong> ${data.clientAge || '-'} | <strong>Peso:</strong> ${data.clientWeight || '-'}kg | <strong>Altura:</strong> ${data.clientHeight || '-'}m</p>
                        <p style="margin:2px 0;"><strong>Nível:</strong> ${data.level} | <strong>Frequência:</strong> ${data.frequency}</p>
                    </div>
                    <div style="width: 48%;">
                        <h3 style="margin:0 0 5px 0;">RESPONSÁVEL TÉCNICO</h3>
                        <p style="margin:2px 0;"><strong>Profissional:</strong> ${member.name}</p>
                        <p style="margin:2px 0;"><strong>Categoria:</strong> ${member.category === 'PEF' ? 'Profissional de Educação Física' : 'Treinador Esportivo'}</p>
                        ${member.cref ? `<p style="margin:2px 0;"><strong>CREF:</strong> ${member.cref}</p>` : ''}
                        <p style="margin:2px 0;"><strong>Unidade:</strong> ${unit.name} - ${member.uf}</p>
                    </div>
                </div>
                
                <div style="margin-top: 10px; display:flex; gap:10px;">
                    <p style="margin:0;"><strong>Emissão:</strong> ${dateStr}</p>
                    <p style="margin:0;"><strong>Validade:</strong> ${validStr} (${data.validityDays} dias)</p>
                </div>
            `;

            // Objetivo & Saúde Textos
            const objText = TEXTS_OBJ[data.objective] || '';
            let healthTexts = '';
            data.healthStatuses.forEach(hs => {
                const dict = member.category === 'PEF' ? TEXTS_HEALTH_PEF : TEXTS_HEALTH_TE;
                if(dict[hs]) healthTexts += `<li><strong>${hs}:</strong> ${dict[hs]}</li>`;
            });

            html += `
                <div style="background: #f9fafb; padding: 10px; margin-top: 15px; border: 1px solid #e5e7eb; border-radius: 4px;">
                    <p style="margin:0 0 5px 0;"><strong>Objetivo (${data.objective}):</strong> ${objText}</p>
                    ${healthTexts ? `<ul style="margin:5px 0 0 0; padding-left: 20px; font-size: 11px;">${healthTexts}</ul>` : ''}
                </div>
            `;

            // Workout Tables
            data.tabs.forEach((tab, index) => {
                if(tab.rows.length === 0) return;
                
                // Add page break before every tab except the first one, IF it has many rows. 
                // A simpler approach is avoiding break inside the block.
                html += `
                    <div class="avoid-break" style="margin-top: 20px;">
                        <h3 style="background: #111827; color: white; padding: 5px 10px; margin: 0 0 5px 0; -webkit-print-color-adjust: exact;">${tab.name}</h3>
                        <table>
                            <thead>
                                <tr>
                                    <th style="width: 35%;">Exercício</th>
                                    <th style="width: 10%; text-align:center;">Séries</th>
                                    <th style="width: 10%; text-align:center;">Reps</th>
                                    <th style="width: 15%;">Técnica</th>
                                    <th style="width: 30%;">Observações</th>
                                </tr>
                            </thead>
                            <tbody>
                `;
                tab.rows.forEach(r => {
                    html += `
                        <tr>
                            <td><strong>${r.exercise}</strong> <br><span style="font-size:9px; color:#6b7280;">${r.group}</span></td>
                            <td style="text-align:center; font-weight:bold; font-size:14px;">${r.sets}</td>
                            <td style="text-align:center; font-weight:bold; font-size:14px;">${r.reps}</td>
                            <td>${r.tech !== 'Nenhuma' ? r.tech : '-'}</td>
                            <td>${r.obs}</td>
                        </tr>
                    `;
                });
                html += `</tbody></table></div>`;
            });

            // Recomendações
            if(data.obs) {
                html += `
                    <div class="avoid-break" style="margin-top: 15px;">
                        <h3 style="margin: 0 0 5px 0; font-size: 13px;">RECOMENDAÇÕES PROFISSIONAIS</h3>
                        <p style="white-space: pre-wrap; font-size: 11px; padding: 10px; border: 1px solid #ccc; border-radius: 4px;">${data.obs}</p>
                    </div>
                `;
            }

            // Legal Footer
            const legalText = member.category === 'PEF' ? LEGAL_PEF : LEGAL_TE;
            html += `
                <div class="legal-footer avoid-break">
                    ${legalText}
                </div>
            `;

            printUi.innerHTML = html;
        }

    </script>
</body>
</html>
