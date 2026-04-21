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
            --bg-body: #0f172a; --bg-card: #1e293b; --text-main: #f8fafc; --text-muted: #94a3b8;
            --border-color: #334155; --primary: #3b82f6; --primary-hover: #2563eb;
            --input-bg: #0f172a; --input-border: #475569; --sidebar-bg: #0b1120;
        }

        [data-theme="Feminino"] {
            --bg-body: #fdf2f8; --bg-card: #ffffff; --text-main: #1e293b; --text-muted: #64748b;
            --border-color: #fbcfe8; --primary: #ec4899; --primary-hover: #db2777;
            --input-bg: #f8fafc; --input-border: #f1f5f9; --sidebar-bg: #fce7f3;
        }

        body { font-family: 'Inter', sans-serif; background-color: var(--bg-body); color: var(--text-main); transition: all 0.3s ease; }
        .card { background-color: var(--bg-card); border: 1px solid var(--border-color); }
        .input-field { background-color: var(--input-bg); border: 1px solid var(--input-border); color: var(--text-main); }
        .input-field:focus { outline: none; border-color: var(--primary); box-shadow: 0 0 0 1px var(--primary); }
        .btn-primary { background-color: var(--primary); color: white; transition: background-color 0.2s; }
        .btn-primary:hover { background-color: var(--primary-hover); }
        .text-primary { color: var(--primary); }
        
        #print-area { display: none; }
        
        /* Loading Overlay */
        #loading-overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.8); z-index: 9999; display: flex; justify-content: center; align-items: center; flex-direction: column; }
        .spinner { border: 4px solid rgba(255,255,255,0.1); border-left-color: var(--primary); border-radius: 50%; width: 40px; height: 40px; animation: spin 1s linear infinite; }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }

        /* Estilo Planilha (A4) */
        @media print {
            body { background: white !important; color: black !important; margin: 0; padding: 0; }
            #app-root, .no-print, #loading-overlay { display: none !important; }
            #print-area { display: block !important; padding: 10mm 15mm; font-family: 'Arial', sans-serif; font-size: 10px; }
            @page { size: A4; margin: 0; }
            
            .print-header { border-bottom: 2px solid #000; padding-bottom: 8px; margin-bottom: 12px; }
            .print-title { font-size: 18px; font-weight: bold; margin: 0; text-transform: uppercase; text-align: center; }
            .prof-info { font-size: 11px; margin-top: 5px; text-align: center; font-weight: bold; }
            
            .print-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 4px; margin-bottom: 15px; border: 1px solid #000; padding: 8px; }
            .print-grid div { font-size: 10.5px; }
            
            .print-guidelines { border: 1px solid #000; padding: 8px; margin-bottom: 15px; font-size: 9px; }
            .print-guidelines h4 { margin: 0 0 4px 0; font-size: 10px; text-transform: uppercase; background: #eee; padding: 3px; border-bottom: 1px solid #ccc; }
            .print-guidelines ul { margin: 0; padding-left: 15px; }
            
            .print-workout { margin-bottom: 15px; page-break-inside: avoid; }
            .print-workout h3 { background: #e0e0e0; border: 1px solid #000; border-bottom: none; padding: 4px 8px; margin: 0; font-size: 12px; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            table { width: 100%; border-collapse: collapse; margin-bottom: 0; border: 1px solid #000; }
            th, td { border: 1px solid #000; padding: 4px; text-align: left; font-size: 9.5px; }
            th { background-color: #f0f0f0; font-weight: bold; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            
            .print-footer-section { margin-top: 15px; border: 1px solid #000; padding: 8px; page-break-inside: avoid; }
            .legal-text { font-size: 7.5px; color: #000; text-align: justify; margin-top: 5px; font-style: italic; border-top: 1px dashed #ccc; padding-top: 5px; }
        }
        
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
    </style>
</head>
<body>

    <!-- LOADING OVERLAY -->
    <div id="loading-overlay">
        <div class="spinner mb-4"></div>
        <p class="text-white text-sm font-medium tracking-widest" id="loading-text">INICIALIZANDO SISTEMA...</p>
    </div>

    <!-- MAIN APP WRAPPER -->
    <div id="app-root" class="flex h-screen overflow-hidden hidden">
        
        <!-- SIDEBAR -->
        <aside class="w-64 flex-shrink-0 border-r flex flex-col transition-colors z-20" style="background-color: var(--sidebar-bg); border-color: var(--border-color);">
            <div class="p-6 border-b border-opacity-20 flex items-center gap-3" style="border-color: var(--border-color);">
                <i class="fas fa-dumbbell text-2xl text-primary"></i>
                <span class="font-bold text-xl tracking-tight">PowFit <span class="text-primary">Pro</span></span>
            </div>
            
            <nav class="flex-1 overflow-y-auto p-4 space-y-2">
                <button onclick="navigate('dashboard')" class="nav-btn w-full flex items-center gap-3 px-4 py-3 rounded-lg text-sm font-medium transition text-left hover:bg-black hover:bg-opacity-10" id="nav-dashboard">
                    <i class="fas fa-chart-pie w-5"></i> Dashboard / Rede
                </button>
                <button onclick="navigate('members')" class="nav-btn w-full flex items-center gap-3 px-4 py-3 rounded-lg text-sm font-medium transition text-left hover:bg-black hover:bg-opacity-10" id="nav-members">
                    <i class="fas fa-users w-5"></i> Equipe (Membros)
                </button>
                <button onclick="navigate('builder')" class="nav-btn w-full flex items-center gap-3 px-4 py-3 rounded-lg text-sm font-medium transition text-left hover:bg-black hover:bg-opacity-10" id="nav-builder">
                    <i class="fas fa-clipboard-list w-5"></i> Montar Ficha
                </button>
                <button onclick="navigate('history')" class="nav-btn w-full flex items-center gap-3 px-4 py-3 rounded-lg text-sm font-medium transition text-left hover:bg-black hover:bg-opacity-10" id="nav-history">
                    <i class="fas fa-history w-5"></i> Histórico na Nuvem
                </button>
                <button onclick="navigate('custom-exercises')" class="nav-btn w-full flex items-center gap-3 px-4 py-3 rounded-lg text-sm font-medium transition text-left hover:bg-black hover:bg-opacity-10" id="nav-custom-exercises">
                    <i class="fas fa-plus-square w-5"></i> Exercícios Custom.
                </button>
            </nav>

            <div class="p-4 border-t border-opacity-20 text-xs" style="border-color: var(--border-color);">
                <div class="flex items-center gap-2 mb-3">
                    <img id="user-avatar" src="" class="w-8 h-8 rounded-full bg-gray-600 hidden" alt="Avatar">
                    <div class="flex-1 truncate">
                        <p class="font-bold truncate" id="user-name-display">Usuário</p>
                        <p class="opacity-60 truncate text-[10px]" id="user-email-display">email@email.com</p>
                    </div>
                </div>
                <button onclick="appLogout()" class="w-full py-2 rounded border border-red-500 text-red-500 hover:bg-red-500 hover:text-white transition font-medium">
                    <i class="fas fa-sign-out-alt"></i> Sair
                </button>
            </div>
        </aside>

        <!-- MAIN CONTENT AREA -->
        <main class="flex-1 flex flex-col h-screen overflow-y-auto relative">
            
            <!-- VIEW: LOGIN -->
            <div id="view-login" class="view-section hidden flex-1 flex items-center justify-center p-6">
                <div class="card p-8 rounded-2xl shadow-2xl max-w-md w-full text-center">
                    <i class="fas fa-dumbbell text-5xl text-primary mb-4"></i>
                    <h2 class="text-2xl font-bold mb-2">Bem-vindo ao PowFit Pro</h2>
                    <p class="text-sm opacity-70 mb-8">Plataforma Master de Gestão de Prescrição para Redes e Franquias.</p>
                    <button onclick="appLogin()" class="w-full bg-white text-gray-800 hover:bg-gray-100 font-bold py-3 px-4 rounded-lg shadow border border-gray-300 flex items-center justify-center gap-3 transition">
                        <img src="https://www.svgrepo.com/show/475656/google-color.svg" class="w-5 h-5" alt="Google">
                        Entrar com Google
                    </button>
                    <p class="mt-6 text-[10px] opacity-50">Seus dados e de sua rede serão salvos na nuvem de forma segura.</p>
                </div>
            </div>

            <!-- VIEW: DASHBOARD (REDE & UNIDADES) -->
            <div id="view-dashboard" class="view-section hidden p-6 lg:p-10 space-y-6">
                <div>
                    <h2 class="text-2xl font-bold"><i class="fas fa-building text-primary mr-2"></i> Gestão da Rede</h2>
                    <p class="text-sm opacity-70">Cadastre e gerencie as unidades da sua franquia.</p>
                </div>
                
                <div class="card p-5 rounded-xl">
                    <h3 class="font-semibold mb-3">Adicionar Nova Unidade</h3>
                    <div class="flex gap-3">
                        <input type="text" id="new-unit-name" class="input-field flex-1 rounded-lg px-3 py-2 text-sm" placeholder="Nome da Unidade (Ex: Unidade Centro)">
                        <button onclick="addUnit()" class="btn-primary px-6 py-2 rounded-lg font-medium text-sm">Adicionar</button>
                    </div>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4" id="units-list">
                    <!-- Unidades JS -->
                </div>
            </div>

            <!-- VIEW: EQUIPE (MEMBROS) -->
            <div id="view-members" class="view-section hidden p-6 lg:p-10 space-y-6">
                <div>
                    <h2 class="text-2xl font-bold"><i class="fas fa-users text-primary mr-2"></i> Gestão da Equipe</h2>
                    <p class="text-sm opacity-70">Cadastre os profissionais que irão prescrever treinos.</p>
                </div>

                <div class="card p-5 rounded-xl space-y-4">
                    <h3 class="font-semibold">Cadastrar Novo Membro</h3>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div>
                            <label class="block text-xs font-medium mb-1">Nome Completo</label>
                            <input type="text" id="mem-name" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                        </div>
                        <div>
                            <label class="block text-xs font-medium mb-1">CPF</label>
                            <input type="text" id="mem-cpf" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                        </div>
                        <div>
                            <label class="block text-xs font-medium mb-1">Unidade</label>
                            <select id="mem-unit" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                <!-- JS -->
                            </select>
                        </div>
                        <div>
                            <label class="block text-xs font-medium mb-1">Categoria de Atuação</label>
                            <select id="mem-role" onchange="toggleMemCref()" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                <option value="PEF">Profissional de Educação Física (PEF)</option>
                                <option value="TE">Treinador Esportivo (TE)</option>
                            </select>
                        </div>
                        <div id="mem-cref-container">
                            <label class="block text-xs font-medium mb-1">CREF</label>
                            <input type="text" id="mem-cref" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: 000000">
                        </div>
                        <div>
                            <label class="block text-xs font-medium mb-1">Estado (UF)</label>
                            <input type="text" id="mem-state" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: SP, RJ" maxlength="2">
                        </div>
                    </div>
                    <button onclick="addMember()" class="btn-primary w-full py-2 rounded-lg font-medium text-sm">Salvar Membro na Rede</button>
                </div>

                <div class="space-y-3" id="members-list">
                    <!-- Membros JS -->
                </div>
            </div>

            <!-- VIEW: MONTAR FICHA -->
            <div id="view-builder" class="view-section hidden p-4 sm:p-6 lg:p-8 space-y-6">
                
                <div class="flex justify-between items-center mb-2">
                    <h2 class="text-2xl font-bold"><i class="fas fa-clipboard-list text-primary mr-2"></i> Criar Ficha de Treino</h2>
                    <button onclick="saveAndPrint()" class="btn-primary px-5 py-2.5 rounded-lg font-bold shadow-lg flex items-center gap-2">
                        <i class="fas fa-save"></i> Salvar & Imprimir
                    </button>
                </div>

                <!-- Quem está prescrevendo? -->
                <div class="card p-4 rounded-xl border-l-4 border-l-primary shadow-sm bg-black bg-opacity-5">
                    <label class="block text-xs font-bold uppercase mb-2 opacity-70">Membro Responsável pela Prescrição</label>
                    <select id="builder-member" onchange="updateBuilderHealth()" class="input-field w-full md:w-1/2 rounded-lg px-3 py-2 text-sm font-semibold">
                        <option value="">Selecione o profissional (Cadastre na aba Equipe primeiro)</option>
                        <!-- JS -->
                    </select>
                </div>

                <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
                    <!-- Coluna Esq: Aluno -->
                    <div class="lg:col-span-4 space-y-6">
                        <div class="card rounded-xl p-5 shadow-sm">
                            <div class="flex justify-between items-center mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                                <h3 class="font-semibold"><i class="fas fa-user text-primary mr-2"></i> Dados do Aluno</h3>
                                <div id="imc-display"></div>
                            </div>
                            <div class="space-y-3">
                                <div>
                                    <label class="block text-xs mb-1">Nome Completo</label>
                                    <input type="text" id="stu-name" class="input-field w-full rounded-lg px-3 py-1.5 text-sm">
                                </div>
                                <div class="grid grid-cols-3 gap-2">
                                    <div><label class="block text-xs mb-1">Idade</label><input type="number" id="stu-age" class="input-field w-full rounded-lg px-2 py-1.5 text-sm"></div>
                                    <div><label class="block text-xs mb-1">Peso(kg)</label><input type="number" id="stu-weight" oninput="calcIMC()" class="input-field w-full rounded-lg px-2 py-1.5 text-sm"></div>
                                    <div><label class="block text-xs mb-1">Altura(m)</label><input type="number" id="stu-height" step="0.01" oninput="calcIMC()" class="input-field w-full rounded-lg px-2 py-1.5 text-sm"></div>
                                </div>
                                <div class="grid grid-cols-2 gap-2">
                                    <div>
                                        <label class="block text-xs mb-1">Gênero</label>
                                        <select id="stu-gender" onchange="setTheme()" class="input-field w-full rounded-lg px-2 py-1.5 text-sm">
                                            <option value="Masculino">Masculino</option><option value="Feminino">Feminino</option>
                                        </select>
                                    </div>
                                    <div>
                                        <label class="block text-xs mb-1">Nível</label>
                                        <select id="stu-level" class="input-field w-full rounded-lg px-2 py-1.5 text-sm">
                                            <option>Iniciante</option><option>Intermediário</option><option>Avançado</option>
                                        </select>
                                    </div>
                                </div>
                                <div>
                                    <label class="block text-xs mb-1">Objetivo Principal</label>
                                    <select id="stu-objective" class="input-field w-full rounded-lg px-3 py-1.5 text-sm">
                                        <option>Emagrecimento</option><option>Hipertrofia</option><option>Definição</option>
                                        <option>Condicionamento</option><option>Resistência</option><option>Força</option>
                                        <option>Reabilitação</option><option>Saúde geral</option>
                                    </select>
                                </div>
                                <div>
                                    <label class="block text-xs mb-1">Frequência Semanal</label>
                                    <select id="stu-freq" class="input-field w-full rounded-lg px-3 py-1.5 text-sm">
                                        <option>3 dias</option><option>5 dias</option><option>6 dias</option><option>Personalizado</option>
                                    </select>
                                </div>
                            </div>
                        </div>

                        <!-- Estado de Saúde -->
                        <div class="card rounded-xl p-4 shadow-sm">
                            <h3 class="font-semibold mb-1"><i class="fas fa-heartbeat text-primary mr-2"></i> Estado de Saúde</h3>
                            <p class="text-[10px] opacity-70 mb-3">Diretrizes automáticas baseadas na categoria do membro selecionado.</p>
                            <div class="grid grid-cols-2 gap-1.5 text-[11px]" id="health-container">
                                <!-- JS -->
                            </div>
                        </div>

                        <!-- Validade e Recomendações -->
                        <div class="card rounded-xl p-4 shadow-sm space-y-3">
                            <div>
                                <label class="block text-xs mb-1">Validade da Ficha</label>
                                <select id="stu-validity" class="input-field w-full rounded px-2 py-1.5 text-sm">
                                    <option>15 dias</option><option selected>30 dias</option><option>60 dias</option><option>90 dias</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs mb-1">Recomendações Manuais</label>
                                <textarea id="stu-recs" rows="3" class="input-field w-full rounded px-2 py-1.5 text-sm" placeholder="Hidratação, descanso, cuidados..."></textarea>
                            </div>
                        </div>
                    </div>

                    <!-- Coluna Dir: Construtor -->
                    <div class="lg:col-span-8 space-y-4">
                        <div class="flex flex-col sm:flex-row justify-between items-center bg-opacity-10 p-3 rounded-xl card border-dashed border-2 gap-3">
                            <div>
                                <h3 class="text-lg font-bold">Dias de Treino</h3>
                                <p class="text-[11px] opacity-70">Adicione os dias e preencha livremente.</p>
                            </div>
                            <button onclick="addDay()" class="btn-primary px-4 py-2 rounded-lg text-sm font-medium w-full sm:w-auto">
                                + Adicionar Dia
                            </button>
                        </div>
                        <div id="builder-days" class="space-y-4">
                            <!-- Dias JS -->
                        </div>
                    </div>
                </div>
            </div>

            <!-- VIEW: HISTÓRICO NA NUVEM -->
            <div id="view-history" class="view-section hidden p-6 lg:p-10 flex flex-col h-full">
                <div class="flex justify-between items-end mb-6">
                    <div>
                        <h2 class="text-2xl font-bold"><i class="fas fa-cloud text-primary mr-2"></i> Histórico & Relatórios</h2>
                        <p class="text-sm opacity-70">Fichas salvas na nuvem da sua rede.</p>
                    </div>
                    <button onclick="printReport()" class="bg-gray-700 text-white px-4 py-2 rounded-lg text-sm font-medium hover:bg-gray-600 transition">
                        <i class="fas fa-print"></i> Relatório Mensal
                    </button>
                </div>
                
                <div class="card p-4 rounded-xl mb-4 flex gap-4 overflow-x-auto">
                    <select id="filter-unit" class="input-field rounded px-3 py-1.5 text-sm min-w-[150px]" onchange="renderHistory()">
                        <option value="">Todas as Unidades</option>
                    </select>
                    <select id="filter-member" class="input-field rounded px-3 py-1.5 text-sm min-w-[150px]" onchange="renderHistory()">
                        <option value="">Todos os Membros</option>
                    </select>
                    <select id="filter-month" class="input-field rounded px-3 py-1.5 text-sm min-w-[150px]" onchange="renderHistory()">
                        <option value="">Todos os Meses</option>
                        <option value="01">Janeiro</option><option value="02">Fevereiro</option><option value="03">Março</option>
                        <option value="04">Abril</option><option value="05">Maio</option><option value="06">Junho</option>
                        <option value="07">Julho</option><option value="08">Agosto</option><option value="09">Setembro</option>
                        <option value="10">Outubro</option><option value="11">Novembro</option><option value="12">Dezembro</option>
                    </select>
                </div>

                <div class="flex-1 overflow-y-auto space-y-2 pr-2" id="history-list">
                    <!-- Fichas JS -->
                </div>
            </div>

            <!-- VIEW: EXERCÍCIOS CUSTOMIZADOS -->
            <div id="view-custom-exercises" class="view-section hidden p-6 lg:p-10">
                <h2 class="text-2xl font-bold mb-2"><i class="fas fa-plus-square text-primary mr-2"></i> Banco Personalizado</h2>
                <p class="text-sm opacity-70 mb-6">Adicione exercícios únicos para a sua rede. Eles aparecerão no final da lista da categoria selecionada.</p>
                
                <div class="card p-5 rounded-xl mb-6">
                    <div class="flex flex-col sm:flex-row gap-3">
                        <select id="custom-cat" class="input-field rounded-lg px-3 py-2 text-sm w-full sm:w-1/3">
                            <option value="🔥 PEITO">🔥 PEITO</option><option value="🦍 COSTAS">🦍 COSTAS</option>
                            <option value="🦵 PERNAS">🦵 PERNAS</option><option value="💪 BRAÇOS">💪 BRAÇOS</option>
                            <option value="🪨 OMBROS">🪨 OMBROS</option><option value="🧠 ABDÔMEN">🧠 ABDÔMEN</option>
                            <option value="🫀 CARDIO">🫀 CARDIO</option>
                        </select>
                        <input type="text" id="custom-name" class="input-field rounded-lg px-3 py-2 text-sm flex-1" placeholder="Nome do Exercício">
                        <button onclick="addCustomExercise()" class="btn-primary px-6 py-2 rounded-lg font-medium text-sm">Salvar Exercício</button>
                    </div>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-3" id="custom-ex-list">
                    <!-- Exercicios Custom JS -->
                </div>
            </div>

        </main>
    </div>

    <!-- MODAL DE EXERCÍCIOS PARA O BUILDER -->
    <div id="modal-exercises" class="fixed inset-0 bg-black bg-opacity-70 backdrop-blur-sm hidden z-[60] flex items-center justify-center p-2 sm:p-4">
        <div class="card w-full max-w-5xl rounded-xl flex flex-col h-[90vh]">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold">Adicionar Exercício</h3>
                <button onclick="closeExModal()" class="text-gray-400 hover:text-red-500 text-2xl leading-none">&times;</button>
            </div>
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <div class="w-full md:w-1/4 border-r overflow-y-auto p-2 space-y-1" style="border-color: var(--border-color)" id="modal-cat-list"></div>
                <div class="w-full md:w-3/4 overflow-y-auto p-3 bg-black bg-opacity-5" id="modal-ex-items"></div>
            </div>
        </div>
    </div>

    <!-- ÁREA DE IMPRESSÃO (Oculta) -->
    <div id="print-area"></div>


    <!-- ==================== LÓGICA JAVASCRIPT ==================== -->
    <script type="module">
        // 1. IMPORTAÇÕES FIREBASE (Módulos CDN)
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInWithPopup, GoogleAuthProvider, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, addDoc, getDocs, updateDoc, deleteDoc, doc, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // 2. CONFIGURAÇÃO FIREBASE (Fornecida no prompt)
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

        // 3. DADOS ESTÁTICOS / REGRAS DE NEGÓCIO
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
            "🍬 Diabetes": "Pessoas com diabetes devem manter acompanhamento médico regular. A musculação pode auxiliar na rotina física quando liberada por profissional de saúde. Em casos de tontura, mal-estar ou alteração de glicemia, o treino deve ser interrompido e o aluno deve procurar orientação médica.",
            "❤️ Hipertensão": "Pessoas com hipertensão devem manter acompanhamento médico regular e respeitar as orientações profissionais. Evitar prender a respiração e controlar a intensidade dos exercícios.",
            "🔵 Hipotensão": "Pessoas com hipotensão devem evitar mudanças bruscas de posição durante o treino. Manter boa hidratação e respeitar a intensidade adequada ajuda a reduzir episódios de mal-estar.",
            "💔 Problemas cardíacos": "A prática de exercícios deve ocorrer apenas com liberação e acompanhamento médico. O treino deve respeitar limites individuais, com controle de intensidade.",
            "🦴 Problemas articulares": "Exercícios com menor impacto e maior controle de movimento costumam ser mais indicados. O acompanhamento profissional ajuda na segurança.",
            "🫁 Problemas respiratórios": "A progressão do treino deve ser gradual e respeitar a capacidade respiratória individual. Em caso de falta de ar excessiva, interromper.",
            "⚠️ Lesões": "A adaptação dos exercícios deve respeitar a limitação existente e evitar dor. Seguir orientação profissional adequada antes da prática.",
            "🤰 Gestante": "A prática deve ocorrer com liberação médica e acompanhamento adequado. Foco deve ser segurança, mobilidade e bem-estar, evitando impacto e risco.",
            "🤱 Lactante": "A atividade física pode ser mantida normalmente, respeitando a recuperação, hidratação e alimentação adequada.",
            "👴 Idoso": "A musculação pode contribuir para força, equilíbrio e mobilidade. Respeitar limitações individuais, intensidade moderada e segurança."
        };

        const objectiveData = {
            "Emagrecimento": "Foco em déficit calórico com treinos mistos de força (manter massa magra) e aeróbicos (maior gasto).",
            "Hipertrofia": "Prioridade na progressão de carga e volume adequado. Essencial superávit calórico e descanso.",
            "Definição": "Manutenção de massa muscular enquanto reduz o percentual de gordura. Atenção estrita à dieta.",
            "Condicionamento": "Treinos com menor tempo de intervalo, circuitos e alta integração cardiopulmonar.",
            "Resistência": "Séries mais longas, cadência controlada e aprimoramento da capacidade muscular.",
            "Força": "Cargas altas, baixas repetições e intervalos de descanso maiores.",
            "Reabilitação": "Treino focado em fortalecimento específico, mobilidade e controle motor. Respeitar limites.",
            "Saúde geral": "Equilíbrio entre força, cardio e flexibilidade. O principal objetivo é a constância e bem-estar."
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

        // 4. ESTADO GLOBAL DA APLICAÇÃO
        let appState = {
            user: null,
            units: [],
            members: [],
            history: [],
            customExercises: [],
            
            // Estado do construtor
            currentFichaId: null,
            builderDays: [],
            modalActiveDayIdx: null,
            modalActiveCat: "🔥 PEITO"
        };

        // 5. FUNÇÕES BASE (Auth e DB)
        function showLoading(text = "CARREGANDO...") {
            document.getElementById('loading-text').innerText = text;
            document.getElementById('loading-overlay').classList.remove('hidden');
        }
        function hideLoading() { document.getElementById('loading-overlay').classList.add('hidden'); }
        
        // Expõe funções pro HTML
        window.appLogin = async () => {
            showLoading("CONECTANDO AO GOOGLE...");
            try {
                const provider = new GoogleAuthProvider();
                await signInWithPopup(auth, provider);
            } catch (error) {
                hideLoading();
                alert("Erro ao logar. Se estiver em um visualizador restrito, abra esta página em uma nova guia. Detalhe: " + error.message);
            }
        };

        window.appLogout = async () => {
            if(confirm("Deseja realmente sair?")) {
                showLoading("SAINDO...");
                await signOut(auth);
            }
        };

        window.navigate = (viewId) => {
            document.querySelectorAll('.view-section').forEach(el => el.classList.add('hidden'));
            document.querySelectorAll('.nav-btn').forEach(el => el.classList.remove('text-primary', 'bg-black', 'bg-opacity-20'));
            
            document.getElementById('view-' + viewId).classList.remove('hidden');
            if(document.getElementById('nav-' + viewId)) {
                document.getElementById('nav-' + viewId).classList.add('text-primary', 'bg-black', 'bg-opacity-20');
            }
            if(viewId === 'history') renderHistory();
            if(viewId === 'builder') populateBuilderMembers();
        };

        // LISTENER DE AUTH
        onAuthStateChanged(auth, async (user) => {
            if (user) {
                appState.user = user;
                document.getElementById('user-name-display').innerText = user.displayName || 'Gestor da Rede';
                document.getElementById('user-email-display').innerText = user.email;
                if(user.photoURL) {
                    document.getElementById('user-avatar').src = user.photoURL;
                    document.getElementById('user-avatar').classList.remove('hidden');
                }
                
                document.getElementById('app-root').classList.remove('hidden');
                document.getElementById('view-login').classList.add('hidden');
                
                showLoading("CARREGANDO DADOS DA REDE...");
                await fetchAllData();
                navigate('dashboard');
                hideLoading();
            } else {
                appState.user = null;
                document.getElementById('app-root').classList.add('hidden');
                document.getElementById('view-login').classList.remove('hidden');
                document.getElementById('view-login').classList.add('flex');
                hideLoading();
            }
        });

        // FETCH ALL DATA (Simple queries due to rules)
        async function fetchAllData() {
            if(!appState.user) return;
            const uid = appState.user.uid;
            
            try {
                // Fetch Units
                const unitsSnap = await getDocs(collection(db, `users/${uid}/units`));
                appState.units = unitsSnap.docs.map(d => ({id: d.id, ...d.data()}));
                
                // Fetch Members
                const memSnap = await getDocs(collection(db, `users/${uid}/members`));
                appState.members = memSnap.docs.map(d => ({id: d.id, ...d.data()}));

                // Fetch History (Fichas)
                const histSnap = await getDocs(collection(db, `users/${uid}/fichas`));
                appState.history = histSnap.docs.map(d => ({id: d.id, ...d.data()}));

                // Fetch Custom Exercises
                const exSnap = await getDocs(collection(db, `users/${uid}/custom_exercises`));
                appState.customExercises = exSnap.docs.map(d => ({id: d.id, ...d.data()}));

                renderUnits();
                renderMembers();
                renderCustomEx();
            } catch (e) {
                console.error("Erro ao buscar dados:", e);
                alert("Erro ao buscar dados do servidor.");
            }
        }

        // 6. GESTÃO DE REDE / UNIDADES
        window.addUnit = async () => {
            const name = document.getElementById('new-unit-name').value.trim();
            if(!name) return alert('Preencha o nome da unidade');
            showLoading("SALVANDO...");
            try {
                const docRef = await addDoc(collection(db, `users/${appState.user.uid}/units`), { name });
                appState.units.push({id: docRef.id, name});
                document.getElementById('new-unit-name').value = '';
                renderUnits();
                renderMembers(); // refresh dropdowns
            } catch(e) { alert("Erro ao salvar unidade."); }
            hideLoading();
        };

        window.deleteUnit = async (id) => {
            if(!confirm("Atenção: Excluir a unidade não exclui os membros atrelados a ela, mas eles ficarão sem unidade. Continuar?")) return;
            showLoading("EXCLUINDO...");
            try {
                await deleteDoc(doc(db, `users/${appState.user.uid}/units`, id));
                appState.units = appState.units.filter(u => u.id !== id);
                renderUnits();
            } catch(e) { alert("Erro ao excluir."); }
            hideLoading();
        }

        function renderUnits() {
            const container = document.getElementById('units-list');
            if(appState.units.length === 0) {
                container.innerHTML = `<div class="col-span-full opacity-50 text-sm">Nenhuma unidade cadastrada.</div>`;
                return;
            }
            container.innerHTML = appState.units.map(u => {
                const memCount = appState.members.filter(m => m.unitId === u.id).length;
                return `
                <div class="card p-4 rounded-xl flex justify-between items-center">
                    <div><h4 class="font-bold">${u.name}</h4><p class="text-xs opacity-60">${memCount} Membros na equipe</p></div>
                    <button onclick="deleteUnit('${u.id}')" class="text-red-500 hover:bg-red-500 hover:text-white p-2 rounded transition"><i class="fas fa-trash"></i></button>
                </div>`
            }).join('');

            // Atualiza dropdown de cadastro de membro
            const memUnitSel = document.getElementById('mem-unit');
            if(memUnitSel) {
                memUnitSel.innerHTML = appState.units.map(u => `<option value="${u.id}">${u.name}</option>`).join('');
            }
        }

        // 7. GESTÃO DE EQUIPE (MEMBROS)
        window.toggleMemCref = () => {
            const role = document.getElementById('mem-role').value;
            document.getElementById('mem-cref-container').style.display = role === 'TE' ? 'none' : 'block';
        };

        window.addMember = async () => {
            const name = document.getElementById('mem-name').value.trim();
            const cpf = document.getElementById('mem-cpf').value.trim();
            const unitId = document.getElementById('mem-unit').value;
            const role = document.getElementById('mem-role').value;
            const cref = document.getElementById('mem-cref').value.trim();
            const stateUf = document.getElementById('mem-state').value.trim().toUpperCase();

            if(!name || !unitId || !stateUf) return alert('Preencha Nome, Unidade e Estado.');
            if(role === 'PEF' && !cref) return alert('CREF é obrigatório para PEF.');

            showLoading("SALVANDO MEMBRO...");
            const data = { name, cpf, unitId, role, cref: role === 'TE' ? '' : cref, state: stateUf };
            try {
                const docRef = await addDoc(collection(db, `users/${appState.user.uid}/members`), data);
                appState.members.push({id: docRef.id, ...data});
                document.getElementById('mem-name').value = '';
                document.getElementById('mem-cpf').value = '';
                document.getElementById('mem-cref').value = '';
                renderMembers();
                renderUnits(); // refresh contagem
            } catch(e) { alert("Erro ao salvar membro."); }
            hideLoading();
        };

        window.deleteMember = async (id) => {
            if(!confirm("Excluir este membro da equipe? O histórico de fichas dele será mantido na nuvem.")) return;
            showLoading("EXCLUINDO...");
            try {
                await deleteDoc(doc(db, `users/${appState.user.uid}/members`, id));
                appState.members = appState.members.filter(m => m.id !== id);
                renderMembers();
                renderUnits();
            } catch(e) { alert("Erro ao excluir."); }
            hideLoading();
        }

        function renderMembers() {
            const container = document.getElementById('members-list');
            if(appState.members.length === 0) {
                container.innerHTML = `<div class="opacity-50 text-sm">Nenhum membro cadastrado.</div>`;
                return;
            }
            container.innerHTML = appState.members.map(m => {
                const u = appState.units.find(un => un.id === m.unitId);
                const unitName = u ? u.name : 'Unidade Excluída';
                const roleBadge = m.role === 'PEF' ? 'PEF (Ed. Física)' : 'TE (Treinador)';
                return `
                <div class="card p-4 rounded-xl flex justify-between items-center border-l-4 border-l-primary">
                    <div>
                        <h4 class="font-bold">${m.name} <span class="text-[10px] bg-primary bg-opacity-20 text-primary px-2 py-0.5 rounded ml-2">${roleBadge}</span></h4>
                        <p class="text-xs opacity-60">Unidade: ${unitName} | UF: ${m.state} ${m.role === 'PEF' ? '| CREF: '+m.cref : ''}</p>
                    </div>
                    <button onclick="deleteMember('${m.id}')" class="text-red-500 hover:bg-red-500 hover:text-white p-2 rounded transition"><i class="fas fa-trash"></i></button>
                </div>`
            }).join('');
        }

        // 8. BUILDER (MONTAR FICHA)
        function populateBuilderMembers() {
            const sel = document.getElementById('builder-member');
            sel.innerHTML = `<option value="">Selecione o membro...</option>` + 
                appState.members.map(m => {
                    const u = appState.units.find(un => un.id === m.unitId);
                    return `<option value="${m.id}">${m.name} (${u ? u.name : 'Sem unidade'})</option>`;
                }).join('');
            
            // Auto add 1st day if empty
            if(appState.builderDays.length === 0) addDay();
        }

        window.updateBuilderHealth = () => {
            const memId = document.getElementById('builder-member').value;
            const mem = appState.members.find(m => m.id === memId);
            const container = document.getElementById('health-container');
            if(!mem) {
                container.innerHTML = '<span class="opacity-50 col-span-2">Selecione o membro para ver as diretrizes.</span>';
                return;
            }
            const dataObj = mem.role === 'PEF' ? healthDataPEF : healthDataTE;
            container.innerHTML = Object.keys(dataObj).map(opt => `
                <label class="flex items-center space-x-1.5 cursor-pointer hover:bg-black hover:bg-opacity-10 p-1 rounded transition">
                    <input type="checkbox" value="${opt}" class="health-cb rounded text-primary focus:ring-primary w-3 h-3 border-gray-500">
                    <span class="truncate" title="${opt}">${opt}</span>
                </label>
            `).join('');
        };

        window.calcIMC = () => {
            const w = parseFloat(document.getElementById('stu-weight').value);
            const h = parseFloat(document.getElementById('stu-height').value);
            const display = document.getElementById('imc-display');
            if (w > 0 && h > 0) {
                const imc = (w / (h * h)).toFixed(1);
                let classif = "";
                if (imc < 18.5) classif = "Abaixo do peso";
                else if (imc < 25) classif = "Peso normal";
                else if (imc < 30) classif = "Sobrepeso";
                else if (imc < 35) classif = "Obesidade I";
                else if (imc < 40) classif = "Obesidade II";
                else classif = "Obesidade III";
                display.innerHTML = `<span class="bg-primary bg-opacity-20 text-primary px-2 py-0.5 rounded text-[10px] font-bold">IMC: ${imc} (${classif})</span>`;
            } else { display.innerHTML = ''; }
        };

        window.setTheme = () => { document.body.setAttribute('data-theme', document.getElementById('stu-gender').value); };

        window.addDay = () => {
            const c = appState.builderDays.length;
            const title = c < 7 ? `TREINO ${daysSequence[c]}` : `NOVO TREINO ${c+1}`;
            appState.builderDays.push({ title, exercises: [] });
            renderBuilderDays();
        };

        window.removeDay = (idx) => {
            if(confirm('Excluir dia de treino?')) {
                appState.builderDays.splice(idx, 1);
                renderBuilderDays();
            }
        };

        window.updateDayTitle = (idx, val) => { appState.builderDays[idx].title = val; };

        function renderBuilderDays() {
            const container = document.getElementById('builder-days');
            container.innerHTML = appState.builderDays.map((day, dIdx) => {
                let exHtml = day.exercises.length === 0 ? `<div class="p-3 text-center text-xs opacity-50">Vazio</div>` :
                    day.exercises.map((ex, eIdx) => `
                        <div class="flex items-center gap-2 p-2 border-b last:border-0 border-opacity-20 text-xs" style="border-color: var(--border-color);">
                            <div class="flex-1 min-w-0">
                                <p class="text-[9px] opacity-60 uppercase truncate">${ex.category}</p>
                                <p class="font-medium truncate">${ex.name}</p>
                            </div>
                            <input type="text" value="${ex.sets}" onchange="appState.builderDays[${dIdx}].exercises[${eIdx}].sets=this.value" class="input-field w-10 text-center rounded p-1" placeholder="S">
                            <span class="opacity-50">x</span>
                            <input type="text" value="${ex.reps}" onchange="appState.builderDays[${dIdx}].exercises[${eIdx}].reps=this.value" class="input-field w-12 text-center rounded p-1" placeholder="R">
                            <select onchange="appState.builderDays[${dIdx}].exercises[${eIdx}].technique=this.value" class="input-field w-20 rounded p-1 text-[10px]">
                                ${techniques.map(t => `<option ${ex.technique===t?'selected':''}>${t}</option>`).join('')}
                            </select>
                            <input type="text" value="${ex.obs}" onchange="appState.builderDays[${dIdx}].exercises[${eIdx}].obs=this.value" class="input-field w-24 rounded p-1 hidden sm:block" placeholder="Obs">
                            <button onclick="appState.builderDays[${dIdx}].exercises.splice(${eIdx},1); renderBuilderDays();" class="text-red-500 px-2"><i class="fas fa-trash"></i></button>
                        </div>
                    `).join('');

                return `
                <div class="card rounded-xl overflow-hidden shadow-sm">
                    <div class="bg-black bg-opacity-10 p-2 flex justify-between items-center border-b" style="border-color: var(--border-color)">
                        <input type="text" value="${day.title}" onchange="updateDayTitle(${dIdx}, this.value)" class="input-field bg-transparent font-bold text-sm w-1/2 px-2 py-1 rounded">
                        <button onclick="removeDay(${dIdx})" class="text-red-500 hover:bg-red-500 hover:text-white px-2 py-1 rounded text-xs transition"><i class="fas fa-trash"></i> Excluir</button>
                    </div>
                    <div>${exHtml}</div>
                    <div class="p-2 border-t" style="border-color: var(--border-color)">
                        <button onclick="openExModal(${dIdx})" class="w-full text-primary hover:bg-primary hover:bg-opacity-10 py-1.5 rounded text-xs font-bold transition">+ Adicionar Exercício</button>
                    </div>
                </div>`;
            }).join('');
        }

        // 9. MODAL DE EXERCÍCIOS
        window.openExModal = (dIdx) => {
            appState.modalActiveDayIdx = dIdx;
            document.getElementById('modal-exercises').classList.remove('hidden');
            renderExCategories();
            renderExItems();
        };
        window.closeExModal = () => { document.getElementById('modal-exercises').classList.add('hidden'); };
        
        function renderExCategories() {
            const list = document.getElementById('modal-cat-list');
            list.innerHTML = Object.keys(defaultCategories).map(cat => `
                <button onclick="appState.modalActiveCat='${cat}'; renderExCategories(); renderExItems();" class="w-full text-left px-2 py-2 rounded text-xs font-medium ${appState.modalActiveCat === cat ? 'bg-primary text-white' : 'hover:bg-black hover:bg-opacity-10'}">${cat}</button>
            `).join('');
        }

        function renderExItems() {
            const container = document.getElementById('modal-ex-items');
            const cat = appState.modalActiveCat;
            // Merge defaults and customs
            const defaults = defaultCategories[cat] || [];
            const customs = appState.customExercises.filter(c => c.category === cat).map(c => c.name);
            const all = [...defaults, ...customs];

            container.innerHTML = `<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-2">` + 
                all.map(ex => {
                    const isCardio = cat === "🫀 CARDIO";
                    return `<button onclick="addExToDay('${ex}', ${isCardio})" class="card p-2 rounded text-left text-xs font-medium hover:border-primary transition flex justify-between group">
                        <span>${ex}</span> <i class="fas fa-plus opacity-0 group-hover:opacity-100 text-primary"></i>
                    </button>`
                }).join('') + `</div>`;
        }

        window.addExToDay = (name, isCardio) => {
            if(appState.modalActiveDayIdx !== null) {
                appState.builderDays[appState.modalActiveDayIdx].exercises.push({
                    category: appState.modalActiveCat,
                    name: name,
                    sets: isCardio ? '1' : '3',
                    reps: isCardio ? '-' : '10 a 12',
                    technique: 'Nenhuma',
                    obs: ''
                });
                renderBuilderDays();
                // Visual feedback
                const container = document.getElementById('modal-ex-items');
                container.style.opacity = '0.5';
                setTimeout(() => container.style.opacity = '1', 100);
            }
        };

        // 10. SALVAR E IMPRIMIR FICHA
        window.saveAndPrint = async () => {
            const memId = document.getElementById('builder-member').value;
            if(!memId) return alert('Selecione o Membro Responsável no topo.');
            const stuName = document.getElementById('stu-name').value.trim();
            if(!stuName) return alert('Digite o nome do aluno.');

            showLoading("SALVANDO NA NUVEM...");
            
            // Construir payload
            const payload = {
                memberId: memId,
                studentName: stuName,
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
                days: appState.builderDays,
                timestamp: new Date().toISOString()
            };

            try {
                if(appState.currentFichaId) {
                    await updateDoc(doc(db, `users/${appState.user.uid}/fichas`, appState.currentFichaId), payload);
                    const idx = appState.history.findIndex(h => h.id === appState.currentFichaId);
                    if(idx>=0) appState.history[idx] = {id: appState.currentFichaId, ...payload};
                } else {
                    const docRef = await addDoc(collection(db, `users/${appState.user.uid}/fichas`), payload);
                    appState.currentFichaId = docRef.id;
                    appState.history.push({id: docRef.id, ...payload});
                }
                executePrint(payload);
            } catch(e) {
                console.error(e);
                alert("Erro ao salvar no banco.");
            }
            hideLoading();
        };

        function executePrint(data) {
            const mem = appState.members.find(m => m.id === data.memberId);
            const unit = appState.units.find(u => u.id === mem.unitId);
            
            let imcStr = "-";
            const w = parseFloat(data.stuWeight); const h = parseFloat(data.stuHeight);
            if(w>0 && h>0) imcStr = (w / (h * h)).toFixed(1);

            const isPEF = mem.role === 'PEF';
            const healthSourceDict = isPEF ? healthDataPEF : healthDataTE;
            const objRecommendation = objectiveData[data.stuObjective] || "";

            const profLabel = isPEF ? 'Profissional de Educação Física' : 'Treinador Esportivo';
            const crefText = isPEF ? `CREF: ${mem.cref} - Estado: ${mem.state}` : `Atuação Técnica e Esportiva - ${mem.state}`;
            
            const legalTextPEF = `⚠️ OBSERVAÇÃO LEGAL – PROFISSIONAL DE EDUCAÇÃO FÍSICA: Conforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º estabelece que compete ao profissional coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos.`;
            const legalTextTE = `⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO: Conforme a Lei nº 14.597/2023, Art. 75, a profissão de treinador esportivo é reconhecida, com atuação de caráter técnico voltada à preparação de treinos. A atuação possui finalidade orientativa e não substitui avaliação médica. Em casos de doenças ou lesões, recomenda-se avaliação prévia. As recomendações possuem caráter informativo e orientativo.`;

            let html = `
                <div class="print-header">
                    <h1 class="print-title">Planilha de Treinamento - ${data.stuObjective}</h1>
                    <div class="prof-info">
                        Prescrição por: ${mem.name} (${profLabel})<br>
                        ${crefText} | Unidade: ${unit ? unit.name : ''}
                    </div>
                </div>

                <div class="print-grid">
                    <div><strong>Aluno:</strong> ${data.studentName}</div>
                    <div><strong>Idade:</strong> ${data.stuAge || '-'} anos</div>
                    <div><strong>Gênero:</strong> ${data.stuGender}</div>
                    <div><strong>Peso:</strong> ${data.stuWeight || '-'} kg</div>
                    <div><strong>Altura:</strong> ${data.stuHeight || '-'} m</div>
                    <div><strong>IMC:</strong> ${imcStr}</div>
                    <div><strong>Nível:</strong> ${data.stuLevel}</div>
                    <div><strong>Frequência:</strong> ${data.stuFreq}</div>
                    <div><strong>Validade:</strong> ${data.stuValidity}</div>
                </div>
            `;

            if (objRecommendation || data.health.length > 0) {
                html += `<div class="print-guidelines"><h4>Diretrizes de Perfil</h4><ul>`;
                if(objRecommendation) html += `<li><strong>Objetivo (${data.stuObjective}):</strong> ${objRecommendation}</li>`;
                data.health.forEach(k => {
                    if(healthSourceDict[k]) html += `<li><strong>Saúde (${k.replace(/[\u{1F300}-\u{1F9FF}\u{2600}-\u{26FF}\u{2700}-\u{27BF}\u{FE0F}]/gu, '').trim()}):</strong> ${healthSourceDict[k]}</li>`;
                });
                html += `</ul></div>`;
            }

            data.days.forEach(day => {
                html += `<div class="print-workout"><h3>${day.title}</h3><table><thead><tr>
                            <th style="width:35%">Exercício</th><th style="width:8%;text-align:center;">Séries</th>
                            <th style="width:12%;text-align:center;">Reps</th><th style="width:15%">Técnica</th><th style="width:30%">Obs</th>
                         </tr></thead><tbody>`;
                if (day.exercises.length === 0) {
                    html += `<tr><td colspan="5" style="text-align:center;font-style:italic;">Sem exercícios</td></tr>`;
                } else {
                    day.exercises.forEach(ex => {
                        html += `<tr><td><strong>${ex.name}</strong></td><td style="text-align:center;">${ex.sets}</td>
                                 <td style="text-align:center;">${ex.reps}</td><td>${ex.technique!=='Nenhuma'?ex.technique:'-'}</td><td>${ex.obs||'-'}</td></tr>`;
                    });
                }
                html += `</tbody></table></div>`;
            });

            html += `<div class="print-footer-section">`;
            if(data.stuRecs.trim()) html += `<strong>Recomendações Específicas:</strong><br><p style="white-space: pre-wrap; margin: 3px 0 10px 0;">${data.stuRecs}</p>`;
            html += `<div class="legal-text">${isPEF ? legalTextPEF : legalTextTE}</div>
                     <div style="text-align:center; margin-top: 10px; font-size: 7px; color: #666;">Gerado por PowFit Pro Master em ${new Date().toLocaleDateString('pt-BR')}</div>
                     </div>`;

            document.getElementById('print-area').innerHTML = html;
            setTimeout(() => { window.print(); }, 400);
        }

        // 11. HISTÓRICO E RELATÓRIOS (NUVEM)
        window.renderHistory = () => {
            const fUnit = document.getElementById('filter-unit').value;
            const fMem = document.getElementById('filter-member').value;
            const fMonth = document.getElementById('filter-month').value;

            // Update dropdowns se vazio
            if(document.getElementById('filter-unit').options.length === 1) {
                document.getElementById('filter-unit').innerHTML += appState.units.map(u => `<option value="${u.id}">${u.name}</option>`).join('');
                document.getElementById('filter-member').innerHTML += appState.members.map(m => `<option value="${m.id}">${m.name}</option>`).join('');
            }

            let filtered = [...appState.history];

            if(fUnit || fMem) {
                filtered = filtered.filter(h => {
                    const mem = appState.members.find(m => m.id === h.memberId);
                    if(!mem) return false;
                    if(fUnit && mem.unitId !== fUnit) return false;
                    if(fMem && h.memberId !== fMem) return false;
                    return true;
                });
            }

            if(fMonth) {
                filtered = filtered.filter(h => {
                    if(!h.timestamp) return false;
                    const m = new Date(h.timestamp).getMonth() + 1;
                    return m.toString().padStart(2, '0') === fMonth;
                });
            }

            // Ordernar decrescente
            filtered.sort((a,b) => new Date(b.timestamp||0) - new Date(a.timestamp||0));

            const list = document.getElementById('history-list');
            if(filtered.length === 0) {
                list.innerHTML = `<div class="text-center opacity-50 py-10">Nenhuma ficha encontrada.</div>`;
                return;
            }

            const now = new Date();
            list.innerHTML = filtered.map(h => {
                const mem = appState.members.find(m => m.id === h.memberId);
                const memName = mem ? mem.name : 'Excluído';
                
                // Validação de data
                let statusBadge = `<span class="bg-green-500 bg-opacity-20 text-green-400 text-[10px] px-2 py-0.5 rounded font-bold">ATIVA</span>`;
                if(h.timestamp && h.stuValidity) {
                    const created = new Date(h.timestamp);
                    const daysValid = parseInt(h.stuValidity); // "30 dias" -> 30
                    const expires = new Date(created);
                    expires.setDate(expires.getDate() + daysValid);
                    if(now > expires) {
                        statusBadge = `<span class="bg-red-500 bg-opacity-20 text-red-400 text-[10px] px-2 py-0.5 rounded font-bold">VENCIDA</span>`;
                    }
                }

                return `
                <div class="card p-3 rounded-lg flex justify-between items-center border border-opacity-20">
                    <div class="min-w-0 flex-1">
                        <div class="font-bold flex items-center gap-2 truncate">${h.studentName} ${statusBadge}</div>
                        <div class="text-[10px] opacity-70 truncate">Criada por: ${memName} | ${new Date(h.timestamp).toLocaleDateString('pt-BR')}</div>
                    </div>
                    <div class="flex gap-2 ml-4">
                        <button onclick="loadFicha('${h.id}')" class="btn-primary px-3 py-1.5 rounded text-xs font-semibold">EDITAR</button>
                    </div>
                </div>`
            }).join('');
        };

        window.loadFicha = (id) => {
            const h = appState.history.find(x => x.id === id);
            if(!h) return;
            appState.currentFichaId = id;
            
            document.getElementById('builder-member').value = h.memberId || '';
            updateBuilderHealth();

            document.getElementById('stu-name').value = h.studentName || '';
            document.getElementById('stu-age').value = h.stuAge || '';
            document.getElementById('stu-weight').value = h.stuWeight || '';
            document.getElementById('stu-height').value = h.stuHeight || '';
            document.getElementById('stu-gender').value = h.stuGender || 'Masculino';
            document.getElementById('stu-level').value = h.stuLevel || 'Iniciante';
            document.getElementById('stu-objective').value = h.stuObjective || 'Emagrecimento';
            document.getElementById('stu-freq').value = h.stuFreq || '3 dias';
            document.getElementById('stu-validity').value = h.stuValidity || '30 dias';
            document.getElementById('stu-recs').value = h.stuRecs || '';
            
            setTheme();
            calcIMC();

            document.querySelectorAll('.health-cb').forEach(cb => {
                cb.checked = (h.health || []).includes(cb.value);
            });

            appState.builderDays = h.days || [];
            renderBuilderDays();
            navigate('builder');
        };

        window.printReport = () => {
            // Relatório de produtividade (conta fichas da visualização atual)
            const fUnit = document.getElementById('filter-unit').value;
            const fMonth = document.getElementById('filter-month').value;
            
            let filtered = [...appState.history];
            if(fMonth) {
                filtered = filtered.filter(h => {
                    if(!h.timestamp) return false;
                    return (new Date(h.timestamp).getMonth() + 1).toString().padStart(2, '0') === fMonth;
                });
            }

            // Agrupar por Membro
            let counts = {};
            filtered.forEach(h => {
                counts[h.memberId] = (counts[h.memberId] || 0) + 1;
            });

            // Filtro de unidade para exibição
            let membersToShow = appState.members;
            if(fUnit) membersToShow = membersToShow.filter(m => m.unitId === fUnit);

            let html = `
                <div class="print-header">
                    <h1 class="print-title">Relatório de Produtividade - ${fMonth ? 'Mês '+fMonth : 'Geral'}</h1>
                    <div class="prof-info">Rede PowFit | Unidade: ${fUnit ? appState.units.find(u=>u.id===fUnit)?.name : 'Todas'}</div>
                </div>
                <table>
                    <thead><tr><th>Profissional</th><th>Unidade</th><th>Fichas Criadas</th></tr></thead>
                    <tbody>
            `;
            let total = 0;
            membersToShow.forEach(m => {
                const c = counts[m.id] || 0;
                total += c;
                html += `<tr><td>${m.name}</td><td>${appState.units.find(u=>u.id===m.unitId)?.name||''}</td><td style="text-align:center">${c}</td></tr>`;
            });
            html += `<tr><td colspan="2" style="text-align:right"><strong>TOTAL</strong></td><td style="text-align:center"><strong>${total}</strong></td></tr></tbody></table>`;
            
            document.getElementById('print-area').innerHTML = html;
            setTimeout(() => { window.print(); }, 200);
        };

        // 12. EXERCÍCIOS CUSTOMIZADOS
        window.addCustomExercise = async () => {
            const category = document.getElementById('custom-cat').value;
            const name = document.getElementById('custom-name').value.trim();
            if(!name) return;

            showLoading("SALVANDO...");
            try {
                const docRef = await addDoc(collection(db, `users/${appState.user.uid}/custom_exercises`), { category, name });
                appState.customExercises.push({ id: docRef.id, category, name });
                document.getElementById('custom-name').value = '';
                renderCustomEx();
            } catch(e) { alert("Erro ao salvar."); }
            hideLoading();
        };

        window.deleteCustomExercise = async (id) => {
            if(!confirm("Excluir este exercício do seu banco?")) return;
            showLoading("EXCLUINDO...");
            try {
                await deleteDoc(doc(db, `users/${appState.user.uid}/custom_exercises`, id));
                appState.customExercises = appState.customExercises.filter(c => c.id !== id);
                renderCustomEx();
            } catch(e) { alert("Erro ao excluir."); }
            hideLoading();
        };

        function renderCustomEx() {
            const container = document.getElementById('custom-ex-list');
            if(appState.customExercises.length === 0) {
                container.innerHTML = `<div class="col-span-full opacity-50 text-sm">Nenhum exercício customizado.</div>`;
                return;
            }
            container.innerHTML = appState.customExercises.map(c => `
                <div class="card p-3 rounded-lg flex justify-between items-center text-sm border-l-2 border-l-primary">
                    <div class="truncate pr-2">
                        <span class="text-[9px] opacity-60 block">${c.category}</span>
                        <strong>${c.name}</strong>
                    </div>
                    <button onclick="deleteCustomExercise('${c.id}')" class="text-red-500 hover:text-red-300 transition"><i class="fas fa-trash"></i></button>
                </div>
            `).join('');
        }

    </script>
</body>
</html>
