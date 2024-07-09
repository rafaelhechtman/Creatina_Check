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
            margin-top: 1em;
        }

        .calendar-container {
            margin-bottom: 2em;
        }

        .calendar-container h2 {
            font-size: 1.2em;
            text-align: center;
            background-color: #007bff;
            color: white;
            padding: 0.5em;
            font-family: 'Verdana', sans-serif;
            margin-bottom: 0.5em;
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

        .calendar .arrow {
            width: 0; 
            height: 0; 
            border-left: 0.5em solid transparent;
            border-right: 0.5em solid transparent;
            border-top: 0.5em solid #28a745;
            cursor: pointer;
            position: absolute;
            bottom: 0.5em;
            right: 0.5em;
        }

        .color-picker {
            display: flex;
            justify-content: space-around;
            margin-top: 1em;
            position: absolute;
            bottom: 2em;
            right: 0.5em;
            background-color: #fff;
            padding: 0.5em;
            border: 1px solid #ccc;
            border-radius: 0.5em;
        }

        .color-button {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            cursor: pointer;
        }

        .blue { background-color: blue; }
        .yellow { background-color: yellow; }
        .green { background-color: green; }
        .hidden { display: none; }

        .muscle-table {
            margin-top: 1em;
            border-collapse: collapse;
            width: 100%;
        }

        .muscle-table th, .muscle-table td {
            border: 1px solid #ccc;
            padding: 0.5em;
            text-align: center;
        }

        .muscle-table th {
            background-color: #007bff;
            color: white;
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
                    <span id="creatina-month-year"></span>
                </div>
                <table class="calendar" id="creatina-calendar"></table>
            </div>
            <div class="calendar-container">
                <h2>Calendário de Musculação</h2>
                <div class="calendar-nav">
                    <span id="musculacao-month-year"></span>
                </div>
                <table class="calendar" id="musculacao-calendar"></table>
                <div class="muscle-table">
                    <h3>Tabela de Grupos Musculares</h3>
                    <table class="muscle-table">
                        <tr>
                            <th>Peito, Ombro e Tríceps</th>
                            <td class="blue"></td>
                        </tr>
                        <tr>
                            <th>Costas e Bíceps</th>
                            <td class="yellow"></td>
                        </tr>
                        <tr>
                            <th>Perna</th>
                            <td class="green"></td>
                        </tr>
                    </table>
                </div>
            </div>
        </section>
    </main>
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
                musculacao: {
                    element: document.getElementById('musculacao-calendar'),
                    monthYear: document.getElementById('musculacao-month-year'),
                    key: 'musculacao',
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
                            if (key === 'creatina') {
                                const button = document.createElement('button');
                                button.type = 'button';
                                button.classList.add('mark-button');
                                button.dataset.date = `${date}-${currentMonth + 1}-${currentYear}`;
                                button.addEventListener('click', () => toggleCompletion(button, key));
                                cell.textContent = date;
                                cell.appendChild(button);
                            } else if (key === 'musculacao') {
                                const arrow = document.createElement('div');
                                arrow.classList.add('arrow');
                                arrow.dataset.date = `${date}-${currentMonth + 1}-${currentYear}`;
                                arrow.addEventListener('click', () => showColorPicker(cell, arrow, key));
                                cell.textContent = date;
                                cell.appendChild(arrow);
                            }
                            date++;
                        }
                        row.appendChild(cell);
                    }
                    element.appendChild(row);
                }

                loadSavedData(calendar);
            }

            function toggleCompletion(button, key) {
                button.classList.toggle('completed');
                const date = button.dataset.date;
                if (!calendars[key].data[date]) {
                    calendars[key].data[date] = {};
                }
                calendars[key].data[date].completed = button.classList.contains('completed');
                saveData();
            }

            function showColorPicker(cell, arrow, key) {
                let colorPicker = cell.querySelector('.color-picker');
                if (colorPicker) {
                    colorPicker.remove();
                    return;
                }

                colorPicker = document.createElement('div');
                colorPicker.classList.add('color-picker');

                ['blue', 'yellow', 'green'].forEach(color => {
                    const colorButton = document.createElement('button');
                    colorButton.classList.add('color-button', color);
                    colorButton.addEventListener('click', () => {
                        arrow.className = `arrow ${color}`;
                        saveCompletion(arrow, key, color);
                        colorPicker.remove();
                    });
                    colorPicker.appendChild(colorButton);
                });

                cell.appendChild(colorPicker);
            }

            function saveCompletion(arrow, key, color) {
                const date = arrow.dataset.date;
                if (!calendars[key].data[date]) {
                    calendars[key].data[date] = {};
                }
                calendars[key].data[date].color = color;
                saveData();
            }

            function loadSavedData(calendar) {
                const { key } = calendar;
                const savedData = JSON.parse(localStorage.getItem(key)) || {};
                calendars[key].data = savedData;

                const buttons = document.querySelectorAll(`#${calendar.key}-calendar .mark-button`);
                buttons.forEach(button => {
                    const date = button.dataset.date;
                    if (calendars[key].data[date] && calendars[key].data[date].completed) {
                        button.classList.add('completed');
                    }
                });

                const arrows = document.querySelectorAll(`#${calendar.key}-calendar .arrow`);
                arrows.forEach(arrow => {
                    const date = arrow.dataset.date;
                    if (calendars[key].data[date] && calendars[key].data[date].color) {
                        arrow.className = `arrow ${calendars[key].data[date].color}`;
                    }
                });
            }

            function saveData() {
                Object.keys(calendars).forEach(key => {
                    localStorage.setItem(key, JSON.stringify(calendars[key].data));
                });
            }

            function getMonthName(monthIndex) {
                const months = ["Janeiro", "Fevereiro", "Março", "Abril", "Maio", "Junho",
                                "Julho", "Agosto", "Setembro", "Outubro", "Novembro", "Dezembro"];
                return months[monthIndex];
            }

            document.getElementById('username').addEventListener('input', function() {
                localStorage.setItem('username', this.value);
            });

            const savedUsername = localStorage.getItem('username');
            if (savedUsername) {
                document.getElementById('username').value = savedUsername;
            }

            renderCalendar(calendars.creatina);
            renderCalendar(calendars.musculacao);
        });
    </script>
</body>
</html>
