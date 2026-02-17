<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Rocket Casino - Ultimate</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&display=swap');

        :root {
            --bg-dark: #0f172a;
            --bg-panel: #1e293b;
            --primary: #3b82f6;
            --success: #10b981;
            --danger: #ef4444;
            --text-main: #f8fafc;
            --text-muted: #94a3b8;
            --accent: #f59e0b;
            --vip-gold: #fbbf24;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            user-select: none;
        }

        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg-dark);
            color: var(--text-main);
            height: 100vh;
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }

        /* Header */
        header {
            padding: 1rem 2rem;
            background: var(--bg-panel);
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid #334155;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
            z-index: 10;
        }

        .logo {
            font-weight: 800;
            font-size: 1.5rem;
            color: var(--primary);
            display: flex;
            align-items: center;
            gap: 0.5rem;
            transition: color 0.3s;
        }

        .balance-container {
            background: #0f172a;
            padding: 0.5rem 1rem;
            border-radius: 99px;
            border: 1px solid #334155;
            font-weight: 600;
            display: flex;
            align-items: center;
            gap: 0.5rem;
            position: relative;
            overflow: hidden;
        }

        .balance-amount {
            color: var(--success);
        }

        .vip-badge {
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: linear-gradient(45deg, transparent, rgba(251, 191, 36, 0.1), transparent);
            transform: rotate(45deg);
            animation: shine 3s infinite;
            pointer-events: none;
        }

        @keyframes shine {
            0% { transform: translateX(-100%) rotate(45deg); }
            100% { transform: translateX(100%) rotate(45deg); }
        }

        /* Main Game Area */
        main {
            flex: 1;
            display: flex;
            flex-direction: column;
            position: relative;
        }

        .game-stage {
            flex: 1;
            position: relative;
            background: radial-gradient(circle at center, #1e293b 0%, #0f172a 100%);
            overflow: hidden;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        canvas {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1;
        }

        .multiplier-display {
            position: absolute;
            z-index: 2;
            font-size: 5rem;
            font-weight: 800;
            color: var(--text-main);
            text-shadow: 0 0 20px rgba(59, 130, 246, 0.5);
            transition: color 0.2s;
        }

        .multiplier-display.crashed {
            color: var(--danger);
            text-shadow: 0 0 20px rgba(239, 68, 68, 0.5);
        }

        .status-message {
            position: absolute;
            top: 20%;
            z-index: 2;
            font-size: 1.5rem;
            font-weight: 600;
            color: var(--text-muted);
            opacity: 0;
            transition: opacity 0.3s;
        }

        .luck-indicator {
            position: absolute;
            top: 20px;
            right: 20px;
            background: rgba(251, 191, 36, 0.2);
            border: 1px solid var(--vip-gold);
            color: var(--vip-gold);
            padding: 0.5rem 1rem;
            border-radius: 8px;
            font-weight: bold;
            display: none;
            z-index: 5;
            box-shadow: 0 0 15px rgba(251, 191, 36, 0.3);
            animation: pulse-gold 2s infinite;
        }

        @keyframes pulse-gold {
            0% { box-shadow: 0 0 10px rgba(251, 191, 36, 0.3); }
            50% { box-shadow: 0 0 25px rgba(251, 191, 36, 0.6); }
            100% { box-shadow: 0 0 10px rgba(251, 191, 36, 0.3); }
        }

        /* Controls */
        .controls-area {
            background: var(--bg-panel);
            padding: 1.5rem;
            border-top: 1px solid #334155;
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 1rem;
            z-index: 10;
        }

        .bet-panel {
            display: flex;
            flex-direction: column;
            gap: 0.5rem;
        }

        .input-group {
            display: flex;
            gap: 0.5rem;
        }

        input[type="number"], input[type="text"] {
            background: #0f172a;
            border: 1px solid #334155;
            color: white;
            padding: 0.75rem;
            border-radius: 0.5rem;
            font-size: 1rem;
            width: 100%;
            outline: none;
        }

        input[type="number"]:focus, input[type="text"]:focus {
            border-color: var(--primary);
        }

        .btn {
            padding: 0.75rem 1.5rem;
            border: none;
            border-radius: 0.5rem;
            font-weight: 600;
            font-size: 1rem;
            cursor: pointer;
            transition: all 0.2s;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .btn-primary {
            background: var(--primary);
            color: white;
            box-shadow: 0 4px 14px 0 rgba(59, 130, 246, 0.39);
        }

        .btn-primary:hover {
            background: #2563eb;
            transform: translateY(-1px);
        }

        .btn-primary:disabled {
            background: #475569;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }

        .btn-cashout {
            background: var(--accent);
            color: #0f172a;
            box-shadow: 0 4px 14px 0 rgba(245, 158, 11, 0.39);
            display: none;
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.02); }
            100% { transform: scale(1); }
        }

        /* Promo Section */
        .promo-section {
            display: flex;
            flex-direction: column;
            gap: 0.5rem;
            border-left: 1px solid #334155;
            padding-left: 1rem;
        }

        .promo-input-group {
            display: flex;
            gap: 0.5rem;
        }

        .btn-promo {
            background: #334155;
            color: white;
            white-space: nowrap;
        }
        
        .btn-promo:hover {
            background: #475569;
        }

        .toast {
            position: fixed;
            top: 20px;
            right: 20px;
            background: var(--bg-panel);
            border-left: 4px solid var(--success);
            padding: 1rem;
            border-radius: 4px;
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.5);
            transform: translateX(200%);
            transition: transform 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            z-index: 100;
            display: flex;
            align-items: center;
            gap: 0.5rem;
            min-width: 300px;
        }

        .toast.show {
            transform: translateX(0);
        }

        .toast.error {
            border-left-color: var(--danger);
        }

        .toast.vip {
            border-left-color: var(--vip-gold);
            background: linear-gradient(90deg, #1e293b, #422006);
        }

        /* Responsive */
        @media (max-width: 768px) {
            .controls-area {
                grid-template-columns: 1fr;
            }
            .promo-section {
                border-left: none;
                border-top: 1px solid #334155;
                padding-left: 0;
                padding-top: 1rem;
            }
            .multiplier-display {
                font-size: 3.5rem;
            }
        }
    </style>
</head>
<body>

    <header>
        <div class="logo" id="logoElement">
            <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4.5 16.5c-1.5 1.26-2 5-2 5s3.74-.5 5-2c.71-.84.7-2.13-.09-2.91a2.18 2.18 0 0 0-2.91-.09z"></path><path d="m12 15-3-3a22 22 0 0 1 2-3.95A12.88 12.88 0 0 1 22 2c0 2.72-.78 7.5-6 11a22.35 22.35 0 0 1-4 2z"></path><path d="M9 12H4s.55-3.03 2-4c1.62-1.08 5 0 5 0"></path><path d="M12 15v5s3.03-.55 4-2c1.08-1.62 0-5 0-5"></path></svg>
            ROCKET BET
        </div>
        <div class="balance-container">
            <div class="vip-badge"></div>
            <span>Баланс:</span>
            <span class="balance-amount" id="balanceDisplay">$100.00</span>
        </div>
    </header>

    <main>
        <div class="game-stage">
            <canvas id="gameCanvas"></canvas>
            <div class="status-message" id="statusMessage">Ожидание ставки...</div>
            <div class="multiplier-display" id="multiplierDisplay">1.00x</div>
            <div class="luck-indicator" id="luckIndicator">
                🍀 УДАЧА x2 АКТИВНА
            </div>
        </div>

        <div class="controls-area">
            <div class="bet-panel">
                <label style="color: var(--text-muted); font-size: 0.875rem;">Сумма ставки</label>
                <div class="input-group">
                    <input type="number" id="betInput" value="10" min="1" max="10000">
                    <button class="btn btn-primary" id="betButton" style="width: 100%;">Ставка</button>
                    <button class="btn btn-cashout" id="cashoutButton">Забрать</button>
                </div>
            </div>

            <div class="promo-section">
                <label style="color: var(--text-muted); font-size: 0.875rem;">Промокод</label>
                <div class="promo-input-group">
                    <input type="text" id="promoInput" placeholder="Введите код" maxlength="20">
                    <button class="btn btn-promo" id="promoButton">Применить</button>
                </div>
            </div>
        </div>
    </main>

    <div class="toast" id="toast">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="20 6 9 17 4 12"></polyline></svg>
        <span id="toastMessage">Успешно!</span>
    </div>

    <script>
        // Game State
        let balance = 100.00;
        let currentBet = 0;
        let isPlaying = false;
        let isCrashed = false;
        let multiplier = 1.00;
        let crashPoint = 0;
        let animationId;
        let startTime;
        
        // Promo State
        const usedCodes = new Set();
        let luckMultiplier = 1.0; // Default luck
        
        // Particle Systems
        let particles = [];
        let explosionParticles = [];
        let shockwave = null;
        let shakeIntensity = 0;

        // DOM Elements
        const balanceDisplay = document.getElementById('balanceDisplay');
        const betInput = document.getElementById('betInput');
        const betButton = document.getElementById('betButton');
        const cashoutButton = document.getElementById('cashoutButton');
        const multiplierDisplay = document.getElementById('multiplierDisplay');
        const statusMessage = document.getElementById('statusMessage');
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        const promoInput = document.getElementById('promoInput');
        const promoButton = document.getElementById('promoButton');
        const toast = document.getElementById('toast');
        const toastMessage = document.getElementById('toastMessage');
        const luckIndicator = document.getElementById('luckIndicator');
        const logoElement = document.getElementById('logoElement');

        // Canvas Setup
        let width, height;
        function resizeCanvas() {
            width = canvas.width = canvas.parentElement.offsetWidth;
            height = canvas.height = canvas.parentElement.offsetHeight;
        }
        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();

        // Utils
        function updateBalance(amount) {
            balance += amount;
            balanceDisplay.textContent = `$${balance.toFixed(2)}`;
        }

        function showToast(msg, type = 'success') {
            toastMessage.textContent = msg;
            toast.className = `toast show ${type}`;
            
            // Reset animation
            void toast.offsetWidth; 
            
            setTimeout(() => {
                toast.classList.remove('show');
            }, 4000);
        }

        function generateCrashPoint() {
            // Standard algorithm with slight house edge
            if (Math.random() < 0.03) return 1.00;
            const e = 2 ** 32;
            const h = crypto.getRandomValues(new Uint32Array(1))[0];
            const crash = Math.floor((100 * e - h) / (e - h)) / 100;
            return Math.max(1.00, crash);
        }

        // Particle Class
        class Particle {
            constructor(x, y, color, speed, size, life) {
                this.x = x;
                this.y = y;
                this.color = color;
                this.angle = Math.random() * Math.PI * 2;
                this.speed = Math.random() * speed;
                this.vx = Math.cos(this.angle) * this.speed;
                this.vy = Math.sin(this.angle) * this.speed;
                this.size = Math.random() * size + 1;
                this.life = life;
                this.maxLife = life;
                this.gravity = 0;
            }

            update() {
                this.x += this.vx;
                this.y += this.vy;
                this.vy += this.gravity;
                this.life--;
                this.size *= 0.95; // Shrink
            }

            draw(ctx) {
                ctx.globalAlpha = this.life / this.maxLife;
                ctx.fillStyle = this.color;
                ctx.beginPath();
                ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
                ctx.fill();
                ctx.globalAlpha = 1.0;
            }
        }

        // Drawing Functions
        function drawRocket(x, y, angle, isFlying) {
            ctx.save();
            ctx.translate(x, y);
            ctx.rotate(angle);
            
            // Rocket Body - Gold if lucky
            const bodyColor = luckMultiplier > 1 ? '#fbbf24' : '#f59e0b';
            const finColor = '#ef4444';
            const windowColor = '#60a5fa';

            // 1. Fins (Back)
            ctx.fillStyle = finColor;
            ctx.beginPath();
            ctx.moveTo(-15, 0);
            ctx.lineTo(-28, -12);
            ctx.lineTo(-20, -5);
            ctx.lineTo(-20, 5);
            ctx.lineTo(-28, 12);
            ctx.fill();

            // 2. Main Body
            ctx.fillStyle = bodyColor;
            ctx.beginPath();
            // Nose
            ctx.moveTo(20, 0);
            ctx.quadraticCurveTo(10, 0, 0, 8);
            // Back
            ctx.lineTo(-20, 8);
            ctx.lineTo(-20, -8);
            ctx.quadraticCurveTo(0, 0, 20, 0);
            ctx.fill();

            // 3. Details (Stripe)
            ctx.fillStyle = 'rgba(255,255,255,0.3)';
            ctx.beginPath();
            ctx.moveTo(5, 0);
            ctx.quadraticCurveTo(0, 0, -5, 6);
            ctx.lineTo(-5, -6);
            ctx.fill();

            // 4. Window
            ctx.fillStyle = windowColor;
            ctx.beginPath();
            ctx.arc(5, 0, 5, 0, Math.PI * 2);
            ctx.fill();
            
            // Window Reflection
            ctx.fillStyle = 'rgba(255,255,255,0.6)';
            ctx.beginPath();
            ctx.arc(7, -2, 1.5, 0, Math.PI * 2);
            ctx.fill();

            // 5. Flame (if flying)
            if (isFlying) {
                const flicker = Math.random() * 10 + 10;
                const flameColor = luckMultiplier > 1 ? '#3b82f6' : '#f97316';
                
                ctx.fillStyle = flameColor;
                ctx.beginPath();
                ctx.moveTo(-20, 0);
                ctx.lineTo(-20 - flicker, -6);
                ctx.lineTo(-20 - flicker * 0.5, 0);
                ctx.lineTo(-20 - flicker, 6);
                ctx.fill();
                
                // Inner flame
                ctx.fillStyle = '#fbbf24';
                ctx.beginPath();
                ctx.moveTo(-20, 0);
                ctx.lineTo(-20 - flicker * 0.6, -3);
                ctx.lineTo(-20 - flicker * 0.3, 0);
                ctx.lineTo(-20 - flicker * 0.6, 3);
                ctx.fill();
            }

            ctx.restore();
        }

        function drawGrid(offset) {
            ctx.strokeStyle = '#334155';
            ctx.lineWidth = 1;
            ctx.beginPath();
            
            const gridSize = 100;
            const offsetX = offset % gridSize;
            
            for (let x = -offsetX; x < width; x += gridSize) {
                ctx.moveTo(x, 0);
                ctx.lineTo(x, height);
            }
            
            for (let y = 0; y < height; y += gridSize) {
                ctx.moveTo(0, y);
                ctx.lineTo(width, y);
            }
            ctx.stroke();
        }

        function drawCurve(progress) {
            ctx.strokeStyle = luckMultiplier > 1 ? '#fbbf24' : '#3b82f6';
            ctx.lineWidth = 4;
            ctx.lineCap = 'round';
            ctx.beginPath();
            ctx.moveTo(0, height);
            
            const endX = Math.min(width * 0.8, 100 + progress * 50);
            const endY = height - Math.min(height * 0.8, 100 + progress * 30);
            
            ctx.quadraticCurveTo(endX / 2, height, endX, endY);
            ctx.stroke();
        }

        function createExplosion(x, y) {
            // Shockwave
            shockwave = { x: x, y: y, radius: 1, alpha: 1 };
            
            // Debris
            for (let i = 0; i < 50; i++) {
                explosionParticles.push(new Particle(x, y, '#f59e0b', 8, 4, 60));
                explosionParticles.push(new Particle(x, y, '#ef4444', 6, 3, 50));
                explosionParticles.push(new Particle(x, y, '#ffffff', 5, 2, 40));
            }
            
            // Smoke
            for (let i = 0; i < 30; i++) {
                explosionParticles.push(new Particle(x, y, '#475569', 4, 8, 80));
            }
            
            shakeIntensity = 20;
        }

        // Game Loop
        function gameLoop(timestamp) {
            // Screen Shake Logic
            let shakeX = 0;
            let shakeY = 0;
            if (shakeIntensity > 0) {
                shakeX = (Math.random() - 0.5) * shakeIntensity;
                shakeY = (Math.random() - 0.5) * shakeIntensity;
                shakeIntensity *= 0.9;
                if (shakeIntensity < 0.5) shakeIntensity = 0;
            }

            ctx.save();
            ctx.translate(shakeX, shakeY);

            if (!isPlaying && !isCrashed && explosionParticles.length === 0) {
                ctx.restore();
                return; // Stop loop if nothing is happening
            }

            ctx.clearRect(0, 0, width, height);
            
            // Background Grid
            if (isPlaying) {
                const elapsed = (timestamp - startTime) / 1000;
                drawGrid(elapsed * 50);
            } else {
                drawGrid(0);
            }

            // Handle Particles
            // 1. Trail Particles (during flight)
            if (isPlaying && !isCrashed) {
                const elapsed = (timestamp - startTime) / 1000;
                const rocketX = Math.min(width * 0.8, 100 + elapsed * 60);
                const rocketY = height - Math.min(height * 0.8, 100 + elapsed * 40);
                
                // Add trail particles
                if (Math.random() > 0.5) {
                    particles.push(new Particle(
                        rocketX - 20, 
                        rocketY, 
                        luckMultiplier > 1 ? '#3b82f6' : '#f97316', 
                        2, 
                        3, 
                        20
                    ));
                }
            }

            // Update and Draw Trail Particles
            for (let i = particles.length - 1; i >= 0; i--) {
                particles[i].update();
                particles[i].draw(ctx);
                if (particles[i].life <= 0) particles.splice(i, 1);
            }

            // Update and Draw Explosion Particles
            for (let i = explosionParticles.length - 1; i >= 0; i--) {
                explosionParticles[i].update();
                explosionParticles[i].draw(ctx);
                if (explosionParticles[i].life <= 0) explosionParticles.splice(i, 1);
            }

            // Draw Shockwave
            if (shockwave) {
                shockwave.radius += 5;
                shockwave.alpha -= 0.02;
                ctx.beginPath();
                ctx.arc(shockwave.x, shockwave.y, shockwave.radius, 0, Math.PI * 2);
                ctx.strokeStyle = `rgba(255, 255, 255, ${shockwave.alpha})`;
                ctx.lineWidth = 5;
                ctx.stroke();
                if (shockwave.alpha <= 0) shockwave = null;
            }

            // Game Logic
            if (isPlaying && !isCrashed) {
                const elapsed = (timestamp - startTime) / 1000;
                
                // Growth formula
                const baseGrowth = (0.1 * elapsed) + (0.05 * Math.pow(elapsed, 2));
                multiplier = 1.00 + (baseGrowth * luckMultiplier);
                
                multiplierDisplay.textContent = `${multiplier.toFixed(2)}x`;

                // Check Crash
                if (multiplier >= crashPoint) {
                    crashGame();
                }

                // Draw Rocket
                const rocketX = Math.min(width * 0.8, 100 + elapsed * 60);
                const rocketY = height - Math.min(height * 0.8, 100 + elapsed * 40);
                const angle = Math.atan2(40, 60);
                
                drawCurve(elapsed);
                drawRocket(rocketX, rocketY, -angle, true);

                // Update Cashout Button Text
                const currentWin = (currentBet * multiplier).toFixed(2);
                cashoutButton.textContent = `Забрать $${currentWin}`;
            }

            ctx.restore();
            animationId = requestAnimationFrame(gameLoop);
        }

        function startGame() {
            const betAmount = parseFloat(betInput.value);
            
            if (isNaN(betAmount) || betAmount <= 0) {
                showToast("Неверная сумма ставки", "error");
                return;
            }
            if (betAmount > balance) {
                showToast("Недостаточно средств", "error");
                return;
            }

            // Deduct bet
            updateBalance(-betAmount);
            currentBet = betAmount;
            isPlaying = true;
            isCrashed = false;
            multiplier = 1.00;
            crashPoint = generateCrashPoint();
            startTime = performance.now();
            
            // Clear old particles
            particles = [];
            explosionParticles = [];
            shockwave = null;

            // UI Updates
            betButton.style.display = 'none';
            cashoutButton.style.display = 'block';
            betInput.disabled = true;
            multiplierDisplay.classList.remove('crashed');
            multiplierDisplay.style.color = 'var(--text-main)';
            statusMessage.style.opacity = '0';
            promoInput.disabled = true;
            promoButton.disabled = true;

            if (!animationId) {
                animationId = requestAnimationFrame(gameLoop);
            }
        }

        function cashOut() {
            if (!isPlaying || isCrashed) return;

            isPlaying = false;
            // Don't cancel animation immediately to let trail fade, but stop logic
            // We set a flag to stop logic updates in loop
            
            const winAmount = currentBet * multiplier;
            updateBalance(winAmount);
            
            statusMessage.textContent = `Вы выиграли $${winAmount.toFixed(2)}!`;
            statusMessage.style.color = 'var(--success)';
            statusMessage.style.opacity = '1';
            
            endGameUI();
            showToast(`Победа! +$${winAmount.toFixed(2)}`);
        }

        function crashGame() {
            isPlaying = false;
            isCrashed = true;
            
            multiplierDisplay.textContent = `${crashPoint.toFixed(2)}x`;
            multiplierDisplay.classList.add('crashed');
            
            statusMessage.textContent = "Ракета улетела!";
            statusMessage.style.color = 'var(--danger)';
            statusMessage.style.opacity = '1';

            // Calculate crash position for explosion
            const elapsed = (performance.now() - startTime) / 1000;
            const rocketX = Math.min(width * 0.8, 100 + elapsed * 60);
            const rocketY = height - Math.min(height * 0.8, 100 + elapsed * 40);

            createExplosion(rocketX, rocketY);

            endGameUI();
            showToast("Вы проиграли ставку", "error");
        }

        function endGameUI() {
            betButton.style.display = 'block';
            cashoutButton.style.display = 'none';
            betInput.disabled = false;
            promoInput.disabled = false;
            promoButton.disabled = false;
            betButton.textContent = "Ставка";

            // Reset Luck for next round
            resetLuck();
        }

        function resetLuck() {
            if (luckMultiplier > 1) {
                luckMultiplier = 1.0;
                luckIndicator.style.display = 'none';
                // Reset theme
                document.documentElement.style.setProperty('--primary', '#3b82f6');
                logoElement.style.color = '#3b82f6';
            }
        }

        // Event Listeners
        betButton.addEventListener('click', startGame);
        cashoutButton.addEventListener('click', cashOut);

        promoButton.addEventListener('click', () => {
            const code = promoInput.value.trim().toUpperCase();
            
            if (usedCodes.has(code)) {
                showToast("Этот код уже был использован", "error");
                return;
            }

            if (code === 'ВЫИГРЫШ10') {
                // Apply VIP Bonus
                updateBalance(1000);
                luckMultiplier = 2.0;
                usedCodes.add(code);
                promoInput.value = '';
                
                // Visual Feedback
                luckIndicator.style.display = 'block';
                document.documentElement.style.setProperty('--primary', '#fbbf24'); // Change theme color to gold
                logoElement.style.color = '#fbbf24';
                showToast("🎉 ВИП БОНУС: +$1000 и УДАЧА x2 на 1 раунд!", "vip");
                
            } else if (code === 'DEP') {
                updateBalance(10);
                usedCodes.add(code);
                promoInput.value = '';
                showToast("Промокод активирован! +$10");
                
            } else if (code === 'ИГРА10') {
                updateBalance(100);
                usedCodes.add(code);
                promoInput.value = '';
                showToast("Промокод активирован! +$100");
                
            } else {
                showToast("Неверный промокод", "error");
            }
        });

        // Initial Draw
        ctx.fillStyle = '#334155';
        ctx.font = "20px Inter";
        ctx.textAlign = "center";
        ctx.fillText("Сделайте ставку, чтобы начать", width/2, height/2);

    </script>
</body>
</html>
