<html>
<head>
<style>
body {
    font-family: Arial, sans-serif;
}
#quiz {
    margin-bottom: 30px;
}
#quiz h1 {
    font-weight: bold;
    margin-bottom: 20px;
}
#quiz h2 {
    font-size: 1.2em;
}
.question {
    margin: 20px 0;
}
.answers {
    margin-bottom: 20px;
}
#submit, #restart {
    font-size: 1.2em;
    padding: 10px 20px;
    margin: 5px;
}
#results {
    font-weight: bold;
    background: #030000;
    padding: 20px;
    margin-top: 10px;
    border: 1px solid #ddd;
    border-radius: 10px;
}
</style>
</head>
<body>
<div id="quiz">
    <h1>Stock Game Quiz</h1>
    <h2 id="question"></h2>
    <div class="answers">
        <input type="radio" name="answer" id="a" value="a">
        <label for="a" id="a_text"></label><br>
        <input type="radio" name="answer" id="b" value="b">
        <label for="b" id="b_text"></label><br>
        <input type="radio" name="answer" id="c" value="c">
        <label for="c" id="c_text"></label><br>
    </div>
    <button id="submit">Next</button>
</div>
<div id="results"></div>
<button id="restart" style="display:none;">Restart</button>
<script>
var currentQuestion = 0;
var score = 0;
var questions = [
    {
        question: 'What does IPO stand for?',
        answers: {
            a: 'Initial Public Offering',
            b: 'Internal Price Options',
            c: 'Intelligent Personal Object'
        },
        correctAnswer: 'a'
    },
    {
        question: 'What is a bull market?',
        answers: {
            a: 'A market in decline',
            b: 'A market on the rise',
            c: 'A market with a lot of trading activity'
        },
        correctAnswer: 'b'
    },
    {
        question: 'What is a bear market?',
        answers: {
            a: 'A market on the rise',
            b: 'A market in decline',
            c: 'A market with a lot of trading activity'
        },
        correctAnswer: 'b'
    },
    {
        question: 'What is a dividend?',
        answers: {
            a: 'A debt instrument',
            b: 'A share in a company\'s profits',
            c: 'A financial derivative'
        },
        correctAnswer: 'b'
    },
    {
        question: 'What is the role of a stockbroker?',
        answers: {
            a: 'To oversee corporate mergers',
            b: 'To trade stocks on behalf of clients',
            c: 'To regulate the stock market'
        },
        correctAnswer: 'b'
    },
    {
        question: 'What is market capitalization?',
        answers: {
            a: 'The total value of a company\'s outstanding shares',
            b: 'The cost of buying shares in the market',
            c: 'The profit margin of a company'
        },
        correctAnswer: 'a'
    }
];
function shuffleArray(array) {
    for (var i = array.length - 1; i > 0; i--) {
        var j = Math.floor(Math.random() * (i + 1));
        var temp = array[i];
        array[i] = array[j];
        array[j] = temp;
    }
    return array;
}
function selectRandomQuestions() {
    var randomizedQuestions = shuffleArray(questions);
    return randomizedQuestions.slice(0, 3);
}
var selectedQuestions = selectRandomQuestions();
function showQuestion() {
    var q = selectedQuestions[currentQuestion];
    document.getElementById('question').textContent = q.question;
    document.getElementById('a_text').textContent = q.answers.a;
    document.getElementById('b_text').textContent = q.answers.b;
    document.getElementById('c_text').textContent = q.answers.c;
}
function checkAnswer() {
    var selectedAnswer = document.querySelector('input[name="answer"]:checked').value;
    if (selectedAnswer === selectedQuestions[currentQuestion].correctAnswer) {
        score++;
    }
    currentQuestion++;
    if (currentQuestion >= selectedQuestions.length) {
        showResults();
    } else {
        showQuestion();
    }
}
function showResults() {
    document.getElementById('quiz').style.display = 'none';
    document.getElementById('results').style.display = 'block';
    document.getElementById('results').textContent = 'You scored ' + score + ' out of ' + selectedQuestions.length + '.';
    document.getElementById('restart').style.display = 'block';
}
function restartQuiz() {
    score = 0;
    currentQuestion = 0;
    selectedQuestions = selectRandomQuestions();
    document.getElementById('quiz').style.display = 'block';
    document.getElementById('results').style.display = 'none';
    document.getElementById('restart').style.display = 'none';
    showQuestion();
}
document.getElementById('submit').addEventListener('click', checkAnswer);
document.getElementById('restart').addEventListener('click', restartQuiz);
showQuestion();
</script>
</body>
</html>