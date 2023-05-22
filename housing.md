# Stock Market Quiz

<html>
<head>
    <title>Stock Market Quiz</title>
    <style>
    body {
        font-family: Arial, sans-serif;
        background-color: #f4f4f4;
        margin: 0;
        padding: 0;
    }

    .quiz-container {
        width: 80%;
        margin: 0 auto;
        background-color: white;
        padding: 20px;
        border-radius: 5px;
        box-shadow: 0px 0px 10px 2px #ddd;
    }

    h1 {
        text-align: center;
    }

    button {
        display: block;
        margin: 20px auto;
        background-color: #4285f4;
        color: white;
        border: none;
        padding: 10px 20px;
        border-radius: 5px;
    }

    button:hover {
        background-color: #3073f3;
    }

    button:disabled {
        background-color: #bbb;
        cursor: not-allowed;
    }
</style>
</head>
<body>
    <div class="quiz-container" id="quiz">
        <button id="submit" onclick="submitQuiz()">Submit Quiz</button>
        <div id="result"></div>
    </div>

    <script>
    const quizData = [
        {
            question: "What does IPO stand for?",
            a: "Initial Public Offering",
            b: "Instant Price Option",
            c: "Investment Potential Output",
            d: "Intelligent Profit Operation",
            correct: "a"
        },
        {
            question: "What does NASDAQ stand for?",
            a: "National Association of Securities Dealers Automated Quotations",
            b: "National Assembly of Stock Dealers And Quantities",
            c: "National Association of Security Dealers And Quality",
            d: "None of the above",
            correct: "a"
        },
        // Add more questions as needed
    ];

    let currentQuiz = 0;
    let score = 0;

    const loadQuiz = () => {
        const currentQuizData = quizData[currentQuiz];

        const quizElement = document.getElementById("quiz");
        quizElement.innerHTML = `
            <h2>${currentQuizData.question}</h2>
            ${["a", "b", "c", "d"].map(letter => `
                <label>
                    <input type="radio" name="answer" value="${letter}">
                    ${letter}: ${currentQuizData[letter]}
                </label>
            `).join('')}
        `;
    }

    const submitQuiz = () => {
        const answerElements = document.getElementsByName("answer");
        let answer = undefined;

        answerElements.forEach((element) => {
            if (element.checked) {
                answer = element.value;
            }
        });

        if (answer) {
            if (answer === quizData[currentQuiz].correct) {
                score++;
            }
            currentQuiz++;

            if (currentQuiz < quizData.length) {
                loadQuiz();
            } else {
                document.getElementById("result").innerHTML = `
                    <h2>You completed the quiz. Your score is ${score} out of ${quizData.length}.</h2>
                `;
                document.getElementById("submit").disabled = true;
            }
        } else {
            alert("Please select an option before moving on.");
        }
    }

    loadQuiz();
</script>
</body>
</html>