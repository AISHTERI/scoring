<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SYSTÈME DE SCORING - MALE ALPHA</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        /* --- FOND SOMBRE & TEXTE BLANC --- */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: #1e1e1e; /* Fond très sombre */
            color: #f0f0f0; /* Texte clair */
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            background: #252526; /* Fond intérieur sombre */
            border-radius: 20px;
            box-shadow: 0 10px 40px rgba(0,0,0,0.5);
            padding: 40px;
        }

        header {
            text-align: center;
            margin-bottom: 40px;
        }

        /* --- COULEUR DOMINANTE : AURA DORÉE (JAUNE VIF) --- */
        h1 {
            color: #ffd700; /* Jaune Or */
            font-size: 2.5em;
            margin-bottom: 10px;
        }

        .tagline {
            color: #aaa;
            font-size: 1.1em;
        }

        .dashboard {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-bottom: 40px;
        }

        .stat-card {
            /* Fond des cartes plus foncé */
            background: #333; 
            color: #ffd700; /* Texte en Or */
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.3);
        }

        .stat-card h3 {
            font-size: 0.9em;
            opacity: 0.8;
            margin-bottom: 10px;
            color: #f0f0f0; /* Titres de cartes en blanc */
        }

        .stat-value {
            font-size: 2.5em;
            font-weight: bold;
            margin-bottom: 5px;
        }

        .stat-label {
            font-size: 0.85em;
            opacity: 0.8;
            color: #aaa;
        }

        /* Les cartes des métriques restent dans les tons sombres pour le contraste */
        .level-card, .xp-card, .streak-card {
            background: #444; 
        }

        .progress-ring {
            width: 150px;
            height: 150px;
            margin: 20px auto;
        }

        .progress-ring circle {
            transition: stroke-dashoffset 0.5s ease;
        }

        /* Le cercle de progression devient doré */
        #progressCircle {
            stroke: #ffd700 !important;
        }

        #ringPercent {
            fill: #ffd700 !important;
        }

        .scoring-table {
            overflow-x: auto;
            margin-bottom: 40px;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 20px;
        }

        /* --- EN-TÊTE DU TABLEAU EN DORÉ/SOMBE --- */
        th {
            background: #444; 
            color: #ffd700; 
            padding: 15px;
            text-align: left;
            font-weight: 600;
        }

        td {
            padding: 12px 15px;
            border-bottom: 1px solid #333; /* Séparateur plus foncé */
        }

        /* --- CHAMPS DE SAISIE --- */
        input[type="number"] {
            width: 60px;
            padding: 8px;
            border: 2px solid #555;
            border-radius: 5px;
            font-size: 14px;
            text-align: center;
            background: #333; /* Fond sombre */
            color: #f0f0f0; /* Texte blanc */
        }

        input[type="number"]:focus {
            outline: none;
            border-color: #ffd700; /* Bordure dorée au focus */
        }

        .status {
            padding: 5px 12px;
            border-radius: 20px;
            font-size: 0.85em;
            font-weight: 600;
            color: #1e1e1e; /* Texte sombre sur les pastilles de statut */
        }
        
        /* Les couleurs de statut restent vives pour la visibilité */
        .status-excellent { background: #43e97b; } /* Vert */
        .status-bien { background: #4facfe; } /* Bleu */
        .status-moyen { background: #f5af19; } /* Orange */
        .status-faible { background: #f5576c; } /* Rouge */

        .chart-container {
            background: #252526;
            padding: 30px;
            border-radius: 15px;
            margin-bottom: 40px;
        }

        .bar-chart {
            display: flex;
            align-items: flex-end;
            justify-content: space-around;
            height: 300px;
            margin-top: 20px;
        }

        /* --- BARRES DU GRAPHIQUE EN DORÉ --- */
        .bar {
            flex: 1;
            margin: 0 5px;
            background: linear-gradient(to top, #ffd700, #b8860b); /* Dégradé Or */
            border-radius: 5px 5px 0 0;
            position: relative;
            transition: all 0.3s ease;
        }

        .bar:hover {
            opacity: 0.8;
        }

        .bar-label {
            position: absolute;
            bottom: -25px;
            left: 50%;
            transform: translateX(-50%);
            font-size: 0.75em;
            color: #aaa;
        }

        .bar-value {
            position: absolute;
            top: -25px;
            left: 50%;
            transform: translateX(-50%);
            font-weight: 600;
            color: #ffd700;
        }

        /* --- RECOMMANDATIONS SUR FOND DORÉ PÂLE --- */
        .recommendations {
            background: #3a3219; /* Fond sombre pour les reco */
            border-left: 4px solid #ffd700;
            padding: 20px;
            border-radius: 5px;
            margin-bottom: 30px;
            color: #f0f0f0;
        }

        .recommendations h3 {
            color: #ffd700;
            margin-bottom: 10px;
        }

        .recommendations ul {
            list-style-position: inside;
            color: #f0f0f0;
        }

        .recommendations li {
            margin-bottom: 5px;
        }

        .buttons {
            display: flex;
            gap: 15px;
            justify-content: center;
            margin-top: 30px;
        }

        button {
            padding: 15px 30px;
            font-size: 1em;
            font-weight: 600;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        /* --- BOUTONS SUR FOND DORÉ/VERT/BLEU --- */
        .btn-save {
            background: #ffd700;
            color: #1e1e1e; /* Texte sombre sur bouton doré */
        }

        .btn-export {
            background: #4facfe;
            color: white;
        }

        .btn-reset {
            background: #f5576c;
            color: white;
        }

        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0,0,0,0.5);
        }

        .week-summary {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin: 30px 0;
        }

        .week-card {
            background: #333; /* Cartes de résumé sombres */
            padding: 20px;
            border-radius: 10px;
            text-align: center;
        }

        .week-card h4 {
            color: #ffd700; /* Titres en Doré */
            margin-bottom: 10px;
        }

        .week-score {
            font-size: 2em;
            font-weight: bold;
            color: #f0f0f0;
        }

        /* --- GUIDE D'UTILISATION (Bas de page) --- */
        .guide-box {
            margin-top: 40px; 
            padding: 20px; 
            background: #333; 
            border-radius: 10px;
        }

        .guide-box h3 {
            color: #ffd700; 
            margin-bottom: 15px;
        }

        .guide-box ul {
            list-style-position: inside;
            margin-left: 20px; 
            color: #aaa;
        }
        
        .guide-box p {
            color: #aaa;
        }

        @media (max-width: 768px) {
            .container {
                padding: 20px;
            }

            h1 {
                font-size: 1.8em;
            }

            .dashboard {
                grid-template-columns: 1fr;
            }

            table {
                font-size: 0.85em;
            }

            input[type="number"] {
                width: 50px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>👑 SYSTÈME DE SCORING - MALE ALPHA</h1>
            <p class="tagline">Trackez vos progrès, montez de niveau, atteignez vos objectifs !</p>
        </header>

        <div class="dashboard">
            <div class="stat-card">
                <h3>SCORE GLOBAL</h3>
                <div class="stat-value" id="globalScore">0%</div>
                <div class="stat-label">Performance totale</div>
            </div>
            <div class="stat-card level-card">
                <h3>NIVEAU ACTUEL</h3>
                <div class="stat-value" id="currentLevel">NOVICE</div>
                <div class="stat-label" id="levelProgress">0/300 XP</div>
            </div>
            <div class="stat-card xp-card">
                <h3>EXPÉRIENCE</h3>
                <div class="stat-value" id="totalXP">0</div>
                <div class="stat-label">XP Total</div>
            </div>
            <div class="stat-card streak-card">
                <h3>SÉRIE</h3>
                <div class="stat-value" id="streak">0</div>
                <div class="stat-label">Jours consécutifs</div>
            </div>
        </div>

        <svg class="progress-ring" viewBox="0 0 160 160">
            <circle cx="80" cy="80" r="70" fill="none" stroke="#333" stroke-width="12"/>
            <circle id="progressCircle" cx="80" cy="80" r="70" fill="none" stroke="#ffd700" stroke-width="12"
                    stroke-dasharray="439.8" stroke-dashoffset="439.8" transform="rotate(-90 80 80)"
                    stroke-linecap="round"/>
            <text x="80" y="85" text-anchor="middle" font-size="24" font-weight="bold" fill="#ffd700" id="ringPercent">0%</text>
        </svg>

        <div class="recommendations" id="recommendations">
            <h3>💡 Recommandations Personnalisées</h3>
            <ul id="recommendationsList">
                <li>Commence à entrer tes scores quotidiens pour recevoir des conseils personnalisés !</li>
            </ul>
        </div>

        <div class="scoring-table">
            <h2 style="color: #ffd700; margin-bottom: 20px;">📋 Tableau de Scoring - 21 Jours</h2>
            <table id="scoringTable">
                <thead>
                    <tr>
                        <th>Jour</th>
                        <th>Énergie (/10)</th>
                        <th>Discipline (/10)</th>
                        <th>Focus (/10)</th>
                        <th>Total (/30)</th>
                        <th>XP Gagné</th>
                        <th>Statut</th>
                    </tr>
                </thead>
                <tbody id="tableBody">
                </tbody>
            </table>
        </div>

        <div class="week-summary">
            <div class="week-card">
                <h4>📅 Semaine 1</h4>
                <div class="week-score" id="week1Score">0/210</div>
            </div>
            <div class="week-card">
                <h4>📅 Semaine 2</h4>
                <div class="week-score" id="week2Score">0/210</div>
            </div>
            <div class="week-card">
                <h4>📅 Semaine 3</h4>
                <div class="week-score" id="week3Score">0/210</div>
            </div>
        </div>

        <div class="chart-container">
            <h3 style="color: #ffd700; margin-bottom: 10px;">📈 Évolution Quotidienne</h3>
            <div class="bar-chart" id="barChart"></div>
        </div>

        <div class="buttons">
            <button class="btn-save" onclick="saveData()">💾 SAUVEGARDER</button>
            <button class="btn-export" onclick="exportToCSV()">📊 EXPORTER CSV</button>
            <button class="btn-reset" onclick="resetData()">🔄 RÉINITIALISER</button>
        </div>

        <div class="guide-box">
            <h3>📖 Guide d'utilisation</h3>
            <p style="margin-bottom: 10px;"><strong>🎯 Les 3 Métriques (Mindset & Système) :</strong></p>
            <ul style="margin-left: 20px; color: #aaa;">
                <li><strong>Énergie (/10)</strong> : Ton niveau d'énergie physique et mentale dans la journée.</li>
                <li><strong>Discipline (/10)</strong> : As-tu tenu tes engagements (Micro-Habitudes) sans négocier ?</li>
                <li><strong>Focus (/10)</strong> : As-tu réussi à te concentrer sur tes priorités (Deep Work) ?</li>
            </ul>
            <p style="margin-top: 15px; margin-bottom: 10px;"><strong>🏆 Système de Niveaux (Progression) :</strong></p>
            <ul style="margin-left: 20px; color: #aaa;">
                <li><strong>NOVICE</strong> : 0-300 XP</li>
                <li><strong>APPRENTI</strong> : 300-700 XP</li>
                <li><strong>EXPERT</strong> : 700-1200 XP</li>
                <li><strong>MAÎTRE</strong> : 1200+ XP</li>
            </ul>
            <p style="margin-top: 15px; color: #aaa;"><em>Chaque point de score = 2 XP | Score parfait = 30/30 = 60 XP par jour.</em></p>
        </div>
    </div>

    <script>
        let scores = [];
        const DAYS = 21;

        function initTable() {
            const tbody = document.getElementById('tableBody');
            tbody.innerHTML = '';
            
            for (let i = 1; i <= DAYS; i++) {
                const row = tbody.insertRow();
                row.innerHTML = `
                    <td><strong>Jour ${i}</strong></td>
                    <td><input type="number" min="0" max="10" value="0" onchange="updateScores()" id="energie${i}"></td>
                    <td><input type="number" min="0" max="10" value="0" onchange="updateScores()" id="discipline${i}"></td>
                    <td><input type="number" min="0" max="10" value="0" onchange="updateScores()" id="focus${i}"></td>
                    <td id="total${i}">0</td>
                    <td id="xp${i}">0</td>
                    <td id="status${i}"><span class="status status-faible">Faible</span></td>
                `;
            }
            
            loadData();
            updateScores();
        }

        function updateScores() {
            scores = [];
            let totalXP = 0;
            let totalScore = 0;
            let streak = 0;
            let lastScoreWasGood = false;
            let daysCompleted = 0;

            for (let i = 1; i <= DAYS; i++) {
                const energieInput = document.getElementById(`energie${i}`);
                const disciplineInput = document.getElementById(`discipline${i}`);
                const focusInput = document.getElementById(`focus${i}`);
                
                const energie = parseInt(energieInput.value) || 0;
                const discipline = parseInt(disciplineInput.value) || 0;
                const focus = parseInt(focusInput.value) || 0;
                
                // Mettre les valeurs à 0 si elles sont invalides
                if (energie < 0 || energie > 10) energieInput.value = 0;
                if (discipline < 0 || discipline > 10) disciplineInput.value = 0;
                if (focus < 0 || focus > 10) focusInput.value = 0;

                const dayTotal = energie + discipline + focus;
                const dayXP = dayTotal * 2;
                
                scores.push({energie, discipline, focus, total: dayTotal, xp: dayXP});
                
                totalXP += dayXP;
                totalScore += dayTotal;
                
                document.getElementById(`total${i}`).textContent = dayTotal;
                document.getElementById(`xp${i}`).textContent = dayXP;
                
                let statusClass = 'status-faible';
                let statusText = 'Faible';
                
                if (dayTotal >= 27) {
                    statusClass = 'status-excellent';
                    statusText = 'Excellent';
                } else if (dayTotal >= 21) {
                    statusClass = 'status-bien';
                    statusText = 'Bien';
                } else if (dayTotal >= 15) {
                    statusClass = 'status-moyen';
                    statusText = 'Moyen';
                }
                
                document.getElementById(`status${i}`).innerHTML = `<span class="status ${statusClass}">${statusText}</span>`;
                
                // Calcul de la série et des jours complétés (score > 0)
                if (dayTotal > 0) {
                    daysCompleted++;
                }

                if (dayTotal >= 15) {
                    if (lastScoreWasGood || i === 1) {
                        streak++;
                        lastScoreWasGood = true;
                    }
                } else {
                    lastScoreWasGood = false;
                }
            }

            const maxScore = daysCompleted * 30;
            const currentScore = totalScore;
            
            // Calcul du pourcentage sur les jours complétés
            const globalPercent = daysCompleted > 0 ? Math.round((currentScore / maxScore) * 100) : 0;
            
            document.getElementById('globalScore').textContent = globalPercent + '%';
            document.getElementById('totalXP').textContent = totalXP;
            document.getElementById('streak').textContent = streak;
            
            updateLevel(totalXP);
            updateProgressRing(globalPercent);
            updateChart();
            updateWeekScores();
            updateRecommendations(globalPercent);
        }

        function updateLevel(xp) {
            let level = 'NOVICE';
            let progress = '';
            
            if (xp >= 1200) {
                level = 'MAÎTRE';
                progress = `${xp} XP`;
            } else if (xp >= 700) {
                level = 'EXPERT';
                progress = `${xp}/1200 XP`;
            } else if (xp >= 300) {
                level = 'APPRENTI';
                progress = `${xp}/700 XP`;
            } else {
                level = 'NOVICE';
                progress = `${xp}/300 XP`;
            }
            
            document.getElementById('currentLevel').textContent = level;
            document.getElementById('levelProgress').textContent = progress;
        }

        function updateProgressRing(percent) {
            const circle = document.getElementById('progressCircle');
            const circumference = 439.8;
            const offset = circumference - (percent / 100) * circumference;
            
            circle.style.strokeDashoffset = offset;
            document.getElementById('ringPercent').textContent = percent + '%';
        }

        function updateChart() {
            const chartDiv = document.getElementById('barChart');
            chartDiv.innerHTML = '';
            
            scores.forEach((score, index) => {
                if (score.total > 0) {
                    const bar = document.createElement('div');
                    bar.className = 'bar';
                    const heightPercent = (score.total / 30) * 100;
                    bar.style.height = heightPercent + '%';
                    
                    const label = document.createElement('div');
                    label.className = 'bar-label';
                    label.textContent = `J${index + 1}`;
                    
                    const value = document.createElement('div');
                    value.className = 'bar-value';
                    value.textContent = score.total;
                    
                    bar.appendChild(label);
                    bar.appendChild(value);
                    chartDiv.appendChild(bar);
                }
            });
        }

        function updateWeekScores() {
            let week1 = 0, week2 = 0, week3 = 0;
            const maxWeekScore = 7 * 30; // 210

            scores.forEach((score, index) => {
                if (index < 7) week1 += score.total;
                else if (index < 14) week2 += score.total;
                else week3 += score.total;
            });
            
            document.getElementById('week1Score').textContent = `${week1}/${maxWeekScore}`;
            document.getElementById('week2Score').textContent = `${week2}/${maxWeekScore}`;
            document.getElementById('week3Score').textContent = `${week3}/${maxWeekScore}`;
        }

        function updateRecommendations(percent) {
            const list = document.getElementById('recommendationsList');
            list.innerHTML = '';
            
            if (percent < 40) {
                list.innerHTML = `
                    <li>🔴 Reviens au Plan B (Level 3) : Ne garde qu'UNE métrique à travailler (Énergie ou Discipline).</li>
                    <li>🧠 Lis la section **The Mountain Is You** : Identifie la peur qui sabote ta progression.</li>
                    <li>📅 Lis la section **Deep Work** : Mets ton téléphone en mode avion pendant 60 minutes.</li>
                `;
            } else if (percent < 70) {
                list.innerHTML = `
                    <li>🟡 Bonne progression ! Concentre-toi sur la métrique la plus faible.</li>
                    <li>💪 Lis la section **Can't Hurt Me** : Applique la Règle des 40% pour dépasser l'inconfort.</li>
                    <li>🎯 Relis la **Règle du 1%** (Atomic Habits) : Ajoute une micro-victoire non négociable.</li>
                `;
            } else {
                list.innerHTML = `
                    <li>🟢 Performance digne d'un ALPHA ! Tu es en train de transformer ton identité 🏆</li>
                    <li>🚀 Lis la section **The 5AM Club** : Si tu es déjà Expert, challenge-toi sur le rituel matinal complet.</li>
                    <li>📚 Prépare ton prochain cycle : Quels seront tes 3 nouveaux objectifs dans 21 jours ?</li>
                `;
            }
        }

        function saveData() {
            const data = {
                scores: scores.map((score, index) => ({
                    energie: parseInt(document.getElementById(`energie${index + 1}`).value) || 0,
                    discipline: parseInt(document.getElementById(`discipline${index + 1}`).value) || 0,
                    focus: parseInt(document.getElementById(`focus${index + 1}`).value) || 0,
                })),
                timestamp: new Date().toISOString()
            };
            localStorage.setItem('scoringData', JSON.stringify(data));
            alert('✅ Données sauvegardées avec succès !');
        }

        function loadData() {
            const saved = localStorage.getItem('scoringData');
            if (saved) {
                const data = JSON.parse(saved);
                data.scores.forEach((score, index) => {
                    if (index < DAYS) {
                        document.getElementById(`energie${index + 1}`).value = score.energie;
                        document.getElementById(`discipline${index + 1}`).value = score.discipline;
                        document.getElementById(`focus${index + 1}`).value = score.focus;
                    }
                });
            }
        }

        function exportToCSV() {
            let csv = 'Jour,Énergie,Discipline,Focus,Total,XP,Statut\n';
            
            scores.forEach((score, index) => {
                let status = 'Faible';
                if (score.total >= 27) status = 'Excellent';
                else if (score.total >= 21) status = 'Bien';
                else if (score.total >= 15) status = 'Moyen';
                
                csv += `${index + 1},${score.energie},${score.discipline},${score.focus},${score.total},${score.xp},${status}\n`;
            });
            
            const blob = new Blob([csv], { type: 'text/csv' });
            const url = window.URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = 'scoring_objectifs_male_alpha.csv';
            a.click();
        }

        function resetData() {
            if (confirm('⚠️ Êtes-vous sûr de vouloir réinitialiser toutes les données ?')) {
                localStorage.removeItem('scoringData');
                initTable();
                alert('✅ Données réinitialisées !');
            }
        }

        window.onload = initTable;
    </script>
</body>
</html># scoring
