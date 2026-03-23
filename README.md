# ball-connect-puzzle-game
**Ball Connect** ek simple aur addictive puzzle game hai jisme player ka goal same color ke balls ko ek continuous path se connect karna hota hai. Player ko grid board par lines draw karni hoti hain bina paths ko overlap kiye. Jaise-jaise level badhta hai, puzzles zyada challenging ho jaate hain, 

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Flow Connect Pro | Scrollable Map</title>
    <style>
        * {
            user-select: none;
            -webkit-tap-highlight-color: transparent;
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            width: 100vw;
            height: 100vh;
            overflow: hidden;
            position: fixed;
            font-family: 'Segoe UI', 'Poppins', system-ui, sans-serif;
            background: radial-gradient(circle at 20% 30%, #0a0f2a, #010008);
        }

        /* Animated Cosmic Background */
        .cosmic-bg {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -2;
            background: linear-gradient(135deg, #0a0f2a 0%, #03061a 50%, #000000 100%);
        }

        .stars {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: -1;
        }

        .star {
            position: absolute;
            background: white;
            border-radius: 50%;
            animation: twinkle 3s infinite ease-in-out;
        }

        @keyframes twinkle {
            0%, 100% { opacity: 0.3; transform: scale(1); }
            50% { opacity: 1; transform: scale(1.2); }
        }

        .nebula {
            position: fixed;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: radial-gradient(circle at 30% 40%, rgba(100, 80, 200, 0.2) 0%, rgba(0, 0, 0, 0) 70%),
                        radial-gradient(circle at 70% 60%, rgba(0, 200, 180, 0.15) 0%, rgba(0, 0, 0, 0) 70%);
            pointer-events: none;
            z-index: -1;
            animation: drift 60s infinite linear;
        }

        @keyframes drift {
            0% { transform: rotate(0deg) scale(1); }
            100% { transform: rotate(360deg) scale(1.2); }
        }

        .screen {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            overflow: hidden;
            animation: fade 0.4s ease;
            background: transparent;
        }
        .active {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }

        @keyframes fade {
            from { opacity: 0; transform: scale(0.96); }
            to { opacity: 1; transform: scale(1); }
        }

        /* Start Screen */
        #startScreen {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 1.5rem;
            background: transparent;
        }

        .logo-container {
            text-align: center;
            animation: float 3s ease-in-out infinite;
        }

        @keyframes float {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-12px); }
        }

        .game-title {
            font-size: 3.2rem;
            font-weight: 800;
            background: linear-gradient(135deg, #fff, #4aff9e, #00ffff, #ff66cc);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            letter-spacing: 4px;
        }

        .game-subtitle {
            color: rgba(255,255,255,0.8);
            font-size: 1rem;
            background: rgba(0,0,0,0.4);
            backdrop-filter: blur(8px);
            padding: 8px 20px;
            border-radius: 40px;
            display: inline-block;
            margin-top: 8px;
        }

        .start-btn {
            background: linear-gradient(135deg, #ff66cc, #ff44aa);
            border: none;
            padding: 16px 48px;
            font-size: 1.6rem;
            font-weight: bold;
            color: white;
            border-radius: 60px;
            cursor: pointer;
            animation: pulse 2s infinite;
            box-shadow: 0 0 25px rgba(255, 102, 204, 0.6);
        }

        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.05); }
        }

        .start-btn:active { transform: scale(0.96); }

        .features {
            display: flex;
            gap: 1rem;
            flex-wrap: wrap;
            justify-content: center;
        }

        .feature {
            background: rgba(255,255,255,0.12);
            backdrop-filter: blur(10px);
            padding: 8px 18px;
            border-radius: 40px;
            font-size: 0.8rem;
            color: #aaffff;
            border: 1px solid rgba(0,255,255,0.3);
        }

        /* Map Screen with Scrollable Canvas */
        #mapScreen {
            gap: 0.5rem;
            justify-content: flex-start;
            padding-top: 15px;
            overflow-y: auto;
            overflow-x: hidden;
        }
        
        #mapScreen::-webkit-scrollbar {
            width: 5px;
        }
        
        #mapScreen::-webkit-scrollbar-track {
            background: rgba(0,0,0,0.3);
            border-radius: 10px;
        }
        
        #mapScreen::-webkit-scrollbar-thumb {
            background: linear-gradient(135deg, #ff66cc, #00ffff);
            border-radius: 10px;
        }
        
        #mapScreen h2 {
            font-size: 1.6rem;
            margin: 0;
            background: linear-gradient(135deg, #fff, #4aff9e, #00ffff);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
        }
        .map-sub {
            font-size: 0.7rem;
            background: rgba(0,0,0,0.5);
            backdrop-filter: blur(8px);
            padding: 4px 12px;
            border-radius: 40px;
        }
        .map-container {
            display: flex;
            justify-content: center;
            margin: 10px 0;
        }
        #mapCanvas {
            background: rgba(3, 6, 26, 0.55);
            backdrop-filter: blur(2px);
            border-radius: 40px;
            box-shadow: 0 15px 35px rgba(0,0,0,0.5), 0 0 0 2px rgba(0,255,200,0.2);
            cursor: pointer;
            width: 92vw;
            height: auto;
            max-width: 450px;
            display: block;
        }
        .infinite-badge {
            background: linear-gradient(135deg, rgba(255, 217, 102, 0.9), rgba(255, 153, 102, 0.9));
            backdrop-filter: blur(8px);
            color: #1a1a2e;
            padding: 5px 16px;
            border-radius: 40px;
            font-weight: bold;
            font-size: 0.75rem;
            margin-top: 5px;
        }
        .status-badge {
            font-size: 11px;
            background: rgba(10,15,26,0.7);
            backdrop-filter: blur(8px);
            padding: 4px 12px;
            border-radius: 20px;
            margin-bottom: 10px;
        }
        footer {
            font-size: 10px;
            opacity: 0.7;
            background: rgba(0,0,0,0.4);
            padding: 4px 12px;
            border-radius: 20px;
            margin-bottom: 15px;
        }
        .scroll-hint {
            font-size: 10px;
            color: #88aaff;
            background: rgba(0,0,0,0.4);
            padding: 3px 12px;
            border-radius: 20px;
            margin-top: 5px;
        }

        /* Game Screen */
        #gameScreen {
            gap: 0.6rem;
            justify-content: center;
            background: transparent;
        }
        .game-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            width: min(400px, 90vw);
            background: rgba(10,20,40,0.75);
            backdrop-filter: blur(12px);
            border-radius: 50px;
            padding: 6px 16px;
            border: 1px solid rgba(0,255,255,0.4);
        }
        .back-btn, .next-level-btn {
            background: linear-gradient(135deg, #1e2a4a, #0f172a);
            border: none;
            padding: 6px 16px;
            border-radius: 40px;
            color: cyan;
            font-weight: bold;
            cursor: pointer;
            font-size: 0.85rem;
        }
        .back-btn:active, .next-level-btn:active { transform: scale(0.96); }
        .level-badge {
            background: #0b1120cc;
            backdrop-filter: blur(4px);
            padding: 4px 16px;
            border-radius: 40px;
            font-weight: bold;
        }
        .level-badge span {
            color: #5effbc;
            font-size: 1.4rem;
            font-weight: 800;
        }
        #gameCanvas {
            background: rgba(4, 7, 28, 0.65);
            backdrop-filter: blur(1px);
            border-radius: 32px;
            box-shadow: 0 15px 30px rgba(0, 0, 0, 0.5), inset 0 0 0 2px rgba(0,255,200,0.3);
            touch-action: none;
            cursor: crosshair;
            width: min(75vw, 75vh, 380px);
            height: min(75vw, 75vh, 380px);
            display: block;
        }
        .hint {
            font-size: 0.7rem;
            background: rgba(0,0,0,0.55);
            backdrop-filter: blur(8px);
            border-radius: 24px;
            padding: 5px 14px;
            color: #bbddff;
        }
        .error-flash {
            animation: errorFlash 0.2s ease-in-out 3;
        }
        @keyframes errorFlash {
            0% { box-shadow: inset 0 0 0 2px rgba(0,255,200,0.3); }
            50% { box-shadow: inset 0 0 0 2px rgba(255,80,80,0.8), 0 0 20px 5px rgba(255,0,0,0.8); }
            100% { box-shadow: inset 0 0 0 2px rgba(0,255,200,0.3); }
        }

        /* EXCELLENT ANIMATION STYLES */
        .excellent-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 1000;
            display: flex;
            align-items: center;
            justify-content: center;
            background: radial-gradient(circle, rgba(0,0,0,0.3) 0%, rgba(0,0,0,0) 100%);
            animation: bgFade 0.8s ease-out forwards;
        }
        @keyframes bgFade {
            0% { backdrop-filter: blur(0px); }
            100% { backdrop-filter: blur(8px); }
        }
        .excellent-text {
            font-size: 4.5rem;
            font-weight: 900;
            background: linear-gradient(135deg, #ffd700, #ffaa00, #ff6600, #ff3388, #ff00cc);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-shadow: 0 0 30px rgba(255,215,0,0.8);
            animation: textPop 0.5s cubic-bezier(0.34, 1.3, 0.64, 1) forwards, textGlow 1s ease-in-out infinite;
            white-space: nowrap;
            letter-spacing: 8px;
        }
        @keyframes textPop {
            0% { transform: scale(0.2) rotate(-15deg); opacity: 0; letter-spacing: 20px; }
            50% { transform: scale(1.2) rotate(2deg); }
            100% { transform: scale(1) rotate(0deg); opacity: 1; letter-spacing: 8px; }
        }
        @keyframes textGlow {
            0%, 100% { text-shadow: 0 0 20px rgba(255,215,0,0.6); }
            50% { text-shadow: 0 0 50px rgba(255,215,0,1), 0 0 20px rgba(255,100,0,0.8); }
        }
        .confetti {
            position: absolute;
            width: 10px;
            height: 10px;
            background: linear-gradient(135deg, #ffd700, #ffaa00);
            border-radius: 2px;
            animation: confettiFall 1.5s ease-out forwards;
            pointer-events: none;
        }
        @keyframes confettiFall {
            0% { transform: translateY(-100vh) rotate(0deg); opacity: 1; }
            100% { transform: translateY(100vh) rotate(720deg); opacity: 0; }
        }
        @media (max-width: 480px) {
            .excellent-text { font-size: 2.2rem; letter-spacing: 4px; }
            .game-title { font-size: 2rem; }
            .start-btn { padding: 12px 35px; font-size: 1.3rem; }
            #mapCanvas { width: 95vw; }
        }
    </style>
</head>
<body>

<div class="cosmic-bg"></div>
<div class="nebula"></div>
<div class="stars" id="starsContainer"></div>

<div id="startScreen" class="screen active">
    <div class="logo-container">
        <h1 class="game-title">FLOW CONNECT</h1>
        <h1 class="game-title" style="font-size: 2rem;">PRO</h1>
        <div class="game-subtitle">✦ SCROLLABLE MAP ✦</div>
    </div>
    <button class="start-btn" id="startBtn">▶ BEGIN JOURNEY</button>
    <div class="features">
        <div class="feature">📜 Scrollable Level Map</div>
        <div class="feature">🚫 Lines Cannot Cross</div>
        <div class="feature">✨ Excellent Celebration</div>
        <div class="feature">♾️ Unlimited Levels</div>
    </div>
    <footer>drag to connect • lines must not intersect!</footer>
</div>

<div id="mapScreen" class="screen">
    <h2>🌀 INFINITE LEVEL MAP</h2>
    <div class="map-sub">✦ scroll to explore more levels ✦</div>
    <div class="map-container">
        <canvas id="mapCanvas" width="400" height="600"></canvas>
    </div>
    <div class="infinite-badge">♾️ UNLIMITED LEVELS • KEEP SCROLLING</div>
    <div class="status-badge" id="mapProgress">🏆 mastering infinite flow</div>
    <div class="scroll-hint">⬇️ swipe up/down to scroll ⬆️</div>
    <footer>✓ tap any unlocked level to play</footer>
</div>

<div id="gameScreen" class="screen">
    <div class="game-header">
        <button class="back-btn" onclick="backToMap()">◀ MAP</button>
        <div class="level-badge"><span>⚡</span> LEVEL <span id="levelText">1</span></div>
        <button class="next-level-btn" onclick="skipToNextLevel()">▶ NEXT</button>
    </div>
    <canvas id="gameCanvas" width="400" height="400"></canvas>
    <div class="hint" id="gameHint">✧ drag to connect • lines cannot cross! ✧</div>
</div>

<script>
    // ====================== COSMIC BACKGROUND ======================
    function createStars() {
        const starsContainer = document.getElementById('starsContainer');
        starsContainer.innerHTML = '';
        for (let i = 0; i < 220; i++) {
            const star = document.createElement('div');
            star.className = 'star';
            const size = Math.random() * 2.5 + 1;
            star.style.width = size + 'px';
            star.style.height = size + 'px';
            star.style.left = Math.random() * 100 + '%';
            star.style.top = Math.random() * 100 + '%';
            star.style.animationDelay = Math.random() * 5 + 's';
            starsContainer.appendChild(star);
        }
    }
    createStars();
    
    // ====================== GAME STATE ======================
    let currentLevel = 1;
    let unlockedLevel = 1;
    let completedLevels = new Set();
    let totalLevelsToShow = 80; // Show up to level 80 initially, expands with scroll
    
    // ====================== SCROLLABLE MAP SYSTEM ======================
    const mapCanvas = document.getElementById("mapCanvas");
    const mctx = mapCanvas.getContext("2d");
    let levelPositions = [];
    let mapOffsetY = 0;
    let mapTotalHeight = 0;
    
    function generateMapPositions(maxLevels) {
        levelPositions = [];
        const startY = 80;
        const spacing = 52;
        for (let i = 0; i < maxLevels; i++) {
            let levelNum = i + 1;
            let waveX = Math.sin(i * 0.45) * 95;
            let x = 200 + waveX + (Math.sin(i * 0.3) * 8);
            let y = startY + i * spacing + Math.cos(i * 0.4) * 6;
            x = Math.min(360, Math.max(40, x));
            levelPositions.push({ x, y, level: levelNum });
        }
        mapTotalHeight = startY + (maxLevels - 1) * spacing + 100;
        // Set canvas height based on content
        mapCanvas.height = Math.max(600, mapTotalHeight);
    }
    
    function drawMap() {
        if (!mctx) return;
        // Show up to unlockedLevel + 30, but at least 40, max 150
        let maxLevels = Math.min(Math.max(unlockedLevel + 35, 45), 150);
        if (maxLevels > totalLevelsToShow) totalLevelsToShow = maxLevels;
        generateMapPositions(totalLevelsToShow);
        
        mctx.clearRect(0, 0, mapCanvas.width, mapCanvas.height);
        
        // Draw connecting lines
        for (let i = 0; i < levelPositions.length - 1; i++) {
            const p1 = levelPositions[i];
            const p2 = levelPositions[i+1];
            const gradient = mctx.createLinearGradient(p1.x, p1.y, p2.x, p2.y);
            gradient.addColorStop(0, "#ff66cc");
            gradient.addColorStop(0.5, "#3effe6");
            gradient.addColorStop(1, "#6c9eff");
            mctx.beginPath();
            mctx.lineWidth = 5;
            mctx.strokeStyle = gradient;
            mctx.shadowBlur = 5;
            mctx.shadowColor = "#00ffff";
            mctx.moveTo(p1.x, p1.y);
            const cpX = (p1.x + p2.x) / 2 + Math.sin(i) * 3;
            const cpY = (p1.y + p2.y) / 2 + Math.cos(i * 0.6) * 2;
            mctx.quadraticCurveTo(cpX, cpY, p2.x, p2.y);
            mctx.stroke();
        }
        mctx.shadowBlur = 0;
        
        // Draw nodes
        levelPositions.forEach((p) => {
            const lvl = p.level;
            mctx.beginPath();
            mctx.arc(p.x, p.y, 16, 0, Math.PI * 2);
            mctx.shadowBlur = 8;
            mctx.shadowColor = lvl <= unlockedLevel ? "#0affff" : "#333";
            
            if (lvl > unlockedLevel) {
                mctx.fillStyle = "#2a2f44cc";
                mctx.fill();
                mctx.fillStyle = "#8f9bb5";
            } else if (completedLevels.has(lvl)) {
                mctx.fillStyle = "#2ecc71";
                mctx.fill();
                mctx.fillStyle = "white";
            } else {
                mctx.fillStyle = "#1e88e5";
                mctx.fill();
                mctx.fillStyle = "white";
            }
            mctx.shadowBlur = 0;
            mctx.font = "bold 14px 'Segoe UI'";
            mctx.fillText(lvl, p.x - 6, p.y + 5);
            
            if (lvl > unlockedLevel) {
                mctx.font = "10px sans-serif";
                mctx.fillStyle = "#ffaa66";
                mctx.fillText("🔒", p.x + 11, p.y - 7);
            } else if (completedLevels.has(lvl)) {
                mctx.font = "11px sans-serif";
                mctx.fillStyle = "#ffff99";
                mctx.fillText("✓", p.x + 10, p.y - 8);
            }
        });
        
        document.getElementById("mapProgress").innerHTML = `🌟 ${completedLevels.size} mastered | ∞ unlocked: ${unlockedLevel}+ | total: ${totalLevelsToShow}+ levels`;
    }
    
    // Handle map clicks with scrolling offset
    function handleMapClick(e) {
        const rect = mapCanvas.getBoundingClientRect();
        const scaleX = mapCanvas.width / rect.width;
        const scaleY = mapCanvas.height / rect.height;
        let clientX, clientY;
        if (e.touches) {
            clientX = e.touches[0].clientX;
            clientY = e.touches[0].clientY;
        } else {
            clientX = e.clientX;
            clientY = e.clientY;
        }
        let canvasX = (clientX - rect.left) * scaleX;
        let canvasY = (clientY - rect.top) * scaleY;
        
        for (let node of levelPositions) {
            if (Math.hypot(node.x - canvasX, node.y - canvasY) < 22) {
                if (node.level <= unlockedLevel) {
                    startLevel(node.level);
                } else {
                    const tip = document.createElement("div");
                    tip.innerText = "✨ Complete previous levels first! ✨";
                    tip.style.cssText = "position:fixed; bottom:20%; left:50%; transform:translateX(-50%); background:#ff9966; padding:8px 20px; border-radius:40px; font-size:12px; z-index:999; backdrop-filter:blur(8px);";
                    document.body.appendChild(tip);
                    setTimeout(() => tip.remove(), 1000);
                }
                break;
            }
        }
    }
    
    mapCanvas.addEventListener("click", handleMapClick);
    mapCanvas.addEventListener("touchstart", (e) => { 
        // Don't prevent default on touchstart for scrolling, but handle click after
        const rect = mapCanvas.getBoundingClientRect();
        const touch = e.touches[0];
        const scaleX = mapCanvas.width / rect.width;
        const scaleY = mapCanvas.height / rect.height;
        let canvasX = (touch.clientX - rect.left) * scaleX;
        let canvasY = (touch.clientY - rect.top) * scaleY;
        
        for (let node of levelPositions) {
            if (Math.hypot(node.x - canvasX, node.y - canvasY) < 22) {
                e.preventDefault();
                if (node.level <= unlockedLevel) startLevel(node.level);
                break;
            }
        }
    }, { passive: false });
    
    // ====================== GAME ENGINE WITH COLLISION ======================
    const canvas = document.getElementById("gameCanvas");
    const ctx = canvas.getContext("2d");
    let balls = [];
    let paths = {};
    let completedColors = new Set();
    let drawing = false;
    let currentColor = null;
    let currentPath = [];
    
    const connectSound = new Audio("https://assets.mixkit.co/active_storage/sfx/270/270-preview.mp3");
    const winSound = new Audio("https://assets.mixkit.co/active_storage/sfx/2013/2013-preview.mp3");
    const errorSound = new Audio("https://assets.mixkit.co/active_storage/sfx/257/257-preview.mp3");
    connectSound.volume = 0.4;
    winSound.volume = 0.5;
    errorSound.volume = 0.5;
    
    function resizeCanvasGame() {
        let size = Math.min(window.innerWidth - 40, window.innerHeight - 140, 380);
        size = Math.max(280, size);
        canvas.width = size;
        canvas.height = size;
        if (balls.length) drawGame();
    }
    window.addEventListener("resize", () => { resizeCanvasGame(); drawGame(); });
    
    // Collision detection
    function segmentsIntersect(p1, p2, p3, p4) {
        function orientation(px, py, qx, qy, rx, ry) {
            const val = (qy - py) * (rx - qx) - (qx - px) * (ry - qy);
            if (val === 0) return 0;
            return val > 0 ? 1 : 2;
        }
        const o1 = orientation(p1.x, p1.y, p2.x, p2.y, p3.x, p3.y);
        const o2 = orientation(p1.x, p1.y, p2.x, p2.y, p4.x, p4.y);
        const o3 = orientation(p3.x, p3.y, p4.x, p4.y, p1.x, p1.y);
        const o4 = orientation(p3.x, p3.y, p4.x, p4.y, p2.x, p2.y);
        if (o1 !== o2 && o3 !== o4) return true;
        return false;
    }
    
    function checkCollisionWithExistingPaths(newStart, newEnd, excludeColor = null) {
        for (let col in paths) {
            if (excludeColor !== null && col === excludeColor) continue;
            const pts = paths[col];
            if (!pts || pts.length < 2) continue;
            for (let i = 0; i < pts.length - 1; i++) {
                if (segmentsIntersect(newStart, newEnd, pts[i], pts[i + 1])) return true;
            }
        }
        return false;
    }
    
    function flashError() {
        canvas.classList.add('error-flash');
        errorSound.play().catch(e => {});
        setTimeout(() => canvas.classList.remove('error-flash'), 300);
    }
    
    // EXCELLENT ANIMATION
    function showExcellentAnimation() {
        const overlay = document.createElement('div');
        overlay.className = 'excellent-overlay';
        
        const textDiv = document.createElement('div');
        textDiv.className = 'excellent-text';
        textDiv.innerHTML = 'EXCELLENT!';
        overlay.appendChild(textDiv);
        document.body.appendChild(overlay);
        
        for (let i = 0; i < 100; i++) {
            setTimeout(() => {
                const confetti = document.createElement('div');
                confetti.className = 'confetti';
                confetti.style.left = Math.random() * 100 + '%';
                confetti.style.background = `linear-gradient(135deg, ${['#ffd700', '#ffaa00', '#ff66cc', '#4aff9e', '#00ffff'][Math.floor(Math.random() * 5)]}, ${['#ff6600', '#ff3388', '#2ecc71'][Math.floor(Math.random() * 3)]})`;
                confetti.style.width = Math.random() * 12 + 5 + 'px';
                confetti.style.height = Math.random() * 12 + 5 + 'px';
                confetti.style.animationDuration = Math.random() * 1 + 1 + 's';
                overlay.appendChild(confetti);
            }, i * 12);
        }
        
        winSound.play().catch(e => {});
        
        setTimeout(() => {
            overlay.style.animation = 'fadeOut 0.5s ease-out forwards';
            setTimeout(() => overlay.remove(), 500);
        }, 1800);
    }
    
    function generateInfiniteLevel(level) {
        balls = [];
        paths = {};
        completedColors.clear();
        drawing = false;
        currentColor = null;
        currentPath = [];
        
        let pairCount = 3 + Math.floor(level / 8);
        if (pairCount > 8) pairCount = 8;
        if (level < 3) pairCount = 3;
        
        const baseColors = ["#ff4d6d", "#4d9eff", "#41e2ba", "#ffbc3c", "#c084fc", "#ff66cc", "#2ecc71", "#f39c12"];
        const colorList = [];
        for (let i = 0; i < pairCount; i++) colorList.push(baseColors[i % baseColors.length]);
        
        const used = [];
        const isOverlap = (x, y, minDist = 52) => {
            for (let p of used) if (Math.hypot(p.x - x, p.y - y) < minDist) return true;
            return false;
        };
        
        for (let i = 0; i < pairCount; i++) {
            const col = colorList[i];
            let pos1, pos2;
            let attempts = 0;
            do {
                pos1 = { x: 40 + Math.random() * (canvas.width - 80), y: 40 + Math.random() * (canvas.height - 80) };
                attempts++;
                if (attempts > 200) break;
            } while (isOverlap(pos1.x, pos1.y, 55));
            used.push(pos1);
            attempts = 0;
            do {
                pos2 = { x: 40 + Math.random() * (canvas.width - 80), y: 40 + Math.random() * (canvas.height - 80) };
                attempts++;
                if (attempts > 200) break;
            } while (isOverlap(pos2.x, pos2.y, 55) || Math.hypot(pos1.x - pos2.x, pos1.y - pos2.y) < 75);
            used.push(pos2);
            balls.push({ x: pos1.x, y: pos1.y, color: col }, { x: pos2.x, y: pos2.y, color: col });
        }
        for (let i = balls.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [balls[i], balls[j]] = [balls[j], balls[i]];
        }
    }
    
    function findBallAt(px, py) {
        for (let b of balls) if (Math.hypot(b.x - px, b.y - py) < 20) return b;
        return null;
    }
    
    function drawGame() {
        if (!ctx) return;
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        
        ctx.strokeStyle = "rgba(100, 150, 255, 0.25)";
        ctx.lineWidth = 0.8;
        for (let i = 0; i < canvas.width; i += 35) {
            ctx.beginPath(); ctx.moveTo(i, 0); ctx.lineTo(i, canvas.height); ctx.stroke();
            ctx.beginPath(); ctx.moveTo(0, i); ctx.lineTo(canvas.width, i); ctx.stroke();
        }
        
        for (let col in paths) {
            const pts = paths[col];
            if (!pts || pts.length < 2) continue;
            ctx.beginPath();
            ctx.lineWidth = 13;
            ctx.lineCap = "round";
            ctx.lineJoin = "round";
            ctx.shadowBlur = 8;
            ctx.shadowColor = col;
            ctx.strokeStyle = col;
            for (let i = 0; i < pts.length - 1; i++) {
                const a = pts[i], b = pts[i+1];
                const mx = (a.x + b.x) / 2, my = (a.y + b.y) / 2;
                if (i === 0) ctx.moveTo(a.x, a.y);
                ctx.quadraticCurveTo(mx, my, b.x, b.y);
            }
            ctx.stroke();
        }
        ctx.shadowBlur = 0;
        
        if (drawing && currentColor && currentPath.length > 1) {
            ctx.beginPath();
            ctx.lineWidth = 13;
            ctx.lineCap = "round";
            ctx.strokeStyle = currentColor;
            ctx.shadowBlur = 10;
            ctx.shadowColor = currentColor;
            for (let i = 0; i < currentPath.length - 1; i++) {
                const a = currentPath[i], b = currentPath[i+1];
                const mx = (a.x + b.x) / 2, my = (a.y + b.y) / 2;
                if (i === 0) ctx.moveTo(a.x, a.y);
                ctx.quadraticCurveTo(mx, my, b.x, b.y);
            }
            ctx.stroke();
            ctx.shadowBlur = 0;
        }
        
        balls.forEach(b => {
            ctx.shadowBlur = 12;
            ctx.shadowColor = b.color;
            ctx.beginPath();
            ctx.arc(b.x, b.y, 18, 0, Math.PI * 2);
            ctx.fillStyle = b.color;
            ctx.fill();
            ctx.shadowBlur = 3;
            ctx.beginPath();
            ctx.arc(b.x, b.y, 10, 0, Math.PI * 2);
            ctx.fillStyle = "#ffffffdd";
            ctx.fill();
            ctx.fillStyle = "#111";
            ctx.font = "bold 12px monospace";
            ctx.fillText("●", b.x-3, b.y+3);
        });
        ctx.shadowBlur = 0;
        
        const totalPairs = balls.length / 2;
        const remaining = totalPairs - completedColors.size;
        document.getElementById("gameHint").innerHTML = remaining === 0 ? "🎉 ALL CONNECTED! 🎉" : `🚫 lines cannot cross • ${remaining} pair${remaining !== 1 ? 's' : ''} left`;
    }
    
    function evaluateAndComplete() {
        const totalPairs = balls.length / 2;
        if (completedColors.size === totalPairs) {
            let allValid = true;
            for (let col of completedColors) if (!paths[col] || paths[col].length < 2) allValid = false;
            if (!allValid) return false;
            
            showExcellentAnimation();
            
            if (!completedLevels.has(currentLevel)) {
                completedLevels.add(currentLevel);
                if (currentLevel === unlockedLevel) unlockedLevel = currentLevel + 1;
                saveProgress();
            }
            
            setTimeout(() => {
                backToMap();
                drawMap();
            }, 2000);
            return true;
        }
        return false;
    }
    
    function finalizeConnection(color, endPoint) {
        if (!currentPath.length) return false;
        const lastPoint = currentPath[currentPath.length - 1];
        if (checkCollisionWithExistingPaths(lastPoint, endPoint, color)) {
            flashError();
            return false;
        }
        const finalPath = [...currentPath, { x: endPoint.x, y: endPoint.y }];
        paths[color] = finalPath;
        completedColors.add(color);
        connectSound.play().catch(e => {});
        drawing = false;
        currentColor = null;
        currentPath = [];
        drawGame();
        evaluateAndComplete();
        return true;
    }
    
    function getCanvasCoords(e) {
        const rect = canvas.getBoundingClientRect();
        const scaleX = canvas.width / rect.width;
        const scaleY = canvas.height / rect.height;
        let clientX, clientY;
        if (e.touches) {
            clientX = e.touches[0].clientX;
            clientY = e.touches[0].clientY;
        } else {
            clientX = e.clientX;
            clientY = e.clientY;
        }
        let x = (clientX - rect.left) * scaleX;
        let y = (clientY - rect.top) * scaleY;
        x = Math.min(Math.max(5, x), canvas.width - 5);
        y = Math.min(Math.max(5, y), canvas.height - 5);
        return { x, y };
    }
    
    function onDragStart(e) {
        e.preventDefault();
        if (drawing) return;
        const { x, y } = getCanvasCoords(e);
        const ball = findBallAt(x, y);
        if (!ball) return;
        if (completedColors.has(ball.color)) return;
        
        drawing = true;
        currentColor = ball.color;
        if (paths[currentColor]) delete paths[currentColor];
        currentPath = [{ x: ball.x, y: ball.y }];
        drawGame();
    }
    
    function onDragMove(e) {
        if (!drawing || !currentColor) return;
        e.preventDefault();
        const { x, y } = getCanvasCoords(e);
        const newPoint = { x, y };
        const lastPoint = currentPath[currentPath.length - 1];
        
        if (checkCollisionWithExistingPaths(lastPoint, newPoint, currentColor)) {
            flashError();
            return;
        }
        
        currentPath.push(newPoint);
        if (currentPath.length > 250) currentPath.shift();
        
        const targetBall = findBallAt(x, y);
        if (targetBall && targetBall.color === currentColor && !completedColors.has(currentColor)) {
            const startPos = currentPath[0];
            const isSelf = (Math.abs(targetBall.x - startPos.x) < 8 && Math.abs(targetBall.y - startPos.y) < 8);
            if (!isSelf) {
                finalizeConnection(currentColor, { x: targetBall.x, y: targetBall.y });
                return;
            }
        }
        drawGame();
    }
    
    function onDragEnd(e) {
        if (!drawing) return;
        drawing = false;
        currentColor = null;
        currentPath = [];
        drawGame();
    }
    
    canvas.addEventListener("mousedown", onDragStart);
    canvas.addEventListener("mousemove", onDragMove);
    canvas.addEventListener("mouseup", onDragEnd);
    canvas.addEventListener("touchstart", onDragStart, { passive: false });
    canvas.addEventListener("touchmove", onDragMove, { passive: false });
    canvas.addEventListener("touchend", onDragEnd);
    
    function startLevel(lvl) {
        currentLevel = lvl;
        document.getElementById("levelText").innerText = lvl;
        resizeCanvasGame();
        generateInfiniteLevel(lvl);
        drawGame();
        document.getElementById("mapScreen").classList.remove("active");
        document.getElementById("gameScreen").classList.add("active");
        drawing = false;
        currentColor = null;
        currentPath = [];
    }
    
    function backToMap() {
        document.getElementById("gameScreen").classList.remove("active");
        document.getElementById("mapScreen").classList.add("active");
        drawMap();
    }
    
    function skipToNextLevel() {
        let next = currentLevel + 1;
        if (next > unlockedLevel) { unlockedLevel = next; saveProgress(); }
        startLevel(next);
    }
    
    function saveProgress() {
        try {
            let completedArray = Array.from(completedLevels);
            if (completedArray.length > 1000) completedArray = completedArray.slice(-1000);
            localStorage.setItem("flowInfiniteSave", JSON.stringify({ unlocked: unlockedLevel, completed: completedArray }));
        } catch(e) {}
    }
    
    function loadProgress() {
        try {
            const saved = localStorage.getItem("flowInfiniteSave");
            if (saved) {
                const data = JSON.parse(saved);
                if (data.unlocked) unlockedLevel = Math.max(1, data.unlocked);
                if (data.completed) data.completed.forEach(lvl => completedLevels.add(lvl));
            }
        } catch(e) {}
        if (unlockedLevel < 1) unlockedLevel = 1;
        totalLevelsToShow = Math.max(unlockedLevel + 40, 50);
    }
    
    function startGameFromEntry() {
        document.getElementById("startScreen").classList.remove("active");
        document.getElementById("mapScreen").classList.add("active");
        drawMap();
        resizeCanvasGame();
    }
    
    document.getElementById("startBtn").addEventListener("click", startGameFromEntry);
    
    function init() {
        loadProgress();
        generateMapPositions(totalLevelsToShow);
        resizeCanvasGame();
        drawMap();
        function animate() {
            if (document.getElementById("mapScreen").classList.contains("active")) drawMap();
            else if (document.getElementById("gameScreen").classList.contains("active")) drawGame();
            requestAnimationFrame(animate);
        }
        animate();
    }
    init();
    window.backToMap = backToMap;
    window.skipToNextLevel = skipToNextLevel;
</script>
</body>
</html>
