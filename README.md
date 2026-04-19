<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro - Cloud & Network</title>
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

        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg-body);
            color: var(--text-main);
            transition: background-color 0.3s ease, color 0.3s ease;
        }

        .card {
            background-color: var(--bg-card);
            border: 1px solid var(--border-color);
            transition: background-color 0.3s ease, border-color 0.3s ease;
        }

        .input-field {
            background-color: var(--input-bg);
            border: 1px solid var(--input-border);
            color: var(--text-main);
        }
        
        .input-field:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 1px var(--primary);
        }

        .btn-primary {
            background-color: var(--primary);
            color: white;
            transition: background-color 0.2s;
        }

        .btn-primary:hover { background-color: var(--primary-hover); }

        /* Oculta áreas de impressão na tela */
        #print-area, #print-report-area { display: none; }

        /* Estilos de Impressão */
        @media print {
            body { background: white !important; color: black !important; margin: 0; padding: 0; }
            #app-container, #login-screen, .modal, .no-print { display: none !important; }
            
            #print-area, #print-report-area {
                padding: 10mm 15mm;
                font-family: 'Arial', sans-serif;
                font-size: 10px;
            }

            .print-active { display: block !important; }

            @page { size: A4; margin: 0; }
            
            .print-header { border-bottom: 2px solid #000; padding-bottom: 8px; margin-bottom: 12px; }
            .print-title { font-size: 18px; font-weight: bold; margin: 0; text-transform: uppercase; text-align: center;}
            .prof-info { font-size: 11px; margin-top: 5px; text-align: center; font-weight: bold;}
            
            .print-grid {
                display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 4px;
                margin-bottom: 15px; border: 1px solid #000; padding: 8px; background: #fff;
            }
            .print-grid div { font-size: 11px; }
            
            .print-guidelines { border: 1px solid #000; padding: 8px; margin-bottom: 15px; font-size: 9.5px; }
            .print-guidelines h4 { margin: 0 0 4px 0; font-size: 11px; text-transform: uppercase; background: #eee; padding: 3px;}
            .print-guidelines ul { margin: 0; padding-left: 15px; }
            
            .print-workout { margin-bottom: 15px; page-break-inside: avoid; }
            .print-workout h3 {
                background: #e0e0e0; border: 1px solid #000; border-bottom: none;
                padding: 4px 8px; margin: 0; font-size: 12px; text-transform: uppercase;
                -webkit-print-color-adjust: exact; color-adjust: exact;
            }
            table { width: 100%; border-collapse: collapse; margin-bottom: 0; border: 1px solid #000; }
            th, td { border: 1px solid #000; padding: 4px 6px; text-align: left; font-size: 10px; }
            th { background-color: #f0f0f0; font-weight: bold; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            
            .print-footer-section { margin-top: 15px; border: 1px solid #000; padding: 8px; font-size: 9px; page-break-inside: avoid;}
            .legal-text { font-size: 8px; color: #333; text-align: justify; margin-top: 5px; font-style: italic;}

            /* Estilo do Relatório */
            .report-table th { background: #333; color: #fff; }
        }

        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
    </style>
</head>
<body data-theme="Masculino" class="min-h-screen">

    <div id="login-screen" class="fixed inset-0 bg-[#0f172a] z-[100] flex flex-col items-center justify-center p-4">
        <div class="card p-8 rounded-2xl shadow-2xl max-w-md w-full text-center border border-gray-700">
            <i class="fas fa-dumbbell text-5xl text-blue-500 mb-4"></i>
            <h1 class="text-3xl font-bold text-white mb-2">PowFit Pro</h1>
            <p class="text-gray-400 text-sm mb-8">Plataforma Profissional de Treinos. Faça login para acessar sua rede e nuvem.</p>
            <button onclick="window.loginGoogle()" class="w-full bg-white text-gray-900 hover:bg-gray-100 font-bold py-3 px-4 rounded-lg flex items-center justify-center gap-3 transition">
                <img src="https://www.svgrepo.com/show/475656/google-color.svg" class="w-5 h-5" alt="Google">
                Entrar com Conta Google
            </button>
        </div>
    </div>

    <div id="app-container" class="hidden max-w-7xl mx-auto p-4 sm:p-6 lg:p-8 pb-20">
        
        <div class="flex flex-col xl:flex-row justify-between items-start xl:items-center mb-8 gap-4 border-b border-opacity-20 pb-4" style="border-color: var(--border-color)">
            <div class="flex items-center gap-3">
                <i class="fas fa-dumbbell text-3xl text-primary"></i>
                <div>
                    <h1 class="text-2xl font-bold tracking-tight">PowFit Pro <span class="text-xs bg-primary text-white px-2 py-0.5 rounded-full uppercase ml-2">Cloud</span></h1>
                    <p class="text-xs var(--text-muted)" id="display-network-info">Rede: Carregando...</p>
                </div>
            </div>
            <div class="flex flex-wrap gap-2 w-full xl:w-auto">
                <button onclick="window.openReport()" class="bg-indigo-600 hover:bg-indigo-500 text-white px-4 py-2 rounded-lg font-medium shadow flex items-center justify-center gap-2 transition flex-1 xl:flex-none text-sm">
                    <i class="fas fa-chart-bar"></i> Relatório
                </button>
                <button onclick="window.openHistory()" class="bg-gray-700 hover:bg-gray-600 text-white px-4 py-2 rounded-lg font-medium shadow flex items-center justify-center gap-2 transition flex-1 xl:flex-none text-sm">
                    <i class="fas fa-cloud"></i> Memória
                </button>
                <button onclick="window.openProfileSetup()" class="bg-gray-700 hover:bg-gray-600 text-white px-4 py-2 rounded-lg font-medium shadow flex items-center justify-center gap-2 transition flex-1 xl:flex-none text-sm">
                    <i class="fas fa-user-cog"></i> Perfil
                </button>
                <button onclick="window.logoutGoogle()" class="border border-red-500 text-red-500 hover:bg-red-500 hover:text-white px-4 py-2 rounded-lg font-medium shadow flex items-center justify-center gap-2 transition flex-1 xl:flex-none text-sm">
                    <i class="fas fa-sign-out-alt"></i> Sair
                </button>
            </div>
        </div>

        <div class="grid grid-cols-1 xl:grid-cols-12 gap-6">
            
            <div class="xl:col-span-4 space-y-6">
                
                <div class="flex flex-col gap-2">
                     <button onclick="window.generatePrint()" class="btn-primary w-full py-4 rounded-xl font-bold shadow-lg flex items-center justify-center gap-2 text-lg">
                        <i class="fas fa-save"></i> Salvar na Nuvem & Imprimir
                    </button>
                </div>

                <div class="card rounded-xl p-5 shadow-sm">
                    <div class="flex justify-between items-center mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <h2 class="text-lg font-semibold flex items-center gap-2">
                            <i class="fas fa-user text-primary"></i> Dados do Aluno
                        </h2>
                        <div id="imc-display"></div>
                    </div>
                    
                    <div class="space-y-4">
                        <div>
                            <label class="block text-xs font-medium mb-1">Nome Completo</label>
                            <input type="text" id="stu-name" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                        </div>
                        <div class="grid grid-cols-3 gap-3">
                            <div>
                                <label class="block text-xs font-medium mb-1">Idade</label>
                                <input type="number" id="stu-age" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                            </div>
                            <div>
                                <label class="block text-xs font-medium mb-1">Peso (kg)</label>
                                <input type="number" id="stu-weight" oninput="window.calculateIMC()" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                            </div>
                            <div>
                                <label class="block text-xs font-medium mb-1">Altura (m)</label>
                                <input type="number" id="stu-height" step="0.01" oninput="window.calculateIMC()" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                            </div>
                        </div>
                        <div class="grid grid-cols-2 gap-3">
                            <div>
                                <label class="block text-xs font-medium mb-1">Gênero</label>
                                <select id="stu-gender" onchange="window.changeTheme()" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                    <option value="Masculino">Masculino</option>
                                    <option value="Feminino">Feminino</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs font-medium mb-1">Nível</label>
                                <select id="stu-level" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                    <option>Iniciante</option>
                                    <option>Intermediário</option>
                                    <option>Avançado</option>
                                </select>
                            </div>
                        </div>
                        <div>
                            <label class="block text-xs font-medium mb-1">Objetivo</label>
                            <select id="stu-objective" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                <option value="Emagrecimento">Emagrecimento</option>
                                <option value="Hipertrofia">Hipertrofia</option>
                                <option value="Definição">Definição</option>
                                <option value="Condicionamento">Condicionamento</option>
                                <option value="Resistência">Resistência</option>
                                <option value="Força">Força</option>
                                <option value="Reabilitação">Reabilitação</option>
                                <option value="Saúde geral">Saúde geral</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-xs font-medium mb-1">Frequência</label>
                            <select id="stu-freq" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                <option>3 dias</option>
                                <option>5 dias</option>
                                <option>6 dias</option>
                                <option>Personalizado</option>
                            </select>
                        </div>
                    </div>
                </div>

                <div class="card rounded-xl p-5 shadow-sm">
                    <h2 class="text-lg font-semibold mb-2 flex items-center gap-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <i class="fas fa-heartbeat text-primary"></i> Estado de Saúde
                    </h2>
                    <div class="grid grid-cols-2 gap-2 text-xs" id="health-container">
                        </div>
                </div>

            </div>

            <div class="xl:col-span-8 space-y-6">
                
                <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center bg-opacity-10 p-4 rounded-xl card border-dashed border-2 gap-4">
                    <div>
                        <h2 class="text-xl font-bold flex items-center gap-2">
                            <i class="fas fa-list-alt text-primary"></i> Montagem da Ficha
                        </h2>
                        <div class="flex gap-2 mt-2">
                            <select id="stu-validity" class="input-field rounded px-2 py-1 text-xs">
                                <option>Validade: 15 dias</option>
                                <option selected>Validade: 30 dias</option>
                                <option>Validade: 60 dias</option>
                                <option>Validade: 90 dias</option>
                            </select>
                        </div>
                    </div>
                    <button onclick="window.addWorkout()" class="btn-primary px-4 py-2 rounded-lg text-sm font-medium flex items-center justify-center gap-2 w-full sm:w-auto">
                        <i class="fas fa-plus"></i> Adicionar Dia
                    </button>
                </div>

                <div id="workouts-container" class="space-y-6"></div>

                <div class="card rounded-xl p-5 shadow-sm">
                    <h2 class="text-sm font-semibold mb-2"><i class="fas fa-pen text-primary"></i> Recomendações Livres</h2>
                    <textarea id="stu-recs" rows="3" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Hidratação, descanso, cuidados específicos..."></textarea>
                </div>

            </div>
        </div>
    </div>

    <div id="profile-modal" class="fixed inset-0 bg-black bg-opacity-80 backdrop-blur-sm hidden z-[80] flex items-center justify-center p-4 modal">
        <div class="card w-full max-w-xl rounded-xl shadow-2xl flex flex-col">
            <div class="p-5 border-b" style="border-color: var(--border-color)">
                <h3 class="text-xl font-bold"><i class="fas fa-building text-primary mr-2"></i> Configuração da Rede & Profissional</h3>
                <p class="text-xs opacity-70 mt-1">Configure seus dados. Eles serão vinculados à sua conta Google.</p>
            </div>
            <div class="p-5 space-y-4 overflow-y-auto max-h-[70vh]">
                <div class="grid grid-cols-2 gap-4 border-b pb-4 border-gray-600 border-opacity-30">
                    <div>
                        <label class="block text-xs font-medium mb-1 text-gray-400">Nome da Rede</label>
                        <input type="text" id="prof-network" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="Ex: Power Fitness">
                    </div>
                    <div>
                        <label class="block text-xs font-medium mb-1 text-gray-400">Total de Unidades na Rede</label>
                        <input type="number" id="prof-net-count" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="Ex: 5">
                    </div>
                    <div class="col-span-2">
                        <label class="block text-xs font-medium mb-1 text-gray-400">Nome desta Unidade (Onde você atua)</label>
                        <input type="text" id="prof-unit" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="Ex: Unidade Centro">
                    </div>
                </div>

                <div>
                    <label class="block text-xs font-medium mb-1">Nome do Membro (Você)</label>
                    <input type="text" id="prof-name" class="input-field w-full rounded px-3 py-2 text-sm">
                </div>
                <div>
                    <label class="block text-xs font-medium mb-1">CPF</label>
                    <input type="text" id="prof-cpf" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="000.000.000-00">
                </div>
                <div>
                    <label class="block text-xs font-medium mb-1">Categoria de Atuação</label>
                    <select id="prof-type" onchange="window.toggleProfType()" class="input-field w-full rounded px-3 py-2 text-sm">
                        <option value="PEF">Profissional de Educação Física</option>
                        <option value="TE">Treinador Esportivo</option>
                    </select>
                </div>
                <div class="grid grid-cols-2 gap-4" id="cref-container">
                    <div>
                        <label class="block text-xs font-medium mb-1">CREF</label>
                        <input type="text" id="prof-cref" class="input-field w-full rounded px-3 py-2 text-sm">
                    </div>
                    <div>
                        <label class="block text-xs font-medium mb-1">Estado</label>
                        <input type="text" id="prof-state" class="input-field w-full rounded px-3 py-2 text-sm">
                    </div>
                </div>
            </div>
            <div class="p-4 border-t bg-black bg-opacity-20 flex justify-end gap-3" style="border-color: var(--border-color)">
                <button onclick="window.closeProfileSetup()" class="px-4 py-2 rounded text-sm hover:bg-gray-700 transition">Cancelar</button>
                <button onclick="window.saveProfileSetup()" class="btn-primary px-6 py-2 rounded text-sm font-bold shadow">Salvar Dados</button>
            </div>
        </div>
    </div>

    <div id="exercise-modal" class="fixed inset-0 bg-black bg-opacity-80 backdrop-blur-sm hidden z-[60] flex items-center justify-center p-2 sm:p-4 modal">
        <div class="card w-full max-w-5xl rounded-xl shadow-2xl flex flex-col h-[90vh]">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold"><i class="fas fa-search text-primary mr-2"></i> Adicionar Exercício</h3>
                <button onclick="window.closeModal()" class="text-gray-400 hover:text-red-500 transition-colors text-2xl leading-none">&times;</button>
            </div>
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <div class="w-full md:w-1/4 border-r overflow-y-auto p-2 space-y-1" style="border-color: var(--border-color)" id="modal-categories"></div>
                <div class="w-full md:w-3/4 overflow-y-auto p-3 bg-black bg-opacity-5" id="modal-exercises"></div>
            </div>
        </div>
    </div>

    <div id="history-modal" class="fixed inset-0 bg-black bg-opacity-80 backdrop-blur-sm hidden z-[70] flex items-center justify-center p-2 sm:p-4 modal">
        <div class="card w-full max-w-4xl rounded-xl shadow-2xl flex flex-col max-h-[90vh]">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold"><i class="fas fa-cloud text-primary mr-2"></i> Fichas na Nuvem</h3>
                <button onclick="window.closeHistory()" class="text-gray-400 hover:text-red-500 transition-colors text-2xl leading-none">&times;</button>
            </div>
            <div class="p-4 overflow-y-auto flex-1 space-y-3" id="history-list">
                <div class="text-center py-10"><i class="fas fa-spinner fa-spin text-3xl text-primary"></i></div>
            </div>
        </div>
    </div>

    <div id="report-modal" class="fixed inset-0 bg-black bg-opacity-80 backdrop-blur-sm hidden z-[75] flex items-center justify-center p-2 sm:p-4 modal">
        <div class="card w-full max-w-2xl rounded-xl shadow-2xl flex flex-col">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold"><i class="fas fa-chart-bar text-primary mr-2"></i> Relatório da Unidade</h3>
                <button onclick="window.closeReport()" class="text-gray-400 hover:text-red-500 transition-colors text-2xl leading-none">&times;</button>
            </div>
            <div class="p-6 text-center" id="report-content">
                </div>
            <div class="p-4 border-t flex justify-end bg-black bg-opacity-10" style="border-color: var(--border-color)">
                <button onclick="window.printReport()" class="btn-primary px-6 py-2 rounded font-bold"><i class="fas fa-print"></i> Imprimir Relatório</button>
            </div>
        </div>
    </div>

    <div id="print-area"></div>
    <div id="print-report-area"></div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-app.js";
        import { getAuth, signInWithPopup, GoogleAuthProvider, signOut, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-auth.js";
        import { getFirestore, collection, addDoc, getDocs, doc, setDoc, getDoc, deleteDoc, query, orderBy } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-firestore.js";

        // Configuração Firebase solicitada
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

        // BANCO DE DADOS LOCAL
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
            "🟢 Saudável": "Manter treinos regulares de 3–5x por semana. Constância, descanso e boa alimentação contribuem para resultados.",
            "⚪ Sedentário": "Início gradual, treinos leves e foco na adaptação do corpo. Aprender execução correta.",
            "🟡 Sobrepeso": "Musculação + cardio para melhora do condicionamento. Progressão respeitando a individualidade.",
            "🔴 Obesidade": "Exercícios de menor impacto e progressão gradual. Segurança e mobilidade são prioridades.",
            "⚖️ Baixo peso": "Foco em evolução progressiva e recuperação muscular com alimentação adequada.",
            "🍬 Diabetes": "Acompanhamento médico regular. Interromper em caso de tontura, mal-estar ou alteração de glicemia.",
            "❤️ Hipertensão": "Acompanhamento médico. Evitar prender a respiração (Valsalva) e controlar a intensidade.",
            "🔵 Hipotensão": "Evitar mudanças bruscas de posição. Manter hidratação e respeitar a intensidade adequada.",
            "💔 Problemas cardíacos": "Apenas com liberação médica. Controle estrito de intensidade.",
            "🦴 Problemas articulares": "Menor impacto e maior controle de movimento. Adaptação dos exercícios ajuda na segurança.",
            "🫁 Problemas respiratórios": "Progressão gradual. Interromper em caso de falta de ar excessiva.",
            "⚠️ Lesões": "Respeitar a limitação existente e evitar dor. Seguir orientação profissional.",
            "🤰 Gestante": "Liberação médica. Foco na segurança, mobilidade e bem-estar, evitando impacto.",
            "🤱 Lactante": "Atividade mantida normalmente, respeitando recuperação, hidratação e alimentação.",
            "👴 Idoso": "Força, equilíbrio, mobilidade. Respeitar limitações com intensidade moderada e foco na segurança."
        };

        const objectiveData = {
            "Emagrecimento": "Déficit calórico. Treinos mistos de força (manutenção muscular) e aeróbicos (gasto calórico).",
            "Hipertrofia": "Progressão de carga e volume adequado. Superávit calórico e descanso são essenciais.",
            "Definição": "Manutenção de massa muscular enquanto reduz percentual de gordura. Atenção estrita à dieta.",
            "Condicionamento": "Menor tempo de intervalo, circuitos e alta integração cardiopulmonar.",
            "Resistência": "Séries longas, cadência controlada e aprimoramento da capacidade muscular.",
            "Força": "Cargas altas, baixas repetições e intervalos de descanso maiores.",
            "Reabilitação": "Fortalecimento específico, mobilidade e controle motor. Respeitar limites rigorosamente.",
            "Saúde geral": "Equilíbrio entre força, cardio e flexibilidade. Foco na constância."
        };

        const dbCategories = {
            "🔥 PEITO": [
                "Supino Reto", "Supino Inclinado", "Supino Inclinado com Halteres", "Supino Fechado com Halteres", 
                "Cross Over", "Cross Over Alto", "Cross Over Baixo", "Crucifixo Reto", "Crucifixo Inclinado com Halteres", 
                "Crucifixo na Máquina", "Peck Fly", "Peck Fly Unilateral", "Pullover", 
                "Flexão de Braço", "Flexão com Pés Elevados", "Flexão Explosiva"
            ],
            "🦍 COSTAS": [
                "Puxada Alta", "Puxada de Frente Supinada", "Pulldown", "Remada Aberta", "Remada Baixa", 
                "Remada Curvada", "Remada Curvada Supinada", "Remada Cavalinho (T-Bar)", "Serrote", 
                "Facepull (puxada de cima para baixo)"
            ],
            "🦵 PERNAS": [
                "Agachamento Livre", "Agachamento Taça", "Agachamento no Smith", "Agachamento com passada lateral", 
                "Squat", "Hack Machine", "Leg 45°", "Leg 90°", "Agachamento Sumô", "Agachamento Sissy (Livre)", 
                "Afundo", "Recuo", "Avanço", "Passada", "Búlgaro", "Step-up", "Levantamento Terra", 
                "Levantamento Terra Romeno", "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", "Cadeira Flexora", 
                "Elevação Pélvica no Banco", "Elevação Pélvica no Chão", "Elevação Pélvica Unilateral no Chão", 
                "Extensão de Quadril (Glúteo Máximo)", "Extensão Cruzada (Glúteo Médio)", "Coice", "Cachorrinho", 
                "Cadeira Extensora", "Adução", "Abdução", "Abdução Inclinada", "Flexão Nórdica", "Flexão Nórdica Invertida", 
                "Panturrilha Livre", "Panturrilha no Leg Press", "Panturrilha Banco", "Panturrilha Squat", "Panturrilha Unilateral"
            ],
            "💪 BRAÇOS": [
                "(Bíceps) Rosca Direta", "(Bíceps) Rosca Alternada", "(Bíceps) Rosca 21", "(Bíceps) Rosca Scott Barra W", 
                "(Bíceps) Rosca Scott Unilateral", "(Bíceps) Rosca Scott com Halteres", "(Bíceps) Rosca Martelo", 
                "(Bíceps) Rosca Cross", "(Bíceps) Rosca Inversa", "(Bíceps) Rosca 45°",
                "(Tríceps) Pulley Unilateral", "(Tríceps) Pulley Barra", "(Tríceps) Pulley Corda", "(Tríceps) Pulley Pegada Inversa", 
                "(Tríceps) Francês na Corda", "(Tríceps) Francês com Halter", "(Tríceps) Francês Unilateral", 
                "(Tríceps) Cruzado Polia Dupla", "(Tríceps) Coice Unilateral", "(Tríceps) Arremesso", "(Tríceps) Testa", "(Tríceps) Mergulho no Banco"
            ],
            "🪨 OMBROS": [
                "Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", "Elevação Lateral Unilateral Cross", 
                "Elevação Lateral Sentado", "Desenvolvimento com Halteres", "Desenvolvimento com Barra", "Arnold Press", 
                "Elevação Borboleta", "Crucifixo Inverso Sentado com Halteres", "Crucifixo Inverso na Polia", 
                "Crucifixo Inverso Unilateral na Polia", "Facepull (puxada reta)", "Remada Alta", "Encolhimento (Trapézio)"
            ],
            "🧠 ABDÔMEN": [
                "Infra com Elevação de Perna", "Abdominal Supra", "Abdominal Remador", "Abdominal Bicicleta", 
                "Abdominal Twister com Peso", "Prancha", "Prancha Lateral", "Trituração de Cabos em Pé", "Isometria na parede"
            ],
            "🫀 CARDIO": [
                "Bicicleta 10 Minutos", "Bicicleta 15 Minutos", "Bicicleta 20 Minutos",
                "Esteira 10 Minutos", "Esteira 15 Minutos", "Esteira 20 Minutos", "Pular Corda"
            ]
        };

        const dbTechniques = ["Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", "Negativa", "Isometria", "Parciais", "Pirâmide"];
        const daysOfWeek = ["SEGUNDA-FEIRA", "TERÇA-FEIRA", "QUARTA-FEIRA", "QUINTA-FEIRA", "SEXTA-FEIRA", "SÁBADO", "DOMINGO"];

        let state = {
            currentUser: null,
            userProfileData: {},
            currentWorkoutDocId: null,
            workouts: [],
            activeModalWorkoutId: null,
            activeCategory: "🔥 PEITO"
        };

        // --- AUTH & SETUP LOGIC ---
        onAuthStateChanged(auth, async (user) => {
            if (user) {
                state.currentUser = user;
                document.getElementById('login-screen').classList.add('hidden');
                document.getElementById('app-container').classList.remove('hidden');
                await loadUserProfile();
            } else {
                state.currentUser = null;
                document.getElementById('login-screen').classList.remove('hidden');
                document.getElementById('app-container').classList.add('hidden');
            }
        });

        window.loginGoogle = () => {
            const provider = new GoogleAuthProvider();
            signInWithPopup(auth, provider).catch(error => alert("Erro ao fazer login: " + error.message));
        };

        window.logoutGoogle = () => {
            signOut(auth).then(() => {
                state.workouts = [];
                state.currentWorkoutDocId = null;
                renderWorkouts();
            });
        };

        async function loadUserProfile() {
            try {
                const docRef = doc(db, "users", state.currentUser.uid);
                const docSnap = await getDoc(docRef);
                if (docSnap.exists()) {
                    state.userProfileData = docSnap.data();
                    updateUIDisplay();
                } else {
                    window.openProfileSetup();
                }
                
                if(state.workouts.length === 0) window.addWorkout();
                
            } catch (error) { console.error("Erro ao carregar perfil:", error); }
        }

        function updateUIDisplay() {
            const p = state.userProfileData;
            document.getElementById('display-network-info').innerText = `Rede: ${p.networkName || 'Não definida'} | Unidade: ${p.unitName || 'Não definida'} | Profissional: ${p.memberName || state.currentUser.email}`;
            
            document.getElementById('prof-network').value = p.networkName || '';
            document.getElementById('prof-net-count').value = p.networkCount || '';
            document.getElementById('prof-unit').value = p.unitName || '';
            document.getElementById('prof-name').value = p.memberName || '';
            document.getElementById('prof-cpf').value = p.cpf || '';
            document.getElementById('prof-type').value = p.profType || 'PEF';
            document.getElementById('prof-cref').value = p.cref || '';
            document.getElementById('prof-state').value = p.state || '';
            
            window.toggleProfType();
        }

        window.openProfileSetup = () => { document.getElementById('profile-modal').classList.remove('hidden'); };
        window.closeProfileSetup = () => { document.getElementById('profile-modal').classList.add('hidden'); };

        window.saveProfileSetup = async () => {
            const data = {
                networkName: document.getElementById('prof-network').value,
                networkCount: document.getElementById('prof-net-count').value,
                unitName: document.getElementById('prof-unit').value,
                memberName: document.getElementById('prof-name').value,
                cpf: document.getElementById('prof-cpf').value,
                profType: document.getElementById('prof-type').value,
                cref: document.getElementById('prof-cref').value,
                state: document.getElementById('prof-state').value,
                updatedAt: new Date().toISOString()
            };
            try {
                await setDoc(doc(db, "users", state.currentUser.uid), data, { merge: true });
                state.userProfileData = data;
                updateUIDisplay();
                window.closeProfileSetup();
            } catch(e) { alert("Erro ao salvar perfil: " + e.message); }
        };

        window.toggleProfType = () => {
            const type = document.getElementById('prof-type').value;
            const crefContainer = document.getElementById('cref-container');
            if(type === 'TE') {
                crefContainer.classList.add('hidden');
                window.renderHealthOptions(healthDataTE);
            } else {
                crefContainer.classList.remove('hidden');
                window.renderHealthOptions(healthDataPEF);
            }
        };

        window.renderHealthOptions = (dataObj) => {
            const container = document.getElementById('health-container');
            const selected = Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value);
            container.innerHTML = Object.keys(dataObj).map(opt => {
                const isChecked = selected.includes(opt) ? 'checked' : '';
                return `<label class="flex items-start space-x-2 cursor-pointer p-1.5 rounded hover:bg-black hover:bg-opacity-5 transition border border-transparent hover:border-gray-500 hover:border-opacity-20">
                    <input type="checkbox" value="${opt}" ${isChecked} class="health-cb mt-0.5 rounded border-gray-400 text-primary focus:ring-primary w-3.5 h-3.5">
                    <span class="font-medium">${opt}</span>
                </label>`
            }).join('');
        };

        // --- LÓGICA DA INTERFACE DE TREINOS ---
        window.changeTheme = () => { document.body.setAttribute('data-theme', document.getElementById('stu-gender').value); };
        window.calculateIMC = () => {
            const w = parseFloat(document.getElementById('stu-weight').value);
            const h = parseFloat(document.getElementById('stu-height').value);
            const d = document.getElementById('imc-display');
            if (w > 0 && h > 0) {
                const imc = (w / (h * h)).toFixed(1);
                let cls = imc < 18.5 ? "Abaixo" : imc < 24.9 ? "Normal" : imc < 29.9 ? "Sobrepeso" : "Obesidade";
                d.innerHTML = `<span class="bg-primary bg-opacity-20 text-primary px-3 py-1 rounded-full text-xs font-bold border border-primary border-opacity-30">IMC: ${imc} (${cls})</span>`;
            } else { d.innerHTML = ''; }
        };

        window.addWorkout = () => {
            const count = state.workouts.length;
            const title = count < 7 ? `TREINO ${daysOfWeek[count]}` : `NOVO TREINO ${count + 1}`;
            state.workouts.push({ id: Math.random().toString(36).substr(2, 9), title: title, exercises: [] });
            renderWorkouts();
        };

        window.removeWorkout = (id) => {
            if(confirm("Excluir este dia?")) { state.workouts = state.workouts.filter(w => w.id !== id); renderWorkouts(); }
        };
        
        window.duplicateWorkout = (id) => {
            const w = state.workouts.find(x => x.id === id);
            if(w) {
                const nw = JSON.parse(JSON.stringify(w));
                nw.id = Math.random().toString(36).substr(2, 9);
                nw.title += " (Cópia)";
                state.workouts.push(nw);
                renderWorkouts();
            }
        };

        window.updateWorkoutTitle = (id, val) => { const w = state.workouts.find(x => x.id === id); if(w) w.title = val; };
        
        window.removeExercise = (wId, exIdx) => { const w = state.workouts.find(x => x.id === wId); w.exercises.splice(exIdx, 1); renderWorkouts(); };
        window.moveExercise = (wId, exIdx, dir) => {
            const w = state.workouts.find(x => x.id === wId);
            if (dir === 'up' && exIdx > 0) { const temp = w.exercises[exIdx]; w.exercises[exIdx] = w.exercises[exIdx-1]; w.exercises[exIdx-1] = temp; }
            else if (dir === 'down' && exIdx < w.exercises.length - 1) { const temp = w.exercises[exIdx]; w.exercises[exIdx] = w.exercises[exIdx+1]; w.exercises[exIdx+1] = temp; }
            renderWorkouts();
        };
        window.updateExercise = (wId, exIdx, field, val) => { const w = state.workouts.find(x => x.id === wId); if(w && w.exercises[exIdx]) w.exercises[exIdx][field] = val; };

        function renderWorkouts() {
            const container = document.getElementById('workouts-container');
            container.innerHTML = '';
            state.workouts.forEach(w => {
                let html = w.exercises.length === 0 ? `<div class="text-center p-4 text-xs opacity-50 italic">Sem exercícios.</div>` : 
                w.exercises.map((ex, idx) => `
                    <div class="flex flex-col sm:flex-row gap-2 p-2 items-start sm:items-center border-b last:border-0 border-opacity-10" style="border-color: var(--border-color)">
                        <div class="flex gap-1 hidden sm:flex">
                            <button onclick="window.moveExercise('${w.id}', ${idx}, 'up')" class="text-[10px] p-1 rounded hover:bg-black hover:bg-opacity-10"><i class="fas fa-chevron-up"></i></button>
                            <button onclick="window.moveExercise('${w.id}', ${idx}, 'down')" class="text-[10px] p-1 rounded hover:bg-black hover:bg-opacity-10"><i class="fas fa-chevron-down"></i></button>
                        </div>
                        <div class="flex-1 font-medium w-full sm:w-auto">
                            <div class="text-[9px] opacity-60 uppercase tracking-wider">${ex.category}</div>
                            <div class="text-xs">${ex.name}</div>
                        </div>
                        <div class="flex flex-wrap gap-1 w-full sm:w-auto">
                            <input type="text" value="${ex.sets}" onchange="window.updateExercise('${w.id}', ${idx}, 'sets', this.value)" class="input-field w-12 rounded px-1 py-1 text-xs text-center" placeholder="Séries">
                            <span class="self-center opacity-50 text-xs">x</span>
                            <input type="text" value="${ex.reps}" onchange="window.updateExercise('${w.id}', ${idx}, 'reps', this.value)" class="input-field w-16 rounded px-1 py-1 text-xs text-center" placeholder="Reps">
                            <select onchange="window.updateExercise('${w.id}', ${idx}, 'technique', this.value)" class="input-field w-24 rounded px-1 py-1 text-[10px]">
                                ${dbTechniques.map(t => `<option value="${t}" ${ex.technique === t ? 'selected' : ''}>${t}</option>`).join('')}
                            </select>
                            <input type="text" value="${ex.obs}" onchange="window.updateExercise('${w.id}', ${idx}, 'obs', this.value)" class="input-field flex-1 sm:w-32 rounded px-1 py-1 text-xs" placeholder="Obs...">
                        </div>
                        <div class="flex items-center gap-1 w-full sm:w-auto justify-end mt-1 sm:mt-0">
                            <button onclick="window.removeExercise('${w.id}', ${idx})" class="text-red-500 hover:text-red-700 p-1 rounded text-sm"><i class="fas fa-trash"></i></button>
                        </div>
                    </div>`).join('');
                
                container.insertAdjacentHTML('beforeend', `
                    <div class="card rounded-xl overflow-hidden shadow-sm">
                        <div class="p-3 border-b flex justify-between items-center bg-black bg-opacity-5" style="border-color: var(--border-color);">
                            <input type="text" value="${w.title}" onchange="window.updateWorkoutTitle('${w.id}', this.value)" class="input-field bg-transparent font-bold text-sm w-1/2 px-2 py-1 rounded uppercase">
                            <div class="flex gap-1">
                                <button onclick="window.duplicateWorkout('${w.id}')" class="text-xs p-1.5 rounded hover:bg-black hover:bg-opacity-10 transition"><i class="fas fa-copy"></i></button>
                                <button onclick="window.removeWorkout('${w.id}')" class="text-xs p-1.5 text-red-500 rounded hover:bg-red-500 hover:text-white transition"><i class="fas fa-trash"></i></button>
                            </div>
                        </div>
                        <div class="p-0">${html}</div>
                        <div class="p-2 border-t" style="border-color: var(--border-color)">
                            <button onclick="window.openModal('${w.id}')" class="w-full text-primary py-1.5 rounded-lg text-xs font-semibold hover:bg-primary hover:bg-opacity-10 transition">+ ADICIONAR EXERCÍCIO</button>
                        </div>
                    </div>`);
            });
        }

        window.openModal = (id) => { state.activeModalWorkoutId = id; document.getElementById('exercise-modal').classList.remove('hidden'); window.renderModalCategories(); window.renderModalExercises(); };
        window.closeModal = () => { document.getElementById('exercise-modal').classList.add('hidden'); state.activeModalWorkoutId = null; };
        
        window.renderModalCategories = () => {
            document.getElementById('modal-categories').innerHTML = Object.keys(dbCategories).map(cat => `
                <button onclick="window.setModalCategory('${cat}')" class="w-full text-left px-2 py-2 rounded-lg text-xs font-medium transition ${state.activeCategory === cat ? 'bg-primary text-white' : 'hover:bg-black hover:bg-opacity-10'}">${cat}</button>
            `).join('');
        };
        window.setModalCategory = (cat) => { state.activeCategory = cat; window.renderModalCategories(); window.renderModalExercises(); };
        window.renderModalExercises = () => {
            document.getElementById('modal-exercises').innerHTML = `<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-2">
                ${dbCategories[state.activeCategory].map(ex => `<button onclick="window.addExerciseToWorkout('${ex}', ${state.activeCategory === '🫀 CARDIO'})" class="card p-2 rounded-lg text-left text-xs font-medium border hover:border-primary transition flex justify-between items-center group"><span>${ex}</span><i class="fas fa-plus opacity-0 group-hover:opacity-100 transition text-primary"></i></button>`).join('')}
            </div>`;
        };
        window.addExerciseToWorkout = (exName, isCardio) => {
            const w = state.workouts.find(x => x.id === state.activeModalWorkoutId);
            if(w) {
                w.exercises.push({ category: state.activeCategory, name: exName, sets: isCardio ? '1' : '3', reps: isCardio ? '-' : '10 a 12', technique: 'Nenhuma', obs: '' });
                renderWorkouts();
                document.getElementById('modal-exercises').style.opacity = '0.5';
                setTimeout(() => document.getElementById('modal-exercises').style.opacity = '1', 100);
            }
        };

        // --- NUVEM: SALVAR E HISTÓRICO ---
        async function saveToCloud() {
            if(!state.currentUser) return alert("Faça login para salvar!");
            const d = {
                dateSaved: new Date().toISOString(),
                monthYear: new Date().toISOString().substring(0,7), // YYYY-MM
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
                workouts: state.workouts,
                networkName: state.userProfileData.networkName || '',
                unitName: state.userProfileData.unitName || ''
            };

            try {
                if(state.currentWorkoutDocId) {
                    await setDoc(doc(db, "users", state.currentUser.uid, "workouts", state.currentWorkoutDocId), d);
                } else {
                    const docRef = await addDoc(collection(db, "users", state.currentUser.uid, "workouts"), d);
                    state.currentWorkoutDocId = docRef.id;
                }
            } catch(e) { console.error("Erro ao salvar:", e); alert("Erro ao salvar na nuvem."); }
        }

        window.openHistory = async () => {
            document.getElementById('history-modal').classList.remove('hidden');
            const list = document.getElementById('history-list');
            list.innerHTML = '<div class="text-center py-10"><i class="fas fa-spinner fa-spin text-3xl text-primary"></i></div>';
            
            try {
                const q = query(collection(db, "users", state.currentUser.uid, "workouts"), orderBy("dateSaved", "desc"));
                const querySnapshot = await getDocs(q);
                
                if(querySnapshot.empty) {
                    list.innerHTML = `<div class="text-center opacity-50 py-10">Nenhuma ficha salva na nuvem.</div>`;
                    return;
                }

                let html = '';
                querySnapshot.forEach((docSnap) => {
                    const h = docSnap.data();
                    const dStr = new Date(h.dateSaved).toLocaleString('pt-BR');
                    html += `
                        <div class="card p-4 rounded-lg flex justify-between items-center border border-gray-600 border-opacity-30">
                            <div>
                                <div class="font-bold">${h.studentName}</div>
                                <div class="text-xs opacity-70">Salvo: ${dStr} | Unidade: ${h.unitName}</div>
                            </div>
                            <div class="flex gap-2">
                                <button onclick="window.loadHistoryItem('${docSnap.id}')" class="btn-primary px-3 py-1.5 rounded text-xs font-semibold">CARREGAR</button>
                                <button onclick="window.deleteHistoryItem('${docSnap.id}')" class="bg-red-500 hover:bg-red-600 text-white px-3 py-1.5 rounded text-xs"><i class="fas fa-trash"></i></button>
                            </div>
                        </div>`;
                });
                list.innerHTML = html;
            } catch(e) { list.innerHTML = 'Erro ao carregar histórico.'; console.error(e); }
        };

        window.closeHistory = () => { document.getElementById('history-modal').classList.add('hidden'); };

        window.loadHistoryItem = async (docId) => {
            try {
                const docSnap = await getDoc(doc(db, "users", state.currentUser.uid, "workouts", docId));
                if(docSnap.exists()) {
                    const item = docSnap.data();
                    state.currentWorkoutDocId = docId;
                    
                    document.getElementById('stu-name').value = item.studentName || '';
                    document.getElementById('stu-age').value = item.stuAge || '';
                    document.getElementById('stu-weight').value = item.stuWeight || '';
                    document.getElementById('stu-height').value = item.stuHeight || '';
                    document.getElementById('stu-gender').value = item.stuGender || 'Masculino';
                    document.getElementById('stu-level').value = item.stuLevel || 'Iniciante';
                    document.getElementById('stu-objective').value = item.stuObjective || 'Emagrecimento';
                    document.getElementById('stu-freq').value = item.stuFreq || '3 dias';
                    document.getElementById('stu-validity').value = item.stuValidity || 'Validade: 30 dias';
                    document.getElementById('stu-recs').value = item.stuRecs || '';
                    
                    window.changeTheme();
                    window.calculateIMC();

                    document.querySelectorAll('.health-cb').forEach(cb => cb.checked = (item.health || []).includes(cb.value));

                    state.workouts = item.workouts || [];
                    renderWorkouts();
                    window.closeHistory();
                }
            } catch(e) { alert("Erro ao carregar ficha."); }
        };

        window.deleteHistoryItem = async (docId) => {
            if(confirm("Excluir esta ficha permanentemente da nuvem?")) {
                await deleteDoc(doc(db, "users", state.currentUser.uid, "workouts", docId));
                window.openHistory();
                if(state.currentWorkoutDocId === docId) state.currentWorkoutDocId = null;
            }
        };

        // --- RELATÓRIO MENSAL ---
        window.openReport = async () => {
            document.getElementById('report-modal').classList.remove('hidden');
            const cont = document.getElementById('report-content');
            cont.innerHTML = '<i class="fas fa-spinner fa-spin text-3xl text-primary"></i> Gerando...';
            
            try {
                const q = query(collection(db, "users", state.currentUser.uid, "workouts"));
                const querySnapshot = await getDocs(q);
                
                const currentMonth = new Date().toISOString().substring(0,7);
                let currentMonthCount = 0;
                let totalCount = 0;

                querySnapshot.forEach((docSnap) => {
                    totalCount++;
                    if(docSnap.data().monthYear === currentMonth) currentMonthCount++;
                });

                const p = state.userProfileData;
                const dateStr = new Date().toLocaleDateString('pt-BR', { month: 'long', year: 'numeric' });

                let html = `
                    <h2 class="text-xl font-bold uppercase mb-1">${p.networkName || 'Rede Não Definida'}</h2>
                    <h3 class="text-md font-medium text-gray-400 mb-6">${p.unitName || 'Unidade Não Definida'}</h3>
                    
                    <div class="grid grid-cols-2 gap-4 mb-6">
                        <div class="card p-4 rounded border border-primary border-opacity-50">
                            <div class="text-sm opacity-70">Fichas Geradas (${dateStr})</div>
                            <div class="text-3xl font-bold text-primary">${currentMonthCount}</div>
                        </div>
                        <div class="card p-4 rounded border border-gray-600 border-opacity-50">
                            <div class="text-sm opacity-70">Total Geral Acumulado</div>
                            <div class="text-3xl font-bold">${totalCount}</div>
                        </div>
                    </div>

                    <table class="w-full text-left border-collapse text-sm report-table border border-gray-700">
                        <tr><th class="p-2 border border-gray-700">Profissional</th><td class="p-2 border border-gray-700">${p.memberName || '-'}</td></tr>
                        <tr><th class="p-2 border border-gray-700">Categoria</th><td class="p-2 border border-gray-700">${p.profType === 'TE' ? 'Treinador Esportivo' : 'Ed. Física'}</td></tr>
                        <tr><th class="p-2 border border-gray-700">Documento</th><td class="p-2 border border-gray-700">${p.profType === 'TE' ? 'CPF: '+p.cpf : 'CREF: '+p.cref}</td></tr>
                    </table>
                `;
                cont.innerHTML = html;
                document.getElementById('print-report-area').innerHTML = `<div class="print-header"><h1 class="print-title">RELATÓRIO DE FICHAS MENSAIS</h1></div>` + html;

            } catch(e) { cont.innerHTML = 'Erro ao gerar relatório.'; }
        };
        window.closeReport = () => { document.getElementById('report-modal').classList.add('hidden'); };
        window.printReport = () => {
            document.getElementById('print-report-area').classList.add('print-active');
            window.print();
            setTimeout(() => document.getElementById('print-report-area').classList.remove('print-active'), 500);
        };

        // --- IMPRESSÃO DA FICHA ---
        window.generatePrint = async () => {
            await saveToCloud(); // Salva na nuvem antes de imprimir

            const p = state.userProfileData;
            const w = parseFloat(document.getElementById('stu-weight').value);
            const h = parseFloat(document.getElementById('stu-height').value);
            let imcStr = "-"; if(w>0 && h>0) imcStr = (w / (h * h)).toFixed(1);

            const obj = document.getElementById('stu-objective').value;
            const objRecommendation = objectiveData[obj] || "";
            const isPEF = p.profType === 'PEF';
            const healthSourceDict = isPEF ? healthDataPEF : healthDataTE;
            const selectedHealthBoxes = Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value);
            
            const profLabel = isPEF ? 'Profissional de Educação Física' : 'Treinador Esportivo';
            const crefText = isPEF ? `CREF: ${p.cref || '---'} - Estado: ${p.state || '---'}` : '';
            
            let legalTextPEF = `⚠️ OBSERVAÇÃO LEGAL – PROFISSIONAL DE EDUCAÇÃO FÍSICA: Conforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º estabelece que compete ao profissional coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos.`;
            let legalTextTE = `⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO: Conforme a Lei nº 14.597/2023 (Lei Geral do Esporte), Art. 75, a profissão de treinador esportivo é reconhecida e regulada no Brasil. As recomendações desta ficha possuem caráter informativo e orientativo, voltadas ao treinamento esportivo e prática de musculação. Este material não substitui avaliação médica ou acompanhamento de profissionais da saúde. Em casos de dores, lesões, doenças, gestação ou condição específica, recomenda-se procurar um profissional habilitado antes de iniciar ou continuar os treinos.`;

            let html = `
                <div class="print-header">
                    <h1 class="print-title">PLANILHA DE TREINO - ${obj}</h1>
                    <div class="prof-info">
                        Prescrição feita por: ${p.memberName || 'Profissional'} (${profLabel})<br>${crefText}
                    </div>
                </div>

                <div class="print-grid">
                    <div><strong>Aluno:</strong> ${document.getElementById('stu-name').value || '-'}</div>
                    <div><strong>Idade:</strong> ${document.getElementById('stu-age').value || '-'} anos</div>
                    <div><strong>Gênero:</strong> ${document.getElementById('stu-gender').value}</div>
                    
                    <div><strong>Peso:</strong> ${w || '-'} kg</div>
                    <div><strong>Altura:</strong> ${h || '-'} m</div>
                    <div><strong>IMC:</strong> ${imcStr}</div>
                    
                    <div><strong>Nível:</strong> ${document.getElementById('stu-level').value}</div>
                    <div><strong>Frequência:</strong> ${document.getElementById('stu-freq').value}</div>
                    <div><strong>Validade:</strong> ${document.getElementById('stu-validity').value.replace('Validade: ', '')}</div>
                </div>
            `;

            if (objRecommendation || selectedHealthBoxes.length > 0) {
                html += `<div class="print-guidelines"><h4>Diretrizes de Perfil Automáticas</h4><ul>`;
                if(objRecommendation) html += `<li><strong>Objetivo (${obj}):</strong> ${objRecommendation}</li>`;
                selectedHealthBoxes.forEach(healthKey => {
                    if(healthSourceDict[healthKey]) {
                        const cleanKey = healthKey.replace(/[\u{1F300}-\u{1F9FF}\u{2600}-\u{26FF}\u{2700}-\u{27BF}\u{FE0F}]/gu, '').trim();
                        html += `<li><strong>Saúde (${cleanKey}):</strong> ${healthSourceDict[healthKey]}</li>`;
                    }
                });
                html += `</ul></div>`;
            }

            state.workouts.forEach(wk => {
                html += `
                    <div class="print-workout">
                        <h3>${wk.title}</h3>
                        <table>
                            <thead><tr><th style="width: 35%">Exercício</th><th style="width: 8%; text-align:center;">Séries</th><th style="width: 12%; text-align:center;">Reps</th><th style="width: 15%">Técnica</th><th style="width: 30%">Observações</th></tr></thead>
                            <tbody>
                `;
                if (wk.exercises.length === 0) {
                    html += `<tr><td colspan="5" style="text-align:center; font-style:italic;">Sem exercícios</td></tr>`;
                } else {
                    wk.exercises.forEach(ex => {
                        const techStr = ex.technique !== 'Nenhuma' ? ex.technique : '-';
                        html += `<tr><td><strong>${ex.name}</strong></td><td style="text-align:center;">${ex.sets}</td><td style="text-align:center;">${ex.reps}</td><td>${techStr}</td><td>${ex.obs || '-'}</td></tr>`;
                    });
                }
                html += `</tbody></table></div>`;
            });

            const recs = document.getElementById('stu-recs').value;
            html += `<div class="print-footer-section">`;
            if(recs.trim()) { html += `<strong>Recomendações Específicas do Profissional:</strong><br><p style="white-space: pre-wrap; margin: 3px 0 10px 0;">${recs}</p>`; }
            html += `<div class="legal-text">${isPEF ? legalTextPEF : legalTextTE}</div>
                     <div style="text-align:center; margin-top: 10px; font-size: 8px;">Gerado em ${new Date().toLocaleString('pt-BR')} via PowFit Pro Cloud - Rede: ${p.networkName || '-'} / ${p.unitName || '-'}</div>
                  </div>`;

            document.getElementById('print-area').innerHTML = html;
            document.getElementById('print-area').classList.add('print-active');
            window.print();
            setTimeout(() => document.getElementById('print-area').classList.remove('print-active'), 500);
        };
    </script>
</body>
</html>
