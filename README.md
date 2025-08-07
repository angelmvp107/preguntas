
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quiz de Polímeros - UAQ FI</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }

        .container {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
            max-width: 800px;
            width: 100%;
            padding: 40px;
            text-align: center;
            position: relative;
            overflow: hidden;
        }

        .container::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 4px;
            background: linear-gradient(90deg, #667eea, #764ba2, #f093fb);
        }

        h1 {
            color: #2c3e50;
            font-size: 2.5rem;
            margin-bottom: 10px;
            font-weight: 700;
            background: linear-gradient(135deg, #667eea, #764ba2);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .authors {
            color: #7f8c8d;
            font-size: 1rem;
            margin-bottom: 5px;
            font-weight: 500;
        }

        .university {
            color: #95a5a6;
            font-size: 0.9rem;
            margin-bottom: 30px;
            font-weight: 400;
        }

        .question-container {
            margin-bottom: 30px;
            text-align: left;
        }

        .question-number {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            padding: 8px 16px;
            border-radius: 20px;
            font-size: 0.9rem;
            font-weight: 600;
            display: inline-block;
            margin-bottom: 15px;
        }

        .question {
            font-size: 1.3rem;
            color: #2c3e50;
            margin-bottom: 20px;
            font-weight: 600;
            line-height: 1.4;
        }

        .options {
            display: grid;
            gap: 12px;
        }

        .option {
            background: #f8f9fa;
            border: 2px solid #e9ecef;
            border-radius: 12px;
            padding: 15px 20px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 1rem;
            text-align: left;
            position: relative;
            overflow: hidden;
        }

        .option::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.4), transparent);
            transition: left 0.5s;
        }

        .option:hover {
            border-color: #667eea;
            background: #f0f4ff;
            transform: translateY(-2px);
            box-shadow: 0 8px 20px rgba(102, 126, 234, 0.15);
        }

        .option:hover::before {
            left: 100%;
        }

        .option.selected {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            border-color: #667eea;
        }

        .option.correct {
            background: linear-gradient(135deg, #4CAF50, #45a049);
            border-color: #4CAF50;
            color: white;
        }

        .option.incorrect {
            background: linear-gradient(135deg, #f44336, #e53935);
            border-color: #f44336;
            color: white;
        }

        .navigation {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 30px;
            gap: 20px;
        }

        .btn {
            padding: 12px 30px;
            border: none;
            border-radius: 25px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .btn-primary {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
        }

        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 20px rgba(102, 126, 234, 0.3);
        }

        .btn-secondary {
            background: #6c757d;
            color: white;
        }

        .btn-secondary:hover {
            background: #5a6268;
            transform: translateY(-2px);
        }

        .btn:disabled {
            opacity: 0.6;
            cursor: not-allowed;
            transform: none;
        }

        .progress-bar {
            background: #e9ecef;
            height: 8px;
            border-radius: 4px;
            margin-bottom: 30px;
            overflow: hidden;
        }

        .progress-fill {
            background: linear-gradient(90deg, #667eea, #764ba2);
            height: 100%;
            border-radius: 4px;
            transition: width 0.5s ease;
        }

        .results {
            text-align: center;
            padding: 40px 20px;
        }

        .score {
            font-size: 3rem;
            font-weight: 700;
            background: linear-gradient(135deg, #667eea, #764ba2);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            margin-bottom: 20px;
        }

        .score-message {
            font-size: 1.3rem;
            color: #2c3e50;
            margin-bottom: 30px;
        }

        .hidden {
            display: none;
        }

        .fade-in {
            animation: fadeIn 0.5s ease-in;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        @media (max-width: 768px) {
            .container {
                padding: 30px 20px;
            }

            h1 {
                font-size: 2rem;
            }

            .question {
                font-size: 1.1rem;
            }

            .navigation {
                flex-direction: column;
            }

            .btn {
                width: 100%;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Quiz de Polímeros</h1>
        <div class="authors">Andrés Guadalupe García Franco, Ángel Antonio Ruiz García, Pablo Nicolás García Huerta</div>
        <div class="university">UAQ FI</div>

        <div class="progress-bar">
            <div class="progress-fill" id="progressFill"></div>
        </div>

        <div id="quizContainer" class="fade-in">
            <div class="question-container">
                <div class="question-number" id="questionNumber">Pregunta 1 de 5</div>
                <div class="question" id="questionText"></div>
                <div class="options" id="optionsContainer"></div>
            </div>

            <div class="navigation">
                <button class="btn btn-secondary" id="prevBtn" onclick="previousQuestion()">Anterior</button>
                <button class="btn btn-primary" id="nextBtn" onclick="nextQuestion()">Siguiente</button>
            </div>
        </div>

        <div id="resultsContainer" class="results hidden">
            <div class="score" id="finalScore">0/5</div>
            <div class="score-message" id="scoreMessage"></div>
            <button class="btn btn-primary" onclick="restartQuiz()">Reiniciar Quiz</button>
        </div>
    </div>

    <script>
        const questions = [
            {
                question: "¿En qué año se creó el primer polímero sintético y quién lo desarrolló?",
                options: [
                    "1905 - Hermann Staudinger",
                    "1907 - Leo Hendrik Baekeland",
                    "1922 - Marcellin Berthelot",
                    "1926 - Leo Hendrik Baekeland"
                ],
                correct: 1,
                explanation: "Leo Hendrik Baekeland desarrolló la baquelita en 1907 con fenol y formaldehído."
            },
            {
                question: "¿Cuál es el proceso de formación de polímeros donde los monómeros se unen eliminando una molécula pequeña como agua o alcohol?",
                options: [
                    "Polimerización por adición",
                    "Polimerización por condensación",
                    "Moldeo por inyección",
                    "Termoformado"
                ],
                correct: 1,
                explanation: "En la polimerización por condensación se elimina una molécula pequeña como agua."
            },
            {
                question: "¿Qué significa la palabra 'polímero' según su origen griego?",
                options: [
                    "Muchas formas",
                    "Largas cadenas",
                    "Muchas partes",
                    "Grandes moléculas"
                ],
                correct: 2,
                explanation: "Polímero proviene del griego: polys (muchos) y meros (partes, segmentos)."
            },
            {
                question: "¿Cuáles son ejemplos de polímeros naturales mencionados en el documento?",
                options: [
                    "Nylon y PVC",
                    "Polietileno y polipropileno",
                    "Ácidos nucleicos y proteínas",
                    "Baquelita y poliestireno"
                ],
                correct: 2,
                explanation: "Los ácidos nucleicos y proteínas son ejemplos de polímeros naturales."
            },
            {
                question: "¿Qué características físicas presentan los polímeros a bajas temperaturas?",
                options: [
                    "Son elásticos y flexibles",
                    "Poseen dureza y propiedades vítreas",
                    "Se vuelven líquidos",
                    "Pierden su estructura cristalina"
                ],
                correct: 1,
                explanation: "A bajas temperaturas los polímeros poseen dureza y propiedades vítreas."
            }
        ];

        let currentQuestion = 0;
        let userAnswers = [];
        let score = 0;
        let quizCompleted = false;

        function initializeQuiz() {
            currentQuestion = 0;
            userAnswers = [];
            score = 0;
            quizCompleted = false;
            document.getElementById('quizContainer').classList.remove('hidden');
            document.getElementById('resultsContainer').classList.add('hidden');
            updateProgress();
            displayQuestion();
        }

        function displayQuestion() {
            const question = questions[currentQuestion];
            document.getElementById('questionNumber').textContent = `Pregunta ${currentQuestion + 1} de ${questions.length}`;
            document.getElementById('questionText').textContent = question.question;
            
            const optionsContainer = document.getElementById('optionsContainer');
            optionsContainer.innerHTML = '';
            
            question.options.forEach((option, index) => {
                const optionElement = document.createElement('div');
                optionElement.className = 'option';
                optionElement.textContent = option;
                optionElement.onclick = () => selectOption(index);
                
                if (userAnswers[currentQuestion] === index) {
                    optionElement.classList.add('selected');
                }
                
                optionsContainer.appendChild(optionElement);
            });
            
            updateNavigation();
        }

        function selectOption(optionIndex) {
            userAnswers[currentQuestion] = optionIndex;
            
            const options = document.querySelectorAll('.option');
            options.forEach((option, index) => {
                option.classList.remove('selected');
                if (index === optionIndex) {
                    option.classList.add('selected');
                }
            });
            
            updateNavigation();
        }

        function updateNavigation() {
            const prevBtn = document.getElementById('prevBtn');
            const nextBtn = document.getElementById('nextBtn');
            
            prevBtn.disabled = currentQuestion === 0;
            
            if (currentQuestion === questions.length - 1) {
                nextBtn.textContent = userAnswers[currentQuestion] !== undefined ? 'Finalizar' : 'Finalizar';
                nextBtn.disabled = userAnswers[currentQuestion] === undefined;
            } else {
                nextBtn.textContent = 'Siguiente';
                nextBtn.disabled = userAnswers[currentQuestion] === undefined;
            }
        }

        function nextQuestion() {
            if (currentQuestion < questions.length - 1) {
                currentQuestion++;
                updateProgress();
                displayQuestion();
            } else {
                finishQuiz();
            }
        }

        function previousQuestion() {
            if (currentQuestion > 0) {
                currentQuestion--;
                updateProgress();
                displayQuestion();
            }
        }

        function updateProgress() {
            const progress = ((currentQuestion + 1) / questions.length) * 100;
            document.getElementById('progressFill').style.width = progress + '%';
        }

        function finishQuiz() {
            score = 0;
            userAnswers.forEach((answer, index) => {
                if (answer === questions[index].correct) {
                    score++;
                }
            });
            
            showResults();
        }

        function showResults() {
            document.getElementById('quizContainer').classList.add('hidden');
            document.getElementById('resultsContainer').classList.remove('hidden');
            
            const finalScore = document.getElementById('finalScore');
            const scoreMessage = document.getElementById('scoreMessage');
            
            finalScore.textContent = `${score}/${questions.length}`;
            
            let message = '';
            const percentage = (score / questions.length) * 100;
            
            if (percentage >= 90) {
                message = '¡Excelente! Dominas muy bien el tema de polímeros.';
            } else if (percentage >= 70) {
                message = '¡Muy bien! Tienes un buen conocimiento sobre polímeros.';
            } else if (percentage >= 50) {
                message = 'Bien. Necesitas repasar algunos conceptos sobre polímeros.';
            } else {
                message = 'Te recomendamos estudiar más sobre polímeros para mejorar.';
            }
            
            scoreMessage.textContent = message;
            
            document.getElementById('resultsContainer').classList.add('fade-in');
        }

        function restartQuiz() {
            initializeQuiz();
            document.getElementById('quizContainer').classList.add('fade-in');
        }

        // Inicializar el quiz al cargar la página
        window.onload = initializeQuiz;
    </script>
</body>
</html>
