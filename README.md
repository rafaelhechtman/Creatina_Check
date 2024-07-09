<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bolão Esportivo - Campeonato Brasileiro</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f4;
        }

        header {
            background-color: #333;
            color: white;
            padding: 1em 0;
            text-align: center;
        }

        header h1 {
            margin: 0;
        }

        nav ul {
            list-style: none;
            padding: 0;
            margin: 0;
            display: flex;
            justify-content: center;
        }

        nav ul li {
            margin: 0 1em;
        }

        nav ul li a {
            color: white;
            text-decoration: none;
            cursor: pointer;
        }

        main {
            padding: 1em;
            max-width: 800px;
            margin: auto;
        }

        h2 {
            color: #333;
        }

        .game-list {
            display: flex;
            flex-direction: column;
        }

        .game {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background-color: white;
            padding: 1em;
            margin: 1em 0;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }

        .game .team {
            flex: 1;
            text-align: center;
        }

        .game .details {
            flex: 2;
            text-align: center;
        }

        .bet-btn {
            background-color: #28a745;
            color: white;
            border: none;
            padding: 0.5em 1em;
            cursor: pointer;
            border-radius: 4px;
        }

        .bet-btn:hover {
            background-color: #218838;
        }

        .bet-form {
            background-color: white;
            padding: 1em;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            max-width: 400px;
            margin: auto;
        }

        .bet-form label {
            display: block;
            margin: 1em 0 0.5em;
        }

        .bet-form input, .bet-form select {
            width: 100%;
            padding: 0.5em;
            margin-bottom: 1em;
            border: 1px solid #ccc;
            border-radius: 4px;
        }

        .bet-form button {
            background-color: #28a745;
            color: white;
            border: none;
            padding: 0.5em 1em;
            cursor: pointer;
            border-radius: 4px;
            width: 100%;
        }

        .bet-form button:hover {
            background-color: #218838;
        }

        footer {
            background-color: #333;
            color: white;
            text-align: center;
            padding: 1em 0;
            position: fixed;
            width: 100%;
            bottom: 0;
        }
    </style>
</head>
<body>
    <header>
        <h1>Bolão Esportivo - Campeonato Brasileiro</h1>
        <nav>
            <ul>
                <li><a onclick="showPage('jogos')">Jogos</a></li>
                <li><a onclick="showPage('apostas')">Apostas</a></li>
            </ul>
        </nav>
    </header>
    <main>
        <div id="jogos" class="page">
            <h2>Próximos Jogos</h2>
            <div class="game-list">
                <!-- Exemplo de jogo -->
                <div class="game">
                    <div class="team">
                        <span>Time A</span>
                    </div>
                    <div class="details">
                        <span>vs</span>
                        <span>10/07/2024</span>
                        <span>19:00</span>
                    </div>
                    <div class="team">
                        <span>Time B</span>
                    </div>
                    <button class="bet-btn" onclick="showPage('apostas')">Apostar</button>
                </div>
                <!-- Repita a estrutura do jogo para mais jogos -->
            </div>
        </div>
        <div id="apostas" class="page" style="display: none;">
            <h2>Fazer Apostas</h2>
            <form class="bet-form">
                <label for="game">Jogo:</label>
                <select id="game" name="game">
                    <option value="game1">Time A vs Time B - 10/07/2024</option>
                    <!-- Adicione mais jogos conforme necessário -->
                </select>

                <label for="bet">Aposta:</label>
                <input type="text" id="bet" name="bet" placeholder="Valor da aposta">

                <button type="submit">Enviar Aposta</button>
            </form>
        </div>
    </main>
    <footer>
        <p>&copy; 2024 Bolão Esportivo. Todos os direitos reservados.</p>
    </footer>
    <script>
        function showPage(pageId) {
            document.querySelectorAll('.page').forEach(page => {
                page.style.display = 'none';
            });
            document.getElementById(pageId).style.display = 'block';
        }
    </script>
</body>
</html>
