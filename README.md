<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calendário da Creatina</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background-color: #f0f0f0;
            margin: 0;
            padding: 0;
        }

        nav {
            background-color: #007bff;
            color: white;
            padding: 1em;
            text-align: center;
        }

        nav ul {
            list-style-type: none;
            padding: 0;
        }

        nav ul li {
            display: inline;
            margin: 0 1em;
        }

        nav ul li a {
            color: white;
            text-decoration: none;
            font-weight: bold;
        }

        main {
            padding: 2em;
            max-width: 600px;
            margin: auto;
        }

        .calendar-section {
            display: none;
        }

        .calendar-section:target {
            display: block;
        }

        h1 {
            color: #007bff;
        }

        h2 {
            color: #333;
        }

        .calendar {
            list-style-type: none;
            padding: 0;
        }

        .calendar li {
            padding: 0.5em;
            border-bottom: 1px solid #ccc;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        button {
            background-color: #28a745;
            color: white;
            border: none;
            padding: 0.5em 1em;
            cursor: pointer;
        }

        button.completed {
            background-color: #dc3545;
        }
    </style>
</head>
<body>
    <nav>
        <ul>
            <li><a href="#duda">Duda</a></li>
            <li><a href="#rafa">Rafa</a></li>
            <li><a href="#ale">Alê</a></li>
        </ul>
    </nav>
    <main>
        <section id="duda" class="calendar-section">
            <h1>Duda</h1>
            <h2>Calendário da Creatina</h2>
            <ul class="calendar" id="duda-calendar"></ul>
        </section>
        <section id="rafa" class="calendar-section">
            <h1>Rafa</h1>
            <h2>Calendário da Creatina</h2>
            <ul class="calendar" id="rafa-calendar"></ul>
        </section>
        <section id="ale" class="calendar-section">
            <h1>Alê</h1>
            <h2>Calendário da Creatina</h2>
            <ul class="calendar" id="ale-calendar"></ul>
        </section>
    </main>
    <script>
        document.addEventListener("DOMContentLoaded", () => {
            const calendars = {
                'duda': document.getElementById('duda-calendar'),
                'rafa': document.getElementById('rafa-calendar'),
                'ale': document.getElementById('ale-calendar')
            };

            const months = ["Janeiro", "Fevereiro", "Março", "Abril", "Maio", "Junho", "Julho", "Agosto", "Setembro", "Outubro", "Novembro", "Dezembro"];
            const daysInMonth = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31];

            Object.keys(calendars).forEach(person => {
                let calendar = calendars[person];
                for (let month = 0; month < months.length; month++) {
                    for (let day = 1; day <= daysInMonth[month]; day++) {
                        let listItem = document.createElement('li');
                        listItem.textContent = `${day} de ${months[month]}`;
                        let button = document.createElement('button');
                        button.textContent = "Marcar";
                        button.addEventListener('click', () => {
                            toggleCompletion(button, person, `${day}-${month}`);
                        });
                        listItem.appendChild(button);
                        calendar.appendChild(listItem);
                    }
                }
            });

            function toggleCompletion(button, person, date) {
                let completed = button.classList.toggle('completed');
                button.textContent = completed ? "Desmarcar" : "Marcar";
                saveCompletion(person, date, completed);
            }

            function saveCompletion(person, date, completed) {
                let data = JSON.parse(localStorage.getItem(person) || '{}');
                data[date] = completed;
                localStorage.setItem(person, JSON.stringify(data));
            }

            function loadCompletion() {
                Object.keys(calendars).forEach(person => {
                    let data = JSON.parse(localStorage.getItem(person) || '{}');
                    Object.keys(data).forEach(date => {
                        let [day, month] = date.split('-');
                        let button = calendars[person].children[day - 1 + month * daysInMonth[month]].querySelector('button');
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
