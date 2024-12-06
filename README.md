<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MCQ App with Consecutive Feedback</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f4;
        }
        .container {
            width: 60%;
            margin: 50px auto;
            padding: 20px;
            background-color: #fff;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        h1 {
            text-align: center;
            color: #333;
        }
        .question {
            font-size: 18px;
            margin-bottom: 20px;
        }
        .choices {
            list-style-type: none;
            padding: 0;
        }
        .choices li {
            margin: 10px 0;
        }
        button {
            padding: 10px 20px;
            background-color: #4CAF50;
            color: white;
            border: none;
            cursor: pointer;
            font-size: 16px;
            border-radius: 5px;
        }
        button:hover {
            background-color: #45a049;
        }
        .feedback {
            font-size: 20px;
            font-weight: bold;
            text-align: center;
            margin-top: 20px;
        }
        .correct {
            color: green;
        }
        .incorrect {
            color: red;
        }
        .result {
            text-align: center;
            font-size: 20px;
            margin-top: 20px;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>MCQ Quiz</h1>
    <div id="quiz-container">
        <p class="question" id="question"></p>
        <ul class="choices" id="choices"></ul>
        <button onclick="submitAnswer()">Submit Answer</button>
        <div class="feedback" id="feedback"></div>
        <div class="result" id="result"></div>
    </div>
</div>

<script>
    const questions = [
        {
            question: "What is the capital of France?",
            choices: ["Berlin", "Madrid", "Paris", "Rome"],
            answer: "Paris"
        },
        {
            question: "Which programming language is used for web development?",
            choices: ["Python", "Java", "JavaScript", "C++"],
            answer: "JavaScript"
        },
        {
            question: "What is 2 + 2?",
            choices: ["3", "4", "5", "6"],
            answer: "4"
        }
    ];

    let currentQuestionIndex = 0;
    let score = 0;
    let correctStreak = 0; // Tracks consecutive correct answers
    let incorrectStreak = 0; // Tracks consecutive incorrect answers

    function loadQuestion() {
        const question = questions[currentQuestionIndex];
        document.getElementById("question").textContent = question.question;
        const choicesList = document.getElementById("choices");
        choicesList.innerHTML = "";
        document.getElementById("feedback").textContent = "";

        question.choices.forEach(choice => {
            const li = document.createElement("li");
            li.innerHTML = `<input type="radio" name="choice" value="${choice}" /> ${choice}`;
            choicesList.appendChild(li);
        });
    }

    function submitAnswer() {
        const selectedChoice = document.querySelector('input[name="choice"]:checked');
        if (!selectedChoice) {
            alert("Please select an answer before submitting.");
            return;
        }

        const selectedAnswer = selectedChoice.value;
        const feedback = document.getElementById("feedback");

        if (selectedAnswer === questions[currentQuestionIndex].answer) {
            correctStreak++;
            incorrectStreak = 0; // Reset incorrect streak
            score++;
            if (correctStreak === 1) {
                feedback.textContent = "+1p, Good";
            } else if (correctStreak === 2) {
                feedback.textContent = "+1p, Very Good";
            } else if (correctStreak === 3) {
                feedback.textContent = "+1p, MashAllah";
            } else if (correctStreak === 4) {
                feedback.textContent = "+1p, Allahu Akbar";
                correctStreak = 0; // Reset streak after 4 correct
            }
            feedback.className = "feedback correct";
        } else {
            incorrectStreak++;
            correctStreak = 0; // Reset correct streak
            score--;
            if (incorrectStreak === 1) {
                feedback.textContent = "-1p, Wrong";
            } else if (incorrectStreak === 2) {
                feedback.textContent = "-1p, InshAllah Next Time";
            } else if (incorrectStreak === 3) {
                feedback.textContent = "-1p, Astaghfurullah Try Again";
                incorrectStreak = 0; // Reset streak after 3 incorrect
               }
            feedback.className = "feedback incorrect";
        }

        currentQuestionIndex++;
        if (currentQuestionIndex < questions.length) {
            setTimeout(loadQuestion, 2000); // Wait 2 seconds before loading the next question
        } else {
            setTimeout(showResult, 2000); // Wait 2 seconds before showing the result
        }
    }

    function showResult() {
        document.getElementById("quiz-container").style.display = "none";
        const result = document.getElementById("result");
        result.textContent = `Quiz finished! Your final score is ${score}.`;
    }

    loadQuestion();
</script>

</body>
</html>
