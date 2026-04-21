<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro - Gestão de Franquias & Prescrição</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg-body: #0f172a;
            --bg-card: #1e293b;
            --text-main: #f8fafc;
            --text-muted: #94a3b8;
            --border-color: #334155;
            --primary: #3b82f6;
            --primary-hover: #2563eb;
            --input-bg: #0f172a;
            --input-border: #475569;
            --danger: #ef4444;
        }

        [data-theme="Feminino"] {
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
        .btn-primary { background-color: var(--primary); color: white; transition: 0.2s; }
        .btn-primary:hover { background-color: var(--primary-hover); }
        
        .view { display: none; }
        .view.active { display: block; }

        /* Loader */
        .loader { border: 3px solid rgba(255,255,255,0.1); border-top: 3px solid var(--primary); border-radius: 50%; width: 24px; height: 24px; animation: spin 1s linear infinite; }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }

        /* Impressão Planilha A4 */
        #print-area, #print-report-area { display: none; }
        @media print {
            body { background: white !important; color: black !important; margin: 0; padding: 0; }
            .no-print, #app-views, .modal { display: none !important; }
            
            #print-area, #print-report-area { display: block !important; padding: 10mm 15mm; font-family: 'Arial', sans-serif; font-size: 10px; }
            @page { size: A4; margin: 0; }
            
            .print-header { border-bottom: 2px solid #000; padding-bottom: 8px; margin-bottom: 12px; }
            .print-title { font-size: 16px; font-weight: bold; margin: 0; text-transform: uppercase; text-align: center;}
            .prof-info { font-size: 11px; margin-top: 5px; text-align: center; font-weight: bold;}
            
            .print-grid { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 4px; margin-bottom: 15px; border: 1px solid #000; padding: 8px; background: #fff; }
            .print-grid div { font-size: 10px; }
            
            .print-guidelines { border: 1px solid #000; padding: 8px; margin-bottom: 15px; font-size: 9px; }
            .print-guidelines h4 { margin: 0 0 4px 0; font-size: 10px; text-transform: uppercase; background: #eee; padding: 3px;}
            .print-guidelines ul { margin: 0; padding-left: 15px; }
            
            .print-workout { margin-bottom: 15px; page-break-inside: avoid; }
            .print-workout h3 { background: #e0e0e0; border: 1px solid #000; border-bottom: none; padding: 4px 8px; margin: 0; font-size: 11px; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            table { width: 100%; border-collapse: collapse; border: 1px solid #000; }
            th, td { border: 1px solid #000; padding: 4px; text-align: left; font-size: 9px; }
            th { background-color: #f0f0f0; font-weight: bold; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            
            .legal-text { font-size: 7.5px; color: #333; text-align: justify; margin-top: 15px; font-style: italic; border-top: 1px dashed #ccc; padding-top: 5px; page-break-inside: avoid;}
            
            .expired-stamp { color: red; border: 2px solid red; padding: 2px 5px; font-weight: bold; display: inline-block; transform: rotate(-15deg); margin-left: 10px; }
        }
        ::-webkit-scrollbar { width: 6px; } ::-webkit-scrollbar-track { background: transparent; } ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
    </style>
</head>
<body data-theme="Masculino">

    <div id="app-views">
        <!-- ========================================== -->
        <!-- VIEW: LOGIN -->
        <!-- ========================================== -->
        <div id="view-login" class="view active flex items-center justify-center min-h-screen p-4">
            <div class="card p-8 rounded-2xl shadow-2xl max-w-md w-full text-center">
                <i class="fas fa-dumbbell text-5xl text-primary mb-4"></i>
                <h1 class="text-3xl font-bold mb-2">PowFit Pro</h1>
                <p class="text-sm opacity-70 mb-8">Plataforma Profissional & Gestão de Redes</p>
                
                <button id="btn-login-google" class="w-full bg-white text-gray-800 font-medium py-3 px-4 rounded-lg flex items-center justify-center gap-3 hover:bg-gray-100 transition shadow">
                    <img src="https://www.svgrepo.com/show/475656/google-color.svg" class="w-5 h-5" alt="Google">
                    Entrar com o Google
                </button>
                <div id="login-loading" class="hidden justify-center mt-4"><div class="loader"></div></div>
            </div>
        </div>

        <!-- ========================================== -->
        <!-- VIEW: DASHBOARD (FRANQUIAS & RELATÓRIOS) -->
        <!-- ========================================== -->
        <div id="view-dashboard" class="view min-h-screen p-4 sm:p-8 max-w-6xl mx-auto">
            <div class="flex justify-between items-center mb-8 border-b pb-4" style="border-color: var(--border-color)">
                <div>
                    <h1 class="text-2xl font-bold"><i class="fas fa-network-wired text-primary"></i> Painel de Gestão</h1>
                    <p class="text-sm opacity-70">Gerencie Redes, Unidades e Equipe</p>
                </div>
                <div class="flex gap-3">
                    <button onclick="showReportModal()" class="btn-primary px-4 py-2 rounded-lg font-medium text-sm flex items-center gap-2"><i class="fas fa-chart-bar"></i> Relatório Mensal</button>
                    <button id="btn-logout" class="bg-red-500 hover:bg-red-600 text-white px-4 py-2 rounded-lg font-medium text-sm"><i class="fas fa-sign-out-alt"></i> Sair</button>
                </div>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <!-- REDES -->
                <div class="card rounded-xl p-5 h-[70vh] flex flex-col">
                    <div class="flex justify-between items-center mb-4">
                        <h2 class="font-bold">1. Redes</h2>
                        <button onclick="addNetwork()" class="text-primary text-sm hover:underline"><i class="fas fa-plus"></i> Nova</button>
                    </div>
                    <div id="list-networks" class="flex-1 overflow-y-auto space-y-2"></div>
                </div>
                <!-- UNIDADES -->
                <div class="card rounded-xl p-5 h-[70vh] flex flex-col opacity-50 transition-opacity" id="col-units">
                    <div class="flex justify-between items-center mb-4">
                        <h2 class="font-bold">2. Unidades</h2>
                        <button onclick="addUnit()" class="text-primary text-sm hover:underline"><i class="fas fa-plus"></i> Nova</button>
                    </div>
                    <div id="list-units" class="flex-1 overflow-y-auto space-y-2">Selecione uma rede...</div>
                </div>
                <!-- MEMBROS -->
                <div class="card rounded-xl p-5 h-[70vh] flex flex-col opacity-50 transition-opacity" id="col-members">
                    <div class="flex justify-between items-center mb-4">
                        <h2 class="font-bold">3. Membros (Equipe)</h2>
                        <button onclick="openMemberModal()" class="text-primary text-sm hover:underline"><i class="fas fa-plus"></i> Novo</button>
                    </div>
                    <div id="list-members" class="flex-1 overflow-y-auto space-y-2">Selecione uma unidade...</div>
                </div>
            </div>
        </div>

        <!-- ========================================== -->
        <!-- VIEW: APP BUILDER (PRANCHETA) -->
        <!-- ========================================== -->
        <div id="view-builder" class="view min-h-screen p-4 sm:p-6 lg:p-8 max-w-7xl mx-auto pb-24">
            <div class="flex flex-col sm:flex-row justify-between items-center mb-6 gap-4">
                <div>
                    <button onclick="switchView('view-dashboard')" class="text-sm opacity-70 hover:text-primary mb-1"><i class="fas fa-arrow-left"></i> Voltar ao Painel</button>
                    <h1 class="text-2xl font-bold flex items-center gap-2">
                        <i class="fas fa-clipboard-check text-primary"></i> Prancheta de Montagem
                    </h1>
                    <p class="text-sm text-primary font-medium" id="active-member-display">Atuando como: --</p>
                </div>
                <div class="flex gap-2">
                    <button onclick="openWorkoutHistory()" class="bg-gray-700 hover:bg-gray-600 text-white px-4 py-2 rounded-lg font-medium text-sm flex items-center gap-2 transition">
                        <i class="fas fa-cloud"></i> Histórico da Equipe
                    </button>
                    <button onclick="saveAndPrint()" class="btn-primary px-5 py-2 rounded-lg font-medium shadow flex items-center gap-2">
                        <i class="fas fa-save"></i> Salvar & Imprimir
                    </button>
                </div>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
                <!-- COLUNA ESQUERDA -->
                <div class="lg:col-span-4 space-y-6">
                    <!-- Dados do Aluno -->
                    <div class="card rounded-xl p-5">
                        <div class="flex justify-between items-center mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                            <h2 class="font-semibold flex items-center gap-2"><i class="fas fa-user text-primary"></i> Dados do Aluno</h2>
                            <div id="imc-display"></div>
                        </div>
                        <div class="space-y-4">
                            <input type="text" id="stu-name" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Nome do aluno">
                            <div class="grid grid-cols-3 gap-2">
                                <input type="number" id="stu-age" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Idade">
                                <input type="number" id="stu-weight" oninput="calculateIMC()" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Peso(kg)">
                                <input type="number" id="stu-height" step="0.01" oninput="calculateIMC()" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Alt(m)">
                            </div>
                            <div class="grid grid-cols-2 gap-2">
                                <select id="stu-gender" onchange="changeTheme()" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                    <option value="Masculino">Masculino</option><option value="Feminino">Feminino</option>
                                </select>
                                <select id="stu-level" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                    <option>Iniciante</option><option>Intermediário</option><option>Avançado</option>
                                </select>
                            </div>
                            <select id="stu-objective" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                <option value="Emagrecimento">Objetivo: Emagrecimento</option><option value="Hipertrofia">Objetivo: Hipertrofia</option>
                                <option value="Definição">Objetivo: Definição</option><option value="Condicionamento">Objetivo: Condicionamento</option>
                                <option value="Resistência">Objetivo: Resistência</option><option value="Força">Objetivo: Força</option>
                                <option value="Reabilitação">Objetivo: Reabilitação</option><option value="Saúde geral">Objetivo: Saúde geral</option>
                            </select>
                            <select id="stu-freq" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                <option>Freq: 3 dias</option><option>Freq: 5 dias</option><option>Freq: 6 dias</option><option>Personalizado</option>
                            </select>
                        </div>
                    </div>

                    <!-- Saúde -->
                    <div class="card rounded-xl p-5">
                        <h2 class="font-semibold mb-2 flex items-center gap-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                            <i class="fas fa-notes-medical text-primary"></i> Estado de Saúde
                        </h2>
                        <div class="grid grid-cols-2 gap-2 text-xs" id="health-container"></div>
                    </div>
                </div>

                <!-- COLUNA DIREITA -->
                <div class="lg:col-span-8 space-y-4">
                    <div class="flex justify-between items-center bg-opacity-10 p-3 rounded-lg card border-dashed border-2 gap-4">
                        <div class="flex gap-2">
                            <select id="stu-validity" class="input-field rounded px-2 py-1 text-xs"><option>Validade: 15 dias</option><option selected>Validade: 30 dias</option><option>Validade: 60 dias</option><option>Validade: 90 dias</option></select>
                        </div>
                        <button onclick="addWorkoutDay()" class="btn-primary px-3 py-1.5 rounded text-xs font-medium"><i class="fas fa-plus"></i> Dia de Treino</button>
                    </div>

                    <div id="workouts-container" class="space-y-4"></div>

                    <div class="card rounded-xl p-4">
                        <h2 class="text-sm font-semibold mb-2"><i class="fas fa-pen text-primary"></i> Recomendações Manuais</h2>
                        <textarea id="stu-recs" rows="3" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Hidratação, descanso, cuidados..."></textarea>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- MODAIS -->
    <!-- Modal Cadastro de Membro -->
    <div id="modal-member" class="fixed inset-0 bg-black bg-opacity-80 hidden z-50 flex items-center justify-center p-4">
        <div class="card w-full max-w-md rounded-xl p-5">
            <h3 class="text-lg font-bold mb-4">Adicionar Membro à Unidade</h3>
            <div class="space-y-3">
                <input type="text" id="m-name" class="input-field w-full rounded p-2 text-sm" placeholder="Nome Completo">
                <input type="text" id="m-cpf" class="input-field w-full rounded p-2 text-sm" placeholder="CPF">
                <input type="text" id="m-state" class="input-field w-full rounded p-2 text-sm" placeholder="Estado (UF)">
                <select id="m-category" onchange="toggleCrefModal()" class="input-field w-full rounded p-2 text-sm">
                    <option value="PEF">Profissional de Educação Física (PEF)</option>
                    <option value="TE">Treinador Esportivo (TE)</option>
                </select>
                <input type="text" id="m-cref" class="input-field w-full rounded p-2 text-sm" placeholder="CREF">
            </div>
            <div class="flex gap-2 mt-4">
                <button onclick="closeModal('modal-member')" class="flex-1 bg-gray-600 text-white rounded p-2 text-sm">Cancelar</button>
                <button onclick="saveMember()" class="flex-1 btn-primary rounded p-2 text-sm">Salvar Membro</button>
            </div>
        </div>
    </div>

    <!-- Modal de Exercícios -->
    <div id="modal-exercise" class="fixed inset-0 bg-black bg-opacity-80 hidden z-50 flex items-center justify-center p-2 sm:p-4">
        <div class="card w-full max-w-5xl rounded-xl flex flex-col h-[85vh]">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="font-bold">Adicionar Exercício</h3>
                <button onclick="closeModal('modal-exercise')" class="text-gray-400 text-xl">&times;</button>
            </div>
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <div class="w-full md:w-1/4 border-r overflow-y-auto p-2" style="border-color: var(--border-color)" id="modal-categories"></div>
                <div class="w-full md:w-3/4 flex flex-col">
                    <div class="p-2 border-b flex gap-2" style="border-color: var(--border-color)">
                        <input type="text" id="custom-ex-name" class="input-field flex-1 rounded p-1 text-xs" placeholder="Criar exercício personalizado nesta categoria...">
                        <button onclick="saveCustomExercise()" class="btn-primary px-3 rounded text-xs font-bold">+</button>
                    </div>
                    <div class="flex-1 overflow-y-auto p-3 bg-black bg-opacity-5" id="modal-exercises-list"></div>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal Histórico Nuvem -->
    <div id="modal-history" class="fixed inset-0 bg-black bg-opacity-80 hidden z-50 flex items-center justify-center p-4">
        <div class="card w-full max-w-3xl rounded-xl flex flex-col h-[80vh]">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="font-bold"><i class="fas fa-cloud text-primary"></i> Fichas Salvas na Nuvem (Esta Unidade)</h3>
                <button onclick="closeModal('modal-history')" class="text-gray-400 text-xl">&times;</button>
            </div>
            <div class="flex-1 overflow-y-auto p-4 space-y-2" id="history-list-container">Carregando...</div>
        </div>
    </div>

    <!-- Modal Relatório -->
    <div id="modal-report" class="fixed inset-0 bg-black bg-opacity-80 hidden z-50 flex items-center justify-center p-4">
        <div class="card w-full max-w-md rounded-xl p-5">
            <h3 class="text-lg font-bold mb-4">Relatório de Produtividade</h3>
            <div class="space-y-3 mb-4">
                <select id="rep-unit" class="input-field w-full rounded p-2 text-sm" onchange="generateReportData()"></select>
                <div class="flex gap-2">
                    <select id="rep-month" class="input-field flex-1 rounded p-2 text-sm" onchange="generateReportData()">
                        <option value="01">Janeiro</option><option value="02">Fevereiro</option><option value="03">Março</option><option value="04">Abril</option><option value="05">Maio</option><option value="06">Junho</option><option value="07">Julho</option><option value="08">Agosto</option><option value="09">Setembro</option><option value="10">Outubro</option><option value="11">Novembro</option><option value="12">Dezembro</option>
                    </select>
                    <input type="number" id="rep-year" class="input-field w-24 rounded p-2 text-sm" onchange="generateReportData()">
                </div>
            </div>
            <div id="report-output" class="bg-black bg-opacity-10 p-3 rounded text-sm min-h-[100px] max-h-[200px] overflow-y-auto mb-4 border border-gray-600"></div>
            <div class="flex gap-2">
                <button onclick="closeModal('modal-report')" class="flex-1 bg-gray-600 text-white rounded p-2 text-sm">Fechar</button>
                <button onclick="printReport()" class="flex-1 btn-primary rounded p-2 text-sm"><i class="fas fa-print"></i> Imprimir Relatório</button>
            </div>
        </div>
    </div>


    <!-- ÁREAS DE IMPRESSÃO OCULTAS -->
    <div id="print-area"></div>
    <div id="print-report-area"></div>

    <!-- SCRIPT TIPO MÓDULO PARA FIREBASE E LÓGICA -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-app.js";
        import { getAuth, signInWithPopup, GoogleAuthProvider, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, addDoc, query, where, getDocs, orderBy, deleteDoc } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyBWAbrGzyGdDJHbsXYKjAbF3jeULH3FVp0",
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

        // --- DADOS ESTÁTICOS ---
        const healthPEF = { "🟢 Saudável": "Manter treinos regulares 3–5x por semana, combinando musculação e cardio.", "⚪ Sedentário": "Iniciar com treinos leves 2–3x por semana. Priorizar adaptação.", "🟡 Sobrepeso": "Treinar 3–5x por semana com foco em gasto calórico.", "🔴 Obesidade": "Iniciar com exercícios de baixo impacto. Evolução gradual.", "⚖️ Baixo peso": "Foco em musculação para ganho de massa. Treinos moderados.", "🍬 Diabetes": "Treinos regulares moderados. Monitorar glicemia e evitar jejum.", "❤️ Hipertensão": "Treinos moderados, evitar prender a respiração.", "🔵 Hipotensão": "Evitar mudanças bruscas. Manter hidratação.", "💔 Problemas cardíacos": "Treinos leves com liberação médica. Monitorar frequência.", "🦴 Problemas articulares": "Evitar impacto. Priorizar máquinas.", "🫁 Problemas respiratórios": "Treinos leves a moderados. Atenção à respiração.", "⚠️ Lesões": "Adaptar exercícios. Evitar dor e focar na recuperação.", "🤰 Gestante": "Treinos leves a moderados, sem impacto. Liberação médica.", "🤱 Lactante": "Treinar normalmente com atenção à hidratação e energia.", "👴 Idoso": "Foco em força, equilíbrio e mobilidade. Segurança em 1º lugar." };
        const healthTE = { "🟢 Saudável": "Manter treinos regulares de 3–5x por semana. Constância e boa alimentação contribuem para resultados.", "⚪ Sedentário": "Início gradual, treinos leves. Prioridade é desenvolver constância e execução correta.", "🟡 Sobrepeso": "Musculação e cardio contribuem para condicionamento. Progressão respeitando a individualidade.", "🔴 Obesidade": "Menor impacto e progressão gradual. Segurança e constância são prioridades.", "⚖️ Baixo peso": "Musculação auxilia no ganho de massa com descanso adequado. Evolução progressiva.", "🍬 Diabetes": "Manter acompanhamento médico. Interromper se houver tontura ou mal-estar.", "❤️ Hipertensão": "Manter acompanhamento médico. Evitar prender a respiração (Manobra de Valsalva).", "🔵 Hipotensão": "Evitar mudanças bruscas de posição. Manter hidratação.", "💔 Problemas cardíacos": "Prática apenas com liberação médica. Respeitar limites e controle de intensidade.", "🦴 Problemas articulares": "Menor impacto. Adaptação dos exercícios ajuda na segurança.", "🫁 Problemas respiratórios": "Progressão gradual. Interromper em caso de falta de ar.", "⚠️ Lesões": "Adaptação respeitando a limitação, evitar dor. Seguir orientação profissional.", "🤰 Gestante": "Liberação médica essencial. Foco na segurança e bem-estar, sem risco.", "🤱 Lactante": "Atividade mantida respeitando a recuperação e hidratação.", "👴 Idoso": "Musculação contribui para autonomia. Respeitar limitações com intensidade moderada." };
        const objTexts = { "Emagrecimento": "Foco em déficit calórico com treinos mistos de força e aeróbicos.", "Hipertrofia": "Prioridade na progressão de carga e volume. Essencial superávit calórico.", "Definição": "Manutenção muscular reduzindo percentual de gordura. Atenção estrita à dieta.", "Condicionamento": "Menor intervalo, circuitos e alta integração cardiopulmonar.", "Resistência": "Séries mais longas, cadência controlada e aprimoramento muscular.", "Força": "Cargas altas, baixas reps e maiores intervalos de descanso.", "Reabilitação": "Fortalecimento específico, mobilidade e controle motor. Respeitar limites.", "Saúde geral": "Equilíbrio entre força, cardio e flexibilidade. Foco em constância." };
        const dbCategories = {
            "🔥 PEITO": ["Supino Reto", "Supino Inclinado", "Supino Inclinado com Halteres", "Supino Fechado com Halteres", "Cross Over", "Cross Over Alto", "Cross Over Baixo", "Crucifixo Reto", "Crucifixo Inclinado com Halteres", "Crucifixo na Máquina", "Peck Fly", "Peck Fly Unilateral", "Pullover", "Flexão de Braço", "Flexão com Pés Elevados", "Flexão Explosiva"],
            "🦍 COSTAS": ["Puxada Alta", "Puxada de Frente Supinada", "Pulldown", "Remada Aberta", "Remada Baixa", "Remada Curvada", "Remada Curvada Supinada", "Remada Cavalinho (T-Bar)", "Serrote", "Facepull (puxada de cima para baixo)"],
            "🦵 PERNAS": ["Agachamento Livre", "Agachamento Taça", "Agachamento no Smith", "Agachamento com passada lateral", "Squat", "Hack Machine", "Leg 45°", "Leg 90°", "Agachamento Sumô", "Agachamento Sissy (Livre)", "Afundo", "Recuo", "Avanço", "Passada", "Búlgaro", "Step-up", "Levantamento Terra", "Levantamento Terra Romeno", "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", "Cadeira Flexora", "Elevação Pélvica no Banco", "Elevação Pélvica no Chão", "Elevação Pélvica Unilateral no Chão", "Extensão de Quadril (Glúteo Máximo)", "Extensão Cruzada (Glúteo Médio)", "Coice", "Cachorrinho", "Cadeira Extensora", "Adução", "Abdução", "Abdução Inclinada", "Flexão Nórdica", "Flexão Nórdica Invertida", "Panturrilha Livre", "Panturrilha no Leg Press", "Panturrilha Banco", "Panturrilha Squat", "Panturrilha Unilateral"],
            "💪 BÍCEPS": ["Rosca Direta", "Rosca Alternada", "Rosca 21", "Rosca Scott Barra W", "Rosca Scott Unilateral", "Rosca Scott com Halteres", "Rosca Martelo", "Rosca Cross", "Rosca Inversa", "Rosca 45°"],
            "💪 TRÍCEPS": ["Triceps Pulley Unilateral", "Tríceps Pulley Barra", "Tríceps Pulley Corda", "Tríceps Pulley Pegada Inversa", "Tríceps Francês na Corda", "Tríceps Francês com Halter", "Tríceps Francês Unilateral", "Tríceps Cruzado Polia Dupla", "Tríceps Coice Unilateral", "Tríceps Arremesso", "Tríceps Testa", "Mergulho no Banco"],
            "🪨 OMBROS": ["Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", "Elevação Lateral Unilateral Cross", "Elevação Lateral Sentado", "Desenvolvimento com Halteres", "Desenvolvimento com Barra", "Arnold Press", "Elevação Borboleta", "Crucifixo Inverso Sentado com Halteres", "Crucifixo Inverso na Polia", "Crucifixo Inverso Unilateral na Polia", "Facepull (puxada reta)", "Remada Alta", "Encolhimento (Trapézio)"],
            "🧠 ABDÔMEN": ["Infra com Elevação de Perna", "Abdominal Supra", "Abdominal Remador", "Abdominal Bicicleta", "Abdominal Twister com Peso", "Prancha", "Prancha Lateral", "Trituração de Cabos em Pé", "Isometria na parede"],
            "🫀 CARDIO": ["Bicicleta 10 Minutos", "Bicicleta 15 Minutos", "Bicicleta 20 Minutos", "Esteira 10 Minutos", "Esteira 15 Minutos", "Esteira 20 Minutos", "Pular Corda"]
        };
        const dbTechs = ["Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", "Negativa", "Isometria", "Parciais", "Pirâmide"];
        const daysW = ["SEGUNDA-FEIRA", "TERÇA-FEIRA", "QUARTA-FEIRA", "QUINTA-FEIRA", "SEXTA-FEIRA", "SÁBADO", "DOMINGO"];

        // --- STATE MGT ---
        window.APP = {
            user: null, currentNetwork: null, currentUnit: null, currentMember: null,
            workouts: [], modalWorkoutId: null, modalCategory: "🔥 PEITO", customEx: [], editingDocId: null
        };

        // --- AUTH ---
        document.getElementById('btn-login-google').addEventListener('click', async () => {
            const btn = document.getElementById('btn-login-google');
            const loader = document.getElementById('login-loading');
            btn.classList.add('hidden'); loader.classList.remove('hidden'); loader.classList.add('flex');
            try { await signInWithPopup(auth, new GoogleAuthProvider()); } 
            catch (e) { alert("Erro ao logar."); btn.classList.remove('hidden'); loader.classList.add('hidden'); }
        });

        document.getElementById('btn-logout').addEventListener('click', () => signOut(auth));

        onAuthStateChanged(auth, async (user) => {
            if(user) {
                APP.user = user;
                switchView('view-dashboard');
                await loadCustomExercises();
                loadNetworks();
            } else {
                APP.user = null;
                switchView('view-login');
                document.getElementById('btn-login-google').classList.remove('hidden');
                document.getElementById('login-loading').classList.add('hidden');
            }
        });

        // --- UTILS ---
        window.switchView = (id) => {
            document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            if(id === 'view-builder' && APP.workouts.length === 0) window.addWorkoutDay();
        };
        window.genId = () => Math.random().toString(36).substr(2, 9);
        window.closeModal = (id) => document.getElementById(id).classList.add('hidden');
        window.openModal = (id) => document.getElementById(id).classList.remove('hidden');

        // --- DASHBOARD (HIERARCHY) ---
        async function loadNetworks() {
            const list = document.getElementById('list-networks');
            list.innerHTML = '<div class="loader mx-auto"></div>';
            const q = query(collection(db, "networks"), where("ownerId", "==", APP.user.uid));
            const snap = await getDocs(q);
            list.innerHTML = '';
            if(snap.empty) { list.innerHTML = '<div class="text-xs opacity-50">Nenhuma rede.</div>'; return; }
            snap.forEach(doc => {
                const data = doc.data();
                list.innerHTML += `<button onclick="selectNetwork('${doc.id}', '${data.name}')" class="w-full text-left p-2 rounded text-sm bg-black bg-opacity-10 hover:bg-opacity-20 font-bold border-l-4 border-primary transition">${data.name}</button>`;
            });
            document.getElementById('col-units').classList.add('opacity-50');
            document.getElementById('col-members').classList.add('opacity-50');
        }

        window.addNetwork = async () => {
            const name = prompt("Nome da Rede:");
            if(name) { await addDoc(collection(db, "networks"), { name, ownerId: APP.user.uid }); loadNetworks(); }
        };

        window.selectNetwork = async (id, name) => {
            APP.currentNetwork = {id, name}; APP.currentUnit = null; APP.currentMember = null;
            document.getElementById('col-units').classList.remove('opacity-50');
            document.getElementById('col-members').classList.add('opacity-50');
            document.getElementById('list-members').innerHTML = 'Selecione uma unidade...';
            
            const list = document.getElementById('list-units'); list.innerHTML = '<div class="loader mx-auto"></div>';
            const q = query(collection(db, "units"), where("networkId", "==", id));
            const snap = await getDocs(q); list.innerHTML = '';
            if(snap.empty) { list.innerHTML = '<div class="text-xs opacity-50">Nenhuma unidade.</div>'; return; }
            snap.forEach(d => {
                list.innerHTML += `<button onclick="selectUnit('${d.id}', '${d.data().name}')" class="w-full text-left p-2 rounded text-sm bg-black bg-opacity-10 hover:bg-opacity-20 border-l-4 border-green-500 transition">${d.data().name}</button>`;
            });
        };

        window.addUnit = async () => {
            if(!APP.currentNetwork) return alert("Selecione uma rede.");
            const name = prompt("Nome da Unidade:");
            if(name) { await addDoc(collection(db, "units"), { name, networkId: APP.currentNetwork.id, ownerId: APP.user.uid }); selectNetwork(APP.currentNetwork.id, APP.currentNetwork.name); }
        };

        window.selectUnit = async (id, name) => {
            APP.currentUnit = {id, name}; APP.currentMember = null;
            document.getElementById('col-members').classList.remove('opacity-50');
            
            const list = document.getElementById('list-members'); list.innerHTML = '<div class="loader mx-auto"></div>';
            const q = query(collection(db, "members"), where("unitId", "==", id));
            const snap = await getDocs(q); list.innerHTML = '';
            if(snap.empty) { list.innerHTML = '<div class="text-xs opacity-50">Nenhum membro.</div>'; return; }
            snap.forEach(d => {
                const dt = d.data();
                list.innerHTML += `<div class="p-2 rounded text-sm bg-black bg-opacity-10 border-l-4 border-yellow-500 mb-2">
                    <div class="font-bold">${dt.name}</div>
                    <div class="text-[10px] opacity-70">${dt.category} - ${dt.state}</div>
                    <button onclick="startBuilder('${d.id}', '${dt.name}', '${dt.category}', '${dt.cref || ''}', '${dt.state}')" class="mt-2 w-full text-xs btn-primary py-1 rounded">Usar este Perfil</button>
                </div>`;
            });
        };

        window.openMemberModal = () => { if(!APP.currentUnit) return alert("Selecione uma unidade."); openModal('modal-member'); };
        window.toggleCrefModal = () => document.getElementById('m-cref').style.display = document.getElementById('m-category').value === 'PEF' ? 'block' : 'none';
        
        window.saveMember = async () => {
            const name = document.getElementById('m-name').value; const cpf = document.getElementById('m-cpf').value;
            const state = document.getElementById('m-state').value; const cat = document.getElementById('m-category').value;
            const cref = document.getElementById('m-cref').value;
            if(!name || !cpf || !state) return alert("Preencha campos básicos.");
            await addDoc(collection(db, "members"), { name, cpf, state, category: cat, cref: cat==='PEF'?cref:'', unitId: APP.currentUnit.id, ownerId: APP.user.uid });
            closeModal('modal-member'); selectUnit(APP.currentUnit.id, APP.currentUnit.name);
        };

        // --- BUILDER LOGIC ---
        window.startBuilder = (id, name, cat, cref, state) => {
            APP.currentMember = {id, name, category: cat, cref, state};
            document.getElementById('active-member-display').innerText = `Atuando como: ${name} (${cat})`;
            APP.workouts = []; APP.editingDocId = null; // reset
            renderHealthCb();
            switchView('view-builder');
        };

        window.renderHealthCb = () => {
            const hData = APP.currentMember.category === 'PEF' ? healthPEF : healthTE;
            const c = document.getElementById('health-container');
            c.innerHTML = Object.keys(hData).map(k => `<label class="flex items-center space-x-1 cursor-pointer"><input type="checkbox" value="${k}" class="health-cb w-3 h-3 text-primary bg-transparent border-gray-500"> <span class="truncate">${k}</span></label>`).join('');
        };

        window.calculateIMC = () => {
            const w = parseFloat(document.getElementById('stu-weight').value); const h = parseFloat(document.getElementById('stu-height').value);
            const d = document.getElementById('imc-display');
            if (w > 0 && h > 0) {
                const imc = (w / (h * h)).toFixed(1);
                d.innerHTML = `<span class="bg-primary text-white px-2 py-0.5 rounded-full text-[10px] font-bold">IMC: ${imc}</span>`;
            } else d.innerHTML = '';
        };

        window.changeTheme = () => document.body.setAttribute('data-theme', document.getElementById('stu-gender').value);

        // --- WORKOUT MANAGEMENT ---
        window.addWorkoutDay = () => {
            const prefix = APP.workouts.length < 7 ? `TREINO ${daysW[APP.workouts.length]}` : `NOVO TREINO ${APP.workouts.length+1}`;
            APP.workouts.push({ id: genId(), title: prefix, exercises: [] }); renderW();
        };
        window.dupW = (id) => { const w = APP.workouts.find(x=>x.id===id); if(w) { const nw = JSON.parse(JSON.stringify(w)); nw.id=genId(); nw.title+=' (Cópia)'; APP.workouts.push(nw); renderW();} };
        window.remW = (id) => { if(confirm("Remover dia?")) { APP.workouts = APP.workouts.filter(x=>x.id!==id); renderW(); } };
        window.updWT = (id, v) => { const w = APP.workouts.find(x=>x.id===id); if(w) w.title=v; };
        
        window.remEx = (wId, idx) => { APP.workouts.find(w=>w.id===wId).exercises.splice(idx, 1); renderW(); };
        window.movEx = (wId, idx, dir) => {
            const arr = APP.workouts.find(w=>w.id===wId).exercises;
            if(dir==='up' && idx>0) [arr[idx], arr[idx-1]] = [arr[idx-1], arr[idx]];
            if(dir==='down' && idx<arr.length-1) [arr[idx], arr[idx+1]] = [arr[idx+1], arr[idx]];
            renderW();
        };
        window.updEx = (wId, idx, f, v) => APP.workouts.find(w=>w.id===wId).exercises[idx][f] = v;

        window.renderW = () => {
            const c = document.getElementById('workouts-container'); c.innerHTML = '';
            APP.workouts.forEach(w => {
                let exs = w.exercises.length === 0 ? `<div class="p-4 text-center text-xs opacity-50">Sem exercícios</div>` : w.exercises.map((ex, i) => `
                    <div class="flex gap-2 p-2 items-center border-b border-opacity-10 border-gray-500 text-xs">
                        <div class="flex flex-col gap-1 w-4"><button onclick="movEx('${w.id}',${i},'up')">↑</button><button onclick="movEx('${w.id}',${i},'down')">↓</button></div>
                        <div class="flex-1 truncate"><span class="text-[9px] opacity-60 uppercase block">${ex.category}</span><span class="font-bold">${ex.name}</span></div>
                        <input type="text" value="${ex.sets}" onchange="updEx('${w.id}',${i},'sets',this.value)" class="input-field w-10 text-center rounded p-1" placeholder="S">
                        <span>x</span>
                        <input type="text" value="${ex.reps}" onchange="updEx('${w.id}',${i},'reps',this.value)" class="input-field w-12 text-center rounded p-1" placeholder="R">
                        <select onchange="updEx('${w.id}',${i},'technique',this.value)" class="input-field w-20 rounded p-1 truncate text-[10px]">${dbTechs.map(t=>`<option ${ex.technique===t?'selected':''}>${t}</option>`).join('')}</select>
                        <input type="text" value="${ex.obs}" onchange="updEx('${w.id}',${i},'obs',this.value)" class="input-field w-24 rounded p-1" placeholder="Obs">
                        <button onclick="remEx('${w.id}',${i})" class="text-red-500"><i class="fas fa-times"></i></button>
                    </div>`).join('');
                c.innerHTML += `<div class="card rounded-lg overflow-hidden border">
                    <div class="p-2 bg-black bg-opacity-10 flex justify-between items-center">
                        <input type="text" value="${w.title}" onchange="updWT('${w.id}', this.value)" class="input-field bg-transparent font-bold text-sm w-2/3 uppercase px-1">
                        <div class="flex gap-2"><button onclick="dupW('${w.id}')"><i class="fas fa-copy"></i></button><button onclick="remW('${w.id}')" class="text-red-500"><i class="fas fa-trash"></i></button></div>
                    </div>
                    <div>${exs}</div>
                    <button onclick="openExModal('${w.id}')" class="w-full text-[11px] font-bold text-primary p-2 bg-black bg-opacity-5 hover:bg-opacity-10 text-left pl-4">+ ADD EXERCÍCIO</button>
                </div>`;
            });
        };

        // --- EXERCISE MODAL & CUSTOM EXERCISES ---
        window.openExModal = (id) => { APP.modalWorkoutId = id; openModal('modal-exercise'); renderExCats(); renderExs(); };
        window.renderExCats = () => {
            document.getElementById('modal-categories').innerHTML = Object.keys(dbCategories).map(cat => `
                <button onclick="APP.modalCategory='${cat}'; renderExCats(); renderExs();" class="w-full text-left p-2 rounded text-xs mb-1 ${APP.modalCategory===cat?'bg-primary text-white':'hover:bg-black hover:bg-opacity-10'}">${cat}</button>
            `).join('');
        };
        
        async function loadCustomExercises() {
            if(!APP.user) return;
            const q = query(collection(db, "custom_exercises"), where("ownerId", "==", APP.user.uid));
            const snap = await getDocs(q); APP.customEx = [];
            snap.forEach(d => APP.customEx.push({id: d.id, ...d.data()}));
        }

        window.saveCustomExercise = async () => {
            const name = document.getElementById('custom-ex-name').value.trim();
            if(!name) return;
            document.getElementById('custom-ex-name').value = '';
            const docRef = await addDoc(collection(db, "custom_exercises"), { name, category: APP.modalCategory, ownerId: APP.user.uid });
            APP.customEx.push({ id: docRef.id, name, category: APP.modalCategory });
            renderExs();
        };

        window.delCustomEx = async (id, event) => {
            event.stopPropagation();
            if(confirm("Excluir este exercício da nuvem?")) {
                await deleteDoc(doc(db, "custom_exercises", id));
                APP.customEx = APP.customEx.filter(x => x.id !== id);
                renderExs();
            }
        };

        window.renderExs = () => {
            const list = dbCategories[APP.modalCategory];
            const customList = APP.customEx.filter(x => x.category === APP.modalCategory);
            const isCardio = APP.modalCategory === "🫀 CARDIO";
            
            let html = list.map(name => `<button onclick="addExToW('${name}', ${isCardio})" class="card p-2 rounded border hover:border-primary text-xs text-left mb-2 flex justify-between group w-full"><span>${name}</span><i class="fas fa-plus opacity-0 group-hover:opacity-100 text-primary"></i></button>`).join('');
            
            if(customList.length > 0) {
                html += `<div class="mt-4 mb-2 text-[10px] font-bold text-primary uppercase">Meus Exercícios Customizados</div>`;
                html += customList.map(c => `<button onclick="addExToW('${c.name}', ${isCardio})" class="card p-2 rounded border border-primary hover:bg-primary hover:bg-opacity-10 text-xs text-left mb-2 flex justify-between items-center group w-full">
                    <span>${c.name}</span>
                    <div class="flex gap-2 opacity-0 group-hover:opacity-100 transition"><span class="text-primary"><i class="fas fa-plus"></i></span> <span onclick="delCustomEx('${c.id}', event)" class="text-red-500 hover:text-red-700"><i class="fas fa-trash"></i></span></div>
                </button>`).join('');
            }
            document.getElementById('modal-exercises-list').innerHTML = html;
        };

        window.addExToW = (name, isC) => {
            APP.workouts.find(w=>w.id===APP.modalWorkoutId).exercises.push({ category: APP.modalCategory, name, sets: isC?'1':'3', reps: isC?'-':'10 a 12', technique: 'Nenhuma', obs: '' });
            renderW(); document.getElementById('modal-exercises-list').style.opacity = '0.5'; setTimeout(()=>document.getElementById('modal-exercises-list').style.opacity = '1', 100);
        };


        // --- NUVEM & HISTÓRICO ---
        function getPayload() {
            return {
                memberId: APP.currentMember.id, memberName: APP.currentMember.name, unitId: APP.currentUnit.id, ownerId: APP.user.uid,
                createdAt: new Date().toISOString(), studentName: document.getElementById('stu-name').value || 'Sem Nome',
                stuAge: document.getElementById('stu-age').value, stuWeight: document.getElementById('stu-weight').value, stuHeight: document.getElementById('stu-height').value,
                stuGender: document.getElementById('stu-gender').value, stuLevel: document.getElementById('stu-level').value, stuObjective: document.getElementById('stu-objective').value,
                stuFreq: document.getElementById('stu-freq').value, stuValidity: document.getElementById('stu-validity').value, stuRecs: document.getElementById('stu-recs').value,
                health: Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value), workouts: APP.workouts
            };
        }

        window.openWorkoutHistory = async () => {
            openModal('modal-history'); const c = document.getElementById('history-list-container'); c.innerHTML = '<div class="loader mx-auto mt-10"></div>';
            const q = query(collection(db, "workouts"), where("unitId", "==", APP.currentUnit.id), orderBy("createdAt", "desc"));
            const snap = await getDocs(q); c.innerHTML = '';
            if(snap.empty) { c.innerHTML = '<div class="opacity-50 text-center mt-10">Nenhuma ficha salva nesta unidade.</div>'; return; }
            
            const now = new Date();
            snap.forEach(d => {
                const dt = d.data();
                const dDate = new Date(dt.createdAt);
                
                // Calculo de Validade
                let valDays = parseInt(dt.stuValidity.match(/\d+/)?.[0] || 30);
                const expDate = new Date(dDate.getTime() + valDays * 24 * 60 * 60 * 1000);
                const isExpired = now > expDate;
                
                c.innerHTML += `<div class="card p-3 rounded flex justify-between items-center text-sm border border-gray-600">
                    <div><div class="font-bold">${dt.studentName} ${isExpired?'<span class="text-[10px] text-red-500 font-bold ml-2">⚠️ VENCIDA</span>':''}</div>
                    <div class="text-[10px] opacity-70">Criado por: ${dt.memberName} em ${dDate.toLocaleDateString('pt-BR')}</div></div>
                    <button onclick="loadHistoryItem('${d.id}')" class="btn-primary px-3 py-1 text-xs rounded">CARREGAR</button>
                </div>`;
            });
        };

        window.loadHistoryItem = async (id) => {
            const d = await getDoc(doc(db, "workouts", id)); if(!d.exists()) return;
            const data = d.data(); APP.editingDocId = id;
            document.getElementById('stu-name').value = data.studentName; document.getElementById('stu-age').value = data.stuAge || '';
            document.getElementById('stu-weight').value = data.stuWeight || ''; document.getElementById('stu-height').value = data.stuHeight || '';
            document.getElementById('stu-gender').value = data.stuGender || 'Masculino'; document.getElementById('stu-level').value = data.stuLevel || 'Iniciante';
            document.getElementById('stu-objective').value = data.stuObjective || 'Emagrecimento'; document.getElementById('stu-freq').value = data.stuFreq || '3 dias';
            document.getElementById('stu-validity').value = data.stuValidity || 'Validade: 30 dias'; document.getElementById('stu-recs').value = data.stuRecs || '';
            changeTheme(); calculateIMC();
            document.querySelectorAll('.health-cb').forEach(cb => cb.checked = (data.health || []).includes(cb.value));
            APP.workouts = data.workouts || []; renderW(); closeModal('modal-history');
        };

        // --- RELATÓRIO DE PRODUTIVIDADE ---
        window.showReportModal = async () => {
            openModal('modal-report');
            document.getElementById('rep-year').value = new Date().getFullYear();
            const currMonth = String(new Date().getMonth() + 1).padStart(2, '0');
            document.getElementById('rep-month').value = currMonth;
            
            // Popular select de unidades da rede selecionada (ou todas do usuario logado)
            const uSel = document.getElementById('rep-unit'); uSel.innerHTML = '<option value="ALL">Todas as Unidades</option>';
            const q = query(collection(db, "units"), where("ownerId", "==", APP.user.uid));
            const snap = await getDocs(q);
            snap.forEach(d => uSel.innerHTML += `<option value="${d.id}">${d.data().name}</option>`);
            generateReportData();
        };

        window.generateReportData = async () => {
            const out = document.getElementById('report-output'); out.innerHTML = 'Processando dados na nuvem... <div class="loader inline-block ml-2 w-3 h-3"></div>';
            const unitId = document.getElementById('rep-unit').value;
            const mStr = document.getElementById('rep-month').value;
            const yStr = document.getElementById('rep-year').value;
            const targetPrefix = `${yStr}-${mStr}`; // Ex: 2026-04

            let q;
            if(unitId === 'ALL') q = query(collection(db, "workouts"), where("ownerId", "==", APP.user.uid));
            else q = query(collection(db, "workouts"), where("unitId", "==", unitId));
            
            const snap = await getDocs(q);
            let countTotal = 0; const prodMap = {};

            snap.forEach(d => {
                const dt = d.data();
                if(dt.createdAt && dt.createdAt.startsWith(targetPrefix)) {
                    countTotal++;
                    prodMap[dt.memberName] = (prodMap[dt.memberName] || 0) + 1;
                }
            });

            if(countTotal === 0) { out.innerHTML = 'Nenhuma ficha gerada neste mês.'; return; }

            let html = `<strong>Total do Mês: ${countTotal} fichas</strong><br><br>Produtividade por Membro:<ul class="list-disc pl-4 mt-2">`;
            Object.keys(prodMap).sort((a,b)=>prodMap[b]-prodMap[a]).forEach(k => html += `<li>${k}: ${prodMap[k]} fichas</li>`);
            html += `</ul>`; out.innerHTML = html;
        };

        window.printReport = () => {
            const out = document.getElementById('report-output').innerHTML;
            const m = document.getElementById('rep-month').options[document.getElementById('rep-month').selectedIndex].text;
            const y = document.getElementById('rep-year').value;
            const u = document.getElementById('rep-unit').options[document.getElementById('rep-unit').selectedIndex].text;
            
            document.getElementById('print-report-area').innerHTML = `
                <div class="print-header"><h1 class="print-title">RELATÓRIO DE PRODUTIVIDADE</h1><div class="prof-info">${m}/${y} - ${u}</div></div>
                <div style="font-size: 12px; margin-top:20px;">${out}</div>
                <div class="legal-text" style="text-align:center;">Relatório gerado automaticamente por PowFit Pro.</div>
            `;
            const pa = document.getElementById('print-area'); pa.style.display = 'none'; // oculta ficha
            document.getElementById('print-report-area').style.display = 'block'; // mostra relatorio
            window.print();
        };

        // --- SALVAR & IMPRIMIR FICHA ---
        window.saveAndPrint = async () => {
            // Salvar no Firebase
            try {
                if(APP.editingDocId) await setDoc(doc(db, "workouts", APP.editingDocId), getPayload(), {merge:true});
                else await addDoc(collection(db, "workouts"), getPayload());
            } catch(e) { console.error(e); alert("Aviso: Falha ao sincronizar com a nuvem, mas a impressão continuará."); }

            // Configurar Impressão
            document.getElementById('print-report-area').style.display = 'none';
            document.getElementById('print-area').style.display = 'block';

            const d = getPayload();
            let imcStr = "-"; const w = parseFloat(d.stuWeight); const h = parseFloat(d.stuHeight);
            if(w>0 && h>0) imcStr = (w / (h * h)).toFixed(1);

            const isPEF = APP.currentMember.category === 'PEF';
            const hSource = isPEF ? healthPEF : healthTE;
            const profLabel = isPEF ? 'Profissional de Educação Física' : 'Treinador Esportivo';
            const crefText = isPEF ? `CREF: ${APP.currentMember.cref} - UF: ${APP.currentMember.state}` : '';
            
            const legalPEF = `⚠️ OBSERVAÇÃO LEGAL – PROFISSIONAL DE EDUCAÇÃO FÍSICA<br>Conforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º da mesma lei estabelece que compete ao profissional de Educação Física coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos especializados nas áreas de atividades físicas e do desporto.`;
            const legalTE = `⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO<br>Conforme a Lei nº 14.597/2023 (Lei Geral do Esporte), Art. 75, a profissão de treinador esportivo é reconhecida e regulada no Brasil, com atuação de caráter técnico e esportivo voltada à preparação, supervisão, orientação e acompanhamento de treinos físicos e esportivos. A atuação possui finalidade orientativa e não substitui avaliação médica, diagnóstico clínico, prescrição medicamentosa ou acompanhamento de profissionais da saúde. Em casos de doenças, lesões, gestação, limitações físicas, uso de medicações ou qualquer condição específica de saúde, recomenda-se avaliação prévia por profissional habilitado antes do início ou continuidade da prática de exercícios. O treinamento proposto respeita os limites individuais, priorizando segurança, execução correta e evolução progressiva, dentro da atuação técnica e esportiva permitida por lei. As recomendações desta ficha possuem caráter informativo e orientativo, voltadas ao treinamento esportivo e prática de musculação.`;

            // Checa validade para carimbo
            let valDays = parseInt(d.stuValidity.match(/\d+/)?.[0] || 30);
            const expDate = new Date(new Date(d.createdAt).getTime() + valDays * 24 * 60 * 60 * 1000);
            const isExpired = new Date() > expDate;
            const stamp = isExpired ? '<div class="expired-stamp">⚠️ FICHA VENCIDA</div>' : '';

            let html = `
                <div class="print-header">
                    <h1 class="print-title">Planilha de Treinamento - ${d.stuObjective} ${stamp}</h1>
                    <div class="prof-info">Prescrição feita por: ${APP.currentMember.name} (${profLabel})<br>${crefText}</div>
                </div>
                <div class="print-grid">
                    <div><strong>Aluno:</strong> ${d.studentName}</div><div><strong>Idade:</strong> ${d.stuAge||'-'}</div><div><strong>Gênero:</strong> ${d.stuGender}</div>
                    <div><strong>Peso:</strong> ${d.stuWeight||'-'} kg</div><div><strong>Altura:</strong> ${d.stuHeight||'-'} m</div><div><strong>IMC:</strong> ${imcStr}</div>
                    <div><strong>Nível:</strong> ${d.stuLevel}</div><div><strong>Freq:</strong> ${d.stuFreq}</div><div><strong>Validade:</strong> ${d.stuValidity.replace('Validade: ','')}</div>
                </div>`;

            if (objTexts[d.stuObjective] || d.health.length > 0) {
                html += `<div class="print-guidelines"><h4>Diretrizes de Perfil</h4><ul>`;
                if(objTexts[d.stuObjective]) html += `<li><strong>Objetivo:</strong> ${objTexts[d.stuObjective]}</li>`;
                d.health.forEach(k => { if(hSource[k]) html += `<li><strong>${k.replace(/[\u{1F300}-\u{1F9FF}\u{2600}-\u{26FF}\u{2700}-\u{27BF}\u{FE0F}]/gu, '').trim()}:</strong> ${hSource[k]}</li>`; });
                html += `</ul></div>`;
            }

            d.workouts.forEach(w => {
                html += `<div class="print-workout"><h3>${w.title}</h3><table>
                    <thead><tr><th style="width: 35%">Exercício</th><th style="width: 8%; text-align:center;">Séries</th><th style="width: 12%; text-align:center;">Reps</th><th style="width: 15%">Técnica</th><th style="width: 30%">Obs</th></tr></thead><tbody>`;
                if(w.exercises.length===0) html += `<tr><td colspan="5" style="text-align:center;font-style:italic;">Sem exercícios</td></tr>`;
                else w.exercises.forEach(ex => html += `<tr><td><strong>${ex.name}</strong></td><td style="text-align:center;">${ex.sets}</td><td style="text-align:center;">${ex.reps}</td><td>${ex.technique!=='Nenhuma'?ex.technique:'-'}</td><td>${ex.obs||'-'}</td></tr>`);
                html += `</tbody></table></div>`;
            });

            if(d.stuRecs.trim()) html += `<div style="font-size:9px; margin-top:10px; border:1px solid #000; padding:5px;"><strong>Recomendações Manuais:</strong><br>${d.stuRecs.replace(/\n/g, '<br>')}</div>`;
            html += `<div class="legal-text">${isPEF ? legalPEF : legalTE}</div>`;

            document.getElementById('print-area').innerHTML = html;
            setTimeout(() => window.print(), 300);
        };

    </script>
</body>
</html>
