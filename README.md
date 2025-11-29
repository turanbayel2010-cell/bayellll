<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <title>Убегающее облако за экран</title>
  <style>
    body {
      margin: 0;
      height: 100vh;
      background: black;
      overflow: hidden;
      perspective: 800px;
      font-family: 'Arial', sans-serif;
      cursor: pointer;
    }

    .cloud {
      width: 200px;
      height: 100px;
      background: white;
      border-radius: 50%;
      position: absolute;
      top: 100px;
      left: -250px;
      box-shadow: 0 10px 30px rgba(255, 255, 255, 0.5);
      transform-style: preserve-3d;
      display: flex;
      justify-content: center;
      align-items: center;
      color: #333;
      font-size: 20px;
      font-weight: bold;
      text-shadow: 1px 1px 2px rgba(0,0,0,0.5);
      transition: transform 0.2s, left 0.5s, top 0.5s;
    }

    .cloud::before,
    .cloud::after {
      content: '';
      position: absolute;
      background: white;
      width: 150px;
      height: 100px;
      border-radius: 50%;
    }

    .cloud::before {
      top: -20px;
      left: 20px;
    }

    .cloud::after {
      top: -10px;
      left: 80px;
    }
  </style>
</head>
<body>
  <div class="cloud" id="cloud">Поймай меня!</div>

  <!-- Звук -->
  <audio id="scream" src="https://www.soundjay.com/human/sounds/shout-01.mp3"></audio>

  <script>
    const cloud = document.getElementById('cloud');
    const scream = document.getElementById('scream');

    let posX = -250;
    let posY = 150;
    const speed = 0.3;
    let hidden = false; // состояние облака

    function animateCloud() {
      if (!hidden) {
        posX += speed;
        if (posX > window.innerWidth) {
          posX = -250;
          posY = Math.random() * (window.innerHeight - 120);
        }
        const scale = 0.8 + 0.4 * Math.sin(Date.now() / 500);
        const rotateY = (Date.now() / 1000) % 360;
        cloud.style.transform = `translate3d(${posX}px, ${posY}px, 0px) scale(${scale}) rotateY(${rotateY}deg)`;
      }
      requestAnimationFrame(animateCloud);
    }

    animateCloud();

    // убегание при наведении
    cloud.addEventListener('mouseenter', () => {
      if (!hidden) {
        posX = Math.random() * (window.innerWidth - 200);
        posY = Math.random() * (window.innerHeight - 100);
      }
    });

    // при клике облако “убегает за экран” и играет звук
    cloud.addEventListener('click', () => {
      hidden = true; // скрываем облако
      scream.currentTime = 0;
      scream.play();

      // двигаем облако за экран справа
      cloud.style.left = window.innerWidth + 'px';

      // через 2 секунды возвращаем облако слева
      setTimeout(() => {
        posX = -250;
        posY = Math.random() * (window.innerHeight - 120);
        hidden = false;
      }, 2000);
    });
  </script>
</body>
</html>
