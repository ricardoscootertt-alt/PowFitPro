<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro - Gestão Master de Treinos</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
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
        .btn-primary { background-color: var(--primary); color: white; transition: 0.2s; }
        .btn-primary:hover { background-color: var(--primary-hover); }
        .text-primary { color: var(--primary); }
        
        .hidden { display: none !important; }
        .loader { border: 3px solid rgba(255,255,255,0.3); border-radius: 50%; border-top: 3px solid white; width: 24px; height: 24px; animation: spin 1s linear infinite; }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }

        /* Scrollbar */
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }

        /* Estilos de Impressão (Planilha Profissional A4) */
        #print-area { display: none; }
        @media print {
            body { background: white !important; color: black !important; margin: 0; padding: 0; }
            #app-container, .modal, .no-print, nav { display: none !important; }
            #print-area { display: block !important; padding: 10mm; font-family: 'Arial', sans-serif; font-size: 10px; }
            @page { size: A4; margin: 0; }
            
            .print-header { border-bottom: 2px solid #000; padding-bottom: 8px; margin-bottom: 12px; text-align: center; }
            .print-title { font-size: 18px; font-weight: bold; text-transform: uppercase; margin: 0; letter-spacing: 1px;}
            .prof-info { font-size: 11px; margin-top: 4px; font-weight: bold; }
            
            .print-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 4px; margin-bottom: 12px; border: 1px solid #000; padding: 8px; background: #fff;}
            .print-grid div { font-size: 10px; }
            
            .print-guidelines { border: 1px solid #000; padding: 8px; margin-bottom: 12px; font-size: 9px; }
            .print-guidelines h4 { margin: 0 0 4px 0; font-size: 10px; text-transform: uppercase; background: #eee; padding: 3px; border-bottom: 1px solid #ccc; }
            .print-guidelines ul { margin: 0; padding-left: 15px; }
            .print-guidelines li { margin-bottom: 3px; text-align: justify; }

            .print-workout { margin-bottom: 15px; page-break-inside: avoid; }
            .print-workout h3 { background: #e0e0e0; border: 1px solid #000; border-bottom: none; padding: 4px 8px; margin: 0; font-size: 12px; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            table { width: 100%; border-collapse: collapse; border: 1px solid #000; }
            th, td { border: 1px solid #000; padding: 4px 6px; text-align: left; font-size: 10px; }
            th { background-color: #f0f0f0; font-weight: bold; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            
            .print-footer-section { margin-top: 15px; border: 1px solid #000; padding: 8px; font-size: 9px; page-break-inside: avoid; }
            .legal-text { font-size: 8px; color: #111; text-align: justify; margin-top: 8px; font-style: italic; white-space: pre-wrap; line-height: 1.3;}
            
            /* Estilos exclusivos para Relatório de Produtividade Impresso */
            .report-table { width: 100%; border-collapse: collapse; margin-top: 10px; }
            .report-table th, .report-table td { border: 1px solid #000; padding: 8px; text-align: left; font-size: 12px; }
            .report-table th { background-color: #eee; font-weight: bold; }
            .report-unit-header { background-color: #ddd; font-weight: bold; text-align: left; padding: 8px; border: 1px solid #000; margin-top: 15px; font-size: 14px;}
        }
    </style>
</head>
<body data-theme="Masculino">

    <div id="global-loader" class="fixed inset-0 bg-black bg-opacity-80 z-[100] flex flex-col items-center justify-center hidden">
        <div class="loader mb-4 w-12 h-12 border-4"></div>
        <p class="text-white font-bold tracking-wide" id="loader-text">Conectando à Nuvem...</p>
    </div>

    <div id="login-screen" class="min-h-screen flex items-center justify-center p-4 bg-gradient-to-br from-slate-900 to-black">
        <div class="card p-8 rounded-2xl shadow-2xl max-w-md w-full text-center border-t-4 border-t-primary relative overflow-hidden">
            <div class="absolute -top-10 -right-10 w-32 h-32 bg-primary opacity-10 rounded-full blur-2xl"></div>
            <i class="fas fa-dumbbell text-5xl text-primary mb-4 relative z-10"></i>
            <h1 class="text-3xl font-bold mb-2 relative z-10">PowFit Pro</h1>
            <p class="opacity-70 mb-8 text-sm relative z-10">Plataforma Profissional e Gestão de Franquias</p>
            <button id="btn-login" class="w-full btn-primary py-3 rounded-lg font-bold flex items-center justify-center gap-2 relative z-10 shadow-lg hover:shadow-primary/50 transition">
                <i class="fab fa-google"></i> Acesso Profissional com Google
            </button>
            <p class="text-[10px] opacity-50 mt-6 relative z-10">Seus dados e de seus alunos estarão seguros na nuvem.</p>
        </div>
    </div>

    <div id="setup-screen" class="min-h-screen flex items-center justify-center p-4 hidden">
        <div class="card p-8 rounded-2xl shadow-2xl max-w-lg w-full border-t-4 border-t-primary">
            <h2 class="text-2xl font-bold mb-2"><i class="fas fa-building text-primary"></i> Criar Franquia / Rede</h2>
            <p class="opacity-70 mb-6 text-sm">Estruture sua marca principal. Mais unidades poderão ser adicionadas depois.</p>
            
            <div class="space-y-4">
                <div>
                    <label class="block text-sm font-medium mb-1 uppercase opacity-70 text-[10px] tracking-wider">Sua Marca / Rede</label>
                    <input type="text" id="setup-network-name" class="input-field w-full rounded-lg px-3 py-3" placeholder="Ex: Academia FitLife">
                </div>
                <div>
                    <label class="block text-sm font-medium mb-1 uppercase opacity-70 text-[10px] tracking-wider">Sua Primeira Unidade</label>
                    <input type="text" id="setup-unit-name" class="input-field w-full rounded-lg px-3 py-3" placeholder="Ex: Matriz (Centro)">
                </div>
                <button id="btn-save-setup" class="w-full btn-primary py-3 rounded-lg font-bold mt-4 shadow-lg">
                    Finalizar Configuração
                </button>
            </div>
        </div>
    </div>

    <div id="app-screen" class="hidden min-h-screen pb-20">
        
        <nav class="bg-black bg-opacity-20 border-b border-opacity-20 backdrop-blur-md sticky top-0 z-40" style="border-color: var(--border-color)">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-3 flex flex-wrap justify-between items-center gap-4">
                <div class="flex items-center gap-3">
                    <div class="w-10 h-10 bg-primary rounded-lg flex items-center justify-center text-white font-bold text-xl shadow-lg">
                        <i class="fas fa-dumbbell"></i>
                    </div>
                    <div>
                        <h1 class="font-bold leading-tight" id="nav-network-name">Minha Rede</h1>
                        <p class="text-[9px] opacity-70 uppercase tracking-widest">Painel de Gestão</p>
                    </div>
                </div>
                
                <div class="flex items-center gap-2 w-full lg:w-auto flex-wrap">
                    <div class="flex items-center gap-1 bg-black bg-opacity-20 p-1 rounded-lg border border-gray-600 border-opacity-30">
                        <i class="fas fa-map-marker-alt text-[10px] text-gray-400 ml-2"></i>
                        <select id="nav-select-unit" class="bg-transparent text-xs font-medium px-2 py-1.5 outline-none max-w-[140px] text-white"></select>
                        <div class="w-px h-4 bg-gray-600 mx-1"></div>
                        <i class="fas fa-user-tie text-[10px] text-gray-400"></i>
                        <select id="nav-select-member" class="bg-transparent text-xs font-medium px-2 py-1.5 outline-none max-w-[140px] text-white"></select>
                    </div>
                    
                    <button onclick="openModal('modal-manage')" class="text-xs px-3 py-2 rounded hover:bg-black hover:bg-opacity-20 transition border border-transparent hover:border-gray-600 tooltip" title="Gerenciar Unidades e Treinadores">
                        <i class="fas fa-users-cog"></i> <span class="hidden md:inline">Equipe</span>
                    </button>
                    <button onclick="openReports()" class="text-xs px-3 py-2 rounded hover:bg-black hover:bg-opacity-20 transition border border-transparent hover:border-gray-600">
                        <i class="fas fa-chart-line"></i> <span class="hidden md:inline">Produtividade</span>
                    </button>
                    <button id="btn-logout" class="text-xs px-3 py-2 rounded text-red-400 hover:bg-red-500 hover:bg-opacity-10 transition">
                        <i class="fas fa-power-off"></i> Sair
                    </button>
                </div>
            </div>
        </nav>

        <div id="app-container" class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-6">
            
            <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center mb-6 gap-4">
                <div>
                    <h2 class="text-2xl font-bold tracking-tight">Prescrição de Treino</h2>
                    <p class="text-xs opacity-60">Monte a ficha do seu aluno manualmente e salve na nuvem.</p>
                </div>
                <div class="flex gap-2 w-full sm:w-auto">
                    <button onclick="openHistory()" class="flex-1 sm:flex-none bg-gray-700 hover:bg-gray-600 text-white px-4 py-2.5 rounded-lg text-sm font-medium shadow transition">
                        <i class="fas fa-history"></i> Histórico
                    </button>
                    <button onclick="generatePrint()" class="flex-1 sm:flex-none btn-primary px-6 py-2.5 rounded-lg text-sm font-bold shadow-lg flex items-center justify-center gap-2">
                        <i class="fas fa-print"></i> Salvar e Imprimir
                    </button>
                </div>
            </div>

            <div id="alert-no-member" class="bg-red-500/10 border border-red-500/50 text-red-400 p-4 rounded-xl text-sm mb-6 hidden flex items-center gap-3">
                <i class="fas fa-exclamation-circle text-2xl"></i>
                <div>
                    <strong class="block">Atenção!</strong>
                    Nenhum profissional selecionado. Selecione ou cadastre um treinador no menu superior (Equipe) para prescrever treinos.
                </div>
            </div>

            <div class="grid grid-cols-1 xl:grid-cols-12 gap-6">
                <div class="xl:col-span-4 space-y-6">
                    
                    <div class="card rounded-2xl p-5 shadow-sm border-t-2 border-t-blue-500">
                        <div class="flex justify-between items-center mb-4 border-b border-opacity-10 pb-2" style="border-color: var(--border-color)">
                            <h3 class="font-bold flex items-center gap-2"><i class="fas fa-user-graduate text-primary"></i> Perfil do Aluno</h3>
                            <div id="imc-display"></div>
                        </div>
                        <div class="space-y-4">
                            <div>
                                <label class="block text-[10px] uppercase tracking-wider font-bold opacity-60 mb-1">Nome Completo</label>
                                <input type="text" id="stu-name" class="input-field w-full rounded-lg p-2 text-sm" placeholder="Nome do aluno">
                            </div>
                            <div class="grid grid-cols-3 gap-2">
                                <div>
                                    <label class="block text-[10px] uppercase tracking-wider font-bold opacity-60 mb-1">Idade</label>
                                    <input type="number" id="stu-age" class="input-field w-full rounded-lg p-2 text-sm" placeholder="Anos">
                                </div>
                                <div>
                                    <label class="block text-[10px] uppercase tracking-wider font-bold opacity-60 mb-1">Peso(kg)</label>
                                    <input type="number" step="0.1" id="stu-weight" oninput="calculateIMC()" class="input-field w-full rounded-lg p-2 text-sm" placeholder="0.0">
                                </div>
                                <div>
                                    <label class="block text-[10px] uppercase tracking-wider font-bold opacity-60 mb-1">Altura(m)</label>
                                    <input type="number" step="0.01" id="stu-height" oninput="calculateIMC()" class="input-field w-full rounded-lg p-2 text-sm" placeholder="0.00">
                                </div>
                            </div>
                            <div class="grid grid-cols-2 gap-2">
                                <div>
                                    <label class="block text-[10px] uppercase tracking-wider font-bold opacity-60 mb-1">Gênero</label>
                                    <select id="stu-gender" onchange="changeTheme()" class="input-field w-full rounded-lg p-2 text-sm font-medium">
                                        <option value="Masculino">Masculino</option>
                                        <option value="Feminino">Feminino</option>
                                    </select>
                                </div>
                                <div>
                                    <label class="block text-[10px] uppercase tracking-wider font-bold opacity-60 mb-1">Nível</label>
                                    <select id="stu-level" class="input-field w-full rounded-lg p-2 text-sm">
                                        <option>Iniciante</option><option>Intermediário</option><option>Avançado</option>
                                    </select>
                                </div>
                            </div>
                            <div>
                                <label class="block text-[10px] uppercase tracking-wider font-bold opacity-60 mb-1">Objetivo (Gera Diretriz)</label>
                                <select id="stu-objective" class="input-field w-full rounded-lg p-2 text-sm">
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
                            <div class="grid grid-cols-2 gap-2">
                                <div>
                                    <label class="block text-[10px] uppercase tracking-wider font-bold opacity-60 mb-1">Frequência</label>
                                    <select id="stu-freq" class="input-field w-full rounded-lg p-2 text-sm">
                                        <option>3 dias</option><option>5 dias</option><option>6 dias</option><option>Personalizado</option>
                                    </select>
                                </div>
                                <div>
                                    <label class="block text-[10px] uppercase tracking-wider font-bold opacity-60 mb-1">Validade</label>
                                    <select id="stu-validity" class="input-field w-full rounded-lg p-2 text-sm">
                                        <option>15 dias</option><option selected>30 dias</option><option>60 dias</option><option>90 dias</option>
                                    </select>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div class="card rounded-2xl p-5 shadow-sm">
                        <h3 class="font-bold mb-1 flex items-center gap-2 border-b border-opacity-10 pb-2" style="border-color: var(--border-color)">
                            <i class="fas fa-heartbeat text-primary"></i> Condições de Saúde
                        </h3>
                        <p class="text-[9px] uppercase tracking-wider opacity-60 mb-3 font-bold text-primary">Diretrizes automáticas baseadas na atuação (PEF ou TE).</p>
                        <div class="grid grid-cols-1 gap-1.5 text-xs max-h-56 overflow-y-auto pr-2" id="health-container">
                            </div>
                    </div>
                </div>

                <div class="xl:col-span-8 space-y-4">
                    
                    <div class="card p-4 rounded-2xl flex flex-col md:flex-row justify-between items-center gap-4 shadow-sm border-t-2 border-t-blue-500">
                        <div>
                            <h2 class="font-bold text-lg"><i class="fas fa-layer-group text-primary mr-2"></i>Montagem da Ficha</h2>
                        </div>
                        <div class="flex flex-wrap gap-2 justify-end">
                            <button onclick="addDay('Segunda-Feira')" class="bg-white/5 hover:bg-white/10 border border-gray-500/20 px-3 py-1.5 rounded-lg text-xs font-bold transition">Segunda</button>
                            <button onclick="addDay('Terça-Feira')" class="bg-white/5 hover:bg-white/10 border border-gray-500/20 px-3 py-1.5 rounded-lg text-xs font-bold transition">Terça</button>
                            <button onclick="addDay('Quarta-Feira')" class="bg-white/5 hover:bg-white/10 border border-gray-500/20 px-3 py-1.5 rounded-lg text-xs font-bold transition">Quarta</button>
                            <button onclick="addDay('Quinta-Feira')" class="bg-white/5 hover:bg-white/10 border border-gray-500/20 px-3 py-1.5 rounded-lg text-xs font-bold transition">Quinta</button>
                            <button onclick="addDay('Sexta-Feira')" class="bg-white/5 hover:bg-white/10 border border-gray-500/20 px-3 py-1.5 rounded-lg text-xs font-bold transition">Sexta</button>
                            <button onclick="addDay('Sábado')" class="bg-white/5 hover:bg-white/10 border border-gray-500/20 px-3 py-1.5 rounded-lg text-xs font-bold transition">Sábado</button>
                            <button onclick="addDay('Domingo')" class="bg-white/5 hover:bg-white/10 border border-gray-500/20 px-3 py-1.5 rounded-lg text-xs font-bold transition">Domingo</button>
                            <button onclick="addDay('Novo Treino')" class="btn-primary px-3 py-1.5 rounded-lg text-xs font-bold shadow-md">+ Extra</button>
                        </div>
                    </div>

                    <div id="workouts-container" class="space-y-5">
                        </div>

                    <div class="card rounded-2xl p-5 shadow-sm">
                        <h2 class="text-md font-bold mb-3 flex items-center gap-2"><i class="fas fa-comment-medical text-primary"></i> Recomendações do Treinador (Livre)</h2>
                        <textarea id="stu-recs" rows="4" class="input-field w-full rounded-xl p-3 text-sm" placeholder="Dicas sobre hidratação, intensidade, tempo de descanso..."></textarea>
                    </div>

                </div>
            </div>
        </div>
    </div>

    <div id="modal-manage" class="modal fixed inset-0 bg-black bg-opacity-80 backdrop-blur-sm hidden z-[60] flex items-center justify-center p-4">
        <div class="card w-full max-w-4xl rounded-2xl shadow-2xl flex flex-col max-h-[90vh]">
            <div class="p-5 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <div>
                    <h3 class="text-xl font-bold"><i class="fas fa-sitemap text-primary mr-2"></i> Gestão da Franquia e Equipe</h3>
                    <p class="text-xs opacity-60 mt-1">Organize suas unidades e cadastre os profissionais habilitados.</p>
                </div>
                <button onclick="closeModal('modal-manage')" class="text-gray-400 hover:text-white text-3xl leading-none">&times;</button>
            </div>
            <div class="p-6 overflow-y-auto flex-1 grid grid-cols-1 md:grid-cols-2 gap-8">
                
                <div class="space-y-4 p-5 bg-black bg-opacity-20 rounded-xl border border-gray-500/20">
                    <h4 class="font-bold text-md flex items-center gap-2"><i class="fas fa-store text-blue-400"></i> Adicionar Nova Unidade</h4>
                    <p class="text-xs opacity-60">Expanda sua rede criando locais físicos de atuação.</p>
                    <div>
                        <input type="text" id="new-unit-name" class="input-field w-full rounded-lg p-3 text-sm" placeholder="Nome da Unidade (Ex: Filial Sul)">
                    </div>
                    <button onclick="addNewUnit()" class="btn-primary w-full py-2.5 rounded-lg text-sm font-bold shadow-md">Salvar Unidade</button>
                </div>

                <div class="space-y-4 p-5 bg-black bg-opacity-20 rounded-xl border border-gray-500/20">
                    <h4 class="font-bold text-md flex items-center gap-2"><i class="fas fa-id-card-alt text-blue-400"></i> Cadastrar Profissional</h4>
                    <p class="text-xs opacity-60">Adicione treinadores às suas unidades.</p>
                    
                    <div>
                        <label class="block text-[10px] uppercase font-bold opacity-60 mb-1">Unidade de Lotação</label>
                        <select id="new-member-unit" class="input-field w-full rounded-lg p-2.5 text-sm"></select>
                    </div>
                    <div>
                        <label class="block text-[10px] uppercase font-bold opacity-60 mb-1">Categoria Profissional</label>
                        <select id="new-member-type" onchange="toggleNewMemberCref()" class="input-field w-full rounded-lg p-2.5 text-sm font-medium">
                            <option value="PEF">Profissional de Educação Física (PEF)</option>
                            <option value="TE">Treinador Esportivo (TE)</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-[10px] uppercase font-bold opacity-60 mb-1">Nome Completo</label>
                        <input type="text" id="new-member-name" class="input-field w-full rounded-lg p-2.5 text-sm" placeholder="Nome do Treinador">
                    </div>
                    <div id="new-member-cref-container" class="grid grid-cols-2 gap-2">
                        <div>
                            <label class="block text-[10px] uppercase font-bold opacity-60 mb-1">CREF</label>
                            <input type="text" id="new-member-cref" class="input-field w-full rounded-lg p-2.5 text-sm" placeholder="Ex: 000000-G/XX">
                        </div>
                        <div>
                            <label class="block text-[10px] uppercase font-bold opacity-60 mb-1">Estado (UF)</label>
                            <input type="text" id="new-member-state" class="input-field w-full rounded-lg p-2.5 text-sm" placeholder="Ex: SP">
                        </div>
                    </div>
                    <button onclick="addNewMember()" class="btn-primary w-full py-2.5 rounded-lg text-sm font-bold shadow-md mt-2">Salvar Treinador</button>
                </div>

            </div>
        </div>
    </div>

    <div id="modal-exercises" class="modal fixed inset-0 bg-black bg-opacity-80 backdrop-blur-sm hidden z-[70] flex items-center justify-center p-2 sm:p-4">
        <div class="card w-full max-w-6xl rounded-2xl shadow-2xl flex flex-col h-[90vh] overflow-hidden border-t-4 border-t-primary">
            <div class="p-4 border-b flex justify-between items-center bg-black bg-opacity-20" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold"><i class="fas fa-search text-primary mr-2"></i> Banco de Exercícios Profissional</h3>
                <button onclick="closeModal('modal-exercises')" class="text-gray-400 hover:text-white text-3xl leading-none">&times;</button>
            </div>
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <div class="w-full md:w-1/4 border-r overflow-y-auto p-3 space-y-1 bg-black bg-opacity-10" style="border-color: var(--border-color)" id="modal-cats"></div>
                <div class="w-full md:w-3/4 overflow-y-auto p-4" id="modal-ex-list"></div>
            </div>
        </div>
    </div>

    <div id="modal-history" class="modal fixed inset-0 bg-black bg-opacity-80 backdrop-blur-sm hidden z-[60] flex items-center justify-center p-4">
        <div class="card w-full max-w-5xl rounded-2xl shadow-2xl flex flex-col max-h-[90vh]">
            <div class="p-5 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-xl font-bold" id="history-title"><i class="fas fa-cloud text-primary mr-2"></i> Fichas na Nuvem</h3>
                <div class="flex items-center gap-3">
                    <button onclick="loadReportMode()" id="btn-toggle-report" class="bg-gray-700 hover:bg-gray-600 transition px-4 py-2 text-xs rounded-lg font-bold flex items-center gap-2">
                        <i class="fas fa-chart-pie"></i> Relatório Mensal
                    </button>
                    <button onclick="closeModal('modal-history')" class="text-gray-400 hover:text-white text-3xl leading-none">&times;</button>
                </div>
            </div>
            <div class="p-6 overflow-y-auto flex-1" id="history-content">
                </div>
        </div>
    </div>

    <div id="print-area"></div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-app.js";
        import { getAuth, signInWithPopup, GoogleAuthProvider, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, addDoc, query, where, getDocs, deleteDoc } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-firestore.js";

        // --- 1. CONFIGURAÇÃO FIREBASE ---
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

        // --- 2. BANCO DE DADOS LOCAL (REGRAS E EXERCÍCIOS) ---
        const D = {
            healthPEF: {
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
            },
            healthTE: {
                "🟢 Saudável": "Manter treinos regulares de 3–5x por semana, combinando musculação e atividades cardiovasculares. A constância, descanso adequado e boa alimentação contribuem para melhores resultados.",
                "⚪ Sedentário": "O início deve ser gradual, com treinos leves e foco na adaptação do corpo. A prioridade é desenvolver constância, aprender a execução correta e evitar excesso de carga.",
                "🟡 Sobrepeso": "A prática regular de musculação associada ao cardio pode contribuir para melhora do condicionamento físico e composição corporal. A progressão deve respeitar a individualidade e o nível de adaptação.",
                "🔴 Obesidade": "É recomendado iniciar com exercícios de menor impacto e progressão gradual. A segurança, mobilidade e constância são prioridades durante o processo.",
                "⚖️ Baixo peso": "A musculação pode auxiliar no ganho de massa muscular quando associada a alimentação adequada e descanso. O treino deve priorizar evolução progressiva e recuperação muscular.",
                "🍬 Diabetes": "Pessoas com diabetes devem manter acompanhamento médico regular antes e durante a prática de exercícios. A musculação pode auxiliar na rotina de atividade física quando liberada por profissional de saúde. Em casos de tontura, mal-estar ou alteração de glicemia, o treino deve ser interrompido e procurar orientação médica.",
                "❤️ Hipertensão": "Pessoas com hipertensão devem manter acompanhamento médico regular e respeitar as orientações profissionais. Durante o treino, é importante evitar prender a respiração e controlar a intensidade dos exercícios.",
                "🔵 Hipotensão": "Pessoas com hipotensão devem evitar mudanças bruscas de posição durante o treino. Manter boa hidratação e respeitar a intensidade adequada ajuda a reduzir episódios de mal-estar.",
                "💔 Problemas cardíacos": "A prática de exercícios deve ocorrer apenas com liberação e acompanhamento médico. O treino deve respeitar limites individuais, com controle de intensidade e atenção aos sinais do corpo.",
                "🦴 Problemas articulares": "Exercícios com menor impacto e maior controle de movimento costumam ser mais indicados. O acompanhamento profissional e a adaptação dos exercícios ajudam na segurança durante o treino.",
                "🫁 Problemas respiratórios": "A progressão do treino deve ser gradual e respeitar a capacidade respiratória individual. Em caso de falta de ar excessiva ou desconforto, o exercício deve ser interrompido.",
                "⚠️ Lesões": "A adaptação dos exercícios deve respeitar a limitação existente e evitar dor durante a execução. Em situações de lesão, é importante seguir orientação profissional adequada antes da prática.",
                "🤰 Gestante": "A prática de exercícios deve ocorrer com liberação médica e acompanhamento adequado. O foco deve ser segurança, mobilidade e bem-estar, evitando impacto excessivo e situações de risco.",
                "🤱 Lactante": "A atividade física pode ser mantida normalmente, respeitando a recuperação individual e mantendo boa hidratação e alimentação adequada.",
                "👴 Idoso": "A musculação pode contribuir para força, equilíbrio, mobilidade e autonomia. O treino deve respeitar limitações individuais, com intensidade moderada e foco na segurança."
            },
            objectives: {
                "Emagrecimento": "Foco em déficit calórico com treinos mistos de força (manter massa magra) e aeróbicos (maior gasto).",
                "Hipertrofia": "Prioridade na progressão de carga e volume adequado. Essencial superávit calórico e descanso.",
                "Definição": "Manutenção de massa muscular enquanto reduz o percentual de gordura. Atenção estrita à dieta.",
                "Condicionamento": "Treinos com menor tempo de intervalo, circuitos e alta integração cardiopulmonar.",
                "Resistência": "Séries mais longas, cadência controlada e aprimromamento da capacidade muscular.",
                "Força": "Cargas altas, baixas repetições e intervalos de descanso maiores.",
                "Reabilitação": "Treino focado em fortalecimento específico, mobilidade e controle motor. Respeitar limites.",
                "Saúde geral": "Equilíbrio entre força, cardio e flexibilidade. O principal objetivo é a constância e bem-estar."
            },
            categories: {
                "🔥 PEITO": [
                    "Supino Reto", "Supino Inclinado", "Supino com Halteres", "Supino Fechado com Halteres", 
                    "Cross Over", "Cross Over Alto", "Cross Over Baixo", "Crucifixo Reto", "Crucifixo Inclinado", 
                    "Crucifixo na Máquina", "Peck Fly", "Peck Fly Unilateral", "Pullover", 
                    "Flexão de Braço", "Flexão com Pés Elevados", "Flexão Explosiva"
                ],
                "🦍 COSTAS": [
                    "Puxada Alta", "Puxada Unilateral", "Pulldown", "Remada Aberta", "Remada Baixa", 
                    "Remada Curvada", "Remada Curvada na Polia", "Remada Unilateral", "Remada Cavalinho (T-Bar)", 
                    "Remada no Cross", "Serrote", "Facepull (puxada de cima para baixo)", "Encolhimento (Trapézio)"
                ],
                "🦵 PERNAS": [
                    "Agachamento Livre", "Agachamento Taça", "Agachamento com Barra", "Agachamento no Smith", 
                    "Squat", "Hack Machine", "Leg 45°", "Leg 90°", "Agachamento Sumô", "Agachamento Sissy (Livre)", 
                    "Afundo", "Recuo", "Avanço", "Passada", "Búlgaro", "Step-up", "Levantamento Terra", 
                    "Levantamento Terra Romeno", "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", "Cadeira Flexora", 
                    "Elevação Pélvica no Banco", "Elevação Pélvica no Chão", "Elevação Pélvica Unilateral no Chão", 
                    "Extensão de Quadril (Glúteo Máximo)", "Extensão Cruzada (Glúteo Médio)", "Abdução no Cross (Glúteo Médio + Mínimo)", 
                    "Coice", "Cachorrinho", "Caranguejo", "Cadeira Extensora", "Adução", "Abdução", "Cadeira Abdutora Inclinada", 
                    "Flexão Nórdica", "Flexão Nórdica Invertida", "Panturrilha em Pé (Máquina)", "Panturrilha Livre", 
                    "Panturrilha no Leg Press", "Panturrilha Banco", "Panturrilha Squat", "Panturrilha Unilateral"
                ],
                "💪 BRAÇOS (Bíceps)": [
                    "Rosca Direta", "Rosca Alternada", "Rosca 21", "Rosca Scott Barra W", 
                    "Rosca Scott Unilateral", "Rosca Scott com Halteres", "Rosca Martelo", 
                    "Rosca Cross", "Rosca Concentrada", "Rosca Inversa", "Rosca 45°"
                ],
                "💪 BRAÇOS (Tríceps)": [
                    "Triceps Pulley Unilateral", "Tríceps Pulley Barra", "Tríceps Pulley Corda", "Tríceps Pulley Pegada Inversa", 
                    "Tríceps Francês na Corda", "Tríceps Francês com Halter", "Tríceps Francês Unilateral", 
                    "Tríceps Cruzado Polia Dupla", "Tríceps Coice Unilateral", "Tríceps Arremesso", "Tríceps Testa", "Mergulho no Banco"
                ],
                "🪨 OMBROS": [
                    "Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", "Elevação Lateral na Polia", 
                    "Elevação Lateral Sentado", "Desenvolvimento com Halteres", "Desenvolvimento com Barra", "Arnold Press", 
                    "Elevação Borboleta", "Crucifixo Inverso Sentado com Halteres", "Crucifixo Inverso na Polia", 
                    "Crucifixo Inverso Unilateral na Polia", "Facepull (puxada reta)", "Remada Alta"
                ],
                "🧠 ABDÔMEN": [
                    "Infra com Elevação de Perna", "Abdominal Supra", "Abdominal Remador", "Abdominal Bicicleta", 
                    "Abdominal Twister com Peso", "Prancha", "Prancha Lateral", "Trituração de Cabos em Pé", 
                    "Isometria na parede", "Abdominal isometrico"
                ],
                "🫀 CARDIO": [
                    "Bicicleta - 10 Minutos", "Bicicleta - 15 Minutos", "Bicicleta - 20 Minutos",
                    "Esteira - 10 Minutos", "Esteira - 15 Minutos", "Esteira - 20 Minutos",
                    "Pular Corda"
                ]
            },
            techniques: ["Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", "Negativa", "Isometria", "Parciais", "Pirâmide"],
            legalPEF: "⚠️ OBSERVAÇÃO LEGAL – PROFISSIONAL DE EDUCAÇÃO FÍSICA\nConforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º da mesma lei estabelece que compete ao profissional de Educação Física coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos especializados nas áreas de atividades físicas e do desporto.",
            legalTE: "⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO\nConforme a Lei nº 14.597/2023 (Lei Geral do Esporte), Art. 75, a profissão de treinador esportivo é reconhecida e regulada no Brasil, com atuação de caráter técnico e esportivo voltada à preparação, supervisão, orientação e acompanhamento de treinos físicos e esportivos. A atuação possui finalidade orientativa e não substitui avaliação médica, diagnóstico clínico, prescrição medicamentosa ou acompanhamento de profissionais da saúde. Em casos de doenças, lesões, gestação, limitações físicas, uso de medicações ou qualquer condição específica de saúde, recomenda-se avaliação prévia por profissional habilitado antes do início ou continuidade da prática de exercícios. O treinamento proposto respeita os limites individuais, priorizando segurança, execução correta e evolução progressiva, dentro da atuação técnica e esportiva permitida por lei. As recomendações desta ficha possuem caráter informativo e orientativo, voltadas ao treinamento esportivo e prática de musculação."
        };

        // --- 3. ESTADO GLOBAL DA APLICAÇÃO ---
        window.STATE = {
            user: null, network: null, units: [], members: [], 
            workouts: [], currentFichaId: null, activeWorkoutId: null, activeCategory: "🔥 PEITO"
        };

        // --- 4. FUNÇÕES DE UTILIDADE E UI ---
        const $ = id => document.getElementById(id);
        const showLoader = (text="Aguarde...") => { $('loader-text').innerText = text; $('global-loader').classList.remove('hidden'); };
        const hideLoader = () => $('global-loader').classList.add('hidden');
        const genId = () => Math.random().toString(36).substr(2, 9);
        
        window.openModal = id => $(id).classList.remove('hidden');
        window.closeModal = id => $(id).classList.add('hidden');
        
        window.toggleNewMemberCref = () => {
            const t = $('new-member-type').value;
            $('new-member-cref-container').style.display = t === 'TE' ? 'none' : 'grid';
        };
        
        window.changeTheme = () => document.body.setAttribute('data-theme', $('stu-gender').value);
        
        window.calculateIMC = () => {
            const w = parseFloat($('stu-weight').value), h = parseFloat($('stu-height').value);
            if(w>0 && h>0) {
                const imc = (w/(h*h)).toFixed(1);
                let cls = imc<18.5?"Abaixo do Peso":imc<24.9?"Normal":imc<29.9?"Sobrepeso":"Obesidade";
                $('imc-display').innerHTML = `<span class="bg-primary bg-opacity-20 text-primary px-3 py-1 rounded-lg text-[10px] font-bold uppercase tracking-wide border border-primary border-opacity-30">IMC: ${imc} (${cls})</span>`;
            } else $('imc-display').innerHTML = '';
        };

        // --- 5. AUTENTICAÇÃO E SETUP DE REDE ---
        onAuthStateChanged(auth, async (user) => {
            if (user) {
                STATE.user = user;
                $('login-screen').classList.add('hidden');
                showLoader("Carregando Dados da Franquia...");
                await loadNetworkData();
                hideLoader();
            } else {
                $('login-screen').classList.remove('hidden');
                $('setup-screen').classList.add('hidden');
                $('app-screen').classList.add('hidden');
            }
        });

        $('btn-login').onclick = () => signInWithPopup(auth, new GoogleAuthProvider()).catch(e=>alert("Erro login: "+e.message));
        $('btn-logout').onclick = () => signOut(auth);

        async function loadNetworkData() {
            try {
                // TUDO É SALVO DENTRO DA PASTA DO SEU USUÁRIO NO FIREBASE!
                const netDoc = await getDoc(doc(db, "users", STATE.user.uid));
                if (!netDoc.exists() || !netDoc.data().networkName) {
                    $('setup-screen').classList.remove('hidden');
                } else {
                    STATE.network = netDoc.data();
                    STATE.units = STATE.network.units || [];
                    const memSnap = await getDocs(collection(db, "users", STATE.user.uid, "members"));
                    STATE.members = memSnap.docs.map(d => ({id: d.id, ...d.data()}));
                    initAppUI();
                }
            } catch (e) { alert("Erro ao carregar rede: " + e.message); }
        }

        $('btn-save-setup').onclick = async () => {
            const netName = $('setup-network-name').value.trim();
            const unitName = $('setup-unit-name').value.trim();
            if(!netName || !unitName) return alert("Preencha o Nome da Rede e da Primeira Unidade.");
            
            showLoader("Estruturando Franquia...");
            const initUnit = { id: genId(), name: unitName };
            await setDoc(doc(db, "users", STATE.user.uid), {
                networkName: netName, ownerEmail: STATE.user.email, units: [initUnit], createdAt: new Date()
            }, {merge: true});
            $('setup-screen').classList.add('hidden');
            await loadNetworkData();
            hideLoader();
        };

        // --- 6. GESTÃO DE EQUIPE E UNIDADES ---
        window.addNewUnit = async () => {
            const name = $('new-unit-name').value.trim();
            if(!name) return;
            showLoader("Salvando Unidade...");
            const newUnit = { id: genId(), name };
            STATE.units.push(newUnit);
            await setDoc(doc(db, "users", STATE.user.uid), { units: STATE.units }, {merge: true});
            $('new-unit-name').value = '';
            updateNavSelects();
            hideLoader();
        };

        window.addNewMember = async () => {
            const name = $('new-member-name').value.trim();
            const type = $('new-member-type').value;
            const unitId = $('new-member-unit').value;
            const cref = type === 'PEF' ? $('new-member-cref').value.trim() : '';
            const stateUf = type === 'PEF' ? $('new-member-state').value.trim() : '';
            
            if(!name || !unitId) return alert("Nome do treinador e Unidade são obrigatórios.");
            if(type === 'PEF' && (!cref || !stateUf)) return alert("CREF e Estado são obrigatórios para a Categoria PEF.");

            showLoader("Cadastrando Profissional...");
            const memberData = { unitId, name, type, cref, state: stateUf, active: true };
            const docRef = await addDoc(collection(db, "users", STATE.user.uid, "members"), memberData);
            STATE.members.push({id: docRef.id, ...memberData});
            
            $('new-member-name').value = ''; $('new-member-cref').value = ''; $('new-member-state').value = '';
            updateNavSelects();
            hideLoader();
            closeModal('modal-manage');
        };

        function initAppUI() {
            $('app-screen').classList.remove('hidden');
            $('nav-network-name').innerText = STATE.network.networkName;
            updateNavSelects();
            $('nav-select-member').addEventListener('change', updateHealthOptions);
        }

        function updateNavSelects() {
            const sUnit = $('nav-select-unit'); const sMem = $('nav-select-member'); const sNewMemUnit = $('new-member-unit');
            const prevUnit = sUnit.value;
            const prevMem = sMem.value;

            sUnit.innerHTML = STATE.units.map(u => `<option value="${u.id}">${u.name}</option>`).join('');
            sNewMemUnit.innerHTML = sUnit.innerHTML;
            if(prevUnit && STATE.units.find(u=>u.id===prevUnit)) sUnit.value = prevUnit;

            const updateMembers = () => {
                const unitMembers = STATE.members.filter(m => m.unitId === sUnit.value);
                if(unitMembers.length === 0) {
                    sMem.innerHTML = `<option value="">-- Sem equipe --</option>`;
                    $('alert-no-member').classList.remove('hidden');
                } else {
                    sMem.innerHTML = unitMembers.map(m => `<option value="${m.id}">${m.name} (${m.type})</option>`).join('');
                    if(prevMem && unitMembers.find(m=>m.id===prevMem)) sMem.value = prevMem;
                    $('alert-no-member').classList.add('hidden');
                }
                updateHealthOptions();
            };
            sUnit.removeEventListener('change', updateMembers);
            sUnit.addEventListener('change', updateMembers);
            updateMembers();
        }

        function updateHealthOptions() {
            const member = STATE.members.find(m => m.id === $('nav-select-member').value);
            const dataObj = (member && member.type === 'TE') ? D.healthTE : D.healthPEF;
            const selected = Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value);
            
            $('health-container').innerHTML = Object.keys(dataObj).map(opt => {
                const checked = selected.includes(opt) ? 'checked' : '';
                return `
                <label class="flex items-start space-x-2 cursor-pointer p-1.5 rounded-lg hover:bg-black hover:bg-opacity-20 transition border border-transparent hover:border-gray-600">
                    <input type="checkbox" value="${opt}" ${checked} class="health-cb mt-1 rounded border-gray-500 text-primary bg-black/20">
                    <div class="flex-1">
                        <strong class="block text-[11px]">${opt}</strong>
                        <span class="block text-[9px] opacity-70 leading-tight mt-0.5">${dataObj[opt]}</span>
                    </div>
                </label>`;
            }).join('');
        }

        // --- 7. MONTAGEM DE TREINOS E EXERCÍCIOS ---
        window.addDay = (dayName) => {
            STATE.workouts.push({ id: genId(), title: dayName.toUpperCase(), exercises: [] });
            renderWorkouts();
        };

        window.duplicateWorkout = (id) => {
            const w = STATE.workouts.find(x => x.id === id);
            if(w) { STATE.workouts.push({ ...JSON.parse(JSON.stringify(w)), id: genId(), title: w.title+" (CÓPIA)" }); renderWorkouts(); }
        };

        window.removeWorkout = (id) => {
            if(confirm("Deseja realmente remover esta ficha do dia?")) { STATE.workouts = STATE.workouts.filter(w => w.id !== id); renderWorkouts(); }
        };

        window.updateWorkoutTitle = (id, val) => { const w = STATE.workouts.find(x => x.id === id); if(w) w.title = val; };

        // Operações em Exercícios
        window.removeEx = (wId, idx) => { STATE.workouts.find(w=>w.id===wId).exercises.splice(idx,1); renderWorkouts(); };
        window.moveEx = (wId, idx, dir) => {
            const arr = STATE.workouts.find(w=>w.id===wId).exercises;
            if(dir==='up' && idx>0) { [arr[idx], arr[idx-1]] = [arr[idx-1], arr[idx]]; }
            if(dir==='down' && idx<arr.length-1) { [arr[idx], arr[idx+1]] = [arr[idx+1], arr[idx]]; }
            renderWorkouts();
        };
        window.updateEx = (wId, idx, fld, val) => { STATE.workouts.find(w=>w.id===wId).exercises[idx][fld] = val; };

        function renderWorkouts() {
            const container = $('workouts-container'); container.innerHTML = '';
            STATE.workouts.forEach(w => {
                let exs = w.exercises.length === 0 ? `<div class="text-center p-6 text-xs opacity-50 font-medium tracking-wide uppercase">Nenhum exercício lançado nesta ficha.</div>` : w.exercises.map((ex, i) => `
                    <div class="flex flex-col md:flex-row gap-3 p-3 items-start md:items-center border-b border-gray-600 border-opacity-30 last:border-0 hover:bg-black hover:bg-opacity-10 transition">
                        <div class="flex md:flex-col gap-1 hidden md:flex">
                            <button onclick="moveEx('${w.id}',${i},'up')" class="text-[10px] p-1.5 rounded-lg hover:bg-white hover:bg-opacity-10 text-gray-400 hover:text-white transition"><i class="fas fa-chevron-up"></i></button>
                            <button onclick="moveEx('${w.id}',${i},'down')" class="text-[10px] p-1.5 rounded-lg hover:bg-white hover:bg-opacity-10 text-gray-400 hover:text-white transition"><i class="fas fa-chevron-down"></i></button>
                        </div>
                        <div class="flex-1 w-full md:w-auto">
                            <div class="text-[9px] opacity-60 uppercase font-bold tracking-widest text-primary">${ex.category}</div>
                            <div class="font-bold text-sm leading-tight">${ex.name}</div>
                        </div>
                        <div class="flex flex-wrap gap-2 w-full md:w-auto items-center">
                            <input type="text" value="${ex.sets}" onchange="updateEx('${w.id}',${i},'sets',this.value)" class="input-field w-14 text-center rounded-lg p-2 text-xs font-bold" placeholder="Séries">
                            <span class="opacity-40 text-xs font-bold">X</span>
                            <input type="text" value="${ex.reps}" onchange="updateEx('${w.id}',${i},'reps',this.value)" class="input-field w-20 text-center rounded-lg p-2 text-xs font-bold" placeholder="Reps">
                            <select onchange="updateEx('${w.id}',${i},'technique',this.value)" class="input-field w-32 rounded-lg p-2 text-xs font-medium">
                                ${D.techniques.map(t=>`<option ${ex.technique===t?'selected':''}>${t}</option>`).join('')}
                            </select>
                            <input type="text" value="${ex.obs}" onchange="updateEx('${w.id}',${i},'obs',this.value)" class="input-field flex-1 md:w-48 rounded-lg p-2 text-xs" placeholder="Observações livres">
                        </div>
                        <div class="flex w-full md:w-auto justify-end mt-2 md:mt-0">
                            <button onclick="removeEx('${w.id}',${i})" class="text-red-400 hover:bg-red-500 hover:text-white hover:bg-opacity-20 p-2 rounded-lg text-sm transition"><i class="fas fa-trash"></i></button>
                        </div>
                    </div>
                `).join('');

                container.innerHTML += `
                    <div class="card rounded-2xl overflow-hidden shadow-md border border-gray-600 border-opacity-30">
                        <div class="p-3 bg-black bg-opacity-30 flex justify-between items-center border-b border-gray-600 border-opacity-30">
                            <input type="text" value="${w.title}" onchange="updateWorkoutTitle('${w.id}',this.value)" class="bg-transparent border-none outline-none font-black text-lg uppercase tracking-wide w-full md:w-1/2 focus:ring-2 focus:ring-primary rounded px-2">
                            <div class="flex gap-2">
                                <button onclick="duplicateWorkout('${w.id}')" class="px-3 py-1.5 text-xs font-bold bg-white/5 hover:bg-white/10 rounded-lg transition"><i class="fas fa-copy mr-1"></i> Duplicar</button>
                                <button onclick="removeWorkout('${w.id}')" class="px-3 py-1.5 text-xs font-bold text-red-400 bg-red-500/10 hover:bg-red-500 hover:text-white rounded-lg transition"><i class="fas fa-trash"></i></button>
                            </div>
                        </div>
                        <div class="p-1">${exs}</div>
                        <div class="p-3 bg-black bg-opacity-20 border-t border-gray-600 border-opacity-30">
                            <button onclick="openExModal('${w.id}')" class="w-full text-center py-2.5 rounded-xl border border-dashed border-gray-500/50 hover:border-primary hover:text-primary hover:bg-primary hover:bg-opacity-5 font-bold text-sm transition flex justify-center items-center gap-2">
                                <i class="fas fa-plus-circle"></i> Adicionar Exercício
                            </button>
                        </div>
                    </div>`;
            });
        }

        window.openExModal = wId => { STATE.activeWorkoutId = wId; openModal('modal-exercises'); renderExCats(); renderExList(); };
        const renderExCats = () => $('modal-cats').innerHTML = Object.keys(D.categories).map(c=>`<button onclick="setExCat('${c}')" class="w-full text-left p-3 rounded-lg text-xs font-bold transition ${STATE.activeCategory===c?'bg-primary text-white shadow-md':'hover:bg-white/5 text-gray-300'}">${c}</button>`).join('');
        window.setExCat = c => { STATE.activeCategory = c; renderExCats(); renderExList(); };
        const renderExList = () => {
            const isCardio = STATE.activeCategory === "🫀 CARDIO";
            $('modal-ex-list').innerHTML = `<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-3">${D.categories[STATE.activeCategory].map(ex=>`<button onclick="addEx('${ex}',${isCardio})" class="card p-3 rounded-xl text-xs text-left font-bold border border-gray-600/30 hover:border-primary hover:text-primary transition shadow-sm flex justify-between items-center group"><span>${ex}</span> <i class="fas fa-plus opacity-0 group-hover:opacity-100 transition"></i></button>`).join('')}</div>`;
        };
        window.addEx = (name, isCardio) => {
            STATE.workouts.find(w=>w.id===STATE.activeWorkoutId).exercises.push({ category: STATE.activeCategory, name, sets: isCardio?'1':'3', reps: isCardio?'-':'10 a 12', technique: 'Nenhuma', obs: '' });
            renderWorkouts();
        };

        // --- 8. SALVAR NA NUVEM E IMPRIMIR ---
        window.generatePrint = async () => {
            const memberId = $('nav-select-member').value;
            if(!memberId) return alert("Erro: Cadastre ou selecione a sua Unidade e Nome no menu superior 'Equipe' antes de gerar a ficha.");
            const member = STATE.members.find(m => m.id === memberId);
            const unit = STATE.units.find(u => u.id === $('nav-select-unit').value);

            if(STATE.workouts.length === 0) return alert("Adicione pelo menos um treino na ficha.");

            const w = parseFloat($('stu-weight').value), h = parseFloat($('stu-height').value);
            const imcStr = (w>0 && h>0) ? (w/(h*h)).toFixed(1) : "-";
            const health = Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value);
            
            const now = new Date();
            const reportKey = `${now.getFullYear()}-${String(now.getMonth()+1).padStart(2, '0')}`;

            const data = {
                unitId: unit.id, memberId: member.id, reportKey,
                profName: member.name, profType: member.type, profCref: member.cref || '', profState: member.state || '',
                studentName: $('stu-name').value || 'Aluno Não Informado', stuAge: $('stu-age').value, stuWeight: $('stu-weight').value, stuHeight: $('stu-height').value,
                stuGender: $('stu-gender').value, stuLevel: $('stu-level').value, stuObjective: $('stu-objective').value,
                stuFreq: $('stu-freq').value, stuValidity: $('stu-validity').value, stuRecs: $('stu-recs').value,
                health, workouts: STATE.workouts,
                createdAt: now, updatedAt: now
            };

            showLoader("Salvando Ficha na Nuvem...");
            try {
                if(STATE.currentFichaId) { 
                    await setDoc(doc(db, "users", STATE.user.uid, "workouts", STATE.currentFichaId), data, {merge: true}); 
                } else { 
                    const ref = await addDoc(collection(db, "users", STATE.user.uid, "workouts"), data); 
                    STATE.currentFichaId = ref.id; 
                }
                hideLoader();
                
                // Construção Dinâmica da Impressão
                const isPEF = member.type === 'PEF';
                const healthDict = isPEF ? D.healthPEF : D.healthTE;
                const legalTxt = isPEF ? D.legalPEF : D.legalTE;
                const profLabel = isPEF ? 'Profissional de Educação Física' : 'Treinador Esportivo';
                const crefHtml = isPEF ? `<br><span style="font-weight:normal">CREF: ${member.cref} - Estado: ${member.state}</span>` : '';

                let html = `
                    <div class="print-header">
                        <h1 class="print-title">PLANILHA DE TREINAMENTO</h1>
                        <div class="prof-info">Prescrição feita por: ${member.name} (${profLabel})${crefHtml}</div>
                    </div>
                    
                    <div class="print-grid">
                        <div><strong>Aluno:</strong> ${data.studentName}</div>
                        <div><strong>Idade:</strong> ${data.stuAge||'-'} anos</div>
                        <div><strong>Gênero:</strong> ${data.stuGender}</div>
                        <div><strong>Peso:</strong> ${data.stuWeight||'-'} kg</div>
                        <div><strong>Altura:</strong> ${data.stuHeight||'-'} m</div>
                        <div><strong>IMC:</strong> ${imcStr}</div>
                        <div><strong>Objetivo:</strong> ${data.stuObjective}</div>
                        <div><strong>Nível:</strong> ${data.stuLevel}</div>
                        <div><strong>Frequência:</strong> ${data.stuFreq}</div>
                    </div>`;

                if(D.objectives[data.stuObjective] || health.length > 0) {
                    html += `<div class="print-guidelines"><h4>Diretrizes de Perfil e Estado de Saúde</h4><ul>`;
                    if(D.objectives[data.stuObjective]) html += `<li><strong>Objetivo (${data.stuObjective}):</strong> ${D.objectives[data.stuObjective]}</li>`;
                    health.forEach(k => { if(healthDict[k]) html += `<li><strong>${k.replace(/[^\w\sÀ-ú]/g,'').trim()}:</strong> ${healthDict[k]}</li>`; });
                    html += `</ul></div>`;
                }

                data.workouts.forEach(wk => {
                    html += `<div class="print-workout"><h3>${wk.title}</h3><table>
                        <thead><tr><th style="width:35%">Exercício</th><th style="width:8%; text-align:center;">Séries</th><th style="width:12%; text-align:center;">Reps</th><th style="width:15%">Técnica</th><th style="width:30%">Observações</th></tr></thead><tbody>`;
                    if(wk.exercises.length===0) html += `<tr><td colspan="5" style="text-align:center;font-style:italic;padding:8px">Sem exercícios preenchidos</td></tr>`;
                    else wk.exercises.forEach(ex => html += `<tr><td><strong>${ex.name}</strong></td><td style="text-align:center;">${ex.sets}</td><td style="text-align:center;">${ex.reps}</td><td>${ex.technique!=='Nenhuma'?ex.technique:'-'}</td><td>${ex.obs||'-'}</td></tr>`);
                    html += `</tbody></table></div>`;
                });

                html += `<div class="print-footer-section">`;
                if(data.stuRecs) html += `<strong>Recomendações Específicas do Profissional:</strong><p style="white-space:pre-wrap; margin:3px 0 10px 0">${data.stuRecs}</p>`;
                html += `<div class="legal-text">${legalTxt}</div><div style="text-align:center; margin-top:15px; font-size:7px; text-transform:uppercase;">Validade da Ficha: ${data.stuValidity.replace('Validade: ','')} | PowFit Pro - Impresso em ${now.toLocaleDateString('pt-BR')} às ${now.toLocaleTimeString('pt-BR',{hour:'2-digit',minute:'2-digit'})}</div></div>`;

                $('print-area').innerHTML = html;
                setTimeout(() => window.print(), 400);

            } catch(e) { hideLoader(); alert("Erro ao salvar ficha: " + e.message); }
        };

        // --- 9. HISTÓRICO E RELATÓRIO DE PRODUTIVIDADE ---
        window.openHistory = async () => {
            showLoader("Buscando Histórico Cloud...");
            openModal('modal-history');
            const q = collection(db, "users", STATE.user.uid, "workouts");
            try {
                const snap = await getDocs(q);
                // Ordenar no cliente
                window.HISTORY_DATA = snap.docs.map(d => ({id: d.id, ...d.data()})).sort((a,b) => b.createdAt - a.createdAt);
                renderHistoryList();
            } catch(e) { alert("Erro ao carregar histórico: "+e.message); }
            hideLoader();
        };

        function renderHistoryList() {
            $('history-title').innerHTML = '<i class="fas fa-cloud text-primary mr-2"></i> Fichas Salvas na Nuvem';
            $('btn-toggle-report').innerHTML = '<i class="fas fa-chart-pie"></i> Relatório Produtividade';
            $('btn-toggle-report').onclick = window.loadReportMode;

            const cont = $('history-content');
            if(window.HISTORY_DATA.length === 0) return cont.innerHTML = `<p class="opacity-50 text-center py-10 font-bold tracking-widest uppercase">O Histórico está Vazio.</p>`;
            
            cont.innerHTML = window.HISTORY_DATA.map(h => {
                const date = h.createdAt?.toDate ? h.createdAt.toDate().toLocaleDateString('pt-BR') : 'Sem data';
                const isExpired = checkValidade(h.createdAt?.toDate(), h.stuValidity);
                const mem = STATE.members.find(m=>m.id===h.memberId)?.name || 'Profissional Excluído';
                const unit = STATE.units.find(u=>u.id===h.unitId)?.name || 'Unidade Excluída';
                
                return `<div class="p-4 mb-3 card rounded-xl flex flex-col md:flex-row justify-between md:items-center border-l-4 ${isExpired?'border-l-yellow-500':'border-l-blue-500'} hover:bg-black hover:bg-opacity-10 transition">
                    <div class="mb-3 md:mb-0">
                        <strong class="block text-md mb-1">${h.studentName}</strong>
                        <span class="text-xs opacity-70 block mb-1">Unidade: ${unit} | Prof: ${mem}</span>
                        <span class="text-[10px] bg-black bg-opacity-30 px-2 py-1 rounded font-mono">Data: ${date} | Vencimento: ${h.stuValidity}</span>
                        ${isExpired ? `<span class="text-[10px] text-yellow-500 bg-yellow-500 bg-opacity-10 px-2 py-1 rounded font-bold ml-2 border border-yellow-500/50"><i class="fas fa-exclamation-triangle"></i> VENCIDA</span>` : ''}
                    </div>
                    <div class="flex gap-2">
                        <button onclick="loadFicha('${h.id}')" class="btn-primary text-xs px-4 py-2 rounded-lg font-bold shadow-md">EDITAR/IMPRIMIR</button>
                        <button onclick="deleteFicha('${h.id}')" class="bg-red-900 text-white text-xs px-3 py-2 rounded-lg font-bold shadow-md hover:bg-red-700 transition" title="Excluir da Nuvem"><i class="fas fa-trash"></i></button>
                    </div>
                </div>`;
            }).join('');
        }

        window.deleteFicha = async (id) => {
            if(confirm("Excluir definitivamente esta ficha da nuvem? Esta ação não pode ser desfeita.")) {
                showLoader("Excluindo...");
                try {
                    await deleteDoc(doc(db, "users", STATE.user.uid, "workouts", id));
                    if(STATE.currentFichaId === id) STATE.currentFichaId = null;
                    await window.openHistory(); // refresh a lista
                } catch(e) { alert("Erro ao excluir: " + e.message); }
                hideLoader();
            }
        };

        function checkValidade(dateObj, validadeStr) {
            if(!dateObj) return false;
            const days = parseInt(validadeStr);
            if(isNaN(days)) return false;
            const diffDays = Math.ceil(Math.abs(new Date() - dateObj) / (1000 * 60 * 60 * 24)); 
            return diffDays > days;
        }

        window.loadFicha = id => {
            const f = window.HISTORY_DATA.find(x => x.id === id); if(!f) return;
            STATE.currentFichaId = f.id;
            $('nav-select-unit').value = f.unitId;
            updateNavSelects();
            setTimeout(() => {
                if(f.memberId) $('nav-select-member').value = f.memberId;
                updateHealthOptions();
                
                $('stu-name').value = f.studentName||''; $('stu-age').value = f.stuAge||''; $('stu-weight').value = f.stuWeight||'';
                $('stu-height').value = f.stuHeight||''; $('stu-gender').value = f.stuGender||'Masculino';
                $('stu-level').value = f.stuLevel||'Iniciante'; $('stu-objective').value = f.stuObjective||'Emagrecimento';
                $('stu-freq').value = f.stuFreq||'3 dias'; $('stu-validity').value = f.stuValidity||'30 dias'; $('stu-recs').value = f.stuRecs||'';
                
                document.querySelectorAll('.health-cb').forEach(cb => cb.checked = (f.health||[]).includes(cb.value));
                STATE.workouts = f.workouts || [];
                renderWorkouts(); calculateIMC(); changeTheme();
                closeModal('modal-history');
                
                window.scrollTo({top:0, behavior:'smooth'});
            }, 100);
        };

        window.openReports = () => { window.openHistory(); setTimeout(window.loadReportMode, 500); };
        
        window.loadReportMode = () => {
            const btn = $('btn-toggle-report');
            btn.innerHTML = '<i class="fas fa-arrow-left"></i> Voltar ao Histórico';
            btn.onclick = renderHistoryList;
            $('history-title').innerHTML = '<i class="fas fa-chart-line text-primary mr-2"></i> Relatório de Produtividade Mensal';
            
            const now = new Date();
            const currentReportKey = `${now.getFullYear()}-${String(now.getMonth()+1).padStart(2, '0')}`;
            const monthName = now.toLocaleString('pt-BR', { month: 'long', year: 'numeric' }).toUpperCase();
            
            const counts = {}; 
            const unitTotals = {}; 

            window.HISTORY_DATA.forEach(w => {
                if(w.reportKey === currentReportKey) {
                    const k = `${w.unitId}_${w.memberId}`;
                    counts[k] = (counts[k]||0) + 1;
                    unitTotals[w.unitId] = (unitTotals[w.unitId]||0) + 1;
                }
            });

            let html = `
                <div class="flex justify-between items-center mb-6 border-b border-gray-600 border-opacity-30 pb-4">
                    <div>
                        <h4 class="text-xl font-bold text-primary">Referência: ${monthName}</h4>
                        <p class="text-xs opacity-60">Volume de fichas criadas por treinador e por unidade neste mês.</p>
                    </div>
                    <button onclick="printReport('${monthName}')" class="btn-primary px-4 py-2.5 rounded-lg text-sm font-bold flex items-center gap-2 shadow-lg">
                        <i class="fas fa-print"></i> Imprimir Relatório
                    </button>
                </div>
                <div id="report-view-data" class="space-y-6">`;
            
            let globTotal = 0;
            STATE.units.forEach(u => {
                const uTotal = unitTotals[u.id] || 0;
                globTotal += uTotal;
                
                html += `<div class="card p-5 rounded-2xl border border-gray-600 border-opacity-20 shadow-sm bg-black bg-opacity-20">
                            <div class="flex justify-between items-end border-b border-gray-600 border-opacity-40 pb-2 mb-4">
                                <h4 class="font-bold text-lg"><i class="fas fa-store text-primary mr-2"></i>${u.name}</h4>
                                <span class="text-xs font-bold bg-primary text-white px-3 py-1 rounded-full shadow">Total: ${uTotal} Fichas</span>
                            </div>`;
                
                const mems = STATE.members.filter(m => m.unitId === u.id);
                let hasData = false;
                mems.forEach(m => {
                    const total = counts[`${u.id}_${m.id}`] || 0;
                    if(total > 0) { 
                        html += `<div class="flex justify-between items-center text-sm py-2.5 border-b border-gray-600 border-opacity-20 last:border-0 hover:bg-white hover:bg-opacity-5 px-2 rounded transition">
                            <span class="font-medium">${m.name} <span class="text-[10px] opacity-50 ml-2 border border-gray-500 rounded px-1">${m.type}</span></span>
                            <span class="font-black text-primary">${total} <span class="text-[10px] font-normal opacity-60">fichas criadas</span></span>
                        </div>`; 
                        hasData = true; 
                    }
                });
                if(!hasData) html += `<p class="text-[11px] opacity-40 font-bold uppercase tracking-widest text-center py-2">Sem produção nesta unidade neste mês.</p>`;
                html += `</div>`;
            });
            html += `</div>`;
            
            if(globTotal === 0) {
                html = `<div class="text-center py-10"><i class="fas fa-folder-open text-4xl opacity-20 mb-3"></i><p class="font-bold opacity-50 uppercase tracking-widest">Nenhuma ficha criada em ${monthName}.</p></div>`;
            }

            window.CURRENT_REPORT_HTML = buildPrintReportHTML(monthName, counts, unitTotals, globTotal);
            $('history-content').innerHTML = html;
        };

        function buildPrintReportHTML(monthName, counts, unitTotals, globTotal) {
            let p = `
                <div class="print-header" style="margin-bottom:20px;">
                    <h1 class="print-title">RELATÓRIO DE PRODUTIVIDADE GERAL</h1>
                    <div class="prof-info">Rede: ${STATE.network.networkName} | Período: ${monthName}</div>
                </div>
                <div style="font-size: 14px; margin-bottom: 20px;"><strong>Volume Total da Rede no Mês:</strong> ${globTotal} Fichas Geradas.</div>
            `;
            
            STATE.units.forEach(u => {
                const uTotal = unitTotals[u.id] || 0;
                if(uTotal > 0) {
                    p += `<div class="report-unit-header">Unidade: ${u.name} (Total: ${uTotal})</div>
                          <table class="report-table" style="margin-bottom: 20px;">
                              <thead><tr><th style="width:70%">Treinador Responsável</th><th style="width:30%; text-align:center;">Fichas Criadas</th></tr></thead><tbody>`;
                    
                    const mems = STATE.members.filter(m => m.unitId === u.id);
                    mems.forEach(m => {
                        const total = counts[`${u.id}_${m.id}`] || 0;
                        if(total > 0) p += `<tr><td>${m.name} (${m.type})</td><td style="text-align:center;">${total}</td></tr>`;
                    });
                    p += `</tbody></table>`;
                }
            });
            p += `<div style="text-align:center; margin-top:30px; font-size:10px;">Documento Gerado Oficialmente via Plataforma PowFit Pro.</div>`;
            return p;
        }

        window.printReport = () => {
            if(!window.CURRENT_REPORT_HTML) return;
            $('print-area').innerHTML = window.CURRENT_REPORT_HTML;
            setTimeout(() => window.print(), 300);
        };

    </script>
</body>
</html>
