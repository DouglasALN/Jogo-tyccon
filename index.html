<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Timber Tycoon: Aventura da Madeira</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <!-- Firebase SDKs -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, getDoc, setDoc, collection } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Variáveis globais para Firebase
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : {};
        const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;

        let app;
        let db;
        let auth;
        let userId = null;
        let isAuthReady = false;

        // Inicializa Firebase e autentica o usuário
        async function initFirebase() {
            try {
                app = initializeApp(firebaseConfig);
                db = getFirestore(app);
                auth = getAuth(app);

                onAuthStateChanged(auth, async (user) => {
                    if (user) {
                        userId = user.uid;
                        isAuthReady = true;
                        console.log("Firebase Auth Ready. User ID:", userId);
                    } else {
                        // Se não houver token inicial, faz login anônimo
                        if (initialAuthToken) {
                            await signInWithCustomToken(auth, initialAuthToken);
                        } else {
                            await signInAnonymously(auth);
                        }
                        // O onAuthStateChanged será chamado novamente com o usuário logado
                    }
                });
            } catch (error) {
                console.error("Erro ao inicializar Firebase:", error);
                showMessage("Erro ao iniciar o serviço de salvamento. Tente recarregar.", 5000);
            }
        }

        // Chamada para inicializar o Firebase no início
        initFirebase();

        // Restante do seu código JavaScript do jogo (dentro de uma IIFE ou modularizado)
        // Para fins de demonstração, o código do jogo continua aqui, mas em um projeto maior,
        // você o importaria como um módulo.
        // As variáveis globais do jogo precisam ser acessíveis aqui.

        // --- Configuração da Cena Three.js ---
        let scene, camera, renderer;
        let player;
        const trees = [];
        const treeCutThreshold = 10; // Quantos cliques para cortar uma árvore
        const playerSpeed = 0.1;
        const maxWood = 10; // Capacidade máxima de madeira
        let currentWood = 0;
        let coins = 0;

        // Elementos da UI
        const coinCountElement = document.getElementById('coin-count');
        const woodCountElement = document.getElementById('wood-count');
        const instructionsElement = document.getElementById('instructions');
        const startButton = document.getElementById('start-button');
        const messageBox = document.getElementById('message-box');
        const saveButton = document.getElementById('save-game-button'); // Novo
        const loadButton = document.getElementById('load-game-button'); // Novo
        const newGameButton = document.getElementById('new-game-button'); // Novo

        // Controles de Toque
        const touchUpButton = document.getElementById('touch-up');
        const touchDownButton = document.getElementById('touch-down');
        const touchLeftButton = document.getElementById('touch-left');
        const touchRightButton = document.getElementById('touch-right');
        const touchActionButton = document.getElementById('touch-action');
        const marketTipButton = document.getElementById('market-tip-button'); // Botão da Dica

        let gameStarted = false;
        let cuttingTree = null; // A árvore que está sendo cortada
        let cutProgress = 0; // Progresso de corte da árvore

        // Teclas e toques pressionados
        const keys = {
            ArrowLeft: false, ArrowRight: false, ArrowUp: false, ArrowDown: false,
            KeyA: false, KeyD: false, KeyW: false, KeyS: false,
            Space: false,
            TouchLeft: false, TouchRight: false, TouchUp: false, TouchDown: false,
            TouchAction: false
        };

        // Variável para controlar o cooldown da dica do mercado
        let marketTipCooldown = false;

        // Função para exibir mensagens temporárias na tela
        function showMessage(text, duration = 2000) {
            messageBox.textContent = text;
            messageBox.classList.add('show');
            setTimeout(() => {
                messageBox.classList.remove('show');
            }, duration);
        }

        // Inicializa a cena, câmera e renderizador
        function init() {
            // Cena
            scene = new THREE.Scene();
            scene.background = new THREE.Color(0x87CEEB); // Céu azul claro

            // Câmera (Ortográfica para sensação 2D em um mundo 3D)
            const aspectRatio = window.innerWidth / window.innerHeight;
            const frustumSize = 20; // Tamanho do frustum para a câmera ortográfica
            camera = new THREE.OrthographicCamera(
                frustumSize * aspectRatio / -2,
                frustumSize * aspectRatio / 2,
                frustumSize / 2,
                frustumSize / -2,
                0.1,
                1000
            );
            camera.position.set(0, 10, 15); // Posição um pouco acima e à frente
            camera.lookAt(0, 0, 0); // Aponta para o centro da cena
            camera.zoom = 1; // Ajusta o zoom da câmera para um melhor campo de visão
            camera.updateProjectionMatrix();

            // Renderizador
            const gameContainer = document.getElementById('game-container');
            renderer = new THREE.WebGLRenderer({ antialias: true });
            renderer.setSize(gameContainer.clientWidth, gameContainer.clientHeight);
            renderer.setPixelRatio(window.devicePixelRatio);
            gameContainer.appendChild(renderer.domElement);

            // Luzes
            const ambientLight = new THREE.AmbientLight(0x404040); // Luz ambiente suave
            scene.add(ambientLight);
            const directionalLight = new THREE.DirectionalLight(0xffffff, 0.8); // Luz direcional
            directionalLight.position.set(5, 10, 7);
            scene.add(directionalLight);

            // Chão (plano verde)
            const groundGeometry = new THREE.PlaneGeometry(50, 50);
            const groundMaterial = new THREE.MeshLambertMaterial({ color: 0x6B8E23 }); // Verde oliva
            const ground = new THREE.Mesh(groundGeometry, groundMaterial);
            ground.rotation.x = -Math.PI / 2; // Rotaciona para ficar plano
            scene.add(ground);

            // Jogador (representado por um cubo simples por enquanto)
            const playerGeometry = new THREE.BoxGeometry(1, 1.5, 1);
            const playerMaterial = new THREE.MeshLambertMaterial({ color: 0x007bff }); // Azul
            player = new THREE.Mesh(playerGeometry, playerMaterial);
            player.position.y = 0.75; // Acima do chão
            scene.add(player);

            // Gerar árvores iniciais
            generateTrees(20);

            // Ponto de venda (um cubo simples)
            const shopGeometry = new THREE.BoxGeometry(2, 3, 2);
            const shopMaterial = new THREE.MeshLambertMaterial({ color: 0xFFD700 }); // Amarelo dourado
            const shop = new THREE.Mesh(shopGeometry, shopMaterial);
            shop.position.set(15, 1.5, 0); // Posição do posto de venda
            scene.add(shop);
            shop.name = 'shop'; // Identifica o objeto como loja

            // Event listeners para redimensionamento da janela
            window.addEventListener('resize', onWindowResize, false);
            // Event listeners para controle do teclado
            window.addEventListener('keydown', onKeyDown, false);
            window.addEventListener('keyup', onKeyUp, false);

            // Event listeners para controle de toque (com verificação de existência)
            if (touchUpButton) {
                touchUpButton.addEventListener('touchstart', () => keys.TouchUp = true);
                touchUpButton.addEventListener('touchend', () => keys.TouchUp = false);
            }
            if (touchDownButton) {
                touchDownButton.addEventListener('touchstart', () => keys.TouchDown = true);
                touchDownButton.addEventListener('touchend', () => keys.TouchDown = false);
            }
            if (touchLeftButton) {
                touchLeftButton.addEventListener('touchstart', () => keys.TouchLeft = true);
                touchLeftButton.addEventListener('touchend', () => keys.TouchLeft = false);
            }
            if (touchRightButton) {
                touchRightButton.addEventListener('touchstart', () => keys.TouchRight = true);
                touchRightButton.addEventListener('touchend', () => keys.TouchRight = false);
            }
            if (touchActionButton) {
                touchActionButton.addEventListener('touchstart', onTouchActionStart);
                touchActionButton.addEventListener('touchend', onTouchActionEnd);
            }
            
            // Event listener para o novo botão de dica do mercado (com verificação de existência)
            if (marketTipButton) {
                marketTipButton.addEventListener('click', getMarketTip);
            }

            startButton.addEventListener('click', startGame);
            saveButton.addEventListener('click', saveGame); // Novo
            loadButton.addEventListener('click', loadGame); // Novo
            newGameButton.addEventListener('click', newGame); // Novo
        }

        // Função para redimensionar a cena e câmera
        function onWindowResize() {
            const gameContainer = document.getElementById('game-container');
            const aspectRatio = gameContainer.clientWidth / gameContainer.clientHeight;
            const frustumSize = 20;

            camera.left = frustumSize * aspectRatio / -2;
            camera.right = frustumSize * aspectRatio / 2;
            camera.top = frustumSize / 2;
            camera.bottom = frustumSize / -2;
            camera.updateProjectionMatrix();

            renderer.setSize(gameContainer.clientWidth, gameContainer.clientHeight);
        }

        // Função para gerar árvores
        function generateTrees(count) {
            // Remove todas as árvores existentes antes de gerar novas
            trees.forEach(tree => scene.remove(tree));
            trees.length = 0; // Limpa o array de árvores

            for (let i = 0; i < count; i++) {
                const treeGroup = new THREE.Group(); // Grupo para tronco e copa

                // Tronco
                const trunkGeometry = new THREE.CylinderGeometry(0.2, 0.3, 2, 8);
                const trunkMaterial = new THREE.MeshLambertMaterial({ color: 0x8B4513 }); // Marrom
                const trunk = new THREE.Mesh(trunkGeometry, trunkMaterial);
                trunk.position.y = 1; // Metade da altura do tronco

                // Copa (um cone simples)
                const leavesGeometry = new THREE.ConeGeometry(1, 2, 8);
                const leavesMaterial = new THREE.MeshLambertMaterial({ color: 0x228B22 }); // Verde floresta
                const leaves = new THREE.Mesh(leavesGeometry, leavesMaterial);
                leaves.position.y = 2.5; // Acima do tronco

                treeGroup.add(trunk);
                treeGroup.add(leaves);

                // Posição aleatória na área do chão, evitando o centro e a loja
                let x, z;
                do {
                    x = (Math.random() - 0.5) * 40; // Entre -20 e 20
                    z = (Math.random() - 0.5) * 40; // Entre -20 e 20
                } while (
                    (x > -5 && x < 5 && z > -5 && z < 5) || // Evita o centro
                    (x > 10 && x < 20 && z > -5 && z < 5)   // Evita a área da loja
                );

                treeGroup.position.set(x, 0, z);
                treeGroup.userData.isTree = true; // Marca como árvore
                treeGroup.userData.cutCount = 0; // Contador de cortes
                trees.push(treeGroup);
                scene.add(treeGroup);
            }
        }

        // Lógica de controle do teclado
        function onKeyDown(event) {
            if (gameStarted) {
                keys[event.code] = true;
                if (event.code === 'Space' && (cuttingTree || isNearShop())) {
                    performAction();
                }
            }
        }

        function onKeyUp(event) {
            if (gameStarted) {
                keys[event.code] = false;
            }
        }

        // Lógica de controle de toque para o botão de ação
        function onTouchActionStart() {
            if (gameStarted) {
                keys.TouchAction = true;
                performAction();
            }
        }

        function onTouchActionEnd() {
            if (gameStarted) {
                keys.TouchAction = false;
            }
        }

        // Função para verificar se o jogador está perto da loja
        function isNearShop() {
            const shop = scene.getObjectByName('shop');
            if (shop) {
                const distanceToShop = player.position.distanceTo(shop.position);
                return distanceToShop < 3;
            }
            return false;
        }

        // Função unificada para realizar a ação (cortar/vender)
        function performAction() {
            // Tenta cortar a árvore primeiro
            if (cuttingTree) {
                cutProgress++;
                showMessage(`Cortando... ${cutProgress}/${treeCutThreshold}`);
                if (cutProgress >= treeCutThreshold) {
                    cutTree(cuttingTree);
                    cuttingTree = null;
                    cutProgress = 0;
                }
                return; // Já realizou uma ação, não tenta vender
            }

            // Se não estiver cortando, tenta vender
            if (isNearShop() && currentWood > 0) {
                sellWood();
            }
        }

        // Função para cortar uma árvore
        function cutTree(tree) {
            if (currentWood < maxWood) {
                scene.remove(tree); // Remove a árvore da cena
                trees.splice(trees.indexOf(tree), 1); // Remove do array
                currentWood++;
                updateUI();
                showMessage('Árvore cortada! +1 Madeira', 1500);

                // Gera uma nova árvore em um local aleatório após um tempo
                setTimeout(() => {
                    generateTrees(1);
                }, 5000); // Regenera a árvore após 5 segundos
            } else {
                showMessage('Inventário cheio! Venda sua madeira.', 2000);
            }
        }

        // Função para vender madeira
        function sellWood() {
            if (currentWood > 0) {
                coins += currentWood * 5; // 5 moedas por madeira
                currentWood = 0;
                updateUI();
                showMessage('Madeira vendida! Moedas ganhas!', 2000);
            } else {
                showMessage('Nenhuma madeira para vender!', 2000);
            }
        }

        // Função para obter uma dica do mercado usando a API Gemini
        async function getMarketTip() {
            if (marketTipCooldown) {
                showMessage('Espere um pouco para a próxima dica!', 2000);
                return;
            }

            showMessage('Obtendo dica do mercado...', 3000);
            marketTipCooldown = true;
            if (marketTipButton) {
                marketTipButton.disabled = true; // Desabilita o botão durante o cooldown
            }

            try {
                let chatHistory = [];
                const prompt = "Generate a short, whimsical, or helpful market tip for a lumberjack selling wood in an arcade game. Keep it under 50 words. Example: 'The price of pine is soaring! Time to clear-cut those evergreens!' or 'Rumor has it, rare woods fetch a fortune in the northern lands.'";
                chatHistory.push({ role: "user", parts: [{ text: prompt }] });
                const payload = { contents: chatHistory };
                const apiKey = ""; // A chave da API será fornecida pelo ambiente Canvas
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;

                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });

                const result = await response.json();

                if (result.candidates && result.candidates.length > 0 &&
                    result.candidates[0].content && result.candidates[0].content.parts &&
                    result.candidates[0].content.parts.length > 0) {
                    const text = result.candidates[0].content.parts[0].text;
                    showMessage(`Dica do Mercado: ${text}`, 5000); // Exibe a dica por mais tempo
                } else {
                    showMessage('Não foi possível obter a dica do mercado. Tente novamente.', 3000);
                }
            } catch (error) {
                console.error('Erro ao chamar a API Gemini:', error);
                showMessage('Erro ao conectar com o mercado. Tente novamente.', 3000);
            } finally {
                // Reabilita o botão após um tempo (cooldown)
                setTimeout(() => {
                    marketTipCooldown = false;
                    if (marketTipButton) {
                        marketTipButton.disabled = false;
                    }
                }, 10000); // Cooldown de 10 segundos
            }
        }

        // --- Funções de Salvar/Carregar Jogo (Firebase Firestore) ---
        async function saveGame() {
            if (!isAuthReady || !userId) {
                showMessage("Serviço de salvamento não pronto. Tente novamente em breve.", 3000);
                return;
            }

            showMessage("Salvando jogo...", 2000);
            try {
                const gameData = {
                    coins: coins,
                    currentWood: currentWood,
                    playerPositionX: player.position.x,
                    playerPositionZ: player.position.z,
                    // Não salvamos o estado individual das árvores, elas serão regeneradas
                    // ao carregar para manter a simplicidade.
                };
                const docRef = doc(db, `artifacts/${appId}/users/${userId}/game_saves`, 'current_game');
                await setDoc(docRef, gameData);
                showMessage("Jogo salvo com sucesso!", 2000);
            } catch (error) {
                console.error("Erro ao salvar jogo:", error);
                showMessage("Erro ao salvar jogo. Tente novamente.", 3000);
            }
        }

        async function loadGame() {
            if (!isAuthReady || !userId) {
                showMessage("Serviço de salvamento não pronto. Tente novamente em breve.", 3000);
                return;
            }

            showMessage("Carregando jogo...", 2000);
            try {
                const docRef = doc(db, `artifacts/${appId}/users/${userId}/game_saves`, 'current_game');
                const docSnap = await getDoc(docRef);

                if (docSnap.exists()) {
                    const data = docSnap.data();
                    coins = data.coins;
                    currentWood = data.currentWood;
                    player.position.x = data.playerPositionX;
                    player.position.z = data.playerPositionZ;
                    player.position.y = 0.75; // Garante que o jogador esteja no chão

                    // Regenera todas as árvores ao carregar um jogo
                    generateTrees(20);

                    updateUI();
                    showMessage("Jogo carregado com sucesso!", 2000);
                } else {
                    showMessage("Nenhum jogo salvo encontrado.", 3000);
                }
            } catch (error) {
                console.error("Erro ao carregar jogo:", error);
                showMessage("Erro ao carregar jogo. Tente novamente.", 3000);
            }
        }

        function newGame() {
            showMessage("Iniciando novo jogo...", 2000);
            coins = 0;
            currentWood = 0;
            player.position.set(0, 0.75, 0); // Reseta a posição do jogador
            generateTrees(20); // Gera um novo conjunto de árvores
            updateUI();
            showMessage("Novo jogo iniciado!", 2000);
        }

        // Atualiza a interface do usuário
        function updateUI() {
            coinCountElement.textContent = coins;
            woodCountElement.textContent = currentWood;
        }

        // Loop principal do jogo
        function animate() {
            requestAnimationFrame(animate);

            if (!gameStarted) {
                renderer.render(scene, camera);
                return;
            }

            // Movimento do jogador (teclado e toque)
            let moved = false;
            if (keys.ArrowLeft || keys.KeyA || keys.TouchLeft) {
                player.position.x -= playerSpeed;
                moved = true;
            }
            if (keys.ArrowRight || keys.KeyD || keys.TouchRight) {
                player.position.x += playerSpeed;
                moved = true;
            }
            if (keys.ArrowUp || keys.KeyW || keys.TouchUp) {
                player.position.z -= playerSpeed; // Move para Z negativo (para cima na tela)
                moved = true;
            }
            if (keys.ArrowDown || keys.KeyS || keys.TouchDown) {
                player.position.z += playerSpeed; // Move para Z positivo (para baixo na tela)
                moved = true;
            }

            // Limita o movimento do jogador ao plano do chão
            player.position.x = Math.max(-24, Math.min(24, player.position.x));
            player.position.z = Math.max(-24, Math.min(24, player.position.z));

            // Detecção de proximidade com árvores
            let nearTree = false;
            for (const tree of trees) {
                const distance = player.position.distanceTo(tree.position);
                if (distance < 2) { // Se o jogador estiver perto o suficiente
                    cuttingTree = tree;
                    nearTree = true;
                    showMessage('Pressione ESPAÇO (ou machado) para cortar!', 1000);
                    break;
                }
            }
            if (!nearTree) {
                cuttingTree = null; // Não está perto de nenhuma árvore
                cutProgress = 0; // Reseta o progresso de corte
            }

            // Detecção de proximidade com a loja
            if (isNearShop()) {
                if (marketTipButton) {
                    marketTipButton.style.display = 'block'; // Mostra o botão da dica
                }
                if (currentWood > 0) {
                    showMessage('Pressione ESPAÇO (ou machado) para vender madeira!', 1000);
                }
            } else {
                if (marketTipButton) {
                    marketTipButton.style.display = 'none'; // Esconde o botão da dica
                }
            }

            renderer.render(scene, camera);
        }

        // Inicia o jogo
        function startGame() {
            instructionsElement.style.display = 'none';
            gameStarted = true;
            // Mostra os botões de controle do jogo
            saveButton.style.display = 'block';
            loadButton.style.display = 'block';
            newGameButton.style.display = 'block';
            showMessage('Jogo iniciado! Boa sorte!', 2000);
        }

        // Inicia a aplicação Three.js quando a janela carregar
        window.onload = function() {
            init();
            animate(); // Inicia o loop de animação
            updateUI(); // Inicializa a UI
        };
    </script>
    <style>
        body {
            margin: 0;
            overflow: hidden; /* Evita barras de rolagem */
            font-family: 'Inter', sans-serif; /* Usando a fonte Inter */
            background-color: #222;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            color: #fff;
        }
        #game-container {
            position: relative;
            width: 90vw; /* Largura fluida */
            height: 90vh; /* Altura fluida */
            max-width: 1200px; /* Limite máximo para desktop */
            max-height: 800px;
            background-color: #333;
            border-radius: 15px;
            overflow: hidden;
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.5);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }
        canvas {
            display: block;
            width: 100% !important; /* Garante que o canvas ocupe 100% da largura do contêiner */
            height: 100% !important; /* Garante que o canvas ocupe 100% da altura do contêiner */
            border-radius: 15px;
        }
        #ui-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            padding: 20px;
            box-sizing: border-box;
            pointer-events: none; /* Permite interagir com o canvas por baixo */
        }
        #top-bar {
            display: flex;
            justify-content: space-between;
            width: 100%;
        }
        .info-box {
            background-color: rgba(0, 0, 0, 0.6);
            padding: 10px 15px;
            border-radius: 10px;
            font-size: 1.2em;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        .info-box span {
            font-weight: bold;
            color: #4CAF50; /* Verde para moedas/madeira */
        }
        #instructions {
            background-color: rgba(0, 0, 0, 0.7);
            padding: 20px;
            border-radius: 10px;
            text-align: center;
            max-width: 400px;
            margin: auto;
            pointer-events: all; /* Permite interação */
            animation: fadeIn 1s ease-out;
        }
        #instructions h2 {
            color: #FFD700; /* Dourado */
            margin-top: 0;
        }
        #instructions p {
            margin-bottom: 15px;
            line-height: 1.5;
        }
        #start-button {
            background-color: #4CAF50;
            color: white;
            padding: 12px 25px;
            border: none;
            border-radius: 8px;
            font-size: 1.1em;
            cursor: pointer;
            transition: background-color 0.3s ease, transform 0.2s ease;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
            pointer-events: all; /* Permite interação */
        }
        #start-button:hover {
            background-color: #45a049;
            transform: translateY(-2px);
        }
        #start-button:active {
            transform: translateY(0);
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
        }
        #message-box {
            position: absolute;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            background-color: rgba(0, 0, 0, 0.8);
            color: white;
            padding: 10px 20px;
            border-radius: 10px;
            font-size: 1.1em;
            opacity: 0;
            transition: opacity 0.5s ease-out;
            pointer-events: none;
            z-index: 10;
        }
        #message-box.show {
            opacity: 1;
        }

        /* Controles de Toque */
        #touch-controls {
            position: absolute;
            bottom: 20px;
            width: 100%;
            display: flex;
            justify-content: space-between; /* Empurra D-pad e botão de ação para as extremidades */
            align-items: flex-end; /* Alinha os itens na parte inferior */
            padding: 0 20px;
            box-sizing: border-box;
            pointer-events: none; /* Permite interagir com o canvas por baixo */
            z-index: 5;
        }
        .touch-button {
            background-color: rgba(0, 0, 0, 0.6);
            color: white;
            border: 2px solid rgba(255, 255, 255, 0.3);
            border-radius: 50%;
            width: 60px;
            height: 60px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 1.8em;
            cursor: pointer;
            user-select: none; /* Evita seleção de texto */
            pointer-events: all; /* Permite interação */
            transition: background-color 0.2s ease;
        }
        .touch-button:active {
            background-color: rgba(255, 255, 255, 0.2);
        }
        #action-button {
            background-color: #E67E22; /* Laranja para ação */
            width: 80px; /* Ligeiramente maior para o botão de ação */
            height: 80px;
            border-radius: 50%;
        }
        #action-button:active {
            background-color: #D35400;
        }

        /* Estilos para o D-pad */
        #dpad-wrapper {
            display: grid;
            grid-template-columns: repeat(3, 1fr); /* 3 colunas para a grade do D-pad */
            grid-template-rows: repeat(3, 1fr); /* 3 linhas para a grade do D-pad */
            width: 180px; /* Tamanho do D-pad */
            height: 180px;
            gap: 5px; /* Espaçamento entre os botões do D-pad */
            pointer-events: all;
        }
        .dpad-empty {
            /* Placeholder para células vazias na grade do D-pad */
            width: 60px; /* Combina com o tamanho do botão */
            height: 60px; /* Combina com o tamanho do botão */
        }
        .dpad-button {
            border-radius: 15px; /* Botões quadrados com cantos arredondados para o D-pad */
            width: 60px;
            height: 60px;
            font-size: 1.5em; /* Ajusta o tamanho do ícone */
        }

        /* Novo botão para a dica do mercado */
        #market-tip-button {
            background-color: #8A2BE2; /* Azul violeta */
            color: white;
            padding: 10px 15px;
            border: none;
            border-radius: 8px;
            font-size: 1em;
            cursor: pointer;
            transition: background-color 0.3s ease, transform 0.2s ease;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
            pointer-events: all; /* Permite interação */
            position: absolute; /* Posiciona absolutamente */
            bottom: 80px; /* Acima dos controles de toque */
            left: 50%;
            transform: translateX(-50%);
            display: none; /* Esconde por padrão */
            z-index: 6; /* Acima dos controles de toque */
        }
        #market-tip-button:hover {
            background-color: #6A1BA0;
            transform: translateX(-50%) translateY(-2px);
        }
        #market-tip-button:active {
            transform: translateX(-50%) translateY(0);
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
        }

        /* Botões de Salvar/Carregar/Novo Jogo */
        #game-controls {
            position: absolute;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 10px;
            z-index: 10;
            pointer-events: none; /* Permite interagir com o canvas por baixo */
        }
        #game-controls button {
            background-color: #3498DB; /* Azul */
            color: white;
            padding: 10px 15px;
            border: none;
            border-radius: 8px;
            font-size: 0.9em;
            cursor: pointer;
            transition: background-color 0.3s ease, transform 0.2s ease;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
            pointer-events: all; /* Permite interação */
            display: none; /* Esconde por padrão, mostra após iniciar o jogo */
        }
        #game-controls button:hover {
            background-color: #2980B9;
            transform: translateY(-2px);
        }
        #game-controls button:active {
            transform: translateY(0);
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
        }
        #new-game-button {
            background-color: #E74C3C; /* Vermelho */
        }
        #new-game-button:hover {
            background-color: #C0392B;
        }


        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* Ícones simples usando Font Awesome */
        .icon {
            font-family: "Font Awesome 5 Free";
            font-weight: 900;
        }
        .icon-axe::before { content: "\f6b2"; } /* solid axe */
        .icon-tree::before { content: "\f1bb"; } /* solid tree */
        .icon-coin::before { content: "\f51e"; } /* solid coin */
        .icon-sparkle::before { content: "\f005"; } /* solid star, will use as sparkle */


        /* Media Queries para responsividade */
        @media (max-width: 768px) {
            #game-container {
                width: 98vw;
                height: 98vh;
                border-radius: 10px;
            }
            .info-box {
                font-size: 0.9em;
                padding: 6px 10px;
            }
            #instructions {
                padding: 15px;
                font-size: 0.8em;
                max-width: 90%;
            }
            #start-button {
                padding: 10px 20px;
                font-size: 0.9em;
            }
            /* Ajustes para botões de toque em telas menores */
            .touch-button {
                width: 50px;
                height: 50px;
                font-size: 1.5em;
            }
            #action-button {
                width: 70px;
                height: 70px;
                font-size: 1.5em;
            }
            #touch-controls {
                bottom: 10px;
                padding: 0 10px;
            }
            #market-tip-button {
                bottom: 70px; /* Ajusta a posição para não colidir com os botões de toque */
                padding: 8px 12px;
                font-size: 0.9em;
            }
            #dpad-wrapper {
                width: 150px;
                height: 150px;
                gap: 3px;
            }
            .dpad-button {
                width: 45px;
                height: 45px;
                font-size: 1.2em;
            }
            .dpad-empty {
                width: 45px;
                height: 45px;
            }
            #game-controls {
                top: 10px;
                gap: 5px;
            }
            #game-controls button {
                padding: 8px 12px;
                font-size: 0.8em;
            }
        }

        /* Ocultar controles de toque em telas maiores */
        @media (min-width: 769px) {
            #touch-controls {
                display: none;
            }
        }
    </style>
    <!-- Incluindo Font Awesome para ícones -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.4/css/all.min.css">
</head>
<body>
    <div id="game-container">
        <div id="ui-overlay">
            <div id="top-bar">
                <div class="info-box">
                    <i class="icon icon-coin"></i>
                    Moedas: <span id="coin-count">0</span>
                </div>
                <div class="info-box">
                    <i class="icon icon-axe"></i>
                    Madeira: <span id="wood-count">0</span>
                </div>
            </div>
            <div id="instructions">
                <h2>Bem-vindo ao Timber Tycoon!</h2>
                <p>Mova-se com as setas do teclado (cima/baixo/esquerda/direita) ou W/A/S/D.</p>
                <p>Em dispositivos móveis, use os botões de flecha na parte inferior esquerda da tela.</p>
                <p>Pressione <strong>ESPAÇO</strong> (ou o botão de machado) para cortar árvores quando estiver perto delas.</p>
                <p>Vá até a loja (bloco amarelo) para vender sua madeira e, se quiser, pedir uma ✨ Dica do Mercado ✨!</p>
                <button id="start-button">Começar Jogo</button>
            </div>
            <div id="message-box"></div>
        </div>

        <!-- Controles de Jogo (Salvar/Carregar/Novo) -->
        <div id="game-controls">
            <button id="save-game-button">Salvar Jogo</button>
            <button id="load-game-button">Carregar Jogo</button>
            <button id="new-game-button">Novo Jogo</button>
        </div>

        <!-- Controles de Toque para Celular -->
        <div id="touch-controls">
            <div id="dpad-wrapper">
                <div class="dpad-row">
                    <div class="dpad-empty"></div>
                    <div class="touch-button dpad-button" id="touch-up"><i class="icon fas fa-arrow-up"></i></div>
                    <div class="dpad-empty"></div>
                </div>
                <div class="dpad-row">
                    <div class="touch-button dpad-button" id="touch-left"><i class="icon fas fa-arrow-left"></i></div>
                    <div class="dpad-empty"></div>
                    <div class="touch-button dpad-button" id="touch-right"><i class="icon fas fa-arrow-right"></i></div>
                </div>
                <div class="dpad-row">
                    <div class="dpad-empty"></div>
                    <div class="touch-button dpad-button" id="touch-down"><i class="icon fas fa-arrow-down"></i></div>
                    <div class="dpad-empty"></div>
                </div>
            </div>
            <div class="touch-button" id="action-button"><i class="icon icon-axe"></i></div>
        </div>

        <!-- Botão da Dica do Mercado (Gemini API) -->
        <button id="market-tip-button" style="display: none;">Pedir Dica do Mercado ✨</button>
    </div>
</body>
</html>
