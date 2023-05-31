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
    background: #f7f2f2;
    padding: 20px;
    margin-top: 10px;
    border: 1px solid #ddd;
    border-radius: 10px;
}
#wrongAnswers {
    margin-top: 20px;
}
</style>
</head>
<body>
<div id="quiz">
    <h1>Stock Game Quiz</h1>
    <img id="questionImage" src="" alt="question image" style="display: none;">
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
<div id="wrongAnswers"></div>
<button id="restart" style="display:none;">Restart</button>
<script>
var currentQuestion = 0;
var score = 0;
var wrongAnswers = [];
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
   },
   {
       question: 'What does NASDAQ stand for?',
       answers: {
           a: 'National Association of Securities Dealers Automated Quotations',
           b: 'New York Stock Dealers Association Quotations',
           c: 'National Association of Stock Dealers Automatic Questions'
       },
       correctAnswer: 'a'
   },
   {
       question: 'What is a Blue Chip stock?',
       answers: {
           a: 'A stock from a large, nationally recognized and well-established company',
           b: 'A stock from a small, innovative and new company',
           c: 'A stock that gives high dividends'
       },
       correctAnswer: 'a'
   },
   {
       question: 'What is a short selling?',
       answers: {
           a: 'Selling a stock you own and hope to buy back at a lower price',
           b: 'Selling a stock you do not own and hope to buy back at a lower price',
           c: 'Selling a stock just after buying it'
       },
       correctAnswer: 'b'
   },
   {
       question: 'What is a mutual fund?',
       answers: {
           a: 'A fund that ensures mutual benefits for all stockholders',
           b: 'A collection of stocks, bonds, or other securities owned by a group of investors and managed by a professional investment company',
           c: 'A fund that invests in real estate only'
       },
       correctAnswer: 'b'
   },
   {
       question: 'What is a hedge fund?',
       answers: {
           a: 'A fund that invests in agricultural products',
           b: 'A fund that only invests in safe and risk-free assets',
           c: 'A private investment structure that employs complex strategies to generate high returns, often more risky'
       },
       correctAnswer: 'c'
   },
   {
       question: 'What does ETF stand for?',
       answers: {
           a: 'Exchange-Traded Fund',
           b: 'Equity Transfer Facility',
           c: 'Exchange Transaction Fee'
       },
       correctAnswer: 'a'
   },
   {
   question: 'What is a stock split?',
   answers: {
       a: 'A strategy used by companies to decrease the price of individual shares by increasing the number of shares',
       b: 'A strategy used by companies to increase the price of individual shares by decreasing the number of shares',
       c: 'A situation where a company divides its assets equally among shareholders'
   },
   correctAnswer: 'a'
   },
   {
       question: 'Here is a graph of a stock. What does the pattern represent?',
       answers: {
           a: 'Bullish trend',
           b: 'Bearish trend',
           c: 'Consolidation phase'
       },
       imageURL: 'https://a.c-dn.net/c/content/dam/publicsites/igcom/uk/images/ContentImage/Cup%20and%20handle.png',
       correctAnswer: 'a'
   },
   {
       question: 'What type of candlestick pattern is this?',
       answers: {
           a: 'Hammer',
           b: 'Shooting Star',
           c: 'Doji'
       },
       imageURL: 'https://www.adigitalblogger.com/wp-content/uploads/shooting-star.png',
       correctAnswer: 'b'
   },
   {
       question: 'What does this type of volume pattern suggest?',
       answers: {
           a: 'Strong buying interest',
           b: 'Strong selling pressure',
           c: 'Low trading activity'
       },
       imageURL: 'https://school.stockcharts.com/lib/exe/fetch.php?media=chart_analysis:candlestick_bearish_reversal_patterns:bearrev1-nke-bearengresist.png',
       correctAnswer: 'b'
   },
       {
       question: 'What is this chart pattern called?',
       answers: {
           a: 'Head and Shoulders',
           b: 'Double Top',
           c: 'Cup and Handle'
       },
       imageURL: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQBIT6kGYcWe6G9QB6j7dPEAPLxVGFHk7tidA&usqp=CAU',
       correctAnswer: 'a'
   },
   {
       question: 'What is this candlestick pattern called?',
       answers: {
           a: 'Bullish Engulfing',
           b: 'Doji',
           c: 'Bearish Engulfing'
       },
       imageURL: 'https://a.c-dn.net/c/content/dam/publicsites/igcom/uk/images/ContentImage/doji%20patterns@2x%20(002).png',
       correctAnswer: 'b'
   },
   {
       question: 'What does this volume pattern suggest?',
       answers: {
           a: 'Market Consolidation',
           b: 'Buying Pressure',
           c: 'Selling Pressure'
       },
       imageURL: 'https://www.marketvolume.com/vsa/img/upthrust-bar.png',
       correctAnswer: 'c'
   },
   {
       question: 'What is this chart pattern called?',
       answers: {
           a: 'Falling Wedge',
           b: 'Rising Wedge',
           c: 'Bull Flag'
       },
       imageURL: 'https://bpcdn.co/images/2016/05/grade7-falling-wedge-reversal-before.png',
       correctAnswer: 'a'
   },
   {
       question: 'What does this type of candlestick pattern suggest?',
       answers: {
           a: 'Bullish Reversal',
           b: 'Bearish Reversal',
           c: 'Market Consolidation'
       },
       imageURL: 'https://www.incrediblecharts.com/images/png_images/sna_stock_screen_consolidation.png',
       correctAnswer: 'c'
   },
   {
       question: 'What is this chart pattern called?',
       answers: {
           a: 'Bull Flag',
           b: 'Bear Flag',
           c: 'Pennant'
       },
       imageURL: 'https://media.warriortrading.com/2020/07/28111357/shutterstock_1407751412.jpg',
       correctAnswer: 'c'
   },
   {
       question: 'What does this volume pattern suggest?',
       answers: {
           a: 'Buying Pressure',
           b: 'Selling Pressure',
           c: 'Market Consolidation'
       },
       imageURL: 'https://qph.cf2.quoracdn.net/main-qimg-a823fdcc41eed80a908f407dfb04ebf0-pjlq',
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
   return randomizedQuestions.slice(0, 10);
}
var selectedQuestions = selectRandomQuestions();
function showQuestion() {
   var q = selectedQuestions[currentQuestion];
   document.getElementById('question').textContent = q.question;
   document.getElementById('a_text').textContent = q.answers.a;
   document.getElementById('b_text').textContent = q.answers.b;
   document.getElementById('c_text').textContent = q.answers.c;
   var questionImage = document.getElementById('questionImage');
   if (q.imageURL) {
       questionImage.src = q.imageURL;
       questionImage.style.display = '';
   } else {
       questionImage.style.display = 'none';
   }
    var inputs = document.getElementsByName('answer');
    for(var i = 0; i < inputs.length; i++) {
        inputs[i].checked = false;
    }
}
function checkAnswer() {
    var selectedAnswer = document.querySelector('input[name="answer"]:checked').value;
    if (selectedAnswer === selectedQuestions[currentQuestion].correctAnswer) {
        score++;
    } else {
        wrongAnswers.push({
            question: selectedQuestions[currentQuestion].question,
            correctAnswer: selectedQuestions[currentQuestion].answers[selectedQuestions[currentQuestion].correctAnswer]
        });
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
    if (wrongAnswers.length > 0) {
        var wrongAnswersDiv = document.getElementById('wrongAnswers');
        wrongAnswersDiv.innerHTML = '<h2>Incorrect Answers:</h2>';
        for (var i = 0; i < wrongAnswers.length; i++) {
            wrongAnswersDiv.innerHTML += '<h3>' + wrongAnswers[i].question + '</h3>';
            wrongAnswersDiv.innerHTML += '<p>Correct answer: ' + wrongAnswers[i].correctAnswer + '</p>';
        }
    }
}
function restartQuiz() {
    currentQuestion = 0;
    score = 0;
    wrongAnswers = [];
    selectedQuestions = selectRandomQuestions();
    document.getElementById('quiz').style.display = 'block';
    document.getElementById('results').style.display = 'none';
    document.getElementById('restart').style.display = 'none';
    document.getElementById('wrongAnswers').innerHTML = '';
    showQuestion();
}
document.getElementById('submit').addEventListener('click', checkAnswer);
document.getElementById('restart').addEventListener('click', restartQuiz);
showQuestion();
</script>
</body>
</html>