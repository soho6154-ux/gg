<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Be My Valentine 💖</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Pacifico&display=swap');

    html, body {
      margin: 0;
      padding: 0;
      width: 100%;
      height: 100%;
      overflow: hidden;
      font-family: 'Pacifico', cursive;
      background: linear-gradient(120deg, #ff9a9e, #fad0c4);
      display: flex;
      justify-content: center;
      align-items: center;
    }

    .container {
      text-align: center;
      background: rgba(255, 255, 255, 0.85);
      padding: 50px;
      border-radius: 20px;
      box-shadow: 0 0 30px rgba(0,0,0,0.2);
      position: relative;
      z-index: 1;
    }

    h1 {
      font-size: 3em;
      margin-bottom: 20px;
      color: #ff4d6d;
    }

    p {
      font-size: 1.5em;
      margin-bottom: 30px;
      color: #ff4d6d;
    }

    button {
      font-size: 1.2em;
      padding: 15px 30px;
      margin: 10px;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      transition: transform 0.2s, background 0.3s;
      color: white;
    }

    #yesBtn {
      background: #ff4d6d;
    }

    #noBtn {
      background: #ffb6b9;
      position: relative;
    }

    .heart {
      position: absolute;
      width: 20px;
      height: 20px;
      background: red;
      transform: rotate(-45deg);
      animation: floatUp linear infinite;
    }

    .heart:before,
    .heart:after {
      content: "";
      position: absolute;
      width: 20px;
      height: 20px;
      background: red;
      border-radius: 50%;
    }

    .heart:before { top: -10px; left: 0; }
    .heart:after { left: 10px; top: 0; }

    @keyframes floatUp {
      0% { transform: translateY(0) rotate(-45deg); opacity: 1; }
      100% { transform: translateY(-100vh) rotate(-45deg); opacity: 0; }
    }

    canvas {
      position: absolute;
      top: 0;
      left: 0;
      pointer-events: none;
      z-index: 0;
    }
  </style>
</head>
<body>
  <canvas id="confetti"></canvas>
  <div class="container">
    <h1>💖 Will You Be My Valentine? 💖</h1>
    <p>Click carefully...</p>
    <button id="yesBtn">Yes 😍</button>
    <button id="noBtn">No 😢</button>
    <p id="secretMessage" style="margin-top:20px; font-size:1em; color:#ff4d6d;"></p>
  </div>

  <script>
    // Elements
    const yesBtn = document.getElementById('yesBtn');
    const noBtn = document.getElementById('noBtn');
    const secret = document.getElementById('secretMessage');
    const body = document.body;

    // No Button runs away
    noBtn.addEventListener('mouseenter', () => {
      const x = Math.random() * (window.innerWidth - noBtn.offsetWidth);
      const y = Math.random() * (window.innerHeight - noBtn.offsetHeight);
      noBtn.style.position = 'absolute';
      noBtn.style.left = x + 'px';
      noBtn.style.top = y + 'px';
    });

    // Yes Button triggers confetti & hearts
    yesBtn.addEventListener('click', () => {
      createHearts(50);
      startConfetti();
      secret.textContent = "💖 I knew you'd say yes! 💖";
    });

    // Random hearts
    function createHearts(num) {
      for (let i = 0; i < num; i++) {
        const heart = document.createElement('div');
        heart.className = 'heart';
        heart.style.left = Math.random() * window.innerWidth + 'px';
        heart.style.animationDuration = 2 + Math.random() * 3 + 's';
        body.appendChild(heart);
        setTimeout(() => heart.remove(), 5000);
      }
    }

    // Confetti setup
    const canvas = document.getElementById('confetti');
    const ctx = canvas.getContext('2d');
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;

    let confettiParticles = [];

    function createConfetti(num) {
      for (let i = 0; i < num; i++) {
        confettiParticles.push({
          x: Math.random() * canvas.width,
          y: Math.random() * canvas.height,
          r: Math.random() * 6 + 4,
          d: Math.random() * num,
          color: `hsl(${Math.random()*360},100%,50%)`,
          tilt: Math.random() * 10 - 10,
          tiltAngleIncrement: Math.random() * 0.07 + 0.05,
          tiltAngle: 0
        });
      }
    }

    function drawConfetti() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      confettiParticles.forEach(p => {
        ctx.beginPath();
        ctx.lineWidth = p.r / 2;
        ctx.strokeStyle = p.color;
        ctx.moveTo(p.x + p.tilt + p.r / 4, p.y);
        ctx.lineTo(p.x + p.tilt, p.y + p.tilt + p.r / 4);
        ctx.stroke();
      });
      updateConfetti();
    }

    function updateConfetti() {
      confettiParticles.forEach(p => {
        p.tiltAngle += p.tiltAngleIncrement;
        p.y += (Math.cos(p.d) + 3 + p.r / 2) / 2;
        p.tilt = Math.sin(p.tiltAngle) * 15;
        if (p.y > canvas.height) p.y = -10;
      });
    }

    let confettiInterval;
    function startConfetti() {
      confettiParticles = [];
      createConfetti(150);
      if(confettiInterval) clearInterval(confettiInterval);
      confettiInterval = setInterval(drawConfetti, 20);
      setTimeout(() => clearInterval(confettiInterval), 5000); // stop after 5s
    }

    // Secret message on body click
    body.addEventListener('click', (e) => {
      if (e.target === body) {
        secret.textContent = "✨ You found the secret! ✨";
      }
    });

    // Adjust canvas on resize
    window.addEventListener('resize', () => {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
    });
  </script>
</body>
</html>
