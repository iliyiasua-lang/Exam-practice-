<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>MTH 102 Exam Practice</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f7fa;
      margin: 0;
      padding: 20px;
    }

    .container {
      max-width: 700px;
      margin: auto;
      background: white;
      padding: 30px;
      border-radius: 10px;
      box-shadow: 0 0 10px #ccc;
    }

    h1, h2 {
      text-align: center;
    }

    button {
      margin-top: 20px;
      padding: 12px 20px;
      font-size: 16px;
      background-color: #007bff;
      color: white;
      border: none;
      border-radius: 6px;
      cursor: pointer;
    }

    button:disabled {
      background-color: #aaa;
      cursor: not-allowed;
    }

    label {
      display: block;
      margin-bottom: 10px;
      font-size: 16px;
    }

    .hidden {
      display: none;
    }
  </style>
</head>
<body>
  <div class="container" id="home-page">
    <h1>MTH 102 Practice Exam</h1>
    <p>35 questions on Differentiation & Integration<br>2 marks each.</p>
    <button onclick="startExam()">Start Exam</button>
  </div>

  <div class="container hidden" id="exam-page">
    <h2 id="question-number">Question 1 of 35</h2>
    <p id="question-text"></p>
    <form id="options-form"></form>
    <button id="next-btn" disabled onclick="nextQuestion()">Next</button>
  </div>

  <div class="container hidden" id="result-page">
    <h1>Exam Completed!</h1>
    <p id="score"></p>
    <button onclick="goHome()">Go Back</button>
  </div>

  <script>
    const questions = [
      { question: "What is the derivative of f(x) = x²?", options: ["2x", "x", "x²", "1"], answer: "2x" },
      { question: "∫ x dx = ?", options: ["x²/2 + C", "x/2 + C", "2x + C", "ln(x) + C"], answer: "x²/2 + C" },
      { question: "If f(x) = sin(x), then f'(x) = ?", options: ["cos(x)", "-cos(x)", "-sin(x)", "sec(x)"], answer: "cos(x)" },
      { question: "What is d/dx (ln(x))?", options: ["1/x", "x", "ln(x)", "0"], answer: "1/x" },
      { question: "∫ 1/x dx = ?", options: ["ln|x| + C", "1/x + C", "x + C", "x²/2 + C"], answer: "ln|x| + C" },
      { question: "What is the derivative of f(x) = e^x?", options: ["e^x", "x*e^x", "ln(x)", "1/x"], answer: "e^x" },
      { question: "d/dx (cos(x)) = ?", options: ["-sin(x)", "sin(x)", "cos(x)", "-cos(x)"], answer: "-sin(x)" },
      { question: "∫ cos(x) dx = ?", options: ["sin(x) + C", "-cos(x) + C", "tan(x) + C", "-sin(x) + C"], answer: "sin(x) + C" },
      { question: "d/dx (tan(x)) = ?", options: ["sec²(x)", "tan²(x)", "cos²(x)", "sec(x)"], answer: "sec²(x)" },
      { question: "∫ sec²(x) dx = ?", options: ["tan(x) + C", "sec(x) + C", "cos(x) + C", "ln|x| + C"], answer: "tan(x) + C" },
      { question: "What is the derivative of f(x) = ln(x²)?", options: ["2/x", "1/x", "x", "2x"], answer: "2/x" },
      { question: "∫ e^x dx = ?", options: ["e^x + C", "x*e^x", "ln(x)", "x^e + C"], answer: "e^x + C" },
      { question: "d/dx (x⁴) = ?", options: ["4x³", "x³", "x⁴", "4x²"], answer: "4x³" },
      { question: "∫ x² dx = ?", options: ["x³/3 + C", "2x + C", "x²/2 + C", "3x² + C"], answer: "x³/3 + C" },
      { question: "d/dx (a^x) = ?", options: ["a^x ln(a)", "x*a^x", "ln(x)", "1/x"], answer: "a^x ln(a)" },
      { question: "∫ a^x dx = ?", options: ["a^x/ln(a) + C", "x*a^x + C", "ln(x) + C", "1/x + C"], answer: "a^x/ln(a) + C" },
      { question: "d/dx (log₁₀x) = ?", options: ["1/(x ln(10))", "1/x", "ln(x)", "10/x"], answer: "1/(x ln(10))" },
      { question: "∫ x⁴ dx = ?", options: ["x⁵/5 + C", "4x³ + C", "x⁴/4 + C", "x⁵ + C"], answer: "x⁵/5 + C" },
      { question: "d/dx (sqrt(x)) = ?", options: ["1/(2√x)", "2√x", "√x", "1/x"], answer: "1/(2√x)" },
      { question: "∫ √x dx = ?", options: ["(2/3)x^(3/2) + C", "x^(1/2) + C", "x² + C", "ln(x) + C"], answer: "(2/3)x^(3/2) + C" },
      { question: "d/dx (1/x) = ?", options: ["-1/x²", "1/x", "0", "-x"], answer: "-1/x²" },
      { question: "∫ -1/x² dx = ?", options: ["1/x + C", "-x + C", "-1/x + C", "x² + C"], answer: "1/x + C" },
      { question: "d/dx (arcsin(x)) = ?", options: ["1/√(1 - x²)", "1/(1 + x²)", "arccos(x)", "sec(x)"], answer: "1/√(1 - x²)" },
      { question: "∫ 1/√(1 - x²) dx = ?", options: ["arcsin(x) + C", "arccos(x) + C", "ln(x) + C", "tan⁻¹(x) + C"], answer: "arcsin(x) + C" },
      { question: "d/dx (tan⁻¹x) = ?", options: ["1/(1 + x²)", "x", "1/x", "ln(x)"], answer: "1/(1 + x²)" },
      { question: "∫ 1/(1 + x²) dx = ?", options: ["tan⁻¹x + C", "sec⁻¹x + C", "ln(x) + C", "1/x + C"], answer: "tan⁻¹x + C" },
      { question: "What is the integral of 0 dx?", options: ["C", "0", "1", "Undefined"], answer: "C" },
      { question: "What is d/dx (x ln x)?", options: ["ln x + 1", "1/x", "x/x", "x ln x"], answer: "ln x + 1" },
      { question: "What is d/dx (x⁻²)?", options: ["-2x⁻³", "2x⁻¹", "x⁻³", "-x²"], answer: "-2x⁻³" },
      { question: "∫ x⁻² dx = ?", options: ["-1/x + C", "x + C", "x⁻¹ + C", "ln x + C"], answer: "-1/x + C" },
      { question: "d/dx (sec x) = ?", options: ["sec x tan x", "tan x", "cos x", "-sec x"], answer: "sec x tan x" },
      { question: "∫ sin x dx = ?", options: ["-cos x + C", "cos x + C", "tan x + C", "-sin x + C"], answer: "-cos x + C" },
      { question: "What is d/dx (x³ + 2x² + x)?", options: ["3x² + 4x + 1", "3x + 2", "x² + 2x", "3x² + 2x"], answer: "3x² + 4x + 1" },
      { question: "What is ∫ (3x² + 4x + 1) dx?", options: ["x³ + 2x² + x + C", "3x + 2 + C", "x² + 2x + C", "x³ + x² + C"], answer: "x³ + 2x² + x + C" }
    ];

    let currentQuestion = 0;
    let score = 0;
    let selectedAnswer = null;

    const questionText = document.getElementById("question-text");
    const optionsForm = document.getElementById("options-form");
    const nextBtn = document.getElementById("next-btn");
    const qNumber = document.getElementById("question-number");
    const homePage = document.getElementById("home-page");
    const examPage = document.getElementById("exam-page");
    const resultPage = document.getElementById("result-page");
    const scoreText = document.getElementById("score");

    function startExam() {
      homePage.classList.add("hidden");
      examPage.classList.remove("hidden");
      loadQuestion();
    }

    function loadQuestion() {
      selectedAnswer = null;
      const q = questions[currentQuestion];
      qNumber.textContent = `Question ${currentQuestion + 1} of ${questions.length}`;
      questionText.textContent = q.question;
      optionsForm.innerHTML = "";
      const labels = ["A", "B", "C", "D"];
      q.options.forEach((opt, i) => {
        const label = document.createElement("label");
        const input = document.createElement("input");
        input.type = "radio";
        input.name = "option";
        input.value = opt;
        input.onclick = () => {
          selectedAnswer = opt;
          nextBtn.disabled = false;
        };
        label.appendChild(input);
        label.appendChild(document.createTextNode(` ${labels[i]}. ${opt}`));
        optionsForm.appendChild(label);
      });
      nextBtn.disabled = true;
    }

    function nextQuestion() {
      if (selectedAnswer === questions[currentQuestion].answer) {
        score += 2;
      }
      currentQuestion++;
      if (currentQuestion < questions.length) {
        loadQuestion();
      } else {
        examPage.classList.add("hidden");
        resultPage.classList.remove("hidden");
        scoreText.textContent = `Your score: ${score} / 70`;
      }
    }

    function goHome() {
      location.reload();
    }
  </script>
</body>
</html>