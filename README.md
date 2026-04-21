<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro - Plataforma Profissional e Gestão de Franquias</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg-body: #0f172a; --bg-card: #1e293b; --bg-panel: #0b1120;
            --text-main: #f8fafc; --text-muted: #94a3b8; --border-color: #334155;
            --primary: #3b82f6; --primary-hover: #2563eb;
            --input-bg: #0f172a; --input-border: #475569;
        }
        [data-theme="Feminino"] {
            --bg-body: #fdf2f8; --bg-card: #ffffff; --bg-panel: #fce7f3;
            --text-main: #1e293b; --text-muted: #64748b; --border-color: #fbcfe8;
            --primary: #ec4899; --primary-hover: #db2777;
            --input-bg: #f8fafc; --input-border: #f1f5f9;
        }
        body { font-family: 'Inter', sans-serif; background-color: var(--bg-body); color: var(--text-main); transition: all 0.3s ease; }
        .card { background-color: var(--bg-card); border: 1px solid var(--border-color); }
        .input-field { background-color: var(--input-bg); border: 1px solid var(--input-border); color: var(--text-main); }
        .input-field:focus { outline: none; border-color: var(--primary); box-shadow: 0 0 0 1px var(--primary); }
        .btn-primary { background-color: var(--primary); color: white; transition: 0.2s; }
        .btn-primary:hover { background-color: var(--primary-hover); }
        .text-primary { color: var(--primary); }
        .loader { border: 3px solid rgba(255,255,255,0.1); border-left-color: var(--primary); border-radius: 50%; width: 24px; height: 24px; animation: spin 1s linear infinite; }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
        #print-area, #report-print-area { display: none; }

        @media print {
            body { background: white !important; color: black !important; margin: 0; padding: 0; font-family: Arial, sans-serif; }
            #app-container, #login-screen, #profile-setup, .modal, .no-print, nav { display: none !important; }
            #print-area, #report-print-area { display: block !important; padding: 10mm 15mm; font-size: 10px; }
            @page { size: A4; margin: 0; }
            .print-header { border-bottom: 2px solid #000; padding-bottom: 8px; margin-bottom: 12px; text-align: center; }
            .print-title { font-size: 18px; font-weight: bold; margin: 0; text-transform: uppercase; }
            .prof-info { font-size: 11px; margin-top: 5px; font-weight: bold; }
            .print-grid { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 4px; margin-bottom: 12px; border: 1px solid #000; padding: 8px; background: #fff; }
            .print-grid div { font-size: 10px; }
            .print-guidelines { border: 1px solid #000; padding: 8px; margin-bottom: 15px; font-size: 9px; }
            .print-guidelines h4 { margin: 0 0 4px 0; font-size: 10px; text-transform: uppercase; background: #eee; padding: 3px; -webkit-print-color-adjust: exact; color-adjust: exact; }
            .print-guidelines ul { margin: 0; padding-left: 15px; }
            .print-guidelines li { margin-bottom: 2px; }
            .print-workout { margin-bottom: 15px; page-break-inside: avoid; }
            .print-workout h3 { background: #e0e0e0; border: 1px solid #000; border-bottom: none; padding: 4px 8px; margin: 0; font-size: 11px; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            table { width: 100%; border-collapse: collapse; border: 1px solid #000; margin-bottom: 0; }
            th, td { border: 1px solid #000; padding: 5px; text-align: left; font-size: 9.5px; vertical-align: middle; }
            th { background-color: #f0f0f0; font-weight: bold; text-transform: uppercase; text-align: center; -webkit-print-color-adjust: exact; color-adjust: exact; }
            td.text-center { text-align: center; }
            .print-footer-section { margin-top: 15px; border: 1px solid #000; padding: 8px; font-size: 9px; page-break-inside: avoid; }
            .legal-text { font-size: 8px; color: #111; text-align: justify; margin-top: 5px; font-style: italic; line-height: 1.3; }
            .report-header { text-align: center; margin-bottom: 20px; border-bottom: 2px solid #000; padding-bottom: 10px; }
            .report-title { font-size: 18px; font-weight: bold; text-transform: uppercase; }
            .report-unit-section { margin-bottom: 20px; }
            .report-unit-title { background: #333; color: white; padding: 5px; font-size: 12px; font-weight: bold; -webkit-print-color-adjust: exact; color-adjust: exact; }
            .report-table { width: 100%; border-collapse: collapse; margin-top: 5px; }
            .report-table th, .report-table td { border: 1px solid #000; padding: 6px; text-align: left; font-size: 10px; }
        }
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
    </style>
</head>
<body data-theme="Masculino">

    <div id="login-screen" class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-90 z-[100] backdrop-blur-sm">
        <div class="card p-8 rounded-2xl shadow-2xl text-center max-w-sm w-full mx-4">
            <i class="fas fa-dumbbell text-5xl text-primary mb-4"></i>
            <h1 class="text-3xl font-bold mb-2">PowFit Pro</h1>
            <p class="text-sm opacity-70 mb-8">Gestão de Franquias & Fichas de Treino</p>
            <button id="btn-google-login" onclick="loginGoogle()" class="w-full bg-white text-black hover:bg-gray-200 font-bold py-3 px-4 rounded-lg flex items-center justify-center gap-3 transition shadow-lg">
                <img src="https://www.svgrepo.com/show/475656/google-color.svg" class="w-5 h-5" alt="Google">
                Entrar com Google
            </button>
            <div id="login-loader" class="hidden mt-4 flex justify-center"><div class="loader"></div></div>
            <p class="text-[10px] opacity-50 mt-6">Dados protegidos na nuvem Google (Firebase).</p>
        </div>
    </div>

    <div id="profile-setup" class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-90 z-[90] backdrop-blur-sm hidden">
        <div class="card p-6 rounded-2xl shadow-2xl max-w-md w-full mx-4 max-h-[90vh] overflow-y-auto">
            <h2 class="text-xl font-bold mb-1">Completar Cadastro na Rede</h2>
            <p class="text-xs opacity-70 mb-4">Selecione sua unidade e configure seu perfil profissional.</p>
            
            <div class="space-y-4">
                <div><label class="block text-xs font-medium mb-1">Nome Completo</label><input type="text" id="reg-name" class="input-field w-full rounded-lg px-3 py-2 text-sm"></div>
                <div><label class="block text-xs font-medium mb-1">CPF</label><input type="text" id="reg-cpf" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="000.000.000-00"></div>
                <div><label class="block text-xs font-medium mb-1">Unidade (Franquia)</label><select id="reg-unit" class="input-field w-full rounded-lg px-3 py-2 text-sm"></select><p class="text-[10px] opacity-50 mt-1">Se sua unidade não existir, crie no Dashboard da Rede após logar ou peça a um gestor.</p></div>
                <div><label class="block text-xs font-medium mb-1">Estado (UF)</label><input type="text" id="reg-uf" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: SP, RJ"></div>
                <div><label class="block text-xs font-medium mb-1">Categoria de Atuação</label><select id="reg-category" onchange="toggleRegCref()" class="input-field w-full rounded-lg px-3 py-2 text-sm"><option value="PEF">Profissional de Educação Física</option><option value="TE">Treinador Esportivo</option></select></div>
                <div id="reg-cref-container"><label class="block text-xs font-medium mb-1">CREF</label><input type="text" id="reg-cref" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="000000-G/UF"></div>
                <button onclick="saveProfile()" id="btn-save-profile" class="btn-primary w-full py-2.5 rounded-lg font-bold mt-2 shadow-lg">Salvar Perfil</button>
            </div>
        </div>
    </div>

    <div id="app-container" class="hidden min-h-screen flex flex-col">
        <nav class="bg-[var(--bg-panel)] border-b border-[var(--border-color)] sticky top-0 z-40">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                <div class="flex justify-between items-center h-16">
                    <div class="flex items-center gap-3">
                        <i class="fas fa-dumbbell text-2xl text-primary"></i>
                        <div>
                            <span class="font-bold text-lg hidden sm:block leading-none">PowFit Pro</span>
                            <div id="active-member-display" class="text-[10px] text-primary font-medium mt-1">Carregando...</div>
                        </div>
                    </div>
                    <div class="flex items-center gap-1 sm:gap-2 overflow-x-auto whitespace-nowrap">
                        <button onclick="switchTab('builder')" id="tab-builder" class="px-3 py-2 text-sm font-medium rounded-md bg-black bg-opacity-20 text-white transition"><i class="fas fa-clipboard-list sm:mr-1"></i> <span class="hidden sm:inline">Montar Ficha</span></button>
                        <button onclick="switchTab('history')" id="tab-history" class="px-3 py-2 text-sm font-medium rounded-md opacity-70 hover:opacity-100 transition"><i class="fas fa-clock sm:mr-1"></i> <span class="hidden sm:inline">Nuvem (Histórico)</span></button>
                        <button onclick="switchTab('database')" id="tab-database" class="px-3 py-2 text-sm font-medium rounded-md opacity-70 hover:opacity-100 transition"><i class="fas fa-database sm:mr-1"></i> <span class="hidden sm:inline">Exercícios</span></button>
                        <button onclick="switchTab('admin')" id="tab-admin" class="px-3 py-2 text-sm font-medium rounded-md opacity-70 hover:opacity-100 transition"><i class="fas fa-network-wired sm:mr-1"></i> <span class="hidden sm:inline">Gestão de Rede</span></button>
                        <button onclick="logout()" class="px-3 py-2 text-sm font-medium rounded-md text-red-400 hover:text-red-300 ml-2" title="Sair"><i class="fas fa-sign-out-alt"></i></button>
                    </div>
                </div>
            </div>
        </nav>

        <div class="max-w-7xl mx-auto p-4 sm:p-6 lg:p-8 w-full flex-1">
            <div id="view-builder" class="space-y-6">
                <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center bg-black bg-opacity-10 p-4 rounded-xl border border-[var(--border-color)] gap-4 shadow-sm">
                    <div>
                        <h2 class="text-xl font-bold flex items-center gap-2"><i class="fas fa-clipboard-list text-primary"></i> Montagem de Treino</h2>
                        <p class="text-xs opacity-70 mt-1">Crie a ficha manualmente. A impressão salvará a cópia na sua rede.</p>
                    </div>
                    <button onclick="saveAndPrint()" id="btn-save-print" class="btn-primary px-6 py-2.5 rounded-lg text-sm font-bold shadow-lg flex items-center justify-center gap-2 w-full sm:w-auto transition hover:scale-105">
                        <i class="fas fa-save"></i> <i class="fas fa-print"></i> Salvar & Imprimir
                    </button>
                </div>

                <div class="grid grid-cols-1 xl:grid-cols-12 gap-6">
                    <div class="xl:col-span-4 space-y-4">
                        <div class="card rounded-xl p-5 shadow-sm">
                            <div class="flex justify-between items-center mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                                <h2 class="font-bold"><i class="fas fa-user text-primary"></i> Cadastro do Aluno</h2>
                                <div id="imc-display"></div>
                            </div>
                            <div class="space-y-3">
                                <div><label class="block text-[10px] font-medium mb-1 uppercase opacity-70">Nome Completo</label><input type="text" id="stu-name" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="Nome do aluno"></div>
                                <div class="grid grid-cols-3 gap-2">
                                    <div><label class="block text-[10px] font-medium mb-1 uppercase opacity-70">Idade</label><input type="number" id="stu-age" class="input-field w-full rounded px-2 py-2 text-sm" placeholder="Anos"></div>
                                    <div><label class="block text-[10px] font-medium mb-1 uppercase opacity-70">Peso (kg)</label><input type="number" id="stu-weight" oninput="calcIMC()" class="input-field w-full rounded px-2 py-2 text-sm" placeholder="kg"></div>
                                    <div><label class="block text-[10px] font-medium mb-1 uppercase opacity-70">Altura (m)</label><input type="number" id="stu-height" step="0.01" oninput="calcIMC()" class="input-field w-full rounded px-2 py-2 text-sm" placeholder="Opcional"></div>
                                </div>
                                <div class="grid grid-cols-2 gap-2">
                                    <div><label class="block text-[10px] font-medium mb-1 uppercase opacity-70">Gênero</label><select id="stu-gender" onchange="changeTheme()" class="input-field w-full rounded px-2 py-2 text-sm"><option value="Masculino">Masculino</option><option value="Feminino">Feminino</option></select></div>
                                    <div><label class="block text-[10px] font-medium mb-1 uppercase opacity-70">Nível</label><select id="stu-level" class="input-field w-full rounded px-2 py-2 text-sm"><option>Iniciante</option><option>Intermediário</option><option>Avançado</option></select></div>
                                </div>
                                <div class="grid grid-cols-2 gap-2">
                                    <div><label class="block text-[10px] font-medium mb-1 uppercase opacity-70">Frequência</label><select id="stu-freq" class="input-field w-full rounded px-2 py-2 text-sm"><option>3 dias</option><option>5 dias</option><option>6 dias</option><option>Personalizado</option></select></div>
                                    <div><label class="block text-[10px] font-medium mb-1 uppercase opacity-70">Validade</label><select id="stu-validity" class="input-field w-full rounded px-2 py-2 text-sm"><option value="15">15 dias</option><option value="30" selected>30 dias</option><option value="60">60 dias</option><option value="90">90 dias</option></select></div>
                                </div>
                                <div><label class="block text-[10px] font-medium mb-1 uppercase opacity-70">Objetivo (Gera Diretriz)</label><select id="stu-objective" class="input-field w-full rounded px-2 py-2 text-sm"><option>Emagrecimento</option><option>Hipertrofia</option><option>Definição</option><option>Condicionamento</option><option>Resistência</option><option>Força</option><option>Reabilitação</option><option>Saúde geral</option></select></div>
                            </div>
                        </div>
                        <div class="card rounded-xl p-4 shadow-sm">
                            <h2 class="font-bold mb-1"><i class="fas fa-notes-medical text-primary"></i> Saúde</h2>
                            <p class="text-[9px] opacity-60 mb-2 leading-tight">Múltipla escolha. As diretrizes da impressão serão ajustadas automaticamente.</p>
                            <div id="health-container" class="grid grid-cols-1 sm:grid-cols-2 gap-1 text-[11px]"></div>
                        </div>
                        <div class="card rounded-xl p-4 shadow-sm">
                            <h2 class="font-bold mb-2"><i class="fas fa-pen text-primary"></i> Recomendações (Opcional)</h2>
                            <textarea id="stu-recs" rows="3" class="input-field w-full rounded px-2 py-2 text-sm" placeholder="Descanso, hidratação, intensidade..."></textarea>
                        </div>
                    </div>
                    <div class="xl:col-span-8 space-y-4">
                        <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center p-4 rounded-xl card border-dashed border-2 bg-black bg-opacity-5 gap-4">
                            <div><h2 class="font-bold text-lg"><i class="fas fa-layer-group text-primary"></i> Sequência de Treinos</h2><p class="text-[10px] opacity-70">Adicione os dias (Segunda, Terça, etc) e insira os exercícios manualmente.</p></div>
                            <button onclick="addWorkoutDay()" class="bg-gray-700 hover:bg-gray-600 text-white px-4 py-2 rounded-lg text-sm font-bold transition shadow flex-1 sm:flex-none w-full sm:w-auto"><i class="fas fa-plus"></i> Adicionar Dia</button>
                        </div>
                        <div id="workouts-container" class="space-y-5"></div>
                    </div>
                </div>
            </div>

            <div id="view-history" class="hidden space-y-6">
                <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center bg-black bg-opacity-10 p-4 rounded-xl border border-[var(--border-color)] gap-4">
                    <div><h2 class="text-xl font-bold"><i class="fas fa-cloud text-primary"></i> Fichas Salvas (Nuvem)</h2><p class="text-xs opacity-70">Acesse, edite ou exclua as fichas prescritas pela sua Unidade.</p></div>
                    <button onclick="loadCloudData()" class="bg-gray-700 hover:bg-gray-600 text-white px-4 py-2 rounded text-sm font-bold transition"><i class="fas fa-sync-alt"></i> Atualizar Nuvem</button>
                </div>
                <div id="history-list" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4"></div>
            </div>

            <div id="view-database" class="hidden space-y-6">
                <div class="card p-5 rounded-xl">
                    <h2 class="font-bold text-xl mb-2"><i class="fas fa-database text-primary"></i> Meu Banco de Exercícios</h2>
                    <p class="text-sm opacity-70 mb-6">Consulte os exercícios nativos e cadastre novos personalizados.</p>
                    <div class="flex flex-col md:flex-row gap-4 mb-6 items-end border-b border-opacity-20 pb-6" style="border-color: var(--border-color)">
                        <div class="w-full md:w-1/3">
                            <label class="block text-xs font-medium mb-1">Selecione a Categoria</label>
                            <select id="db-category-select" class="input-field rounded-lg px-3 py-2 text-sm w-full" onchange="renderDBExercises()"></select>
                        </div>
                        <div class="w-full md:w-2/3">
                            <label class="block text-xs font-medium mb-1">Cadastrar Exercício Customizado</label>
                            <div class="flex gap-2">
                                <input type="text" id="db-new-exercise" class="input-field flex-1 rounded-lg px-3 py-2 text-sm" placeholder="Ex: Tríceps Testa no Cross">
                                <button onclick="addCustomExercise()" class="btn-primary px-6 py-2 rounded-lg shadow font-bold transition hover:bg-primary-hover"><i class="fas fa-plus"></i> Salvar</button>
                            </div>
                        </div>
                    </div>
                    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-3" id="db-exercise-list"></div>
                </div>
            </div>

            <div id="view-admin" class="hidden space-y-6">
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                    <div class="card rounded-xl p-5 md:col-span-1 h-fit shadow-sm">
                        <h2 class="font-bold text-lg mb-2"><i class="fas fa-chart-line text-primary"></i> Relatório Mensal</h2>
                        <p class="text-xs opacity-70 mb-4">Gere um documento imprimível contabilizando as fichas.</p>
                        <div class="flex flex-col gap-3">
                            <input type="month" id="report-month" class="input-field rounded px-3 py-2 text-sm w-full">
                            <button onclick="generateReport()" class="btn-primary w-full py-2.5 rounded font-bold shadow hover:bg-primary-hover transition"><i class="fas fa-print"></i> Imprimir Relatório</button>
                        </div>
                    </div>
                    <div class="card rounded-xl p-5 md:col-span-2 shadow-sm">
                        <div class="flex justify-between items-center mb-4">
                            <div><h2 class="font-bold text-lg"><i class="fas fa-store text-primary"></i> Unidades da Rede</h2><p class="text-xs opacity-70">Cadastre as franquias da rede PowFit.</p></div>
                            <button onclick="promptAddUnit()" class="bg-gray-700 hover:bg-gray-600 text-white px-3 py-1.5 rounded text-xs font-bold transition"><i class="fas fa-plus"></i> Nova Unidade</button>
                        </div>
                        <div id="admin-units-list" class="space-y-2 max-h-[60vh] overflow-y-auto pr-2"></div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <div id="exercise-modal" class="fixed inset-0 bg-black bg-opacity-80 z-[60] flex items-center justify-center p-2 hidden backdrop-blur-sm">
        <div class="card w-full max-w-5xl rounded-xl shadow-2xl flex flex-col h-[95vh] sm:h-[85vh]">
            <div class="p-4 border-b flex justify-between items-center bg-black bg-opacity-20" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold"><i class="fas fa-search text-primary mr-2"></i> Adicionar Exercício à Ficha</h3>
                <button onclick="closeExerciseModal()" class="text-gray-400 hover:text-red-500 text-2xl leading-none">&times;</button>
            </div>
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <div class="w-full md:w-1/4 border-r overflow-y-auto p-2 space-y-1 bg-black bg-opacity-10" style="border-color: var(--border-color)" id="modal-categories"></div>
                <div class="w-full md:w-3/4 overflow-y-auto p-3" id="modal-exercises"></div>
            </div>
        </div>
    </div>

    <div id="print-area"></div>
    <div id="report-print-area"></div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-app.js";
        import { getAuth, signInWithPopup, GoogleAuthProvider, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, collection, addDoc, getDocs, deleteDoc } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-firestore.js";

        // ==============================================================
        // ⚠️ ATENÇÃO: COLOQUE AQUI AS CHAVES DO SEU PROJETO NOVO
        // ==============================================================
        const firebaseConfig = {
            apiKey: "AIzaSyBWAbrGzyGdDJHbsXYKjAbF3jeULH3FVp0", 
            authDomain: "powfitpro-b1f61.firebaseapp.com",
            projectId: "powfitpro-b1f61",
            storageBucket: "powfitpro-b1f61.firebasestorage.app",
            messagingSenderId: "COLE_SEU_MESSAGING_SENDER_ID_AQUI", 
            appId: "COLE_SEU_APP_ID_AQUI" 
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);

        // Constantes do Sistema
        const HEALTH_PEF = { "🟢 Saudável": "Manter treinos regulares 3–5x por semana, combinando musculação e cardio.", "⚪ Sedentário": "Iniciar com treinos leves 2–3x por semana. Priorizar adaptação.", "🟡 Sobrepeso": "Treinar 3–5x por semana com foco em gasto calórico.", "🔴 Obesidade": "Iniciar com exercícios de baixo impacto. Evolução gradual.", "⚖️ Baixo peso": "Foco em musculação para ganho de massa.", "🍬 Diabetes": "Treinos regulares moderados. Monitorar glicemia.", "❤️ Hipertensão": "Treinos moderados, evitar prender a respiração.", "🔵 Hipotensão": "Evitar mudanças bruscas.", "💔 Problemas cardíacos": "Treinos leves com liberação médica.", "🦴 Problemas articulares": "Evitar impacto.", "🫁 Problemas respiratórios": "Treinos leves a moderados.", "⚠️ Lesões": "Adaptar exercícios.", "🤰 Gestante": "Treinos leves a moderados, sem impacto.", "🤱 Lactante": "Treinar normalmente com atenção à hidratação.", "👴 Idoso": "Foco em força e equilíbrio." };
        const HEALTH_TE = { "🟢 Saudável": "Manter treinos regulares de 3–5x por semana.", "⚪ Sedentário": "O início deve ser gradual, com treinos leves.", "🟡 Sobrepeso": "A prática regular de musculação associada ao cardio contribui.", "🔴 Obesidade": "Início com menor impacto e progressão gradual.", "⚖️ Baixo peso": "A musculação pode auxiliar no ganho de massa.", "🍬 Diabetes": "Acompanhamento médico regular.", "❤️ Hipertensão": "Evitar prender a respiração (Valsalva).", "🔵 Hipotensão": "Evitar mudanças bruscas de posição.", "💔 Problemas cardíacos": "Prática apenas com liberação médica.", "🦴 Problemas articulares": "Menor impacto e maior controle.", "🫁 Problemas respiratórios": "Progressão gradual.", "⚠️ Lesões": "Respeitar a limitação existente.", "🤰 Gestante": "Prática com liberação médica.", "🤱 Lactante": "Atividade mantida com atenção à hidratação.", "👴 Idoso": "Contribui para força e autonomia." };
        const OBJECTIVES = { "Emagrecimento": "Foco em déficit calórico com treinos mistos.", "Hipertrofia": "Prioridade na progressão de carga.", "Definição": "Manutenção de massa muscular reduzindo gordura.", "Condicionamento": "Treinos com menor tempo de intervalo e circuitos.", "Resistência": "Séries longas, cadência controlada.", "Força": "Cargas altas, baixas repetições.", "Reabilitação": "Treino focado em fortalecimento específico.", "Saúde geral": "Equilíbrio entre força, cardio e flexibilidade." };
        const DEFAULT_CATEGORIES = { "🔥 PEITO": ["Supino Reto", "Supino Inclinado", "Cross Over", "Peck Fly", "Flexão"], "🦍 COSTAS": ["Puxada Alta", "Pulldown", "Remada Aberta", "Remada Baixa", "Serrote"], "🦵 PERNAS": ["Agachamento Livre", "Leg 45°", "Mesa Flexora", "Cadeira Extensora", "Panturrilha"], "💪 BRAÇOS": ["Rosca Direta", "Rosca Martelo", "Tríceps Pulley", "Tríceps Testa", "Mergulho"], "🪨 OMBROS": ["Elevação Lateral", "Desenvolvimento", "Elevação Frontal", "Crucifixo Inverso"], "🧠 ABDÔMEN": ["Abdominal Supra", "Prancha", "Infra com Elevação"], "🫀 CARDIO": ["Bicicleta", "Esteira", "Pular Corda"] };
        const TECHNIQUES = ["Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", "Rest-pause", "FST-7", "Pirâmide"];

        window.APP_STATE = { user: null, profile: null, units: [], members: [], customExercises: [], fichas: [], currentFichaId: null, workouts: [], activeModalWorkoutId: null, activeCategory: "🔥 PEITO" };

        window.loginGoogle = async () => {
            document.getElementById('btn-google-login').classList.add('hidden');
            document.getElementById('login-loader').classList.remove('hidden');
            try { await signInWithPopup(auth, new GoogleAuthProvider()); } 
            catch (e) { alert("Erro de Login: Verifique sua API KEY no código. " + e.message); document.getElementById('btn-google-login').classList.remove('hidden'); document.getElementById('login-loader').classList.add('hidden'); }
        };
        
        window.logout = async () => { await signOut(auth); location.reload(); };

        onAuthStateChanged(auth, async (user) => {
            if (user) {
                window.APP_STATE.user = user;
                document.getElementById('login-screen').classList.add('hidden');
                await window.loadCloudData();
                checkProfile(); 
            } else {
                document.getElementById('login-screen').classList.remove('hidden');
                document.getElementById('app-container').classList.add('hidden');
            }
        });

        window.loadCloudData = async () => {
            if(!window.APP_STATE.user) return;
            const uid = window.APP_STATE.user.uid;
            try {
                const uSnap = await getDocs(collection(db, "users", uid, "units")); window.APP_STATE.units = uSnap.docs.map(d => ({id: d.id, ...d.data()}));
                const mSnap = await getDocs(collection(db, "users", uid, "members")); window.APP_STATE.members = mSnap.docs.map(d => ({id: d.id, ...d.data()}));
                const eSnap = await getDocs(collection(db, "users", uid, "custom_exercises")); window.APP_STATE.customExercises = eSnap.docs.map(d => ({id: d.id, ...d.data()}));
                const fSnap = await getDocs(collection(db, "users", uid, "fichas")); window.APP_STATE.fichas = fSnap.docs.map(d => ({id: d.id, ...d.data()}));

                document.getElementById('db-category-select').innerHTML = Object.keys(DEFAULT_CATEGORIES).map(c=>`<option value="${c}">${c}</option>`).join('');
                populateUnitsDropdown();

                if(!document.getElementById('view-history').classList.contains('hidden')) renderHistory();
                if(!document.getElementById('view-admin').classList.contains('hidden')) renderAdmin();
                if(!document.getElementById('view-database').classList.contains('hidden')) window.renderDBExercises();
            } catch(e) { console.error("Erro carregando banco:", e); }
        };

        function checkProfile() {
            const uid = window.APP_STATE.user.uid;
            const profile = window.APP_STATE.members.find(m => m.id === uid);
            if (profile) {
                window.APP_STATE.profile = profile;
                document.getElementById('profile-setup').classList.add('hidden');
                document.getElementById('app-container').classList.remove('hidden');
                const unitName = window.APP_STATE.units.find(u => u.id === profile.unitId)?.name || 'Desconhecida';
                document.getElementById('active-member-display').innerHTML = `<span class="bg-primary text-white px-2 py-0.5 rounded text-[10px] ml-2">👤 ${profile.name} (${profile.category}) - 🏢 ${unitName}</span>`;
                initBuilder(); window.switchTab('builder');
            } else {
                document.getElementById('profile-setup').classList.remove('hidden');
                document.getElementById('app-container').classList.add('hidden');
            }
        }

        window.switchTab = (tab) => {
            ['builder', 'history', 'database', 'admin'].forEach(t => { document.getElementById(`view-${t}`).classList.add('hidden'); const tBtn = document.getElementById(`tab-${t}`); if(tBtn) { tBtn.classList.remove('bg-black', 'bg-opacity-20'); tBtn.classList.add('opacity-70'); } });
            document.getElementById(`view-${tab}`).classList.remove('hidden'); const aBtn = document.getElementById(`tab-${tab}`); if(aBtn) { aBtn.classList.add('bg-black', 'bg-opacity-20'); aBtn.classList.remove('opacity-70'); }
            if(tab === 'admin') renderAdmin(); if(tab === 'database') window.renderDBExercises(); if(tab === 'history') renderHistory();
        };

        function populateUnitsDropdown() {
            const sel = document.getElementById('reg-unit');
            sel.innerHTML = `<option value="">-- Selecione sua Franquia --</option>` + window.APP_STATE.units.map(u => `<option value="${u.id}">${u.name}</option>`).join('');
            if(window.APP_STATE.units.length === 0) sel.innerHTML = `<option value="">(Peça ao Gestor para criar a primeira Unidade)</option>`;
        }

        window.toggleRegCref = () => document.getElementById('reg-cref-container').style.display = document.getElementById('reg-category').value === 'PEF' ? 'block' : 'none';

        window.saveProfile = async () => {
            const btn = document.getElementById('btn-save-profile');
            const name = document.getElementById('reg-name').value; const cpf = document.getElementById('reg-cpf').value; const unitId = document.getElementById('reg-unit').value;
            const uf = document.getElementById('reg-uf').value; const category = document.getElementById('reg-category').value; const cref = document.getElementById('reg-cref').value;
            if(!name || !unitId || !category || !uf) return alert("Preencha Nome, Unidade, Estado e Categoria.");
            
            const uid = window.APP_STATE.user.uid;
            const mData = { name, cpf, unitId, uf, category, cref: category === 'PEF' ? cref : '' };

            btn.innerHTML = `<div class="loader inline-block" style="width:14px;height:14px;"></div> Salvando...`;
            try {
                await setDoc(doc(db, "users", uid, "members", uid), mData);
                window.APP_STATE.members.push({id: uid, ...mData});
                document.getElementById('app-container').classList.remove('hidden');
                checkProfile();
            } catch(e) { alert("Erro ao criar membro."); console.error(e);} finally { btn.innerHTML = `Salvar Perfil`; }
        };

        // --- BUILDER ---
        function initBuilder() {
            window.APP_STATE.currentFichaId = null;
            document.getElementById('stu-name').value = ''; document.getElementById('stu-age').value = ''; document.getElementById('stu-weight').value = ''; document.getElementById('stu-height').value = ''; document.getElementById('stu-recs').value = '';
            window.APP_STATE.workouts = []; window.addWorkoutDay(); updateHealthBoxes(); calcIMC();
        }

        window.calcIMC = () => {
            const w = parseFloat(document.getElementById('stu-weight').value), h = parseFloat(document.getElementById('stu-height').value);
            const display = document.getElementById('imc-display');
            if (w > 0 && h > 0) {
                const imc = (w / (h * h)).toFixed(1); let cls = "Normal"; if (imc < 18.5) cls = "Abaixo"; else if (imc >= 25 && imc < 30) cls = "Sobrepeso"; else if (imc >= 30) cls = "Obesidade";
                display.innerHTML = `<span class="bg-primary bg-opacity-20 text-primary px-3 py-1 rounded-full text-xs font-bold border border-primary border-opacity-30">IMC: ${imc} (${cls})</span>`;
            } else display.innerHTML = '';
        };

        window.changeTheme = () => document.body.setAttribute('data-theme', document.getElementById('stu-gender').value);

        function updateHealthBoxes() {
            if(!window.APP_STATE.profile) return;
            const healthDict = window.APP_STATE.profile.category === 'PEF' ? HEALTH_PEF : HEALTH_TE;
            const selected = Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value);
            document.getElementById('health-container').innerHTML = Object.keys(healthDict).map(opt => `
                <label class="flex items-start space-x-2 cursor-pointer p-1.5 rounded hover:bg-black hover:bg-opacity-5 transition">
                    <input type="checkbox" value="${opt}" ${selected.includes(opt)?'checked':''} class="health-cb mt-0.5 rounded border-gray-400 text-primary w-3.5 h-3.5"><span class="font-medium">${opt}</span>
                </label>`).join('');
        }

        window.addWorkoutDay = () => { window.APP_STATE.workouts.push({ id: Math.random().toString(36).substr(2, 9), title: `NOVO TREINO ${window.APP_STATE.workouts.length + 1}`, exercises: [] }); window.renderBuilderWorkouts(); };
        window.removeWorkout = (id) => { if(confirm("Remover este dia?")) { window.APP_STATE.workouts = window.APP_STATE.workouts.filter(w => w.id !== id); window.renderBuilderWorkouts(); } };
        window.duplicateWorkout = (id) => { const w = window.APP_STATE.workouts.find(x => x.id === id); if(w) { const nw = JSON.parse(JSON.stringify(w)); nw.id = Math.random().toString(36).substr(2, 9); nw.title += " (Cópia)"; window.APP_STATE.workouts.push(nw); window.renderBuilderWorkouts(); } };
        window.updateWorkoutTitle = (id, title) => { const w = window.APP_STATE.workouts.find(x => x.id === id); if(w) w.title = title; };

        window.removeExercise = (wId, idx) => { window.APP_STATE.workouts.find(w=>w.id===wId).exercises.splice(idx,1); window.renderBuilderWorkouts(); };
        window.moveExercise = (wId, idx, dir) => {
            const w = window.APP_STATE.workouts.find(w=>w.id===wId);
            if(dir==='up' && idx>0) { const t=w.exercises[idx]; w.exercises[idx]=w.exercises[idx-1]; w.exercises[idx-1]=t; }
            if(dir==='down' && idx<w.exercises.length-1) { const t=w.exercises[idx]; w.exercises[idx]=w.exercises[idx+1]; w.exercises[idx+1]=t; }
            window.renderBuilderWorkouts();
        };
        window.updateExercise = (wId, idx, field, val) => { window.APP_STATE.workouts.find(w=>w.id===wId).exercises[idx][field] = val; };

        window.renderBuilderWorkouts = () => {
            document.getElementById('workouts-container').innerHTML = window.APP_STATE.workouts.map(w => {
                const exHtml = w.exercises.length === 0 ? `<div class="text-center p-4 text-[10px] opacity-50 italic">Sem exercícios neste dia.</div>` : w.exercises.map((ex, idx) => `
                    <div class="flex flex-col sm:flex-row gap-2 p-2 items-start sm:items-center border-b border-opacity-10 last:border-0">
                        <div class="flex gap-1 hidden sm:flex"><button onclick="moveExercise('${w.id}',${idx},'up')" class="text-[10px] p-1"><i class="fas fa-chevron-up"></i></button><button onclick="moveExercise('${w.id}',${idx},'down')" class="text-[10px] p-1"><i class="fas fa-chevron-down"></i></button></div>
                        <div class="flex-1 w-full sm:w-auto"><div class="text-[9px] opacity-60 uppercase">${ex.category}</div><div class="text-xs font-bold">${ex.name}</div></div>
                        <div class="flex flex-wrap gap-1 w-full sm:w-auto items-center">
                            <input type="text" value="${ex.sets}" onchange="updateExercise('${w.id}',${idx},'sets',this.value)" class="input-field w-12 text-center text-xs px-1 py-1 rounded" placeholder="Séries"><span class="opacity-50 text-xs">x</span>
                            <input type="text" value="${ex.reps}" onchange="updateExercise('${w.id}',${idx},'reps',this.value)" class="input-field w-16 text-center text-xs px-1 py-1 rounded" placeholder="Reps">
                            <select onchange="updateExercise('${w.id}',${idx},'technique',this.value)" class="input-field w-24 text-[10px] px-1 py-1 rounded">${TECHNIQUES.map(t=>`<option value="${t}" ${ex.technique===t?'selected':''}>${t}</option>`).join('')}</select>
                            <input type="text" value="${ex.obs}" onchange="updateExercise('${w.id}',${idx},'obs',this.value)" class="input-field flex-1 sm:w-32 text-xs px-2 py-1 rounded" placeholder="Observações...">
                            <button onclick="removeExercise('${w.id}',${idx})" class="text-red-500 hover:text-red-700 p-1"><i class="fas fa-trash"></i></button>
                        </div>
                    </div>`).join('');

                return `
                <div class="card rounded-xl overflow-hidden shadow-sm border">
                    <div class="p-3 border-b bg-black bg-opacity-20 flex justify-between items-center" style="border-color: var(--border-color)">
                        <input type="text" value="${w.title}" onchange="updateWorkoutTitle('${w.id}',this.value)" class="input-field bg-transparent font-bold text-sm w-1/2 px-2 py-1 rounded uppercase">
                        <div><button onclick="duplicateWorkout('${w.id}')" class="p-1.5 opacity-70 hover:opacity-100 transition"><i class="fas fa-copy"></i></button><button onclick="removeWorkout('${w.id}')" class="p-1.5 text-red-500 transition"><i class="fas fa-trash"></i></button></div>
                    </div>
                    <div>${exHtml}</div>
                    <button onclick="openExerciseModal('${w.id}')" class="w-full text-primary py-2 text-xs font-bold bg-black bg-opacity-10 hover:bg-opacity-30 transition border-t" style="border-color:var(--border-color)">+ ADICIONAR EXERCÍCIO</button>
                </div>`;
            }).join('');
        };

        window.openExerciseModal = (wId) => { window.APP_STATE.activeModalWorkoutId = wId; document.getElementById('exercise-modal').classList.remove('hidden'); window.renderModalCategories(); window.renderModalExercises(); };
        window.closeExerciseModal = () => document.getElementById('exercise-modal').classList.add('hidden');
        window.renderModalCategories = () => { document.getElementById('modal-categories').innerHTML = Object.keys(DEFAULT_CATEGORIES).map(cat => `<button onclick="setModalCategory('${cat}')" class="w-full text-left px-3 py-2 rounded-lg text-xs font-medium transition ${window.APP_STATE.activeCategory === cat ? 'bg-primary text-white shadow' : 'hover:bg-white hover:bg-opacity-10'}">${cat}</button>`).join(''); };
        window.setModalCategory = (cat) => { window.APP_STATE.activeCategory = cat; window.renderModalCategories(); window.renderModalExercises(); };
        window.renderModalExercises = () => {
            const cat = window.APP_STATE.activeCategory;
            const exercisesList = [...DEFAULT_CATEGORIES[cat], ...window.APP_STATE.customExercises.filter(e => e.category === cat).map(e => e.name)];
            document.getElementById('modal-exercises').innerHTML = `<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-2">` + exercisesList.map(name => {
                const safeName = name.replace(/'/g, "\\'");
                return `<button onclick="addExerciseToWorkout('${safeName}', ${cat === "🫀 CARDIO"})" class="card p-3 rounded-lg text-left text-xs font-medium border hover:border-primary transition flex justify-between items-center group"><span>${name}</span><i class="fas fa-plus opacity-0 group-hover:opacity-100 text-primary"></i></button>`
            }).join('') + `</div>`;
        };
        window.addExerciseToWorkout = (name, isCardio) => {
            const w = window.APP_STATE.workouts.find(w => w.id === window.APP_STATE.activeModalWorkoutId);
            if(w) { w.exercises.push({ category: window.APP_STATE.activeCategory, name: name, sets: isCardio ? '1' : '3', reps: isCardio ? '-' : '10 a 12', technique: 'Nenhuma', obs: '' }); window.renderBuilderWorkouts(); const cont = document.getElementById('modal-exercises'); cont.style.opacity = '0.5'; setTimeout(() => cont.style.opacity = '1', 150); }
        };

        // --- SALVAR & IMPRIMIR ---
        window.saveAndPrint = async () => {
            if(!window.APP_STATE.profile) return alert("Você precisa completar seu cadastro.");
            const uid = window.APP_STATE.user.uid;
            
            const fData = {
                memberId: window.APP_STATE.profile.id, unitId: window.APP_STATE.profile.unitId, createdAt: Date.now(),
                stuName: document.getElementById('stu-name').value || 'Não informado', stuAge: document.getElementById('stu-age').value,
                stuWeight: document.getElementById('stu-weight').value, stuHeight: document.getElementById('stu-height').value,
                stuGender: document.getElementById('stu-gender').value, stuLevel: document.getElementById('stu-level').value,
                stuFreq: document.getElementById('stu-freq').value, stuObjective: document.getElementById('stu-objective').value,
                stuValidity: document.getElementById('stu-validity').value, stuRecs: document.getElementById('stu-recs').value,
                health: Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value), workouts: window.APP_STATE.workouts
            };

            const btn = document.getElementById('btn-save-print'); const originalText = btn.innerHTML; btn.innerHTML = `<div class="loader inline-block" style="width:16px; height:16px;"></div>`;
            try {
                let idToUse = window.APP_STATE.currentFichaId;
                if(idToUse) {
                    await setDoc(doc(db, "users", uid, "fichas", idToUse), fData);
                    const idx = window.APP_STATE.fichas.findIndex(f => f.id === idToUse);
                    if(idx>=0) window.APP_STATE.fichas[idx] = {id: idToUse, ...fData};
                } else {
                    const dRef = await addDoc(collection(db, "users", uid, "fichas"), fData);
                    window.APP_STATE.currentFichaId = dRef.id;
                    window.APP_STATE.fichas.push({id: dRef.id, ...fData});
                }
                btn.innerHTML = originalText; executePrint(fData); 
            } catch(e) { console.error(e); alert("Erro ao salvar na nuvem."); btn.innerHTML = originalText; }
        };

        function executePrint(d) {
            const prof = window.APP_STATE.profile;
            const unit = window.APP_STATE.units.find(u => u.id === prof.unitId)?.name || '';
            const isPEF = prof.category === 'PEF';
            let imcStr = "-"; if(d.stuWeight>0 && d.stuHeight>0) imcStr = (d.stuWeight / (d.stuHeight * d.stuHeight)).toFixed(1);
            const objRec = OBJECTIVES[d.stuObjective] || "";
            const hDict = isPEF ? HEALTH_PEF : HEALTH_TE;
            const legalPEF = `⚠️ OBSERVAÇÃO LEGAL – PROF. ED. FÍSICA<br>Conforme Lei nº 9.696/1998, Art. 1º e 3º, o planejamento e execução são prerrogativas do profissional de Educação Física no CREF.`;
            const legalTE = `⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO<br>Conforme Lei nº 14.597/2023, Art. 75, atuação de caráter técnico esportivo. Possui finalidade orientativa e não substitui avaliação médica.`;

            let html = `
                <div class="print-header">
                    <h1 class="print-title">Planilha de Treinamento - ${d.stuObjective}</h1>
                    <div class="prof-info">Prescrição feita por: ${prof.name} (${isPEF ? 'Prof. Ed. Física' : 'Treinador Esportivo'}) | ${isPEF ? `CREF: ${prof.cref} - UF: ${prof.uf}` : `UF: ${prof.uf}`}</div>
                </div>
                <div class="print-grid">
                    <div><strong>Aluno:</strong> ${d.stuName}</div><div><strong>Idade:</strong> ${d.stuAge || '-'}</div><div><strong>Gênero:</strong> ${d.stuGender}</div>
                    <div><strong>Peso:</strong> ${d.stuWeight || '-'} kg</div><div><strong>Altura:</strong> ${d.stuHeight || '-'} m</div><div><strong>IMC:</strong> ${imcStr}</div>
                    <div><strong>Nível:</strong> ${d.stuLevel}</div><div><strong>Frequência:</strong> ${d.stuFreq}</div><div><strong>Validade:</strong> ${d.stuValidity} dias</div>
                </div>`;

            if (objRec || d.health.length > 0) {
                html += `<div class="print-guidelines"><h4>Diretrizes de Saúde</h4><ul>`;
                if(objRec) html += `<li><strong>Objetivo:</strong> ${objRec}</li>`;
                d.health.forEach(k => { if(hDict[k]) html += `<li><strong>${k.replace(/[\u{1F300}-\u{1F9FF}\u{2600}-\u{26FF}\u{2700}-\u{27BF}\u{FE0F}]/gu, '').trim()}:</strong> ${hDict[k]}</li>`; });
                html += `</ul></div>`;
            }

            d.workouts.forEach(w => {
                html += `<div class="print-workout"><h3>${w.title}</h3><table><thead><tr><th style="width: 35%">Exercício</th><th style="width: 8%">Séries</th><th style="width: 12%">Reps</th><th style="width: 15%">Técnica</th><th style="width: 30%">Obs</th></tr></thead><tbody>`;
                if (w.exercises.length === 0) html += `<tr><td colspan="5" class="text-center" style="font-style:italic">Sem exercícios</td></tr>`;
                else w.exercises.forEach(ex => { html += `<tr><td><strong>${ex.name}</strong></td><td class="text-center">${ex.sets}</td><td class="text-center">${ex.reps}</td><td class="text-center">${ex.technique==='Nenhuma'?'-':ex.technique}</td><td>${ex.obs || '-'}</td></tr>`; });
                html += `</tbody></table></div>`;
            });

            html += `<div class="print-footer-section">`;
            if(d.stuRecs.trim()) html += `<strong>Recomendações:</strong><p style="white-space: pre-wrap; margin: 3px 0 10px 0;">${d.stuRecs}</p>`;
            html += `<div class="legal-text">${isPEF ? legalPEF : legalTE}</div><div style="text-align:center; margin-top:10px; font-size:8px; font-weight:bold;">PowFit Pro - ${new Date(d.createdAt).toLocaleString('pt-BR')} - Unidade: ${unit}</div></div>`;

            document.getElementById('print-area').innerHTML = html;
            setTimeout(() => window.print(), 400);
        }

        // --- RELATÓRIOS E NUVEM ---
        window.renderHistory = () => {
            const list = document.getElementById('history-list');
            const unitFichas = window.APP_STATE.fichas.filter(f => f.unitId === window.APP_STATE.profile.unitId);
            if(unitFichas.length === 0) { list.innerHTML = '<div class="text-xs opacity-50 p-2 col-span-3 text-center">Nenhuma ficha salva nesta Unidade.</div>'; return; }
            
            const sorted = [...unitFichas].sort((a,b) => b.createdAt - a.createdAt);
            list.innerHTML = sorted.map(f => {
                const isExpired = (f.createdAt + (parseInt(f.stuValidity) * 24 * 60 * 60 * 1000)) < Date.now();
                return `
                <div class="card p-4 rounded-xl flex flex-col gap-3 shadow-sm">
                    <div>
                        <div class="font-bold text-sm">${f.stuName} ${isExpired ? `<span class="bg-red-500 text-white px-2 py-0.5 rounded text-[9px] font-bold ml-2">Vencida</span>` : ''}</div>
                        <div class="text-[10px] opacity-70">Salvo: ${new Date(f.createdAt).toLocaleDateString('pt-BR')} | Obj: ${f.stuObjective}</div>
                    </div>
                    <div class="flex gap-2"><button onclick="loadFicha('${f.id}')" class="flex-1 bg-gray-700 hover:bg-gray-600 text-white py-2 rounded text-xs font-bold shadow"><i class="fas fa-edit"></i> Editar</button><button onclick="deleteFicha('${f.id}')" class="bg-red-500 hover:bg-red-600 text-white px-3 py-2 rounded text-xs shadow"><i class="fas fa-trash"></i></button></div>
                </div>`;
            }).join('');
        };

        window.loadFicha = (id) => {
            const f = window.APP_STATE.fichas.find(x => x.id === id); if(!f) return;
            window.APP_STATE.currentFichaId = f.id;
            ['name', 'age', 'weight', 'height', 'gender', 'level', 'freq', 'objective', 'validity', 'recs'].forEach(k => {
                const el = document.getElementById(`stu-${k}`);
                if (el) el.value = f[`stu${k.charAt(0).toUpperCase() + k.slice(1)}`] || '';
            });
            window.changeTheme(); window.calcIMC();
            document.querySelectorAll('.health-cb').forEach(cb => { cb.checked = (f.health || []).includes(cb.value); });
            window.APP_STATE.workouts = f.workouts || [];
            window.switchTab('builder'); window.renderBuilderWorkouts();
        };

        window.deleteFicha = async (id) => {
            if(confirm("Excluir permanentemente da nuvem?")) {
                try { await deleteDoc(doc(db, "users", window.APP_STATE.user.uid, "fichas", id)); window.APP_STATE.fichas = window.APP_STATE.fichas.filter(x => x.id !== id); if(window.APP_STATE.currentFichaId === id) window.APP_STATE.currentFichaId = null; window.renderHistory(); } catch(e) { alert("Erro ao excluir."); }
            }
        };

        // --- ADMIN & DB ---
        window.renderDBExercises = () => {
            const cat = document.getElementById('db-category-select').value || "🔥 PEITO";
            let html = '';
            DEFAULT_CATEGORIES[cat].forEach(name => { html += `<div class="card p-3 rounded-xl text-xs font-medium flex justify-between items-center opacity-70"><span>${name}</span><i class="fas fa-lock text-[9px]"></i></div>`; });
            window.APP_STATE.customExercises.filter(e => e.category === cat).forEach(e => { html += `<div class="card p-3 rounded-xl text-xs font-medium border-primary border flex justify-between items-center shadow-md bg-black bg-opacity-20"><span>${e.name}</span><button onclick="deleteCustomExercise('${e.id}')" class="text-red-500 hover:text-red-400 p-1"><i class="fas fa-trash"></i></button></div>`; });
            document.getElementById('db-exercise-list').innerHTML = html;
        };

        window.addCustomExercise = async () => {
            const cat = document.getElementById('db-category-select').value, name = document.getElementById('db-new-exercise').value;
            if(!name.trim()) return;
            try { const docRef = await addDoc(collection(db, "users", window.APP_STATE.user.uid, "custom_exercises"), { category: cat, name: name.trim() }); window.APP_STATE.customExercises.push({ id: docRef.id, category: cat, name: name.trim() }); document.getElementById('db-new-exercise').value = ''; window.renderDBExercises(); } catch(e) { alert("Erro."); }
        };

        window.deleteCustomExercise = async (id) => { if(confirm("Apagar exercício customizado?")) { try { await deleteDoc(doc(db, "users", window.APP_STATE.user.uid, "custom_exercises", id)); window.APP_STATE.customExercises = window.APP_STATE.customExercises.filter(e => e.id !== id); window.renderDBExercises(); } catch(e) { alert("Erro ao deletar."); } } };

        window.renderAdmin = () => {
            document.getElementById('admin-units-list').innerHTML = window.APP_STATE.units.length === 0 ? '<div class="text-xs opacity-50">Sem unidades. Crie a primeira.</div>' : window.APP_STATE.units.map(u => `<div class="p-3 border border-gray-500 border-opacity-20 rounded-xl mb-2 flex justify-between bg-black bg-opacity-10"><span class="text-sm font-medium">${u.name}</span><span class="text-[10px] bg-primary text-white px-2 py-0.5 rounded-full">${window.APP_STATE.members.filter(m => m.unitId === u.id).length} Membros</span></div>`).join('');
        };

        window.promptAddUnit = async () => {
            const name = prompt("Nome da Nova Unidade (Franquia):");
            if(name && name.trim()) { const docRef = await addDoc(collection(db, "users", window.APP_STATE.user.uid, "units"), { name: name.trim() }); window.APP_STATE.units.push({ id: docRef.id, name: name.trim() }); window.renderAdmin(); populateUnitsDropdown(); }
        };

        window.generateReport = () => {
            const monthVal = document.getElementById('report-month').value; if(!monthVal) return alert("Selecione um mês/ano");
            const [rYear, rMonth] = monthVal.split('-');
            let html = `<div class="report-header"><h1 class="report-title">Relatório de Produtividade - ${rMonth}/${rYear}</h1><p>Total de Prescrições Geradas pela Rede</p></div>`;
            let hasData = false;

            window.APP_STATE.units.forEach(unit => {
                let unitTable = ''; let unitTotal = 0;
                window.APP_STATE.members.filter(m => m.unitId === unit.id).forEach(member => {
                    const fichasDoMembro = window.APP_STATE.fichas.filter(f => f.memberId === member.id && new Date(f.createdAt).getFullYear() == rYear && (new Date(f.createdAt).getMonth() + 1) == rMonth);
                    if(fichasDoMembro.length > 0) { unitTotal += fichasDoMembro.length; unitTable += `<tr><td>${member.name} (${member.category})</td><td style="text-align:center">${fichasDoMembro.length}</td></tr>`; hasData = true; }
                });
                if(unitTotal > 0) html += `<div class="report-unit-section"><div class="report-unit-title">📍 ${unit.name} - Total: ${unitTotal} fichas prescritas</div><table class="report-table"><thead><tr><th>Membro da Equipe</th><th style="width:100px; text-align:center">Qtd. Fichas</th></tr></thead><tbody>${unitTable}</tbody></table></div>`;
            });
            if(!hasData) html += '<p style="text-align:center; padding: 20px;">Nenhuma ficha registrada na rede neste mês.</p>';

            document.getElementById('report-print-area').innerHTML = html;
            const appC = document.getElementById('app-container'); const printA = document.getElementById('print-area'); const repA = document.getElementById('report-print-area');
            appC.style.display = 'none'; printA.style.display = 'none'; repA.style.display = 'block'; window.print(); appC.style.display = 'flex'; printA.style.display = 'none'; repA.style.display = 'none';
        };
    </script>
</body>
</html>
