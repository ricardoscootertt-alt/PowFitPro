<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro - Master Edition (Cloud & Franquias)</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            /* Tema Padrão Masculino (Escuro Fitness) */
            --bg-body: #0f172a; --bg-card: #1e293b; --text-main: #f8fafc; --text-muted: #94a3b8;
            --border-color: #334155; --primary: #3b82f6; --primary-hover: #2563eb;
            --input-bg: #0f172a; --input-border: #475569;
        }
        [data-theme="Feminino"] {
            /* Tema Feminino (Rosa Moderno Elegante) */
            --bg-body: #fdf2f8; --bg-card: #ffffff; --text-main: #1e293b; --text-muted: #64748b;
            --border-color: #fbcfe8; --primary: #ec4899; --primary-hover: #db2777;
            --input-bg: #f8fafc; --input-border: #f1f5f9;
        }

        body { font-family: 'Inter', sans-serif; background-color: var(--bg-body); color: var(--text-main); transition: all 0.3s ease; }
        .card { background-color: var(--bg-card); border: 1px solid var(--border-color); }
        .input-field { background-color: var(--input-bg); border: 1px solid var(--input-border); color: var(--text-main); }
        .input-field:focus { outline: none; border-color: var(--primary); box-shadow: 0 0 0 1px var(--primary); }
        .btn-primary { background-color: var(--primary); color: white; transition: background-color 0.2s; }
        .btn-primary:hover { background-color: var(--primary-hover); }
        
        /* Ocultar elementos de impressão por padrão */
        .print-container { display: none; }

        /* Estilos de Impressão */
        @media print {
            body { background: white !important; color: black !important; margin: 0; padding: 0; }
            #login-screen, #app-screen, .modal, .no-print { display: none !important; }
            .print-container.active-print {
                display: block !important; padding: 10mm 15mm; font-family: 'Arial', sans-serif; font-size: 10px;
            }
            @page { size: A4; margin: 0; }
            
            /* Compartilhado */
            .print-header { border-bottom: 2px solid #000; padding-bottom: 8px; margin-bottom: 12px; text-align: center; }
            .print-title { font-size: 18px; font-weight: bold; margin: 0; text-transform: uppercase; }
            .print-subtitle { font-size: 12px; margin-top: 5px; font-weight: bold; }
            
            /* Ficha Aluno */
            .print-grid { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 4px; margin-bottom: 15px; border: 1px solid #000; padding: 8px; background: #fff; }
            .print-grid div { font-size: 11px; }
            .print-guidelines { border: 1px solid #000; padding: 8px; margin-bottom: 15px; font-size: 9.5px; }
            .print-guidelines h4 { margin: 0 0 4px 0; font-size: 11px; text-transform: uppercase; background: #eee; padding: 3px; }
            .print-guidelines ul { margin: 0; padding-left: 15px; }
            .print-workout { margin-bottom: 15px; page-break-inside: avoid; }
            .print-workout h3 { background: #e0e0e0; border: 1px solid #000; border-bottom: none; padding: 4px 8px; margin: 0; font-size: 12px; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            table { width: 100%; border-collapse: collapse; margin-bottom: 0; border: 1px solid #000; }
            th, td { border: 1px solid #000; padding: 4px 6px; text-align: left; font-size: 10px; }
            th { background-color: #f0f0f0; font-weight: bold; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            .print-footer-section { margin-top: 15px; border: 1px solid #000; padding: 8px; font-size: 9px; page-break-inside: avoid;}
            .legal-text { font-size: 8px; color: #333; text-align: justify; margin-top: 5px; font-style: italic;}
            
            /* Relatório Produtividade */
            .report-table { margin-top: 20px; }
            .report-table th { background-color: #333; color: white; -webkit-print-color-adjust: exact; color-adjust: exact; }
            .report-total { font-size: 14px; font-weight: bold; margin-top: 15px; text-align: right; }
        }

        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
        .tab-active { border-bottom: 2px solid var(--primary); color: var(--primary); font-weight: 600; }
    </style>
</head>
<body>

    <!-- TELA DE LOGIN -->
    <div id="login-screen" class="min-h-screen flex items-center justify-center p-4">
        <div class="card p-8 rounded-2xl shadow-2xl max-w-md w-full text-center border border-gray-700">
            <i class="fas fa-dumbbell text-5xl text-primary mb-4"></i>
            <h1 class="text-3xl font-bold mb-2">PowFit Pro</h1>
            <p class="text-sm opacity-70 mb-8">Plataforma Master - Nuvem & Franquias</p>
            <button onclick="loginGoogle()" class="w-full bg-white text-gray-900 hover:bg-gray-100 font-semibold py-3 px-4 rounded-lg shadow transition flex items-center justify-center gap-3">
                <img src="https://www.svgrepo.com/show/475656/google-color.svg" class="w-5 h-5" alt="Google">
                Entrar com Conta Google
            </button>
            <p class="mt-6 text-xs opacity-50">Seus dados e históricos são salvos na nuvem de forma segura.</p>
        </div>
    </div>

    <!-- TELA PRINCIPAL (APP) -->
    <div id="app-screen" class="hidden min-h-screen pb-20">
        
        <!-- Navbar -->
        <nav class="card border-b sticky top-0 z-40 shadow-sm">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                <div class="flex justify-between h-16">
                    <div class="flex items-center gap-3">
                        <i class="fas fa-dumbbell text-2xl text-primary"></i>
                        <span class="font-bold text-lg hidden sm:block">PowFit Pro</span>
                    </div>
                    <div class="flex space-x-2 sm:space-x-8 overflow-x-auto items-center no-scrollbar text-sm sm:text-base">
                        <button onclick="switchTab('tab-workout')" id="btn-tab-workout" class="whitespace-nowrap py-4 px-2 tab-active transition"><i class="fas fa-clipboard-list"></i> Montar Ficha</button>
                        <button onclick="switchTab('tab-dashboard')" id="btn-tab-dashboard" class="whitespace-nowrap py-4 px-2 opacity-70 hover:opacity-100 transition"><i class="fas fa-sitemap"></i> Franquias & Equipe</button>
                        <button onclick="switchTab('tab-exercises')" id="btn-tab-exercises" class="whitespace-nowrap py-4 px-2 opacity-70 hover:opacity-100 transition"><i class="fas fa-database"></i> Exercícios</button>
                        <button onclick="switchTab('tab-history')" id="btn-tab-history" class="whitespace-nowrap py-4 px-2 opacity-70 hover:opacity-100 transition"><i class="fas fa-history"></i> Histórico Nuvem</button>
                    </div>
                    <div class="flex items-center gap-4">
                        <div class="text-xs text-right hidden lg:block">
                            <div class="font-bold" id="user-display-name">Usuário</div>
                            <div class="opacity-60 cursor-pointer hover:text-red-400" onclick="logout()">Sair da Conta</div>
                        </div>
                        <img id="user-display-img" class="h-8 w-8 rounded-full border border-primary cursor-pointer lg:cursor-default" onclick="logout()" title="Clique para sair">
                    </div>
                </div>
            </div>
        </nav>

        <!-- AVISO DE SELEÇÃO DE MEMBRO -->
        <div id="member-warning" class="bg-red-500 bg-opacity-20 border-l-4 border-red-500 text-red-100 p-4 max-w-7xl mx-auto mt-4 mx-4 sm:mx-auto rounded hidden">
            <div class="flex items-center">
                <i class="fas fa-exclamation-triangle mr-3 text-xl text-red-500"></i>
                <p><strong>Atenção:</strong> Você precisa selecionar ou criar seu Perfil de Profissional na aba <a href="#" onclick="switchTab('tab-dashboard')" class="underline font-bold">Franquias & Equipe</a> antes de montar uma ficha.</p>
            </div>
        </div>

        <div class="max-w-7xl mx-auto p-4 sm:p-6 lg:p-8">

            <!-- ============================== -->
            <!-- TAB: MONTAR FICHA              -->
            <!-- ============================== -->
            <div id="tab-workout" class="tab-content">
                <div class="flex flex-col sm:flex-row justify-between items-end sm:items-center mb-6 gap-4">
                    <div>
                        <h2 class="text-xl font-bold">Montagem de Treino</h2>
                        <p class="text-xs opacity-70 mt-1" id="acting-as-display">Atuando como: Nenhum membro selecionado</p>
                    </div>
                    <button onclick="saveAndPrintWorkout()" id="btn-save-print" class="btn-primary px-6 py-2.5 rounded-lg font-medium shadow-lg flex items-center gap-2 opacity-50 cursor-not-allowed" disabled>
                        <i class="fas fa-save"></i> <i class="fas fa-print"></i> Salvar na Nuvem & Imprimir
                    </button>
                </div>

                <div class="grid grid-cols-1 xl:grid-cols-12 gap-6">
                    <!-- Coluna Esquerda: Dados -->
                    <div class="xl:col-span-4 space-y-6">
                        <!-- Card Aluno -->
                        <div class="card rounded-xl p-5 shadow-sm">
                            <div class="flex justify-between items-center mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                                <h2 class="text-lg font-semibold flex items-center gap-2"><i class="fas fa-user text-primary"></i> Dados do Aluno</h2>
                                <div id="imc-display"></div>
                            </div>
                            <div class="space-y-4">
                                <div>
                                    <label class="block text-sm font-medium mb-1">Nome do Aluno</label>
                                    <input type="text" id="stu-name" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Nome">
                                </div>
                                <div class="grid grid-cols-3 gap-3">
                                    <div><label class="block text-sm font-medium mb-1">Idade</label><input type="number" id="stu-age" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Anos"></div>
                                    <div><label class="block text-sm font-medium mb-1">Peso(kg)</label><input type="number" id="stu-weight" oninput="calculateIMC()" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="kg"></div>
                                    <div><label class="block text-sm font-medium mb-1">Alt(m)</label><input type="number" id="stu-height" step="0.01" oninput="calculateIMC()" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="m"></div>
                                </div>
                                <div class="grid grid-cols-2 gap-3">
                                    <div><label class="block text-sm font-medium mb-1">Gênero</label><select id="stu-gender" onchange="changeTheme()" class="input-field w-full rounded-lg px-3 py-2 text-sm"><option value="Masculino">Masculino</option><option value="Feminino">Feminino</option></select></div>
                                    <div><label class="block text-sm font-medium mb-1">Nível</label><select id="stu-level" class="input-field w-full rounded-lg px-3 py-2 text-sm"><option>Iniciante</option><option>Intermediário</option><option>Avançado</option></select></div>
                                </div>
                                <div>
                                    <label class="block text-sm font-medium mb-1">Objetivo (Gera diretriz)</label>
                                    <select id="stu-objective" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                        <option value="Emagrecimento">Emagrecimento</option><option value="Hipertrofia">Hipertrofia</option>
                                        <option value="Definição">Definição</option><option value="Condicionamento">Condicionamento</option>
                                        <option value="Resistência">Resistência</option><option value="Força">Força</option>
                                        <option value="Reabilitação">Reabilitação</option><option value="Saúde geral">Saúde geral</option>
                                    </select>
                                </div>
                                <div>
                                    <label class="block text-sm font-medium mb-1">Frequência</label>
                                    <select id="stu-freq" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                        <option>3 dias</option><option>5 dias</option><option>6 dias</option><option>Personalizado</option>
                                    </select>
                                </div>
                            </div>
                        </div>

                        <!-- Card Saúde -->
                        <div class="card rounded-xl p-5 shadow-sm">
                            <h2 class="text-lg font-semibold mb-2 flex items-center gap-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                                <i class="fas fa-notes-medical text-primary"></i> Estado de Saúde
                            </h2>
                            <p class="text-[11px] opacity-70 mb-3 leading-tight">As diretrizes impressas se adaptam dinamicamente ao seu perfil profissional (PEF ou TE).</p>
                            <div class="grid grid-cols-2 gap-2 text-xs" id="health-container">
                                <!-- JS -->
                            </div>
                        </div>
                    </div>

                    <!-- Coluna Direita: Treinos -->
                    <div class="xl:col-span-8 space-y-6">
                        <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center bg-opacity-10 p-4 rounded-xl card border-dashed border-2 gap-4">
                            <div>
                                <h2 class="text-xl font-bold flex items-center gap-2"><i class="fas fa-clipboard-list text-primary"></i> Ficha de Treino</h2>
                                <select id="stu-validity" class="input-field rounded px-2 py-1 text-xs mt-2">
                                    <option>Validade: 15 dias</option><option selected>Validade: 30 dias</option><option>Validade: 60 dias</option><option>Validade: 90 dias</option>
                                </select>
                            </div>
                            <button onclick="addWorkoutDay()" class="btn-primary px-4 py-2 rounded-lg text-sm font-medium flex items-center gap-2 w-full sm:w-auto justify-center">
                                <i class="fas fa-plus"></i> Adicionar Dia
                            </button>
                        </div>

                        <div id="workouts-container" class="space-y-6"></div>

                        <div class="card rounded-xl p-5 shadow-sm">
                            <h2 class="text-md font-semibold mb-2"><i class="fas fa-pen text-primary"></i> Recomendações Livres (Opcional)</h2>
                            <textarea id="stu-recs" rows="3" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Hidratação, descanso, cuidados específicos..."></textarea>
                        </div>
                    </div>
                </div>
            </div>

            <!-- ============================== -->
            <!-- TAB: DASHBOARD (FRANQUIAS)     -->
            <!-- ============================== -->
            <div id="tab-dashboard" class="tab-content hidden">
                <div class="mb-6">
                    <h2 class="text-2xl font-bold">Gestão de Rede & Equipe</h2>
                    <p class="text-sm opacity-70">Administre Redes, Unidades, Treinadores e acesse os Relatórios.</p>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                    <!-- 1. Rede -->
                    <div class="card p-5 rounded-xl">
                        <h3 class="font-bold border-b pb-2 mb-4 border-gray-600"><span class="bg-gray-700 text-white w-6 h-6 inline-flex items-center justify-center rounded-full text-xs mr-2">1</span> Redes Cadastradas</h3>
                        <div id="networks-list" class="space-y-2 mb-4 max-h-40 overflow-y-auto"></div>
                        <div class="flex gap-2">
                            <input type="text" id="new-network-name" class="input-field w-full rounded px-2 py-1 text-sm" placeholder="Nova Rede...">
                            <button onclick="createNetwork()" class="btn-primary px-3 rounded"><i class="fas fa-plus"></i></button>
                        </div>
                    </div>

                    <!-- 2. Unidades -->
                    <div class="card p-5 rounded-xl">
                        <h3 class="font-bold border-b pb-2 mb-4 border-gray-600"><span class="bg-gray-700 text-white w-6 h-6 inline-flex items-center justify-center rounded-full text-xs mr-2">2</span> Unidades (Selecione Rede)</h3>
                        <p id="unit-warning" class="text-xs opacity-50 mb-2 italic">Selecione uma Rede primeiro.</p>
                        <div id="units-list" class="space-y-2 mb-4 max-h-40 overflow-y-auto"></div>
                        <div class="flex gap-2 hidden" id="unit-add-form">
                            <input type="text" id="new-unit-name" class="input-field w-full rounded px-2 py-1 text-sm" placeholder="Nova Unidade...">
                            <button onclick="createUnit()" class="btn-primary px-3 rounded"><i class="fas fa-plus"></i></button>
                        </div>
                    </div>

                    <!-- 3. Membros -->
                    <div class="card p-5 rounded-xl">
                        <h3 class="font-bold border-b pb-2 mb-4 border-gray-600"><span class="bg-gray-700 text-white w-6 h-6 inline-flex items-center justify-center rounded-full text-xs mr-2">3</span> Equipe (Selecione Unidade)</h3>
                        <p id="member-warning-text" class="text-xs opacity-50 mb-2 italic">Selecione uma Unidade primeiro.</p>
                        <div id="members-list" class="space-y-2 mb-4 max-h-40 overflow-y-auto"></div>
                        <button onclick="openMemberModal()" id="btn-add-member" class="w-full btn-primary py-2 rounded text-sm hidden"><i class="fas fa-user-plus"></i> Adicionar Profissional</button>
                    </div>
                </div>

                <!-- Relatórios -->
                <div class="mt-8 card p-6 rounded-xl border-dashed border-2">
                    <div class="flex justify-between items-center mb-4">
                        <div>
                            <h3 class="text-lg font-bold"><i class="fas fa-chart-bar text-primary"></i> Relatório de Produtividade Mensal</h3>
                            <p class="text-xs opacity-70">Contagem de fichas geradas no mês atual por equipe.</p>
                        </div>
                        <div class="flex gap-2">
                            <select id="report-unit-select" class="input-field rounded px-3 py-2 text-sm"><option value="">Selecione uma Unidade...</option></select>
                            <button onclick="generateReport()" class="btn-primary px-4 py-2 rounded shadow font-medium"><i class="fas fa-search"></i> Gerar</button>
                        </div>
                    </div>
                    <div id="report-container" class="hidden">
                        <!-- HTML Report -->
                        <div id="report-content" class="bg-black bg-opacity-20 p-4 rounded mb-4"></div>
                        <button onclick="printReport()" class="bg-green-600 hover:bg-green-500 text-white px-4 py-2 rounded font-medium"><i class="fas fa-print"></i> Imprimir Relatório</button>
                    </div>
                </div>
            </div>

            <!-- ============================== -->
            <!-- TAB: EXERCÍCIOS CUSTOMIZADOS   -->
            <!-- ============================== -->
            <div id="tab-exercises" class="tab-content hidden">
                <div class="mb-6 flex justify-between items-end">
                    <div>
                        <h2 class="text-2xl font-bold">Banco de Exercícios</h2>
                        <p class="text-sm opacity-70">Adicione exercícios personalizados que ficarão salvos na sua conta.</p>
                    </div>
                </div>
                
                <div class="card p-5 rounded-xl mb-6">
                    <h3 class="font-bold mb-4">Novo Exercício Customizado</h3>
                    <div class="flex flex-col sm:flex-row gap-4">
                        <select id="custom-ex-cat" class="input-field rounded-lg px-3 py-2 sm:w-1/3">
                            <!-- Populated JS -->
                        </select>
                        <input type="text" id="custom-ex-name" class="input-field rounded-lg px-3 py-2 flex-1" placeholder="Nome do exercício...">
                        <button onclick="addCustomExercise()" class="btn-primary px-6 py-2 rounded-lg font-medium"><i class="fas fa-save"></i> Adicionar</button>
                    </div>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4" id="custom-ex-list">
                    <!-- JS List -->
                </div>
            </div>

            <!-- ============================== -->
            <!-- TAB: HISTÓRICO NUVEM           -->
            <!-- ============================== -->
            <div id="tab-history" class="tab-content hidden">
                <div class="mb-6">
                    <h2 class="text-2xl font-bold">Histórico na Nuvem</h2>
                    <p class="text-sm opacity-70">Todas as fichas geradas pelo seu perfil selecionado.</p>
                </div>
                <div class="space-y-4" id="cloud-history-list">
                    <!-- JS List -->
                    <div class="text-center opacity-50 py-10">Selecione um perfil de membro para ver o histórico.</div>
                </div>
            </div>

        </div>
    </div>

    <!-- MODAL DE MEMBRO (ADD/EDIT) -->
    <div id="member-modal" class="modal fixed inset-0 bg-black bg-opacity-70 backdrop-blur-sm hidden z-50 flex items-center justify-center p-4">
        <div class="card w-full max-w-lg rounded-xl shadow-2xl">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold">Cadastro de Profissional</h3>
                <button onclick="closeMemberModal()" class="text-gray-400 hover:text-red-500 text-2xl">&times;</button>
            </div>
            <div class="p-4 space-y-4">
                <div><label class="block text-sm mb-1">Nome Completo</label><input type="text" id="m-name" class="input-field w-full rounded p-2"></div>
                <div class="grid grid-cols-2 gap-4">
                    <div><label class="block text-sm mb-1">CPF</label><input type="text" id="m-cpf" class="input-field w-full rounded p-2"></div>
                    <div><label class="block text-sm mb-1">Estado (UF)</label><input type="text" id="m-state" class="input-field w-full rounded p-2" maxlength="2" placeholder="SP"></div>
                </div>
                <div>
                    <label class="block text-sm mb-1">Categoria de Atuação</label>
                    <select id="m-role" class="input-field w-full rounded p-2" onchange="toggleModalCref()">
                        <option value="PEF">Profissional de Educação Física (PEF)</option>
                        <option value="TE">Treinador Esportivo (TE)</option>
                    </select>
                </div>
                <div id="m-cref-container">
                    <label class="block text-sm mb-1">CREF</label><input type="text" id="m-cref" class="input-field w-full rounded p-2" placeholder="000000-G/UF">
                </div>
            </div>
            <div class="p-4 border-t bg-black bg-opacity-10 text-right" style="border-color: var(--border-color)">
                <button onclick="saveMember()" class="btn-primary px-6 py-2 rounded shadow font-medium">Salvar Profissional</button>
            </div>
        </div>
    </div>

    <!-- MODAL EXERCICIOS -->
    <div id="exercise-modal" class="modal fixed inset-0 bg-black bg-opacity-70 backdrop-blur-sm hidden z-50 flex items-center justify-center p-2 sm:p-4">
        <div class="card w-full max-w-5xl rounded-xl shadow-2xl flex flex-col h-[90vh]">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold"><i class="fas fa-search text-primary mr-2"></i> Buscar Exercício</h3>
                <button onclick="closeExModal()" class="text-gray-400 hover:text-red-500 text-2xl">&times;</button>
            </div>
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <div class="w-full md:w-1/4 border-r overflow-y-auto p-2 space-y-1" style="border-color: var(--border-color)" id="modal-categories"></div>
                <div class="w-full md:w-3/4 overflow-y-auto p-3 bg-black bg-opacity-5" id="modal-exercises"></div>
            </div>
        </div>
    </div>

    <!-- ============================== -->
    <!-- ÁREAS DE IMPRESSÃO             -->
    <!-- ============================== -->
    
    <!-- Impressão da Ficha de Treino -->
    <div id="print-workout-area" class="print-container"></div>
    
    <!-- Impressão do Relatório de Produtividade -->
    <div id="print-report-area" class="print-container"></div>


    <!-- ============================== -->
    <!-- LÓGICA DO SISTEMA (JAVASCRIPT) -->
    <!-- ============================== -->
    <script type="module">
        // ----------------------------------------------------
        // 1. FIREBASE CONFIG & IMPORTS
        // ----------------------------------------------------
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-app.js";
        import { getAuth, signInWithPopup, GoogleAuthProvider, signOut, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-auth.js";
        import { getFirestore, collection, addDoc, getDocs, doc, deleteDoc, serverTimestamp, query, where, orderBy } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-firestore.js";

        // Configuração original fornecida pelo usuário
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
        const provider = new GoogleAuthProvider();

        // ----------------------------------------------------
        // 2. DADOS E CONSTANTES GLOBAIS
        // ----------------------------------------------------
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
            "🍬 Diabetes": "Pessoas com diabetes devem manter acompanhamento médico regular. A musculação pode auxiliar na rotina física quando liberada por profissional de saúde. Em casos de tontura, mal-estar ou alteração de glicemia, o treino deve ser interrompido e procurar orientação.",
            "❤️ Hipertensão": "Manter acompanhamento médico regular. Durante o treino, é importante evitar prender a respiração (Manobra de Valsalva) e controlar a intensidade dos exercícios.",
            "🔵 Hipotensão": "Evitar mudanças bruscas de posição durante o treino. Manter boa hidratação e respeitar a intensidade adequada ajuda a reduzir episódios de mal-estar.",
            "💔 Problemas cardíacos": "A prática de exercícios deve ocorrer apenas com liberação e acompanhamento médico. O treino deve respeitar limites individuais, com controle de intensidade.",
            "🦴 Problemas articulares": "Exercícios com menor impacto e maior controle de movimento costumam ser mais indicados. O acompanhamento profissional e a adaptação ajudam na segurança.",
            "🫁 Problemas respiratórios": "A progressão do treino deve ser gradual e respeitar a capacidade respiratória individual. Em caso de falta de ar excessiva, o exercício deve ser interrompido.",
            "⚠️ Lesões": "A adaptação dos exercícios deve respeitar a limitação existente e evitar dor. É importante seguir orientação profissional adequada antes da prática.",
            "🤰 Gestante": "Prática com liberação médica e acompanhamento adequado. Foco em segurança, mobilidade e bem-estar, evitando impacto e risco.",
            "🤱 Lactante": "Atividade mantida normalmente, respeitando a recuperação, hidratação e alimentação adequada.",
            "👴 Idoso": "A musculação contribui para força, equilíbrio e mobilidade. Respeitar limitações individuais, com intensidade moderada e foco na segurança."
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

        const baseCategories = {
            "🔥 PEITO": ["Supino Reto", "Supino Inclinado", "Supino Inclinado com Halteres", "Supino Fechado com Halteres", "Cross Over", "Cross Over Alto", "Cross Over Baixo", "Crucifixo Reto", "Crucifixo Inclinado com Halteres", "Crucifixo na Máquina", "Peck Fly", "Peck Fly Unilateral", "Pullover", "Flexão de Braço", "Flexão com Pés Elevados", "Flexão Explosiva"],
            "🦍 COSTAS": ["Puxada Alta", "Puxada de Frente Supinada", "Pulldown", "Remada Aberta", "Remada Baixa", "Remada Curvada", "Remada Curvada Supinada", "Remada Cavalinho (T-Bar)", "Serrote", "Facepull (puxada de cima para baixo)"],
            "🦵 PERNAS": ["Agachamento Livre", "Agachamento Taça", "Agachamento no Smith", "Agachamento com passada lateral", "Squat", "Hack Machine", "Leg 45°", "Leg 90°", "Agachamento Sumô", "Agachamento Sissy (Livre)", "Afundo", "Recuo", "Avanço", "Passada", "Búlgaro", "Step-up", "Levantamento Terra", "Levantamento Terra Romeno", "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", "Cadeira Flexora", "Elevação Pélvica no Banco", "Elevação Pélvica no Chão", "Elevação Pélvica Unilateral no Chão", "Extensão de Quadril (Glúteo Máximo)", "Extensão Cruzada (Glúteo Médio)", "Coice", "Cachorrinho", "Cadeira Extensora", "Adução", "Abdução", "Abdução Inclinada", "Flexão Nórdica", "Flexão Nórdica Invertida", "Panturrilha Livre", "Panturrilha no Leg Press", "Panturrilha Banco", "Panturrilha Squat", "Panturrilha Unilateral"],
            "💪 BRAÇOS (Bíceps)": ["Rosca Direta", "Rosca Alternada", "Rosca 21", "Rosca Scott Barra W", "Rosca Scott Unilateral", "Rosca Scott com Halteres", "Rosca Martelo", "Rosca Cross", "Rosca Inversa", "Rosca 45°"],
            "💪 BRAÇOS (Tríceps)": ["Triceps Pulley Unilateral", "Tríceps Pulley Barra", "Tríceps Pulley Corda", "Tríceps Pulley Pegada Inversa", "Tríceps Francês na Corda", "Tríceps Francês com Halter", "Tríceps Francês Unilateral", "Tríceps Cruzado Polia Dupla", "Tríceps Coice Unilateral", "Tríceps Arremesso", "Tríceps Testa", "Mergulho no Banco"],
            "🪨 OMBROS": ["Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", "Elevação Lateral Unilateral Cross", "Elevação Lateral Sentado", "Desenvolvimento com Halteres", "Desenvolvimento com Barra", "Arnold Press", "Elevação Borboleta", "Crucifixo Inverso Sentado com Halteres", "Crucifixo Inverso na Polia", "Crucifixo Inverso Unilateral na Polia", "Facepull (puxada reta)", "Remada Alta", "Encolhimento (Trapézio)"],
            "🧠 ABDÔMEN": ["Infra com Elevação de Perna", "Abdominal Supra", "Abdominal Remador", "Abdominal Bicicleta", "Abdominal Twister com Peso", "Prancha", "Prancha Lateral", "Trituração de Cabos em Pé", "Isometria na parede"],
            "🫀 CARDIO": ["Bicicleta 10 Minutos", "Bicicleta 15 Minutos", "Bicicleta 20 Minutos", "Esteira 10 Minutos", "Esteira 15 Minutos", "Esteira 20 Minutos", "Pular Corda"]
        };

        const dbTechniques = ["Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", "Negativa", "Isometria", "Parciais", "Pirâmide"];
        const daysOfWeek = ["SEGUNDA-FEIRA", "TERÇA-FEIRA", "QUARTA-FEIRA", "QUINTA-FEIRA", "SEXTA-FEIRA", "SÁBADO", "DOMINGO"];

        // Estado do App
        let state = {
            currentUser: null,
            // DB Data
            networks: [], units: [], members: [], customExercises: [], cloudWorkouts: [],
            // Seleções Dashboard
            selectedNetwork: null, selectedUnit: null, selectedMember: null,
            // Construção de Ficha
            draftWorkouts: [], activeModalWorkoutId: null, activeCategory: "🔥 PEITO"
        };

        // ----------------------------------------------------
        // 3. AUTENTICAÇÃO E INICIALIZAÇÃO
        // ----------------------------------------------------
        onAuthStateChanged(auth, async (user) => {
            if (user) {
                state.currentUser = user;
                document.getElementById('user-display-name').textContent = user.displayName;
                document.getElementById('user-display-img').src = user.photoURL;
                document.getElementById('login-screen').classList.add('hidden');
                document.getElementById('app-screen').classList.remove('hidden');
                
                // Popula select de categorias
                const catSelect = document.getElementById('custom-ex-cat');
                catSelect.innerHTML = Object.keys(baseCategories).map(c => `<option value="${c}">${c}</option>`).join('');
                
                await fetchAllData();
                checkMemberContext();
                addWorkoutDay(); // Inicia com Segunda
            } else {
                state.currentUser = null;
                document.getElementById('login-screen').classList.remove('hidden');
                document.getElementById('app-screen').classList.add('hidden');
            }
        });

        window.loginGoogle = () => { signInWithPopup(auth, provider).catch(e => alert("Erro ao fazer login: " + e.message)); };
        window.logout = () => { signOut(auth); };

        window.switchTab = (tabId) => {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.add('hidden'));
            document.getElementById(tabId).classList.remove('hidden');
            
            document.querySelectorAll('nav button').forEach(b => {
                b.classList.remove('tab-active', 'opacity-70');
                b.classList.add('opacity-70');
            });
            document.getElementById(`btn-${tabId}`).classList.remove('opacity-70');
            document.getElementById(`btn-${tabId}`).classList.add('tab-active');
            
            if(tabId === 'tab-history') renderCloudHistory();
            if(tabId === 'tab-dashboard') populateReportUnits();
        };

        // ----------------------------------------------------
        // 4. FIREBASE FETCHES
        // ----------------------------------------------------
        async function fetchAllData() {
            try {
                // Fetch Networks
                const netSnap = await getDocs(collection(db, "powfit_networks"));
                state.networks = netSnap.docs.map(d => ({ id: d.id, ...d.data() }));
                // Fetch Units
                const unitSnap = await getDocs(collection(db, "powfit_units"));
                state.units = unitSnap.docs.map(d => ({ id: d.id, ...d.data() }));
                // Fetch Members
                const memSnap = await getDocs(collection(db, "powfit_members"));
                state.members = memSnap.docs.map(d => ({ id: d.id, ...d.data() }));
                // Fetch Custom Exercises (Global for simplicity, or we could filter by uid)
                const exSnap = await getDocs(query(collection(db, "powfit_custom_exercises"), where("uid", "==", state.currentUser.uid)));
                state.customExercises = exSnap.docs.map(d => ({ id: d.id, ...d.data() }));
                
                renderDashboard();
                renderCustomExercises();
            } catch (error) {
                console.error("Error fetching data: ", error);
            }
        }

        // ----------------------------------------------------
        // 5. DASHBOARD: GESTÃO DE REDE/EQUIPE
        // ----------------------------------------------------
        window.createNetwork = async () => {
            const name = document.getElementById('new-network-name').value.trim();
            if(!name) return;
            try {
                const docRef = await addDoc(collection(db, "powfit_networks"), { name: name, createdAt: serverTimestamp() });
                state.networks.push({ id: docRef.id, name });
                document.getElementById('new-network-name').value = '';
                renderDashboard();
            } catch(e) { alert("Erro: " + e); }
        };

        window.createUnit = async () => {
            if(!state.selectedNetwork) return;
            const name = document.getElementById('new-unit-name').value.trim();
            if(!name) return;
            try {
                const docRef = await addDoc(collection(db, "powfit_units"), { networkId: state.selectedNetwork, name: name, createdAt: serverTimestamp() });
                state.units.push({ id: docRef.id, networkId: state.selectedNetwork, name });
                document.getElementById('new-unit-name').value = '';
                renderDashboard();
            } catch(e) { alert("Erro: " + e); }
        };

        function renderDashboard() {
            // Networks
            const netList = document.getElementById('networks-list');
            netList.innerHTML = state.networks.map(n => `
                <div class="p-2 rounded cursor-pointer border ${state.selectedNetwork === n.id ? 'border-primary bg-primary bg-opacity-20' : 'border-transparent hover:bg-white hover:bg-opacity-5'}" onclick="selectNetwork('${n.id}')">
                    ${n.name}
                </div>
            `).join('');

            // Units
            const unitList = document.getElementById('units-list');
            if(state.selectedNetwork) {
                document.getElementById('unit-warning').classList.add('hidden');
                document.getElementById('unit-add-form').classList.remove('hidden');
                const netUnits = state.units.filter(u => u.networkId === state.selectedNetwork);
                unitList.innerHTML = netUnits.map(u => `
                    <div class="p-2 rounded cursor-pointer border ${state.selectedUnit === u.id ? 'border-primary bg-primary bg-opacity-20' : 'border-transparent hover:bg-white hover:bg-opacity-5'}" onclick="selectUnit('${u.id}')">
                        ${u.name}
                    </div>
                `).join('');
            } else {
                unitList.innerHTML = '';
            }

            // Members
            const memList = document.getElementById('members-list');
            if(state.selectedUnit) {
                document.getElementById('member-warning-text').classList.add('hidden');
                document.getElementById('btn-add-member').classList.remove('hidden');
                const unitMems = state.members.filter(m => m.unitId === state.selectedUnit);
                memList.innerHTML = unitMems.map(m => `
                    <div class="p-2 rounded cursor-pointer border ${state.selectedMember?.id === m.id ? 'border-green-500 bg-green-500 bg-opacity-20' : 'border-transparent hover:bg-white hover:bg-opacity-5'}" onclick="selectMember('${m.id}')">
                        <div class="font-bold">${m.name} <span class="text-[10px] bg-gray-700 px-1 rounded ml-1">${m.role}</span></div>
                        <div class="text-[10px] opacity-70">CPF: ${m.cpf}</div>
                        ${state.selectedMember?.id === m.id ? '<div class="text-xs text-green-400 mt-1"><i class="fas fa-check"></i> Selecionado para atuar</div>' : ''}
                    </div>
                `).join('');
            } else {
                memList.innerHTML = '';
            }
            populateReportUnits();
        }

        window.selectNetwork = (id) => { state.selectedNetwork = id; state.selectedUnit = null; renderDashboard(); };
        window.selectUnit = (id) => { state.selectedUnit = id; renderDashboard(); };
        window.selectMember = (id) => { 
            state.selectedMember = state.members.find(m => m.id === id); 
            renderDashboard(); 
            checkMemberContext(); 
        };

        // --- CADASTRO DE MEMBRO ---
        window.openMemberModal = () => { document.getElementById('member-modal').classList.remove('hidden'); };
        window.closeMemberModal = () => { document.getElementById('member-modal').classList.add('hidden'); };
        window.toggleModalCref = () => {
            const role = document.getElementById('m-role').value;
            document.getElementById('m-cref-container').style.display = role === 'PEF' ? 'block' : 'none';
        };
        window.saveMember = async () => {
            const data = {
                unitId: state.selectedUnit,
                name: document.getElementById('m-name').value,
                cpf: document.getElementById('m-cpf').value,
                state: document.getElementById('m-state').value.toUpperCase(),
                role: document.getElementById('m-role').value,
                cref: document.getElementById('m-role').value === 'PEF' ? document.getElementById('m-cref').value : ''
            };
            if(!data.name || !data.cpf) return alert("Preencha os dados obrigatórios.");
            
            try {
                const docRef = await addDoc(collection(db, "powfit_members"), data);
                state.members.push({ id: docRef.id, ...data });
                closeMemberModal();
                renderDashboard();
            } catch(e) { alert(e); }
        };

        // ----------------------------------------------------
        // 6. CONTEXTO DE ATUAÇÃO E FICHA
        // ----------------------------------------------------
        function checkMemberContext() {
            const warn = document.getElementById('member-warning');
            const btn = document.getElementById('btn-save-print');
            const display = document.getElementById('acting-as-display');
            
            if(state.selectedMember) {
                warn.classList.add('hidden');
                btn.disabled = false;
                btn.classList.remove('opacity-50', 'cursor-not-allowed');
                const u = state.units.find(un => un.id === state.selectedMember.unitId);
                display.innerHTML = `Atuando como: <strong>${state.selectedMember.name}</strong> (${state.selectedMember.role}) - Unidade: ${u?.name}`;
                
                // Renderizar opções de saúde baseadas na categoria
                renderHealthOptions(state.selectedMember.role === 'PEF' ? healthDataPEF : healthDataTE);
            } else {
                warn.classList.remove('hidden');
                btn.disabled = true;
                btn.classList.add('opacity-50', 'cursor-not-allowed');
                display.innerHTML = `Atuando como: <span class="text-red-500">Nenhum membro selecionado</span>`;
                renderHealthOptions(healthDataPEF); // Default
            }
        }

        window.changeTheme = () => { document.body.setAttribute('data-theme', document.getElementById('stu-gender').value); };
        window.calculateIMC = () => {
            const w = parseFloat(document.getElementById('stu-weight').value);
            const h = parseFloat(document.getElementById('stu-height').value);
            const disp = document.getElementById('imc-display');
            if(w>0 && h>0) {
                const imc = (w/(h*h)).toFixed(1);
                disp.innerHTML = `<span class="bg-primary bg-opacity-20 text-primary px-3 py-1 rounded-full text-xs font-bold border border-primary border-opacity-30">IMC: ${imc}</span>`;
            } else { disp.innerHTML = ''; }
        };

        function renderHealthOptions(dataObj) {
            const c = document.getElementById('health-container');
            const selected = Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value);
            c.innerHTML = Object.keys(dataObj).map(opt => `
                <label class="flex items-start space-x-2 cursor-pointer p-1.5 rounded border border-transparent hover:border-gray-500 hover:border-opacity-20 transition">
                    <input type="checkbox" value="${opt}" ${selected.includes(opt)?'checked':''} class="health-cb mt-0.5 rounded focus:ring-primary w-3.5 h-3.5">
                    <span class="font-medium">${opt}</span>
                </label>
            `).join('');
        }

        window.addWorkoutDay = () => {
            const count = state.draftWorkouts.length;
            const title = count < 7 ? `TREINO ${daysOfWeek[count]}` : `NOVO TREINO ${count+1}`;
            state.draftWorkouts.push({ id: Math.random().toString(36).substr(2, 9), title, exercises: [] });
            renderDraftWorkouts();
        };
        window.removeWorkout = (id) => { state.draftWorkouts = state.draftWorkouts.filter(w => w.id !== id); renderDraftWorkouts(); };
        window.duplicateWorkout = (id) => {
            const w = state.draftWorkouts.find(w => w.id === id);
            if(w) state.draftWorkouts.push({ ...JSON.parse(JSON.stringify(w)), id: Math.random().toString(36).substr(2, 9), title: w.title + " (Cópia)" });
            renderDraftWorkouts();
        };
        window.updateWorkoutTitle = (id, val) => { const w = state.draftWorkouts.find(w => w.id === id); if(w) w.title = val; };
        window.removeExercise = (wId, idx) => { state.draftWorkouts.find(w => w.id === wId).exercises.splice(idx,1); renderDraftWorkouts(); };
        window.updateExercise = (wId, idx, field, val) => { state.draftWorkouts.find(w => w.id === wId).exercises[idx][field] = val; };
        window.moveExercise = (wId, idx, dir) => {
            const list = state.draftWorkouts.find(w => w.id === wId).exercises;
            if (dir === 'up' && idx > 0) [list[idx], list[idx-1]] = [list[idx-1], list[idx]];
            if (dir === 'down' && idx < list.length-1) [list[idx], list[idx+1]] = [list[idx+1], list[idx]];
            renderDraftWorkouts();
        };

        function renderDraftWorkouts() {
            const c = document.getElementById('workouts-container');
            c.innerHTML = state.draftWorkouts.map(w => `
                <div class="card rounded-xl overflow-hidden shadow-sm">
                    <div class="p-3 border-b flex justify-between items-center bg-black bg-opacity-5" style="border-color: var(--border-color);">
                        <input type="text" value="${w.title}" onchange="updateWorkoutTitle('${w.id}', this.value)" class="input-field bg-transparent font-bold text-sm w-1/2 px-2 py-1 rounded uppercase">
                        <div class="flex gap-1">
                            <button onclick="duplicateWorkout('${w.id}')" class="text-xs p-1.5 rounded hover:bg-black hover:bg-opacity-10 transition"><i class="fas fa-copy"></i></button>
                            <button onclick="removeWorkout('${w.id}')" class="text-xs p-1.5 text-red-500 rounded hover:bg-red-500 hover:text-white transition"><i class="fas fa-trash"></i></button>
                        </div>
                    </div>
                    <div class="p-0">
                        ${w.exercises.length===0 ? '<div class="text-center p-4 text-xs opacity-50 italic">Sem exercícios.</div>' : w.exercises.map((ex, idx) => `
                            <div class="flex flex-col sm:flex-row gap-2 p-2 items-start sm:items-center border-b last:border-0 border-opacity-10" style="border-color: var(--border-color)">
                                <div class="flex gap-1 hidden sm:flex">
                                    <button onclick="moveExercise('${w.id}', ${idx}, 'up')" class="text-[10px] p-1 rounded hover:bg-black hover:bg-opacity-10"><i class="fas fa-chevron-up"></i></button>
                                    <button onclick="moveExercise('${w.id}', ${idx}, 'down')" class="text-[10px] p-1 rounded hover:bg-black hover:bg-opacity-10"><i class="fas fa-chevron-down"></i></button>
                                </div>
                                <div class="flex-1 font-medium w-full sm:w-auto">
                                    <div class="text-[9px] opacity-60 uppercase tracking-wider">${ex.category}</div>
                                    <div class="text-xs">${ex.name}</div>
                                </div>
                                <div class="flex flex-wrap gap-1 w-full sm:w-auto">
                                    <input type="text" value="${ex.sets}" onchange="updateExercise('${w.id}', ${idx}, 'sets', this.value)" class="input-field w-12 rounded px-1 py-1 text-xs text-center" placeholder="Séries">
                                    <span class="self-center opacity-50 text-xs">x</span>
                                    <input type="text" value="${ex.reps}" onchange="updateExercise('${w.id}', ${idx}, 'reps', this.value)" class="input-field w-16 rounded px-1 py-1 text-xs text-center" placeholder="Reps">
                                    <select onchange="updateExercise('${w.id}', ${idx}, 'technique', this.value)" class="input-field w-24 rounded px-1 py-1 text-[10px]">
                                        ${dbTechniques.map(t => `<option value="${t}" ${ex.technique===t?'selected':''}>${t}</option>`).join('')}
                                    </select>
                                    <input type="text" value="${ex.obs}" onchange="updateExercise('${w.id}', ${idx}, 'obs', this.value)" class="input-field flex-1 sm:w-32 rounded px-1 py-1 text-xs" placeholder="Obs...">
                                </div>
                                <button onclick="removeExercise('${w.id}', ${idx})" class="text-red-500 hover:text-red-700 p-1 rounded"><i class="fas fa-trash"></i></button>
                            </div>
                        `).join('')}
                    </div>
                    <div class="p-2 border-t" style="border-color: var(--border-color)">
                        <button onclick="openExModal('${w.id}')" class="w-full text-primary py-1.5 rounded-lg text-xs font-semibold hover:bg-primary hover:bg-opacity-10 transition">
                            + ADICIONAR EXERCÍCIO
                        </button>
                    </div>
                </div>
            `).join('');
        }

        // --- EXERCÍCIOS MODAL ---
        function getCombinedExercises(category) {
            const base = baseCategories[category] || [];
            const custom = state.customExercises.filter(e => e.category === category).map(e => e.name + " (Custom)");
            return [...base, ...custom];
        }

        window.openExModal = (id) => { state.activeModalWorkoutId = id; document.getElementById('exercise-modal').classList.remove('hidden'); renderModalCategories(); renderModalExercises(); };
        window.closeExModal = () => { document.getElementById('exercise-modal').classList.add('hidden'); };
        window.setModalCategory = (c) => { state.activeCategory = c; renderModalCategories(); renderModalExercises(); };
        
        function renderModalCategories() {
            document.getElementById('modal-categories').innerHTML = Object.keys(baseCategories).map(cat => `
                <button onclick="setModalCategory('${cat}')" class="w-full text-left px-2 py-2 rounded-lg text-xs font-medium transition ${state.activeCategory === cat ? 'bg-primary text-white' : 'hover:bg-black hover:bg-opacity-10'}">${cat}</button>
            `).join('');
        }
        function renderModalExercises() {
            const exList = getCombinedExercises(state.activeCategory);
            const isCardio = state.activeCategory === "🫀 CARDIO";
            document.getElementById('modal-exercises').innerHTML = `<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-2">` + 
                exList.map(ex => `<button onclick="addExercise('${ex}', ${isCardio})" class="card p-2 rounded-lg text-left text-xs font-medium border hover:border-primary transition flex justify-between items-center group"><span>${ex}</span><i class="fas fa-plus opacity-0 group-hover:opacity-100 text-primary"></i></button>`).join('') + `</div>`;
        }
        window.addExercise = (name, isC) => {
            state.draftWorkouts.find(w => w.id === state.activeModalWorkoutId).exercises.push({ category: state.activeCategory, name, sets: isC?'1':'3', reps: isC?'-':'10 a 12', technique: 'Nenhuma', obs: '' });
            renderDraftWorkouts();
            document.getElementById('modal-exercises').style.opacity = '0.5'; setTimeout(()=>document.getElementById('modal-exercises').style.opacity = '1', 100);
        };

        // ----------------------------------------------------
        // 7. EXERCÍCIOS CUSTOMIZADOS (NUVEM)
        // ----------------------------------------------------
        window.addCustomExercise = async () => {
            const cat = document.getElementById('custom-ex-cat').value;
            const name = document.getElementById('custom-ex-name').value.trim();
            if(!name) return;
            try {
                const data = { uid: state.currentUser.uid, category: cat, name: name };
                const docRef = await addDoc(collection(db, "powfit_custom_exercises"), data);
                state.customExercises.push({ id: docRef.id, ...data });
                document.getElementById('custom-ex-name').value = '';
                renderCustomExercises();
            } catch(e) { alert(e); }
        };
        window.deleteCustomEx = async (id) => {
            if(confirm("Excluir este exercício do seu banco na nuvem?")) {
                try {
                    await deleteDoc(doc(db, "powfit_custom_exercises", id));
                    state.customExercises = state.customExercises.filter(e => e.id !== id);
                    renderCustomExercises();
                } catch(e) { alert(e); }
            }
        };
        function renderCustomExercises() {
            document.getElementById('custom-ex-list').innerHTML = state.customExercises.map(ex => `
                <div class="card p-3 rounded flex justify-between items-center text-sm border border-gray-600">
                    <div><span class="text-[10px] opacity-70 block">${ex.category}</span><strong>${ex.name}</strong></div>
                    <button onclick="deleteCustomEx('${ex.id}')" class="text-red-500 hover:bg-red-500 hover:text-white p-1.5 rounded"><i class="fas fa-trash"></i></button>
                </div>
            `).join('');
        }

        // ----------------------------------------------------
        // 8. SALVAR, IMPRIMIR E RELATÓRIOS
        // ----------------------------------------------------
        window.saveAndPrintWorkout = async () => {
            if(!state.selectedMember) return alert("Selecione um profissional.");
            const d = new Date();
            const payload = {
                uid: state.currentUser.uid,
                memberId: state.selectedMember.id,
                unitId: state.selectedMember.unitId,
                date: d.toLocaleDateString('pt-BR'),
                monthYear: `${d.getMonth()+1}-${d.getFullYear()}`, // Para relatório
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
                workouts: state.draftWorkouts,
                createdAt: serverTimestamp()
            };

            // Salvar no Firebase
            try {
                const btn = document.getElementById('btn-save-print');
                btn.innerHTML = '<i class="fas fa-spinner fa-spin"></i> Salvando...';
                await addDoc(collection(db, "powfit_workouts"), payload);
                btn.innerHTML = '<i class="fas fa-save"></i> <i class="fas fa-print"></i> Salvo na Nuvem & Imprimir';
            } catch(e) {
                alert("Erro ao salvar na nuvem: " + e.message);
                return;
            }

            // Preparar HTML Impressão (Ficha)
            const pArea = document.getElementById('print-workout-area');
            const m = state.selectedMember;
            const isPEF = m.role === 'PEF';
            const healthSource = isPEF ? healthDataPEF : healthDataTE;
            
            const legalText = isPEF 
                ? `⚠️ OBSERVAÇÃO LEGAL – PROFISSIONAL DE EDUCAÇÃO FÍSICA\nConforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º estabelece que compete ao profissional coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos especializados.`
                : `⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO\nConforme a Lei nº 14.597/2023 (Lei Geral do Esporte), Art. 75, a profissão de treinador esportivo é reconhecida e regulada no Brasil. A atuação possui finalidade orientativa e não substitui avaliação médica. Em casos de doenças, lesões, gestação ou condições específicas, recomenda-se avaliação prévia por profissional habilitado. O treinamento proposto respeita limites individuais, priorizando segurança e evolução progressiva dentro da atuação permitida por lei. As recomendações desta ficha possuem caráter informativo e orientativo.`;

            let wHTML = `
                <div class="print-header">
                    <h1 class="print-title">Planilha de Treinamento - ${payload.stuObjective}</h1>
                    <div class="print-subtitle">
                        Prescrição feita por: ${m.name} (${m.role === 'PEF' ? 'Profissional de Ed. Física' : 'Treinador Esportivo'})<br>
                        ${isPEF ? `CREF: ${m.cref} - UF: ${m.state}` : `UF: ${m.state}`}
                    </div>
                </div>
                <div class="print-grid">
                    <div><strong>Aluno:</strong> ${payload.studentName}</div>
                    <div><strong>Idade:</strong> ${payload.stuAge}</div>
                    <div><strong>Gênero:</strong> ${payload.stuGender}</div>
                    <div><strong>Peso:</strong> ${payload.stuWeight} kg</div>
                    <div><strong>Altura:</strong> ${payload.stuHeight} m</div>
                    <div><strong>Nível:</strong> ${payload.stuLevel}</div>
                    <div><strong>Frequência:</strong> ${payload.stuFreq}</div>
                    <div><strong>Validade:</strong> ${payload.stuValidity}</div>
                    <div><strong>Data:</strong> ${payload.date}</div>
                </div>`;
            
            if (payload.health.length > 0 || objectiveData[payload.stuObjective]) {
                wHTML += `<div class="print-guidelines"><h4>Diretrizes Automáticas</h4><ul>`;
                if(objectiveData[payload.stuObjective]) wHTML += `<li><strong>${payload.stuObjective}:</strong> ${objectiveData[payload.stuObjective]}</li>`;
                payload.health.forEach(h => { if(healthSource[h]) wHTML += `<li><strong>${h.split(' ')[1] || h}:</strong> ${healthSource[h]}</li>`; });
                wHTML += `</ul></div>`;
            }

            payload.workouts.forEach(w => {
                wHTML += `<div class="print-workout"><h3>${w.title}</h3><table><thead><tr>
                    <th style="width:35%">Exercício</th><th style="width:8%;text-align:center">Séries</th><th style="width:12%;text-align:center">Reps</th><th style="width:15%">Técnica</th><th style="width:30%">Observações</th>
                </tr></thead><tbody>`;
                w.exercises.forEach(ex => {
                    wHTML += `<tr><td><strong>${ex.name}</strong></td><td style="text-align:center">${ex.sets}</td><td style="text-align:center">${ex.reps}</td><td>${ex.technique==='Nenhuma'?'-':ex.technique}</td><td>${ex.obs||'-'}</td></tr>`;
                });
                wHTML += `</tbody></table></div>`;
            });

            wHTML += `<div class="print-footer-section">`;
            if(payload.stuRecs) wHTML += `<strong>Recomendações:</strong><p style="white-space:pre-wrap;margin:3px 0 10px 0;">${payload.stuRecs}</p>`;
            wHTML += `<div class="legal-text">${legalText}</div></div>`;

            pArea.innerHTML = wHTML;
            
            // Disparar impressão (ocultando relatório)
            document.getElementById('print-report-area').classList.remove('active-print');
            pArea.classList.add('active-print');
            setTimeout(() => { window.print(); }, 500);
        };

        // HISTÓRICO DA NUVEM
        async function renderCloudHistory() {
            const list = document.getElementById('cloud-history-list');
            if(!state.selectedMember) {
                list.innerHTML = `<div class="text-center opacity-50 py-10">⚠️ Selecione sua Unidade e Perfil na aba "Franquias & Equipe" para ver o histórico.</div>`;
                return;
            }
            list.innerHTML = `<div class="text-center py-10"><i class="fas fa-spinner fa-spin"></i> Buscando na nuvem...</div>`;
            try {
                const q = query(collection(db, "powfit_workouts"), where("memberId", "==", state.selectedMember.id));
                const snap = await getDocs(q);
                let history = snap.docs.map(d => ({ docId: d.id, ...d.data() }));
                // Sort manual pois orderBy precisaria de index no firestore
                history.sort((a,b) => b.createdAt?.toMillis() - a.createdAt?.toMillis());
                
                if(history.length===0) { list.innerHTML = `<div class="text-center opacity-50 py-10">Nenhuma ficha registrada no seu perfil.</div>`; return; }

                list.innerHTML = history.map(h => {
                    // Calculo de validade (aviso visual simples)
                    const isOld = (Date.now() - (h.createdAt?.toMillis()||Date.now())) > 30*24*60*60*1000; 
                    return `
                    <div class="card p-4 rounded-xl flex justify-between items-center border border-gray-600">
                        <div>
                            <div class="font-bold text-lg">${h.studentName} ${isOld ? '<span class="bg-red-500 text-white text-[10px] px-2 py-0.5 rounded-full ml-2">Possível Vencida</span>' : ''}</div>
                            <div class="text-xs opacity-70">Data: ${h.date} | Objetivo: ${h.stuObjective} | Frequência: ${h.stuFreq}</div>
                        </div>
                        <div class="text-xs opacity-50"><i class="fas fa-cloud text-primary"></i> Salvo</div>
                    </div>
                `}).join('');
            } catch(e) { list.innerHTML = `Erro: ${e.message}`; }
        }

        // RELATÓRIO DE PRODUTIVIDADE MENSAL
        window.populateReportUnits = () => {
            const select = document.getElementById('report-unit-select');
            select.innerHTML = `<option value="">Todas as minhas Unidades...</option>` + state.units.map(u => `<option value="${u.id}">${u.name}</option>`).join('');
        };

        window.generateReport = async () => {
            const unitId = document.getElementById('report-unit-select').value;
            const d = new Date();
            const currentMonthYear = `${d.getMonth()+1}-${d.getFullYear()}`;
            
            try {
                const btn = event.currentTarget;
                const originalText = btn.innerHTML;
                btn.innerHTML = '<i class="fas fa-spinner fa-spin"></i>';
                
                // Busca todas as fichas do mes atual
                const q = query(collection(db, "powfit_workouts"), where("monthYear", "==", currentMonthYear));
                const snap = await getDocs(q);
                let workouts = snap.docs.map(doc => doc.data());

                if(unitId) workouts = workouts.filter(w => w.unitId === unitId);

                // Agrupamento
                const counts = {};
                let total = 0;
                workouts.forEach(w => {
                    counts[w.memberId] = (counts[w.memberId] || 0) + 1;
                    total++;
                });

                const c = document.getElementById('report-content');
                let html = `<h4 class="font-bold text-lg mb-2">Relatório - Mês ${currentMonthYear}</h4>
                            <table class="w-full text-sm text-left border-collapse report-table">
                                <thead><tr><th class="p-2 border border-gray-600">Treinador(a) / Perfil</th><th class="p-2 border border-gray-600">Unidade</th><th class="p-2 border border-gray-600 text-center">Fichas Geradas</th></tr></thead><tbody>`;
                
                Object.keys(counts).forEach(mId => {
                    const m = state.members.find(x => x.id === mId) || {name: 'Desconhecido', role: '-', unitId: null};
                    const u = state.units.find(x => x.id === m.unitId)?.name || '-';
                    html += `<tr><td class="p-2 border border-gray-600 font-medium">${m.name} <span class="text-[10px] opacity-70">(${m.role})</span></td><td class="p-2 border border-gray-600">${u}</td><td class="p-2 border border-gray-600 text-center font-bold">${counts[mId]}</td></tr>`;
                });
                
                html += `</tbody></table><div class="report-total">TOTAL DA REDE/UNIDADE: ${total} Fichas</div>`;
                c.innerHTML = html;
                document.getElementById('report-container').classList.remove('hidden');
                
                btn.innerHTML = originalText;
            } catch(e) { alert("Erro ao gerar relatório: " + e.message); }
        };

        window.printReport = () => {
            const content = document.getElementById('report-content').innerHTML;
            const pArea = document.getElementById('print-report-area');
            pArea.innerHTML = `
                <div class="print-header">
                    <h1 class="print-title">RELATÓRIO DE PRODUTIVIDADE DE FRANQUIA</h1>
                    <div class="print-subtitle">PowFit Pro System</div>
                </div>
                ${content}
            `;
            document.getElementById('print-workout-area').classList.remove('active-print');
            pArea.classList.add('active-print');
            setTimeout(() => { window.print(); }, 200);
        };

    </script>
</body>
</html>
