<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro - Plataforma Master</title>
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
        .text-primary { color: var(--primary); }
        
        /* Oculta área de impressão na tela */
        #print-area { display: none; }

        /* Estilos de Impressão Estilo Planilha */
        @media print {
            body { background: white !important; color: black !important; margin: 0; padding: 0; }
            #app-view, #login-view, .no-print, .modals { display: none !important; }
            #print-area { display: block !important; padding: 10mm; font-family: 'Arial', sans-serif; font-size: 10px; }
            @page { size: A4; margin: 0; }
            
            .print-header { border-bottom: 2px solid #000; padding-bottom: 5px; margin-bottom: 10px; text-align: center; }
            .print-title { font-size: 16px; font-weight: bold; margin: 0; text-transform: uppercase; }
            .prof-info { font-size: 11px; margin-top: 3px; font-weight: bold; }
            
            .print-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 4px; margin-bottom: 10px; border: 1px solid #000; padding: 6px; }
            .print-grid div { font-size: 10px; }
            .print-grid .full-row { grid-column: 1 / -1; }
            
            .print-guidelines { border: 1px solid #000; padding: 6px; margin-bottom: 10px; font-size: 9px; }
            .print-guidelines h4 { margin: 0 0 3px 0; font-size: 10px; text-transform: uppercase; background: #e5e5e5; padding: 2px; border-bottom: 1px solid #000;}
            .print-guidelines ul { margin: 0; padding-left: 12px; }
            .print-guidelines li { margin-bottom: 2px; }

            .print-workout { margin-bottom: 10px; page-break-inside: avoid; }
            .print-workout h3 { background: #d4d4d4; border: 1px solid #000; border-bottom: none; padding: 3px 6px; margin: 0; font-size: 11px; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; font-weight: bold;}
            table { width: 100%; border-collapse: collapse; border: 1px solid #000; }
            th, td { border: 1px solid #000; padding: 3px 4px; text-align: left; font-size: 9.5px; }
            th { background-color: #e5e5e5; font-weight: bold; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            
            .print-footer-section { margin-top: 10px; border: 1px solid #000; padding: 6px; font-size: 9px; page-break-inside: avoid;}
            .legal-text { font-size: 8px; color: #222; text-align: justify; margin-top: 5px; font-style: italic;}
            .expired-stamp { color: red; font-weight: bold; border: 2px solid red; padding: 5px; display: inline-block; margin-bottom: 10px; transform: rotate(-5deg); -webkit-print-color-adjust: exact;}
        }

        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
    </style>
</head>
<body data-theme="Masculino" class="min-h-screen pb-10">

    <!-- LOGIN VIEW -->
    <div id="login-view" class="min-h-screen flex items-center justify-center p-4">
        <div class="card p-8 rounded-2xl shadow-2xl text-center max-w-md w-full">
            <i class="fas fa-dumbbell text-5xl text-primary mb-4"></i>
            <h1 class="text-3xl font-bold mb-2">PowFit Pro</h1>
            <p class="text-sm var(--text-muted) mb-8">Plataforma Administrativa de Prescrição</p>
            <button onclick="window.loginGoogle()" class="w-full bg-white text-gray-800 font-bold py-3 px-4 rounded-lg shadow hover:bg-gray-100 flex items-center justify-center gap-3 transition">
                <img src="https://www.svgrepo.com/show/475656/google-color.svg" class="w-5 h-5" alt="Google">
                Entrar com Google
            </button>
            <p class="text-xs opacity-50 mt-4">Sincronização na nuvem ativada.</p>
        </div>
    </div>

    <!-- APP VIEW -->
    <div id="app-view" class="hidden">
        <!-- NAVBAR -->
        <nav class="border-b border-opacity-20 mb-6 p-4 sticky top-0 z-40 backdrop-blur-md" style="border-color: var(--border-color); background-color: rgba(var(--bg-card), 0.9);">
            <div class="max-w-7xl mx-auto flex flex-wrap justify-between items-center gap-4">
                <div class="flex items-center gap-3">
                    <i class="fas fa-dumbbell text-2xl text-primary"></i>
                    <span class="text-xl font-bold">PowFit Pro</span>
                </div>
                <div class="flex flex-wrap items-center gap-2 sm:gap-4">
                    <button onclick="window.switchTab('dashboard')" id="btn-tab-dashboard" class="px-4 py-2 rounded-lg font-medium text-sm transition btn-primary">Dashboard & Gestão</button>
                    <button onclick="window.switchTab('creator')" id="btn-tab-creator" class="px-4 py-2 rounded-lg font-medium text-sm transition hover:bg-black hover:bg-opacity-20 border border-transparent">Prancheta (Criar Ficha)</button>
                    <div class="h-6 w-px bg-gray-500 opacity-30 hidden sm:block"></div>
                    <div class="flex items-center gap-2 text-sm">
                        <img id="user-avatar" class="w-8 h-8 rounded-full border border-primary" src="" alt="Avatar">
                        <button onclick="window.logout()" class="text-red-400 hover:text-red-300" title="Sair"><i class="fas fa-sign-out-alt"></i></button>
                    </div>
                </div>
            </div>
        </nav>

        <div class="max-w-7xl mx-auto p-4 sm:p-0">
            
            <!-- ================= DASHBOARD & GESTÃO ================= -->
            <div id="dashboard-section" class="space-y-6">
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                    
                    <!-- UNIDADES -->
                    <div class="card p-5 rounded-xl">
                        <div class="flex justify-between items-center mb-4">
                            <h2 class="text-lg font-bold"><i class="fas fa-building text-primary mr-2"></i> Unidades da Rede</h2>
                            <button onclick="window.openModal('unit-modal')" class="text-xs bg-primary px-3 py-1.5 rounded text-white"><i class="fas fa-plus"></i> Nova</button>
                        </div>
                        <div id="units-list" class="space-y-2 max-h-64 overflow-y-auto pr-2">
                            <!-- JS -->
                        </div>
                    </div>

                    <!-- MEMBROS (EQUIPE) -->
                    <div class="card p-5 rounded-xl">
                        <div class="flex justify-between items-center mb-4">
                            <h2 class="text-lg font-bold"><i class="fas fa-users text-primary mr-2"></i> Equipe (Membros)</h2>
                            <button onclick="window.openModal('member-modal')" class="text-xs bg-primary px-3 py-1.5 rounded text-white"><i class="fas fa-plus"></i> Novo</button>
                        </div>
                        <div id="members-list" class="space-y-2 max-h-64 overflow-y-auto pr-2">
                            <!-- JS -->
                        </div>
                    </div>
                </div>

                <!-- RELATÓRIO E HISTÓRICO -->
                <div class="card p-5 rounded-xl">
                    <div class="flex flex-col sm:flex-row justify-between items-center mb-6 gap-4">
                        <h2 class="text-lg font-bold"><i class="fas fa-chart-bar text-primary mr-2"></i> Produtividade & Histórico</h2>
                        <div class="flex gap-2 w-full sm:w-auto">
                            <input type="month" id="report-month" class="input-field rounded px-3 py-2 text-sm w-full sm:w-auto" onchange="window.loadWorkouts()">
                            <button onclick="window.printReport()" class="btn-primary px-4 py-2 rounded text-sm whitespace-nowrap"><i class="fas fa-print"></i> Imprimir Relatório</button>
                        </div>
                    </div>
                    
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                        <!-- Resumo do Mês -->
                        <div class="md:col-span-1 bg-black bg-opacity-20 p-4 rounded-lg border border-gray-500 border-opacity-20 h-fit">
                            <h3 class="font-bold border-b border-gray-500 border-opacity-30 pb-2 mb-3">Resumo da Rede</h3>
                            <div id="report-summary" class="text-sm space-y-2">
                                <!-- JS -->
                            </div>
                        </div>
                        
                        <!-- Lista de Fichas -->
                        <div class="md:col-span-2 space-y-3 max-h-96 overflow-y-auto pr-2" id="workouts-history-list">
                            <!-- JS -->
                        </div>
                    </div>
                </div>
            </div>

            <!-- ================= PRANCHETA (CRIAR FICHA) ================= -->
            <div id="creator-section" class="hidden space-y-6">
                
                <!-- SELETOR DE CONTEXTO (Quem está criando) -->
                <div class="card p-4 rounded-xl border-l-4 border-l-primary bg-primary bg-opacity-5">
                    <div class="flex flex-col sm:flex-row gap-4 items-center">
                        <div class="font-bold text-sm sm:w-1/4">Autor da Ficha:</div>
                        <select id="ctx-unit" onchange="window.updateCtxMembers()" class="input-field rounded-lg px-3 py-2 text-sm w-full sm:w-1/3">
                            <option value="">Selecione a Unidade...</option>
                        </select>
                        <select id="ctx-member" onchange="window.applyMemberContext()" class="input-field rounded-lg px-3 py-2 text-sm w-full sm:w-1/3">
                            <option value="">Selecione o Profissional...</option>
                        </select>
                    </div>
                    <p id="ctx-warning" class="text-xs text-red-400 mt-2 hidden">Selecione o profissional para liberar a montagem e regras legais.</p>
                </div>

                <!-- ÁREA DE MONTAGEM (Oculta até selecionar membro) -->
                <div id="assembly-area" class="grid grid-cols-1 xl:grid-cols-12 gap-6 opacity-50 pointer-events-none transition-opacity">
                    
                    <!-- ESQUERDA: Dados do Aluno -->
                    <div class="xl:col-span-4 space-y-4">
                        <div class="card rounded-xl p-5">
                            <div class="flex justify-between items-center mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                                <h2 class="font-bold"><i class="fas fa-user text-primary mr-2"></i> Dados do Aluno</h2>
                                <div id="imc-display"></div>
                            </div>
                            <div class="space-y-3 text-sm">
                                <input type="text" id="stu-name" class="input-field w-full rounded px-3 py-2" placeholder="Nome Completo">
                                <div class="grid grid-cols-3 gap-2">
                                    <input type="number" id="stu-age" class="input-field rounded px-2 py-2" placeholder="Idade">
                                    <input type="number" id="stu-weight" oninput="window.calcIMC()" class="input-field rounded px-2 py-2" placeholder="Peso (kg)">
                                    <input type="number" id="stu-height" step="0.01" oninput="window.calcIMC()" class="input-field rounded px-2 py-2" placeholder="Alt (m)">
                                </div>
                                <div class="grid grid-cols-2 gap-2">
                                    <select id="stu-gender" onchange="window.changeTheme()" class="input-field rounded px-3 py-2">
                                        <option value="Masculino">Masculino</option>
                                        <option value="Feminino">Feminino</option>
                                    </select>
                                    <select id="stu-level" class="input-field rounded px-3 py-2">
                                        <option>Iniciante</option><option>Intermediário</option><option>Avançado</option>
                                    </select>
                                </div>
                                <select id="stu-freq" class="input-field w-full rounded px-3 py-2">
                                    <option>Frequência: 3 dias</option><option>Frequência: 5 dias</option><option>Frequência: 6 dias</option><option>Personalizado</option>
                                </select>
                                <select id="stu-objective" onchange="window.updateObjText()" class="input-field w-full rounded px-3 py-2">
                                    <option value="Emagrecimento">Obj: Emagrecimento</option>
                                    <option value="Hipertrofia">Obj: Hipertrofia</option>
                                    <option value="Definição">Obj: Definição</option>
                                    <option value="Condicionamento">Obj: Condicionamento</option>
                                    <option value="Resistência">Obj: Resistência</option>
                                    <option value="Força">Obj: Força</option>
                                    <option value="Reabilitação">Obj: Reabilitação</option>
                                    <option value="Saúde geral">Obj: Saúde geral</option>
                                </select>
                                <p id="obj-hint" class="text-[10px] opacity-70 p-2 bg-black bg-opacity-20 rounded border border-gray-600 hidden"></p>
                            </div>
                        </div>

                        <div class="card rounded-xl p-5">
                            <h2 class="font-bold mb-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                                <i class="fas fa-heartbeat text-primary mr-2"></i> Saúde (Diretrizes)
                            </h2>
                            <div class="grid grid-cols-2 gap-2 text-xs" id="health-container">
                                <!-- Preenchido baseado na categoria do membro -->
                            </div>
                        </div>
                    </div>

                    <!-- DIREITA: Treinos -->
                    <div class="xl:col-span-8 space-y-4">
                        <div class="flex justify-between items-center card p-3 rounded-xl border-dashed border-2 bg-opacity-10">
                            <select id="stu-validity" class="input-field rounded px-3 py-1.5 text-sm">
                                <option>Validade: 15 dias</option><option selected>Validade: 30 dias</option><option>Validade: 60 dias</option><option>Validade: 90 dias</option>
                            </select>
                            <div class="flex gap-2">
                                <button onclick="window.addDay()" class="btn-primary px-3 py-1.5 rounded text-sm"><i class="fas fa-plus"></i> Dia</button>
                                <button onclick="window.saveAndPrint()" class="bg-green-600 hover:bg-green-500 text-white px-4 py-1.5 rounded text-sm font-bold shadow-lg flex items-center gap-2"><i class="fas fa-save"></i> Imprimir</button>
                            </div>
                        </div>

                        <div id="days-container" class="space-y-4">
                            <!-- Dias JS -->
                        </div>

                        <div class="card rounded-xl p-4">
                            <h2 class="font-bold mb-2 text-sm"><i class="fas fa-pen text-primary mr-2"></i> Recomendações Livres</h2>
                            <textarea id="stu-recs" rows="2" class="input-field w-full rounded p-2 text-sm" placeholder="Hidratação, cuidados..."></textarea>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- MODAIS DE GESTÃO -->
    <div id="unit-modal" class="fixed inset-0 bg-black bg-opacity-70 hidden z-50 flex items-center justify-center modals p-4">
        <div class="card w-full max-w-sm rounded-xl p-5">
            <h3 class="font-bold text-lg mb-4">Nova Unidade</h3>
            <input type="text" id="unit-name" class="input-field w-full rounded p-2 text-sm mb-4" placeholder="Nome da Unidade (Ex: Centro)">
            <div class="flex justify-end gap-2">
                <button onclick="window.closeModal('unit-modal')" class="px-4 py-2 rounded text-sm opacity-70">Cancelar</button>
                <button onclick="window.saveUnit()" class="btn-primary px-4 py-2 rounded text-sm">Salvar</button>
            </div>
        </div>
    </div>

    <div id="member-modal" class="fixed inset-0 bg-black bg-opacity-70 hidden z-50 flex items-center justify-center modals p-4">
        <div class="card w-full max-w-md rounded-xl p-5">
            <h3 class="font-bold text-lg mb-4">Novo Membro (Equipe)</h3>
            <div class="space-y-3 text-sm">
                <input type="text" id="mem-name" class="input-field w-full rounded p-2" placeholder="Nome Completo">
                <input type="text" id="mem-cpf" class="input-field w-full rounded p-2" placeholder="CPF">
                <select id="mem-unit" class="input-field w-full rounded p-2"><option value="">Selecione a Unidade...</option></select>
                <select id="mem-cat" onchange="window.toggleMemCref()" class="input-field w-full rounded p-2">
                    <option value="PEF">Profissional de Educação Física (PEF)</option>
                    <option value="TE">Treinador Esportivo (TE)</option>
                </select>
                <div class="grid grid-cols-2 gap-2" id="mem-cref-group">
                    <input type="text" id="mem-cref" class="input-field rounded p-2" placeholder="CREF">
                    <input type="text" id="mem-state" class="input-field rounded p-2" placeholder="Estado (UF)">
                </div>
            </div>
            <div class="flex justify-end gap-2 mt-5">
                <button onclick="window.closeModal('member-modal')" class="px-4 py-2 rounded text-sm opacity-70">Cancelar</button>
                <button onclick="window.saveMember()" class="btn-primary px-4 py-2 rounded text-sm">Salvar</button>
            </div>
        </div>
    </div>

    <!-- MODAL DE EXERCÍCIOS -->
    <div id="exercise-modal" class="fixed inset-0 bg-black bg-opacity-80 backdrop-blur-sm hidden z-50 flex items-center justify-center p-2 sm:p-4 modals">
        <div class="card w-full max-w-5xl rounded-xl shadow-2xl flex flex-col h-[90vh]">
            <div class="p-4 border-b flex justify-between items-center bg-black bg-opacity-20" style="border-color: var(--border-color)">
                <h3 class="font-bold"><i class="fas fa-search text-primary mr-2"></i> Banco de Exercícios</h3>
                <button onclick="window.closeModal('exercise-modal')" class="text-gray-400 hover:text-white text-2xl leading-none">&times;</button>
            </div>
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <div class="w-full md:w-1/4 border-r overflow-y-auto p-2 space-y-1 bg-black bg-opacity-10" style="border-color: var(--border-color)" id="modal-categories"></div>
                <div class="w-full md:w-3/4 flex flex-col">
                    <div class="p-2 border-b border-gray-600 flex gap-2 bg-black bg-opacity-30">
                        <input type="text" id="new-custom-ex" class="input-field flex-1 rounded px-2 py-1 text-xs" placeholder="Criar exercício customizado nesta categoria...">
                        <button onclick="window.saveCustomExercise()" class="btn-primary px-3 py-1 rounded text-xs">Adicionar ao BD</button>
                    </div>
                    <div class="flex-1 overflow-y-auto p-3" id="modal-exercises"></div>
                </div>
            </div>
        </div>
    </div>

    <!-- ÁREA DE IMPRESSÃO -->
    <div id="print-area"></div>

    <!-- FIREBASE E LÓGICA (MODULAR) -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-app.js";
        import { getAuth, signInWithPopup, GoogleAuthProvider, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-auth.js";
        import { getFirestore, collection, doc, setDoc, getDoc, addDoc, query, where, getDocs, deleteDoc, orderBy } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-firestore.js";

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

        // --- ESTADO GLOBAL ---
        const state = {
            user: null,
            units: [],
            members: [],
            workoutsHistory: [],
            customExercises: [], // {category, name}
            
            // Estado do Criador de Ficha
            currentWorkoutId: null, // Se estiver editando
            creatorUnitId: "",
            creatorMemberId: "",
            days: [], // { id, title, exercises: [] }
            activeModalDayId: null,
            activeCategory: "🔥 PEITO"
        };

        // --- DADOS FIXOS ---
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
            "🟢 Saudável": "Manter treinos regulares de 3–5x por semana. Constância, descanso e alimentação contribuem para resultados.",
            "⚪ Sedentário": "Início gradual, com treinos leves e foco na adaptação do corpo. Aprender a execução correta.",
            "🟡 Sobrepeso": "Musculação + cardio para melhora do condicionamento. Progressão respeitando a individualidade.",
            "🔴 Obesidade": "Iniciar com menor impacto. Segurança, mobilidade e constância são prioridades.",
            "⚖️ Baixo peso": "Musculação auxilia no ganho de massa com alimentação e descanso. Evolução progressiva.",
            "🍬 Diabetes": "Manter acompanhamento médico. Em casos de tontura, mal-estar ou alteração de glicemia, interromper o treino.",
            "❤️ Hipertensão": "Acompanhamento médico. Evitar prender a respiração (Valsalva) e controlar a intensidade.",
            "🔵 Hipotensão": "Evitar mudanças bruscas de posição. Manter hidratação e respeitar a intensidade.",
            "💔 Problemas cardíacos": "Prática apenas com liberação médica. Respeitar limites e controle de intensidade.",
            "🦴 Problemas articulares": "Menor impacto e maior controle de movimento são indicados. Adaptação ajuda na segurança.",
            "🫁 Problemas respiratórios": "Progressão gradual. Interromper em caso de falta de ar excessiva.",
            "⚠️ Lesões": "Adaptação deve respeitar a limitação e evitar dor. Seguir orientação profissional.",
            "🤰 Gestante": "Prática com liberação médica. Foco na segurança, mobilidade e bem-estar.",
            "🤱 Lactante": "Atividade normal, respeitando a recuperação, hidratação e alimentação.",
            "👴 Idoso": "Contribui para força e autonomia. Respeitar limitações com intensidade moderada."
        };

        const objText = {
            "Emagrecimento": "Foco em déficit calórico com treinos mistos de força (manter massa magra) e aeróbicos (maior gasto).",
            "Hipertrofia": "Prioridade na progressão de carga e volume adequado. Essencial superávit calórico e descanso.",
            "Definição": "Manutenção de massa muscular enquanto reduz o percentual de gordura. Atenção estrita à dieta.",
            "Condicionamento": "Treinos com menor tempo de intervalo, circuitos e alta integração cardiopulmonar.",
            "Resistência": "Séries mais longas, cadência controlada e aprimoramento da capacidade muscular.",
            "Força": "Cargas altas, baixas repetições e intervalos de descanso maiores.",
            "Reabilitação": "Treino focado em fortalecimento específico, mobilidade e controle motor. Respeitar limites.",
            "Saúde geral": "Equilíbrio entre força, cardio e flexibilidade. O principal objetivo é a constância e bem-estar."
        };

        const baseDbCategories = {
            "🔥 PEITO": ["Supino Reto", "Supino Inclinado", "Supino Inclinado com Halteres", "Supino Fechado com Halteres", "Cross Over", "Cross Over Alto", "Cross Over Baixo", "Crucifixo Reto", "Crucifixo Inclinado com Halteres", "Crucifixo na Máquina", "Peck Fly", "Peck Fly Unilateral", "Pullover", "Flexão de Braço", "Flexão com Pés Elevados", "Flexão Explosiva"],
            "🦍 COSTAS": ["Puxada Alta", "Puxada de Frente Supinada", "Pulldown", "Remada Aberta", "Remada Baixa", "Remada Curvada", "Remada Curvada Supinada", "Remada Cavalinho (T-Bar)", "Serrote", "Facepull (puxada de cima para baixo)"],
            "🦵 PERNAS": ["Agachamento Livre", "Agachamento Taça", "Agachamento no Smith", "Agachamento com passada lateral", "Squat", "Hack Machine", "Leg 45°", "Leg 90°", "Agachamento Sumô", "Agachamento Sissy (Livre)", "Afundo", "Recuo", "Avanço", "Passada", "Búlgaro", "Step-up", "Levantamento Terra", "Levantamento Terra Romeno", "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", "Cadeira Flexora", "Elevação Pélvica no Banco", "Elevação Pélvica no Chão", "Elevação Pélvica Unilateral no Chão", "Extensão de Quadril (Glúteo Máximo)", "Extensão Cruzada (Glúteo Médio)", "Coice", "Cachorrinho", "Cadeira Extensora", "Adução", "Abdução", "Abdução Inclinada", "Flexão Nórdica", "Flexão Nórdica Invertida", "Panturrilha Livre", "Panturrilha no Leg Press", "Panturrilha Banco", "Panturrilha Squat", "Panturrilha Unilateral"],
            "💪 BRAÇOS (Bíceps)": ["Rosca Direta", "Rosca Alternada", "Rosca 21", "Rosca Scott Barra W", "Rosca Scott Unilateral", "Rosca Scott com Halteres", "Rosca Martelo", "Rosca Cross", "Rosca Inversa", "Rosca 45°"],
            "💪 BRAÇOS (Tríceps)": ["Triceps Pulley Unilateral", "Tríceps Pulley Barra", "Tríceps Pulley Corda", "Tríceps Pulley Pegada Inversa", "Tríceps Francês na Corda", "Tríceps Francês com Halter", "Tríceps Francês Unilateral", "Tríceps Cruzado Polia Dupla", "Tríceps Coice Unilateral", "Tríceps Arremesso", "Tríceps Testa", "Mergulho no Banco"],
            "🪨 OMBROS": ["Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", "Elevação Lateral Unilateral Cross", "Elevação Lateral Sentado", "Desenvolvimento com Halteres", "Desenvolvimento com Barra", "Arnold Press", "Elevação Borboleta", "Crucifixo Inverso Sentado com Halteres", "Crucifixo Inverso na Polia", "Crucifixo Inverso Unilateral na Polia", "Facepull (puxada reta)", "Remada Alta", "Encolhimento (Trapézio)"],
            "🧠 ABDÔMEN": ["Infra com Elevação de Perna", "Abdominal Supra", "Abdominal Remador", "Abdominal Bicicleta", "Abdominal Twister com Peso", "Prancha", "Prancha Lateral", "Trituração de Cabos em Pé", "Isometria na parede"],
            "🫀 CARDIO": ["Bicicleta 10 Minutos", "Bicicleta 15 Minutos", "Bicicleta 20 Minutos", "Esteira 10 Minutos", "Esteira 15 Minutos", "Esteira 20 Minutos", "Pular Corda"]
        };
        const techniques = ["Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", "Negativa", "Isometria", "Parciais", "Pirâmide"];
        const dayNames = ["SEGUNDA-FEIRA", "TERÇA-FEIRA", "QUARTA-FEIRA", "QUINTA-FEIRA", "SEXTA-FEIRA", "SÁBADO", "DOMINGO"];

        // --- AUTH ---
        window.loginGoogle = async () => {
            const provider = new GoogleAuthProvider();
            try { await signInWithPopup(auth, provider); } 
            catch (e) { alert("Erro ao fazer login: " + e.message); }
        };
        window.logout = () => signOut(auth);

        onAuthStateChanged(auth, async (user) => {
            if (user) {
                state.user = user;
                document.getElementById('login-view').classList.add('hidden');
                document.getElementById('app-view').classList.remove('hidden');
                document.getElementById('user-avatar').src = user.photoURL || 'https://via.placeholder.com/32';
                
                // Init Defaults
                const today = new Date();
                const monthStr = `${today.getFullYear()}-${String(today.getMonth()+1).padStart(2,'0')}`;
                document.getElementById('report-month').value = monthStr;
                
                window.switchTab('dashboard');
                await loadAllData();
            } else {
                state.user = null;
                document.getElementById('login-view').classList.remove('hidden');
                document.getElementById('app-view').classList.add('hidden');
            }
        });

        // --- TABS ---
        window.switchTab = (tab) => {
            if (tab === 'dashboard') {
                document.getElementById('dashboard-section').classList.remove('hidden');
                document.getElementById('creator-section').classList.add('hidden');
                document.getElementById('btn-tab-dashboard').classList.add('btn-primary');
                document.getElementById('btn-tab-dashboard').classList.remove('border', 'border-transparent', 'hover:bg-black', 'hover:bg-opacity-20');
                document.getElementById('btn-tab-creator').classList.remove('btn-primary');
                document.getElementById('btn-tab-creator').classList.add('border', 'border-transparent', 'hover:bg-black', 'hover:bg-opacity-20');
            } else {
                document.getElementById('dashboard-section').classList.add('hidden');
                document.getElementById('creator-section').classList.remove('hidden');
                document.getElementById('btn-tab-creator').classList.add('btn-primary');
                document.getElementById('btn-tab-creator').classList.remove('border', 'border-transparent', 'hover:bg-black', 'hover:bg-opacity-20');
                document.getElementById('btn-tab-dashboard').classList.remove('btn-primary');
                document.getElementById('btn-tab-dashboard').classList.add('border', 'border-transparent', 'hover:bg-black', 'hover:bg-opacity-20');
                if(state.days.length === 0) window.addDay();
            }
        };

        // --- FIREBASE LOADERS ---
        async function loadAllData() {
            await Promise.all([loadUnits(), loadMembers(), loadCustomExercises(), loadWorkouts()]);
        }

        async function loadUnits() {
            const q = query(collection(db, `networks/${state.user.uid}/units`), orderBy("name"));
            const snap = await getDocs(q);
            state.units = snap.docs.map(d => ({id: d.id, ...d.data()}));
            
            // Popula Lists e Selects
            const list = document.getElementById('units-list');
            list.innerHTML = state.units.map(u => `<div class="p-2 border-b border-gray-600 flex justify-between text-sm"><span>${u.name}</span> <button onclick="window.deleteUnit('${u.id}')" class="text-red-500"><i class="fas fa-trash"></i></button></div>`).join('');
            
            const selects = ['mem-unit', 'ctx-unit'];
            selects.forEach(id => {
                const el = document.getElementById(id);
                el.innerHTML = '<option value="">Selecione...</option>' + state.units.map(u => `<option value="${u.id}">${u.name}</option>`).join('');
            });
        }

        async function loadMembers() {
            const q = query(collection(db, `networks/${state.user.uid}/members`), orderBy("name"));
            const snap = await getDocs(q);
            state.members = snap.docs.map(d => ({id: d.id, ...d.data()}));
            
            const list = document.getElementById('members-list');
            list.innerHTML = state.members.map(m => {
                const u = state.units.find(x => x.id === m.unitId);
                return `<div class="p-2 border-b border-gray-600 flex justify-between text-xs">
                    <div><b>${m.name}</b> (${m.cat})<br><span class="opacity-60">${u ? u.name : 'Sem unidade'}</span></div>
                    <button onclick="window.deleteMember('${m.id}')" class="text-red-500"><i class="fas fa-trash"></i></button>
                </div>`
            }).join('');
        }

        async function loadCustomExercises() {
            const snap = await getDocs(collection(db, `networks/${state.user.uid}/customExercises`));
            state.customExercises = snap.docs.map(d => d.data());
        }

        window.loadWorkouts = async () => {
            const monthStr = document.getElementById('report-month').value; // YYYY-MM
            if(!monthStr) return;
            
            // Simples filtro por prefixo da data ISO armazenada (YYYY-MM)
            const snap = await getDocs(collection(db, `networks/${state.user.uid}/workouts`));
            const allW = snap.docs.map(d => ({id: d.id, ...d.data()}));
            state.workoutsHistory = allW.filter(w => w.createdAt && w.createdAt.startsWith(monthStr));
            
            renderReport();
        };

        // --- GESTÃO CRUD ---
        window.openModal = id => document.getElementById(id).classList.remove('hidden');
        window.closeModal = id => document.getElementById(id).classList.add('hidden');
        
        window.saveUnit = async () => {
            const name = document.getElementById('unit-name').value.trim();
            if(!name) return;
            await addDoc(collection(db, `networks/${state.user.uid}/units`), { name });
            document.getElementById('unit-name').value = '';
            window.closeModal('unit-modal');
            loadUnits();
        };
        window.deleteUnit = async (id) => {
            if(confirm("Excluir unidade?")) { await deleteDoc(doc(db, `networks/${state.user.uid}/units/${id}`)); loadUnits(); }
        }

        window.toggleMemCref = () => {
            const cat = document.getElementById('mem-cat').value;
            document.getElementById('mem-cref-group').style.display = cat === 'PEF' ? 'grid' : 'none';
        };

        window.saveMember = async () => {
            const name = document.getElementById('mem-name').value.trim();
            const cpf = document.getElementById('mem-cpf').value.trim();
            const unitId = document.getElementById('mem-unit').value;
            const cat = document.getElementById('mem-cat').value;
            const cref = document.getElementById('mem-cref').value;
            const stateUf = document.getElementById('mem-state').value;
            
            if(!name || !unitId) { alert("Nome e Unidade são obrigatórios"); return; }
            
            await addDoc(collection(db, `networks/${state.user.uid}/members`), { name, cpf, unitId, cat, cref, stateUf });
            window.closeModal('member-modal');
            loadMembers();
        };
        window.deleteMember = async (id) => {
            if(confirm("Excluir membro?")) { await deleteDoc(doc(db, `networks/${state.user.uid}/members/${id}`)); loadMembers(); }
        }

        // --- DASHBOARD REPORT ---
        function renderReport() {
            const summaryDiv = document.getElementById('report-summary');
            const listDiv = document.getElementById('workouts-history-list');
            
            let htmlSummary = `<div class="flex justify-between py-1 font-bold border-b border-gray-600"><span>Total Geral:</span> <span>${state.workoutsHistory.length} fichas</span></div>`;
            
            // Agrupar por unidade -> membro
            const counts = {};
            state.workoutsHistory.forEach(w => {
                if(!counts[w.unitId]) counts[w.unitId] = { total: 0, members: {} };
                counts[w.unitId].total++;
                if(!counts[w.unitId].members[w.memberId]) counts[w.unitId].members[w.memberId] = 0;
                counts[w.unitId].members[w.memberId]++;
            });

            for(const uId in counts) {
                const uName = state.units.find(u => u.id === uId)?.name || 'Desconhecida';
                htmlSummary += `<div class="mt-2 font-bold text-primary">${uName} (Total: ${counts[uId].total})</div>`;
                for(const mId in counts[uId].members) {
                    const mName = state.members.find(m => m.id === mId)?.name || 'Desconhecido';
                    htmlSummary += `<div class="flex justify-between pl-2 text-xs"><span>- ${mName}</span> <span>${counts[uId].members[mId]}</span></div>`;
                }
            }
            summaryDiv.innerHTML = htmlSummary || '<p class="opacity-50">Sem dados no mês.</p>';

            // Lista Histórico
            // Ordenar decrescente
            const sortedHistory = [...state.workoutsHistory].sort((a,b) => b.createdAt.localeCompare(a.createdAt));
            
            listDiv.innerHTML = sortedHistory.map(w => {
                const mName = state.members.find(m => m.id === w.memberId)?.name || '?';
                
                // Verificar Validade
                let isExpired = false;
                if(w.createdAt) {
                    const createdDate = new Date(w.createdAt);
                    const daysValid = parseInt(w.student.validity.replace(/\D/g, '')) || 30;
                    const expireDate = new Date(createdDate.getTime() + (daysValid * 24 * 60 * 60 * 1000));
                    isExpired = new Date() > expireDate;
                }

                return `
                <div class="p-3 bg-black bg-opacity-20 rounded border ${isExpired ? 'border-red-500 border-opacity-50' : 'border-gray-600'}">
                    <div class="flex justify-between items-start">
                        <div>
                            <span class="font-bold text-sm">${w.student.name || 'Sem nome'}</span>
                            ${isExpired ? '<span class="ml-2 text-[10px] bg-red-600 px-1.5 py-0.5 rounded text-white">VENCIDA</span>' : ''}
                            <div class="text-[10px] opacity-70">Criado em: ${w.createdAt ? w.createdAt.substring(0,10).split('-').reverse().join('/') : '-'} | Por: ${mName}</div>
                        </div>
                        <div class="flex gap-2">
                            <button onclick="window.loadWorkoutToCreator('${w.id}')" class="text-xs bg-primary px-2 py-1 rounded">Carregar</button>
                            <button onclick="window.deleteWorkout('${w.id}')" class="text-xs text-red-500"><i class="fas fa-trash"></i></button>
                        </div>
                    </div>
                </div>`;
            }).join('');
        }

        window.deleteWorkout = async (id) => {
            if(confirm("Apagar ficha do histórico?")) { await deleteDoc(doc(db, `networks/${state.user.uid}/workouts/${id}`)); loadWorkouts(); }
        }

        // --- PRANCHETA LÓGICA ---
        window.updateCtxMembers = () => {
            const uid = document.getElementById('ctx-unit').value;
            const sel = document.getElementById('ctx-member');
            sel.innerHTML = '<option value="">Selecione o Profissional...</option>';
            if(!uid) return;
            const filtered = state.members.filter(m => m.unitId === uid);
            sel.innerHTML += filtered.map(m => `<option value="${m.id}">${m.name}</option>`).join('');
            window.applyMemberContext();
        }

        window.applyMemberContext = () => {
            const mid = document.getElementById('ctx-member').value;
            const area = document.getElementById('assembly-area');
            const warn = document.getElementById('ctx-warning');
            
            if(!mid) {
                area.classList.add('opacity-50', 'pointer-events-none');
                warn.classList.remove('hidden');
                state.creatorMemberId = null;
                return;
            }
            
            area.classList.remove('opacity-50', 'pointer-events-none');
            warn.classList.add('hidden');
            state.creatorMemberId = mid;
            state.creatorUnitId = document.getElementById('ctx-unit').value;

            // Renderizar Saúde baseada no tipo do profissional
            const member = state.members.find(m => m.id === mid);
            const healthDict = member && member.cat === 'TE' ? healthTE : healthPEF;
            
            // Manter checks se houver
            const checked = Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value);
            const container = document.getElementById('health-container');
            container.innerHTML = Object.keys(healthDict).map(k => `
                <label class="flex items-start space-x-1 cursor-pointer p-1 rounded hover:bg-white hover:bg-opacity-5 border border-transparent">
                    <input type="checkbox" value="${k}" ${checked.includes(k)?'checked':''} class="health-cb mt-0.5 rounded text-primary focus:ring-primary w-3 h-3">
                    <span class="leading-tight">${k}</span>
                </label>
            `).join('');
        }

        window.calcIMC = () => {
            const w = parseFloat(document.getElementById('stu-weight').value);
            const h = parseFloat(document.getElementById('stu-height').value);
            const disp = document.getElementById('imc-display');
            if(w>0 && h>0) {
                const imc = (w/(h*h)).toFixed(1);
                let cls = imc<18.5?"Abaixo":imc<24.9?"Normal":imc<29.9?"Sobrepeso":"Obesidade";
                disp.innerHTML = `<span class="bg-primary bg-opacity-20 text-primary px-2 py-0.5 rounded text-xs border border-primary border-opacity-30">IMC: ${imc} (${cls})</span>`;
            } else disp.innerHTML = '';
        };

        window.changeTheme = () => document.body.setAttribute('data-theme', document.getElementById('stu-gender').value);
        
        window.updateObjText = () => {
            const val = document.getElementById('stu-objective').value;
            const hint = document.getElementById('obj-hint');
            if(objText[val]) { hint.innerText = objText[val]; hint.classList.remove('hidden'); }
            else hint.classList.add('hidden');
        };

        // --- DAYS E EXERCISES ---
        window.addDay = () => {
            const idx = state.days.length;
            state.days.push({
                id: Math.random().toString(36).substr(2,9),
                title: idx < 7 ? `TREINO ${dayNames[idx]}` : `NOVO TREINO ${idx+1}`,
                exercises: []
            });
            renderDays();
        };

        window.removeDay = (id) => {
            if(confirm("Remover este dia?")) { state.days = state.days.filter(d => d.id !== id); renderDays(); }
        }

        window.updateDayTitle = (id, val) => {
            const d = state.days.find(x => x.id === id);
            if(d) d.title = val;
        }

        window.moveEx = (dayId, exIdx, dir) => {
            const d = state.days.find(x => x.id === dayId);
            if(dir==='up' && exIdx>0) { const tmp = d.exercises[exIdx]; d.exercises[exIdx] = d.exercises[exIdx-1]; d.exercises[exIdx-1] = tmp; }
            if(dir==='down' && exIdx<d.exercises.length-1) { const tmp = d.exercises[exIdx]; d.exercises[exIdx] = d.exercises[exIdx+1]; d.exercises[exIdx+1] = tmp; }
            renderDays();
        }
        window.rmEx = (dayId, exIdx) => { const d = state.days.find(x => x.id === dayId); d.exercises.splice(exIdx,1); renderDays(); }
        window.updEx = (dayId, exIdx, field, val) => { const d = state.days.find(x => x.id === dayId); d.exercises[exIdx][field] = val; }

        function renderDays() {
            const c = document.getElementById('days-container');
            c.innerHTML = state.days.map(d => {
                let exs = d.exercises.length ? d.exercises.map((ex, i) => `
                    <div class="flex gap-2 items-center border-b border-gray-600 p-1">
                        <div class="flex flex-col gap-1 hidden sm:flex">
                            <button onclick="window.moveEx('${d.id}',${i},'up')" class="text-[8px] opacity-50 hover:opacity-100"><i class="fas fa-chevron-up"></i></button>
                            <button onclick="window.moveEx('${d.id}',${i},'down')" class="text-[8px] opacity-50 hover:opacity-100"><i class="fas fa-chevron-down"></i></button>
                        </div>
                        <div class="w-1/3 min-w-[120px]"><div class="text-[8px] opacity-50 uppercase">${ex.category.split(' ')[1] || ex.category}</div><div class="text-xs font-bold leading-tight">${ex.name}</div></div>
                        <input value="${ex.sets}" onchange="window.updEx('${d.id}',${i},'sets',this.value)" class="input-field w-10 text-center text-xs p-1 rounded" placeholder="S">
                        <span class="text-xs opacity-50">x</span>
                        <input value="${ex.reps}" onchange="window.updEx('${d.id}',${i},'reps',this.value)" class="input-field w-12 text-center text-xs p-1 rounded" placeholder="R">
                        <select onchange="window.updEx('${d.id}',${i},'technique',this.value)" class="input-field w-20 text-[10px] p-1 rounded">${techniques.map(t=>`<option ${ex.technique===t?'selected':''}>${t}</option>`).join('')}</select>
                        <input value="${ex.obs}" onchange="window.updEx('${d.id}',${i},'obs',this.value)" class="input-field flex-1 min-w-[80px] text-xs p-1 rounded" placeholder="Obs">
                        <button onclick="window.rmEx('${d.id}',${i})" class="text-red-500 text-xs ml-1"><i class="fas fa-times"></i></button>
                    </div>
                `).join('') : '<div class="text-xs opacity-50 p-2 text-center">Nenhum exercício</div>';

                return `
                <div class="card rounded-lg overflow-hidden border border-gray-600">
                    <div class="bg-black bg-opacity-20 p-2 flex justify-between items-center border-b border-gray-600">
                        <input value="${d.title}" onchange="window.updateDayTitle('${d.id}',this.value)" class="input-field bg-transparent font-bold text-sm w-1/2 uppercase px-1">
                        <button onclick="window.removeDay('${d.id}')" class="text-red-500 text-xs px-2"><i class="fas fa-trash"></i></button>
                    </div>
                    <div>${exs}</div>
                    <button onclick="window.openExModal('${d.id}')" class="w-full text-primary text-xs py-2 bg-black bg-opacity-10 hover:bg-opacity-20 font-bold">+ ADICIONAR EXERCÍCIO</button>
                </div>
                `;
            }).join('');
        }

        // --- MODAL DE EXERCÍCIOS ---
        function getFullExercisesDict() {
            const dict = JSON.parse(JSON.stringify(baseDbCategories));
            state.customExercises.forEach(cx => {
                if(!dict[cx.category]) dict[cx.category] = [];
                if(!dict[cx.category].includes(cx.name)) dict[cx.category].push(cx.name);
            });
            // Sort arrays
            for(let k in dict) dict[k].sort();
            return dict;
        }

        window.openExModal = (dayId) => {
            state.activeModalDayId = dayId;
            window.openModal('exercise-modal');
            window.renderExCats();
            window.renderExList();
        }

        window.renderExCats = () => {
            const dict = getFullExercisesDict();
            const c = document.getElementById('modal-categories');
            c.innerHTML = Object.keys(dict).map(cat => `
                <button onclick="state.activeCategory='${cat}'; window.renderExCats(); window.renderExList()" class="w-full text-left px-2 py-1.5 rounded text-xs font-bold ${state.activeCategory===cat?'bg-primary text-white':'hover:bg-white hover:bg-opacity-5'}">${cat}</button>
            `).join('');
        }

        window.renderExList = () => {
            const dict = getFullExercisesDict();
            const c = document.getElementById('modal-exercises');
            const exs = dict[state.activeCategory] || [];
            c.innerHTML = `<div class="grid grid-cols-2 md:grid-cols-3 gap-2">` + exs.map(ex => `
                <button onclick="window.addExToDay('${ex}')" class="card p-2 text-left text-xs border hover:border-primary transition flex justify-between group">
                    <span>${ex}</span><i class="fas fa-plus opacity-0 group-hover:opacity-100 text-primary"></i>
                </button>
            `).join('') + `</div>`;
        }

        window.addExToDay = (exName) => {
            const d = state.days.find(x => x.id === state.activeModalDayId);
            if(d) {
                const isCardio = state.activeCategory.includes("CARDIO");
                d.exercises.push({ category: state.activeCategory, name: exName, sets: isCardio?'1':'3', reps: isCardio?'-':'10-12', technique:'Nenhuma', obs:''});
                renderDays();
                const container = document.getElementById('modal-exercises');
                container.style.opacity = '0.5'; setTimeout(()=>container.style.opacity='1', 100);
            }
        }

        window.saveCustomExercise = async () => {
            const input = document.getElementById('new-custom-ex');
            const name = input.value.trim();
            if(!name) return;
            
            const newEx = { category: state.activeCategory, name: name };
            await addDoc(collection(db, `networks/${state.user.uid}/customExercises`), newEx);
            state.customExercises.push(newEx);
            input.value = '';
            window.renderExCats();
            window.renderExList();
        }

        // --- SALVAR E IMPRIMIR ---
        function getWorkoutData() {
            return {
                memberId: state.creatorMemberId,
                unitId: state.creatorUnitId,
                createdAt: new Date().toISOString(),
                student: {
                    name: document.getElementById('stu-name').value,
                    age: document.getElementById('stu-age').value,
                    weight: document.getElementById('stu-weight').value,
                    height: document.getElementById('stu-height').value,
                    gender: document.getElementById('stu-gender').value,
                    level: document.getElementById('stu-level').value,
                    freq: document.getElementById('stu-freq').value,
                    objective: document.getElementById('stu-objective').value,
                    validity: document.getElementById('stu-validity').value,
                    recs: document.getElementById('stu-recs').value,
                    health: Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value)
                },
                days: state.days
            };
        }

        window.saveAndPrint = async () => {
            if(!state.creatorMemberId) { alert("Selecione o Autor da Ficha antes de salvar/imprimir."); return; }
            
            const data = getWorkoutData();
            
            // Save to Firebase
            if(state.currentWorkoutId) {
                await setDoc(doc(db, `networks/${state.user.uid}/workouts/${state.currentWorkoutId}`), data);
            } else {
                const res = await addDoc(collection(db, `networks/${state.user.uid}/workouts`), data);
                state.currentWorkoutId = res.id;
            }
            
            // Generate HTML for Print
            const m = state.members.find(x => x.id === state.creatorMemberId);
            const u = state.units.find(x => x.id === state.creatorUnitId);
            
            const isPEF = m.cat === 'PEF';
            const profLabel = isPEF ? 'Profissional de Educação Física' : 'Treinador Esportivo';
            const crefText = isPEF ? `CREF: ${m.cref || '-'} - Estado: ${m.stateUf || '-'}` : '';
            
            let legalTextPEF = `⚠️ OBSERVAÇÃO LEGAL – PROFISSIONAL DE EDUCAÇÃO FÍSICA: Conforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º estabelece que compete ao profissional coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos especializados.`;
            let legalTextTE = `⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO: Conforme a Lei nº 14.597/2023 (Lei Geral do Esporte), Art. 75, a profissão de treinador esportivo é reconhecida no Brasil. As recomendações desta ficha possuem caráter orientativo. Este material não substitui avaliação médica. Em casos de doenças ou limitações, recomenda-se procurar profissional habilitado. O treinamento respeita os limites individuais, priorizando segurança dentro da atuação técnica e esportiva permitida por lei.`;

            let imcStr = "-";
            const weight = parseFloat(data.student.weight);
            const height = parseFloat(data.student.height);
            if(weight>0 && height>0) imcStr = (weight/(height*height)).toFixed(1);

            let html = `
                <div class="print-header">
                    <h1 class="print-title">Planilha de Treinamento - ${data.student.objective}</h1>
                    <div class="prof-info">Prescrição feita por: ${m.name} (${profLabel})<br>${crefText}</div>
                </div>

                <div class="print-grid">
                    <div class="full-row"><strong>Aluno:</strong> ${data.student.name || '-'}</div>
                    <div><strong>Idade:</strong> ${data.student.age || '-'}</div>
                    <div><strong>Peso:</strong> ${data.student.weight || '-'}kg</div>
                    <div><strong>Altura:</strong> ${data.student.height || '-'}m</div>
                    <div><strong>IMC:</strong> ${imcStr}</div>
                    
                    <div><strong>Gênero:</strong> ${data.student.gender}</div>
                    <div><strong>Nível:</strong> ${data.student.level}</div>
                    <div><strong>Freq:</strong> ${data.student.freq}</div>
                    <div><strong>Validade:</strong> ${data.student.validity.replace('Validade: ','')}</div>
                </div>
            `;

            const healthDict = isPEF ? healthPEF : healthTE;
            const objRec = objText[data.student.objective] || "";
            if(objRec || data.student.health.length > 0) {
                html += `<div class="print-guidelines"><h4>Diretrizes de Perfil</h4><ul>`;
                if(objRec) html += `<li><strong>Objetivo:</strong> ${objRec}</li>`;
                data.student.health.forEach(k => {
                    if(healthDict[k]) html += `<li><strong>${k.replace(/[\u{1F300}-\u{1F9FF}\u{2600}-\u{26FF}\u{2700}-\u{27BF}\u{FE0F}]/gu, '').trim()}:</strong> ${healthDict[k]}</li>`;
                });
                html += `</ul></div>`;
            }

            data.days.forEach(d => {
                html += `<div class="print-workout"><h3>${d.title}</h3><table><thead><tr>
                    <th style="width:35%">Exercício</th><th style="width:8%;text-align:center">Séries</th><th style="width:10%;text-align:center">Reps</th><th style="width:15%">Técnica</th><th style="width:32%">Obs</th>
                </tr></thead><tbody>`;
                if(d.exercises.length === 0) html += `<tr><td colspan="5" style="text-align:center;font-style:italic">Sem exercícios</td></tr>`;
                else d.exercises.forEach(ex => {
                    html += `<tr><td><strong>${ex.name}</strong></td><td style="text-align:center">${ex.sets}</td><td style="text-align:center">${ex.reps}</td><td>${ex.technique==='Nenhuma'?'-':ex.technique}</td><td>${ex.obs||'-'}</td></tr>`;
                });
                html += `</tbody></table></div>`;
            });

            html += `<div class="print-footer-section">`;
            if(data.student.recs) html += `<strong>Recomendações Manuais:</strong><br><p style="white-space:pre-wrap;margin:2px 0 6px 0">${data.student.recs}</p>`;
            html += `<div class="legal-text">${isPEF ? legalTextPEF : legalTextTE}</div>
                <div style="text-align:center; margin-top:8px; font-size:7px;">Ficha gerada em ${new Date().toLocaleDateString('pt-BR')} - PowFit Pro - Unidade: ${u?.name||'-'}</div>
            </div>`;

            document.getElementById('print-area').innerHTML = html;
            
            // Reload history to reflect the saved item, then print
            await window.loadWorkouts(); 
            setTimeout(() => window.print(), 300);
        }

        // --- LOAD TO CREATOR ---
        window.loadWorkoutToCreator = (id) => {
            const w = state.workoutsHistory.find(x => x.id === id);
            if(!w) return;
            
            state.currentWorkoutId = w.id;
            
            // Seletores de Contexto
            document.getElementById('ctx-unit').value = w.unitId || '';
            window.updateCtxMembers();
            document.getElementById('ctx-member').value = w.memberId || '';
            window.applyMemberContext();

            // Form Aluno
            document.getElementById('stu-name').value = w.student.name || '';
            document.getElementById('stu-age').value = w.student.age || '';
            document.getElementById('stu-weight').value = w.student.weight || '';
            document.getElementById('stu-height').value = w.student.height || '';
            document.getElementById('stu-gender').value = w.student.gender || 'Masculino';
            document.getElementById('stu-level').value = w.student.level || 'Iniciante';
            document.getElementById('stu-objective').value = w.student.objective || 'Emagrecimento';
            document.getElementById('stu-freq').value = w.student.freq || 'Frequência: 3 dias';
            document.getElementById('stu-validity').value = w.student.validity || 'Validade: 30 dias';
            document.getElementById('stu-recs').value = w.student.recs || '';

            window.changeTheme();
            window.calcIMC();
            window.updateObjText();

            // Checks Saúde
            document.querySelectorAll('.health-cb').forEach(cb => {
                cb.checked = (w.student.health || []).includes(cb.value);
            });

            state.days = w.days || [];
            renderDays();
            
            window.switchTab('creator');
        }

        // Relatório solto
        window.printReport = () => {
            const html = `
                <div style="font-family: Arial; padding: 20px;">
                    <h1 style="text-align:center; font-size: 18px; border-bottom: 2px solid #000; padding-bottom: 10px;">Relatório de Produtividade - ${document.getElementById('report-month').value}</h1>
                    ${document.getElementById('report-summary').innerHTML}
                </div>
            `;
            const printWindow = window.open('', '', 'height=600,width=800');
            printWindow.document.write('<html><head><title>Relatório</title></head><body>');
            printWindow.document.write(html);
            printWindow.document.write('</body></html>');
            printWindow.document.close();
            setTimeout(()=>printWindow.print(), 300);
        }
    </script>
</body>
</html>
