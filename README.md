<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro - Sistema de Prescrição</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    
    <style>
        :root {
            /* Tema Padrão Escuro */
            --bg-body: #0f172a; --bg-card: #1e293b; --text-main: #f8fafc; --text-muted: #94a3b8;
            --border-color: #334155; --primary: #3b82f6; --primary-hover: #2563eb; --input-bg: #0f172a; --input-border: #475569;
        }
        [data-theme="Feminino"] {
            /* Tema Feminino */
            --bg-body: #fdf2f8; --bg-card: #ffffff; --text-main: #1e293b; --text-muted: #64748b;
            --border-color: #fbcfe8; --primary: #ec4899; --primary-hover: #db2777; --input-bg: #f8fafc; --input-border: #f1f5f9;
        }

        body { font-family: 'Inter', sans-serif; background-color: var(--bg-body); color: var(--text-main); transition: all 0.3s ease; }
        .card { background-color: var(--bg-card); border: 1px solid var(--border-color); }
        .input-field { background-color: var(--input-bg); border: 1px solid var(--input-border); color: var(--text-main); }
        .input-field:focus { outline: none; border-color: var(--primary); box-shadow: 0 0 0 1px var(--primary); }
        .btn-primary { background-color: var(--primary); color: white; transition: 0.2s; }
        .btn-primary:hover { background-color: var(--primary-hover); }

        /* Esconder tudo na impressão exceto a área de impressão */
        #print-area { display: none; }

        /* ESTILO PLANILHA A4 PARA IMPRESSÃO */
        @media print {
            body { background: white !important; color: black !important; margin: 0; padding: 0; }
            #app-root, .no-print { display: none !important; }
            #print-area { display: block !important; padding: 10mm; font-family: 'Arial', sans-serif; font-size: 10px; }
            @page { size: A4; margin: 0; }
            
            .print-header { border-bottom: 2px solid #000; padding-bottom: 5px; margin-bottom: 10px; text-align: center; }
            .print-title { font-size: 16px; font-weight: bold; margin: 0; text-transform: uppercase; }
            .prof-info { font-size: 11px; margin-top: 4px; font-weight: bold; }
            
            .print-grid { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 4px; margin-bottom: 10px; border: 1px solid #000; padding: 6px; }
            .print-grid div { font-size: 10px; }
            
            .print-guidelines { border: 1px solid #000; padding: 6px; margin-bottom: 10px; font-size: 9px; }
            .print-guidelines h4 { margin: 0 0 4px 0; font-size: 10px; text-transform: uppercase; background: #eaeaea; padding: 2px; }
            .print-guidelines ul { margin: 0; padding-left: 15px; }
            .print-guidelines li { margin-bottom: 2px; }

            .print-workout { margin-bottom: 12px; page-break-inside: avoid; }
            .print-workout h3 { background: #d4d4d4; border: 1px solid #000; border-bottom: none; padding: 4px; margin: 0; font-size: 11px; text-transform: uppercase; -webkit-print-color-adjust: exact; }
            table { width: 100%; border-collapse: collapse; border: 1px solid #000; }
            th, td { border: 1px solid #000; padding: 3px 5px; text-align: left; font-size: 9.5px; }
            th { background-color: #eaeaea; font-weight: bold; text-transform: uppercase; -webkit-print-color-adjust: exact; }
            
            .print-footer-section { margin-top: 10px; border: 1px solid #000; padding: 6px; font-size: 9px; page-break-inside: avoid; }
            .legal-text { font-size: 8px; text-align: justify; margin-top: 5px; font-style: italic; border-top: 1px solid #ccc; padding-top: 4px;}
        }

        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
    </style>
</head>
<body data-theme="Masculino" class="min-h-screen pb-10">

    <div id="app-root">
        <!-- TELA 1: LOGIN -->
        <div id="view-login" class="flex items-center justify-center min-h-screen p-4 hidden">
            <div class="card p-8 rounded-2xl max-w-md w-full text-center shadow-2xl">
                <i class="fas fa-dumbbell text-6xl text-primary mb-4"></i>
                <h1 class="text-3xl font-bold mb-2">PowFit Pro</h1>
                <p class="text-sm var(--text-muted) mb-8">Plataforma Profissional para Treinadores</p>
                <button id="btn-login" class="w-full bg-white text-gray-900 font-bold py-3 px-4 rounded-lg shadow hover:bg-gray-100 transition flex items-center justify-center gap-3">
                    <img src="https://www.svgrepo.com/show/475656/google-color.svg" class="w-5 h-5" alt="Google">
                    Entrar com Conta Google
                </button>
            </div>
        </div>

        <!-- TELA 2: DASHBOARD PRINCIPAL -->
        <div id="view-dashboard" class="max-w-7xl mx-auto p-4 sm:p-6 lg:p-8 hidden">
            <!-- Header App -->
            <div class="flex flex-col sm:flex-row justify-between items-center mb-6 gap-4 border-b border-opacity-20 pb-4" style="border-color: var(--border-color)">
                <div class="flex items-center gap-3">
                    <i class="fas fa-dumbbell text-3xl text-primary"></i>
                    <div>
                        <h1 class="text-2xl font-bold" id="dash-network-name">Minha Rede</h1>
                        <p class="text-xs opacity-70" id="dash-user-email">email@google.com</p>
                    </div>
                </div>
                <div class="flex gap-2">
                    <button onclick="ui.logout()" class="bg-red-500 hover:bg-red-600 text-white px-4 py-2 rounded font-medium shadow text-sm transition">
                        <i class="fas fa-sign-out-alt"></i> Sair
                    </button>
                </div>
            </div>

            <!-- Navegação Tabs -->
            <div class="flex space-x-2 mb-6 overflow-x-auto pb-2">
                <button onclick="ui.switchTab('tab-routines')" id="nav-routines" class="px-4 py-2 rounded-lg font-medium btn-primary whitespace-nowrap">Fichas e Histórico</button>
                <button onclick="ui.switchTab('tab-team')" id="nav-team" class="px-4 py-2 rounded-lg font-medium card whitespace-nowrap hover:text-primary transition">Rede e Equipe</button>
                <button onclick="ui.switchTab('tab-reports')" id="nav-reports" class="px-4 py-2 rounded-lg font-medium card whitespace-nowrap hover:text-primary transition">Relatórios</button>
            </div>

            <!-- TAB: FICHAS E HISTÓRICO -->
            <div id="tab-routines" class="space-y-6">
                <div class="flex justify-between items-center bg-opacity-10 p-4 rounded-xl card border-dashed border-2" style="border-color: var(--border-color)">
                    <div>
                        <h2 class="text-xl font-bold flex items-center gap-2"><i class="fas fa-file-alt text-primary"></i> Gerenciador de Fichas</h2>
                        <p class="text-sm opacity-70">Crie novas prescrições ou consulte o histórico na nuvem.</p>
                    </div>
                    <button onclick="ui.openRoutineBuilder()" class="btn-primary px-5 py-2.5 rounded-lg font-bold flex items-center gap-2 shadow-lg">
                        <i class="fas fa-plus"></i> NOVA FICHA
                    </button>
                </div>

                <div class="card rounded-xl p-5">
                    <h3 class="font-bold mb-4 border-b pb-2 border-opacity-20" style="border-color: var(--border-color)">Histórico de Fichas Salvas</h3>
                    <div id="history-list" class="space-y-3">
                        <!-- Carregado via JS -->
                    </div>
                </div>
            </div>

            <!-- TAB: REDE E EQUIPE -->
            <div id="tab-team" class="hidden space-y-6">
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <!-- Unidades -->
                    <div class="card rounded-xl p-5">
                        <h3 class="font-bold mb-4 border-b pb-2 flex justify-between items-center border-opacity-20" style="border-color: var(--border-color)">
                            Unidades da Rede
                            <button onclick="ui.addUnit()" class="text-primary text-sm hover:underline"><i class="fas fa-plus"></i> Adicionar</button>
                        </h3>
                        <div id="units-list" class="space-y-2 text-sm"></div>
                    </div>
                    <!-- Membros -->
                    <div class="card rounded-xl p-5">
                        <h3 class="font-bold mb-4 border-b pb-2 flex justify-between items-center border-opacity-20" style="border-color: var(--border-color)">
                            Membros da Equipe
                            <button onclick="ui.openMemberModal()" class="text-primary text-sm hover:underline"><i class="fas fa-plus"></i> Adicionar</button>
                        </h3>
                        <div id="members-list" class="space-y-2 text-sm"></div>
                    </div>
                </div>
            </div>

            <!-- TAB: RELATÓRIOS -->
            <div id="tab-reports" class="hidden space-y-6">
                <div class="card rounded-xl p-5">
                    <h3 class="font-bold mb-4 border-b pb-2 border-opacity-20" style="border-color: var(--border-color)">Relatório Mensal de Fichas</h3>
                    <div class="flex gap-4 mb-6">
                        <input type="month" id="report-month" class="input-field rounded px-3 py-2 text-sm" onchange="logic.generateReport()">
                        <button onclick="window.print()" class="btn-primary px-4 py-2 rounded text-sm"><i class="fas fa-print"></i> Imprimir Relatório</button>
                    </div>
                    <table class="w-full text-left text-sm border-collapse" id="report-table">
                        <thead>
                            <tr class="border-b" style="border-color: var(--border-color)">
                                <th class="py-2">Unidade</th>
                                <th class="py-2 text-center">Fichas Geradas</th>
                            </tr>
                        </thead>
                        <tbody id="report-body"></tbody>
                    </table>
                </div>
            </div>
        </div>

        <!-- TELA 3: CRIADOR DE FICHA (NOVA FICHA) -->
        <div id="view-creator" class="max-w-7xl mx-auto p-4 sm:p-6 lg:p-8 hidden">
            <div class="flex justify-between items-center mb-6">
                <button onclick="ui.backToDashboard()" class="text-primary hover:underline flex items-center gap-2"><i class="fas fa-arrow-left"></i> Voltar</button>
                <button onclick="logic.saveAndPrintRoutine()" class="btn-primary px-6 py-2.5 rounded-lg font-bold shadow-lg flex items-center gap-2">
                    <i class="fas fa-save"></i> <i class="fas fa-print"></i> Salvar e Imprimir
                </button>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
                <!-- Coluna Esquerda: Dados -->
                <div class="lg:col-span-4 space-y-6">
                    
                    <div class="card rounded-xl p-5">
                        <h2 class="font-bold mb-3 border-b pb-2 border-opacity-20" style="border-color: var(--border-color)"><i class="fas fa-id-badge text-primary"></i> Profissional Responsável</h2>
                        <select id="select-creator" class="input-field w-full rounded-lg px-3 py-2 text-sm" onchange="ui.updateHealthList()">
                            <!-- Opções JS -->
                        </select>
                    </div>

                    <div class="card rounded-xl p-5">
                        <div class="flex justify-between items-center mb-3 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                            <h2 class="font-bold"><i class="fas fa-user text-primary"></i> Dados do Aluno</h2>
                            <div id="imc-display" class="text-xs font-bold text-primary"></div>
                        </div>
                        <div class="space-y-3">
                            <input type="text" id="stu-name" class="input-field w-full rounded px-3 py-1.5 text-sm" placeholder="Nome Completo">
                            <div class="grid grid-cols-3 gap-2">
                                <input type="number" id="stu-age" class="input-field rounded px-2 py-1.5 text-sm" placeholder="Idade">
                                <input type="number" id="stu-weight" oninput="ui.calcIMC()" class="input-field rounded px-2 py-1.5 text-sm" placeholder="Peso (kg)">
                                <input type="number" id="stu-height" step="0.01" oninput="ui.calcIMC()" class="input-field rounded px-2 py-1.5 text-sm" placeholder="Alt (m)">
                            </div>
                            <div class="grid grid-cols-2 gap-2">
                                <select id="stu-gender" onchange="ui.applyTheme()" class="input-field rounded px-2 py-1.5 text-sm">
                                    <option value="Masculino">Masc.</option><option value="Feminino">Fem.</option>
                                </select>
                                <select id="stu-level" class="input-field rounded px-2 py-1.5 text-sm">
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
                        </div>
                    </div>

                    <div class="card rounded-xl p-5">
                        <h2 class="font-bold mb-2 border-b pb-2 border-opacity-20" style="border-color: var(--border-color)"><i class="fas fa-notes-medical text-primary"></i> Estado de Saúde</h2>
                        <div class="grid grid-cols-2 gap-2 text-[11px]" id="health-container"></div>
                    </div>
                    
                    <div class="card rounded-xl p-5">
                        <h2 class="font-bold mb-2 border-b pb-2 border-opacity-20" style="border-color: var(--border-color)"><i class="fas fa-pen text-primary"></i> Validade & Recomendações</h2>
                        <select id="stu-validity" class="input-field w-full rounded px-2 py-1.5 text-sm mb-2">
                            <option>15 dias</option><option selected>30 dias</option><option>60 dias</option><option>90 dias</option>
                        </select>
                        <textarea id="stu-recs" rows="3" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="Recomendações manuais..."></textarea>
                    </div>
                </div>

                <!-- Coluna Direita: Treinos -->
                <div class="lg:col-span-8 space-y-6">
                    <div class="flex justify-between items-center bg-opacity-10 p-4 rounded-xl card border-dashed border-2">
                        <h2 class="text-lg font-bold"><i class="fas fa-list text-primary"></i> Montagem Manual</h2>
                        <button onclick="logic.addDay()" class="btn-primary px-4 py-2 rounded text-sm font-medium"><i class="fas fa-plus"></i> Adicionar Dia</button>
                    </div>
                    <div id="workouts-container" class="space-y-4"></div>
                </div>
            </div>
        </div>
    </div>

    <!-- MODAL DE EXERCÍCIOS -->
    <div id="exercise-modal" class="fixed inset-0 bg-black bg-opacity-80 hidden z-50 flex items-center justify-center p-2">
        <div class="card w-full max-w-5xl rounded-xl flex flex-col h-[85vh]">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="font-bold text-lg"><i class="fas fa-search text-primary"></i> Banco de Exercícios</h3>
                <button onclick="ui.closeExerciseModal()" class="text-2xl text-gray-500 hover:text-red-500">&times;</button>
            </div>
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <div class="w-full md:w-1/4 border-r overflow-y-auto p-2" style="border-color: var(--border-color)" id="modal-categories"></div>
                <div class="w-full md:w-3/4 overflow-y-auto p-3 bg-black bg-opacity-10" id="modal-exercises"></div>
            </div>
        </div>
    </div>

    <!-- MODAL ADD MEMBRO -->
    <div id="member-modal" class="fixed inset-0 bg-black bg-opacity-80 hidden z-50 flex items-center justify-center p-2">
        <div class="card p-6 rounded-xl w-full max-w-md">
            <h3 class="font-bold text-lg mb-4">Adicionar Membro (Profissional)</h3>
            <div class="space-y-3 text-sm">
                <input type="text" id="mem-name" class="input-field w-full rounded px-3 py-2" placeholder="Nome Completo">
                <select id="mem-role" class="input-field w-full rounded px-3 py-2" onchange="ui.toggleCrefField()">
                    <option value="PEF">Profissional de Educação Física</option>
                    <option value="TE">Treinador Esportivo</option>
                </select>
                <div id="mem-cref-div" class="grid grid-cols-2 gap-2">
                    <input type="text" id="mem-cref" class="input-field rounded px-3 py-2" placeholder="CREF">
                    <input type="text" id="mem-state" class="input-field rounded px-3 py-2" placeholder="Estado (ex: SP)">
                </div>
                <input type="text" id="mem-cpf" class="input-field w-full rounded px-3 py-2" placeholder="CPF">
                <select id="mem-unit" class="input-field w-full rounded px-3 py-2">
                    <!-- Gerado JS -->
                </select>
            </div>
            <div class="flex justify-end gap-2 mt-6">
                <button onclick="document.getElementById('member-modal').classList.add('hidden')" class="px-4 py-2 rounded border border-gray-500 hover:bg-gray-800 transition">Cancelar</button>
                <button onclick="logic.saveMember()" class="btn-primary px-4 py-2 rounded">Salvar Membro</button>
            </div>
        </div>
    </div>

    <!-- ÁREA DE IMPRESSÃO -->
    <div id="print-area"></div>

    <!-- FIREBASE & LOGIC SCIPTS -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-app.js";
        import { getAuth, signInWithPopup, GoogleAuthProvider, signOut, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-auth.js";
        import { getFirestore, collection, doc, setDoc, getDoc, getDocs, addDoc, query, where, deleteDoc } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-firestore.js";

        // Configuração fornecida pelo usuário
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

        // --- BANCO DE DADOS LOCAL E ESTADOS ---
        const DB_DATA = {
            healthPEF: {
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
            },
            healthTE: {
                "🟢 Saudável": "Manter treinos regulares de 3–5x por semana, combinando musculação e atividades cardiovasculares. A constância, descanso adequado e boa alimentação contribuem para melhores resultados.",
                "⚪ Sedentário": "O início deve ser gradual, com treinos leves e foco na adaptação do corpo. A prioridade é desenvolver constância, aprender a execução correta e evitar excesso de carga.",
                "🟡 Sobrepeso": "A prática regular de musculação associada ao cardio pode contribuir para melhora do condicionamento físico e composição corporal. A progressão deve respeitar a individualidade.",
                "🔴 Obesidade": "É recomendado iniciar com exercícios de menor impacto e progressão gradual. A segurança, mobilidade e constância são prioridades durante o processo.",
                "⚖️ Baixo peso": "A musculação pode auxiliar no ganho de massa muscular quando associada a alimentação adequada e descanso. O treino deve priorizar evolução progressiva.",
                "🍬 Diabetes": "Pessoas com diabetes devem manter acompanhamento médico regular. Em casos de tontura, mal-estar ou alteração de glicemia, o treino deve ser interrompido.",
                "❤️ Hipertensão": "Manter acompanhamento médico regular e respeitar as orientações profissionais. Evitar prender a respiração e controlar a intensidade.",
                "🔵 Hipotensão": "Evitar mudanças bruscas de posição durante o treino. Manter boa hidratação e respeitar a intensidade adequada.",
                "💔 Problemas cardíacos": "A prática de exercícios deve ocorrer apenas com liberação e acompanhamento médico. O treino deve respeitar limites individuais.",
                "🦴 Problemas articulares": "Exercícios com menor impacto e maior controle de movimento costumam ser mais indicados. O acompanhamento profissional ajuda na segurança.",
                "🫁 Problemas respiratórios": "A progressão do treino deve ser gradual e respeitar a capacidade respiratória individual. Em caso de falta de ar excessiva, interromper.",
                "⚠️ Lesões": "A adaptação dos exercícios deve respeitar a limitação existente e evitar dor durante a execução. Seguir orientação profissional adequada.",
                "🤰 Gestante": "Prática com liberação médica e acompanhamento. Foco deve ser segurança, mobilidade e bem-estar, evitando impacto excessivo.",
                "🤱 Lactante": "Atividade mantida normalmente, respeitando a recuperação individual, mantendo boa hidratação e alimentação adequada.",
                "👴 Idoso": "A musculação contribui para força, equilíbrio, mobilidade e autonomia. O treino deve respeitar limitações individuais, com segurança."
            },
            objectives: {
                "Emagrecimento": "Foco em déficit calórico com treinos mistos de força (manter massa magra) e aeróbicos (maior gasto).",
                "Hipertrofia": "Prioridade na progressão de carga e volume adequado. Essencial superávit calórico e descanso.",
                "Definição": "Manutenção de massa muscular enquanto reduz o percentual de gordura. Atenção estrita à dieta.",
                "Condicionamento": "Treinos com menor tempo de intervalo, circuitos e alta integração cardiopulmonar.",
                "Resistência": "Séries mais longas, cadência controlada e aprimoramento da capacidade muscular.",
                "Força": "Cargas altas, baixas repetições e intervalos de descanso maiores.",
                "Reabilitação": "Treino focado em fortalecimento específico, mobilidade e controle motor. Respeitar limites de dor.",
                "Saúde geral": "Equilíbrio entre força, cardio e flexibilidade. O principal objetivo é a constância e bem-estar."
            },
            categories: {
                "🔥 PEITO": ["Supino Reto", "Supino Inclinado", "Supino Inclinado com Halteres", "Supino Fechado com Halteres", "Cross Over", "Cross Over Alto", "Cross Over Baixo", "Crucifixo Reto", "Crucifixo Inclinado com Halteres", "Crucifixo na Máquina", "Peck Fly", "Peck Fly Unilateral", "Pullover", "Flexão de Braço", "Flexão com Pés Elevados", "Flexão Explosiva"],
                "🦍 COSTAS": ["Puxada Alta", "Puxada de Frente Supinada", "Pulldown", "Remada Aberta", "Remada Baixa", "Remada Curvada", "Remada Curvada Supinada", "Remada Cavalinho (T-Bar)", "Serrote", "Facepull (puxada de cima para baixo)"],
                "🦵 PERNAS": ["Agachamento Livre", "Agachamento Taça", "Agachamento no Smith", "Agachamento com passada lateral", "Squat", "Hack Machine", "Leg 45°", "Leg 90°", "Agachamento Sumô", "Agachamento Sissy (Livre)", "Afundo", "Recuo", "Avanço", "Passada", "Búlgaro", "Step-up", "Levantamento Terra", "Levantamento Terra Romeno", "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", "Cadeira Flexora", "Elevação Pélvica no Banco", "Elevação Pélvica no Chão", "Elevação Pélvica Unilateral no Chão", "Extensão de Quadril (Glúteo Máximo)", "Extensão Cruzada (Glúteo Médio)", "Coice", "Cachorrinho", "Cadeira Extensora", "Adução", "Abdução", "Abdução Inclinada", "Flexão Nórdica", "Flexão Nórdica Invertida", "Panturrilha Livre", "Panturrilha no Leg Press", "Panturrilha Banco", "Panturrilha Squat", "Panturrilha Unilateral"],
                "💪 BRAÇOS": ["(Bíceps) Rosca Direta", "(Bíceps) Rosca Alternada", "(Bíceps) Rosca 21", "(Bíceps) Rosca Scott Barra W", "(Bíceps) Rosca Scott Unilateral", "(Bíceps) Rosca Scott com Halteres", "(Bíceps) Rosca Martelo", "(Bíceps) Rosca Cross", "(Bíceps) Rosca Inversa", "(Bíceps) Rosca 45°", "(Tríceps) Pulley Unilateral", "(Tríceps) Pulley Barra", "(Tríceps) Pulley Corda", "(Tríceps) Pulley Pegada Inversa", "(Tríceps) Francês na Corda", "(Tríceps) Francês com Halter", "(Tríceps) Francês Unilateral", "(Tríceps) Cruzado Polia Dupla", "(Tríceps) Coice Unilateral", "(Tríceps) Arremesso", "(Tríceps) Testa", "(Tríceps) Mergulho no Banco"],
                "🪨 OMBROS": ["Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", "Elevação Lateral Unilateral Cross", "Elevação Lateral Sentado", "Desenvolvimento com Halteres", "Desenvolvimento com Barra", "Arnold Press", "Elevação Borboleta", "Crucifixo Inverso Sentado com Halteres", "Crucifixo Inverso na Polia", "Crucifixo Inverso Unilateral na Polia", "Facepull (puxada reta)", "Remada Alta", "Encolhimento (Trapézio)"],
                "🧠 ABDÔMEN": ["Infra com Elevação de Perna", "Abdominal Supra", "Abdominal Remador", "Abdominal Bicicleta", "Abdominal Twister com Peso", "Prancha", "Prancha Lateral", "Trituração de Cabos em Pé", "Isometria na parede", "Abdominal isometrico"],
                "🫀 CARDIO": ["Bicicleta 10 Minutos", "Bicicleta 15 Minutos", "Bicicleta 20 Minutos", "Esteira 10 Minutos", "Esteira 15 Minutos", "Esteira 20 Minutos", "Pular Corda."]
            },
            techniques: ["Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", "Negativa", "Isometria", "Parciais", "Pirâmide"],
            days: ["SEGUNDA-FEIRA", "TERÇA-FEIRA", "QUARTA-FEIRA", "QUINTA-FEIRA", "SEXTA-FEIRA", "SÁBADO", "DOMINGO"]
        };

        const state = {
            user: null,
            networkName: null,
            units: [],
            members: [],
            routines: [], // Histórico atual carregado
            
            // Ficha atual
            currentRoutineId: null,
            workouts: [],
            activeModalDayId: null,
            activeCategory: "🔥 PEITO"
        };

        // --- SISTEMA UI E EVENTOS GLOBAIS ---
        window.ui = {
            show: (id) => document.getElementById(id).classList.remove('hidden'),
            hide: (id) => document.getElementById(id).classList.add('hidden'),
            switchTab: (tabId) => {
                ['tab-routines', 'tab-team', 'tab-reports'].forEach(t => ui.hide(t));
                ['nav-routines', 'nav-team', 'nav-reports'].forEach(b => {
                    document.getElementById(b).classList.replace('btn-primary', 'card');
                });
                ui.show(tabId);
                document.getElementById('nav-' + tabId.split('-')[1]).classList.replace('card', 'btn-primary');
                if(tabId === 'tab-reports') logic.generateReport();
            },
            openRoutineBuilder: () => {
                if(state.members.length === 0) {
                    alert("Por favor, adicione pelo menos um Profissional/Membro na aba 'Rede e Equipe' primeiro.");
                    return;
                }
                state.currentRoutineId = logic.generateId();
                state.workouts = [];
                logic.addDay(); // Adiciona 1 dia vazio
                ui.clearForm();
                ui.updateHealthList();
                ui.hide('view-dashboard');
                ui.show('view-creator');
            },
            backToDashboard: () => {
                ui.hide('view-creator');
                ui.show('view-dashboard');
            },
            calcIMC: () => {
                const w = parseFloat(document.getElementById('stu-weight').value);
                const h = parseFloat(document.getElementById('stu-height').value);
                if (w > 0 && h > 0) {
                    const imc = (w / (h * h)).toFixed(1);
                    document.getElementById('imc-display').innerText = `IMC: ${imc}`;
                } else {
                    document.getElementById('imc-display').innerText = '';
                }
            },
            applyTheme: () => {
                document.body.setAttribute('data-theme', document.getElementById('stu-gender').value);
            },
            toggleCrefField: () => {
                const role = document.getElementById('mem-role').value;
                if(role === 'TE') ui.hide('mem-cref-div');
                else ui.show('mem-cref-div');
            },
            openMemberModal: () => {
                if(state.units.length === 0) {
                    alert("Você precisa cadastrar uma Unidade primeiro!");
                    return;
                }
                document.getElementById('mem-unit').innerHTML = state.units.map(u => `<option value="${u.id}">${u.name}</option>`).join('');
                document.getElementById('mem-name').value = '';
                document.getElementById('mem-cref').value = '';
                document.getElementById('mem-state').value = '';
                document.getElementById('mem-cpf').value = '';
                ui.show('member-modal');
            },
            updateHealthList: () => {
                const memberId = document.getElementById('select-creator').value;
                const member = state.members.find(m => m.id === memberId);
                const role = member ? member.role : 'PEF';
                const healthDict = role === 'PEF' ? DB_DATA.healthPEF : DB_DATA.healthTE;
                
                const container = document.getElementById('health-container');
                const selected = Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value);
                
                container.innerHTML = Object.keys(healthDict).map(opt => `
                    <label class="flex items-start space-x-1 cursor-pointer">
                        <input type="checkbox" value="${opt}" ${selected.includes(opt)?'checked':''} class="health-cb mt-0.5 rounded text-primary w-3 h-3">
                        <span>${opt}</span>
                    </label>`).join('');
            },
            addUnit: async () => {
                const name = prompt("Nome da Unidade (ex: Centro, Sul):");
                if(!name) return;
                try {
                    const docRef = await addDoc(collection(db, `users/${state.user.uid}/units`), { name });
                    state.units.push({ id: docRef.id, name });
                    logic.renderTeam();
                } catch(e) { console.error(e); alert("Erro ao salvar no Firebase."); }
            },
            openExerciseModal: (dayId) => {
                state.activeModalDayId = dayId;
                ui.show('exercise-modal');
                ui.renderCategories();
                ui.renderExercises();
            },
            closeExerciseModal: () => { ui.hide('exercise-modal'); state.activeModalDayId = null; },
            renderCategories: () => {
                document.getElementById('modal-categories').innerHTML = Object.keys(DB_DATA.categories).map(cat => `
                    <button onclick="ui.setCategory('${cat}')" class="w-full text-left px-2 py-2 rounded text-xs font-bold ${state.activeCategory === cat ? 'bg-primary text-white' : 'hover:bg-black hover:bg-opacity-10'}">
                        ${cat}
                    </button>`).join('');
            },
            setCategory: (cat) => { state.activeCategory = cat; ui.renderCategories(); ui.renderExercises(); },
            renderExercises: () => {
                const ex = DB_DATA.categories[state.activeCategory];
                const isCardio = state.activeCategory === "🫀 CARDIO";
                document.getElementById('modal-exercises').innerHTML = `<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-2">
                    ${ex.map(e => `<button onclick="logic.addExercise('${e}', ${isCardio})" class="card p-2 rounded text-left text-xs font-medium border hover:border-primary transition">${e}</button>`).join('')}
                </div>`;
            },
            clearForm: () => {
                document.getElementById('stu-name').value = '';
                document.getElementById('stu-age').value = '';
                document.getElementById('stu-weight').value = '';
                document.getElementById('stu-height').value = '';
                document.getElementById('stu-recs').value = '';
                document.getElementById('imc-display').innerText = '';
                document.querySelectorAll('.health-cb').forEach(cb => cb.checked = false);
            },
            logout: () => {
                signOut(auth).then(() => {
                    ui.hide('view-dashboard'); ui.hide('view-creator'); ui.show('view-login');
                });
            }
        };

        // --- LÓGICA CORE ---
        window.logic = {
            generateId: () => Math.random().toString(36).substr(2, 9),
            
            // TEAM (Firebase)
            saveMember: async () => {
                const member = {
                    name: document.getElementById('mem-name').value,
                    role: document.getElementById('mem-role').value,
                    cpf: document.getElementById('mem-cpf').value,
                    cref: document.getElementById('mem-cref').value,
                    state: document.getElementById('mem-state').value,
                    unitId: document.getElementById('mem-unit').value
                };
                if(!member.name || !member.cpf) return alert("Preencha campos obrigatórios");
                
                try {
                    const docRef = await addDoc(collection(db, `users/${state.user.uid}/members`), member);
                    member.id = docRef.id;
                    state.members.push(member);
                    ui.hide('member-modal');
                    logic.renderTeam();
                } catch(e) { console.error(e); alert("Erro ao salvar Membro"); }
            },
            renderTeam: () => {
                const uList = document.getElementById('units-list');
                uList.innerHTML = state.units.length === 0 ? '<div class="opacity-50 italic">Nenhuma unidade</div>' :
                    state.units.map(u => `<div class="p-2 border rounded border-gray-600 border-opacity-30">🏢 ${u.name}</div>`).join('');
                
                const mList = document.getElementById('members-list');
                mList.innerHTML = state.members.length === 0 ? '<div class="opacity-50 italic">Nenhum membro</div>' :
                    state.members.map(m => {
                        const unitName = state.units.find(u => u.id === m.unitId)?.name || 'Desconhecida';
                        const prof = m.role === 'PEF' ? `PEF (CREF: ${m.cref})` : 'Treinador Esportivo';
                        return `<div class="p-2 border rounded border-gray-600 border-opacity-30 flex justify-between">
                            <div><strong>${m.name}</strong><br><span class="text-xs opacity-70">${prof} | CPF: ${m.cpf}</span></div>
                            <div class="text-xs self-center bg-black bg-opacity-20 px-2 py-1 rounded">${unitName}</div>
                        </div>`;
                    }).join('');

                // Preenche o seletor da Nova Ficha
                const selectCreator = document.getElementById('select-creator');
                selectCreator.innerHTML = state.members.map(m => `<option value="${m.id}">${m.name} (${m.role})</option>`).join('');
            },

            // ROUTINE BUILDER
            addDay: () => {
                const count = state.workouts.length;
                const title = count < 7 ? `TREINO ${DB_DATA.days[count]}` : `NOVO TREINO ${count+1}`;
                state.workouts.push({ id: logic.generateId(), title, exercises: [] });
                logic.renderWorkouts();
            },
            removeDay: (id) => {
                state.workouts = state.workouts.filter(w => w.id !== id);
                logic.renderWorkouts();
            },
            updateDayTitle: (id, val) => {
                const w = state.workouts.find(w => w.id === id);
                if(w) w.title = val;
            },
            addExercise: (name, isCardio) => {
                const w = state.workouts.find(w => w.id === state.activeModalDayId);
                if(w) {
                    w.exercises.push({ category: state.activeCategory, name, sets: isCardio ? '1' : '3', reps: isCardio ? '-' : '10-12', technique: 'Nenhuma', obs: '' });
                    logic.renderWorkouts();
                    const container = document.getElementById('modal-exercises');
                    container.style.opacity = '0.5'; setTimeout(() => container.style.opacity = '1', 100);
                }
            },
            removeExercise: (wId, exIdx) => {
                state.workouts.find(w => w.id === wId).exercises.splice(exIdx, 1);
                logic.renderWorkouts();
            },
            moveExercise: (wId, exIdx, dir) => {
                const w = state.workouts.find(w => w.id === wId);
                if (dir === 'up' && exIdx > 0) {
                    [w.exercises[exIdx], w.exercises[exIdx-1]] = [w.exercises[exIdx-1], w.exercises[exIdx]];
                } else if (dir === 'down' && exIdx < w.exercises.length - 1) {
                    [w.exercises[exIdx], w.exercises[exIdx+1]] = [w.exercises[exIdx+1], w.exercises[exIdx]];
                }
                logic.renderWorkouts();
            },
            updateExercise: (wId, exIdx, field, val) => {
                state.workouts.find(w => w.id === wId).exercises[exIdx][field] = val;
            },
            renderWorkouts: () => {
                document.getElementById('workouts-container').innerHTML = state.workouts.map(w => {
                    let exHtml = w.exercises.length === 0 ? '<div class="p-3 text-center text-xs opacity-50">Sem exercícios</div>' :
                        w.exercises.map((ex, idx) => `
                        <div class="flex flex-col sm:flex-row gap-2 p-2 border-b border-opacity-10 items-center text-sm" style="border-color: var(--border-color)">
                            <div class="flex gap-1">
                                <button onclick="logic.moveExercise('${w.id}', ${idx}, 'up')" class="p-1"><i class="fas fa-chevron-up text-[10px]"></i></button>
                                <button onclick="logic.moveExercise('${w.id}', ${idx}, 'down')" class="p-1"><i class="fas fa-chevron-down text-[10px]"></i></button>
                            </div>
                            <div class="flex-1 w-full"><div class="text-[9px] opacity-60">${ex.category}</div><div class="font-bold">${ex.name}</div></div>
                            <input type="text" value="${ex.sets}" onchange="logic.updateExercise('${w.id}', ${idx}, 'sets', this.value)" class="input-field w-10 text-center rounded px-1 py-1" placeholder="Séries">
                            <span class="opacity-50">x</span>
                            <input type="text" value="${ex.reps}" onchange="logic.updateExercise('${w.id}', ${idx}, 'reps', this.value)" class="input-field w-14 text-center rounded px-1 py-1" placeholder="Reps">
                            <select onchange="logic.updateExercise('${w.id}', ${idx}, 'technique', this.value)" class="input-field w-24 rounded px-1 py-1 text-[10px]">
                                ${DB_DATA.techniques.map(t => `<option ${ex.technique===t?'selected':''}>${t}</option>`).join('')}
                            </select>
                            <input type="text" value="${ex.obs}" onchange="logic.updateExercise('${w.id}', ${idx}, 'obs', this.value)" class="input-field flex-1 rounded px-2 py-1" placeholder="Obs">
                            <button onclick="logic.removeExercise('${w.id}', ${idx})" class="text-red-500 p-1"><i class="fas fa-trash"></i></button>
                        </div>`).join('');

                    return `
                    <div class="card rounded-xl overflow-hidden shadow-sm">
                        <div class="p-3 border-b flex justify-between bg-black bg-opacity-5" style="border-color: var(--border-color)">
                            <input type="text" value="${w.title}" onchange="logic.updateDayTitle('${w.id}', this.value)" class="input-field bg-transparent font-bold text-sm w-1/2 px-1 uppercase">
                            <button onclick="logic.removeDay('${w.id}')" class="text-red-500 text-xs px-2 hover:underline">Excluir</button>
                        </div>
                        <div>${exHtml}</div>
                        <div class="p-2 text-center border-t" style="border-color: var(--border-color)">
                            <button onclick="ui.openExerciseModal('${w.id}')" class="text-primary text-xs font-bold w-full py-1">+ ADICIONAR EXERCÍCIO</button>
                        </div>
                    </div>`;
                }).join('');
            },

            // SAVING & PRINTING
            saveAndPrintRoutine: async () => {
                const memberId = document.getElementById('select-creator').value;
                const member = state.members.find(m => m.id === memberId);
                if(!member) return alert("Selecione um profissional.");

                const stuName = document.getElementById('stu-name').value;
                if(!stuName) return alert("Insira o nome do aluno.");

                const now = new Date();
                const routineData = {
                    memberId,
                    unitId: member.unitId,
                    dateStr: now.toLocaleDateString('pt-BR'),
                    monthYear: `${now.getMonth()+1}/${now.getFullYear()}`,
                    timestamp: now.getTime(),
                    student: {
                        name: stuName,
                        age: document.getElementById('stu-age').value,
                        weight: document.getElementById('stu-weight').value,
                        height: document.getElementById('stu-height').value,
                        gender: document.getElementById('stu-gender').value,
                        level: document.getElementById('stu-level').value,
                        objective: document.getElementById('stu-objective').value,
                        freq: document.getElementById('stu-freq').value,
                        validity: document.getElementById('stu-validity').value,
                        recs: document.getElementById('stu-recs').value,
                        health: Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value)
                    },
                    workouts: state.workouts
                };

                // Salva no Firestore
                try {
                    await setDoc(doc(db, `users/${state.user.uid}/routines`, state.currentRoutineId), routineData);
                    logic.loadRoutines(); // Atualiza histórico
                    logic.executePrint(routineData, member);
                } catch(e) {
                    console.error("Erro Firebase", e);
                    alert("Erro ao salvar na nuvem. A ficha será impressa mesmo assim.");
                    logic.executePrint(routineData, member);
                }
            },

            executePrint: (data, member) => {
                const p = document.getElementById('print-area');
                const s = data.student;
                
                let imcStr = "-";
                if(s.weight > 0 && s.height > 0) imcStr = (s.weight / (s.height * s.height)).toFixed(1);

                const isPEF = member.role === 'PEF';
                const healthDict = isPEF ? DB_DATA.healthPEF : DB_DATA.healthTE;
                const objText = DB_DATA.objectives[s.objective] || "";

                // Header
                let html = `
                <div class="print-header">
                    <h1 class="print-title">PLANILHA DE TREINAMENTO</h1>
                    <div class="prof-info">
                        Prescrição feita por ${isPEF ? 'Personal Trainer' : 'Treinador Esportivo'}: ${member.name}<br>
                        ${isPEF ? `CREF: ${member.cref} - Estado: ${member.state}` : ''}
                    </div>
                </div>
                <div class="print-grid">
                    <div><strong>Aluno:</strong> ${s.name}</div>
                    <div><strong>Idade:</strong> ${s.age || '-'} anos</div>
                    <div><strong>Gênero:</strong> ${s.gender}</div>
                    <div><strong>Peso:</strong> ${s.weight || '-'} kg</div>
                    <div><strong>Altura:</strong> ${s.height || '-'} m</div>
                    <div><strong>IMC:</strong> ${imcStr}</div>
                    <div><strong>Objetivo:</strong> ${s.objective}</div>
                    <div><strong>Nível:</strong> ${s.level}</div>
                    <div><strong>Frequência:</strong> ${s.freq}</div>
                </div>`;

                // Diretrizes
                if(objText || s.health.length > 0) {
                    html += `<div class="print-guidelines"><h4>Diretrizes de Saúde e Objetivo</h4><ul>`;
                    if(objText) html += `<li><strong>${s.objective}:</strong> ${objText}</li>`;
                    s.health.forEach(h => {
                        const cleanKey = h.replace(/[\u{1F300}-\u{1F9FF}\u{2600}-\u{26FF}\u{2700}-\u{27BF}\u{FE0F}]/gu, '').trim();
                        if(healthDict[h]) html += `<li><strong>${cleanKey}:</strong> ${healthDict[h]}</li>`;
                    });
                    html += `</ul></div>`;
                }

                // Treinos Tabela
                data.workouts.forEach(w => {
                    html += `<div class="print-workout"><h3>${w.title}</h3><table>
                        <thead><tr><th style="width: 35%">Exercício</th><th style="width: 10%">Séries</th><th style="width: 10%">Reps</th><th style="width: 15%">Técnica</th><th style="width: 30%">Observações</th></tr></thead><tbody>`;
                    if(w.exercises.length === 0) {
                        html += `<tr><td colspan="5" style="text-align:center;font-style:italic">Descanso / Sem exercícios</td></tr>`;
                    } else {
                        w.exercises.forEach(ex => {
                            html += `<tr>
                                <td><strong>${ex.name}</strong></td>
                                <td>${ex.sets}</td>
                                <td>${ex.reps}</td>
                                <td>${ex.technique === 'Nenhuma' ? '-' : ex.technique}</td>
                                <td>${ex.obs}</td>
                            </tr>`;
                        });
                    }
                    html += `</tbody></table></div>`;
                });

                // Footer Legal
                const legalPEF = "⚠️ OBSERVAÇÃO LEGAL – PROFISSIONAL DE EDUCAÇÃO FÍSICA: Conforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º estabelece que compete ao profissional coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos especializados nas áreas de atividades físicas e do desporto.";
                const legalTE = "⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO: Conforme a Lei nº 14.597/2023 (Lei Geral do Esporte), Art. 75, a profissão de treinador esportivo é reconhecida e regulada no Brasil, com atuação de caráter técnico e esportivo. As recomendações desta ficha possuem caráter informativo e orientativo, voltadas ao treinamento esportivo e prática de musculação. Este material não substitui avaliação médica ou acompanhamento de profissionais da saúde. Em casos de dores, lesões, doenças, gestação ou qualquer condição específica, recomenda-se procurar um profissional habilitado antes de iniciar ou continuar os treinos.";

                html += `<div class="print-footer-section">`;
                if(s.recs) html += `<strong>Recomendações do Profissional:</strong><p style="white-space:pre-wrap;margin:3px 0">${s.recs}</p>`;
                html += `<div class="legal-text">${isPEF ? legalPEF : legalTE}</div>
                         <div style="text-align:right; margin-top:10px; font-weight:bold;">Válido por: ${s.validity.replace('Validade: ','')} | Data: ${data.dateStr}</div>
                         </div>`;

                p.innerHTML = html;
                setTimeout(() => window.print(), 300);
            },

            // LOAD DATA
            loadNetwork: async () => {
                const docSnap = await getDoc(doc(db, "users", state.user.uid));
                if(docSnap.exists()) {
                    state.networkName = docSnap.data().networkName;
                } else {
                    const netName = prompt("Bem-vindo! Qual o nome da sua Rede / Empresa?");
                    if(netName) {
                        await setDoc(doc(db, "users", state.user.uid), { networkName: netName });
                        state.networkName = netName;
                    }
                }
                document.getElementById('dash-network-name').innerText = state.networkName || "Minha Rede";
            },
            loadUnitsAndMembers: async () => {
                const qU = query(collection(db, `users/${state.user.uid}/units`));
                const snapU = await getDocs(qU);
                state.units = snapU.docs.map(d => ({id: d.id, ...d.data()}));

                const qM = query(collection(db, `users/${state.user.uid}/members`));
                const snapM = await getDocs(qM);
                state.members = snapM.docs.map(d => ({id: d.id, ...d.data()}));
                
                logic.renderTeam();
            },
            loadRoutines: async () => {
                const q = query(collection(db, `users/${state.user.uid}/routines`));
                const snap = await getDocs(q);
                // Ordenar por timestamp decrescente
                state.routines = snap.docs.map(d => ({id: d.id, ...d.data()})).sort((a,b) => b.timestamp - a.timestamp);
                
                const list = document.getElementById('history-list');
                if(state.routines.length === 0) {
                    list.innerHTML = '<div class="opacity-50 text-sm">Nenhuma ficha salva na nuvem.</div>';
                    return;
                }
                
                list.innerHTML = state.routines.map(r => {
                    const prof = state.members.find(m => m.id === r.memberId)?.name || 'Desconhecido';
                    return `<div class="p-3 border rounded border-gray-600 border-opacity-30 flex justify-between items-center text-sm">
                        <div><strong>Aluno: ${r.student.name}</strong><br><span class="text-xs opacity-70">Data: ${r.dateStr} | Prof: ${prof}</span></div>
                        <div class="flex gap-2">
                            <button onclick="logic.loadRoutineToEdit('${r.id}')" class="text-primary hover:underline">Carregar</button>
                            <button onclick="logic.deleteRoutine('${r.id}')" class="text-red-500 hover:underline">Excluir</button>
                        </div>
                    </div>`;
                }).join('');
            },
            loadRoutineToEdit: (id) => {
                const r = state.routines.find(r => r.id === id);
                if(!r) return;
                
                state.currentRoutineId = r.id;
                document.getElementById('select-creator').value = r.memberId;
                ui.updateHealthList();
                
                document.getElementById('stu-name').value = r.student.name;
                document.getElementById('stu-age').value = r.student.age;
                document.getElementById('stu-weight').value = r.student.weight;
                document.getElementById('stu-height').value = r.student.height;
                document.getElementById('stu-gender').value = r.student.gender;
                document.getElementById('stu-level').value = r.student.level;
                document.getElementById('stu-objective').value = r.student.objective;
                document.getElementById('stu-freq').value = r.student.freq;
                document.getElementById('stu-validity').value = r.student.validity;
                document.getElementById('stu-recs').value = r.student.recs;
                
                ui.calcIMC();
                ui.applyTheme();
                
                // Set health boxes
                document.querySelectorAll('.health-cb').forEach(cb => {
                    cb.checked = (r.student.health || []).includes(cb.value);
                });

                state.workouts = r.workouts || [];
                logic.renderWorkouts();
                
                ui.hide('view-dashboard');
                ui.show('view-creator');
            },
            deleteRoutine: async (id) => {
                if(confirm("Excluir permanentemente da nuvem?")) {
                    await deleteDoc(doc(db, `users/${state.user.uid}/routines`, id));
                    logic.loadRoutines();
                }
            },
            generateReport: () => {
                const monthVal = document.getElementById('report-month').value; // YYYY-MM
                if(!monthVal) return;
                
                const [year, month] = monthVal.split('-');
                const targetStr = `${parseInt(month)}/${year}`;
                
                const filtered = state.routines.filter(r => r.monthYear === targetStr);
                
                // Agrupar por Unidade
                const counts = {};
                state.units.forEach(u => counts[u.id] = { name: u.name, total: 0 });
                filtered.forEach(r => {
                    if(counts[r.unitId]) counts[r.unitId].total++;
                });

                const tbody = document.getElementById('report-body');
                tbody.innerHTML = Object.values(counts).map(c => `
                    <tr class="border-b border-opacity-10" style="border-color: var(--border-color)">
                        <td class="py-2">${c.name}</td>
                        <td class="py-2 text-center font-bold text-primary">${c.total}</td>
                    </tr>
                `).join('');
            }
        };

        // --- AUTH E INICIALIZAÇÃO ---
        document.getElementById('btn-login').addEventListener('click', () => {
            const provider = new GoogleAuthProvider();
            signInWithPopup(auth, provider).catch(error => {
                console.error(error); alert("Falha no Login: " + error.message);
            });
        });

        onAuthStateChanged(auth, async (user) => {
            if (user) {
                state.user = user;
                document.getElementById('dash-user-email').innerText = user.email;
                ui.hide('view-login');
                ui.show('view-dashboard');
                
                // Pré-preenche mês atual no relatório
                const now = new Date();
                const m = (now.getMonth() + 1).toString().padStart(2, '0');
                document.getElementById('report-month').value = `${now.getFullYear()}-${m}`;

                try {
                    await logic.loadNetwork();
                    await logic.loadUnitsAndMembers();
                    await logic.loadRoutines();
                } catch(e) {
                    console.error("Erro ao conectar no Firestore:", e);
                    alert("Atenção: Banco de dados Firebase indisponível ou permissões negadas. Verifique as configurações no console do Firebase.");
                }

            } else {
                state.user = null;
                ui.hide('view-dashboard');
                ui.hide('view-creator');
                ui.show('view-login');
            }
        });

    </script>
</body>
</html>
