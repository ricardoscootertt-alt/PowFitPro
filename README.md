<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro - Gestão Master</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    
    <style>
        :root {
            /* Tema Masculino (Escuro Fitness) */
            --bg-body: #0f172a; --bg-card: #1e293b; --text-main: #f8fafc; --text-muted: #94a3b8;
            --border-color: #334155; --primary: #3b82f6; --primary-hover: #2563eb;
            --input-bg: #0f172a; --input-border: #475569;
        }

        [data-theme="Feminino"] {
            /* Tema Feminino (Rosa Moderno Elegante) */
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

        /* Esconder áreas que não são de impressão */
        #print-area { display: none; }

        @media print {
            body { background: white !important; color: black !important; margin: 0; padding: 0; font-family: 'Arial', sans-serif; font-size: 10px; }
            #app-container, .modal, .no-print { display: none !important; }
            #print-area { display: block !important; padding: 10mm; }
            @page { size: A4; margin: 0; }
            
            .print-header { border-bottom: 2px solid #000; padding-bottom: 5px; margin-bottom: 10px; text-align: center; }
            .print-title { font-size: 16px; font-weight: bold; margin: 0; text-transform: uppercase; }
            .prof-info { font-size: 11px; margin-top: 3px; font-weight: bold; }
            
            .print-grid { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 4px; margin-bottom: 10px; border: 1px solid #000; padding: 5px; }
            .print-grid div { font-size: 10px; }
            
            .print-guidelines { border: 1px solid #000; padding: 5px; margin-bottom: 10px; font-size: 9px; }
            .print-guidelines h4 { margin: 0 0 3px 0; font-size: 10px; background: #eee; padding: 2px; text-transform: uppercase; }
            .print-guidelines ul { margin: 0; padding-left: 15px; }
            
            .print-workout { margin-bottom: 15px; page-break-inside: avoid; }
            .print-workout h3 { background: #d1d5db; border: 1px solid #000; border-bottom: none; padding: 3px 5px; margin: 0; font-size: 11px; text-transform: uppercase; -webkit-print-color-adjust: exact; }
            table { width: 100%; border-collapse: collapse; border: 1px solid #000; }
            th, td { border: 1px solid #000; padding: 3px 5px; text-align: left; font-size: 9px; vertical-align: middle; }
            th { background-color: #f3f4f6; font-weight: bold; text-transform: uppercase; -webkit-print-color-adjust: exact; }
            .ex-img { max-width: 40px; max-height: 40px; border-radius: 4px; display: block; margin: 0 auto; }
            
            .legal-section { margin-top: 15px; border: 1px solid #000; padding: 5px; font-size: 8px; page-break-inside: avoid; text-align: justify; font-style: italic; }
        }

        /* Scrollbar */
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
    </style>
</head>
<body data-theme="Masculino" class="min-h-screen">

    <!-- LOGIN SCREEN -->
    <div id="login-screen" class="fixed inset-0 z-50 flex items-center justify-center bg-gray-900 bg-opacity-90 backdrop-blur-sm">
        <div class="bg-white text-gray-900 p-8 rounded-2xl shadow-2xl max-w-md w-full text-center">
            <i class="fas fa-dumbbell text-5xl text-blue-600 mb-4"></i>
            <h1 class="text-3xl font-bold mb-2">PowFit Pro Master</h1>
            <p class="text-sm text-gray-500 mb-8">Plataforma de Gestão de Franquias e Prescrição Profissional.</p>
            <button onclick="loginWithGoogle()" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-4 rounded-xl flex items-center justify-center gap-3 transition">
                <i class="fab fa-google text-xl"></i> Entrar com o Google
            </button>
        </div>
    </div>

    <!-- MAIN APP CONTAINER -->
    <div id="app-container" class="hidden min-h-screen flex flex-col">
        
        <!-- Navbar -->
        <nav class="bg-gray-900 text-white shadow-md p-4 border-b border-gray-800">
            <div class="max-w-7xl mx-auto flex flex-wrap justify-between items-center gap-4">
                <div class="flex items-center gap-3">
                    <i class="fas fa-dumbbell text-2xl text-blue-500"></i>
                    <span class="text-xl font-bold tracking-tight">PowFit Pro</span>
                </div>
                <div class="flex gap-2 sm:gap-4 overflow-x-auto text-sm font-medium">
                    <button onclick="switchTab('prescricao')" id="tab-prescricao" class="px-3 py-2 rounded-lg bg-blue-600 text-white transition">Nova Ficha</button>
                    <button onclick="switchTab('rede')" id="tab-rede" class="px-3 py-2 rounded-lg hover:bg-gray-800 transition">Rede & Equipe</button>
                    <button onclick="switchTab('relatorios')" id="tab-relatorios" class="px-3 py-2 rounded-lg hover:bg-gray-800 transition">Produtividade</button>
                    <button onclick="switchTab('historico')" id="tab-historico" class="px-3 py-2 rounded-lg hover:bg-gray-800 transition">Histórico em Nuvem</button>
                </div>
                <div class="flex items-center gap-3">
                    <img id="user-avatar" src="" class="w-8 h-8 rounded-full border border-gray-600">
                    <button onclick="logout()" class="text-sm text-red-400 hover:text-red-300"><i class="fas fa-sign-out-alt"></i> Sair</button>
                </div>
            </div>
        </nav>

        <!-- VIEW: PRESCRIÇÃO -->
        <div id="view-prescricao" class="flex-1 max-w-7xl mx-auto w-full p-4 sm:p-6 lg:p-8">
            <div class="flex justify-between items-center mb-6">
                <h2 class="text-2xl font-bold">Montagem de Ficha</h2>
                <button onclick="saveAndPrintWorkout()" class="btn-primary px-6 py-2.5 rounded-lg font-medium shadow-lg flex items-center gap-2">
                    <i class="fas fa-cloud-upload-alt"></i> Salvar na Nuvem & Imprimir
                </button>
            </div>

            <div class="grid grid-cols-1 xl:grid-cols-12 gap-6">
                <!-- Coluna Esquerda: Configurações -->
                <div class="xl:col-span-4 space-y-6">
                    
                    <!-- Profissional Seleção -->
                    <div class="card rounded-xl p-5 shadow-sm">
                        <h3 class="text-md font-semibold mb-3 border-b border-opacity-20 pb-2 flex items-center gap-2" style="border-color: var(--border-color)"><i class="fas fa-id-card text-primary"></i> Profissional Responsável</h3>
                        <select id="sel-member" class="input-field w-full rounded-lg px-3 py-2 text-sm" onchange="updateHealthOptionsBasedOnMember()">
                            <option value="">Carregando equipe...</option>
                        </select>
                    </div>

                    <!-- Dados do Aluno -->
                    <div class="card rounded-xl p-5 shadow-sm">
                        <div class="flex justify-between items-center mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                            <h3 class="text-md font-semibold flex items-center gap-2"><i class="fas fa-user text-primary"></i> Dados do Aluno</h3>
                            <div id="imc-display"></div>
                        </div>
                        <div class="space-y-3">
                            <input type="text" id="stu-name" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Nome do aluno">
                            <div class="grid grid-cols-3 gap-2">
                                <input type="number" id="stu-age" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Idade">
                                <input type="number" id="stu-weight" oninput="calculateIMC()" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Peso (kg)">
                                <input type="number" id="stu-height" step="0.01" oninput="calculateIMC()" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Alt (m)">
                            </div>
                            <div class="grid grid-cols-2 gap-2">
                                <select id="stu-gender" onchange="changeTheme()" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                    <option value="Masculino">Masculino</option>
                                    <option value="Feminino">Feminino</option>
                                </select>
                                <select id="stu-level" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                    <option>Iniciante</option><option>Intermediário</option><option>Avançado</option>
                                </select>
                            </div>
                            <select id="stu-objective" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                <option>Emagrecimento</option><option>Hipertrofia</option><option>Definição</option>
                                <option>Condicionamento</option><option>Resistência</option><option>Força</option>
                                <option>Reabilitação</option><option>Saúde geral</option>
                            </select>
                            <select id="stu-freq" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                <option>3 dias</option><option>5 dias</option><option>6 dias</option><option>Personalizado</option>
                            </select>
                        </div>
                    </div>

                    <!-- Estado de Saúde -->
                    <div class="card rounded-xl p-5 shadow-sm">
                        <h3 class="text-md font-semibold mb-1 border-b border-opacity-20 pb-2 flex items-center gap-2" style="border-color: var(--border-color)"><i class="fas fa-heartbeat text-primary"></i> Estado de Saúde</h3>
                        <p class="text-[10px] opacity-70 mb-2">Diretrizes automáticas variam conforme o Profissional selecionado.</p>
                        <div class="grid grid-cols-2 gap-1 text-[11px]" id="health-container"></div>
                    </div>
                    
                </div>

                <!-- Coluna Direita: Treinos -->
                <div class="xl:col-span-8 space-y-6">
                    <div class="flex justify-between items-center bg-opacity-10 p-3 rounded-xl card border-dashed border-2">
                        <div class="flex gap-2 items-center w-full">
                            <i class="fas fa-list text-primary text-xl"></i>
                            <select id="stu-validity" class="input-field rounded px-2 py-1 text-xs">
                                <option>Validade: 15 dias</option><option selected>Validade: 30 dias</option><option>Validade: 60 dias</option><option>Validade: 90 dias</option>
                            </select>
                        </div>
                        <button onclick="addWorkoutDay()" class="btn-primary px-4 py-2 rounded-lg text-xs font-bold whitespace-nowrap"><i class="fas fa-plus"></i> ADD DIA</button>
                    </div>

                    <div id="workouts-container" class="space-y-4"></div>

                    <!-- Recomendações -->
                    <div class="card rounded-xl p-4 shadow-sm">
                        <h3 class="text-sm font-semibold mb-2"><i class="fas fa-pen text-primary"></i> Recomendações Livres</h3>
                        <textarea id="stu-recs" rows="3" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Hidratação, descanso..."></textarea>
                    </div>
                </div>
            </div>
        </div>

        <!-- VIEW: REDE E EQUIPE -->
        <div id="view-rede" class="hidden flex-1 max-w-7xl mx-auto w-full p-4 sm:p-6 lg:p-8 space-y-6">
            <h2 class="text-2xl font-bold border-b border-opacity-20 pb-4" style="border-color: var(--border-color)">Gestão da Rede e Unidades</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <!-- Cadastro de Unidade -->
                <div class="card p-5 rounded-xl">
                    <h3 class="font-bold mb-3">Adicionar Unidade</h3>
                    <div class="flex gap-2">
                        <input type="text" id="new-unit-name" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="Nome da Unidade (Ex: Matriz)">
                        <button onclick="addUnit()" class="btn-primary px-4 rounded font-bold"><i class="fas fa-plus"></i></button>
                    </div>
                    <ul id="units-list" class="mt-4 space-y-2 text-sm"></ul>
                </div>
                <!-- Cadastro de Membro -->
                <div class="card p-5 rounded-xl space-y-3">
                    <h3 class="font-bold border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">Adicionar Membro à Equipe</h3>
                    <input type="text" id="mem-name" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="Nome Completo">
                    <div class="grid grid-cols-2 gap-2">
                        <input type="text" id="mem-cpf" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="CPF">
                        <select id="mem-unit" class="input-field w-full rounded px-3 py-2 text-sm"><option value="">Selecione Unidade</option></select>
                    </div>
                    <div class="grid grid-cols-2 gap-2">
                        <select id="mem-role" class="input-field w-full rounded px-3 py-2 text-sm" onchange="toggleCrefField()">
                            <option value="PEF">Profissional de Ed. Física</option>
                            <option value="TE">Treinador Esportivo</option>
                        </select>
                        <input type="text" id="mem-state" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="Estado (UF)">
                    </div>
                    <input type="text" id="mem-cref" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="CREF (Obrigatório para PEF)">
                    <button onclick="addMember()" class="w-full btn-primary py-2 rounded font-bold"><i class="fas fa-user-plus"></i> Salvar Membro</button>
                </div>
            </div>
            <!-- Lista de Membros -->
            <div class="card p-5 rounded-xl">
                <h3 class="font-bold mb-3">Equipe Cadastrada</h3>
                <div class="overflow-x-auto">
                    <table class="w-full text-sm text-left">
                        <thead class="text-xs uppercase bg-black bg-opacity-10">
                            <tr><th class="px-4 py-2">Nome</th><th class="px-4 py-2">Categoria</th><th class="px-4 py-2">Unidade</th><th class="px-4 py-2">Ação</th></tr>
                        </thead>
                        <tbody id="members-table-body"></tbody>
                    </table>
                </div>
            </div>
        </div>

        <!-- VIEW: RELATÓRIOS -->
        <div id="view-relatorios" class="hidden flex-1 max-w-7xl mx-auto w-full p-4 sm:p-6 lg:p-8 space-y-6">
            <div class="flex justify-between items-center border-b border-opacity-20 pb-4" style="border-color: var(--border-color)">
                <h2 class="text-2xl font-bold">Relatório de Produtividade</h2>
                <button onclick="printReport()" class="bg-green-600 hover:bg-green-500 text-white px-4 py-2 rounded shadow font-bold"><i class="fas fa-print"></i> Imprimir Relatório</button>
            </div>
            <div class="flex gap-4 items-center">
                <input type="month" id="report-month" class="input-field rounded px-3 py-2 text-sm">
                <button onclick="generateReport()" class="btn-primary px-4 py-2 rounded font-bold">Gerar</button>
            </div>
            <div id="report-content" class="card p-5 rounded-xl min-h-[300px]">
                <p class="text-center opacity-50 mt-10">Selecione o mês e clique em Gerar.</p>
            </div>
        </div>

        <!-- VIEW: HISTÓRICO -->
        <div id="view-historico" class="hidden flex-1 max-w-7xl mx-auto w-full p-4 sm:p-6 lg:p-8 space-y-6">
            <h2 class="text-2xl font-bold border-b border-opacity-20 pb-4" style="border-color: var(--border-color)">Histórico de Fichas na Nuvem</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4" id="cloud-history-list">
                <p class="opacity-50">Carregando histórico...</p>
            </div>
        </div>

    </div>

    <!-- MODAL DE EXERCÍCIOS -->
    <div id="exercise-modal" class="fixed inset-0 bg-black bg-opacity-70 backdrop-blur-sm hidden z-[60] flex items-center justify-center p-2 sm:p-4">
        <div class="card w-full max-w-5xl rounded-xl shadow-2xl flex flex-col h-[90vh]">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold"><i class="fas fa-search text-primary mr-2"></i> Adicionar Exercício</h3>
                <button onclick="closeModal()" class="text-gray-400 hover:text-red-500 transition-colors text-2xl leading-none">&times;</button>
            </div>
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <div class="w-full md:w-1/4 border-r overflow-y-auto p-2 space-y-1" style="border-color: var(--border-color)" id="modal-categories"></div>
                <div class="w-full md:w-3/4 overflow-y-auto p-3 bg-black bg-opacity-5" id="modal-exercises"></div>
            </div>
        </div>
    </div>

    <!-- ÁREA DE IMPRESSÃO (Oculta na tela) -->
    <div id="print-area"></div>


    <!-- FIREBASE MODULES E LÓGICA -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-app.js";
        import { getAuth, signInWithPopup, GoogleAuthProvider, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, addDoc, query, where, getDocs, orderBy, deleteDoc } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-firestore.js";

        // CONFIGURAÇÃO DO USUÁRIO
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

        // VARIÁVEIS GLOBAIS
        let currentUser = null;
        let globalUnits = [];
        let globalMembers = [];
        
        let state = {
            workouts: [],
            activeModalWorkoutId: null,
            activeCategory: "🔥 PEITO",
            currentWorkoutId: null // Se estiver editando do histórico
        };

        const daysOfWeek = ["SEGUNDA-FEIRA", "TERÇA-FEIRA", "QUARTA-FEIRA", "QUINTA-FEIRA", "SEXTA-FEIRA", "SÁBADO", "DOMINGO"];

        // BANCO DE DADOS FIXO
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

        // ---------------- AUTHENTICATION ----------------
        window.loginWithGoogle = () => {
            const provider = new GoogleAuthProvider();
            signInWithPopup(auth, provider).catch(error => alert("Erro ao fazer login: " + error.message));
        };

        window.logout = () => {
            signOut(auth).then(() => {
                document.getElementById('login-screen').classList.remove('hidden');
                document.getElementById('app-container').classList.add('hidden');
            });
        };

        onAuthStateChanged(auth, (user) => {
            if (user) {
                currentUser = user;
                document.getElementById('user-avatar').src = user.photoURL || '';
                document.getElementById('login-screen').classList.add('hidden');
                document.getElementById('app-container').classList.remove('hidden');
                initApp();
            } else {
                document.getElementById('login-screen').classList.remove('hidden');
                document.getElementById('app-container').classList.add('hidden');
            }
        });

        async function initApp() {
            await loadNetworkData();
            if(state.workouts.length === 0) addWorkoutDay();
            document.getElementById('report-month').value = new Date().toISOString().slice(0,7);
            switchTab('prescricao');
        }

        window.switchTab = (tabId) => {
            ['prescricao', 'rede', 'relatorios', 'historico'].forEach(id => {
                document.getElementById(`view-${id}`).classList.add('hidden');
                document.getElementById(`tab-${id}`).classList.remove('bg-blue-600', 'text-white');
                document.getElementById(`tab-${id}`).classList.add('hover:bg-gray-800');
            });
            document.getElementById(`view-${tabId}`).classList.remove('hidden');
            document.getElementById(`tab-${tabId}`).classList.add('bg-blue-600', 'text-white');
            document.getElementById(`tab-${tabId}`).classList.remove('hover:bg-gray-800');
            
            if(tabId === 'historico') loadCloudHistory();
        };

        // ---------------- GESTÃO DE REDE (FIRESTORE) ----------------
        async function loadNetworkData() {
            try {
                // Carregar Unidades
                const unitsSnap = await getDocs(collection(db, "powfit_units"));
                globalUnits = unitsSnap.docs.map(d => ({ id: d.id, ...d.data() }));
                
                // Carregar Membros
                const membersSnap = await getDocs(collection(db, "powfit_members"));
                globalMembers = membersSnap.docs.map(d => ({ id: d.id, ...d.data() }));

                renderNetworkUI();
                renderMemberSelect();
            } catch (e) {
                console.error("Erro ao carregar dados da rede", e);
            }
        }

        window.addUnit = async () => {
            const name = document.getElementById('new-unit-name').value;
            if(!name) return;
            await addDoc(collection(db, "powfit_units"), { name: name, createdAt: new Date() });
            document.getElementById('new-unit-name').value = '';
            loadNetworkData();
        };

        window.toggleCrefField = () => {
            const role = document.getElementById('mem-role').value;
            document.getElementById('mem-cref').style.display = role === 'TE' ? 'none' : 'block';
        };

        window.addMember = async () => {
            const name = document.getElementById('mem-name').value;
            const cpf = document.getElementById('mem-cpf').value;
            const unit = document.getElementById('mem-unit').value;
            const role = document.getElementById('mem-role').value;
            const stateUf = document.getElementById('mem-state').value;
            const cref = document.getElementById('mem-cref').value;
            
            if(!name || !unit) return alert("Nome e Unidade são obrigatórios");
            
            await addDoc(collection(db, "powfit_members"), {
                name, cpf, unitId: unit, role, state: stateUf, cref: role === 'PEF' ? cref : '', createdAt: new Date()
            });
            
            document.getElementById('mem-name').value = '';
            document.getElementById('mem-cpf').value = '';
            loadNetworkData();
        };

        function renderNetworkUI() {
            // Dropdowns de unidade
            const unitSelect = document.getElementById('mem-unit');
            unitSelect.innerHTML = `<option value="">Selecione Unidade</option>` + globalUnits.map(u => `<option value="${u.id}">${u.name}</option>`).join('');
            
            // Lista de unidades
            document.getElementById('units-list').innerHTML = globalUnits.map(u => `<li><i class="fas fa-building text-blue-400"></i> ${u.name}</li>`).join('');

            // Tabela de membros
            document.getElementById('members-table-body').innerHTML = globalMembers.map(m => {
                const unitName = globalUnits.find(u => u.id === m.unitId)?.name || 'Sem Unidade';
                const roleStr = m.role === 'PEF' ? 'Ed. Física' : 'Tr. Esportivo';
                return `<tr class="border-b border-gray-800"><td class="px-4 py-2">${m.name}</td><td class="px-4 py-2">${roleStr}</td><td class="px-4 py-2">${unitName}</td><td class="px-4 py-2"><button onclick="deleteMember('${m.id}')" class="text-red-500"><i class="fas fa-trash"></i></button></td></tr>`;
            }).join('');
        }

        window.deleteMember = async (id) => {
            if(confirm("Excluir membro?")) {
                await deleteDoc(doc(db, "powfit_members", id));
                loadNetworkData();
            }
        };

        function renderMemberSelect() {
            const sel = document.getElementById('sel-member');
            sel.innerHTML = `<option value="">Selecione o Profissional para Assinar a Ficha</option>` + 
                globalMembers.map(m => `<option value="${m.id}">${m.name} (${m.role === 'PEF' ? 'Ed. Física' : 'Tr. Esportivo'})</option>`).join('');
            updateHealthOptionsBasedOnMember();
        }

        // ---------------- LÓGICA DE PRESCRIÇÃO E INTERFACE ----------------
        window.calculateIMC = () => {
            const w = parseFloat(document.getElementById('stu-weight').value);
            const h = parseFloat(document.getElementById('stu-height').value);
            const display = document.getElementById('imc-display');
            if (w > 0 && h > 0) {
                const imc = (w / (h * h)).toFixed(1);
                let cls = imc < 18.5 ? "Abaixo" : imc < 24.9 ? "Normal" : imc < 29.9 ? "Sobrepeso" : "Obesidade";
                display.innerHTML = `<span class="bg-primary bg-opacity-20 text-primary px-3 py-1 rounded-full text-xs font-bold border border-primary border-opacity-30">IMC: ${imc} (${cls})</span>`;
            } else display.innerHTML = '';
        };

        window.changeTheme = () => document.body.setAttribute('data-theme', document.getElementById('stu-gender').value);

        window.updateHealthOptionsBasedOnMember = () => {
            const memberId = document.getElementById('sel-member').value;
            const member = globalMembers.find(m => m.id === memberId);
            const healthObj = (member && member.role === 'TE') ? healthDataTE : healthDataPEF;
            
            const container = document.getElementById('health-container');
            const selected = Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value);
            
            container.innerHTML = Object.keys(healthObj).map(opt => `
                <label class="flex items-start space-x-1.5 cursor-pointer p-1 rounded hover:bg-black hover:bg-opacity-10 transition">
                    <input type="checkbox" value="${opt}" ${selected.includes(opt) ? 'checked' : ''} class="health-cb mt-0.5 w-3 h-3 text-primary">
                    <span class="font-medium">${opt}</span>
                </label>`
            ).join('');
        };

        // --- MONTAGEM DE EXERCÍCIOS ---
        const generateId = () => Math.random().toString(36).substr(2, 9);

        window.addWorkoutDay = () => {
            const count = state.workouts.length;
            const title = count < 7 ? `TREINO ${daysOfWeek[count]}` : `NOVO TREINO ${count + 1}`;
            state.workouts.push({ id: generateId(), title: title, exercises: [] });
            renderWorkouts();
        };

        window.removeWorkout = (id) => {
            if(confirm("Excluir este dia?")) { state.workouts = state.workouts.filter(w => w.id !== id); renderWorkouts(); }
        };

        window.duplicateWorkout = (id) => {
            const w = state.workouts.find(w => w.id === id);
            if(w) { state.workouts.push({ ...JSON.parse(JSON.stringify(w)), id: generateId(), title: w.title + " (Cópia)" }); renderWorkouts(); }
        };

        window.updateWorkoutTitle = (id, val) => { const w = state.workouts.find(w => w.id === id); if(w) w.title = val; };
        window.removeExercise = (wId, idx) => { const w = state.workouts.find(w => w.id === wId); w.exercises.splice(idx, 1); renderWorkouts(); };
        window.updateExercise = (wId, idx, f, v) => { const w = state.workouts.find(w => w.id === wId); w.exercises[idx][f] = v; };
        
        window.moveExercise = (wId, idx, dir) => {
            const w = state.workouts.find(w => w.id === wId);
            if(dir === 'up' && idx > 0) [w.exercises[idx], w.exercises[idx-1]] = [w.exercises[idx-1], w.exercises[idx]];
            if(dir === 'down' && idx < w.exercises.length - 1) [w.exercises[idx], w.exercises[idx+1]] = [w.exercises[idx+1], w.exercises[idx]];
            renderWorkouts();
        };

        // Processamento Local de Imagem (Canvas Resize -> Base64) para não pesar o banco
        window.handleImageUpload = (wId, exIdx, inputElem) => {
            const file = inputElem.files[0];
            if(!file) return;
            const reader = new FileReader();
            reader.onload = (e) => {
                const img = new Image();
                img.onload = () => {
                    const canvas = document.createElement('canvas');
                    const ctx = canvas.getContext('2d');
                    const MAX_WIDTH = 100; // Thumbnail size
                    const scale = MAX_WIDTH / img.width;
                    canvas.width = MAX_WIDTH;
                    canvas.height = img.height * scale;
                    ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
                    const base64Str = canvas.toDataURL('image/jpeg', 0.6); // Compressão
                    updateExercise(wId, exIdx, 'image', base64Str);
                    renderWorkouts();
                };
                img.src = e.target.result;
            };
            reader.readAsDataURL(file);
        };

        function renderWorkouts() {
            const container = document.getElementById('workouts-container');
            container.innerHTML = state.workouts.map(w => `
                <div class="card rounded-xl overflow-hidden shadow-sm">
                    <div class="p-3 border-b flex justify-between items-center bg-black bg-opacity-5" style="border-color: var(--border-color);">
                        <input type="text" value="${w.title}" onchange="updateWorkoutTitle('${w.id}', this.value)" class="input-field bg-transparent font-bold text-sm w-1/2 px-2 py-1 rounded uppercase">
                        <div class="flex gap-1">
                            <button onclick="duplicateWorkout('${w.id}')" class="text-xs p-1.5 rounded hover:bg-black hover:bg-opacity-10 transition"><i class="fas fa-copy"></i></button>
                            <button onclick="removeWorkout('${w.id}')" class="text-xs p-1.5 text-red-500 rounded hover:bg-red-500 hover:text-white transition"><i class="fas fa-trash"></i></button>
                        </div>
                    </div>
                    <div class="p-0">
                        ${w.exercises.length === 0 ? `<div class="text-center p-3 text-xs opacity-50 italic">Sem exercícios.</div>` : 
                        w.exercises.map((ex, idx) => `
                            <div class="flex flex-col sm:flex-row gap-2 p-2 items-start sm:items-center border-b last:border-0 border-opacity-10" style="border-color: var(--border-color)">
                                <div class="flex flex-col gap-1 hidden sm:flex">
                                    <button onclick="moveExercise('${w.id}', ${idx}, 'up')" class="text-[10px] p-0.5 rounded hover:bg-black hover:bg-opacity-10"><i class="fas fa-chevron-up"></i></button>
                                    <button onclick="moveExercise('${w.id}', ${idx}, 'down')" class="text-[10px] p-0.5 rounded hover:bg-black hover:bg-opacity-10"><i class="fas fa-chevron-down"></i></button>
                                </div>
                                <div class="w-12 h-12 bg-black bg-opacity-10 rounded flex items-center justify-center overflow-hidden flex-shrink-0 cursor-pointer relative group" onclick="document.getElementById('img-up-${w.id}-${idx}').click()">
                                    ${ex.image ? `<img src="${ex.image}" class="w-full h-full object-cover">` : `<i class="fas fa-image opacity-30"></i>`}
                                    <div class="absolute inset-0 bg-black bg-opacity-50 hidden group-hover:flex items-center justify-center text-white text-[10px]">IMG</div>
                                    <input type="file" id="img-up-${w.id}-${idx}" accept="image/*" class="hidden" onchange="handleImageUpload('${w.id}', ${idx}, this)">
                                </div>
                                <div class="flex-1 font-medium w-full sm:w-auto">
                                    <div class="text-[9px] opacity-60 uppercase tracking-wider">${ex.category}</div>
                                    <div class="text-xs">${ex.name}</div>
                                </div>
                                <div class="flex flex-wrap gap-1 w-full sm:w-auto items-center">
                                    <input type="text" value="${ex.sets}" onchange="updateExercise('${w.id}', ${idx}, 'sets', this.value)" class="input-field w-10 rounded px-1 py-1 text-xs text-center" placeholder="Séries">
                                    <span class="opacity-50 text-xs">x</span>
                                    <input type="text" value="${ex.reps}" onchange="updateExercise('${w.id}', ${idx}, 'reps', this.value)" class="input-field w-14 rounded px-1 py-1 text-xs text-center" placeholder="Reps">
                                    <select onchange="updateExercise('${w.id}', ${idx}, 'technique', this.value)" class="input-field w-24 rounded px-1 py-1 text-[10px]">
                                        ${dbTechniques.map(t => `<option value="${t}" ${ex.technique === t ? 'selected' : ''}>${t}</option>`).join('')}
                                    </select>
                                    <input type="text" value="${ex.obs}" onchange="updateExercise('${w.id}', ${idx}, 'obs', this.value)" class="input-field flex-1 sm:w-32 rounded px-1 py-1 text-xs" placeholder="Obs...">
                                    <button onclick="removeExercise('${w.id}', ${idx})" class="text-red-500 hover:text-red-700 p-1 rounded ml-1"><i class="fas fa-trash"></i></button>
                                </div>
                            </div>
                        `).join('')}
                    </div>
                    <div class="p-2 border-t" style="border-color: var(--border-color)">
                        <button onclick="openModal('${w.id}')" class="w-full text-primary py-1 rounded-lg text-xs font-semibold hover:bg-primary hover:bg-opacity-10 transition">+ ADICIONAR EXERCÍCIO</button>
                    </div>
                </div>
            `).join('');
        }

        // --- MODAL DE EXERCÍCIOS ---
        window.openModal = (id) => { state.activeModalWorkoutId = id; document.getElementById('exercise-modal').classList.remove('hidden'); renderModalCats(); renderModalEx(); };
        window.closeModal = () => document.getElementById('exercise-modal').classList.add('hidden');
        window.setModalCategory = (c) => { state.activeCategory = c; renderModalCats(); renderModalEx(); };

        function renderModalCats() {
            document.getElementById('modal-categories').innerHTML = Object.keys(dbCategories).map(cat => `
                <button onclick="setModalCategory('${cat}')" class="w-full text-left px-2 py-2 rounded-lg text-xs font-medium transition ${state.activeCategory === cat ? 'bg-primary text-white' : 'hover:bg-black hover:bg-opacity-10'}">${cat}</button>
            `).join('');
        }
        function renderModalEx() {
            const isCardio = state.activeCategory === "🫀 CARDIO";
            document.getElementById('modal-exercises').innerHTML = `<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-2">` + 
                dbCategories[state.activeCategory].map(ex => `<button onclick="addExerciseToWorkout('${ex}', ${isCardio})" class="card p-2 rounded-lg text-left text-xs font-medium hover:border-primary transition flex justify-between group"><span>${ex}</span><i class="fas fa-plus opacity-0 group-hover:opacity-100 text-primary"></i></button>`).join('') + `</div>`;
        }
        window.addExerciseToWorkout = (name, isCardio) => {
            const w = state.workouts.find(w => w.id === state.activeModalWorkoutId);
            if(w) { w.exercises.push({ category: state.activeCategory, name: name, sets: isCardio ? '1' : '3', reps: isCardio ? '-' : '10-12', technique: 'Nenhuma', obs: '', image: null }); renderWorkouts(); }
        };


        // ---------------- SALVAR, IMPRIMIR E RELATÓRIOS ----------------
        function getFormData() {
            const memberId = document.getElementById('sel-member').value;
            const member = globalMembers.find(m => m.id === memberId);
            if(!member) throw new Error("Selecione o Profissional Responsável.");
            const unitName = globalUnits.find(u => u.id === member.unitId)?.name || '';

            return {
                memberId: member.id,
                memberName: member.name,
                memberRole: member.role,
                memberCref: member.cref || '',
                memberState: member.state || '',
                unitName: unitName,
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
                health: Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value),
                workouts: state.workouts,
                timestamp: new Date(),
                monthKey: new Date().toISOString().slice(0,7) // YYYY-MM para relatórios rápidos
            };
        }

        window.saveAndPrintWorkout = async () => {
            try {
                const data = getFormData();
                
                // Salvar na Nuvem (Firestore)
                if(state.currentWorkoutId) {
                    await setDoc(doc(db, "powfit_workouts", state.currentWorkoutId), data);
                } else {
                    const docRef = await addDoc(collection(db, "powfit_workouts"), data);
                    state.currentWorkoutId = docRef.id;
                }

                // Chamar Impressão (gera HTML e aciona window.print)
                generatePrintHTML(data);
                
            } catch(e) {
                alert(e.message);
            }
        };

        function generatePrintHTML(d) {
            let imcStr = "-";
            const w = parseFloat(d.stuWeight); const h = parseFloat(d.stuHeight);
            if(w>0 && h>0) imcStr = (w / (h * h)).toFixed(1);

            const isPEF = d.memberRole === 'PEF';
            const healthSource = isPEF ? healthDataPEF : healthDataTE;
            const objTxt = objectiveData[d.stuObjective] || "";
            
            const legalPEF = `⚠️ OBSERVAÇÃO LEGAL – PROFISSIONAL DE EDUCAÇÃO FÍSICA: Conforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º estabelece que compete ao profissional coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos.`;
            const legalTE = `⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO: Conforme a Lei nº 14.597/2023, Art. 75, a profissão de treinador esportivo é reconhecida no Brasil, com atuação voltada à preparação e acompanhamento de treinos. A atuação possui finalidade orientativa e não substitui avaliação médica. Em casos de doenças ou lesões, recomenda-se avaliação médica prévia. O treinamento respeita limites individuais e a segurança.`;

            let html = `
                <div class="print-header">
                    <h1 class="print-title">Planilha de Treinamento - ${d.stuObjective}</h1>
                    <div class="prof-info">
                        Prescrição feita por: ${d.memberName} (${isPEF ? 'Profissional de Educação Física' : 'Treinador Esportivo'})<br>
                        ${isPEF ? `CREF: ${d.memberCref} - Estado: ${d.memberState}` : `Unidade: ${d.unitName}`}
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

            if (objTxt || d.health.length > 0) {
                html += `<div class="print-guidelines"><h4>Diretrizes Automáticas de Perfil</h4><ul>`;
                if(objTxt) html += `<li><strong>Objetivo (${d.stuObjective}):</strong> ${objTxt}</li>`;
                d.health.forEach(k => { if(healthSource[k]) html += `<li><strong>Saúde (${k.replace(/[\u{1F300}-\u{1F9FF}\u{2600}-\u{26FF}\u{2700}-\u{27BF}\u{FE0F}]/gu, '').trim()}):</strong> ${healthSource[k]}</li>`; });
                html += `</ul></div>`;
            }

            d.workouts.forEach(work => {
                html += `<div class="print-workout"><h3>${work.title}</h3><table>
                    <thead><tr><th style="width:50px">Mídia</th><th style="width:25%">Exercício</th><th style="width:8%;text-align:center">Séries</th><th style="width:12%;text-align:center">Reps</th><th style="width:15%">Técnica</th><th style="width:30%">Observações</th></tr></thead><tbody>`;
                if(work.exercises.length===0) html += `<tr><td colspan="6" style="text-align:center">Sem exercícios</td></tr>`;
                work.exercises.forEach(ex => {
                    const imgHtml = ex.image ? `<img src="${ex.image}" class="ex-img">` : '';
                    html += `<tr><td style="text-align:center">${imgHtml}</td><td><strong>${ex.name}</strong></td><td style="text-align:center">${ex.sets}</td><td style="text-align:center">${ex.reps}</td><td>${ex.technique !== 'Nenhuma'?ex.technique:'-'}</td><td>${ex.obs||'-'}</td></tr>`;
                });
                html += `</tbody></table></div>`;
            });

            html += `<div class="legal-section">
                ${d.stuRecs.trim() ? `<strong>Recomendações Manuais:</strong><br><p style="margin: 2px 0 8px 0; white-space:pre-wrap;">${d.stuRecs}</p>` : ''}
                ${isPEF ? legalPEF : legalTE}
                <div style="text-align:center; margin-top:10px; font-size:7px; font-style:normal;">Ficha gerada em ${new Date(d.timestamp).toLocaleDateString('pt-BR')} - PowFit Pro Master</div>
            </div>`;

            document.getElementById('print-area').innerHTML = html;
            setTimeout(() => window.print(), 300);
        }

        // --- HISTÓRICO EM NUVEM ---
        window.loadCloudHistory = async () => {
            const list = document.getElementById('cloud-history-list');
            list.innerHTML = `<p class="col-span-full text-center opacity-50">Buscando no servidor...</p>`;
            try {
                // Traz os últimos 50 treinos
                const q = query(collection(db, "powfit_workouts"), orderBy("timestamp", "desc"));
                const snap = await getDocs(q);
                
                if(snap.empty) { list.innerHTML = `<p class="col-span-full text-center opacity-50">Nenhuma ficha salva na nuvem ainda.</p>`; return; }

                list.innerHTML = snap.docs.map(docSnap => {
                    const d = docSnap.data();
                    const dt = d.timestamp?.toDate ? d.timestamp.toDate() : new Date(d.timestamp);
                    // Checagem de Validade Visual (Simples: +30 dias marca como vencida)
                    const isExpired = (new Date() - dt) > (30 * 24 * 60 * 60 * 1000); 

                    return `
                    <div class="card p-4 rounded-xl flex flex-col justify-between border ${isExpired ? 'border-red-500 border-opacity-50' : 'border-gray-500 border-opacity-20'}">
                        <div class="mb-3">
                            <div class="flex justify-between items-start">
                                <span class="font-bold text-lg">${d.studentName}</span>
                                ${isExpired ? `<span class="bg-red-500 text-white text-[9px] px-2 py-0.5 rounded font-bold uppercase tracking-wider">Vencida</span>` : `<span class="bg-green-500 text-white text-[9px] px-2 py-0.5 rounded font-bold uppercase tracking-wider">Ativa</span>`}
                            </div>
                            <div class="text-xs opacity-70 mt-1"><i class="fas fa-calendar-alt"></i> ${dt.toLocaleDateString('pt-BR')}</div>
                            <div class="text-xs opacity-70"><i class="fas fa-user-tie"></i> Resp: ${d.memberName}</div>
                        </div>
                        <button onclick="openCloudWorkout('${docSnap.id}')" class="btn-primary w-full py-2 rounded text-sm font-bold"><i class="fas fa-download"></i> Carregar para Edição</button>
                    </div>`;
                }).join('');
            } catch (e) {
                list.innerHTML = `<p class="col-span-full text-center text-red-500">Erro: ${e.message}</p>`;
            }
        };

        window.openCloudWorkout = async (docId) => {
            try {
                const docSnap = await getDoc(doc(db, "powfit_workouts", docId));
                if (docSnap.exists()) {
                    const item = docSnap.data();
                    state.currentWorkoutId = docId;
                    
                    document.getElementById('sel-member').value = item.memberId || '';
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
                    
                    changeTheme(); calculateIMC(); updateHealthOptionsBasedOnMember();
                    
                    setTimeout(() => {
                        document.querySelectorAll('.health-cb').forEach(cb => { cb.checked = (item.health || []).includes(cb.value); });
                    }, 100);

                    state.workouts = item.workouts || [];
                    renderWorkouts();
                    switchTab('prescricao');
                }
            } catch(e) { alert("Erro ao carregar: " + e.message); }
        };

        // --- RELATÓRIO DE PRODUTIVIDADE ---
        window.generateReport = async () => {
            const month = document.getElementById('report-month').value;
            const content = document.getElementById('report-content');
            content.innerHTML = `<p class="text-center opacity-50 mt-10">Calculando produtividade...</p>`;
            
            try {
                const q = query(collection(db, "powfit_workouts"), where("monthKey", "==", month));
                const snap = await getDocs(q);
                
                // Agrupamentos
                let total = snap.size;
                let byUnit = {};
                let byMember = {};

                snap.docs.forEach(docSnap => {
                    const d = docSnap.data();
                    byUnit[d.unitName] = (byUnit[d.unitName] || 0) + 1;
                    byMember[d.memberName] = (byMember[d.memberName] || 0) + 1;
                });

                let html = `
                    <div class="print-report-area">
                        <h3 class="text-xl font-bold mb-4 text-center">Relatório de Produtividade: ${month}</h3>
                        <div class="bg-blue-600 bg-opacity-20 text-blue-400 p-4 rounded-lg text-center mb-6 border border-blue-500 border-opacity-30">
                            <span class="text-sm uppercase tracking-widest block font-bold">Total de Fichas Geradas na Rede</span>
                            <span class="text-4xl font-bold">${total}</span>
                        </div>
                        
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                            <div>
                                <h4 class="font-bold border-b border-opacity-20 pb-2 mb-3" style="border-color: var(--border-color)">Por Unidade</h4>
                                <ul class="space-y-2">
                                    ${Object.keys(byUnit).map(u => `<li class="flex justify-between bg-black bg-opacity-10 p-2 rounded"><span>${u}</span> <span class="font-bold">${byUnit[u]}</span></li>`).join('')}
                                </ul>
                            </div>
                            <div>
                                <h4 class="font-bold border-b border-opacity-20 pb-2 mb-3" style="border-color: var(--border-color)">Por Treinador (Membro)</h4>
                                <ul class="space-y-2">
                                    ${Object.keys(byMember).map(m => `<li class="flex justify-between bg-black bg-opacity-10 p-2 rounded"><span>${m}</span> <span class="font-bold">${byMember[m]}</span></li>`).join('')}
                                </ul>
                            </div>
                        </div>
                    </div>
                `;
                content.innerHTML = html;

            } catch(e) {
                content.innerHTML = `<p class="text-center text-red-500 mt-10">Erro: ${e.message}</p>`;
            }
        };

        window.printReport = () => {
            const reportHTML = document.getElementById('report-content').innerHTML;
            if(!reportHTML.includes("Relatório de Produtividade")) return alert("Gere o relatório primeiro.");
            
            document.getElementById('print-area').innerHTML = `
                <div style="font-family: Arial; padding: 20px;">
                    <div style="border-bottom: 2px solid #000; padding-bottom: 10px; margin-bottom: 20px; text-align: center;">
                        <h1 style="margin:0; font-size: 20px; text-transform: uppercase;">POWFIT PRO MASTER</h1>
                        <p style="margin:5px 0 0 0; font-size: 12px;">Relatório de Produtividade Oficial</p>
                    </div>
                    ${reportHTML}
                </div>
            `;
            setTimeout(() => window.print(), 200);
        };

    </script>
</body>
</html>
