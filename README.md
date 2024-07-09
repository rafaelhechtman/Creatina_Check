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

        .return-home {
            text-align: center;
            margin-top: 1em;
        }

        .return-home button {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 0.5em 1em;
            cursor: pointer;
        }

        .color-picker {
            display: flex;
            justify-content: space-around;
            margin-top: 1em;
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
    </style>
</head>
<body>
    <header>
        <input type="text" class="name-input" id="username" placeholder="Digite seu nome">
    </header>
    <main>
        <!-- Página Inicial -->
        <section id="home-page">
            <div class="return-home">
                <button onclick="showCalendar('creatina')">Rotina Creatina</button>
                <button onclick="showCalendar('musculacao')">Rotina Musculação</button>
            </div>
        </section>

        <!-- Página de Calendário da Creatina -->
        <section id="creatina-page" class="calendar-section" style="display: none;">
            <div class="calendar-container">
                <h2>Calendário da Creatina</h2>
                <div class="calendar-nav">
                    <button onclick="changeMonth('creatina', -1)">&lt; Mês Anterior</button>
                    <span id="creatina-month-year"></span>
                    <button onclick="changeMonth('creatina', 1)">Próximo Mês &gt;</button>
                </div>
                <table class="calendar" id="creatina-calendar"></table>
                <div class="return-home">
                    <button onclick="showHomePage()">Voltar para a página inicial</button>
                </div>
            </div>
        </section>

        <!-- Página de Calendário de Musculação -->
        <section id="musculacao-page" class="calendar-section" style="display: none;">
            <div class="calendar-container">
                <h2>Calendário de Musculação</h2>
                <div class="calendar-nav">
                    <button onclick="changeMonth('musculacao', -1)">&lt; Mês Anterior</button>
                    <span id="musculacao-month-year"></span>
                    <button onclick="changeMonth('musculacao', 1)">Próximo Mês &gt;</button>
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
                <div class="color-picker">
                    <button class="color-button blue" onclick="chooseColor('blue')">Azul</button>
                    <button class="color-button yellow" onclick="chooseColor('yellow')">Amarelo</button>
                    <button class="color-button green" onclick="chooseColor('green')">Verde</button>
                </div>
                <div class="return-home">
                    <button onclick="showHomePage()">Voltar para a página inicial</button>
                </div>
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

            function loadSavedData(calendar) {
                const { key } = calendar;
                const savedData = JSON.parse(localStorage.getItem(key)) || {};
                calendars[key].data = savedData;

                const buttons = document.querySelectorAll(`#${calendar.key}-calendar .mark-button`);
                buttons.forEach(button => {
                    const date = button.dataset.date;
                    if (calendars[key].data[date] && calendars[key].data[date].completed) {
                        button.classList.add('completed');
                    } else {
                        button.classList.remove('completed');
                    }
                });
            }

            function saveData() {
                Object.keys(calendars).forEach(key => {
                    localStorage.setItem(key, JSON.stringify(calendars[key].data));
                });
            }

            function changeMonth(calendarKey, direction) {
                currentMonth += direction;
                if (currentMonth === 12) {
                    currentMonth = 0;
                    currentYear++;
                } else if (currentMonth === -1) {
                    currentMonth = 11;
                    currentYear--;
                }
                renderCalendar(calendars[calendarKey]);
            }

            function getMonthName(monthIndex) {
                const months = ["Janeiro", "Fevereiro", "Março", "Abril", "Maio", "Junho",
                                "Julho", "Agosto", "Setembro", "Outubro", "Novembro", "Dezembro"];
                return months[monthIndex];
            }

            function showHomePage() {
                document.getElementById('home-page').style.display = 'block';
                document.getElementById('creatina-page').style.display = 'none';
                document.getElementById('musculacao-page').style.display = 'none';
            }

            function showCalendar(calendarType) {
                if (calendarType === 'creatina') {
                    document.getElementById('home-page').style.display = 'none';
                    document.getElementById('creatina-page').style.display = 'block';
                    document.getElementById('musculacao-page').style.display = 'none';
                    renderCalendar(calendars.creatina);
                } else if (calendarType === 'musculacao') {
                    document.getElementById('home-page').style.display = 'none';
                    document.getElementById('creatina-page').style.display = 'none';
                    document.getElementById('musculacao-page').style.display = 'block';
                    renderCalendar(calendars.musculacao);
                }
            }

            function chooseColor(color) {
                const colorButtons = document.querySelectorAll('.color-button');
                colorButtons.forEach(button => button.classList.remove('selected'));
                
                switch (color) {
                    case 'blue':
                        document.querySelector('.blue').classList.add('selected');
                        break;
                    case 'yellow':
                        document.querySelector('.yellow').classList.add('selected');
                        break;
                    case 'green':
                        document.querySelector('.green').classList.add('selected');
                        break;
                    default:
                        break;
                }
            }

            document.getElementById('username').addEventListener('input', function() {
                localStorage.setItem('username', this.value);
            });

            const savedUsername = localStorage.getItem('username');
            if (savedUsername) {
                document.getElementById('username').value = savedUsername;
            }

            showHomePage(); // Mostra a página inicial por padrão
        });
    </script>
</body>
</html>

































