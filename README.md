[poçoartesiano.html](https://github.com/user-attachments/files/23151004/pocoartesiano.html)
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Poço Artesiano - Simulador com Patos</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(to bottom, #87CEEB, #1E90FF);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
            color: #333;
        }
        
        .container {
            max-width: 1000px;
            width: 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        
        header {
            text-align: center;
            margin-bottom: 20px;
            width: 100%;
            background-color: rgba(255, 255, 255, 0.8);
            padding: 15px;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }
        
        h1 {
            color: #1E5799;
            margin-bottom: 10px;
            font-size: 2.5rem;
        }
        
        .stats {
            display: flex;
            justify-content: space-around;
            width: 100%;
            margin-bottom: 20px;
            background-color: rgba(255, 255, 255, 0.8);
            padding: 15px;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }
        
        .stat-box {
            text-align: center;
        }
        
        .stat-value {
            font-size: 1.8rem;
            font-weight: bold;
            color: #1E5799;
        }
        
        .game-area {
            display: flex;
            width: 100%;
            gap: 20px;
        }
        
        .well-container {
            flex: 2;
            background-color: rgba(255, 255, 255, 0.8);
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        
        .well {
            width: 300px;
            height: 500px;
            background-color: #8B4513;
            border: 10px solid #654321;
            border-radius: 10px;
            position: relative;
            overflow: hidden;
            margin-bottom: 20px;
        }
        
        .brick {
            width: 100%;
            height: 20px;
            background-color: #A0522D;
            border-bottom: 2px solid #8B4513;
            position: absolute;
            left: 0;
        }
        
        .water {
            position: absolute;
            bottom: 0;
            width: 100%;
            background: linear-gradient(to top, #1E90FF, #00BFFF);
            transition: height 0.5s;
        }
        
        .duck {
            position: absolute;
            width: 40px;
            height: 40px;
            background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><path d="M50,20 C60,10 80,15 80,30 C90,40 85,60 70,65 C75,75 65,85 50,80 C35,85 25,75 30,65 C15,60 10,40 20,30 C20,15 40,10 50,20 Z" fill="yellow"/><circle cx="35" cy="35" r="5" fill="black"/><path d="M60,45 C70,50 75,60 65,65" stroke="orange" stroke-width="5" fill="none"/></svg>');
            background-size: contain;
            background-repeat: no-repeat;
            z-index: 10;
            transition: all 2s ease-in-out;
        }
        
        .click-area {
            width: 200px;
            height: 80px;
            background-color: #1E5799;
            color: white;
            border: none;
            border-radius: 10px;
            font-size: 1.5rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.2s;
            box-shadow: 0 4px 0 #0E3766;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        
        .click-area:active {
            transform: translateY(4px);
            box-shadow: 0 0 0 #0E3766;
        }
        
        .upgrades {
            flex: 1;
            background-color: rgba(255, 255, 255, 0.8);
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            display: flex;
            flex-direction: column;
            max-height: 600px;
            overflow-y: auto;
        }
        
        .upgrade {
            background-color: #f0f8ff;
            border: 2px solid #1E5799;
            border-radius: 8px;
            padding: 15px;
            margin-bottom: 15px;
            cursor: pointer;
            transition: all 0.2s;
        }
        
        .upgrade:hover {
            background-color: #e1f0ff;
            transform: translateY(-2px);
        }
        
        .upgrade:active {
            transform: translateY(1px);
        }
        
        .upgrade-title {
            font-weight: bold;
            color: #1E5799;
            font-size: 1.2rem;
        }
        
        .upgrade-description {
            font-size: 0.9rem;
            margin: 5px 0;
        }
        
        .upgrade-cost {
            font-weight: bold;
            color: #2E8B57;
        }
        
        .upgrade-owned {
            color: #666;
            font-size: 0.8rem;
            margin-top: 5px;
        }
        
        .disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }
        
        .disabled:hover {
            transform: none;
            background-color: #f0f8ff;
        }
        
        .achievements {
            width: 100%;
            background-color: rgba(255, 255, 255, 0.8);
            border-radius: 10px;
            padding: 20px;
            margin-top: 20px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }
        
        .achievement {
            display: flex;
            align-items: center;
            margin-bottom: 10px;
            padding: 10px;
            background-color: #f9f9f9;
            border-radius: 5px;
        }
        
        .achievement-icon {
            width: 40px;
            height: 40px;
            background-color: #FFD700;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            margin-right: 10px;
            font-weight: bold;
        }
        
        .achievement-locked {
            background-color: #ccc;
        }
        
        @media (max-width: 768px) {
            .game-area {
                flex-direction: column;
            }
            
            .well {
                width: 250px;
                height: 400px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>Poço Artesiano Simulator</h1>
            <p>Clique para construir tijolos e encher seu poço de água. Compre upgrades para aumentar sua produção e atraia patos!</p>
        </header>
        
        <div class="stats">
            <div class="stat-box">
                <div class="stat-label">Tijolos</div>
                <div class="stat-value" id="bricks">0</div>
            </div>
            <div class="stat-box">
                <div class="stat-label">Água por Clique</div>
                <div class="stat-value" id="water-per-click">1</div>
            </div>
            <div class="stat-box">
                <div class="stat-label">Água por Segundo</div>
                <div class="stat-value" id="water-per-second">0</div>
            </div>
            <div class="stat-box">
                <div class="stat-label">Água Total</div>
                <div class="stat-value" id="total-water">0</div>
            </div>
            <div class="stat-box">
                <div class="stat-label">Patos</div>
                <div class="stat-value" id="ducks">0</div>
            </div>
        </div>
        
        <div class="game-area">
            <div class="well-container">
                <div class="well" id="well">
                    <div class="water" id="water" style="height: 0%;"></div>
                    <!-- Patos serão adicionados aqui via JavaScript -->
                </div>
                <button class="click-area" id="click-button">
                    CAVAR POÇO
                </button>
            </div>
            
            <div class="upgrades" id="upgrades">
                <!-- Upgrades serão adicionados via JavaScript -->
            </div>
        </div>
        
        <div class="achievements" id="achievements">
            <h2>Conquistas</h2>
            <!-- Conquistas serão adicionadas via JavaScript -->
        </div>
    </div>

    <script>
        // Dados do jogo
        const gameState = {
            bricks: 0,
            totalWater: 0,
            waterPerClick: 1,
            waterPerSecond: 0,
            waterLevel: 0,
            ducks: 0,
            maxDucks: 0,
            upgrades: [
                {
                    id: 1,
                    name: "Pá Melhorada",
                    description: "Aumenta a água por clique em 1",
                    baseCost: 10,
                    cost: 10,
                    owned: 0,
                    effect: () => { gameState.waterPerClick += 1; }
                },
                {
                    id: 2,
                    name: "Balde Resistente",
                    description: "Aumenta a água por clique em 2",
                    baseCost: 50,
                    cost: 50,
                    owned: 0,
                    effect: () => { gameState.waterPerClick += 2; }
                },
                {
                    id: 3,
                    name: "Bomba Manual",
                    description: "Gera 1 água por segundo",
                    baseCost: 100,
                    cost: 100,
                    owned: 0,
                    effect: () => { gameState.waterPerSecond += 1; }
                },
                {
                    id: 4,
                    name: "Motor Elétrico",
                    description: "Gera 5 água por segundo",
                    baseCost: 500,
                    cost: 500,
                    owned: 0,
                    effect: () => { gameState.waterPerSecond += 5; }
                },
                {
                    id: 5,
                    name: "Poço Profundo",
                    description: "Dobra a água por clique",
                    baseCost: 1000,
                    cost: 1000,
                    owned: 0,
                    effect: () => { gameState.waterPerClick *= 2; }
                },
                {
                    id: 6,
                    name: "Sistema de Filtragem",
                    description: "Triplica a água por segundo",
                    baseCost: 5000,
                    cost: 5000,
                    owned: 0,
                    effect: () => { gameState.waterPerSecond *= 3; }
                },
                {
                    id: 7,
                    name: "Atrair Patos",
                    description: "Atrai 1 pato para seu poço (gera água extra)",
                    baseCost: 200,
                    cost: 200,
                    owned: 0,
                    effect: () => { 
                        gameState.maxDucks += 1;
                        addDuck();
                    }
                },
                {
                    id: 8,
                    name: "Fazenda de Patos",
                    description: "Atrai 5 patos de uma vez",
                    baseCost: 1500,
                    cost: 1500,
                    owned: 0,
                    effect: () => { 
                        gameState.maxDucks += 5;
                        for (let i = 0; i < 5; i++) {
                            setTimeout(() => addDuck(), i * 500);
                        }
                    }
                },
                {
                    id: 9,
                    name: "Alimentador Automático",
                    description: "Cada pato gera 1 água por segundo",
                    baseCost: 3000,
                    cost: 3000,
                    owned: 0,
                    effect: () => { 
                        // Efeito aplicado no gameLoop
                    }
                },
                {
                    id: 10,
                    name: "Poço Sagrado",
                    description: "Dobra todos os ganhos de água",
                    baseCost: 10000,
                    cost: 10000,
                    owned: 0,
                    effect: () => { 
                        gameState.waterPerClick *= 2;
                        gameState.waterPerSecond *= 2;
                    }
                },
                {
                    id: 11,
                    name: "Torneira Mágica",
                    description: "Gera 10% da água total por segundo",
                    baseCost: 20000,
                    cost: 20000,
                    owned: 0,
                    effect: () => { 
                        // Efeito aplicado no gameLoop
                    }
                },
                {
                    id: 12,
                    name: "Fonte Inesgotável",
                    description: "Gera 1000 água por segundo",
                    baseCost: 50000,
                    cost: 50000,
                    owned: 0,
                    effect: () => { 
                        gameState.waterPerSecond += 1000;
                    }
                }
            ],
            achievements: [
                {
                    id: 1,
                    name: "Primeiros Passos",
                    description: "Construa 10 tijolos",
                    goal: 10,
                    achieved: false
                },
                {
                    id: 2,
                    name: "Poço Raso",
                    description: "Alcance 100 de água",
                    goal: 100,
                    achieved: false
                },
                {
                    id: 3,
                    name: "Cavador Experiente",
                    description: "Construa 100 tijolos",
                    goal: 100,
                    achieved: false
                },
                {
                    id: 4,
                    name: "Amigo dos Patos",
                    description: "Tenha 5 patos no poço",
                    goal: 5,
                    achieved: false
                },
                {
                    id: 5,
                    name: "Poço Artesiano",
                    description: "Alcance 1000 de água",
                    goal: 1000,
                    achieved: false
                },
                {
                    id: 6,
                    name: "Fazendeiro de Patos",
                    description: "Tenha 20 patos no poço",
                    goal: 20,
                    achieved: false
                },
                {
                    id: 7,
                    name: "Mestre dos Poços",
                    description: "Alcance 10000 de água",
                    goal: 10000,
                    achieved: false
                },
                {
                    id: 8,
                    name: "Lenda dos Patos",
                    description: "Tenha 50 patos no poço",
                    goal: 50,
                    achieved: false
                }
            ]
        };

        // Elementos do DOM
        const bricksElement = document.getElementById('bricks');
        const waterPerClickElement = document.getElementById('water-per-click');
        const waterPerSecondElement = document.getElementById('water-per-second');
        const totalWaterElement = document.getElementById('total-water');
        const ducksElement = document.getElementById('ducks');
        const clickButton = document.getElementById('click-button');
        const wellElement = document.getElementById('well');
        const waterElement = document.getElementById('water');
        const upgradesElement = document.getElementById('upgrades');
        const achievementsElement = document.getElementById('achievements');

        // Inicialização do jogo
        function initGame() {
            updateDisplay();
            renderUpgrades();
            renderAchievements();
            
            // Configurar o botão de clique
            clickButton.addEventListener('click', handleClick);
            
            // Configurar o loop do jogo
            setInterval(gameLoop, 1000);
        }

        // Loop principal do jogo
        function gameLoop() {
            // Adicionar água automática
            let waterThisSecond = gameState.waterPerSecond;
            
            // Adicionar água dos patos (se tiver o upgrade)
            const alimentadorUpgrade = gameState.upgrades.find(u => u.id === 9);
            if (alimentadorUpgrade && alimentadorUpgrade.owned > 0) {
                waterThisSecond += gameState.ducks;
            }
            
            // Adicionar água da torneira mágica (se tiver o upgrade)
            const torneiraUpgrade = gameState.upgrades.find(u => u.id === 11);
            if (torneiraUpgrade && torneiraUpgrade.owned > 0) {
                waterThisSecond += Math.floor(gameState.totalWater * 0.1);
            }
            
            gameState.totalWater += waterThisSecond;
            
            // Atualizar nível da água
            updateWaterLevel();
            
            // Verificar conquistas
            checkAchievements();
            
            // Atualizar display
            updateDisplay();
        }

        // Manipulador de clique
        function handleClick() {
            // Adicionar tijolo
            gameState.bricks += 1;
            
            // Adicionar água
            gameState.totalWater += gameState.waterPerClick;
            
            // Atualizar nível da água
            updateWaterLevel();
            
            // Adicionar tijolo visual
            addBrick();
            
            // Chance de atrair pato naturalmente
            if (gameState.maxDucks > gameState.ducks && Math.random() < 0.01) {
                addDuck();
            }
            
            // Verificar conquistas
            checkAchievements();
            
            // Atualizar display
            updateDisplay();
        }

        // Adicionar tijolo visual
        function addBrick() {
            const brick = document.createElement('div');
            brick.className = 'brick';
            
            // Calcular a posição do tijolo (do fundo para cima)
            const brickHeight = 20;
            const brickCount = gameState.bricks;
            const bottomPosition = brickCount * brickHeight;
            
            brick.style.bottom = `${bottomPosition}px`;
            
            wellElement.appendChild(brick);
        }

        // Adicionar pato
        function addDuck() {
            if (gameState.ducks >= gameState.maxDucks) return;
            
            gameState.ducks += 1;
            
            const duck = document.createElement('div');
            duck.className = 'duck';
            duck.id = `duck-${gameState.ducks}`;
            
            // Posicionar o pato aleatoriamente na água
            const waterHeight = parseFloat(waterElement.style.height) || 0;
            const maxTop = Math.min(waterHeight - 10, 90); // Garantir que o pato fique na água
            const topPosition = Math.max(10, Math.random() * maxTop);
            const leftPosition = 10 + Math.random() * 80;
            
            duck.style.top = `${topPosition}%`;
            duck.style.left = `${leftPosition}%`;
            
            wellElement.appendChild(duck);
            
            // Animar o pato
            animateDuck(duck);
            
            updateDisplay();
        }

        // Animar pato
        function animateDuck(duck) {
            setInterval(() => {
                if (Math.random() < 0.3) {
                    const currentLeft = parseFloat(duck.style.left);
                    const newLeft = Math.max(5, Math.min(95, currentLeft + (Math.random() * 20 - 10)));
                    duck.style.left = `${newLeft}%`;
                    
                    // Virar o pato na direção do movimento
                    if (newLeft > currentLeft) {
                        duck.style.transform = 'scaleX(1)';
                    } else {
                        duck.style.transform = 'scaleX(-1)';
                    }
                }
            }, 2000);
        }

        // Atualizar nível da água
        function updateWaterLevel() {
            // Calcular a porcentagem de água baseada no total (máximo 100%)
            const maxWater = 100000; // Água máxima para encher o poço
            const waterPercentage = Math.min((gameState.totalWater / maxWater) * 100, 100);
            
            waterElement.style.height = `${waterPercentage}%`;
        }

        // Atualizar display
        function updateDisplay() {
            bricksElement.textContent = gameState.bricks.toLocaleString();
            waterPerClickElement.textContent = gameState.waterPerClick.toLocaleString();
            waterPerSecondElement.textContent = gameState.waterPerSecond.toLocaleString();
            totalWaterElement.textContent = Math.floor(gameState.totalWater).toLocaleString();
            ducksElement.textContent = `${gameState.ducks}/${gameState.maxDucks}`;
            
            // Atualizar upgrades
            renderUpgrades();
        }

        // Renderizar upgrades
        function renderUpgrades() {
            upgradesElement.innerHTML = '<h2>Melhorias</h2>';
            
            gameState.upgrades.forEach(upgrade => {
                const upgradeElement = document.createElement('div');
                upgradeElement.className = `upgrade ${gameState.totalWater < upgrade.cost ? 'disabled' : ''}`;
                
                upgradeElement.innerHTML = `
                    <div class="upgrade-title">${upgrade.name}</div>
                    <div class="upgrade-description">${upgrade.description}</div>
                    <div class="upgrade-cost">Custo: ${upgrade.cost.toLocaleString()} água</div>
                    <div class="upgrade-owned">Comprado: ${upgrade.owned}</div>
                `;
                
                if (gameState.totalWater >= upgrade.cost) {
                    upgradeElement.addEventListener('click', () => purchaseUpgrade(upgrade));
                }
                
                upgradesElement.appendChild(upgradeElement);
            });
        }

        // Comprar upgrade
        function purchaseUpgrade(upgrade) {
            if (gameState.totalWater >= upgrade.cost) {
                gameState.totalWater -= upgrade.cost;
                upgrade.owned += 1;
                upgrade.cost = Math.floor(upgrade.baseCost * Math.pow(1.5, upgrade.owned));
                
                // Aplicar efeito do upgrade
                upgrade.effect();
                
                updateDisplay();
            }
        }

        // Renderizar conquistas
        function renderAchievements() {
            achievementsElement.innerHTML = '<h2>Conquistas</h2>';
            
            gameState.achievements.forEach(achievement => {
                const achievementElement = document.createElement('div');
                achievementElement.className = 'achievement';
                
                let goalType;
                if (achievement.name.includes('tijolos')) {
                    goalType = gameState.bricks;
                } else if (achievement.name.includes('patos') || achievement.name.includes('Pato')) {
                    goalType = gameState.ducks;
                } else {
                    goalType = gameState.totalWater;
                }
                
                achievementElement.innerHTML = `
                    <div class="achievement-icon ${achievement.achieved ? '' : 'achievement-locked'}">
                        ${achievement.achieved ? '✓' : '?'}
                    </div>
                    <div>
                        <div><strong>${achievement.name}</strong></div>
                        <div>${achievement.description}</div>
                        <div>Progresso: ${Math.min(goalType, achievement.goal).toLocaleString()}/${achievement.goal.toLocaleString()}</div>
                    </div>
                `;
                
                achievementsElement.appendChild(achievementElement);
            });
        }

        // Verificar conquistas
        function checkAchievements() {
            gameState.achievements.forEach(achievement => {
                if (!achievement.achieved) {
                    let goalType;
                    if (achievement.name.includes('tijolos')) {
                        goalType = gameState.bricks;
                    } else if (achievement.name.includes('patos') || achievement.name.includes('Pato')) {
                        goalType = gameState.ducks;
                    } else {
                        goalType = gameState.totalWater;
                    }
                    
                    if (goalType >= achievement.goal) {
                        achievement.achieved = true;
                        renderAchievements();
                        
                        // Recompensa por conquista
                        if (achievement.id === 4) { // Amigo dos Patos
                            gameState.maxDucks += 2;
                            addDuck();
                            addDuck();
                        } else if (achievement.id === 6) { // Fazendeiro de Patos
                            gameState.maxDucks += 5;
                            for (let i = 0; i < 5; i++) {
                                setTimeout(() => addDuck(), i * 500);
                            }
                        } else if (achievement.id === 8) { // Lenda dos Patos
                            gameState.waterPerSecond += 100;
                        }
                    }
                }
            });
        }

        // Iniciar o jogo quando a página carregar
        window.addEventListener('load', initGame);
    </script>
</body>
</html>
