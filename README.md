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

        .view-section { display: none; }
        .view-section.active { display: block; }
        #print-area { display: none; }

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

    <div id="login-view" class="min-h-screen flex items-center justify-center p-4">
        <div class="card p-8 rounded-2xl shadow-2xl max-w-md w-full text-center border-t-4 border-t-primary">
            <i class="fas fa-dumbbell text-5xl text-primary mb-4"></i>
            <h1 class="text-3xl font-black mb-2">PowFit Pro</h1>
            <p class="text-sm opacity-70 mb-8">Acesso Administrativo Master</p>
            
            <div class="space-y-4 mb-6">
                <input type="email" id="login-email" class="input-field w-full rounded-lg px-4 py-3 text-sm" placeholder="E-mail de acesso">
                <input type="password" id="login-senha" class="input-field w-full rounded-lg px-4 py-3 text-sm" placeholder="Sua senha">
            </div>

            <button id="btn-login" class="btn-primary w-full py-3 rounded-lg font-bold flex items-center justify-center gap-3 text-lg shadow-lg">
                <i class="fas fa-sign-in-alt"></i> Acessar Sistema
            </button>
            <p class="text-xs opacity-50 mt-4"><i class="fas fa-shield-alt"></i> Conexão Segura (Firebase Auth)</p>
        </div>
    </div>

    <div id="app-container" class="hidden min-h-screen flex flex-col">
        
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
                <button id="btn-logout" class="px-4 py-2 rounded text-red-500 hover:bg-red-500 hover:text-white transition"><i class="fas fa-power-off"></i></button>
            </div>
        </nav>

        <div id="view-rede" class="view-section flex-1 p-4 sm:p-8 max-w-5xl mx-auto w-full">
            <h2 class="text-2xl font-bold mb-6"><i class="fas fa-building text-primary"></i> Gestão da Rede de Franquias</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div class="card p-5 rounded-xl">
                    <h3 class="text-lg font-bold mb-4 border-b border-opacity-20 pb-2">1. Unidades da Rede</h3>
                    <div class="flex gap-2 mb-4">
                        <input type="text" id="new-unit-name" class="input-field flex-1 rounded px-3 py-2 text-sm" placeholder="Nome da Unidade (Ex: Matriz)">
                        <button onclick="addUnit()" class="btn-primary px-4 rounded text-sm"><i class="fas fa-plus"></i></button>
                    </div>
                    <div id="units-list" class="space-y-2 max-h-60 overflow-y-auto"></div>
                </div>
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

        <div id="view-ficha" class="view-section active flex-1 p-4 sm:p-8 max-w-7xl mx-auto w-full">
            <div class="card p-4 rounded-xl mb-6 border-l-4 border-l-primary flex flex-col sm:flex-row justify-between items-center gap-4">
                <div>
                    <h3 class="font-bold text-lg">Quem está montando este treino?</h3>
                    <p class="text-xs opacity-70">A ficha será assinada digitalmente por este membro.</p>
                </div>
                <select id="current-prescriber" class="input-field rounded-lg px-4 py-2 text-sm w-full sm:w-1/3 font-medium" onchange="updateHealthGuidelines()">
                    <option value="">-- Selecione o Membro --</option>
                </select>
            </div>
            <div class="grid grid-cols-1 xl:grid-cols-12 gap-6" id="ficha-workspace" style="opacity: 0.5; pointer-events: none;">
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
                        <p class="text-[10px] opacity-70 mb-3 leading-tight text-primary font-bold" id="health-mode-label">Selecione o membro acima</p>
                        <div class="grid grid-cols-2 gap-2 text-[11px]" id="health-container"></div>
                    </div>
                </div>
                <div class="xl:col-span-8 space-y-6">
                    <div class="flex flex-col sm:flex-row justify-between items-center bg-opacity-10 p-4 rounded-xl card border-dashed border-2 gap-4">
                        <select id="stu-validity" class="input-field rounded px-2 py-1 text-xs font-bold">
                            <option>Validade: 15 dias</option><option selected>Validade: 30 dias</option>
                            <option>Validade: 60 dias</option><option>Validade: 90 dias</option>
                        </select>
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

        <div id="view-historico" class="view-section flex-1 p-4 sm:p-8 max-w-5xl mx-auto w-full">
            <h2 class="text-2xl font-bold mb-6"><i class="fas fa-cloud text-primary"></i> Fichas Salvas na Nuvem</h2>
            <div id="history-list" class="space-y-3"></div>
        </div>

        <div id="view-relatorio" class="view-section flex-1 p-4 sm:p-8 max-w-5xl mx-auto w-full">
            <div class="flex justify-between items-center mb-6">
                <h2 class="text-2xl font-bold"><i class="fas fa-chart-pie text-primary"></i> Produtividade Mensal</h2>
                <button onclick="window.print()" class="btn-primary px-4 py-2 rounded shadow"><i class="fas fa-print"></i> Imprimir Relatório</button>
            </div>
            <div class="card p-6 rounded-xl"><div id="report-content" class="space-y-6"></div></div>
        </div>
    </div>

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
                        <input type="text" id="custom-ex-input" class="input-field flex-1 rounded px-3 py-1 text-sm" placeholder="Novo exercício para esta categoria...">
                        <button onclick="addCustomExercise()" class="btn-primary px-4 rounded text-sm">Salvar Nuvem</button>
                    </div>
                    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-2" id="modal-exercises"></div>
                </div>
            </div>
        </div>
    </div>

    <div id="print-area"></div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getAuth, signInWithEmailAndPassword, signOut, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, addDoc, query, getDocs, deleteDoc, serverTimestamp, orderBy } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        // --- FIREBASE CONFIG ---
        const firebaseConfig = {
            apiKey: "AIzaSyD-iuERHA_x1f69smJXh7b8wTLO1zTadIU",
            authDomain: "powfitpro-d8577.firebaseapp.com",
            projectId: "powfitpro-d8577",
            storageBucket: "powfitpro-d8577.firebasestorage.app",
            messagingSenderId: "651381183985",
            appId: "1:651381183985:web:9a425692372a9d67d9e050"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        
        let userId = null;
        let appId_str = 'powfit-pro-oficial';

        // Monitor de Autenticação
        onAuthStateChanged(auth, async (user) => {
            if (user) {
                userId = user.uid;
                document.getElementById('login-view').classList.add('hidden');
                document.getElementById('app-container').classList.remove('hidden');
                await loadNetworkData();
                await loadCustomExercises();
            } else {
                document.getElementById('login-view').classList.remove('hidden');
                document.getElementById('app-container').classList.add('hidden');
            }
        });

        // Botão de Login Oficial
        document.getElementById('btn-login').addEventListener('click', async () => {
            const btn = document.getElementById('btn-login');
            const email = document.getElementById('login-email').value.trim();
            const senha = document.getElementById('login-senha').value.trim();

            if(!email || !senha) return alert("Preencha e-mail e senha.");

            btn.innerHTML = `<i class="fas fa-spinner fa-spin"></i> Entrando...`;
            try {
                await signInWithEmailAndPassword(auth, email, senha);
            } catch (error) {
                console.error(error);
                alert("Erro: E-mail ou senha inválidos.");
                btn.innerHTML = `<i class="fas fa-sign-in-alt"></i> Acessar Sistema`;
            }
        });

        // Botão Logout
        document.getElementById('btn-logout').addEventListener('click', () => signOut(auth));

        // --- DADOS ESTÁTICOS ---
        const healthDataPEF = { "🟢 Saudável": "Manter treinos regulares 3–5x por semana, combinando musculação e cardio. Foco em evolução e constância.", "⚪ Sedentário": "Iniciar com treinos leves 2–3x por semana. Priorizar adaptação, técnica e evitar excesso de carga.", "🟡 Sobrepeso": "Treinar 3–5x por semana com foco em gasto calórico. Combinar musculação e cardio moderado.", "🔴 Obesidade": "Iniciar com exercícios de baixo impacto. Evolução gradual, priorizando saúde e segurança.", "⚖️ Baixo peso": "Foco em musculação para ganho de massa. Treinos moderados com alimentação adequada.", "🍬 Diabetes": "Treinos regulares moderados. Monitorar glicemia e evitar treinos em jejum.", "❤️ Hipertensão": "Treinos moderados, evitar prender a respiração. Priorizar controle da intensidade.", "🔵 Hipotensão": "Evitar mudanças bruscas. Manter hidratação e intensidade leve a moderada.", "💔 Problemas cardíacos": "Treinos leves com liberação médica. Monitorar frequência cardíaca.", "🦴 Problemas articulares": "Evitar impacto. Priorizar exercícios controlados e máquinas.", "🫁 Problemas respiratórios": "Treinos leves a moderados com progressão gradual. Atenção à respiração.", "⚠️ Lesões": "Adaptar exercícios. Evitar dor e focar na recuperação.", "🤰 Gestante": "Treinos leves a moderados, sem impacto ou risco. Foco em mobilidade e bem-estar.", "🤱 Lactante": "Treinar normalmente com atenção à hidratação e energia.", "👴 Idoso": "Foco em força, equilíbrio e mobilidade. Intensidade moderada com segurança." };
        const healthDataTE = { "🟢 Saudável": "Manter treinos regulares de 3–5x por semana. A constância e boa alimentação contribuem para melhores resultados.", "⚪ Sedentário": "O início deve ser gradual, com treinos leves e foco na adaptação do corpo.", "🟡 Sobrepeso": "A prática regular de musculação associada ao cardio contribui para a composição corporal.", "🔴 Obesidade": "É recomendado iniciar com exercícios de menor impacto. A segurança e constância são prioridades.", "⚖️ Baixo peso": "A musculação auxilia no ganho de massa quando associada a alimentação adequada.", "🍬 Diabetes": "Em casos de tontura ou alteração de glicemia, o treino deve ser interrompido e procurar orientação médica.", "❤️ Hipertensão": "Evitar prender a respiração e controlar a intensidade do esforço.", "🔵 Hipotensão": "Evitar mudanças bruscas de posição durante o treino. Manter boa hidratação.", "💔 Problemas cardíacos": "A prática de exercícios deve ocorrer apenas com liberação e acompanhamento médico.", "🦴 Problemas articulares": "Exercícios com menor impacto e maior controle de movimento são indicados.", "🫁 Problemas respiratórios": "A progressão do treino deve ser gradual e respeitar a capacidade respiratória.", "⚠️ Lesões": "A adaptação dos exercícios deve respeitar a limitação existente e evitar dor.", "🤰 Gestante": "Prática com liberação médica. Foco na segurança e bem-estar, sem impacto excessivo.", "🤱 Lactante": "Atividade mantida normalmente, respeitando a recuperação e hidratação.", "👴 Idoso": "A musculação contribui para força e mobilidade. Respeitar limitações e focar na segurança." };
        const objData = { "Emagrecimento": "Foco em déficit calórico com treinos mistos de força e aeróbicos.", "Hipertrofia": "Prioridade na progressão de carga e volume. Essencial descanso.", "Definição": "Manutenção de massa muscular com redução de gordura. Atenção à dieta.", "Condicionamento": "Treinos com menor tempo de intervalo e alta integração cardiopulmonar.", "Resistência": "Séries mais longas, cadência controlada e aprimoramento muscular.", "Força": "Cargas altas, baixas repetições e intervalos de descanso maiores.", "Reabilitação": "Treino focado em fortalecimento específico e controle motor.", "Saúde geral": "Equilíbrio entre força, cardio e flexibilidade." };
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
            customExercises: {},
            currentSheet: { id: null, workouts: [] },
            activeUnitId: null,
            activeModalWorkoutId: null,
            activeCategory: "🔥 PEITO"
        };

        const genId = () => Math.random().toString(36).substr(2, 9);
        const getDocPath = (col) => doc(db, 'powfit_v1', userId, col, 'data');

        // --- GESTÃO DE REDE ---
        async function loadNetworkData() {
            const snap = await getDoc(getDocPath('network'));
            if (snap.exists()) state.network = snap.data();
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
            await saveNetworkData(); renderUnits();
        };

        window.deleteUnit = async (id) => {
            if(!confirm("Excluir unidade?")) return;
            state.network.units = state.network.units.filter(u => u.id !== id);
            state.network.members = state.network.members.filter(m => m.unitId !== id);
            await saveNetworkData(); renderUnits();
        };

        window.selectUnit = (id, name) => {
            state.activeUnitId = id;
            document.getElementById('selected-unit-label').innerText = `- ${name}`;
            document.getElementById('members-panel').classList.remove('opacity-50', 'pointer-events-none');
            renderMembers();
        };

        window.toggleCrefInput = () => {
            document.getElementById('new-mem-cref').style.display = document.getElementById('new-mem-cat').value === 'PEF' ? 'block' : 'none';
        };

        window.addMember = async () => {
            const name = document.getElementById('new-mem-name').value.trim();
            if(!name || !state.activeUnitId) return;
            state.network.members.push({
                id: genId(), unitId: state.activeUnitId, name,
                cpf: document.getElementById('new-mem-cpf').value,
                uf: document.getElementById('new-mem-uf').value,
                cat: document.getElementById('new-mem-cat').value,
                cref: document.getElementById('new-mem-cref').value
            });
            await saveNetworkData(); renderMembers();
        };

        function renderUnits() {
            document.getElementById('units-list').innerHTML = state.network.units.map(u => `
                <div class="flex justify-between p-2 bg-black bg-opacity-20 rounded border border-gray-600 cursor-pointer hover:border-primary" onclick="selectUnit('${u.id}', '${u.name}')">
                    <span class="font-bold">${u.name}</span>
                    <button onclick="event.stopPropagation(); deleteUnit('${u.id}')" class="text-red-500"><i class="fas fa-trash"></i></button>
                </div>`).join('');
        }

        function renderMembers() {
            const members = state.network.members.filter(m => m.unitId === state.activeUnitId);
            document.getElementById('members-list').innerHTML = members.map(m => `
                <div class="flex justify-between p-2 bg-black bg-opacity-20 rounded border border-gray-600 text-sm">
                    <div><div class="font-bold text-primary">${m.name}</div><div class="text-[10px] opacity-70">${m.cat}</div></div>
                    <button onclick="deleteMember('${m.id}')" class="text-red-500"><i class="fas fa-trash"></i></button>
                </div>`).join('');
        }

        window.deleteMember = async (id) => {
            state.network.members = state.network.members.filter(m => m.id !== id);
            await saveNetworkData(); renderMembers();
        };

        function updatePrescriberSelect() {
            const sel = document.getElementById('current-prescriber');
            sel.innerHTML = `<option value="">-- Selecione o Membro --</option>` + state.network.members.map(m => `<option value="${m.id}">${m.name} - ${m.cat}</option>`).join('');
        }

        // --- FICHA E TREINOS ---
        window.switchView = (vId) => {
            document.querySelectorAll('.view-section').forEach(el => el.classList.remove('active'));
            document.getElementById(`view-${vId}`).classList.add('active');
            if(vId === 'historico') loadHistory();
            if(vId === 'relatorio') generateReport();
        };

        window.calcIMC = () => {
            const w = parseFloat(document.getElementById('stu-weight').value);
            const h = parseFloat(document.getElementById('stu-height').value);
            if (w > 0 && h > 0) document.getElementById('imc-display').innerHTML = `<span class="text-primary font-bold">IMC: ${(w/(h*h)).toFixed(1)}</span>`;
        };

        window.changeTheme = () => document.body.setAttribute('data-theme', document.getElementById('stu-gender').value);

        window.updateHealthGuidelines = () => {
            const mId = document.getElementById('current-prescriber').value;
            const ws = document.getElementById('ficha-workspace');
            if(!mId) return;
            ws.style.opacity = '1'; ws.style.pointerEvents = 'auto';
            const member = state.network.members.find(m => m.id === mId);
            const hd = member.cat === 'PEF' ? healthDataPEF : healthDataTE;
            document.getElementById('health-container').innerHTML = Object.keys(hd).map(opt => `
                <label class="flex items-start space-x-1 cursor-pointer text-[10px]">
                    <input type="checkbox" value="${opt}" class="health-cb"> <span>${opt}</span>
                </label>`).join('');
        };

        window.addWorkoutDay = () => {
            state.currentSheet.workouts.push({ id: genId(), title: daysOrder[state.currentSheet.workouts.length] || "NOVO TREINO", exercises: [] });
            renderWorkoutSheet();
        };

        window.removeWorkoutDay = (id) => { state.currentSheet.workouts = state.currentSheet.workouts.filter(w => w.id !== id); renderWorkoutSheet(); };

        function renderWorkoutSheet() {
            document.getElementById('workouts-container').innerHTML = state.currentSheet.workouts.map(w => `
                <div class="card rounded-xl overflow-hidden mb-4">
                    <div class="p-2 bg-black bg-opacity-10 flex justify-between">
                        <input type="text" value="${w.title}" onchange="state.currentSheet.workouts.find(x=>x.id==='${w.id}').title=this.value" class="bg-transparent font-bold text-sm uppercase">
                        <button onclick="removeWorkoutDay('${w.id}')" class="text-red-500"><i class="fas fa-trash"></i></button>
                    </div>
                    <div class="p-1">${w.exercises.map((ex, idx) => `
                        <div class="flex gap-2 p-1 border-b border-gray-700 items-center text-[10px]">
                            <div class="flex-1"><b>${ex.name}</b></div>
                            <input type="text" value="${ex.sets}" onchange="updEx('${w.id}',${idx},'sets',this.value)" class="input-field w-8 text-center rounded" placeholder="S">
                            <input type="text" value="${ex.reps}" onchange="updEx('${w.id}',${idx},'reps',this.value)" class="input-field w-12 text-center rounded" placeholder="R">
                            <button onclick="rmEx('${w.id}', ${idx})" class="text-red-500"><i class="fas fa-times"></i></button>
                        </div>`).join('')}
                    </div>
                    <button onclick="openModal('${w.id}')" class="w-full text-primary text-xs py-2">+ EXERCÍCIO</button>
                </div>`).join('');
        }

        window.updEx = (wId, idx, field, val) => { state.currentSheet.workouts.find(x=>x.id===wId).exercises[idx][field] = val; };
        window.rmEx = (wId, idx) => { state.currentSheet.workouts.find(x=>x.id===wId).exercises.splice(idx, 1); renderWorkoutSheet(); };

        // --- MODAL EXERCÍCIOS ---
        window.openModal = (wId) => { state.activeModalWorkoutId = wId; document.getElementById('exercise-modal').classList.remove('hidden'); renderModalCats(); };
        window.closeModal = () => document.getElementById('exercise-modal').classList.add('hidden');
        window.setModalCat = (c) => { state.activeCategory = c; renderModalCats(); };

        function renderModalCats() {
            document.getElementById('modal-categories').innerHTML = Object.keys(coreExercises).map(cat => `
                <button onclick="setModalCat('${cat}')" class="w-full text-left p-2 rounded text-xs ${state.activeCategory===cat?'bg-primary':'hover:bg-gray-800'}">${cat}</button>`).join('');
            renderModalExs();
        }

        function renderModalExs() {
            const exs = [...coreExercises[state.activeCategory], ...(state.customExercises[state.activeCategory] || [])];
            document.getElementById('modal-exercises').innerHTML = exs.map(ex => `<button onclick="addEx('${ex}')" class="card p-2 text-xs text-left hover:border-primary">${ex}</button>`).join('');
        }

        window.addEx = (name) => {
            state.currentSheet.workouts.find(x=>x.id===state.activeModalWorkoutId).exercises.push({ name, sets: '3', reps: '10-12', technique: 'Nenhuma', obs: '' });
            renderWorkoutSheet();
        };

        async function loadCustomExercises() {
            const snap = await getDoc(getDocPath('customExercises'));
            if(snap.exists()) state.customExercises = snap.data();
        }

        window.addCustomExercise = async () => {
            const val = document.getElementById('custom-ex-input').value.trim();
            if(!val) return;
            if(!state.customExercises[state.activeCategory]) state.customExercises[state.activeCategory] = [];
            state.customExercises[state.activeCategory].push(val);
            await setDoc(getDocPath('customExercises'), state.customExercises);
            renderModalExs();
        };

        // --- SALVAR E IMPRIMIR ---
        window.saveAndPrint = async () => {
            const mId = document.getElementById('current-prescriber').value;
            if(!mId) return alert("Selecione o membro!");
            const member = state.network.members.find(m => m.id === mId);
            
            const data = {
                memberId: member.id, memberName: member.name, unitId: member.unitId, cat: member.cat, cref: member.cref, uf: member.uf,
                date: new Date().toISOString(), student: document.getElementById('stu-name').value || 'Sem Nome',
                age: document.getElementById('stu-age').value, weight: document.getElementById('stu-weight').value,
                height: document.getElementById('stu-height').value, gender: document.getElementById('stu-gender').value,
                objective: document.getElementById('stu-objective').value, validityDays: 30,
                health: Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value),
                workouts: state.currentSheet.workouts, recs: document.getElementById('stu-recs').value
            };

            await addDoc(collection(db, 'powfit_v1', userId, 'workouts'), data);
            
            // Gerar HTML de Impressão
            let html = `<div class="print-header"><h1 class="print-title">Treinamento: ${data.student}</h1></div>`;
            data.workouts.forEach(w => {
                html += `<div class="print-workout"><h3>${w.title}</h3><table><thead><tr><th>Exercício</th><th>Séries</th><th>Reps</th></tr></thead><tbody>`;
                w.exercises.forEach(ex => html += `<tr><td>${ex.name}</td><td>${ex.sets}</td><td>${ex.reps}</td></tr>`);
                html += `</tbody></table></div>`;
            });
            document.getElementById('print-area').innerHTML = html;
            setTimeout(() => window.print(), 500);
        };

        window.loadHistory = async () => {
            const q = query(collection(db, 'powfit_v1', userId, 'workouts'), orderBy('date', 'desc'));
            const snap = await getDocs(q);
            document.getElementById('history-list').innerHTML = snap.empty ? "Vazio" : "";
            snap.forEach(d => {
                const data = d.data();
                document.getElementById('history-list').innerHTML += `
                <div class="card p-3 rounded flex justify-between items-center">
                    <div><b>${data.student}</b><br><small>${new Date(data.date).toLocaleDateString()}</small></div>
                    <button onclick="alert('Funcionalidade de edição em breve')" class="btn-primary px-3 py-1 rounded text-xs">VER</button>
                </div>`;
            });
        };

        window.generateReport = () => { document.getElementById('report-content').innerHTML = "Relatório em desenvolvimento..."; };

        addWorkoutDay();
    </script>
</body>
</html>
