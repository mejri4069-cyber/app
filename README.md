<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>For You 💌</title>

  <!-- Google Font -->
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">

  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Poppins', sans-serif; }

    body {
      background: linear-gradient(135deg, #fce4ec, #f8bbd0, #e1bee7);
      height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      overflow: hidden;
    }

    .card {
      background: rgba(255, 255, 255, 0.2);
      backdrop-filter: blur(15px);
      border-radius: 30px;
      padding: 40px;
      width: 90%;
      max-width: 450px;
      text-align: center;
      box-shadow: 0 10px 40px rgba(0,0,0,0.15);
      animation: fadeIn 1.5s ease;
    }

    h1 { color: #880e4f; margin-bottom: 15px; font-weight: 600; }
    h2 { color: #880e4f; margin-bottom: 15px; font-weight: 600; }

    p { color: #6a1b9a; font-size: 15px; margin-bottom: 25px; }

    button {
      background: linear-gradient(135deg, #ff80ab, #b388ff);
      border: none;
      padding: 12px 22px;
      border-radius: 20px;
      color: white;
      font-size: 14px;
      cursor: pointer;
      transition: transform 0.2s ease, box-shadow 0.2s ease;
    }

    button:hover {
      transform: scale(1.05);
      box-shadow: 0 5px 20px rgba(0,0,0,0.2);
    }

    .hidden { display: none; margin-top: 20px; animation: fadeIn 1s ease; }

    img {
      width: 100%;
      border-radius: 20px;
      margin-top: 15px;
      box-shadow: 0 5px 20px rgba(0,0,0,0.2);
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(20px); }
      to { opacity: 1; transform: translateY(0); }
    }

    .hearts {
      position: absolute;
      width: 100%;
      height: 100%;
      pointer-events: none;
      overflow: hidden;
    }

    .heart {
      position: absolute;
      bottom: -20px;
      font-size: 20px;
      animation: floatUp 6s linear infinite;
      opacity: 0.7;
    }

    @keyframes floatUp {
      to { transform: translateY(-110vh) rotate(360deg); opacity: 0; }
    }

    /* Memory Game Cards */
    .memoryCard {
      width: 80px; height: 120px;
      background: #f8bbd0; border-radius:12px;
      display:flex; align-items:center; justify-content:center;
      font-size:24px; cursor:pointer;
      box-shadow: 0 5px 15px rgba(0,0,0,0.2);
      transition: transform 0.5s;
      user-select: none;
    }

    .memoryCard.flipped {
      background: #ff80ab;
      transform: rotateY(180deg);
    }

    #cardsContainer {
      display:flex;
      flex-wrap:wrap;
      justify-content:center;
      gap:10px;
      margin-top: 15px;
    }

  </style>
</head>

<body>
  <div class="hearts" id="hearts"></div>

  <div class="card">
    <h1>Hey You 💗</h1>
    <p>I made something small… but full of feelings just for you.</p>
    <button onclick="showSurprise()">Open the surprise ✨</button>
    <button onclick="startGame()" style="margin-top:15px;">Play Memory Game 🎴</button>

    <div class="hidden" id="surprise">
      <p>
        You make ordinary days feel magical.<br>
        Your smile = my favorite notification.<br><br>
        Thank you for being in my life 💕
      </p>
      <img src="https://picsum.photos/400/300?romantic" alt="memory" />
      <button onclick="nextMessage()" style="margin-top:15px;">One more thing… 🌸</button>
    </div>

    <div class="hidden" id="final">
      <h1>Happy Valentine’s Day 💘</h1>
      <p>
        If I had one wish…<br>
        it would be more moments with you ✨
      </p>
    </div>

    <!-- Memory Game -->
    <div class="hidden" id="memoryGame">
      <h2>Memory Game 💌</h2>
      <p>Click on the cards to reveal them and answer the couple questions!</p>
      <div id="cardsContainer"></div>
      <p id="gameMessage" style="margin-top:15px; color:#880e4f;"></p>
      <button onclick="endGame()" style="margin-top:10px;">Finish Game 🎉</button>
    </div>

  </div>

  <script>
    // Hearts animation
    function createHearts() {
      const container = document.getElementById('hearts');
      setInterval(() => {
        const heart = document.createElement('div');
        heart.className = 'heart';
        heart.innerText = '💖';
        heart.style.left = Math.random() * 100 + 'vw';
        heart.style.animationDuration = (4 + Math.random() * 3) + 's';
        container.appendChild(heart);
        setTimeout(() => heart.remove(), 7000);
      }, 400);
    }

    function showSurprise() {
      document.getElementById('surprise').style.display = 'block';
      createHearts();
    }

    function nextMessage() {
      document.getElementById('final').style.display = 'block';
    }

    // Memory Game
    const coupleQuestions = [
      "What's our first date memory?",
      "Favorite thing about your partner?",
      "A dream vacation together?",
      "Your partner's cutest habit?",
      "Song that reminds you of us?"
    ];

    const memoryCards = [];

    let firstCard = null;
    let secondCard = null;

    function startGame() {
      document.getElementById('memoryGame').style.display = 'block';
      const container = document.getElementById('cardsContainer');
      container.innerHTML = '';
      memoryCards.length = 0;

      // generate pairs
      for (let i = 0; i < coupleQuestions.length; i++) {
        memoryCards.push({ id: i, question: coupleQuestions[i], flipped: false });
        memoryCards.push({ id: i, question: coupleQuestions[i], flipped: false });
      }

      // shuffle
      memoryCards.sort(() => Math.random() - 0.5);

      memoryCards.forEach((card, index) => {
        const div = document.createElement('div');
        div.className = 'memoryCard';
        div.dataset.index = index;
        div.innerText = '❓';
        div.addEventListener('click', flipCard);
        container.appendChild(div);
      });
    }

    function flipCard() {
      const idx = this.dataset.index;
      const card = memoryCards[idx];
      if (card.flipped) return;

      this.classList.add('flipped');
      this.innerText = '💖';
      card.flipped = true;

      if (!firstCard) {
        firstCard = { card, element: this };
      } else {
        secondCard = { card, element: this };
        checkMatch();
      }
    }

    function checkMatch() {
      if (firstCard.card.id === secondCard.card.id) {
        document.getElementById('gameMessage').innerText = firstCard.card.question;
      } else {
        setTimeout(() => {
          firstCard.element.classList.remove('flipped');
          secondCard.element.classList.remove('flipped');
          firstCard.element.innerText = '❓';
          secondCard.element.innerText = '❓';
          firstCard.card.flipped = false;
          secondCard.card.flipped = false;
        }, 1000);
        document.getElementById('gameMessage').innerText = "Try again 💕";
      }
      firstCard = null;
      secondCard = null;
    }

    function endGame() {
      const container = document.getElementById('cardsContainer');
      container.innerHTML = '';
      document.getElementById('gameMessage').innerText = '';
      document.getElementById('memoryGame').style.display = 'none';

      // Show long birthday message after game
      const surprise = document.getElementById('surprise');
      surprise.style.display = 'block';
      surprise.querySelector('p').innerHTML = `
        Happy Birthday! 🎉<br>
        I hope this little game made you smile.<br>
        You are my sunshine, my happiness, and everything in between.<br>
        Can't wait to spend more magical moments with you! 💖
      `;
    }
  </script>
</body>
</html># app
