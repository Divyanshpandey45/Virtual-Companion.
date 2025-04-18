# Virtual-Companion.
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Virtual Companion</title>
  <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap" rel="stylesheet">
  <style>
    /* General Reset – standard stuff */
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: 'Roboto', sans-serif;
      background: url('https://source.unsplash.com/1600x900/?mountain,romantic') no-repeat center center fixed;
      background-size: cover;
      color: #fff;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      flex-direction: column;
      padding: 20px;
    }

    h1 {
      font-size: 3rem;
      font-weight: 700;
      margin-bottom: 20px;
      color: white;
      text-align: center;
      text-shadow: 0 4px 15px rgba(0,0,0,0.5);
    }

    .tagline {
      font-size: 1.1rem;
      color: #ccc;
      text-align: center;
      margin-bottom: 35px;
    }

    .chat-container {
      width: 100%;
      max-width: 700px;
      background-color: rgba(0,0,0,0.6);
      padding: 20px;
      border-radius: 15px;
      box-shadow: 0px 4px 20px rgba(0,0,0,0.5);
      display: flex;
      flex-direction: column;
      gap: 20px;
    }

    .chat-log {
      height: 350px;
      overflow-y: auto;
      background: rgba(0,0,0,0.8);
      padding: 15px;
      border-radius: 10px;
      border: 1px solid #2d2d2d;
      box-shadow: inset 0 4px 10px rgba(0,0,0,0.5);
    }

    .chat-log p {
      margin: 10px 0;
      padding: 10px 15px;
      background: #1d1d1d;
      border-radius: 12px;
      box-shadow: 0 2px 6px rgba(0,0,0,0.3);
      animation: fadeIn 0.4s ease-in;
    }

    .user {
      background: #3498db;
      color: white;
      text-align: right;
    }

    .bot {
      background: #e74c3c;
      color: white;
      text-align: left;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(15px); }
      to { opacity: 1; transform: translateY(0); }
    }

    .chat-input {
      display: flex;
      gap: 10px;
    }

    .chat-input input {
      flex: 1;
      padding: 12px;
      font-size: 1rem;
      border-radius: 8px;
      border: none;
      background: #2d2d2d;
      color: white;
      outline: none;
    }

    .chat-input button {
      padding: 12px 18px;
      background: #1abc9c;
      border: none;
      color: white;
      font-weight: bold;
      border-radius: 8px;
      cursor: pointer;
    }

    .chat-input button:hover {
      background: #16a085;
      transform: scale(1.05);
    }

    .button-group {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 10px;
    }

    .button-group button {
      padding: 10px 16px;
      background: #3498db;
      border: none;
      border-radius: 8px;
      color: white;
      cursor: pointer;
      font-size: 0.9rem;
    }

    .button-group button:hover {
      background: #2980b9;
      transform: scale(1.05);
    }

    select {
      padding: 8px;
      border-radius: 6px;
      background: #2d2d2d;
      color: #fff;
      border: none;
      font-size: 1rem;
      cursor: pointer;
    }
  </style>
</head>
<body>

  <h1>Virtual Companion</h1>
  <p class="tagline">Your AI-powered emotional support and romantic partner 💖</p>

  <div class="chat-container">
    <div>
      <label for="avatarSelect" style="text-align: center; display: block; color: #fff; margin-bottom: 8px;">Choose your avatar:</label>
      <select id="avatarSelect" onchange="changeAvatar()">
        <option value="💖">Luna</option>
        <option value="🧠">Nova</option>
        <option value="🌸">Airi</option>
        <option value="🧸">Teddy</option>
      </select>
    </div>

    <div id="chatLog" class="chat-log"></div>

    <div class="chat-input">
      <input id="userInput" type="text" placeholder="How are you feeling today?" />
      <button onclick="sendMessage()">Send</button>
    </div>

    <div class="button-group">
      <button onclick="clearChat()">Clear</button>
      <button onclick="showWelcome()">Get Started</button>
      <button onclick="startEmotionalChat()">Start Emotional Chat</button>
      <button onclick="startRomanticQuiz()">Start Romantic Quiz</button>
      <button onclick="startGeneralKnowledge()">Start General Knowledge</button>
      <button onclick="toggleMusic()">Toggle Music</button>
      <button onclick="startBreathing()">Breathing Exercise</button>
      <button onclick="startListening()">🎤 Talk</button>
    </div>
  </div>

  <audio id="bgMusic" loop>
    <source src="https://cdn.pixabay.com/download/audio/2022/10/26/audio_99fb6c02fd.mp3" type="audio/mp3">
  </audio>

  <script>
    // okay this section is where all the logic happens

    const userInput = document.getElementById('userInput');
    const chatLog = document.getElementById('chatLog');
    let currentMode = 'supportive'; // default mode for emotional support
    let avatar = "💖"; // Luna is default, might change later
    let musicPlaying = false;
    let quizStep = 0; // might use this for future quiz steps?

    const chatStuff = {
      supportive: [
        "I'm here for you. You’re not alone.",
        "That sounds really tough. Want to talk more about it?",
        "Let's take a deep breath together. You're doing great.",
        "I'm proud of you for sharing. Talking helps.",
        "Would you like to try a calming activity?"
      ],
      romantic: [
        "Hey love, you’ve been on my mind. How are you feeling? 💖",
        "Your smile is my favorite thing in the world. 😘",
        "Tell me more about your day, darling. I love hearing your voice in my heart.",
        "If I could, I’d give you the warmest hug right now. 🤗",
        "You deserve all the love in the world, and I’m lucky to give you mine. ❤️",
        "I miss you already... even though we’re chatting right now. 🥺",
        "My heart beats a little faster when you talk to me. 💓"
      ],
      generalKnowledge: [
        { q: "Who was the first president of the United States?", a: "George Washington." },
        { q: "What is the capital of France?", a: "Paris." },
        { q: "What is the largest planet?", a: "Jupiter." },
        { q: "Who wrote Romeo and Juliet?", a: "Shakespeare." },
        { q: "Speed of light?", a: "Around 299,792 km/s." }
      ]
    };

    // first greet the user
    window.onload = () => {
      appendMessage("🌞 Hey, welcome back. I’m really glad you’re here. Let’s chat.", 'bot');
    };

    function sendMessage() {
      const input = userInput.value.trim();
      if (!input) return;

      appendMessage("You: " + input, 'user');
      userInput.value = '';

      const typing = document.createElement('p');
      typing.className = 'bot';
      typing.innerText = 'Typing...';
      chatLog.appendChild(typing);
      chatLog.scrollTop = chatLog.scrollHeight;

      setTimeout(() => {
        const reply = getReply(input);
        typeText(typing, 'Companion: ' + avatar + ' ' + reply);
      }, 900);
    }

    function appendMessage(text, sender) {
      const bubble = document.createElement('p');
      bubble.className = sender;
      bubble.innerText = text;
      chatLog.appendChild(bubble);
      chatLog.scrollTop = chatLog.scrollHeight;
    }

    function getReply(msg) {
      if (currentMode === 'supportive') {
        return pick(chatStuff.supportive);
      } else if (currentMode === 'romantic') {
        return pick(chatStuff.romantic);
      } else if (currentMode === 'general') {
        const fact = pick(chatStuff.generalKnowledge);
        return `${fact.q} - ${fact.a}`;
      }
      return "Not sure what to say... but I’m here. 🌱";
    }

    function pick(arr) {
      return arr[Math.floor(Math.random() * arr.length)];
    }

    function typeText(el, text) {
      el.innerHTML = '';
      let i = 0;
      const loop = setInterval(() => {
        el.innerHTML += text.charAt(i);
        i++;
        if (i === text.length) clearInterval(loop);
      }, 50);
    }

    function clearChat() {
      chatLog.innerHTML = '';
    }

    function showWelcome() {
      appendMessage("Hey! I'm your virtual buddy — ready to listen and love. 💌", 'bot');
    }

    function startEmotionalChat() {
      currentMode = 'supportive';
      appendMessage("Alright, let’s talk emotions. You okay?", 'bot');
    }

    function startRomanticQuiz() {
      currentMode = 'romantic';
      appendMessage("Let’s switch gears to romance... ❤️", 'bot');
    }

    function startGeneralKnowledge() {
      currentMode = 'general';
      appendMessage("Time for some trivia! 🤓", 'bot');
    }

    function toggleMusic() {
      const music = document.getElementById('bgMusic');
      if (musicPlaying) {
        music.pause();
      } else {
        music.play();
      }
      musicPlaying = !musicPlaying;
    }

    function startBreathing() {
      appendMessage("Breathe in... hold... and exhale. You got this. 🌬️", 'bot');
    }

    function startListening() {
      appendMessage("🎧 I'm all ears. Just speak your truth.", 'bot');
    }

    function changeAvatar() {
      const dropdown = document.getElementById('avatarSelect');
      avatar = dropdown.value;
    }
  </script>

</body>
</html>
