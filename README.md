<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro - Plataforma Profissional</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Variáveis CSS para Tematização Dinâmica */
        :root {
            --bg-color: #111827; /* Dark Gray */
            --panel-bg: #1f2937;
            --text-main: #f9fafb;
            --text-muted: #9ca3af;
            --accent: #3b82f6; /* Blue */
            --accent-hover: #2563eb;
            --border-color: #374151;
            --input-bg: #374151;
            --badge-expired-bg: #ef4444;
            --badge-active-bg: #10b981;
        }

        /* Tema Feminino (Rosa Elegante) */
        body.theme-female {
            --bg-color: #fdf2f8; /* Pink 50 */
            --panel-bg: #ffffff;
            --text-main: #1f2937;
            --text-muted: #6b7280;
            --accent: #ec4899; /* Pink 500 */
            --accent-hover: #db2777;
            --border-color: #e5e7eb;
            --input-bg: #f9fafb;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-main);
            transition: background-color 0.3s, color 0.3s;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        .panel { background-color: var(--panel-bg); border-color: var(--border-color); }
        .input-field { 
            background-color: var(--input-bg); 
            color: var(--text-main); 
            border-color: var(--border-color); 
        }
        .btn-primary { background-color: var(--accent); color: white; transition: background-color 0.2s; }
        .btn-primary:hover { background-color: var(--accent-hover); }
        .text-accent { color: var(--accent); }
        .border-accent { border-color: var(--accent); }

        /* Estilos de Impressão */
        @media print {
            body { background: white; color: black; font-size: 11px; }
            #app-ui, #login-ui { display: none !important; }
            #print-ui { display: block !important; }
            .print-page-break { page-break-before: always; }
            .avoid-break { page-break-inside: avoid; }
            table { width: 100%; border-collapse: collapse; margin-bottom: 0.5rem; }
            th, td { border: 1px solid #d1d5db; padding: 4px; text-align: left; }
            th { background-color: #f3f4f6; -webkit-print-color-adjust: exact; color: #111827; }
            h1, h2, h3 { color: #111827; margin-bottom: 0.25rem; }
            .header-banner { background-color: #111827; color: white; padding: 8px; text-align: center; -webkit-print-color-adjust: exact; }
            .legal-footer { font-size: 9px; color: #4b5563; margin-top: 15px; border-top: 1px solid #d1d5db; padding-top: 5px; text-align: justify; }
        }
        
        #print-ui { display: none; }
        
        /* Custom Scrollbar */
        ::-webkit-scrollbar { width: 8px; height: 8px; }
        ::-webkit-scrollbar-track { background: var(--bg-color); }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: var(--accent); }
    </style>
</head>
<body class="min-h-screen">

    <!-- LOGIN UI -->
    <div id="login-ui" class="flex items-center justify-center min-h-screen flex-col p-4">
        <div class="panel p-8 rounded-2xl shadow-2xl max-w-md w-full text-center border">
            <h1 class="text-5xl font-black italic mb-2 tracking-tighter text-accent">POWFIT PRO</h1>
            <p class="text-muted mb-8 text-sm">Plataforma Profissional de Prescrição Manual</p>
            <button id="btn-login" class="btn-primary w-full py-3 rounded-lg font-bold flex items-center justify-center gap-2">
                <svg class="w-5 h-5" viewBox="0 0 24 24" fill="currentColor"><path d="M12.545,10.239v3.821h5.445c-0.712,2.315-2.647,3.972-5.445,3.972c-3.332,0-6.033-2.701-6.033-6.032s2.701-6.032,6.033-6.032c1.498,0,2.866,0.549,3.921,1.453l2.814-2.814C17.503,2.988,15.139,2,12.545,2C7.021,2,2.543,6.477,2.543,12s4.478,10,10.002,10c8.396,0,10.249-7.85,9.426-11.761H12.545z"/></svg>
                Entrar com Google
            </button>
            <p id="login-error" class="text-red-500 mt-4 text-sm hidden"></p>
        </div>
    </div>

    <!-- MAIN APP UI -->
    <div id="app-ui" class="hidden flex flex-col md:flex-row min-h-screen">
        <!-- Sidebar -->
        <aside class="panel w-full md:w-64 border-r md:min-h-screen flex flex-col shrink-0">
            <div class="p-4 border-b border-color">
                <h1 class="text-3xl font-black italic tracking-tighter text-accent">POWFIT PRO</h1>
                <p id="user-email-display" class="text-xs text-muted mt-1 truncate"></p>
            </div>
            <nav class="flex-1 p-4 flex flex-col gap-2">
                <button onclick="switchView('builder')" class="text-left px-4 py-3 rounded-lg hover:bg-black/10 dark:hover:bg-white/10 font-medium transition-colors flex items-center gap-2" id="nav-builder">🏋️ Montar Treino</button>
                <button onclick="switchView('history')" class="text-left px-4 py-3 rounded-lg hover:bg-black/10 dark:hover:bg-white/10 font-medium transition-colors flex items-center gap-2" id="nav-history">📂 Histórico e Edição</button>
                <button onclick="switchView('dashboard')" class="text-left px-4 py-3 rounded-lg hover:bg-black/10 dark:hover:bg-white/10 font-medium transition-colors flex items-center gap-2" id="nav-dashboard">🏢 Gestão da Rede</button>
                <button onclick="switchView('reports')" class="text-left px-4 py-3 rounded-lg hover:bg-black/10 dark:hover:bg-white/10 font-medium transition-colors flex items-center gap-2" id="nav-reports">📊 Produtividade</button>
            </nav>
            <div class="p-4 border-t border-color">
                <button id="btn-logout" class="text-sm text-red-500 hover:text-red-400 font-bold w-full text-left">🚪 Sair do Sistema</button>
            </div>
        </aside>

        <!-- Main Content -->
        <main class="flex-1 p-4 md:p-8 overflow-y-auto h-screen relative">
            
            <!-- ALERTS -->
            <div id="system-alert" class="hidden absolute top-4 right-4 bg-green-500 text-white px-4 py-2 rounded shadow-lg z-50 font-bold transition-opacity">
                Mensagem
            </div>

            <!-- VIEW: BUILDER (MONTAGEM) -->
            <div id="view-builder" class="hidden space-y-6 max-w-6xl mx-auto">
                <div class="flex flex-col md:flex-row justify-between items-start md:items-end gap-4 border-b border-color pb-4">
                    <div>
                        <h2 class="text-3xl font-bold mb-1" id="builder-title">Nova Ficha de Treino</h2>
                        <p class="text-muted text-sm">Prancheta digital profissional. Você no controle.</p>
                    </div>
                    <div class="flex gap-2">
                        <button onclick="clearBuilder()" class="px-4 py-2 rounded font-bold border border-color hover:bg-black/10 text-sm">🧹 Limpar</button>
                        <button onclick="saveRoutine(false)" class="bg-gray-600 hover:bg-gray-500 text-white px-4 py-2 rounded font-bold text-sm shadow">💾 Apenas Salvar</button>
                        <button onclick="saveRoutine(true)" class="btn-primary px-6 py-2 rounded font-bold shadow-lg flex items-center gap-2 text-base">🖨️ Salvar e Imprimir</button>
                    </div>
                </div>

                <!-- Responsável -->
                <div class="panel p-5 rounded-xl border border-accent border-l-4 shadow-sm">
                    <h3 class="font-bold mb-3 text-sm tracking-wide uppercase text-accent">1. Profissional Responsável</h3>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div>
                            <label class="block text-xs font-bold mb-1">Unidade de Atendimento</label>
                            <select id="builder-unit" class="input-field w-full p-2.5 rounded border" onchange="updateBuilderMembers()"></select>
                        </div>
                        <div>
                            <label class="block text-xs font-bold mb-1">Perfil do Treinador</label>
                            <select id="builder-member" class="input-field w-full p-2.5 rounded border"></select>
                        </div>
                    </div>
                </div>

                <!-- Dados do Aluno -->
                <div class="panel p-5 rounded-xl border shadow-sm">
                    <h3 class="font-bold mb-3 text-sm tracking-wide uppercase text-accent">2. Dados do Aluno</h3>
                    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-5 gap-4 mb-4">
                        <div class="md:col-span-2">
                            <label class="block text-xs font-bold mb-1">Nome Completo</label>
                            <input type="text" id="client-name" class="input-field w-full p-2.5 rounded border" placeholder="Ex: João Silva">
                        </div>
                        <div>
                            <label class="block text-xs font-bold mb-1">Gênero</label>
                            <select id="client-gender" class="input-field w-full p-2.5 rounded border" onchange="toggleTheme()">
                                <option value="Masculino">Masculino</option>
                                <option value="Feminino">Feminino</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-xs font-bold mb-1">Idade</label>
                            <input type="number" id="client-age" class="input-field w-full p-2.5 rounded border">
                        </div>
                        <div>
                            <label class="block text-xs font-bold mb-1">Nível</label>
                            <select id="client-level" class="input-field w-full p-2.5 rounded border">
                                <option>Iniciante</option><option>Intermediário</option><option>Avançado</option>
                            </select>
                        </div>
                    </div>
                    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-5 gap-4">
                        <div>
                            <label class="block text-xs font-bold mb-1">Peso (kg)</label>
                            <input type="number" step="0.1" id="client-weight" class="input-field w-full p-2.5 rounded border" oninput="calculateBMI()">
                        </div>
                        <div>
                            <label class="block text-xs font-bold mb-1">Altura (m)</label>
                            <input type="number" step="0.01" id="client-height" class="input-field w-full p-2.5 rounded border" oninput="calculateBMI()">
                        </div>
                        <div class="flex flex-col justify-center bg-black/5 dark:bg-white/5 rounded p-2 border border-color">
                            <span class="text-xs text-muted mb-1">Situação (IMC)</span>
                            <span class="font-bold text-sm" id="bmi-result">--</span>
                        </div>
                        <div>
                            <label class="block text-xs font-bold mb-1">Frequência</label>
                            <select id="client-freq" class="input-field w-full p-2.5 rounded border">
                                <option>3 dias</option><option>5 dias</option><option>6 dias</option><option>Personalizado</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-xs font-bold mb-1">Objetivo</label>
                            <select id="client-obj" class="input-field w-full p-2.5 rounded border">
                                <option>Emagrecimento</option><option>Hipertrofia</option><option>Definição</option>
                                <option>Condicionamento</option><option>Resistência</option><option>Força</option>
                                <option>Reabilitação</option><option>Saúde geral</option>
                            </select>
                        </div>
                    </div>
                </div>

                <!-- Estado de Saúde -->
                <div class="panel p-5 rounded-xl border shadow-sm">
                    <h3 class="font-bold mb-3 text-sm tracking-wide uppercase text-accent">3. Estado de Saúde (Múltipla Seleção)</h3>
                    <div class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-5 gap-3 text-sm" id="health-status-container">
                        <!-- Gerado via JS -->
                    </div>
                </div>

                <!-- Montagem do Treino -->
                <div class="panel p-5 rounded-xl border shadow-sm">
                    <div class="flex justify-between items-center mb-4 flex-wrap gap-2">
                        <h3 class="font-bold text-sm tracking-wide uppercase text-accent">4. Montagem da Ficha</h3>
                        <div class="flex items-center gap-3">
                            <label class="text-sm font-bold">Validade:</label>
                            <select id="routine-validity" class="input-field p-2 rounded border text-sm font-bold">
                                <option value="15">15 dias</option>
                                <option value="30" selected>30 dias</option>
                                <option value="60">60 dias</option>
                                <option value="90">90 dias</option>
                            </select>
                        </div>
                    </div>

                    <!-- Tabs Header -->
                    <div class="flex gap-1 border-b border-color mb-4 overflow-x-auto pb-1" id="tabs-header">
                        <!-- Gerado via JS -->
                    </div>
                    
                    <div class="mb-4 flex flex-wrap gap-3 items-center">
                        <button onclick="addTab()" class="text-sm font-bold text-white bg-accent px-3 py-1 rounded hover:bg-accent-hover shadow">+ Nova Aba</button>
                        <button onclick="removeCurrentTab()" class="text-sm font-bold text-red-500 hover:text-red-400">🗑️ Remover Aba Atual</button>
                    </div>

                    <!-- Tabs Content -->
                    <div id="tabs-content" class="overflow-x-auto pb-4">
                        <!-- Tabela ativa gerada via JS -->
                    </div>
                    
                    <div class="mt-2 flex flex-wrap gap-3">
                        <button onclick="addExerciseRow()" class="btn-primary px-4 py-2 rounded font-bold text-sm shadow">+ Adicionar Exercício na Tabela</button>
                        <button onclick="toggleModal('modal-custom-ex')" class="bg-gray-600 hover:bg-gray-500 text-white px-4 py-2 rounded font-bold text-sm shadow">⚙️ Banco de Exercícios Customizados</button>
                    </div>
                </div>

                <!-- Recomendações -->
                <div class="panel p-5 rounded-xl border shadow-sm">
                    <h3 class="font-bold mb-3 text-sm tracking-wide uppercase text-accent">5. Recomendações Profissionais Livres</h3>
                    <textarea id="routine-obs" class="input-field w-full p-3 rounded border h-24 text-sm" placeholder="Escreva observações sobre hidratação, cadência, descanso, foco mental..."></textarea>
                </div>
            </div>

            <!-- VIEW: HISTÓRICO -->
            <div id="view-history" class="hidden space-y-6 max-w-6xl mx-auto">
                <div class="border-b border-color pb-4">
                    <h2 class="text-3xl font-bold mb-1">Histórico de Fichas</h2>
                    <p class="text-muted text-sm">Acesse, edite ou exclua treinos criados. Fichas na nuvem.</p>
                </div>

                <div class="panel p-5 rounded-xl border shadow-sm">
                    <div class="flex flex-col sm:flex-row gap-4 mb-6">
                        <input type="text" id="filter-history-name" class="input-field flex-1 p-2.5 rounded border text-sm" placeholder="Buscar por nome do aluno..." oninput="renderHistory()">
                        <select id="filter-history-status" class="input-field p-2.5 rounded border text-sm w-full sm:w-48" onchange="renderHistory()">
                            <option value="">Todos os status</option>
                            <option value="active">🟢 Ativas</option>
                            <option value="expired">🔴 Expiradas</option>
                        </select>
                    </div>
                    
                    <div class="overflow-x-auto">
                        <table class="w-full text-sm text-left">
                            <thead class="border-b border-color">
                                <tr>
                                    <th class="p-3">Aluno</th>
                                    <th class="p-3">Treinador</th>
                                    <th class="p-3">Criada em</th>
                                    <th class="p-3">Validade</th>
                                    <th class="p-3">Status</th>
                                    <th class="p-3 text-right">Ações</th>
                                </tr>
                            </thead>
                            <tbody id="history-list">
                                <!-- Gerado via JS -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- VIEW: DASHBOARD (REDE) -->
            <div id="view-dashboard" class="hidden space-y-6 max-w-6xl mx-auto">
                <div class="border-b border-color pb-4">
                    <h2 class="text-3xl font-bold mb-1">Gestão da Rede e Equipe</h2>
                    <p class="text-muted text-sm">Configure as franquias e cadastre os treinadores.</p>
                </div>
                
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                    <!-- Unidades -->
                    <div class="panel p-6 rounded-xl border shadow-sm">
                        <h3 class="text-xl font-bold mb-4 flex justify-between items-center text-accent">
                            Unidades (Franquias)
                            <button onclick="toggleModal('modal-unit')" class="btn-primary text-sm px-3 py-1.5 rounded font-bold shadow">+ Nova Unidade</button>
                        </h3>
                        <div id="units-list" class="space-y-2 max-h-80 overflow-y-auto pr-2"></div>
                    </div>

                    <!-- Membros -->
                    <div class="panel p-6 rounded-xl border shadow-sm">
                        <h3 class="text-xl font-bold mb-4 flex justify-between items-center text-accent">
                            Equipe Técnica
                            <button onclick="toggleModal('modal-member')" class="btn-primary text-sm px-3 py-1.5 rounded font-bold shadow">+ Novo Membro</button>
                        </h3>
                        <select id="filter-unit-members" class="input-field w-full p-2.5 rounded mb-4 border text-sm" onchange="renderMembers()">
                            <option value="">Filtrar por unidade...</option>
                        </select>
                        <div id="members-list" class="space-y-2 max-h-72 overflow-y-auto pr-2"></div>
                    </div>
                </div>
            </div>

            <!-- VIEW: RELATORIOS -->
            <div id="view-reports" class="hidden space-y-6 max-w-6xl mx-auto">
                <div class="border-b border-color pb-4">
                    <h2 class="text-3xl font-bold mb-1">Relatório de Produtividade</h2>
                    <p class="text-muted text-sm">Controle quantitativo de fichas criadas.</p>
                </div>
                <div class="panel p-6 rounded-xl border shadow-sm">
                    <div class="flex flex-wrap gap-4 mb-6 bg-black/5 dark:bg-white/5 p-4 rounded-lg">
                        <div class="flex-1 min-w-[200px]">
                            <label class="block text-xs font-bold mb-1">Mês de Referência</label>
                            <input type="month" id="report-month" class="input-field w-full p-2.5 rounded border">
                        </div>
                        <div class="flex-1 min-w-[200px]">
                            <label class="block text-xs font-bold mb-1">Unidade</label>
                            <select id="report-unit" class="input-field w-full p-2.5 rounded border">
                                <option value="">Todas as unidades</option>
                            </select>
                        </div>
                        <div class="flex items-end gap-2">
                            <button onclick="generateReport()" class="btn-primary px-5 py-2.5 rounded font-bold shadow h-[42px]">Gerar</button>
                            <button onclick="printReport()" class="bg-gray-600 text-white px-5 py-2.5 rounded font-bold hover:bg-gray-500 shadow h-[42px]">🖨️ Imprimir</button>
                        </div>
                    </div>
                    <div id="report-results" class="space-y-4">
                        <p class="text-center text-muted p-8">Selecione os filtros e clique em Gerar.</p>
                    </div>
                </div>
            </div>

        </main>
    </div>

    <!-- MODAIS -->

    <!-- Modal: Adicionar Unidade -->
    <div id="modal-unit" class="hidden fixed inset-0 bg-black/60 backdrop-blur-sm flex items-center justify-center z-50 p-4">
        <div class="panel p-6 rounded-2xl border max-w-sm w-full shadow-2xl">
            <h3 class="text-xl font-bold mb-4 text-accent">Nova Unidade</h3>
            <input type="text" id="unit-name" class="input-field w-full p-3 rounded-lg border mb-5" placeholder="Ex: Power Fitness Centro">
            <div class="flex justify-end gap-3">
                <button onclick="toggleModal('modal-unit')" class="px-4 py-2 rounded font-bold hover:bg-black/10 dark:hover:bg-white/10">Cancelar</button>
                <button onclick="saveUnit()" class="btn-primary px-6 py-2 rounded font-bold shadow">Salvar</button>
            </div>
        </div>
    </div>

    <!-- Modal: Adicionar Membro -->
    <div id="modal-member" class="hidden fixed inset-0 bg-black/60 backdrop-blur-sm flex items-center justify-center z-50 p-4">
        <div class="panel p-6 rounded-2xl border max-w-md w-full shadow-2xl">
            <h3 class="text-xl font-bold mb-4 text-accent">Cadastrar Treinador</h3>
            <div class="space-y-4">
                <div>
                    <label class="block text-xs font-bold mb-1">Nome Completo</label>
                    <input type="text" id="mem-name" class="input-field w-full p-2.5 rounded border">
                </div>
                <div class="grid grid-cols-2 gap-4">
                    <div>
                        <label class="block text-xs font-bold mb-1">CPF</label>
                        <input type="text" id="mem-cpf" class="input-field w-full p-2.5 rounded border">
                    </div>
                    <div>
                        <label class="block text-xs font-bold mb-1">Estado (UF)</label>
                        <input type="text" id="mem-uf" class="input-field w-full p-2.5 rounded border" placeholder="Ex: SP" maxlength="2">
                    </div>
                </div>
                <div>
                    <label class="block text-xs font-bold mb-1">Unidade</label>
                    <select id="mem-unit" class="input-field w-full p-2.5 rounded border"></select>
                </div>
                <div>
                    <label class="block text-xs font-bold mb-1">Categoria Profissional</label>
                    <select id="mem-cat" class="input-field w-full p-2.5 rounded border" onchange="toggleCrefField()">
                        <option value="PEF">Profissional de Educação Física (PEF)</option>
                        <option value="TE">Treinador Esportivo (TE)</option>
                    </select>
                </div>
                <div id="cref-container">
                    <label class="block text-xs font-bold mb-1">CREF</label>
                    <input type="text" id="mem-cref" class="input-field w-full p-2.5 rounded border" placeholder="Ex: 000000-G/SP">
                </div>
            </div>
            <div class="flex justify-end gap-3 mt-6">
                <button onclick="toggleModal('modal-member')" class="px-4 py-2 rounded font-bold hover:bg-black/10 dark:hover:bg-white/10">Cancelar</button>
                <button onclick="saveMember()" class="btn-primary px-6 py-2 rounded font-bold shadow">Salvar</button>
            </div>
        </div>
    </div>

    <!-- Modal: Exercícios Customizados -->
    <div id="modal-custom-ex" class="hidden fixed inset-0 bg-black/60 backdrop-blur-sm flex items-center justify-center z-50 p-4">
        <div class="panel p-6 rounded-2xl border max-w-md w-full shadow-2xl flex flex-col max-h-[90vh]">
            <h3 class="text-xl font-bold mb-4 text-accent">Exercícios Customizados</h3>
            
            <div class="space-y-3 mb-6 bg-black/5 dark:bg-white/5 p-4 rounded-lg">
                <p class="text-xs font-bold mb-2">Adicionar Novo:</p>
                <select id="custom-ex-group" class="input-field w-full p-2.5 rounded border text-sm">
                    <option value="PEITO">Peito</option><option value="COSTAS">Costas</option>
                    <option value="PERNAS">Pernas</option><option value="BRAÇOS (BÍCEPS)">Braços (Bíceps)</option>
                    <option value="BRAÇOS (TRÍCEPS)">Braços (Tríceps)</option><option value="OMBROS">Ombros</option>
                    <option value="ABDÔMEN">Abdômen</option><option value="CARDIO">Cardio</option>
                </select>
                <input type="text" id="custom-ex-name" class="input-field w-full p-2.5 rounded border text-sm" placeholder="Nome do Exercício">
                <button onclick="saveCustomExercise()" class="btn-primary w-full py-2 rounded font-bold text-sm shadow">Adicionar ao Banco</button>
            </div>

            <p class="text-xs font-bold mb-2">Seus Exercícios Salvos:</p>
            <div id="custom-ex-list" class="flex-1 overflow-y-auto space-y-2 min-h-[100px] border border-color rounded-lg p-2">
                <!-- Gerado via JS -->
            </div>

            <div class="flex justify-end gap-3 mt-4">
                <button onclick="toggleModal('modal-custom-ex')" class="px-6 py-2 rounded border border-color font-bold hover:bg-black/10 dark:hover:bg-white/10">Fechar</button>
            </div>
        </div>
    </div>

    <!-- PRINT CONTAINER (Escondido na tela, visível na impressão) -->
    <div id="print-ui"></div>

    <!-- SCRIPT DA APLICAÇÃO -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInWithPopup, GoogleAuthProvider, signInWithCustomToken, signInAnonymously, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, doc, setDoc, getDoc, addDoc, updateDoc, onSnapshot, deleteDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Firebase Config Setup
        let firebaseConfig;
        try {
            firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : {
                apiKey: "AIzaSyD-iuERHA_x1f69smJXh7b8wTLO1zTadIU",
                authDomain: "powfitpro-d8577.firebaseapp.com",
                projectId: "powfitpro-d8577",
                storageBucket: "powfitpro-d8577.firebasestorage.app",
                messagingSenderId: "651381183985",
                appId: "1:651381183985:web:9a425692372a9d67d9e050"
            };
        } catch(e) {
            console.error("Erro na Configuração do Firebase:", e);
        }

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'powfit-pro-default';

        // Variáveis Globais de Estado
        let currentUser = null;
        let unsubscribes = [];
        let state = {
            units: [],
            members: [],
            customExercises: [],
            routines: [],
            workoutTabs: [{ id: 'A', name: 'Treino A', rows: [] }],
            activeTabId: 'A',
            editingRoutineId: null // Se preenchido, estamos editando uma ficha existente
        };

        // --- BANCO DE DADOS FIXO ---
        const EXERCISES_DB = {
            "PEITO": ["Supino Reto", "Supino Inclinado", "Supino Inclinado com Halteres", "Supino Fechado com Halteres", "Cross Over", "Cross Over Alto", "Cross Over Baixo", "Crucifixo Reto", "Crucifixo Inclinado com Halteres", "Crucifixo na Máquina", "Peck Fly", "Peck Fly Unilateral", "Pullover", "Flexão de Braço", "Flexão com Pés Elevados", "Flexão Explosiva"],
            "COSTAS": ["Puxada Alta", "Puxada de Frente Supinada", "Pulldown", "Remada Aberta", "Remada Baixa", "Remada Curvada", "Remada Curvada Supinada", "Remada Cavalinho (T-Bar)", "Serrote", "Facepull (puxada de cima para baixo)"],
            "PERNAS": ["Agachamento Livre", "Agachamento Taça", "Agachamento no Smith", "Agachamento com passada lateral", "Squat", "Hack Machine", "Leg 45°", "Leg 90°", "Agachamento Sumô", "Agachamento Sissy (Livre)", "Afundo", "Recuo", "Avanço", "Passada", "Búlgaro", "Step-up", "Levantamento Terra", "Levantamento Terra Romeno", "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", "Cadeira Flexora", "Elevação Pélvica no Banco", "Elevação Pélvica no Chão", "Elevação Pélvica Unilateral no Chão", "Extensão de Quadril (Glúteo Máximo)", "Extensão Cruzada (Glúteo Médio)", "Coice", "Cachorrinho", "Cadeira Extensora", "Adução", "Abdução", "Abdução Inclinada", "Flexão Nórdica", "Flexão Nórdica Invertida", "Panturrilha Livre", "Panturrilha no Leg Press", "Panturrilha Banco", "Panturrilha Squat", "Panturrilha Unilateral"],
            "BRAÇOS (BÍCEPS)": ["Rosca Direta", "Rosca Alternada", "Rosca 21", "Rosca Scott Barra W", "Rosca Scott Unilateral", "Rosca Scott com Halteres", "Rosca Martelo", "Rosca Cross", "Rosca Inversa", "Rosca 45°"],
            "BRAÇOS (TRÍCEPS)": ["Triceps Pulley Unilateral", "Tríceps Pulley Barra", "Tríceps Pulley Corda", "Tríceps Pulley Pegada Inversa", "Tríceps Francês na Corda", "Tríceps Francês com Halter", "Tríceps Francês Unilateral", "Tríceps Cruzado Polia Dupla", "Tríceps Coice Unilateral", "Tríceps Arremesso", "Tríceps Testa", "Mergulho no Banco"],
            "OMBROS": ["Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", "Elevação Lateral Unilateral Cross", "Elevação Lateral Sentado", "Desenvolvimento com Halteres", "Desenvolvimento com Barra", "Arnold Press", "Elevação Borboleta", "Crucifixo Inverso Sentado com Halteres", "Crucifixo Inverso na Polia", "Crucifixo Inverso Unilateral na Polia", "Facepull (puxada reta)", "Remada Alta", "Encolhimento (Trapézio)"],
            "ABDÔMEN": ["Infra com Elevação de Perna", "Abdominal Supra", "Abdominal Remador", "Abdominal Bicicleta", "Abdominal Twister com Peso", "Prancha", "Prancha Lateral", "Trituração de Cabos em Pé", "Isometria na parede"],
            "CARDIO": ["Bicicleta 10 Minutos", "Bicicleta 15 Minutos", "Bicicleta 20 Minutos", "Esteira 10 Minutos", "Esteira 15 Minutos", "Esteira 20 Minutos", "Pular Corda"]
        };

        const TECHNIQUES = ["Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", "Negativa", "Isometria", "Parciais", "Pirâmide"];
        const HEALTH_OPTIONS = ["🟢 Saudável", "⚪ Sedentário", "🟡 Sobrepeso", "🔴 Obesidade", "⚖️ Baixo peso", "🍬 Diabetes", "❤️ Hipertensão", "🔵 Hipotensão", "💔 Problemas cardíacos", "🦴 Problemas articulares", "🫁 Problemas respiratórios", "⚠️ Lesões", "🤰 Gestante", "🤱 Lactante", "👴 Idoso"];

        const TEXTS_OBJ = {
            "Emagrecimento": "Foco em déficit calórico com treinos mistos de força (manter massa magra) e aeróbicos (maior gasto).",
            "Hipertrofia": "Prioridade na progressão de carga e volume adequado. Essencial superávit calórico e descanso.",
            "Definição": "Manutenção de massa muscular enquanto reduz o percentual de gordura. Atenção estrita à dieta.",
            "Condicionamento": "Treinos com menor tempo de intervalo, circuitos e alta integração cardiopulmonar.",
            "Resistência": "Séries mais longas, cadência controlada e aprimoramento da capacidade muscular.",
            "Força": "Cargas altas, baixas repetições e intervalos de descanso maiores.",
            "Reabilitação": "Treino focado em fortalecimento específico, mobilidade e controle motor. Respeitar limites.",
            "Saúde geral": "Equilíbrio entre força, cardio e flexibilidade. O principal objetivo é a constância e bem-estar."
        };

        const TEXTS_HEALTH_PEF = {
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

        const TEXTS_HEALTH_TE = {
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

        const LEGAL_PEF = "⚠️ OBSERVAÇÃO LEGAL – PROFISSIONAL DE EDUCAÇÃO FÍSICA<br>Conforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º da mesma lei estabelece que compete ao profissional de Educação Física coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos especializados nas áreas de atividades físicas e do desporto.";
        const LEGAL_TE = "⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO<br>Conforme a Lei nº 14.597/2023 (Lei Geral do Esporte), Art. 75, a profissão de treinador esportivo é reconhecida e regulada no Brasil, com atuação de caráter técnico e esportivo voltada à preparação, supervisão, orientação e acompanhamento de treinos físicos e esportivos. A atuação possui finalidade orientativa e não substitui avaliação médica, diagnóstico clínico, prescrição medicamentosa ou acompanhamento de profissionais da saúde. Em casos de doenças, lesões, gestação, limitações físicas, uso de medicações ou qualquer condição específica de saúde, recomenda-se avaliação prévia por profissional habilitado antes do início ou continuidade da prática de exercícios. O treinamento proposto respeita os limites individuais, priorizando segurança, execução correta e evolução progressiva, dentro da atuação técnica e esportiva permitida por lei. As recomendações desta ficha possuem caráter informativo e orientativo, voltadas ao treinamento esportivo e prática de musculação.";

        // --- SISTEMA DE ALERTA ---
        function showAlert(msg, isError = false) {
            const el = document.getElementById('system-alert');
            el.innerText = msg;
            el.className = `absolute top-4 right-4 px-4 py-2 rounded shadow-lg z-50 font-bold transition-opacity duration-300 ${isError ? 'bg-red-500 text-white' : 'bg-green-500 text-white'}`;
            el.classList.remove('hidden');
            setTimeout(() => { el.classList.add('hidden'); }, 3000);
        }

        // --- AUTH LOGIC (MANDATORY RULE 3) ---
        document.getElementById('btn-login').addEventListener('click', async () => {
            const errEl = document.getElementById('login-error');
            try {
                if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                    await signInWithCustomToken(auth, __initial_auth_token);
                } else {
                    const provider = new GoogleAuthProvider();
                    await signInWithPopup(auth, provider);
                }
            } catch (error) {
                console.error("Login erro", error);
                errEl.innerText = "Erro no login Google. Tentando acesso anônimo...";
                errEl.classList.remove('hidden');
                try { await signInAnonymously(auth); } catch(e) {}
            }
        });

        document.getElementById('btn-logout').addEventListener('click', () => signOut(auth));

        onAuthStateChanged(auth, user => {
            currentUser = user;
            if (user) {
                document.getElementById('login-ui').classList.add('hidden');
                document.getElementById('app-ui').classList.remove('hidden');
                document.getElementById('user-email-display').innerText = user.email || 'Modo Anônimo';
                initDataListeners();
                switchView('builder');
                initHealthCheckboxes();
                clearBuilder();
            } else {
                document.getElementById('login-ui').classList.remove('hidden');
                document.getElementById('app-ui').classList.add('hidden');
                clearListeners();
            }
        });

        // --- FIREBASE DATABASE LOGIC (MANDATORY RULE 1) ---
        const getPath = (collName) => collection(db, 'artifacts', appId, 'users', currentUser.uid, collName);
        const getDocPath = (collName, docId) => doc(db, 'artifacts', appId, 'users', currentUser.uid, collName, docId);

        function initDataListeners() {
            if (!currentUser) return;
            clearListeners();
            
            // Listen Units
            unsubscribes.push(onSnapshot(getPath('units'), snap => {
                state.units = snap.docs.map(d => ({id: d.id, ...d.data()}));
                renderUnits();
                populateUnitSelects();
            }, e => console.error("Error Units:", e)));

            // Listen Members
            unsubscribes.push(onSnapshot(getPath('members'), snap => {
                state.members = snap.docs.map(d => ({id: d.id, ...d.data()}));
                renderMembers();
                updateBuilderMembers();
            }, e => console.error("Error Members:", e)));

            // Listen Custom Exercises
            unsubscribes.push(onSnapshot(getPath('custom_exercises'), snap => {
                state.customExercises = snap.docs.map(d => ({id: d.id, ...d.data()}));
                renderCustomExercisesList();
                if(state.activeTabId) renderTable();
            }, e => console.error("Error Custom Ex:", e)));

            // Listen Routines (History)
            unsubscribes.push(onSnapshot(getPath('routines'), snap => {
                state.routines = snap.docs.map(d => ({id: d.id, ...d.data()}));
                // Sort by newest first
                state.routines.sort((a,b) => new Date(b.createdAt) - new Date(a.createdAt));
                renderHistory();
            }, e => console.error("Error Routines:", e)));
        }

        function clearListeners() {
            unsubscribes.forEach(unsub => unsub());
            unsubscribes = [];
        }

        window.deleteDocItem = async function(collName, id) {
            if(confirm("Deseja realmente excluir este item? Esta ação não pode ser desfeita.")) {
                try {
                    await deleteDoc(getDocPath(collName, id));
                    showAlert("Excluído com sucesso.");
                } catch(e) {
                    showAlert("Erro ao excluir", true);
                }
            }
        }

        // --- UI NAVEGAÇÃO ---
        window.switchView = function(view) {
            ['dashboard', 'builder', 'reports', 'history'].forEach(v => {
                document.getElementById(`view-${v}`).classList.add('hidden');
                document.getElementById(`nav-${v}`).classList.remove('text-accent', 'bg-black/10', 'dark:bg-white/10');
            });
            
            document.getElementById(`view-${view}`).classList.remove('hidden');
            document.getElementById(`nav-${view}`).classList.add('text-accent', 'bg-black/10', 'dark:bg-white/10');

            if(view === 'builder') {
                renderTabs();
                renderTable();
            } else if (view === 'history') {
                renderHistory();
            }
        }

        window.toggleModal = function(id) {
            const el = document.getElementById(id);
            if(el.classList.contains('hidden')) el.classList.remove('hidden');
            else el.classList.add('hidden');
        }

        // --- GESTÃO DA REDE E EQUIPE ---
        window.saveUnit = async function() {
            if(!currentUser) return;
            const name = document.getElementById('unit-name').value.trim();
            if(!name) return showAlert("Nome da unidade obrigatório.", true);
            
            await addDoc(getPath('units'), { name, createdAt: new Date().toISOString() });
            document.getElementById('unit-name').value = '';
            toggleModal('modal-unit');
            showAlert("Unidade salva!");
        }

        function renderUnits() {
            const c = document.getElementById('units-list');
            if(state.units.length === 0) c.innerHTML = '<p class="text-sm text-muted">Nenhuma unidade cadastrada.</p>';
            else c.innerHTML = state.units.map(u => `<div class="p-3 bg-black/5 dark:bg-white/5 rounded-lg flex justify-between items-center border border-transparent hover:border-color">
                <span class="font-bold text-sm">${u.name}</span> 
                <button onclick="deleteDocItem('units', '${u.id}')" class="text-red-500 hover:bg-red-500 hover:text-white px-2 py-1 rounded text-xs transition-colors">Excluir</button>
            </div>`).join('');
        }

        function populateUnitSelects() {
            const opts = '<option value="">Selecione a Unidade...</option>' + state.units.map(u => `<option value="${u.id}">${u.name}</option>`).join('');
            document.getElementById('mem-unit').innerHTML = opts;
            document.getElementById('filter-unit-members').innerHTML = '<option value="">Filtrar por unidade...</option>' + state.units.map(u => `<option value="${u.id}">${u.name}</option>`).join('');
            document.getElementById('builder-unit').innerHTML = opts;
            document.getElementById('report-unit').innerHTML = '<option value="">Todas as unidades</option>' + state.units.map(u => `<option value="${u.id}">${u.name}</option>`).join('');
        }

        window.toggleCrefField = function() {
            const cat = document.getElementById('mem-cat').value;
            document.getElementById('cref-container').style.display = cat === 'PEF' ? 'block' : 'none';
        }

        window.saveMember = async function() {
            if(!currentUser) return;
            const name = document.getElementById('mem-name').value.trim();
            const cpf = document.getElementById('mem-cpf').value.trim();
            const unitId = document.getElementById('mem-unit').value;
            const uf = document.getElementById('mem-uf').value.trim().toUpperCase();
            const cat = document.getElementById('mem-cat').value;
            const cref = document.getElementById('mem-cref').value.trim();

            if(!name || !unitId || !uf) return showAlert("Nome, Unidade e UF são obrigatórios", true);
            
            await addDoc(getPath('members'), { name, cpf, unitId, uf, category: cat, cref: cat === 'PEF' ? cref : '', createdAt: new Date().toISOString() });
            
            document.getElementById('mem-name').value = '';
            document.getElementById('mem-cpf').value = '';
            document.getElementById('mem-cref').value = '';
            document.getElementById('mem-uf').value = '';
            toggleModal('modal-member');
            showAlert("Treinador salvo!");
        }

        window.renderMembers = function() {
            const filterId = document.getElementById('filter-unit-members').value;
            const c = document.getElementById('members-list');
            let filtered = state.members;
            if(filterId) filtered = filtered.filter(m => m.unitId === filterId);

            if(filtered.length === 0) c.innerHTML = '<p class="text-sm text-muted">Nenhum membro cadastrado.</p>';
            else c.innerHTML = filtered.map(m => {
                const unitName = state.units.find(u => u.id === m.unitId)?.name || 'Sem unidade';
                return `<div class="p-3 bg-black/5 dark:bg-white/5 rounded-lg flex justify-between items-center border border-transparent hover:border-color">
                    <div>
                        <div class="font-bold text-sm">${m.name} <span class="text-[10px] bg-accent text-white px-1.5 py-0.5 rounded ml-1">${m.category}</span></div>
                        <div class="text-xs text-muted">${unitName} - ${m.uf}</div>
                    </div>
                    <button onclick="deleteDocItem('members', '${m.id}')" class="text-red-500 hover:bg-red-500 hover:text-white px-2 py-1 rounded text-xs transition-colors">Excluir</button>
                </div>`;
            }).join('');
        }

        // --- RELATÓRIOS ---
        window.generateReport = function() {
            const monthStr = document.getElementById('report-month').value; // YYYY-MM
            const unitFilter = document.getElementById('report-unit').value;
            
            let filteredRoutines = state.routines;
            if (monthStr) {
                filteredRoutines = filteredRoutines.filter(r => r.createdAt && r.createdAt.startsWith(monthStr));
            }
            if (unitFilter) {
                filteredRoutines = filteredRoutines.filter(r => r.unitId === unitFilter);
            }

            let reportData = {};
            filteredRoutines.forEach(r => {
                const m = state.members.find(mem => mem.id === r.memberId);
                const memberName = m ? m.name : 'Membro Excluído';
                const u = state.units.find(un => un.id === r.unitId);
                const unitName = u ? u.name : 'Unidade Excluída';
                
                const key = `${unitName} | ${memberName}`;
                if(!reportData[key]) reportData[key] = 0;
                reportData[key]++;
            });

            const resDiv = document.getElementById('report-results');
            if(Object.keys(reportData).length === 0) {
                resDiv.innerHTML = '<p class="text-center text-muted p-4">Nenhuma ficha encontrada para este filtro.</p>';
                return;
            }

            let html = `
                <div class="bg-black/5 dark:bg-white/5 p-4 rounded-lg flex justify-between items-center mb-4 border border-color">
                    <span class="font-bold">Total Geral no Período:</span>
                    <span class="text-2xl font-black text-accent">${filteredRoutines.length} fichas</span>
                </div>
                <table class="w-full text-left text-sm border border-color">
                    <thead class="bg-black/10 dark:bg-white/10">
                        <tr><th class="p-3">Unidade | Treinador</th><th class="p-3 w-32 text-center">Quantidade</th></tr>
                    </thead>
                    <tbody>
            `;
            for(const [key, count] of Object.entries(reportData)) {
                html += `<tr class="border-b border-color"><td class="p-3">${key}</td><td class="p-3 text-center font-bold text-accent">${count}</td></tr>`;
            }
            html += '</tbody></table>';
            resDiv.innerHTML = html;
        }

        window.printReport = function() {
            const reportContent = document.getElementById('report-results').innerHTML;
            const printUi = document.getElementById('print-ui');
            let month = document.getElementById('report-month').value;
            if(month) {
                const [y, m] = month.split('-');
                month = `${m}/${y}`;
            } else {
                month = 'Todo o período';
            }
            
            printUi.innerHTML = `
                <div class="header-banner">
                    <h1 style="color:white; margin:0; font-style:italic;">POWFIT PRO</h1>
                    <p style="margin:2px 0 0 0; font-size:12px;">RELATÓRIO DE PRODUTIVIDADE E DESEMPENHO</p>
                </div>
                <div style="padding: 20px;">
                    <p><strong>Período Analisado:</strong> ${month}</p>
                    ${reportContent}
                </div>
            `;
            window.print();
        }

        // --- CONSTRUTOR DE FICHA (BUILDER) ---
        window.clearBuilder = function() {
            state.editingRoutineId = null;
            document.getElementById('builder-title').innerText = "Nova Ficha de Treino";
            
            document.getElementById('client-name').value = '';
            document.getElementById('client-age').value = '';
            document.getElementById('client-weight').value = '';
            document.getElementById('client-height').value = '';
            document.getElementById('client-gender').value = 'Masculino';
            document.getElementById('client-level').value = 'Iniciante';
            document.getElementById('client-freq').value = '3 dias';
            document.getElementById('client-obj').value = 'Emagrecimento';
            document.getElementById('routine-validity').value = '30';
            document.getElementById('routine-obs').value = '';
            
            document.querySelectorAll('.health-cb').forEach(cb => cb.checked = false);
            
            state.workoutTabs = [{ id: 'A', name: 'Treino A', rows: [] }];
            state.activeTabId = 'A';
            
            calculateBMI();
            toggleTheme();
            renderTabs();
            renderTable();
        }

        window.updateBuilderMembers = function() {
            const unitId = document.getElementById('builder-unit').value;
            const mSelect = document.getElementById('builder-member');
            if(!unitId) {
                mSelect.innerHTML = '<option value="">Selecione a Unidade 1º</option>';
                return;
            }
            mSelect.innerHTML = '<option value="">Selecione seu Perfil...</option>' + 
                state.members.filter(m => m.unitId === unitId).map(m => `<option value="${m.id}">${m.name} (${m.category})</option>`).join('');
        }

        window.toggleTheme = function() {
            const gender = document.getElementById('client-gender').value;
            if(gender === 'Feminino') document.body.classList.add('theme-female');
            else document.body.classList.remove('theme-female');
        }

        window.calculateBMI = function() {
            const w = parseFloat(document.getElementById('client-weight').value);
            const h = parseFloat(document.getElementById('client-height').value);
            const resEl = document.getElementById('bmi-result');
            
            if(w > 0 && h > 0) {
                const imc = w / (h * h);
                let sit = "";
                let colorClass = "";
                if(imc < 18.5) { sit = "Abaixo do peso"; colorClass = "text-yellow-500"; }
                else if(imc < 25) { sit = "Peso normal"; colorClass = "text-green-500"; }
                else if(imc < 30) { sit = "Sobrepeso"; colorClass = "text-yellow-500"; }
                else if(imc < 35) { sit = "Obesidade Grau I"; colorClass = "text-red-500"; }
                else if(imc < 40) { sit = "Obesidade Grau II"; colorClass = "text-red-600"; }
                else { sit = "Obesidade Grau III"; colorClass = "text-red-700"; }
                
                resEl.className = `font-bold text-sm ${colorClass}`;
                resEl.innerText = `${imc.toFixed(1)} - ${sit}`;
            } else {
                resEl.className = "font-bold text-sm";
                resEl.innerText = `--`;
            }
        }

        function initHealthCheckboxes() {
            const c = document.getElementById('health-status-container');
            c.innerHTML = HEALTH_OPTIONS.map(opt => `
                <label class="flex items-center gap-2 cursor-pointer p-2 rounded hover:bg-black/5 dark:hover:bg-white/5 border border-transparent transition-colors">
                    <input type="checkbox" value="${opt}" class="health-cb rounded bg-transparent border-color w-4 h-4 text-accent">
                    <span class="truncate select-none">${opt}</span>
                </label>
            `).join('');
        }

        window.addTab = function() {
            const nextLetter = String.fromCharCode(65 + state.workoutTabs.length);
            const newTab = { id: nextLetter, name: `Treino ${nextLetter}`, rows: [] };
            state.workoutTabs.push(newTab);
            state.activeTabId = newTab.id;
            renderTabs();
            renderTable();
        }

        window.removeCurrentTab = function() {
            if(state.workoutTabs.length <= 1) return showAlert("Deve haver pelo menos um treino.", true);
            state.workoutTabs = state.workoutTabs.filter(t => t.id !== state.activeTabId);
            // Renomear sequencialmente
            state.workoutTabs.forEach((t, i) => {
                t.id = String.fromCharCode(65 + i);
                t.name = `Treino ${t.id}`;
            });
            state.activeTabId = state.workoutTabs[0].id;
            renderTabs();
            renderTable();
        }

        window.switchTab = function(id) {
            state.activeTabId = id;
            renderTabs();
            renderTable();
        }

        function renderTabs() {
            const c = document.getElementById('tabs-header');
            c.innerHTML = state.workoutTabs.map(t => `
                <button onclick="switchTab('${t.id}')" class="px-5 py-2.5 rounded-t-lg font-bold border-b-4 whitespace-nowrap transition-colors text-sm ${t.id === state.activeTabId ? 'border-accent text-accent bg-black/5 dark:bg-white/5 shadow-inner' : 'border-transparent text-muted hover:bg-black/5 dark:hover:bg-white/5'}">
                    ${t.name}
                </button>
            `).join('');
        }

        window.addExerciseRow = function() {
            const tab = state.workoutTabs.find(t => t.id === state.activeTabId);
            tab.rows.push({ group: '', exercise: '', sets: '', reps: '', tech: 'Nenhuma', obs: '' });
            renderTable();
        }

        window.removeExerciseRow = function(index) {
            const tab = state.workoutTabs.find(t => t.id === state.activeTabId);
            tab.rows.splice(index, 1);
            renderTable();
        }

        window.updateRow = function(index, field, value) {
            const tab = state.workoutTabs.find(t => t.id === state.activeTabId);
            tab.rows[index][field] = value;
            if(field === 'group') {
                tab.rows[index].exercise = ''; 
                renderTable(); 
            }
        }

        function getExercisesForGroup(group) {
            if(!group) return [];
            const standard = EXERCISES_DB[group] || [];
            const custom = state.customExercises.filter(c => c.group === group).map(c => `${c.name}`);
            return [...standard, ...custom];
        }

        window.renderTable = function() {
            const tab = state.workoutTabs.find(t => t.id === state.activeTabId);
            const c = document.getElementById('tabs-content');
            
            if(tab.rows.length === 0) {
                c.innerHTML = `<div class="text-center p-12 text-muted border-dashed border-2 border-color rounded-xl flex flex-col items-center justify-center">
                    <span class="text-3xl mb-2">📋</span>
                    <p>Nenhum exercício neste treino. Adicione abaixo.</p>
                </div>`;
                return;
            }

            let html = `
                <table class="w-full text-sm min-w-[900px] border border-color rounded-lg overflow-hidden block sm:table">
                    <thead class="text-left bg-black/10 dark:bg-white/10 border-b border-color">
                        <tr>
                            <th class="p-3 w-40">Grupo Muscular</th>
                            <th class="p-3">Exercício</th>
                            <th class="p-3 w-20 text-center">Séries</th>
                            <th class="p-3 w-24 text-center">Reps</th>
                            <th class="p-3 w-36">Técnica</th>
                            <th class="p-3 w-48">Observações</th>
                            <th class="p-3 w-12 text-center">❌</th>
                        </tr>
                    </thead>
                    <tbody>
            `;

            tab.rows.forEach((row, i) => {
                const groupOpts = '<option value="">Selecionar...</option>' + Object.keys(EXERCISES_DB).map(g => `<option value="${g}" ${row.group===g?'selected':''}>${g}</option>`).join('');
                
                let exOpts = '<option value="">Grupo primeiro</option>';
                if(row.group) {
                    exOpts = '<option value="">Selecionar...</option>' + getExercisesForGroup(row.group).map(ex => `<option value="${ex}" ${row.exercise===ex?'selected':''}>${ex}</option>`).join('');
                }

                html += `
                    <tr class="border-b border-color hover:bg-black/5 dark:hover:bg-white/5 transition-colors">
                        <td class="p-2"><select class="input-field w-full p-2 rounded text-xs" onchange="updateRow(${i}, 'group', this.value)">${groupOpts}</select></td>
                        <td class="p-2"><select class="input-field w-full p-2 rounded text-xs font-bold" onchange="updateRow(${i}, 'exercise', this.value)">${exOpts}</select></td>
                        <td class="p-2"><input type="text" placeholder="Ex: 4" class="input-field w-full p-2 rounded text-xs text-center font-bold" value="${row.sets}" onchange="updateRow(${i}, 'sets', this.value)"></td>
                        <td class="p-2"><input type="text" placeholder="Ex: 10-12" class="input-field w-full p-2 rounded text-xs text-center font-bold" value="${row.reps}" onchange="updateRow(${i}, 'reps', this.value)"></td>
                        <td class="p-2">
                            <select class="input-field w-full p-2 rounded text-xs" onchange="updateRow(${i}, 'tech', this.value)">
                                ${TECHNIQUES.map(t => `<option value="${t}" ${row.tech===t?'selected':''}>${t}</option>`).join('')}
                            </select>
                        </td>
                        <td class="p-2"><input type="text" placeholder="Descanso, cadência..." class="input-field w-full p-2 rounded text-xs" value="${row.obs}" onchange="updateRow(${i}, 'obs', this.value)"></td>
                        <td class="p-2 text-center"><button onclick="removeExerciseRow(${i})" class="text-red-500 hover:bg-red-500 hover:text-white rounded w-8 h-8 flex items-center justify-center transition-colors">🗑️</button></td>
                    </tr>
                `;
            });

            html += `</tbody></table>`;
            c.innerHTML = html;
        }

        // --- EXERCÍCIOS CUSTOMIZADOS ---
        window.saveCustomExercise = async function() {
            if(!currentUser) return;
            const group = document.getElementById('custom-ex-group').value;
            const name = document.getElementById('custom-ex-name').value.trim();
            if(!name) return;
            
            await addDoc(getPath('custom_exercises'), { group, name, createdAt: new Date().toISOString() });
            document.getElementById('custom-ex-name').value = '';
            showAlert("Exercício adicionado ao banco!");
        }

        function renderCustomExercisesList() {
            const c = document.getElementById('custom-ex-list');
            if(state.customExercises.length === 0) {
                c.innerHTML = '<p class="text-xs text-muted text-center p-4">Nenhum exercício customizado.</p>';
                return;
            }
            c.innerHTML = state.customExercises.map(ex => `
                <div class="flex justify-between items-center p-2 bg-black/5 dark:bg-white/5 rounded">
                    <div class="text-xs">
                        <span class="font-bold text-accent">[${ex.group}]</span> ${ex.name}
                    </div>
                    <button onclick="deleteDocItem('custom_exercises', '${ex.id}')" class="text-red-500 hover:bg-red-500 hover:text-white px-2 py-1 rounded text-xs">Excluir</button>
                </div>
            `).join('');
        }

        // --- SALVAR, EDITAR E IMPRIMIR ---
        window.saveRoutine = async function(shouldPrint = false) {
            if(!currentUser) return;
            
            const memberId = document.getElementById('builder-member').value;
            if(!memberId) return showAlert("Selecione seu Perfil de Treinador (Passo 1).", true);
            
            const clientName = document.getElementById('client-name').value.trim();
            if(!clientName) return showAlert("Nome do aluno é obrigatório.", true);

            const member = state.members.find(m => m.id === memberId);
            const unit = state.units.find(u => u.id === member.unitId);
            const valDays = parseInt(document.getElementById('routine-validity').value);

            // Validar Tabs Vazias
            const hasEmptyTabs = state.workoutTabs.some(t => t.rows.length === 0);
            if(hasEmptyTabs && !confirm("Há abas de treino sem nenhum exercício. Deseja salvar mesmo assim?")) return;

            const healthCbs = document.querySelectorAll('.health-cb:checked');
            const healthStatuses = Array.from(healthCbs).map(cb => cb.value);

            // Calcular Data de Expiração
            const now = new Date();
            const expDate = new Date(now);
            expDate.setDate(expDate.getDate() + valDays);

            const routineData = {
                memberId,
                unitId: member.unitId,
                clientName,
                clientAge: document.getElementById('client-age').value,
                clientWeight: document.getElementById('client-weight').value,
                clientHeight: document.getElementById('client-height').value,
                clientGender: document.getElementById('client-gender').value,
                level: document.getElementById('client-level').value,
                frequency: document.getElementById('client-freq').value,
                objective: document.getElementById('client-obj').value,
                healthStatuses,
                validityDays: valDays,
                obs: document.getElementById('routine-obs').value,
                tabs: state.workoutTabs,
                createdAt: state.editingRoutineId ? undefined : now.toISOString(), // manter criação original se for edição
                updatedAt: now.toISOString(),
                expirationDate: expDate.toISOString()
            };

            try {
                if(state.editingRoutineId) {
                    await updateDoc(getDocPath('routines', state.editingRoutineId), routineData);
                    showAlert("Ficha ATUALIZADA com sucesso!");
                } else {
                    await addDoc(getPath('routines'), routineData);
                    showAlert("Ficha CRIADA com sucesso!");
                }
                
                if(shouldPrint) {
                    generatePrintView(routineData, member, unit, state.editingRoutineId ? routineData.createdAt : now.toISOString());
                    window.print();
                } else {
                    // Após salvar sem imprimir, redireciona pro histórico pra ver que salvou
                    switchView('history');
                }
            } catch(e) {
                console.error("Erro ao salvar ficha:", e);
                showAlert("Erro ao salvar. Verifique a conexão.", true);
            }
        }

        // --- HISTÓRICO ---
        window.renderHistory = function() {
            const list = document.getElementById('history-list');
            const nameFilter = document.getElementById('filter-history-name').value.toLowerCase();
            const statusFilter = document.getElementById('filter-history-status').value;
            const now = new Date();

            let filtered = state.routines.filter(r => r.clientName.toLowerCase().includes(nameFilter));

            filtered = filtered.filter(r => {
                const isExpired = new Date(r.expirationDate) < now;
                if(statusFilter === 'active') return !isExpired;
                if(statusFilter === 'expired') return isExpired;
                return true;
            });

            if(filtered.length === 0) {
                list.innerHTML = `<tr><td colspan="6" class="p-4 text-center text-muted">Nenhuma ficha encontrada no histórico.</td></tr>`;
                return;
            }

            list.innerHTML = filtered.map(r => {
                const isExpired = new Date(r.expirationDate) < now;
                const m = state.members.find(mem => mem.id === r.memberId);
                const trainerName = m ? m.name : 'Desconhecido';
                
                // Formatar Data Br
                const dObj = new Date(r.createdAt || r.updatedAt);
                const dateStr = dObj.toLocaleDateString('pt-BR');

                return `
                    <tr class="border-b border-color hover:bg-black/5 dark:hover:bg-white/5 transition-colors">
                        <td class="p-3 font-bold">${r.clientName}</td>
                        <td class="p-3 text-xs">${trainerName}</td>
                        <td class="p-3 text-xs">${dateStr}</td>
                        <td class="p-3 text-xs">${r.validityDays} dias</td>
                        <td class="p-3">
                            <span class="px-2 py-1 rounded text-xs font-bold text-white ${isExpired ? 'bg-red-500' : 'bg-green-500'}">
                                ${isExpired ? '🔴 Expirada' : '🟢 Ativa'}
                            </span>
                        </td>
                        <td class="p-3 text-right space-x-2">
                            <button onclick="editRoutine('${r.id}')" class="text-blue-500 hover:text-blue-400 font-bold text-sm bg-blue-500/10 px-2 py-1 rounded">✏️ Editar</button>
                            <button onclick="printRoutineFromHistory('${r.id}')" class="text-gray-500 hover:text-gray-400 font-bold text-sm bg-gray-500/10 px-2 py-1 rounded">🖨️</button>
                            <button onclick="deleteDocItem('routines', '${r.id}')" class="text-red-500 hover:text-red-400 font-bold text-sm bg-red-500/10 px-2 py-1 rounded">🗑️</button>
                        </td>
                    </tr>
                `;
            }).join('');
        }

        window.editRoutine = function(id) {
            const r = state.routines.find(x => x.id === id);
            if(!r) return;

            state.editingRoutineId = id;
            document.getElementById('builder-title').innerText = "Editando Ficha: " + r.clientName;

            // Popular UI Builder
            document.getElementById('builder-unit').value = r.unitId;
            updateBuilderMembers();
            document.getElementById('builder-member').value = r.memberId;
            
            document.getElementById('client-name').value = r.clientName;
            document.getElementById('client-gender').value = r.clientGender;
            document.getElementById('client-age').value = r.clientAge;
            document.getElementById('client-weight').value = r.clientWeight;
            document.getElementById('client-height').value = r.clientHeight;
            document.getElementById('client-level').value = r.level;
            document.getElementById('client-freq').value = r.frequency;
            document.getElementById('client-obj').value = r.objective;
            
            // Health
            document.querySelectorAll('.health-cb').forEach(cb => {
                cb.checked = (r.healthStatuses || []).includes(cb.value);
            });

            document.getElementById('routine-validity').value = r.validityDays;
            document.getElementById('routine-obs').value = r.obs;

            // Tabs
            state.workoutTabs = JSON.parse(JSON.stringify(r.tabs)); // clone
            if(state.workoutTabs.length > 0) state.activeTabId = state.workoutTabs[0].id;
            else state.activeTabId = null;

            calculateBMI();
            toggleTheme();
            switchView('builder');
            showAlert("Ficha carregada para edição.");
        }

        window.printRoutineFromHistory = function(id) {
            const r = state.routines.find(x => x.id === id);
            if(!r) return;
            const m = state.members.find(mem => mem.id === r.memberId) || { name: 'N/A', category: 'TE', uf: '' };
            const u = state.units.find(un => un.id === r.unitId) || { name: 'N/A' };
            generatePrintView(r, m, u, r.createdAt || r.updatedAt);
            window.print();
        }

        function generatePrintView(data, member, unit, creationDateISO) {
            const printUi = document.getElementById('print-ui');
            
            const creationDate = creationDateISO ? new Date(creationDateISO) : new Date();
            const dateStr = creationDate.toLocaleDateString('pt-BR');
            
            const expDate = new Date(data.expirationDate || new Date().toISOString());
            const validStr = expDate.toLocaleDateString('pt-BR');

            // Header
            let html = `
                <div class="header-banner">
                    <h1 style="color:white; margin:0; font-style: italic; font-size: 24px; letter-spacing:-1px;">POWFIT PRO</h1>
                    <p style="margin:2px 0 0 0; font-size:12px; letter-spacing: 1px;">PROGRAMA DE TREINAMENTO PERSONALIZADO</p>
                </div>
                
                <div style="display:flex; justify-content:space-between; margin-top:15px; border-bottom: 2px solid #111827; padding-bottom:10px;">
                    <div style="width: 48%;">
                        <h3 style="margin:0 0 5px 0; font-size: 13px; text-transform:uppercase;">Dados do Aluno</h3>
                        <p style="margin:2px 0;"><strong>Nome:</strong> ${data.clientName}</p>
                        <p style="margin:2px 0;"><strong>Idade:</strong> ${data.clientAge || '-'} | <strong>Peso:</strong> ${data.clientWeight || '-'}kg | <strong>Altura:</strong> ${data.clientHeight || '-'}m</p>
                        <p style="margin:2px 0;"><strong>Nível:</strong> ${data.level} | <strong>Frequência:</strong> ${data.frequency}</p>
                    </div>
                    <div style="width: 48%;">
                        <h3 style="margin:0 0 5px 0; font-size: 13px; text-transform:uppercase;">Responsável Técnico</h3>
                        <p style="margin:2px 0;"><strong>Profissional:</strong> ${member.name}</p>
                        <p style="margin:2px 0;"><strong>Atuação:</strong> ${member.category === 'PEF' ? 'Profissional de Educação Física' : 'Treinador Esportivo'}</p>
                        ${member.cref ? `<p style="margin:2px 0;"><strong>CREF:</strong> ${member.cref}</p>` : ''}
                        <p style="margin:2px 0;"><strong>Unidade:</strong> ${unit.name} - ${member.uf}</p>
                    </div>
                </div>
                
                <div style="margin-top: 10px; display:flex; gap:15px; font-size: 10px;">
                    <p style="margin:0;"><strong>Data de Emissão:</strong> ${dateStr}</p>
                    <p style="margin:0; background: #e5e7eb; padding: 2px 5px; border-radius:3px;"><strong>Vencimento da Ficha:</strong> ${validStr} (${data.validityDays} dias)</p>
                </div>
            `;

            // Objetivo & Saúde Textos
            const objText = TEXTS_OBJ[data.objective] || '';
            let healthTexts = '';
            (data.healthStatuses || []).forEach(hs => {
                const dict = member.category === 'PEF' ? TEXTS_HEALTH_PEF : TEXTS_HEALTH_TE;
                if(dict[hs]) healthTexts += `<li style="margin-bottom:2px;"><strong>${hs}:</strong> ${dict[hs]}</li>`;
            });

            html += `
                <div style="background: #f9fafb; padding: 10px; margin-top: 15px; border: 1px solid #d1d5db; border-radius: 4px;">
                    <p style="margin:0 0 5px 0;"><strong>Objetivo Principal (${data.objective}):</strong> ${objText}</p>
                    ${healthTexts ? `<ul style="margin:5px 0 0 0; padding-left: 20px; font-size: 10px;">${healthTexts}</ul>` : ''}
                </div>
            `;

            // Workout Tables
            data.tabs.forEach((tab) => {
                if(tab.rows.length === 0) return;
                
                html += `
                    <div class="avoid-break" style="margin-top: 20px;">
                        <h3 style="background: #111827; color: white; padding: 6px 10px; margin: 0 0 0 0; font-size:12px; border-radius:4px 4px 0 0; -webkit-print-color-adjust: exact;">${tab.name}</h3>
                        <table style="margin-top:0;">
                            <thead>
                                <tr>
                                    <th style="width: 35%;">Exercício / Máquina</th>
                                    <th style="width: 10%; text-align:center;">Séries</th>
                                    <th style="width: 15%; text-align:center;">Reps</th>
                                    <th style="width: 15%;">Técnica</th>
                                    <th style="width: 25%;">Observações</th>
                                </tr>
                            </thead>
                            <tbody>
                `;
                tab.rows.forEach(r => {
                    const exName = r.exercise; // Clean custom star for print
                    html += `
                        <tr>
                            <td><strong style="font-size:12px;">${exName}</strong> <br><span style="font-size:9px; color:#6b7280; text-transform:uppercase;">${r.group}</span></td>
                            <td style="text-align:center; font-weight:bold; font-size:14px;">${r.sets}</td>
                            <td style="text-align:center; font-weight:bold; font-size:14px;">${r.reps}</td>
                            <td style="font-size:10px;">${r.tech !== 'Nenhuma' ? r.tech : '-'}</td>
                            <td style="font-size:10px;">${r.obs}</td>
                        </tr>
                    `;
                });
                html += `</tbody></table></div>`;
            });

            // Recomendações Livres
            if(data.obs) {
                html += `
                    <div class="avoid-break" style="margin-top: 15px;">
                        <h3 style="margin: 0 0 5px 0; font-size: 12px; text-transform:uppercase;">Recomendações Profissionais Adicionais</h3>
                        <p style="white-space: pre-wrap; font-size: 11px; padding: 10px; border: 1px solid #d1d5db; border-radius: 4px; background:#f9fafb;">${data.obs}</p>
                    </div>
                `;
            }

            // Legal Footer
            const legalText = member.category === 'PEF' ? LEGAL_PEF : LEGAL_TE;
            html += `
                <div class="legal-footer avoid-break">
                    ${legalText}
                </div>
            `;

            printUi.innerHTML = html;
        }

    </script>
</body>
</html>
