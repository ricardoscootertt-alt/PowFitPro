<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro - Master Edition</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            /* Tema Masculino / Dark Padrão */
            --bg-body: #0f172a; --bg-card: #1e293b; --text-main: #f8fafc; --text-muted: #94a3b8;
            --border-color: #334155; --primary: #3b82f6; --primary-hover: #2563eb;
            --input-bg: #0f172a; --input-border: #475569;
        }
        [data-theme="Feminino"] {
            /* Tema Feminino / Light Moderno */
            --bg-body: #fdf2f8; --bg-card: #ffffff; --text-main: #1e293b; --text-muted: #64748b;
            --border-color: #fbcfe8; --primary: #ec4899; --primary-hover: #db2777;
            --input-bg: #f8fafc; --input-border: #f1f5f9;
        }
        body { font-family: 'Inter', sans-serif; background-color: var(--bg-body); color: var(--text-main); transition: all 0.3s ease; }
        .card { background-color: var(--bg-card); border: 1px solid var(--border-color); }
        .input-field { background-color: var(--input-bg); border: 1px solid var(--input-border); color: var(--text-main); }
        .input-field:focus { outline: none; border-color: var(--primary); box-shadow: 0 0 0 1px var(--primary); }
        .btn-primary { background-color: var(--primary); color: white; transition: 0.2s; }
        .btn-primary:hover { background-color: var(--primary-hover); }
        .text-primary { color: var(--primary); }
        
        /* Oculta áreas de impressão na tela normal */
        #print-area, #print-report-area { display: none; }

        /* Estilos de Impressão Profissional (Estilo Planilha) */
        @media print {
            body { background: white !important; color: black !important; margin: 0; padding: 0; font-family: 'Arial', sans-serif; }
            #app-container, .modal, .no-print { display: none !important; }
            
            /* Impressão Ficha de Treino */
            #print-area.active-print { display: block !important; padding: 10mm; font-size: 10px; }
            #print-area .print-header { border-bottom: 2px solid #000; padding-bottom: 8px; margin-bottom: 12px; text-align: center; }
            #print-area .print-title { font-size: 18px; font-weight: bold; text-transform: uppercase; margin:0; }
            #print-area .prof-info { font-size: 11px; margin-top: 4px; font-weight: bold; }
            #print-area .print-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 4px; border: 1px solid #000; padding: 6px; margin-bottom: 12px; }
            #print-area .print-grid div { font-size: 10px; }
            #print-area .print-guidelines { border: 1px solid #000; padding: 6px; margin-bottom: 12px; font-size: 9px; }
            #print-area .print-guidelines h4 { margin: 0 0 4px 0; font-size: 10px; text-transform: uppercase; background: #eee; padding: 3px; }
            #print-area .print-guidelines ul { margin: 0; padding-left: 15px; }
            #print-area .print-workout { margin-bottom: 15px; page-break-inside: avoid; }
            #print-area .print-workout h3 { background: #e0e0e0; border: 1px solid #000; border-bottom: none; padding: 4px; margin: 0; font-size: 11px; text-transform: uppercase; -webkit-print-color-adjust: exact; }
            #print-area table { width: 100%; border-collapse: collapse; border: 1px solid #000; margin-bottom: 0; }
            #print-area th, #print-area td { border: 1px solid #000; padding: 4px; text-align: left; font-size: 9px; }
            #print-area th { background-color: #f0f0f0; font-weight: bold; -webkit-print-color-adjust: exact; text-transform: uppercase; }
            #print-area .print-footer-section { border: 1px solid #000; padding: 6px; font-size: 8px; page-break-inside: avoid; margin-top: 10px;}
            #print-area .legal-text { font-size: 7.5px; color: #222; text-align: justify; margin-top: 4px; font-style: italic; }

            /* Impressão Relatório */
            #print-report-area.active-print { display: block !important; padding: 10mm; font-size: 12px; }
            #print-report-area h1 { text-align: center; border-bottom: 2px solid #000; padding-bottom: 10px; }
            #print-report-area table { width: 100%; border-collapse: collapse; margin-top: 20px; }
            #print-report-area th, #print-report-area td { border: 1px solid #000; padding: 8px; text-align: left; }
            #print-report-area th { background: #eee; -webkit-print-color-adjust: exact; }
        }

        /* Modals & Overlays */
        .modal { z-index: 100; }
        .hidden { display: none !important; }
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
    </style>
</head>
<body data-theme="Masculino">

    <!-- Loading Splash / Auth State -->
    <div id="loading-overlay" class="fixed inset-0 bg-black z-[200] flex flex-col items-center justify-center text-white">
        <i class="fas fa-circle-notch fa-spin text-5xl text-blue-500 mb-4"></i>
        <h2 class="text-xl font-bold">Conectando ao PowFit Pro...</h2>
        <p class="text-sm opacity-60">Sincronizando nuvem</p>
    </div>

    <!-- Custom Confirm Modal -->
    <div id="custom-confirm" class="fixed inset-0 bg-black bg-opacity-70 backdrop-blur-sm hidden z-[150] flex items-center justify-center p-4">
        <div class="bg-gray-800 border border-gray-600 rounded-xl max-w-sm w-full p-6 text-white shadow-2xl">
            <h3 id="confirm-title" class="text-lg font-bold mb-2">Confirmação</h3>
            <p id="confirm-msg" class="text-sm text-gray-300 mb-6">Tem certeza que deseja continuar?</p>
            <div class="flex justify-end gap-3">
                <button id="confirm-btn-no" class="px-4 py-2 rounded bg-gray-600 hover:bg-gray-500 transition text-sm font-medium">Não</button>
                <button id="confirm-btn-yes" class="px-4 py-2 rounded bg-red-600 hover:bg-red-500 transition text-sm font-medium">Sim</button>
            </div>
        </div>
    </div>

    <!-- Notificações Toast -->
    <div id="toast-container" class="fixed top-4 right-4 z-[160] flex flex-col gap-2 pointer-events-none"></div>

    <!-- NAVEGAÇÃO PRINCIPAL -->
    <nav class="border-b border-opacity-20" style="border-color: var(--border-color); background-color: var(--bg-card);">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between h-16">
                <div class="flex items-center gap-2">
                    <i class="fas fa-dumbbell text-2xl text-primary"></i>
                    <span class="font-bold text-lg tracking-tight">PowFit Pro</span>
                </div>
                <div class="hidden md:flex items-center space-x-1 overflow-x-auto">
                    <button onclick="nav('nova-ficha')" class="nav-btn px-3 py-2 rounded-md text-sm font-medium hover:bg-black hover:bg-opacity-10 text-primary">Nova Ficha</button>
                    <button onclick="nav('rede')" class="nav-btn px-3 py-2 rounded-md text-sm font-medium hover:bg-black hover:bg-opacity-10 opacity-70">Rede & Equipe</button>
                    <button onclick="nav('historico')" class="nav-btn px-3 py-2 rounded-md text-sm font-medium hover:bg-black hover:bg-opacity-10 opacity-70">Histórico</button>
                    <button onclick="nav('relatorios')" class="nav-btn px-3 py-2 rounded-md text-sm font-medium hover:bg-black hover:bg-opacity-10 opacity-70">Relatórios</button>
                    <button onclick="nav('exercicios')" class="nav-btn px-3 py-2 rounded-md text-sm font-medium hover:bg-black hover:bg-opacity-10 opacity-70">Meus Exercícios</button>
                </div>
                <!-- Menu Mobile -->
                <div class="md:hidden flex items-center">
                    <select id="mobile-nav" onchange="nav(this.value)" class="input-field text-sm p-1 rounded">
                        <option value="nova-ficha">Nova Ficha</option>
                        <option value="rede">Rede & Equipe</option>
                        <option value="historico">Histórico</option>
                        <option value="relatorios">Relatórios</option>
                        <option value="exercicios">Meus Exercícios</option>
                    </select>
                </div>
            </div>
        </div>
    </nav>

    <!-- CONTAINER DA APLICAÇÃO -->
    <div id="app-container" class="max-w-7xl mx-auto p-4 sm:p-6 lg:p-8">
        
        <!-- ========================================== -->
        <!-- VIEW: NOVA FICHA                           -->
        <!-- ========================================== -->
        <div id="view-nova-ficha" class="app-view">
            <div class="flex justify-between items-center mb-6">
                <h2 class="text-xl font-bold"><i class="fas fa-clipboard-check text-primary mr-2"></i> Criar Ficha de Treino</h2>
                <button onclick="saveAndPrintWorkout()" class="btn-primary px-6 py-2 rounded-lg font-bold shadow-lg flex items-center gap-2">
                    <i class="fas fa-save"></i> <i class="fas fa-print"></i> Salvar e Imprimir
                </button>
            </div>

            <div class="grid grid-cols-1 xl:grid-cols-12 gap-6">
                <!-- Coluna Esquerda: Configs -->
                <div class="xl:col-span-4 space-y-4">
                    
                    <!-- Profissional Selecionado -->
                    <div class="card rounded-xl p-4 shadow-sm">
                        <label class="block text-sm font-bold mb-2 text-primary">Profissional Responsável (Membro)</label>
                        <select id="wf-member" onchange="onMemberSelectChange()" class="input-field w-full rounded-lg px-3 py-2 text-sm font-medium">
                            <option value="">-- Selecione o Treinador --</option>
                            <!-- Povoado via JS -->
                        </select>
                        <p id="wf-member-info" class="text-xs mt-2 opacity-70 italic"></p>
                    </div>

                    <!-- Dados do Aluno -->
                    <div class="card rounded-xl p-4 shadow-sm">
                        <div class="flex justify-between items-center mb-3">
                            <h3 class="font-bold">Dados do Aluno</h3>
                            <div id="imc-display"></div>
                        </div>
                        <div class="space-y-3">
                            <input type="text" id="stu-name" class="input-field w-full rounded px-3 py-1.5 text-sm" placeholder="Nome do aluno">
                            <div class="grid grid-cols-3 gap-2">
                                <input type="number" id="stu-age" class="input-field w-full rounded px-2 py-1.5 text-sm" placeholder="Idade">
                                <input type="number" id="stu-weight" oninput="calculateIMC()" class="input-field w-full rounded px-2 py-1.5 text-sm" placeholder="Peso(kg)">
                                <input type="number" id="stu-height" step="0.01" oninput="calculateIMC()" class="input-field w-full rounded px-2 py-1.5 text-sm" placeholder="Alt(m)">
                            </div>
                            <div class="grid grid-cols-2 gap-2">
                                <select id="stu-gender" onchange="changeTheme()" class="input-field w-full rounded px-2 py-1.5 text-sm">
                                    <option value="Masculino">Masculino</option><option value="Feminino">Feminino</option>
                                </select>
                                <select id="stu-level" class="input-field w-full rounded px-2 py-1.5 text-sm">
                                    <option>Iniciante</option><option>Intermediário</option><option>Avançado</option>
                                </select>
                            </div>
                            <select id="stu-objective" onchange="updateObjectiveText()" class="input-field w-full rounded px-2 py-1.5 text-sm">
                                <option value="Emagrecimento">Emagrecimento</option><option value="Hipertrofia">Hipertrofia</option>
                                <option value="Definição">Definição</option><option value="Condicionamento">Condicionamento</option>
                                <option value="Resistência">Resistência</option><option value="Força">Força</option>
                                <option value="Reabilitação">Reabilitação</option><option value="Saúde geral">Saúde geral</option>
                            </select>
                            <p id="obj-text" class="text-[10px] opacity-70 leading-tight"></p>
                            <select id="stu-freq" class="input-field w-full rounded px-2 py-1.5 text-sm">
                                <option>3 dias</option><option>5 dias</option><option>6 dias</option><option>Personalizado</option>
                            </select>
                            <select id="stu-validity" class="input-field w-full rounded px-2 py-1.5 text-sm">
                                <option value="15">Validade: 15 dias</option>
                                <option value="30" selected>Validade: 30 dias</option>
                                <option value="60">Validade: 60 dias</option>
                                <option value="90">Validade: 90 dias</option>
                            </select>
                        </div>
                    </div>

                    <!-- Estado de Saúde -->
                    <div class="card rounded-xl p-4 shadow-sm">
                        <h3 class="font-bold mb-2 text-sm">Estado de Saúde</h3>
                        <p class="text-[10px] opacity-70 mb-2">Diretrizes automáticas baseadas na categoria do profissional logado.</p>
                        <div class="grid grid-cols-2 gap-1 text-[11px]" id="health-container">
                            <!-- JS popula -->
                        </div>
                    </div>
                </div>

                <!-- Coluna Direita: Treinos -->
                <div class="xl:col-span-8 space-y-4">
                    <div class="flex justify-between items-center">
                        <h3 class="font-bold text-lg">Montagem dos Treinos</h3>
                        <button onclick="addWorkoutDay()" class="btn-primary px-3 py-1.5 rounded text-sm font-medium"><i class="fas fa-plus"></i> Adicionar Dia</button>
                    </div>
                    
                    <div id="workout-days-container" class="space-y-4">
                        <!-- JS popula -->
                    </div>

                    <div class="card rounded-xl p-4 shadow-sm mt-4">
                        <h3 class="font-bold text-sm mb-2">Recomendações Manuais Livres</h3>
                        <textarea id="stu-recs" rows="2" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="Hidratação, descanso..."></textarea>
                    </div>
                </div>
            </div>
        </div>

        <!-- ========================================== -->
        <!-- VIEW: REDE E EQUIPE                        -->
        <!-- ========================================== -->
        <div id="view-rede" class="app-view hidden">
            <h2 class="text-xl font-bold mb-6"><i class="fas fa-network-wired text-primary mr-2"></i> Gestão de Franquias e Equipe</h2>
            
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <!-- Redes -->
                <div class="card p-4 rounded-xl">
                    <h3 class="font-bold mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">1. Redes</h3>
                    <div class="flex gap-2 mb-4">
                        <input type="text" id="new-network-name" class="input-field w-full rounded px-2 py-1 text-sm" placeholder="Nome da Rede">
                        <button onclick="createNetwork()" class="btn-primary px-3 rounded"><i class="fas fa-plus"></i></button>
                    </div>
                    <div id="network-list" class="space-y-2 text-sm"></div>
                </div>

                <!-- Unidades -->
                <div class="card p-4 rounded-xl">
                    <h3 class="font-bold mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">2. Unidades</h3>
                    <select id="sel-network-for-unit" class="input-field w-full rounded px-2 py-1 text-sm mb-2"><option value="">- Selecione a Rede -</option></select>
                    <div class="flex gap-2 mb-4">
                        <input type="text" id="new-unit-name" class="input-field w-full rounded px-2 py-1 text-sm" placeholder="Nome da Unidade">
                        <button onclick="createUnit()" class="btn-primary px-3 rounded"><i class="fas fa-plus"></i></button>
                    </div>
                    <div id="unit-list" class="space-y-2 text-sm"></div>
                </div>

                <!-- Membros -->
                <div class="card p-4 rounded-xl">
                    <h3 class="font-bold mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">3. Membros (Equipe)</h3>
                    <select id="sel-unit-for-member" class="input-field w-full rounded px-2 py-1 text-sm mb-2"><option value="">- Selecione a Unidade -</option></select>
                    
                    <div class="space-y-2 mb-4 bg-black bg-opacity-10 p-3 rounded">
                        <input type="text" id="new-mem-name" class="input-field w-full rounded px-2 py-1 text-sm" placeholder="Nome Completo">
                        <input type="text" id="new-mem-cpf" class="input-field w-full rounded px-2 py-1 text-sm" placeholder="CPF">
                        <input type="text" id="new-mem-state" class="input-field w-full rounded px-2 py-1 text-sm" placeholder="Estado (UF)">
                        <select id="new-mem-cat" onchange="toggleCrefField()" class="input-field w-full rounded px-2 py-1 text-sm">
                            <option value="PEF">Profissional de Educação Física (PEF)</option>
                            <option value="TE">Treinador Esportivo (TE)</option>
                        </select>
                        <input type="text" id="new-mem-cref" class="input-field w-full rounded px-2 py-1 text-sm" placeholder="Número do CREF">
                        <button onclick="createMember()" class="btn-primary w-full py-1.5 rounded text-sm font-bold mt-2">Adicionar Membro</button>
                    </div>
                    <div id="member-list" class="space-y-2 text-sm"></div>
                </div>
            </div>
        </div>

        <!-- ========================================== -->
        <!-- VIEW: HISTÓRICO                            -->
        <!-- ========================================== -->
        <div id="view-historico" class="app-view hidden">
            <h2 class="text-xl font-bold mb-6"><i class="fas fa-cloud text-primary mr-2"></i> Histórico na Nuvem</h2>
            <div class="card rounded-xl p-4 overflow-x-auto">
                <table class="w-full text-left text-sm">
                    <thead>
                        <tr class="border-b" style="border-color: var(--border-color)">
                            <th class="p-2">Data</th>
                            <th class="p-2">Aluno</th>
                            <th class="p-2">Profissional</th>
                            <th class="p-2">Unidade</th>
                            <th class="p-2">Status</th>
                            <th class="p-2">Ações</th>
                        </tr>
                    </thead>
                    <tbody id="history-tbody">
                        <!-- JS -->
                    </tbody>
                </table>
            </div>
        </div>

        <!-- ========================================== -->
        <!-- VIEW: RELATÓRIOS                           -->
        <!-- ========================================== -->
        <div id="view-relatorios" class="app-view hidden">
            <div class="flex justify-between items-center mb-6">
                <h2 class="text-xl font-bold"><i class="fas fa-chart-bar text-primary mr-2"></i> Relatório de Produtividade</h2>
                <button onclick="printReport()" class="btn-primary px-4 py-2 rounded shadow font-medium"><i class="fas fa-print"></i> Imprimir Relatório</button>
            </div>
            
            <div class="card rounded-xl p-4 mb-6 flex gap-4 items-end">
                <div>
                    <label class="block text-xs mb-1 opacity-70">Mês</label>
                    <select id="report-month" class="input-field rounded px-3 py-1.5 text-sm">
                        <option value="01">Janeiro</option><option value="02">Fevereiro</option><option value="03">Março</option>
                        <option value="04">Abril</option><option value="05">Maio</option><option value="06">Junho</option>
                        <option value="07">Julho</option><option value="08">Agosto</option><option value="09">Setembro</option>
                        <option value="10">Outubro</option><option value="11">Novembro</option><option value="12">Dezembro</option>
                    </select>
                </div>
                <div>
                    <label class="block text-xs mb-1 opacity-70">Ano</label>
                    <input type="number" id="report-year" class="input-field rounded px-3 py-1.5 text-sm w-24" value="2026">
                </div>
                <div>
                    <label class="block text-xs mb-1 opacity-70">Filtrar por Unidade</label>
                    <select id="report-unit" class="input-field rounded px-3 py-1.5 text-sm">
                        <option value="ALL">Todas as Unidades</option>
                        <!-- JS -->
                    </select>
                </div>
                <button onclick="generateReport()" class="bg-gray-700 hover:bg-gray-600 text-white px-4 py-1.5 rounded transition font-medium">Gerar</button>
            </div>

            <div class="card rounded-xl p-4 overflow-x-auto">
                <table class="w-full text-left text-sm">
                    <thead>
                        <tr class="border-b" style="border-color: var(--border-color)">
                            <th class="p-2">Unidade</th>
                            <th class="p-2">Membro (Profissional)</th>
                            <th class="p-2 text-center">Total de Fichas Emitidas</th>
                        </tr>
                    </thead>
                    <tbody id="report-tbody">
                        <tr><td colspan="3" class="text-center opacity-50 p-4">Selecione os filtros e clique em Gerar</td></tr>
                    </tbody>
                </table>
            </div>
        </div>

        <!-- ========================================== -->
        <!-- VIEW: EXERCÍCIOS                           -->
        <!-- ========================================== -->
        <div id="view-exercicios" class="app-view hidden">
            <h2 class="text-xl font-bold mb-6"><i class="fas fa-dumbbell text-primary mr-2"></i> Banco de Exercícios Customizados</h2>
            
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div class="card rounded-xl p-4">
                    <h3 class="font-bold mb-3">Adicionar Novo Exercício à Nuvem</h3>
                    <select id="custom-ex-cat" class="input-field w-full rounded px-3 py-2 text-sm mb-3">
                        <option value="🔥 PEITO">🔥 PEITO</option><option value="🦍 COSTAS">🦍 COSTAS</option>
                        <option value="🦵 PERNAS">🦵 PERNAS</option><option value="💪 BRAÇOS">💪 BRAÇOS</option>
                        <option value="🪨 OMBROS">🪨 OMBROS</option><option value="🧠 ABDÔMEN">🧠 ABDÔMEN</option>
                        <option value="🫀 CARDIO">🫀 CARDIO</option>
                    </select>
                    <input type="text" id="custom-ex-name" class="input-field w-full rounded px-3 py-2 text-sm mb-3" placeholder="Nome do Exercício">
                    <button onclick="addCustomExercise()" class="btn-primary w-full py-2 rounded font-bold">Salvar no Banco</button>
                </div>
                
                <div class="card rounded-xl p-4">
                    <h3 class="font-bold mb-3">Meus Exercícios Customizados</h3>
                    <div id="custom-ex-list" class="space-y-2 text-sm max-h-64 overflow-y-auto">
                        <!-- JS -->
                    </div>
                </div>
            </div>
        </div>

    </div> <!-- Fim App Container -->

    <!-- MODAL ESCOLHER EXERCÍCIO -->
    <div id="exercise-modal" class="modal fixed inset-0 bg-black bg-opacity-80 backdrop-blur-sm hidden flex items-center justify-center p-2">
        <div class="card w-full max-w-5xl rounded-xl shadow-2xl flex flex-col h-[90vh]">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold">Selecione os Exercícios</h3>
                <button onclick="closeExerciseModal()" class="text-gray-400 hover:text-red-500 text-2xl leading-none">&times;</button>
            </div>
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <div class="w-full md:w-1/4 border-r overflow-y-auto p-2" style="border-color: var(--border-color)" id="modal-categories"></div>
                <div class="w-full md:w-3/4 overflow-y-auto p-3 bg-black bg-opacity-10" id="modal-exercises"></div>
            </div>
        </div>
    </div>

    <!-- ========================================== -->
    <!-- ÁREA DE IMPRESSÃO - FICHA                  -->
    <!-- ========================================== -->
    <div id="print-area"></div>
    
    <!-- ========================================== -->
    <!-- ÁREA DE IMPRESSÃO - RELATÓRIO              -->
    <!-- ========================================== -->
    <div id="print-report-area"></div>

    <!-- FIREBASE & LÓGICA DO APP -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-app.js";
        import { getAuth, signInWithCustomToken, signInAnonymously, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-auth.js";
        import { getFirestore, collection, doc, setDoc, getDocs, addDoc, deleteDoc, updateDoc } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-firestore.js";

        // Firebase Setup
        const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : {
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
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'powfit-pro-master';
        let currentUser = null;

        // Dicionários de Dados Fixos
        const healthPEF = {
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

        const healthTE = {
            "🟢 Saudável": "Manter treinos regulares de 3–5x por semana, combinando musculação e atividades cardiovasculares. A constância, descanso adequado e boa alimentação contribuem para melhores resultados.",
            "⚪ Sedentário": "O início deve ser gradual, com treinos leves e foco na adaptação do corpo. A prioridade é desenvolver constância, aprender a execução correta e evitar excesso de carga.",
            "🟡 Sobrepeso": "A prática regular de musculação associada ao cardio pode contribuir para melhora do condicionamento físico e composição corporal. A progressão deve respeitar a individualidade e o nível de adaptação.",
            "🔴 Obesidade": "É recomendado iniciar com exercícios de menor impacto e progressão gradual. A segurança, mobilidade e constância são prioridades durante o processo.",
            "⚖️ Baixo peso": "A musculação pode auxiliar no ganho de massa muscular quando associada a alimentação adequada e descanso. O treino deve priorizar evolução progressiva e recuperação muscular.",
            "🍬 Diabetes": "Pessoas com diabetes devem manter acompanhamento médico regular antes e durante a prática de exercícios. A musculação pode auxiliar na rotina de atividade física quando liberada por profissional de saúde. Em casos de tontura, mal-estar ou alteração de glicemia, o treino deve ser interrompido e o aluno deve procurar orientação médica.",
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
        const daysOrder = ["SEGUNDA-FEIRA", "TERÇA-FEIRA", "QUARTA-FEIRA", "QUINTA-FEIRA", "SEXTA-FEIRA", "SÁBADO", "DOMINGO"];

        // Estado Global
        const state = {
            networks: [], units: [], members: [],
            customExercises: [], workouts: [],
            currentFichaId: null, // se estamos editando
            workoutDays: [],
            activeModalDayIdx: null,
            activeCategory: "🔥 PEITO"
        };

        // --- FUNÇÕES UTILITÁRIAS ---
        window.showToast = (msg, type = 'success') => {
            const toast = document.createElement('div');
            const color = type === 'success' ? 'bg-green-600' : 'bg-red-600';
            toast.className = `${color} text-white px-4 py-2 rounded shadow-lg text-sm transition-opacity duration-300`;
            toast.textContent = msg;
            document.getElementById('toast-container').appendChild(toast);
            setTimeout(() => toast.classList.add('opacity-0'), 2500);
            setTimeout(() => toast.remove(), 2800);
        };

        window.showConfirm = (title, message) => {
            return new Promise((resolve) => {
                const modal = document.getElementById('custom-confirm');
                document.getElementById('confirm-title').textContent = title;
                document.getElementById('confirm-msg').textContent = message;
                modal.classList.remove('hidden');
                
                const btnYes = document.getElementById('confirm-btn-yes');
                const btnNo = document.getElementById('confirm-btn-no');
                
                const cleanup = () => {
                    modal.classList.add('hidden');
                    btnYes.replaceWith(btnYes.cloneNode(true));
                    btnNo.replaceWith(btnNo.cloneNode(true));
                };

                btnYes.addEventListener('click', () => { cleanup(); resolve(true); }, {once: true});
                btnNo.addEventListener('click', () => { cleanup(); resolve(false); }, {once: true});
            });
        };

        const getBasePath = () => `artifacts/${appId}/users/${currentUser.uid}`;

        // --- INICIALIZAÇÃO E AUTH FIREBASE ---
        async function initAuth() {
            if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                await signInWithCustomToken(auth, __initial_auth_token);
            } else {
                await signInAnonymously(auth);
            }
        }

        onAuthStateChanged(auth, async (user) => {
            if (user) {
                currentUser = user;
                await loadAllData();
                document.getElementById('loading-overlay').classList.add('hidden');
                updateObjectiveText();
                window.addWorkoutDay(); // Começa com um dia vazio
            }
        });

        // Bootstrap App
        window.onload = () => {
            initAuth().catch(e => {
                console.error("Auth erro:", e);
                showToast("Erro de autenticação", "error");
            });
            document.getElementById('report-month').value = String(new Date().getMonth() + 1).padStart(2, '0');
            document.getElementById('report-year').value = new Date().getFullYear();
        };

        // --- DATA LAYER (FIREBASE) ---
        async function loadAllData() {
            const base = getBasePath();
            try {
                // Fetch Networks
                const netSnap = await getDocs(collection(db, base, 'networks'));
                state.networks = netSnap.docs.map(d => ({id: d.id, ...d.data()}));
                // Fetch Units
                const uniSnap = await getDocs(collection(db, base, 'units'));
                state.units = uniSnap.docs.map(d => ({id: d.id, ...d.data()}));
                // Fetch Members
                const memSnap = await getDocs(collection(db, base, 'members'));
                state.members = memSnap.docs.map(d => ({id: d.id, ...d.data()}));
                // Fetch Custom Exercises
                const exSnap = await getDocs(collection(db, base, 'customExercises'));
                state.customExercises = exSnap.docs.map(d => ({id: d.id, ...d.data()}));
                // Fetch Workouts
                const workSnap = await getDocs(collection(db, base, 'workouts'));
                state.workouts = workSnap.docs.map(d => ({id: d.id, ...d.data()}));

                renderNetworkManagement();
                renderFichaMemberSelect();
                renderCustomExercisesList();
                renderHistory();
            } catch (e) { console.error(e); showToast("Erro ao carregar dados.", "error"); }
        }

        // --- NAVEGAÇÃO E UI ---
        window.nav = (viewId) => {
            document.querySelectorAll('.app-view').forEach(v => v.classList.add('hidden'));
            document.querySelectorAll('.nav-btn').forEach(b => {
                b.classList.remove('text-primary');
                b.classList.add('opacity-70');
            });
            document.getElementById(`view-${viewId}`).classList.remove('hidden');
            
            // Ativa botão topo
            const btns = Array.from(document.querySelectorAll('.nav-btn'));
            const activeBtn = btns.find(b => b.getAttribute('onclick') === `nav('${viewId}')`);
            if(activeBtn) {
                activeBtn.classList.remove('opacity-70');
                activeBtn.classList.add('text-primary');
            }
            document.getElementById('mobile-nav').value = viewId;

            if(viewId === 'relatorios') populateReportUnits();
        };

        window.changeTheme = () => {
            document.body.setAttribute('data-theme', document.getElementById('stu-gender').value);
        };

        window.calculateIMC = () => {
            const w = parseFloat(document.getElementById('stu-weight').value);
            const h = parseFloat(document.getElementById('stu-height').value);
            const disp = document.getElementById('imc-display');
            if (w > 0 && h > 0) {
                const imc = (w / (h * h)).toFixed(1);
                let cls = "";
                if (imc < 18.5) cls = "Abaixo do peso";
                else if (imc < 25) cls = "Peso normal";
                else if (imc < 30) cls = "Sobrepeso";
                else if (imc < 35) cls = "Obesidade Grau I";
                else if (imc < 40) cls = "Obesidade Grau II";
                else cls = "Obesidade Grau III";
                disp.innerHTML = `<span class="bg-primary bg-opacity-20 text-primary px-3 py-1 rounded-full text-[10px] font-bold border border-primary border-opacity-30">IMC: ${imc} (${cls})</span>`;
            } else { disp.innerHTML = ''; }
        };

        window.updateObjectiveText = () => {
            const obj = document.getElementById('stu-objective').value;
            document.getElementById('obj-text').textContent = objectiveData[obj] || '';
        };

        // --- GESTÃO DE REDE, UNIDADE E MEMBROS ---
        window.createNetwork = async () => {
            const name = document.getElementById('new-network-name').value.trim();
            if(!name) return;
            try {
                const docRef = await addDoc(collection(db, getBasePath(), 'networks'), { name, createdAt: Date.now() });
                state.networks.push({ id: docRef.id, name });
                document.getElementById('new-network-name').value = '';
                showToast("Rede criada!");
                renderNetworkManagement();
            } catch(e) { showToast("Erro", "error"); }
        };

        window.createUnit = async () => {
            const netId = document.getElementById('sel-network-for-unit').value;
            const name = document.getElementById('new-unit-name').value.trim();
            if(!netId || !name) return showToast("Selecione rede e digite nome", "error");
            try {
                const docRef = await addDoc(collection(db, getBasePath(), 'units'), { name, networkId: netId, createdAt: Date.now() });
                state.units.push({ id: docRef.id, name, networkId: netId });
                document.getElementById('new-unit-name').value = '';
                showToast("Unidade criada!");
                renderNetworkManagement();
                renderFichaMemberSelect();
            } catch(e) { showToast("Erro", "error"); }
        };

        window.toggleCrefField = () => {
            const cat = document.getElementById('new-mem-cat').value;
            document.getElementById('new-mem-cref').style.display = cat === 'TE' ? 'none' : 'block';
        };

        window.createMember = async () => {
            const unitId = document.getElementById('sel-unit-for-member').value;
            const name = document.getElementById('new-mem-name').value.trim();
            const cpf = document.getElementById('new-mem-cpf').value.trim();
            const stateUf = document.getElementById('new-mem-state').value.trim();
            const cat = document.getElementById('new-mem-cat').value;
            const cref = document.getElementById('new-mem-cref').value.trim();

            if(!unitId || !name || !cpf || !stateUf) return showToast("Preencha todos os campos", "error");
            if(cat === 'PEF' && !cref) return showToast("CREF é obrigatório para PEF", "error");

            const memObj = { unitId, name, cpf, state: stateUf, category: cat, cref: cat === 'PEF' ? cref : '', createdAt: Date.now() };
            try {
                const docRef = await addDoc(collection(db, getBasePath(), 'members'), memObj);
                state.members.push({ id: docRef.id, ...memObj });
                document.getElementById('new-mem-name').value = '';
                document.getElementById('new-mem-cpf').value = '';
                showToast("Membro cadastrado!");
                renderNetworkManagement();
                renderFichaMemberSelect();
            } catch(e) { showToast("Erro", "error"); }
        };

        window.deleteEntity = async (type, id) => {
            if(!(await showConfirm("Excluir", "Tem certeza? Isso apagará este item permanentemente da nuvem."))) return;
            try {
                await deleteDoc(doc(db, getBasePath(), type, id));
                state[type] = state[type].filter(i => i.id !== id);
                renderNetworkManagement();
                renderFichaMemberSelect();
                showToast("Excluído!");
            } catch(e) { showToast("Erro", "error"); }
        };

        function renderNetworkManagement() {
            // Networks
            document.getElementById('network-list').innerHTML = state.networks.map(n => `
                <div class="flex justify-between items-center p-2 bg-black bg-opacity-10 rounded">
                    <span>${n.name}</span>
                    <button onclick="deleteEntity('networks', '${n.id}')" class="text-red-500 hover:text-white hover:bg-red-500 p-1 rounded"><i class="fas fa-trash"></i></button>
                </div>
            `).join('');

            // Units list & select
            document.getElementById('sel-network-for-unit').innerHTML = `<option value="">- Selecione a Rede -</option>` + state.networks.map(n => `<option value="${n.id}">${n.name}</option>`).join('');
            
            document.getElementById('unit-list').innerHTML = state.units.map(u => {
                const netName = state.networks.find(n => n.id === u.networkId)?.name || 'Rede Órfã';
                return `<div class="flex justify-between items-center p-2 bg-black bg-opacity-10 rounded">
                    <div><span class="font-bold">${u.name}</span> <span class="text-[10px] opacity-60">(${netName})</span></div>
                    <button onclick="deleteEntity('units', '${u.id}')" class="text-red-500 hover:text-white hover:bg-red-500 p-1 rounded"><i class="fas fa-trash"></i></button>
                </div>`
            }).join('');

            // Members list & select
            document.getElementById('sel-unit-for-member').innerHTML = `<option value="">- Selecione a Unidade -</option>` + state.units.map(u => `<option value="${u.id}">${u.name}</option>`).join('');
            
            document.getElementById('member-list').innerHTML = state.members.map(m => {
                const uName = state.units.find(u => u.id === m.unitId)?.name || '';
                return `<div class="flex justify-between items-center p-2 bg-black bg-opacity-10 rounded">
                    <div>
                        <div class="font-bold">${m.name} <span class="text-primary text-[10px] bg-primary bg-opacity-20 px-1 rounded">${m.category}</span></div>
                        <div class="text-[10px] opacity-70">${uName} - CPF: ${m.cpf}</div>
                    </div>
                    <button onclick="deleteEntity('members', '${m.id}')" class="text-red-500 hover:text-white hover:bg-red-500 p-1 rounded"><i class="fas fa-trash"></i></button>
                </div>`
            }).join('');
        }

        // --- FICHA DE TREINO LOGIC ---
        function renderFichaMemberSelect() {
            const sel = document.getElementById('wf-member');
            sel.innerHTML = `<option value="">-- Selecione o Treinador --</option>`;
            
            // Group by unit
            state.units.forEach(u => {
                const mems = state.members.filter(m => m.unitId === u.id);
                if(mems.length > 0) {
                    const group = document.createElement('optgroup');
                    group.label = `Unidade: ${u.name}`;
                    mems.forEach(m => {
                        const opt = document.createElement('option');
                        opt.value = m.id;
                        opt.textContent = `${m.name} (${m.category})`;
                        group.appendChild(opt);
                    });
                    sel.appendChild(group);
                }
            });
        }

        window.onMemberSelectChange = () => {
            const memId = document.getElementById('wf-member').value;
            const infoP = document.getElementById('wf-member-info');
            const healthCont = document.getElementById('health-container');
            
            if(!memId) {
                infoP.textContent = '';
                healthCont.innerHTML = '<span class="opacity-50 italic">Selecione o treinador primeiro.</span>';
                return;
            }
            
            const mem = state.members.find(m => m.id === memId);
            infoP.textContent = mem.category === 'PEF' ? `CREF: ${mem.cref} - UF: ${mem.state}` : `Treinador Esportivo - UF: ${mem.state}`;
            
            // Render Health Checkboxes
            const dict = mem.category === 'PEF' ? healthPEF : healthTE;
            healthCont.innerHTML = Object.keys(dict).map(opt => `
                <label class="flex items-start space-x-1 cursor-pointer">
                    <input type="checkbox" value="${opt}" class="health-cb mt-1 rounded border-gray-400 text-primary w-3 h-3">
                    <span>${opt}</span>
                </label>
            `).join('');
        };

        // --- WORKOUT BUILDER ---
        window.addWorkoutDay = () => {
            const currentDays = state.workoutDays.length;
            const nextName = currentDays < 7 ? `TREINO ${daysOrder[currentDays]}` : `NOVO TREINO ${currentDays+1}`;
            state.workoutDays.push({ id: generateId(), title: nextName, exercises: [] });
            renderWorkoutDays();
        };

        window.deleteDay = (idx) => {
            state.workoutDays.splice(idx, 1);
            renderWorkoutDays();
        }

        window.updateDayTitle = (idx, val) => { state.workoutDays[idx].title = val; };

        function renderWorkoutDays() {
            const cont = document.getElementById('workout-days-container');
            cont.innerHTML = state.workoutDays.map((day, dIdx) => {
                let exHtml = day.exercises.length === 0 ? `<div class="text-center p-3 opacity-50 text-xs">Sem exercícios</div>` : 
                day.exercises.map((ex, eIdx) => `
                    <div class="flex flex-col sm:flex-row gap-2 p-2 items-center border-b border-opacity-20" style="border-color: var(--border-color)">
                        <div class="hidden sm:flex flex-col gap-1">
                            <button onclick="moveEx(${dIdx}, ${eIdx}, -1)" class="text-[10px] opacity-70 hover:opacity-100"><i class="fas fa-chevron-up"></i></button>
                            <button onclick="moveEx(${dIdx}, ${eIdx}, 1)" class="text-[10px] opacity-70 hover:opacity-100"><i class="fas fa-chevron-down"></i></button>
                        </div>
                        <div class="flex-1 w-full sm:w-auto">
                            <div class="text-[9px] opacity-60">${ex.category}</div>
                            <div class="text-xs font-bold">${ex.name}</div>
                        </div>
                        <div class="flex gap-1 w-full sm:w-auto">
                            <input type="text" value="${ex.sets}" onchange="updateEx(${dIdx}, ${eIdx}, 'sets', this.value)" class="input-field w-12 rounded px-1 py-1 text-xs text-center" placeholder="Séries">
                            <input type="text" value="${ex.reps}" onchange="updateEx(${dIdx}, ${eIdx}, 'reps', this.value)" class="input-field w-16 rounded px-1 py-1 text-xs text-center" placeholder="Reps">
                            <select onchange="updateEx(${dIdx}, ${eIdx}, 'technique', this.value)" class="input-field w-24 rounded px-1 py-1 text-[10px]">
                                ${techniques.map(t => `<option value="${t}" ${ex.technique === t ? 'selected':''}>${t}</option>`).join('')}
                            </select>
                            <input type="text" value="${ex.obs}" onchange="updateEx(${dIdx}, ${eIdx}, 'obs', this.value)" class="input-field flex-1 sm:w-32 rounded px-1 py-1 text-xs" placeholder="Obs">
                            <button onclick="delEx(${dIdx}, ${eIdx})" class="text-red-500 hover:text-white hover:bg-red-500 px-2 rounded"><i class="fas fa-trash"></i></button>
                        </div>
                    </div>
                `).join('');

                return `
                <div class="card rounded-xl overflow-hidden shadow-sm">
                    <div class="p-2 border-b bg-black bg-opacity-10 flex justify-between items-center" style="border-color: var(--border-color)">
                        <input type="text" value="${day.title}" onchange="updateDayTitle(${dIdx}, this.value)" class="input-field bg-transparent font-bold text-sm w-1/2 px-1">
                        <button onclick="deleteDay(${dIdx})" class="text-xs text-red-500 p-1 hover:bg-red-500 hover:text-white rounded transition"><i class="fas fa-trash"></i></button>
                    </div>
                    <div class="p-1">${exHtml}</div>
                    <div class="p-2 border-t" style="border-color: var(--border-color)">
                        <button onclick="openExerciseModal(${dIdx})" class="w-full text-primary py-1 rounded text-xs font-bold hover:bg-primary hover:bg-opacity-10">
                            + ADICIONAR EXERCÍCIO
                        </button>
                    </div>
                </div>
                `
            }).join('');
        }

        window.moveEx = (dIdx, eIdx, dir) => {
            const arr = state.workoutDays[dIdx].exercises;
            if(eIdx + dir < 0 || eIdx + dir >= arr.length) return;
            const temp = arr[eIdx];
            arr[eIdx] = arr[eIdx + dir];
            arr[eIdx + dir] = temp;
            renderWorkoutDays();
        };
        window.updateEx = (dIdx, eIdx, f, v) => { state.workoutDays[dIdx].exercises[eIdx][f] = v; };
        window.delEx = (dIdx, eIdx) => { state.workoutDays[dIdx].exercises.splice(eIdx, 1); renderWorkoutDays(); };

        // --- EXERCÍCIOS MODAL & CUSTOM DB ---
        function getMergedCategories() {
            let merged = JSON.parse(JSON.stringify(defaultCategories));
            state.customExercises.forEach(cx => {
                if(!merged[cx.category]) merged[cx.category] = [];
                merged[cx.category].push(cx.name);
            });
            return merged;
        }

        window.openExerciseModal = (dIdx) => {
            state.activeModalDayIdx = dIdx;
            document.getElementById('exercise-modal').classList.remove('hidden');
            renderModalCats();
            renderModalExs();
        };
        window.closeExerciseModal = () => { document.getElementById('exercise-modal').classList.add('hidden'); };

        function renderModalCats() {
            const merged = getMergedCategories();
            document.getElementById('modal-categories').innerHTML = Object.keys(merged).map(cat => `
                <button onclick="setModalCat('${cat}')" class="w-full text-left px-2 py-2 rounded text-xs font-bold ${state.activeCategory === cat ? 'bg-primary text-white' : 'hover:bg-white hover:bg-opacity-10'}">${cat}</button>
            `).join('');
        }
        window.setModalCat = (c) => { state.activeCategory = c; renderModalCats(); renderModalExs(); };
        
        function renderModalExs() {
            const merged = getMergedCategories();
            const exs = merged[state.activeCategory] || [];
            document.getElementById('modal-exercises').innerHTML = `<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-2">` + exs.map(ex => {
                const isCardio = state.activeCategory === "🫀 CARDIO";
                return `<button onclick="addExToDay('${ex}', ${isCardio})" class="card p-2 rounded text-left text-xs font-medium border hover:border-primary transition group flex justify-between items-center">
                    ${ex} <i class="fas fa-plus text-primary opacity-0 group-hover:opacity-100"></i>
                </button>`;
            }).join('') + `</div>`;
        }
        window.addExToDay = (name, isCardio) => {
            state.workoutDays[state.activeModalDayIdx].exercises.push({
                category: state.activeCategory, name: name,
                sets: isCardio ? '1' : '3', reps: isCardio ? '-' : '10 a 12',
                technique: 'Nenhuma', obs: ''
            });
            renderWorkoutDays();
            showToast("Adicionado", "success");
        };

        window.addCustomExercise = async () => {
            const cat = document.getElementById('custom-ex-cat').value;
            const name = document.getElementById('custom-ex-name').value.trim();
            if(!name) return;
            try {
                const docRef = await addDoc(collection(db, getBasePath(), 'customExercises'), { category: cat, name: name });
                state.customExercises.push({ id: docRef.id, category: cat, name: name });
                document.getElementById('custom-ex-name').value = '';
                showToast("Exercício Salvo!");
                renderCustomExercisesList();
            } catch(e) { showToast("Erro", "error"); }
        };

        function renderCustomExercisesList() {
            document.getElementById('custom-ex-list').innerHTML = state.customExercises.map(ex => `
                <div class="flex justify-between items-center p-2 border-b border-opacity-20" style="border-color: var(--border-color)">
                    <div><span class="text-[9px] opacity-70">${ex.category}</span><br>${ex.name}</div>
                    <button onclick="deleteEntity('customExercises', '${ex.id}')" class="text-red-500 hover:text-white hover:bg-red-500 p-1 rounded"><i class="fas fa-trash"></i></button>
                </div>
            `).join('');
        }

        // --- SALVAR & IMPRIMIR ---
        window.saveAndPrintWorkout = async () => {
            const memId = document.getElementById('wf-member').value;
            if(!memId) return showToast("Selecione um Profissional (Membro)!", "error");
            
            const mem = state.members.find(m => m.id === memId);
            const wId = state.currentFichaId || generateId();
            
            const data = {
                memberId: memId,
                unitId: mem.unitId,
                profName: mem.name,
                profCat: mem.category,
                profCref: mem.cref,
                profState: mem.state,
                studentName: document.getElementById('stu-name').value || 'Sem Nome',
                stuAge: document.getElementById('stu-age').value,
                stuWeight: document.getElementById('stu-weight').value,
                stuHeight: document.getElementById('stu-height').value,
                stuGender: document.getElementById('stu-gender').value,
                stuLevel: document.getElementById('stu-level').value,
                stuObjective: document.getElementById('stu-objective').value,
                stuFreq: document.getElementById('stu-freq').value,
                stuValidity: parseInt(document.getElementById('stu-validity').value),
                recs: document.getElementById('stu-recs').value,
                health: Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value),
                days: state.workoutDays,
                createdAt: state.currentFichaId ? state.workouts.find(w=>w.id===wId).createdAt : Date.now()
            };

            // Save to Firebase
            try {
                await setDoc(doc(db, getBasePath(), 'workouts', wId), data);
                if(!state.currentFichaId) { state.workouts.push({id: wId, ...data}); state.currentFichaId = wId; }
                else { const idx = state.workouts.findIndex(w=>w.id===wId); state.workouts[idx] = {id: wId, ...data}; }
                showToast("Ficha salva na nuvem!");
                renderHistory();
                execPrint(data);
            } catch(e) { console.error(e); showToast("Erro ao salvar", "error"); }
        };

        function execPrint(d) {
            let imcStr = "-";
            const w = parseFloat(d.stuWeight); const h = parseFloat(d.stuHeight);
            if(w>0 && h>0) imcStr = (w / (h * h)).toFixed(1);

            const isPEF = d.profCat === 'PEF';
            const profLabel = isPEF ? 'Profissional de Educação Física' : 'Treinador Esportivo';
            const crefText = isPEF ? `CREF: ${d.profCref} - UF: ${d.profState}` : `UF: ${d.profState}`;
            const healthSource = isPEF ? healthPEF : healthTE;
            const objText = objectiveData[d.stuObjective] || "";

            const legalPEF = `⚠️ OBSERVAÇÃO LEGAL – PROFISSIONAL DE EDUCAÇÃO FÍSICA
Conforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º da mesma lei estabelece que compete ao profissional de Educação Física coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos especializados nas áreas de atividades físicas e do desporto.`;

            const legalTE = `⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO
Conforme a Lei nº 14.597/2023 (Lei Geral do Esporte), Art. 75, a profissão de treinador esportivo é reconhecida e regulada no Brasil, com atuação de caráter técnico e esportivo voltada à preparação, supervisão, orientação e acompanhamento de treinos físicos e esportivos. A atuação possui finalidade orientativa e não substitui avaliação médica, diagnóstico clínico, prescrição medicamentosa ou acompanhamento de profissionais da saúde. Em casos de doenças, lesões, gestação, limitações físicas, uso de medicações ou qualquer condição específica de saúde, recomenda-se avaliação prévia por profissional habilitado antes do início ou continuidade da prática de exercícios. O treinamento proposto respeita os limites individuais, priorizando segurança, execução correta e evolução progressiva, dentro da atuação técnica e esportiva permitida por lei. As recomendações desta ficha possuem caráter informativo e orientativo, voltadas ao treinamento esportivo e prática de musculação.`;

            let html = `
                <div class="print-header">
                    <h1 class="print-title">Planilha de Treinamento - ${d.stuObjective}</h1>
                    <div class="prof-info">Prescrição feita por: ${d.profName} (${profLabel})<br>${crefText}</div>
                </div>
                <div class="print-grid">
                    <div><strong>Aluno(a):</strong> ${d.studentName}</div>
                    <div><strong>Idade:</strong> ${d.stuAge||'-'} | <strong>Gênero:</strong> ${d.stuGender}</div>
                    <div><strong>Peso:</strong> ${d.stuWeight||'-'}kg | <strong>Alt:</strong> ${d.stuHeight||'-'}m | <strong>IMC:</strong> ${imcStr}</div>
                    <div><strong>Nível:</strong> ${d.stuLevel}</div>
                    <div><strong>Frequência:</strong> ${d.stuFreq}</div>
                    <div><strong>Validade:</strong> ${d.stuValidity} dias</div>
                </div>
            `;

            if (objText || d.health.length > 0) {
                html += `<div class="print-guidelines"><h4>Diretrizes Automáticas de Perfil</h4><ul>`;
                if(objText) html += `<li><strong>Objetivo (${d.stuObjective}):</strong> ${objText}</li>`;
                d.health.forEach(k => {
                    if(healthSource[k]) {
                        const clean = k.replace(/[\u{1F300}-\u{1F9FF}\u{2600}-\u{26FF}\u{2700}-\u{27BF}\u{FE0F}]/gu, '').trim();
                        html += `<li><strong>Saúde (${clean}):</strong> ${healthSource[k]}</li>`;
                    }
                });
                html += `</ul></div>`;
            }

            d.days.forEach(day => {
                html += `<div class="print-workout"><h3>${day.title}</h3><table><thead><tr>
                    <th style="width:35%">Exercício</th><th style="width:10%;text-align:center">Séries</th><th style="width:15%;text-align:center">Reps</th><th style="width:15%">Técnica</th><th style="width:25%">Observações</th>
                </tr></thead><tbody>`;
                if(day.exercises.length === 0) html += `<tr><td colspan="5" style="text-align:center">Sem exercícios</td></tr>`;
                else {
                    day.exercises.forEach(ex => {
                        html += `<tr><td><strong>${ex.name}</strong></td><td style="text-align:center">${ex.sets}</td><td style="text-align:center">${ex.reps}</td><td>${ex.technique==='Nenhuma'?'-':ex.technique}</td><td>${ex.obs||'-'}</td></tr>`;
                    });
                }
                html += `</tbody></table></div>`;
            });

            html += `<div class="print-footer-section">`;
            if(d.recs.trim()) html += `<strong>Recomendações Manuais:</strong><p style="white-space:pre-wrap; margin:3px 0 10px 0;">${d.recs}</p>`;
            html += `<div class="legal-text">${isPEF ? legalPEF : legalTE}</div>
                <div style="text-align:center; margin-top:10px; font-size:7px;">Gerado em ${new Date().toLocaleDateString('pt-BR')} - PowFit Pro Master</div>
            </div>`;

            document.getElementById('print-area').innerHTML = html;
            document.getElementById('print-area').classList.add('active-print');
            document.getElementById('print-report-area').classList.remove('active-print');
            setTimeout(() => { window.print(); }, 300);
        }

        // --- HISTÓRICO ---
        function renderHistory() {
            const tbody = document.getElementById('history-tbody');
            const now = Date.now();
            const sorted = [...state.workouts].sort((a,b) => b.createdAt - a.createdAt);

            tbody.innerHTML = sorted.map(w => {
                const dateObj = new Date(w.createdAt);
                const expDate = w.createdAt + (w.stuValidity * 24 * 60 * 60 * 1000);
                const isExp = now > expDate;
                const statusHtml = isExp ? `<span class="bg-red-500 bg-opacity-20 text-red-500 px-2 py-1 rounded text-[10px] font-bold">EXPIRADA</span>` : `<span class="bg-green-500 bg-opacity-20 text-green-500 px-2 py-1 rounded text-[10px] font-bold">VÁLIDA</span>`;
                const uName = state.units.find(u=>u.id===w.unitId)?.name || 'N/A';

                return `<tr class="border-b border-opacity-10" style="border-color:var(--border-color)">
                    <td class="p-2">${dateObj.toLocaleDateString('pt-BR')}</td>
                    <td class="p-2 font-bold">${w.studentName}</td>
                    <td class="p-2">${w.profName} <span class="text-[9px] opacity-70">(${w.profCat})</span></td>
                    <td class="p-2 text-xs">${uName}</td>
                    <td class="p-2">${statusHtml}</td>
                    <td class="p-2 flex gap-1">
                        <button onclick="loadWorkout('${w.id}')" class="btn-primary px-2 py-1 rounded text-xs">Carregar</button>
                        <button onclick="deleteEntity('workouts', '${w.id}')" class="bg-red-500 text-white px-2 py-1 rounded text-xs"><i class="fas fa-trash"></i></button>
                    </td>
                </tr>`;
            }).join('');
        }

        window.loadWorkout = (id) => {
            const w = state.workouts.find(x => x.id === id);
            if(!w) return;
            state.currentFichaId = w.id;
            
            document.getElementById('wf-member').value = w.memberId;
            window.onMemberSelectChange();

            document.getElementById('stu-name').value = w.studentName;
            document.getElementById('stu-age').value = w.stuAge;
            document.getElementById('stu-weight').value = w.stuWeight;
            document.getElementById('stu-height').value = w.stuHeight;
            document.getElementById('stu-gender').value = w.stuGender;
            document.getElementById('stu-level').value = w.stuLevel;
            document.getElementById('stu-objective').value = w.stuObjective;
            document.getElementById('stu-freq').value = w.stuFreq;
            document.getElementById('stu-validity').value = w.stuValidity;
            document.getElementById('stu-recs').value = w.recs;
            
            window.changeTheme();
            window.calculateIMC();
            window.updateObjectiveText();

            document.querySelectorAll('.health-cb').forEach(cb => { cb.checked = (w.health || []).includes(cb.value); });
            
            state.workoutDays = JSON.parse(JSON.stringify(w.days));
            renderWorkoutDays();
            nav('nova-ficha');
            showToast("Ficha carregada");
        };

        // --- RELATÓRIOS DE PRODUTIVIDADE ---
        function populateReportUnits() {
            document.getElementById('report-unit').innerHTML = `<option value="ALL">Todas as Unidades</option>` + state.units.map(u => `<option value="${u.id}">${u.name}</option>`).join('');
        }

        window.generateReport = () => {
            const m = document.getElementById('report-month').value;
            const y = document.getElementById('report-year').value;
            const uId = document.getElementById('report-unit').value;
            
            let filtered = state.workouts.filter(w => {
                const d = new Date(w.createdAt);
                const monthMatch = String(d.getMonth() + 1).padStart(2,'0') === m;
                const yearMatch = String(d.getFullYear()) === y;
                const unitMatch = uId === 'ALL' || w.unitId === uId;
                return monthMatch && yearMatch && unitMatch;
            });

            // Count per member
            const counts = {};
            filtered.forEach(w => {
                const key = `${w.unitId}_${w.memberId}`;
                if(!counts[key]) counts[key] = { unitId: w.unitId, memId: w.memberId, profName: w.profName, count: 0 };
                counts[key].count++;
            });

            const tbody = document.getElementById('report-tbody');
            const sortedKeys = Object.keys(counts).sort((a,b) => counts[b].count - counts[a].count);
            
            if(sortedKeys.length === 0) {
                tbody.innerHTML = `<tr><td colspan="3" class="text-center opacity-50 p-4">Nenhuma ficha encontrada neste período.</td></tr>`;
                return;
            }

            tbody.innerHTML = sortedKeys.map(k => {
                const c = counts[k];
                const uName = state.units.find(u=>u.id===c.unitId)?.name || 'N/A';
                return `<tr class="border-b border-opacity-10" style="border-color:var(--border-color)">
                    <td class="p-2">${uName}</td>
                    <td class="p-2 font-bold">${c.profName}</td>
                    <td class="p-2 text-center text-lg font-bold text-primary">${c.count}</td>
                </tr>`;
            }).join('');
        };

        window.printReport = () => {
            const m = document.getElementById('report-month').options[document.getElementById('report-month').selectedIndex].text;
            const y = document.getElementById('report-year').value;
            const html = `
                <h1>Relatório de Produtividade Mensal - ${m}/${y}</h1>
                <table>
                    <thead><tr><th>Unidade</th><th>Profissional</th><th style="text-align:center;">Fichas Emitidas</th></tr></thead>
                    <tbody>${document.getElementById('report-tbody').innerHTML}</tbody>
                </table>
                <p style="text-align:right; font-size:10px; margin-top:20px;">PowFit Pro Master Dashboard</p>
            `;
            document.getElementById('print-report-area').innerHTML = html;
            document.getElementById('print-area').classList.remove('active-print');
            document.getElementById('print-report-area').classList.add('active-print');
            setTimeout(() => window.print(), 300);
        };

    </script>
</body>
</html>
