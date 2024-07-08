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

        header h1 {
            font-size: 2em;
            font-family: 'Verdana', sans-serif;
            margin: 0;
        }

        main {
            padding: 1em;
            max-width: 800px;
            margin: auto;
        }

        .calendar-section {
            margin-bottom: 2em;
        }

        .calendar-section h1 {
            text-align: center;
            font-size: 1.5em;
            font-family: 'Verdana', sans-serif;
            margin: 0;
        }

        .calendar-container {
            margin-top: 1em;
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
    </style>
</head>
<body>
    <header>
        <h1>Minhas Tarefas</h1>
    </header>
    <main>
        <section class="calendar-section">
            <h1>Minha rotina</h1>
            <div class="calendar-container">
                <h2>Calendário da Creatina</h2>
                <div class="calendar-nav">
                    <button onclick="prevMonth('creatina')">&lt; Mês Anterior</button>
                    <span id="creatina-month-year"></span>
                    <button onclick="nextMonth('creatina')">Próximo Mês &gt;</button>
                </div>
                <table class="calendar" id="creatina-calendar"></table>
            </div>
            <div class="calendar-container">
                <h2>Calendário de Exercícios</h2>
                <div class="calendar-nav">
                    <button onclick="prevMonth('exercicio')">&lt; Mês Anterior</button>
                    <span id="exercicio-month-year"></span>
                    <button onclick="nextMonth('exercicio')">Próximo Mês &gt;</button>
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
            const currentMonth = today.getMonth();
            const currentYear = today.getFullYear();

            const calendars = {
                creatina: {
                    element: document.getElementById('creatina-calendar'),
                    monthYear: document.getElementById('creatina-month-year'),
                    currentMonth: currentMonth,
                    currentYear: currentYear,
                    key: 'creatina'
                },
                exercicio: {
                    element: document.getElementById('exercicio-calendar'),
                    monthYear: document.getElementById('exercicio-month-year'),
                    currentMonth: currentMonth,
                    currentYear: currentYear,
                    key: 'exercicio'
                }
            };

            const daysOfWeek = ["Dom", "Seg", "Ter", "Qua", "Qui", "Sex", "Sáb"];

            function renderCalendar(calendar) {
                const {element, monthYear, currentMonth, currentYear, key} = calendar;
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
                            cell.textContent = date;
                            const button = document.createElement('button');
                            button.addEventListener('click', () => {
                                toggleCompletion(button, key, `${date}-${currentMonth}-${currentYear}`);
                            });
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

            function prevMonth(calendarKey) {
                const calendar = calendars[calendarKey];
                calendar.currentMonth--;
                if (calendar.currentMonth < 0) {
                    calendar.currentMonth = 11;
                    calendar.currentYear--;
                }
                renderCalendar(calendar);
            }

            function nextMonth(calendarKey) {
                const calendar = calendars[calendarKey];
                calendar.currentMonth++;
                if (calendar.currentMonth > 11) {
                    calendar.currentMonth = 0;
                    calendar.currentYear++;
                }
                renderCalendar(calendar);
            }

            function toggleCompletion(button, key, date) {
                let completed = button.classList.toggle('completed');
                saveCompletion(key, date, completed);
            }

            function saveCompletion(key, date, completed) {
                let data = JSON.parse(localStorage.getItem(key) || '{}');
                data[date] = completed;
                localStorage.setItem(key, JSON.stringify(data));
            }

            function loadCompletion(calendar) {
                const {element, key, currentMonth, currentYear} = calendar;
                let data = JSON.parse(localStorage.getItem(key) || '{}');
                Object.keys(data).forEach(date => {
                    const [day, month, year] = date.split('-');
                    if (parseInt(month) === currentMonth && parseInt(year) === currentYear) {
                        const button = element.querySelector(`td:nth-child(${parseInt(day) + (new Date(currentYear, currentMonth).getDay()) % 7}) button`);
                        if (data[date]) {
                            button.classList.add('completed');
                        }
                    }
                });
            }

            Object.keys(calendars).forEach(key => {
                renderCalendar(calendars[key]);
            });
        });
    </script>
</body>
</html>
