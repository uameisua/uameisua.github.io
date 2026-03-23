# uameisua.github.io
<!DOCTYPE html>
<html lang="uk">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Професійно-орієнтаційний тест ДДТУ</title>
    <style>
        :root {
            --bg-page: #cfcbc3;
            --main-brown: #5f5037;
            --white: #ffffff;
        }

        body { 
            font-family: 'Times New Roman', Times, serif; 
            background-color: var(--bg-page); 
            margin: 0; padding: 20px;
            display: flex; justify-content: center;
        }

        .container { 
            max-width: 800px; width: 100%;
            background: var(--white); border-radius: 4px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.2);
            border-top: 10px solid var(--main-brown);
            overflow: hidden;
        }

        .header-banner img { width: 100%; height: auto; display: block; background-color: #eee; }

        .content { padding: 40px; }

        h1 { color: #333; font-size: 28px; margin-bottom: 20px; }

        .intro-text { font-size: 18px; line-height: 1.6; text-align: justify; border-top: 1px solid #ddd; padding-top: 20px; }

        .step-indicator {
            color: var(--main-brown);
            font-weight: bold;
            margin-bottom: 10px;
            display: block;
            text-align: right;
        }

        .question-block { margin-top: 20px; padding-bottom: 15px; border-bottom: 1px solid #f0f0f0; }

        .options { list-style: none; padding: 0; margin-top: 10px; }
        .options li { margin: 8px 0; }

        label { cursor: pointer; display: flex; align-items: center; font-size: 17px; }
        input[type="radio"] { margin-right: 12px; transform: scale(1.2); accent-color: var(--main-brown); }

        .buttons-container { display: flex; justify-content: center; gap: 20px; margin-top: 30px; }

        .btn {
            background: var(--main-brown); color: white; border: none;
            padding: 12px 35px; font-size: 18px; cursor: pointer;
            border-radius: 4px; transition: 0.3s;
        }

        .btn:hover { opacity: 0.9; }

        .hidden { display: none; }
        #result-text { font-size: 22px; color: #333; margin: 25px 0; line-height: 1.5; }
        .faculty-highlight { color: var(--main-brown); font-weight: bold; display: block; margin-top: 10px; }
    </style>
</head>
<body>

<div class="container">
    <div class="header-banner">
        <img src="top.jpg" alt="ДДТУ">
    </div>
    
    <div class="content">
        <div id="intro-page">
            <h1>Професійно-орієнтаційний тест</h1>
            <p class="intro-text">
                На роздоріжжі можливостей випускникам важко одразу обрати шлях, яким будуть йти усе подальше життя. Тож, з метою допомогти розібратися з власним профілем і схильностями, пропонуємо Вам пройти короткий тест, який згідно з Вашими навичками і вподобаннями вкаже на один з чотирьох факультетів Дніпровського державного технічного університету.
            </p>
            <button class="btn" onclick="startTest()">Почати тест</button>
        </div>

        <div id="test-page" class="hidden">
            <span id="step-info" class="step-indicator">Блок 1 з 4</span>
            <form id="quiz-form">
                <div id="questions-container"></div>
                <div class="buttons-container">
                    <button type="button" id="next-btn" class="btn" onclick="nextStep()">Далі</button>
                </div>
            </form>
        </div>

        <div id="result-page" class="hidden" style="text-align: center;">
            <h1>Ваш результат:</h1>
            <div id="result-text"></div>
            <button class="btn" onclick="location.reload()">Пройти ще раз</button>
        </div>
    </div>
</div>

<script>
    const questionGroups = [
        [
            { q: "Яка стаття в науковому журналі Вас би зацікавила найбільше?", a: "Нові методи захисту даних", b: "Як бренди маніпулюють свідомістю", v: "Тест-драйв безпілотного авто", g: "Створення надміцних сплавів" },
            { q: "Яку виставку Ви би відвідали із задоволенням?", a: "Робототехніки та ШІ", b: "Психології маркетингу", v: "Сучасного автопрому", g: "Технологій виплавки металів" },
            { q: "Про що Вам цікавіше дивитися відео на YouTube?", a: "Огляди гаджетів", b: "Інтерв’ю з підприємцями", v: "Реставрація авто", g: "Робота доменних печей" },
            { q: "Яке хобі Вам ближче?", a: "Збирання ПК, налаштування софту чи геймінг", b: "Ведення блогу, волонтерство або настільні ігри на стратегію", v: "Ремонт техніки", g: "Колекціонування мінералів або експерименти з хімічними наборами" }
        ],
        [
            { q: "Яке завдання для Вас найпростіше?", a: "Написати алгоритм для розв'язання логічного рівняння", b: "Підготувати презентацію про розвиток економіки країни", v: "Побудувати складне креслення деталі в ізометрії", g: "Провести лабораторний дослід з хімічними реактивами" },
            { q: "Яку олімпіаду Ви б обрали, якби участь була обов'язковою?", a: "Інформатика/Математика", b: "Історія/Англійська", v: "Фізика/Креслення", g: "Хімія/Біологія" },
            { q: "У якому кабінеті Ви почувалися б найкраще?", a: "Кабінет інформатики з встановленими новими програмами", b: "Актова зала, або кабінет мов", v: "Майстерня з верстаками та інструментами", g: "Лабораторія з хімічними реагентами" },
            { q: "Яку тему Вам було б легше засвоїти?", a: "Двійкова система числення та алгоритми", b: "Соціальна структура суспільства та закони ринку", v: "Закони руху Ньютона та будова двигунів", g: "Таблиця Менделєєва та кристалічні решітки" }
        ],
        [
            { q: "Який результат роботи принесе Вам найбільше задоволення?", a: "Працююча програма", b: "Успішний договір", v: "Зібраний двигун", g: "Нова марка сталі" },
            { q: "У якому середовищі Ви хочете працювати?", a: "У сучасному IT-офісі або віддалено (дистанційно)", b: "У бізнес-центрі, медіа-холдингу або банку", v: "У конструкторському бюро, автосервісі або транспортному терміналі", g: "На великому промисловому підприємстві чи науково-дослідному інституті" },
            { q: "Яку роль у команді Ви б обрали для себе?", a: "Розробник", b: "Аналітик", v: "Інженер-механік", g: "Експерт із матеріалів" },
            { q: "Що для Вас є найбільшим викликом у роботі?", a: "Помилка в коді", b: "Конфлікт партнерів", v: "Зламаний механізм", g: "Якість при плавці" }
        ],
        [
            { q: "Що друзі кажуть про Вас найчастіше?", a: "Ти знайдеш будь-яку помилку", b: "Ти переконаєш будь-кого", v: "Ти знаєш, як усе влаштовано", g: "Ти дуже уважний до деталей" },
            { q: "Як Ви підходите до розв'язання проблем?", a: "Логічно", b: "Через компроміс", v: "Розбираю на деталі", g: "Шукаю формулу рішення" },
            { q: "Що є Вашою сильною стороною?", a: "Концентрація", b: "Емпатія", v: "3D-уява", g: "Стійкість" },
            { q: "Що робите з новим гаджетом?", a: "Налаштовую софт", b: "Дивлюсь умови гарантії", v: "Вивчаю збірку", g: "Дивлюсь на матеріали" }
        ]
    ];

    const faculties = {
        'a': 'Факультет Комп\'ютерних технологій та енергетики',
        'b': 'Факультет Економіки та соціальних комунікацій',
        'v': 'Факультет Машинобудування та транспортних технологій',
        'g': 'Металургійний факультет'
    };

    let currentStep = 0;

    function startTest() {
        document.getElementById('intro-page').classList.add('hidden');
        document.getElementById('test-page').classList.remove('hidden');
        renderStep();
    }

    function renderStep() {
        const container = document.getElementById('questions-container');
        const info = document.getElementById('step-info');
        const btn = document.getElementById('next-btn');
        container.innerHTML = '';
        info.innerText = `Блок ${currentStep + 1} з 4`;
        if (currentStep === 3) btn.innerText = "Отримати результат";

        questionGroups[currentStep].forEach((item, index) => {
            container.innerHTML += `
                <div class="question-block">
                    <p><strong>${index + 1}. ${item.q}</strong></p>
                    <ul class="options">
                        <li><label><input type="radio" name="s${currentStep}q${index}" value="a" required> ${item.a}</label></li>
                        <li><label><input type="radio" name="s${currentStep}q${index}" value="b"> ${item.b}</label></li>
                        <li><label><input type="radio" name="s${currentStep}q${index}" value="v"> ${item.v}</label></li>
                        <li><label><input type="radio" name="s${currentStep}q${index}" value="g"> ${item.g}</label></li>
                    </ul>
                </div>
            `;
        });
        window.scrollTo(0,0);
    }

    function nextStep() {
        const form = document.getElementById('quiz-form');
        const formData = new FormData(form);
        let answered = 0;
        for (let pair of formData.entries()) if (pair[0].startsWith(`s${currentStep}`)) answered++;

        if (answered < 4) { alert("Будь ласка, дайте відповіді на всі питання!"); return; }

        if (currentStep < 3) {
            currentStep++;
            renderStep();
        } else {
            calculateResult(formData);
        }
    }

    function calculateResult(allData) {
        const counts = { a: 0, b: 0, v: 0, g: 0 };
        for (let value of allData.values()) counts[value]++;

        const maxScore = Math.max(...Object.values(counts));
        const winners = Object.keys(counts).filter(key => counts[key] === maxScore);

        let finalMessage = "";

        if (winners.length === 4) {
            finalMessage = "Ви дуже різносторонні, тож згідно опитування вам підходять усі факультети:";
        } else if (winners.length > 1) {
            finalMessage = "Ви різностороння особистість, і вам підходять:";
        } else {
            finalMessage = "Згідно з Вашими вподобаннями, Вам слід звернути увагу на:";
        }

        let facultyListHtml = winners.map(code => `<span class="faculty-highlight">${faculties[code]}</span>`).join("");

        document.getElementById('test-page').classList.add('hidden');
        document.getElementById('result-page').classList.remove('hidden');
        document.getElementById('result-text').innerHTML = finalMessage + facultyListHtml;
        window.scrollTo(0,0);
    }
</script>

</body>
</html>
