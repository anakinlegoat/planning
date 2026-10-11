<!doctype html>
<html lang="fr">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <meta name="description" content="Planning de la journée heure par heure, avec rappel à chaque changement d'heure.">
  <title>Planning du jour</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Bricolage+Grotesque:wght@600;700&family=Instrument+Sans:wght@400;500;600;700&display=swap">
  <style>
    :root {
      --bg: #eef5f8;
      --bg-2: #f9fbfc;
      --surface: rgba(255, 255, 255, 0.88);
      --surface-strong: #ffffff;
      --ink: #17232d;
      --muted: #586a78;
      --line: #d9e4ea;
      --shadow: 0 18px 48px rgba(23, 35, 45, 0.08);
      --accent: #236d8d;
      --accent-strong: #174d66;
      --accent-soft: #e2f2f8;
      --now: #fff5c9;
      --hi: #b23735;
      --mid: #b07619;
      --lo: #2f8f63;
      --success: #2d8c6c;
      --font-display: "Bricolage Grotesque", "Trebuchet MS", system-ui, sans-serif;
      --font-body: "Instrument Sans", system-ui, -apple-system, "Segoe UI", sans-serif;
    }

    :root[data-theme="dark"] {
      --bg: #111b24;
      --bg-2: #162430;
      --surface: rgba(23, 33, 43, 0.92);
      --surface-strong: #1b2b38;
      --ink: #edf4f9;
      --muted: #9bb0bf;
      --line: #2a3c4d;
      --shadow: 0 18px 46px rgba(0, 0, 0, 0.24);
      --accent: #67b7d8;
      --accent-strong: #8dd2ed;
      --accent-soft: #1b394f;
      --now: rgba(188, 157, 61, 0.25);
      --hi: #ff8e85;
      --mid: #e7ba5b;
      --lo: #75d7a1;
      --success: #7ce0af;
      color-scheme: dark;
    }

    * { box-sizing: border-box; }
    html { scroll-behavior: smooth; }
    body {
      margin: 0;
      min-height: 100vh;
      background:
        radial-gradient(circle at top left, rgba(35, 109, 141, 0.12), transparent 32%),
        linear-gradient(180deg, var(--bg), var(--bg-2));
      color: var(--ink);
      font-family: var(--font-body);
      font-size: 15px;
      line-height: 1.4;
      padding: 28px 16px 52px;
    }
    [hidden] { display: none !important; }

    .wrap {
      max-width: 1100px;
      margin: 0 auto;
      display: flex;
      flex-direction: column;
      gap: 18px;
    }

    .topbar {
      background: var(--surface);
      border: 1px solid var(--line);
      border-radius: 18px;
      box-shadow: var(--shadow);
      padding: 20px 18px;
      backdrop-filter: blur(12px);
    }

    header {
      display: flex;
      flex-wrap: wrap;
      align-items: flex-end;
      justify-content: space-between;
      gap: 12px 24px;
    }

    h1 {
      margin: 0;
      font-family: var(--font-display);
      font-size: clamp(28px, 5vw, 42px);
      line-height: 1.02;
      letter-spacing: -0.02em;
      text-wrap: balance;
    }

    .dateline {
      margin: 6px 0 0;
      color: var(--muted);
      font-size: 0.96rem;
      text-transform: capitalize;
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
      background: var(--surface-strong);
      color: var(--ink);
      border: 1px solid var(--line);
      border-radius: 10px;
      padding: 9px 13px;
      min-height: 40px;
      cursor: pointer;
      transition: transform 0.15s ease, border-color 0.2s ease, box-shadow 0.2s ease;
    }

    .btn:hover {
      border-color: var(--accent);
      box-shadow: 0 8px 18px rgba(35, 109, 141, 0.08);
    }

    .btn:active { transform: translateY(1px); }

    .btn.primary {
      background: linear-gradient(135deg, var(--accent), var(--accent-strong));
      color: white;
      border-color: transparent;
      font-weight: 700;
    }

    .btn.soft {
      background: var(--accent-soft);
      border-color: transparent;
      color: var(--accent-strong);
      font-weight: 600;
    }

    input[type="date"] {
      background: var(--surface-strong);
      border: 1px solid var(--line);
      border-radius: 10px;
      padding: 8px 10px;
      min-height: 40px;
    }

    :focus-visible {
      outline: 2px solid var(--accent);
      outline-offset: 2px;
    }

    .summary {
      display: flex;
      flex-wrap: wrap;
      gap: 10px 18px;
      align-items: center;
      justify-content: space-between;
      color: var(--muted);
      font-size: 14px;
    }

    .bar {
      flex: 1 1 220px;
      max-width: 320px;
      height: 8px;
      border-radius: 999px;
      background: var(--line);
      overflow: hidden;
      box-shadow: inset 0 1px 2px rgba(0,0,0,0.04);
    }

    .bar i {
      display: block;
      height: 100%;
      width: 0;
      background: linear-gradient(90deg, var(--lo), var(--success));
      transition: width 0.25s ease;
    }

    .remind {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      min-height: 40px;
      cursor: pointer;
      color: var(--ink);
      padding: 7px 10px;
      border-radius: 10px;
      background: var(--surface-strong);
      border: 1px solid var(--line);
    }

    .remind input {
      width: 18px;
      height: 18px;
      accent-color: var(--accent);
      margin: 0;
    }

    .sheet {
      background: var(--surface);
      border: 1px solid var(--line);
      border-radius: 16px;
      overflow: hidden;
      box-shadow: var(--shadow);
    }

    .head, .row {
      display: grid;
      grid-template-columns: 96px minmax(0, 1.5fr) minmax(0, 1.25fr) 112px 76px minmax(0, 1.2fr);
      gap: 0 10px;
      align-items: center;
      padding: 8px 14px;
    }

    .head {
      background: var(--accent-soft);
      font-size: 11px;
      letter-spacing: 0.08em;
      text-transform: uppercase;
      color: var(--muted);
      font-weight: 700;
      padding-block: 12px;
    }

    .row {
      border-top: 1px solid var(--line);
      transition: background 0.2s ease;
    }

    .row.now { background: var(--now); }

    .time {
      font-family: var(--font-display);
      font-weight: 700;
      font-variant-numeric: tabular-nums;
      font-size: 17px;
      display: flex;
      align-items: baseline;
      gap: 6px;
    }

    .time small {
      font-family: var(--font-body);
      font-weight: 500;
      font-size: 12px;
      color: var(--muted);
    }

    .row input[type="text"], .row select {
      width: 100%;
      min-width: 0;
      background: transparent;
      border: 1px solid transparent;
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
      background: var(--surface-strong);
    }

    .row select { appearance: auto; cursor: pointer; }
    .row select[data-p="Haute"] { color: var(--hi); font-weight: 700; }
    .row select[data-p="Moyenne"] { color: var(--mid); font-weight: 700; }
    .row select[data-p="Basse"] { color: var(--lo); font-weight: 700; }

    .done {
      display: flex;
      align-items: center;
      gap: 6px;
      cursor: pointer;
      min-height: 38px;
      padding-inline: 4px;
      user-select: none;
    }

    .done input {
      width: 20px;
      height: 20px;
      accent-color: var(--lo);
      cursor: pointer;
      margin: 0;
    }

    .row.is-done .a {
      text-decoration: line-through;
      color: var(--muted);
    }

    .foot {
      display: flex;
      flex-wrap: wrap;
      gap: 10px 16px;
      justify-content: space-between;
      align-items: center;
      color: var(--muted);
      font-size: 13px;
    }

    .status {
      min-height: 1.2em;
      font-weight: 600;
    }

    .foot-actions {
      display: flex;
      gap: 10px;
      flex-wrap: wrap;
    }

    .alert {
      display: flex;
      flex-wrap: wrap;
      align-items: center;
      justify-content: space-between;
      gap: 10px 16px;
      background: linear-gradient(135deg, var(--accent), var(--accent-strong));
      color: white;
      border-radius: 12px;
      padding: 12px 16px;
      font-weight: 700;
      box-shadow: var(--shadow);
    }

    .alert .btn { color: var(--ink); }

    @media (max-width: 760px) {
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
      .summary { align-items: stretch; }
      .remind { flex: 1; justify-content: center; }
    }

    @media (prefers-reduced-motion: reduce) {
      html { scroll-behavior: auto; }
      .bar i, .btn { transition: none; }
    }
  </style>
</head>
<body>
  <div class="wrap">
    <div class="topbar">
      <header>
        <div>
          <h1>Planning du jour</h1>
          <p class="dateline" id="dateline"></p>
        </div>
        <div class="controls">
          <button class="btn" id="prev" type="button" aria-label="Jour précédent">‹</button>
          <input type="date" id="date" aria-label="Choisir le jour">
          <button class="btn" id="next" type="button" aria-label="Jour suivant">›</button>
          <button class="btn primary" id="today" type="button">Aujourd’hui</button>
          <button class="btn" id="themeToggle" type="button" aria-label="Changer de thème">🌙</button>
        </div>
      </header>
    </div>

    <div class="alert" id="alert" role="alert" hidden>
      <span id="alerttext"></span>
      <button class="btn" id="alertok" type="button">OK</button>
    </div>

    <div class="summary">
      <span id="count">Aucune tâche pour ce jour</span>
      <div class="bar" aria-hidden="true"><i id="barfill"></i></div>
      <label class="remind"><input type="checkbox" id="remind"> Me prévenir à chaque changement d’heure</label>
    </div>

    <div class="sheet">
      <div class="head" aria-hidden="true">
        <span>Heure</span><span>Activité / tâche</span><span>Lieu / avec qui</span><span>Priorité</span><span>Fait</span><span>Notes</span>
      </div>
      <div id="rows"></div>
    </div>

    <div class="foot">
      <span class="status" id="status" role="status" aria-live="polite"></span>
      <div class="foot-actions">
        <button class="btn soft" id="addSlot" type="button">+ Ajouter un créneau</button>
        <button class="btn" id="removeSlot" type="button">− Retirer le dernier</button>
        <button class="btn" id="clear" type="button">Vider la journée</button>
      </div>
    </div>
  </div>

  <script>
    (function () {
      var START = 6;
      var END = 23;
      var rowsEl = document.getElementById('rows');
      var dateEl = document.getElementById('date');
      var statusEl = document.getElementById('status');
      var dateKey = keyOf(new Date());
      var lastHour = new Date().getHours();
      var reminderEnabled = localStorage.getItem('planning:remind') === '1';
      var audioCtx = null;
      var wakeLock = null;
      var themeToggleBtn = document.getElementById('themeToggle');

      function pad(n) { return String(n).padStart(2, '0'); }
      function keyOf(date) {
        return date.getFullYear() + '-' + pad(date.getMonth() + 1) + '-' + pad(date.getDate());
      }
      function parseKey(key) {
        var p = key.split('-');
        return new Date(Number(p[0]), Number(p[1]) - 1, Number(p[2]));
      }
      function byId(id) { return document.getElementById(id); }

      function applyTheme(theme) {
        document.documentElement.setAttribute('data-theme', theme);
        localStorage.setItem('planning:theme', theme);
        themeToggleBtn.textContent = theme === 'dark' ? '☀️' : '🌙';
        themeToggleBtn.title = theme === 'dark' ? 'Passer en mode clair' : 'Passer en mode sombre';
      }

      function buildRows() {
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

      function refresh() {
        var total = 0;
        var done = 0;

        for (var h = START; h <= END; h++) {
          var row = rowsEl.children[h - START];
          var isDone = byId('d-' + h).checked;
          row.classList.toggle('is-done', isDone);
          byId('p-' + h).dataset.p = byId('p-' + h).value;

          if (byId('a-' + h).value.trim() || isDone) {
            total += 1;
            if (isDone) done += 1;
          }
        }

        byId('count').textContent = total === 0
          ? 'Aucune tâche pour ce jour'
          : done + ' sur ' + total + (total > 1 ? ' tâches faites' : ' tâche faite');

        byId('barfill').style.width = total ? Math.round((done / total) * 100) + '%' : '0%';
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
        refresh();
      }

      function showDate() {
        dateEl.value = dateKey;
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
        for (var h = START; h <= END; h++) {
          rowsEl.children[h - START].classList.toggle('now', isToday && now.getHours() === h);
        }
      }

      function setStatus(message) {
        statusEl.textContent = message || '';
      }

      function storageKey(date) {
        return 'planning:' + date;
      }

      function loadDay(key) {
        try {
          var raw = localStorage.getItem(storageKey(key));
          return raw ? JSON.parse(raw) : {};
        } catch (e) {
          return {};
        }
      }

      function saveDay(key, data) {
        try {
          localStorage.setItem(storageKey(key), JSON.stringify(data));
        } catch (e) {
          // ignore
        }
      }

      function persistNow() {
        setStatus('Enregistré');
        saveDay(dateKey, getData());
      }

      function onInputChange() {
        refresh();
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
          setStatus('Journée maximale atteinte');
          return;
        }
        END += 1;
        buildRows();
        setData(loadDay(dateKey));
        refresh();
      }

      function removeSlot() {
        if (END <= START) {
          setStatus('Impossible de supprimer le dernier créneau');
          return;
        }
        END -= 1;
        buildRows();
        setData(loadDay(dateKey));
        refresh();
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
        } catch (e) { /* ignore */ }
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
        } catch (e) { /* ignore */ }
      }

      function announceHour(hour) {
        if (!byId('a-' + hour)) return;
        var currentTask = byId('a-' + hour).value.trim();
        var nextTask = byId('a-' + (hour + 1)) ? byId('a-' + (hour + 1)).value.trim() : '';
        var text = 'Il est ' + pad(hour) + 'h. ' + (currentTask ? 'À faire maintenant : ' + currentTask + '.' : 'Rien de prévu sur ce créneau.');
        if (nextTask) text += ' Ensuite à ' + pad(hour + 1) + 'h : ' + nextTask + '.';

        byId('alerttext').textContent = text;
        byId('alert').hidden = false;
        beep();
        try { if (navigator.vibrate) navigator.vibrate([200, 100, 200]); } catch (e) {}
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

      function initTheme() {
        var saved = localStorage.getItem('planning:theme');
        var theme = saved || 'light';
        applyTheme(theme);
        themeToggleBtn.addEventListener('click', function () {
          var current = document.documentElement.getAttribute('data-theme') === 'dark' ? 'light' : 'dark';
          applyTheme(current);
        });
      }

      rowsEl.addEventListener('input', onInputChange);
      rowsEl.addEventListener('change', onInputChange);
      dateEl.addEventListener('change', function () {
        if (dateEl.value) goTo(dateEl.value);
      });

      byId('prev').addEventListener('click', function () {
        var d = parseKey(dateKey);
        d.setDate(d.getDate() - 1);
        goTo(keyOf(d));
      });

      byId('next').addEventListener('click', function () {
        var d = parseKey(dateKey);
        d.setDate(d.getDate() + 1);
        goTo(keyOf(d));
      });

      byId('today').addEventListener('click', function () {
        goTo(keyOf(new Date()));
      });

      byId('addSlot').addEventListener('click', addSlot);
      byId('removeSlot').addEventListener('click', removeSlot);

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

      document.getElementById('remind').checked = reminderEnabled;
      document.getElementById('remind').addEventListener('change', function () {
        reminderEnabled = document.getElementById('remind').checked;
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
      if (reminderEnabled) {
        document.getElementById('remind').checked = true;
      }
    })();
  </script>
</body>
</html>
