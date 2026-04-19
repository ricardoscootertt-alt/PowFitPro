<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro - Plataforma Profissional de Treinos</title>
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

        .btn-primary { background-color: var(--primary); color: white; transition: background-color 0.2s; }
        .btn-primary:hover { background-color: var(--primary-hover); }

        /* Oculta área de impressão na tela normal */
        #print-area { display: none; }

        /* ESTILOS DE IMPRESSÃO (Estilo Planilha) */
        @media print {
            body { background: white !important; color: black !important; margin: 0; padding: 0; }
            #app-container, #login-screen, #history-modal, #exercise-modal, #toast-container, #confirm-modal, .no-print { display: none !important; }
            #print-area {
                display: block !important;
                padding: 10mm;
                font-family: 'Arial', sans-serif;
                font-size: 10px;
            }
            @page { size: A4; margin: 0; }
            
            .print-header { border-bottom: 2px solid #000; padding-bottom: 8px; margin-bottom: 12px; }
            .print-title { font-size: 18px; font-weight: bold; margin: 0; text-transform: uppercase; text-align: center;}
            .prof-info { font-size: 11px; margin-top: 5px; text-align: center; font-weight: bold;}
            
            .print-grid {
                display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 4px;
                margin-bottom: 15px; border: 1px solid #000; padding: 8px; background: #fff;
            }
            .print-grid div { font-size: 11px; }
            
            .print-guidelines {
                border: 1px solid #000; padding: 8px; margin-bottom: 15px; font-size: 9.5px;
            }
            .print-guidelines h4 { margin: 0 0 4px 0; font-size: 11px; text-transform: uppercase; background: #eee; padding: 3px; border: 1px solid #ccc;}
            .print-guidelines ul { margin: 0; padding-left: 15px; }
            .print-guidelines li { margin-bottom: 3px; }

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
            .legal-text { font-size: 8.5px; color: #111; text-align: justify; margin-top: 8px; font-style: italic; line-height: 1.3;}
        }

        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
    </style>
</head>
<body data-theme="Masculino" class="min-h-screen">

    <!-- TELA DE LOGIN -->
    <div id="login-screen" class="fixed inset-0 bg-black bg-opacity-90 flex items-center justify-center z-[100]">
        <div class="card p-8 rounded-2xl max-w-md w-full text-center shadow-2xl border border-gray-700">
            <i class="fas fa-dumbbell text-5xl text-primary mb-4"></i>
            <h1 class="text-3xl font-bold mb-2">PowFit Pro</h1>
            <p class="text-sm opacity-70 mb-8">Plataforma Profissional na Nuvem</p>
            
            <button onclick="loginWithGoogle()" class="w-full bg-white text-gray-900 hover:bg-gray-100 font-bold py-3 px-4 rounded-xl shadow transition flex items-center justify-center gap-3">
                <img src="https://www.gstatic.com/firebasejs/ui/2.0.0/images/auth/google.svg" class="w-6 h-6" alt="Google">
                Entrar com Google
            </button>
            <p class="text-xs opacity-50 mt-6">Seus treinos salvos com segurança em sua conta.</p>
        </div>
    </div>

    <!-- MENSAGENS DE SISTEMA (TOAST) -->
    <div id="toast-container" class="fixed top-5 left-1/2 transform -translate-x-1/2 z-[200] flex flex-col gap-2 pointer-events-none"></div>

    <!-- MODAL DE CONFIRMAÇÃO CUSTOMIZADO -->
    <div id="confirm-modal" class="hidden fixed inset-0 bg-black bg-opacity-70 z-[150] flex items-center justify-center p-4">
        <div class="card p-6 rounded-xl max-w-sm w-full text-center shadow-2xl border border-gray-700">
            <i class="fas fa-exclamation-triangle text-3xl text-yellow-500 mb-3"></i>
            <p id="confirm-msg" class="mb-6 text-sm font-medium"></p>
            <div class="flex gap-3 justify-center">
                <button onclick="closeConfirm(false)" class="flex-1 px-4 py-2 bg-gray-600 hover:bg-gray-500 rounded-lg text-white font-medium transition">Cancelar</button>
                <button onclick="closeConfirm(true)" class="flex-1 px-4 py-2 bg-red-600 hover:bg-red-500 rounded-lg text-white font-medium transition">Confirmar</button>
            </div>
        </div>
    </div>

    <!-- ÁREA PRINCIPAL DO SISTEMA -->
    <div id="app-container" class="max-w-7xl mx-auto p-4 sm:p-6 lg:p-8 hidden">
        
        <!-- Header -->
        <div class="flex flex-col md:flex-row justify-between items-center mb-8 gap-4 bg-black bg-opacity-20 p-4 rounded-xl border border-white border-opacity-10 shadow-sm">
            <div class="flex items-center gap-3">
                <i class="fas fa-dumbbell text-3xl text-primary"></i>
                <div>
                    <h1 class="text-2xl font-bold tracking-tight">PowFit Pro</h1>
                    <div class="flex items-center gap-2 text-xs var(--text-muted)">
                        <img id="user-avatar" src="" class="w-4 h-4 rounded-full hidden">
                        <span id="user-email">Carregando...</span>
                    </div>
                </div>
            </div>
            <div class="flex flex-wrap justify-center gap-2">
                <button onclick="openHistory()" class="bg-gray-700 hover:bg-gray-600 text-white px-4 py-2.5 rounded-lg text-sm font-medium shadow flex items-center gap-2 transition">
                    <i class="fas fa-cloud"></i> Histórico / Nuvem
                </button>
                <button onclick="generatePrint()" class="btn-primary px-5 py-2.5 rounded-lg text-sm font-medium shadow-lg flex items-center gap-2">
                    <i class="fas fa-print"></i> Salvar & Imprimir
                </button>
                <button onclick="logout()" class="bg-red-600 hover:bg-red-700 text-white px-4 py-2.5 rounded-lg text-sm font-medium shadow transition">
                    <i class="fas fa-sign-out-alt"></i> Sair
                </button>
            </div>
        </div>

        <div class="grid grid-cols-1 xl:grid-cols-12 gap-6 pb-10">
            
            <!-- COLUNA ESQUERDA: Configurações -->
            <div class="xl:col-span-4 space-y-6">
                
                <!-- Card: Profissional -->
                <div class="card rounded-xl p-5 shadow-sm relative overflow-hidden">
                    <div class="absolute top-0 right-0 w-16 h-16 bg-primary opacity-10 rounded-bl-full"></div>
                    <h2 class="text-lg font-semibold mb-4 flex items-center gap-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <i class="fas fa-id-badge text-primary"></i> Profissional
                    </h2>
                    <div class="space-y-4">
                        <div>
                            <label class="block text-xs font-medium mb-1 opacity-70">Tipo de Atuação</label>
                            <select id="prof-type" onchange="toggleProfType()" class="input-field w-full rounded-lg px-3 py-2 text-sm font-medium shadow-sm">
                                <option value="PEF">Profissional de Educação Física</option>
                                <option value="TE">Treinador Esportivo</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-xs font-medium mb-1 opacity-70">Nome na Assinatura</label>
                            <input type="text" id="prof-name" class="input-field w-full rounded-lg px-3 py-2 text-sm shadow-sm" placeholder="Seu nome profissional">
                        </div>
                        <div class="grid grid-cols-2 gap-3" id="cref-container">
                            <div>
                                <label class="block text-xs font-medium mb-1 opacity-70">CREF</label>
                                <input type="text" id="prof-cref" class="input-field w-full rounded-lg px-3 py-2 text-sm shadow-sm" placeholder="Ex: 000000">
                            </div>
                            <div>
                                <label class="block text-xs font-medium mb-1 opacity-70">Estado</label>
                                <input type="text" id="prof-state" class="input-field w-full rounded-lg px-3 py-2 text-sm shadow-sm" placeholder="Ex: SP, RJ">
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Card: Dados do Aluno -->
                <div class="card rounded-xl p-5 shadow-sm">
                    <div class="flex justify-between items-center mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <h2 class="text-lg font-semibold flex items-center gap-2">
                            <i class="fas fa-user text-primary"></i> Aluno
                        </h2>
                        <div id="imc-display"></div>
                    </div>
                    
                    <div class="space-y-4">
                        <div>
                            <label class="block text-xs font-medium mb-1 opacity-70">Nome Completo</label>
                            <input type="text" id="stu-name" class="input-field w-full rounded-lg px-3 py-2 text-sm shadow-sm" placeholder="Nome do aluno">
                        </div>
                        <div class="grid grid-cols-3 gap-3">
                            <div>
                                <label class="block text-xs font-medium mb-1 opacity-70">Idade</label>
                                <input type="number" id="stu-age" class="input-field w-full rounded-lg px-2 py-2 text-sm text-center shadow-sm" placeholder="Anos">
                            </div>
                            <div>
                                <label class="block text-xs font-medium mb-1 opacity-70">Peso (kg)</label>
                                <input type="number" id="stu-weight" oninput="calculateIMC()" class="input-field w-full rounded-lg px-2 py-2 text-sm text-center shadow-sm" placeholder="kg">
                            </div>
                            <div>
                                <label class="block text-xs font-medium mb-1 opacity-70">Altura (m)</label>
                                <input type="number" id="stu-height" step="0.01" oninput="calculateIMC()" class="input-field w-full rounded-lg px-2 py-2 text-sm text-center shadow-sm" placeholder="m">
                            </div>
                        </div>
                        <div class="grid grid-cols-2 gap-3">
                            <div>
                                <label class="block text-xs font-medium mb-1 opacity-70">Gênero (Tema)</label>
                                <select id="stu-gender" onchange="changeTheme()" class="input-field w-full rounded-lg px-3 py-2 text-sm shadow-sm">
                                    <option value="Masculino">Masculino</option>
                                    <option value="Feminino">Feminino</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs font-medium mb-1 opacity-70">Nível</label>
                                <select id="stu-level" class="input-field w-full rounded-lg px-3 py-2 text-sm shadow-sm">
                                    <option>Iniciante</option>
                                    <option>Intermediário</option>
                                    <option>Avançado</option>
                                </select>
                            </div>
                        </div>
                        <div>
                            <label class="block text-xs font-medium mb-1 opacity-70">Objetivo Principal</label>
                            <select id="stu-objective" class="input-field w-full rounded-lg px-3 py-2 text-sm shadow-sm">
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
                            <label class="block text-xs font-medium mb-1 opacity-70">Frequência</label>
                            <select id="stu-freq" class="input-field w-full rounded-lg px-3 py-2 text-sm shadow-sm">
                                <option>3 dias</option>
                                <option>5 dias</option>
                                <option>6 dias</option>
                                <option>Personalizado</option>
                            </select>
                        </div>
                    </div>
                </div>

                <!-- Card: Estado de Saúde -->
                <div class="card rounded-xl p-5 shadow-sm">
                    <h2 class="text-lg font-semibold mb-2 flex items-center gap-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <i class="fas fa-heartbeat text-primary"></i> Estado de Saúde
                    </h2>
                    <p class="text-[11px] opacity-70 mb-3 leading-tight">Gera diretrizes legais automáticas com base na sua categoria.</p>
                    <div class="grid grid-cols-2 gap-2 text-xs" id="health-container">
                        <!-- Gerado via JS -->
                    </div>
                </div>

            </div>

            <!-- COLUNA DIREITA: Montagem dos Treinos -->
            <div class="xl:col-span-8 space-y-6">
                
                <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center bg-black bg-opacity-20 p-5 rounded-xl border border-white border-opacity-10 gap-4 shadow-sm">
                    <div>
                        <h2 class="text-xl font-bold flex items-center gap-2">
                            <i class="fas fa-clipboard-list text-primary"></i> Ficha de Treino
                        </h2>
                        <div class="flex gap-2 mt-2">
                            <select id="stu-validity" class="input-field rounded-lg px-2 py-1.5 text-xs shadow-sm">
                                <option>Validade: 15 dias</option>
                                <option selected>Validade: 30 dias</option>
                                <option>Validade: 60 dias</option>
                                <option>Validade: 90 dias</option>
                            </select>
                        </div>
                    </div>
                    <button onclick="addWorkout()" class="btn-primary px-5 py-2.5 rounded-lg text-sm font-medium flex items-center justify-center gap-2 w-full sm:w-auto shadow-md">
                        <i class="fas fa-calendar-plus"></i> Adicionar Dia de Treino
                    </button>
                </div>

                <!-- Container de Treinos -->
                <div id="workouts-container" class="space-y-6">
                    <!-- Treinos JS -->
                </div>

                <!-- Recomendações Livres -->
                <div class="card rounded-xl p-5 shadow-sm">
                    <h2 class="text-md font-semibold mb-3 flex items-center gap-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <i class="fas fa-pen text-primary"></i> Recomendações Específicas
                    </h2>
                    <textarea id="stu-recs" rows="3" class="input-field w-full rounded-lg px-3 py-2 text-sm shadow-sm" placeholder="Escreva sobre hidratação, descanso, cuidados para este aluno..."></textarea>
                </div>

            </div>
        </div>
    </div>

    <!-- MODAL DE EXERCÍCIOS -->
    <div id="exercise-modal" class="hidden fixed inset-0 bg-black bg-opacity-80 backdrop-blur-sm z-[60] flex items-center justify-center p-2 sm:p-4">
        <div class="card w-full max-w-5xl rounded-2xl shadow-2xl flex flex-col h-[90vh] border border-gray-600">
            <div class="p-4 border-b flex justify-between items-center bg-black bg-opacity-20 rounded-t-2xl" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold"><i class="fas fa-search text-primary mr-2"></i> Adicionar Exercício</h3>
                <button onclick="closeModal()" class="text-gray-400 hover:text-red-500 transition-colors text-2xl leading-none">&times;</button>
            </div>
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <div class="w-full md:w-1/4 border-r overflow-y-auto p-3 space-y-1 bg-black bg-opacity-10" style="border-color: var(--border-color)" id="modal-categories"></div>
                <div class="w-full md:w-3/4 overflow-y-auto p-4" id="modal-exercises"></div>
            </div>
        </div>
    </div>

    <!-- MODAL DE HISTÓRICO (MEMÓRIA NUVEM) -->
    <div id="history-modal" class="hidden fixed inset-0 bg-black bg-opacity-80 backdrop-blur-sm z-[70] flex items-center justify-center p-2 sm:p-4">
        <div class="card w-full max-w-4xl rounded-2xl shadow-2xl flex flex-col max-h-[90vh] border border-gray-600">
            <div class="p-5 border-b flex justify-between items-center bg-black bg-opacity-20 rounded-t-2xl" style="border-color: var(--border-color)">
                <div>
                    <h3 class="text-lg font-bold"><i class="fas fa-cloud text-primary mr-2"></i> Nuvem PowFit Pro</h3>
                    <p class="text-xs opacity-70 mt-1">Suas fichas salvas sincronizadas no Firebase</p>
                </div>
                <button onclick="closeHistory()" class="text-gray-400 hover:text-red-500 transition-colors text-2xl leading-none">&times;</button>
            </div>
            <div class="p-4 overflow-y-auto flex-1 space-y-3 bg-black bg-opacity-5" id="history-list">
                <div class="text-center opacity-50 py-10"><i class="fas fa-spinner fa-spin text-2xl mb-2"></i><br>Carregando fichas...</div>
            </div>
            <div class="p-4 border-t text-right" style="border-color: var(--border-color)">
                <button onclick="newFicha()" class="btn-primary px-4 py-2 rounded-lg text-sm font-medium shadow transition">
                    <i class="fas fa-plus"></i> Iniciar Nova Ficha em Branco
                </button>
            </div>
        </div>
    </div>

    <!-- ÁREA DE IMPRESSÃO (Oculta na tela) -->
    <div id="print-area"></div>

    <!-- FIREBASE & LÓGICA DO SISTEMA -->
    <script type="module">
        // Importações do Firebase SDK v9 (compatível)
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, GoogleAuthProvider, signInWithPopup, signOut, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, addDoc, getDocs, deleteDoc, doc, setDoc, query, orderBy } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Configuração do Firebase fornecida
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
        const provider = new GoogleAuthProvider();

        let currentUser = null;

        // --- SISTEMA DE TOAST E CONFIRMAÇÃO ---
        window.showToast = (msg, type='info') => {
            const container = document.getElementById('toast-container');
            const toast = document.createElement('div');
            const colors = type === 'error' ? 'bg-red-600' : (type === 'success' ? 'bg-green-600' : 'bg-gray-800');
            toast.className = `${colors} text-white px-6 py-3 rounded-lg shadow-xl text-sm font-medium transform transition-all duration-300 opacity-0 translate-y-[-20px]`;
            toast.innerText = msg;
            container.appendChild(toast);
            
            setTimeout(() => { toast.classList.remove('opacity-0', 'translate-y-[-20px]'); }, 10);
            setTimeout(() => {
                toast.classList.add('opacity-0', 'translate-y-[-20px]');
                setTimeout(() => toast.remove(), 300);
            }, 3000);
        }

        let confirmCallback = null;
        window.showConfirm = (msg, cb) => {
            document.getElementById('confirm-msg').innerText = msg;
            document.getElementById('confirm-modal').classList.remove('hidden');
            confirmCallback = cb;
        }
        window.closeConfirm = (res) => {
            document.getElementById('confirm-modal').classList.add('hidden');
            if(res && confirmCallback) confirmCallback();
        }

        // --- AUTENTICAÇÃO ---
        window.loginWithGoogle = () => {
            signInWithPopup(auth, provider).catch(err => {
                console.error(err);
                showToast("Erro ao fazer login. Tente novamente.", "error");
            });
        };

        window.logout = () => {
            signOut(auth).then(() => {
                state.currentId = null;
                showToast("Sessão encerrada com sucesso.", "success");
            });
        };

        onAuthStateChanged(auth, (user) => {
            if (user) {
                currentUser = user;
                document.getElementById('login-screen').classList.add('hidden');
                document.getElementById('app-container').classList.remove('hidden');
                document.getElementById('user-email').innerText = user.email;
                if(user.photoURL) {
                    const avatar = document.getElementById('user-avatar');
                    avatar.src = user.photoURL;
                    avatar.classList.remove('hidden');
                }
                initApp(); // Inicia app após login
            } else {
                currentUser = null;
                document.getElementById('login-screen').classList.remove('hidden');
                document.getElementById('app-container').classList.add('hidden');
            }
        });

        // --- DADOS DO SISTEMA ---
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
            "🟢 Saudável": "Manter treinos regulares de 3–5x por semana, combinando musculação e atividades cardiovasculares. A constância e boa alimentação contribuem para resultados.",
            "⚪ Sedentário": "Início gradual, com treinos leves. Prioridade é desenvolver constância, aprender a execução e evitar excesso de carga.",
            "🟡 Sobrepeso": "Musculação associada ao cardio contribui para condicionamento e composição corporal. Progressão respeitando a adaptação.",
            "🔴 Obesidade": "Iniciar com exercícios de menor impacto e progressão gradual. Segurança, mobilidade e constância são prioridades.",
            "⚖️ Baixo peso": "Musculação auxilia no ganho de massa quando associada a alimentação e descanso. Priorizar evolução progressiva.",
            "🍬 Diabetes": "Manter acompanhamento médico. Musculação auxilia na rotina se liberada. Em caso de tontura ou mal-estar, interromper o treino.",
            "❤️ Hipertensão": "Acompanhamento médico regular. Evitar prender a respiração (Valsalva) e controlar a intensidade dos exercícios.",
            "🔵 Hipotensão": "Evitar mudanças bruscas de posição. Manter boa hidratação e respeitar a intensidade adequada para evitar mal-estar.",
            "💔 Problemas cardíacos": "Prática apenas com liberação médica. Respeitar limites individuais e controle estrito de intensidade.",
            "🦴 Problemas articulares": "Exercícios com menor impacto e maior controle. Acompanhamento profissional ajuda na segurança.",
            "🫁 Problemas respiratórios": "Progressão gradual respeitando a capacidade respiratória. Interromper em caso de falta de ar.",
            "⚠️ Lesões": "Adaptação deve respeitar a limitação e evitar dor. Seguir orientação profissional antes da prática.",
            "🤰 Gestante": "Liberação médica necessária. Foco na segurança, mobilidade e bem-estar, evitando impacto e risco.",
            "🤱 Lactante": "Atividade mantida normalmente, respeitando recuperação, hidratação e alimentação adequada.",
            "👴 Idoso": "Musculação para força, equilíbrio e mobilidade. Respeitar limitações com intensidade moderada e segurança."
        };

        const objectiveData = {
            "Emagrecimento": "Déficit calórico com treinos mistos de força (manter massa magra) e aeróbicos (maior gasto).",
            "Hipertrofia": "Progressão de carga e volume adequado. Superávit calórico e descanso são fundamentais.",
            "Definição": "Manutenção de massa muscular reduzindo percentual de gordura. Atenção estrita à dieta.",
            "Condicionamento": "Treinos com menor intervalo, circuitos e integração cardiopulmonar.",
            "Resistência": "Séries mais longas, cadência controlada e aprimoramento da capacidade muscular.",
            "Força": "Cargas altas, baixas repetições e intervalos de descanso maiores.",
            "Reabilitação": "Fortalecimento específico, mobilidade e controle motor. Respeitar limites de dor.",
            "Saúde geral": "Equilíbrio entre força, cardio e flexibilidade. Foco na constância e bem-estar."
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
                "Remada Curvada", "Remada Curvada Supinada", "Remada Cavalinho (T-Bar)", "Remada Unilateral",
                "Remada no Cross", "Serrote", "Facepull (puxada de cima para baixo)", "Encolhimento (Trapézio)"
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
                "Esteira 10 Minutos", "Esteira 15 Minutos", "Esteira 20 Minutos", "Pular Corda."
            ]
        };

        const dbTechniques = [
            "Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", 
            "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", 
            "Negativa", "Isometria", "Parciais", "Pirâmide"
        ];

        const daysOfWeek = ["SEGUNDA-FEIRA", "TERÇA-FEIRA", "QUARTA-FEIRA", "QUINTA-FEIRA", "SEXTA-FEIRA", "SÁBADO", "DOMINGO"];

        // --- ESTADO DO APP ---
        let state = {
            currentId: null,
            workouts: [],
            activeModalWorkoutId: null,
            activeCategory: "🔥 PEITO"
        };

        function generateId() { return Math.random().toString(36).substr(2, 9); }

        function initApp() {
            window.toggleProfType();
            
            // Tenta carregar dados do prof no local storage pra agilizar
            const savedType = localStorage.getItem('powfit_ptype');
            if(savedType) { document.getElementById('prof-type').value = savedType; window.toggleProfType(); }
            document.getElementById('prof-name').value = localStorage.getItem('powfit_pname') || '';
            document.getElementById('prof-cref').value = localStorage.getItem('powfit_pcref') || '';
            document.getElementById('prof-state').value = localStorage.getItem('powfit_pstate') || '';

            if(state.workouts.length === 0) window.addWorkout(); // Adiciona o 1º dia padrão se vazio
        }

        // --- EXPORTAÇÕES PARA O HTML ---
        window.toggleProfType = () => {
            const type = document.getElementById('prof-type').value;
            const crefContainer = document.getElementById('cref-container');
            
            if(type === 'TE') {
                crefContainer.classList.add('hidden');
                renderHealthOptions(healthDataTE);
            } else {
                crefContainer.classList.remove('hidden');
                renderHealthOptions(healthDataPEF);
            }
        };

        window.changeTheme = () => {
            document.body.setAttribute('data-theme', document.getElementById('stu-gender').value);
        };

        window.calculateIMC = () => {
            const w = parseFloat(document.getElementById('stu-weight').value);
            const h = parseFloat(document.getElementById('stu-height').value);
            const display = document.getElementById('imc-display');
            if (w > 0 && h > 0) {
                const imc = (w / (h * h)).toFixed(1);
                let classif = imc < 18.5 ? "Abaixo" : (imc < 24.9 ? "Normal" : (imc < 29.9 ? "Sobrepeso" : "Obesidade"));
                display.innerHTML = `<span class="bg-primary bg-opacity-20 text-primary px-3 py-1 rounded-lg text-xs font-bold border border-primary border-opacity-30">IMC: ${imc} (${classif})</span>`;
            } else {
                display.innerHTML = '';
            }
        };

        function renderHealthOptions(dataObj) {
            const container = document.getElementById('health-container');
            const selected = Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value);
            container.innerHTML = Object.keys(dataObj).map(opt => {
                const isChecked = selected.includes(opt) ? 'checked' : '';
                return `<label class="flex items-start space-x-2 cursor-pointer p-1.5 rounded hover:bg-black hover:bg-opacity-10 transition border border-transparent hover:border-gray-500 hover:border-opacity-30">
                    <input type="checkbox" value="${opt}" ${isChecked} class="health-cb mt-0.5 rounded border-gray-400 text-primary focus:ring-primary w-4 h-4 shadow-sm">
                    <span class="font-medium">${opt}</span>
                </label>`
            }).join('');
        }

        // --- TREINOS ---
        window.addWorkout = () => {
            const count = state.workouts.length;
            const title = count < 7 ? `TREINO ${daysOfWeek[count]}` : `NOVO TREINO ${count + 1}`;
            state.workouts.push({ id: generateId(), title, exercises: [] });
            renderWorkouts();
        };

        window.duplicateWorkout = (id) => {
            const workout = state.workouts.find(w => w.id === id);
            if(workout) {
                const newW = JSON.parse(JSON.stringify(workout));
                newW.id = generateId();
                newW.title += " (Cópia)";
                state.workouts.push(newW);
                renderWorkouts();
            }
        };

        window.removeWorkout = (id) => {
            showConfirm("Deseja realmente excluir este dia de treino da ficha?", () => {
                state.workouts = state.workouts.filter(w => w.id !== id);
                renderWorkouts();
            });
        };

        window.updateWorkoutTitle = (id, val) => {
            const w = state.workouts.find(w => w.id === id);
            if(w) w.title = val;
        };

        window.moveExercise = (wId, idx, dir) => {
            const w = state.workouts.find(w => w.id === wId);
            if(!w) return;
            if (dir === 'up' && idx > 0) {
                [w.exercises[idx], w.exercises[idx-1]] = [w.exercises[idx-1], w.exercises[idx]];
            } else if (dir === 'down' && idx < w.exercises.length - 1) {
                [w.exercises[idx], w.exercises[idx+1]] = [w.exercises[idx+1], w.exercises[idx]];
            }
            renderWorkouts();
        };

        window.removeExercise = (wId, idx) => {
            const w = state.workouts.find(w => w.id === wId);
            if(w) { w.exercises.splice(idx, 1); renderWorkouts(); }
        };

        window.updateExercise = (wId, idx, field, val) => {
            const w = state.workouts.find(w => w.id === wId);
            if(w && w.exercises[idx]) w.exercises[idx][field] = val;
        };

        function renderWorkouts() {
            const container = document.getElementById('workouts-container');
            container.innerHTML = '';

            state.workouts.forEach(workout => {
                let exercisesHtml = workout.exercises.length === 0 
                    ? `<div class="text-center p-4 text-xs opacity-50 italic">Nenhum exercício neste dia.</div>`
                    : workout.exercises.map((ex, idx) => `
                        <div class="flex flex-col sm:flex-row gap-2 p-2 items-start sm:items-center border-b last:border-0 border-opacity-10 hover:bg-black hover:bg-opacity-5 transition" style="border-color: var(--border-color)">
                            <div class="flex gap-1 hidden sm:flex">
                                <button onclick="moveExercise('${workout.id}', ${idx}, 'up')" class="text-[10px] p-1.5 rounded bg-black bg-opacity-10 hover:bg-primary hover:text-white transition"><i class="fas fa-chevron-up"></i></button>
                                <button onclick="moveExercise('${workout.id}', ${idx}, 'down')" class="text-[10px] p-1.5 rounded bg-black bg-opacity-10 hover:bg-primary hover:text-white transition"><i class="fas fa-chevron-down"></i></button>
                            </div>
                            <div class="flex-1 font-medium w-full sm:w-auto">
                                <div class="text-[9px] opacity-60 uppercase tracking-wider font-bold">${ex.category}</div>
                                <div class="text-xs">${ex.name}</div>
                            </div>
                            <div class="flex flex-wrap gap-1.5 w-full sm:w-auto">
                                <input type="text" value="${ex.sets}" onchange="updateExercise('${workout.id}', ${idx}, 'sets', this.value)" class="input-field w-12 rounded px-1 py-1 text-xs text-center border-gray-500 border-opacity-30" placeholder="Séries">
                                <span class="self-center opacity-50 text-[10px]">x</span>
                                <input type="text" value="${ex.reps}" onchange="updateExercise('${workout.id}', ${idx}, 'reps', this.value)" class="input-field w-16 rounded px-1 py-1 text-xs text-center border-gray-500 border-opacity-30" placeholder="Reps">
                                <select onchange="updateExercise('${workout.id}', ${idx}, 'technique', this.value)" class="input-field w-24 rounded px-1 py-1 text-[10px] border-gray-500 border-opacity-30">
                                    ${dbTechniques.map(t => `<option value="${t}" ${ex.technique === t ? 'selected' : ''}>${t}</option>`).join('')}
                                </select>
                                <input type="text" value="${ex.obs}" onchange="updateExercise('${workout.id}', ${idx}, 'obs', this.value)" class="input-field flex-1 sm:w-36 rounded px-2 py-1 text-xs border-gray-500 border-opacity-30" placeholder="Obs...">
                            </div>
                            <div class="flex items-center gap-1 w-full sm:w-auto justify-end mt-1 sm:mt-0">
                                <button onclick="removeExercise('${workout.id}', ${idx})" class="text-red-400 hover:text-red-600 bg-red-500 bg-opacity-10 hover:bg-opacity-20 p-1.5 rounded transition text-sm">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                    `).join('');

                const workoutCard = `
                    <div class="card rounded-2xl overflow-hidden shadow-md border border-gray-600 border-opacity-40">
                        <div class="p-3 border-b flex justify-between items-center bg-black bg-opacity-10" style="border-color: var(--border-color);">
                            <div class="flex items-center gap-2 w-1/2">
                                <i class="fas fa-calendar-day text-primary opacity-80"></i>
                                <input type="text" value="${workout.title}" onchange="updateWorkoutTitle('${workout.id}', this.value)" class="input-field bg-transparent font-bold text-sm w-full px-2 py-1 rounded uppercase focus:bg-black focus:bg-opacity-10">
                            </div>
                            <div class="flex gap-1.5">
                                <button onclick="duplicateWorkout('${workout.id}')" class="text-xs p-2 rounded bg-black bg-opacity-10 hover:bg-primary hover:text-white transition" title="Duplicar Treino">
                                    <i class="fas fa-copy"></i>
                                </button>
                                <button onclick="removeWorkout('${workout.id}')" class="text-xs p-2 text-red-500 bg-red-500 bg-opacity-10 rounded hover:bg-red-500 hover:text-white transition" title="Excluir Treino">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                        <div class="p-0">${exercisesHtml}</div>
                        <div class="p-2 border-t bg-black bg-opacity-5" style="border-color: var(--border-color)">
                            <button onclick="openModal('${workout.id}')" class="w-full text-primary py-2 rounded-lg text-xs font-bold hover:bg-primary hover:text-white border border-primary border-opacity-30 transition flex justify-center items-center gap-2">
                                <i class="fas fa-plus"></i> ADICIONAR EXERCÍCIO
                            </button>
                        </div>
                    </div>
                `;
                container.insertAdjacentHTML('beforeend', workoutCard);
            });
        }

        // --- MODAIS ---
        window.openModal = (wId) => {
            state.activeModalWorkoutId = wId;
            document.getElementById('exercise-modal').classList.remove('hidden');
            renderModalCategories();
            renderModalExercises();
        };
        window.closeModal = () => document.getElementById('exercise-modal').classList.add('hidden');
        window.setModalCategory = (cat) => { state.activeCategory = cat; renderModalCategories(); renderModalExercises(); };

        function renderModalCategories() {
            const container = document.getElementById('modal-categories');
            container.innerHTML = Object.keys(dbCategories).map(cat => `
                <button onclick="setModalCategory('${cat}')" class="w-full text-left px-3 py-2.5 rounded-lg text-xs font-bold transition ${state.activeCategory === cat ? 'bg-primary text-white shadow' : 'hover:bg-black hover:bg-opacity-20'}">
                    ${cat}
                </button>
            `).join('');
        }

        function renderModalExercises() {
            const container = document.getElementById('modal-exercises');
            container.innerHTML = `<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-2">${
                dbCategories[state.activeCategory].map(ex => {
                    const isCardio = state.activeCategory === "🫀 CARDIO";
                    return `<button onclick="addExerciseToWorkout('${ex}', ${isCardio})" class="card p-3 rounded-xl text-left text-xs font-medium border border-gray-600 border-opacity-30 hover:border-primary hover:shadow-lg transition flex justify-between items-center group bg-black bg-opacity-20 hover:bg-opacity-40">
                        <span>${ex}</span>
                        <i class="fas fa-plus opacity-0 group-hover:opacity-100 transition text-primary text-lg"></i>
                    </button>`
                }).join('')
            }</div>`;
        }

        window.addExerciseToWorkout = (exName, isCardio) => {
            const w = state.workouts.find(w => w.id === state.activeModalWorkoutId);
            if(w) {
                w.exercises.push({ category: state.activeCategory, name: exName, sets: isCardio ? '1' : '3', reps: isCardio ? '-' : '10 a 12', technique: 'Nenhuma', obs: '' });
                renderWorkouts();
                const container = document.getElementById('modal-exercises');
                container.style.opacity = '0.5';
                setTimeout(() => container.style.opacity = '1', 100);
            }
        };

        // --- NUVEM / FIREBASE (HISTÓRICO) ---
        function getUIData() {
            return {
                timestamp: Date.now(),
                dateStr: new Date().toLocaleDateString('pt-BR') + ' ' + new Date().toLocaleTimeString('pt-BR', {hour: '2-digit', minute:'2-digit'}),
                studentName: document.getElementById('stu-name').value || 'Sem Nome',
                profType: document.getElementById('prof-type').value,
                profName: document.getElementById('prof-name').value,
                profCref: document.getElementById('prof-cref').value,
                profState: document.getElementById('prof-state').value,
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
                workouts: state.workouts
            };
        }

        async function saveToCloud() {
            if (!currentUser) { showToast("Usuário não autenticado.", "error"); return false; }
            
            const data = getUIData();
            data.userId = currentUser.uid; // Segurança

            // Salva prefs do prof no dispositivo
            localStorage.setItem('powfit_ptype', data.profType);
            localStorage.setItem('powfit_pname', data.profName);
            localStorage.setItem('powfit_pcref', data.profCref);
            localStorage.setItem('powfit_pstate', data.profState);

            try {
                // Exibição de UI loading (opcional)
                const btn = document.querySelector('button[onclick="generatePrint()"]');
                const oldContent = btn.innerHTML;
                btn.innerHTML = `<i class="fas fa-spinner fa-spin"></i> Salvando...`;
                
                if (state.currentId) {
                    await setDoc(doc(db, 'powfit_fichas', state.currentId), data);
                } else {
                    const docRef = await addDoc(collection(db, 'powfit_fichas'), data);
                    state.currentId = docRef.id;
                }
                
                btn.innerHTML = oldContent;
                showToast("Ficha salva na nuvem!", "success");
                return true;
            } catch (e) {
                console.error(e);
                // Caso o usuário não tenha permissões na base de dados powfitpro-582df
                showToast("Ficha gerada para impressão (Aviso: O Firestore bloqueou a gravação por falta de permissões).", "info");
                return true; // Continua a impressão mesmo se falhar o firebase
            }
        }

        window.openHistory = async () => {
            if(!currentUser) return showToast("Faça login para ver a nuvem", "error");
            document.getElementById('history-modal').classList.remove('hidden');
            const list = document.getElementById('history-list');
            list.innerHTML = `<div class="text-center opacity-50 py-10"><i class="fas fa-spinner fa-spin text-2xl mb-2"></i><br>Buscando na nuvem...</div>`;
            
            try {
                const q = query(collection(db, 'powfit_fichas'), orderBy("timestamp", "desc"));
                const querySnapshot = await getDocs(q);
                
                let html = "";
                querySnapshot.forEach((doc) => {
                    const d = doc.data();
                    if(d.userId !== currentUser.uid) return; // Filtro de segurança extra
                    
                    html += `
                    <div class="card p-4 rounded-xl flex justify-between items-center border border-gray-600 border-opacity-30 bg-black bg-opacity-10 hover:bg-opacity-30 transition">
                        <div>
                            <div class="font-bold text-sm">${d.studentName}</div>
                            <div class="text-[10px] opacity-70 mt-1"><i class="far fa-clock"></i> ${d.dateStr} | ${d.stuObjective}</div>
                        </div>
                        <div class="flex gap-2">
                            <button onclick="loadHistoryItem('${doc.id}')" class="btn-primary px-3 py-1.5 rounded-lg text-xs font-bold shadow"><i class="fas fa-download"></i> ABRIR</button>
                            <button onclick="deleteHistoryItem('${doc.id}')" class="bg-red-600 hover:bg-red-700 text-white px-3 py-1.5 rounded-lg text-xs font-bold shadow"><i class="fas fa-trash"></i></button>
                        </div>
                    </div>`;
                });

                if(html === "") list.innerHTML = `<div class="text-center opacity-50 py-10">Nenhuma ficha na sua nuvem.</div>`;
                else list.innerHTML = html;

            } catch (e) {
                console.error(e);
                list.innerHTML = `<div class="text-center text-red-500 py-10">Erro de Permissão no Banco de Dados. Verifique as regras do Firebase.</div>`;
            }
        };

        window.closeHistory = () => document.getElementById('history-modal').classList.add('hidden');
        
        window.newFicha = () => {
            showConfirm("Limpar tudo e iniciar nova ficha?", () => {
                state.currentId = null;
                document.getElementById('stu-name').value = '';
                document.getElementById('stu-age').value = '';
                document.getElementById('stu-weight').value = '';
                document.getElementById('stu-height').value = '';
                document.getElementById('stu-recs').value = '';
                document.getElementById('imc-display').innerHTML = '';
                document.querySelectorAll('.health-cb').forEach(cb => cb.checked = false);
                state.workouts = [];
                window.addWorkout();
                window.closeHistory();
                showToast("Nova ficha iniciada.", "success");
            });
        };

        window.loadHistoryItem = async (docId) => {
            const list = document.getElementById('history-list');
            try {
                const q = query(collection(db, 'powfit_fichas'));
                const querySnapshot = await getDocs(q);
                let item = null;
                querySnapshot.forEach(doc => { if(doc.id === docId) item = doc.data(); });
                
                if(!item) return showToast("Ficha não encontrada.", "error");

                state.currentId = docId;
                
                document.getElementById('prof-type').value = item.profType || 'PEF';
                window.toggleProfType();
                document.getElementById('prof-name').value = item.profName || '';
                document.getElementById('prof-cref').value = item.profCref || '';
                document.getElementById('prof-state').value = item.profState || '';
                
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
                showToast("Ficha carregada com sucesso!", "success");
            } catch(e) {
                console.error(e);
                showToast("Erro ao carregar ficha da nuvem.", "error");
            }
        };

        window.deleteHistoryItem = (docId) => {
            showConfirm("Excluir esta ficha permanentemente da nuvem?", async () => {
                try {
                    await deleteDoc(doc(db, 'powfit_fichas', docId));
                    if(state.currentId === docId) state.currentId = null;
                    showToast("Ficha excluída.", "success");
                    window.openHistory(); // refresh list
                } catch(e) {
                    console.error(e);
                    showToast("Erro ao excluir.", "error");
                }
            });
        };

        // --- IMPRESSÃO ---
        window.generatePrint = async () => {
            const success = await saveToCloud();
            if(!success) return; // Se der crash forte, aborta

            const d = getUIData();
            let imcStr = "-";
            const w = parseFloat(d.stuWeight), h = parseFloat(d.stuHeight);
            if(w>0 && h>0) imcStr = (w / (h * h)).toFixed(1);

            const isPEF = d.profType === 'PEF';
            const healthSourceDict = isPEF ? healthDataPEF : healthDataTE;
            const objRecommendation = objectiveData[d.stuObjective] || "";
            
            const profLabel = isPEF ? 'Profissional de Educação Física' : 'Treinador Esportivo';
            const crefText = isPEF ? `CREF: ${d.profCref} - Estado: ${d.profState}` : '';
            
            let legalTextPEF = `⚠️ OBSERVAÇÃO LEGAL – PROF. DE EDUCAÇÃO FÍSICA: Conforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º estabelece que compete ao profissional coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos especializados nas áreas de atividades físicas e do desporto.`;
            
            let legalTextTE = `⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO: Conforme a Lei nº 14.597/2023 (Lei Geral do Esporte), Art. 75, a profissão de treinador esportivo é reconhecida e regulada no Brasil, com atuação de caráter técnico e esportivo voltada à preparação, supervisão, orientação e acompanhamento de treinos. A atuação possui finalidade orientativa e não substitui avaliação médica, diagnóstico clínico ou acompanhamento de profissionais da saúde. Em casos de doenças, lesões, gestação, limitações ou condição específica, recomenda-se avaliação prévia por profissional habilitado. O treinamento proposto respeita limites, priorizando segurança.`;

            let html = `
                <div class="print-header">
                    <h1 class="print-title">PLANILHA DE TREINAMENTO - ${d.stuObjective}</h1>
                    <div class="prof-info">Prescrição feita por: ${d.profName} (${profLabel})<br>${crefText}</div>
                </div>

                <div class="print-grid">
                    <div><strong>Aluno(a):</strong> ${d.studentName}</div>
                    <div><strong>Idade:</strong> ${d.stuAge || '-'} anos</div>
                    <div><strong>Gênero:</strong> ${d.stuGender}</div>
                    
                    <div><strong>Peso:</strong> ${d.stuWeight || '-'} kg</div>
                    <div><strong>Altura:</strong> ${d.stuHeight || '-'} m</div>
                    <div><strong>IMC:</strong> ${imcStr}</div>
                    
                    <div><strong>Nível:</strong> ${d.stuLevel}</div>
                    <div><strong>Frequência:</strong> ${d.stuFreq}</div>
                    <div><strong>Validade:</strong> ${d.stuValidity.replace('Validade: ', '')}</div>
                </div>
            `;

            if (objRecommendation || d.health.length > 0) {
                html += `<div class="print-guidelines"><h4>Diretrizes Automáticas</h4><ul>`;
                if(objRecommendation) html += `<li><strong>Objetivo (${d.stuObjective}):</strong> ${objRecommendation}</li>`;
                d.health.forEach(k => { if(healthSourceDict[k]) html += `<li><strong>Saúde (${k.replace(/[\u{1F300}-\u{1F9FF}\u{2600}-\u{26FF}\u{2700}-\u{27BF}\u{FE0F}]/gu, '').trim()}):</strong> ${healthSourceDict[k]}</li>`; });
                html += `</ul></div>`;
            }

            d.workouts.forEach(w => {
                html += `<div class="print-workout"><h3>${w.title}</h3><table><thead><tr>
                    <th style="width: 35%">Exercício</th><th style="width: 8%; text-align:center;">Séries</th>
                    <th style="width: 12%; text-align:center;">Reps</th><th style="width: 15%">Técnica</th>
                    <th style="width: 30%">Observações</th></tr></thead><tbody>`;
                
                if (w.exercises.length === 0) html += `<tr><td colspan="5" style="text-align:center; font-style:italic; padding:10px;">Sem exercícios</td></tr>`;
                else {
                    w.exercises.forEach(ex => {
                        html += `<tr><td><strong>${ex.name}</strong></td><td style="text-align:center;">${ex.sets}</td>
                        <td style="text-align:center;">${ex.reps}</td><td>${ex.technique !== 'Nenhuma' ? ex.technique : '-'}</td>
                        <td>${ex.obs || '-'}</td></tr>`;
                    });
                }
                html += `</tbody></table></div>`;
            });

            html += `<div class="print-footer-section">`;
            if(d.stuRecs.trim()) html += `<strong>Recomendações Específicas:</strong><br><p style="white-space: pre-wrap; margin: 3px 0 10px 0;">${d.stuRecs}</p>`;
            html += `<div class="legal-text">${isPEF ? legalTextPEF : legalTextTE}</div>
                     <div style="text-align:center; margin-top: 15px; font-size: 8px;">Ficha impressa em ${d.dateStr} - Plataforma PowFit Pro</div>
                  </div>`;

            document.getElementById('print-area').innerHTML = html;
            setTimeout(() => { window.print(); }, 400);
        };
    </script>
</body>
</html>
