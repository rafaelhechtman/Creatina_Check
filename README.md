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
            cursor: pointer;
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

        .color-picker {
            display: none;
            position: absolute;
            top: 2em;
            left: 50%;
            transform: translateX(-50%);
            background-color: #fff;
            border: 1px solid #ccc;
            padding: 0.5em;
            border-radius: 0.5em;
            z-index: 10;
        }

        .color-picker .color-button {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            cursor: pointer;
            margin: 0.2em;
        }

        .color-picker .close-btn {
            background-color: #ccc;
            border: none;
            cursor: pointer;
            padding: 0.2em 0.5em;
            margin-left: 0.5em;
        }

        .blue { background-color: blue; }
        .yellow { background-color: yellow; }
        .green { background-color: green; }

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
            let selectedCell = null;

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
                                cell.dataset.date = `${date}-${currentMonth + 1}-${currentYear}`;
                                cell.addEventListener('click', () => showColorPicker(cell, key));
                                cell.textContent = date;
                                const colorPicker = createColorPicker();
                                cell.appendChild(colorPicker);
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

            function createColorPicker() {
                const colorPicker = document.createElement('div');
                colorPicker.classList.add('color-picker');
                ['blue', 'yellow', 'green'].forEach(color => {
                    const colorButton = document.createElement('div');
                    colorButton.classList.add('color-button', color);
                    colorButton.addEventListener('click', () => selectColor(color, colorPicker.parentElement));
                    colorPicker.appendChild(colorButton);
                });
                const closeButton = document.createElement('button');
                closeButton.classList.add('close-btn');
                closeButton.textContent = 'X';
                closeButton.addEventListener('click', () => closeColorPicker(colorPicker));
                colorPicker.appendChild(closeButton);
                return colorPicker;
            }

            function closeColorPicker(colorPicker) {
                colorPicker.style.display = 'none';
            }

            function showColorPicker(cell, key) {
                selectedCell = cell;
                const colorPicker = cell.querySelector('.color-picker');
                colorPicker.style.display = 'flex';
            }

            function selectColor(color, cell) {
                cell.style.backgroundColor = color;
                saveCompletion(cell, 'musculacao', color);
                const colorPicker = cell.querySelector('.color-picker');
                colorPicker.style.display = 'none';
            }

            function saveCompletion(cell, key, color) {
                const date = cell.dataset.date;
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

                const cells = document.querySelectorAll(`#${calendar.key}-calendar td`);
                cells.forEach(cell => {
                    const date = cell.dataset.date;
                    if (calendars[key].data[date] && calendars[key].data[date].color) {
                        cell.style.backgroundColor = calendars[key].data[date].color;
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
