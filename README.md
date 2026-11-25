<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculadora de Notas - RS</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #111827; /* gray-900 */
            color: #f3f4f6; /* gray-100 */
        }
        /* Custom scrollbar for better look */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #1f2937; 
        }
        ::-webkit-scrollbar-thumb {
            background: #4b5563; 
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #6b7280; 
        }
        
        /* Hide arrows on number inputs */
        input[type="number"]::-webkit-outer-spin-button,
        input[type="number"]::-webkit-inner-spin-button {
            -webkit-appearance: none;
            margin: 0;
        }
        input[type="number"] {
            -moz-appearance: textfield;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col antialiased">

    <!-- Header -->
    <header class="bg-gray-800 shadow-md py-4 px-6 sticky top-0 z-10">
        <div class="max-w-3xl mx-auto flex items-center justify-center">
            <svg class="w-8 h-8 text-blue-500 mr-3" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 7h6m0 10v-3m-3 3h.01M9 17h.01M9 14h.01M12 14h.01M15 11h.01M12 11h.01M9 11h.01M7 21h10a2 2 0 002-2V5a2 2 0 00-2-2H7a2 2 0 00-2 2v14a2 2 0 002 2z"></path></svg>
            <h1 class="text-2xl font-bold text-center bg-clip-text text-transparent bg-gradient-to-r from-blue-400 to-emerald-400">Calculadora Escolar</h1>
        </div>
    </header>

    <!-- Main Content -->
    <main class="flex-grow max-w-3xl w-full mx-auto p-4 sm:p-6 lg:p-8">
        
        <!-- Navigation Tabs -->
        <div class="flex space-x-2 bg-gray-800 p-1 rounded-xl mb-6 shadow-lg">
            <button id="btn-annual" onclick="switchTab('annual')" class="flex-1 py-2.5 text-sm font-medium rounded-lg transition-colors bg-blue-600 text-white shadow">
                Aprovação Anual
            </button>
            <button id="btn-trimester" onclick="switchTab('trimester')" class="flex-1 py-2.5 text-sm font-medium rounded-lg transition-colors text-gray-400 hover:text-white hover:bg-gray-700">
                Média do Trimestre
            </button>
        </div>

        <!-- TAB 1: Calculadora Anual -->
        <div id="tab-annual" class="bg-gray-800 rounded-2xl shadow-xl p-6 border border-gray-700 transition-all duration-300">
            <h2 class="text-xl font-semibold mb-2 text-blue-400">Status do Ano Letivo</h2>
            <p class="text-sm text-gray-400 mb-6">Insira suas notas para calcular quantos pontos faltam para atingir os 18.0 pontos (média 6.0).</p>

            <div class="grid grid-cols-1 md:grid-cols-3 gap-5 mb-8">
                <!-- Trimestre 1 -->
                <div class="bg-gray-700/50 p-4 rounded-xl border border-gray-600 focus-within:border-blue-500 transition-colors">
                    <label class="block text-sm font-medium text-gray-300 mb-2">1º Trimestre</label>
                    <input type="number" id="t1" min="0" max="10" step="0.1" placeholder="Ex: 6.5" 
                        class="w-full bg-transparent text-2xl font-bold text-white placeholder-gray-500 outline-none text-center"
                        oninput="validateInput(this); calculateAnnual()">
                </div>
                
                <!-- Trimestre 2 -->
                <div class="bg-gray-700/50 p-4 rounded-xl border border-gray-600 focus-within:border-blue-500 transition-colors">
                    <label class="block text-sm font-medium text-gray-300 mb-2">2º Trimestre</label>
                    <input type="number" id="t2" min="0" max="10" step="0.1" placeholder="Ex: 5.0" 
                        class="w-full bg-transparent text-2xl font-bold text-white placeholder-gray-500 outline-none text-center"
                        oninput="validateInput(this); calculateAnnual()">
                </div>

                <!-- Trimestre 3 -->
                <div class="bg-gray-700/50 p-4 rounded-xl border border-gray-600 focus-within:border-blue-500 transition-colors">
                    <label class="block text-sm font-medium text-gray-300 mb-2">3º Trimestre</label>
                    <input type="number" id="t3" min="0" max="10" step="0.1" placeholder="Ex: -" 
                        class="w-full bg-transparent text-2xl font-bold text-white placeholder-gray-500 outline-none text-center"
                        oninput="validateInput(this); calculateAnnual()">
                </div>
            </div>

            <!-- Result Box -->
            <div id="annual-result-box" class="rounded-xl p-5 border border-gray-600 bg-gray-700/30 text-center flex flex-col items-center justify-center min-h-[120px]">
                <p id="annual-status" class="text-lg font-medium text-gray-300">Preencha suas notas para ver o resultado.</p>
                <p id="annual-substatus" class="text-sm text-gray-400 mt-2"></p>
            </div>
            
            <div class="mt-4 flex justify-end">
                <button onclick="clearAnnual()" class="text-sm text-gray-400 hover:text-red-400 transition-colors flex items-center">
                    <svg class="w-4 h-4 mr-1" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"></path></svg>
                    Limpar Dados
                </button>
            </div>
        </div>

        <!-- TAB 2: Calculadora de Média do Trimestre -->
        <div id="tab-trimester" class="hidden bg-gray-800 rounded-2xl shadow-xl p-6 border border-gray-700 transition-all duration-300">
            <div class="flex justify-between items-start mb-6">
                <div>
                    <h2 class="text-xl font-semibold text-emerald-400">Simular Média do Trimestre</h2>
                    <p class="text-sm text-gray-400 mt-1">Calcule a média ponderada de provas e trabalhos de um trimestre.</p>
                </div>
                <div class="bg-gray-900 rounded-lg p-3 text-center min-w-[100px] border border-gray-700 shadow-inner">
                    <span class="block text-xs text-gray-400 uppercase tracking-wide">Média Final</span>
                    <span id="trimester-result" class="text-2xl font-bold text-white">0.0</span>
                </div>
            </div>

            <!-- Headers da Lista -->
            <div class="grid grid-cols-12 gap-3 mb-2 px-2 text-xs font-medium text-gray-400 uppercase tracking-wider">
                <div class="col-span-6 sm:col-span-7">Nota Obtida</div>
                <div class="col-span-4 sm:col-span-3 text-center">Peso</div>
                <div class="col-span-2 text-center">Ação</div>
            </div>

            <!-- Lista de Notas -->
            <div id="grades-list" class="space-y-3 mb-6">
                <!-- Linhas inseridas via JS -->
            </div>

            <div class="flex flex-col sm:flex-row gap-3">
                <button onclick="addGradeRow()" class="flex-1 flex items-center justify-center py-3 bg-gray-700 hover:bg-gray-600 text-white rounded-xl transition-colors font-medium border border-gray-600">
                    <svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6v6m0 0v6m0-6h6m-6 0H6"></path></svg>
                    Adicionar Nota
                </button>
                <button onclick="clearTrimester()" class="flex items-center justify-center py-3 px-4 bg-red-900/30 hover:bg-red-900/50 text-red-400 rounded-xl transition-colors font-medium border border-red-800/50">
                    <svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"></path></svg>
                    Limpar
                </button>
            </div>
        </div>

    </main>

    <!-- Footer -->
    <footer class="text-center py-6 text-gray-500 text-sm">
        <p>Desenvolvido para ajudar alunos a calcular suas notas. Os dados ficam salvos apenas no seu navegador.</p>
    </footer>

    <!-- JavaScript Logic -->
    <script>
        // --- 1. CONFIGURAÇÕES E ESTADO GERAL ---
        const TARGET_POINTS = 18.0;
        let trimesterGrades = [];

        // Inicialização quando a página carrega
        window.onload = function() {
            loadAnnualData();
            loadTrimesterData();
        };

        // --- 2. CONTROLE DE ABAS (SPA) ---
        function switchTab(tabId) {
            const tabAnnual = document.getElementById('tab-annual');
            const tabTrimester = document.getElementById('tab-trimester');
            const btnAnnual = document.getElementById('btn-annual');
            const btnTrimester = document.getElementById('btn-trimester');

            if (tabId === 'annual') {
                tabAnnual.classList.remove('hidden');
                tabTrimester.classList.add('hidden');
                
                btnAnnual.className = "flex-1 py-2.5 text-sm font-medium rounded-lg transition-colors bg-blue-600 text-white shadow";
                btnTrimester.className = "flex-1 py-2.5 text-sm font-medium rounded-lg transition-colors text-gray-400 hover:text-white hover:bg-gray-700";
            } else {
                tabAnnual.classList.add('hidden');
                tabTrimester.classList.remove('hidden');
                
                btnTrimester.className = "flex-1 py-2.5 text-sm font-medium rounded-lg transition-colors bg-emerald-600 text-white shadow";
                btnAnnual.className = "flex-1 py-2.5 text-sm font-medium rounded-lg transition-colors text-gray-400 hover:text-white hover:bg-gray-700";
            }
        }

        // --- 3. VALIDAÇÃO DE INPUTS ---
        function validateInput(input) {
            // Remove pontuação inválida e limita entre 0 e 10
            if (input.value !== '') {
                let val = parseFloat(input.value);
                if (val > 10) input.value = 10;
                if (val < 0) input.value = 0;
            }
        }

        // --- 4. LÓGICA DA CALCULADORA ANUAL ---
        function loadAnnualData() {
            const data = JSON.parse(localStorage.getItem('escolaRS_annual')) || { t1: '', t2: '', t3: '' };
            document.getElementById('t1').value = data.t1;
            document.getElementById('t2').value = data.t2;
            document.getElementById('t3').value = data.t3;
            calculateAnnual();
        }

        function saveAnnualData() {
            const data = {
                t1: document.getElementById('t1').value,
                t2: document.getElementById('t2').value,
                t3: document.getElementById('t3').value
            };
            localStorage.setItem('escolaRS_annual', JSON.stringify(data));
        }

        function clearAnnual() {
            document.getElementById('t1').value = '';
            document.getElementById('t2').value = '';
            document.getElementById('t3').value = '';
            calculateAnnual();
        }

        function calculateAnnual() {
            saveAnnualData(); // Salva sempre que digita

            const t1Str = document.getElementById('t1').value;
            const t2Str = document.getElementById('t2').value;
            const t3Str = document.getElementById('t3').value;

            const t1 = t1Str !== '' ? parseFloat(t1Str) : null;
            const t2 = t2Str !== '' ? parseFloat(t2Str) : null;
            const t3 = t3Str !== '' ? parseFloat(t3Str) : null;

            const statusEl = document.getElementById('annual-status');
            const substatusEl = document.getElementById('annual-substatus');
            const boxEl = document.getElementById('annual-result-box');

            // Reset box styling
            boxEl.className = "rounded-xl p-5 border text-center flex flex-col items-center justify-center min-h-[120px] transition-colors duration-300";

            let totalPoints = 0;
            let filledCount = 0;

            if (t1 !== null) { totalPoints += t1; filledCount++; }
            if (t2 !== null) { totalPoints += t2; filledCount++; }
            if (t3 !== null) { totalPoints += t3; filledCount++; }

            if (filledCount === 0) {
                statusEl.textContent = "Preencha suas notas para ver o resultado.";
                substatusEl.textContent = "São necessários 18.0 pontos no total.";
                boxEl.classList.add("border-gray-600", "bg-gray-700/30");
                statusEl.className = "text-lg font-medium text-gray-300";
                return;
            }

            const pointsNeeded = TARGET_POINTS - totalPoints;

            if (filledCount === 3) {
                // Fim de ano (3 notas)
                if (totalPoints >= TARGET_POINTS) {
                    const extra = (totalPoints - TARGET_POINTS).toFixed(1);
                    statusEl.textContent = "🎉 APROVADO!";
                    substatusEl.textContent = extra > 0 ? `Você passou com ${extra} ponto(s) de sobra. Total: ${totalPoints.toFixed(1)}` : `Passou cravado! Total: 18.0`;
                    boxEl.classList.add("border-emerald-500/50", "bg-emerald-900/20");
                    statusEl.className = "text-2xl font-bold text-emerald-400";
                } else {
                    const missing = pointsNeeded.toFixed(1);
                    statusEl.textContent = "⚠️ RECUPERAÇÃO / REPROVADO";
                    substatusEl.textContent = `Faltaram ${missing} ponto(s) para os 18.0. Seu total foi ${totalPoints.toFixed(1)}.`;
                    boxEl.classList.add("border-red-500/50", "bg-red-900/20");
                    statusEl.className = "text-2xl font-bold text-red-400";
                }
            } else if (filledCount === 2) {
                // Falta apenas 1 trimestre
                if (pointsNeeded <= 0) {
                    statusEl.textContent = "🎉 JÁ PASSOU!";
                    substatusEl.textContent = "Você já atingiu os 18 pontos antes mesmo do 3º trimestre acabar!";
                    boxEl.classList.add("border-emerald-500/50", "bg-emerald-900/20");
                    statusEl.className = "text-2xl font-bold text-emerald-400";
                } else if (pointsNeeded > 10) {
                    statusEl.textContent = "🚨 ALERTA VERMELHO";
                    substatusEl.textContent = `Você precisa de ${pointsNeeded.toFixed(1)} pontos. Como a nota máxima é 10, você já está em recuperação.`;
                    boxEl.classList.add("border-red-500/50", "bg-red-900/20");
                    statusEl.className = "text-2xl font-bold text-red-400";
                } else {
                    statusEl.textContent = `Faltam ${pointsNeeded.toFixed(1)} pontos no 3º Trimestre.`;
                    substatusEl.textContent = `Você já acumulou ${totalPoints.toFixed(1)} pontos.`;
                    boxEl.classList.add("border-blue-500/50", "bg-blue-900/20");
                    statusEl.className = "text-xl font-bold text-blue-400";
                }
            } else if (filledCount === 1) {
                // Apenas 1 nota preenchida
                if (pointsNeeded > 20) { // Impossivel, max é 10. Só pra prevenção.
                    statusEl.textContent = "Mantenha o foco!";
                } else {
                    const averageNeeded = (pointsNeeded / 2).toFixed(1);
                    statusEl.textContent = `Faltam ${pointsNeeded.toFixed(1)} pontos para aprovação.`;
                    substatusEl.textContent = `Você precisa tirar uma média de ${averageNeeded} nos próximos dois trimestres.`;
                    boxEl.classList.add("border-gray-500/50", "bg-gray-800/50");
                    statusEl.className = "text-xl font-bold text-gray-300";
                }
            }
        }

        // --- 5. LÓGICA DA CALCULADORA DE TRIMESTRE ---
        function loadTrimesterData() {
            const saved = JSON.parse(localStorage.getItem('escolaRS_trimester'));
            if (saved && saved.length > 0) {
                trimesterGrades = saved;
            } else {
                // Estado inicial: 2 linhas vazias
                trimesterGrades = [
                    { id: Date.now(), grade: '', weight: 1 },
                    { id: Date.now() + 1, grade: '', weight: 1 }
                ];
            }
            renderTrimesterRows();
        }

        function saveTrimesterData() {
            localStorage.setItem('escolaRS_trimester', JSON.stringify(trimesterGrades));
        }

        function addGradeRow() {
            trimesterGrades.push({ id: Date.now(), grade: '', weight: 1 });
            renderTrimesterRows();
            saveTrimesterData();
        }

        function removeGradeRow(id) {
            trimesterGrades = trimesterGrades.filter(item => item.id !== id);
            if (trimesterGrades.length === 0) {
                // Garante que tenha pelo menos uma linha
                trimesterGrades.push({ id: Date.now(), grade: '', weight: 1 });
            }
            renderTrimesterRows();
            calculateTrimesterAverage();
        }

        function updateTrimesterItem(id, field, value) {
            const item = trimesterGrades.find(i => i.id === id);
            if (item) {
                // Valida o input para não passar de 10 na nota
                let numVal = parseFloat(value);
                if (field === 'grade') {
                    if (numVal > 10) numVal = 10;
                    if (numVal < 0) numVal = 0;
                    item.grade = isNaN(numVal) ? '' : numVal;
                } else if (field === 'weight') {
                    if (numVal < 0) numVal = 0;
                    item.weight = isNaN(numVal) ? 1 : numVal; // default peso 1
                }
                saveTrimesterData();
                calculateTrimesterAverage();
            }
        }

        function clearTrimester() {
            trimesterGrades = [
                { id: Date.now(), grade: '', weight: 1 },
                { id: Date.now() + 1, grade: '', weight: 1 }
            ];
            renderTrimesterRows();
            calculateTrimesterAverage();
            saveTrimesterData();
        }

        function renderTrimesterRows() {
            const listEl = document.getElementById('grades-list');
            listEl.innerHTML = ''; // Limpa atual

            trimesterGrades.forEach((item) => {
                const row = document.createElement('div');
                row.className = "grid grid-cols-12 gap-3 items-center bg-gray-700/40 p-2 sm:p-3 rounded-xl border border-gray-600 focus-within:border-emerald-500 transition-colors";
                
                row.innerHTML = `
                    <div class="col-span-6 sm:col-span-7 relative">
                        <input type="number" min="0" max="10" step="0.1" placeholder="Nota" value="${item.grade !== '' ? item.grade : ''}"
                            class="w-full bg-gray-800 text-white rounded-lg p-2.5 border border-gray-600 focus:outline-none focus:border-emerald-500"
                            oninput="updateTrimesterItem(${item.id}, 'grade', this.value); this.value = this.value > 10 ? 10 : (this.value < 0 ? 0 : this.value)">
                    </div>
                    <div class="col-span-4 sm:col-span-3">
                        <input type="number" min="0" step="1" placeholder="Peso (Ex: 1)" value="${item.weight}"
                            class="w-full bg-gray-800 text-white rounded-lg p-2.5 border border-gray-600 focus:outline-none focus:border-emerald-500 text-center"
                            oninput="updateTrimesterItem(${item.id}, 'weight', this.value)">
                    </div>
                    <div class="col-span-2 flex justify-center">
                        <button onclick="removeGradeRow(${item.id})" class="p-2 text-gray-400 hover:text-red-400 hover:bg-red-900/30 rounded-lg transition-colors" title="Remover">
                            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"></path></svg>
                        </button>
                    </div>
                `;
                listEl.appendChild(row);
            });
            calculateTrimesterAverage();
        }

        function calculateTrimesterAverage() {
            let totalGradeByWeight = 0;
            let totalWeight = 0;

            trimesterGrades.forEach(item => {
                if (item.grade !== '') {
                    const grade = parseFloat(item.grade);
                    const weight = parseFloat(item.weight) || 0; // Previne NaN no peso
                    totalGradeByWeight += (grade * weight);
                    totalWeight += weight;
                }
            });

            const resultEl = document.getElementById('trimester-result');
            
            if (totalWeight === 0) {
                resultEl.textContent = "0.0";
                resultEl.className = "text-2xl font-bold text-gray-400";
            } else {
                const average = totalGradeByWeight / totalWeight;
                resultEl.textContent = average.toFixed(1);
                
                // Colorize o resultado com base na média 6.0
                if (average >= 6.0) {
                    resultEl.className = "text-2xl font-bold text-emerald-400";
                } else {
                    resultEl.className = "text-2xl font-bold text-red-400";
                }
            }
        }
    </script>
</body>
</html>
