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

        .calendar-section h1 {
            display: flex;
            align-items: center;
            font-size: 1.2em;
            margin-bottom: 1em;
        }

        .calendar-section h1 img {
            margin-left: 10px;
            width: 40px;
            height: 40px;
            border-radius: 50%;
        }

        h1, h2 {
            color: #007bff;
            font-size: 1em;
        }

        .calendars {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
        }

        .calendar-container {
            flex: 1;
            margin: 1em 0.5em;
            min-width: 45%;
        }

        .calendar-container h2 {
            margin-top: 0;
            font-size: 1.2em;
            text-align: center;
            background-color: #007bff;
            color: white;
            padding: 0.5em;
            font-family: 'Verdana', sans-serif;
        }

        .calendar {
            display: flex;
            flex-wrap: wrap;
            list-style-type: none;
            padding: 0;
            margin: 0;
        }

        .calendar li {
            padding: 1em;
            border: 1px solid #ccc;
            display: flex;
            justify-content: space-between;
            align-items: center;
            width: calc(50% - 12px);
            box-sizing: border-box;
            margin: 6px;
            font-size: 0.8em;
        }

        button {
            background-color: #28a745;
            color: white;
            border: none;
            padding: 0.3em 0.5em;
            cursor: pointer;
            font-size: 0.7em;
        }

        button.completed {
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
            <h1>Minha rotina <img src="https://via.placeholder.com/40" alt="Creatina"></h1>
            <div class="calendars">
                <div class="calendar-container">
                    <h2>Calendário da Creatina</h2>
                    <ul class="calendar" id="creatina-calendar"></ul>
                </div>
                <div class="calendar-container">
                    <h2>Calendário de Exercícios</h2>
                    <ul class="calendar" id="exercicio-calendar"></ul>
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

            Object.keys(calendars).forEach(type => {
                let calendar = calendars[type];
                for (let month = 0; month < months.length; month++) {
                    for (let day = (month === 0 ? 8 : 1); day <= daysInMonth[month]; day++) {
                        let listItem = document.createElement('li');
                        listItem.textContent = `${day} de ${months[month]}`;
                        let button = document.createElement('button');
                        button.textContent = "Marcar";
                        button.addEventListener('click', () => {
                            toggleCompletion(button, `${type}`, `${day}-${month}`);
                        });
                        listItem.appendChild(button);
                        calendar.appendChild(listItem);
                    }
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
                    let key = `${type}`;
                    let data = JSON.parse(localStorage.getItem(key) || '{}');
                    Object.keys(data).forEach(date => {
                        let [day, month] = date.split('-');
                        let index = (month === '0' ? day - 8 : parseInt(day) + 23 + 31 * (month - 1));
                        let button = calendars[type].children[index].querySelector('button');
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
