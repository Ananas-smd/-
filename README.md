from pathlib import Path
import zipfile

out_dir = Path("/mnt/data/neon_detective_wedding_invite_site")
out_dir.mkdir(exist_ok=True)

html = r"""<!doctype html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0" />
  <title>Неоновое дело — Настя & Илья</title>
  <meta name="description" content="Свадебное приглашение-квест Насти и Ильи" />
  <style>
    :root {
      --bg: #03040a;
      --panel: rgba(7, 10, 22, .84);
      --panel2: rgba(13, 18, 35, .76);
      --text: #eaf4ff;
      --muted: rgba(234, 244, 255, .68);
      --cyan: #00f5ff;
      --pink: #ff2bd6;
      --violet: #8a5cff;
      --green: #50ff9f;
      --yellow: #ffe66d;
      --red: #ff3d6e;
    }

    * {
      box-sizing: border-box;
      -webkit-tap-highlight-color: transparent;
    }

    html, body {
      margin: 0;
      min-height: 100%;
      font-family: Inter, -apple-system, BlinkMacSystemFont, "Segoe UI", Arial, sans-serif;
      color: var(--text);
      background:
        radial-gradient(circle at 12% 8%, rgba(255, 43, 214, .22), transparent 24%),
        radial-gradient(circle at 88% 10%, rgba(0, 245, 255, .20), transparent 26%),
        radial-gradient(circle at 50% 100%, rgba(138, 92, 255, .18), transparent 32%),
        linear-gradient(145deg, #02030a, #080a14 45%, #03040a);
      overflow-x: hidden;
    }

    body::before {
      content: "";
      position: fixed;
      inset: 0;
      background:
        linear-gradient(rgba(0,245,255,.04) 1px, transparent 1px),
        linear-gradient(90deg, rgba(255,43,214,.035) 1px, transparent 1px);
      background-size: 34px 34px;
      mask-image: radial-gradient(circle, black 55%, transparent 100%);
      pointer-events: none;
      z-index: 0;
    }

    body::after {
      content: "";
      position: fixed;
      inset: 0;
      background: repeating-linear-gradient(
        0deg,
        rgba(255,255,255,.035) 0px,
        rgba(255,255,255,.035) 1px,
        transparent 2px,
        transparent 5px
      );
      opacity: .16;
      pointer-events: none;
      z-index: 3;
      mix-blend-mode: screen;
    }

    .wrap {
      position: relative;
      z-index: 1;
      min-height: 100vh;
      display: grid;
      place-items: center;
      padding: 16px;
    }

    .phone {
      width: min(100%, 520px);
      min-height: 690px;
      position: relative;
      border-radius: 34px;
      padding: 18px;
      background:
        linear-gradient(135deg, rgba(0,245,255,.6), rgba(255,43,214,.55), rgba(138,92,255,.5));
      box-shadow:
        0 0 36px rgba(0,245,255,.18),
        0 0 54px rgba(255,43,214,.14),
        0 28px 90px rgba(0,0,0,.62);
    }

    .case {
      min-height: 654px;
      border-radius: 27px;
      padding: 22px;
      background:
        linear-gradient(180deg, rgba(8,10,25,.96), rgba(3,4,10,.98)),
        radial-gradient(circle at top, rgba(255,255,255,.08), transparent 40%);
      border: 1px solid rgba(255,255,255,.12);
      overflow: hidden;
      position: relative;
    }

    .case::before {
      content: "";
      position: absolute;
      inset: 0;
      background:
        radial-gradient(circle at 20% 20%, rgba(0,245,255,.11), transparent 34%),
        radial-gradient(circle at 80% 0%, rgba(255,43,214,.11), transparent 36%);
      pointer-events: none;
    }

    .screen {
      position: relative;
      z-index: 2;
      min-height: 610px;
      display: flex;
      flex-direction: column;
      animation: enter .38s ease both;
    }

    .topline {
      display: flex;
      justify-content: space-between;
      gap: 10px;
      align-items: center;
      margin-bottom: 18px;
      font-size: 11px;
      text-transform: uppercase;
      letter-spacing: 1.8px;
      color: var(--muted);
    }

    .status {
      padding: 6px 9px;
      border-radius: 999px;
      border: 1px solid rgba(0,245,255,.28);
      color: var(--cyan);
      box-shadow: 0 0 16px rgba(0,245,255,.14);
      white-space: nowrap;
    }

    h1, h2 {
      margin: 0 0 14px;
      line-height: 1.02;
      letter-spacing: -.8px;
    }

    h1 {
      font-size: clamp(40px, 11vw, 58px);
      text-shadow:
        0 0 18px rgba(0,245,255,.42),
        0 0 24px rgba(255,43,214,.24);
    }

    h2 {
      font-size: clamp(29px, 8vw, 40px);
    }

    .neon {
      color: var(--cyan);
      text-shadow: 0 0 14px rgba(0,245,255,.62);
    }

    .pink {
      color: var(--pink);
      text-shadow: 0 0 14px rgba(255,43,214,.52);
    }

    p {
      margin: 8px 0;
      font-size: 16.5px;
      line-height: 1.55;
      color: rgba(234,244,255,.82);
    }

    b { color: white; }

    .panel {
      margin: 16px 0;
      padding: 16px;
      border-radius: 20px;
      background: var(--panel);
      border: 1px solid rgba(255,255,255,.12);
      box-shadow: inset 0 0 28px rgba(0,245,255,.04);
    }

    .glitch {
      position: relative;
      display: inline-block;
    }

    .glitch::before,
    .glitch::after {
      content: attr(data-text);
      position: absolute;
      left: 0;
      top: 0;
      opacity: .55;
      pointer-events: none;
    }

    .glitch::before {
      transform: translate(2px, 0);
      color: var(--pink);
      clip-path: inset(0 0 55% 0);
    }

    .glitch::after {
      transform: translate(-2px, 0);
      color: var(--cyan);
      clip-path: inset(48% 0 0 0);
    }

    .buttons {
      display: grid;
      gap: 12px;
      margin-top: auto;
      padding-top: 24px;
    }

    button, .button {
      border: 0;
      border-radius: 17px;
      min-height: 54px;
      padding: 14px 16px;
      color: #02030a;
      background: linear-gradient(135deg, var(--cyan), #a9ffff);
      font-size: 16px;
      font-weight: 850;
      cursor: pointer;
      text-align: center;
      text-decoration: none;
      box-shadow:
        0 0 22px rgba(0,245,255,.23),
        0 12px 24px rgba(0,0,0,.34);
      transition: transform .16s ease, opacity .16s ease, filter .16s ease;
    }

    button:active {
      transform: scale(.985);
      opacity: .84;
    }

    .secondary {
      color: var(--text);
      background: rgba(255,255,255,.07);
      border: 1px solid rgba(255,255,255,.16);
      box-shadow: none;
    }

    .pinkbtn {
      background: linear-gradient(135deg, var(--pink), #ffc1f2);
      box-shadow: 0 0 22px rgba(255,43,214,.24), 0 12px 24px rgba(0,0,0,.34);
    }

    .choice-grid {
      display: grid;
      gap: 10px;
      margin-top: 18px;
    }

    .choice {
      color: var(--text);
      background: rgba(255,255,255,.07);
      border: 1px solid rgba(255,255,255,.14);
      box-shadow: none;
      text-align: left;
      font-weight: 750;
    }

    .choice.good {
      border-color: rgba(80,255,159,.55);
      box-shadow: 0 0 18px rgba(80,255,159,.12);
    }

    .input-row {
      display: grid;
      gap: 10px;
      margin-top: 18px;
    }

    input {
      width: 100%;
      min-height: 54px;
      border-radius: 16px;
      padding: 0 15px;
      color: white;
      background: rgba(255,255,255,.07);
      border: 1px solid rgba(0,245,255,.28);
      outline: none;
      font-size: 18px;
      text-align: center;
      letter-spacing: 4px;
      text-transform: uppercase;
      box-shadow: inset 0 0 20px rgba(0,245,255,.06);
    }

    input:focus {
      border-color: var(--cyan);
      box-shadow: 0 0 18px rgba(0,245,255,.18), inset 0 0 20px rgba(0,245,255,.06);
    }

    .toast {
      margin-top: 14px;
      min-height: 54px;
      padding: 12px 14px;
      border-radius: 16px;
      color: rgba(234,244,255,.86);
      background: rgba(255,255,255,.06);
      border: 1px solid rgba(255,255,255,.1);
      font-size: 15px;
      line-height: 1.35;
    }

    .progress {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 8px;
      margin: 10px 0 18px;
    }

    .step {
      height: 7px;
      border-radius: 999px;
      background: rgba(255,255,255,.11);
      overflow: hidden;
    }

    .step.active {
      background: linear-gradient(90deg, var(--cyan), var(--pink));
      box-shadow: 0 0 14px rgba(0,245,255,.35);
    }

    .cipher {
      display: grid;
      gap: 8px;
      margin-top: 16px;
      font-family: "SFMono-Regular", Consolas, monospace;
    }

    .cipher-row {
      display: grid;
      grid-template-columns: 1fr auto;
      gap: 10px;
      align-items: center;
      padding: 10px 12px;
      border-radius: 14px;
      background: rgba(255,255,255,.06);
      border: 1px solid rgba(255,255,255,.11);
    }

    .tag {
      color: var(--yellow);
      font-size: 14px;
    }

    .map {
      position: relative;
      height: 230px;
      margin: 18px 0;
      border-radius: 22px;
      background:
        radial-gradient(circle at 20% 30%, rgba(0,245,255,.22), transparent 10%),
        radial-gradient(circle at 76% 72%, rgba(255,43,214,.24), transparent 12%),
        linear-gradient(135deg, rgba(255,255,255,.08), rgba(255,255,255,.03));
      border: 1px solid rgba(255,255,255,.13);
      overflow: hidden;
    }

    .map::before {
      content: "";
      position: absolute;
      inset: 0;
      background:
        linear-gradient(35deg, transparent 48%, rgba(255,255,255,.12) 49%, transparent 50%),
        linear-gradient(120deg, transparent 40%, rgba(255,255,255,.08) 41%, transparent 42%),
        linear-gradient(75deg, transparent 60%, rgba(255,255,255,.09) 61%, transparent 62%);
    }

    .pin {
      position: absolute;
      width: 46px;
      height: 46px;
      border-radius: 50% 50% 50% 5px;
      transform: rotate(-45deg);
      display: grid;
      place-items: center;
      cursor: pointer;
      border: 2px solid rgba(255,255,255,.75);
    }

    .pin span {
      transform: rotate(45deg);
      font-weight: 900;
      font-family: Arial, sans-serif;
    }

    .pin.zags {
      left: 22%;
      top: 28%;
      background: var(--cyan);
      color: #02030a;
      box-shadow: 0 0 22px rgba(0,245,255,.5);
    }

    .pin.cafe {
      right: 18%;
      bottom: 18%;
      background: var(--pink);
      color: #02030a;
      box-shadow: 0 0 22px rgba(255,43,214,.5);
    }

    .pin.fake {
      left: 52%;
      top: 20%;
      background: rgba(255,255,255,.17);
      color: white;
      box-shadow: none;
    }

    .final-card {
      display: grid;
      gap: 12px;
      margin: 16px 0;
    }

    .info {
      padding: 15px;
      border-radius: 18px;
      background: rgba(255,255,255,.07);
      border: 1px solid rgba(255,255,255,.12);
    }

    .info .label {
      font-size: 12px;
      text-transform: uppercase;
      letter-spacing: 1.4px;
      color: var(--muted);
      margin-bottom: 5px;
    }

    .info .value {
      font-size: 18px;
      font-weight: 850;
    }

    .unlock {
      color: var(--green);
      text-shadow: 0 0 14px rgba(80,255,159,.42);
    }

    .small {
      font-size: 13px;
      color: rgba(234,244,255,.52);
      text-align: center;
      margin-top: 14px;
    }

    @keyframes enter {
      from { opacity: 0; transform: translateY(8px) scale(.99); filter: blur(2px); }
      to { opacity: 1; transform: translateY(0) scale(1); filter: blur(0); }
    }

    @media (max-width: 380px) {
      .wrap { padding: 8px; }
      .phone { padding: 10px; border-radius: 26px; }
      .case { padding: 18px; border-radius: 22px; min-height: 630px; }
      .screen { min-height: 590px; }
      p { font-size: 15.8px; }
      .map { height: 210px; }
    }
  </style>
</head>

<body>
  <div class="wrap">
    <main class="phone">
      <section class="case">
        <div id="screen" class="screen"></div>
      </section>
    </main>
  </div>

  <script>
    const DATA = {
      bride: "Настя",
      groom: "Илья",

      // ЗАМЕНИТЬ НА ВАШИ ДАННЫЕ
      date: "27.10.2026",
      timeZags: "14:30",
      zags: "ЗАГС: [вставить адрес ЗАГСа]",
      timeCafe: "17:00",
      cafe: "Кафе: [вставить название и адрес кафе]",

      telegramUsername: "your_username"
    };

    const tg = window.Telegram?.WebApp;
    if (tg) {
      tg.ready();
      tg.expand();
    }

    const screen = document.getElementById("screen");
    const state = {
      puzzle: 0,
      dateUnlocked: false,
      timeUnlocked: false,
      zagsUnlocked: false,
      cafeUnlocked: false
    };

    function render(html) {
      screen.innerHTML = html;
      screen.style.animation = "none";
      requestAnimationFrame(() => screen.style.animation = "enter .38s ease both");
    }

    function progress() {
      const n = [
        state.dateUnlocked,
        state.timeUnlocked,
        state.zagsUnlocked,
        state.cafeUnlocked
      ].filter(Boolean).length;

      return `
        <div class="progress">
          <div class="step ${n >= 1 ? "active" : ""}"></div>
          <div class="step ${n >= 2 ? "active" : ""}"></div>
          <div class="step ${n >= 3 ? "active" : ""}"></div>
          <div class="step ${n >= 4 ? "active" : ""}"></div>
        </div>
      `;
    }

    function top(label = "classified") {
      return `
        <div class="topline">
          <span>NEON DETECTIVE FILE</span>
          <span class="status">${label}</span>
        </div>
      `;
    }

    function start() {
      render(`
        ${top("locked")}
        <h1><span class="glitch" data-text="Секретное дело">Секретное дело</span></h1>
        <div class="panel">
          <p><b>Фигуранты:</b> ${DATA.bride} и ${DATA.groom}</p>
          <p><b>Статус:</b> готовят событие, которое нельзя пропустить.</p>
          <p><b>Доступ:</b> только для приглашённых свидетелей.</p>
        </div>
        <p>Чтобы узнать дату, время, место ЗАГСа и кафе, нужно пройти несколько головоломок.</p>
        <p>Каждая разгаданная загадка откроет часть приглашения.</p>
        <div class="buttons">
          <button onclick="puzzleDate()">Начать расследование</button>
        </div>
      `);
    }

    function puzzleDate(message = "") {
      render(`
        ${top("puzzle 01")}
        <h2>Головоломка 1: дата</h2>
        ${progress()}
        <p>Следователь нашёл зашифрованную дату. Разгадайте код.</p>

        <div class="panel">
          <p><b>Подсказка:</b> день = 3³, месяц = 10.</p>
          <p>Формат ответа: <b>ДД.ММ.ГГГГ</b></p>
        </div>

        <div class="input-row">
          <input id="dateInput" placeholder="__.__.____" maxlength="10" inputmode="numeric" />
          <button onclick="checkDate()">Проверить дату</button>
        </div>

        <div class="toast">${message || "Введите предполагаемую дату."}</div>

        <div class="buttons">
          <button class="secondary" onclick="dateHint()">Подсказка</button>
        </div>
      `);
    }

    function dateHint() {
      puzzleDate("3³ = 27. Месяц 10 — это октябрь. Осталось собрать дату полностью.");
    }

    function checkDate() {
      const value = document.getElementById("dateInput").value.trim();
      const normalized = value.replaceAll("/", ".").replaceAll("-", ".");
      if (normalized === DATA.date) {
        state.dateUnlocked = true;
        puzzleTime();
      } else {
        puzzleDate("Почти. Попробуйте ещё раз: день — 27, месяц — 10.");
      }
    }

    function puzzleTime(message = "") {
      render(`
        ${top("puzzle 02")}
        <h2>Головоломка 2: время</h2>
        ${progress()}
        <p>Время спрятано в протоколе. Нужно выбрать верную пару.</p>

        <div class="cipher">
          <div class="cipher-row"><span>14:30</span><span class="tag">код: ЗАГС</span></div>
          <div class="cipher-row"><span>17:00</span><span class="tag">код: КАФЕ</span></div>
          <div class="cipher-row"><span>03:15</span><span class="tag">код: ложный след</span></div>
        </div>

        <p>Какой вариант подходит для официальной церемонии?</p>

        <div class="choice-grid">
          <button class="choice" onclick="wrongTime()">03:15 — подозрительно драматично</button>
          <button class="choice good" onclick="rightTime()">14:30 — церемония в ЗАГСе</button>
          <button class="choice" onclick="wrongTime()">17:00 — уже празднуем в кафе</button>
        </div>

        <div class="toast">${message || "Выберите правильное время церемонии."}</div>
      `);
    }

    function wrongTime() {
      puzzleTime("Это ложный след. Церемония должна быть раньше кафе.");
    }

    function rightTime() {
      state.timeUnlocked = true;
      puzzleMap();
    }

    function puzzleMap(message = "") {
      render(`
        ${top("puzzle 03")}
        <h2>Головоломка 3: карта</h2>
        ${progress()}
        <p>На неоновой карте отмечены три точки. Две из них настоящие.</p>
        <p>Найдите сначала ЗАГС, потом кафе.</p>

        <div class="map">
          <button class="pin zags" onclick="pickZags()"><span>З</span></button>
          <button class="pin fake" onclick="fakePin()"><span>?</span></button>
          <button class="pin cafe" onclick="pickCafe()"><span>К</span></button>
        </div>

        <div class="toast">${message || "Нажмите на точку «З», чтобы открыть место ЗАГСа."}</div>

        <div class="buttons">
          ${state.zagsUnlocked && state.cafeUnlocked ? `<button onclick="final()">Открыть финальное досье</button>` : ``}
        </div>
      `);
    }

    function pickZags() {
      state.zagsUnlocked = true;
      puzzleMap(`ЗАГС открыт: ${DATA.zags}. Теперь найдите кафе — точку «К».`);
    }

    function pickCafe() {
      if (!state.zagsUnlocked) {
        puzzleMap("Сначала нужно открыть ЗАГС. Следствие любит порядок 😄");
        return;
      }
      state.cafeUnlocked = true;
      puzzleMap(`Кафе открыто: ${DATA.cafe}. Все данные собраны.`);
    }

    function fakePin() {
      puzzleMap("Ложный след. Там только подозрительный голубь и кофе навынос.");
    }

    function final() {
      render(`
        ${top("unlocked")}
        <h2><span class="unlock">Дело раскрыто</span></h2>
        ${progress()}
        <p>Все головоломки решены. Доступ к свадебному досье открыт.</p>

        <div class="final-card">
          <div class="info">
            <div class="label">Фигуранты дела</div>
            <div class="value">${DATA.bride} & ${DATA.groom}</div>
          </div>
          <div class="info">
            <div class="label">Дата</div>
            <div class="value">${DATA.date}</div>
          </div>
          <div class="info">
            <div class="label">Церемония</div>
            <div class="value">${DATA.timeZags} — ${DATA.zags}</div>
          </div>
          <div class="info">
            <div class="label">Праздничный ужин</div>
            <div class="value">${DATA.timeCafe} — ${DATA.cafe}</div>
          </div>
        </div>

        <p>Настя и Илья приглашают вас стать свидетелем этого события. Без вас дело будет неполным.</p>

        <div class="buttons">
          <button onclick="answer()">Передать ответ</button>
          <button class="secondary" onclick="start()">Пройти заново</button>
        </div>
      `);
    }

    function answer() {
      render(`
        ${top("witness protocol")}
        <h2>Ваш ответ</h2>
        <p>Передайте протокол свидетеля Насте и Илье.</p>

        <div class="buttons">
          <button onclick="sendAnswer('yes')">Буду на месте события ❤️</button>
          <button class="secondary" onclick="sendAnswer('no')">К сожалению, не смогу</button>
          <button class="secondary" onclick="final()">Назад к досье</button>
        </div>

        <p class="small">Если Telegram не откроется, текст ответа будет скопирован.</p>
      `);
    }

    function sendAnswer(type) {
      const text = type === "yes"
        ? `${DATA.bride} и ${DATA.groom}, привет! ❤️ Я буду на свадьбе. Дело раскрыто, протокол свидетеля подтверждаю!`
        : `${DATA.bride} и ${DATA.groom}, привет! К сожалению, не смогу быть на свадьбе, но очень вас поздравляю ❤️`;

      if (tg?.sendData) {
        tg.sendData(JSON.stringify({ answer: type, text }));
        tg.close();
        return;
      }

      if (DATA.telegramUsername && DATA.telegramUsername !== "your_username") {
        const clean = DATA.telegramUsername.replace("@", "");
        window.open(`https://t.me/${clean}?text=${encodeURIComponent(text)}`, "_blank");
        return;
      }

      navigator.clipboard?.writeText(text);
      render(`
        ${top("copied")}
        <h2>Ответ подготовлен</h2>
        <p>Текст скопирован. Его можно отправить Насте или Илье:</p>
        <div class="panel"><p>${text}</p></div>
        <div class="buttons">
          <button onclick="final()">Вернуться к приглашению</button>
        </div>
      `);
    }

    start();
  </script>
</body>
</html>
"""

(out_dir / "index.html").write_text(html, encoding="utf-8")

readme = """# Неоновое детективное свадебное приглашение

Готовый сайт-квест для Насти и Ильи.

## Что внутри

Гости проходят 3 головоломки:
1. Разгадывают дату.
2. Выбирают правильное время церемонии.
3. Находят точки ЗАГСа и кафе на карте.

После этого открывается финальное свадебное досье.

## Что заменить в index.html

Найдите блок:

```js
const DATA = {
  bride: "Настя",
  groom: "Илья",
  date: "27.10.2026",
  timeZags: "14:30",
  zags: "ЗАГС: [вставить адрес ЗАГСа]",
  timeCafe: "17:00",
  cafe: "Кафе: [вставить название и адрес кафе]",
  telegramUsername: "your_username"
};
```

Поставьте свои данные:
- дату;
- время ЗАГСа;
- адрес ЗАГСа;
- время кафе;
- название и адрес кафе;
- Telegram username.

## Как открыть

Можно просто открыть `index.html` в браузере.

## Как выложить в интернет

Самый простой вариант:
1. Зайти на https://app.netlify.com/drop
2. Перетащить туда `index.html`.
3. Получить ссылку.
4. Отправить ссылку гостям.
"""

(out_dir / "README.md").write_text(readme, encoding="utf-8")

zip_path = Path("/mnt/data/neon_detective_wedding_invite_site.zip")
with zipfile.ZipFile(zip_path, "w", zipfile.ZIP_DEFLATED) as z:
    z.write(out_dir / "index.html", arcname="index.html")
    z.write(out_dir / "README.md", arcname="README.md")

zip_path.as_posix()
