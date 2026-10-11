<!doctype html>
<html lang="fr">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <meta name="description" content="Planning du jour premium, hour by hour, avec rappel automatique et impression." />
  <title>Planning premium</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=Bricolage+Grotesque:wght@600;700;800&display=swap">
  <style>
    :root {
      --bg: #eef5fb;
      --bg-2: #f9fbff;
      --panel: rgba(255,255,255,0.84);
      --panel-strong: #ffffff;
      --ink: #163042;
      --muted: #5e7282;
      --line: #dfeaf1;
      --accent: #1f6f8f;
      --accent-2: #2b84a9;
      --accent-soft: #e3f3fb;
      --success: #2e8f67;
      --warning: #f7d279;
      --danger: #d64b4b;
      --shadow: 0 18px 40px rgba(17, 40, 56, 0.12);
      --radius: 18px;
      --font-display: "Bricolage Grotesque", "Inter", sans-serif;
      --font-body: "Inter", sans-serif;
    }

    :root[data-theme="dark"] {
      --bg: #0f1b28;
      --bg-2: #152533;
      --panel: rgba(19, 31, 42, 0.9);
      --panel-strong: #192d3d;
      --ink: #edf6ff;
      --muted: #a2b6c7;
      --line: #2a4257;
      --accent: #71c2df;
      --accent-2: #93d8f0;
      --accent-soft: rgba(113,194,223,0.18);
      --success: #7ed6a8;
      --warning: #edc77b;
      --danger: #ff8e8e;
      --shadow: 0 22px 48px rgba(0,0,0,0.25);
      color-scheme: dark;
    }

    * { box-sizing: border-box; }
    html { scroll-behavior: smooth; }
    body {
      margin: 0;
      min-height: 100vh;
      font-family: var(--font-body);
      background:
        radial-gradient(circle at top left, rgba(31, 111, 143, 0.12), transparent 30%),
        linear-gradient(180deg, var(--bg), var(--bg-2));
      color: var(--ink);
      padding: 28px 16px 52px;
    }
    [hidden] { display: none !important; }

    .wrap {
      max-width: 1160px;
      margin: 0 auto;
      display: flex;
      flex-direction: column;
      gap: 18px;
    }

    .top {
      background: var(--panel);
      border: 1px solid var(--line);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      padding: 20px 18px 18px;
      backdrop-filter: blur(10px);
    }

    header {
      display: flex;
      flex-wrap: wrap;
      align-items: flex-end;
      justify-content: space-between;
      gap: 18px;
    }

    h1 {
      margin: 0;
      font-family: var(--font-display);
      letter-spacing: -0.04em;
      font-size: clamp(30px, 5vw, 44px);
      line-height: 1;
    }

    .dateline {
      margin: 6px 0 0;
      font-size: .96rem;
      text-transform: capitalize;
      color: var(--muted);
    }

    .controls {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
      align-items: center;
    }

    button, input, select {
      font: inherit;
      color: inherit;
    }

    .btn {
      appearance: none;
      border: 1px solid var(--line);
      background: var(--panel-strong);
      color: var(--ink);
      border-radius: 12px;
      padding: 9px 13px;
      min-height: 40px;
      cursor: pointer;
      transition: transform .15s ease, border-color .2s ease, box-shadow .2s ease;
    }

    .btn:hover {
      border-color: var(--accent);
      box-shadow: 0 10px 20px rgba(31, 111, 143, 0.08);
    }

    .btn:active { transform: translateY(1px); }

    .btn.primary {
      background: linear-gradient(135deg, var(--accent), var(--accent-2));
      color: white;
      border-color: transparent;
      font-weight: 700;
    }

    .btn.soft {
      background: var(--accent-soft);
      border-color: transparent;
      color: var(--accent);
      font-weight: 700;
    }

    .icon-btn {
      width: 40px;
      min-width: 40px;
      padding: 0;
      font-size: 1.05rem;
    }

    input[type="date"] {
      border: 1px solid var(--line);
      background: var(--panel-strong);
      min-height: 40px;
      border-radius: 12px;
      padding: 8px 10px;
    }

    .summary {
      display: flex;
      flex-wrap: wrap;
      align-items: center;
      gap: 12px 18px;
      justify-content: space-between;
      padding-top: 14px;
      color: var(--muted);
      font-size: 0.94rem;
    }

    .stats {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      align-items: center;
      flex: 1 1 auto;
    }

    .chip {
      background: var(--panel-strong);
      border: 1px solid var(--line);
      border-radius: 12px;
      padding: 8px 12px;
      font-weight: 600;
      color: var(--ink);
    }

    .bar {
      flex: 1 1 240px;
      max-width: 360px;
      height: 10px;
      background: var(--line);
      border-radius: 999px;
      overflow: hidden;
      box-shadow: inset 0 1px 2px rgba(0,0,0,0.04);
    }

    .bar i {
      display: block;
      height: 100%;
      width: 0;
      background: linear-gradient(90deg, var(--success), #7ad1bc);
      transition: width .25s ease;
    }

    .remind {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      padding: 8px 12px;
      border-radius: 12px;
      background: var(--panel-strong);
      border: 1px solid var(--line);
      color: var(--ink);
      cursor: pointer;
      min-height: 40px;
      user-select: none;
    }

    .remind input {
      width: 18px;
      height: 18px;
      accent-color: var(--accent);
      margin: 0;
    }

    .alert {
      display: flex;
      flex-wrap: wrap;
      align-items: center;
      justify-content: space-between;
      gap: 10px 18px;
      background: linear-gradient(135deg, var(--accent), var(--accent-2));
      color: white;
      border-radius: 14px;
      padding: 12px 14px;
      font-weight: 700;
      box-shadow: var(--shadow);
    }

    .alert .btn { color: var(--ink); }

    .sheet {
      background: var(--panel);
      border: 1px solid var(--line);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      overflow: hidden;
    }

    .head, .row {
      display: grid;
      grid-template-columns: 96px minmax(0,1.5fr) minmax(0,1.25fr) 120px 76px minmax(0,1.2fr);
      gap: 0 10px;
      align-items: center;
      padding: 8px 14px;
    }

    .head {
      background: var(--accent-soft);
      color: var(--muted);
      font-size: 11px;
      font-weight: 800;
      text-transform: uppercase;
      letter-spacing: 0.08em;
      padding-block: 12px;
    }

    .row {
      border-top: 1px solid var(--line);
      transition: background .2s ease;
    }

    .row.now {
      background: rgba(247, 210, 121, 0.18);
    }

    .time {
      font-family: var(--font-display);
      font-size: 17px;
      font-weight: 800;
      font-variant-numeric: tabular-nums;
      display: flex;
      align-items: baseline;
      gap: 6px;
    }

    .time small {
      font-family: var(--font-body);
      font-size: 11px;
      color: var(--muted);
      font-weight: 600;
    }

    .row input[type="text"], .row select {
      width: 100%;
      border: 1px solid transparent;
      background: transparent;
      border-radius: 8px;
      padding: 8px 9px;
      min-height: 38px;
      color: var(--ink);
    }

    .row input[type="text"]:hover, .row select:hover {
      border-color: var(--line);
    }

    .row input[type="text"]:focus, .row select:focus {
      border-color: var(--accent);
      background: var(--panel-strong);
    }

    .row select { appearance: auto; cursor: pointer; }
    .row select[data-p="Haute"] { color: var(--danger); font-weight:700; }
    .row select[data-p="Moyenne"] { color: var(--warning); font-weight:700; }
    .row select[data-p="Basse"] { color: var(--success); font-weight:700; }

    .done {
      display: flex;
      align-items: center;
      gap: 6px;
      min-height: 38px;
      user-select: none;
      cursor: pointer;
      padding-inline: 4px;
    }

    .done input {
      width: 20px;
      height: 20px;
      accent-color: var(--success);
      margin: 0;
      cursor: pointer;
    }

    .row.is-done .a {
      text-decoration: line-through;
      color: var(--muted);
    }

    .foot {
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      gap: 10px 14px;
      align-items: center;
      color: var(--muted);
      font-size: 13px;
      padding-top: 4px;
    }

    .status {
      min-height: 1.2em;
      font-weight: 700;
    }

    .actions {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
    }

    @media (max-width: 780px) {
      .head { display: none; }
      .row {
        grid-template-columns: 1fr auto auto;
        grid-template-areas:
          "time p d"
          "a a a"
          "l l l"
          "n n n";
        gap: 6px 8px;
        padding: 12px 14px;
      }
      .time { grid-area: time; }
      .row .p { grid-area: p; width: auto; }
      .row .d { grid-area: d; }
      .row .a { grid-area: a; }
      .row .l { grid-area: l; }
      .row .n { grid-area: n; }
      .row input[type="text"], .row select { border-color: var(--line); }
      .stats { width: 100%; }
      .bar { max-width: none; }
      .summary { align-items: stretch; }
      .remind { flex: 1 1 100%; justify-content: center; }
    }

    @media (prefers-reduced-motion: reduce) {
      html { scroll-behavior: auto; }
      .bar i, .btn { transition: none; }
    }

    @media print {
      body {
        background: white;
        padding: 0;
      }
      .alert, .controls, .remind, .actions, .summary .chip, .summary .bar, #themeToggle { display: none !important; }
      .top, .sheet { box-shadow: none; border-color: #ccc; }
      .wrap { max-width: 100%; }
    }
  </style>
</head>
<body>
  <div class="wrap">
    <div class="top">
      <header>
        <div>
          <h1>Planning du jour</h1>
          <p class="dateline" id="dateline"></p>
        </div>
        <div class="controls">
          <button type="button" class="btn icon-btn" id="prev" aria-label="Jour précédent">‹</button>
          <input type="date" id="date" aria-label="Choisir le jour">
          <button type="button" class="btn icon-btn" id="next" aria-label="Jour suivant">›</button>
          <button type="button" class="btn primary" id="today">Aujourd’hui</button>
          <button type="button" class="btn" id="themeToggle" aria-label="Changer de thème">🌙</button>
          <button type="button" class="btn soft" id="printBtn">Imprimer</button>
        </div>
      </header>

      <div class="summary">
        <div class="stats">
          <span class="chip" id="count">0 tâche</span>
          <span class="chip" id="nextWindow">Prochain créneau : —</span>
        </div>
        <div class="bar" aria-hidden="true"><i id="barfill"></i></div>
        <label class="remind"><input type="checkbox" id="remind"> Rappel à chaque changement d’heure</label>
      </div>
    </div>

    <div class="alert" id="alert" role="alert" hidden>
      <span id="alerttext"></span>
      <button type="button" class="btn" id="alertok">OK</button>
    </div>

    <div class="sheet">
      <div class="head" aria-hidden="true">
        <span>Heure</span>
        <span>Activité / tâche</span>
        <span>Lieu / avec qui</span>
        <span>Priorité</span>
        <span>Fait</span>
        <span>Notes</span>
      </div>
      <div id="rows"></div>
    </div>

    <div class="foot">
      <span class="status" id="status" role="status" aria-live="polite"></span>
      <div class="actions">
        <button type="button" class="btn soft" id="addSlot">+ Ajouter un créneau</button>
        <button type="button" class="btn" id="removeSlot">− Retirer le dernier</button>
        <button type="button" class="btn" id="clear">Vider la journée</button>
      </div>
    </div>
  </div>

  <script>
    (function () {
      var START = 6;
      var END = 23;
      var dateKey = keyOf(new Date());
      var lastHour = new Date().getHours();
      var reminderEnabled = localStorage.getItem('planning:remind') === '1';
      var audioCtx = null;
      var wakeLock = null;

      function pad(n) {
        return String(n).padStart(2, '0');
      }

      function keyOf(date) {
        return date.getFullYear() + '-' + pad(date.getMonth() + 1) + '-' + pad(date.getDate());
      }

      function parseKey(key) {
        var p = key.split('-');
        return new Date(Number(p[0]), Number(p[1]) - 1, Number(p[2]));
      }

      function byId(id) {
        return document.getElementById(id);
      }

      function buildRows() {
        var rowsEl = byId('rows');
        rowsEl.innerHTML = '';

        for (var h = START; h <= END; h++) {
          var row = document.createElement('div');
          row.className = 'row';
          row.dataset.h = String(h);

          var options = '<option value="">Priorité</option>' + ['Haute', 'Moyenne', 'Basse'].map(function (p) {
            return '<option value="' + p + '">' + p + '</option>';
          }).join('');

          row.innerHTML =
            '<div class="time">' + pad(h) + 'h<small>→ ' + pad((h + 1) % 24) + 'h</small></div>' +
            '<input type="text" class="a" id="a-' + h + '" aria-label="Activité de ' + pad(h) + 'h" placeholder="' + (h === 9 ? 'ex. Réunion d’équipe' : 'Activité / tâche') + '" autocomplete="off">' +
            '<input type="text" class="l" id="l-' + h + '" aria-label="Lieu ou personnes à ' + pad(h) + 'h" placeholder="' + (h === 9 ? 'ex. Salle B, avec Marie' : 'Lieu / avec qui') + '" autocomplete="off">' +
            '<select class="p" id="p-' + h + '" aria-label="Priorité à ' + pad(h) + 'h">' + options + '</select>' +
            '<label class="d done"><input type="checkbox" id="d-' + h + '" aria-label="Fait à ' + pad(h) + 'h"><span>Fait</span></label>' +
            '<input type="text" class="n" id="n-' + h + '" aria-label="Notes à ' + pad(h) + 'h" placeholder="Notes" autocomplete="off">';

          rowsEl.appendChild(row);
        }
      }

      function refreshVisuals() {
        var total = 0;
        var done = 0;
        var nextText = 'Prochain créneau : —';

        for (var h = START; h <= END; h++) {
          var row = byId('rows').children[h - START];
          var isDone = byId('d-' + h).checked;
          if (!row) continue;

          row.classList.toggle('is-done', isDone);
          byId('p-' + h).dataset.p = byId('p-' + h).value;

          if (byId('a-' + h).value.trim() || isDone) {
            total += 1;
            if (isDone) done += 1;
          }

          if (!byId('a-' + h).value.trim() && !isDone) {
            if (nextText === 'Prochain créneau : —') {
              nextText = 'Prochain créneau : ' + pad(h) + 'h';
            }
          }
        }

        byId('count').textContent = total === 0 ? '0 tâche' : done + ' / ' + total + ' faites';
        byId('barfill').style.width = total ? Math.round((done / total) * 100) + '%' : '0%';
        byId('nextWindow').textContent = nextText;
      }

      function getData() {
        var out = {};
        for (var h = START; h <= END; h++) {
          var item = {
            a: byId('a-' + h).value.trim(),
            l: byId('l-' + h).value.trim(),
            p: byId('p-' + h).value,
            d: byId('d-' + h).checked,
            n: byId('n-' + h).value.trim()
          };
          if (item.a || item.l || item.p || item.d || item.n) {
            out['h' + h] = item;
          }
        }
        return out;
      }

      function setData(data) {
        data = data || {};
        for (var h = START; h <= END; h++) {
          var item = data['h' + h] || {};
          byId('a-' + h).value = item.a || '';
          byId('l-' + h).value = item.l || '';
          byId('p-' + h).value = item.p || '';
          byId('d-' + h).checked = !!item.d;
          byId('n-' + h).value = item.n || '';
        }
        refreshVisuals();
      }

      function showDate() {
        byId('date').value = dateKey;
        byId('dateline').textContent = parseKey(dateKey).toLocaleDateString('fr-FR', {
          weekday: 'long',
          day: 'numeric',
          month: 'long',
          year: 'numeric'
        });
        markNow();
      }

      function markNow() {
        var now = new Date();
        var isToday = keyOf(now) === dateKey;
        var rows = byId('rows').children;

        for (var h = START; h <= END; h++) {
          if (rows[h - START]) {
            rows[h - START].classList.toggle('now', isToday && now.getHours() === h);
          }
        }
      }

      function setStatus(message) {
        byId('status').textContent = message || '';
      }

      function storageKey(dayKey) {
        return 'planning:' + dayKey;
      }

      function loadDay(dayKeyToLoad) {
        try {
          var raw = localStorage.getItem(storageKey(dayKeyToLoad));
          return raw ? JSON.parse(raw) : {};
        } catch (e) {
          return {};
        }
      }

      function saveDay(dayKeyToSave, data) {
        try {
          localStorage.setItem(storageKey(dayKeyToSave), JSON.stringify(data));
        } catch (e) {
          // ignore localStorage limits
        }
      }

      function persistNow() {
        setStatus('Enregistré');
        saveDay(dateKey, getData());
      }

      function onInputChange() {
        refreshVisuals();
        setStatus('Modification en cours…');
        clearTimeout(window._saveTimer);
        window._saveTimer = setTimeout(persistNow, 300);
      }

      function goTo(key) {
        dateKey = key;
        showDate();
        setData(loadDay(key));
        setStatus('');
      }

      function addSlot() {
        if (END >= 24) {
          setStatus('Plage maximale atteinte');
          return;
        }
        END += 1;
        buildRows();
        setData(loadDay(dateKey));
        refreshVisuals();
      }

      function removeSlot() {
        if (END <= START) {
          setStatus('Impossible de supprimer');
          return;
        }
        END -= 1;
        buildRows();
        setData(loadDay(dateKey));
        refreshVisuals();
      }

      function ensureAudio() {
        try {
          if (!audioCtx) {
            var AudioCtor = window.AudioContext || window.webkitAudioContext;
            if (AudioCtor) audioCtx = new AudioCtor();
          }
          if (audioCtx && audioCtx.state === 'suspended') {
            audioCtx.resume();
          }
        } catch (e) {}
      }

      function beep() {
        try {
          if (!audioCtx) return;
          var t = audioCtx.currentTime;
          [0, 0.25, 0.5].forEach(function (offset, index) {
            var osc = audioCtx.createOscillator();
            var gain = audioCtx.createGain();
            osc.frequency.value = index === 2 ? 988 : 784;
            osc.connect(gain);
            gain.connect(audioCtx.destination);
            gain.gain.setValueAtTime(0.0001, t + offset);
            gain.gain.exponentialRampToValueAtTime(0.25, t + offset + 0.02);
            gain.gain.exponentialRampToValueAtTime(0.0001, t + offset + 0.2);
            osc.start(t + offset);
            osc.stop(t + offset + 0.22);
          });
        } catch (e) {}
      }

      function announceHour(hour) {
        var currentTask = byId('a-' + hour);
        if (!currentTask) return;

        var taskText = currentTask.value.trim();
        var nextEl = byId('a-' + (hour + 1));
        var nextText = nextEl ? nextEl.value.trim() : '';
        var message = 'Il est ' + pad(hour) + 'h. ' + (taskText ? 'À faire maintenant : ' + taskText + '.' : 'Rien de prévu sur ce créneau.');
        if (nextText) message += ' Ensuite à ' + pad(hour + 1) + 'h : ' + nextText + '.';

        byId('alerttext').textContent = message;
        byId('alert').hidden = false;
        beep();
        try { if (navigator.vibrate) navigator.vibrate([180, 80, 180]); } catch (e) {}
      }

      function keepAwake() {
        try {
          if (!reminderEnabled || document.hidden || !navigator.wakeLock) return;
          if (!wakeLock) {
            navigator.wakeLock.request('screen').then(function (lock) {
              wakeLock = lock;
              lock.addEventListener('release', function () { wakeLock = null; });
            }).catch(function () {});
          }
        } catch (e) {}
      }

      function tickReminder() {
        var now = new Date();
        var hour = now.getHours();

        if (hour !== lastHour) {
          lastHour = hour;
          markNow();
          if (reminderEnabled && keyOf(now) === dateKey) {
            announceHour(hour);
          }
        }
      }

      function applyTheme(theme) {
        document.documentElement.setAttribute('data-theme', theme);
        localStorage.setItem('planning:theme', theme);
        byId('themeToggle').textContent = theme === 'dark' ? '☀️' : '🌙';
        byId('themeToggle').title = theme === 'dark' ? 'Mode clair' : 'Mode sombre';
      }

      function initTheme() {
        var saved = localStorage.getItem('planning:theme') || 'light';
        applyTheme(saved);
        byId('themeToggle').addEventListener('click', function () {
          var current = document.documentElement.getAttribute('data-theme') === 'dark' ? 'light' : 'dark';
          applyTheme(current);
        });
      }

      byId('rows').addEventListener('input', onInputChange);
      byId('rows').addEventListener('change', onInputChange);
      byId('date').addEventListener('change', function () {
        if (byId('date').value) {
          goTo(byId('date').value);
        }
      });

      byId('prev').addEventListener('click', function () {
        var day = parseKey(dateKey);
        day.setDate(day.getDate() - 1);
        goTo(keyOf(day));
      });

      byId('next').addEventListener('click', function () {
        var day = parseKey(dateKey);
        day.setDate(day.getDate() + 1);
        goTo(keyOf(day));
      });

      byId('today').addEventListener('click', function () {
        goTo(keyOf(new Date()));
      });

      byId('addSlot').addEventListener('click', addSlot);
      byId('removeSlot').addEventListener('click', removeSlot);

      byId('printBtn').addEventListener('click', function () {
        window.print();
      });

      var clearTimer = null;
      byId('clear').addEventListener('click', function () {
        var btn = byId('clear');
        if (btn.dataset.armed) {
          clearTimeout(clearTimer);
          delete btn.dataset.armed;
          btn.textContent = 'Vider la journée';
          setData({});
          persistNow();
          return;
        }

        btn.dataset.armed = '1';
        btn.textContent = 'Confirmer : tout effacer ?';
        clearTimer = setTimeout(function () {
          delete btn.dataset.armed;
          btn.textContent = 'Vider la journée';
        }, 3500);
      });

      byId('alertok').addEventListener('click', function () {
        byId('alert').hidden = true;
      });

      byId('remind').checked = reminderEnabled;
      byId('remind').addEventListener('change', function () {
        reminderEnabled = byId('remind').checked;
        localStorage.setItem('planning:remind', reminderEnabled ? '1' : '0');

        if (reminderEnabled) {
          ensureAudio();
          beep();
          keepAwake();
          setStatus('Rappel activé');
        } else {
          try { if (wakeLock) wakeLock.release(); } catch (e) {}
          setStatus('Rappel désactivé');
        }
      });

      document.addEventListener('visibilitychange', function () {
        if (!document.hidden) {
          keepAwake();
          tickReminder();
        }
      });

      setInterval(tickReminder, 15000);
      initTheme();
      buildRows();
      showDate();
      setData(loadDay(dateKey));
    })();
  </script>
</body>
</html>



