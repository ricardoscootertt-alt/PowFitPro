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
            --bg-body: #0f172a; --bg-card: #1e293b; --text-main: #f8fafc;
            --text-muted: #94a3b8; --border-color: #334155; --primary: #3b82f6;
            --primary-hover: #2563eb; --input-bg: #0f172a; --input-border: #475569;
        }
        [data-theme="Feminino"] {
            --bg-body: #fdf2f8; --bg-card: #ffffff; --text-main: #1e293b;
            --text-muted: #64748b; --border-color: #fbcfe8; --primary: #ec4899;
            --primary-hover: #db2777; --input-bg: #f8fafc; --input-border: #f1f5f9;
        }

        body { font-family: 'Inter', sans-serif; background-color: var(--bg-body); color: var(--text-main); transition: all 0.3s ease; }
        .card { background-color: var(--bg-card); border: 1px solid var(--border-color); }
        .input-field { background-color: var(--input-bg); border: 1px solid var(--input-border); color: var(--text-main); }
        .input-field:focus { outline: none; border-color: var(--primary); box-shadow: 0 0 0 1px var(--primary); }
        .btn-primary { background-color: var(--primary); color: white; transition: 0.2s; }
        .btn-primary:hover { background-color: var(--primary-hover); }

        #print-area { display: none; }
        
        /* Estilos de Impressão (Planilha A4) */
        @media print {
            body { background: white !important; color: black !important; margin: 0; padding: 0; }
            #app-screens, .no-print { display: none !important; }
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
            .print-workout { margin-bottom: 15px; page-break-inside: avoid; }
            .print-workout h3 { background: #e0e0e0; border: 1px solid #000; border-bottom: none; padding: 4px 8px; margin: 0; font-size: 12px; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            table { width: 100%; border-collapse: collapse; margin-bottom: 0; border: 1px solid #000; }
            th, td { border: 1px solid #000; padding: 4px 6px; text-align: left; font-size: 10px; }
            th { background-color: #f0f0f0; font-weight: bold; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            .print-footer-section { margin-top: 15px; border: 1px solid #000; padding: 8px; font-size: 9px; page-break-inside: avoid;}
            .legal-text { font-size: 8.5px; color: #111; text-align: justify; margin-top: 5px; font-style: italic;}
        }

        /* Loading Overlay */
        #loading-overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.8); z-index: 9999; display: flex; flex-direction: column; align-items: center; justify-content: center; color: white; display: none; }
        .loader { border: 4px solid #f3f3f3; border-top: 4px solid var(--primary); border-radius: 50%; width: 40px; height: 40px; animation: spin 1s linear infinite; }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
        
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
    </style>
</head>
<body data-theme="Masculino" class="min-h-screen pb-20">

    <div id="loading-overlay">
        <div class="loader mb-4"></div>
        <p id="loading-text" class="text-sm font-medium">Carregando...</p>
    </div>

    <!-- Container de Telas -->
    <div id="app-screens" class="max-w-7xl mx-auto p-4 sm:p-6 lg:p-8 relative">
        
        <!-- TELA 1: LOGIN -->
        <div id="screen-login" class="flex flex-col items-center justify-center min-h-[70vh] hidden">
            <div class="card p-8 rounded-2xl shadow-xl text-center max-w-md w-full">
                <i class="fas fa-dumbbell text-5xl text-primary mb-4"></i>
                <h1 class="text-3xl font-bold mb-2">PowFit Pro</h1>
                <p class="text-sm var(--text-muted) mb-8">Gestão de Treinos & Franquias</p>
                <button onclick="window.loginWithGoogle()" class="w-full bg-white text-gray-800 hover:bg-gray-100 font-bold py-3 px-4 rounded-lg shadow flex items-center justify-center gap-3 transition border">
                    <img src="https://www.svgrepo.com/show/475656/google-color.svg" class="w-5 h-5" alt="Google">
                    Entrar com Google
                </button>
            </div>
        </div>

        <!-- TELA 2: SETUP DE PERFIL -->
        <div id="screen-setup" class="hidden max-w-2xl mx-auto mt-10">
            <div class="card p-8 rounded-2xl shadow-xl">
                <h2 class="text-2xl font-bold mb-2 border-b border-opacity-20 pb-4" style="border-color: var(--border-color)">Completar Perfil Profissional</h2>
                <p class="text-sm opacity-70 mb-6">Você precisa configurar seu vínculo de rede e unidade antes de acessar a plataforma.</p>
                
                <div class="space-y-5">
                    <div>
                        <label class="block text-sm font-medium mb-1">Nome Completo</label>
                        <input type="text" id="setup-name" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Seu nome">
                    </div>
                    <div class="grid grid-cols-2 gap-4">
                        <div>
                            <label class="block text-sm font-medium mb-1">CPF</label>
                            <input type="text" id="setup-cpf" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="000.000.000-00">
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-1">Estado (UF)</label>
                            <input type="text" id="setup-state" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: SP">
                        </div>
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium mb-1">Rede de Franquia</label>
                        <select id="setup-network" class="input-field w-full rounded-lg px-3 py-2 text-sm mb-2"></select>
                        <input type="text" id="setup-new-network" class="input-field w-full rounded-lg px-3 py-2 text-sm hidden" placeholder="Nome da Nova Rede">
                        <button onclick="window.toggleNewNetwork()" class="text-xs text-primary hover:underline mt-1">Ou cadastrar nova Rede</button>
                    </div>

                    <div>
                        <label class="block text-sm font-medium mb-1">Unidade</label>
                        <input type="text" id="setup-unit" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Nome da sua Unidade (Ex: Unidade Centro)">
                    </div>

                    <div class="p-4 bg-black bg-opacity-10 rounded-lg border" style="border-color: var(--border-color)">
                        <label class="block text-sm font-medium mb-2">Categoria Profissional</label>
                        <select id="setup-category" onchange="window.toggleSetupCategory()" class="input-field w-full rounded-lg px-3 py-2 text-sm font-bold text-primary mb-3">
                            <option value="PEF">Profissional de Educação Física (PEF)</option>
                            <option value="TE">Treinador Esportivo (TE)</option>
                        </select>
                        <div id="setup-cref-container">
                            <label class="block text-sm font-medium mb-1">Número do CREF</label>
                            <input type="text" id="setup-cref" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: 000000-G/SP">
                        </div>
                    </div>

                    <button onclick="window.saveProfile()" class="w-full btn-primary py-3 rounded-lg font-bold shadow-lg mt-4">Salvar e Acessar Plataforma</button>
                </div>
            </div>
        </div>

        <!-- TELA 3: APP PRINCIPAL -->
        <div id="screen-app" class="hidden">
            <!-- Header App -->
            <div class="flex flex-col md:flex-row justify-between items-center mb-8 gap-4 border-b border-opacity-20 pb-4" style="border-color: var(--border-color)">
                <div class="flex items-center gap-3">
                    <i class="fas fa-dumbbell text-3xl text-primary"></i>
                    <div>
                        <h1 class="text-xl font-bold tracking-tight">PowFit Pro</h1>
                        <p id="app-user-info" class="text-xs var(--text-muted)"></p>
                    </div>
                </div>
                <div class="flex flex-wrap gap-2 justify-center">
                    <button onclick="window.switchTab('ficha')" id="tab-ficha" class="px-4 py-2 rounded-lg text-sm font-medium bg-primary text-white transition shadow">Nova Ficha</button>
                    <button onclick="window.switchTab('historico')" id="tab-historico" class="px-4 py-2 rounded-lg text-sm font-medium bg-opacity-20 hover:bg-black hover:bg-opacity-10 transition">Histórico</button>
                    <button onclick="window.switchTab('relatorio')" id="tab-relatorio" class="px-4 py-2 rounded-lg text-sm font-medium bg-opacity-20 hover:bg-black hover:bg-opacity-10 transition">Relatórios</button>
                    <button onclick="window.logout()" class="px-4 py-2 rounded-lg text-sm font-medium text-red-500 hover:bg-red-500 hover:text-white transition border border-red-500 border-opacity-30"><i class="fas fa-sign-out-alt"></i> Sair</button>
                </div>
            </div>

            <!-- TAB: NOVA FICHA -->
            <div id="tab-content-ficha">
                <div class="flex justify-end mb-4">
                    <button onclick="window.generatePrint()" class="btn-primary px-6 py-2 rounded-lg font-medium shadow-lg flex items-center gap-2">
                        <i class="fas fa-save"></i> <i class="fas fa-print"></i> Salvar na Nuvem & Imprimir
                    </button>
                </div>
                
                <div class="grid grid-cols-1 xl:grid-cols-12 gap-6">
                    <!-- Esquerda: Dados do Aluno -->
                    <div class="xl:col-span-4 space-y-6">
                        <div class="card rounded-xl p-5 shadow-sm">
                            <div class="flex justify-between items-center mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                                <h2 class="text-lg font-semibold flex items-center gap-2"><i class="fas fa-user text-primary"></i> Dados do Aluno</h2>
                                <div id="imc-display"></div>
                            </div>
                            <div class="space-y-4">
                                <div><label class="block text-sm font-medium mb-1">Nome do Aluno</label><input type="text" id="stu-name" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Nome do aluno"></div>
                                <div class="grid grid-cols-3 gap-3">
                                    <div><label class="block text-sm font-medium mb-1">Idade</label><input type="number" id="stu-age" class="input-field w-full rounded-lg px-3 py-2 text-sm"></div>
                                    <div><label class="block text-sm font-medium mb-1">Peso (kg)</label><input type="number" id="stu-weight" oninput="window.calculateIMC()" class="input-field w-full rounded-lg px-3 py-2 text-sm"></div>
                                    <div><label class="block text-sm font-medium mb-1">Altura (m)</label><input type="number" id="stu-height" step="0.01" oninput="window.calculateIMC()" class="input-field w-full rounded-lg px-3 py-2 text-sm"></div>
                                </div>
                                <div class="grid grid-cols-2 gap-3">
                                    <div><label class="block text-sm font-medium mb-1">Gênero</label>
                                        <select id="stu-gender" onchange="window.changeTheme()" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                            <option value="Masculino">Masculino</option><option value="Feminino">Feminino</option>
                                        </select>
                                    </div>
                                    <div><label class="block text-sm font-medium mb-1">Nível</label>
                                        <select id="stu-level" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                            <option>Iniciante</option><option>Intermediário</option><option>Avançado</option>
                                        </select>
                                    </div>
                                </div>
                                <div><label class="block text-sm font-medium mb-1">Objetivo</label>
                                    <select id="stu-objective" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                        <option value="Emagrecimento">Emagrecimento</option><option value="Hipertrofia">Hipertrofia</option>
                                        <option value="Definição">Definição</option><option value="Condicionamento">Condicionamento</option>
                                        <option value="Resistência">Resistência</option><option value="Força">Força</option>
                                        <option value="Reabilitação">Reabilitação</option><option value="Saúde geral">Saúde geral</option>
                                    </select>
                                </div>
                                <div><label class="block text-sm font-medium mb-1">Frequência</label>
                                    <select id="stu-freq" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                        <option>3 dias</option><option>5 dias</option><option>6 dias</option><option>Personalizado</option>
                                    </select>
                                </div>
                            </div>
                        </div>

                        <!-- Estado de Saúde -->
                        <div class="card rounded-xl p-5 shadow-sm">
                            <h2 class="text-lg font-semibold mb-2 flex items-center gap-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)"><i class="fas fa-notes-medical text-primary"></i> Estado de Saúde</h2>
                            <p class="text-[11px] opacity-70 mb-3 leading-tight">Diretrizes automáticas (Perfil <span id="lbl-perfil">PEF</span>).</p>
                            <div class="grid grid-cols-2 gap-2 text-xs" id="health-container"></div>
                        </div>
                    </div>

                    <!-- Direita: Treinos -->
                    <div class="xl:col-span-8 space-y-6">
                        <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center bg-opacity-10 p-4 rounded-xl card border-dashed border-2 gap-4">
                            <div>
                                <h2 class="text-xl font-bold flex items-center gap-2"><i class="fas fa-clipboard-list text-primary"></i> Ficha de Treino</h2>
                                <select id="stu-validity" class="input-field rounded px-2 py-1 text-xs mt-2">
                                    <option>Validade: 15 dias</option><option selected>Validade: 30 dias</option><option>Validade: 60 dias</option><option>Validade: 90 dias</option>
                                </select>
                            </div>
                            <button onclick="window.addWorkout()" class="btn-primary px-4 py-2 rounded-lg text-sm font-medium flex items-center gap-2"><i class="fas fa-plus"></i> Adicionar Dia</button>
                        </div>

                        <div id="workouts-container" class="space-y-6"></div>

                        <div class="card rounded-xl p-5 shadow-sm">
                            <h2 class="text-md font-semibold mb-2"><i class="fas fa-pen text-primary"></i> Recomendações Livres (Opcional)</h2>
                            <textarea id="stu-recs" rows="3" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Hidratação, descanso, cuidados específicos..."></textarea>
                        </div>
                    </div>
                </div>
            </div>

            <!-- TAB: HISTÓRICO -->
            <div id="tab-content-historico" class="hidden space-y-4">
                <div class="card p-6 rounded-xl shadow-sm">
                    <h2 class="text-xl font-bold mb-4 flex items-center gap-2"><i class="fas fa-history text-primary"></i> Minhas Fichas Salvas na Nuvem</h2>
                    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4" id="history-list"></div>
                </div>
            </div>

            <!-- TAB: RELATÓRIOS -->
            <div id="tab-content-relatorio" class="hidden space-y-4">
                <div class="card p-6 rounded-xl shadow-sm">
                    <div class="flex justify-between items-center mb-6">
                        <h2 class="text-xl font-bold flex items-center gap-2"><i class="fas fa-chart-bar text-primary"></i> Relatório de Produtividade (Mês Atual)</h2>
                        <button onclick="window.printReport()" class="btn-primary px-4 py-2 rounded-lg text-sm"><i class="fas fa-print"></i> Imprimir Relatório</button>
                    </div>
                    <p class="text-sm opacity-80 mb-4">Unidade: <strong id="report-unit-name" class="text-primary">Carregando...</strong></p>
                    <div class="overflow-x-auto">
                        <table class="w-full text-sm text-left">
                            <thead class="bg-black bg-opacity-10 font-bold uppercase" style="border-bottom: 1px solid var(--border-color)">
                                <tr>
                                    <th class="px-4 py-3">Membro (Treinador)</th>
                                    <th class="px-4 py-3">Categoria</th>
                                    <th class="px-4 py-3 text-center">Fichas Geradas</th>
                                </tr>
                            </thead>
                            <tbody id="report-tbody" class="divide-y" style="border-color: var(--border-color)">
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- MODAL DE EXERCÍCIOS -->
    <div id="exercise-modal" class="fixed inset-0 bg-black bg-opacity-70 backdrop-blur-sm hidden z-[60] flex items-center justify-center p-2 sm:p-4">
        <div class="card w-full max-w-6xl rounded-xl shadow-2xl flex flex-col h-[95vh] sm:h-[85vh]">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold"><i class="fas fa-search text-primary mr-2"></i> Banco de Exercícios</h3>
                <button onclick="window.closeModal()" class="text-gray-400 hover:text-red-500 transition-colors text-2xl leading-none">&times;</button>
            </div>
            
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <!-- Categorias -->
                <div class="w-full md:w-1/4 border-r overflow-y-auto p-2 space-y-1" style="border-color: var(--border-color)" id="modal-categories"></div>
                
                <!-- Lista de Exercícios -->
                <div class="w-full md:w-3/4 flex flex-col bg-black bg-opacity-5">
                    
                    <!-- Adicionar Novo Exercício -->
                    <div class="p-3 border-b flex gap-2 items-center" style="border-color: var(--border-color)">
                        <input type="text" id="new-custom-ex" class="input-field flex-1 rounded px-3 py-1.5 text-sm" placeholder="Adicionar novo exercício nesta categoria...">
                        <button onclick="window.addCustomExercise()" class="btn-primary px-4 py-1.5 rounded text-sm font-bold shadow">Salvar</button>
                    </div>

                    <div class="overflow-y-auto p-3 flex-1" id="modal-exercises"></div>
                </div>
            </div>
        </div>
    </div>

    <!-- ÁREA DE IMPRESSÃO (Oculta) -->
    <div id="print-area"></div>

    <!-- FIREBASE & LÓGICA DA APLICAÇÃO -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-app.js";
        import { getAuth, signInWithPopup, GoogleAuthProvider, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, addDoc, query, where, getDocs, orderBy, deleteDoc, serverTimestamp } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-firestore.js";

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

        // --- VARIÁVEIS GLOBAIS DE ESTADO ---
        let appState = {
            user: null,
            profile: null,
            networks: [],
            customExercises: [],
            workouts: [],
            currentFichaId: null, // ID no firebase se editando
            activeModalWorkoutId: null,
            activeCategory: "🔥 PEITO"
        };

        const daysOfWeek = ["SEGUNDA-FEIRA", "TERÇA-FEIRA", "QUARTA-FEIRA", "QUINTA-FEIRA", "SEXTA-FEIRA", "SÁBADO", "DOMINGO"];
        const dbTechniques = ["Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", "Negativa", "Isometria", "Parciais", "Pirâmide"];
        
        const baseCategories = {
            "🔥 PEITO": ["Supino Reto", "Supino Inclinado", "Supino Inclinado com Halteres", "Supino Fechado com Halteres", "Cross Over", "Cross Over Alto", "Cross Over Baixo", "Crucifixo Reto", "Crucifixo Inclinado com Halteres", "Crucifixo na Máquina", "Peck Fly", "Peck Fly Unilateral", "Pullover", "Flexão de Braço", "Flexão com Pés Elevados", "Flexão Explosiva"],
            "🦍 COSTAS": ["Puxada Alta", "Puxada de Frente Supinada", "Pulldown", "Remada Aberta", "Remada Baixa", "Remada Curvada", "Remada Curvada Supinada", "Remada Cavalinho (T-Bar)", "Serrote", "Facepull (puxada de cima para baixo)"],
            "🦵 PERNAS": ["Agachamento Livre", "Agachamento Taça", "Agachamento no Smith", "Agachamento com passada lateral", "Squat", "Hack Machine", "Leg 45°", "Leg 90°", "Agachamento Sumô", "Agachamento Sissy (Livre)", "Afundo", "Recuo", "Avanço", "Passada", "Búlgaro", "Step-up", "Levantamento Terra", "Levantamento Terra Romeno", "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", "Cadeira Flexora", "Elevação Pélvica no Banco", "Elevação Pélvica no Chão", "Elevação Pélvica Unilateral no Chão", "Extensão de Quadril (Glúteo Máximo)", "Extensão Cruzada (Glúteo Médio)", "Coice", "Cachorrinho", "Cadeira Extensora", "Adução", "Abdução", "Abdução Inclinada", "Flexão Nórdica", "Flexão Nórdica Invertida", "Panturrilha Livre", "Panturrilha no Leg Press", "Panturrilha Banco", "Panturrilha Squat", "Panturrilha Unilateral"],
            "💪 BRAÇOS": ["(Bíceps) Rosca Direta", "(Bíceps) Rosca Alternada", "(Bíceps) Rosca 21", "(Bíceps) Rosca Scott Barra W", "(Bíceps) Rosca Scott Unilateral", "(Bíceps) Rosca Scott com Halteres", "(Bíceps) Rosca Martelo", "(Bíceps) Rosca Cross", "(Bíceps) Rosca Inversa", "(Bíceps) Rosca 45°", "(Tríceps) Pulley Unilateral", "(Tríceps) Pulley Barra", "(Tríceps) Pulley Corda", "(Tríceps) Pulley Pegada Inversa", "(Tríceps) Francês na Corda", "(Tríceps) Francês com Halter", "(Tríceps) Francês Unilateral", "(Tríceps) Cruzado Polia Dupla", "(Tríceps) Coice Unilateral", "(Tríceps) Arremesso", "(Tríceps) Testa", "(Tríceps) Mergulho no Banco"],
            "🪨 OMBROS": ["Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", "Elevação Lateral Unilateral Cross", "Elevação Lateral Sentado", "Desenvolvimento com Halteres", "Desenvolvimento com Barra", "Arnold Press", "Elevação Borboleta", "Crucifixo Inverso Sentado com Halteres", "Crucifixo Inverso na Polia", "Crucifixo Inverso Unilateral na Polia", "Facepull (puxada reta)", "Remada Alta", "Encolhimento (Trapézio)"],
            "🧠 ABDÔMEN": ["Infra com Elevação de Perna", "Abdominal Supra", "Abdominal Remador", "Abdominal Bicicleta", "Abdominal Twister com Peso", "Prancha", "Prancha Lateral", "Trituração de Cabos em Pé", "Isometria na parede"],
            "🫀 CARDIO": ["Bicicleta 10 Minutos", "Bicicleta 15 Minutos", "Bicicleta 20 Minutos", "Esteira 10 Minutos", "Esteira 15 Minutos", "Esteira 20 Minutos", "Pular Corda"]
        };

        const healthDataPEF = { "🟢 Saudável": "Manter treinos regulares 3–5x por semana, combinando musculação e cardio. Foco em evolução e constância.", "⚪ Sedentário": "Iniciar com treinos leves 2–3x por semana. Priorizar adaptação, técnica e evitar excesso de carga.", "🟡 Sobrepeso": "Treinar 3–5x por semana com foco em gasto calórico. Combinar musculação e cardio moderado.", "🔴 Obesidade": "Iniciar com exercícios de baixo impacto. Evolução gradual, priorizando saúde e segurança.", "⚖️ Baixo peso": "Foco em musculação para ganho de massa. Treinos moderados com alimentação adequada.", "🍬 Diabetes": "Treinos regulares moderados. Monitorar glicemia e evitar treinos em jejum.", "❤️ Hipertensão": "Treinos moderados, evitar prender a respiração. Priorizar controle da intensidade.", "🔵 Hipotensão": "Evitar mudanças bruscas. Manter hidratação e intensidade leve a moderada.", "💔 Problemas cardíacos": "Treinos leves com liberação médica. Monitorar frequência cardíaca.", "🦴 Problemas articulares": "Evitar impacto. Priorizar exercícios controlados e máquinas.", "🫁 Problemas respiratórios": "Treinos leves a moderados com progressão gradual. Atenção à respiração.", "⚠️ Lesões": "Adaptar exercícios. Evitar dor e focar na recuperação.", "🤰 Gestante": "Treinos leves a moderados, sem impacto ou risco. Foco em mobilidade e bem-estar.", "🤱 Lactante": "Treinar normalmente com atenção à hidratação e energia.", "👴 Idoso": "Foco em força, equilíbrio e mobilidade. Intensidade moderada com segurança." };
        const healthDataTE = { "🟢 Saudável": "Manter treinos regulares de 3–5x por semana, combinando musculação e atividades cardiovasculares. A constância, descanso adequado e boa alimentação contribuem para melhores resultados.", "⚪ Sedentário": "O início deve ser gradual, com treinos leves e foco na adaptação do corpo. A prioridade é desenvolver constância, aprender a execução correta e evitar excesso de carga.", "🟡 Sobrepeso": "A prática regular de musculação associada ao cardio pode contribuir para melhora do condicionamento físico e composição corporal. A progressão deve respeitar a individualidade e o nível de adaptação.", "🔴 Obesidade": "É recomendado iniciar com exercícios de menor impacto e progressão gradual. A segurança, mobilidade e constância são prioridades durante o processo.", "⚖️ Baixo peso": "A musculação pode auxiliar no ganho de massa muscular quando associada a alimentação adequada e descanso. O treino deve priorizar evolução progressiva e recuperação muscular.", "🍬 Diabetes": "Pessoas com diabetes devem manter acompanhamento médico regular. Em casos de tontura, mal-estar ou alteração de glicemia, o treino deve ser interrompido e o aluno deve procurar orientação médica.", "❤️ Hipertensão": "Pessoas com hipertensão devem manter acompanhamento médico regular. Durante o treino, é importante evitar prender a respiração e controlar a intensidade.", "🔵 Hipotensão": "Evitar mudanças bruscas de posição durante o treino. Manter boa hidratação e respeitar a intensidade adequada.", "💔 Problemas cardíacos": "A prática de exercícios deve ocorrer apenas com liberação e acompanhamento médico.", "🦴 Problemas articulares": "Exercícios com menor impacto e maior controle de movimento costumam ser mais indicados. A adaptação ajuda na segurança.", "🫁 Problemas respiratórios": "A progressão do treino deve ser gradual e respeitar a capacidade respiratória individual.", "⚠️ Lesões": "A adaptação dos exercícios deve respeitar a limitação existente e evitar dor durante a execução. Seguir orientação profissional antes da prática.", "🤰 Gestante": "Prática com liberação médica. Foco deve ser segurança, mobilidade e bem-estar, evitando impacto excessivo e risco.", "🤱 Lactante": "A atividade física pode ser mantida normalmente, respeitando hidratação e alimentação.", "👴 Idoso": "A musculação contribui para força, equilíbrio e mobilidade. Respeitar limitações com intensidade moderada e foco na segurança." };
        const objectiveData = { "Emagrecimento": "Foco em déficit calórico com treinos mistos de força (manter massa magra) e aeróbicos (maior gasto).", "Hipertrofia": "Prioridade na progressão de carga e volume adequado. Essencial superávit calórico e descanso.", "Definição": "Manutenção de massa muscular enquanto reduz o percentual de gordura. Atenção estrita à dieta.", "Condicionamento": "Treinos com menor tempo de intervalo, circuitos e alta integração cardiopulmonar.", "Resistência": "Séries mais longas, cadência controlada e aprimoramento da capacidade muscular.", "Força": "Cargas altas, baixas repetições e intervalos de descanso maiores.", "Reabilitação": "Treino focado em fortalecimento específico, mobilidade e controle motor. Respeitar limites.", "Saúde geral": "Equilíbrio entre força, cardio e flexibilidade. O principal objetivo é a constância e bem-estar." };

        // UTILS
        function showLoading(msg) { document.getElementById('loading-text').innerText = msg; document.getElementById('loading-overlay').style.display = 'flex'; }
        function hideLoading() { document.getElementById('loading-overlay').style.display = 'none'; }
        function generateId() { return Math.random().toString(36).substr(2, 9); }
        function getMonthYear() { const d = new Date(); return `${d.getFullYear()}-${String(d.getMonth() + 1).padStart(2, '0')}`; }

        // --- AUTH E SETUP ---
        window.loginWithGoogle = async () => {
            try {
                showLoading('Autenticando...');
                await signInWithPopup(auth, new GoogleAuthProvider());
            } catch (e) {
                hideLoading(); alert("Erro no login: " + e.message);
            }
        };

        window.logout = async () => { await signOut(auth); location.reload(); };

        onAuthStateChanged(auth, async (user) => {
            if (user) {
                appState.user = user;
                document.getElementById('screen-login').classList.add('hidden');
                await checkProfile(user.uid);
            } else {
                hideLoading();
                document.getElementById('screen-login').classList.remove('hidden');
                document.getElementById('screen-app').classList.add('hidden');
                document.getElementById('screen-setup').classList.add('hidden');
            }
        });

        async function checkProfile(uid) {
            showLoading('Carregando Perfil...');
            try {
                const docRef = doc(db, "powfit_members", uid);
                const docSnap = await getDoc(docRef);
                
                if (docSnap.exists()) {
                    appState.profile = docSnap.data();
                    await loadCustomExercises();
                    await initApp();
                } else {
                    await loadNetworksForSetup();
                    document.getElementById('screen-setup').classList.remove('hidden');
                    hideLoading();
                }
            } catch (e) {
                console.error(e); hideLoading(); alert("Erro ao carregar perfil.");
            }
        }

        async function loadNetworksForSetup() {
            const q = query(collection(db, "powfit_networks"), orderBy("name"));
            const snap = await getDocs(q);
            const select = document.getElementById('setup-network');
            select.innerHTML = '<option value="">-- Selecione uma Rede --</option>';
            snap.forEach(d => {
                select.innerHTML += `<option value="${d.id}">${d.data().name}</option>`;
            });
        }

        window.toggleNewNetwork = () => {
            const select = document.getElementById('setup-network');
            const input = document.getElementById('setup-new-network');
            if (select.classList.contains('hidden')) {
                select.classList.remove('hidden'); input.classList.add('hidden'); input.value = '';
            } else {
                select.classList.add('hidden'); input.classList.remove('hidden'); select.value = '';
            }
        };

        window.toggleSetupCategory = () => {
            const cat = document.getElementById('setup-category').value;
            const cref = document.getElementById('setup-cref-container');
            if (cat === 'PEF') cref.classList.remove('hidden');
            else cref.classList.add('hidden');
        };

        window.saveProfile = async () => {
            const name = document.getElementById('setup-name').value.trim();
            const cpf = document.getElementById('setup-cpf').value.trim();
            const state = document.getElementById('setup-state').value.trim();
            const unit = document.getElementById('setup-unit').value.trim();
            const cat = document.getElementById('setup-category').value;
            const cref = document.getElementById('setup-cref').value.trim();
            
            let networkId = document.getElementById('setup-network').value;
            const newNetworkName = document.getElementById('setup-new-network').value.trim();

            if (!name || !cpf || !state || !unit) return alert("Preencha todos os campos básicos.");
            if (cat === 'PEF' && !cref) return alert("Profissionais PEF devem informar o CREF.");
            if (!networkId && !newNetworkName) return alert("Selecione ou crie uma Rede.");

            showLoading('Salvando Perfil...');
            try {
                // Cria rede se necessario
                if (newNetworkName) {
                    const netRef = await addDoc(collection(db, "powfit_networks"), { name: newNetworkName });
                    networkId = netRef.id;
                }

                // Cria unidade associada à rede
                const unitRef = await addDoc(collection(db, "powfit_units"), { name: unit, networkId: networkId });

                const profileData = {
                    uid: appState.user.uid,
                    name, cpf, state, category: cat, cref: cat === 'PEF' ? cref : '',
                    unitId: unitRef.id, networkId: networkId, unitName: unit
                };

                await setDoc(doc(db, "powfit_members", appState.user.uid), profileData);
                appState.profile = profileData;
                
                document.getElementById('screen-setup').classList.add('hidden');
                await loadCustomExercises();
                await initApp();
            } catch (e) {
                console.error(e); hideLoading(); alert("Erro ao salvar perfil.");
            }
        };

        // --- APP PRINCIPAL ---
        async function initApp() {
            document.getElementById('app-user-info').innerText = `${appState.profile.name} - ${appState.profile.unitName} (${appState.profile.category})`;
            document.getElementById('lbl-perfil').innerText = appState.profile.category;
            
            renderHealthOptions();
            if (appState.workouts.length === 0) window.addWorkout(); // Cria Segunda Feira
            
            document.getElementById('screen-app').classList.remove('hidden');
            window.switchTab('ficha');
            hideLoading();
        }

        window.switchTab = (tab) => {
            ['ficha', 'historico', 'relatorio'].forEach(t => {
                document.getElementById(`tab-content-${t}`).classList.add('hidden');
                document.getElementById(`tab-${t}`).classList.remove('bg-primary', 'text-white');
                document.getElementById(`tab-${t}`).classList.add('bg-opacity-20');
            });
            document.getElementById(`tab-content-${tab}`).classList.remove('hidden');
            document.getElementById(`tab-${tab}`).classList.remove('bg-opacity-20');
            document.getElementById(`tab-${tab}`).classList.add('bg-primary', 'text-white');

            if(tab === 'historico') loadHistory();
            if(tab === 'relatorio') loadReport();
        };

        function renderHealthOptions() {
            const dataObj = appState.profile.category === 'PEF' ? healthDataPEF : healthDataTE;
            const container = document.getElementById('health-container');
            const selected = Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value);
            
            container.innerHTML = Object.keys(dataObj).map(opt => {
                const isChecked = selected.includes(opt) ? 'checked' : '';
                return `<label class="flex items-start space-x-2 cursor-pointer p-1.5 rounded hover:bg-black hover:bg-opacity-5 transition border border-transparent hover:border-gray-500 hover:border-opacity-20">
                    <input type="checkbox" value="${opt}" ${isChecked} class="health-cb mt-0.5 rounded border-gray-400 text-primary focus:ring-primary w-3.5 h-3.5">
                    <span class="font-medium">${opt}</span>
                </label>`
            }).join('');
        }

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

        window.changeTheme = () => {
            document.body.setAttribute('data-theme', document.getElementById('stu-gender').value);
        };

        // --- LÓGICA DE FICHAS (UI) ---
        window.addWorkout = () => {
            const count = appState.workouts.length;
            const title = count < 7 ? `TREINO ${daysOfWeek[count]}` : `NOVO TREINO ${count + 1}`;
            appState.workouts.push({ id: generateId(), title: title, exercises: [] });
            renderWorkouts();
        };

        window.duplicateWorkout = (id) => {
            const w = appState.workouts.find(x => x.id === id);
            if(w) {
                const nw = JSON.parse(JSON.stringify(w));
                nw.id = generateId(); nw.title += " (Cópia)";
                appState.workouts.push(nw); renderWorkouts();
            }
        };

        window.removeWorkout = (id) => {
            if(confirm("Remover dia inteiro?")) {
                appState.workouts = appState.workouts.filter(w => w.id !== id); renderWorkouts();
            }
        };

        window.updateWorkoutTitle = (id, val) => { const w = appState.workouts.find(x => x.id === id); if(w) w.title = val; };
        window.updateExercise = (wId, eIdx, field, val) => { appState.workouts.find(x => x.id === wId).exercises[eIdx][field] = val; };
        window.removeExercise = (wId, eIdx) => { appState.workouts.find(x => x.id === wId).exercises.splice(eIdx, 1); renderWorkouts(); };
        window.moveExercise = (wId, eIdx, dir) => {
            const arr = appState.workouts.find(x => x.id === wId).exercises;
            if (dir === 'up' && eIdx > 0) [arr[eIdx], arr[eIdx-1]] = [arr[eIdx-1], arr[eIdx]];
            else if (dir === 'down' && eIdx < arr.length - 1) [arr[eIdx], arr[eIdx+1]] = [arr[eIdx+1], arr[eIdx]];
            renderWorkouts();
        };

        function renderWorkouts() {
            const container = document.getElementById('workouts-container');
            container.innerHTML = '';
            appState.workouts.forEach(w => {
                let exs = w.exercises.length === 0 ? `<div class="p-4 text-xs opacity-50 italic text-center">Sem exercícios.</div>` : 
                w.exercises.map((ex, idx) => `
                    <div class="flex flex-col sm:flex-row gap-2 p-2 items-start sm:items-center border-b last:border-0 border-opacity-10" style="border-color: var(--border-color)">
                        <div class="flex gap-1 hidden sm:flex">
                            <button onclick="window.moveExercise('${w.id}', ${idx}, 'up')" class="text-[10px] p-1 hover:bg-black hover:bg-opacity-10"><i class="fas fa-chevron-up"></i></button>
                            <button onclick="window.moveExercise('${w.id}', ${idx}, 'down')" class="text-[10px] p-1 hover:bg-black hover:bg-opacity-10"><i class="fas fa-chevron-down"></i></button>
                        </div>
                        <div class="flex-1 w-full sm:w-auto"><div class="text-[9px] opacity-60 uppercase">${ex.category}</div><div class="text-xs font-bold">${ex.name}</div></div>
                        <div class="flex flex-wrap gap-1 w-full sm:w-auto">
                            <input value="${ex.sets}" onchange="window.updateExercise('${w.id}', ${idx}, 'sets', this.value)" class="input-field w-12 rounded px-1 py-1 text-xs text-center" placeholder="Séries"> <span class="opacity-50 text-xs self-center">x</span>
                            <input value="${ex.reps}" onchange="window.updateExercise('${w.id}', ${idx}, 'reps', this.value)" class="input-field w-16 rounded px-1 py-1 text-xs text-center" placeholder="Reps">
                            <select onchange="window.updateExercise('${w.id}', ${idx}, 'technique', this.value)" class="input-field w-24 rounded px-1 py-1 text-[10px]">
                                ${dbTechniques.map(t => `<option value="${t}" ${ex.technique===t?'selected':''}>${t}</option>`).join('')}
                            </select>
                            <input value="${ex.obs}" onchange="window.updateExercise('${w.id}', ${idx}, 'obs', this.value)" class="input-field flex-1 sm:w-32 rounded px-1 py-1 text-xs" placeholder="Obs...">
                        </div>
                        <button onclick="window.removeExercise('${w.id}', ${idx})" class="text-red-500 hover:text-red-700 p-1"><i class="fas fa-trash"></i></button>
                    </div>
                `).join('');

                container.insertAdjacentHTML('beforeend', `
                    <div class="card rounded-xl overflow-hidden shadow-sm">
                        <div class="p-3 border-b flex justify-between bg-black bg-opacity-5" style="border-color: var(--border-color);">
                            <input value="${w.title}" onchange="window.updateWorkoutTitle('${w.id}', this.value)" class="input-field bg-transparent font-bold text-sm w-1/2 px-2 py-1 uppercase">
                            <div class="flex gap-1">
                                <button onclick="window.duplicateWorkout('${w.id}')" class="p-1.5 hover:bg-black hover:bg-opacity-10"><i class="fas fa-copy"></i></button>
                                <button onclick="window.removeWorkout('${w.id}')" class="p-1.5 text-red-500 hover:bg-red-500 hover:text-white"><i class="fas fa-trash"></i></button>
                            </div>
                        </div>
                        <div>${exs}</div>
                        <div class="p-2 border-t" style="border-color: var(--border-color)">
                            <button onclick="window.openModal('${w.id}')" class="w-full text-primary py-1.5 text-xs font-bold hover:bg-primary hover:bg-opacity-10">+ ADICIONAR EXERCÍCIO</button>
                        </div>
                    </div>
                `);
            });
        }

        // --- BANCO DE EXERCÍCIOS ---
        async function loadCustomExercises() {
            try {
                const q = query(collection(db, "powfit_custom_exercises"), where("uid", "==", appState.user.uid));
                const snap = await getDocs(q);
                appState.customExercises = snap.docs.map(d => d.data());
            } catch (e) { console.error("Erro custom exercises:", e); }
        }

        window.openModal = (id) => { appState.activeModalWorkoutId = id; document.getElementById('exercise-modal').classList.remove('hidden'); renderModalCategories(); renderModalExercises(); };
        window.closeModal = () => { document.getElementById('exercise-modal').classList.add('hidden'); appState.activeModalWorkoutId = null; };
        
        window.setModalCategory = (cat) => { appState.activeCategory = cat; renderModalCategories(); renderModalExercises(); };
        
        function renderModalCategories() {
            document.getElementById('modal-categories').innerHTML = Object.keys(baseCategories).map(cat => `
                <button onclick="window.setModalCategory('${cat}')" class="w-full text-left px-2 py-2 rounded-lg text-xs font-medium transition ${appState.activeCategory === cat ? 'bg-primary text-white' : 'hover:bg-black hover:bg-opacity-10'}">${cat}</button>
            `).join('');
        }

        function renderModalExercises() {
            let exs = [...baseCategories[appState.activeCategory]];
            // Adiciona customizados da categoria
            const custom = appState.customExercises.filter(c => c.category === appState.activeCategory).map(c => c.name);
            exs = [...new Set([...exs, ...custom])]; // junta sem duplicar

            const isCardio = appState.activeCategory === "🫀 CARDIO";
            document.getElementById('modal-exercises').innerHTML = `
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-2">
                    ${exs.map(ex => `<button onclick="window.addExerciseToWorkout('${ex}', ${isCardio})" class="card p-2 rounded-lg text-left text-xs font-medium border hover:border-primary transition flex justify-between items-center group">
                        <span>${ex}</span><i class="fas fa-plus opacity-0 group-hover:opacity-100 text-primary"></i>
                    </button>`).join('')}
                </div>
            `;
        }

        window.addExerciseToWorkout = (name, isCardio) => {
            const w = appState.workouts.find(x => x.id === appState.activeModalWorkoutId);
            if(w) {
                w.exercises.push({ category: appState.activeCategory, name, sets: isCardio?'1':'3', reps: isCardio?'-':'10 a 12', technique: 'Nenhuma', obs: '' });
                renderWorkouts();
                const container = document.getElementById('modal-exercises'); container.style.opacity = '0.5'; setTimeout(()=>container.style.opacity='1', 100);
            }
        };

        window.addCustomExercise = async () => {
            const input = document.getElementById('new-custom-ex');
            const val = input.value.trim();
            if(!val) return;
            
            try {
                input.value = "Salvando..."; input.disabled = true;
                const newEx = { uid: appState.user.uid, category: appState.activeCategory, name: val };
                await addDoc(collection(db, "powfit_custom_exercises"), newEx);
                appState.customExercises.push(newEx);
                input.value = ""; input.disabled = false;
                renderModalExercises(); // Atualiza a lista visual
            } catch (e) {
                alert("Erro ao salvar exercício."); input.disabled = false; input.value = val;
            }
        };

        // --- SALVAR NA NUVEM & HISTÓRICO ---
        function getFichaData() {
            return {
                uid: appState.user.uid,
                memberId: appState.profile.uid, // mesmissima coisa
                memberName: appState.profile.name,
                unitId: appState.profile.unitId,
                networkId: appState.profile.networkId,
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
                workouts: appState.workouts,
                monthYear: getMonthYear(),
                dateStr: new Date().toLocaleDateString('pt-BR')
            };
        }

        window.saveFichaToCloud = async () => {
            showLoading("Salvando Ficha...");
            try {
                const data = getFichaData();
                data.timestamp = serverTimestamp();
                
                if (appState.currentFichaId) {
                    await setDoc(doc(db, "powfit_workouts", appState.currentFichaId), data, { merge: true });
                } else {
                    const ref = await addDoc(collection(db, "powfit_workouts"), data);
                    appState.currentFichaId = ref.id;
                }
                hideLoading();
                return true;
            } catch (e) {
                console.error(e); hideLoading(); alert("Erro ao salvar na nuvem."); return false;
            }
        };

        async function loadHistory() {
            document.getElementById('history-list').innerHTML = '<div class="col-span-full text-center py-10 opacity-50"><i class="fas fa-spinner fa-spin text-2xl"></i></div>';
            try {
                const q = query(collection(db, "powfit_workouts"), where("uid", "==", appState.user.uid), orderBy("timestamp", "desc"));
                const snap = await getDocs(q);
                
                if(snap.empty) {
                    document.getElementById('history-list').innerHTML = '<div class="col-span-full text-center py-10 opacity-50">Nenhuma ficha salva.</div>';
                    return;
                }

                document.getElementById('history-list').innerHTML = snap.docs.map(doc => {
                    const d = doc.data();
                    return `
                    <div class="card p-4 rounded-lg flex flex-col justify-between border border-opacity-20">
                        <div class="mb-3">
                            <div class="font-bold text-lg">${d.studentName}</div>
                            <div class="text-xs opacity-70">Salvo: ${d.dateStr} | ${d.stuObjective}</div>
                        </div>
                        <div class="flex gap-2">
                            <button onclick="window.loadHistoryItem('${doc.id}')" class="btn-primary flex-1 py-1.5 rounded text-xs font-bold">CARREGAR</button>
                            <button onclick="window.deleteHistoryItem('${doc.id}')" class="bg-red-500 hover:bg-red-600 text-white px-3 py-1.5 rounded text-xs"><i class="fas fa-trash"></i></button>
                        </div>
                    </div>`;
                }).join('');
            } catch (e) {
                console.error(e);
                // Fallback in case orderBy fails due to missing index: query without orderBy and sort locally
                try {
                    const qFallback = query(collection(db, "powfit_workouts"), where("uid", "==", appState.user.uid));
                    const snapFallback = await getDocs(qFallback);
                    let docsArr = snapFallback.docs.map(d => ({id: d.id, data: d.data()}));
                    docsArr.sort((a,b) => (b.data.timestamp?.seconds || 0) - (a.data.timestamp?.seconds || 0));
                    
                    document.getElementById('history-list').innerHTML = docsArr.map(doc => {
                        const d = doc.data;
                        return `
                        <div class="card p-4 rounded-lg flex flex-col justify-between border border-opacity-20">
                            <div class="mb-3">
                                <div class="font-bold text-lg">${d.studentName}</div>
                                <div class="text-xs opacity-70">Salvo: ${d.dateStr} | ${d.stuObjective}</div>
                            </div>
                            <div class="flex gap-2">
                                <button onclick="window.loadHistoryItem('${doc.id}')" class="btn-primary flex-1 py-1.5 rounded text-xs font-bold">CARREGAR</button>
                                <button onclick="window.deleteHistoryItem('${doc.id}')" class="bg-red-500 hover:bg-red-600 text-white px-3 py-1.5 rounded text-xs"><i class="fas fa-trash"></i></button>
                            </div>
                        </div>`;
                    }).join('');
                } catch(e2) {
                     document.getElementById('history-list').innerHTML = '<div class="col-span-full text-center py-10 text-red-500">Erro ao buscar histórico.</div>';
                }
            }
        }

        window.loadHistoryItem = async (docId) => {
            showLoading("Carregando Ficha...");
            try {
                const docSnap = await getDoc(doc(db, "powfit_workouts", docId));
                if (docSnap.exists()) {
                    const d = docSnap.data();
                    appState.currentFichaId = docId;
                    
                    document.getElementById('stu-name').value = d.studentName || '';
                    document.getElementById('stu-age').value = d.stuAge || '';
                    document.getElementById('stu-weight').value = d.stuWeight || '';
                    document.getElementById('stu-height').value = d.stuHeight || '';
                    document.getElementById('stu-gender').value = d.stuGender || 'Masculino';
                    document.getElementById('stu-level').value = d.stuLevel || 'Iniciante';
                    document.getElementById('stu-objective').value = d.stuObjective || 'Emagrecimento';
                    document.getElementById('stu-freq').value = d.stuFreq || '3 dias';
                    document.getElementById('stu-validity').value = d.stuValidity || 'Validade: 30 dias';
                    document.getElementById('stu-recs').value = d.stuRecs || '';
                    
                    window.changeTheme();
                    window.calculateIMC();

                    document.querySelectorAll('.health-cb').forEach(cb => { cb.checked = (d.health || []).includes(cb.value); });
                    
                    appState.workouts = d.workouts || [];
                    renderWorkouts();
                    window.switchTab('ficha');
                }
            } catch (e) { console.error(e); alert("Erro ao carregar ficha."); }
            hideLoading();
        };

        window.deleteHistoryItem = async (docId) => {
            if(confirm("Excluir definitivamente esta ficha da nuvem?")) {
                try {
                    await deleteDoc(doc(db, "powfit_workouts", docId));
                    if(appState.currentFichaId === docId) appState.currentFichaId = null;
                    loadHistory();
                } catch (e) { alert("Erro ao excluir."); }
            }
        };

        // --- RELATÓRIOS ---
        async function loadReport() {
            document.getElementById('report-unit-name').innerText = appState.profile.unitName;
            document.getElementById('report-tbody').innerHTML = '<tr><td colspan="3" class="text-center py-4 opacity-50"><i class="fas fa-spinner fa-spin"></i> Processando dados...</td></tr>';
            
            try {
                // Pega fichas do mes atual E da mesma Unidade do cara
                const currentMonth = getMonthYear();
                const q = query(collection(db, "powfit_workouts"), where("unitId", "==", appState.profile.unitId), where("monthYear", "==", currentMonth));
                const snap = await getDocs(q);
                
                // Agrupar
                let counts = {};
                snap.forEach(doc => {
                    const d = doc.data();
                    if(!counts[d.memberId]) counts[d.memberId] = { name: d.memberName, cat: 'Profissional', count: 0 };
                    counts[d.memberId].count++;
                });

                // Se houver zero
                if(Object.keys(counts).length === 0) {
                    document.getElementById('report-tbody').innerHTML = '<tr><td colspan="3" class="text-center py-4 opacity-50">Nenhuma ficha gerada nesta unidade este mês.</td></tr>';
                    return;
                }

                // Renderizar
                document.getElementById('report-tbody').innerHTML = Object.values(counts).sort((a,b)=>b.count - a.count).map(m => `
                    <tr>
                        <td class="px-4 py-3 font-bold">${m.name}</td>
                        <td class="px-4 py-3 opacity-80">Membro da Unidade</td>
                        <td class="px-4 py-3 text-center text-primary font-bold text-lg">${m.count}</td>
                    </tr>
                `).join('');

            } catch (e) {
                console.error(e);
                document.getElementById('report-tbody').innerHTML = '<tr><td colspan="3" class="text-center py-4 text-red-500">Erro ao carregar relatório. Tente recarregar.</td></tr>';
            }
        }

        window.printReport = () => {
            const head = document.getElementById('report-unit-name').innerText;
            const body = document.getElementById('report-tbody').innerHTML;
            const pa = document.getElementById('print-area');
            
            pa.innerHTML = `
                <div class="print-header">
                    <h1 class="print-title">Relatório de Produtividade - ${new Date().toLocaleDateString('pt-BR', {month:'long', year:'numeric'})}</h1>
                    <div class="prof-info">Unidade: ${head}</div>
                </div>
                <table>
                    <thead><tr><th style="width:50%">Profissional</th><th style="width:20%">Unidade</th><th style="width:30%; text-align:center;">Fichas Produzidas</th></tr></thead>
                    <tbody>${body.replace(/<td/g, '<td style="border:1px solid #000; padding:6px;"')}</tbody>
                </table>
            `;
            setTimeout(() => window.print(), 300);
        };

        // --- IMPRESSÃO DA FICHA ---
        window.generatePrint = async () => {
            const success = await saveFichaToCloud();
            if(!success) return; // Cancela impressão se falhar

            const d = getFichaData();
            let imcStr = "-";
            const w = parseFloat(d.stuWeight); const h = parseFloat(d.stuHeight);
            if(w>0 && h>0) imcStr = (w / (h * h)).toFixed(1);

            const isPEF = appState.profile.category === 'PEF';
            const healthSourceDict = isPEF ? healthDataPEF : healthDataTE;
            const profLabel = isPEF ? 'Profissional de Educação Física' : 'Treinador Esportivo';
            const crefText = isPEF ? `CREF: ${appState.profile.cref} - Estado: ${appState.profile.state}` : '';
            
            let legalTextPEF = `⚠️ OBSERVAÇÃO LEGAL – PROFISSIONAL DE EDUCAÇÃO FÍSICA: Conforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º estabelece que compete ao profissional coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos especializados nas áreas de atividades físicas e do desporto.`;
            let legalTextTE = `⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO: Conforme a Lei nº 14.597/2023 (Lei Geral do Esporte), Art. 75, a profissão de treinador esportivo é reconhecida e regulada no Brasil, com atuação de caráter técnico e esportivo voltada à preparação, supervisão, orientação e acompanhamento de treinos. As recomendações desta ficha possuem caráter informativo e orientativo, voltadas ao treinamento esportivo e prática de musculação. Este material não substitui avaliação médica ou acompanhamento de profissionais da saúde. Em casos de dores, lesões, doenças, gestação ou qualquer condição específica, recomenda-se procurar um profissional habilitado antes de iniciar ou continuar os treinos. O treinamento respeita os limites individuais, priorizando segurança, execução correta e evolução progressiva, dentro da atuação técnica e esportiva permitida por lei.`;

            let html = `
                <div class="print-header">
                    <h1 class="print-title">Planilha de Treinamento - ${d.stuObjective}</h1>
                    <div class="prof-info">Prescrição feita por: ${d.memberName} (${profLabel})<br>${crefText}</div>
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

            if (objectiveData[d.stuObjective] || d.health.length > 0) {
                html += `<div class="print-guidelines"><h4>Diretrizes de Perfil Automáticas</h4><ul>`;
                if(objectiveData[d.stuObjective]) html += `<li><strong>Objetivo:</strong> ${objectiveData[d.stuObjective]}</li>`;
                d.health.forEach(k => {
                    if(healthSourceDict[k]) html += `<li><strong>Saúde (${k.replace(/[\u{1F300}-\u{1F9FF}\u{2600}-\u{26FF}\u{2700}-\u{27BF}\u{FE0F}]/gu, '').trim()}):</strong> ${healthSourceDict[k]}</li>`;
                });
                html += `</ul></div>`;
            }

            d.workouts.forEach(w => {
                html += `
                    <div class="print-workout">
                        <h3>${w.title}</h3>
                        <table>
                            <thead><tr><th style="width: 35%">Exercício</th><th style="width: 8%; text-align:center;">Séries</th><th style="width: 12%; text-align:center;">Reps</th><th style="width: 15%">Técnica</th><th style="width: 30%">Observações</th></tr></thead>
                            <tbody>
                `;
                if (w.exercises.length === 0) html += `<tr><td colspan="5" style="text-align:center; font-style:italic; padding:10px;">Sem exercícios</td></tr>`;
                else w.exercises.forEach(ex => html += `<tr><td><strong>${ex.name}</strong></td><td style="text-align:center;">${ex.sets}</td><td style="text-align:center;">${ex.reps}</td><td>${ex.technique !== 'Nenhuma' ? ex.technique : '-'}</td><td>${ex.obs || '-'}</td></tr>`);
                html += `</tbody></table></div>`;
            });

            html += `<div class="print-footer-section">`;
            if(d.stuRecs.trim()) html += `<strong>Recomendações Manuais:</strong><br><p style="white-space: pre-wrap; margin: 3px 0 10px 0;">${d.stuRecs}</p>`;
            html += `<div class="legal-text">${isPEF ? legalTextPEF : legalTextTE}</div>
                     <div style="text-align:center; margin-top: 15px; font-size: 8px;">Ficha gerada em ${d.dateStr} - Unidade: ${appState.profile.unitName} - PowFit Pro</div></div>`;

            document.getElementById('print-area').innerHTML = html;
            setTimeout(() => window.print(), 300);
        };

    </script>
</body>
</html>
