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
        .btn-primary { background-color: var(--primary); color: white; transition: background-color 0.2s; }
        .btn-primary:hover { background-color: var(--primary-hover); }
        
        /* Screens */
        .screen { display: none; }
        .screen.active { display: block; }
        
        /* Print Area */
        #print-area, #print-report-area { display: none; }

        @media print {
            body { background: white !important; color: black !important; margin: 0; padding: 0; font-family: Arial, sans-serif; }
            .screen, .no-print, #exercise-modal, #custom-exercise-modal { display: none !important; }
            
            /* WORKOUT PRINT */
            #print-area.active-print { display: block !important; padding: 10mm 15mm; font-size: 10px; }
            #print-area .print-header { border-bottom: 2px solid #000; padding-bottom: 8px; margin-bottom: 12px; text-align: center; }
            #print-area .print-title { font-size: 18px; font-weight: bold; margin: 0; text-transform: uppercase; }
            #print-area .prof-info { font-size: 12px; margin-top: 5px; font-weight: bold; }
            #print-area .print-grid { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 4px; margin-bottom: 15px; border: 1px solid #000; padding: 8px; }
            #print-area .print-guidelines { border: 1px solid #000; padding: 8px; margin-bottom: 15px; font-size: 9.5px; }
            #print-area .print-guidelines h4 { margin: 0 0 4px 0; font-size: 11px; text-transform: uppercase; background: #eee; padding: 3px; -webkit-print-color-adjust: exact; }
            #print-area .print-guidelines ul { margin: 0; padding-left: 15px; }
            #print-area .print-workout { margin-bottom: 15px; page-break-inside: avoid; }
            #print-area .print-workout h3 { background: #e0e0e0; border: 1px solid #000; border-bottom: none; padding: 4px 8px; margin: 0; font-size: 12px; text-transform: uppercase; -webkit-print-color-adjust: exact; }
            #print-area table { width: 100%; border-collapse: collapse; border: 1px solid #000; }
            #print-area th, #print-area td { border: 1px solid #000; padding: 4px 6px; text-align: left; font-size: 10px; }
            #print-area th { background-color: #f0f0f0; font-weight: bold; text-transform: uppercase; -webkit-print-color-adjust: exact; }
            #print-area .print-footer-section { margin-top: 15px; border: 1px solid #000; padding: 8px; font-size: 9px; page-break-inside: avoid; }
            #print-area .legal-text { font-size: 8px; color: #333; text-align: justify; margin-top: 5px; font-style: italic; }

            /* REPORT PRINT */
            #print-report-area.active-print { display: block !important; padding: 15mm; }
            #print-report-area h1 { text-align: center; border-bottom: 2px solid #000; padding-bottom: 10px; margin-bottom: 20px;}
            #print-report-area table { width: 100%; border-collapse: collapse; margin-top: 20px;}
            #print-report-area th, #print-report-area td { border: 1px solid #000; padding: 8px; text-align: left; }
            #print-report-area th { background-color: #eee; -webkit-print-color-adjust: exact; }
            @page { size: A4; margin: 0; }
        }

        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
    </style>
</head>
<body>

    <!-- LOADING OVERLAY -->
    <div id="loading-overlay" class="fixed inset-0 bg-black bg-opacity-90 z-[100] flex flex-col items-center justify-center text-white hidden">
        <i class="fas fa-circle-notch fa-spin text-4xl text-primary mb-4"></i>
        <p id="loading-text" class="text-lg font-medium">Sincronizando com a Nuvem...</p>
    </div>

    <!-- SCREEN 1: LOGIN -->
    <div id="screen-login" class="screen active min-h-screen flex items-center justify-center p-4">
        <div class="card p-8 rounded-2xl shadow-2xl max-w-md w-full text-center">
            <i class="fas fa-dumbbell text-5xl text-primary mb-4"></i>
            <h1 class="text-3xl font-bold mb-2">PowFit Pro</h1>
            <p class="text-sm opacity-70 mb-8">Plataforma de Gestão e Prescrição</p>
            
            <button onclick="handleGoogleLogin()" class="w-full bg-white text-gray-800 font-bold py-3 px-4 rounded-lg shadow flex items-center justify-center gap-3 hover:bg-gray-100 transition">
                <img src="https://www.svgrepo.com/show/475656/google-color.svg" class="w-5 h-5" alt="Google">
                Entrar com Google
            </button>
            <p class="text-[10px] opacity-50 mt-6">Seus dados serão salvos de forma segura na nuvem.</p>
        </div>
    </div>

    <!-- SCREEN 2: DASHBOARD (FRANQUIAS) -->
    <div id="screen-dashboard" class="screen min-h-screen pb-20">
        <div class="max-w-6xl mx-auto p-4 sm:p-6 lg:p-8">
            <div class="flex justify-between items-center mb-8 border-b border-opacity-20 pb-4" style="border-color: var(--border-color)">
                <div>
                    <h1 class="text-2xl font-bold tracking-tight"><i class="fas fa-network-wired text-primary mr-2"></i> Dashboard Corporativo</h1>
                    <p class="text-sm opacity-70">Gestão de Redes, Unidades e Equipe</p>
                </div>
                <div class="flex gap-3">
                    <button onclick="openReportModal()" class="bg-indigo-600 hover:bg-indigo-700 text-white px-4 py-2 rounded-lg font-medium shadow flex items-center gap-2 text-sm">
                        <i class="fas fa-chart-bar"></i> Relatório
                    </button>
                    <button onclick="handleLogout()" class="bg-red-500 hover:bg-red-600 text-white px-4 py-2 rounded-lg font-medium shadow flex items-center gap-2 text-sm">
                        <i class="fas fa-sign-out-alt"></i> Sair
                    </button>
                </div>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <!-- REDES -->
                <div class="card rounded-xl p-4 shadow-sm flex flex-col h-[70vh]">
                    <div class="flex justify-between items-center mb-4">
                        <h2 class="font-bold text-lg">1. Redes</h2>
                        <button onclick="promptCreateNetwork()" class="text-primary hover:text-white transition"><i class="fas fa-plus-circle text-xl"></i></button>
                    </div>
                    <div id="list-networks" class="flex-1 overflow-y-auto space-y-2"></div>
                </div>

                <!-- UNIDADES -->
                <div class="card rounded-xl p-4 shadow-sm flex flex-col h-[70vh]">
                    <div class="flex justify-between items-center mb-4">
                        <h2 class="font-bold text-lg">2. Unidades</h2>
                        <button onclick="promptCreateUnit()" id="btn-add-unit" class="text-primary hover:text-white transition hidden"><i class="fas fa-plus-circle text-xl"></i></button>
                    </div>
                    <p id="msg-select-network" class="text-xs opacity-50 italic text-center mt-10">Selecione uma Rede primeiro.</p>
                    <div id="list-units" class="flex-1 overflow-y-auto space-y-2 hidden"></div>
                </div>

                <!-- MEMBROS (EQUIPE) -->
                <div class="card rounded-xl p-4 shadow-sm flex flex-col h-[70vh]">
                    <div class="flex justify-between items-center mb-4">
                        <h2 class="font-bold text-lg">3. Membros</h2>
                        <button onclick="openMemberModal()" id="btn-add-member" class="text-primary hover:text-white transition hidden"><i class="fas fa-user-plus text-xl"></i></button>
                    </div>
                    <p id="msg-select-unit" class="text-xs opacity-50 italic text-center mt-10">Selecione uma Unidade primeiro.</p>
                    <div id="list-members" class="flex-1 overflow-y-auto space-y-2 hidden"></div>
                </div>
            </div>
        </div>
    </div>

    <!-- SCREEN 3: EDITOR (PRANCHETA) -->
    <div id="screen-editor" class="screen min-h-screen pb-20">
        <div class="max-w-7xl mx-auto p-4 sm:p-6 lg:p-8">
            <!-- Header Editor -->
            <div class="flex flex-col sm:flex-row justify-between items-center mb-6 gap-4 bg-black bg-opacity-20 p-4 rounded-xl border" style="border-color: var(--border-color)">
                <div class="flex items-center gap-3">
                    <button onclick="backToDashboard()" class="text-gray-400 hover:text-white text-xl mr-2"><i class="fas fa-arrow-left"></i></button>
                    <div>
                        <h1 class="text-xl font-bold tracking-tight text-primary">Prancheta Digital</h1>
                        <p class="text-xs opacity-70" id="editor-active-member-info">Profissional: Carregando...</p>
                    </div>
                </div>
                <div class="flex gap-2">
                    <button onclick="openCloudHistory()" class="bg-gray-700 hover:bg-gray-600 text-white px-4 py-2 rounded-lg font-medium shadow flex items-center gap-2 transition text-sm">
                        <i class="fas fa-cloud-download-alt"></i> Histórico Nuvem
                    </button>
                    <button onclick="saveAndPrint()" class="btn-primary px-5 py-2 rounded-lg font-medium shadow-lg flex items-center gap-2 text-sm">
                        <i class="fas fa-print"></i> Salvar & Imprimir
                    </button>
                </div>
            </div>

            <div class="grid grid-cols-1 xl:grid-cols-12 gap-6">
                <!-- Coluna Esquerda: Dados do Aluno -->
                <div class="xl:col-span-3 space-y-4">
                    <div class="card rounded-xl p-4 shadow-sm">
                        <div class="flex justify-between items-center mb-3 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                            <h2 class="text-md font-semibold"><i class="fas fa-user text-primary"></i> Aluno</h2>
                            <div id="imc-display" class="text-[10px]"></div>
                        </div>
                        <div class="space-y-3">
                            <input type="text" id="stu-name" class="input-field w-full rounded px-2 py-1.5 text-sm" placeholder="Nome Completo">
                            <div class="grid grid-cols-3 gap-2">
                                <input type="number" id="stu-age" class="input-field w-full rounded px-2 py-1.5 text-sm" placeholder="Idade">
                                <input type="number" id="stu-weight" oninput="calculateIMC()" class="input-field w-full rounded px-2 py-1.5 text-sm" placeholder="Peso(kg)">
                                <input type="number" id="stu-height" step="0.01" oninput="calculateIMC()" class="input-field w-full rounded px-2 py-1.5 text-sm" placeholder="Alt(m)">
                            </div>
                            <div class="grid grid-cols-2 gap-2">
                                <select id="stu-gender" onchange="changeTheme()" class="input-field w-full rounded px-2 py-1.5 text-sm">
                                    <option value="Masculino">Masculino</option><option value="Feminino">Feminino</option>
                                </select>
                                <select id="stu-level" class="input-field w-full rounded px-2 py-1.5 text-sm">
                                    <option>Iniciante</option><option>Intermediário</option><option>Avançado</option>
                                </select>
                            </div>
                            <select id="stu-objective" class="input-field w-full rounded px-2 py-1.5 text-sm">
                                <option>Emagrecimento</option><option>Hipertrofia</option><option>Definição</option>
                                <option>Condicionamento</option><option>Resistência</option><option>Força</option>
                                <option>Reabilitação</option><option>Saúde geral</option>
                            </select>
                            <select id="stu-freq" class="input-field w-full rounded px-2 py-1.5 text-sm">
                                <option>3 dias</option><option>5 dias</option><option>6 dias</option><option>Personalizado</option>
                            </select>
                            <select id="stu-validity" class="input-field w-full rounded px-2 py-1.5 text-sm">
                                <option>Validade: 15 dias</option><option selected>Validade: 30 dias</option>
                                <option>Validade: 60 dias</option><option>Validade: 90 dias</option>
                            </select>
                        </div>
                    </div>

                    <div class="card rounded-xl p-4 shadow-sm">
                        <h2 class="text-md font-semibold mb-2"><i class="fas fa-heartbeat text-primary"></i> Saúde</h2>
                        <div class="grid grid-cols-1 gap-1 text-[11px]" id="health-container"></div>
                    </div>

                    <div class="card rounded-xl p-4 shadow-sm">
                        <h2 class="text-md font-semibold mb-2"><i class="fas fa-pen text-primary"></i> Recs. Livres</h2>
                        <textarea id="stu-recs" rows="3" class="input-field w-full rounded px-2 py-1.5 text-xs" placeholder="Hidratação, descanso..."></textarea>
                    </div>
                </div>

                <!-- Coluna Direita: Treinos -->
                <div class="xl:col-span-9 space-y-4">
                    <div class="flex justify-between items-center bg-opacity-10 p-3 rounded-xl card border-dashed border-2">
                        <h2 class="text-lg font-bold"><i class="fas fa-dumbbell text-primary"></i> Montagem</h2>
                        <button onclick="addWorkout()" class="btn-primary px-3 py-1.5 rounded text-xs font-medium"><i class="fas fa-plus"></i> Dia de Treino</button>
                    </div>
                    <div id="workouts-container" class="space-y-4"></div>
                </div>
            </div>
        </div>
    </div>

    <!-- MODAIS -->
    <!-- Modal Cadastro Membro -->
    <div id="modal-member" class="fixed inset-0 bg-black bg-opacity-80 z-50 flex items-center justify-center hidden">
        <div class="card p-6 rounded-xl w-full max-w-md">
            <h2 class="text-xl font-bold mb-4">Adicionar Membro</h2>
            <div class="space-y-3">
                <input type="text" id="mem-name" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="Nome Completo">
                <input type="text" id="mem-cpf" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="CPF">
                <select id="mem-role" onchange="toggleMemRole()" class="input-field w-full rounded px-3 py-2 text-sm font-bold text-primary">
                    <option value="PEF">Profissional de Educação Física (PEF)</option>
                    <option value="TE">Treinador Esportivo (TE)</option>
                </select>
                <div id="mem-cref-container" class="grid grid-cols-2 gap-2">
                    <input type="text" id="mem-cref" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="CREF">
                    <input type="text" id="mem-state" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="Estado (UF)">
                </div>
            </div>
            <div class="flex justify-end gap-2 mt-6">
                <button onclick="closeMemberModal()" class="px-4 py-2 rounded text-sm hover:bg-gray-700">Cancelar</button>
                <button onclick="saveMember()" class="btn-primary px-4 py-2 rounded text-sm font-bold">Salvar Membro</button>
            </div>
        </div>
    </div>

    <!-- Modal Adicionar Exercício -->
    <div id="exercise-modal" class="fixed inset-0 bg-black bg-opacity-80 z-50 flex items-center justify-center p-2 hidden">
        <div class="card w-full max-w-5xl rounded-xl shadow-2xl flex flex-col h-[90vh]">
            <div class="p-3 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold"><i class="fas fa-search text-primary mr-2"></i> Banco de Exercícios</h3>
                <button onclick="closeModal()" class="text-gray-400 hover:text-red-500 text-2xl leading-none">&times;</button>
            </div>
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <div class="w-full md:w-1/4 border-r overflow-y-auto p-2 space-y-1" style="border-color: var(--border-color)" id="modal-categories"></div>
                <div class="w-full md:w-3/4 overflow-y-auto p-3 bg-black bg-opacity-5 flex flex-col">
                    <div class="flex justify-between items-center mb-3">
                        <span class="text-xs opacity-70">Clique para adicionar à ficha</span>
                        <button onclick="openCustomExerciseModal()" class="text-xs bg-indigo-600 hover:bg-indigo-500 text-white px-2 py-1 rounded"><i class="fas fa-plus"></i> Criar Customizado</button>
                    </div>
                    <div id="modal-exercises" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-2"></div>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal Criar Exercício Customizado -->
    <div id="custom-exercise-modal" class="fixed inset-0 bg-black bg-opacity-90 z-[60] flex items-center justify-center hidden">
        <div class="card p-6 rounded-xl w-full max-w-sm">
            <h2 class="text-lg font-bold mb-4">Novo Exercício</h2>
            <div class="space-y-3">
                <select id="custom-ex-cat" class="input-field w-full rounded px-3 py-2 text-sm"></select>
                <input type="text" id="custom-ex-name" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="Nome do Exercício">
            </div>
            <div class="flex justify-end gap-2 mt-6">
                <button onclick="closeCustomExerciseModal()" class="px-3 py-1.5 rounded text-sm hover:bg-gray-700">Cancelar</button>
                <button onclick="saveCustomExercise()" class="btn-primary px-3 py-1.5 rounded text-sm font-bold">Salvar</button>
            </div>
        </div>
    </div>

    <!-- Modal Histórico Nuvem -->
    <div id="history-modal" class="fixed inset-0 bg-black bg-opacity-80 z-50 flex items-center justify-center p-2 hidden">
        <div class="card w-full max-w-4xl rounded-xl shadow-2xl flex flex-col max-h-[90vh]">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold"><i class="fas fa-cloud text-primary mr-2"></i> Histórico da Equipe (Nuvem)</h3>
                <button onclick="closeCloudHistory()" class="text-gray-400 hover:text-red-500 text-2xl leading-none">&times;</button>
            </div>
            <div class="p-4 bg-black bg-opacity-20 flex gap-2 overflow-x-auto text-sm border-b" style="border-color: var(--border-color)">
                <span>Filtro Unidade:</span>
                <select id="hist-filter-unit" onchange="renderHistoryList()" class="input-field rounded px-2 py-0.5 text-xs"><option value="ALL">Todas as Unidades</option></select>
            </div>
            <div class="p-4 overflow-y-auto flex-1 space-y-2" id="history-list"></div>
        </div>
    </div>

    <!-- Modal Relatório -->
    <div id="report-modal" class="fixed inset-0 bg-black bg-opacity-90 z-50 flex items-center justify-center p-4 hidden">
        <div class="card w-full max-w-3xl rounded-xl shadow-2xl flex flex-col max-h-[90vh]">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold"><i class="fas fa-chart-line text-primary mr-2"></i> Relatório de Produtividade</h3>
                <button onclick="closeReportModal()" class="text-gray-400 hover:text-red-500 text-2xl leading-none">&times;</button>
            </div>
            <div class="p-4 flex gap-3">
                <select id="report-month" class="input-field rounded px-3 py-1.5 text-sm flex-1"></select>
                <select id="report-unit" class="input-field rounded px-3 py-1.5 text-sm flex-1"><option value="ALL">Todas as Unidades</option></select>
                <button onclick="generateReportData()" class="btn-primary px-4 py-1.5 rounded text-sm font-bold">Filtrar</button>
            </div>
            <div class="p-4 overflow-y-auto flex-1" id="report-content"></div>
            <div class="p-4 border-t flex justify-end" style="border-color: var(--border-color)">
                <button onclick="printReport()" class="bg-green-600 hover:bg-green-700 text-white px-4 py-2 rounded text-sm font-bold shadow flex gap-2 items-center"><i class="fas fa-print"></i> Imprimir Relatório</button>
            </div>
        </div>
    </div>

    <!-- ÁREA DE IMPRESSÃO FICHA -->
    <div id="print-area"></div>
    
    <!-- ÁREA DE IMPRESSÃO RELATÓRIO -->
    <div id="print-report-area"></div>

    <!-- SCRIPTS FIREBASE & LOGIC -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, GoogleAuthProvider, signInWithPopup, onAuthStateChanged, signInAnonymously, signInWithCustomToken, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, doc, setDoc, getDoc, getDocs, addDoc, deleteDoc, updateDoc, onSnapshot } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // --- CONFIGURAÇÃO FIREBASE ---
        let firebaseConfig = {
            apiKey: "AIzaSyD-iuERHA_x1f69smJXh7b8wTLO1zTadIU",
            authDomain: "powfitpro-d8577.firebaseapp.com",
            projectId: "powfitpro-d8577",
            storageBucket: "powfitpro-d8577.firebasestorage.app",
            messagingSenderId: "651381183985",
            appId: "1:651381183985:web:9a425692372a9d67d9e050"
        };
        
        if (typeof __firebase_config !== 'undefined' && __firebase_config) {
            try { firebaseConfig = JSON.parse(__firebase_config); } catch(e){}
        }

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'powfit-pro';

        // --- ESTADO GLOBAL ---
        window.AppState = {
            user: null,
            userId: null,
            networks: [], units: [], members: [], workoutsHistory: [], customExercises: [],
            selectedNetworkId: null, selectedUnitId: null,
            activeMember: null, // O treinador que está logado na prancheta
            currentFichaId: null, // ID da ficha sendo editada
            workouts: [], // Dias de treino da ficha atual
            activeCategory: "🔥 PEITO"
        };

        const getBasePath = () => `artifacts/${appId}/users/${window.AppState.userId}`;

        // --- DADOS ESTÁTICOS ---
        const healthPEF = { "🟢 Saudável": "Manter treinos regulares 3–5x por semana, combinando musculação e cardio. Foco em evolução e constância.", "⚪ Sedentário": "Iniciar com treinos leves 2–3x por semana. Priorizar adaptação, técnica e evitar excesso de carga.", "🟡 Sobrepeso": "Treinar 3–5x por semana com foco em gasto calórico. Combinar musculação e cardio moderado.", "🔴 Obesidade": "Iniciar com exercícios de baixo impacto. Evolução gradual, priorizando saúde e segurança.", "⚖️ Baixo peso": "Foco em musculação para ganho de massa. Treinos moderados com alimentação adequada.", "🍬 Diabetes": "Treinos regulares moderados. Monitorar glicemia e evitar treinos em jejum.", "❤️ Hipertensão": "Treinos moderados, evitar prender a respiração. Priorizar controle da intensidade.", "🔵 Hipotensão": "Evitar mudanças bruscas. Manter hidratação e intensidade leve a moderada.", "💔 Problemas cardíacos": "Treinos leves com liberação médica. Monitorar frequência cardíaca.", "🦴 Problemas articulares": "Evitar impacto. Priorizar exercícios controlados e máquinas.", "🫁 Problemas respiratórios": "Treinos leves a moderados com progressão gradual. Atenção à respiração.", "⚠️ Lesões": "Adaptar exercícios. Evitar dor e focar na recuperação.", "🤰 Gestante": "Treinos leves a moderados, sem impacto ou risco. Foco em mobilidade e bem-estar.", "🤱 Lactante": "Treinar normalmente com atenção à hidratação e energia.", "👴 Idoso": "Foco em força, equilíbrio e mobilidade. Intensidade moderada com segurança." };
        const healthTE = { "🟢 Saudável": "Manter treinos regulares de 3–5x por semana, combinando musculação e atividades cardiovasculares. A constância e boa alimentação contribuem para resultados.", "⚪ Sedentário": "Início gradual, treinos leves, foco na adaptação. Prioridade é desenvolver constância e execução correta.", "🟡 Sobrepeso": "A musculação associada ao cardio contribui para condicionamento. Progressão deve respeitar individualidade.", "🔴 Obesidade": "Iniciar com menor impacto e progressão gradual. Segurança, mobilidade e constância são prioridades.", "⚖️ Baixo peso": "Musculação auxilia no ganho de massa. Priorizar evolução progressiva e recuperação muscular.", "🍬 Diabetes": "Acompanhamento médico regular. Em casos de mal-estar ou alteração de glicemia, interromper e buscar orientação.", "❤️ Hipertensão": "Acompanhamento médico. Evitar prender a respiração (Manobra de Valsalva) e controlar a intensidade.", "🔵 Hipotensão": "Evitar mudanças bruscas de posição. Manter boa hidratação e respeitar intensidade adequada.", "💔 Problemas cardíacos": "Prática apenas com liberação e acompanhamento médico. Respeitar limites e controlar intensidade.", "🦴 Problemas articulares": "Menor impacto e maior controle de movimento são indicados. Acompanhamento ajuda na segurança.", "🫁 Problemas respiratórios": "Progressão gradual respeitando a capacidade. Interromper em caso de falta de ar excessiva.", "⚠️ Lesões": "Adaptação respeitando limitação e evitando dor. Seguir orientação profissional antes da prática.", "🤰 Gestante": "Liberação médica. Foco na segurança, mobilidade e bem-estar, evitando impacto e risco.", "🤱 Lactante": "Atividade normal, respeitando recuperação, hidratação e alimentação.", "👴 Idoso": "Musculação contribui para força e autonomia. Respeitar limitações com intensidade moderada e segurança." };
        const objectiveData = { "Emagrecimento": "Foco em déficit calórico com treinos mistos de força (manter massa magra) e aeróbicos (maior gasto).", "Hipertrofia": "Prioridade na progressão de carga e volume adequado. Essencial superávit calórico e descanso.", "Definição": "Manutenção de massa muscular enquanto reduz o percentual de gordura. Atenção estrita à dieta.", "Condicionamento": "Treinos com menor tempo de intervalo, circuitos e alta integração cardiopulmonar.", "Resistência": "Séries mais longas, cadência controlada e aprimoramento da capacidade muscular.", "Força": "Cargas altas, baixas repetições e intervalos de descanso maiores.", "Reabilitação": "Treino focado em fortalecimento específico, mobilidade e controle motor. Respeitar limites.", "Saúde geral": "Equilíbrio entre força, cardio e flexibilidade. O principal objetivo é a constância e bem-estar." };
        const baseCategories = {
            "🔥 PEITO": ["Supino Reto", "Supino Inclinado", "Supino Inclinado com Halteres", "Supino Fechado com Halteres", "Cross Over", "Cross Over Alto", "Cross Over Baixo", "Crucifixo Reto", "Crucifixo Inclinado com Halteres", "Crucifixo na Máquina", "Peck Fly", "Peck Fly Unilateral", "Pullover", "Flexão de Braço", "Flexão com Pés Elevados", "Flexão Explosiva"],
            "🦍 COSTAS": ["Puxada Alta", "Puxada de Frente Supinada", "Pulldown", "Remada Aberta", "Remada Baixa", "Remada Curvada", "Remada Curvada Supinada", "Remada Curvada na Polia", "Remada Cavalinho (T-Bar)", "Remada Unilateral", "Remada no Cross", "Serrote", "Facepull (puxada de cima para baixo)"],
            "🦵 PERNAS": ["Agachamento Livre", "Agachamento Taça", "Agachamento no Smith", "Agachamento com barra", "Agachamento com passada lateral", "Squat", "Hack Machine", "Leg 45°", "Leg 90°", "Agachamento Sumô", "Agachamento Sissy (Livre)", "Afundo", "Recuo", "Avanço", "Passada", "Búlgaro", "Step-up", "Levantamento Terra", "Levantamento Terra Romeno", "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", "Cadeira Flexora", "Elevação Pélvica no Banco", "Elevação Pélvica no Chão", "Elevação Pélvica Unilateral no Chão", "Extensão de Quadril (Glúteo Máximo)", "Extensão Cruzada (Glúteo Médio)", "Abdução no Cross (Glúteo Médio + Mínimo)", "Coice", "Cachorrinho", "Caranguejo", "Cadeira Extensora", "Adução", "Abdução", "Abdução Inclinada", "Cadeira Abdutora Inclinada", "Flexão Nórdica", "Flexão Nórdica Invertida", "Panturrilha Livre", "Panturrilha em Pé (Máquina)", "Panturrilha no Leg Press", "Panturrilha Banco", "Panturrilha Squat", "Panturrilha Unilateral"],
            "💪 BRAÇOS": ["(Bíceps) Rosca Direta", "(Bíceps) Rosca Alternada", "(Bíceps) Rosca 21", "(Bíceps) Rosca Scott Barra W", "(Bíceps) Rosca Scott Unilateral", "(Bíceps) Rosca Scott com Halteres", "(Bíceps) Rosca Martelo", "(Bíceps) Rosca Cross", "(Bíceps) Rosca Concentrada", "(Bíceps) Rosca Inversa", "(Bíceps) Rosca 45°", "(Tríceps) Pulley Unilateral", "(Tríceps) Pulley Barra", "(Tríceps) Pulley Corda", "(Tríceps) Pulley Pegada Inversa", "(Tríceps) Francês na Corda", "(Tríceps) Francês com Halter", "(Tríceps) Francês Unilateral", "(Tríceps) Cruzado Polia Dupla", "(Tríceps) Coice Unilateral", "(Tríceps) Arremesso", "(Tríceps) Testa", "(Tríceps) Mergulho no Banco"],
            "🪨 OMBROS": ["Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", "Elevação Lateral na Polia", "Elevação Lateral Unilateral Cross", "Elevação Lateral Sentado", "Desenvolvimento com Halteres", "Desenvolvimento com Barra", "Arnold Press", "Elevação Borboleta", "Crucifixo Inverso Sentado com Halteres", "Crucifixo Inverso na Polia", "Crucifixo Inverso Unilateral na Polia", "Facepull (puxada reta)", "Remada Alta", "Encolhimento (Trapézio)"],
            "🧠 ABDÔMEN": ["Infra com Elevação de Perna", "Abdominal Supra", "Abdominal Remador", "Abdominal Bicicleta", "Abdominal Twister com Peso", "Prancha", "Prancha Lateral", "Trituração de Cabos em Pé", "Isometria na parede", "Abdominal isometrico"],
            "🫀 CARDIO": ["Bicicleta 10 Minutos", "Bicicleta 15 Minutos", "Bicicleta 20 Minutos", "Esteira 10 Minutos", "Esteira 15 Minutos", "Esteira 20 Minutos", "Pular Corda"]
        };
        const dbTechniques = ["Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", "Negativa", "Isometria", "Parciais", "Pirâmide"];
        const daysOfWeek = ["SEGUNDA-FEIRA", "TERÇA-FEIRA", "QUARTA-FEIRA", "QUINTA-FEIRA", "SEXTA-FEIRA", "SÁBADO", "DOMINGO"];

        // --- CORE FUNCTIONS ---
        const showLoading = (msg) => { document.getElementById('loading-text').innerText = msg; document.getElementById('loading-overlay').classList.remove('hidden'); };
        const hideLoading = () => document.getElementById('loading-overlay').classList.add('hidden');
        const showScreen = (id) => { document.querySelectorAll('.screen').forEach(s => s.classList.remove('active')); document.getElementById(id).classList.add('active'); };
        const genId = () => Math.random().toString(36).substr(2, 9);

        // --- AUTHENTICATION ---
        window.handleGoogleLogin = async () => {
            showLoading("Iniciando Login...");
            try {
                if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                    await signInWithCustomToken(auth, __initial_auth_token);
                } else {
                    try {
                        const provider = new GoogleAuthProvider();
                        await signInWithPopup(auth, provider);
                    } catch (popupError) {
                        console.warn("Popup blocked or not supported, falling back to anonymous", popupError);
                        await signInAnonymously(auth);
                    }
                }
            } catch (error) {
                console.error("Auth error", error);
                alert("Erro ao fazer login. Tente novamente.");
                hideLoading();
            }
        };

        window.handleLogout = async () => { await signOut(auth); showScreen('screen-login'); };

        onAuthStateChanged(auth, async (user) => {
            if (user) {
                window.AppState.user = user;
                window.AppState.userId = user.uid;
                await initRealtimeListeners();
                showScreen('screen-dashboard');
                hideLoading();
            } else {
                window.AppState.user = null;
                window.AppState.userId = null;
                showScreen('screen-login');
                hideLoading();
                if(window.location.hostname.includes('googleusercontent') || window.location.hostname.includes('sandbox')) {
                    handleGoogleLogin();
                }
            }
        });

        // --- DATABASE LISTENERS ---
        let unsubscribers = [];
        async function initRealtimeListeners() {
            unsubscribers.forEach(u => u()); unsubscribers = [];
            if(!window.AppState.userId) return;
            showLoading("Sincronizando dados...");

            const basePath = getBasePath();
            
            unsubscribers.push(onSnapshot(collection(db, `${basePath}/networks`), snap => {
                window.AppState.networks = snap.docs.map(d => ({id: d.id, ...d.data()}));
                renderNetworks();
            }, e => console.error("Net error", e)));

            unsubscribers.push(onSnapshot(collection(db, `${basePath}/units`), snap => {
                window.AppState.units = snap.docs.map(d => ({id: d.id, ...d.data()}));
                renderUnits(); updateFilters();
            }, e => console.error("Unit error", e)));

            unsubscribers.push(onSnapshot(collection(db, `${basePath}/members`), snap => {
                window.AppState.members = snap.docs.map(d => ({id: d.id, ...d.data()}));
                renderMembers();
            }, e => console.error("Mem error", e)));

            unsubscribers.push(onSnapshot(collection(db, `${basePath}/workouts`), snap => {
                window.AppState.workoutsHistory = snap.docs.map(d => ({id: d.id, ...d.data()}));
            }, e => console.error("Wkt error", e)));

            unsubscribers.push(onSnapshot(collection(db, `${basePath}/custom_exercises`), snap => {
                window.AppState.customExercises = snap.docs.map(d => ({id: d.id, ...d.data()}));
            }, e => console.error("CEx error", e)));

            setTimeout(hideLoading, 1000);
        }

        // --- DASHBOARD: HIERARCHY MANAGEMENT ---
        window.promptCreateNetwork = async () => {
            const name = prompt("Nome da Nova Rede de Franquias:");
            if(name && name.trim() && window.AppState.userId) {
                await addDoc(collection(db, `${getBasePath()}/networks`), { name: name.trim(), createdAt: new Date().toISOString() });
            }
        };

        window.renderNetworks = () => {
            const list = document.getElementById('list-networks');
            list.innerHTML = window.AppState.networks.map(n => `
                <div onclick="selectNetwork('${n.id}')" class="p-3 border rounded cursor-pointer transition ${window.AppState.selectedNetworkId === n.id ? 'border-primary bg-primary bg-opacity-10 text-primary font-bold' : 'border-gray-600 border-opacity-30 hover:bg-black hover:bg-opacity-20'}">
                    <i class="fas fa-building mr-2 opacity-50"></i> ${n.name}
                </div>
            `).join('');
            if(!window.AppState.selectedNetworkId && window.AppState.networks.length > 0) selectNetwork(window.AppState.networks[0].id);
        };

        window.selectNetwork = (id) => {
            window.AppState.selectedNetworkId = id;
            window.AppState.selectedUnitId = null;
            document.getElementById('btn-add-unit').classList.remove('hidden');
            document.getElementById('msg-select-network').classList.add('hidden');
            document.getElementById('list-units').classList.remove('hidden');
            renderNetworks(); renderUnits(); renderMembers();
        };

        window.promptCreateUnit = async () => {
            if(!window.AppState.selectedNetworkId) return;
            const name = prompt("Nome/Localização da Nova Unidade:");
            if(name && name.trim()) {
                await addDoc(collection(db, `${getBasePath()}/units`), { networkId: window.AppState.selectedNetworkId, name: name.trim() });
            }
        };

        window.renderUnits = () => {
            const list = document.getElementById('list-units');
            const filtered = window.AppState.units.filter(u => u.networkId === window.AppState.selectedNetworkId);
            list.innerHTML = filtered.map(u => `
                <div onclick="selectUnit('${u.id}')" class="p-3 border rounded cursor-pointer transition ${window.AppState.selectedUnitId === u.id ? 'border-primary bg-primary bg-opacity-10 text-primary font-bold' : 'border-gray-600 border-opacity-30 hover:bg-black hover:bg-opacity-20'}">
                    <i class="fas fa-map-marker-alt mr-2 opacity-50"></i> ${u.name}
                </div>
            `).join('');
        };

        window.selectUnit = (id) => {
            window.AppState.selectedUnitId = id;
            document.getElementById('btn-add-member').classList.remove('hidden');
            document.getElementById('msg-select-unit').classList.add('hidden');
            document.getElementById('list-members').classList.remove('hidden');
            renderUnits(); renderMembers();
        };

        window.toggleMemRole = () => {
            const role = document.getElementById('mem-role').value;
            document.getElementById('mem-cref-container').style.display = role === 'TE' ? 'none' : 'grid';
        };

        window.openMemberModal = () => document.getElementById('modal-member').classList.remove('hidden');
        window.closeMemberModal = () => {
            document.getElementById('modal-member').classList.add('hidden');
            document.getElementById('mem-name').value = '';
            document.getElementById('mem-cpf').value = '';
            document.getElementById('mem-cref').value = '';
            document.getElementById('mem-state').value = '';
        };

        window.saveMember = async () => {
            const name = document.getElementById('mem-name').value.trim();
            const cpf = document.getElementById('mem-cpf').value.trim();
            const role = document.getElementById('mem-role').value;
            const cref = document.getElementById('mem-cref').value.trim();
            const state = document.getElementById('mem-state').value.trim();

            if(!name || !cpf || (role === 'PEF' && (!cref || !state))) return alert("Preencha os campos obrigatórios.");
            
            await addDoc(collection(db, `${getBasePath()}/members`), {
                unitId: window.AppState.selectedUnitId, networkId: window.AppState.selectedNetworkId,
                name, cpf, role, cref: role === 'PEF' ? cref : '', state: role === 'PEF' ? state : ''
            });
            closeMemberModal();
        };

        window.renderMembers = () => {
            const list = document.getElementById('list-members');
            const filtered = window.AppState.members.filter(m => m.unitId === window.AppState.selectedUnitId);
            list.innerHTML = filtered.map(m => `
                <div class="p-3 border rounded border-gray-600 border-opacity-30 hover:border-primary transition flex justify-between items-center group">
                    <div>
                        <div class="font-bold text-sm">${m.name}</div>
                        <div class="text-[10px] opacity-70">${m.role === 'PEF' ? 'Prof. Ed. Física' : 'Treinador Esportivo'} ${m.cref ? `| CREF: ${m.cref}-${m.state}` : ''}</div>
                    </div>
                    <button onclick="enterEditor('${m.id}')" class="btn-primary text-xs px-3 py-1.5 rounded opacity-0 group-hover:opacity-100 transition">Acessar Prancheta</button>
                </div>
            `).join('');
        };


        // --- EDITOR (PRANCHETA) LOGIC ---
        window.enterEditor = (memberId) => {
            window.AppState.activeMember = window.AppState.members.find(m => m.id === memberId);
            if(!window.AppState.activeMember) return;
            
            document.getElementById('editor-active-member-info').innerText = `Profissional: ${window.AppState.activeMember.name} (${window.AppState.activeMember.role})`;
            
            const dict = window.AppState.activeMember.role === 'PEF' ? healthPEF : healthTE;
            document.getElementById('health-container').innerHTML = Object.keys(dict).map(opt => `
                <label class="flex items-start space-x-2 cursor-pointer p-1 rounded hover:bg-black hover:bg-opacity-10 border border-transparent hover:border-gray-500 hover:border-opacity-20">
                    <input type="checkbox" value="${opt}" class="health-cb mt-0.5 rounded border-gray-400 text-primary w-3.5 h-3.5">
                    <span class="font-medium">${opt}</span>
                </label>`).join('');

            document.getElementById('stu-name').value = '';
            document.getElementById('stu-age').value = '';
            document.getElementById('stu-weight').value = '';
            document.getElementById('stu-height').value = '';
            document.getElementById('stu-recs').value = '';
            window.AppState.currentFichaId = null;
            window.AppState.workouts = [];
            window.addWorkout(); 
            window.calculateIMC();
            showScreen('screen-editor');
        };

        window.backToDashboard = () => {
            if(confirm("Sair sem salvar?")) showScreen('screen-dashboard');
        };

        // --- CÁLCULO AUTOMÁTICO DE IMC ATUALIZADO ---
        window.calculateIMC = () => {
            const w = parseFloat(document.getElementById('stu-weight').value);
            const h = parseFloat(document.getElementById('stu-height').value);
            const display = document.getElementById('imc-display');
            if (w > 0 && h > 0) {
                const imcVal = w / (h * h);
                const imc = imcVal.toFixed(1);
                
                let cls = "";
                if (imcVal < 18.5) {
                    cls = "Abaixo do peso";
                } else if (imcVal < 25) { // 18.5 a 24.9
                    cls = "Peso normal";
                } else if (imcVal < 30) { // 25 a 29.9
                    cls = "Sobrepeso";
                } else if (imcVal < 35) { // 30 a 34.9
                    cls = "Obesidade Grau I";
                } else if (imcVal < 40) { // 35 a 39.9
                    cls = "Obesidade Grau II";
                } else { // >= 40
                    cls = "Obesidade Grau III";
                }
                
                display.innerHTML = `<span class="bg-primary text-white px-2 py-0.5 rounded font-bold text-[10px]">IMC: ${imc} (${cls})</span>`;
            } else {
                display.innerHTML = '';
            }
        };

        window.changeTheme = () => document.body.setAttribute('data-theme', document.getElementById('stu-gender').value);

        // Workouts Management
        window.addWorkout = () => {
            const count = window.AppState.workouts.length;
            const title = count < 7 ? `TREINO ${daysOfWeek[count]}` : `NOVO TREINO ${count+1}`;
            window.AppState.workouts.push({ id: genId(), title, exercises: [] });
            window.renderWorkoutBoxes();
        };

        window.duplicateWorkout = (id) => {
            const w = window.AppState.workouts.find(x => x.id === id);
            if(w) {
                const nw = JSON.parse(JSON.stringify(w)); nw.id = genId(); nw.title += " (Cópia)";
                window.AppState.workouts.push(nw); window.renderWorkoutBoxes();
            }
        };

        window.removeWorkout = (id) => {
            if(confirm("Remover este dia inteiro?")) {
                window.AppState.workouts = window.AppState.workouts.filter(w => w.id !== id);
                window.renderWorkoutBoxes();
            }
        };
        window.updateWorkoutTitle = (id, val) => { const w = window.AppState.workouts.find(x => x.id === id); if(w) w.title = val; };

        // Exercise Management
        window.moveExercise = (wId, idx, dir) => {
            const w = window.AppState.workouts.find(x => x.id === wId);
            if(dir==='up' && idx>0) [w.exercises[idx], w.exercises[idx-1]] = [w.exercises[idx-1], w.exercises[idx]];
            if(dir==='down' && idx<w.exercises.length-1) [w.exercises[idx], w.exercises[idx+1]] = [w.exercises[idx+1], w.exercises[idx]];
            window.renderWorkoutBoxes();
        };
        window.removeExercise = (wId, idx) => { window.AppState.workouts.find(x => x.id === wId).exercises.splice(idx, 1); window.renderWorkoutBoxes(); };
        window.updateExercise = (wId, idx, field, val) => { window.AppState.workouts.find(x => x.id === wId).exercises[idx][field] = val; };

        window.renderWorkoutBoxes = () => {
            const container = document.getElementById('workouts-container');
            container.innerHTML = window.AppState.workouts.map(w => {
                let exHtml = w.exercises.length === 0 ? `<div class="text-center p-3 text-xs opacity-50 italic">Sem exercícios.</div>` : w.exercises.map((ex, idx) => `
                    <div class="flex flex-wrap sm:flex-nowrap gap-2 p-2 items-center border-b border-gray-500 border-opacity-20 last:border-0">
                        <div class="flex flex-col gap-1 w-full sm:w-auto">
                            <div class="text-[9px] opacity-60 uppercase">${ex.category}</div>
                            <div class="text-xs font-bold leading-tight">${ex.name}</div>
                        </div>
                        <div class="flex flex-1 gap-1 items-center justify-end">
                            <input type="text" value="${ex.sets}" onchange="updateExercise('${w.id}', ${idx}, 'sets', this.value)" class="input-field w-10 text-center text-xs p-1 rounded" placeholder="Sér">
                            <span class="opacity-50 text-xs">x</span>
                            <input type="text" value="${ex.reps}" onchange="updateExercise('${w.id}', ${idx}, 'reps', this.value)" class="input-field w-14 text-center text-xs p-1 rounded" placeholder="Rep">
                            <select onchange="updateExercise('${w.id}', ${idx}, 'technique', this.value)" class="input-field w-24 text-[10px] p-1 rounded">
                                ${dbTechniques.map(t => `<option value="${t}" ${ex.technique===t?'selected':''}>${t}</option>`).join('')}
                            </select>
                            <input type="text" value="${ex.obs}" onchange="updateExercise('${w.id}', ${idx}, 'obs', this.value)" class="input-field w-24 text-xs p-1 rounded" placeholder="Obs">
                            <div class="flex flex-col gap-0.5 ml-1">
                                <button onclick="moveExercise('${w.id}', ${idx}, 'up')" class="text-[10px] p-0.5 hover:bg-gray-600 rounded"><i class="fas fa-chevron-up"></i></button>
                                <button onclick="moveExercise('${w.id}', ${idx}, 'down')" class="text-[10px] p-0.5 hover:bg-gray-600 rounded"><i class="fas fa-chevron-down"></i></button>
                            </div>
                            <button onclick="removeExercise('${w.id}', ${idx})" class="text-red-500 hover:text-red-400 p-1 ml-1"><i class="fas fa-times"></i></button>
                        </div>
                    </div>`).join('');

                return `
                <div class="card rounded-xl overflow-hidden shadow-sm">
                    <div class="p-2 border-b flex justify-between items-center bg-black bg-opacity-10" style="border-color: var(--border-color);">
                        <input type="text" value="${w.title}" onchange="updateWorkoutTitle('${w.id}', this.value)" class="input-field bg-transparent font-bold text-sm w-2/3 px-2 py-1 rounded uppercase border-transparent focus:border-primary">
                        <div class="flex gap-1">
                            <button onclick="duplicateWorkout('${w.id}')" class="text-xs p-1.5 hover:bg-black hover:bg-opacity-20 rounded" title="Duplicar"><i class="fas fa-copy"></i></button>
                            <button onclick="removeWorkout('${w.id}')" class="text-xs p-1.5 text-red-500 hover:bg-red-500 hover:text-white rounded" title="Excluir"><i class="fas fa-trash"></i></button>
                        </div>
                    </div>
                    <div>${exHtml}</div>
                    <button onclick="openModal('${w.id}')" class="w-full text-primary py-2 text-xs font-bold hover:bg-primary hover:bg-opacity-10 transition border-t" style="border-color: var(--border-color);">+ ADICIONAR EXERCÍCIO</button>
                </div>`;
            }).join('');
        };

        // --- EXERCISE MODAL & DB ---
        window.openModal = (wId) => { window.AppState.activeModalWorkoutId = wId; document.getElementById('exercise-modal').classList.remove('hidden'); renderModalCategories(); renderModalExercises(); };
        window.closeModal = () => document.getElementById('exercise-modal').classList.add('hidden');
        window.setModalCategory = (cat) => { window.AppState.activeCategory = cat; renderModalCategories(); renderModalExercises(); };
        
        const getMergedExercises = (cat) => {
            const base = baseCategories[cat] || [];
            const custom = window.AppState.customExercises.filter(c => c.category === cat).map(c => c.name + " ⭐");
            return [...base, ...custom];
        };

        window.renderModalCategories = () => {
            document.getElementById('modal-categories').innerHTML = Object.keys(baseCategories).map(cat => `
                <button onclick="setModalCategory('${cat}')" class="w-full text-left px-2 py-2 rounded text-xs font-bold transition ${window.AppState.activeCategory === cat ? 'bg-primary text-white' : 'hover:bg-black hover:bg-opacity-20'}">${cat}</button>
            `).join('');
        };

        window.renderModalExercises = () => {
            const list = getMergedExercises(window.AppState.activeCategory);
            document.getElementById('modal-exercises').innerHTML = list.map(ex => {
                const isCardio = window.AppState.activeCategory === "🫀 CARDIO";
                const exRealName = ex.replace(" ⭐", "");
                const isCustom = ex.includes("⭐");
                let delBtn = isCustom ? `<button onclick="deleteCustomExercise(event, '${exRealName}')" class="text-red-500 ml-2 hover:text-red-300"><i class="fas fa-trash"></i></button>` : '';
                
                return `<div class="card p-2 rounded text-xs font-medium border hover:border-primary transition flex justify-between items-center group cursor-pointer" onclick="addExerciseToWorkout('${exRealName}', ${isCardio})">
                    <span>${ex}</span>
                    <div class="flex items-center"><i class="fas fa-plus opacity-0 group-hover:opacity-100 transition text-primary"></i>${delBtn}</div>
                </div>`;
            }).join('');
        };

        window.addExerciseToWorkout = (exName, isCardio) => {
            const w = window.AppState.workouts.find(x => x.id === window.AppState.activeModalWorkoutId);
            if(w) {
                w.exercises.push({ category: window.AppState.activeCategory, name: exName, sets: isCardio ? '1' : '3', reps: isCardio ? '-' : '10 a 12', technique: 'Nenhuma', obs: '' });
                window.renderWorkoutBoxes();
                document.getElementById('modal-exercises').style.opacity = '0.5'; setTimeout(() => document.getElementById('modal-exercises').style.opacity = '1', 100);
            }
        };

        // Custom Exercises Logic
        window.openCustomExerciseModal = () => {
            document.getElementById('custom-ex-cat').innerHTML = Object.keys(baseCategories).map(c => `<option>${c}</option>`).join('');
            document.getElementById('custom-ex-cat').value = window.AppState.activeCategory;
            document.getElementById('custom-ex-name').value = '';
            document.getElementById('custom-exercise-modal').classList.remove('hidden');
        };
        window.closeCustomExerciseModal = () => document.getElementById('custom-exercise-modal').classList.add('hidden');
        
        window.saveCustomExercise = async () => {
            const cat = document.getElementById('custom-ex-cat').value;
            const name = document.getElementById('custom-ex-name').value.trim();
            if(name && window.AppState.userId) {
                await addDoc(collection(db, `${getBasePath()}/custom_exercises`), { category: cat, name: name });
                closeCustomExerciseModal();
                setModalCategory(cat); 
            }
        };

        window.deleteCustomExercise = async (e, name) => {
            e.stopPropagation();
            if(confirm(`Excluir exercício customizado '${name}' da sua base de dados na nuvem?`)) {
                const target = window.AppState.customExercises.find(c => c.name === name && c.category === window.AppState.activeCategory);
                if(target) {
                    await deleteDoc(doc(db, `${getBasePath()}/custom_exercises`, target.id));
                    renderModalExercises(); 
                }
            }
        };

        // --- SALVAR & HISTÓRICO (NUVEM) ---
        const getWorkoutPayload = () => {
            return {
                memberId: window.AppState.activeMember.id,
                unitId: window.AppState.activeMember.unitId,
                networkId: window.AppState.activeMember.networkId,
                dateStr: new Date().toISOString().slice(0,10),
                timestamp: Date.now(),
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
                workouts: window.AppState.workouts
            };
        };

        window.saveToCloud = async () => {
            if(!window.AppState.userId || !window.AppState.activeMember) return false;
            showLoading("Salvando Ficha na Nuvem...");
            try {
                const payload = getWorkoutPayload();
                if(window.AppState.currentFichaId) {
                    await updateDoc(doc(db, `${getBasePath()}/workouts`, window.AppState.currentFichaId), payload);
                } else {
                    const docRef = await addDoc(collection(db, `${getBasePath()}/workouts`), payload);
                    window.AppState.currentFichaId = docRef.id;
                }
                hideLoading();
                return true;
            } catch(e) { console.error(e); alert("Erro ao salvar"); hideLoading(); return false; }
        };

        window.openCloudHistory = () => {
            const filter = document.getElementById('hist-filter-unit');
            filter.innerHTML = `<option value="ALL">Todas as Unidades</option>` + window.AppState.units.map(u => `<option value="${u.id}">${u.name}</option>`).join('');
            document.getElementById('history-modal').classList.remove('hidden');
            renderHistoryList();
        };
        window.closeCloudHistory = () => document.getElementById('history-modal').classList.add('hidden');

        window.renderHistoryList = () => {
            const unitFilter = document.getElementById('hist-filter-unit').value;
            let list = window.AppState.workoutsHistory.sort((a,b) => b.timestamp - a.timestamp); 
            if(unitFilter !== 'ALL') list = list.filter(w => w.unitId === unitFilter);

            const container = document.getElementById('history-list');
            if(list.length === 0) { container.innerHTML = `<p class="text-center opacity-50 py-10 text-sm">Nenhum registro encontrado.</p>`; return; }

            const now = Date.now();
            container.innerHTML = list.map(w => {
                const mem = window.AppState.members.find(m => m.id === w.memberId);
                const memName = mem ? mem.name : 'Desconhecido';
                
                let valDays = 30;
                if(w.stuValidity) valDays = parseInt(w.stuValidity.replace(/\D/g, '')) || 30;
                const expirationDate = w.timestamp + (valDays * 24 * 60 * 60 * 1000);
                const isExpired = now > expirationDate;
                const expiredTag = isExpired ? `<span class="bg-red-500 text-white text-[10px] px-1.5 py-0.5 rounded font-bold ml-2">VENCIDA</span>` : '';

                return `
                <div class="card p-3 rounded flex justify-between items-center border border-gray-500 border-opacity-20 hover:border-primary transition">
                    <div>
                        <div class="font-bold text-sm flex items-center">${w.studentName} ${expiredTag}</div>
                        <div class="text-[10px] opacity-70">Prescrito em: ${new Date(w.timestamp).toLocaleDateString()} | Por: ${memName}</div>
                    </div>
                    <div class="flex gap-2">
                        <button onclick="loadCloudWorkout('${w.id}')" class="btn-primary px-3 py-1.5 rounded text-xs font-bold">CARREGAR</button>
                        <button onclick="deleteCloudWorkout('${w.id}')" class="bg-red-500 hover:bg-red-600 text-white px-2 py-1.5 rounded text-xs"><i class="fas fa-trash"></i></button>
                    </div>
                </div>`;
            }).join('');
        };

        window.loadCloudWorkout = (id) => {
            const w = window.AppState.workoutsHistory.find(x => x.id === id);
            if(!w) return;
            
            const creator = window.AppState.members.find(m => m.id === w.memberId);
            if(creator) window.AppState.activeMember = creator;
            document.getElementById('editor-active-member-info').innerText = `Profissional: ${window.AppState.activeMember.name} (${window.AppState.activeMember.role})`;
            
            window.AppState.currentFichaId = w.id;
            document.getElementById('stu-name').value = w.studentName || '';
            document.getElementById('stu-age').value = w.stuAge || '';
            document.getElementById('stu-weight').value = w.stuWeight || '';
            document.getElementById('stu-height').value = w.stuHeight || '';
            document.getElementById('stu-gender').value = w.stuGender || 'Masculino';
            document.getElementById('stu-level').value = w.stuLevel || 'Iniciante';
            document.getElementById('stu-objective').value = w.stuObjective || 'Emagrecimento';
            document.getElementById('stu-freq').value = w.stuFreq || '3 dias';
            document.getElementById('stu-validity').value = w.stuValidity || 'Validade: 30 dias';
            document.getElementById('stu-recs').value = w.stuRecs || '';

            changeTheme(); calculateIMC();

            const dict = window.AppState.activeMember.role === 'PEF' ? healthPEF : healthTE;
            document.getElementById('health-container').innerHTML = Object.keys(dict).map(opt => `
                <label class="flex items-start space-x-2 cursor-pointer p-1 rounded hover:bg-black hover:bg-opacity-10">
                    <input type="checkbox" value="${opt}" ${(w.health||[]).includes(opt)?'checked':''} class="health-cb mt-0.5 rounded text-primary w-3.5 h-3.5">
                    <span class="font-medium">${opt}</span>
                </label>`).join('');

            window.AppState.workouts = w.workouts || [];
            renderWorkoutBoxes();
            closeCloudHistory();
        };

        window.deleteCloudWorkout = async (id) => {
            if(confirm("Excluir esta ficha permanentemente da nuvem?")) {
                await deleteDoc(doc(db, `${getBasePath()}/workouts`, id));
                if(window.AppState.currentFichaId === id) window.AppState.currentFichaId = null;
            }
        };

        // --- RELATÓRIO DE PRODUTIVIDADE ---
        window.updateFilters = () => {
            const uSelect = document.getElementById('report-unit');
            if(uSelect) uSelect.innerHTML = `<option value="ALL">Todas as Unidades</option>` + window.AppState.units.map(u => `<option value="${u.id}">${u.name}</option>`).join('');
            
            const mSelect = document.getElementById('report-month');
            if(mSelect && mSelect.options.length === 0) {
                const d = new Date();
                for(let i=0; i<6; i++) {
                    const month = d.toISOString().slice(0,7); 
                    mSelect.innerHTML += `<option value="${month}">${month}</option>`;
                    d.setMonth(d.getMonth() - 1);
                }
            }
        };

        window.openReportModal = () => { updateFilters(); document.getElementById('report-modal').classList.remove('hidden'); generateReportData(); };
        window.closeReportModal = () => { document.getElementById('report-modal').classList.add('hidden'); document.getElementById('print-report-area').classList.remove('active-print'); };

        window.generateReportData = () => {
            const month = document.getElementById('report-month').value;
            const unitId = document.getElementById('report-unit').value;
            
            let filtered = window.AppState.workoutsHistory.filter(w => w.dateStr.startsWith(month));
            if(unitId !== 'ALL') filtered = filtered.filter(w => w.unitId === unitId);

            const counts = {};
            filtered.forEach(w => { counts[w.memberId] = (counts[w.memberId] || 0) + 1; });

            const sortedMembers = Object.keys(counts).map(mId => {
                const mem = window.AppState.members.find(m => m.id === mId);
                const unit = mem ? window.AppState.units.find(u => u.id === mem.unitId) : null;
                return { name: mem?mem.name:'Removido', unitName: unit?unit.name:'-', count: counts[mId] };
            }).sort((a,b) => b.count - a.count);

            const total = filtered.length;

            let html = `
                <div class="mb-4">
                    <div class="text-3xl font-bold text-primary">${total}</div>
                    <div class="text-xs uppercase opacity-70">Total de Fichas Geradas</div>
                </div>
                <table class="w-full text-sm border border-gray-600 border-opacity-30 rounded overflow-hidden">
                    <thead class="bg-black bg-opacity-20 text-left">
                        <tr><th class="p-2">Profissional</th><th class="p-2">Unidade</th><th class="p-2 text-center">Qtd. Fichas</th></tr>
                    </thead>
                    <tbody>
            `;
            if(sortedMembers.length === 0) html += `<tr><td colspan="3" class="p-4 text-center italic opacity-50">Nenhum dado neste período.</td></tr>`;
            else {
                sortedMembers.forEach(m => {
                    html += `<tr class="border-t border-gray-600 border-opacity-20 hover:bg-black hover:bg-opacity-5"><td class="p-2 font-bold">${m.name}</td><td class="p-2">${m.unitName}</td><td class="p-2 text-center text-primary font-bold">${m.count}</td></tr>`;
                });
            }
            html += `</tbody></table>`;
            document.getElementById('report-content').innerHTML = html;
        };

        window.printReport = () => {
            const content = document.getElementById('report-content').innerHTML;
            const month = document.getElementById('report-month').value;
            const unit = document.getElementById('report-unit').options[document.getElementById('report-unit').selectedIndex].text;
            
            document.getElementById('print-report-area').innerHTML = `
                <h1>Relatório Corporativo - PowFit Pro</h1>
                <p><strong>Mês Referência:</strong> ${month}</p>
                <p><strong>Unidade(s):</strong> ${unit}</p>
                ${content}
            `;
            
            document.getElementById('print-report-area').classList.add('active-print');
            window.print();
            setTimeout(() => document.getElementById('print-report-area').classList.remove('active-print'), 1000);
        };

        // --- IMPRESSÃO DA FICHA E CLASSIFICAÇÃO DE IMC NA FOLHA ---
        window.saveAndPrint = async () => {
            const saved = await saveToCloud();
            if(!saved) return;

            const d = getWorkoutPayload();
            const isPEF = window.AppState.activeMember.role === 'PEF';
            const healthSourceDict = isPEF ? healthPEF : healthTE;
            
            let imcStr = "-"; 
            const w = parseFloat(d.stuWeight); 
            const h = parseFloat(d.stuHeight);
            
            if(w > 0 && h > 0) {
                const imcVal = w / (h * h);
                const imc = imcVal.toFixed(1);
                let cls = "";
                if (imcVal < 18.5) {
                    cls = "Abaixo do peso";
                } else if (imcVal < 25) {
                    cls = "Peso normal";
                } else if (imcVal < 30) {
                    cls = "Sobrepeso";
                } else if (imcVal < 35) {
                    cls = "Obesidade Grau I";
                } else if (imcVal < 40) {
                    cls = "Obesidade Grau II";
                } else {
                    cls = "Obesidade Grau III";
                }
                imcStr = `${imc} (${cls})`;
            }

            const objRec = objectiveData[d.stuObjective] || "";
            const profLabel = isPEF ? 'Profissional de Educação Física' : 'Treinador Esportivo';
            const crefText = isPEF ? `CREF: ${window.AppState.activeMember.cref} - ${window.AppState.activeMember.state}` : '';
            
            let legalTextPEF = `⚠️ OBSERVAÇÃO LEGAL – PROFISSIONAL DE EDUCAÇÃO FÍSICA: Conforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º estabelece que compete ao profissional coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos especializados nas áreas de atividades físicas e do desporto.`;
            let legalTextTE = `⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO: Conforme a Lei nº 14.597/2023 (Lei Geral do Esporte), Art. 75, a profissão de treinador esportivo é reconhecida e regulada no Brasil, com atuação de caráter técnico e esportivo voltada à preparação, supervisão, orientação e acompanhamento de treinos. As recomendações desta ficha possuem caráter informativo e orientativo. Este material não substitui avaliação médica. Em casos de dores, lesões, doenças ou qualquer condição, procure um profissional habilitado. O treinamento respeita os limites individuais, priorizando segurança e evolução progressiva dentro da atuação técnica.`;

            let html = `
                <div class="print-header">
                    <h1 class="print-title">Planilha de Treinamento - ${d.stuObjective}</h1>
                    <div class="prof-info">Prescrição feita por: ${window.AppState.activeMember.name} (${profLabel})<br>${crefText}</div>
                </div>
                <div class="print-grid">
                    <div><strong>Aluno:</strong> ${d.studentName}</div><div><strong>Idade:</strong> ${d.stuAge||'-'}</div><div><strong>Gênero:</strong> ${d.stuGender}</div>
                    <div><strong>Peso:</strong> ${d.stuWeight||'-'} kg</div><div><strong>Altura:</strong> ${d.stuHeight||'-'} m</div><div><strong>IMC:</strong> ${imcStr}</div>
                    <div><strong>Nível:</strong> ${d.stuLevel}</div><div><strong>Freq:</strong> ${d.stuFreq}</div><div><strong>Validade:</strong> ${d.stuValidity.replace('Validade: ','')}</div>
                </div>`;

            if (objRec || d.health.length > 0) {
                html += `<div class="print-guidelines"><h4>Diretrizes Automáticas de Perfil</h4><ul>`;
                if(objRec) html += `<li><strong>Objetivo (${d.stuObjective}):</strong> ${objRec}</li>`;
                d.health.forEach(k => { if(healthSourceDict[k]) html += `<li><strong>Saúde (${k.replace(/[\u{1F300}-\u{1F9FF}\u{2600}-\u{26FF}\u{2700}-\u{27BF}\u{FE0F}]/gu, '').trim()}):</strong> ${healthSourceDict[k]}</li>`; });
                html += `</ul></div>`;
            }

            d.workouts.forEach(wk => {
                html += `<div class="print-workout"><h3>${wk.title}</h3><table><thead><tr>
                    <th style="width:35%">Exercício</th><th style="width:8%;text-align:center;">Séries</th><th style="width:12%;text-align:center;">Reps</th><th style="width:15%">Técnica</th><th style="width:30%">Observações</th>
                </tr></thead><tbody>`;
                if (wk.exercises.length === 0) html += `<tr><td colspan="5" style="text-align:center; font-style:italic; padding:10px;">Sem exercícios</td></tr>`;
                else {
                    wk.exercises.forEach(ex => {
                        html += `<tr><td><strong>${ex.name}</strong></td><td style="text-align:center;">${ex.sets}</td><td style="text-align:center;">${ex.reps}</td><td>${ex.technique!=='Nenhuma'?ex.technique:'-'}</td><td>${ex.obs||'-'}</td></tr>`;
                    });
                }
                html += `</tbody></table></div>`;
            });

            html += `<div class="print-footer-section">`;
            if(d.stuRecs.trim()) html += `<strong>Recomendações Específicas do Profissional:</strong><br><p style="white-space: pre-wrap; margin: 3px 0 10px 0;">${d.stuRecs}</p>`;
            html += `<div class="legal-text">${isPEF ? legalTextPEF : legalTextTE}</div>
                     <div style="text-align:center; margin-top: 15px; font-size: 8px;">ID Ficha: ${window.AppState.currentFichaId} | PowFit Pro Corporativo</div></div>`;

            document.getElementById('print-area').innerHTML = html;
            document.getElementById('print-area').classList.add('active-print');
            
            window.print();
            setTimeout(() => document.getElementById('print-area').classList.remove('active-print'), 1000);
        };

    </script>
</body>
</html>
