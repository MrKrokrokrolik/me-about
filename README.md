<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Проверка туннеля...</title>
<style>
  body {
    margin: 0;
    height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    font-family: "Poppins", sans-serif;
    background: radial-gradient(circle at top left, #081229, #152850, #234F8C);
    color: white;
    text-align: center;
  }
  h1 { font-size: 2.3em; animation: glow 3s ease-in-out infinite alternate; }
  @keyframes glow { from { text-shadow: 0 0 15px #00b7ff; } to { text-shadow: 0 0 35px #00d5ff; } }
  .error { color: #ff6b6b; text-shadow: 0 0 15px rgba(255,0,0,0.7); }
  .loader {
    border: 4px solid rgba(255,255,255,0.2);
    border-top: 4px solid #00c3ff;
    border-radius: 50%;
    width: 40px;
    height: 40px;
    animation: spin 1s linear infinite;
    margin: 20px;
  }
  @keyframes spin { 100% { transform: rotate(360deg); } }
</style>
</head>
<body>
  <div class="loader"></div>
  <h1 id="title">Проверка подключения...</h1>
  <p id="desc">Получаем адрес сервера...</p>

<script>
(async () => {
  const title = document.getElementById("title");
  const desc = document.getElementById("desc");

  try {
    // Загружаем JSON с GitHub
    const res = await fetch("https://raw.githubusercontent.com/MrKrokrokrolik/me-about/main/target.json");
    if (!res.ok) throw new Error("Не удалось получить target.json");

    const data = await res.json();
    const tunnelUrl = data.url;
    if (!tunnelUrl) throw new Error("URL туннеля отсутствует");

    desc.textContent = "Проверяем доступность сервера...";

    // Проверяем наличие favicon
    const favicon = tunnelUrl + "/static/favicon.ico?_=" + Date.now();
    const img = new Image();

    img.onload = () => {
      title.textContent = "Сервер доступен!";
      desc.textContent = "Переход на сайт...";
      setTimeout(() => window.location.href = tunnelUrl, 700);
    };

    img.onerror = () => {
      showError("Сервер недоступен или туннель не запущен.");
    };

    img.src = favicon;

  } catch (err) {
    showError("Ошибка: " + err.message);
  }

  function showError(msg) {
    title.textContent = "Ошибка подключения";
    title.classList.add("error");
    desc.textContent = msg;
  }
})();
</script>
</body>
</html>
