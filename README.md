<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro - Master Edition</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            /* Tema Masculino (Escuro Fitness) */
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
        .btn-primary { background-color: var(--primary); color: white; transition: 0.2s; }
        .btn-primary:hover { background-color: var(--primary-hover); }
        
        /* Ocultar elementos na tela */
        .hidden-screen { display: none !important; }
        #print-area { display: none; }

        /* Estilos de Impressão A4 - Estilo Planilha */
        @media print {
            body { background: white !important; color: black !important; margin: 0; padding: 0; }
            #app-root, .no-print { display: none !important; }
            #print-area { display: block !important; padding: 10mm; font-family: 'Arial', sans-serif; font-size: 10px; }
            @page { size: A4; margin: 0; }
            
            .print-header { border-bottom: 2px solid #000; padding-bottom: 8px; margin-bottom: 12px; text-align: center; }
            .print-title { font-size: 16px; font-weight: bold; margin: 0; text-transform: uppercase; }
            .prof-info { font-size: 11px; margin-top: 4px; font-weight: bold; }
            
            .print-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 4px; margin-bottom: 15px; border: 1px solid #000; padding: 6px; }
            .print-grid div { font-size: 10px; }
            
            .print-guidelines { border: 1px solid #000; padding: 6px; margin-bottom: 15px; font-size: 9px; }
            .print-guidelines h4 { margin: 0 0 4px 0; font-size: 10px; background: #eee; padding: 3px; text-transform: uppercase; }
            .print-guidelines ul { margin: 0; padding-left: 15px; }
            .print-guidelines li { margin-bottom: 2px; }

            .print-workout { margin-bottom: 15px; page-break-inside: avoid; }
            .print-workout h3 { background: #d1d5db; border: 1px solid #000; border-bottom: none; padding: 4px; margin: 0; font-size: 11px; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact;}
            table { width: 100%; border-collapse: collapse; border: 1px solid #000; }
            th, td { border: 1px solid #000; padding: 4px; text-align: center; font-size: 9px; vertical-align: middle; }
            td.text-left { text-align: left; }
            th { background-color: #f3f4f6; font-weight: bold; -webkit-print-color-adjust: exact; color-adjust: exact;}
            .ex-img-print { width: 30px; height: 30px; object-fit: cover; border-radius: 4px; margin-right: 5px; vertical-align: middle; display: inline-block; }

            .print-footer-section { margin-top: 15px; border: 1px solid #000; padding: 8px; page-break-inside: avoid; }
            .legal-text { font-size: 7px; color: #333; text-align: justify; margin-top: 5px; font-style: italic; }
        }
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
    </style>
</head>
<body data-theme="Masculino">

    <div id="app-root">
        <!-- TELA DE LOGIN -->
        <div id="login-screen" class="min-h-screen flex items-center justify-center p-4">
            <div class="card p-8 rounded-2xl shadow-2xl max-w-md w-full text-center">
                <i class="fas fa-dumbbell text-5xl text-primary mb-4"></i>
                <h1 class="text-3xl font-bold mb-2">PowFit Pro</h1>
                <p class="text-sm var(--text-muted) mb-8">Plataforma Profissional de Prescrição & Gestão de Rede</p>
                <button onclick="window.loginGoogle()" class="btn-primary w-full py-3 rounded-xl font-bold text-lg flex items-center justify-center gap-3">
                    <i class="fab fa-google"></i> Entrar com Google
                </button>
            </div>
        </div>

        <!-- TELA DE SETUP DE PERFIL (Primeiro Acesso) -->
        <div id="setup-screen" class="hidden-screen min-h-screen flex items-center justify-center p-4">
            <div class="card p-8 rounded-2xl shadow-2xl max-w-lg w-full">
                <h2 class="text-2xl font-bold mb-6 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">Configuração de Perfil Profissional</h2>
                <div class="space-y-4">
                    <div>
                        <label class="block text-sm font-medium mb-1">Nome Completo</label>
                        <input type="text" id="setup-name" class="input-field w-full rounded-lg px-3 py-2">
                    </div>
                    <div>
                        <label class="block text-sm font-medium mb-1">Rede de Academias / Franquia</label>
                        <input type="text" id="setup-network" class="input-field w-full rounded-lg px-3 py-2" placeholder="Ex: Power Fitness">
                    </div>
                    <div>
                        <label class="block text-sm font-medium mb-1">Unidade de Atuação</label>
                        <input type="text" id="setup-unit" class="input-field w-full rounded-lg px-3 py-2" placeholder="Ex: Unidade Centro">
                    </div>
                    <div>
                        <label class="block text-sm font-medium mb-1">Categoria Profissional</label>
                        <select id="setup-role" onchange="window.toggleSetupCref()" class="input-field w-full rounded-lg px-3 py-2">
                            <option value="PEF">Profissional de Educação Física</option>
                            <option value="TE">Treinador Esportivo</option>
                        </select>
                    </div>
                    <div id="setup-cref-container" class="grid grid-cols-2 gap-3">
                        <div>
                            <label class="block text-sm font-medium mb-1">CREF</label>
                            <input type="text" id="setup-cref" class="input-field w-full rounded-lg px-3 py-2" placeholder="000000">
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-1">Estado (UF)</label>
                            <input type="text" id="setup-state" class="input-field w-full rounded-lg px-3 py-2" placeholder="SP">
                        </div>
                    </div>
                    <button onclick="window.saveProfile()" class="btn-primary w-full py-3 rounded-xl font-bold mt-4">Salvar e Entrar no Sistema</button>
                </div>
            </div>
        </div>

        <!-- TELA PRINCIPAL (DASHBOARD E FICHA) -->
        <div id="main-screen" class="hidden-screen max-w-7xl mx-auto p-4 sm:p-6 lg:p-8 pb-20">
            
            <!-- Navbar -->
            <div class="flex flex-col md:flex-row justify-between items-center mb-8 gap-4 bg-black bg-opacity-20 p-4 rounded-xl border border-opacity-10" style="border-color: var(--border-color)">
                <div class="flex items-center gap-3">
                    <i class="fas fa-dumbbell text-2xl text-primary"></i>
                    <div>
                        <h1 class="text-xl font-bold leading-tight">PowFit Pro</h1>
                        <p class="text-[10px] var(--text-muted)" id="nav-user-info">Carregando...</p>
                    </div>
                </div>
                <div class="flex flex-wrap justify-center gap-2">
                    <button onclick="window.switchTab('prescribe')" id="tab-btn-prescribe" class="px-4 py-2 rounded-lg font-medium text-sm transition bg-primary text-white"><i class="fas fa-pen"></i> Nova Ficha</button>
                    <button onclick="window.switchTab('history')" id="tab-btn-history" class="px-4 py-2 rounded-lg font-medium text-sm transition hover:bg-black hover:bg-opacity-20"><i class="fas fa-chart-bar"></i> Gestão & Nuvem</button>
                    <button onclick="window.switchTab('images')" id="tab-btn-images" class="px-4 py-2 rounded-lg font-medium text-sm transition hover:bg-black hover:bg-opacity-20"><i class="fas fa-images"></i> Banco de Imagens</button>
                    <button onclick="window.logout()" class="px-4 py-2 rounded-lg font-medium text-sm transition text-red-400 hover:bg-red-500 hover:text-white"><i class="fas fa-sign-out-alt"></i> Sair</button>
                </div>
            </div>

            <!-- ABA: PRESCREVER FICHA -->
            <div id="tab-prescribe" class="grid grid-cols-1 xl:grid-cols-12 gap-6">
                <!-- Coluna Esquerda: Dados -->
                <div class="xl:col-span-4 space-y-4">
                    <div class="card rounded-xl p-5 shadow-sm">
                        <div class="flex justify-between items-center mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                            <h2 class="text-lg font-semibold flex items-center gap-2"><i class="fas fa-user text-primary"></i> Cadastro do Aluno</h2>
                            <div id="imc-display"></div>
                        </div>
                        <div class="space-y-3">
                            <input type="text" id="stu-name" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Nome do aluno">
                            <div class="grid grid-cols-3 gap-2">
                                <input type="number" id="stu-age" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Idade">
                                <input type="number" id="stu-weight" oninput="window.calcIMC()" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Peso (kg)">
                                <input type="number" id="stu-height" step="0.01" oninput="window.calcIMC()" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Alt (m)">
                            </div>
                            <div class="grid grid-cols-2 gap-2">
                                <select id="stu-gender" onchange="window.updateTheme()" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                    <option value="Masculino">Masculino</option>
                                    <option value="Feminino">Feminino</option>
                                </select>
                                <select id="stu-level" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                    <option>Iniciante</option><option>Intermediário</option><option>Avançado</option>
                                </select>
                            </div>
                            <select id="stu-objective" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                <option value="Emagrecimento">Emagrecimento</option><option value="Hipertrofia">Hipertrofia</option>
                                <option value="Definição">Definição</option><option value="Condicionamento">Condicionamento</option>
                                <option value="Resistência">Resistência</option><option value="Força">Força</option>
                                <option value="Reabilitação">Reabilitação</option><option value="Saúde geral">Saúde geral</option>
                            </select>
                            <select id="stu-freq" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                <option>3 dias</option><option>5 dias</option><option>6 dias</option><option>Personalizado</option>
                            </select>
                            <select id="stu-validity" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                <option>15 dias</option><option selected>30 dias</option><option>60 dias</option><option>90 dias</option>
                            </select>
                        </div>
                    </div>

                    <div class="card rounded-xl p-5 shadow-sm">
                        <h2 class="text-md font-semibold mb-2 flex items-center gap-2"><i class="fas fa-heartbeat text-primary"></i> Estado de Saúde</h2>
                        <p class="text-[10px] opacity-70 mb-2">Gera orientações automáticas de acordo com seu perfil (PEF/TE).</p>
                        <div class="grid grid-cols-2 gap-2 text-[11px]" id="health-container"></div>
                    </div>
                </div>

                <!-- Coluna Direita: Treinos -->
                <div class="xl:col-span-8 space-y-4">
                    <div class="flex justify-between items-center card p-4 rounded-xl border-dashed border-2">
                        <h2 class="text-xl font-bold flex items-center gap-2"><i class="fas fa-clipboard text-primary"></i> Montagem</h2>
                        <div class="flex gap-2">
                            <button onclick="window.addDay()" class="btn-primary px-3 py-2 rounded-lg text-xs font-bold"><i class="fas fa-plus"></i> DIA</button>
                            <button onclick="window.printAndSave()" class="bg-green-600 hover:bg-green-500 text-white px-4 py-2 rounded-lg text-xs font-bold shadow-lg"><i class="fas fa-print"></i> SALVAR & IMPRIMIR</button>
                        </div>
                    </div>
                    <div id="workout-days-container" class="space-y-4"></div>
                    <div class="card rounded-xl p-4 shadow-sm">
                        <h2 class="text-sm font-semibold mb-2">Recomendações Livres</h2>
                        <textarea id="stu-recs" rows="3" class="input-field w-full rounded-lg px-3 py-2 text-xs" placeholder="Hidratação, descanso..."></textarea>
                    </div>
                </div>
            </div>

            <!-- ABA: GESTÃO & NUVEM (Relatório de Rede) -->
            <div id="tab-history" class="hidden-screen space-y-6">
                <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                    <div class="card p-6 rounded-xl text-center">
                        <i class="fas fa-network-wired text-3xl text-primary mb-2"></i>
                        <h3 class="font-bold text-lg" id="dash-network">Rede</h3>
                        <p class="text-xs opacity-70">Sua Franquia</p>
                    </div>
                    <div class="card p-6 rounded-xl text-center">
                        <i class="fas fa-map-marker-alt text-3xl text-primary mb-2"></i>
                        <h3 class="font-bold text-lg" id="dash-unit">Unidade</h3>
                        <p class="text-xs opacity-70">Sua Base</p>
                    </div>
                    <div class="card p-6 rounded-xl text-center">
                        <i class="fas fa-file-invoice text-3xl text-primary mb-2"></i>
                        <h3 class="font-bold text-lg" id="dash-count">0</h3>
                        <p class="text-xs opacity-70">Fichas Geradas (Mês Atual)</p>
                    </div>
                </div>
                
                <div class="card rounded-xl p-5">
                    <div class="flex justify-between items-center mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <h2 class="text-lg font-bold"><i class="fas fa-cloud text-primary"></i> Histórico de Fichas da Equipe (Nuvem)</h2>
                        <button onclick="window.loadHistoryCloud()" class="text-xs btn-primary px-3 py-1 rounded"><i class="fas fa-sync"></i> Atualizar</button>
                    </div>
                    <div id="cloud-history-list" class="space-y-2 max-h-96 overflow-y-auto pr-2">
                        <!-- Itens carregados via JS -->
                    </div>
                </div>
            </div>

            <!-- ABA: BANCO DE IMAGENS -->
            <div id="tab-images" class="hidden-screen space-y-4">
                <div class="card p-5 rounded-xl text-center border-dashed border-2">
                    <h2 class="text-xl font-bold mb-2">Banco de Imagens Customizado</h2>
                    <p class="text-sm opacity-70 mb-4">Faça upload de miniaturas para os exercícios. Elas ficarão salvas na nuvem e sairão na impressão.</p>
                    
                    <div class="max-w-md mx-auto space-y-3 text-left">
                        <select id="img-ex-select" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                            <option value="">-- Selecione o Exercício --</option>
                            <!-- Povoado via JS -->
                        </select>
                        <input type="file" id="img-upload" accept="image/*" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                        <button onclick="window.saveExerciseImage()" class="btn-primary w-full py-2 rounded-lg font-bold"><i class="fas fa-upload"></i> Salvar Imagem na Nuvem</button>
                    </div>
                </div>
                
                <div class="grid grid-cols-2 md:grid-cols-4 gap-4" id="image-gallery">
                    <!-- Galeria carregada via JS -->
                </div>
            </div>
        </div>

        <!-- MODAL DE EXERCÍCIOS -->
        <div id="exercise-modal" class="fixed inset-0 bg-black bg-opacity-80 backdrop-blur-sm hidden flex items-center justify-center p-4 z-50">
            <div class="card w-full max-w-5xl rounded-xl shadow-2xl flex flex-col h-[85vh]">
                <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                    <h3 class="text-lg font-bold"><i class="fas fa-search text-primary mr-2"></i> Adicionar Exercício</h3>
                    <button onclick="window.closeExModal()" class="text-gray-400 hover:text-red-500 text-2xl">&times;</button>
                </div>
                <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                    <div class="w-full md:w-1/4 border-r overflow-y-auto p-2" style="border-color: var(--border-color)" id="modal-categories"></div>
                    <div class="w-full md:w-3/4 overflow-y-auto p-3 bg-black bg-opacity-5" id="modal-exercises"></div>
                </div>
            </div>
        </div>
    </div>

    <!-- ÁREA DE IMPRESSÃO (Oculta) -->
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

        // --- BANCO DE DADOS LOCAL ---
        const dbCat = {
            "🔥 PEITO": ["Supino Reto", "Supino Inclinado", "Supino Inclinado com Halteres", "Supino Fechado com Halteres", "Cross Over", "Cross Over Alto", "Cross Over Baixo", "Crucifixo Reto", "Crucifixo Inclinado com Halteres", "Crucifixo na Máquina", "Peck Fly", "Peck Fly Unilateral", "Pullover", "Flexão de Braço", "Flexão com Pés Elevados", "Flexão Explosiva"],
            "🦍 COSTAS": ["Puxada Alta", "Puxada de Frente Supinada", "Pulldown", "Remada Aberta", "Remada Baixa", "Remada Curvada", "Remada Curvada Supinada", "Remada Cavalinho (T-Bar)", "Serrote", "Facepull (puxada de cima para baixo)"],
            "🦵 PERNAS": ["Agachamento Livre", "Agachamento Taça", "Agachamento no Smith", "Agachamento com passada lateral", "Squat", "Hack Machine", "Leg 45°", "Leg 90°", "Agachamento Sumô", "Agachamento Sissy (Livre)", "Afundo", "Recuo", "Avanço", "Passada", "Búlgaro", "Step-up", "Levantamento Terra", "Levantamento Terra Romeno", "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", "Cadeira Flexora", "Elevação Pélvica no Banco", "Elevação Pélvica no Chão", "Elevação Pélvica Unilateral no Chão", "Extensão de Quadril (Glúteo Máximo)", "Extensão Cruzada (Glúteo Médio)", "Coice", "Cachorrinho", "Cadeira Extensora", "Adução", "Abdução", "Abdução Inclinada", "Flexão Nórdica", "Flexão Nórdica Invertida", "Panturrilha Livre", "Panturrilha no Leg Press", "Panturrilha Banco", "Panturrilha Squat", "Panturrilha Unilateral"],
            "💪 BRAÇOS (Bíceps)": ["Rosca Direta", "Rosca Alternada", "Rosca 21", "Rosca Scott Barra W", "Rosca Scott Unilateral", "Rosca Scott com Halteres", "Rosca Martelo", "Rosca Cross", "Rosca Inversa", "Rosca 45°"],
            "💪 BRAÇOS (Tríceps)": ["Triceps Pulley Unilateral", "Tríceps Pulley Barra", "Tríceps Pulley Corda", "Tríceps Pulley Pegada Inversa", "Tríceps Francês na Corda", "Tríceps Francês com Halter", "Tríceps Francês Unilateral", "Tríceps Cruzado Polia Dupla", "Tríceps Coice Unilateral", "Tríceps Arremesso", "Tríceps Testa", "Mergulho no Banco"],
            "🪨 OMBROS": ["Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", "Elevação Lateral Unilateral Cross", "Elevação Lateral Sentado", "Desenvolvimento com Halteres", "Desenvolvimento com Barra", "Arnold Press", "Elevação Borboleta", "Crucifixo Inverso Sentado com Halteres", "Crucifixo Inverso na Polia", "Crucifixo Inverso Unilateral na Polia", "Facepull (puxada reta)", "Remada Alta", "Encolhimento (Trapézio)"],
            "🧠 ABDÔMEN": ["Infra com Elevação de Perna", "Abdominal Supra", "Abdominal Remador", "Abdominal Bicicleta", "Abdominal Twister com Peso", "Prancha", "Prancha Lateral", "Trituração de Cabos em Pé", "Isometria na parede", "Abdominal isometrico"],
            "🫀 CARDIO": ["Bicicleta 10 Minutos", "Bicicleta 15 Minutos", "Bicicleta 20 Minutos", "Esteira 10 Minutos", "Esteira 15 Minutos", "Esteira 20 Minutos", "Pular Corda"]
        };
        const dbTech = ["Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", "Negativa", "Isometria", "Parciais", "Pirâmide"];
        const healthPEF = {"🟢 Saudável": "Manter treinos regulares 3–5x por semana, combinando musculação e cardio.", "⚪ Sedentário": "Iniciar com treinos leves 2–3x por semana. Priorizar adaptação.", "🟡 Sobrepeso": "Treinar 3–5x por semana com foco em gasto calórico.", "🔴 Obesidade": "Iniciar com exercícios de baixo impacto. Evolução gradual.", "⚖️ Baixo peso": "Foco em musculação para ganho de massa.", "🍬 Diabetes": "Monitorar glicemia e evitar treinos em jejum.", "❤️ Hipertensão": "Treinos moderados, evitar prender a respiração.", "🔵 Hipotensão": "Evitar mudanças bruscas. Manter hidratação.", "💔 Problemas cardíacos": "Treinos leves com liberação médica.", "🦴 Problemas articulares": "Evitar impacto. Priorizar exercícios controlados.", "🫁 Problemas respiratórios": "Progressão gradual. Atenção à respiração.", "⚠️ Lesões": "Adaptar exercícios. Evitar dor e focar na recuperação.", "🤰 Gestante": "Sem impacto ou risco. Foco em mobilidade e bem-estar.", "🤱 Lactante": "Atenção à hidratação e energia.", "👴 Idoso": "Foco em força, equilíbrio e mobilidade com segurança."};
        const healthTE = {"🟢 Saudável": "Manter treinos regulares de 3–5x por semana, combinando musculação e cardio. Constância e alimentação contribuem para resultados.", "⚪ Sedentário": "Início gradual, treinos leves. Prioridade é constância e execução correta.", "🟡 Sobrepeso": "A prática regular contribui para melhora do condicionamento. Progressão individual.", "🔴 Obesidade": "Menor impacto e progressão gradual. Segurança e constância são prioridades.", "⚖️ Baixo peso": "Auxilia ganho de massa associado a alimentação. Priorizar recuperação.", "🍬 Diabetes": "Acompanhamento médico regular. Em casos de mal-estar, interromper o treino e buscar orientação.", "❤️ Hipertensão": "Evitar Manobra de Valsalva e controlar intensidade. Respeitar orientação médica.", "🔵 Hipotensão": "Evitar mudanças bruscas. Manter hidratação.", "💔 Problemas cardíacos": "Apenas com liberação médica. Respeitar limites.", "🦴 Problemas articulares": "Menor impacto. Acompanhamento ajuda na segurança.", "🫁 Problemas respiratórios": "Progressão gradual. Interromper em caso de falta de ar.", "⚠️ Lesões": "Respeitar limitações e evitar dor. Seguir orientação técnica.", "🤰 Gestante": "Prática com liberação médica. Foco na segurança e mobilidade.", "🤱 Lactante": "Atividade mantida respeitando recuperação e hidratação.", "👴 Idoso": "Contribui para força e autonomia. Respeitar limitações individuais."};
        const objTexts = {"Emagrecimento": "Foco em déficit calórico com treinos mistos de força e aeróbicos.", "Hipertrofia": "Prioridade na progressão de carga e volume adequado. Superávit calórico.", "Definição": "Manutenção de massa muscular enquanto reduz o percentual de gordura.", "Condicionamento": "Treinos com menor intervalo, circuitos e integração cardiopulmonar.", "Resistência": "Séries longas, cadência controlada.", "Força": "Cargas altas, baixas repetições e intervalos maiores.", "Reabilitação": "Fortalecimento específico, mobilidade e controle motor.", "Saúde geral": "Equilíbrio entre força, cardio e flexibilidade."};
        const weekDays = ["SEGUNDA-FEIRA", "TERÇA-FEIRA", "QUARTA-FEIRA", "QUINTA-FEIRA", "SEXTA-FEIRA", "SÁBADO", "DOMINGO"];

        // --- ESTADO ---
        let appState = {
            user: null,
            profile: null,
            workouts: [],
            modalDayId: null,
            modalCat: "🔥 PEITO",
            exerciseImages: {} // Map: "Nome do Exercicio" -> "Base64"
        };

        // --- AUTH & INIT ---
        window.loginGoogle = () => signInWithPopup(auth, new GoogleAuthProvider());
        window.logout = () => signOut(auth);

        onAuthStateChanged(auth, async (user) => {
            if (user) {
                appState.user = user;
                document.getElementById('login-screen').classList.add('hidden-screen');
                await loadUserProfile(user.uid);
            } else {
                appState.user = null;
                appState.profile = null;
                document.getElementById('login-screen').classList.remove('hidden-screen');
                document.getElementById('setup-screen').classList.add('hidden-screen');
                document.getElementById('main-screen').classList.add('hidden-screen');
            }
        });

        async function loadUserProfile(uid) {
            const docRef = doc(db, "users", uid);
            const docSnap = await getDoc(docRef);
            if (docSnap.exists()) {
                appState.profile = docSnap.data();
                startApp();
            } else {
                document.getElementById('setup-screen').classList.remove('hidden-screen');
                document.getElementById('setup-name').value = appState.user.displayName || "";
            }
        }

        window.toggleSetupCref = () => {
            const role = document.getElementById('setup-role').value;
            document.getElementById('setup-cref-container').style.display = role === 'TE' ? 'none' : 'grid';
        };

        window.saveProfile = async () => {
            const p = {
                name: document.getElementById('setup-name').value,
                network: document.getElementById('setup-network').value || "Independente",
                unit: document.getElementById('setup-unit').value || "Principal",
                role: document.getElementById('setup-role').value,
                cref: document.getElementById('setup-cref').value,
                state: document.getElementById('setup-state').value,
                createdAt: new Date().toISOString()
            };
            if(p.role === 'PEF' && !p.cref) return alert('Preencha o CREF');
            if(!p.name) return alert('Preencha o nome');
            
            await setDoc(doc(db, "users", appState.user.uid), p);
            appState.profile = p;
            document.getElementById('setup-screen').classList.add('hidden-screen');
            startApp();
        };

        async function startApp() {
            document.getElementById('main-screen').classList.remove('hidden-screen');
            document.getElementById('nav-user-info').innerText = `${appState.profile.name} | ${appState.profile.unit} (${appState.profile.network})`;
            document.getElementById('dash-network').innerText = appState.profile.network;
            document.getElementById('dash-unit').innerText = appState.profile.unit;
            
            renderHealthList();
            populateImageSelect();
            await loadExerciseImages();
            
            if(appState.workouts.length === 0) window.addDay();
            window.switchTab('prescribe');
        }

        // --- NAVEGAÇÃO ---
        window.switchTab = (tab) => {
            ['prescribe', 'history', 'images'].forEach(t => {
                document.getElementById(`tab-${t}`).classList.add('hidden-screen');
                document.getElementById(`tab-btn-${t}`).classList.remove('bg-primary', 'text-white');
                document.getElementById(`tab-btn-${t}`).classList.add('hover:bg-black', 'hover:bg-opacity-20');
            });
            document.getElementById(`tab-${tab}`).classList.remove('hidden-screen');
            document.getElementById(`tab-btn-${tab}`).classList.remove('hover:bg-black', 'hover:bg-opacity-20');
            document.getElementById(`tab-btn-${tab}`).classList.add('bg-primary', 'text-white');
            
            if(tab === 'history') window.loadHistoryCloud();
        };

        // --- PRESCRIÇÃO LÓGICA ---
        window.calcIMC = () => {
            const w = parseFloat(document.getElementById('stu-weight').value);
            const h = parseFloat(document.getElementById('stu-height').value);
            const d = document.getElementById('imc-display');
            if(w>0 && h>0) {
                const imc = (w/(h*h)).toFixed(1);
                d.innerHTML = `<span class="bg-primary bg-opacity-20 text-primary px-3 py-1 rounded-full text-[10px] font-bold">IMC: ${imc}</span>`;
            } else d.innerHTML = '';
        };

        window.updateTheme = () => {
            document.body.setAttribute('data-theme', document.getElementById('stu-gender').value);
        };

        function renderHealthList() {
            const hData = appState.profile.role === 'PEF' ? healthPEF : healthTE;
            const c = document.getElementById('health-container');
            c.innerHTML = Object.keys(hData).map(k => `
                <label class="flex items-start gap-1 cursor-pointer hover:bg-black hover:bg-opacity-5 p-1 rounded">
                    <input type="checkbox" value="${k}" class="health-cb mt-0.5"> <span class="leading-tight">${k}</span>
                </label>
            `).join('');
        }

        window.addDay = () => {
            appState.workouts.push({ id: Date.now().toString(), title: `TREINO ${weekDays[appState.workouts.length % 7] || 'EXTRA'}`, exercises: [] });
            renderDays();
        };
        window.removeDay = (id) => { appState.workouts = appState.workouts.filter(w=>w.id!==id); renderDays(); };
        window.dupDay = (id) => { 
            const w = appState.workouts.find(x=>x.id===id);
            if(w) { appState.workouts.push(JSON.parse(JSON.stringify({...w, id: Date.now().toString(), title: w.title+' CÓPIA'}))); renderDays(); }
        };
        window.updateDayTitle = (id, val) => { appState.workouts.find(w=>w.id===id).title = val; };

        // Exercicio actions
        window.remEx = (dId, eIdx) => { appState.workouts.find(w=>w.id===dId).exercises.splice(eIdx,1); renderDays(); };
        window.updEx = (dId, eIdx, f, v) => { appState.workouts.find(w=>w.id===dId).exercises[eIdx][f] = v; };
        window.movEx = (dId, eIdx, dir) => {
            const list = appState.workouts.find(w=>w.id===dId).exercises;
            if(dir==='up' && eIdx>0) [list[eIdx], list[eIdx-1]] = [list[eIdx-1], list[eIdx]];
            if(dir==='down' && eIdx<list.length-1) [list[eIdx], list[eIdx+1]] = [list[eIdx+1], list[eIdx]];
            renderDays();
        };

        function renderDays() {
            const c = document.getElementById('workout-days-container');
            c.innerHTML = appState.workouts.map(w => `
                <div class="card rounded-xl shadow-sm overflow-hidden">
                    <div class="bg-black bg-opacity-5 p-2 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                        <input type="text" value="${w.title}" onchange="window.updateDayTitle('${w.id}', this.value)" class="input-field bg-transparent font-bold text-sm w-1/2 px-2 py-1 uppercase rounded">
                        <div class="flex gap-1">
                            <button onclick="window.dupDay('${w.id}')" class="p-1 hover:bg-black hover:bg-opacity-10 rounded text-xs"><i class="fas fa-copy"></i></button>
                            <button onclick="window.removeDay('${w.id}')" class="p-1 text-red-500 hover:bg-red-500 hover:text-white rounded text-xs"><i class="fas fa-trash"></i></button>
                        </div>
                    </div>
                    <div>
                        ${w.exercises.length === 0 ? '<div class="p-3 text-center text-xs opacity-50">Vazio</div>' : w.exercises.map((ex, i) => `
                            <div class="flex flex-col md:flex-row gap-2 p-2 border-b last:border-0 border-opacity-10 items-start md:items-center" style="border-color: var(--border-color)">
                                <div class="flex flex-row md:flex-col gap-1">
                                    <button onclick="window.movEx('${w.id}', ${i}, 'up')" class="text-[9px] bg-black bg-opacity-10 px-1 rounded"><i class="fas fa-chevron-up"></i></button>
                                    <button onclick="window.movEx('${w.id}', ${i}, 'down')" class="text-[9px] bg-black bg-opacity-10 px-1 rounded"><i class="fas fa-chevron-down"></i></button>
                                </div>
                                <div class="flex-1 w-full">
                                    <div class="text-[8px] uppercase opacity-60">${ex.category}</div>
                                    <div class="text-xs font-bold">${ex.name}</div>
                                </div>
                                <div class="flex gap-1 items-center w-full md:w-auto">
                                    <input type="text" value="${ex.sets}" onchange="window.updEx('${w.id}', ${i}, 'sets', this.value)" class="input-field w-10 text-center text-xs p-1 rounded" placeholder="S"> x
                                    <input type="text" value="${ex.reps}" onchange="window.updEx('${w.id}', ${i}, 'reps', this.value)" class="input-field w-14 text-center text-xs p-1 rounded" placeholder="Reps">
                                    <select onchange="window.updEx('${w.id}', ${i}, 'technique', this.value)" class="input-field w-24 text-[9px] p-1 rounded">
                                        ${dbTech.map(t=>`<option ${ex.technique===t?'selected':''}>${t}</option>`).join('')}
                                    </select>
                                    <input type="text" value="${ex.obs}" onchange="window.updEx('${w.id}', ${i}, 'obs', this.value)" class="input-field flex-1 md:w-32 text-xs p-1 rounded" placeholder="Obs">
                                    <button onclick="window.remEx('${w.id}', ${i})" class="text-red-500 p-1 text-sm"><i class="fas fa-times"></i></button>
                                </div>
                            </div>
                        `).join('')}
                    </div>
                    <button onclick="window.openExModal('${w.id}')" class="w-full p-2 text-xs font-bold text-primary hover:bg-primary hover:bg-opacity-10 border-t" style="border-color: var(--border-color)">+ ADICIONAR EXERCÍCIO</button>
                </div>
            `).join('');
        }

        // --- MODAL EXERCICIOS ---
        window.openExModal = (id) => { appState.modalDayId = id; document.getElementById('exercise-modal').classList.remove('hidden'); renderExCat(); renderExList(); };
        window.closeExModal = () => document.getElementById('exercise-modal').classList.add('hidden');
        window.setExCat = (c) => { appState.modalCat = c; renderExCat(); renderExList(); };

        function renderExCat() {
            document.getElementById('modal-categories').innerHTML = Object.keys(dbCat).map(c=>`
                <button onclick="window.setExCat('${c}')" class="w-full text-left p-2 rounded text-xs mb-1 ${appState.modalCat===c?'bg-primary text-white':'hover:bg-black hover:bg-opacity-10'}">${c}</button>
            `).join('');
        }
        function renderExList() {
            const isCardio = appState.modalCat === "🫀 CARDIO";
            document.getElementById('modal-exercises').innerHTML = `<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-2">` + 
                dbCat[appState.modalCat].map(ex => `
                <button onclick="window.addExToDay('${ex}', ${isCardio})" class="card p-2 text-left text-xs font-bold rounded flex justify-between group hover:border-primary">
                    <span>${ex}</span> <i class="fas fa-plus opacity-0 group-hover:opacity-100 text-primary"></i>
                </button>
            `).join('') + `</div>`;
        }
        window.addExToDay = (exName, isCardio) => {
            appState.workouts.find(w=>w.id===appState.modalDayId).exercises.push({
                category: appState.modalCat, name: exName, sets: isCardio?'1':'3', reps: isCardio?'-':'10-12', technique: 'Nenhuma', obs: ''
            });
            renderDays();
            const m = document.getElementById('modal-exercises'); m.style.opacity='0.5'; setTimeout(()=>m.style.opacity='1', 100);
        };

        // --- GESTÃO DE IMAGENS NA NUVEM ---
        function populateImageSelect() {
            const sel = document.getElementById('img-ex-select');
            Object.values(dbCat).flat().sort().forEach(ex => {
                sel.insertAdjacentHTML('beforeend', `<option value="${ex}">${ex}</option>`);
            });
        }

        async function loadExerciseImages() {
            // Busca o documento de imagens do usuario
            const docRef = doc(db, "user_images", appState.user.uid);
            const docSnap = await getDoc(docRef);
            if(docSnap.exists()) {
                appState.exerciseImages = docSnap.data().images || {};
            }
            renderImageGallery();
        }

        window.saveExerciseImage = () => {
            const exName = document.getElementById('img-ex-select').value;
            const file = document.getElementById('img-upload').files[0];
            if(!exName || !file) return alert("Selecione o exercício e a imagem.");

            const reader = new FileReader();
            reader.onload = function(e) {
                const img = new Image();
                img.onload = async function() {
                    // Redimensiona p/ economizar firebase
                    const canvas = document.createElement('canvas');
                    const MAX_WIDTH = 150; const MAX_HEIGHT = 150;
                    let width = img.width; let height = img.height;
                    if (width > height) { if (width > MAX_WIDTH) { height *= MAX_WIDTH / width; width = MAX_WIDTH; } } 
                    else { if (height > MAX_HEIGHT) { width *= MAX_HEIGHT / height; height = MAX_HEIGHT; } }
                    canvas.width = width; canvas.height = height;
                    const ctx = canvas.getContext('2d');
                    ctx.drawImage(img, 0, 0, width, height);
                    const base64 = canvas.toDataURL('image/jpeg', 0.8);
                    
                    appState.exerciseImages[exName] = base64;
                    
                    // Salva na nuvem
                    await setDoc(doc(db, "user_images", appState.user.uid), { images: appState.exerciseImages });
                    alert("Imagem salva na nuvem com sucesso!");
                    renderImageGallery();
                }
                img.src = e.target.result;
            }
            reader.readAsDataURL(file);
        };

        function renderImageGallery() {
            const g = document.getElementById('image-gallery');
            g.innerHTML = Object.keys(appState.exerciseImages).map(k => `
                <div class="card p-2 rounded text-center relative group">
                    <img src="${appState.exerciseImages[k]}" class="w-full h-24 object-cover rounded mb-1">
                    <p class="text-[9px] font-bold truncate">${k}</p>
                    <button onclick="window.deleteImage('${k}')" class="absolute top-1 right-1 bg-red-500 text-white p-1 rounded text-[10px] opacity-0 group-hover:opacity-100"><i class="fas fa-trash"></i></button>
                </div>
            `).join('');
        }
        
        window.deleteImage = async (k) => {
            if(confirm("Excluir imagem da nuvem?")) {
                delete appState.exerciseImages[k];
                await setDoc(doc(db, "user_images", appState.user.uid), { images: appState.exerciseImages });
                renderImageGallery();
            }
        };


        // --- NUVEM & RELATÓRIOS ---
        window.loadHistoryCloud = async () => {
            const list = document.getElementById('cloud-history-list');
            list.innerHTML = `<div class="text-center p-4"><i class="fas fa-spinner fa-spin"></i> Sincronizando...</div>`;
            
            // Puxa tudo da unidade do cara
            const q = collection(db, "workouts");
            const querySnapshot = await getDocs(q);
            
            let myUnitWorkouts = [];
            let currentMonthCount = 0;
            const currentMonth = new Date().getMonth();
            const currentYear = new Date().getFullYear();

            querySnapshot.forEach((doc) => {
                const data = doc.data();
                // Filtra no cliente pela unidade para respeitar restrições de indice simples no prompt
                if(data.unit === appState.profile.unit && data.network === appState.profile.network) {
                    data.docId = doc.id;
                    myUnitWorkouts.push(data);
                    
                    const d = new Date(data.createdAt);
                    if(d.getMonth() === currentMonth && d.getFullYear() === currentYear) {
                        currentMonthCount++;
                    }
                }
            });

            document.getElementById('dash-count').innerText = currentMonthCount;

            myUnitWorkouts.sort((a,b) => new Date(b.createdAt) - new Date(a.createdAt));

            list.innerHTML = myUnitWorkouts.length === 0 ? '<div class="text-center text-xs opacity-50 p-4">Nenhuma ficha salva nesta unidade.</div>' : 
                myUnitWorkouts.map(w => {
                    const dateObj = new Date(w.createdAt);
                    const isExpired = (new Date() - dateObj) > (parseInt(w.validity) * 24*60*60*1000);
                    return `
                    <div class="card p-3 rounded-lg border flex justify-between items-center text-sm ${isExpired ? 'border-red-500 border-opacity-50 opacity-70' : ''}">
                        <div>
                            <div class="font-bold flex items-center gap-2">
                                ${w.stuName} ${isExpired ? '<span class="text-[9px] bg-red-500 text-white px-1 rounded">VENCIDA</span>' : ''}
                            </div>
                            <div class="text-[10px] opacity-70">
                                Criado por: ${w.profName} | ${dateObj.toLocaleDateString('pt-BR')}
                            </div>
                        </div>
                        <button onclick="window.deleteWorkoutCloud('${w.docId}')" class="text-red-500 p-2"><i class="fas fa-trash"></i></button>
                    </div>
                `}).join('');
        };

        window.deleteWorkoutCloud = async (docId) => {
            if(confirm("Apagar da nuvem permanentemente?")) {
                await deleteDoc(doc(db, "workouts", docId));
                window.loadHistoryCloud();
            }
        };

        // --- SALVAR & IMPRIMIR ---
        window.printAndSave = async () => {
            const stuData = {
                stuName: document.getElementById('stu-name').value || 'Não informado',
                stuAge: document.getElementById('stu-age').value,
                stuWeight: document.getElementById('stu-weight').value,
                stuHeight: document.getElementById('stu-height').value,
                stuGender: document.getElementById('stu-gender').value,
                stuLevel: document.getElementById('stu-level').value,
                stuObjective: document.getElementById('stu-objective').value,
                stuFreq: document.getElementById('stu-freq').value,
                validity: document.getElementById('stu-validity').value.replace(/\D/g, ''),
                recs: document.getElementById('stu-recs').value,
                healthKeys: Array.from(document.querySelectorAll('.health-cb:checked')).map(cb=>cb.value)
            };

            if(!stuData.stuName || appState.workouts.length === 0) return alert("Preencha o nome do aluno e adicione treinos.");

            // Salva no Firestore
            const workoutDoc = {
                uid: appState.user.uid,
                profName: appState.profile.name,
                network: appState.profile.network,
                unit: appState.profile.unit,
                createdAt: new Date().toISOString(),
                ...stuData,
                workouts: appState.workouts
            };
            await addDoc(collection(db, "workouts"), workoutDoc);

            // Gera HTML para impressão
            generatePrintView(stuData);
        };

        function generatePrintView(d) {
            let imcStr = "-";
            if(d.stuWeight && d.stuHeight) imcStr = (parseFloat(d.stuWeight) / Math.pow(parseFloat(d.stuHeight), 2)).toFixed(1);

            const isPEF = appState.profile.role === 'PEF';
            const hDict = isPEF ? healthPEF : healthTE;
            const profLabel = isPEF ? 'Profissional de Educação Física' : 'Treinador Esportivo';
            const crefStr = isPEF ? `CREF: ${appState.profile.cref} - ${appState.profile.state}` : '';
            
            const legalPEF = `⚠️ OBSERVAÇÃO LEGAL – PROFISSIONAL DE EDUCAÇÃO FÍSICA: Conforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º estabelece que compete ao profissional coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos especializados.`;
            const legalTE = `⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO: Conforme a Lei nº 14.597/2023 (Lei Geral do Esporte), Art. 75, a profissão de treinador esportivo é reconhecida e regulada no Brasil, com atuação de caráter técnico e esportivo. A atuação possui finalidade orientativa e não substitui avaliação médica. Em casos de doenças ou lesões, recomenda-se avaliação prévia. O treinamento respeita os limites individuais, priorizando segurança, dentro da atuação técnica e esportiva permitida por lei. As recomendações possuem caráter informativo voltadas ao treinamento esportivo.`;

            let html = `
                <div class="print-header">
                    <h1 class="print-title">Planilha de Treino - ${d.stuObjective}</h1>
                    <div class="prof-info">${appState.profile.name} (${profLabel}) <br> ${crefStr} <br> ${appState.profile.network} - ${appState.profile.unit}</div>
                </div>
                <div class="print-grid">
                    <div><strong>Aluno:</strong> ${d.stuName}</div>
                    <div><strong>Idade:</strong> ${d.stuAge||'-'}</div>
                    <div><strong>Peso/Alt:</strong> ${d.stuWeight||'-'}kg / ${d.stuHeight||'-'}m</div>
                    <div><strong>IMC:</strong> ${imcStr}</div>
                    <div><strong>Objetivo:</strong> ${d.stuObjective}</div>
                    <div><strong>Nível:</strong> ${d.stuLevel}</div>
                    <div><strong>Freq:</strong> ${d.stuFreq}</div>
                    <div><strong>Validade:</strong> ${d.validity} dias</div>
                </div>
            `;

            if(objTexts[d.stuObjective] || d.healthKeys.length > 0) {
                html += `<div class="print-guidelines"><h4>Diretrizes de Perfil</h4><ul>`;
                html += `<li><strong>${d.stuObjective}:</strong> ${objTexts[d.stuObjective]}</li>`;
                d.healthKeys.forEach(k => {
                    const cleanK = k.replace(/[\u{1F300}-\u{1F9FF}\u{2600}-\u{26FF}\u{2700}-\u{27BF}\u{FE0F}]/gu, '').trim();
                    html += `<li><strong>Saúde (${cleanK}):</strong> ${hDict[k]}</li>`;
                });
                html += `</ul></div>`;
            }

            appState.workouts.forEach(w => {
                html += `<div class="print-workout"><h3>${w.title}</h3><table>
                    <thead><tr><th style="width:40%">Exercício</th><th style="width:10%">Séries</th><th style="width:15%">Reps</th><th style="width:15%">Técnica</th><th style="width:20%">Obs</th></tr></thead><tbody>`;
                if(w.exercises.length===0) html += `<tr><td colspan="5">Sem exercícios</td></tr>`;
                else {
                    w.exercises.forEach(ex => {
                        const imgTag = appState.exerciseImages[ex.name] ? `<img src="${appState.exerciseImages[ex.name]}" class="ex-img-print">` : '';
                        html += `<tr><td class="text-left">${imgTag} <strong>${ex.name}</strong></td><td>${ex.sets}</td><td>${ex.reps}</td><td>${ex.technique==='Nenhuma'?'-':ex.technique}</td><td>${ex.obs||'-'}</td></tr>`;
                    });
                }
                html += `</tbody></table></div>`;
            });

            html += `<div class="print-footer-section">`;
            if(d.recs.trim()) html += `<strong>Recomendações:</strong><p style="white-space:pre-wrap; margin:3px 0 8px 0; font-size:9px;">${d.recs}</p>`;
            html += `<div class="legal-text">${isPEF ? legalPEF : legalTE}</div>
                <div style="text-align:center; font-size:7px; margin-top:10px;">PowFit Pro Cloud - Gerado em ${new Date().toLocaleDateString('pt-BR')}</div>
            </div>`;

            document.getElementById('print-area').innerHTML = html;
            setTimeout(() => window.print(), 500);
        }
    </script>
</body>
</html>
