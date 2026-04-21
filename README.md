<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro - Master (Rede & Franquias)</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    
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

        /* Esconder abas */
        .view-section { display: none; }
        .view-section.active { display: block; }
        #print-area { display: none; }

        /* Estilo Tabela Planilha (Modo Impressão) */
        @media print {
            body { background: white !important; color: black !important; margin: 0; padding: 0; }
            #app-container, .modal, .no-print { display: none !important; }
            #print-area { display: block !important; padding: 10mm; font-family: 'Arial', sans-serif; font-size: 10px; }
            @page { size: A4; margin: 0; }
            
            .print-header { border-bottom: 2px solid #000; padding-bottom: 5px; margin-bottom: 10px; text-align: center; }
            .print-title { font-size: 16px; font-weight: 900; margin: 0; text-transform: uppercase; }
            .prof-info { font-size: 11px; margin-top: 3px; font-weight: bold; }
            
            .print-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 2px; margin-bottom: 10px; border: 1px solid #000; padding: 5px; }
            .print-grid div { font-size: 10px; }
            
            .print-guidelines { border: 1px solid #000; padding: 5px; margin-bottom: 10px; font-size: 8.5px; }
            .print-guidelines h4 { margin: 0 0 3px 0; font-size: 9px; background: #eee; padding: 2px; text-transform: uppercase; }
            .print-guidelines ul { margin: 0; padding-left: 12px; }
            .print-guidelines li { margin-bottom: 2px; }

            .print-workout { margin-bottom: 10px; page-break-inside: avoid; }
            .print-workout h3 { background: #d1d5db; border: 1px solid #000; border-bottom: none; padding: 3px 5px; margin: 0; font-size: 11px; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            table { width: 100%; border-collapse: collapse; border: 1px solid #000; }
            th, td { border: 1px solid #000; padding: 3px 4px; text-align: left; font-size: 9.5px; }
            th { background-color: #f3f4f6; font-weight: bold; -webkit-print-color-adjust: exact; color-adjust: exact; }
            
            .legal-footer { margin-top: 10px; border: 1px solid #000; padding: 5px; font-size: 7.5px; text-align: justify; page-break-inside: avoid; }
        }

        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
    </style>
</head>
<body data-theme="Masculino">

    <!-- LOGIN SCREEN -->
    <div id="login-view" class="min-h-screen flex items-center justify-center p-4">
        <div class="card p-8 rounded-2xl shadow-2xl max-w-md w-full text-center border-t-4 border-t-primary">
            <i class="fas fa-dumbbell text-5xl text-primary mb-4"></i>
            <h1 class="text-3xl font-black mb-2">PowFit Pro</h1>
            <p class="text-sm opacity-70 mb-8">Plataforma Master para Redes & Franquias</p>
            <button id="btn-login" class="btn-primary w-full py-3 rounded-lg font-bold flex items-center justify-center gap-3 text-lg shadow-lg">
                <i class="fab fa-google"></i> Acessar Sistema
            </button>
            <p class="text-xs opacity-50 mt-4"><i class="fas fa-cloud"></i> Sincronização em Nuvem (Firebase)</p>
        </div>
    </div>

    <!-- MAIN APP CONTAINER -->
    <div id="app-container" class="hidden min-h-screen flex flex-col">
        
        <!-- Navbar -->
        <nav class="card border-b px-4 sm:px-8 py-3 flex flex-wrap justify-between items-center gap-4 sticky top-0 z-40">
            <div class="flex items-center gap-3">
                <i class="fas fa-dumbbell text-2xl text-primary"></i>
                <span class="text-xl font-bold tracking-tight">PowFit Pro</span>
            </div>
            <div class="flex flex-wrap gap-2 text-sm font-medium">
                <button onclick="switchView('ficha')" class="px-4 py-2 rounded hover:bg-black hover:bg-opacity-10 transition"><i class="fas fa-plus"></i> Nova Ficha</button>
                <button onclick="switchView('rede')" class="px-4 py-2 rounded hover:bg-black hover:bg-opacity-10 transition"><i class="fas fa-building"></i> Rede & Equipe</button>
                <button onclick="switchView('historico')" class="px-4 py-2 rounded hover:bg-black hover:bg-opacity-10 transition"><i class="fas fa-cloud"></i> Histórico</button>
                <button onclick="switchView('relatorio')" class="px-4 py-2 rounded hover:bg-black hover:bg-opacity-10 transition"><i class="fas fa-chart-bar"></i> Relatórios</button>
            </div>
        </nav>

        <!-- VIEW: REDE E EQUIPE (FRANQUIAS) -->
        <div id="view-rede" class="view-section flex-1 p-4 sm:p-8 max-w-5xl mx-auto w-full">
            <h2 class="text-2xl font-bold mb-6"><i class="fas fa-building text-primary"></i> Gestão da Rede de Franquias</h2>
            
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <!-- Unidades -->
                <div class="card p-5 rounded-xl">
                    <h3 class="text-lg font-bold mb-4 border-b border-opacity-20 pb-2">1. Unidades da Rede</h3>
                    <div class="flex gap-2 mb-4">
                        <input type="text" id="new-unit-name" class="input-field flex-1 rounded px-3 py-2 text-sm" placeholder="Nome da Unidade (Ex: Matriz)">
                        <button onclick="addUnit()" class="btn-primary px-4 rounded text-sm"><i class="fas fa-plus"></i></button>
                    </div>
                    <div id="units-list" class="space-y-2 max-h-60 overflow-y-auto"></div>
                </div>

                <!-- Membros da Equipe -->
                <div class="card p-5 rounded-xl opacity-50 pointer-events-none transition-all" id="members-panel">
                    <h3 class="text-lg font-bold mb-4 border-b border-opacity-20 pb-2">2. Equipe da Unidade <span id="selected-unit-label" class="text-primary"></span></h3>
                    
                    <div class="space-y-3 mb-4">
                        <input type="text" id="new-mem-name" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="Nome Completo">
                        <div class="grid grid-cols-2 gap-2">
                            <input type="text" id="new-mem-cpf" class="input-field rounded px-3 py-2 text-sm" placeholder="CPF">
                            <input type="text" id="new-mem-uf" class="input-field rounded px-3 py-2 text-sm" placeholder="UF (Ex: SP)">
                        </div>
                        <select id="new-mem-cat" class="input-field w-full rounded px-3 py-2 text-sm" onchange="toggleCrefInput()">
                            <option value="PEF">Profissional de Educação Física</option>
                            <option value="TE">Treinador Esportivo</option>
                        </select>
                        <input type="text" id="new-mem-cref" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="CREF (Obrigatório para PEF)">
                        <button onclick="addMember()" class="btn-primary w-full py-2 rounded text-sm font-bold">Adicionar Membro</button>
                    </div>
                    <div id="members-list" class="space-y-2 max-h-40 overflow-y-auto"></div>
                </div>
            </div>
        </div>

        <!-- VIEW: NOVA FICHA -->
        <div id="view-ficha" class="view-section active flex-1 p-4 sm:p-8 max-w-7xl mx-auto w-full">
            
            <!-- Seleção do Profissional que está prescrevendo -->
            <div class="card p-4 rounded-xl mb-6 border-l-4 border-l-primary flex flex-col sm:flex-row justify-between items-center gap-4">
                <div>
                    <h3 class="font-bold text-lg">Quem está montando este treino?</h3>
                    <p class="text-xs opacity-70">A ficha será assinada digitalmente por este membro e contabilizada em sua unidade.</p>
                </div>
                <select id="current-prescriber" class="input-field rounded-lg px-4 py-2 text-sm w-full sm:w-1/3 font-medium" onchange="updateHealthGuidelines()">
                    <option value="">-- Selecione o Membro --</option>
                </select>
            </div>

            <div class="grid grid-cols-1 xl:grid-cols-12 gap-6" id="ficha-workspace" style="opacity: 0.5; pointer-events: none;">
                
                <!-- Coluna Esquerda: Aluno e Saúde -->
                <div class="xl:col-span-4 space-y-6">
                    <div class="card rounded-xl p-5 shadow-sm">
                        <div class="flex justify-between items-center mb-4 border-b border-opacity-20 pb-2">
                            <h2 class="text-lg font-bold"><i class="fas fa-user text-primary"></i> Dados do Aluno</h2>
                            <div id="imc-display"></div>
                        </div>
                        <div class="space-y-4">
                            <input type="text" id="stu-name" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Nome do Aluno">
                            <div class="grid grid-cols-3 gap-3">
                                <input type="number" id="stu-age" class="input-field rounded-lg px-3 py-2 text-sm" placeholder="Idade">
                                <input type="number" id="stu-weight" oninput="calcIMC()" class="input-field rounded-lg px-3 py-2 text-sm" placeholder="Peso (kg)">
                                <input type="number" id="stu-height" step="0.01" oninput="calcIMC()" class="input-field rounded-lg px-3 py-2 text-sm" placeholder="Alt (m)">
                            </div>
                            <div class="grid grid-cols-2 gap-3">
                                <select id="stu-gender" onchange="changeTheme()" class="input-field rounded-lg px-3 py-2 text-sm">
                                    <option value="Masculino">Masculino</option>
                                    <option value="Feminino">Feminino</option>
                                </select>
                                <select id="stu-level" class="input-field rounded-lg px-3 py-2 text-sm">
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

                    <div class="card rounded-xl p-5 shadow-sm">
                        <h2 class="text-lg font-bold mb-2 border-b border-opacity-20 pb-2"><i class="fas fa-notes-medical text-primary"></i> Estado de Saúde</h2>
                        <p class="text-[10px] opacity-70 mb-3 leading-tight text-primary font-bold" id="health-mode-label">Diretrizes automáticas (Aguardando seleção do membro)</p>
                        <div class="grid grid-cols-2 gap-2 text-[11px]" id="health-container"></div>
                    </div>
                </div>

                <!-- Coluna Direita: Treinos -->
                <div class="xl:col-span-8 space-y-6">
                    <div class="flex flex-col sm:flex-row justify-between items-center bg-opacity-10 p-4 rounded-xl card border-dashed border-2 gap-4">
                        <div class="flex items-center gap-3">
                            <select id="stu-validity" class="input-field rounded px-2 py-1 text-xs font-bold">
                                <option>Validade: 15 dias</option><option selected>Validade: 30 dias</option>
                                <option>Validade: 60 dias</option><option>Validade: 90 dias</option>
                            </select>
                        </div>
                        <div class="flex gap-2 w-full sm:w-auto">
                            <button onclick="addWorkoutDay()" class="btn-primary flex-1 sm:flex-none px-4 py-2 rounded-lg text-sm font-bold shadow"><i class="fas fa-plus"></i> Dia de Treino</button>
                            <button onclick="saveAndPrint()" class="bg-green-600 hover:bg-green-500 text-white flex-1 sm:flex-none px-4 py-2 rounded-lg text-sm font-bold shadow"><i class="fas fa-print"></i> Salvar & Imprimir</button>
                        </div>
                    </div>

                    <div id="workouts-container" class="space-y-6"></div>

                    <div class="card rounded-xl p-5">
                        <h2 class="text-sm font-bold mb-2"><i class="fas fa-pen"></i> Recomendações Livres</h2>
                        <textarea id="stu-recs" rows="3" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Hidratação, descanso, cuidados..."></textarea>
                    </div>
                </div>
            </div>
        </div>

        <!-- VIEW: HISTÓRICO -->
        <div id="view-historico" class="view-section flex-1 p-4 sm:p-8 max-w-5xl mx-auto w-full">
            <h2 class="text-2xl font-bold mb-6"><i class="fas fa-cloud text-primary"></i> Fichas Salvas (Nuvem)</h2>
            <div id="history-list" class="space-y-3"></div>
        </div>

        <!-- VIEW: RELATÓRIOS -->
        <div id="view-relatorio" class="view-section flex-1 p-4 sm:p-8 max-w-5xl mx-auto w-full">
            <div class="flex justify-between items-center mb-6">
                <h2 class="text-2xl font-bold"><i class="fas fa-chart-pie text-primary"></i> Produtividade Mensal</h2>
                <button onclick="window.print()" class="btn-primary px-4 py-2 rounded shadow"><i class="fas fa-print"></i> Imprimir Relatório</button>
            </div>
            <div class="card p-6 rounded-xl">
                <p class="text-sm opacity-70 mb-4">Contagem de fichas geradas por Unidade e por Membro neste mês.</p>
                <div id="report-content" class="space-y-6"></div>
            </div>
        </div>
    </div>

    <!-- MODAL DE EXERCÍCIOS -->
    <div id="exercise-modal" class="fixed inset-0 bg-black bg-opacity-80 backdrop-blur-sm hidden z-[60] flex items-center justify-center p-2 sm:p-4">
        <div class="card w-full max-w-6xl rounded-xl flex flex-col h-[90vh]">
            <div class="p-4 border-b flex justify-between items-center">
                <h3 class="text-lg font-bold"><i class="fas fa-search text-primary"></i> Adicionar Exercício</h3>
                <button onclick="closeModal()" class="text-gray-400 hover:text-red-500 text-2xl">&times;</button>
            </div>
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <div class="w-full md:w-1/4 border-r overflow-y-auto p-2 space-y-1" id="modal-categories"></div>
                <div class="w-full md:w-3/4 overflow-y-auto p-3 flex flex-col">
                    <div class="flex gap-2 mb-4">
                        <input type="text" id="custom-ex-input" class="input-field flex-1 rounded px-3 py-1 text-sm" placeholder="Novo exercício personalizado para esta categoria...">
                        <button onclick="addCustomExercise()" class="btn-primary px-4 rounded text-sm">Salvar na Nuvem</button>
                    </div>
                    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-2" id="modal-exercises"></div>
                </div>
            </div>
        </div>
    </div>

    <!-- ÁREA DE IMPRESSÃO -->
    <div id="print-area"></div>

    <!-- FIREBASE & LOGIC -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getAuth, signInAnonymously, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, addDoc, query, getDocs, deleteDoc, serverTimestamp, orderBy } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        // --- FIREBASE SETUP ---
        // Seguindo estritamente as regras de ambiente para funcionamento no preview (Anonymous/Custom Token)
        let app, auth, db, userId, appId;
        
        const initFirebase = async () => {
            try {
                // Tenta configuração do ambiente seguro (Canvas)
                const config = JSON.parse(window.__firebase_config);
                appId = typeof window.__app_id !== 'undefined' ? window.__app_id : 'default-powfit';
                app = initializeApp(config);
            } catch (e) {
                // Fallback para Configuração fornecida pelo usuário no prompt
                const fallbackConfig = {
                    apiKey: "AIzaSyD-iuERHA_x1f69smJXh7b8wTLO1zTadIU",
                    authDomain: "powfitpro-d8577.firebaseapp.com",
                    projectId: "powfitpro-d8577",
                    storageBucket: "powfitpro-d8577.firebasestorage.app",
                    messagingSenderId: "651381183985",
                    appId: "1:651381183985:web:9a425692372a9d67d9e050"
                };
                appId = 'powfit-pro-local';
                app = initializeApp(fallbackConfig);
            }

            auth = getAuth(app);
            db = getFirestore(app);

            // Listener de Auth
            onAuthStateChanged(auth, async (user) => {
                if (user) {
                    userId = user.uid;
                    document.getElementById('login-view').classList.add('hidden');
                    document.getElementById('app-container').classList.remove('hidden');
                    await loadNetworkData();
                    await loadCustomExercises();
                }
            });
        };

        initFirebase();

        // Login Simulado (No Canvas, forçamos Anônimo para evitar bloqueio de popup. Em prod, trocar para GoogleAuthProvider)
        document.getElementById('btn-login').addEventListener('click', async () => {
            document.getElementById('btn-login').innerHTML = `<i class="fas fa-spinner fa-spin"></i> Conectando...`;
            try {
                if (typeof window.__initial_auth_token !== 'undefined' && window.__initial_auth_token) {
                    const { signInWithCustomToken } = await import("https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js");
                    await signInWithCustomToken(auth, window.__initial_auth_token);
                } else {
                    await signInAnonymously(auth);
                }
            } catch (error) {
                alert("Erro ao conectar: " + error.message);
                document.getElementById('btn-login').innerHTML = `<i class="fab fa-google"></i> Tentar Novamente`;
            }
        });


        // --- DADOS ESTÁTICOS ---
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
            "🍬 Diabetes": "Pessoas com diabetes devem manter acompanhamento médico regular. Em casos de tontura, mal-estar ou alteração de glicemia, o treino deve ser interrompido e o aluno deve procurar orientação médica.",
            "❤️ Hipertensão": "Pessoas com hipertensão devem manter acompanhamento médico regular e respeitar as orientações profissionais. Evitar prender a respiração e controlar a intensidade.",
            "🔵 Hipotensão": "Pessoas com hipotensão devem evitar mudanças bruscas de posição durante o treino. Manter boa hidratação e respeitar a intensidade adequada.",
            "💔 Problemas cardíacos": "A prática de exercícios deve ocorrer apenas com liberação e acompanhamento médico. O treino deve respeitar limites individuais.",
            "🦴 Problemas articulares": "Exercícios com menor impacto e maior controle de movimento costumam ser mais indicados. O acompanhamento profissional ajuda na segurança.",
            "🫁 Problemas respiratórios": "A progressão do treino deve ser gradual e respeitar a capacidade respiratória individual. Interromper em caso de falta de ar.",
            "⚠️ Lesões": "A adaptação dos exercícios deve respeitar a limitação existente e evitar dor. Seguir orientação profissional adequada antes da prática.",
            "🤰 Gestante": "Prática com liberação médica. Foco na segurança, mobilidade e bem-estar, evitando impacto excessivo e situações de risco.",
            "🤱 Lactante": "Atividade mantida normalmente, respeitando a recuperação individual e mantendo boa hidratação e alimentação.",
            "👴 Idoso": "A musculação contribui para força e mobilidade. Respeitar limitações, com intensidade moderada e foco na segurança."
        };

        const objData = {
            "Emagrecimento": "Foco em déficit calórico com treinos mistos de força (manter massa magra) e aeróbicos (maior gasto).",
            "Hipertrofia": "Prioridade na progressão de carga e volume adequado. Essencial superávit calórico e descanso.",
            "Definição": "Manutenção de massa muscular enquanto reduz o percentual de gordura. Atenção estrita à dieta.",
            "Condicionamento": "Treinos com menor tempo de intervalo, circuitos e alta integração cardiopulmonar.",
            "Resistência": "Séries mais longas, cadência controlada e aprimoramento da capacidade muscular.",
            "Força": "Cargas altas, baixas repetições e intervalos de descanso maiores.",
            "Reabilitação": "Treino focado em fortalecimento específico, mobilidade e controle motor. Respeitar limites.",
            "Saúde geral": "Equilíbrio entre força, cardio e flexibilidade. O principal objetivo é a constância e bem-estar."
        };

        const coreExercises = {
            "🔥 PEITO": ["Supino Reto", "Supino Inclinado", "Supino Inclinado com Halteres", "Supino Fechado com Halteres", "Cross Over", "Cross Over Alto", "Cross Over Baixo", "Crucifixo Reto", "Crucifixo Inclinado com Halteres", "Crucifixo na Máquina", "Peck Fly", "Peck Fly Unilateral", "Pullover", "Flexão de Braço", "Flexão com Pés Elevados", "Flexão Explosiva"],
            "🦍 COSTAS": ["Puxada Alta", "Puxada de Frente Supinada", "Pulldown", "Remada Aberta", "Remada Baixa", "Remada Curvada", "Remada Curvada Supinada", "Remada Cavalinho (T-Bar)", "Serrote", "Facepull (puxada de cima para baixo)"],
            "🦵 PERNAS": ["Agachamento Livre", "Agachamento Taça", "Agachamento no Smith", "Agachamento com passada lateral", "Squat", "Hack Machine", "Leg 45°", "Leg 90°", "Agachamento Sumô", "Agachamento Sissy (Livre)", "Afundo", "Recuo", "Avanço", "Passada", "Búlgaro", "Step-up", "Levantamento Terra", "Levantamento Terra Romeno", "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", "Cadeira Flexora", "Elevação Pélvica no Banco", "Elevação Pélvica no Chão", "Elevação Pélvica Unilateral no Chão", "Extensão de Quadril (Glúteo Máximo)", "Extensão Cruzada (Glúteo Médio)", "Coice", "Cachorrinho", "Cadeira Extensora", "Adução", "Abdução", "Abdução Inclinada", "Flexão Nórdica", "Flexão Nórdica Invertida", "Panturrilha Livre", "Panturrilha no Leg Press", "Panturrilha Banco", "Panturrilha Squat", "Panturrilha Unilateral"],
            "💪 BÍCEPS": ["Rosca Direta", "Rosca Alternada", "Rosca 21", "Rosca Scott Barra W", "Rosca Scott Unilateral", "Rosca Scott com Halteres", "Rosca Martelo", "Rosca Cross", "Rosca Inversa", "Rosca 45°"],
            "💪 TRÍCEPS": ["Triceps Pulley Unilateral", "Tríceps Pulley Barra", "Tríceps Pulley Corda", "Tríceps Pulley Pegada Inversa", "Tríceps Francês na Corda", "Tríceps Francês com Halter", "Tríceps Francês Unilateral", "Tríceps Cruzado Polia Dupla", "Tríceps Coice Unilateral", "Tríceps Arremesso", "Tríceps Testa", "Mergulho no Banco"],
            "🪨 OMBROS": ["Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", "Elevação Lateral Unilateral Cross", "Elevação Lateral Sentado", "Desenvolvimento com Halteres", "Desenvolvimento com Barra", "Arnold Press", "Elevação Borboleta", "Crucifixo Inverso Sentado com Halteres", "Crucifixo Inverso na Polia", "Crucifixo Inverso Unilateral na Polia", "Facepull (puxada reta)", "Remada Alta", "Encolhimento (Trapézio)"],
            "🧠 ABDÔMEN": ["Infra com Elevação de Perna", "Abdominal Supra", "Abdominal Remador", "Abdominal Bicicleta", "Abdominal Twister com Peso", "Prancha", "Prancha Lateral", "Trituração de Cabos em Pé", "Isometria na parede"],
            "🫀 CARDIO": ["Bicicleta 10 Minutos", "Bicicleta 15 Minutos", "Bicicleta 20 Minutos", "Esteira 10 Minutos", "Esteira 15 Minutos", "Esteira 20 Minutos", "Pular Corda"]
        };
        const techniques = ["Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", "Negativa", "Isometria", "Parciais", "Pirâmide"];
        const daysOrder = ["SEGUNDA-FEIRA", "TERÇA-FEIRA", "QUARTA-FEIRA", "QUINTA-FEIRA", "SEXTA-FEIRA", "SÁBADO", "DOMINGO"];

        // --- ESTADO GLOBAL ---
        window.state = {
            network: { units: [], members: [] },
            customExercises: {}, // Mesclado com coreExercises
            currentSheet: { id: null, workouts: [] },
            activeUnitId: null,
            activeModalWorkoutId: null,
            activeCategory: "🔥 PEITO"
        };

        // --- UTILITÁRIOS ---
        const genId = () => Math.random().toString(36).substr(2, 9);
        const getDocPath = (col) => doc(db, 'artifacts', appId, 'users', userId, 'powfit', col);

        // --- GESTÃO DE REDE (FRANQUIAS) ---
        async function loadNetworkData() {
            const snap = await getDoc(getDocPath('network'));
            if (snap.exists()) {
                state.network = snap.data();
            } else {
                state.network = { units: [], members: [] };
            }
            renderUnits();
            updatePrescriberSelect();
        }

        async function saveNetworkData() {
            await setDoc(getDocPath('network'), state.network);
            updatePrescriberSelect();
        }

        window.addUnit = async () => {
            const name = document.getElementById('new-unit-name').value.trim();
            if(!name) return;
            state.network.units.push({ id: genId(), name });
            document.getElementById('new-unit-name').value = '';
            await saveNetworkData();
            renderUnits();
        };

        window.deleteUnit = async (id) => {
            if(!confirm("Excluir unidade e seus membros?")) return;
            state.network.units = state.network.units.filter(u => u.id !== id);
            state.network.members = state.network.members.filter(m => m.unitId !== id);
            if(state.activeUnitId === id) { state.activeUnitId = null; document.getElementById('members-panel').classList.add('opacity-50', 'pointer-events-none'); }
            await saveNetworkData();
            renderUnits();
        };

        window.selectUnit = (id, name) => {
            state.activeUnitId = id;
            document.getElementById('selected-unit-label').innerText = `- ${name}`;
            document.getElementById('members-panel').classList.remove('opacity-50', 'pointer-events-none');
            renderMembers();
        };

        window.toggleCrefInput = () => {
            const isPEF = document.getElementById('new-mem-cat').value === 'PEF';
            document.getElementById('new-mem-cref').style.display = isPEF ? 'block' : 'none';
        };

        window.addMember = async () => {
            if(!state.activeUnitId) return;
            const name = document.getElementById('new-mem-name').value.trim();
            const cpf = document.getElementById('new-mem-cpf').value.trim();
            const uf = document.getElementById('new-mem-uf').value.trim();
            const cat = document.getElementById('new-mem-cat').value;
            const cref = cat === 'PEF' ? document.getElementById('new-mem-cref').value.trim() : '';
            
            if(!name || !cpf || !uf) return alert("Preencha Nome, CPF e UF.");
            
            state.network.members.push({ id: genId(), unitId: state.activeUnitId, name, cpf, uf, cat, cref });
            document.getElementById('new-mem-name').value = '';
            document.getElementById('new-mem-cpf').value = '';
            await saveNetworkData();
            renderMembers();
        };

        window.deleteMember = async (id) => {
            if(!confirm("Excluir membro?")) return;
            state.network.members = state.network.members.filter(m => m.id !== id);
            await saveNetworkData();
            renderMembers();
        };

        function renderUnits() {
            const c = document.getElementById('units-list');
            c.innerHTML = state.network.units.map(u => `
                <div class="flex justify-between p-2 bg-black bg-opacity-20 rounded border border-gray-600 cursor-pointer hover:border-primary transition" onclick="selectUnit('${u.id}', '${u.name}')">
                    <span class="font-bold">${u.name}</span>
                    <button onclick="event.stopPropagation(); deleteUnit('${u.id}')" class="text-red-500"><i class="fas fa-trash"></i></button>
                </div>
            `).join('');
        }

        function renderMembers() {
            const c = document.getElementById('members-list');
            const members = state.network.members.filter(m => m.unitId === state.activeUnitId);
            c.innerHTML = members.map(m => `
                <div class="flex justify-between p-2 bg-black bg-opacity-20 rounded border border-gray-600 text-sm">
                    <div>
                        <div class="font-bold text-primary">${m.name}</div>
                        <div class="text-[10px] opacity-70">${m.cat === 'PEF' ? 'PEF - CREF: '+m.cref : 'Treinador Esportivo'} | UF: ${m.uf}</div>
                    </div>
                    <button onclick="deleteMember('${m.id}')" class="text-red-500"><i class="fas fa-trash"></i></button>
                </div>
            `).join('');
        }

        function updatePrescriberSelect() {
            const sel = document.getElementById('current-prescriber');
            sel.innerHTML = `<option value="">-- Cadastre membros na aba Rede --</option>`;
            if(state.network.members.length === 0) return;
            sel.innerHTML = `<option value="">-- Selecione o Membro --</option>` + state.network.members.map(m => {
                const unit = state.network.units.find(u => u.id === m.unitId);
                return `<option value="${m.id}">${m.name} (${unit ? unit.name : '?'}) - ${m.cat}</option>`;
            }).join('');
        }

        // --- FUNCIONALIDADES DA FICHA ---
        window.switchView = (vId) => {
            document.querySelectorAll('.view-section').forEach(el => el.classList.remove('active'));
            document.getElementById(`view-${vId}`).classList.add('active');
            if(vId === 'historico') loadHistory();
            if(vId === 'relatorio') generateReport();
        };

        window.calcIMC = () => {
            const w = parseFloat(document.getElementById('stu-weight').value);
            const h = parseFloat(document.getElementById('stu-height').value);
            const d = document.getElementById('imc-display');
            if (w > 0 && h > 0) {
                const imc = (w / (h * h)).toFixed(1);
                d.innerHTML = `<span class="bg-primary bg-opacity-20 text-primary px-3 py-1 rounded-full text-xs font-bold border border-primary border-opacity-30">IMC: ${imc}</span>`;
            } else d.innerHTML = '';
        };

        window.changeTheme = () => {
            document.body.setAttribute('data-theme', document.getElementById('stu-gender').value);
        };

        window.updateHealthGuidelines = () => {
            const mId = document.getElementById('current-prescriber').value;
            const container = document.getElementById('health-container');
            const label = document.getElementById('health-mode-label');
            const ws = document.getElementById('ficha-workspace');
            
            if(!mId) {
                container.innerHTML = '';
                label.innerText = 'Diretrizes automáticas (Aguardando seleção do membro)';
                ws.style.opacity = '0.5'; ws.style.pointerEvents = 'none';
                return;
            }
            
            ws.style.opacity = '1'; ws.style.pointerEvents = 'auto';
            const member = state.network.members.find(m => m.id === mId);
            const dataObj = member.cat === 'PEF' ? healthDataPEF : healthDataTE;
            label.innerText = `Diretrizes Ativas para: ${member.cat === 'PEF' ? 'Profissional Ed. Física' : 'Treinador Esportivo'}`;
            
            const selected = Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value);
            container.innerHTML = Object.keys(dataObj).map(opt => `
                <label class="flex items-start space-x-1.5 cursor-pointer hover:text-primary transition">
                    <input type="checkbox" value="${opt}" ${selected.includes(opt)?'checked':''} class="health-cb mt-0.5">
                    <span>${opt}</span>
                </label>
            `).join('');
        };

        window.addWorkoutDay = () => {
            const title = daysOrder[state.currentSheet.workouts.length] || `NOVO TREINO ${state.currentSheet.workouts.length+1}`;
            state.currentSheet.workouts.push({ id: genId(), title, exercises: [] });
            renderWorkoutSheet();
        };

        window.removeWorkoutDay = (id) => {
            state.currentSheet.workouts = state.currentSheet.workouts.filter(w => w.id !== id);
            renderWorkoutSheet();
        };
        
        window.updateWTitle = (id, val) => { state.currentSheet.workouts.find(w => w.id === id).title = val; };
        
        window.moveEx = (wId, eIdx, dir) => {
            const w = state.currentSheet.workouts.find(x => x.id === wId);
            if(dir === 'up' && eIdx > 0) [w.exercises[eIdx], w.exercises[eIdx-1]] = [w.exercises[eIdx-1], w.exercises[eIdx]];
            if(dir === 'down' && eIdx < w.exercises.length-1) [w.exercises[eIdx], w.exercises[eIdx+1]] = [w.exercises[eIdx+1], w.exercises[eIdx]];
            renderWorkoutSheet();
        };
        window.rmEx = (wId, eIdx) => { state.currentSheet.workouts.find(x => x.id === wId).exercises.splice(eIdx, 1); renderWorkoutSheet(); };
        window.updEx = (wId, eIdx, field, val) => { state.currentSheet.workouts.find(x => x.id === wId).exercises[eIdx][field] = val; };

        function renderWorkoutSheet() {
            const c = document.getElementById('workouts-container');
            c.innerHTML = state.currentSheet.workouts.map(w => `
                <div class="card rounded-xl overflow-hidden">
                    <div class="p-3 border-b flex justify-between items-center bg-black bg-opacity-10 border-[var(--border-color)]">
                        <input type="text" value="${w.title}" onchange="updateWTitle('${w.id}', this.value)" class="input-field bg-transparent font-bold text-sm w-2/3 px-2 py-1 uppercase">
                        <button onclick="removeWorkoutDay('${w.id}')" class="text-red-500 hover:bg-red-500 hover:text-white px-2 py-1 rounded text-xs"><i class="fas fa-trash"></i></button>
                    </div>
                    <div class="p-1">
                        ${w.exercises.length === 0 ? '<div class="text-center text-xs opacity-50 p-4">Sem exercícios.</div>' : 
                            w.exercises.map((ex, idx) => `
                            <div class="flex flex-wrap gap-2 p-2 border-b border-[var(--border-color)] border-opacity-30 items-center text-xs">
                                <button onclick="moveEx('${w.id}', ${idx}, 'up')"><i class="fas fa-chevron-up"></i></button>
                                <button onclick="moveEx('${w.id}', ${idx}, 'down')"><i class="fas fa-chevron-down"></i></button>
                                <div class="flex-1 min-w-[150px]"><span class="text-[9px] text-primary block">${ex.category}</span><span class="font-bold">${ex.name}</span></div>
                                <input type="text" value="${ex.sets}" onchange="updEx('${w.id}',${idx},'sets',this.value)" class="input-field w-10 text-center rounded py-1" placeholder="S">
                                <input type="text" value="${ex.reps}" onchange="updEx('${w.id}',${idx},'reps',this.value)" class="input-field w-16 text-center rounded py-1" placeholder="Reps">
                                <select onchange="updEx('${w.id}',${idx},'technique',this.value)" class="input-field w-24 rounded py-1">${techniques.map(t=>`<option ${ex.technique===t?'selected':''}>${t}</option>`).join('')}</select>
                                <input type="text" value="${ex.obs}" onchange="updEx('${w.id}',${idx},'obs',this.value)" class="input-field flex-1 min-w-[100px] rounded py-1 px-2" placeholder="Obs">
                                <button onclick="rmEx('${w.id}', ${idx})" class="text-red-500"><i class="fas fa-times"></i></button>
                            </div>
                        `).join('')}
                    </div>
                    <button onclick="openModal('${w.id}')" class="w-full text-primary font-bold text-xs py-2 hover:bg-black hover:bg-opacity-20 transition">+ EXERCÍCIO</button>
                </div>
            `).join('');
        }

        // --- MODAL DE EXERCÍCIOS E CUSTOMIZAÇÃO NUvem ---
        async function loadCustomExercises() {
            const snap = await getDoc(getDocPath('customExercises'));
            if(snap.exists()) state.customExercises = snap.data();
        }

        window.openModal = (wId) => { state.activeModalWorkoutId = wId; document.getElementById('exercise-modal').classList.remove('hidden'); renderModalCats(); };
        window.closeModal = () => { document.getElementById('exercise-modal').classList.add('hidden'); };
        
        window.setModalCat = (c) => { state.activeCategory = c; renderModalCats(); };
        
        function renderModalCats() {
            const c = document.getElementById('modal-categories');
            c.innerHTML = Object.keys(coreExercises).map(cat => `<button onclick="setModalCat('${cat}')" class="w-full text-left p-2 rounded text-xs font-bold mb-1 ${state.activeCategory===cat?'bg-primary text-white':'hover:bg-gray-800'}">${cat}</button>`).join('');
            renderModalExs();
        }

        function renderModalExs() {
            const c = document.getElementById('modal-exercises');
            const core = coreExercises[state.activeCategory] || [];
            const custom = state.customExercises[state.activeCategory] || [];
            
            c.innerHTML = core.map(ex => `<button onclick="addEx('${ex}')" class="card p-2 rounded text-xs text-left border border-[var(--border-color)] hover:border-primary"><span>${ex}</span></button>`).join('') +
                          custom.map((ex, i) => `<div class="card p-2 rounded text-xs flex justify-between items-center border border-green-500"><button class="text-left flex-1" onclick="addEx('${ex}')">${ex} <span class="text-[8px] bg-green-500 text-black px-1 rounded">CUSTOM</span></button><button onclick="delCustomEx('${ex}')" class="text-red-500 ml-2"><i class="fas fa-trash"></i></button></div>`).join('');
        }

        window.addEx = (name) => {
            const w = state.currentSheet.workouts.find(x => x.id === state.activeModalWorkoutId);
            const isCardio = state.activeCategory.includes('CARDIO');
            w.exercises.push({ category: state.activeCategory, name, sets: isCardio?'1':'3', reps: isCardio?'-':'10 a 12', technique: 'Nenhuma', obs: '' });
            renderWorkoutSheet();
        };

        window.addCustomExercise = async () => {
            const val = document.getElementById('custom-ex-input').value.trim();
            if(!val) return;
            if(!state.customExercises[state.activeCategory]) state.customExercises[state.activeCategory] = [];
            state.customExercises[state.activeCategory].push(val);
            await setDoc(getDocPath('customExercises'), state.customExercises);
            document.getElementById('custom-ex-input').value = '';
            renderModalExs();
        };

        window.delCustomEx = async (name) => {
            if(!confirm("Deletar exercício customizado da nuvem?")) return;
            state.customExercises[state.activeCategory] = state.customExercises[state.activeCategory].filter(x => x !== name);
            await setDoc(getDocPath('customExercises'), state.customExercises);
            renderModalExs();
        };

        // --- SALVAR, HISTÓRICO E IMPRESSÃO ---
        window.saveAndPrint = async () => {
            const mId = document.getElementById('current-prescriber').value;
            if(!mId) return alert("Selecione um membro prescrevedor no topo da ficha.");
            const member = state.network.members.find(m => m.id === mId);
            
            const data = {
                memberId: member.id,
                memberName: member.name,
                unitId: member.unitId,
                cat: member.cat,
                cref: member.cref,
                uf: member.uf,
                date: new Date().toISOString(),
                student: document.getElementById('stu-name').value || 'Sem Nome',
                age: document.getElementById('stu-age').value,
                weight: document.getElementById('stu-weight').value,
                height: document.getElementById('stu-height').value,
                gender: document.getElementById('stu-gender').value,
                level: document.getElementById('stu-level').value,
                objective: document.getElementById('stu-objective').value,
                freq: document.getElementById('stu-freq').value,
                validityDays: parseInt(document.getElementById('stu-validity').value.replace(/\D/g,'')) || 30,
                health: Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value),
                recs: document.getElementById('stu-recs').value,
                workouts: state.currentSheet.workouts
            };

            // Salva no Firestore
            try {
                if(!state.currentSheet.id) {
                    const docRef = await addDoc(collection(db, 'artifacts', appId, 'users', userId, 'powfit_workouts'), data);
                    state.currentSheet.id = docRef.id;
                } else {
                    await setDoc(doc(db, 'artifacts', appId, 'users', userId, 'powfit_workouts', state.currentSheet.id), data);
                }
            } catch(e) { console.error("Erro ao salvar:", e); }

            // Gera HTML Impressão
            let imc = "-";
            if(data.weight && data.height) imc = (data.weight / (data.height*data.height)).toFixed(1);
            
            const isPEF = data.cat === 'PEF';
            const legalText = isPEF ? 
                `⚠️ OBSERVAÇÃO LEGAL – PROFISSIONAL DE EDUCAÇÃO FÍSICA<br>Conforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º da mesma lei estabelece que compete ao profissional de Educação Física coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos especializados nas áreas de atividades físicas e do desporto.` : 
                `⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO<br>Conforme a Lei nº 14.597/2023 (Lei Geral do Esporte), Art. 75, a profissão de treinador esportivo é reconhecida e regulada no Brasil, com atuação de caráter técnico e esportivo voltada à preparação, supervisão, orientação e acompanhamento de treinos físicos e esportivos. A atuação possui finalidade orientativa e não substitui avaliação médica, diagnóstico clínico, prescrição medicamentosa ou acompanhamento de profissionais da saúde. Em casos de doenças, lesões, gestação, limitações físicas, uso de medicações ou qualquer condição específica de saúde, recomenda-se avaliação prévia por profissional habilitado antes do início ou continuidade da prática de exercícios. O treinamento proposto respeita os limites individuais, priorizando segurança, execução correta e evolução progressiva, dentro da atuação técnica e esportiva permitida por lei. As recomendações desta ficha possuem caráter informativo e orientativo, voltadas ao treinamento esportivo e prática de musculação.`;

            let html = `
                <div class="print-header">
                    <h1 class="print-title">Planilha de Treinamento - ${data.objective}</h1>
                    <div class="prof-info">Prescrição: ${data.memberName} (${isPEF?'PEF':'Treinador Esportivo'}) ${isPEF?'| CREF: '+data.cref+' - '+data.uf:''}</div>
                </div>
                <div class="print-grid">
                    <div><b>Aluno:</b> ${data.student}</div><div><b>Idade:</b> ${data.age}</div><div><b>Gênero:</b> ${data.gender}</div>
                    <div><b>Peso:</b> ${data.weight}kg</div><div><b>Altura:</b> ${data.height}m</div><div><b>IMC:</b> ${imc}</div>
                    <div><b>Nível:</b> ${data.level}</div><div><b>Freq:</b> ${data.freq}</div><div><b>Validade:</b> ${data.validityDays} dias</div>
                </div>
            `;

            if(objData[data.objective] || data.health.length > 0) {
                const hd = isPEF ? healthDataPEF : healthDataTE;
                html += `<div class="print-guidelines"><h4>Diretrizes de Saúde e Objetivo</h4><ul>`;
                if(objData[data.objective]) html += `<li><b>Objetivo:</b> ${objData[data.objective]}</li>`;
                data.health.forEach(h => { if(hd[h]) html += `<li><b>${h.replace(/[\u{1F300}-\u{1F9FF}]/gu,'')}:</b> ${hd[h]}</li>`; });
                html += `</ul></div>`;
            }

            data.workouts.forEach(w => {
                html += `<div class="print-workout"><h3>${w.title}</h3><table><thead><tr>
                    <th style="width:30%">Exercício</th><th style="width:8%">Séries</th><th style="width:12%">Reps</th><th style="width:15%">Técnica</th><th style="width:35%">Obs</th>
                </tr></thead><tbody>`;
                w.exercises.forEach(ex => {
                    html += `<tr><td><b>${ex.name}</b></td><td>${ex.sets}</td><td>${ex.reps}</td><td>${ex.technique!=='Nenhuma'?ex.technique:'-'}</td><td>${ex.obs||'-'}</td></tr>`;
                });
                html += `</tbody></table></div>`;
            });

            if(data.recs) html += `<div class="print-guidelines" style="margin-top:10px;"><h4>Recomendações do Profissional</h4><p style="margin:2px 0;">${data.recs}</p></div>`;
            
            html += `<div class="legal-footer">${legalText}</div>`;

            document.getElementById('print-area').innerHTML = html;
            setTimeout(() => window.print(), 300);
        };

        window.loadHistory = async () => {
            const list = document.getElementById('history-list');
            list.innerHTML = '<i class="fas fa-spinner fa-spin"></i> Carregando nuvem...';
            const q = query(collection(db, 'artifacts', appId, 'users', userId, 'powfit_workouts'), orderBy('date', 'desc'));
            const snap = await getDocs(q);
            
            if(snap.empty) { list.innerHTML = 'Nenhuma ficha salva.'; return; }

            let html = '';
            snap.forEach(d => {
                const data = d.data();
                const createdDate = new Date(data.date);
                const expiryDate = new Date(createdDate.getTime() + (data.validityDays * 24 * 60 * 60 * 1000));
                const isExpired = new Date() > expiryDate;
                
                html += `
                <div class="card p-4 border border-[var(--border-color)] rounded-lg flex justify-between items-center">
                    <div>
                        <div class="font-bold flex items-center gap-2">
                            ${data.student} 
                            ${isExpired ? '<span class="bg-red-500 text-white text-[9px] px-2 py-0.5 rounded uppercase">Vencida</span>' : ''}
                        </div>
                        <div class="text-xs opacity-70">
                            Resp: ${data.memberName} | Criado: ${createdDate.toLocaleDateString('pt-BR')} | Validade: ${data.validityDays} dias
                        </div>
                    </div>
                    <div class="flex gap-2">
                        <button onclick="loadSheet('${d.id}')" class="btn-primary px-3 py-1 text-xs font-bold rounded">EDITAR</button>
                    </div>
                </div>`;
            });
            list.innerHTML = html;
        };

        window.loadSheet = async (docId) => {
            const snap = await getDoc(doc(db, 'artifacts', appId, 'users', userId, 'powfit_workouts', docId));
            if(!snap.exists()) return;
            const data = snap.data();
            
            state.currentSheet = { id: docId, workouts: data.workouts || [] };
            
            document.getElementById('current-prescriber').value = data.memberId || '';
            document.getElementById('stu-name').value = data.student || '';
            document.getElementById('stu-age').value = data.age || '';
            document.getElementById('stu-weight').value = data.weight || '';
            document.getElementById('stu-height').value = data.height || '';
            document.getElementById('stu-gender').value = data.gender || 'Masculino';
            document.getElementById('stu-level').value = data.level || 'Iniciante';
            document.getElementById('stu-objective').value = data.objective || 'Emagrecimento';
            document.getElementById('stu-freq').value = data.freq || '3 dias';
            document.getElementById('stu-validity').value = `Validade: ${data.validityDays} dias`;
            document.getElementById('stu-recs').value = data.recs || '';
            
            changeTheme();
            calcIMC();
            updateHealthGuidelines();
            
            setTimeout(() => {
                document.querySelectorAll('.health-cb').forEach(cb => cb.checked = (data.health || []).includes(cb.value));
            }, 100);

            renderWorkoutSheet();
            switchView('ficha');
        };

        window.generateReport = async () => {
            const rc = document.getElementById('report-content');
            rc.innerHTML = '<i class="fas fa-spinner fa-spin"></i> Calculando...';
            
            const q = query(collection(db, 'artifacts', appId, 'users', userId, 'powfit_workouts'));
            const snap = await getDocs(q);
            
            const now = new Date();
            const stats = {}; // { unitId: { total: 0, members: { memberId: count } } }

            snap.forEach(d => {
                const data = d.data();
                const date = new Date(data.date);
                if(date.getMonth() === now.getMonth() && date.getFullYear() === now.getFullYear()) {
                    if(!stats[data.unitId]) stats[data.unitId] = { total: 0, members: {} };
                    stats[data.unitId].total++;
                    if(!stats[data.unitId].members[data.memberId]) stats[data.unitId].members[data.memberId] = 0;
                    stats[data.unitId].members[data.memberId]++;
                }
            });

            let html = `<div class="text-xl font-bold border-b pb-2 mb-4">Relatório: ${now.toLocaleDateString('pt-BR', {month:'long', year:'numeric'}).toUpperCase()}</div>`;
            
            if(Object.keys(stats).length === 0) {
                rc.innerHTML = html + '<p>Nenhuma ficha gerada neste mês.</p>'; return;
            }

            for(const uId in stats) {
                const unit = state.network.units.find(u => u.id === uId);
                const uName = unit ? unit.name : 'Unidade Excluída/Desconhecida';
                html += `<div class="mb-6 p-4 bg-black bg-opacity-20 rounded-lg border border-gray-600">
                    <h3 class="font-bold text-lg text-primary flex justify-between"><span><i class="fas fa-map-marker-alt"></i> ${uName}</span> <span>Total: ${stats[uId].total} fichas</span></h3>
                    <div class="mt-3 space-y-2">`;
                
                for(const mId in stats[uId].members) {
                    const member = state.network.members.find(m => m.id === mId);
                    const mName = member ? member.name : 'Membro Excluído';
                    html += `<div class="flex justify-between border-b border-gray-600 border-opacity-30 pb-1 text-sm"><span>${mName}</span> <span class="font-bold">${stats[uId].members[mId]}</span></div>`;
                }
                html += `</div></div>`;
            }
            rc.innerHTML = html;
        };

        // Initialize First Ficha
        addWorkoutDay();

    </script>
</body>
</html>
