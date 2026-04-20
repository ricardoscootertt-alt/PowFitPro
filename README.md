<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro - Master Network Edition</title>
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
        body { font-family: 'Inter', sans-serif; background-color: var(--bg-body); color: var(--text-main); transition: all 0.3s ease; overflow-x: hidden; }
        .card { background-color: var(--bg-card); border: 1px solid var(--border-color); transition: all 0.3s ease; }
        .input-field { background-color: var(--input-bg); border: 1px solid var(--input-border); color: var(--text-main); }
        .input-field:focus { outline: none; border-color: var(--primary); box-shadow: 0 0 0 1px var(--primary); }
        .btn-primary { background-color: var(--primary); color: white; transition: 0.2s; }
        .btn-primary:hover { background-color: var(--primary-hover); }
        
        .hidden { display: none !important; }
        #print-area { display: none; }

        @media print {
            body { background: white !important; color: black !important; margin: 0; padding: 0; }
            #app-screens, #login-screen, #nav-menu, .no-print, .modal-backdrop { display: none !important; }
            #print-area { display: block !important; padding: 10mm; font-family: 'Arial', sans-serif; font-size: 10px; }
            @page { size: A4; margin: 0; }
            
            .print-header { border-bottom: 2px solid #000; padding-bottom: 5px; margin-bottom: 10px; display: flex; justify-content: space-between; align-items: flex-end;}
            .print-title { font-size: 16px; font-weight: bold; margin: 0; text-transform: uppercase;}
            .prof-info { font-size: 10px; text-align: right; font-weight: bold;}
            
            .print-grid { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 4px; margin-bottom: 10px; border: 1px solid #000; padding: 6px; }
            .print-grid div { font-size: 10px; }
            
            .print-guidelines { border: 1px solid #000; padding: 6px; margin-bottom: 10px; font-size: 9px; }
            .print-guidelines h4 { margin: 0 0 4px 0; font-size: 10px; text-transform: uppercase; background: #eee; padding: 2px;}
            .print-guidelines ul { margin: 0; padding-left: 15px; }
            
            .print-workout { margin-bottom: 10px; page-break-inside: avoid; }
            .print-workout h3 { background: #e0e0e0; border: 1px solid #000; border-bottom: none; padding: 3px 6px; margin: 0; font-size: 11px; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            table { width: 100%; border-collapse: collapse; margin-bottom: 0; border: 1px solid #000; }
            th, td { border: 1px solid #000; padding: 4px; text-align: left; font-size: 9px; vertical-align: middle;}
            th { background-color: #f0f0f0; font-weight: bold; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            
            .ex-img-print { width: 30px; height: 30px; object-fit: cover; border-radius: 4px; display: inline-block; vertical-align: middle; margin-right: 5px; border: 1px solid #ccc;}
            
            .print-footer-section { margin-top: 10px; border: 1px solid #000; padding: 6px; font-size: 9px; page-break-inside: avoid;}
            .legal-text { font-size: 8px; color: #333; text-align: justify; margin-top: 5px; font-style: italic;}

            .report-title { font-size: 18px; font-weight: bold; text-align: center; margin-bottom: 15px; text-transform: uppercase;}
            .report-unit { margin-bottom: 15px; border: 1px solid #000; padding: 10px;}
            .report-unit h3 { background: #333; color: white; padding: 5px; margin: -10px -10px 10px -10px; -webkit-print-color-adjust: exact; color-adjust: exact;}
            .report-member { display: flex; justify-content: space-between; border-bottom: 1px solid #ccc; padding: 4px 0;}
            .report-total { font-weight: bold; font-size: 12px; margin-top: 10px; text-align: right;}
        }

        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
        
        .loader { border: 3px solid rgba(255,255,255,0.3); border-radius: 50%; border-top: 3px solid #fff; width: 24px; height: 24px; animation: spin 1s linear infinite; }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
    </style>
</head>
<body data-theme="Masculino" class="min-h-screen flex flex-col">

    <div id="global-loader" class="fixed inset-0 bg-black bg-opacity-90 z-[100] flex flex-col items-center justify-center hidden">
        <div class="loader mb-4 w-12 h-12 border-4"></div>
        <p class="text-white font-medium tracking-wider">Sincronizando Nuvem...</p>
    </div>

    <div id="login-screen" class="flex-1 flex flex-col items-center justify-center p-6 hidden">
        <div class="card p-8 rounded-2xl shadow-2xl max-w-md w-full text-center">
            <i class="fas fa-dumbbell text-5xl text-primary mb-4"></i>
            <h1 class="text-3xl font-bold mb-2">PowFit Pro</h1>
            <p class="text-sm opacity-70 mb-8">Master Network Edition</p>
            
            <button onclick="loginGoogle()" class="w-full bg-white text-gray-800 hover:bg-gray-100 font-bold py-3 px-4 rounded-lg flex items-center justify-center gap-3 transition shadow-md border border-gray-200">
                <img src="https://www.gstatic.com/firebasejs/ui/2.0.0/images/auth/google.svg" class="w-6 h-6" alt="Google">
                Entrar com Google
            </button>
            <p class="text-xs opacity-50 mt-4"><i class="fas fa-cloud"></i> Dados salvos em nuvem segura</p>
        </div>
    </div>

    <div id="setup-screen" class="flex-1 flex flex-col items-center justify-center p-6 hidden">
        <div class="card p-8 rounded-2xl shadow-2xl max-w-lg w-full">
            <h2 class="text-2xl font-bold mb-2"><i class="fas fa-building text-primary"></i> Criar Franquia / Rede</h2>
            <p class="opacity-70 mb-6 text-sm">Estruture sua marca principal. Mais unidades poderão ser adicionadas depois.</p>
            
            <div class="space-y-4">
                <div>
                    <label class="block text-sm font-medium mb-1">Sua Marca / Rede</label>
                    <input type="text" id="setup-network-name" class="input-field w-full rounded-lg px-3 py-3" placeholder="Ex: Academia FitLife">
                </div>
                <div>
                    <label class="block text-sm font-medium mb-1">Sua Primeira Unidade</label>
                    <input type="text" id="setup-unit-name" class="input-field w-full rounded-lg px-3 py-3" placeholder="Ex: Matriz (Centro)">
                </div>
                <button id="btn-save-setup" class="w-full btn-primary py-3 rounded-lg font-bold mt-4 shadow-lg">Finalizar Configuração</button>
            </div>
        </div>
    </div>

    <nav id="nav-menu" class="card border-b px-4 py-3 sticky top-0 z-50 hidden">
        <div class="max-w-7xl mx-auto flex flex-col sm:flex-row justify-between items-center gap-4">
            <div class="flex items-center gap-2">
                <i class="fas fa-dumbbell text-2xl text-primary"></i>
                <div class="flex flex-col">
                    <span class="font-bold text-lg tracking-tight leading-tight">PowFit Pro</span>
                    <span id="nav-network-name" class="text-[10px] text-primary uppercase font-bold tracking-widest"></span>
                </div>
            </div>
            <div class="flex gap-1 overflow-x-auto w-full sm:w-auto pb-2 sm:pb-0 hide-scrollbar">
                <button onclick="switchTab('builder')" id="tab-builder" class="nav-btn px-4 py-2 rounded-lg text-sm font-medium transition bg-primary text-white"><i class="fas fa-plus-circle mr-1"></i> Montar Ficha</button>
                <button onclick="switchTab('network')" id="tab-network" class="nav-btn px-4 py-2 rounded-lg text-sm font-medium transition hover:bg-black hover:bg-opacity-10"><i class="fas fa-sitemap mr-1"></i> Gestão de Rede</button>
                <button onclick="switchTab('database')" id="tab-database" class="nav-btn px-4 py-2 rounded-lg text-sm font-medium transition hover:bg-black hover:bg-opacity-10"><i class="fas fa-database mr-1"></i> Exercícios</button>
                <button onclick="switchTab('history')" id="tab-history" class="nav-btn px-4 py-2 rounded-lg text-sm font-medium transition hover:bg-black hover:bg-opacity-10"><i class="fas fa-history mr-1"></i> Nuvem</button>
                <button onclick="switchTab('reports')" id="tab-reports" class="nav-btn px-4 py-2 rounded-lg text-sm font-medium transition hover:bg-black hover:bg-opacity-10"><i class="fas fa-chart-bar mr-1"></i> Relatórios</button>
            </div>
            <button onclick="logout()" class="text-sm text-red-500 hover:text-red-400 font-medium"><i class="fas fa-sign-out-alt"></i> Sair</button>
        </div>
    </nav>

    <div id="app-screens" class="flex-1 max-w-7xl mx-auto w-full p-4 sm:p-6 lg:p-8 hidden">
        
        <div id="screen-builder" class="screen-content">
            <div class="card rounded-xl p-4 shadow-sm mb-6 bg-opacity-50 border-primary border-opacity-30">
                <label class="block text-sm font-bold mb-2 text-primary"><i class="fas fa-id-badge"></i> Profissional Responsável pela Prescrição</label>
                <select id="active-member" onchange="onMemberChange()" class="input-field w-full md:w-1/2 rounded-lg px-3 py-2 text-sm font-medium">
                    <option value="">Selecione o membro da equipe...</option>
                </select>
                <p id="active-member-info" class="text-xs mt-2 opacity-70 italic"></p>
            </div>

            <div class="grid grid-cols-1 xl:grid-cols-12 gap-6" id="builder-workspace" style="display: none;">
                <div class="xl:col-span-4 space-y-6">
                    <div class="card rounded-xl p-5 shadow-sm">
                        <div class="flex justify-between items-center mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                            <h2 class="text-lg font-semibold flex items-center gap-2"><i class="fas fa-user text-primary"></i> Aluno</h2>
                            <div id="imc-display"></div>
                        </div>
                        <div class="space-y-4">
                            <div><label class="block text-xs font-medium mb-1">Nome</label><input type="text" id="stu-name" class="input-field w-full rounded-lg px-3 py-2 text-sm"></div>
                            <div class="grid grid-cols-3 gap-2">
                                <div><label class="block text-xs font-medium mb-1">Idade</label><input type="number" id="stu-age" class="input-field w-full rounded-lg px-3 py-2 text-sm"></div>
                                <div><label class="block text-xs font-medium mb-1">Peso(kg)</label><input type="number" id="stu-weight" oninput="calcIMC()" class="input-field w-full rounded-lg px-3 py-2 text-sm"></div>
                                <div><label class="block text-xs font-medium mb-1">Alt.(m)</label><input type="number" id="stu-height" step="0.01" oninput="calcIMC()" class="input-field w-full rounded-lg px-3 py-2 text-sm"></div>
                            </div>
                            <div class="grid grid-cols-2 gap-2">
                                <div>
                                    <label class="block text-xs font-medium mb-1">Gênero</label>
                                    <select id="stu-gender" onchange="changeTheme()" class="input-field w-full rounded-lg px-3 py-2 text-sm"><option value="Masculino">Masculino</option><option value="Feminino">Feminino</option></select>
                                </div>
                                <div>
                                    <label class="block text-xs font-medium mb-1">Nível</label>
                                    <select id="stu-level" class="input-field w-full rounded-lg px-3 py-2 text-sm"><option>Iniciante</option><option>Intermediário</option><option>Avançado</option></select>
                                </div>
                            </div>
                            <div>
                                <label class="block text-xs font-medium mb-1">Objetivo (Gera diretriz)</label>
                                <select id="stu-objective" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                    <option>Emagrecimento</option><option>Hipertrofia</option><option>Definição</option><option>Condicionamento</option><option>Resistência</option><option>Força</option><option>Reabilitação</option><option>Saúde geral</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs font-medium mb-1">Frequência</label>
                                <select id="stu-freq" class="input-field w-full rounded-lg px-3 py-2 text-sm"><option>3 dias</option><option>5 dias</option><option>6 dias</option><option>Personalizado</option></select>
                            </div>
                            <div>
                                <label class="block text-xs font-medium mb-1">Validade</label>
                                <select id="stu-validity" class="input-field w-full rounded-lg px-3 py-2 text-sm"><option>15 dias</option><option selected>30 dias</option><option>60 dias</option><option>90 dias</option></select>
                            </div>
                        </div>
                    </div>

                    <div class="card rounded-xl p-5 shadow-sm">
                        <h2 class="text-lg font-semibold mb-2 flex items-center gap-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)"><i class="fas fa-notes-medical text-primary"></i> Saúde (Diretrizes)</h2>
                        <div class="grid grid-cols-2 gap-2 text-[11px]" id="health-container"></div>
                    </div>
                </div>

                <div class="xl:col-span-8 space-y-4">
                    <div class="flex justify-between items-center bg-opacity-10 p-4 rounded-xl card border-dashed border-2">
                        <h2 class="text-xl font-bold"><i class="fas fa-clipboard-list text-primary mr-2"></i> Prancheta</h2>
                        <div class="flex gap-2">
                            <button onclick="addWorkoutDay()" class="bg-gray-700 hover:bg-gray-600 text-white px-3 py-2 rounded-lg text-sm font-medium"><i class="fas fa-plus mr-1"></i> Dia</button>
                            <button onclick="saveAndPrint()" class="btn-primary px-4 py-2 rounded-lg text-sm font-bold shadow-lg"><i class="fas fa-print mr-1"></i> Salvar & Imprimir</button>
                        </div>
                    </div>
                    <div id="workouts-container" class="space-y-4"></div>
                    <div class="card rounded-xl p-5 shadow-sm">
                        <h2 class="text-sm font-bold mb-2"><i class="fas fa-pen text-primary mr-1"></i> Recomendações Livres</h2>
                        <textarea id="stu-recs" rows="3" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: Hidratação, execução..."></textarea>
                    </div>
                </div>
            </div>
        </div>

        <div id="screen-network" class="screen-content hidden space-y-6">
            <h2 class="text-2xl font-bold"><i class="fas fa-sitemap text-primary mr-2"></i> Gestão de Franquias e Equipe</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div class="card p-5 rounded-xl">
                    <h3 class="text-lg font-bold mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">Unidades</h3>
                    <div class="flex gap-2 mb-4">
                        <input type="text" id="new-unit-name" class="input-field flex-1 rounded px-3 py-2 text-sm" placeholder="Nome da Unidade (Ex: Matriz)">
                        <button onclick="addUnit()" class="btn-primary px-4 rounded text-sm font-bold"><i class="fas fa-plus"></i></button>
                    </div>
                    <div id="units-list" class="space-y-2 max-h-60 overflow-y-auto"></div>
                </div>
                <div class="card p-5 rounded-xl">
                    <h3 class="text-lg font-bold mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">Adicionar Membro</h3>
                    <div class="space-y-3">
                        <select id="member-unit" class="input-field w-full rounded px-3 py-2 text-sm"><option value="">Selecione a Unidade...</option></select>
                        <input type="text" id="member-name" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="Nome do Treinador">
                        <input type="text" id="member-cpf" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="CPF">
                        <select id="member-role" onchange="toggleCrefNetwork()" class="input-field w-full rounded px-3 py-2 text-sm">
                            <option value="PEF">Profissional de Educação Física (PEF)</option>
                            <option value="TE">Treinador Esportivo (TE)</option>
                        </select>
                        <div id="cref-box" class="grid grid-cols-2 gap-2">
                            <input type="text" id="member-cref" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="CREF">
                            <input type="text" id="member-state" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="Estado (UF)">
                        </div>
                        <button onclick="addMember()" class="w-full btn-primary py-2 rounded font-bold mt-2">Cadastrar Membro</button>
                    </div>
                </div>
            </div>
            <div class="card p-5 rounded-xl">
                <h3 class="text-lg font-bold mb-4">Equipe Cadastrada</h3>
                <div class="overflow-x-auto">
                    <table class="w-full text-left text-sm whitespace-nowrap">
                        <thead class="bg-black bg-opacity-10">
                            <tr><th class="p-2">Nome</th><th class="p-2">Unidade</th><th class="p-2">Categoria</th><th class="p-2">Documento</th><th class="p-2">Ação</th></tr>
                        </thead>
                        <tbody id="members-table-body" class="divide-y divide-gray-500 divide-opacity-20"></tbody>
                    </table>
                </div>
            </div>
        </div>

        <div id="screen-database" class="screen-content hidden space-y-6">
            <h2 class="text-2xl font-bold"><i class="fas fa-database text-primary mr-2"></i> Customizar Banco de Exercícios</h2>
            <div class="card p-5 rounded-xl flex flex-col md:flex-row gap-6">
                <div class="flex-1 space-y-4">
                    <div><label class="block text-sm font-medium mb-1">Categoria</label><select id="custom-ex-cat" class="input-field w-full rounded px-3 py-2 text-sm"></select></div>
                    <div><label class="block text-sm font-medium mb-1">Nome do Exercício</label><input type="text" id="custom-ex-name" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="Ex: Supino Máquina Convergente"></div>
                    <div>
                        <label class="block text-sm font-medium mb-1">Imagem Demonstrativa (Opcional)</label>
                        <input type="file" id="custom-ex-img" accept="image/*" class="w-full text-sm file:mr-4 file:py-2 file:px-4 file:rounded file:border-0 file:text-sm file:font-semibold file:bg-primary file:text-white hover:file:bg-primary-hover">
                        <p class="text-[10px] opacity-60 mt-1">Imagens serão comprimidas automaticamente.</p>
                    </div>
                    <button onclick="saveCustomExercise()" class="btn-primary px-6 py-2 rounded font-bold w-full"><i class="fas fa-save mr-1"></i> Adicionar ao Banco</button>
                </div>
                <div class="flex-1 border-l pl-0 md:pl-6 border-opacity-20 mt-4 md:mt-0" style="border-color: var(--border-color)">
                    <h3 class="font-bold mb-3">Exercícios Customizados Cadastrados</h3>
                    <div id="custom-ex-list" class="space-y-2 max-h-64 overflow-y-auto pr-2"></div>
                </div>
            </div>
        </div>

        <div id="screen-history" class="screen-content hidden space-y-6">
            <div class="flex justify-between items-center">
                <h2 class="text-2xl font-bold"><i class="fas fa-cloud text-primary mr-2"></i> Arquivo em Nuvem</h2>
                <input type="text" id="search-history" oninput="renderHistory()" class="input-field rounded-lg px-3 py-2 text-sm w-64" placeholder="Buscar aluno...">
            </div>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4" id="history-grid"></div>
        </div>

        <div id="screen-reports" class="screen-content hidden space-y-6">
            <div class="flex justify-between items-center">
                <h2 class="text-2xl font-bold"><i class="fas fa-chart-bar text-primary mr-2"></i> Produtividade Mensal</h2>
                <button onclick="printReport()" class="btn-primary px-4 py-2 rounded-lg font-bold"><i class="fas fa-print mr-1"></i> Imprimir Relatório</button>
            </div>
            <div class="card p-5 rounded-xl">
                <div class="flex gap-4 mb-6">
                    <select id="report-month" onchange="generateReport()" class="input-field rounded px-3 py-2 text-sm w-32"></select>
                    <select id="report-year" onchange="generateReport()" class="input-field rounded px-3 py-2 text-sm w-32"></select>
                </div>
                <div id="report-content" class="bg-black bg-opacity-5 p-6 rounded-lg border border-opacity-20" style="border-color: var(--border-color);"></div>
            </div>
        </div>

    </div>

    <div id="exercise-modal" class="fixed inset-0 bg-black bg-opacity-80 backdrop-blur-sm hidden z-[60] flex items-center justify-center p-2 sm:p-4 modal-backdrop">
        <div class="card w-full max-w-6xl rounded-xl shadow-2xl flex flex-col h-[90vh]">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold"><i class="fas fa-search text-primary mr-2"></i> Adicionar Exercício</h3>
                <button onclick="closeModal()" class="text-gray-400 hover:text-red-500 transition-colors text-2xl leading-none">&times;</button>
            </div>
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <div class="w-full md:w-1/4 border-r overflow-y-auto p-2 space-y-1" style="border-color: var(--border-color)" id="modal-categories"></div>
                <div class="w-full md:w-3/4 overflow-y-auto p-4 bg-black bg-opacity-5" id="modal-exercises"></div>
            </div>
        </div>
    </div>

    <div id="print-area"></div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-app.js";
        import { getAuth, signInWithPopup, GoogleAuthProvider, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, addDoc, getDocs, deleteDoc } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-firestore.js";

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

        // --- BANCO DE DADOS LOCAL (ESTÁTICO + CUSTOMIZADO) ---
        const healthPEF = {
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

        const healthTE = {
            "🟢 Saudável": "Manter treinos regulares de 3–5x por semana, combinando musculação e atividades cardiovasculares.",
            "⚪ Sedentário": "O início deve ser gradual, com treinos leves e foco na adaptação do corpo.",
            "🟡 Sobrepeso": "A prática regular de musculação associada ao cardio pode contribuir para melhora do condicionamento físico.",
            "🔴 Obesidade": "Recomendado iniciar com exercícios de menor impacto e progressão gradual.",
            "⚖️ Baixo peso": "A musculação pode auxiliar no ganho de massa muscular associada a alimentação e descanso.",
            "🍬 Diabetes": "Manter acompanhamento médico regular. Interromper em caso de alteração de glicemia.",
            "❤️ Hipertensão": "Evitar prender a respiração (Manobra de Valsalva) e controlar a intensidade.",
            "🔵 Hipotensão": "Evitar mudanças bruscas de posição. Manter boa hidratação.",
            "💔 Problemas cardíacos": "Prática apenas com liberação e acompanhamento médico.",
            "🦴 Problemas articulares": "Exercícios com menor impacto e maior controle de movimento.",
            "🫁 Problemas respiratórios": "Progressão gradual. Interromper em caso de falta de ar excessiva.",
            "⚠️ Lesões": "Adaptar exercícios respeitando a limitação existente.",
            "🤰 Gestante": "Prática com liberação médica. Foco na segurança e mobilidade.",
            "🤱 Lactante": "Atividade normal, respeitando recuperação e hidratação.",
            "👴 Idoso": "Contribuir para força e equilíbrio respeitando limitações."
        };

        const objData = {
            "Emagrecimento": "Foco em déficit calórico com treinos mistos de força e aeróbicos.",
            "Hipertrofia": "Prioridade na progressão de carga e volume adequado. Superávit calórico.",
            "Definição": "Manutenção de massa muscular enquanto reduz o percentual de gordura.",
            "Condicionamento": "Treinos com menor tempo de intervalo, circuitos e integração cardiopulmonar.",
            "Resistência": "Séries mais longas, cadência controlada e aprimoramento da capacidade.",
            "Força": "Cargas altas, baixas repetições e intervalos de descanso maiores.",
            "Reabilitação": "Treino focado em fortalecimento específico, mobilidade e controle motor.",
            "Saúde geral": "Equilíbrio entre força, cardio e flexibilidade. Constância e bem-estar."
        };

        const defaultCategories = {
            "🔥 PEITO": ["Supino Reto", "Supino Inclinado", "Supino Inclinado com Halteres", "Supino Fechado com Halteres", "Cross Over", "Cross Over Alto", "Cross Over Baixo", "Crucifixo Reto", "Crucifixo Inclinado com Halteres", "Crucifixo na Máquina", "Peck Fly", "Peck Fly Unilateral", "Pullover", "Flexão de Braço", "Flexão com Pés Elevados", "Flexão Explosiva"],
            "🦍 COSTAS": ["Puxada Alta", "Puxada de Frente Supinada", "Pulldown", "Remada Aberta", "Remada Baixa", "Remada Curvada", "Remada Curvada Supinada", "Remada Cavalinho (T-Bar)", "Serrote", "Facepull (puxada de cima para baixo)"],
            "🦵 PERNAS": ["Agachamento Livre", "Agachamento Taça", "Agachamento no Smith", "Agachamento com passada lateral", "Squat", "Hack Machine", "Leg 45°", "Leg 90°", "Agachamento Sumô", "Agachamento Sissy (Livre)", "Afundo", "Recuo", "Avanço", "Passada", "Búlgaro", "Step-up", "Levantamento Terra", "Levantamento Terra Romeno", "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", "Cadeira Flexora", "Elevação Pélvica no Banco", "Elevação Pélvica no Chão", "Elevação Pélvica Unilateral no Chão", "Extensão de Quadril (Glúteo Máximo)", "Extensão Cruzada (Glúteo Médio)", "Coice", "Cachorrinho", "Cadeira Extensora", "Adução", "Abdução", "Abdução Inclinada", "Flexão Nórdica", "Flexão Nórdica Invertida", "Panturrilha Livre", "Panturrilha no Leg Press", "Panturrilha Banco", "Panturrilha Squat", "Panturrilha Unilateral"],
            "💪 BRAÇOS": ["(Bíceps) Rosca Direta", "(Bíceps) Rosca Alternada", "(Bíceps) Rosca 21", "(Bíceps) Rosca Scott Barra W", "(Bíceps) Rosca Scott Unilateral", "(Bíceps) Rosca Scott com Halteres", "(Bíceps) Rosca Martelo", "(Bíceps) Rosca Cross", "(Bíceps) Rosca Inversa", "(Bíceps) Rosca 45°", "(Tríceps) Pulley Unilateral", "(Tríceps) Pulley Barra", "(Tríceps) Pulley Corda", "(Tríceps) Pulley Pegada Inversa", "(Tríceps) Francês na Corda", "(Tríceps) Francês com Halter", "(Tríceps) Francês Unilateral", "(Tríceps) Cruzado Polia Dupla", "(Tríceps) Coice Unilateral", "(Tríceps) Arremesso", "(Tríceps) Testa", "(Tríceps) Mergulho no Banco"],
            "🪨 OMBROS": ["Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", "Elevação Lateral Unilateral Cross", "Elevação Lateral Sentado", "Desenvolvimento com Halteres", "Desenvolvimento com Barra", "Arnold Press", "Elevação Borboleta", "Crucifixo Inverso Sentado com Halteres", "Crucifixo Inverso na Polia", "Crucifixo Inverso Unilateral na Polia", "Facepull (puxada reta)", "Remada Alta", "Encolhimento (Trapézio)"],
            "🧠 ABDÔMEN": ["Infra com Elevação de Perna", "Abdominal Supra", "Abdominal Remador", "Abdominal Bicicleta", "Abdominal Twister com Peso", "Prancha", "Prancha Lateral", "Trituração de Cabos em Pé", "Isometria na parede"],
            "🫀 CARDIO": ["Bicicleta 10 Minutos", "Bicicleta 15 Minutos", "Bicicleta 20 Minutos", "Esteira 10 Minutos", "Esteira 15 Minutos", "Esteira 20 Minutos", "Pular Corda"]
        };
        const techniques = ["Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", "Negativa", "Isometria", "Parciais", "Pirâmide"];
        const daysSequence = ["SEGUNDA-FEIRA", "TERÇA-FEIRA", "QUARTA-FEIRA", "QUINTA-FEIRA", "SEXTA-FEIRA", "SÁBADO", "DOMINGO"];

        window.appState = {
            currentUser: null,
            networkName: '',
            units: [], members: [], customExercises: [], cloudHistory: [], builderWorkouts: [],
            activeModalWorkoutId: null, activeCategory: "🔥 PEITO", currentDocId: null
        };

        const genId = () => Math.random().toString(36).substr(2, 9);

        // --- AUTENTICAÇÃO E INICIALIZAÇÃO ---
        window.loginGoogle = () => { signInWithPopup(auth, new GoogleAuthProvider()).catch(e => alert("Erro: " + e.message)); };
        window.logout = () => signOut(auth);

        onAuthStateChanged(auth, async (user) => {
            if (user) {
                appState.currentUser = user;
                document.getElementById('login-screen').classList.add('hidden');
                document.getElementById('global-loader').classList.remove('hidden');
                await fetchCloudData();
                document.getElementById('global-loader').classList.add('hidden');
            } else {
                appState.currentUser = null;
                document.getElementById('login-screen').classList.remove('hidden');
                document.getElementById('nav-menu').classList.add('hidden');
                document.getElementById('app-screens').classList.add('hidden');
                document.getElementById('setup-screen').classList.add('hidden');
                document.getElementById('global-loader').classList.add('hidden');
            }
        });

        async function fetchCloudData() {
            const uid = appState.currentUser.uid;
            try {
                // Checa a rede
                const netDoc = await getDoc(doc(db, 'users', uid));
                if (!netDoc.exists() || !netDoc.data().networkName) {
                    document.getElementById('setup-screen').classList.remove('hidden');
                    return;
                }
                
                appState.networkName = netDoc.data().networkName;
                document.getElementById('nav-network-name').innerText = appState.networkName;

                // Resto dos dados (Usando a hierarquia correta que obedece as regras do firestore)
                const [snapUnits, snapMembers, snapEx, snapWorkouts] = await Promise.all([
                    getDocs(collection(db, 'users', uid, 'units')),
                    getDocs(collection(db, 'users', uid, 'members')),
                    getDocs(collection(db, 'users', uid, 'customExercises')),
                    getDocs(collection(db, 'users', uid, 'workouts'))
                ]);

                appState.units = snapUnits.docs.map(d => ({ id: d.id, ...d.data() }));
                appState.members = snapMembers.docs.map(d => ({ id: d.id, ...d.data() }));
                appState.customExercises = snapEx.docs.map(d => ({ id: d.id, ...d.data() }));
                appState.cloudHistory = snapWorkouts.docs.map(d => ({ id: d.id, ...d.data() }));

                document.getElementById('nav-menu').classList.remove('hidden');
                document.getElementById('app-screens').classList.remove('hidden');
                document.getElementById('setup-screen').classList.add('hidden');
                
                renderNetworkUI(); populateMemberSelect(); populateReportSelects(); renderHistory(); initBuilderCatSelect();
                
                if(appState.members.length > 0) switchTab('builder'); else switchTab('network');
                if(appState.builderWorkouts.length === 0) window.addWorkoutDay();

            } catch(e) {
                alert("Erro ao conectar base de dados: " + e.message);
            }
        }

        // --- SALVAR SETUP DE REDE ---
        document.getElementById('btn-save-setup').onclick = async () => {
            const netName = document.getElementById('setup-network-name').value.trim();
            const unitName = document.getElementById('setup-unit-name').value.trim();
            if(!netName || !unitName) return alert("Preencha o Nome da Rede e da Primeira Unidade.");
            
            document.getElementById('global-loader').classList.remove('hidden');
            try {
                await setDoc(doc(db, 'users', appState.currentUser.uid), { networkName: netName }, {merge: true});
                await addDoc(collection(db, 'users', appState.currentUser.uid, 'units'), { name: unitName });
                await fetchCloudData();
            } catch(e) { alert("Erro ao configurar rede: " + e.message); }
            document.getElementById('global-loader').classList.add('hidden');
        };

        // --- NAVEGAÇÃO ---
        window.switchTab = (tabId) => {
            document.querySelectorAll('.screen-content').forEach(el => el.classList.add('hidden'));
            document.querySelectorAll('.nav-btn').forEach(el => { el.classList.remove('bg-primary', 'text-white'); el.classList.add('hover:bg-black', 'hover:bg-opacity-10'); });
            document.getElementById(`screen-${tabId}`).classList.remove('hidden');
            document.getElementById(`tab-${tabId}`).classList.add('bg-primary', 'text-white');
            document.getElementById(`tab-${tabId}`).classList.remove('hover:bg-black', 'hover:bg-opacity-10');
        };

        // --- GESTÃO DE REDE ---
        window.addUnit = async () => {
            const nameInput = document.getElementById('new-unit-name');
            const name = nameInput.value.trim();
            if(!name) return;
            const docRef = await addDoc(collection(db, 'users', appState.currentUser.uid, 'units'), { name });
            appState.units.push({ id: docRef.id, name });
            nameInput.value = '';
            renderNetworkUI();
        };

        window.toggleCrefNetwork = () => { document.getElementById('cref-box').style.display = document.getElementById('member-role').value === 'TE' ? 'none' : 'grid'; };

        window.addMember = async () => {
            const unitId = document.getElementById('member-unit').value;
            const name = document.getElementById('member-name').value.trim();
            const cpf = document.getElementById('member-cpf').value.trim();
            const role = document.getElementById('member-role').value;
            const cref = role === 'PEF' ? document.getElementById('member-cref').value.trim() : '';
            const state = role === 'PEF' ? document.getElementById('member-state').value.trim() : '';

            if(!unitId || !name || !cpf) return alert("Preencha Unidade, Nome e CPF.");
            const memberData = { unitId, name, cpf, role, cref, state };
            const docRef = await addDoc(collection(db, 'users', appState.currentUser.uid, 'members'), memberData);
            appState.members.push({ id: docRef.id, ...memberData });
            
            document.getElementById('member-name').value = ''; document.getElementById('member-cpf').value = ''; document.getElementById('member-cref').value = ''; document.getElementById('member-state').value = '';
            renderNetworkUI(); populateMemberSelect();
        };

        window.deleteMember = async (id) => {
            if(confirm("Excluir membro da equipe?")) {
                await deleteDoc(doc(db, 'users', appState.currentUser.uid, 'members', id));
                appState.members = appState.members.filter(m => m.id !== id);
                renderNetworkUI(); populateMemberSelect();
            }
        };

        function renderNetworkUI() {
            document.getElementById('units-list').innerHTML = appState.units.map(u => `<div class="bg-black bg-opacity-10 p-2 rounded text-sm font-bold"><i class="fas fa-building text-primary mr-2"></i>${u.name}</div>`).join('');
            document.getElementById('member-unit').innerHTML = `<option value="">Selecione a Unidade...</option>` + appState.units.map(u => `<option value="${u.id}">${u.name}</option>`).join('');
            document.getElementById('members-table-body').innerHTML = appState.members.map(m => {
                const unitName = appState.units.find(u => u.id === m.unitId)?.name || 'Sem Unidade';
                const docStr = m.role === 'PEF' ? `CREF: ${m.cref}/${m.state}` : `CPF: ${m.cpf}`;
                return `<tr><td class="p-2 font-medium">${m.name}</td><td class="p-2">${unitName}</td><td class="p-2">${m.role === 'PEF' ? 'Ed. Física' : 'Treinador Esp.'}</td><td class="p-2 text-xs opacity-70">${docStr}</td><td class="p-2"><button onclick="deleteMember('${m.id}')" class="text-red-500"><i class="fas fa-trash"></i></button></td></tr>`;
            }).join('');
        }

        // --- BANCO DE EXERCÍCIOS CUSTOMIZADOS ---
        function initBuilderCatSelect() {
            document.getElementById('custom-ex-cat').innerHTML = Object.keys(defaultCategories).map(c => `<option value="${c}">${c}</option>`).join('');
            renderCustomExList();
        }

        window.saveCustomExercise = async () => {
            const cat = document.getElementById('custom-ex-cat').value;
            const name = document.getElementById('custom-ex-name').value.trim();
            const fileInput = document.getElementById('custom-ex-img');
            if(!name) return alert("Digite o nome do exercício.");

            let imgBase64 = null;
            if (fileInput.files && fileInput.files[0]) imgBase64 = await resizeImage(fileInput.files[0], 200);

            const exData = { category: cat, name: name, image: imgBase64 };
            const docRef = await addDoc(collection(db, 'users', appState.currentUser.uid, 'customExercises'), exData);
            appState.customExercises.push({ id: docRef.id, ...exData });
            
            document.getElementById('custom-ex-name').value = ''; fileInput.value = '';
            renderCustomExList();
        };

        window.deleteCustomEx = async (id) => {
            if(confirm("Excluir do banco customizado?")) {
                await deleteDoc(doc(db, 'users', appState.currentUser.uid, 'customExercises', id));
                appState.customExercises = appState.customExercises.filter(e => e.id !== id);
                renderCustomExList();
            }
        };

        function renderCustomExList() {
            document.getElementById('custom-ex-list').innerHTML = appState.customExercises.map(ex => `
                <div class="flex justify-between items-center p-2 border-b border-opacity-10 text-sm" style="border-color: var(--border-color)">
                    <div class="flex items-center gap-2">
                        ${ex.image ? `<img src="${ex.image}" class="w-8 h-8 object-cover rounded">` : `<div class="w-8 h-8 bg-gray-500 bg-opacity-20 rounded flex items-center justify-center text-[10px]">Sem img</div>`}
                        <div><div class="text-[10px] opacity-60">${ex.category}</div><div>${ex.name}</div></div>
                    </div>
                    <button onclick="deleteCustomEx('${ex.id}')" class="text-red-500"><i class="fas fa-trash"></i></button>
                </div>
            `).join('');
        }

        function resizeImage(file, maxSize) {
            return new Promise((resolve) => {
                const reader = new FileReader();
                reader.onload = (e) => {
                    const img = new Image();
                    img.onload = () => {
                        const canvas = document.createElement('canvas');
                        let width = img.width; let height = img.height;
                        if (width > height) { if (width > maxSize) { height *= maxSize / width; width = maxSize; } } else { if (height > maxSize) { width *= maxSize / height; height = maxSize; } }
                        canvas.width = width; canvas.height = height;
                        const ctx = canvas.getContext('2d'); ctx.drawImage(img, 0, 0, width, height);
                        resolve(canvas.toDataURL('image/jpeg', 0.7));
                    };
                    img.src = e.target.result;
                };
                reader.readAsDataURL(file);
            });
        }

        function getFullExerciseList(category) {
            const defaults = defaultCategories[category] || [];
            const customs = appState.customExercises.filter(c => c.category === category);
            const list = defaults.map(name => ({ name, image: null }));
            customs.forEach(c => list.push({ name: c.name, image: c.image }));
            return list;
        }

        // --- BUILDER (MONTAR FICHA) ---
        function populateMemberSelect() {
            document.getElementById('active-member').innerHTML = `<option value="">Selecione o membro da equipe...</option>` + appState.members.map(m => `<option value="${m.id}">${m.name} (${m.role})</option>`).join('');
        }

        window.onMemberChange = () => {
            const memberId = document.getElementById('active-member').value;
            const ws = document.getElementById('builder-workspace');
            const info = document.getElementById('active-member-info');
            if(!memberId) { ws.style.display = 'none'; info.innerText = ''; return; }
            
            const member = appState.members.find(m => m.id === memberId);
            const unit = appState.units.find(u => u.id === member.unitId)?.name || 'Sem Unidade';
            info.innerText = `Atuando como: ${member.role === 'PEF' ? 'Prof. Ed. Física' : 'Treinador Esportivo'} | Unidade: ${unit}`;
            
            ws.style.display = 'grid';
            renderBuilderHealthOptions(member.role);
        };

        function renderBuilderHealthOptions(role) {
            const dataObj = role === 'PEF' ? healthPEF : healthTE;
            const currentSelected = Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value);
            document.getElementById('health-container').innerHTML = Object.keys(dataObj).map(opt => `
                <label class="flex items-start space-x-2 cursor-pointer p-1 rounded hover:bg-black hover:bg-opacity-5">
                    <input type="checkbox" value="${opt}" ${currentSelected.includes(opt)?'checked':''} class="health-cb mt-0.5 w-3 h-3 text-primary"><span>${opt}</span>
                </label>
            `).join('');
        }

        window.changeTheme = () => { document.body.setAttribute('data-theme', document.getElementById('stu-gender').value); };
        window.calcIMC = () => {
            const w = parseFloat(document.getElementById('stu-weight').value), h = parseFloat(document.getElementById('stu-height').value);
            const display = document.getElementById('imc-display');
            if (w > 0 && h > 0) {
                const imc = (w / (h * h)).toFixed(1);
                let cls = imc < 18.5 ? "Abaixo" : imc < 24.9 ? "Normal" : imc < 29.9 ? "Sobrepeso" : "Obesidade";
                display.innerHTML = `<span class="bg-primary bg-opacity-20 text-primary px-2 py-1 rounded-full text-[10px] font-bold border border-primary border-opacity-30">IMC: ${imc} (${cls})</span>`;
            } else display.innerHTML = '';
        };

        window.addWorkoutDay = () => {
            const idx = appState.builderWorkouts.length;
            appState.builderWorkouts.push({ id: genId(), title: idx < daysSequence.length ? `TREINO ${daysSequence[idx]}` : `NOVO TREINO ${idx+1}`, exercises: [] });
            renderBuilderWorkouts();
        };

        window.duplicateWorkout = (id) => {
            const w = appState.builderWorkouts.find(w => w.id === id);
            if(w) { appState.builderWorkouts.push({ ...JSON.parse(JSON.stringify(w)), id: genId(), title: w.title + " (Cópia)" }); renderBuilderWorkouts(); }
        };

        window.removeWorkout = (id) => { if(confirm("Excluir dia?")) { appState.builderWorkouts = appState.builderWorkouts.filter(w => w.id !== id); renderBuilderWorkouts(); } };
        window.updateWorkoutTitle = (id, val) => { const w = appState.builderWorkouts.find(w => w.id === id); if(w) w.title = val; };
        window.removeExercise = (wId, eIdx) => { appState.builderWorkouts.find(w => w.id === wId).exercises.splice(eIdx, 1); renderBuilderWorkouts(); };
        window.updateExercise = (wId, eIdx, f, v) => { appState.builderWorkouts.find(w => w.id === wId).exercises[eIdx][f] = v; };
        window.moveExercise = (wId, eIdx, dir) => {
            const w = appState.builderWorkouts.find(w => w.id === wId);
            if (dir === 'up' && eIdx > 0) [w.exercises[eIdx], w.exercises[eIdx-1]] = [w.exercises[eIdx-1], w.exercises[eIdx]];
            else if (dir === 'down' && eIdx < w.exercises.length - 1) [w.exercises[eIdx], w.exercises[eIdx+1]] = [w.exercises[eIdx+1], w.exercises[eIdx]];
            renderBuilderWorkouts();
        };

        function renderBuilderWorkouts() {
            document.getElementById('workouts-container').innerHTML = appState.builderWorkouts.map(w => {
                const exHtml = w.exercises.length === 0 ? `<div class="p-3 text-xs opacity-50 text-center">Vazio</div>` : w.exercises.map((ex, idx) => `
                    <div class="flex flex-col sm:flex-row gap-2 p-2 items-start sm:items-center border-b border-opacity-10" style="border-color: var(--border-color)">
                        <div class="flex gap-1 hidden sm:flex"><button onclick="moveExercise('${w.id}', ${idx}, 'up')" class="text-[10px] p-1 rounded hover:bg-black hover:bg-opacity-10"><i class="fas fa-chevron-up"></i></button><button onclick="moveExercise('${w.id}', ${idx}, 'down')" class="text-[10px] p-1 rounded hover:bg-black hover:bg-opacity-10"><i class="fas fa-chevron-down"></i></button></div>
                        <div class="flex items-center gap-2 flex-1 w-full sm:w-auto">${ex.image ? `<img src="${ex.image}" class="w-6 h-6 object-cover rounded">` : ''}<div><div class="text-[9px] opacity-60 uppercase">${ex.category}</div><div class="text-xs font-bold">${ex.name}</div></div></div>
                        <div class="flex flex-wrap gap-1 w-full sm:w-auto">
                            <input type="text" value="${ex.sets}" onchange="updateExercise('${w.id}', ${idx}, 'sets', this.value)" class="input-field w-12 rounded px-1 py-1 text-xs text-center" placeholder="Séries"><span class="self-center opacity-50 text-xs">x</span>
                            <input type="text" value="${ex.reps}" onchange="updateExercise('${w.id}', ${idx}, 'reps', this.value)" class="input-field w-16 rounded px-1 py-1 text-xs text-center" placeholder="Reps">
                            <select onchange="updateExercise('${w.id}', ${idx}, 'technique', this.value)" class="input-field w-24 rounded px-1 py-1 text-[10px]">${techniques.map(t => `<option value="${t}" ${ex.technique === t ? 'selected' : ''}>${t}</option>`).join('')}</select>
                            <input type="text" value="${ex.obs}" onchange="updateExercise('${w.id}', ${idx}, 'obs', this.value)" class="input-field flex-1 sm:w-32 rounded px-1 py-1 text-xs" placeholder="Obs...">
                            <button onclick="removeExercise('${w.id}', ${idx})" class="text-red-500 hover:text-red-700 p-1 rounded"><i class="fas fa-trash"></i></button>
                        </div>
                    </div>`).join('');

                return `
                <div class="card rounded-xl overflow-hidden shadow-sm">
                    <div class="p-2 border-b flex justify-between items-center bg-black bg-opacity-5" style="border-color: var(--border-color)">
                        <input type="text" value="${w.title}" onchange="updateWorkoutTitle('${w.id}', this.value)" class="input-field bg-transparent font-bold text-sm w-1/2 px-2 py-1 rounded uppercase">
                        <div class="flex gap-1"><button onclick="duplicateWorkout('${w.id}')" class="text-xs p-1.5 rounded"><i class="fas fa-copy"></i></button><button onclick="removeWorkout('${w.id}')" class="text-xs p-1.5 text-red-500 rounded"><i class="fas fa-trash"></i></button></div>
                    </div>
                    <div>${exHtml}</div>
                    <div class="p-2 border-t" style="border-color: var(--border-color)"><button onclick="openModal('${w.id}')" class="w-full text-primary py-1 rounded text-xs font-bold hover:bg-black hover:bg-opacity-5">+ ADICIONAR EXERCÍCIO</button></div>
                </div>`;
            }).join('');
        }

        window.openModal = (wId) => { appState.activeModalWorkoutId = wId; document.getElementById('exercise-modal').classList.remove('hidden'); renderModalCat(); renderModalEx(); };
        window.closeModal = () => { document.getElementById('exercise-modal').classList.add('hidden'); appState.activeModalWorkoutId = null; };
        
        function renderModalCat() {
            document.getElementById('modal-categories').innerHTML = Object.keys(defaultCategories).map(c => `<button onclick="setModalCat('${c}')" class="w-full text-left px-2 py-2 rounded text-xs font-bold transition ${appState.activeCategory === c ? 'bg-primary text-white' : 'hover:bg-black hover:bg-opacity-10'}">${c}</button>`).join('');
        }
        window.setModalCat = (c) => { appState.activeCategory = c; renderModalCat(); renderModalEx(); };
        function renderModalEx() {
            const list = getFullExerciseList(appState.activeCategory);
            const isCardio = appState.activeCategory === "🫀 CARDIO";
            document.getElementById('modal-exercises').innerHTML = `<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-2">` + list.map((ex, i) => `
                <button onclick="addExToWorkout(${i}, ${isCardio})" class="card p-2 rounded text-left text-xs font-bold border hover:border-primary transition flex justify-between items-center group">
                    <div class="flex items-center gap-2">${ex.image ? `<img src="${ex.image}" class="w-6 h-6 object-cover rounded">` : ''}<span>${ex.name}</span></div>
                    <i class="fas fa-plus opacity-0 group-hover:opacity-100 text-primary"></i>
                </button>`).join('') + `</div>`;
        }

        window.addExToWorkout = (idx, isCardio) => {
            const exObj = getFullExerciseList(appState.activeCategory)[idx];
            const w = appState.builderWorkouts.find(w => w.id === appState.activeModalWorkoutId);
            if(w) { w.exercises.push({ category: appState.activeCategory, name: exObj.name, image: exObj.image, sets: isCardio ? '1' : '3', reps: isCardio ? '-' : '10 a 12', technique: 'Nenhuma', obs: '' }); renderBuilderWorkouts(); }
        };

        // --- SALVAR & IMPRIMIR ---
        window.saveAndPrint = async () => {
            const memberId = document.getElementById('active-member').value;
            if(!memberId) return alert("Selecione o Profissional Responsável na aba Montar Ficha!");
            
            const payload = {
                memberId, timestamp: new Date().getTime(), dateStr: new Date().toLocaleDateString('pt-BR'),
                stuName: document.getElementById('stu-name').value || 'Sem Nome', stuAge: document.getElementById('stu-age').value,
                stuWeight: document.getElementById('stu-weight').value, stuHeight: document.getElementById('stu-height').value,
                stuGender: document.getElementById('stu-gender').value, stuLevel: document.getElementById('stu-level').value,
                stuObjective: document.getElementById('stu-objective').value, stuFreq: document.getElementById('stu-freq').value,
                stuValidity: document.getElementById('stu-validity').value, stuRecs: document.getElementById('stu-recs').value,
                health: Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value),
                workouts: appState.builderWorkouts
            };

            document.getElementById('global-loader').classList.remove('hidden');
            try {
                if(appState.currentDocId) {
                    await setDoc(doc(db, 'users', appState.currentUser.uid, 'workouts', appState.currentDocId), payload);
                    const idx = appState.cloudHistory.findIndex(h => h.id === appState.currentDocId);
                    if(idx > -1) appState.cloudHistory[idx] = { id: appState.currentDocId, ...payload };
                } else {
                    const docRef = await addDoc(collection(db, 'users', appState.currentUser.uid, 'workouts'), payload);
                    appState.currentDocId = docRef.id;
                    appState.cloudHistory.push({ id: docRef.id, ...payload });
                }
                generatePrintHTML(payload);
            } catch(e) { alert("Erro ao salvar na nuvem: " + e.message); }
            finally { document.getElementById('global-loader').classList.add('hidden'); renderHistory(); }
        };

        function generatePrintHTML(data) {
            const member = appState.members.find(m => m.id === data.memberId);
            if(!member) return;
            const unitName = appState.units.find(u => u.id === member.unitId)?.name || '';
            const isPEF = member.role === 'PEF';
            let imcStr = "-"; if(data.stuWeight && data.stuHeight) imcStr = (data.stuWeight / (data.stuHeight * data.stuHeight)).toFixed(1);
            const objTxt = objData[data.stuObjective] || "";
            const hDict = isPEF ? healthPEF : healthTE;

            let legalText = isPEF ? `⚠️ OBSERVAÇÃO LEGAL – PROFISSIONAL DE EDUCAÇÃO FÍSICA<br>Conforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º estabelece que compete ao profissional coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos especializados.` : `⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO<br>Conforme a Lei nº 14.597/2023 (Lei Geral do Esporte), Art. 75, a profissão de treinador esportivo é reconhecida e regulada no Brasil. A atuação possui finalidade orientativa e não substitui avaliação médica. Em casos de doenças ou lesões, recomenda-se avaliação prévia por profissional habilitado. O treinamento respeita limites individuais dentro da atuação técnica e esportiva permitida por lei.`;

            let html = `
                <div class="print-header">
                    <div><h1 class="print-title">Planilha de Treinamento - ${data.stuObjective}</h1><div style="font-size:9px; margin-top:2px;">Unidade: ${unitName}</div></div>
                    <div class="prof-info">Prescrição feita por: ${member.name} (${isPEF ? 'Prof. Educação Física' : 'Treinador Esportivo'})<br>${isPEF ? `CREF: ${member.cref} - UF: ${member.state}` : `CPF: ${member.cpf}`}</div>
                </div>
                <div class="print-grid">
                    <div><strong>Aluno:</strong> ${data.stuName}</div><div><strong>Idade:</strong> ${data.stuAge||'-'}</div><div><strong>Gênero:</strong> ${data.stuGender}</div>
                    <div><strong>Peso:</strong> ${data.stuWeight||'-'}kg</div><div><strong>Altura:</strong> ${data.stuHeight||'-'}m</div><div><strong>IMC:</strong> ${imcStr}</div>
                    <div><strong>Objetivo:</strong> ${data.stuObjective}</div><div><strong>Nível:</strong> ${data.stuLevel}</div><div><strong>Frequência:</strong> ${data.stuFreq}</div>
                    <div><strong>Validade:</strong> ${data.stuValidity}</div>
                </div>`;

            if(objTxt || data.health.length > 0) {
                html += `<div class="print-guidelines"><h4>Diretrizes de Perfil</h4><ul>`;
                if(objTxt) html += `<li><strong>Objetivo (${data.stuObjective}):</strong> ${objTxt}</li>`;
                data.health.forEach(h => { const cleanH = h.replace(/[\u{1F300}-\u{1F9FF}\u{2600}-\u{26FF}\u{2700}-\u{27BF}\u{FE0F}]/gu, '').trim(); if(hDict[h]) html += `<li><strong>Saúde (${cleanH}):</strong> ${hDict[h]}</li>`; });
                html += `</ul></div>`;
            }

            data.workouts.forEach(w => {
                html += `<div class="print-workout"><h3>${w.title}</h3><table><thead><tr><th style="width:35%">Exercício</th><th style="width:10%; text-align:center;">Séries</th><th style="width:10%; text-align:center;">Reps</th><th style="width:15%">Técnica</th><th style="width:30%">Observações</th></tr></thead><tbody>`;
                if(w.exercises.length === 0) html += `<tr><td colspan="5" style="text-align:center;">Vazio</td></tr>`;
                else {
                    w.exercises.forEach(ex => {
                        const imgHtml = ex.image ? `<img src="${ex.image}" class="ex-img-print">` : ``;
                        html += `<tr><td>${imgHtml}<strong>${ex.name}</strong></td><td style="text-align:center;">${ex.sets}</td><td style="text-align:center;">${ex.reps}</td><td>${ex.technique !== 'Nenhuma' ? ex.technique : '-'}</td><td>${ex.obs || '-'}</td></tr>`;
                    });
                }
                html += `</tbody></table></div>`;
            });

            html += `<div class="print-footer-section">`;
            if(data.stuRecs) html += `<strong>Recomendações:</strong><p style="white-space: pre-wrap; margin:2px 0 8px 0;">${data.stuRecs}</p>`;
            html += `<div class="legal-text">${legalText}</div><div style="text-align:center; margin-top:10px; font-size:7px;">Ficha gerada em ${data.dateStr} - PowFit Pro Network</div></div>`;

            document.getElementById('print-area').innerHTML = html;
            setTimeout(() => window.print(), 300);
        }

        // --- NUVEM E RELATÓRIOS ---
        window.renderHistory = () => {
            const term = document.getElementById('search-history').value.toLowerCase();
            let arr = [...appState.cloudHistory].sort((a,b) => b.timestamp - a.timestamp);
            if(term) arr = arr.filter(h => h.stuName.toLowerCase().includes(term));
            const now = new Date().getTime();
            
            document.getElementById('history-grid').innerHTML = arr.map(h => {
                const member = appState.members.find(m => m.id === h.memberId)?.name || 'Prof. Removido';
                const days = parseInt(h.stuValidity.replace(/\D/g, '')) || 30;
                const isExpired = now > h.timestamp + (days * 24 * 60 * 60 * 1000);

                return `<div class="card p-4 rounded-xl border border-opacity-20 flex flex-col justify-between" style="border-color: var(--border-color)">
                    <div><div class="flex justify-between items-start mb-2"><h3 class="font-bold text-lg leading-tight truncate" title="${h.stuName}">${h.stuName}</h3>${isExpired ? `<span class="bg-red-500 bg-opacity-20 text-red-500 text-[10px] font-bold px-2 py-0.5 rounded border border-red-500 border-opacity-30">VENCIDA</span>` : ''}</div>
                    <p class="text-xs opacity-70 mb-1"><i class="fas fa-user-tie"></i> Resp: ${member}</p><p class="text-xs opacity-70 mb-3"><i class="fas fa-calendar-alt"></i> Emissão: ${h.dateStr}</p></div>
                    <div class="flex gap-2 mt-auto"><button onclick="loadFromCloud('${h.id}')" class="btn-primary flex-1 py-1.5 rounded text-xs font-bold shadow"><i class="fas fa-download mr-1"></i> Carregar</button>
                    <button onclick="deleteFromCloud('${h.id}')" class="bg-red-500 hover:bg-red-600 text-white px-3 py-1.5 rounded text-xs transition"><i class="fas fa-trash"></i></button></div>
                </div>`;
            }).join('');
        };

        window.loadFromCloud = (id) => {
            const h = appState.cloudHistory.find(d => d.id === id); if(!h) return;
            appState.currentDocId = h.id; document.getElementById('active-member').value = h.memberId; window.onMemberChange();
            
            ['stu-name','stu-age','stu-weight','stu-height','stu-gender','stu-level','stu-objective','stu-freq','stu-validity','stu-recs'].forEach(field => {
                let v = h[field.replace('stu-','stu')]; 
                if(field === 'stu-gender' && !v) v = 'Masculino';
                if(field === 'stu-level' && !v) v = 'Iniciante';
                if(field === 'stu-objective' && !v) v = 'Emagrecimento';
                if(field === 'stu-freq' && !v) v = '3 dias';
                if(field === 'stu-validity' && !v) v = '30 dias';
                document.getElementById(field).value = v || '';
            });
            
            window.changeTheme(); window.calcIMC();
            document.querySelectorAll('.health-cb').forEach(cb => cb.checked = (h.health || []).includes(cb.value));
            appState.builderWorkouts = h.workouts || []; renderBuilderWorkouts(); switchTab('builder'); window.scrollTo({top:0, behavior:'smooth'});
        };

        window.deleteFromCloud = async (id) => {
            if(!confirm("Excluir definitivamente da nuvem?")) return;
            document.getElementById('global-loader').classList.remove('hidden');
            try {
                await deleteDoc(doc(db, 'users', appState.currentUser.uid, 'workouts', id));
                appState.cloudHistory = appState.cloudHistory.filter(h => h.id !== id);
                if(appState.currentDocId === id) appState.currentDocId = null;
                renderHistory();
            } catch(e) { alert("Erro ao deletar."); } finally { document.getElementById('global-loader').classList.add('hidden'); }
        };

        function populateReportSelects() {
            const months = ["Jan", "Fev", "Mar", "Abr", "Mai", "Jun", "Jul", "Ago", "Set", "Out", "Nov", "Dez"];
            document.getElementById('report-month').innerHTML = months.map((name, i) => `<option value="${i}">${name}</option>`).join('');
            document.getElementById('report-month').value = new Date().getMonth();
            const curYear = new Date().getFullYear();
            document.getElementById('report-year').innerHTML = `<option value="${curYear}">${curYear}</option><option value="${curYear-1}">${curYear-1}</option>`;
        }

        window.generateReport = () => {
            const m = parseInt(document.getElementById('report-month').value);
            const y = parseInt(document.getElementById('report-year').value);
            const filtered = appState.cloudHistory.filter(h => { const d = new Date(h.timestamp); return d.getMonth() === m && d.getFullYear() === y; });

            let reportData = {}; appState.units.forEach(u => reportData[u.id] = { name: u.name, total: 0, members: {} });
            reportData['none'] = { name: 'Sem Unidade', total: 0, members: {} };

            filtered.forEach(h => {
                const member = appState.members.find(mem => mem.id === h.memberId);
                const uId = member ? (member.unitId || 'none') : 'none';
                const mName = member ? member.name : 'Membro Excluído';
                if(!reportData[uId]) reportData[uId] = { name: 'Desconhecida', total: 0, members: {} };
                reportData[uId].total++;
                if(!reportData[uId].members[mName]) reportData[uId].members[mName] = 0;
                reportData[uId].members[mName]++;
            });

            let globalTotal = 0; let html = '';
            for(const [uId, uData] of Object.entries(reportData)) {
                if(uData.total === 0) continue;
                globalTotal += uData.total;
                let membersHtml = '';
                for(const [mName, count] of Object.entries(uData.members)) membersHtml += `<div class="flex justify-between py-1 text-sm border-b border-opacity-10" style="border-color: var(--border-color)"><span>${mName}</span><span class="font-bold">${count} fichas</span></div>`;
                html += `<div class="mb-6 bg-white bg-opacity-5 p-4 rounded-lg"><h3 class="font-bold text-primary mb-2">${uData.name} (Total: ${uData.total})</h3><div class="pl-4">${membersHtml}</div></div>`;
            }

            document.getElementById('report-content').innerHTML = globalTotal === 0 ? `<p class="text-center opacity-50">Nenhuma ficha registrada neste mês.</p>` : `<div class="text-xl font-bold mb-4 text-center">TOTAL GERAL: ${globalTotal} Fichas</div>` + html;
        };

        window.printReport = () => {
            const content = document.getElementById('report-content').innerHTML;
            const mText = document.getElementById('report-month').options[document.getElementById('report-month').selectedIndex].text;
            const yText = document.getElementById('report-year').value;
            let html = `<div class="report-title">Relatório de Produtividade da Rede - ${mText}/${yText}</div>`;
            const div = document.createElement('div'); div.innerHTML = content;
            const units = div.querySelectorAll('.mb-6');
            if(units.length === 0) { html += `<p style="text-align:center;">Nenhum dado encontrado para o período.</p>`; } 
            else {
                html += `<div style="text-align:center; font-weight:bold; font-size:14px; margin-bottom: 10px; border-bottom: 2px solid #000; padding-bottom: 5px;">${div.querySelector('.text-xl').innerText}</div>`;
                units.forEach(u => {
                    const title = u.querySelector('h3').innerText;
                    let mList = '';
                    u.querySelectorAll('.flex').forEach(row => { mList += `<div class="report-member"><span>${row.children[0].innerText}</span><span>${row.children[1].innerText}</span></div>`; });
                    html += `<div class="report-unit"><h3>${title}</h3>${mList}</div>`;
                });
            }
            document.getElementById('print-area').innerHTML = html;
            setTimeout(() => window.print(), 100);
        };
    </script>
</body>
</html>
