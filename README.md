<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ramo de Flores Amarillas</title>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    background: radial-gradient(circle at bottom, #070707, #000);
    height: 100vh;
    overflow: hidden;
    font-family: 'Arial', sans-serif;
}

canvas {
    position: absolute;
    inset: 0;
    z-index: 0;
}

.scene {
    position: relative;
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: flex-end;
    padding-bottom: 60px;
    z-index: 1;
}

/* Ramo responsive */
.bouquet {
    position: relative;
    width: min(280px, 90vw);
    height: min(360px, 70vh);
    transform: scale(1);
}

/* Flor */
.flower {
    position: absolute;
    bottom: 0;
    transform-origin: bottom center;
    animation: sway 7s ease-in-out infinite;
}

.stem {
    width: 4px;
    height: 0;
    background: linear-gradient(#1b5e20, #66bb6a);
    margin: 0 auto;
    animation: growStem 2.5s ease-out forwards;
}

.bloom {
    position: relative;
    width: 42px;
    height: 42px;
    margin: 0 auto;
    transform: scale(0);
    animation: bloom 1.6s ease-out forwards;
}

.petal {
    position: absolute;
    width: 18px;
    height: 36px;
    background: linear-gradient(#fff9c4, #fbc02d);
    border-radius: 50%;
    top: 2px;
    left: 12px;
    opacity: 0;
    transform-origin: bottom center;
    filter: drop-shadow(0 0 6px rgba(255,235,59,.5));
    animation: petalOpen 1.2s ease-out forwards;
}

.center {
    position: absolute;
    width: 14px;
    height: 14px;
    background: #f9a825;
    border-radius: 50%;
    top: 30px;
    left: 14px;
    opacity: 0;
    box-shadow: 0 0 12px rgba(255,193,7,.9);
    animation: fadeIn 1s ease-out forwards;
}

/* Animaciones */
@keyframes growStem { to { height: var(--height); } }
@keyframes bloom { to { transform: scale(1); } }
@keyframes petalOpen { to { opacity: 1; } }
@keyframes fadeIn { to { opacity: 1; } }

@keyframes sway {
    0% { transform: rotate(var(--angle)); }
    50% { transform: rotate(calc(var(--angle) + 1.5deg)); }
    100% { transform: rotate(var(--angle)); }
}

/* Texto */
.message {
    position: absolute;
    bottom: 350px;
    width: 100%;
    text-align: center;
    color: #fdd835;
    letter-spacing: 3px;
    font-size: clamp(11px, 3.5vw, 14px);
    opacity: 0;
    animation: textIn 2s ease forwards;
    animation-delay: 5s;
}

@keyframes textIn { to { opacity: 1; } }
</style>
</head>

<body>

<canvas id="stars"></canvas>

<div class="scene">
    <div class="bouquet"></div>

    <div class="message">
        FLORES AMARILLAS PARA ILUMINAR TU DÍA
    </div>
    
</div>

<script>
const bouquet = document.querySelector('.bouquet');

const flowersData = [
    { left: '50%', angle: 0, height: '180px', delay: 0, offset: -20 },
    { left: '35%', angle: -15, height: '150px', delay: .2, offset: -20 },
    { left: '65%', angle: 15, height: '150px', delay: .4, offset: -20 },
    { left: '25%', angle: -30, height: '120px', delay: .6, offset: -20 },
    { left: '75%', angle: 30, height: '120px', delay: .8, offset: -20 },
];

flowersData.forEach(f => {
    const flower = document.createElement('div');
    flower.className = 'flower';
    flower.style.left = f.left;
    flower.style.transform = `translateX(${f.offset}px)`;
    flower.style.setProperty('--angle', f.angle + 'deg');

    const bloom = document.createElement('div');
    bloom.className = 'bloom';
    bloom.style.animationDelay = (2.5 + f.delay) + 's';

    for (let i = 0; i < 6; i++) {
        const petal = document.createElement('div');
        petal.className = 'petal';
        petal.style.transform = `rotate(${i * 60}deg)`;
        petal.style.animationDelay = (2.7 + f.delay + i * 0.1) + 's';
        bloom.appendChild(petal);
    }

    const center = document.createElement('div');
    center.className = 'center';
    center.style.animationDelay = (3.7 + f.delay) + 's';
    bloom.appendChild(center);

    const stem = document.createElement('div');
    stem.className = 'stem';
    stem.style.setProperty('--height', f.height);
    stem.style.animationDelay = f.delay + 's';

    flower.appendChild(bloom);
    flower.appendChild(stem);
    bouquet.appendChild(flower);
});

// Estrellas suaves (ligeras para móvil)
const canvas = document.getElementById('stars');
const ctx = canvas.getContext('2d');
canvas.width = innerWidth;
canvas.height = innerHeight;

const stars = Array.from({ length: 80 }, () => ({
    x: Math.random() * canvas.width,
    y: Math.random() * canvas.height,
    r: Math.random() * 1.2,
    a: Math.random()
}));

function drawStars() {
    ctx.clearRect(0,0,canvas.width,canvas.height);
    stars.forEach(s => {
        ctx.fillStyle = `rgba(255,255,255,${s.a})`;
        ctx.beginPath();
        ctx.arc(s.x, s.y, s.r, 0, Math.PI * 2);
        ctx.fill();
        s.a += (Math.random() - 0.5) * 0.015;
        s.a = Math.max(0.1, Math.min(0.8, s.a));
    });
    requestAnimationFrame(drawStars);
}
drawStars();
</script>

</body>
</html>
