<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro - Plataforma Cloud de Treinos</title>
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
        
        /* Oculta áreas específicas baseadas no estado da UI */
        .view-section { display: none; }
        .view-section.active { display: block; }
        #print-area { display: none; }

        /* Estilos de Impressão (Estilo Planilha) */
        @media print {
            body { background: white !important; color: black !important; margin: 0; padding: 0; }
            #app-views, .no-print { display: none !important; }
            #print-area { display: block !important; padding: 10mm 15mm; font-family: 'Arial', sans-serif; font-size: 10px; }
            @page { size: A4; margin: 0; }
            
            .print-header { border-bottom: 2px solid #000; padding-bottom: 8px; margin-bottom: 12px; }
            .print-title { font-size: 18px; font-weight: bold; margin: 0; text-transform: uppercase; text-align: center;}
            .prof-info { font-size: 11px; margin-top: 5px; text-align: center; font-weight: bold;}
            
            .print-grid { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 4px; margin-bottom: 15px; border: 1px solid #000; padding: 8px; background: #fff; }
            .print-grid div { font-size: 11px; }
            
            .print-guidelines { border: 1px solid #000; padding: 8px; margin-bottom: 15px; font-size: 9.5px; }
            .print-guidelines h4 { margin: 0 0 4px 0; font-size: 11px; text-transform: uppercase; background: #eee; padding: 3px;}
            .print-guidelines ul { margin: 0; padding-left: 15px; }
            .print-guidelines li { margin-bottom: 3px; }

            .print-workout { margin-bottom: 15px; page-break-inside: avoid; }
            .print-workout h3 { background: #e0e0e0; border: 1px solid #000; border-bottom: none; padding: 4px 8px; margin: 0; font-size: 12px; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            table { width: 100%; border-collapse: collapse; margin-bottom: 0; border: 1px solid #000; }
            th, td { border: 1px solid #000; padding: 4px 6px; text-align: left; font-size: 10px; }
            th { background-color: #f0f0f0; font-weight: bold; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            
            .print-footer-section { margin-top: 15px; border: 1px solid #000; padding: 8px; font-size: 9px; page-break-inside: avoid;}
            .legal-text { font-size: 8px; color: #333; text-align: justify; margin-top: 5px; font-style: italic;}
        }

        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }

        /* Loader */
        .loader { border: 3px solid rgba(255,255,255,0.3); border-radius: 50%; border-top: 3px solid white; width: 24px; height: 24px; animation: spin 1s linear infinite; }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
    </style>
</head>
<body data-theme="Masculino" class="min-h-screen">

    <div id="app-views">
        
        <!-- ================= TELA DE LOGIN ================= -->
        <div id="view-login" class="view-section active flex items-center justify-center min-h-screen p-4">
            <div class="card p-8 rounded-2xl shadow-2xl max-w-md w-full text-center border-t-4 border-primary">
                <i class="fas fa-dumbbell text-5xl text-primary mb-4"></i>
                <h1 class="text-3xl font-bold mb-2">PowFit Pro</h1>
                <p class="text-sm var(--text-muted) mb-8">Plataforma Profissional em Nuvem</p>
                <button id="btn-google-login" class="w-full bg-white text-gray-800 font-bold py-3 px-4 rounded-lg shadow border border-gray-200 hover:bg-gray-50 transition flex items-center justify-center gap-3">
                    <img src="https://www.svgrepo.com/show/475656/google-color.svg" class="w-5 h-5" alt="Google">
                    Entrar com Google
                </button>
            </div>
        </div>

        <!-- ================= TELA DE SETUP (REDE E PERFIL) ================= -->
        <div id="view-setup" class="view-section min-h-screen p-4 flex justify-center items-start pt-10">
            <div class="card p-8 rounded-2xl shadow-xl max-w-lg w-full">
                <h2 class="text-xl font-bold mb-6 border-b pb-2" style="border-color: var(--border-color)">Configuração de Perfil</h2>
                <div class="space-y-4">
                    <div>
                        <label class="block text-sm font-medium mb-1">Nome da Rede / Franquia</label>
                        <input type="text" id="setup-network" class="input-field w-full rounded-lg px-3 py-2" placeholder="Ex: Power Fitness">
                    </div>
                    <div>
                        <label class="block text-sm font-medium mb-1">Nome da Unidade Base</label>
                        <input type="text" id="setup-unit" class="input-field w-full rounded-lg px-3 py-2" placeholder="Ex: Sibaúma">
                    </div>
                    <hr style="border-color: var(--border-color)">
                    <div>
                        <label class="block text-sm font-medium mb-1">Sua Categoria de Atuação</label>
                        <select id="setup-role" class="input-field w-full rounded-lg px-3 py-2 font-medium" onchange="toggleSetupCref()">
                            <option value="PEF">Profissional de Educação Física</option>
                            <option value="TE">Treinador Esportivo</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-sm font-medium mb-1">Seu Nome Profissional</label>
                        <input type="text" id="setup-name" class="input-field w-full rounded-lg px-3 py-2" placeholder="Seu nome">
                    </div>
                    <div class="grid grid-cols-2 gap-3" id="setup-cref-container">
                        <div>
                            <label class="block text-sm font-medium mb-1">CREF</label>
                            <input type="text" id="setup-cref" class="input-field w-full rounded-lg px-3 py-2" placeholder="Ex: 000000">
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-1">Estado</label>
                            <input type="text" id="setup-state" class="input-field w-full rounded-lg px-3 py-2" placeholder="Ex: SP, RJ">
                        </div>
                    </div>
                    <button id="btn-save-setup" class="w-full btn-primary py-3 rounded-lg font-bold mt-4">
                        Salvar e Entrar
                    </button>
                </div>
            </div>
        </div>

        <!-- ================= TELA PRINCIPAL (APP) ================= -->
        <div id="view-app" class="view-section min-h-screen">
            <!-- Navbar -->
            <nav class="card border-b px-6 py-4 flex justify-between items-center sticky top-0 z-40 shadow-sm" style="border-color: var(--border-color)">
                <div class="flex items-center gap-3">
                    <i class="fas fa-dumbbell text-2xl text-primary"></i>
                    <div>
                        <h1 class="text-lg font-bold leading-tight">PowFit Pro</h1>
                        <p class="text-[10px] var(--text-muted) leading-tight" id="nav-user-info">Rede | Unidade</p>
                    </div>
                </div>
                <div class="flex items-center gap-4">
                    <button onclick="switchTab('prescription')" class="tab-btn font-medium text-sm hover:text-primary transition" id="tab-btn-prescription">Nova Ficha</button>
                    <button onclick="switchTab('history')" class="tab-btn font-medium text-sm hover:text-primary transition" id="tab-btn-history">Histórico</button>
                    <button onclick="switchTab('reports')" class="tab-btn font-medium text-sm hover:text-primary transition" id="tab-btn-reports">Relatórios</button>
                    <div class="w-px h-6 bg-gray-500 opacity-30"></div>
                    <button id="btn-logout" class="text-sm text-red-400 hover:text-red-500 transition"><i class="fas fa-sign-out-alt"></i> Sair</button>
                </div>
            </nav>

            <div class="max-w-7xl mx-auto p-4 sm:p-6 lg:p-8">
                
                <!-- TAB: PRESCRIÇÃO -->
                <div id="tab-prescription" class="tab-content active">
                    <div class="flex justify-end mb-4">
                        <button onclick="saveAndPrint()" class="btn-primary px-6 py-2.5 rounded-lg font-bold shadow-lg flex items-center gap-2">
                            <i class="fas fa-save"></i> <i class="fas fa-print"></i> Salvar & Imprimir
                        </button>
                    </div>

                    <div class="grid grid-cols-1 xl:grid-cols-12 gap-6">
                        <!-- Coluna Esquerda: Dados do Aluno -->
                        <div class="xl:col-span-4 space-y-6">
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
                                        <input type="text" id="stu-name" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                    </div>
                                    <div class="grid grid-cols-3 gap-3">
                                        <div>
                                            <label class="block text-sm font-medium mb-1">Idade</label>
                                            <input type="number" id="stu-age" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                        </div>
                                        <div>
                                            <label class="block text-sm font-medium mb-1">Peso (kg)</label>
                                            <input type="number" id="stu-weight" oninput="calculateIMC()" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                        </div>
                                        <div>
                                            <label class="block text-sm font-medium mb-1">Altura (m)</label>
                                            <input type="number" id="stu-height" step="0.01" oninput="calculateIMC()" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                        </div>
                                    </div>
                                    <div class="grid grid-cols-2 gap-3">
                                        <div>
                                            <label class="block text-sm font-medium mb-1">Gênero</label>
                                            <select id="stu-gender" onchange="changeTheme()" class="input-field w-full rounded-lg px-3 py-2 text-sm">
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
                                <p class="text-[11px] opacity-70 mb-3 leading-tight">Diretrizes automáticas baseadas no seu Perfil.</p>
                                <div class="grid grid-cols-2 gap-2 text-xs" id="health-container"></div>
                            </div>
                        </div>

                        <!-- Coluna Direita: Treinos -->
                        <div class="xl:col-span-8 space-y-6">
                            <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center bg-opacity-10 p-4 rounded-xl card border-dashed border-2 gap-4">
                                <div>
                                    <h2 class="text-xl font-bold flex items-center gap-2"><i class="fas fa-clipboard-list text-primary"></i> Montagem de Treinos</h2>
                                    <div class="flex gap-2 mt-2">
                                        <select id="stu-validity" class="input-field rounded px-2 py-1 text-xs">
                                            <option value="15">Validade: 15 dias</option>
                                            <option value="30" selected>Validade: 30 dias</option>
                                            <option value="60">Validade: 60 dias</option>
                                            <option value="90">Validade: 90 dias</option>
                                        </select>
                                    </div>
                                </div>
                                <button onclick="addWorkout()" class="btn-primary px-4 py-2 rounded-lg text-sm font-medium flex items-center gap-2 w-full sm:w-auto justify-center">
                                    <i class="fas fa-plus"></i> Adicionar Dia de Treino
                                </button>
                            </div>

                            <div id="workouts-container" class="space-y-6"></div>

                            <div class="card rounded-xl p-5 shadow-sm">
                                <h2 class="text-md font-semibold mb-2"><i class="fas fa-pen text-primary"></i> Recomendações Livres</h2>
                                <textarea id="stu-recs" rows="3" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Hidratação, descanso..."></textarea>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- TAB: HISTÓRICO -->
                <div id="tab-history" class="tab-content hidden space-y-4">
                    <h2 class="text-2xl font-bold mb-4">Minhas Fichas Salvas</h2>
                    <div id="history-list" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
                        <!-- JS -->
                    </div>
                </div>

                <!-- TAB: RELATÓRIOS -->
                <div id="tab-reports" class="tab-content hidden space-y-6">
                    <h2 class="text-2xl font-bold mb-4">Relatórios da Rede</h2>
                    <div class="card p-6 rounded-xl shadow-sm overflow-x-auto">
                        <h3 class="text-lg font-bold mb-4" id="report-title">Fichas por Unidade/Membro (Mês Atual)</h3>
                        <table class="w-full text-left text-sm">
                            <thead class="border-b border-gray-600 border-opacity-30">
                                <tr>
                                    <th class="py-3 px-4 font-semibold uppercase">Unidade</th>
                                    <th class="py-3 px-4 font-semibold uppercase">Membro (Categoria)</th>
                                    <th class="py-3 px-4 font-semibold uppercase">Qtd. Fichas</th>
                                </tr>
                            </thead>
                            <tbody id="report-table-body">
                                <!-- JS -->
                            </tbody>
                        </table>
                    </div>
                </div>

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

    <!-- ÁREA DE IMPRESSÃO -->
    <div id="print-area"></div>

    <!-- FIREBASE & LÓGICA DO APP -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getAuth, signInWithPopup, GoogleAuthProvider, signOut, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
        import { getFirestore, collection, doc, setDoc, getDoc, getDocs, addDoc, query, where, deleteDoc } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

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

        // --- DADOS DA APLICAÇÃO ---
        let currentUser = null;
        let userProfile = null;
        
        let state = {
            currentWorkoutDocId: null,
            workouts: [],
            activeModalWorkoutId: null,
            activeCategory: "🔥 PEITO"
        };

        // ... (Dados de Saúde, Categorias e Técnicas omitidos para brevidade da explicação, mas inseridos completos abaixo) ...
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
            "💪 BRAÇOS": ["(Bíceps) Rosca Direta", "(Bíceps) Rosca Alternada", "(Bíceps) Rosca 21", "(Bíceps) Rosca Scott Barra W", "(Bíceps) Rosca Scott Unilateral", "(Bíceps) Rosca Scott com Halteres", "(Bíceps) Rosca Martelo", "(Bíceps) Rosca Cross", "(Bíceps) Rosca Inversa", "(Bíceps) Rosca 45°", "(Tríceps) Pulley Unilateral", "(Tríceps) Pulley Barra", "(Tríceps) Pulley Corda", "(Tríceps) Pulley Pegada Inversa", "(Tríceps) Francês na Corda", "(Tríceps) Francês com Halter", "(Tríceps) Francês Unilateral", "(Tríceps) Cruzado Polia Dupla", "(Tríceps) Coice Unilateral", "(Tríceps) Arremesso", "(Tríceps) Testa", "(Tríceps) Mergulho no Banco"],
            "🪨 OMBROS": ["Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", "Elevação Lateral Unilateral Cross", "Elevação Lateral Sentado", "Desenvolvimento com Halteres", "Desenvolvimento com Barra", "Arnold Press", "Elevação Borboleta", "Crucifixo Inverso Sentado com Halteres", "Crucifixo Inverso na Polia", "Crucifixo Inverso Unilateral na Polia", "Facepull (puxada reta)", "Remada Alta", "Encolhimento (Trapézio)"],
            "🧠 ABDÔMEN": ["Infra com Elevação de Perna", "Abdominal Supra", "Abdominal Remador", "Abdominal Bicicleta", "Abdominal Twister com Peso", "Prancha", "Prancha Lateral", "Trituração de Cabos em Pé", "Isometria na parede"],
            "🫀 CARDIO": ["Bicicleta 10 Minutos", "Bicicleta 15 Minutos", "Bicicleta 20 Minutos", "Esteira 10 Minutos", "Esteira 15 Minutos", "Esteira 20 Minutos", "Pular Corda"]
        };

        const dbTechniques = ["Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", "Negativa", "Isometria", "Parciais", "Pirâmide"];
        const daysOfWeek = ["SEGUNDA-FEIRA", "TERÇA-FEIRA", "QUARTA-FEIRA", "QUINTA-FEIRA", "SEXTA-FEIRA", "SÁBADO", "DOMINGO"];

        // --- AUTH & SETUP FLOW ---
        onAuthStateChanged(auth, async (user) => {
            if (user) {
                currentUser = user;
                // Verificar se o usuário já tem perfil
                const docRef = doc(db, "users", user.uid);
                const docSnap = await getDoc(docRef);
                if (docSnap.exists()) {
                    userProfile = docSnap.data();
                    showView('view-app');
                    document.getElementById('nav-user-info').textContent = `${userProfile.networkName} | ${userProfile.unitName}`;
                    renderHealthOptions();
                    if(state.workouts.length === 0) window.addWorkout(); // Adiciona primeiro treino
                } else {
                    showView('view-setup');
                    document.getElementById('setup-name').value = user.displayName || '';
                }
            } else {
                currentUser = null;
                userProfile = null;
                showView('view-login');
            }
        });

        document.getElementById('btn-google-login').addEventListener('click', () => {
            const provider = new GoogleAuthProvider();
            signInWithPopup(auth, provider).catch(err => alert("Erro ao fazer login: " + err.message));
        });

        document.getElementById('btn-logout').addEventListener('click', () => {
            signOut(auth);
        });

        document.getElementById('btn-save-setup').addEventListener('click', async () => {
            const network = document.getElementById('setup-network').value.trim();
            const unit = document.getElementById('setup-unit').value.trim();
            const role = document.getElementById('setup-role').value;
            const name = document.getElementById('setup-name').value.trim();
            const cref = document.getElementById('setup-cref').value.trim();
            const stateField = document.getElementById('setup-state').value.trim();

            if(!network || !unit || !name) return alert("Preencha Rede, Unidade e Nome.");

            const newProfile = {
                uid: currentUser.uid,
                email: currentUser.email,
                networkName: network,
                unitName: unit,
                role: role,
                name: name,
                cref: role === 'PEF' ? cref : '',
                state: role === 'PEF' ? stateField : '',
                createdAt: Date.now()
            };

            await setDoc(doc(db, "users", currentUser.uid), newProfile);
            userProfile = newProfile;
            
            document.getElementById('nav-user-info').textContent = `${network} | ${unit}`;
            renderHealthOptions();
            window.addWorkout();
            showView('view-app');
        });

        // Torna funções globais para o HTML onchange/onclick
        window.toggleSetupCref = function() {
            const type = document.getElementById('setup-role').value;
            const crefContainer = document.getElementById('setup-cref-container');
            if(type === 'TE') crefContainer.classList.add('hidden');
            else crefContainer.classList.remove('hidden');
        };

        window.showView = function(viewId) {
            document.querySelectorAll('.view-section').forEach(el => el.classList.remove('active'));
            document.getElementById(viewId).classList.add('active');
        };

        window.switchTab = function(tabName) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.add('hidden'));
            document.getElementById(`tab-${tabName}`).classList.remove('hidden');
            
            document.querySelectorAll('.tab-btn').forEach(el => {
                el.classList.remove('text-primary', 'border-b-2', 'border-primary');
            });
            document.getElementById(`tab-btn-${tabName}`).classList.add('text-primary', 'border-b-2', 'border-primary');

            if(tabName === 'history') loadHistory();
            if(tabName === 'reports') loadReports();
        };

        // --- LÓGICA DO APP (PRESCRIPTION) ---
        window.changeTheme = function() {
            document.body.setAttribute('data-theme', document.getElementById('stu-gender').value);
        }

        window.calculateIMC = function() {
            const w = parseFloat(document.getElementById('stu-weight').value);
            const h = parseFloat(document.getElementById('stu-height').value);
            const display = document.getElementById('imc-display');
            if (w > 0 && h > 0) {
                const imc = (w / (h * h)).toFixed(1);
                let classif = imc < 18.5 ? "Abaixo" : imc < 24.9 ? "Normal" : imc < 29.9 ? "Sobrepeso" : "Obesidade";
                display.innerHTML = `<span class="bg-primary bg-opacity-20 text-primary px-3 py-1 rounded-full text-xs font-bold border border-primary border-opacity-30">IMC: ${imc} (${classif})</span>`;
            } else { display.innerHTML = ''; }
        }

        window.renderHealthOptions = function() {
            const container = document.getElementById('health-container');
            const dataObj = userProfile.role === 'PEF' ? healthDataPEF : healthDataTE;
            const selected = Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value);
            
            container.innerHTML = Object.keys(dataObj).map(opt => {
                const isChecked = selected.includes(opt) ? 'checked' : '';
                return `<label class="flex items-start space-x-2 cursor-pointer p-1.5 rounded hover:bg-black hover:bg-opacity-5 transition border border-transparent hover:border-gray-500 hover:border-opacity-20">
                    <input type="checkbox" value="${opt}" ${isChecked} class="health-cb mt-0.5 rounded border-gray-400 text-primary focus:ring-primary w-3.5 h-3.5">
                    <span class="font-medium">${opt}</span>
                </label>`
            }).join('');
        }

        window.addWorkout = function() {
            const currentCount = state.workouts.length;
            const title = currentCount < 7 ? `TREINO ${daysOfWeek[currentCount]}` : `NOVO TREINO ${currentCount + 1}`;
            state.workouts.push({ id: Math.random().toString(36).substr(2, 9), title: title, exercises: [] });
            window.renderWorkouts();
        }

        window.removeWorkout = function(id) {
            if(confirm("Excluir este dia de treino?")) {
                state.workouts = state.workouts.filter(w => w.id !== id);
                window.renderWorkouts();
            }
        }
        window.duplicateWorkout = function(id) {
            const workout = state.workouts.find(w => w.id === id);
            if(workout) {
                const newWorkout = JSON.parse(JSON.stringify(workout));
                newWorkout.id = Math.random().toString(36).substr(2, 9);
                newWorkout.title = newWorkout.title + " (Cópia)";
                state.workouts.push(newWorkout);
                window.renderWorkouts();
            }
        }
        window.updateWorkoutTitle = function(id, newTitle) {
            const workout = state.workouts.find(w => w.id === id);
            if(workout) workout.title = newTitle;
        }
        window.moveExercise = function(workoutId, exIndex, direction) {
            const workout = state.workouts.find(w => w.id === workoutId);
            if (direction === 'up' && exIndex > 0) {
                const temp = workout.exercises[exIndex];
                workout.exercises[exIndex] = workout.exercises[exIndex - 1];
                workout.exercises[exIndex - 1] = temp;
            } else if (direction === 'down' && exIndex < workout.exercises.length - 1) {
                const temp = workout.exercises[exIndex];
                workout.exercises[exIndex] = workout.exercises[exIndex + 1];
                workout.exercises[exIndex + 1] = temp;
            }
            window.renderWorkouts();
        }
        window.removeExercise = function(workoutId, exIndex) {
            const workout = state.workouts.find(w => w.id === workoutId);
            workout.exercises.splice(exIndex, 1);
            window.renderWorkouts();
        }
        window.updateExercise = function(workoutId, exIndex, field, value) {
            const workout = state.workouts.find(w => w.id === workoutId);
            if(workout && workout.exercises[exIndex]) workout.exercises[exIndex][field] = value;
        }

        window.renderWorkouts = function() {
            const container = document.getElementById('workouts-container');
            container.innerHTML = state.workouts.map(workout => {
                const exHtml = workout.exercises.length === 0 ? `<div class="text-center p-4 text-xs opacity-50 italic">Sem exercícios.</div>` : 
                    workout.exercises.map((ex, idx) => `
                        <div class="flex flex-col sm:flex-row gap-2 p-2 items-start sm:items-center border-b last:border-0 border-opacity-10" style="border-color: var(--border-color)">
                            <div class="flex gap-1 hidden sm:flex">
                                <button onclick="moveExercise('${workout.id}', ${idx}, 'up')" class="text-[10px] p-1 rounded hover:bg-black hover:bg-opacity-10"><i class="fas fa-chevron-up"></i></button>
                                <button onclick="moveExercise('${workout.id}', ${idx}, 'down')" class="text-[10px] p-1 rounded hover:bg-black hover:bg-opacity-10"><i class="fas fa-chevron-down"></i></button>
                            </div>
                            <div class="flex-1 font-medium w-full sm:w-auto">
                                <div class="text-[9px] opacity-60 uppercase tracking-wider">${ex.category}</div>
                                <div class="text-xs">${ex.name}</div>
                            </div>
                            <div class="flex flex-wrap gap-1 w-full sm:w-auto">
                                <input type="text" value="${ex.sets}" onchange="updateExercise('${workout.id}', ${idx}, 'sets', this.value)" class="input-field w-12 rounded px-1 py-1 text-xs text-center" placeholder="Séries">
                                <span class="self-center opacity-50 text-xs">x</span>
                                <input type="text" value="${ex.reps}" onchange="updateExercise('${workout.id}', ${idx}, 'reps', this.value)" class="input-field w-16 rounded px-1 py-1 text-xs text-center" placeholder="Reps">
                                <select onchange="updateExercise('${workout.id}', ${idx}, 'technique', this.value)" class="input-field w-24 rounded px-1 py-1 text-[10px]">
                                    ${dbTechniques.map(t => `<option value="${t}" ${ex.technique === t ? 'selected' : ''}>${t}</option>`).join('')}
                                </select>
                                <input type="text" value="${ex.obs}" onchange="updateExercise('${workout.id}', ${idx}, 'obs', this.value)" class="input-field flex-1 sm:w-32 rounded px-1 py-1 text-xs" placeholder="Obs...">
                            </div>
                            <div class="flex items-center gap-1 w-full sm:w-auto justify-end mt-1 sm:mt-0">
                                <button onclick="removeExercise('${workout.id}', ${idx})" class="text-red-500 hover:text-red-700 p-1 rounded transition text-sm"><i class="fas fa-trash"></i></button>
                            </div>
                        </div>
                    `).join('');

                return `
                    <div class="card rounded-xl overflow-hidden shadow-sm">
                        <div class="p-3 border-b flex justify-between items-center bg-black bg-opacity-5" style="border-color: var(--border-color);">
                            <input type="text" value="${workout.title}" onchange="updateWorkoutTitle('${workout.id}', this.value)" class="input-field bg-transparent font-bold text-sm w-1/2 px-2 py-1 rounded uppercase">
                            <div class="flex gap-1">
                                <button onclick="duplicateWorkout('${workout.id}')" class="text-xs p-1.5 rounded hover:bg-black hover:bg-opacity-10 transition tooltip"><i class="fas fa-copy"></i></button>
                                <button onclick="removeWorkout('${workout.id}')" class="text-xs p-1.5 text-red-500 rounded hover:bg-red-500 hover:text-white transition tooltip"><i class="fas fa-trash"></i></button>
                            </div>
                        </div>
                        <div class="p-0">${exHtml}</div>
                        <div class="p-2 border-t" style="border-color: var(--border-color)">
                            <button onclick="openModal('${workout.id}')" class="w-full text-primary py-1.5 rounded-lg text-xs font-semibold hover:bg-primary hover:bg-opacity-10 transition">+ ADICIONAR EXERCÍCIO</button>
                        </div>
                    </div>`;
            }).join('');
        }

        window.openModal = function(workoutId) {
            state.activeModalWorkoutId = workoutId;
            document.getElementById('exercise-modal').classList.remove('hidden');
            window.renderModalCategories();
            window.renderModalExercises();
        }
        window.closeModal = function() { document.getElementById('exercise-modal').classList.add('hidden'); state.activeModalWorkoutId = null; }
        window.setModalCategory = function(cat) { state.activeCategory = cat; window.renderModalCategories(); window.renderModalExercises(); }
        
        window.renderModalCategories = function() {
            document.getElementById('modal-categories').innerHTML = Object.keys(dbCategories).map(cat => `
                <button onclick="setModalCategory('${cat}')" class="w-full text-left px-2 py-2 rounded-lg text-xs font-medium transition ${state.activeCategory === cat ? 'bg-primary text-white' : 'hover:bg-black hover:bg-opacity-10'}">${cat}</button>
            `).join('');
        }
        window.renderModalExercises = function() {
            document.getElementById('modal-exercises').innerHTML = `<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-2">` + 
                dbCategories[state.activeCategory].map(ex => {
                    const isCardio = state.activeCategory === "🫀 CARDIO";
                    return `<button onclick="addExerciseToWorkout('${ex}', ${isCardio})" class="card p-2 rounded-lg text-left text-xs font-medium border hover:border-primary transition flex justify-between items-center group">
                        <span>${ex}</span><i class="fas fa-plus opacity-0 group-hover:opacity-100 transition text-primary"></i>
                    </button>`
                }).join('') + `</div>`;
        }
        window.addExerciseToWorkout = function(exerciseName, isCardio) {
            const workout = state.workouts.find(w => w.id === state.activeModalWorkoutId);
            if(workout) {
                workout.exercises.push({ category: state.activeCategory, name: exerciseName, sets: isCardio ? '1' : '3', reps: isCardio ? '-' : '10 a 12', technique: 'Nenhuma', obs: '' });
                window.renderWorkouts();
                const container = document.getElementById('modal-exercises');
                container.style.opacity = '0.5';
                setTimeout(() => container.style.opacity = '1', 100);
            }
        }

        // --- SALVAR & HISTÓRICO NA NUVEM ---
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
                stuValidity: document.getElementById('stu-validity').value, // int (15,30,60,90)
                stuRecs: document.getElementById('stu-recs').value,
                health: Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value)
            };
        }

        window.saveAndPrint = async function() {
            if(!userProfile) return alert("Erro: Perfil não carregado.");
            
            const formData = getFormData();
            const dataToSave = {
                uid: currentUser.uid,
                networkName: userProfile.networkName,
                unitName: userProfile.unitName,
                profName: userProfile.name,
                profRole: userProfile.role,
                createdAt: Date.now(),
                data: formData,
                workouts: state.workouts
            };

            try {
                // Se já tem um ID, atualiza. Senão, cria novo doc na subcoleção do usuário
                if(state.currentWorkoutDocId) {
                    await setDoc(doc(db, "users", currentUser.uid, "workouts", state.currentWorkoutDocId), dataToSave);
                } else {
                    const docRef = await addDoc(collection(db, "users", currentUser.uid, "workouts"), dataToSave);
                    state.currentWorkoutDocId = docRef.id;
                }
                
                // Dispara Impressão
                executePrint(dataToSave);

            } catch (e) {
                console.error("Erro ao salvar: ", e);
                alert("Erro ao salvar a ficha na nuvem.");
            }
        }

        async function loadHistory() {
            const list = document.getElementById('history-list');
            list.innerHTML = `<div class="text-center w-full col-span-full py-10"><div class="loader mx-auto border-primary border-t-transparent"></div></div>`;
            
            try {
                const q = query(collection(db, "users", currentUser.uid, "workouts"));
                const querySnapshot = await getDocs(q);
                let history = [];
                querySnapshot.forEach((doc) => {
                    history.push({ id: doc.id, ...doc.data() });
                });

                if(history.length === 0) {
                    list.innerHTML = `<div class="text-center opacity-50 py-10 col-span-full">Nenhuma ficha salva.</div>`;
                    return;
                }

                history.sort((a,b) => b.createdAt - a.createdAt);

                list.innerHTML = history.map(h => {
                    const date = new Date(h.createdAt).toLocaleDateString('pt-BR');
                    
                    // Cálculo de Vencimento
                    const validityDays = parseInt(h.data.stuValidity) || 30;
                    const expirationDate = new Date(h.createdAt + (validityDays * 24 * 60 * 60 * 1000));
                    const isExpired = Date.now() > expirationDate;
                    const badge = isExpired ? `<span class="bg-red-500 text-white text-[10px] px-2 py-0.5 rounded ml-2">VENCIDA</span>` : 
                                              `<span class="bg-green-500 text-white text-[10px] px-2 py-0.5 rounded ml-2">ATIVA</span>`;

                    return `
                    <div class="card p-4 rounded-lg flex flex-col justify-between border border-gray-500 border-opacity-20 gap-3">
                        <div>
                            <div class="font-bold flex items-center">${h.data.studentName} ${badge}</div>
                            <div class="text-xs opacity-70 mt-1">Data: ${date} | Obj: ${h.data.stuObjective}</div>
                        </div>
                        <div class="flex gap-2 w-full">
                            <button onclick="loadHistoryItem('${h.id}')" class="btn-primary flex-1 py-1.5 rounded text-xs font-semibold">EDITAR</button>
                            <button onclick="deleteHistoryItem('${h.id}')" class="bg-red-500 hover:bg-red-600 text-white px-3 py-1.5 rounded text-xs font-semibold"><i class="fas fa-trash"></i></button>
                        </div>
                    </div>`
                }).join('');
            } catch(e) {
                list.innerHTML = `<div class="text-red-500 text-center col-span-full">Erro ao carregar histórico.</div>`;
            }
        }

        window.loadHistoryItem = async function(docId) {
            try {
                const docSnap = await getDoc(doc(db, "users", currentUser.uid, "workouts", docId));
                if (docSnap.exists()) {
                    const h = docSnap.data();
                    state.currentWorkoutDocId = docId;
                    state.workouts = h.workouts || [];
                    
                    document.getElementById('stu-name').value = h.data.studentName || '';
                    document.getElementById('stu-age').value = h.data.stuAge || '';
                    document.getElementById('stu-weight').value = h.data.stuWeight || '';
                    document.getElementById('stu-height').value = h.data.stuHeight || '';
                    document.getElementById('stu-gender').value = h.data.stuGender || 'Masculino';
                    document.getElementById('stu-level').value = h.data.stuLevel || 'Iniciante';
                    document.getElementById('stu-objective').value = h.data.stuObjective || 'Emagrecimento';
                    document.getElementById('stu-freq').value = h.data.stuFreq || '3 dias';
                    document.getElementById('stu-validity').value = h.data.stuValidity || '30';
                    document.getElementById('stu-recs').value = h.data.stuRecs || '';
                    
                    window.changeTheme();
                    window.calculateIMC();

                    document.querySelectorAll('.health-cb').forEach(cb => { cb.checked = (h.data.health || []).includes(cb.value); });
                    
                    window.switchTab('prescription');
                    window.renderWorkouts();
                }
            } catch(e) { alert("Erro ao carregar item."); }
        }

        window.deleteHistoryItem = async function(docId) {
            if(confirm("Excluir esta ficha permanentemente?")) {
                try {
                    await deleteDoc(doc(db, "users", currentUser.uid, "workouts", docId));
                    if(state.currentWorkoutDocId === docId) state.currentWorkoutDocId = null; // Desvincula se estiver editando
                    loadHistory();
                } catch(e) { alert("Erro ao excluir."); }
            }
        }

        // --- RELATÓRIOS DA REDE ---
        async function loadReports() {
            const tbody = document.getElementById('report-table-body');
            tbody.innerHTML = `<tr><td colspan="3" class="text-center py-4"><div class="loader mx-auto border-primary border-t-transparent"></div></td></tr>`;
            
            try {
                // Busca TODOS os usuários da mesma REDE
                const usersQ = query(collection(db, "users"), where("networkName", "==", userProfile.networkName));
                const usersSnap = await getDocs(usersQ);
                
                let reportData = {}; // Estrutura: { "Unidade": { "Nome (Papel)": count } }
                
                // Para cada usuário na rede, busca as fichas deste mês
                const currentMonth = new Date().getMonth();
                const currentYear = new Date().getFullYear();

                for (const userDoc of usersSnap.docs) {
                    const uData = userDoc.data();
                    const unit = uData.unitName || 'Sem Unidade';
                    const memberLabel = `${uData.name} (${uData.role})`;
                    
                    if(!reportData[unit]) reportData[unit] = {};
                    if(!reportData[unit][memberLabel]) reportData[unit][memberLabel] = 0;

                    const wQ = query(collection(db, "users", userDoc.id, "workouts"));
                    const wSnap = await getDocs(wQ);
                    
                    wSnap.forEach(wDoc => {
                        const date = new Date(wDoc.data().createdAt);
                        if(date.getMonth() === currentMonth && date.getFullYear() === currentYear) {
                            reportData[unit][memberLabel]++;
                        }
                    });
                }

                // Renderiza Tabela
                let html = '';
                for (const unit in reportData) {
                    for (const member in reportData[unit]) {
                        html += `
                            <tr class="border-b border-gray-600 border-opacity-10 hover:bg-black hover:bg-opacity-5">
                                <td class="py-2 px-4">${unit}</td>
                                <td class="py-2 px-4">${member}</td>
                                <td class="py-2 px-4 font-bold text-primary">${reportData[unit][member]}</td>
                            </tr>
                        `;
                    }
                }
                
                if(html === '') html = `<tr><td colspan="3" class="text-center py-4 opacity-50">Nenhuma ficha criada nesta rede este mês.</td></tr>`;
                tbody.innerHTML = html;

            } catch(e) {
                console.error(e);
                tbody.innerHTML = `<tr><td colspan="3" class="text-center py-4 text-red-500">Erro ao carregar relatórios. Garanta regras de leitura públicas no Firestore.</td></tr>`;
            }
        }

        // --- IMPRESSÃO ---
        function executePrint(payload) {
            const d = payload.data;
            const isPEF = payload.profRole === 'PEF';
            const healthSourceDict = isPEF ? healthDataPEF : healthDataTE;
            const objRecommendation = objectiveData[d.stuObjective] || "";
            
            let imcStr = "-";
            const w = parseFloat(d.stuWeight), h = parseFloat(d.stuHeight);
            if(w>0 && h>0) imcStr = (w / (h * h)).toFixed(1);

            const printArea = document.getElementById('print-area');
            const profLabel = isPEF ? 'Profissional de Educação Física' : 'Treinador Esportivo';
            const crefText = isPEF ? `CREF: ${userProfile.cref} - Estado: ${userProfile.state}` : '';
            
            const legalTextPEF = `⚠️ OBSERVAÇÃO LEGAL – PROFISSIONAL DE EDUCAÇÃO FÍSICA: Conforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º estabelece que compete ao profissional coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos especializados nas áreas de atividades físicas e do desporto.`;
            const legalTextTE = `⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO: Conforme a Lei nº 14.597/2023 (Lei Geral do Esporte), Art. 75, a profissão de treinador esportivo é reconhecida e regulada no Brasil, com atuação de caráter técnico e esportivo voltada à preparação, supervisão, orientação e acompanhamento de treinos. As recomendações desta ficha possuem caráter informativo e orientativo. Este material não substitui avaliação médica ou acompanhamento de profissionais da saúde. Em casos de dores, lesões, doenças, gestação ou qualquer condição específica, recomenda-se procurar um profissional habilitado antes de iniciar ou continuar os treinos. O treinamento respeita limites individuais, priorizando segurança e evolução progressiva.`;

            let html = `
                <div class="print-header">
                    <h1 class="print-title">Planilha de Treinamento - ${d.stuObjective}</h1>
                    <div class="prof-info">Prescrição feita por: ${payload.profName} (${profLabel})<br>${crefText}</div>
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
                    <div><strong>Validade:</strong> ${d.stuValidity} dias</div>
                </div>`;

            if (objRecommendation || d.health.length > 0) {
                html += `<div class="print-guidelines"><h4>Diretrizes Automáticas de Perfil</h4><ul>`;
                if(objRecommendation) html += `<li><strong>Objetivo (${d.stuObjective}):</strong> ${objRecommendation}</li>`;
                d.health.forEach(hKey => {
                    if(healthSourceDict[hKey]) html += `<li><strong>Saúde (${hKey.replace(/[\u{1F300}-\u{1F9FF}]/gu, '').trim()}):</strong> ${healthSourceDict[hKey]}</li>`;
                });
                html += `</ul></div>`;
            }

            payload.workouts.forEach(w => {
                html += `<div class="print-workout"><h3>${w.title}</h3><table>
                    <thead><tr>
                        <th style="width: 35%">Exercício</th><th style="width: 8%; text-align:center;">Séries</th>
                        <th style="width: 12%; text-align:center;">Reps</th><th style="width: 15%">Técnica</th><th style="width: 30%">Observações</th>
                    </tr></thead><tbody>`;
                if (w.exercises.length === 0) { html += `<tr><td colspan="5" style="text-align:center; font-style:italic;">Sem exercícios</td></tr>`; } 
                else {
                    w.exercises.forEach(ex => {
                        html += `<tr><td><strong>${ex.name}</strong></td><td style="text-align:center;">${ex.sets}</td>
                        <td style="text-align:center;">${ex.reps}</td><td>${ex.technique !== 'Nenhuma' ? ex.technique : '-'}</td><td>${ex.obs || '-'}</td></tr>`;
                    });
                }
                html += `</tbody></table></div>`;
            });

            html += `<div class="print-footer-section">`;
            if(d.stuRecs.trim()) html += `<strong>Recomendações Específicas:</strong><br><p style="white-space: pre-wrap; margin: 3px 0 10px 0;">${d.stuRecs}</p>`;
            html += `<div class="legal-text">${isPEF ? legalTextPEF : legalTextTE}</div>
                     <div style="text-align:center; margin-top: 10px; font-size: 8px;">Ficha gerada em ${new Date(payload.createdAt).toLocaleDateString('pt-BR')} - PowFit Pro Cloud</div></div>`;

            printArea.innerHTML = html;
            window.print();
        }

        // --- Inicialização Inicial de Tabs (Sem Login) ---
        document.getElementById('tab-btn-prescription').classList.add('text-primary', 'border-b-2', 'border-primary');
    </script>
</body>
</html>
