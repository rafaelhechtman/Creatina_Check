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
            max-width: 1000px;
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
            display: flex;
            justify-content: space-between;
            flex-wrap: wrap;
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

        .calendar {
            display: table;
            border-collapse: collapse;
            width: 100%;
            margin-top: 1em;
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
            color: white;
            border: none;
            padding: 0.3em 0.5em;
            cursor: pointer;
            font-size: 0.7em;
            position: absolute;
            bottom: 0.5em;
            right: 0.5em;
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
                <div class="calendar-wrapper">
                    <table class="calendar" id="creatina-calendar"></table>
                </div>
            </div>
            <div class="calendar-container">
                <h2>Calendário de Exercícios</h2>
                <div class="calendar-wrapper">
                    <table class="calendar" id="exercicio-calendar"></table>
                </div>
            </div>
        </section>
    </main>
    <footer>
        Minhas Tarefas | Desenvolvido por Rafael Edler Hechtman
    </footer>
    <script>
        document.addEventListener("DOMContentLoaded", () => {
            const calendars = {
                creatina: document.getElementById('creatina-calendar'),
                exercicio: document.getElementById('exercicio-calendar')
            };

            const months = ["Julho", "Agosto", "Setembro", "Outubro", "Novembro", "Dezembro"];
            const daysInMonth = [31, 31, 30, 31, 30, 31];
            const startDay = [8, 1, 1, 1, 1, 1];

            function createCalendar(calendar, month, days, startDay) {
                const headerRow = document.createElement('tr');
                const daysOfWeek = ["Dom", "Seg", "Ter", "Qua", "Qui", "Sex", "Sáb"];
                daysOfWeek.forEach(day => {
                    const th = document.createElement('th');
                    th.textContent = day;
                    headerRow.appendChild(th);
                });
                calendar.appendChild(headerRow);

                let date = 1;
                for (let i = 0; i < 6; i++) {
                    const row = document.createElement('tr');

                    for (let j = 0; j < 7; j++) {
                        const cell = document.createElement('td');
                        if (i === 0 && j < startDay[month]) {
                            cell.textContent = "";
                        } else if (date > days[month]) {
                            break;
                        } else {
                            cell.textContent = date;
                            const button = document.createElement('button');
                            button.textContent = "Marcar";
                            button.addEventListener('click', () => {
                                toggleCompletion(button, calendar.id, `${date}-${month}`);
                            });
                            cell.appendChild(button);
                            date++;
                        }
                        row.appendChild(cell);
                    }
                    calendar.appendChild(row);
                }
            }

            Object.keys(calendars).forEach(type => {
                for (let month = 0; month < months.length; month++) {
                    createCalendar(calendars[type], month, daysInMonth, startDay);
                }
            });

            function toggleCompletion(button, key, date) {
                let completed = button.classList.toggle('completed');
                button.textContent = completed ? "Desmarcar" : "Marcar";
                saveCompletion(key, date, completed);
            }

            function saveCompletion(key, date, completed) {
                let data = JSON.parse(localStorage.getItem(key) || '{}');
                data[date] = completed;
                localStorage.setItem(key, JSON.stringify(data));
            }

            function loadCompletion() {
                Object.keys(calendars).forEach(type => {
                    let key = type;
                    let data = JSON.parse(localStorage.getItem(key) || '{}');
                    Object.keys(data).forEach(date => {
                        let [day, month] = date.split('-');
                        let button = calendars[type].querySelector(`td:contains('${day}') button`);
                        if (data[date]) {
                            button.classList.add('completed');
                            button.textContent = "Desmarcar";
                        }
                    });
                });
            }

            loadCompletion();
        });
    </script>
</body>
</html>
