<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>🌈 Kids Quiz Adventure</title>

<style>
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

body {
    font-family: "Comic Sans MS", "Trebuchet MS", sans-serif;
    background: linear-gradient(135deg, #ffe259, #ffa751);
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
}

.container {
    width: 100%;
    max-width: 650px;
}

.card {
    background: white;
    border-radius: 30px;
    padding: 30px;
    box-shadow: 0 12px 30px rgba(0,0,0,0.2);
    text-align: center;
}

h1 {
    color: #ff5c8a;
    font-size: 38px;
    margin-bottom: 10px;
}

h2 {
    color: #5c63d8;
    margin-bottom: 20px;
}

.subtitle {
    color: #666;
    font-size: 18px;
    margin-bottom: 25px;
}

input {
    width: 100%;
    padding: 15px;
    border: 3px solid #ffd166;
    border-radius: 15px;
    font-size: 18px;
    margin-bottom: 20px;
    text-align: center;
}

.categories {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 12px;
    margin-bottom: 20px;
}

.category {
    padding: 18px;
    border: none;
    border-radius: 18px;
    font-size: 18px;
    font-weight: bold;
    cursor: pointer;
    background: #f1f3f8;
    transition: 0.2s;
}

.category:hover {
    transform: scale(1.04);
}

.category.selected {
    background: #7bdff2;
    color: white;
    transform: scale(1.04);
}

.start-btn,
.next-btn,
.restart-btn {
    border: none;
    padding: 15px 35px;
    border-radius: 20px;
    background: #ff6b6b;
    color: white;
    font-size: 20px;
    font-weight: bold;
    cursor: pointer;
    transition: 0.2s;
}

.start-btn:hover,
.next-btn:hover,
.restart-btn:hover {
    transform: scale(1.05);
}

.quiz-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 15px;
    font-size: 18px;
    font-weight: bold;
}

.timer {
    background: #ff7675;
    color: white;
    padding: 8px 15px;
    border-radius: 15px;
}

.progress-container {
    width: 100%;
    height: 14px;
    background: #eee;
    border-radius: 10px;
    margin-bottom: 25px;
    overflow: hidden;
}

.progress {
    height: 100%;
    width: 0%;
    background: #00b894;
    transition: 0.3s;
}

.question {
    font-size: 28px;
    color: #333;
    margin: 25px 0;
    min-height: 70px;
}

.options {
    display: grid;
    gap: 12px;
}

.option {
    padding: 16px;
    border: 3px solid #eee;
    border-radius: 18px;
    background: #fff;
    font-size: 20px;
    cursor: pointer;
    font-family: inherit;
    transition: 0.2s;
}

.option:hover {
    background: #f7f7ff;
    transform: translateY(-2px);
}

.option.correct {
    background: #55efc4;
    border-color: #00b894;
    color: #075e54;
}

.option.wrong {
    background: #ff7675;
    border-color: #d63031;
    color: white;
}

.option:disabled {
    cursor: not-allowed;
}

.feedback {
    min-height: 40px;
    margin: 15px 0;
    font-size: 21px;
    font-weight: bold;
}

.next-btn {
    display: none;
    background: #6c5ce7;
}

.result-emoji {
    font-size: 70px;
    margin: 15px;
}

.result-score {
    font-size: 32px;
    color: #ff6b6b;
    margin: 15px;
}

.stars {
    font-size: 45px;
    margin: 15px;
}

.message {
    font-size: 22px;
    color: #555;
    margin-bottom: 25px;
}

.high-score {
    margin-top: 20px;
    color: #6c5ce7;
    font-size: 18px;
}

.hidden {
    display: none;
}

@media (max-width: 500px) {
    .card {
        padding: 20px;
    }

    h1 {
        font-size: 30px;
    }

    .question {
        font-size: 23px;
    }

    .option {
        font-size: 18px;
    }
}
</style>
</head>

<body>

<div class="container">

<!-- START SCREEN -->
<div class="card" id="startScreen">

    <h1>🌈 Kids Quiz Adventure 🎈</h1>

    <p class="subtitle">
        Learn • Play • Have Fun! 🥳
    </p>

    <input
        type="text"
        id="playerName"
        placeholder="Enter your name 😊"
    >

    <h2>Choose a Subject 📚</h2>

    <div class="categories">

        <button class="category selected" data-category="All">
            🌟 All
        </button>

        <button class="category" data-category="Maths">
            🔢 Maths
        </button>

        <button class="category" data-category="English">
            🔤 English
        </button>

        <button class="category" data-category="GK">
            🌍 GK
        </button>

    </div>

    <button class="start-btn" onclick="startQuiz()">
        🚀 Start Quiz
    </button>

    <p class="high-score">
        🏆 High Score:
        <span id="highScore">0</span>
    </p>

</div>


<!-- QUIZ SCREEN -->
<div class="card hidden" id="quizScreen">

    <div class="quiz-header">

        <span id="questionNumber">
            Question 1/30
        </span>

        <span class="timer">
            ⏰ <span id="time">20</span>
        </span>

    </div>

    <div class="progress-container">
        <div class="progress" id="progress"></div>
    </div>

    <div class="question" id="question">
        Question goes here
    </div>

    <div class="options" id="options"></div>

    <div class="feedback" id="feedback"></div>

    <button class="next-btn" id="nextBtn" onclick="nextQuestion()">
        Next ➡️
    </button>

</div>


<!-- RESULT SCREEN -->
<div class="card hidden" id="resultScreen">

    <h1>🎉 Quiz Complete! 🎉</h1>

    <div class="result-emoji" id="resultEmoji">
        🏆
    </div>

    <h2 id="resultName">
        Great Job!
    </h2>

    <div class="result-score">
        Score: <span id="finalScore">0</span>
    </div>

    <div class="stars" id="stars">
        ⭐⭐⭐
    </div>

    <p class="message" id="resultMessage">
        Amazing!
    </p>

    <button class="restart-btn" onclick="restartQuiz()">
        🔄 Play Again
    </button>

</div>

</div>


<script>

const questions = [

/* ================= MATHS ================= */

{
    category: "Maths",
    question: "What comes after 5? 🔢",
    options: ["4", "6", "7", "3"],
    answer: "6"
},

{
    category: "Maths",
    question: "What is 2 + 3? ➕",
    options: ["4", "5", "6", "7"],
    answer: "5"
},

{
    category: "Maths",
    question: "What is 10 - 4? ➖",
    options: ["5", "6", "7", "8"],
    answer: "6"
},

{
    category: "Maths",
    question: "How many fingers are on one hand? ✋",
    options: ["4", "5", "6", "10"],
    answer: "5"
},

{
    category: "Maths",
    question: "What number comes before 9?",
    options: ["7", "8", "10", "6"],
    answer: "8"
},

{
    category: "Maths",
    question: "What is 1 + 1?",
    options: ["1", "2", "3", "4"],
    answer: "2"
},

{
    category: "Maths",
    question: "Which number is bigger?",
    options: ["3", "8", "2", "1"],
    answer: "8"
},

{
    category: "Maths",
    question: "How many sides does a triangle have? 🔺",
    options: ["2", "3", "4", "5"],
    answer: "3"
},

{
    category: "Maths",
    question: "What is 5 + 5?",
    options: ["8", "9", "10", "11"],
    answer: "10"
},

{
    category: "Maths",
    question: "How many wheels does a bicycle have? 🚲",
    options: ["1", "2", "3", "4"],
    answer: "2"
},


/* ================= ENGLISH ================= */

{
    category: "English",
    question: "Which is a vowel? 🔤",
    options: ["B", "C", "A", "D"],
    answer: "A"
},

{
    category: "English",
    question: "What is the opposite of BIG?",
    options: ["Tall", "Small", "Long", "Fast"],
    answer: "Small"
},

{
    category: "English",
    question: "Which word is a color? 🎨",
    options: ["Apple", "Blue", "Dog", "Run"],
    answer: "Blue"
},

{
    category: "English",
    question: "Complete: C _ T 🐱",
    options: ["A", "B", "E", "O"],
    answer: "A"
},

{
    category: "English",
    question: "Which one is an animal?",
    options: ["Table", "Dog", "Book", "Chair"],
    answer: "Dog"
},

{
    category: "English",
    question: "What is the plural of CAT?",
    options: ["Cats", "Cat", "Cates", "Cat's"],
    answer: "Cats"
},

{
    category: "English",
    question: "Which word starts with B?",
    options: ["Apple", "Ball", "Cat", "Dog"],
    answer: "Ball"
},

{
    category: "English",
    question: "Which one is a fruit? 🍎",
    options: ["Apple", "Car", "House", "Pen"],
    answer: "Apple"
},

{
    category: "English",
    question: "What is the opposite of HOT?",
    options: ["Warm", "Cold", "Big", "Fast"],
    answer: "Cold"
},

{
    category: "English",
    question: "Which word rhymes with CAT?",
    options: ["Dog", "Hat", "Sun", "Pen"],
    answer: "Hat"
},


/* ================= GK ================= */

{
    category: "GK",
    question: "Which animal says MEOW? 🐱",
    options: ["Dog", "Cat", "Cow", "Lion"],
    answer: "Cat"
},

{
    category: "GK",
    question: "Which animal gives us milk? 🐄",
    options: ["Cow", "Tiger", "Dog", "Horse"],
    answer: "Cow"
},

{
    category: "GK",
    question: "How many days are in a week? 📅",
    options: ["5", "6", "7", "8"],
    answer: "7"
},

{
    category: "GK",
    question: "Which planet do we live on? 🌍",
    options: ["Mars", "Earth", "Jupiter", "Moon"],
    answer: "Earth"
},

{
    category: "GK",
    question: "What color is the sun usually shown as? ☀️",
    options: ["Blue", "Green", "Yellow", "Black"],
    answer: "Yellow"
},

{
    category: "GK",
    question: "Which bird can say 'parrot'? 🦜",
    options: ["Parrot", "Cow", "Dog", "Fish"],
    answer: "Parrot"
},

{
    category: "GK",
    question: "Which one lives in water? 🐟",
    options: ["Fish", "Dog", "Cat", "Horse"],
    answer: "Fish"
},

{
    category: "GK",
    question: "How many eyes do most people have? 👀",
    options: ["1", "2", "3", "4"],
    answer: "2"
},

{
    category: "GK",
    question: "Which season is usually very cold? ❄️",
    options: ["Summer", "Winter", "Spring", "Autumn"],
    answer: "Winter"
},

{
    category: "GK",
    question: "Which fruit is yellow and curved? 🍌",
    options: ["Apple", "Banana", "Orange", "Grapes"],
    answer: "Banana"
}

];


let selectedCategory = "All";
let quizQuestions = [];
let currentQuestion = 0;
let score = 0;
let playerName = "";
let timer;
let timeLeft = 20;


/* CATEGORY SELECTION */

document.querySelectorAll(".category").forEach(button => {

    button.addEventListener("click", () => {

        document
        .querySelectorAll(".category")
        .forEach(btn => btn.classList.remove("selected"));

        button.classList.add("selected");

        selectedCategory =
            button.getAttribute("data-category");

    });

});


/* START QUIZ */

function startQuiz() {

    playerName =
        document.getElementById("playerName").value.trim();

    if (playerName === "") {
        playerName = "Little Star";
    }

    if (selectedCategory === "All") {

        quizQuestions = [...questions];

    } else {

        quizQuestions =
            questions.filter(q =>
                q.category === selectedCategory
            );

    }

    shuffleArray(quizQuestions);

    currentQuestion = 0;
    score = 0;

    document
    .getElementById("startScreen")
    .classList.add("hidden");

    document
    .getElementById("resultScreen")
    .classList.add("hidden");

    document
    .getElementById("quizScreen")
    .classList.remove("hidden");

    showQuestion();

}


/* SHOW QUESTION */

function showQuestion() {

    clearInterval(timer);

    timeLeft = 20;

    document.getElementById("time").textContent =
        timeLeft;

    timer = setInterval(() => {

        timeLeft--;

        document.getElementById("time").textContent =
            timeLeft;

        if (timeLeft <= 0) {

            clearInterval(timer);

            timeUp();

        }

    }, 1000);


    const q = quizQuestions[currentQuestion];

    document.getElementById("questionNumber").textContent =
        `Question ${currentQuestion + 1}/${quizQuestions.length}`;

    document.getElementById("question").textContent =
        q.question;

    document.getElementById("feedback").textContent = "";

    document.getElementById("nextBtn").style.display =
        "none";


    const progress =
        ((currentQuestion) / quizQuestions.length) * 100;

    document.getElementById("progress").style.width =
        progress + "%";


    const optionsContainer =
        document.getElementById("options");

    optionsContainer.innerHTML = "";


    let shuffledOptions = [...q.options];

    shuffleArray(shuffledOptions);


    shuffledOptions.forEach(option => {

        const button =
            document.createElement("button");

        button.className = "option";

        button.textContent = option;

        button.onclick = () =>
            checkAnswer(button, option);

        optionsContainer.appendChild(button);

    });

}


/* CHECK ANSWER */

function checkAnswer(button, selectedAnswer) {

    clearInterval(timer);

    const q = quizQuestions[currentQuestion];

    const allOptions =
        document.querySelectorAll(".option");

    allOptions.forEach(btn => {
        btn.disabled = true;
    });


    if (selectedAnswer === q.answer) {

        button.classList.add("correct");

        score++;

        document.getElementById("feedback").textContent =
            "🎉 Correct! Great job! ⭐";

        playSound(true);

    } else {

        button.classList.add("wrong");

        document.getElementById("feedback").textContent =
            `😊 Good try! Answer: ${q.answer}`;

        allOptions.forEach(btn => {

            if (btn.textContent === q.answer) {
                btn.classList.add("correct");
            }

        });

        playSound(false);

    }

    document.getElementById("nextBtn").style.display =
        "inline-block";

}


/* TIME UP */

function timeUp() {

    const q = quizQuestions[currentQuestion];

    const allOptions =
        document.querySelectorAll(".option");

    allOptions.forEach(btn => {

        btn.disabled = true;

        if (btn.textContent === q.answer) {
            btn.classList.add("correct");
        }

    });

    document.getElementById("feedback").textContent =
        `⏰ Time's up! Answer: ${q.answer}`;

    document.getElementById("nextBtn").style.display =
        "inline-block";

}


/* NEXT QUESTION */

function nextQuestion() {

    currentQuestion++;

    if (currentQuestion < quizQuestions.length) {

        showQuestion();

    } else {

        showResult();

    }

}


/* RESULT */

function showResult() {

    clearInterval(timer);

    document
    .getElementById("quizScreen")
    .classList.add("hidden");

    document
    .getElementById("resultScreen")
    .classList.remove("hidden");


    document.getElementById("resultName").textContent =
        `🌟 Well done, ${playerName}!`;


    document.getElementById("finalScore").textContent =
        `${score} / ${quizQuestions.length}`;


    let stars = "⭐";

    if (score >= quizQuestions.length * 0.8) {
        stars = "⭐⭐⭐⭐⭐";
    } else if (score >= quizQuestions.length * 0.6) {
        stars = "⭐⭐⭐⭐";
    } else if (score >= quizQuestions.length * 0.4) {
        stars = "⭐⭐⭐";
    } else if (score >= quizQuestions.length * 0.2) {
        stars = "⭐⭐";
    }

    document.getElementById("stars").textContent =
        stars;


    let message = "";

    if (score >= quizQuestions.length * 0.8) {

        message =
            "🏆 Amazing! You are a Quiz Superstar!";

        document.getElementById("resultEmoji").textContent =
            "🏆";

    } else if (score >= quizQuestions.length * 0.5) {

        message =
            "🎉 Great work! Keep learning and growing!";

        document.getElementById("resultEmoji").textContent =
            "🎉";

    } else {

        message =
            "💪 Good try! Practice makes you better!";

        document.getElementById("resultEmoji").textContent =
            "🌟";

    }

    document.getElementById("resultMessage").textContent =
        message;


    saveHighScore();

}


/* HIGH SCORE */

function saveHighScore() {

    const oldScore =
        Number(localStorage.getItem("kidsHighScore")) || 0;

    if (score > oldScore) {

        localStorage.setItem(
            "kidsHighScore",
            score
        );

    }

}


/* RESTART */

function restartQuiz() {

    document
    .getElementById("resultScreen")
    .classList.add("hidden");

    document
    .getElementById("startScreen")
    .classList.remove("hidden");

    document.getElementById("highScore").textContent =
        localStorage.getItem("kidsHighScore") || 0;

}


/* SHUFFLE */

function shuffleArray(array) {

    for (
        let i = array.length - 1;
        i > 0;
        i--
    ) {

        const j =
            Math.floor(Math.random() * (i + 1));

        [array[i], array[j]] =
            [array[j], array[i]];

    }

}


/* SOUND */

function playSound(correct) {

    try {

        const audioContext =
            new (
                window.AudioContext ||
                window.webkitAudioContext
            )();

        const oscillator =
            audioContext.createOscillator();

        const gain =
            audioContext.createGain();

        oscillator.connect(gain);

        gain.connect(audioContext.destination);

        oscillator.frequency.value =
            correct ? 700 : 250;

        oscillator.start();

        gain.gain.exponentialRampToValueAtTime(
            0.0001,
            audioContext.currentTime + 0.3
        );

        oscillator.stop(
            audioContext.currentTime + 0.3
        );

    } catch (error) {

        console.log("Sound unavailable");

    }

}


/* LOAD HIGH SCORE */

document.getElementById("highScore").textContent =
    localStorage.getItem("kidsHighScore") || 0;

</script>

</body>
</html>
