# B11202021
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>General Knowledge Quiz</title>
  <style>
    body {
      margin: 0;
      min-height: 100vh;
      display: grid;
      place-items: center;
      padding: 24px;
      box-sizing: border-box;
      background: #f1f5f9;
      color: #172033;
      font-family: system-ui, sans-serif;
    }

    main {
      width: min(100%, 560px);
      padding: 32px;
      border-radius: 16px;
      background: white;
      box-shadow: 0 12px 36px #1720331a;
    }

    h1 { margin-top: 0; }
    #progress { color: #64748b; }
    #answers { display: grid; gap: 10px; margin: 24px 0; }

    button {
      padding: 12px 16px;
      border: 1px solid #cbd5e1;
      border-radius: 8px;
      background: white;
      color: inherit;
      font: inherit;
      text-align: left;
      cursor: pointer;
    }

    button:hover:not(:disabled) { background: #eff6ff; }
    button:focus-visible { outline: 3px solid #60a5fa; }
    button:disabled { cursor: default; }
    button.correct { border-color: #16a34a; background: #dcfce7; }
    button.incorrect { border-color: #dc2626; background: #fee2e2; }

    #feedback { min-height: 24px; font-weight: 600; }
    #next, #restart {
      border: 0;
      background: #2563eb;
      color: white;
      text-align: center;
    }
    #next:hover, #restart:hover { background: #1d4ed8; }
    [hidden] { display: none !important; }
  </style>
</head>
<body>
  <main>
    <h1>General Knowledge Quiz</h1>
    <p id="progress" aria-live="polite"></p>
    <section id="quiz" aria-labelledby="question"></section>
    <p id="feedback" aria-live="polite"></p>
    <button id="next" type="button" hidden>Next question</button>
    <section id="result" hidden>
      <h2>Quiz complete!</h2>
      <p id="score"></p>
      <button id="restart" type="button">Play again</button>
    </section>
  </main>

  <script>
    const questions = [
      {
        question: "Which planet is known as the Red Planet?",
        answers: ["Venus", "Mars", "Jupiter", "Mercury"],
        correct: 1
      },
      {
        question: "What is the largest ocean on Earth?",
        answers: ["Atlantic Ocean", "Indian Ocean", "Arctic Ocean", "Pacific Ocean"],
        correct: 3
      },
      {
        question: "How many sides does a hexagon have?",
        answers: ["Five", "Six", "Seven", "Eight"],
        correct: 1
      },
      {
        question: "Which animal is the tallest in the world?",
        answers: ["Elephant", "Ostrich", "Giraffe", "Camel"],
        correct: 2
      },
      {
        question: "What is the chemical symbol for water?",
        answers: ["CO₂", "O₂", "H₂O", "NaCl"],
        correct: 2
      },
      {
        question: "What is the capital of Japan?",
        answers: ["Seoul", "Tokyo", "Beijing", "Bangkok"],
        correct: 1
      },
      {
        question: "How many continents are there?",
        answers: ["Five", "Six", "Seven", "Eight"],
        correct: 2
      },
      {
        question: "Which planet is closest to the Sun?",
        answers: ["Venus", "Earth", "Mercury", "Mars"],
        correct: 2
      },
      {
        question: "What is the largest mammal?",
        answers: ["African elephant", "Blue whale", "Giraffe", "Hippopotamus"],
        correct: 1
      },
      {
        question: "Which language is primarily spoken in Brazil?",
        answers: ["Spanish", "Portuguese", "French", "Italian"],
        correct: 1
      },
      {
        question: "How many minutes are in one hour?",
        answers: ["30", "45", "60", "90"],
        correct: 2
      },
      {
        question: "What is the freezing point of water in Celsius?",
        answers: ["0°C", "10°C", "32°C", "100°C"],
        correct: 0
      },
      {
        question: "Which instrument has black and white keys?",
        answers: ["Guitar", "Piano", "Violin", "Drum"],
        correct: 1
      },
      {
        question: "What is the main ingredient in guacamole?",
        answers: ["Cucumber", "Avocado", "Broccoli", "Spinach"],
        correct: 1
      },
      {
        question: "Which gas do plants absorb from the atmosphere?",
        answers: ["Oxygen", "Carbon dioxide", "Nitrogen", "Helium"],
        correct: 1
      }
    ];

    const progress = document.querySelector("#progress");
    const quiz = document.querySelector("#quiz");
    const feedback = document.querySelector("#feedback");
    const nextButton = document.querySelector("#next");
    const result = document.querySelector("#result");
    const scoreText = document.querySelector("#score");
    const restartButton = document.querySelector("#restart");

    let current = 0;
    let score = 0;

    function showQuestion() {
      const item = questions[current];
      progress.textContent = `Question ${current + 1} of ${questions.length}`;
      quiz.innerHTML = `<h2 id="question">${item.question}</h2>`;

      const answers = document.createElement("div");
      answers.id = "answers";

      item.answers.forEach((answer, index) => {
        const button = document.createElement("button");
        button.type = "button";
        button.textContent = answer;
        button.addEventListener("click", () => chooseAnswer(index, answers));
        answers.append(button);
      });

      quiz.append(answers);
      feedback.textContent = "";
      nextButton.hidden = true;
    }

    function chooseAnswer(selected, answers) {
      const correct = questions[current].correct;
      const buttons = answers.querySelectorAll("button");

      buttons.forEach((button, index) => {
        button.disabled = true;
        if (index === correct) button.classList.add("correct");
        if (index === selected && selected !== correct) {
          button.classList.add("incorrect");
        }
      });

      if (selected === correct) {
        score++;
        feedback.textContent = "Correct!";
      } else {
        feedback.textContent = `Not quite. The answer is ${questions[current].answers[correct]}.`;
      }

      nextButton.textContent =
        current === questions.length - 1 ? "See results" : "Next question";
      nextButton.hidden = false;
    }

    nextButton.addEventListener("click", () => {
      current++;
      if (current < questions.length) {
        showQuestion();
      } else {
        quiz.hidden = true;
        progress.textContent = "";
        feedback.textContent = "";
        nextButton.hidden = true;
        result.hidden = false;
        scoreText.textContent = `You scored ${score} out of ${questions.length}.`;
      }
    });

    restartButton.addEventListener("click", () => {
      current = 0;
      score = 0;
      quiz.hidden = false;
      result.hidden = true;
      showQuestion();
    });

    showQuestion();
  </script>
</body>
</html>
