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
            /* Tema Masculino (Dark Fitness) */
            --bg-body: #0f172a; --bg-card: #1e293b; --text-main: #f8fafc;
            --text-muted: #94a3b8; --border-color: #334155; --primary: #3b82f6;
            --primary-hover: #2563eb; --input-bg: #0f172a; --input-border: #475569;
        }
        [data-theme="Feminino"] {
            /* Tema Feminino (Light Elegante) */
            --bg-body: #fdf2f8; --bg-card: #ffffff; --text-main: #1e293b;
            --text-muted: #64748b; --border-color: #fbcfe8; --primary: #ec4899;
            --primary-hover: #db2777; --input-bg: #f8fafc; --input-border: #f1f5f9;
        }
        body { font-family: 'Inter', sans-serif; background-color: var(--bg-body); color: var(--text-main); transition: background-color 0.3s ease, color 0.3s ease; }
        .card { background-color: var(--bg-card); border: 1px solid var(--border-color); }
        .input-field { background-color: var(--input-bg); border: 1px solid var(--input-border); color: var(--text-main); }
        .input-field:focus { outline: none; border-color: var(--primary); box-shadow: 0 0 0 1px var(--primary); }
        .btn-primary { background-color: var(--primary); color: white; transition: background-color 0.2s; }
        .btn-primary:hover { background-color: var(--primary-hover); }
        
        /* Gestão de Telas */
        .screen { display: none; }
        .screen.active { display: block; }
        
        /* Áreas de Impressão (Ocultas na Tela) */
        #print-area, #print-report-area { display: none; }

        /* Estilização Estrita de Impressão (Planilha A4) */
        @media print {
            /* Garante que o Tailwind não corta a página em branco */
            html, body { height: auto !important; min-height: auto !important; overflow: visible !important; display: block !important; background: white !important; color: black !important; margin: 0; padding: 0; font-family: Arial, sans-serif; }
            .screen, .no-print, .modal-backdrop { display: none !important; }
            
            /* FICHA DE TREINO */
            #print-area.active-print { display: block !important; padding: 10mm 15mm; font-size: 10px; }
            #print-area .print-header { border-bottom: 2px solid #000; padding-bottom: 8px; margin-bottom: 12px; text-align: center; }
            #print-area .print-title { font-size: 18px; font-weight: bold; margin: 0; text-transform: uppercase; }
            #print-area .prof-info { font-size: 12px; margin-top: 5px; font-weight: bold; }
            #print-area .print-grid { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 4px; margin-bottom: 15px; border: 1px solid #000; padding: 8px; }
            #print-area .print-guidelines { border: 1px solid #000; padding: 8px; margin-bottom: 15px; font-size: 9.5px; }
            #print-area .print-guidelines h4 { margin: 0 0 4px 0; font-size: 11px; text-transform: uppercase; background: #eee; padding: 3px; -webkit-print-color-adjust: exact; color-adjust: exact; }
            #print-area .print-guidelines ul { margin: 0; padding-left: 15px; }
            #print-area .print-workout { margin-bottom: 15px; page-break-inside: avoid; }
            #print-area .print-workout h3 { background: #e0e0e0; border: 1px solid #000; border-bottom: none; padding: 4px 8px; margin: 0; font-size: 12px; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            #print-area table { width: 100%; border-collapse: collapse; border: 1px solid #000; }
            #print-area th, #print-area td { border: 1px solid #000; padding: 4px 6px; text-align: left; font-size: 10px; }
            #print-area th { background-color: #f0f0f0; font-weight: bold; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            #print-area .print-footer-section { margin-top: 15px; border: 1px solid #000; padding: 8px; font-size: 9px; page-break-inside: avoid; }
            #print-area .legal-text { font-size: 8px; color: #333; text-align: justify; margin-top: 5px; font-style: italic; }

            /* RELATÓRIO DE PRODUTIVIDADE */
            #print-report-area.active-print { display: block !important; padding: 15mm; }
            #print-report-area h1 { text-align: center; border-bottom: 2px solid #000; padding-bottom: 10px; margin-bottom: 20px;}
            #print-report-area table { width: 100%; border-collapse: collapse; margin-top: 20px;}
            #print-report-area th, #print-report-area td { border: 1px solid #000; padding: 8px; text-align: left; }
            #print-report-area th { background-color: #eee; -webkit-print-color-adjust: exact; color-adjust: exact; }
            @page { size: A4; margin: 0; }
        }

        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
    </style>
</head>
<body>

    <!-- OVERLAYS & MODAIS GLOBAIS -->
    
    <!-- Loading -->
    <div id="loading-overlay" class="fixed inset-0 bg-black bg-opacity-90 z-[100] flex flex-col items-center justify-center text-white modal-backdrop hidden">
        <i class="fas fa-circle-notch fa-spin text-4xl text-primary mb-4"></i>
        <p id="loading-text" class="text-lg font-medium">Sincronizando Nuvem...</p>
    </div>

    <!-- Custom Confirm / Alert Modal -->
    <div id="custom-alert" class="fixed inset-0 bg-black bg-opacity-80 z-[200] flex items-center justify-center p-4 modal-backdrop hidden">
        <div class="bg-gray-800 border border-gray-600 rounded-xl max-w-sm w-full p-6 text-white shadow-2xl">
            <h3 id="alert-title" class="text-lg font-bold mb-2">Aviso</h3>
            <p id="alert-msg" class="text-sm text-gray-300 mb-6"></p>
            <div class="flex justify-end gap-3" id="alert-buttons"></div>
        </div>
    </div>

    <!-- TELA 1: LOGIN -->
    <div id="screen-login" class="screen active min-h-screen flex items-center justify-center p-4">
        <div class="card p-8 rounded-2xl shadow-2xl max-w-md w-full text-center">
            <i class="fas fa-dumbbell text-5xl text-primary mb-4"></i>
            <h1 class="text-3xl font-bold mb-2">PowFit Pro</h1>
            <p class="text-sm opacity-70 mb-8">Master Edition - Cloud & Gestão</p>
            
            <button onclick="handleLogin()" class="w-full bg-white text-gray-800 font-bold py-3 px-4 rounded-lg shadow flex items-center justify-center gap-3 hover:bg-gray-100 transition">
                <i class="fab fa-google text-red-500"></i> Entrar com Google
            </button>
            <p class="text-[10px] opacity-50 mt-6">Dados salvos de forma segura no Firebase.</p>
        </div>
    </div>

    <!-- TELA 2: DASHBOARD (REDES E EQUIPES) -->
    <div id="screen-dashboard" class="screen min-h-screen pb-20">
        <div class="max-w-7xl mx-auto p-4 sm:p-6 lg:p-8">
            <div class="flex flex-col md:flex-row justify-between items-center mb-8 border-b border-opacity-20 pb-4 gap-4" style="border-color: var(--border-color)">
                <div>
                    <h1 class="text-2xl font-bold tracking-tight"><i class="fas fa-network-wired text-primary mr-2"></i> Dashboard Corporativo</h1>
                    <p class="text-sm opacity-70">Gestão de Franquias, Unidades e Equipe</p>
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

            <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                <!-- 1. REDES -->
                <div class="card rounded-xl p-4 shadow-sm flex flex-col h-[65vh]">
                    <div class="flex justify-between items-center mb-4">
                        <h2 class="font-bold text-lg">1. Redes de Franquias</h2>
                        <button onclick="promptCreateNetwork()" class="text-primary hover:text-white transition" title="Nova Rede"><i class="fas fa-plus-circle text-xl"></i></button>
                    </div>
                    <div id="list-networks" class="flex-1 overflow-y-auto space-y-2"></div>
                </div>

                <!-- 2. UNIDADES -->
                <div class="card rounded-xl p-4 shadow-sm flex flex-col h-[65vh]">
                    <div class="flex justify-between items-center mb-4">
                        <h2 class="font-bold text-lg">2. Unidades</h2>
                        <button onclick="promptCreateUnit()" id="btn-add-unit" class="text-primary hover:text-white transition hidden" title="Nova Unidade"><i class="fas fa-plus-circle text-xl"></i></button>
                    </div>
                    <p id="msg-select-network" class="text-xs opacity-50 italic text-center mt-10">Selecione uma Rede primeiro.</p>
                    <div id="list-units" class="flex-1 overflow-y-auto space-y-2 hidden"></div>
                </div>

                <!-- 3. MEMBROS -->
                <div class="card rounded-xl p-4 shadow-sm flex flex-col h-[65vh]">
                    <div class="flex justify-between items-center mb-4">
                        <h2 class="font-bold text-lg">3. Equipe (Membros)</h2>
                        <button onclick="openMemberModal()" id="btn-add-member" class="text-primary hover:text-white transition hidden" title="Novo Membro"><i class="fas fa-user-plus text-xl"></i></button>
                    </div>
                    <p id="msg-select-unit" class="text-xs opacity-50 italic text-center mt-10">Selecione uma Unidade primeiro.</p>
                    <div id="list-members" class="flex-1 overflow-y-auto space-y-2 hidden"></div>
                </div>
            </div>
        </div>
    </div>

    <!-- TELA 3: PRANCHETA DIGITAL (EDITOR) -->
    <div id="screen-editor" class="screen min-h-screen pb-20">
        <div class="max-w-7xl mx-auto p-4 sm:p-6 lg:p-8">
            
            <!-- Header Editor -->
            <div class="flex flex-col lg:flex-row justify-between items-center mb-6 gap-4 bg-black bg-opacity-20 p-4 rounded-xl border" style="border-color: var(--border-color)">
                <div class="flex items-center gap-4">
                    <button onclick="backToDashboard()" class="text-gray-400 hover:text-white text-2xl" title="Voltar ao Dashboard"><i class="fas fa-arrow-left"></i></button>
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
                        <i class="fas fa-save"></i> <i class="fas fa-print"></i> Salvar & Imprimir
                    </button>
                </div>
            </div>

            <div class="grid grid-cols-1 xl:grid-cols-12 gap-6">
                <!-- Coluna Esquerda: Dados & Saúde -->
                <div class="xl:col-span-3 space-y-4">
                    <!-- Aluno -->
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
                                <option value="15">Validade: 15 dias</option><option value="30" selected>Validade: 30 dias</option>
                                <option value="60">Validade: 60 dias</option><option value="90">Validade: 90 dias</option>
                            </select>
                        </div>
                    </div>

                    <!-- Saúde -->
                    <div class="card rounded-xl p-4 shadow-sm">
                        <h2 class="text-md font-semibold mb-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)"><i class="fas fa-heartbeat text-primary"></i> Estado de Saúde</h2>
                        <div class="grid grid-cols-1 gap-1 text-[11px]" id="health-container"></div>
                    </div>

                    <!-- Recs. Livres -->
                    <div class="card rounded-xl p-4 shadow-sm">
                        <h2 class="text-md font-semibold mb-2"><i class="fas fa-pen text-primary"></i> Recs. Extras</h2>
                        <textarea id="stu-recs" rows="3" class="input-field w-full rounded px-2 py-1.5 text-xs" placeholder="Hidratação, descanso, cuidados..."></textarea>
                    </div>
                </div>

                <!-- Coluna Direita: Treinos -->
                <div class="xl:col-span-9 space-y-4">
                    <div class="flex justify-between items-center bg-opacity-10 p-3 rounded-xl card border-dashed border-2">
                        <h2 class="text-lg font-bold"><i class="fas fa-dumbbell text-primary"></i> Montagem Livre</h2>
                        <button onclick="addWorkoutDay()" class="btn-primary px-3 py-1.5 rounded text-xs font-medium"><i class="fas fa-plus"></i> Novo Dia</button>
                    </div>
                    <div id="workouts-container" class="space-y-4"></div>
                </div>
            </div>
        </div>
    </div>

    <!-- MODAIS E OVERLAYS INTERNOS -->

    <!-- Adicionar Membro -->
    <div id="modal-member" class="fixed inset-0 bg-black bg-opacity-80 z-50 flex items-center justify-center hidden modal-backdrop">
        <div class="card p-6 rounded-xl w-full max-w-md">
            <h2 class="text-xl font-bold mb-4">Novo Membro na Equipe</h2>
            <div class="space-y-3">
                <input type="text" id="mem-name" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="Nome Completo">
                <input type="text" id="mem-cpf" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="CPF">
                <select id="mem-role" onchange="toggleMemRole()" class="input-field w-full rounded px-3 py-2 text-sm font-bold text-primary">
                    <option value="PEF">Profissional de Educação Física (PEF)</option>
                    <option value="TE">Treinador Esportivo (TE)</option>
                </select>
                <div id="mem-cref-container" class="grid grid-cols-2 gap-2">
                    <input type="text" id="mem-cref" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="Nº CREF">
                    <input type="text" id="mem-state" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="Estado (UF)">
                </div>
            </div>
            <div class="flex justify-end gap-2 mt-6">
                <button onclick="closeMemberModal()" class="px-4 py-2 rounded text-sm bg-gray-700 hover:bg-gray-600">Cancelar</button>
                <button onclick="saveMember()" class="btn-primary px-4 py-2 rounded text-sm font-bold">Salvar</button>
            </div>
        </div>
    </div>

    <!-- Modal Banco de Exercícios -->
    <div id="exercise-modal" class="fixed inset-0 bg-black bg-opacity-80 z-50 flex items-center justify-center p-2 hidden modal-backdrop">
        <div class="card w-full max-w-5xl rounded-xl shadow-2xl flex flex-col h-[90vh]">
            <div class="p-3 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold"><i class="fas fa-list text-primary mr-2"></i> Banco de Exercícios</h3>
                <button onclick="closeExerciseModal()" class="text-gray-400 hover:text-red-500 text-2xl leading-none">&times;</button>
            </div>
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <div class="w-full md:w-1/4 border-r overflow-y-auto p-2 space-y-1" style="border-color: var(--border-color)" id="modal-categories"></div>
                <div class="w-full md:w-3/4 overflow-y-auto p-3 bg-black bg-opacity-5 flex flex-col">
                    <div class="flex justify-between items-center mb-3">
                        <span class="text-xs opacity-70">Clique no exercício para adicioná-lo ao treino.</span>
                        <button onclick="openCustomExModal()" class="bg-indigo-600 hover:bg-indigo-500 text-white px-3 py-1.5 rounded text-xs font-bold shadow flex gap-2 items-center"><i class="fas fa-plus"></i> Novo Exercício na Nuvem</button>
                    </div>
                    <div id="modal-exercises" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-2"></div>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal Criar Exercício Customizado -->
    <div id="custom-ex-modal" class="fixed inset-0 bg-black bg-opacity-90 z-[60] flex items-center justify-center hidden modal-backdrop">
        <div class="card p-6 rounded-xl w-full max-w-sm">
            <h2 class="text-lg font-bold mb-4 text-primary">Criar Exercício na Nuvem</h2>
            <p class="text-xs opacity-70 mb-4">Ele ficará disponível para todas as fichas futuras.</p>
            <div class="space-y-3">
                <select id="custom-ex-cat" class="input-field w-full rounded px-3 py-2 text-sm"></select>
                <input type="text" id="custom-ex-name" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="Nome do Exercício (Ex: Pulley Triângulo)">
            </div>
            <div class="flex justify-end gap-2 mt-6">
                <button onclick="closeCustomExModal()" class="px-4 py-2 rounded text-sm bg-gray-700 hover:bg-gray-600">Cancelar</button>
                <button onclick="saveCustomEx()" class="btn-primary px-4 py-2 rounded text-sm font-bold">Salvar na Nuvem</button>
            </div>
        </div>
    </div>

    <!-- Modal Histórico -->
    <div id="history-modal" class="fixed inset-0 bg-black bg-opacity-80 z-50 flex items-center justify-center p-4 hidden modal-backdrop">
        <div class="card w-full max-w-4xl rounded-xl shadow-2xl flex flex-col max-h-[90vh]">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold"><i class="fas fa-cloud text-primary mr-2"></i> Histórico da Equipe</h3>
                <button onclick="closeCloudHistory()" class="text-gray-400 hover:text-red-500 text-2xl leading-none">&times;</button>
            </div>
            <div class="p-4 bg-black bg-opacity-20 flex items-center gap-2 text-sm border-b" style="border-color: var(--border-color)">
                <span class="font-bold text-primary"><i class="fas fa-filter"></i> Filtrar Unidade:</span>
                <select id="hist-filter" onchange="renderHistoryList()" class="input-field rounded px-2 py-1 text-xs"><option value="ALL">Todas</option></select>
            </div>
            <div class="p-4 overflow-y-auto flex-1 space-y-2" id="history-list"></div>
        </div>
    </div>

    <!-- Modal Relatório -->
    <div id="report-modal" class="fixed inset-0 bg-black bg-opacity-90 z-50 flex items-center justify-center p-4 hidden modal-backdrop">
        <div class="card w-full max-w-3xl rounded-xl shadow-2xl flex flex-col max-h-[90vh]">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold"><i class="fas fa-chart-bar text-primary mr-2"></i> Relatório de Produtividade</h3>
                <button onclick="closeReportModal()" class="text-gray-400 hover:text-red-500 text-2xl leading-none">&times;</button>
            </div>
            <div class="p-4 flex gap-3 flex-wrap">
                <select id="rep-month" class="input-field rounded px-3 py-1.5 text-sm flex-1"></select>
                <select id="rep-unit" class="input-field rounded px-3 py-1.5 text-sm flex-1"><option value="ALL">Todas as Unidades</option></select>
                <button onclick="generateReport()" class="btn-primary px-4 py-1.5 rounded text-sm font-bold">Filtrar</button>
            </div>
            <div class="p-4 overflow-y-auto flex-1" id="report-content"></div>
            <div class="p-4 border-t flex justify-end" style="border-color: var(--border-color)">
                <button onclick="printReport()" class="bg-green-600 hover:bg-green-700 text-white px-4 py-2 rounded text-sm font-bold shadow flex gap-2 items-center"><i class="fas fa-print"></i> Imprimir</button>
            </div>
        </div>
    </div>

    <!-- IMPRESSÕES -->
    <div id="print-area"></div>
    <div id="print-report-area"></div>

    <!-- FIREBASE & JAVASCRIPT -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, GoogleAuthProvider, signInWithPopup, signInWithCustomToken, signInAnonymously, signOut, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, doc, setDoc, addDoc, deleteDoc, updateDoc, onSnapshot } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // --- INICIALIZAÇÃO FIREBASE ---
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
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'powfit-pro-master';

        // --- ESTADO GERAL (APP STATE) ---
        window.PowFit = {
            userId: null,
            networks: [], units: [], members: [], customExercises: [], history: [],
            selNetId: null, selUnitId: null,
            activeMember: null,
            currentFichaId: null,
            workouts: [],
            activeModalCat: "🔥 PEITO",
            activeDayIdx: null
        };
        const getBasePath = () => `artifacts/${appId}/users/${window.PowFit.userId}`;

        // --- EVENTO DE LIMPEZA DE IMPRESSÃO ---
        // Aqui resolve o problema da folha em branco, limpando o estilo só depois do preview fechar
        window.addEventListener('afterprint', () => {
            document.getElementById('print-area').classList.remove('active-print');
            document.getElementById('print-report-area').classList.remove('active-print');
        });

        // --- CONSTANTES ---
        const healthPEF = { "🟢 Saudável": "Manter treinos regulares 3–5x por semana, combinando musculação e cardio. Foco em evolução e constância.", "⚪ Sedentário": "Iniciar com treinos leves 2–3x por semana. Priorizar adaptação, técnica e evitar excesso de carga.", "🟡 Sobrepeso": "Treinar 3–5x por semana com foco em gasto calórico. Combinar musculação e cardio moderado.", "🔴 Obesidade": "Iniciar com exercícios de baixo impacto. Evolução gradual, priorizando saúde e segurança.", "⚖️ Baixo peso": "Foco em musculação para ganho de massa. Treinos moderados com alimentação adequada.", "🍬 Diabetes": "Treinos regulares moderados. Monitorar glicemia e evitar treinos em jejum.", "❤️ Hipertensão": "Treinos moderados, evitar prender a respiração. Priorizar controle da intensidade.", "🔵 Hipotensão": "Evitar mudanças bruscas. Manter hidratação e intensidade leve a moderada.", "💔 Problemas cardíacos": "Treinos leves com liberação médica. Monitorar frequência cardíaca.", "🦴 Problemas articulares": "Evitar impacto. Priorizar exercícios controlados e máquinas.", "🫁 Problemas respiratórios": "Treinos leves a moderados com progressão gradual. Atenção à respiração.", "⚠️ Lesões": "Adaptar exercícios. Evitar dor e focar na recuperação.", "🤰 Gestante": "Treinos leves a moderados, sem impacto ou risco. Foco em mobilidade e bem-estar.", "🤱 Lactante": "Treinar normalmente com atenção à hidratação e energia.", "👴 Idoso": "Foco em força, equilíbrio e mobilidade. Intensidade moderada com segurança." };
        const healthTE = { "🟢 Saudável": "Manter treinos regulares de 3–5x por semana, combinando musculação e atividades cardiovasculares. A constância, descanso adequado e boa alimentação contribuem para melhores resultados.", "⚪ Sedentário": "O início deve ser gradual, com treinos leves e foco na adaptação do corpo. A prioridade é desenvolver constância, aprender a execução correta e evitar excesso de carga.", "🟡 Sobrepeso": "A prática regular de musculação associada ao cardio pode contribuir para melhora do condicionamento físico e composição corporal. A progressão deve respeitar a individualidade e o nível de adaptação.", "🔴 Obesidade": "É recomendado iniciar com exercícios de menor impacto e progressão gradual. A segurança, mobilidade e constância são prioridades durante o processo.", "⚖️ Baixo peso": "A musculação pode auxiliar no ganho de massa muscular quando associada a alimentação adequada e descanso. O treino deve priorizar evolução progressiva e recuperação muscular.", "🍬 Diabetes": "Pessoas com diabetes devem manter acompanhamento médico regular antes e durante a prática de exercícios. A musculação pode auxiliar na rotina de atividade física quando liberada por profissional de saúde. Em casos de tontura, mal-estar ou alteração de glicemia, o treino deve ser interrompido e o aluno deve procurar orientação médica.", "❤️ Hipertensão": "Pessoas com hipertensão devem manter acompanhamento médico regular e respeitar as orientações profissionais. Durante o treino, é importante evitar prender a respiração e controlar a intensidade dos exercícios.", "🔵 Hipotensão": "Pessoas com hipotensão devem evitar mudanças bruscas de posição durante o treino. Manter boa hidratação e respeitar a intensidade adequada ajuda a reduzir episódios de mal-estar.", "💔 Problemas cardíacos": "A prática de exercícios deve ocorrer apenas com liberação e acompanhamento médico. O treino deve respeitar limites individuais, com controle de intensidade e atenção aos sinais do corpo.", "🦴 Problemas articulares": "Exercícios com menor impacto e maior controle de movimento costumam ser mais indicados. O acompanhamento profissional e a adaptação dos exercícios ajudam na segurança durante o treino.", "🫁 Problemas respiratórios": "A progressão do treino deve ser gradual e respeitar a capacidade respiratória individual. Em caso de falta de ar excessiva ou desconforto, o exercício deve ser interrompido.", "⚠️ Lesões": "A adaptação dos exercícios deve respeitar a limitação existente e evitar dor durante a execução. Em situações de lesão, é importante seguir orientação profissional adequada antes da prática.", "🤰 Gestante": "A prática de exercícios deve ocorrer com liberação médica e acompanhamento adequado. O foco deve ser segurança, mobilidade e bem-estar, evitando impacto excessivo e situações de risco.", "🤱 Lactante": "A atividade física pode ser mantida normalmente, respeitando a recuperação individual e mantendo boa hidratação e alimentação adequada.", "👴 Idoso": "A musculação pode contribuir para força, equilíbrio, mobilidade e autonomia. O treino deve respeitar limitações individuais, com intensidade moderada e foco na segurança." };
        const objectivesDesc = { "Emagrecimento": "Foco em déficit calórico com treinos mistos de força (manter massa magra) e aeróbicos (maior gasto).", "Hipertrofia": "Prioridade na progressão de carga e volume adequado. Essencial superávit calórico e descanso.", "Definição": "Manutenção de massa muscular enquanto reduz o percentual de gordura. Atenção estrita à dieta.", "Condicionamento": "Treinos com menor tempo de intervalo, circuitos e alta integração cardiopulmonar.", "Resistência": "Séries mais longas, cadência controlada e aprimoramento da capacidade muscular.", "Força": "Cargas altas, baixas repetições e intervalos de descanso maiores.", "Reabilitação": "Treino focado em fortalecimento específico, mobilidade e controle motor. Respeitar limites.", "Saúde geral": "Equilíbrio entre força, cardio e flexibilidade. O principal objetivo é a constância e bem-estar." };
        const baseCategories = {
            "🔥 PEITO": ["Supino Reto", "Supino Inclinado", "Supino Inclinado com Halteres", "Supino Fechado com Halteres", "Cross Over", "Cross Over Alto", "Cross Over Baixo", "Crucifixo Reto", "Crucifixo Inclinado com Halteres", "Crucifixo na Máquina", "Peck Fly", "Peck Fly Unilateral", "Pullover", "Flexão de Braço", "Flexão com Pés Elevados", "Flexão Explosiva"],
            "🦍 COSTAS": ["Puxada Alta", "Puxada de Frente Supinada", "Pulldown", "Remada Aberta", "Remada Baixa", "Remada Curvada", "Remada Curvada Supinada", "Remada Curvada na Polia", "Remada Cavalinho (T-Bar)", "Remada Unilateral", "Remada no Cross", "Serrote", "Facepull (puxada de cima para baixo)"],
            "🦵 PERNAS": ["Agachamento Livre", "Agachamento Taça", "Agachamento no Smith", "Agachamento com barra", "Agachamento com passada lateral", "Squat", "Hack Machine", "Leg 45°", "Leg 90°", "Agachamento Sumô", "Agachamento Sissy (Livre)", "Afundo", "Recuo", "Avanço", "Passada", "Búlgaro", "Step-up", "Levantamento Terra", "Levantamento Terra Romeno", "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", "Cadeira Flexora", "Elevação Pélvica no Banco", "Elevação Pélvica no Chão", "Elevação Pélvica Unilateral no Chão", "Extensão de Quadril (Glúteo Máximo)", "Extensão Cruzada (Glúteo Médio)", "Coice", "Cachorrinho", "Cadeira Extensora", "Adução", "Abdução", "Abdução Inclinada", "Flexão Nórdica", "Flexão Nórdica Invertida", "Panturrilha Livre", "Panturrilha em Pé (Máquina)", "Panturrilha no Leg Press", "Panturrilha Banco", "Panturrilha Squat", "Panturrilha Unilateral"],
            "💪 BRAÇOS": ["(Bíceps) Rosca Direta", "(Bíceps) Rosca Alternada", "(Bíceps) Rosca 21", "(Bíceps) Rosca Scott Barra W", "(Bíceps) Rosca Scott Unilateral", "(Bíceps) Rosca Scott com Halteres", "(Bíceps) Rosca Martelo", "(Bíceps) Rosca Cross", "(Bíceps) Rosca Inversa", "(Bíceps) Rosca 45°", "(Tríceps) Pulley Unilateral", "(Tríceps) Pulley Barra", "(Tríceps) Pulley Corda", "(Tríceps) Pulley Pegada Inversa", "(Tríceps) Francês na Corda", "(Tríceps) Francês com Halter", "(Tríceps) Francês Unilateral", "(Tríceps) Cruzado Polia Dupla", "(Tríceps) Coice Unilateral", "(Tríceps) Arremesso", "(Tríceps) Testa", "(Tríceps) Mergulho no Banco"],
            "🪨 OMBROS": ["Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", "Elevação Lateral na Polia", "Elevação Lateral Unilateral Cross", "Elevação Lateral Sentado", "Desenvolvimento com Halteres", "Desenvolvimento com Barra", "Arnold Press", "Elevação Borboleta", "Crucifixo Inverso Sentado com Halteres", "Crucifixo Inverso na Polia", "Crucifixo Inverso Unilateral na Polia", "Facepull (puxada reta)", "Remada Alta", "Encolhimento (Trapézio)"],
            "🧠 ABDÔMEN": ["Infra com Elevação de Perna", "Abdominal Supra", "Abdominal Remador", "Abdominal Bicicleta", "Abdominal Twister com Peso", "Prancha", "Prancha Lateral", "Trituração de Cabos em Pé", "Isometria na parede"],
            "🫀 CARDIO": ["Bicicleta 10 Minutos", "Bicicleta 15 Minutos", "Bicicleta 20 Minutos", "Esteira 10 Minutos", "Esteira 15 Minutos", "Esteira 20 Minutos", "Pular Corda"]
        };
        const techniques = ["Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", "Negativa", "Isometria", "Parciais", "Pirâmide"];
        const wkDays = ["SEGUNDA-FEIRA", "TERÇA-FEIRA", "QUARTA-FEIRA", "QUINTA-FEIRA", "SEXTA-FEIRA", "SÁBADO", "DOMINGO"];

        // --- UTILITÁRIOS (MODAL CUSTOMIZADO) ---
        const generateId = () => Math.random().toString(36).substr(2, 9);
        const setLoading = (msg, state) => { 
            const el = document.getElementById('loading-overlay');
            if(state) { document.getElementById('loading-text').innerText = msg; el.classList.remove('hidden'); }
            else { el.classList.add('hidden'); }
        };

        window.customPrompt = (title, message) => {
            return new Promise((resolve) => {
                const modal = document.getElementById('custom-alert');
                document.getElementById('alert-title').innerText = title;
                document.getElementById('alert-msg').innerText = message;
                
                const input = document.createElement('input');
                input.className = "input-field w-full rounded px-3 py-2 text-sm mb-4";
                document.getElementById('alert-msg').appendChild(input);

                const btnContainer = document.getElementById('alert-buttons');
                btnContainer.innerHTML = '';
                
                const btnCancel = document.createElement('button');
                btnCancel.className = "px-4 py-2 rounded bg-gray-600 hover:bg-gray-500 text-sm font-bold";
                btnCancel.innerText = "Cancelar";
                btnCancel.onclick = () => { modal.classList.add('hidden'); resolve(null); };

                const btnSave = document.createElement('button');
                btnSave.className = "btn-primary px-4 py-2 rounded text-sm font-bold";
                btnSave.innerText = "Salvar";
                btnSave.onclick = () => { modal.classList.add('hidden'); resolve(input.value); };

                btnContainer.append(btnCancel, btnSave);
                modal.classList.remove('hidden');
                input.focus();
            });
        };

        window.customConfirm = (title, message) => {
            return new Promise((resolve) => {
                const modal = document.getElementById('custom-alert');
                document.getElementById('alert-title').innerText = title;
                document.getElementById('alert-msg').innerText = message;
                
                const btnContainer = document.getElementById('alert-buttons');
                btnContainer.innerHTML = '';

                const btnNo = document.createElement('button');
                btnNo.className = "px-4 py-2 rounded bg-gray-600 hover:bg-gray-500 text-sm font-bold";
                btnNo.innerText = "Não";
                btnNo.onclick = () => { modal.classList.add('hidden'); resolve(false); };

                const btnYes = document.createElement('button');
                btnYes.className = "bg-red-600 hover:bg-red-500 text-white px-4 py-2 rounded text-sm font-bold";
                btnYes.innerText = "Sim, Continuar";
                btnYes.onclick = () => { modal.classList.add('hidden'); resolve(true); };

                btnContainer.append(btnNo, btnYes);
                modal.classList.remove('hidden');
            });
        };

        window.customAlert = (title, message) => {
            const modal = document.getElementById('custom-alert');
            document.getElementById('alert-title').innerText = title;
            document.getElementById('alert-msg').innerText = message;
            
            const btnContainer = document.getElementById('alert-buttons');
            btnContainer.innerHTML = '';

            const btnOk = document.createElement('button');
            btnOk.className = "btn-primary px-4 py-2 rounded text-sm font-bold";
            btnOk.innerText = "OK";
            btnOk.onclick = () => { modal.classList.add('hidden'); };

            btnContainer.appendChild(btnOk);
            modal.classList.remove('hidden');
        };

        // --- AUTH ---
        window.handleLogin = async () => {
            setLoading("Autenticando...", true);
            try {
                if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                    await signInWithCustomToken(auth, __initial_auth_token);
                } else {
                    await signInWithPopup(auth, new GoogleAuthProvider());
                }
            } catch (e) {
                console.error(e);
                try { await signInAnonymously(auth); } catch(ex){}
            }
        };

        window.handleLogout = async () => { await signOut(auth); };

        onAuthStateChanged(auth, async (user) => {
            if (user) {
                window.PowFit.userId = user.uid;
                await initRealtime();
                document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
                document.getElementById('screen-dashboard').classList.add('active');
            } else {
                window.PowFit.userId = null;
                document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
                document.getElementById('screen-login').classList.add('active');
                setLoading(false);
                if(window.location.hostname.includes('google') || window.location.hostname.includes('sandbox')) {
                    window.handleLogin(); // Auto-login fallback
                }
            }
        });

        // --- REALTIME SYNC ---
        let unsubscribers = [];
        async function initRealtime() {
            setLoading("Sincronizando...", true);
            unsubscribers.forEach(u => u()); unsubscribers = [];
            const bp = getBasePath();

            unsubscribers.push(onSnapshot(collection(db, `${bp}/networks`), s => { window.PowFit.networks = s.docs.map(d=>({id:d.id, ...d.data()})); renderNetworks(); }));
            unsubscribers.push(onSnapshot(collection(db, `${bp}/units`), s => { window.PowFit.units = s.docs.map(d=>({id:d.id, ...d.data()})); renderUnits(); }));
            unsubscribers.push(onSnapshot(collection(db, `${bp}/members`), s => { window.PowFit.members = s.docs.map(d=>({id:d.id, ...d.data()})); renderMembers(); }));
            unsubscribers.push(onSnapshot(collection(db, `${bp}/custom_ex`), s => { window.PowFit.customExercises = s.docs.map(d=>({id:d.id, ...d.data()})); }));
            unsubscribers.push(onSnapshot(collection(db, `${bp}/history`), s => { window.PowFit.history = s.docs.map(d=>({id:d.id, ...d.data()})); }));

            setTimeout(() => setLoading(false), 800);
        }

        // --- GESTÃO DE REDES ---
        window.promptCreateNetwork = async () => {
            const name = await customPrompt("Nova Rede", "Digite o nome da franquia/rede:");
            if(name && name.trim()) await addDoc(collection(db, `${getBasePath()}/networks`), { name: name.trim() });
        };
        window.renderNetworks = () => {
            const l = document.getElementById('list-networks');
            l.innerHTML = window.PowFit.networks.map(n => `
                <div onclick="selectNetwork('${n.id}')" class="p-3 border rounded cursor-pointer transition flex justify-between items-center ${window.PowFit.selNetId === n.id ? 'border-primary bg-primary bg-opacity-20 text-primary font-bold' : 'border-gray-600 border-opacity-30 hover:bg-black hover:bg-opacity-20'}">
                    <span><i class="fas fa-building opacity-50 mr-2"></i> ${n.name}</span>
                    <button onclick="delNet(event, '${n.id}')" class="text-red-500 hover:text-red-400 p-1"><i class="fas fa-trash"></i></button>
                </div>`).join('');
            if(!window.PowFit.selNetId && window.PowFit.networks.length > 0) selectNetwork(window.PowFit.networks[0].id);
        };
        window.selectNetwork = (id) => { window.PowFit.selNetId = id; window.PowFit.selUnitId = null; document.getElementById('btn-add-unit').classList.remove('hidden'); document.getElementById('msg-select-network').classList.add('hidden'); document.getElementById('list-units').classList.remove('hidden'); renderNetworks(); renderUnits(); renderMembers(); };
        window.delNet = async (e, id) => { e.stopPropagation(); if(await customConfirm("Excluir Rede", "Excluir permanentemente esta rede da nuvem?")) await deleteDoc(doc(db, `${getBasePath()}/networks`, id)); };

        // --- GESTÃO DE UNIDADES ---
        window.promptCreateUnit = async () => {
            if(!window.PowFit.selNetId) return;
            const name = await customPrompt("Nova Unidade", "Nome/Localização da Unidade:");
            if(name && name.trim()) await addDoc(collection(db, `${getBasePath()}/units`), { netId: window.PowFit.selNetId, name: name.trim() });
        };
        window.renderUnits = () => {
            const l = document.getElementById('list-units');
            const units = window.PowFit.units.filter(u => u.netId === window.PowFit.selNetId);
            l.innerHTML = units.map(u => `
                <div onclick="selectUnit('${u.id}')" class="p-3 border rounded cursor-pointer transition flex justify-between items-center ${window.PowFit.selUnitId === u.id ? 'border-primary bg-primary bg-opacity-20 text-primary font-bold' : 'border-gray-600 border-opacity-30 hover:bg-black hover:bg-opacity-20'}">
                    <span><i class="fas fa-map-marker-alt opacity-50 mr-2"></i> ${u.name}</span>
                    <button onclick="delUnit(event, '${u.id}')" class="text-red-500 hover:text-red-400 p-1"><i class="fas fa-trash"></i></button>
                </div>`).join('');
        };
        window.selectUnit = (id) => { window.PowFit.selUnitId = id; document.getElementById('btn-add-member').classList.remove('hidden'); document.getElementById('msg-select-unit').classList.add('hidden'); document.getElementById('list-members').classList.remove('hidden'); renderUnits(); renderMembers(); };
        window.delUnit = async (e, id) => { e.stopPropagation(); if(await customConfirm("Excluir Unidade", "Excluir permanentemente esta unidade?")) await deleteDoc(doc(db, `${getBasePath()}/units`, id)); };

        // --- GESTÃO DE MEMBROS ---
        window.openMemberModal = () => document.getElementById('modal-member').classList.remove('hidden');
        window.closeMemberModal = () => { document.getElementById('modal-member').classList.add('hidden'); document.getElementById('mem-name').value = ''; document.getElementById('mem-cpf').value = ''; document.getElementById('mem-cref').value = ''; document.getElementById('mem-state').value = ''; };
        window.toggleMemRole = () => document.getElementById('mem-cref-container').style.display = document.getElementById('mem-role').value === 'TE' ? 'none' : 'grid';
        
        window.saveMember = async () => {
            const n = document.getElementById('mem-name').value.trim(); const c = document.getElementById('mem-cpf').value.trim();
            const r = document.getElementById('mem-role').value; const cr = document.getElementById('mem-cref').value.trim();
            const st = document.getElementById('mem-state').value.trim();
            if(!n || !c || (r === 'PEF' && (!cr || !st))) return customAlert("Erro", "Preencha todos os campos obrigatórios.");
            await addDoc(collection(db, `${getBasePath()}/members`), { unitId: window.PowFit.selUnitId, name: n, cpf: c, role: r, cref: cr, state: st });
            closeMemberModal();
        };

        window.renderMembers = () => {
            const l = document.getElementById('list-members');
            const mems = window.PowFit.members.filter(m => m.unitId === window.PowFit.selUnitId);
            l.innerHTML = mems.map(m => `
                <div class="p-3 border rounded border-gray-600 border-opacity-30 hover:border-primary transition flex flex-col gap-2 group">
                    <div class="flex justify-between items-start">
                        <div>
                            <div class="font-bold text-sm">${m.name}</div>
                            <div class="text-[10px] opacity-70">${m.role === 'PEF' ? 'Ed. Física' : 'Treinador'} ${m.cref ? `| CREF: ${m.cref}-${m.state}` : ''}</div>
                        </div>
                        <button onclick="delMem('${m.id}')" class="text-red-500 hover:text-red-400 p-1"><i class="fas fa-trash"></i></button>
                    </div>
                    <button onclick="enterEditor('${m.id}')" class="btn-primary w-full text-xs py-1.5 rounded font-bold shadow opacity-90 group-hover:opacity-100 transition"><i class="fas fa-sign-in-alt"></i> ACESSAR PRANCHETA</button>
                </div>`).join('');
        };
        window.delMem = async (id) => { if(await customConfirm("Excluir Membro", "Excluir permanentemente este membro da equipe?")) await deleteDoc(doc(db, `${getBasePath()}/members`, id)); };

        // --- PRANCHETA (EDITOR) ---
        window.enterEditor = (id) => {
            window.PowFit.activeMember = window.PowFit.members.find(m => m.id === id);
            document.getElementById('editor-active-member-info').innerText = `Profissional: ${window.PowFit.activeMember.name} (${window.PowFit.activeMember.role})`;
            
            const hData = window.PowFit.activeMember.role === 'PEF' ? healthPEF : healthTE;
            document.getElementById('health-container').innerHTML = Object.keys(hData).map(k => `
                <label class="flex items-start space-x-2 cursor-pointer p-1 rounded hover:bg-black hover:bg-opacity-10">
                    <input type="checkbox" value="${k}" class="health-cb mt-0.5 rounded border-gray-400 text-primary w-3 h-3">
                    <span class="font-medium">${k}</span>
                </label>`).join('');

            document.getElementById('stu-name').value = ''; document.getElementById('stu-age').value = '';
            document.getElementById('stu-weight').value = ''; document.getElementById('stu-height').value = '';
            document.getElementById('stu-recs').value = ''; document.getElementById('imc-display').innerHTML = '';
            
            window.PowFit.currentFichaId = null;
            window.PowFit.workouts = [];
            window.addWorkoutDay();
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
            document.getElementById('screen-editor').classList.add('active');
        };

        window.backToDashboard = async () => {
            if(await customConfirm("Voltar", "Voltar ao Dashboard Corporativo? Dados não salvos serão perdidos.")) {
                document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
                document.getElementById('screen-dashboard').classList.add('active');
            }
        };

        window.changeTheme = () => document.body.setAttribute('data-theme', document.getElementById('stu-gender').value);

        // --- LÓGICA DE CÁLCULO E CLASSIFICAÇÃO EXATA DE IMC ---
        window.calculateIMC = () => {
            const w = parseFloat(document.getElementById('stu-weight').value);
            const h = parseFloat(document.getElementById('stu-height').value);
            const display = document.getElementById('imc-display');
            
            if (w > 0 && h > 0) {
                const imcVal = (w / (h * h)).toFixed(1);
                const imcNum = parseFloat(imcVal);
                let cls = "";
                
                // Classificação rigorosa 
                if (imcNum < 18.5) cls = "Abaixo do peso";
                else if (imcNum >= 18.5 && imcNum <= 24.9) cls = "Peso normal";
                else if (imcNum >= 25.0 && imcNum <= 29.9) cls = "Sobrepeso";
                else if (imcNum >= 30.0 && imcNum <= 34.9) cls = "Obesidade Grau I";
                else if (imcNum >= 35.0 && imcNum <= 39.9) cls = "Obesidade Grau II";
                else if (imcNum >= 40.0) cls = "Obesidade Grau III";
                
                display.innerHTML = `<span class="bg-primary text-white px-2 py-0.5 rounded font-bold">IMC: ${imcVal} (${cls})</span>`;
            } else {
                display.innerHTML = '';
            }
        };

        // --- MONTAGEM DE TREINOS ---
        window.addWorkoutDay = () => {
            const c = window.PowFit.workouts.length;
            const t = c < 7 ? `TREINO ${wkDays[c]}` : `NOVO TREINO ${c+1}`;
            window.PowFit.workouts.push({ id: generateId(), title: t, exercises: [] });
            window.renderWorkouts();
        };

        window.dupDay = (id) => {
            const w = window.PowFit.workouts.find(x => x.id === id);
            if(w) { const n = JSON.parse(JSON.stringify(w)); n.id = generateId(); n.title += " (Cópia)"; window.PowFit.workouts.push(n); window.renderWorkouts(); }
        };

        window.delDay = async (id) => {
            if(await customConfirm("Excluir", "Remover este dia de treino?")) {
                window.PowFit.workouts = window.PowFit.workouts.filter(w => w.id !== id);
                window.renderWorkouts();
            }
        };
        window.updDayTitle = (id, v) => { window.PowFit.workouts.find(x => x.id === id).title = v; };

        window.renderWorkouts = () => {
            const cont = document.getElementById('workouts-container');
            cont.innerHTML = window.PowFit.workouts.map((w, wIdx) => {
                let exHtml = w.exercises.length === 0 ? `<div class="p-4 text-center text-xs opacity-50 italic">Sem exercícios</div>` : w.exercises.map((ex, eIdx) => `
                    <div class="flex flex-col md:flex-row gap-2 p-2 items-center border-b border-gray-600 border-opacity-30">
                        <div class="flex-1 w-full md:w-auto">
                            <div class="text-[9px] opacity-60 uppercase">${ex.category}</div>
                            <div class="text-xs font-bold leading-tight">${ex.name}</div>
                        </div>
                        <div class="flex gap-1 w-full md:w-auto items-center">
                            <input type="text" value="${ex.sets}" onchange="updEx('${w.id}',${eIdx},'sets',this.value)" class="input-field w-10 text-xs p-1 text-center rounded" placeholder="Sér">
                            <span class="opacity-50 text-xs">x</span>
                            <input type="text" value="${ex.reps}" onchange="updEx('${w.id}',${eIdx},'reps',this.value)" class="input-field w-12 text-xs p-1 text-center rounded" placeholder="Rep">
                            <select onchange="updEx('${w.id}',${eIdx},'technique',this.value)" class="input-field w-20 text-[10px] p-1 rounded">
                                ${techniques.map(t=>`<option value="${t}" ${ex.technique===t?'selected':''}>${t}</option>`).join('')}
                            </select>
                            <input type="text" value="${ex.obs}" onchange="updEx('${w.id}',${eIdx},'obs',this.value)" class="input-field flex-1 md:w-24 text-xs p-1 rounded" placeholder="Obs">
                            <div class="flex flex-col ml-1">
                                <button onclick="movEx('${w.id}',${eIdx},'up')" class="text-[10px] p-0.5 hover:bg-gray-600 rounded"><i class="fas fa-chevron-up"></i></button>
                                <button onclick="movEx('${w.id}',${eIdx},'down')" class="text-[10px] p-0.5 hover:bg-gray-600 rounded"><i class="fas fa-chevron-down"></i></button>
                            </div>
                            <button onclick="delEx('${w.id}',${eIdx})" class="text-red-500 hover:text-red-400 p-1"><i class="fas fa-times"></i></button>
                        </div>
                    </div>`).join('');

                return `
                <div class="card rounded-xl overflow-hidden shadow-sm">
                    <div class="p-2 border-b flex justify-between items-center bg-black bg-opacity-20" style="border-color: var(--border-color);">
                        <input type="text" value="${w.title}" onchange="updDayTitle('${w.id}', this.value)" class="input-field bg-transparent font-bold text-sm w-2/3 px-2 py-1 rounded">
                        <div class="flex gap-1">
                            <button onclick="dupDay('${w.id}')" class="p-1.5 text-xs hover:bg-gray-600 rounded"><i class="fas fa-copy"></i></button>
                            <button onclick="delDay('${w.id}')" class="p-1.5 text-xs text-red-500 hover:bg-red-500 hover:text-white rounded"><i class="fas fa-trash"></i></button>
                        </div>
                    </div>
                    <div>${exHtml}</div>
                    <button onclick="openExerciseModal('${w.id}')" class="w-full text-primary font-bold py-2 text-xs border-t hover:bg-primary hover:bg-opacity-10 transition" style="border-color: var(--border-color);">
                        + ADICIONAR EXERCÍCIO
                    </button>
                </div>`;
            }).join('');
        };

        window.movEx = (wId, idx, dir) => {
            const w = window.PowFit.workouts.find(x => x.id === wId);
            if(dir==='up' && idx>0) [w.exercises[idx], w.exercises[idx-1]] = [w.exercises[idx-1], w.exercises[idx]];
            if(dir==='down' && idx<w.exercises.length-1) [w.exercises[idx], w.exercises[idx+1]] = [w.exercises[idx+1], w.exercises[idx]];
            window.renderWorkouts();
        };
        window.updEx = (wId, idx, f, v) => { window.PowFit.workouts.find(x => x.id === wId).exercises[idx][f] = v; };
        window.delEx = (wId, idx) => { window.PowFit.workouts.find(x => x.id === wId).exercises.splice(idx, 1); window.renderWorkouts(); };

        // --- BANCO DE EXERCÍCIOS (MODAL) ---
        window.openExerciseModal = (wId) => { window.PowFit.activeDayIdx = wId; document.getElementById('exercise-modal').classList.remove('hidden'); renderCat(); renderExs(); };
        window.closeExerciseModal = () => document.getElementById('exercise-modal').classList.add('hidden');
        
        window.setCat = (c) => { window.PowFit.activeModalCat = c; renderCat(); renderExs(); };
        
        function renderCat() {
            document.getElementById('modal-categories').innerHTML = Object.keys(baseCategories).map(c => `
                <button onclick="setCat('${c}')" class="w-full text-left px-2 py-2 rounded text-xs font-bold transition ${window.PowFit.activeModalCat === c ? 'bg-primary text-white' : 'hover:bg-gray-700'}">${c}</button>
            `).join('');
        }

        function renderExs() {
            const cat = window.PowFit.activeModalCat;
            const base = baseCategories[cat] || [];
            const cust = window.PowFit.customExercises.filter(x => x.category === cat);
            
            let html = base.map(ex => `<div onclick="addEx('${ex}')" class="card p-2 rounded text-xs font-medium cursor-pointer hover:border-primary transition group flex justify-between items-center"><span>${ex}</span> <i class="fas fa-plus text-primary opacity-0 group-hover:opacity-100 float-right"></i></div>`).join('');
            
            html += cust.map(c => `<div class="card p-2 rounded text-xs font-medium border-indigo-600 border-opacity-50 hover:border-indigo-500 cursor-pointer flex justify-between items-center group" onclick="addEx('${c.name}')">
                <span>${c.name} <i class="fas fa-cloud text-indigo-400 text-[10px]"></i></span>
                <div>
                    <i class="fas fa-plus text-primary opacity-0 group-hover:opacity-100 mr-2"></i>
                    <button onclick="delCustomEx(event, '${c.id}')" class="text-red-500 hover:text-red-300"><i class="fas fa-trash"></i></button>
                </div>
            </div>`).join('');
            
            document.getElementById('modal-exercises').innerHTML = html;
        }

        window.addEx = (name) => {
            const w = window.PowFit.workouts.find(x => x.id === window.PowFit.activeDayIdx);
            const isCardio = window.PowFit.activeModalCat === "🫀 CARDIO";
            w.exercises.push({ category: window.PowFit.activeModalCat, name: name, sets: isCardio ? '1' : '3', reps: isCardio ? '-' : '10 a 12', technique: 'Nenhuma', obs: '' });
            window.renderWorkouts();
            const md = document.getElementById('modal-exercises'); md.style.opacity = '0.5'; setTimeout(()=>md.style.opacity='1', 100);
        };

        // Exercícios Customizados Nuvem
        window.openCustomExModal = () => {
            document.getElementById('custom-ex-cat').innerHTML = Object.keys(baseCategories).map(c=>`<option>${c}</option>`).join('');
            document.getElementById('custom-ex-cat').value = window.PowFit.activeModalCat;
            document.getElementById('custom-ex-name').value = '';
            document.getElementById('custom-ex-modal').classList.remove('hidden');
        };
        window.closeCustomExModal = () => document.getElementById('custom-ex-modal').classList.add('hidden');
        
        window.saveCustomEx = async () => {
            const c = document.getElementById('custom-ex-cat').value; const n = document.getElementById('custom-ex-name').value.trim();
            if(n) { await addDoc(collection(db, `${getBasePath()}/custom_ex`), { category: c, name: n }); closeCustomExModal(); setCat(c); }
        };
        window.delCustomEx = async (e, id) => { e.stopPropagation(); if(await customConfirm("Excluir Nuvem", "Remover este exercício customizado da base em nuvem?")) await deleteDoc(doc(db, `${getBasePath()}/custom_ex`, id)); renderExs(); };

        // --- SALVAR E IMPRIMIR ---
        const getPayload = () => {
            return {
                memberId: window.PowFit.activeMember.id, unitId: window.PowFit.activeMember.unitId,
                timestamp: Date.now(),
                studentName: document.getElementById('stu-name').value || 'Sem Nome',
                stuAge: document.getElementById('stu-age').value, stuWeight: document.getElementById('stu-weight').value, stuHeight: document.getElementById('stu-height').value,
                stuGender: document.getElementById('stu-gender').value, stuLevel: document.getElementById('stu-level').value,
                stuObjective: document.getElementById('stu-objective').value, stuFreq: document.getElementById('stu-freq').value,
                stuValidity: document.getElementById('stu-validity').value, stuRecs: document.getElementById('stu-recs').value,
                health: Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value),
                workouts: window.PowFit.workouts
            };
        };

        window.saveAndPrint = async () => {
            setLoading("Gerando Impressão e Salvando Nuvem...", true);
            try {
                const payload = getPayload();
                if(window.PowFit.currentFichaId) await updateDoc(doc(db, `${getBasePath()}/history`, window.PowFit.currentFichaId), payload);
                else { const d = await addDoc(collection(db, `${getBasePath()}/history`), payload); window.PowFit.currentFichaId = d.id; }
                
                // Montar Impressão A4 Exata
                const d = payload;
                const isPEF = window.PowFit.activeMember.role === 'PEF';
                
                // Cálculo de IMC para visualização de impressão
                let imcStr = "-"; 
                const w = parseFloat(d.stuWeight); const h = parseFloat(d.stuHeight);
                if(w > 0 && h > 0) {
                    const imcVal = (w / (h * h)).toFixed(1);
                    const imcNum = parseFloat(imcVal);
                    let cls = "";
                    if (imcNum < 18.5) cls = "Abaixo do peso";
                    else if (imcNum >= 18.5 && imcNum <= 24.9) cls = "Peso normal";
                    else if (imcNum >= 25.0 && imcNum <= 29.9) cls = "Sobrepeso";
                    else if (imcNum >= 30.0 && imcNum <= 34.9) cls = "Obesidade Grau I";
                    else if (imcNum >= 35.0 && imcNum <= 39.9) cls = "Obesidade Grau II";
                    else if (imcNum >= 40.0) cls = "Obesidade Grau III";
                    imcStr = `${imcVal} (${cls})`;
                }

                const objRec = objectivesDesc[d.stuObjective] || "";
                const profLabel = isPEF ? 'Profissional de Educação Física' : 'Treinador Esportivo';
                const crefText = isPEF ? `CREF: ${window.PowFit.activeMember.cref} - ${window.PowFit.activeMember.state}` : ``;
                const hDict = isPEF ? healthPEF : healthTE;

                const legalPEF = `⚠️ OBSERVAÇÃO LEGAL – PROFISSIONAL DE EDUCAÇÃO FÍSICA: Conforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º estabelece que compete ao profissional coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos especializados nas áreas de atividades físicas e do desporto.`;
                const legalTE = `⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO: Conforme a Lei nº 14.597/2023 (Lei Geral do Esporte), Art. 75, a profissão de treinador esportivo é reconhecida e regulada no Brasil, com atuação de caráter técnico e esportivo voltada à preparação, supervisão, orientação e acompanhamento de treinos. A atuação possui finalidade orientativa e não substitui avaliação médica, diagnóstico clínico, prescrição medicamentosa ou acompanhamento de profissionais da saúde. Em casos de doenças, lesões, gestação, limitações físicas, uso de medicações ou qualquer condição específica de saúde, recomenda-se avaliação prévia por profissional habilitado. O treinamento proposto respeita limites individuais, priorizando segurança e evolução progressiva dentro da atuação permitida por lei. As recomendações desta ficha possuem caráter informativo e orientativo.`;

                let html = `
                    <div class="print-header">
                        <h1 class="print-title">Planilha de Treinamento - ${d.stuObjective}</h1>
                        <div class="prof-info">Prescrição feita por: ${window.PowFit.activeMember.name} (${profLabel})<br>${crefText}</div>
                    </div>
                    <div class="print-grid">
                        <div><strong>Aluno(a):</strong> ${d.studentName}</div><div><strong>Idade:</strong> ${d.stuAge||'-'}</div><div><strong>Gênero:</strong> ${d.stuGender}</div>
                        <div><strong>Peso:</strong> ${d.stuWeight||'-'} kg</div><div><strong>Altura:</strong> ${d.stuHeight||'-'} m</div><div><strong>IMC:</strong> ${imcStr}</div>
                        <div><strong>Nível:</strong> ${d.stuLevel}</div><div><strong>Freq:</strong> ${d.stuFreq}</div><div><strong>Validade:</strong> ${d.stuValidity.replace('Validade: ','')}</div>
                    </div>`;

                if (objRec || d.health.length > 0) {
                    html += `<div class="print-guidelines"><h4>Diretrizes Automáticas de Perfil</h4><ul>`;
                    if(objRec) html += `<li><strong>Objetivo (${d.stuObjective}):</strong> ${objRec}</li>`;
                    d.health.forEach(k => { if(hDict[k]) html += `<li><strong>Saúde (${k.replace(/[\u{1F300}-\u{1F9FF}\u{2600}-\u{26FF}\u{2700}-\u{27BF}\u{FE0F}]/gu, '').trim()}):</strong> ${hDict[k]}</li>`; });
                    html += `</ul></div>`;
                }

                d.workouts.forEach(wk => {
                    html += `<div class="print-workout"><h3>${wk.title}</h3><table><thead><tr>
                        <th style="width:35%">Exercício</th><th style="width:8%;text-align:center;">Séries</th><th style="width:12%;text-align:center;">Reps</th><th style="width:15%">Técnica</th><th style="width:30%">Observações</th>
                    </tr></thead><tbody>`;
                    if (wk.exercises.length === 0) html += `<tr><td colspan="5" style="text-align:center; font-style:italic; padding:10px;">Sem exercícios cadastrados.</td></tr>`;
                    else {
                        wk.exercises.forEach(ex => {
                            html += `<tr><td><strong>${ex.name}</strong></td><td style="text-align:center;">${ex.sets}</td><td style="text-align:center;">${ex.reps}</td><td>${ex.technique!=='Nenhuma'?ex.technique:'-'}</td><td>${ex.obs||'-'}</td></tr>`;
                        });
                    }
                    html += `</tbody></table></div>`;
                });

                html += `<div class="print-footer-section">`;
                if(d.stuRecs.trim()) html += `<strong>Recomendações Manuais:</strong><br><p style="white-space: pre-wrap; margin: 3px 0 10px 0;">${d.stuRecs}</p>`;
                html += `<div class="legal-text">${isPEF ? legalPEF : legalTE}</div>
                         <div style="text-align:center; margin-top: 15px; font-size: 8px;">ID Ficha: ${window.PowFit.currentFichaId} | PowFit Pro Corporativo</div></div>`;

                document.getElementById('print-area').innerHTML = html;
                document.getElementById('print-area').classList.add('active-print');
                
                setLoading(false);
                window.print();
            } catch(e) { console.error(e); customAlert("Erro", "Falha ao processar a impressão."); setLoading(false); }
        };

        // --- HISTÓRICO NUVEM E RELATÓRIOS ---
        window.openCloudHistory = () => { 
            const f = document.getElementById('hist-filter');
            f.innerHTML = `<option value="ALL">Todas as Unidades</option>` + window.PowFit.units.map(u=>`<option value="${u.id}">${u.name}</option>`).join('');
            document.getElementById('history-modal').classList.remove('hidden'); renderHistoryList(); 
        };
        window.closeCloudHistory = () => document.getElementById('history-modal').classList.add('hidden');

        window.renderHistoryList = () => {
            const uId = document.getElementById('hist-filter').value;
            let list = window.PowFit.history.sort((a,b)=>b.timestamp - a.timestamp);
            if(uId !== 'ALL') list = list.filter(w => w.unitId === uId);

            const now = Date.now();
            document.getElementById('history-list').innerHTML = list.map(w => {
                const mem = window.PowFit.members.find(m => m.id === w.memberId);
                const memName = mem ? mem.name : 'Deletado';
                let exp = 30; if(w.stuValidity) exp = parseInt(w.stuValidity.replace(/\D/g, ''))||30;
                const isExp = now > (w.timestamp + (exp * 24 * 60 * 60 * 1000));
                
                return `
                <div class="card p-3 rounded flex justify-between items-center border border-gray-600 border-opacity-30">
                    <div>
                        <div class="font-bold text-sm">${w.studentName} ${isExp?'<span class="bg-red-500 text-white px-1 py-0.5 text-[9px] rounded font-bold ml-2">VENCIDA</span>':''}</div>
                        <div class="text-[10px] opacity-70">Em: ${new Date(w.timestamp).toLocaleDateString()} | Por: ${memName}</div>
                    </div>
                    <div class="flex gap-2">
                        <button onclick="loadHistory('${w.id}')" class="btn-primary px-3 py-1.5 rounded text-xs font-bold">Carregar</button>
                        <button onclick="delHistory('${w.id}')" class="bg-red-500 hover:bg-red-400 text-white px-2 py-1.5 rounded"><i class="fas fa-trash"></i></button>
                    </div>
                </div>`;
            }).join('') || `<p class="text-center opacity-50 text-sm p-4">Nenhum registro.</p>`;
        };

        window.loadHistory = (id) => {
            const w = window.PowFit.history.find(x => x.id === id);
            if(!w) return;
            const c = window.PowFit.members.find(m => m.id === w.memberId);
            if(c) window.PowFit.activeMember = c;
            
            window.PowFit.currentFichaId = w.id;
            document.getElementById('editor-active-member-info').innerText = `Profissional: ${window.PowFit.activeMember.name} (${window.PowFit.activeMember.role})`;
            
            document.getElementById('stu-name').value = w.studentName||''; document.getElementById('stu-age').value = w.stuAge||'';
            document.getElementById('stu-weight').value = w.stuWeight||''; document.getElementById('stu-height').value = w.stuHeight||'';
            document.getElementById('stu-gender').value = w.stuGender||'Masculino'; document.getElementById('stu-level').value = w.stuLevel||'Iniciante';
            document.getElementById('stu-objective').value = w.stuObjective||'Emagrecimento'; document.getElementById('stu-freq').value = w.stuFreq||'3 dias';
            document.getElementById('stu-validity').value = w.stuValidity||'Validade: 30 dias'; document.getElementById('stu-recs').value = w.stuRecs||'';
            
            changeTheme(); calculateIMC();
            
            const hDict = window.PowFit.activeMember.role === 'PEF' ? healthPEF : healthTE;
            document.getElementById('health-container').innerHTML = Object.keys(hDict).map(opt => `
                <label class="flex items-start space-x-2 cursor-pointer p-1 rounded hover:bg-black hover:bg-opacity-10">
                    <input type="checkbox" value="${opt}" ${(w.health||[]).includes(opt)?'checked':''} class="health-cb mt-0.5 rounded border-gray-400 text-primary w-3 h-3">
                    <span class="font-medium">${opt}</span>
                </label>`).join('');

            window.PowFit.workouts = w.workouts || [];
            renderWorkouts(); closeCloudHistory();
        };

        window.delHistory = async (id) => { if(await customConfirm("Excluir Ficha", "Deletar permanentemente da nuvem?")) await deleteDoc(doc(db, `${getBasePath()}/history`, id)); };

        // --- RELATÓRIOS ---
        window.openReportModal = () => {
            const um = document.getElementById('rep-unit');
            um.innerHTML = `<option value="ALL">Todas</option>` + window.PowFit.units.map(u=>`<option value="${u.id}">${u.name}</option>`).join('');
            const mm = document.getElementById('rep-month');
            if(mm.options.length === 0) {
                const d = new Date();
                for(let i=0; i<6; i++) { const mStr = d.toISOString().slice(0,7); mm.innerHTML += `<option value="${mStr}">${mStr}</option>`; d.setMonth(d.getMonth()-1); }
            }
            document.getElementById('report-modal').classList.remove('hidden');
            generateReport();
        };
        window.closeReportModal = () => document.getElementById('report-modal').classList.add('hidden');

        window.generateReport = () => {
            const m = document.getElementById('rep-month').value; const u = document.getElementById('rep-unit').value;
            let f = window.PowFit.history.filter(w => new Date(w.timestamp).toISOString().startsWith(m));
            if(u !== 'ALL') f = f.filter(w => w.unitId === u);

            const c = {}; f.forEach(w => { c[w.memberId] = (c[w.memberId] || 0) + 1; });
            const s = Object.keys(c).map(id => {
                const mem = window.PowFit.members.find(x => x.id === id);
                const uni = mem ? window.PowFit.units.find(x => x.id === mem.unitId) : null;
                return { name: mem?mem.name:'Removido', unit: uni?uni.name:'-', count: c[id] };
            }).sort((a,b)=>b.count-a.count);

            let html = `<div class="mb-4"><div class="text-3xl font-bold text-primary">${f.length}</div><div class="text-xs uppercase opacity-70">Total de Fichas</div></div>
            <table class="w-full text-sm border border-gray-600 border-opacity-30 rounded">
                <thead class="bg-black bg-opacity-20 text-left"><tr><th class="p-2">Profissional</th><th class="p-2">Unidade</th><th class="p-2 text-center">Fichas</th></tr></thead><tbody>`;
            if(s.length===0) html += `<tr><td colspan="3" class="p-4 text-center italic opacity-50">Sem dados no período.</td></tr>`;
            else s.forEach(x => { html += `<tr class="border-t border-gray-600 border-opacity-20"><td class="p-2 font-bold">${x.name}</td><td class="p-2">${x.unit}</td><td class="p-2 text-center font-bold text-primary">${x.count}</td></tr>`; });
            document.getElementById('report-content').innerHTML = html + `</tbody></table>`;
        };

        window.printReport = () => {
            document.getElementById('print-report-area').innerHTML = `
                <h1>Relatório Corporativo - PowFit Pro</h1>
                <p><strong>Referência:</strong> ${document.getElementById('rep-month').value} | <strong>Unidade:</strong> ${document.getElementById('rep-unit').options[document.getElementById('rep-unit').selectedIndex].text}</p>
                ${document.getElementById('report-content').innerHTML}`;
            document.getElementById('print-report-area').classList.add('active-print');
            window.print();
        };
    </script>
</body>
</html>
