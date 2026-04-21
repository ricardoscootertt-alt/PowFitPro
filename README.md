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
            /* Tema Padrão Masculino (Escuro Fitness) */
            --bg-body: #0f172a; --bg-card: #1e293b; --bg-panel: #0b1120;
            --text-main: #f8fafc; --text-muted: #94a3b8; --border-color: #334155;
            --primary: #3b82f6; --primary-hover: #2563eb;
            --input-bg: #0f172a; --input-border: #475569;
        }

        [data-theme="Feminino"] {
            /* Tema Feminino (Rosa Moderno Elegante) */
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

        /* Loader Spinner */
        .loader { border: 3px solid rgba(255,255,255,0.1); border-left-color: var(--primary); border-radius: 50%; width: 24px; height: 24px; animation: spin 1s linear infinite; }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }

        /* Tabs UI */
        .tab-btn.active { background-color: rgba(0,0,0,0.2); border-bottom: 2px solid var(--primary); color: var(--primary); font-weight: bold; }

        /* Esconder áreas de impressão na UI normal */
        #print-area, #report-print-area { display: none; }

        /* ========================================== */
        /* ESTILOS DE IMPRESSÃO (A4 Planilha)         */
        /* ========================================== */
        @media print {
            body { background: white !important; color: black !important; margin: 0; padding: 0; font-family: Arial, sans-serif; }
            #app-container, .modal, .no-print, nav { display: none !important; }
            
            #print-area, #report-print-area { display: block !important; padding: 10mm 15mm; font-size: 10px; }
            @page { size: A4; margin: 0; }
            
            /* Ficha de Treino */
            .print-header { border-bottom: 2px solid #000; padding-bottom: 8px; margin-bottom: 12px; text-align: center; }
            .print-title { font-size: 18px; font-weight: bold; margin: 0; text-transform: uppercase; }
            .prof-info { font-size: 11px; margin-top: 5px; font-weight: bold; }
            
            .print-grid { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 4px; margin-bottom: 12px; border: 1px solid #000; padding: 8px; background: #fff; }
            .print-grid div { font-size: 10px; }
            
            .print-guidelines { border: 1px solid #000; padding: 8px; margin-bottom: 15px; font-size: 9px; }
            .print-guidelines h4 { margin: 0 0 4px 0; font-size: 10px; text-transform: uppercase; background: #eee; padding: 3px; -webkit-print-color-adjust: exact; }
            .print-guidelines ul { margin: 0; padding-left: 15px; }
            .print-guidelines li { margin-bottom: 2px; }

            .print-workout { margin-bottom: 15px; page-break-inside: avoid; }
            .print-workout h3 { background: #e0e0e0; border: 1px solid #000; border-bottom: none; padding: 4px 8px; margin: 0; font-size: 11px; text-transform: uppercase; -webkit-print-color-adjust: exact; }
            table { width: 100%; border-collapse: collapse; border: 1px solid #000; margin-bottom: 0; }
            th, td { border: 1px solid #000; padding: 5px; text-align: left; font-size: 9.5px; vertical-align: middle; }
            th { background-color: #f0f0f0; font-weight: bold; text-transform: uppercase; text-align: center; -webkit-print-color-adjust: exact; }
            td.text-center { text-align: center; }
            
            .print-footer-section { margin-top: 15px; border: 1px solid #000; padding: 8px; font-size: 9px; page-break-inside: avoid; }
            .legal-text { font-size: 8px; color: #111; text-align: justify; margin-top: 5px; font-style: italic; line-height: 1.3; }

            /* Relatório de Produtividade */
            .report-header { text-align: center; margin-bottom: 20px; border-bottom: 2px solid #000; padding-bottom: 10px; }
            .report-title { font-size: 18px; font-weight: bold; text-transform: uppercase; }
            .report-unit-section { margin-bottom: 20px; }
            .report-unit-title { background: #333; color: white; padding: 5px; font-size: 12px; font-weight: bold; -webkit-print-color-adjust: exact; }
            .report-table { width: 100%; border-collapse: collapse; margin-top: 5px; }
            .report-table th, .report-table td { border: 1px solid #000; padding: 6px; text-align: left; font-size: 10px; }
        }

        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
    </style>
</head>
<body data-theme="Masculino">

    <!-- LOGIN SCREEN -->
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
            <p class="text-[10px] opacity-50 mt-6">Seus dados são salvos com segurança na nuvem.</p>
        </div>
    </div>

    <!-- MAIN APP CONTAINER -->
    <div id="app-container" class="hidden min-h-screen flex flex-col">
        
        <!-- Top Navigation -->
        <nav class="bg-[var(--bg-panel)] border-b border-[var(--border-color)] sticky top-0 z-40">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                <div class="flex justify-between items-center h-16">
                    <div class="flex items-center gap-3">
                        <i class="fas fa-dumbbell text-2xl text-primary"></i>
                        <div>
                            <span class="font-bold text-lg hidden sm:block leading-none">PowFit Pro</span>
                            <div id="active-member-display" class="text-[10px] text-primary font-medium mt-1">Nenhum membro selecionado</div>
                        </div>
                    </div>
                    <div class="flex items-center gap-1 sm:gap-2 overflow-x-auto whitespace-nowrap">
                        <button onclick="switchTab('dashboard')" id="tab-dashboard" class="tab-btn active px-3 py-2 text-sm font-medium rounded-md transition"><i class="fas fa-network-wired sm:mr-1"></i> <span class="hidden sm:inline">Equipe / Rede</span></button>
                        <button onclick="switchTab('builder')" id="tab-builder" class="tab-btn px-3 py-2 text-sm font-medium rounded-md transition"><i class="fas fa-clipboard-list sm:mr-1"></i> <span class="hidden sm:inline">Montar Ficha</span></button>
                        <button onclick="switchTab('database')" id="tab-database" class="tab-btn px-3 py-2 text-sm font-medium rounded-md transition"><i class="fas fa-database sm:mr-1"></i> <span class="hidden sm:inline">Exercícios</span></button>
                        <button onclick="switchTab('history')" id="tab-history" class="tab-btn px-3 py-2 text-sm font-medium rounded-md transition"><i class="fas fa-chart-bar sm:mr-1"></i> <span class="hidden sm:inline">Relatórios</span></button>
                        <button onclick="logout()" class="px-3 py-2 text-sm font-medium rounded-md text-red-400 hover:text-red-300 ml-2"><i class="fas fa-sign-out-alt"></i></button>
                    </div>
                </div>
            </div>
        </nav>

        <div class="max-w-7xl mx-auto p-4 sm:p-6 lg:p-8 w-full flex-1">
            
            <!-- ============================================== -->
            <!-- TAB 1: DASHBOARD (FRANQUIAS E EQUIPE)          -->
            <!-- ============================================== -->
            <div id="view-dashboard" class="space-y-6">
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                    
                    <!-- Unidades -->
                    <div class="card rounded-xl p-5 shadow-sm flex flex-col h-[500px]">
                        <div class="flex justify-between items-center mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                            <div>
                                <h2 class="font-bold text-lg"><i class="fas fa-store text-primary"></i> 1. Unidades da Rede</h2>
                                <p class="text-[10px] opacity-70">Gerencie suas franquias e pontos de atendimento.</p>
                            </div>
                            <button onclick="promptAddUnit()" class="text-xs btn-primary px-3 py-1.5 rounded-lg font-bold transition"><i class="fas fa-plus"></i> Nova Unidade</button>
                        </div>
                        <div id="list-units" class="flex-1 overflow-y-auto space-y-2 text-sm pr-2"></div>
                    </div>

                    <!-- Equipe / Membros -->
                    <div class="card rounded-xl p-5 shadow-sm flex flex-col h-[500px]">
                        <div class="flex justify-between items-center mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                            <div>
                                <h2 class="font-bold text-lg"><i class="fas fa-users text-primary"></i> 2. Equipe (Membros)</h2>
                                <p class="text-[10px] opacity-70">Selecione uma unidade para gerenciar e atuar como membro.</p>
                            </div>
                            <button onclick="openMemberModal()" id="btn-add-member" class="text-xs btn-primary px-3 py-1.5 rounded-lg font-bold transition" disabled><i class="fas fa-user-plus"></i> Novo Membro</button>
                        </div>
                        <p id="msg-select-unit" class="text-xs opacity-50 italic mb-2">Selecione uma Unidade ao lado primeiro.</p>
                        <div id="list-members" class="flex-1 overflow-y-auto space-y-2 text-sm pr-2"></div>
                    </div>

                </div>
            </div>

            <!-- ============================================== -->
            <!-- TAB 2: MONTAR FICHA                            -->
            <!-- ============================================== -->
            <div id="view-builder" class="hidden space-y-6">
                
                <div id="warning-no-member" class="bg-red-500 bg-opacity-20 border border-red-500 text-red-100 p-4 rounded-xl flex flex-col sm:flex-row items-center gap-3 justify-between">
                    <div class="flex items-center gap-3">
                        <i class="fas fa-exclamation-triangle text-2xl"></i>
                        <div>
                            <p class="font-bold">Nenhum Membro Selecionado!</p>
                            <p class="text-xs sm:text-sm">Vá na aba "Equipe / Rede", selecione uma Unidade e clique em "Atuar como este Membro" para poder gerar fichas.</p>
                        </div>
                    </div>
                    <button onclick="switchTab('dashboard')" class="bg-white text-black px-4 py-2 rounded-lg font-bold text-sm w-full sm:w-auto shadow-lg hover:bg-gray-200 transition whitespace-nowrap">
                        Ir para Equipe
                    </button>
                </div>

                <div id="builder-content" class="grid grid-cols-1 xl:grid-cols-12 gap-6 opacity-50 pointer-events-none transition-opacity">
                    <!-- Coluna Esquerda: Dados -->
                    <div class="xl:col-span-4 space-y-4">
                        <!-- Aluno -->
                        <div class="card rounded-xl p-5 shadow-sm">
                            <div class="flex justify-between items-center mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                                <h2 class="font-bold"><i class="fas fa-user text-primary"></i> Aluno</h2>
                                <div id="imc-display"></div>
                            </div>
                            <div class="space-y-3">
                                <div>
                                    <label class="block text-[10px] font-medium mb-1 uppercase opacity-70">Nome Completo</label>
                                    <input type="text" id="stu-name" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="Nome do aluno">
                                </div>
                                <div class="grid grid-cols-3 gap-2">
                                    <div>
                                        <label class="block text-[10px] font-medium mb-1 uppercase opacity-70">Idade</label>
                                        <input type="number" id="stu-age" class="input-field w-full rounded px-2 py-2 text-sm" placeholder="Anos">
                                    </div>
                                    <div>
                                        <label class="block text-[10px] font-medium mb-1 uppercase opacity-70">Peso (kg)</label>
                                        <input type="number" id="stu-weight" oninput="calcIMC()" class="input-field w-full rounded px-2 py-2 text-sm" placeholder="kg">
                                    </div>
                                    <div>
                                        <label class="block text-[10px] font-medium mb-1 uppercase opacity-70">Altura (m)</label>
                                        <input type="number" id="stu-height" step="0.01" oninput="calcIMC()" class="input-field w-full rounded px-2 py-2 text-sm" placeholder="Ex: 1.75">
                                    </div>
                                </div>
                                <div class="grid grid-cols-2 gap-2">
                                    <div>
                                        <label class="block text-[10px] font-medium mb-1 uppercase opacity-70">Gênero</label>
                                        <select id="stu-gender" onchange="changeTheme()" class="input-field w-full rounded px-2 py-2 text-sm">
                                            <option value="Masculino">Masculino</option>
                                            <option value="Feminino">Feminino</option>
                                        </select>
                                    </div>
                                    <div>
                                        <label class="block text-[10px] font-medium mb-1 uppercase opacity-70">Nível</label>
                                        <select id="stu-level" class="input-field w-full rounded px-2 py-2 text-sm">
                                            <option>Iniciante</option><option>Intermediário</option><option>Avançado</option>
                                        </select>
                                    </div>
                                </div>
                                <div class="grid grid-cols-2 gap-2">
                                    <div>
                                        <label class="block text-[10px] font-medium mb-1 uppercase opacity-70">Frequência</label>
                                        <select id="stu-freq" class="input-field w-full rounded px-2 py-2 text-sm">
                                            <option>3 dias</option><option>5 dias</option><option>6 dias</option><option>Personalizado</option>
                                        </select>
                                    </div>
                                    <div>
                                        <label class="block text-[10px] font-medium mb-1 uppercase opacity-70">Validade</label>
                                        <select id="stu-validity" class="input-field w-full rounded px-2 py-2 text-sm">
                                            <option value="15">15 dias</option><option value="30" selected>30 dias</option>
                                            <option value="60">60 dias</option><option value="90">90 dias</option>
                                        </select>
                                    </div>
                                </div>
                                <div>
                                    <label class="block text-[10px] font-medium mb-1 uppercase opacity-70">Objetivo (Gera Diretriz)</label>
                                    <select id="stu-objective" class="input-field w-full rounded px-2 py-2 text-sm">
                                        <option>Emagrecimento</option><option>Hipertrofia</option><option>Definição</option>
                                        <option>Condicionamento</option><option>Resistência</option><option>Força</option>
                                        <option>Reabilitação</option><option>Saúde geral</option>
                                    </select>
                                </div>
                            </div>
                        </div>

                        <!-- Saúde -->
                        <div class="card rounded-xl p-4 shadow-sm">
                            <h2 class="font-bold mb-1"><i class="fas fa-notes-medical text-primary"></i> Saúde</h2>
                            <p class="text-[9px] opacity-60 mb-2 leading-tight">Múltipla escolha. As diretrizes automáticas da impressão serão ajustadas se você for PEF ou Treinador Esportivo.</p>
                            <div id="health-container" class="grid grid-cols-2 gap-1 text-[11px]"></div>
                        </div>

                        <!-- Config -->
                        <div class="card rounded-xl p-4 shadow-sm">
                            <h2 class="font-bold mb-2"><i class="fas fa-pen text-primary"></i> Recomendações (Opcional)</h2>
                            <textarea id="stu-recs" rows="3" class="input-field w-full rounded px-2 py-2 text-sm" placeholder="Descanso, hidratação, intensidade..."></textarea>
                        </div>
                    </div>

                    <!-- Coluna Direita: Treinos -->
                    <div class="xl:col-span-8 space-y-4">
                        <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center p-4 rounded-xl card border-dashed border-2 bg-black bg-opacity-10 gap-4">
                            <div>
                                <h2 class="font-bold text-lg"><i class="fas fa-layer-group text-primary"></i> Ficha de Treinamento</h2>
                                <p class="text-[10px] opacity-70">Crie os dias e monte a sequência 100% manualmente.</p>
                            </div>
                            <div class="flex flex-wrap gap-2 w-full sm:w-auto">
                                <button onclick="addWorkoutDay()" class="bg-gray-700 hover:bg-gray-600 text-white px-3 py-2 rounded-lg text-sm font-bold transition shadow flex-1 sm:flex-none"><i class="fas fa-plus"></i> Novo Dia</button>
                                <button onclick="saveAndPrint()" id="btn-save-print" class="btn-primary px-4 py-2 rounded-lg text-sm font-bold shadow flex items-center justify-center gap-2 flex-1 sm:flex-none"><i class="fas fa-save"></i> <i class="fas fa-print"></i> Salvar & Imprimir</button>
                            </div>
                        </div>
                        <div id="workouts-container" class="space-y-5"></div>
                    </div>
                </div>
            </div>

            <!-- ============================================== -->
            <!-- TAB 3: BANCO DE DADOS EXERCÍCIOS               -->
            <!-- ============================================== -->
            <div id="view-database" class="hidden space-y-6">
                <div class="card p-5 rounded-xl">
                    <h2 class="font-bold text-xl mb-2"><i class="fas fa-database text-primary"></i> Banco de Exercícios</h2>
                    <p class="text-sm opacity-70 mb-6">Consulte os exercícios nativos e cadastre novos para sua conta na nuvem. Eles aparecerão na hora de montar a ficha.</p>
                    
                    <div class="flex flex-col md:flex-row gap-4 mb-6 items-end border-b border-opacity-20 pb-6" style="border-color: var(--border-color)">
                        <div class="w-full md:w-1/3">
                            <label class="block text-xs font-medium mb-1">Selecione a Categoria</label>
                            <select id="db-category-select" class="input-field rounded-lg px-3 py-2 text-sm w-full" onchange="renderDBExercises()">
                                <!-- Preenchido via JS -->
                            </select>
                        </div>
                        <div class="w-full md:w-2/3">
                            <label class="block text-xs font-medium mb-1">Adicionar Novo Exercício na Nuvem</label>
                            <div class="flex gap-2">
                                <input type="text" id="db-new-exercise" class="input-field flex-1 rounded-lg px-3 py-2 text-sm" placeholder="Ex: Tríceps Testa no Cross">
                                <button onclick="addCustomExercise()" class="btn-primary px-6 py-2 rounded-lg shadow font-bold hover:bg-primary-hover"><i class="fas fa-plus"></i> Salvar</button>
                            </div>
                        </div>
                    </div>
                    
                    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-3" id="db-exercise-list">
                        <!-- Gerado via JS -->
                    </div>
                </div>
            </div>

            <!-- ============================================== -->
            <!-- TAB 4: HISTÓRICO E RELATÓRIOS                  -->
            <!-- ============================================== -->
            <div id="view-history" class="hidden space-y-6">
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                    <!-- Relatório -->
                    <div class="card rounded-xl p-5 md:col-span-1 h-fit">
                        <h2 class="font-bold text-lg mb-2"><i class="fas fa-chart-line text-primary"></i> Produtividade</h2>
                        <p class="text-xs opacity-70 mb-4">Gere um relatório contabilizando fichas por membro da rede no mês.</p>
                        <div class="flex flex-col gap-3">
                            <input type="month" id="report-month" class="input-field rounded px-3 py-2 text-sm w-full">
                            <button onclick="generateReport()" class="btn-primary w-full py-2 rounded font-bold shadow"><i class="fas fa-print"></i> Imprimir Relatório</button>
                        </div>
                    </div>
                    
                    <!-- Nuvem / Fichas Salvas -->
                    <div class="card rounded-xl p-5 md:col-span-2">
                        <div class="flex justify-between items-center mb-4">
                            <div>
                                <h2 class="font-bold text-lg"><i class="fas fa-cloud text-primary"></i> Fichas Salvas (Nuvem)</h2>
                                <p class="text-xs opacity-70">Fichas criadas por toda a equipe da sua rede.</p>
                            </div>
                            <button onclick="loadHistory()" class="bg-gray-700 hover:bg-gray-600 text-white px-3 py-1.5 rounded text-xs font-bold transition"><i class="fas fa-sync-alt"></i> Atualizar</button>
                        </div>
                        <div id="history-list" class="space-y-3 max-h-[60vh] overflow-y-auto pr-2">
                            <!-- Gerado via JS -->
                        </div>
                    </div>
                </div>
            </div>

        </div>
    </div>

    <!-- ============================================== -->
    <!-- MODAIS DE CRIAÇÃO E INSERÇÃO                   -->
    <!-- ============================================== -->

    <!-- MODAL NOVO MEMBRO (DASHBOARD) -->
    <div id="modal-member" class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-90 z-[70] backdrop-blur-sm hidden">
        <div class="card p-6 rounded-2xl shadow-2xl max-w-sm w-full mx-4">
            <h3 class="text-lg font-bold mb-4">Cadastrar Novo Membro</h3>
            <div class="space-y-3">
                <div>
                    <label class="block text-xs font-medium mb-1">Nome</label>
                    <input type="text" id="member-name" class="input-field w-full rounded px-3 py-2 text-sm">
                </div>
                <div>
                    <label class="block text-xs font-medium mb-1">CPF</label>
                    <input type="text" id="member-cpf" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="000.000.000-00">
                </div>
                <div>
                    <label class="block text-xs font-medium mb-1">Estado (UF)</label>
                    <input type="text" id="member-uf" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="Ex: SP">
                </div>
                <div>
                    <label class="block text-xs font-medium mb-1">Categoria</label>
                    <select id="member-category" onchange="toggleMemberCref()" class="input-field w-full rounded px-3 py-2 text-sm">
                        <option value="PEF">Profissional de Educação Física</option>
                        <option value="TE">Treinador Esportivo</option>
                    </select>
                </div>
                <div id="member-cref-container">
                    <label class="block text-xs font-medium mb-1">CREF</label>
                    <input type="text" id="member-cref" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="000000-G/UF">
                </div>
                <div class="flex gap-2 mt-4">
                    <button onclick="closeMemberModal()" class="flex-1 bg-gray-600 hover:bg-gray-500 text-white py-2 rounded text-sm font-bold transition">Cancelar</button>
                    <button onclick="saveMember()" class="flex-1 btn-primary py-2 rounded text-sm font-bold shadow">Salvar</button>
                </div>
            </div>
        </div>
    </div>

    <!-- MODAL DE ADICIONAR EXERCÍCIOS NA FICHA -->
    <div id="exercise-modal" class="fixed inset-0 bg-black bg-opacity-80 z-[60] flex items-center justify-center p-2 hidden backdrop-blur-sm">
        <div class="card w-full max-w-5xl rounded-xl shadow-2xl flex flex-col h-[95vh] sm:h-[85vh]">
            <div class="p-4 border-b flex justify-between items-center bg-black bg-opacity-20" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold"><i class="fas fa-search text-primary mr-2"></i> Adicionar à Ficha</h3>
                <button onclick="closeExerciseModal()" class="text-gray-400 hover:text-red-500 text-2xl leading-none">&times;</button>
            </div>
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <div class="w-full md:w-1/4 border-r overflow-y-auto p-2 space-y-1 bg-black bg-opacity-10" style="border-color: var(--border-color)" id="modal-categories"></div>
                <div class="w-full md:w-3/4 overflow-y-auto p-3" id="modal-exercises"></div>
            </div>
        </div>
    </div>


    <!-- ========================================== -->
    <!-- ÁREA DE IMPRESSÃO (Oculta na tela)         -->
    <!-- ========================================== -->
    <div id="print-area"></div>
    <div id="report-print-area"></div>

    <!-- ========================================== -->
    <!-- SCRIPTS CORE / FIREBASE / LÓGICA           -->
    <!-- ========================================== -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-app.js";
        import { getAuth, signInWithPopup, signInWithCustomToken, GoogleAuthProvider, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, addDoc, getDocs, deleteDoc } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-firestore.js";

        // Firebase Configuração - Conformidade com Canvas 
        const providedConfig = {
            apiKey: "AIzaSyBIPqTYYkG5vHr57CndPCmxUeACncNAobM",
            authDomain: "powfitpro-582df.firebaseapp.com",
            projectId: "powfitpro-582df",
            storageBucket: "powfitpro-582df.firebasestorage.app",
            messagingSenderId: "1080284745812",
            appId: "1:1080284745812:web:56729b17fcdd3d44aaddc1",
            measurementId: "G-GVPV7SSBTB"
        };
        const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : providedConfig;
        const rootAppId = typeof __app_id !== 'undefined' ? __app_id : 'powfit-pro';

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);

        // Funções para caminhos seguros do Firestore
        const getCol = (col) => typeof __app_id !== 'undefined' ? collection(db, 'artifacts', rootAppId, 'public', 'data', col) : collection(db, col);
        const getDocRef = (col, id) => typeof __app_id !== 'undefined' ? doc(db, 'artifacts', rootAppId, 'public', 'data', col, id) : doc(db, col, id);

        // --- BANCO DE DADOS LOCAL (ESTÁTICO + DIRETRIZES) ---
        const HEALTH_PEF = {
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
        const HEALTH_TE = {
            "🟢 Saudável": "Manter treinos regulares de 3–5x por semana, combinando musculação e atividades cardiovasculares. A constância, descanso adequado e boa alimentação contribuem para melhores resultados.",
            "⚪ Sedentário": "O início deve ser gradual, com treinos leves e foco na adaptação do corpo. A prioridade é desenvolver constância, aprender a execução correta e evitar excesso de carga.",
            "🟡 Sobrepeso": "A prática regular de musculação associada ao cardio pode contribuir para melhora do condicionamento físico e composição corporal. A progressão deve respeitar a individualidade e o nível de adaptação.",
            "🔴 Obesidade": "É recomendado iniciar com exercícios de menor impacto e progressão gradual. A segurança, mobilidade e constância são prioridades durante o processo.",
            "⚖️ Baixo peso": "A musculação pode auxiliar no ganho de massa muscular quando associada a alimentação adequada e descanso. O treino deve priorizar evolução progressiva e recuperação muscular.",
            "🍬 Diabetes": "Pessoas com diabetes devem manter acompanhamento médico regular. A musculação pode auxiliar na rotina de atividade física quando liberada por profissional de saúde. Em casos de tontura, mal-estar ou alteração de glicemia, o treino deve ser interrompido e o aluno deve procurar orientação médica.",
            "❤️ Hipertensão": "Pessoas com hipertensão devem manter acompanhamento médico regular e respeitar as orientações profissionais. Durante o treino, é importante evitar prender a respiração e controlar a intensidade dos exercícios.",
            "🔵 Hipotensão": "Pessoas com hipotensão devem evitar mudanças bruscas de posição durante o treino. Manter boa hidratação e respeitar a intensidade adequada ajuda a reduzir episódios de mal-estar.",
            "💔 Problemas cardíacos": "A prática de exercícios deve ocorrer apenas com liberação e acompanhamento médico. O treino deve respeitar limites individuais, com controle de intensidade e atenção aos sinais do corpo.",
            "🦴 Problemas articulares": "Exercícios com menor impacto e maior controle de movimento costumam ser mais indicados. O acompanhamento profissional e a adaptação dos exercícios ajudam na segurança durante o treino.",
            "🫁 Problemas respiratórios": "A progressão do treino deve ser gradual e respeitar a capacidade respiratória individual. Em caso de falta de ar excessiva ou desconforto, o exercício deve ser interrompido.",
            "⚠️ Lesões": "A adaptação dos exercícios deve respeitar a limitação existente e evitar dor durante a execução. Em situações de lesão, é importante seguir orientação profissional adequada antes da prática.",
            "🤰 Gestante": "A prática de exercícios deve ocorrer com liberação médica e acompanhamento adequado. O foco deve ser segurança, mobilidade e bem-estar, evitando impacto excessivo e situações de risco.",
            "🤱 Lactante": "A atividade física pode ser mantida normalmente, respeitando a recuperação individual e mantendo boa hidratação e alimentação adequada.",
            "👴 Idoso": "A musculação pode contribuir para força, equilíbrio, mobilidade e autonomia. O treino deve respeitar limitações individuais, com intensidade moderada e foco na segurança."
        };
        const OBJECTIVES = {
            "Emagrecimento": "Foco em déficit calórico com treinos mistos de força (manter massa magra) e aeróbicos (maior gasto).",
            "Hipertrofia": "Prioridade na progressão de carga e volume adequado. Essencial superávit calórico e descanso.",
            "Definição": "Manutenção de massa muscular enquanto reduz o percentual de gordura. Atenção estrita à dieta.",
            "Condicionamento": "Treinos com menor tempo de intervalo, circuitos e alta integração cardiopulmonar.",
            "Resistência": "Séries mais longas, cadência controlada e aprimoramento da capacidade muscular.",
            "Força": "Cargas altas, baixas repetições e intervalos de descanso maiores.",
            "Reabilitação": "Treino focado em fortalecimento específico, mobilidade e controle motor. Respeitar limites.",
            "Saúde geral": "Equilíbrio entre força, cardio e flexibilidade. O principal objetivo é a constância e bem-estar."
        };
        const DEFAULT_CATEGORIES = {
            "🔥 PEITO": ["Supino Reto", "Supino Inclinado", "Supino com Halteres", "Supino Fechado com Halteres", "Cross Over", "Cross Over Alto", "Cross Over Baixo", "Crucifixo Reto", "Crucifixo Inclinado com Halteres", "Crucifixo na Máquina", "Peck Fly", "Peck Fly Unilateral", "Pullover", "Flexão de Braço", "Flexão com Pés Elevados", "Flexão Explosiva"],
            "🦍 COSTAS": ["Puxada Alta", "Puxada de Frente Supinada", "Pulldown", "Remada Aberta", "Remada Baixa", "Remada Curvada", "Remada Curvada na Polia", "Remada Unilateral", "Remada Cavalinho (T-Bar)", "Remada no Cross", "Serrote", "Facepull (puxada de cima para baixo)", "Encolhimento (Trapézio)"],
            "🦵 PERNAS": ["Agachamento Livre", "Agachamento Taça", "Agachamento com Barra", "Agachamento no Smith", "Squat", "Hack Machine", "Leg 45°", "Leg 90°", "Agachamento Sumô", "Agachamento Sissy (Livre)", "Afundo", "Recuo", "Avanço", "Passada", "Búlgaro", "Step-up", "Levantamento Terra", "Levantamento Terra Romeno", "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", "Cadeira Flexora", "Elevação Pélvica no Banco", "Elevação Pélvica no Chão", "Elevação Pélvica Unilateral no Chão", "Extensão de Quadril (Glúteo Máximo)", "Extensão Cruzada (Glúteo Médio)", "Abdução no Cross (Glúteo Médio + Mínimo)", "Coice", "Cachorrinho", "Caranguejo", "Cadeira Extensora", "Adução", "Abdução", "Cadeira Abdutora Inclinada", "Flexão Nórdica", "Flexão Nórdica Invertida", "Panturrilha em Pé (Máquina)", "Panturrilha Livre", "Panturrilha no Leg Press", "Panturrilha Banco", "Panturrilha Squat", "Panturrilha Unilateral"],
            "💪 BRAÇOS": ["(Bíceps) Rosca Direta", "(Bíceps) Rosca Alternada", "(Bíceps) Rosca 21", "(Bíceps) Rosca Scott Barra W", "(Bíceps) Rosca Scott Unilateral", "(Bíceps) Rosca Scott com Halteres", "(Bíceps) Rosca Martelo", "(Bíceps) Rosca Cross", "(Bíceps) Rosca Concentrada", "(Bíceps) Rosca Inversa", "(Bíceps) Rosca 45°", "(Tríceps) Pulley Unilateral", "(Tríceps) Pulley Barra", "(Tríceps) Pulley Corda", "(Tríceps) Pulley Pegada Inversa", "(Tríceps) Francês na Corda", "(Tríceps) Francês com Halter", "(Tríceps) Francês Unilateral", "(Tríceps) Cruzado Polia Dupla", "(Tríceps) Coice Unilateral", "(Tríceps) Arremesso", "(Tríceps) Testa", "(Tríceps) Mergulho no Banco"],
            "🪨 OMBROS": ["Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", "Elevação Lateral na Polia", "Elevação Lateral Sentado", "Desenvolvimento com Halteres", "Desenvolvimento com Barra", "Arnold Press", "Elevação Borboleta", "Crucifixo Inverso Sentado com Halteres", "Crucifixo Inverso na Polia", "Crucifixo Inverso Unilateral na Polia", "Facepull (puxada reta)", "Remada Alta", "Encolhimento (Trapézio)"],
            "🧠 ABDÔMEN": ["Infra com Elevação de Perna", "Abdominal Supra", "Abdominal Remador", "Abdominal Bicicleta", "Abdominal Twister com Peso", "Prancha", "Prancha Lateral", "Trituração de Cabos em Pé", "Isometria na parede", "Abdominal isometrico"],
            "🫀 CARDIO": ["Bicicleta 10 Minutos", "Bicicleta 15 Minutos", "Bicicleta 20 Minutos", "Esteira 10 Minutos", "Esteira 15 Minutos", "Esteira 20 Minutos", "Pular Corda"]
        };
        const TECHNIQUES = ["Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", "Negativa", "Isometria", "Parciais", "Pirâmide"];
        const DAYS = ["SEGUNDA-FEIRA", "TERÇA-FEIRA", "QUARTA-FEIRA", "QUINTA-FEIRA", "SEXTA-FEIRA", "SÁBADO", "DOMINGO"];

        // --- ESTADO GLOBAL DA APLICAÇÃO ---
        window.APP_STATE = {
            user: null,
            profile: null, // O membro ativo (PEF ou TE) pelo qual o usuário está atuando
            adminUnitId: null, // Unidade selecionada no dashboard
            
            // Dados em Memória da Nuvem
            units: [],
            members: [],
            customExercises: [],
            fichas: [],
            
            // Estado do Construtor de Fichas
            currentFichaId: null,
            workouts: [],
            activeModalWorkoutId: null,
            activeCategory: "🔥 PEITO"
        };

        // --- AUTH E BOOTSTRAP ---
        window.loginGoogle = async () => {
            document.getElementById('btn-google-login').classList.add('hidden');
            document.getElementById('login-loader').classList.remove('hidden');
            try {
                if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                    await signInWithCustomToken(auth, __initial_auth_token);
                } else {
                    const provider = new GoogleAuthProvider();
                    await signInWithPopup(auth, provider);
                }
            } catch (error) {
                console.error("Login failed:", error);
                alert("Erro: " + error.message);
                document.getElementById('btn-google-login').classList.remove('hidden');
                document.getElementById('login-loader').classList.add('hidden');
            }
        };

        window.logout = async () => { await signOut(auth); location.reload(); };

        onAuthStateChanged(auth, async (user) => {
            if (user) {
                window.APP_STATE.user = user;
                document.getElementById('login-screen').classList.add('hidden');
                document.getElementById('app-container').classList.remove('hidden');
                await loadCloudData();
                window.switchTab('dashboard'); // Entra por padrão no Admin/Rede
            } else {
                document.getElementById('login-screen').classList.remove('hidden');
                document.getElementById('app-container').classList.add('hidden');
            }
        });

        if (typeof __initial_auth_token !== 'undefined') window.loginGoogle(); 

        async function loadCloudData() {
            try {
                const uSnap = await getDocs(getCol('units'));
                window.APP_STATE.units = uSnap.docs.map(d => ({id: d.id, ...d.data()}));
                
                const mSnap = await getDocs(getCol('members'));
                window.APP_STATE.members = mSnap.docs.map(d => ({id: d.id, ...d.data()}));
                
                const eSnap = await getDocs(getCol('custom_exercises'));
                window.APP_STATE.customExercises = eSnap.docs.map(d => ({id: d.id, ...d.data()}));

                // Carrega combos do DB modal
                document.getElementById('db-category-select').innerHTML = Object.keys(DEFAULT_CATEGORIES).map(c=>`<option value="${c}">${c}</option>`).join('');
                
                await loadHistory();
            } catch(e) { console.error("Erro carregando banco:", e); }
        }

        // --- NAVEGAÇÃO ENTRE ABAS ---
        window.switchTab = (tab) => {
            ['dashboard', 'builder', 'database', 'history'].forEach(t => {
                document.getElementById(`view-${t}`).classList.add('hidden');
                const tBtn = document.getElementById(`tab-${t}`);
                if(tBtn) { tBtn.classList.remove('active'); }
            });
            document.getElementById(`view-${tab}`).classList.remove('hidden');
            const aBtn = document.getElementById(`tab-${tab}`);
            if(aBtn) { aBtn.classList.add('active'); }

            if(tab === 'dashboard') renderDashboard();
            if(tab === 'database') window.renderDBExercises();
            if(tab === 'history') window.renderHistory();
        };

        // ==========================================
        // 1. DASHBOARD E REDE (UNIDADES E MEMBROS)
        // ==========================================
        function renderDashboard() {
            const list = document.getElementById('list-units');
            if(window.APP_STATE.units.length === 0) {
                list.innerHTML = '<div class="text-xs opacity-50 p-2">Nenhuma unidade. Clique em "Nova Unidade" para expandir a rede.</div>';
            } else {
                list.innerHTML = window.APP_STATE.units.map(u => `
                    <div onclick="selectUnit('${u.id}', '${u.name}')" class="p-3 border border-[var(--border-color)] rounded-lg flex justify-between items-center hover:bg-black hover:bg-opacity-10 cursor-pointer transition ${window.APP_STATE.adminUnitId === u.id ? 'bg-black bg-opacity-20 border-primary' : ''}">
                        <span class="font-bold text-sm">${u.name}</span>
                        <i class="fas fa-chevron-right text-xs opacity-50"></i>
                    </div>
                `).join('');
            }
        }

        window.promptAddUnit = async () => {
            const name = prompt("Nome da Nova Unidade (Franquia):");
            if(name && name.trim()) {
                try {
                    const docRef = await addDoc(getCol('units'), { name: name.trim() });
                    window.APP_STATE.units.push({ id: docRef.id, name: name.trim() });
                    renderDashboard();
                } catch(e) { alert("Erro ao criar unidade."); }
            }
        };

        window.selectUnit = (unitId, unitName) => {
            window.APP_STATE.adminUnitId = unitId;
            document.getElementById('msg-select-unit').innerText = `Membros em: ${unitName}`;
            document.getElementById('btn-add-member').disabled = false;
            
            renderDashboard(); // Re-render to highlight active unit
            renderMembers();
        };

        function renderMembers() {
            const list = document.getElementById('list-members');
            const mems = window.APP_STATE.members.filter(m => m.unitId === window.APP_STATE.adminUnitId);
            
            if(mems.length === 0) {
                list.innerHTML = '<div class="text-xs opacity-50 p-2">Sem membros na unidade.</div>';
                return;
            }

            list.innerHTML = mems.map(m => `
                <div class="p-3 border border-[var(--border-color)] rounded-lg bg-black bg-opacity-10 flex flex-col gap-2">
                    <div class="flex justify-between items-start">
                        <div>
                            <div class="font-bold text-sm">${m.name}</div>
                            <div class="text-[10px] opacity-70">${m.category === 'PEF' ? 'Prof. Educação Física' : 'Treinador Esportivo'} | CPF: ${m.cpf}</div>
                        </div>
                    </div>
                    <button onclick="actAsMember('${m.id}')" class="bg-gray-700 hover:bg-primary text-white text-xs px-3 py-1.5 rounded transition shadow">
                        <i class="fas fa-user-check"></i> Atuar como este Membro
                    </button>
                </div>
            `).join('');
        }

        window.openMemberModal = () => document.getElementById('modal-member').classList.remove('hidden');
        window.closeMemberModal = () => document.getElementById('modal-member').classList.add('hidden');
        window.toggleMemberCref = () => {
            const cat = document.getElementById('member-category').value;
            document.getElementById('member-cref-container').style.display = cat === 'PEF' ? 'block' : 'none';
        };

        window.saveMember = async () => {
            if(!window.APP_STATE.adminUnitId) return alert("Selecione uma unidade primeiro.");
            const name = document.getElementById('member-name').value;
            const cpf = document.getElementById('member-cpf').value;
            const uf = document.getElementById('member-uf').value;
            const category = document.getElementById('member-category').value;
            const cref = document.getElementById('member-cref').value;

            if(!name || !category) return alert("Preencha Nome e Categoria.");
            
            const mData = { name, cpf, uf, category, cref: category === 'PEF' ? cref : '', unitId: window.APP_STATE.adminUnitId };
            
            try {
                const docRef = await addDoc(getCol('members'), mData);
                window.APP_STATE.members.push({id: docRef.id, ...mData});
                closeMemberModal();
                renderMembers();
            } catch(e) { alert("Erro ao criar membro."); console.error(e);}
        };

        window.actAsMember = (memberId) => {
            const member = window.APP_STATE.members.find(m => m.id === memberId);
            if(member) {
                window.APP_STATE.profile = member;
                const unitName = window.APP_STATE.units.find(u => u.id === member.unitId)?.name || '';
                const tag = `<span class="bg-primary text-white px-2 py-0.5 rounded text-[10px] ml-2">Ativo: ${member.name} (${unitName})</span>`;
                document.getElementById('active-member-display').innerHTML = tag;
                
                // Unlock Builder
                document.getElementById('warning-no-member').classList.add('hidden');
                document.getElementById('builder-content').classList.remove('opacity-50', 'pointer-events-none');
                
                initBuilder();
                window.switchTab('builder');
            }
        };


        // ==========================================
        // 2. CONSTRUTOR DE FICHA (BUILDER)
        // ==========================================
        function initBuilder() {
            window.APP_STATE.currentFichaId = null;
            document.getElementById('stu-name').value = '';
            document.getElementById('stu-age').value = '';
            document.getElementById('stu-weight').value = '';
            document.getElementById('stu-height').value = '';
            document.getElementById('stu-recs').value = '';
            window.APP_STATE.workouts = [];
            window.addWorkoutDay();
            updateHealthBoxes();
            calcIMC();
        }

        window.calcIMC = () => {
            const w = parseFloat(document.getElementById('stu-weight').value);
            const h = parseFloat(document.getElementById('stu-height').value);
            const display = document.getElementById('imc-display');
            if (w > 0 && h > 0) {
                const imc = (w / (h * h)).toFixed(1);
                let cls = "Normal";
                if (imc < 18.5) cls = "Abaixo"; else if (imc >= 25 && imc < 30) cls = "Sobrepeso"; else if (imc >= 30) cls = "Obesidade";
                display.innerHTML = `<span class="bg-primary bg-opacity-20 text-primary px-3 py-1 rounded-full text-xs font-bold border border-primary border-opacity-30">IMC: ${imc} (${cls})</span>`;
            } else { display.innerHTML = ''; }
        };

        window.changeTheme = () => { document.body.setAttribute('data-theme', document.getElementById('stu-gender').value); };

        function updateHealthBoxes() {
            if(!window.APP_STATE.profile) return;
            const cat = window.APP_STATE.profile.category;
            const healthDict = cat === 'PEF' ? HEALTH_PEF : HEALTH_TE;
            const container = document.getElementById('health-container');
            const selected = Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value);
            
            container.innerHTML = Object.keys(healthDict).map(opt => `
                <label class="flex items-start space-x-2 cursor-pointer p-1.5 rounded hover:bg-black hover:bg-opacity-5 transition border border-transparent hover:border-gray-500 hover:border-opacity-20">
                    <input type="checkbox" value="${opt}" ${selected.includes(opt)?'checked':''} class="health-cb mt-0.5 rounded border-gray-400 text-primary w-3.5 h-3.5">
                    <span class="font-medium">${opt}</span>
                </label>
            `).join('');
        }

        function generateId() { return Math.random().toString(36).substr(2, 9); }

        window.addWorkoutDay = () => {
            const num = window.APP_STATE.workouts.length;
            const title = num < 7 ? `TREINO ${DAYS[num]}` : `NOVO TREINO ${num + 1}`;
            window.APP_STATE.workouts.push({ id: generateId(), title: title, exercises: [] });
            window.renderBuilderWorkouts();
        };

        window.removeWorkout = (id) => {
            if(confirm("Remover este dia da ficha?")) {
                window.APP_STATE.workouts = window.APP_STATE.workouts.filter(w => w.id !== id);
                window.renderBuilderWorkouts();
            }
        };

        window.duplicateWorkout = (id) => {
            const w = window.APP_STATE.workouts.find(x => x.id === id);
            if(w) {
                const nw = JSON.parse(JSON.stringify(w));
                nw.id = generateId(); nw.title += " (Cópia)";
                window.APP_STATE.workouts.push(nw);
                window.renderBuilderWorkouts();
            }
        };

        window.updateWorkoutTitle = (id, title) => { const w = window.APP_STATE.workouts.find(x => x.id === id); if(w) w.title = title; };

        // Exercicio actions
        window.removeExercise = (wId, idx) => { window.APP_STATE.workouts.find(w=>w.id===wId).exercises.splice(idx,1); window.renderBuilderWorkouts(); };
        window.moveExercise = (wId, idx, dir) => {
            const w = window.APP_STATE.workouts.find(w=>w.id===wId);
            if(dir==='up' && idx>0) { const t=w.exercises[idx]; w.exercises[idx]=w.exercises[idx-1]; w.exercises[idx-1]=t; }
            if(dir==='down' && idx<w.exercises.length-1) { const t=w.exercises[idx]; w.exercises[idx]=w.exercises[idx+1]; w.exercises[idx+1]=t; }
            window.renderBuilderWorkouts();
        };
        window.updateExercise = (wId, idx, field, val) => { window.APP_STATE.workouts.find(w=>w.id===wId).exercises[idx][field] = val; };

        window.renderBuilderWorkouts = () => {
            const c = document.getElementById('workouts-container');
            c.innerHTML = window.APP_STATE.workouts.map(w => {
                const exHtml = w.exercises.length === 0 ? `<div class="text-center p-4 text-[10px] opacity-50 italic">Ficha vazia. Adicione exercícios.</div>` : w.exercises.map((ex, idx) => `
                    <div class="flex flex-col sm:flex-row gap-2 p-2 items-start sm:items-center border-b border-opacity-10 last:border-0" style="border-color: var(--border-color)">
                        <div class="flex gap-1 hidden sm:flex">
                            <button onclick="moveExercise('${w.id}',${idx},'up')" class="text-[10px] p-1"><i class="fas fa-chevron-up"></i></button>
                            <button onclick="moveExercise('${w.id}',${idx},'down')" class="text-[10px] p-1"><i class="fas fa-chevron-down"></i></button>
                        </div>
                        <div class="flex-1 w-full sm:w-auto">
                            <div class="text-[9px] opacity-60 uppercase">${ex.category}</div>
                            <div class="text-xs font-bold">${ex.name}</div>
                        </div>
                        <div class="flex flex-wrap gap-1 w-full sm:w-auto">
                            <input type="text" value="${ex.sets}" onchange="updateExercise('${w.id}',${idx},'sets',this.value)" class="input-field w-12 text-center text-xs px-1 py-1 rounded" placeholder="Séries">
                            <span class="opacity-50 text-xs self-center">x</span>
                            <input type="text" value="${ex.reps}" onchange="updateExercise('${w.id}',${idx},'reps',this.value)" class="input-field w-16 text-center text-xs px-1 py-1 rounded" placeholder="Reps">
                            <select onchange="updateExercise('${w.id}',${idx},'technique',this.value)" class="input-field w-20 text-[10px] px-1 py-1 rounded">
                                ${TECHNIQUES.map(t=>`<option value="${t}" ${ex.technique===t?'selected':''}>${t}</option>`).join('')}
                            </select>
                            <input type="text" value="${ex.obs}" onchange="updateExercise('${w.id}',${idx},'obs',this.value)" class="input-field flex-1 sm:w-32 text-xs px-2 py-1 rounded" placeholder="Observações...">
                            <button onclick="removeExercise('${w.id}',${idx})" class="text-red-500 hover:text-red-700 p-1"><i class="fas fa-trash"></i></button>
                        </div>
                    </div>
                `).join('');

                return `
                <div class="card rounded-xl overflow-hidden shadow-sm border">
                    <div class="p-3 border-b bg-black bg-opacity-10 flex justify-between items-center" style="border-color: var(--border-color)">
                        <input type="text" value="${w.title}" onchange="updateWorkoutTitle('${w.id}',this.value)" class="input-field bg-transparent font-bold text-sm w-1/2 px-2 py-1 rounded uppercase">
                        <div>
                            <button onclick="duplicateWorkout('${w.id}')" class="p-1.5 opacity-70 hover:opacity-100 transition"><i class="fas fa-copy"></i></button>
                            <button onclick="removeWorkout('${w.id}')" class="p-1.5 text-red-500 transition"><i class="fas fa-trash"></i></button>
                        </div>
                    </div>
                    <div>${exHtml}</div>
                    <button onclick="openExerciseModal('${w.id}')" class="w-full text-primary py-2 text-xs font-bold bg-black bg-opacity-5 hover:bg-opacity-20 transition border-t" style="border-color:var(--border-color)">+ ADICIONAR EXERCÍCIO</button>
                </div>`;
            }).join('');
        };

        // Modal Exercícios
        window.openExerciseModal = (wId) => {
            window.APP_STATE.activeModalWorkoutId = wId;
            document.getElementById('exercise-modal').classList.remove('hidden');
            window.renderModalCategories();
            window.renderModalExercises();
        };
        window.closeExerciseModal = () => document.getElementById('exercise-modal').classList.add('hidden');

        window.renderModalCategories = () => {
            document.getElementById('modal-categories').innerHTML = Object.keys(DEFAULT_CATEGORIES).map(cat => `
                <button onclick="setModalCategory('${cat}')" class="w-full text-left px-3 py-2 rounded-lg text-xs font-medium transition ${window.APP_STATE.activeCategory === cat ? 'bg-primary text-white' : 'hover:bg-white hover:bg-opacity-10'}">${cat}</button>
            `).join('');
        };
        window.setModalCategory = (cat) => { window.APP_STATE.activeCategory = cat; window.renderModalCategories(); window.renderModalExercises(); };

        window.renderModalExercises = () => {
            const cat = window.APP_STATE.activeCategory;
            let exercisesList = [...DEFAULT_CATEGORIES[cat]];
            const customs = window.APP_STATE.customExercises.filter(e => e.category === cat).map(e => e.name);
            exercisesList = [...exercisesList, ...customs];

            document.getElementById('modal-exercises').innerHTML = `
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-2">
                    ${exercisesList.map(name => {
                        const isCardio = cat === "🫀 CARDIO";
                        const safeName = name.replace(/'/g, "\\'");
                        return `
                        <button onclick="addExerciseToWorkout('${safeName}', ${isCardio})" class="card p-3 rounded-lg text-left text-xs font-medium border hover:border-primary transition flex justify-between items-center group">
                            <span>${name}</span>
                            <i class="fas fa-plus opacity-0 group-hover:opacity-100 text-primary"></i>
                        </button>`
                    }).join('')}
                </div>`;
        };

        window.addExerciseToWorkout = (name, isCardio) => {
            const w = window.APP_STATE.workouts.find(w => w.id === window.APP_STATE.activeModalWorkoutId);
            if(w) {
                w.exercises.push({ category: window.APP_STATE.activeCategory, name: name, sets: isCardio ? '1' : '3', reps: isCardio ? '-' : '10 a 12', technique: 'Nenhuma', obs: '' });
                window.renderBuilderWorkouts();
                
                // Feedback visual
                const cont = document.getElementById('modal-exercises');
                cont.style.opacity = '0.5'; setTimeout(() => cont.style.opacity = '1', 150);
            }
        };

        // ==========================================
        // 3. BANCO DE DADOS EXERCÍCIOS (CUSTOMIZADOS)
        // ==========================================
        window.renderDBExercises = () => {
            const cat = document.getElementById('db-category-select').value || "🔥 PEITO";
            const list = document.getElementById('db-exercise-list');
            
            let html = '';
            // Nativos
            DEFAULT_CATEGORIES[cat].forEach(name => {
                html += `<div class="card p-3 rounded-lg text-xs font-medium flex justify-between items-center opacity-70">
                    <span>${name}</span> <i class="fas fa-lock text-[9px]" title="Nativo do Sistema"></i>
                </div>`;
            });
            // Customizados da Nuvem
            window.APP_STATE.customExercises.filter(e => e.category === cat).forEach(e => {
                html += `<div class="card p-3 rounded-lg text-xs font-medium border-primary border flex justify-between items-center">
                    <span>${e.name}</span> 
                    <button onclick="deleteCustomExercise('${e.id}')" class="text-red-500 hover:text-red-400 p-1"><i class="fas fa-trash"></i></button>
                </div>`;
            });
            list.innerHTML = html;
        };

        window.addCustomExercise = async () => {
            const cat = document.getElementById('db-category-select').value;
            const name = document.getElementById('db-new-exercise').value;
            if(!name.trim()) return;

            try {
                const docRef = await addDoc(getCol('custom_exercises'), { category: cat, name: name.trim() });
                window.APP_STATE.customExercises.push({ id: docRef.id, category: cat, name: name.trim() });
                document.getElementById('db-new-exercise').value = '';
                window.renderDBExercises();
            } catch(e) { alert("Erro ao salvar exercício."); }
        };

        window.deleteCustomExercise = async (id) => {
            if(confirm("Apagar exercício customizado da nuvem? Ele não aparecerá mais para ninguém.")) {
                try {
                    await deleteDoc(getDocRef('custom_exercises', id));
                    window.APP_STATE.customExercises = window.APP_STATE.customExercises.filter(e => e.id !== id);
                    window.renderDBExercises();
                } catch(e) { alert("Erro ao deletar."); }
            }
        };

        // ==========================================
        // 4. HISTÓRICO, NUVEM E IMPRESSÃO (SALVAR & IMPRIMIR)
        // ==========================================
        async function loadHistory() {
            try {
                const snap = await getDocs(getCol('fichas'));
                window.APP_STATE.fichas = snap.docs.map(d => ({id: d.id, ...d.data()}));
                if(document.getElementById('view-history').classList.contains('hidden') === false) {
                    window.renderHistory();
                }
            } catch(e) { console.error(e); }
        }

        window.renderHistory = () => {
            const list = document.getElementById('history-list');
            if(window.APP_STATE.fichas.length === 0) { list.innerHTML = '<div class="text-xs opacity-50 p-2 text-center">Nenhuma ficha salva na nuvem.</div>'; return; }
            
            // Ordem decrescente
            const sorted = [...window.APP_STATE.fichas].sort((a,b) => b.createdAt - a.createdAt);

            list.innerHTML = sorted.map(f => {
                // Checa validade
                const createdTime = f.createdAt;
                const validMs = parseInt(f.stuValidity) * 24 * 60 * 60 * 1000;
                const isExpired = (createdTime + validMs) < Date.now();
                const expBadge = isExpired ? `<span class="bg-red-500 text-white px-2 py-0.5 rounded text-[9px] font-bold ml-2 uppercase">Vencida</span>` : '';

                const dateStr = new Date(f.createdAt).toLocaleDateString('pt-BR');
                const memName = window.APP_STATE.members.find(m => m.id === f.memberId)?.name || 'Desconhecido';

                return `
                <div class="card p-3 rounded-lg flex flex-col gap-2 border border-[var(--border-color)]">
                    <div class="flex justify-between items-start">
                        <div>
                            <div class="font-bold text-sm">${f.stuName} ${expBadge}</div>
                            <div class="text-[10px] opacity-70">Salvo: ${dateStr} | Validade: ${f.stuValidity} dias | Obj: ${f.stuObjective}</div>
                            <div class="text-[9px] text-primary mt-1">Prescrito por: ${memName}</div>
                        </div>
                    </div>
                    <div class="flex gap-2 mt-1">
                        <button onclick="loadFicha('${f.id}')" class="flex-1 bg-gray-700 hover:bg-gray-600 text-white py-1.5 rounded text-xs transition"><i class="fas fa-edit"></i> Editar / Re-imprimir</button>
                        <button onclick="deleteFicha('${f.id}')" class="bg-red-500 hover:bg-red-600 text-white px-3 py-1.5 rounded text-xs transition"><i class="fas fa-trash"></i></button>
                    </div>
                </div>`;
            }).join('');
        };

        window.loadFicha = (id) => {
            const f = window.APP_STATE.fichas.find(x => x.id === id);
            if(!f) return;
            
            // Need to set profile to the one who created it (or just load the data into the builder)
            // But if current user is not from the same unit, it might be weird. Let's force load and let the user modify it under their own profile.
            if(!window.APP_STATE.profile) {
                alert("Você precisa atuar como um membro (Aba Equipe) antes de editar uma ficha.");
                return;
            }

            window.APP_STATE.currentFichaId = f.id;
            document.getElementById('stu-name').value = f.stuName || '';
            document.getElementById('stu-age').value = f.stuAge || '';
            document.getElementById('stu-weight').value = f.stuWeight || '';
            document.getElementById('stu-height').value = f.stuHeight || '';
            document.getElementById('stu-gender').value = f.stuGender || 'Masculino';
            document.getElementById('stu-level').value = f.stuLevel || 'Iniciante';
            document.getElementById('stu-freq').value = f.stuFreq || '3 dias';
            document.getElementById('stu-objective').value = f.stuObjective || 'Emagrecimento';
            document.getElementById('stu-validity').value = f.stuValidity || '30';
            document.getElementById('stu-recs').value = f.stuRecs || '';
            
            window.changeTheme();
            window.calcIMC();

            document.querySelectorAll('.health-cb').forEach(cb => { cb.checked = (f.health || []).includes(cb.value); });
            window.APP_STATE.workouts = f.workouts || [];
            
            window.switchTab('builder');
            window.renderBuilderWorkouts();
        };

        window.deleteFicha = async (id) => {
            if(confirm("Excluir permanentemente da nuvem?")) {
                try {
                    await deleteDoc(getDocRef('fichas', id));
                    window.APP_STATE.fichas = window.APP_STATE.fichas.filter(x => x.id !== id);
                    if(window.APP_STATE.currentFichaId === id) window.APP_STATE.currentFichaId = null;
                    window.renderHistory();
                } catch(e) { alert("Erro ao excluir."); }
            }
        };

        window.saveAndPrint = async () => {
            if(!window.APP_STATE.profile) return alert("Atue como um membro da equipe antes de salvar/imprimir!");

            const fData = {
                memberId: window.APP_STATE.profile.id,
                unitId: window.APP_STATE.profile.unitId,
                createdAt: Date.now(),
                
                stuName: document.getElementById('stu-name').value || 'Não informado',
                stuAge: document.getElementById('stu-age').value,
                stuWeight: document.getElementById('stu-weight').value,
                stuHeight: document.getElementById('stu-height').value,
                stuGender: document.getElementById('stu-gender').value,
                stuLevel: document.getElementById('stu-level').value,
                stuFreq: document.getElementById('stu-freq').value,
                stuObjective: document.getElementById('stu-objective').value,
                stuValidity: document.getElementById('stu-validity').value,
                stuRecs: document.getElementById('stu-recs').value,
                health: Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value),
                workouts: window.APP_STATE.workouts
            };

            const btn = document.getElementById('btn-save-print');
            const originalText = btn.innerHTML;
            btn.innerHTML = `<div class="loader" style="width:16px; height:16px;"></div> Salvando Nuvem...`;

            try {
                let idToUse = window.APP_STATE.currentFichaId;
                if(idToUse) {
                    await setDoc(getDocRef('fichas', idToUse), fData);
                    const idx = window.APP_STATE.fichas.findIndex(f => f.id === idToUse);
                    if(idx>=0) window.APP_STATE.fichas[idx] = {id: idToUse, ...fData};
                } else {
                    const dRef = await addDoc(getCol('fichas'), fData);
                    window.APP_STATE.currentFichaId = dRef.id;
                    window.APP_STATE.fichas.push({id: dRef.id, ...fData});
                }
                
                btn.innerHTML = originalText;
                executePrint(fData); // Monta e Imprime

            } catch(e) {
                console.error(e);
                alert("Erro ao salvar na nuvem.");
                btn.innerHTML = originalText;
            }
        };

        function executePrint(d) {
            const prof = window.APP_STATE.profile;
            const unit = window.APP_STATE.units.find(u => u.id === prof.unitId)?.name || '';
            const isPEF = prof.category === 'PEF';
            
            const w = parseFloat(d.stuWeight);
            const h = parseFloat(d.stuHeight);
            let imcStr = "-"; if(w>0 && h>0) imcStr = (w / (h * h)).toFixed(1);
            
            const objRecommendation = OBJECTIVES[d.stuObjective] || "";
            const healthSourceDict = isPEF ? HEALTH_PEF : HEALTH_TE;

            const legalTextPEF = `⚠️ OBSERVAÇÃO LEGAL – PROFISSIONAL DE EDUCAÇÃO FÍSICA<br>Conforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º da mesma lei estabelece que compete ao profissional de Educação Física coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos especializados nas áreas de atividades físicas e do desporto.`;
            const legalTextTE = `⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO<br>Conforme a Lei nº 14.597/2023 (Lei Geral do Esporte), Art. 75, a profissão de treinador esportivo é reconhecida e regulada no Brasil. A atuação possui finalidade orientativa e não substitui avaliação médica, diagnóstico clínico ou prescrição medicamentosa. Em casos de doenças, lesões, gestação ou qualquer condição específica, recomenda-se avaliação prévia por profissional habilitado antes da prática. O treinamento proposto respeita os limites individuais, priorizando segurança e evolução progressiva, dentro da atuação técnica esportiva permitida por lei. As recomendações possuem caráter informativo e orientativo.`;

            const printArea = document.getElementById('print-area');
            
            let html = `
                <div class="print-header">
                    <h1 class="print-title">Planilha de Treinamento - ${d.stuObjective}</h1>
                    <div class="prof-info">Prescrição feita por: ${prof.name} (${isPEF ? 'Profissional de Educação Física' : 'Treinador Esportivo'})</div>
                    ${isPEF ? `<div class="unit-info">CREF: ${prof.cref} - Estado: ${prof.uf} | Unidade: ${unit}</div>` : `<div class="unit-info">Estado: ${prof.uf} | Unidade: ${unit}</div>`}
                </div>

                <div class="print-grid">
                    <div><strong>Aluno:</strong> ${d.stuName}</div>
                    <div><strong>Idade:</strong> ${d.stuAge || '-'} anos</div>
                    <div><strong>Gênero:</strong> ${d.stuGender}</div>
                    
                    <div><strong>Peso:</strong> ${d.stuWeight || '-'} kg</div>
                    <div><strong>Altura:</strong> ${d.stuHeight || '-'} m</div>
                    <div><strong>IMC:</strong> ${imcStr}</div>
                    
                    <div><strong>Nível:</strong> ${d.stuLevel}</div>
                    <div><strong>Frequência:</strong> ${d.stuFreq}</div>
                    <div><strong>Validade:</strong> ${d.stuValidity} dias</div>
                </div>
            `;

            if (objRecommendation || d.health.length > 0) {
                html += `<div class="print-guidelines"><h4>Diretrizes Automáticas de Perfil</h4><ul>`;
                if(objRecommendation) html += `<li><strong>Objetivo (${d.stuObjective}):</strong> ${objRecommendation}</li>`;
                d.health.forEach(k => {
                    if(healthSourceDict[k]) html += `<li><strong>Saúde (${k.replace(/[\u{1F300}-\u{1F9FF}\u{2600}-\u{26FF}\u{2700}-\u{27BF}\u{FE0F}]/gu, '').trim()}):</strong> ${healthSourceDict[k]}</li>`;
                });
                html += `</ul></div>`;
            }

            d.workouts.forEach(w => {
                html += `
                    <div class="print-workout">
                        <h3>${w.title}</h3>
                        <table>
                            <thead><tr><th style="width: 35%">Exercício</th><th style="width: 8%">Séries</th><th style="width: 12%">Reps</th><th style="width: 15%">Técnica</th><th style="width: 30%">Observações</th></tr></thead>
                            <tbody>
                `;
                if (w.exercises.length === 0) html += `<tr><td colspan="5" class="text-center" style="font-style:italic">Sem exercícios</td></tr>`;
                else {
                    w.exercises.forEach(ex => {
                        const t = ex.technique !== 'Nenhuma' ? ex.technique : '-';
                        html += `<tr><td><strong>${ex.name}</strong></td><td class="text-center">${ex.sets}</td><td class="text-center">${ex.reps}</td><td>${t}</td><td>${ex.obs || '-'}</td></tr>`;
                    });
                }
                html += `</tbody></table></div>`;
            });

            html += `<div class="print-footer-section">`;
            if(d.stuRecs.trim()) html += `<strong>Recomendações Manuais:</strong><p style="white-space: pre-wrap; margin: 3px 0 10px 0;">${d.stuRecs}</p>`;
            html += `<div class="legal-text">${isPEF ? legalTextPEF : legalTextTE}</div><div style="text-align:center; margin-top:10px; font-size:7px; opacity:0.8;">Ficha gerada na nuvem em ${new Date(d.createdAt).toLocaleString('pt-BR')} via PowFit Pro</div></div>`;

            printArea.innerHTML = html;
            
            setTimeout(() => window.print(), 300);
        }

        // ==========================================
        // 5. RELATÓRIO DE PRODUTIVIDADE (IMPRESSÃO)
        // ==========================================
        window.generateReport = () => {
            const monthVal = document.getElementById('report-month').value;
            if(!monthVal) return alert("Selecione um mês/ano.");
            const [rYear, rMonth] = monthVal.split('-');
            
            let html = `<div class="report-header"><h1 class="report-title">Relatório de Produtividade da Rede - ${rMonth}/${rYear}</h1><p>Contabilização de fichas salvas na nuvem.</p></div>`;
            let hasData = false;

            window.APP_STATE.units.forEach(unit => {
                const mems = window.APP_STATE.members.filter(m => m.unitId === unit.id);
                let uTable = ''; let uTotal = 0;

                mems.forEach(member => {
                    const fichasMembro = window.APP_STATE.fichas.filter(f => {
                        if (f.memberId !== member.id) return false;
                        const fd = new Date(f.createdAt);
                        return fd.getFullYear() == rYear && (fd.getMonth() + 1) == rMonth;
                    });
                    if(fichasMembro.length > 0) {
                        uTotal += fichasMembro.length;
                        uTable += `<tr><td>${member.name} (${member.category})</td><td class="text-center">${fichasMembro.length}</td></tr>`;
                        hasData = true;
                    }
                });

                if(uTotal > 0) {
                    html += `
                        <div class="report-unit-section">
                            <div class="report-unit-title">📍 ${unit.name} - Total: ${uTotal} fichas prescritas</div>
                            <table class="report-table">
                                <thead><tr><th>Membro Atuante</th><th style="width:100px; text-align:center">Qtd. Fichas</th></tr></thead>
                                <tbody>${uTable}</tbody>
                            </table>
                        </div>
                    `;
                }
            });

            if(!hasData) html += '<p style="text-align:center">Nenhuma ficha registrada no período.</p>';

            document.getElementById('report-print-area').innerHTML = html;
            
            // Print hack to switch views temporarily
            const appC = document.getElementById('app-container');
            const repA = document.getElementById('report-print-area');
            appC.style.display = 'none'; repA.style.display = 'block';
            window.print();
            appC.style.display = 'flex'; repA.style.display = 'none';
        };

    </script>
</body>
</html>
