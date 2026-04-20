<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro - Gestão Profissional e Franquias</title>
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
        .btn-primary { background-color: var(--primary); color: white; transition: background-color 0.2s; }
        .btn-primary:hover { background-color: var(--primary-hover); }
        .text-primary { color: var(--primary); }
        .bg-primary { background-color: var(--primary); }

        /* Esconder tudo exceto impressão */
        #print-area { display: none; }
        @media print {
            body { background: white !important; color: black !important; margin: 0; padding: 0; }
            #app-container, .modal, .no-print { display: none !important; }
            #print-area { display: block !important; padding: 10mm 15mm; font-family: 'Arial', sans-serif; font-size: 10px; }
            @page { size: A4; margin: 0; }
            
            .print-header { border-bottom: 2px solid #000; padding-bottom: 8px; margin-bottom: 12px; text-align: center; }
            .print-title { font-size: 18px; font-weight: bold; margin: 0; text-transform: uppercase; }
            .prof-info { font-size: 11px; margin-top: 5px; font-weight: bold; }
            .unit-info { font-size: 10px; font-style: italic; margin-top: 2px;}
            
            .print-grid { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 4px; margin-bottom: 15px; border: 1px solid #000; padding: 8px; }
            .print-grid div { font-size: 10px; }
            
            .print-guidelines { border: 1px solid #000; padding: 8px; margin-bottom: 15px; font-size: 9.5px; }
            .print-guidelines h4 { margin: 0 0 4px 0; font-size: 11px; text-transform: uppercase; background: #eee; padding: 3px;}
            .print-guidelines ul { margin: 0; padding-left: 15px; }
            .print-guidelines li { margin-bottom: 3px; }

            .print-workout { margin-bottom: 15px; page-break-inside: avoid; }
            .print-workout h3 { background: #e0e0e0; border: 1px solid #000; border-bottom: none; padding: 4px 8px; margin: 0; font-size: 11px; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            table { width: 100%; border-collapse: collapse; margin-bottom: 0; border: 1px solid #000; }
            th, td { border: 1px solid #000; padding: 4px 6px; text-align: left; font-size: 9.5px; }
            th { background-color: #f0f0f0; font-weight: bold; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            
            .print-footer-section { margin-top: 15px; border: 1px solid #000; padding: 8px; font-size: 9px; page-break-inside: avoid;}
            .legal-text { font-size: 8px; color: #222; text-align: justify; margin-top: 5px; font-style: italic; line-height: 1.3;}
            
            /* Relatórios */
            .report-table th, .report-table td { padding: 8px; font-size: 12px; }
            .report-header { text-align: center; margin-bottom: 20px; }
            .report-header h2 { font-size: 20px; }
        }

        /* Scrollbar */
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
        
        .tab-btn.active { border-bottom: 2px solid var(--primary); color: var(--primary); }
    </style>
</head>
<body data-theme="Masculino" class="min-h-screen pb-10">

    <!-- LOGIN SCREEN -->
    <div id="login-screen" class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-90 z-[100]">
        <div class="card p-8 rounded-2xl shadow-2xl max-w-md w-full text-center">
            <i class="fas fa-dumbbell text-5xl text-primary mb-4"></i>
            <h1 class="text-3xl font-bold mb-2">PowFit Pro</h1>
            <p class="text-sm opacity-70 mb-8">Gestão de Franquias & Fichas de Treino</p>
            <button id="btn-login" class="btn-primary w-full py-3 rounded-lg font-bold text-lg shadow-lg flex items-center justify-center gap-3">
                <i class="fab fa-google"></i> Entrar com o Google
            </button>
            <p class="mt-4 text-xs opacity-50">Seus dados são salvos com segurança na nuvem.</p>
        </div>
    </div>

    <!-- APP CONTAINER -->
    <div id="app-container" class="hidden max-w-7xl mx-auto p-4 sm:p-6">
        
        <!-- Top Navigation -->
        <nav class="flex flex-col md:flex-row justify-between items-center mb-6 gap-4 border-b border-opacity-20 pb-4" style="border-color: var(--border-color)">
            <div class="flex items-center gap-3">
                <i class="fas fa-dumbbell text-2xl text-primary"></i>
                <div>
                    <h1 class="text-xl font-bold leading-none">PowFit Pro</h1>
                    <div id="active-member-display" class="text-xs text-primary font-medium mt-1">Nenhum membro selecionado</div>
                </div>
            </div>
            
            <div class="flex flex-wrap gap-2 justify-center">
                <button onclick="switchTab('dashboard')" class="tab-btn active px-4 py-2 text-sm font-medium transition hover:text-primary"><i class="fas fa-network-wired"></i> Franquias & Equipe</button>
                <button onclick="switchTab('builder')" class="tab-btn px-4 py-2 text-sm font-medium transition hover:text-primary"><i class="fas fa-clipboard-list"></i> Montar Ficha</button>
                <button onclick="switchTab('history')" class="tab-btn px-4 py-2 text-sm font-medium transition hover:text-primary"><i class="fas fa-cloud"></i> Histórico/Relatórios</button>
                <button onclick="switchTab('database')" class="tab-btn px-4 py-2 text-sm font-medium transition hover:text-primary"><i class="fas fa-database"></i> Exercícios</button>
            </div>
            
            <button id="btn-logout" class="text-sm opacity-70 hover:text-red-500 transition"><i class="fas fa-sign-out-alt"></i> Sair</button>
        </nav>

        <!-- ============================================== -->
        <!-- TAB: DASHBOARD (FRANQUIAS E EQUIPE)            -->
        <!-- ============================================== -->
        <div id="tab-dashboard" class="space-y-6">
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                
                <!-- Redes -->
                <div class="card rounded-xl p-5 shadow-sm flex flex-col h-[500px]">
                    <div class="flex justify-between items-center mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <h2 class="font-bold"><i class="fas fa-building text-primary"></i> 1. Redes</h2>
                        <button onclick="promptAddNetwork()" class="text-xs bg-primary bg-opacity-20 text-primary px-2 py-1 rounded hover:bg-opacity-40 transition"><i class="fas fa-plus"></i> Add</button>
                    </div>
                    <div id="list-networks" class="flex-1 overflow-y-auto space-y-2 text-sm"></div>
                </div>

                <!-- Unidades -->
                <div class="card rounded-xl p-5 shadow-sm flex flex-col h-[500px]">
                    <div class="flex justify-between items-center mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <h2 class="font-bold"><i class="fas fa-store text-primary"></i> 2. Unidades</h2>
                        <button onclick="promptAddUnit()" class="text-xs bg-primary bg-opacity-20 text-primary px-2 py-1 rounded hover:bg-opacity-40 transition" id="btn-add-unit" disabled><i class="fas fa-plus"></i> Add</button>
                    </div>
                    <p id="msg-select-network" class="text-xs opacity-50 italic mb-2">Selecione uma Rede primeiro.</p>
                    <div id="list-units" class="flex-1 overflow-y-auto space-y-2 text-sm"></div>
                </div>

                <!-- Equipe / Membros -->
                <div class="card rounded-xl p-5 shadow-sm flex flex-col h-[500px]">
                    <div class="flex justify-between items-center mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <h2 class="font-bold"><i class="fas fa-users text-primary"></i> 3. Equipe</h2>
                        <button onclick="openMemberModal()" class="text-xs bg-primary bg-opacity-20 text-primary px-2 py-1 rounded hover:bg-opacity-40 transition" id="btn-add-member" disabled><i class="fas fa-plus"></i> Add</button>
                    </div>
                    <p id="msg-select-unit" class="text-xs opacity-50 italic mb-2">Selecione uma Unidade primeiro.</p>
                    <div id="list-members" class="flex-1 overflow-y-auto space-y-2 text-sm"></div>
                </div>

            </div>
        </div>

        <!-- ============================================== -->
        <!-- TAB: MONTAR FICHA                              -->
        <!-- ============================================== -->
        <div id="tab-builder" class="hidden space-y-6">
            <div id="warning-no-member" class="bg-red-500 bg-opacity-20 border border-red-500 text-red-100 p-4 rounded-xl flex items-center gap-3">
                <i class="fas fa-exclamation-triangle text-2xl"></i>
                <div>
                    <p class="font-bold">Nenhum Membro/Treinador selecionado!</p>
                    <p class="text-sm">Vá na aba "Franquias & Equipe", selecione uma Rede > Unidade e clique em "Atuar como este Membro" para poder gerar fichas.</p>
                </div>
            </div>

            <div id="builder-content" class="grid grid-cols-1 xl:grid-cols-12 gap-6 opacity-50 pointer-events-none transition-opacity">
                <!-- Coluna Esquerda: Dados -->
                <div class="xl:col-span-4 space-y-4">
                    <!-- Aluno -->
                    <div class="card rounded-xl p-5">
                        <div class="flex justify-between items-center mb-3"><h2 class="font-bold"><i class="fas fa-user text-primary"></i> Aluno</h2><div id="imc-display"></div></div>
                        <div class="space-y-3">
                            <input type="text" id="stu-name" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="Nome Completo">
                            <div class="grid grid-cols-3 gap-2">
                                <input type="number" id="stu-age" class="input-field rounded px-2 py-2 text-sm" placeholder="Idade">
                                <input type="number" id="stu-weight" oninput="calcIMC()" class="input-field rounded px-2 py-2 text-sm" placeholder="Peso(kg)">
                                <input type="number" id="stu-height" step="0.01" oninput="calcIMC()" class="input-field rounded px-2 py-2 text-sm" placeholder="Alt(m)">
                            </div>
                            <div class="grid grid-cols-2 gap-2">
                                <select id="stu-gender" onchange="changeTheme()" class="input-field rounded px-2 py-2 text-sm"><option value="Masculino">Masculino</option><option value="Feminino">Feminino</option></select>
                                <select id="stu-level" class="input-field rounded px-2 py-2 text-sm"><option>Iniciante</option><option>Intermediário</option><option>Avançado</option></select>
                            </div>
                            <select id="stu-objective" class="input-field w-full rounded px-2 py-2 text-sm">
                                <option>Emagrecimento</option><option>Hipertrofia</option><option>Definição</option><option>Condicionamento</option>
                                <option>Resistência</option><option>Força</option><option>Reabilitação</option><option>Saúde geral</option>
                            </select>
                            <select id="stu-freq" class="input-field w-full rounded px-2 py-2 text-sm">
                                <option>3 dias</option><option>5 dias</option><option>6 dias</option><option>Personalizado</option>
                            </select>
                        </div>
                    </div>
                    <!-- Saúde -->
                    <div class="card rounded-xl p-4">
                        <h2 class="font-bold mb-2"><i class="fas fa-notes-medical text-primary"></i> Saúde <span class="text-[10px] opacity-60 font-normal">(Baseado no perfil)</span></h2>
                        <div id="health-container" class="grid grid-cols-2 gap-1 text-[11px]"></div>
                    </div>
                    <!-- Config -->
                    <div class="card rounded-xl p-4 space-y-3">
                        <select id="stu-validity" class="input-field w-full rounded px-2 py-2 text-sm">
                            <option value="15">15 dias</option><option value="30" selected>30 dias</option><option value="60">60 dias</option><option value="90">90 dias</option>
                        </select>
                        <textarea id="stu-recs" rows="3" class="input-field w-full rounded px-2 py-2 text-sm" placeholder="Recomendações livres..."></textarea>
                    </div>
                </div>

                <!-- Coluna Direita: Treinos -->
                <div class="xl:col-span-8 space-y-4">
                    <div class="flex justify-between items-center p-3 rounded-xl card border-dashed border-2 bg-black bg-opacity-10">
                        <h2 class="font-bold text-lg"><i class="fas fa-layer-group text-primary"></i> Estrutura do Treino</h2>
                        <div class="flex gap-2">
                            <button onclick="addWorkoutDay()" class="bg-gray-700 hover:bg-gray-600 text-white px-3 py-1.5 rounded text-sm transition shadow"><i class="fas fa-plus"></i> Aba Treino</button>
                            <button onclick="saveAndPrint()" id="btn-save-print" class="btn-primary px-4 py-1.5 rounded text-sm font-bold shadow flex items-center gap-2"><i class="fas fa-save"></i> Salvar & Imprimir</button>
                        </div>
                    </div>
                    <div id="workouts-container" class="space-y-4"></div>
                </div>
            </div>
        </div>

        <!-- ============================================== -->
        <!-- TAB: HISTÓRICO E RELATÓRIOS                    -->
        <!-- ============================================== -->
        <div id="tab-history" class="hidden space-y-6">
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <!-- Relatórios -->
                <div class="card rounded-xl p-5">
                    <h2 class="font-bold text-lg mb-4"><i class="fas fa-chart-bar text-primary"></i> Relatório de Produtividade Mensal</h2>
                    <p class="text-sm opacity-70 mb-4">Gere um relatório contabilizando a quantidade de fichas criadas por cada membro da equipe neste mês.</p>
                    <button onclick="generateReport()" class="btn-primary w-full py-2 rounded shadow flex justify-center items-center gap-2"><i class="fas fa-print"></i> Gerar & Imprimir Relatório</button>
                </div>
                <!-- Filtro Histórico -->
                <div class="card rounded-xl p-5 flex flex-col justify-center">
                    <h2 class="font-bold text-lg mb-4"><i class="fas fa-search text-primary"></i> Nuvem: Buscar Fichas</h2>
                    <input type="text" id="search-history" onkeyup="renderHistory()" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="Buscar por nome do aluno...">
                </div>
            </div>

            <div class="card rounded-xl p-0 overflow-hidden">
                <table class="w-full text-left text-sm">
                    <thead class="bg-black bg-opacity-20 text-xs uppercase opacity-70 border-b border-gray-600">
                        <tr><th class="p-3">Data</th><th class="p-3">Aluno</th><th class="p-3">Criador (Membro)</th><th class="p-3 text-center">Status</th><th class="p-3 text-right">Ações</th></tr>
                    </thead>
                    <tbody id="history-table-body" class="divide-y divide-gray-700 divide-opacity-30"></tbody>
                </table>
            </div>
        </div>

        <!-- ============================================== -->
        <!-- TAB: BANCO DE DADOS EXERCÍCIOS                 -->
        <!-- ============================================== -->
        <div id="tab-database" class="hidden space-y-6">
            <div class="card p-5 rounded-xl">
                <h2 class="font-bold text-lg mb-4"><i class="fas fa-database text-primary"></i> Gerenciar Banco de Exercícios</h2>
                <div class="flex flex-col md:flex-row gap-4 mb-6">
                    <select id="db-category-select" class="input-field rounded px-3 py-2 text-sm w-full md:w-1/3" onchange="renderDBExercises()">
                        <!-- Preenchido via JS -->
                    </select>
                    <div class="flex w-full md:w-2/3 gap-2">
                        <input type="text" id="db-new-exercise" class="input-field flex-1 rounded px-3 py-2 text-sm" placeholder="Nome do novo exercício...">
                        <button onclick="addCustomExercise()" class="btn-primary px-4 py-2 rounded shadow font-bold"><i class="fas fa-plus"></i></button>
                    </div>
                </div>
                <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-3" id="db-exercise-list">
                    <!-- Gerado via JS -->
                </div>
            </div>
        </div>

    </div>

    <!-- MODAL: ADD MEMBER -->
    <div id="modal-member" class="fixed inset-0 bg-black bg-opacity-80 flex items-center justify-center hidden z-50 p-4">
        <div class="card p-6 rounded-xl w-full max-w-md shadow-2xl border border-gray-600">
            <h3 class="text-lg font-bold mb-4"><i class="fas fa-user-plus text-primary"></i> Adicionar Membro</h3>
            <div class="space-y-3">
                <input type="text" id="m-name" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="Nome Completo">
                <input type="text" id="m-cpf" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="CPF">
                <input type="text" id="m-state" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="Estado (UF)">
                <select id="m-category" onchange="toggleCrefModal()" class="input-field w-full rounded px-3 py-2 text-sm">
                    <option value="PEF">Profissional de Educação Física (PEF)</option>
                    <option value="TE">Treinador Esportivo (TE)</option>
                </select>
                <input type="text" id="m-cref" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="CREF (Ex: 000000-G/SP)">
            </div>
            <div class="flex justify-end gap-2 mt-6">
                <button onclick="closeMemberModal()" class="px-4 py-2 text-sm hover:text-red-500">Cancelar</button>
                <button onclick="saveMember()" class="btn-primary px-6 py-2 rounded shadow font-bold">Salvar</button>
            </div>
        </div>
    </div>

    <!-- MODAL: SELECIONAR EXERCÍCIO -->
    <div id="modal-exercise" class="fixed inset-0 bg-black bg-opacity-80 flex items-center justify-center hidden z-[60] p-2 sm:p-4">
        <div class="card w-full max-w-5xl rounded-xl shadow-2xl flex flex-col h-[90vh]">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="font-bold"><i class="fas fa-search text-primary"></i> Adicionar à Ficha</h3>
                <button onclick="closeExerciseModal()" class="text-2xl text-gray-400 hover:text-red-500 leading-none">&times;</button>
            </div>
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <div class="w-full md:w-1/4 border-r overflow-y-auto p-2 space-y-1" style="border-color: var(--border-color)" id="modal-ex-categories"></div>
                <div class="w-full md:w-3/4 overflow-y-auto p-3 bg-black bg-opacity-10" id="modal-ex-list"></div>
            </div>
        </div>
    </div>

    <!-- ÁREA DE IMPRESSÃO -->
    <div id="print-area"></div>

    <!-- SCRIPTS FIREBASE & LOGIC -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-app.js";
        import { getAuth, signInWithPopup, GoogleAuthProvider, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, addDoc, query, where, getDocs, orderBy, deleteDoc } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-firestore.js";

        // FIREBASE CONFIG (Fornecida no prompt)
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

        // --- CONSTANTES DE DADOS ---
        const dbCategoriesDefault = {
            "🔥 PEITO": ["Supino Reto", "Supino Inclinado", "Supino Inclinado com Halteres", "Supino Fechado com Halteres", "Cross Over", "Cross Over Alto", "Cross Over Baixo", "Crucifixo Reto", "Crucifixo Inclinado com Halteres", "Crucifixo na Máquina", "Peck Fly", "Peck Fly Unilateral", "Pullover", "Flexão de Braço", "Flexão com Pés Elevados", "Flexão Explosiva"],
            "🦍 COSTAS": ["Puxada Alta", "Puxada de Frente Supinada", "Pulldown", "Remada Aberta", "Remada Baixa", "Remada Curvada", "Remada Curvada Supinada", "Remada Cavalinho (T-Bar)", "Serrote", "Facepull (puxada de cima para baixo)"],
            "🦵 PERNAS": ["Agachamento Livre", "Agachamento Taça", "Agachamento no Smith", "Agachamento com passada lateral", "Squat", "Hack Machine", "Leg 45°", "Leg 90°", "Agachamento Sumô", "Agachamento Sissy (Livre)", "Afundo", "Recuo", "Avanço", "Passada", "Búlgaro", "Step-up", "Levantamento Terra", "Levantamento Terra Romeno", "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", "Cadeira Flexora", "Elevação Pélvica no Banco", "Elevação Pélvica no Chão", "Elevação Pélvica Unilateral no Chão", "Extensão de Quadril (Glúteo Máximo)", "Extensão Cruzada (Glúteo Médio)", "Coice", "Cachorrinho", "Cadeira Extensora", "Adução", "Abdução", "Abdução Inclinada", "Flexão Nórdica", "Flexão Nórdica Invertida", "Panturrilha Livre", "Panturrilha no Leg Press", "Panturrilha Banco", "Panturrilha Squat", "Panturrilha Unilateral"],
            "💪 BRAÇOS (Bíceps)": ["Rosca Direta", "Rosca Alternada", "Rosca 21", "Rosca Scott Barra W", "Rosca Scott Unilateral", "Rosca Scott com Halteres", "Rosca Martelo", "Rosca Cross", "Rosca Inversa", "Rosca 45°"],
            "💪 BRAÇOS (Tríceps)": ["Triceps Pulley Unilateral", "Tríceps Pulley Barra", "Tríceps Pulley Corda", "Tríceps Pulley Pegada Inversa", "Tríceps Francês na Corda", "Tríceps Francês com Halter", "Tríceps Francês Unilateral", "Tríceps Cruzado Polia Dupla", "Tríceps Coice Unilateral", "Tríceps Arremesso", "Tríceps Testa", "Mergulho no Banco"],
            "🪨 OMBROS": ["Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", "Elevação Lateral Unilateral Cross", "Elevação Lateral Sentado", "Desenvolvimento com Halteres", "Desenvolvimento com Barra", "Arnold Press", "Elevação Borboleta", "Crucifixo Inverso Sentado com Halteres", "Crucifixo Inverso na Polia", "Crucifixo Inverso Unilateral na Polia", "Facepull (puxada reta)", "Remada Alta", "Encolhimento (Trapézio)"],
            "🧠 ABDÔMEN": ["Infra com Elevação de Perna", "Abdominal Supra", "Abdominal Remador", "Abdominal Bicicleta", "Abdominal Twister com Peso", "Prancha", "Prancha Lateral", "Trituração de Cabos em Pé", "Isometria na parede"],
            "🫀 CARDIO": ["Bicicleta 10 Minutos", "Bicicleta 15 Minutos", "Bicicleta 20 Minutos", "Esteira 10 Minutos", "Esteira 15 Minutos", "Esteira 20 Minutos", "Pular Corda"]
        };

        const healthPEF = {"🟢 Saudável": "Manter treinos regulares 3–5x por semana, combinando musculação e cardio. Foco em evolução e constância.","⚪ Sedentário": "Iniciar com treinos leves 2–3x por semana. Priorizar adaptação, técnica e evitar excesso de carga.","🟡 Sobrepeso": "Treinar 3–5x por semana com foco em gasto calórico. Combinar musculação e cardio moderado.","🔴 Obesidade": "Iniciar com exercícios de baixo impacto. Evolução gradual, priorizando saúde e segurança.","⚖️ Baixo peso": "Foco em musculação para ganho de massa. Treinos moderados com alimentação adequada.","🍬 Diabetes": "Treinos regulares moderados. Monitorar glicemia e evitar treinos em jejum.","❤️ Hipertensão": "Treinos moderados, evitar prender a respiração. Priorizar controle da intensidade.","🔵 Hipotensão": "Evitar mudanças bruscas. Manter hidratação e intensidade leve a moderada.","💔 Problemas cardíacos": "Treinos leves com liberação médica. Monitorar frequência cardíaca.","🦴 Problemas articulares": "Evitar impacto. Priorizar exercícios controlados e máquinas.","🫁 Problemas respiratórios": "Treinos leves a moderados com progressão gradual. Atenção à respiração.","⚠️ Lesões": "Adaptar exercícios. Evitar dor e focar na recuperação.","🤰 Gestante": "Treinos leves a moderados, sem impacto ou risco. Foco em mobilidade e bem-estar.","🤱 Lactante": "Treinar normalmente com atenção à hidratação e energia.","👴 Idoso": "Foco em força, equilíbrio e mobilidade. Intensidade moderada com segurança."};
        const healthTE = {"🟢 Saudável": "Manter treinos regulares de 3–5x por semana, combinando musculação e atividades cardiovasculares. A constância, descanso adequado e boa alimentação contribuem para melhores resultados.","⚪ Sedentário": "O início deve ser gradual, com treinos leves e foco na adaptação do corpo. A prioridade é desenvolver constância, aprender a execução correta e evitar excesso de carga.","🟡 Sobrepeso": "A prática regular de musculação associada ao cardio pode contribuir para melhora do condicionamento físico e composição corporal. A progressão deve respeitar a individualidade e o nível de adaptação.","🔴 Obesidade": "É recomendado iniciar com exercícios de menor impacto e progressão gradual. A segurança, mobilidade e constância são prioridades durante o processo.","⚖️ Baixo peso": "A musculação pode auxiliar no ganho de massa muscular quando associada a alimentação adequada e descanso. O treino deve priorizar evolução progressiva e recuperação muscular.","🍬 Diabetes": "Pessoas com diabetes devem manter acompanhamento médico regular antes e durante a prática de exercícios. A musculação pode auxiliar na rotina de atividade física quando liberada por profissional de saúde. Em casos de tontura, mal-estar ou alteração de glicemia, o treino deve ser interrompido e o aluno deve procurar orientação médica.","❤️ Hipertensão": "Pessoas com hipertensão devem manter acompanhamento médico regular e respeitar as orientações profissionais. Durante o treino, é importante evitar prender a respiração e controlar a intensidade dos exercícios.","🔵 Hipotensão": "Pessoas com hipotensão devem evitar mudanças bruscas de posição durante o treino. Manter boa hidratação e respeitar a intensidade adequada ajuda a reduzir episódios de mal-estar.","💔 Problemas cardíacos": "A prática de exercícios deve ocorrer apenas com liberação e acompanhamento médico. O treino deve respeitar limites individuais, com controle de intensidade e atenção aos sinais do corpo.","🦴 Problemas articulares": "Exercícios com menor impacto e maior controle de movimento costumam ser mais indicados. O acompanhamento profissional e a adaptação dos exercícios ajudam na segurança durante o treino.","🫁 Problemas respiratórios": "A progressão do treino deve ser gradual e respeitar a capacidade respiratória individual. Em caso de falta de ar excessiva ou desconforto, o exercício deve ser interrompido.","⚠️ Lesões": "A adaptação dos exercícios deve respeitar a limitação existente e evitar dor durante a execução. Em situações de lesão, é importante seguir orientação profissional adequada antes da prática.","🤰 Gestante": "A prática de exercícios deve ocorrer com liberação médica e acompanhamento adequado. O foco deve ser segurança, mobilidade e bem-estar, evitando impacto excessivo e situações de risco.","🤱 Lactante": "A atividade física pode ser mantida normalmente, respeitando a recuperação individual e mantendo boa hidratação e alimentação adequada.","👴 Idoso": "A musculação pode contribuir para força, equilíbrio, mobilidade e autonomia. O treino deve respeitar limitações individuais, com intensidade moderada e foco na segurança."};
        const objData = {"Emagrecimento": "Foco em déficit calórico com treinos mistos de força (manter massa magra) e aeróbicos (maior gasto).","Hipertrofia": "Prioridade na progressão de carga e volume adequado. Essencial superávit calórico e descanso.","Definição": "Manutenção de massa muscular enquanto reduz o percentual de gordura. Atenção estrita à dieta.","Condicionamento": "Treinos com menor tempo de intervalo, circuitos e alta integração cardiopulmonar.","Resistência": "Séries mais longas, cadência controlada e aprimoramento da capacidade muscular.","Força": "Cargas altas, baixas repetições e intervalos de descanso maiores.","Reabilitação": "Treino focado em fortalecimento específico, mobilidade e controle motor. Respeitar limites.","Saúde geral": "Equilíbrio entre força, cardio e flexibilidade. O principal objetivo é a constância e bem-estar."};
        const techniques = ["Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", "Negativa", "Isometria", "Parciais", "Pirâmide"];
        const daysWeek = ["SEGUNDA-FEIRA", "TERÇA-FEIRA", "QUARTA-FEIRA", "QUINTA-FEIRA", "SEXTA-FEIRA", "SÁBADO", "DOMINGO"];

        // --- ESTADO GLOBAL ---
        window.state = {
            user: null,
            data: { networks: [], units: [], members: [], customEx: [] },
            mergedEx: {},
            selections: { netId: null, unitId: null, memberId: null },
            builder: { workouts: [], activeCat: "🔥 PEITO", addModalDayId: null },
            historyCache: []
        };

        // --- AUTH ---
        document.getElementById('btn-login').addEventListener('click', () => signInWithPopup(auth, new GoogleAuthProvider()));
        document.getElementById('btn-logout').addEventListener('click', () => signOut(auth));

        onAuthStateChanged(auth, async (user) => {
            if (user) {
                window.state.user = user;
                document.getElementById('login-screen').classList.add('hidden');
                document.getElementById('app-container').classList.remove('hidden');
                await loadUserData();
                switchTab('dashboard');
                updateMergedExercises();
            } else {
                window.state.user = null;
                document.getElementById('login-screen').classList.remove('hidden');
                document.getElementById('app-container').classList.add('hidden');
            }
        });

        // --- DATABASE SYNC ---
        async function loadUserData() {
            try {
                const docSnap = await getDoc(doc(db, "powfit_users", window.state.user.uid));
                if (docSnap.exists()) {
                    window.state.data = { networks: [], units: [], members: [], customEx: [], ...docSnap.data() };
                } else {
                    await saveUserData(); // Creates empty structure
                }
                renderDashboard();
            } catch (e) { console.error("Erro ao carregar dados", e); }
        }

        async function saveUserData() {
            if(!window.state.user) return;
            try {
                await setDoc(doc(db, "powfit_users", window.state.user.uid), window.state.data);
            } catch (e) { console.error("Erro ao salvar", e); }
        }

        // --- UI NAVIGATION ---
        window.switchTab = (tabId) => {
            ['dashboard', 'builder', 'history', 'database'].forEach(id => {
                document.getElementById(`tab-${id}`).classList.add('hidden');
            });
            document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));
            
            document.getElementById(`tab-${tabId}`).classList.remove('hidden');
            event.currentTarget.classList.add('active');

            if(tabId === 'history') loadHistory();
            if(tabId === 'database') renderDBCategorySelect();
        };

        // --- DASHBOARD (REDE/UNIDADE/MEMBRO) ---
        function renderDashboard() {
            const listN = document.getElementById('list-networks');
            listN.innerHTML = window.state.data.networks.map(n => `
                <div onclick="selectNetwork('${n.id}')" class="p-2 rounded cursor-pointer border border-transparent transition ${window.state.selections.netId===n.id ? 'bg-primary text-white' : 'hover:border-gray-500 bg-black bg-opacity-20'}">
                    ${n.name}
                </div>`).join('');

            const listU = document.getElementById('list-units');
            const btnU = document.getElementById('btn-add-unit');
            const msgU = document.getElementById('msg-select-network');
            if(window.state.selections.netId) {
                btnU.disabled = false; msgU.classList.add('hidden');
                const units = window.state.data.units.filter(u => u.netId === window.state.selections.netId);
                listU.innerHTML = units.map(u => `
                    <div onclick="selectUnit('${u.id}')" class="p-2 rounded cursor-pointer border border-transparent transition ${window.state.selections.unitId===u.id ? 'bg-primary text-white' : 'hover:border-gray-500 bg-black bg-opacity-20'}">
                        ${u.name}
                    </div>`).join('');
            } else { btnU.disabled = true; msgU.classList.remove('hidden'); listU.innerHTML=''; }

            const listM = document.getElementById('list-members');
            const btnM = document.getElementById('btn-add-member');
            const msgM = document.getElementById('msg-select-unit');
            if(window.state.selections.unitId) {
                btnM.disabled = false; msgM.classList.add('hidden');
                const members = window.state.data.members.filter(m => m.unitId === window.state.selections.unitId);
                listM.innerHTML = members.map(m => `
                    <div class="p-2 rounded border border-transparent bg-black bg-opacity-20 flex justify-between items-center ${window.state.selections.memberId===m.id ? 'ring-2 ring-primary' : ''}">
                        <div><div class="font-bold">${m.name}</div><div class="text-[10px] opacity-70">${m.category} | ${m.state}</div></div>
                        <button onclick="setActiveMember('${m.id}')" class="text-xs bg-primary text-white px-2 py-1 rounded shadow">Atuar</button>
                    </div>`).join('');
            } else { btnM.disabled = true; msgM.classList.remove('hidden'); listM.innerHTML=''; }
        }

        window.promptAddNetwork = () => {
            const name = prompt("Nome da Franquia/Rede:");
            if(name) { window.state.data.networks.push({id: 'net_'+Date.now(), name}); saveUserData(); renderDashboard(); }
        };
        window.promptAddUnit = () => {
            const name = prompt("Nome da Unidade:");
            if(name) { window.state.data.units.push({id: 'unit_'+Date.now(), netId: window.state.selections.netId, name}); saveUserData(); renderDashboard(); }
        };
        window.selectNetwork = (id) => { window.state.selections.netId = id; window.state.selections.unitId = null; renderDashboard(); };
        window.selectUnit = (id) => { window.state.selections.unitId = id; renderDashboard(); };
        
        // Modal Member
        window.openMemberModal = () => document.getElementById('modal-member').classList.remove('hidden');
        window.closeMemberModal = () => document.getElementById('modal-member').classList.add('hidden');
        window.toggleCrefModal = () => {
            const cat = document.getElementById('m-category').value;
            document.getElementById('m-cref').style.display = (cat === 'PEF') ? 'block' : 'none';
        };
        window.saveMember = () => {
            const m = {
                id: 'mem_'+Date.now(), unitId: window.state.selections.unitId,
                name: document.getElementById('m-name').value, cpf: document.getElementById('m-cpf').value,
                state: document.getElementById('m-state').value, category: document.getElementById('m-category').value,
                cref: document.getElementById('m-cref').value
            };
            if(!m.name || !m.state) return alert("Preencha Nome e Estado");
            window.state.data.members.push(m); saveUserData(); closeMemberModal(); renderDashboard();
            document.getElementById('m-name').value=''; document.getElementById('m-cpf').value=''; document.getElementById('m-cref').value='';
        };

        window.setActiveMember = (id) => {
            window.state.selections.memberId = id;
            const m = window.state.data.members.find(x => x.id === id);
            const u = window.state.data.units.find(x => x.id === m.unitId);
            const n = window.state.data.networks.find(x => x.id === u.netId);
            
            document.getElementById('active-member-display').innerText = `${m.name} (${m.category}) - ${u.name}`;
            document.getElementById('warning-no-member').classList.add('hidden');
            document.getElementById('builder-content').classList.remove('opacity-50', 'pointer-events-none');
            
            // Gerar Health Options baseadas na categoria
            const healthSource = m.category === 'PEF' ? healthPEF : healthTE;
            document.getElementById('health-container').innerHTML = Object.keys(healthSource).map(opt => `
                <label class="flex items-start gap-1.5 cursor-pointer p-1 rounded hover:bg-black hover:bg-opacity-20 border border-transparent">
                    <input type="checkbox" value="${opt}" class="health-cb mt-0.5 w-3 h-3 text-primary bg-transparent border-gray-500">
                    <span>${opt}</span>
                </label>`).join('');
            
            renderDashboard();
        };

        // --- BUILDER (ALUNO E FICHAS) ---
        window.calcIMC = () => {
            const w = parseFloat(document.getElementById('stu-weight').value);
            const h = parseFloat(document.getElementById('stu-height').value);
            const d = document.getElementById('imc-display');
            if(w>0 && h>0) {
                const imc = (w/(h*h)).toFixed(1);
                let c = imc<18.5?"Abaixo":imc<24.9?"Normal":imc<29.9?"Sobrepeso":"Obesidade";
                d.innerHTML = `<span class="bg-primary bg-opacity-20 text-primary px-2 py-1 rounded text-[10px] font-bold">IMC: ${imc} (${c})</span>`;
            } else d.innerHTML='';
        };
        window.changeTheme = () => document.body.setAttribute('data-theme', document.getElementById('stu-gender').value);

        window.addWorkoutDay = () => {
            const count = window.state.builder.workouts.length;
            const title = count < 7 ? `TREINO ${daysWeek[count]}` : `NOVO TREINO ${count+1}`;
            window.state.builder.workouts.push({ id: generateId(), title, exercises: [] });
            renderBuilderWorkouts();
        };
        window.duplicateWorkout = (id) => {
            const w = window.state.builder.workouts.find(x => x.id === id);
            if(w) { window.state.builder.workouts.push({...JSON.parse(JSON.stringify(w)), id: generateId(), title: w.title+" (Cópia)"}); renderBuilderWorkouts(); }
        };
        window.removeWorkout = (id) => { if(confirm("Excluir dia?")) { window.state.builder.workouts = window.state.builder.workouts.filter(w=>w.id!==id); renderBuilderWorkouts(); } };
        window.updateWorkoutTitle = (id, val) => { window.state.builder.workouts.find(w=>w.id===id).title = val; };

        // Ex logic
        window.moveExercise = (wId, idx, dir) => {
            const w = window.state.builder.workouts.find(x=>x.id===wId);
            if(dir==='up'&&idx>0) [w.exercises[idx], w.exercises[idx-1]] = [w.exercises[idx-1], w.exercises[idx]];
            else if(dir==='down'&&idx<w.exercises.length-1) [w.exercises[idx], w.exercises[idx+1]] = [w.exercises[idx+1], w.exercises[idx]];
            renderBuilderWorkouts();
        };
        window.removeExercise = (wId, idx) => { window.state.builder.workouts.find(x=>x.id===wId).exercises.splice(idx,1); renderBuilderWorkouts(); };
        window.updateExercise = (wId, idx, field, val) => { window.state.builder.workouts.find(x=>x.id===wId).exercises[idx][field] = val; };

        function renderBuilderWorkouts() {
            const cont = document.getElementById('workouts-container');
            if(window.state.builder.workouts.length === 0) { cont.innerHTML = '<div class="text-center p-4 opacity-50 text-sm">Nenhum dia de treino adicionado.</div>'; return; }
            
            cont.innerHTML = window.state.builder.workouts.map(w => {
                const exHtml = w.exercises.length===0 ? `<div class="text-center p-3 text-xs opacity-50">Sem exercícios</div>` : 
                    w.exercises.map((ex, i) => `
                        <div class="flex flex-col sm:flex-row gap-1 p-1.5 border-b border-opacity-20 items-center text-xs" style="border-color: var(--border-color)">
                            <div class="flex flex-col gap-0.5 w-full sm:w-auto hidden sm:flex">
                                <button onclick="moveExercise('${w.id}',${i},'up')" class="hover:bg-gray-500 rounded px-1"><i class="fas fa-caret-up"></i></button>
                                <button onclick="moveExercise('${w.id}',${i},'down')" class="hover:bg-gray-500 rounded px-1"><i class="fas fa-caret-down"></i></button>
                            </div>
                            <div class="flex-1 w-full sm:w-auto leading-tight"><div class="text-[9px] opacity-60 uppercase">${ex.category}</div><div class="font-bold">${ex.name}</div></div>
                            <div class="flex gap-1 w-full sm:w-auto">
                                <input type="text" value="${ex.sets}" onchange="updateExercise('${w.id}',${i},'sets',this.value)" class="input-field w-10 text-center rounded p-1" placeholder="S"> x 
                                <input type="text" value="${ex.reps}" onchange="updateExercise('${w.id}',${i},'reps',this.value)" class="input-field w-14 text-center rounded p-1" placeholder="Reps">
                                <select onchange="updateExercise('${w.id}',${i},'technique',this.value)" class="input-field w-20 rounded p-1 text-[10px]">${techniques.map(t=>`<option ${t===ex.technique?'selected':''}>${t}</option>`).join('')}</select>
                                <input type="text" value="${ex.obs}" onchange="updateExercise('${w.id}',${i},'obs',this.value)" class="input-field flex-1 sm:w-24 rounded p-1" placeholder="Obs">
                                <button onclick="removeExercise('${w.id}',${i})" class="text-red-500 hover:bg-red-500 hover:text-white px-2 rounded transition"><i class="fas fa-trash"></i></button>
                            </div>
                        </div>`).join('');

                return `
                <div class="card rounded-xl overflow-hidden shadow-sm">
                    <div class="p-2 border-b bg-black bg-opacity-10 flex justify-between items-center" style="border-color: var(--border-color)">
                        <input type="text" value="${w.title}" onchange="updateWorkoutTitle('${w.id}', this.value)" class="input-field bg-transparent font-bold text-sm px-2 py-1 w-1/2 uppercase border-transparent">
                        <div class="flex gap-1">
                            <button onclick="duplicateWorkout('${w.id}')" class="px-2 py-1 hover:bg-black hover:bg-opacity-20 rounded text-xs"><i class="fas fa-copy"></i></button>
                            <button onclick="removeWorkout('${w.id}')" class="px-2 py-1 text-red-500 hover:bg-red-500 hover:text-white rounded text-xs"><i class="fas fa-trash"></i></button>
                        </div>
                    </div>
                    <div>${exHtml}</div>
                    <div class="p-1 border-t" style="border-color: var(--border-color)">
                        <button onclick="openExerciseModal('${w.id}')" class="w-full text-primary py-1.5 text-xs font-bold hover:bg-primary hover:bg-opacity-10 rounded"> + ADD EXERCÍCIO </button>
                    </div>
                </div>`;
            }).join('');
        }

        // Modal Exercise Builder
        function updateMergedExercises() {
            window.state.mergedEx = JSON.parse(JSON.stringify(dbCategoriesDefault));
            window.state.data.customEx.forEach(c => {
                if(!window.state.mergedEx[c.category]) window.state.mergedEx[c.category] = [];
                window.state.mergedEx[c.category].push(c.name);
            });
        }
        window.openExerciseModal = (id) => { window.state.builder.addModalDayId = id; document.getElementById('modal-exercise').classList.remove('hidden'); renderExModalCats(); };
        window.closeExerciseModal = () => document.getElementById('modal-exercise').classList.add('hidden');
        window.setExModalCat = (cat) => { window.state.builder.activeCat = cat; renderExModalCats(); };
        
        function renderExModalCats() {
            document.getElementById('modal-ex-categories').innerHTML = Object.keys(window.state.mergedEx).map(c => `
                <button onclick="setExModalCat('${c}')" class="w-full text-left p-2 rounded text-xs font-bold ${window.state.builder.activeCat===c?'bg-primary text-white':'hover:bg-black hover:bg-opacity-20'}">${c}</button>
            `).join('');
            
            const isCardio = window.state.builder.activeCat === "🫀 CARDIO";
            document.getElementById('modal-ex-list').innerHTML = `<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-2">` + 
                window.state.mergedEx[window.state.builder.activeCat].map(ex => `
                <button onclick="addExToDay('${ex}', ${isCardio})" class="card p-2 rounded text-left text-xs font-medium border border-transparent hover:border-primary transition group flex justify-between">
                    ${ex} <i class="fas fa-plus opacity-0 group-hover:opacity-100 text-primary"></i>
                </button>`).join('') + `</div>`;
        }
        window.addExToDay = (name, isCardio) => {
            const w = window.state.builder.workouts.find(x => x.id === window.state.builder.addModalDayId);
            w.exercises.push({ category: window.state.builder.activeCat, name, sets: isCardio?'1':'3', reps: isCardio?'-':'10 a 12', technique: 'Nenhuma', obs: '' });
            renderBuilderWorkouts();
            const el = document.getElementById('modal-ex-list'); el.style.opacity='0.5'; setTimeout(()=>el.style.opacity='1', 100);
        };

        // --- DATABASE MANAGER (EXERCISES) ---
        window.renderDBCategorySelect = () => {
            document.getElementById('db-category-select').innerHTML = Object.keys(window.state.mergedEx).map(c => `<option value="${c}">${c}</option>`).join('');
            renderDBExercises();
        };
        window.renderDBExercises = () => {
            const cat = document.getElementById('db-category-select').value;
            const customForCat = window.state.data.customEx.filter(x => x.category === cat).map(x=>x.name);
            const allForCat = window.state.mergedEx[cat] || [];
            
            document.getElementById('db-exercise-list').innerHTML = allForCat.map(ex => {
                const isCustom = customForCat.includes(ex);
                return `
                <div class="card p-2 rounded text-xs flex justify-between items-center border border-gray-600 border-opacity-30">
                    <span>${ex} ${isCustom?'<span class="text-[9px] bg-primary px-1 rounded ml-1 text-white">Custom</span>':''}</span>
                    ${isCustom ? `<button onclick="removeCustomExercise('${cat}', '${ex}')" class="text-red-500 hover:bg-red-500 hover:text-white px-1.5 py-0.5 rounded"><i class="fas fa-trash"></i></button>` : ''}
                </div>`;
            }).join('');
        };
        window.addCustomExercise = () => {
            const cat = document.getElementById('db-category-select').value;
            const name = document.getElementById('db-new-exercise').value.trim();
            if(!name) return;
            if(window.state.mergedEx[cat] && window.state.mergedEx[cat].includes(name)) return alert("Exercício já existe!");
            
            window.state.data.customEx.push({ category: cat, name });
            saveUserData(); updateMergedExercises(); renderDBExercises();
            document.getElementById('db-new-exercise').value = '';
        };
        window.removeCustomExercise = (cat, name) => {
            if(!confirm(`Remover ${name}?`)) return;
            window.state.data.customEx = window.state.data.customEx.filter(x => !(x.category===cat && x.name===name));
            saveUserData(); updateMergedExercises(); renderDBExercises();
        };

        // --- NUVEM: SALVAR E IMPRIMIR ---
        window.saveAndPrint = async () => {
            if(!window.state.selections.memberId) return alert("Selecione um membro no Dashboard primeiro!");
            const btn = document.getElementById('btn-save-print');
            btn.innerHTML = '<i class="fas fa-spinner fa-spin"></i> Salvando...'; btn.disabled = true;

            const mId = window.state.selections.memberId;
            const m = window.state.data.members.find(x => x.id === mId);
            const u = window.state.data.units.find(x => x.id === m.unitId);
            const net = window.state.data.networks.find(x => x.id === u.netId);

            const workoutData = {
                memberId: m.id, memberName: m.name, memberCategory: m.category, memberCref: m.cref, memberState: m.state,
                unitId: u.id, unitName: u.name, networkName: net.name,
                createdAt: Date.now(),
                validityDays: parseInt(document.getElementById('stu-validity').value),
                studentName: document.getElementById('stu-name').value || 'Sem Nome',
                stuAge: document.getElementById('stu-age').value, stuWeight: document.getElementById('stu-weight').value,
                stuHeight: document.getElementById('stu-height').value, stuGender: document.getElementById('stu-gender').value,
                stuLevel: document.getElementById('stu-level').value, stuFreq: document.getElementById('stu-freq').value,
                stuObjective: document.getElementById('stu-objective').value, stuRecs: document.getElementById('stu-recs').value,
                health: Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value),
                workouts: window.state.builder.workouts
            };

            try {
                await addDoc(collection(db, "powfit_users", window.state.user.uid, "workouts"), workoutData);
                buildPrintHTML(workoutData);
                setTimeout(() => window.print(), 500);
            } catch(e) { console.error(e); alert("Erro ao salvar na nuvem."); }
            
            btn.innerHTML = '<i class="fas fa-save"></i> Salvar & Imprimir'; btn.disabled = false;
        };

        function buildPrintHTML(d) {
            let imcStr = "-";
            const w = parseFloat(d.stuWeight); const h = parseFloat(d.stuHeight);
            if(w>0 && h>0) imcStr = (w / (h * h)).toFixed(1);

            const isPEF = d.memberCategory === 'PEF';
            const healthSource = isPEF ? healthPEF : healthTE;
            const objRec = objData[d.stuObjective] || "";
            
            const legalTextPEF = `⚠️ OBSERVAÇÃO LEGAL – PROFISSIONAL DE EDUCAÇÃO FÍSICA<br>Conforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º da mesma lei estabelece que compete ao profissional de Educação Física coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos especializados nas áreas de atividades físicas e do desporto.`;
            const legalTextTE = `⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO<br>Conforme a Lei nº 14.597/2023 (Lei Geral do Esporte), Art. 75, a profissão de treinador esportivo é reconhecida e regulada no Brasil, com atuação de caráter técnico e esportivo voltada à preparação, supervisão, orientação e acompanhamento de treinos físicos e esportivos. A atuação possui finalidade orientativa e não substitui avaliação médica, diagnóstico clínico, prescrição medicamentosa ou acompanhamento de profissionais da saúde. Em casos de doenças, lesões, gestação, limitações físicas, uso de medicações ou qualquer condição específica de saúde, recomenda-se avaliação prévia por profissional habilitado antes do início ou continuidade da prática de exercícios. O treinamento proposto respeita os limites individuais, priorizando segurança, execução correta e evolução progressiva, dentro da atuação técnica e esportiva permitida por lei. As recomendações desta ficha possuem caráter informativo e orientativo, voltadas ao treinamento esportivo e prática de musculação.`;

            let html = `
                <div class="print-header">
                    <h1 class="print-title">Planilha de Treinamento - ${d.stuObjective}</h1>
                    <div class="prof-info">Prescrição: ${d.memberName} (${isPEF?'Profissional de Ed. Física':'Treinador Esportivo'}) ${isPEF?` | CREF: ${d.memberCref} - ${d.memberState}`:''}</div>
                    <div class="unit-info">${d.networkName} - Unidade: ${d.unitName}</div>
                </div>

                <div class="print-grid">
                    <div><strong>Aluno:</strong> ${d.studentName}</div><div><strong>Idade:</strong> ${d.stuAge||'-'}</div><div><strong>Gênero:</strong> ${d.stuGender}</div>
                    <div><strong>Peso:</strong> ${d.stuWeight||'-'} kg</div><div><strong>Altura:</strong> ${d.stuHeight||'-'} m</div><div><strong>IMC:</strong> ${imcStr}</div>
                    <div><strong>Nível:</strong> ${d.stuLevel}</div><div><strong>Frequência:</strong> ${d.stuFreq}</div><div><strong>Validade:</strong> ${d.validityDays} dias</div>
                </div>`;

            if (objRec || d.health.length > 0) {
                html += `<div class="print-guidelines"><h4>Diretrizes Automáticas de Perfil</h4><ul>`;
                if(objRec) html += `<li><strong>Objetivo (${d.stuObjective}):</strong> ${objRec}</li>`;
                d.health.forEach(k => { if(healthSource[k]) html += `<li><strong>Saúde (${k.replace(/[\u{1F300}-\u{1F9FF}\u{2600}-\u{26FF}\u{2700}-\u{27BF}\u{FE0F}]/gu,'').trim()}):</strong> ${healthSource[k]}</li>`; });
                html += `</ul></div>`;
            }

            d.workouts.forEach(work => {
                html += `<div class="print-workout"><h3>${work.title}</h3><table>
                            <thead><tr><th style="width: 35%">Exercício</th><th style="width: 8%; text-align:center;">Séries</th><th style="width: 12%; text-align:center;">Reps</th><th style="width: 15%">Técnica</th><th style="width: 30%">Observações</th></tr></thead>
                            <tbody>`;
                if(work.exercises.length===0) html += `<tr><td colspan="5" style="text-align:center; font-style:italic;">Sem exercícios</td></tr>`;
                else work.exercises.forEach(ex => {
                    html += `<tr><td><strong>${ex.name}</strong></td><td style="text-align:center;">${ex.sets}</td><td style="text-align:center;">${ex.reps}</td><td>${ex.technique==='Nenhuma'?'-':ex.technique}</td><td>${ex.obs||'-'}</td></tr>`;
                });
                html += `</tbody></table></div>`;
            });

            html += `<div class="print-footer-section">`;
            if(d.stuRecs.trim()) html += `<strong>Recomendações Específicas do Profissional:</strong><br><p style="white-space: pre-wrap; margin: 3px 0 10px 0;">${d.stuRecs}</p>`;
            html += `<div class="legal-text">${isPEF ? legalTextPEF : legalTextTE}</div>
                     <div style="text-align:center; margin-top: 15px; font-size: 8px;">Gerado em ${new Date(d.createdAt).toLocaleDateString('pt-BR')} via PowFit Pro Master</div></div>`;
            
            document.getElementById('print-area').innerHTML = html;
        }

        // --- NUVEM: HISTÓRICO E RELATÓRIO ---
        window.loadHistory = async () => {
            document.getElementById('history-table-body').innerHTML = `<tr><td colspan="5" class="p-4 text-center">Carregando da nuvem...</td></tr>`;
            try {
                const q = query(collection(db, "powfit_users", window.state.user.uid, "workouts"), orderBy("createdAt", "desc"));
                const querySnapshot = await getDocs(q);
                window.state.historyCache = [];
                querySnapshot.forEach((doc) => window.state.historyCache.push({ id: doc.id, ...doc.data() }));
                renderHistory();
            } catch (e) { console.error(e); document.getElementById('history-table-body').innerHTML = `<tr><td colspan="5" class="p-4 text-center text-red-500">Erro ao carregar histórico.</td></tr>`; }
        };

        window.renderHistory = () => {
            const search = document.getElementById('search-history').value.toLowerCase();
            const list = window.state.historyCache.filter(h => h.studentName.toLowerCase().includes(search));
            const tb = document.getElementById('history-table-body');
            
            if(list.length === 0) { tb.innerHTML = `<tr><td colspan="5" class="p-4 text-center opacity-50">Nenhuma ficha encontrada.</td></tr>`; return; }

            tb.innerHTML = list.map(h => {
                const dateObj = new Date(h.createdAt);
                const isExpired = Date.now() > (h.createdAt + (h.validityDays * 86400000));
                const statusHtml = isExpired ? `<span class="bg-red-500 bg-opacity-20 text-red-500 px-2 py-1 rounded text-[10px] font-bold border border-red-500">VENCIDA</span>` 
                                             : `<span class="bg-green-500 bg-opacity-20 text-green-500 px-2 py-1 rounded text-[10px] font-bold border border-green-500">ATIVA</span>`;
                
                return `
                <tr class="hover:bg-black hover:bg-opacity-10 transition">
                    <td class="p-3 whitespace-nowrap">${dateObj.toLocaleDateString('pt-BR')}</td>
                    <td class="p-3 font-bold">${h.studentName}</td>
                    <td class="p-3 text-xs opacity-80">${h.memberName} <br><span class="text-[9px] opacity-60">${h.unitName}</span></td>
                    <td class="p-3 text-center">${statusHtml}</td>
                    <td class="p-3 text-right whitespace-nowrap">
                        <button onclick="reprintHistory('${h.id}')" class="text-primary hover:text-white px-2 py-1 rounded" title="Imprimir Novamente"><i class="fas fa-print"></i></button>
                        <button onclick="deleteHistory('${h.id}')" class="text-red-500 hover:text-white px-2 py-1 rounded" title="Excluir"><i class="fas fa-trash"></i></button>
                    </td>
                </tr>`;
            }).join('');
        };

        window.reprintHistory = (id) => { const d = window.state.historyCache.find(x => x.id === id); if(d) { buildPrintHTML(d); setTimeout(() => window.print(), 200); } };
        window.deleteHistory = async (id) => {
            if(!confirm("Tem certeza que deseja excluir esta ficha permanentemente da nuvem?")) return;
            try { await deleteDoc(doc(db, "powfit_users", window.state.user.uid, "workouts", id)); loadHistory(); } 
            catch(e) { console.error(e); alert("Erro ao excluir."); }
        };

        window.generateReport = () => {
            if(window.state.historyCache.length === 0) return alert("Não há dados de fichas para gerar relatório.");
            
            // Filtra fichas do mês atual
            const now = new Date();
            const currMonth = now.getMonth(); const currYear = now.getFullYear();
            const thisMonthFichas = window.state.historyCache.filter(h => { const d = new Date(h.createdAt); return d.getMonth()===currMonth && d.getFullYear()===currYear; });
            
            // Agrupa por Unidade -> Membro
            const reportMap = {};
            thisMonthFichas.forEach(f => {
                if(!reportMap[f.unitName]) reportMap[f.unitName] = { total: 0, members: {} };
                reportMap[f.unitName].total++;
                if(!reportMap[f.unitName].members[f.memberName]) reportMap[f.unitName].members[f.memberName] = 0;
                reportMap[f.unitName].members[f.memberName]++;
            });

            let html = `
                <div class="report-header"><h2>Relatório de Produtividade - ${now.toLocaleDateString('pt-BR', {month:'long', year:'numeric'}).toUpperCase()}</h2></div>
                <table class="report-table"><thead><tr><th style="text-align:left;">Unidade / Equipe</th><th style="width:20%; text-align:center;">Fichas Criadas</th></tr></thead><tbody>
            `;
            
            Object.keys(reportMap).forEach(uName => {
                html += `<tr><td style="background:#eee; font-weight:bold;">📍 Unidade: ${uName}</td><td style="background:#eee; text-align:center; font-weight:bold;">${reportMap[uName].total}</td></tr>`;
                Object.keys(reportMap[uName].members).forEach(mName => {
                    html += `<tr><td style="padding-left: 20px;">👤 ${mName}</td><td style="text-align:center;">${reportMap[uName].members[mName]}</td></tr>`;
                });
            });
            
            html += `<tr><td style="text-align:right; font-weight:bold;">TOTAL GERAL DA REDE:</td><td style="text-align:center; font-weight:bold; font-size:14px;">${thisMonthFichas.length}</td></tr>`;
            html += `</tbody></table><div style="text-align:center; margin-top:20px; font-size:10px;">PowFit Pro Master Edition - Impresso em ${now.toLocaleDateString('pt-BR')}</div>`;

            document.getElementById('print-area').innerHTML = html;
            setTimeout(() => window.print(), 200);
        };

        // Initialize Builder Default
        window.addWorkoutDay();

    </script>
</body>
</html>
