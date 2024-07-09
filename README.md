<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Minhas Tarefas</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background-color: #f0f0f0;
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        header {
            background-color: #007bff;
            color: white;
            padding: 1em;
            text-align: center;
        }

        main {
            padding: 1em;
            max-width: 800px;
            margin: auto;
        }

        .calendar-section {
            display: flex;
            justify-content: space-between;
            flex-wrap: wrap;
        }

        .calendar-container {
            flex: 1;
            margin: 0.5em;
        }

        .calendar-container h2 {
            font-size: 1.2em;
            text-align: center;
            background-color: #007bff;
            color: white;
            padding: 0.5em;
            font-family: 'Verdana', sans-serif;
            width: 100%;
        }

        .calendar-nav {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 1em;
        }

        .calendar-nav button {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 0.5em 1em;
            cursor: pointer;
        }

        .calendar {
            display: table;
            border-collapse: collapse;
            width: 100%;
        }

        .calendar th, .calendar td {
            border: 1px solid #ccc;
            padding: 0.5em;
            text-align: center;
        }

        .calendar th {
            background-color: #007bff;
            color: white;
            font-family: 'Verdana', sans-serif;
        }

        .calendar td {
            position: relative;
            height: 4em;
        }

        .calendar button {
            background-color: #28a745;
            border: none;
            width: 1em;
            height: 1em;
            cursor: pointer;
            position: absolute;
            bottom: 0.5em;
            right: 0.5em;
            border-radius: 50%;
        }

        .calendar button.completed {
            background-color: #dc3545;
        }

        footer {
            text-align: center;
            margin-top: 2em;
            font-size: 0.7em;
            color: #aaa;
        }

        .name-input {
            display: block;
            margin: 0 auto 1em;
            padding: 0.5em;
            font-size: 1em;
            width: 90%;
            max-width: 400px;
            text-align: center;
            font-family: 'Verdana', sans-serif;
        }
    </style>
</head>
<body>
    <header>
        <input type="text" class="name-input" id="username" placeholder="Digite seu nome">
    </header>
    <main>
        <section class="calendar-section">
            <div class="calendar-container">
                <h2>Calendário da Creatina</h2>
                <div class="calendar-nav">
                    <button onclick="changeMonth('creatina', -1)">&lt; Mês Anterior</button>
                    <span id="creatina-month-year"></span>
                    <button onclick="changeMonth('creatina', 1)">Próximo Mês &gt;</button>
                </div>
                <table class="calendar" id="creatina-calendar"></table>
            </div>
            <div class="calendar-container">
                <h2>Calendário de Exercícios</h2>
                <div class="calendar-nav">
                    <button onclick="changeMonth('exercicio', -1)">&lt; Mês Anterior</button>
                    <span id="exercicio-month-year"></span>
                    <button onclick="changeMonth('exercicio', 1)">Próximo Mês &gt;</button>
                </div>
                <table class="calendar" id="exercicio-calendar"></table>
            </div>
        </section>
    </main>
    <footer>
        Minhas Tarefas | Desenvolvido por Rafael Edler Hechtman
    </footer>
    <script>
        document.addEventListener("DOMContentLoaded", () => {
            const today = new Date();
            let currentMonth = today.getMonth();
            let currentYear = today.getFullYear();

            const calendars = {
                creatina: {
                    element: document.getElementById('creatina-calendar'),
                    monthYear: document.getElementById('creatina-month-year'),
                    key: 'creatina',
                    data: {}
                },
                exercicio: {
                    element: document.getElementById('exercicio-calendar'),
                    monthYear: document.getElementById('exercicio-month-year'),
                    key: 'exercicio',
                    data: {}
                }
            };

            const daysOfWeek = ["Dom", "Seg", "Ter", "Qua", "Qui", "Sex", "Sáb"];

            function renderCalendar(calendar) {
                const { element, monthYear, key } = calendar;
                element.innerHTML = '';
                monthYear.textContent = `${getMonthName(currentMonth)} ${currentYear}`;

                const firstDay = new Date(currentYear, currentMonth).getDay();
                const daysInMonth = new Date(currentYear, currentMonth + 1, 0).getDate();

                const headerRow = document.createElement('tr');
                daysOfWeek.forEach(day => {
                    const th = document.createElement('th');
                    th.textContent = day;
                    headerRow.appendChild(th);
                });
                element.appendChild(headerRow);

                let date = 1;
                for (let i = 0; i < 6; i++) {
                    const row = document.createElement('tr');

                    for (let j = 0; j < 7; j++) {
                        const cell = document.createElement('td');
                        if (i === 0 && j < firstDay) {
                            cell.textContent = "";
                        } else if (date > daysInMonth) {
                            break;
                        } else {
                            const button = document.createElement('button');
                            button.type = 'button';
                            button.classList.add('mark-button');
                            button.dataset.date = `${date}-${currentMonth + 1}-${currentYear}`;
                            button.addEventListener('click', () => toggleCompletion(button, key));
                            cell.textContent = date;
                            cell.appendChild(button);
                            date++;
                        }
                        row.appendChild(cell);
                    }
                    element.appendChild(row);
                }

                loadCompletion(calendar);
            }

            function getMonthName(monthIndex) {
                const months = ["Janeiro", "Fevereiro", "Março", "Abril", "Maio", "Junho", "Julho", "Agosto", "Setembro", "Outubro", "Novembro", "Dezembro"];
                return months[monthIndex];
            }

            function changeMonth(calendarKey, offset) {
                const calendar = calendars[calendarKey];
                currentMonth += offset;
                if (currentMonth < 0) {
                    currentMonth = 11;
                    currentYear--;
                } else if (currentMonth > 11) {
                    currentMonth = 0;
                    currentYear++;
                }
                renderCalendar(calendar);
            }

            function toggleCompletion(button, key) {
                button.classList.toggle('completed');
                const date = button.dataset.date;
                saveCompletion(key, date, button.classList.contains('completed'));
            }

            function saveCompletion(key, date, completed) {
                if (!calendars[key].data[date]) {
                    calendars[key].data[date] = completed;
                    localStorage.setItem(key, JSON.stringify(calendars[key].data));
                } else {
                    delete calendars[key].data[date];
                    localStorage.removeItem(key);
                }
            }

            function loadCompletion(calendar) {
                const savedData = JSON.parse(localStorage.getItem(calendar.key)) || {};
                const buttons = calendar.element.querySelectorAll('.mark-button');
                buttons.forEach(button => {
                    const date = button.dataset.date;
                    if (savedData[date]) {
                        button.classList.add('completed');
                    }
                });
            }

            function saveName() {
                const nameInput = document.getElementById('username');
                localStorage.setItem('username', nameInput.value);
            }

            function loadName() {
                const nameInput = document.getElementById('username');
                const savedName = localStorage.getItem('username');
                if (savedName) {
                    nameInput.value = savedName;
                }
            }

            document.getElementById('username').addEventListener('input', saveName);

            loadName();
            Object.values(calendars).forEach(renderCalendar);
        });
    </script>
</body>
</html>






























