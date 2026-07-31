<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
<title>📜 Историк-ИИ</title>
<style>
  :root { --transition: 0.25s ease; }

  html[data-theme="dark"] {
    --bg-black: #08070a;
    --bg-glow: #3a2a14;
    --panel-bg: rgba(24,20,17,0.86);
    --panel-border: rgba(232,192,115,0.16);
    --input-bg: rgba(255,246,230,0.06);
    --input-border: rgba(232,192,115,0.22);
    --text: #f3ead9;
    --text-dim: rgba(243,234,217,0.55);
    --user-msg: linear-gradient(135deg,#8a5a26,#5c3a18);
    --user-msg-text: #fbe9c9;
    --bot-msg: rgba(255,246,230,0.06);
    --accent-1: #e8c073;
    --accent-2: #b8752f;
    --danger: #e05b4a;
    --chip-bg: rgba(255,246,230,0.07);
    --chip-border: rgba(232,192,115,0.2);
  }
  html[data-theme="light"] {
    --bg-black: #faf5ea;
    --bg-glow: #f0dcb0;
    --panel-bg: rgba(255,253,247,0.94);
    --panel-border: rgba(120,84,25,0.15);
    --input-bg: #ffffff;
    --input-border: rgba(120,84,25,0.2);
    --text: #2a2016;
    --text-dim: rgba(42,32,22,0.55);
    --user-msg: linear-gradient(135deg,#c99a4f,#a9762c);
    --user-msg-text: #2a1c08;
    --bot-msg: #f3ead9;
    --accent-1: #b8752f;
    --accent-2: #8a5a26;
    --danger: #c0392b;
    --chip-bg: #f3ead9;
    --chip-border: rgba(120,84,25,0.18);
  }

  * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
  html, body { height: 100%; }
  body {
    margin: 0;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Arial, sans-serif;
    color: var(--text);
    background:
      radial-gradient(ellipse 90% 55% at 50% 100%, var(--bg-glow) 0%, transparent 60%),
      var(--bg-black);
    transition: background var(--transition), color var(--transition);
    height: 100vh;
    display: flex;
    flex-direction: column;
    overflow: hidden;
  }
  .serif { font-family: Georgia, 'Iowan Old Style', 'Palatino Linotype', 'Book Antiqua', serif; }
  svg { display: block; }

  header {
    background: transparent;
    padding: max(12px, env(safe-area-inset-top)) 10px 4px;
    display: flex; align-items: center; gap: 8px; flex-shrink: 0; z-index: 10;
  }
  .icon-btn {
    background: none; border: none; color: var(--text); cursor: pointer;
    width: 38px; height: 38px; border-radius: 50%;
    display: flex; align-items: center; justify-content: center; flex-shrink: 0; padding: 0;
  }
  .icon-btn:active { background: var(--chip-bg); }
  .icon-btn svg { width: 20px; height: 20px; }

  #modeSwitch { flex: 1; display: flex; justify-content: center; }
  .mode-pill { display: flex; background: var(--chip-bg); border: 1px solid var(--chip-border); border-radius: 20px; padding: 3px; gap: 2px; }
  .mode-btn {
    background: none; border: none; color: var(--text-dim); border-radius: 16px; padding: 6px 12px;
    font-size: 12.5px; cursor: pointer; white-space: nowrap; transition: all var(--transition);
    display: inline-flex; align-items: center; gap: 5px;
  }
  .mode-btn svg { width: 14px; height: 14px; }
  .mode-btn.active { color: #2a1c08; background: linear-gradient(135deg, var(--accent-1), var(--accent-2)); font-weight: 600; }

  #overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.5); display: none; z-index: 25; backdrop-filter: blur(2px); }
  #overlay.active { display: block; }

  #sidePanel {
    position: fixed; top: 0; left: -300px; width: 288px; height: 100%;
    background: var(--panel-bg); backdrop-filter: blur(24px);
    border-right: 1px solid var(--panel-border);
    transition: left var(--transition); z-index: 30; display: flex; flex-direction: column;
  }
  #sidePanel.active { left: 0; }
  .panel-top { display: flex; align-items: center; justify-content: space-between; padding: max(16px, env(safe-area-inset-top)) 14px 6px; flex-shrink: 0; }
  .panel-brand { display: flex; align-items: center; gap: 8px; font-weight: 700; font-size: 15px; }
  .panel-brand svg { width: 22px; height: 22px; }
  .panel-scroll { flex: 1; overflow-y: auto; padding: 4px 12px 10px; }

  .menu-row {
    display: flex; align-items: center; gap: 14px; width: 100%;
    background: none; border: none; color: var(--text); font-size: 14.5px; font-weight: 600;
    padding: 11px 8px; border-radius: 12px; cursor: pointer; text-align: left; font-family: inherit;
  }
  .menu-row:active { background: var(--chip-bg); }
  .menu-row svg { width: 20px; height: 20px; flex-shrink: 0; color: var(--accent-1); }

  #searchInputWrap { display: none; padding: 2px 8px 8px; }
  #searchInputWrap.active { display: block; }
  #searchInput { width: 100%; background: var(--input-bg); border: 1px solid var(--input-border); border-radius: 10px; padding: 9px 12px; font-size: 14px; color: var(--text); font-family: inherit; }
  #searchInput:focus { outline: none; border-color: var(--accent-1); }
  #searchInput::placeholder { color: var(--text-dim); }

  .menu-section-label { font-size: 11.5px; text-transform: uppercase; letter-spacing: 0.08em; color: var(--text-dim); font-weight: 700; margin: 14px 10px 4px; }

  .session-row { display: flex; align-items: center; gap: 8px; padding: 10px 10px; border-radius: 12px; cursor: pointer; }
  .session-row:active { background: var(--chip-bg); }
  .session-row.active { background: rgba(232,192,115,0.16); }
  .session-row .session-title { flex: 1; font-size: 14px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
  .session-row .session-del { background: none; border: none; color: var(--text-dim); padding: 5px; border-radius: 50%; flex-shrink: 0; cursor: pointer; display: flex; align-items: center; justify-content: center; }
  .session-row .session-del svg { width: 15px; height: 15px; }
  .session-row .session-del.confirming { color: var(--danger); background: rgba(224,91,74,0.14); }
  .session-empty-hint { padding: 10px; font-size: 13px; color: var(--text-dim); }

  .panel-footer { flex-shrink: 0; border-top: 1px solid var(--panel-border); padding: 12px 14px calc(12px + env(safe-area-inset-bottom)); display: flex; align-items: center; gap: 10px; cursor: pointer; }
  .panel-footer:active { background: var(--chip-bg); }
  .avatar-circle { width: 34px; height: 34px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-weight: 700; font-size: 14px; flex-shrink: 0; background: linear-gradient(135deg, var(--accent-1), var(--accent-2)); color: #2a1c08; }
  .avatar-circle.avatar-generic { background: var(--chip-bg); color: var(--text-dim); border: 1px solid var(--chip-border); }
  .panel-footer .profile-name { flex: 1; font-size: 14.5px; font-weight: 600; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }

  .sheet {
    position: fixed; left: 0; right: 0; bottom: -100%;
    background: var(--panel-bg); backdrop-filter: blur(24px);
    border-top: 1px solid var(--panel-border); border-radius: 20px 20px 0 0;
    max-height: 80vh; overflow-y: auto; padding: 16px 18px calc(20px + env(safe-area-inset-bottom));
    z-index: 40; transition: bottom var(--transition);
  }
  .sheet.active { bottom: 0; }
  .sheet-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 14px; }
  .sheet-header h3 { margin: 0; font-size: 16.5px; }
  .sheet-sub { font-size: 13px; color: var(--text-dim); margin: -8px 0 14px; }

  #libraryChips { display: flex; flex-wrap: wrap; gap: 8px; }

  #notebookArea { width: 100%; min-height: 160px; resize: vertical; background: var(--input-bg); border: 1px solid var(--input-border); border-radius: 12px; padding: 12px; color: var(--text); font-size: 14.5px; font-family: inherit; line-height: 1.5; }
  #notebookArea:focus { outline: none; border-color: var(--accent-1); }
  #notebookSavedHint { font-size: 12px; color: var(--text-dim); margin-top: 8px; height: 16px; }

  .settings-block { background: var(--chip-bg); border: 1px solid var(--chip-border); border-radius: 12px; padding: 12px; margin-bottom: 10px; }
  .settings-block label, .settings-block .s-label { display: block; font-size: 13px; color: var(--text-dim); margin-bottom: 8px; }
  #nameInput { width: 100%; background: var(--input-bg); border: 1px solid var(--input-border); color: var(--text); border-radius: 8px; padding: 9px 10px; font-size: 14px; font-family: inherit; }
  #nameInput:focus { outline: none; border-color: var(--accent-1); }
  #themeRow { display: flex; align-items: center; justify-content: space-between; }
  #themeRow > span { font-size: 14px; }
  .temp-label { display: flex; justify-content: space-between; font-size: 13px; margin-bottom: 8px; color: var(--text-dim); }
  #tempSlider { width: 100%; accent-color: var(--accent-1); }
  .temp-hint { font-size: 11px; opacity: 0.6; margin-top: 6px; }
  .panel-btn { background: var(--chip-bg); color: var(--text); border: 1px solid var(--chip-border); border-radius: 12px; padding: 12px; text-align: left; font-size: 14px; cursor: pointer; display: flex; align-items: center; gap: 10px; width: 100%; margin-bottom: 10px; font-family: inherit; }
  .panel-btn:active { opacity: 0.7; }
  .panel-btn svg { width: 18px; height: 18px; flex-shrink: 0; color: var(--accent-1); }
  .panel-btn.danger { color: var(--danger); }
  .panel-btn.danger svg { color: var(--danger); }
  .panel-btn.danger.confirming { background: rgba(224,91,74,0.14); border-color: var(--danger); }

  #welcomeScreen { flex: 1; display: flex; flex-direction: column; align-items: center; justify-content: center; gap: 18px; padding: 24px 20px; overflow-y: auto; }
  #sealIcon { width: 52px; height: 52px; }
  #welcomeGreeting { margin: 0; text-align: center; font-size: 26px; font-weight: 600; line-height: 1.3; background: linear-gradient(135deg, var(--text) 30%, var(--accent-1)); -webkit-background-clip: text; background-clip: text; -webkit-text-fill-color: transparent; }
  #welcomeSub { margin: -10px 0 0; font-size: 14px; color: var(--text-dim); text-align: center; max-width: 320px; }
  #welcomeChips { display: flex; flex-wrap: wrap; justify-content: center; gap: 8px; max-width: 440px; margin-top: 6px; }

  #chatContainer { flex: 1; overflow-y: auto; padding: 12px 14px; display: none; flex-direction: column; gap: 10px; }
  .msg { max-width: 82%; padding: 10px 14px; border-radius: 16px; line-height: 1.45; font-size: 15px; white-space: pre-wrap; word-wrap: break-word; animation: fadeIn 0.25s ease; }
  @keyframes fadeIn { from { opacity: 0; transform: translateY(6px);} to { opacity: 1; transform: translateY(0);} }
  .msg.user { align-self: flex-end; background: var(--user-msg); color: var(--user-msg-text); border-bottom-right-radius: 4px; }
  .msg.bot { align-self: flex-start; background: var(--bot-msg); border: 1px solid var(--panel-border); border-bottom-left-radius: 4px; position: relative; }
  .msg img { max-width: 100%; border-radius: 10px; margin-top: 6px; display: block; }

  .typing-dots { display: inline-flex; align-items: center; gap: 4px; padding: 4px 0; }
  .typing-dots span { width: 6px; height: 6px; border-radius: 50%; background: var(--accent-1); opacity: 0.5; animation: dotBounce 1.2s infinite ease-in-out; }
  .typing-dots span:nth-child(2) { animation-delay: 0.15s; }
  .typing-dots span:nth-child(3) { animation-delay: 0.3s; }
  @keyframes dotBounce { 0%,60%,100% { transform: translateY(0); opacity:0.4;} 30% { transform: translateY(-5px); opacity:1;} }

  .msg-actions { display: flex; align-items: center; gap: 14px; margin-top: 8px; }
  .copy-btn { background: none; border: none; color: var(--text-dim); font-size: 12px; cursor: pointer; padding: 2px 0; display: inline-flex; align-items: center; gap: 5px; }
  .copy-btn svg { width: 13px; height: 13px; }
  .copy-btn:active { color: var(--accent-1); }
  .copy-btn.speaking { color: var(--accent-1); }

  .followup-chips { display: flex; flex-wrap: wrap; gap: 6px; margin-top: 10px; }
  .followup-chip { background: var(--chip-bg); color: var(--text); border: 1px solid var(--chip-border); border-radius: 12px; padding: 7px 11px; font-size: 12.5px; cursor: pointer; text-align: left; font-family: inherit; }
  .followup-chip:active { opacity: 0.7; background: var(--accent-1); color: #2a1c08; }

  .quick-btn { background: var(--chip-bg); color: var(--text); border: 1px solid var(--chip-border); border-radius: 14px; padding: 9px 14px; font-size: 13px; cursor: pointer; white-space: nowrap; display: inline-flex; align-items: center; gap: 7px; }
  .quick-btn svg { width: 15px; height: 15px; color: var(--accent-1); flex-shrink: 0; }
  .quick-btn .chip-emoji { font-size: 14px; }
  .quick-btn:active { opacity: 0.7; background: var(--accent-1); color: #2a1c08; }
  .quick-btn:active svg { color: #2a1c08; }

  #previewWrap { padding: 0 16px; display: none; align-items: center; gap: 8px; }
  #previewWrap.active { display: flex; padding-top: 8px; }
  #previewWrap img { height: 54px; border-radius: 10px; }
  #previewWrap button { background: none; border: none; color: var(--text); font-size: 18px; cursor: pointer; }

  #inputBarWrap { padding: 8px 12px calc(10px + env(safe-area-inset-bottom)); flex-shrink: 0; }
  #inputBar { display: flex; align-items: center; gap: 4px; background: var(--input-bg); border: 1px solid var(--input-border); border-radius: 28px; padding: 6px; backdrop-filter: blur(20px); }
  #textInput { flex: 1; resize: none; max-height: 120px; min-height: 22px; padding: 8px 6px; border: none; background: transparent; color: var(--text); font-size: 15px; font-family: inherit; }
  #textInput:focus { outline: none; }
  #textInput::placeholder { color: var(--text-dim); }
  #sendBtn { background: linear-gradient(135deg, var(--accent-1), var(--accent-2)); color: #2a1c08; border: none; border-radius: 50%; width: 40px; height: 40px; font-size: 16px; cursor: pointer; flex-shrink: 0; display: flex; align-items: center; justify-content: center; }
  #sendBtn:active { opacity: 0.85; }
  #micBtn.recording { color: var(--danger); animation: micPulse 1s infinite ease-in-out; }
  @keyframes micPulse { 0%,100% { opacity:1;} 50% { opacity:0.4;} }

  #toast { position: fixed; bottom: 90px; left: 50%; transform: translateX(-50%); background: #7a2e22; color: #fff; padding: 10px 18px; border-radius: 10px; font-size: 14px; display: none; z-index: 55; max-width: 90%; text-align: center; box-shadow: 0 8px 24px rgba(0,0,0,0.35); }

  .switch-row { display: flex; align-items: center; justify-content: space-between; gap: 10px; }
  .switch-row + .switch-row { margin-top: 12px; padding-top: 12px; border-top: 1px solid var(--panel-border); }
  .switch-text { flex: 1; }
  .switch-title { font-size: 14px; font-weight: 600; color: var(--text); }
  .switch-desc { font-size: 12px; color: var(--text-dim); margin-top: 2px; line-height: 1.4; }
  .switch-toggle { position: relative; width: 42px; height: 24px; flex-shrink: 0; background: var(--input-bg); border: 1px solid var(--input-border); border-radius: 20px; cursor: pointer; transition: background var(--transition); }
  .switch-toggle::after { content: ''; position: absolute; top: 2px; left: 2px; width: 18px; height: 18px; border-radius: 50%; background: var(--text-dim); transition: transform var(--transition), background var(--transition); }
  .switch-toggle.on { background: linear-gradient(135deg, var(--accent-1), var(--accent-2)); border-color: transparent; }
  .switch-toggle.on::after { transform: translateX(18px); background: #2a1c08; }

  .segmented { display: flex; background: var(--input-bg); border: 1px solid var(--input-border); border-radius: 10px; padding: 3px; gap: 2px; }
  .segmented button { flex: 1; background: none; border: none; color: var(--text-dim); font-size: 13px; padding: 7px 4px; border-radius: 7px; cursor: pointer; font-family: inherit; }
  .segmented button.active { color: #2a1c08; background: linear-gradient(135deg, var(--accent-1), var(--accent-2)); font-weight: 600; }

  #devTikTokRow { display: flex; align-items: center; gap: 10px; }

  @media (min-width: 600px) {
    #chatContainer { padding: 20px 60px; }
    .msg { max-width: 65%; }
    #inputBarWrap { padding-left: 60px; padding-right: 60px; }
    #previewWrap { padding-left: 60px; padding-right: 60px; }
    #welcomeGreeting { font-size: 32px; }
    #sidePanel { width: 320px; }
  }
</style>
</head>
<body>

<header>
  <button id="menuBtn" class="icon-btn" aria-label="Меню">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><path d="M3 6h18M3 12h18M3 18h18"/></svg>
  </button>
  <div id="modeSwitch">
    <div class="mode-pill">
      <button class="mode-btn active" id="modeFactsBtn" data-mode="facts">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M8 3h6l4 4v14H6V3Z"/><path d="M14 3v4h4"/><path d="M9 12h6M9 16h4"/></svg>
        Факты
      </button>
      <button class="mode-btn" id="modeAnalysisBtn" data-mode="analysis">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M9 18h6M10 22h4"/><path d="M12 2a7 7 0 0 0-4 12.7c.6.5 1 1.3 1 2.1v.2h6v-.2c0-.8.4-1.6 1-2.1A7 7 0 0 0 12 2Z"/></svg>
        Аналитика
      </button>
    </div>
  </div>
  <button id="headerClearBtn" class="icon-btn" title="Новый чат" aria-label="Новый чат">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 20h9"/><path d="M16.5 3.5a2.12 2.12 0 0 1 3 3L7 19l-4 1 1-4Z"/></svg>
  </button>
</header>

<div id="overlay"></div>

<div id="sidePanel">
  <div class="panel-top">
    <div class="panel-brand">
      <svg viewBox="0 0 48 48" fill="none">
        <defs><linearGradient id="brandGrad" x1="0" y1="0" x2="48" y2="48"><stop offset="0%" stop-color="#f3d795"/><stop offset="100%" stop-color="#b8752f"/></linearGradient></defs>
        <path d="M24 6 L27 20 L41 24 L27 28 L24 42 L21 28 L7 24 L21 20 Z" fill="url(#brandGrad)"/>
      </svg>
      Историк-ИИ
    </div>
    <button id="panelCloseBtn" class="icon-btn" aria-label="Закрыть">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><path d="M6 6l12 12M18 6L6 18"/></svg>
    </button>
  </div>

  <div class="panel-scroll">
    <button class="menu-row" id="newSessionBtn">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 20h9"/><path d="M16.5 3.5a2.12 2.12 0 0 1 3 3L7 19l-4 1 1-4Z"/></svg>
      Новый чат
    </button>
    <button class="menu-row" id="searchToggleBtn">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="11" cy="11" r="7"/><path d="m21 21-4.3-4.3"/></svg>
      Поиск по чатам
    </button>
    <div id="searchInputWrap">
      <input type="text" id="searchInput" placeholder="Поиск по чатам..." autocomplete="off">
    </div>
    <button class="menu-row" id="libraryBtn">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="3" width="8" height="8" rx="1.5"/><rect x="13" y="3" width="8" height="8" rx="1.5"/><rect x="3" y="13" width="8" height="8" rx="1.5"/><rect x="13" y="13" width="8" height="8" rx="1.5"/></svg>
      Библиотека эпох
    </button>

    <div class="menu-section-label">Блокнот</div>
    <button class="menu-row" id="notebookBtn">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 4h13l3 3v13H4Z"/><path d="M8 4v6l2.5-1.5L13 10V4"/></svg>
      Мои заметки
    </button>

    <div class="menu-section-label">Разработчики </div>
    <button class="menu-row" id="devMeBtn">
      <svg viewBox="0 0 24 24" fill="currentColor"><path d="M16.6 5.82a4.28 4.28 0 0 1-3.05-1.44V16.7a5.3 5.3 0 1 1-4.56-5.25v2.6a2.72 2.72 0 1 0 1.9 2.6V2h2.53a4.28 4.28 0 0 0 3.18 4.14v2.55a6.8 6.8 0 0 1-3.18-1.1v6.85a5.3 5.3 0 0 1-.05.62 6.8 6.8 0 0 0 6.4-6.78V8.36a6.7 6.7 0 0 0 3.87 1.23V6.99a4.26 4.26 0 0 1-2.03-1.17Z"/></svg>
      Самый крутой-Разработчик
    </button>
    <button class="menu-row" id="devFriendBtn">
      <svg viewBox="0 0 24 24" fill="currentColor"><path d="M16.6 5.82a4.28 4.28 0 0 1-3.05-1.44V16.7a5.3 5.3 0 1 1-4.56-5.25v2.6a2.72 2.72 0 1 0 1.9 2.6V2h2.53a4.28 4.28 0 0 0 3.18 4.14v2.55a6.8 6.8 0 0 1-3.18-1.1v6.85a5.3 5.3 0 0 1-.05.62 6.8 6.8 0 0 0 6.4-6.78V8.36a6.7 6.7 0 0 0 3.87 1.23V6.99a4.26 4.26 0 0 1-2.03-1.17Z"/></svg>
      2-разработчик
    </button>

    <div class="menu-section-label" id="recentLabel">Недавние</div>
    <div id="sessionList"></div>
  </div>

  <div class="panel-footer" id="profileRow">
    <div class="avatar-circle" id="avatarCircle"></div>
    <span class="profile-name" id="profileName">Гость</span>
    <button class="icon-btn" id="settingsBtn" aria-label="Настройки">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="3"/><path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 1 1-2.83 2.83l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-4 0v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 1 1-2.83-2.83l.06-.06A1.65 1.65 0 0 0 4.68 15a1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1 0-4h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 1 1 2.83-2.83l.06.06A1.65 1.65 0 0 0 9 4.6a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 4 0v.09A1.65 1.65 0 0 0 15 4.6a1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 1 1 2.83 2.83l-.06.06A1.65 1.65 0 0 0 19.4 9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 0 4h-.09a1.65 1.65 0 0 0-1.51 1Z"/></svg>
    </button>
  </div>
</div>

<div class="sheet" id="librarySheet">
  <div class="sheet-header">
    <h3>Библиотека эпох</h3>
    <button class="icon-btn" id="libraryCloseBtn" aria-label="Закрыть"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><path d="M6 6l12 12M18 6L6 18"/></svg></button>
  </div>
  <div class="sheet-sub">Выберите тему — откроется новый чат с этим вопросом.</div>
  <div id="libraryChips"></div>
</div>

<div class="sheet" id="notebookSheet">
  <div class="sheet-header">
    <h3>Мои заметки</h3>
    <button class="icon-btn" id="notebookCloseBtn" aria-label="Закрыть"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><path d="M6 6l12 12M18 6L6 18"/></svg></button>
  </div>
  <textarea id="notebookArea" placeholder="Записывайте сюда даты, имена, мысли — всё, что хотите сохранить..."></textarea>
  <div id="notebookSavedHint"></div>
</div>

<div class="sheet" id="settingsSheet">
  <div class="sheet-header">
    <h3>Настройки</h3>
    <button class="icon-btn" id="settingsCloseBtn" aria-label="Закрыть"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><path d="M6 6l12 12M18 6L6 18"/></svg></button>
  </div>

  <div class="settings-block">
    <label for="nameInput"> Как к вам обращаться</label>
    <input type="text" id="nameInput" placeholder="Например, Иван" maxlength="30" autocomplete="off">
  </div>

  <div class="settings-block" id="themeRow">
    <span>Тема</span>
    <button class="panel-btn" id="themeToggleBtn" style="width:auto;margin-bottom:0;padding:8px 12px;">
      <svg id="themeIconMoon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 12.8A9 9 0 1 1 11.2 3a7 7 0 0 0 9.8 9.8Z"/></svg>
      <svg id="themeIconSun" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" style="display:none"><circle cx="12" cy="12" r="4"/><path d="M12 2v2M12 20v2M4.9 4.9l1.4 1.4M17.7 17.7l1.4 1.4M2 12h2M20 12h2M4.9 19.1l1.4-1.4M17.7 6.3l1.4-1.4"/></svg>
      <span id="themeLabel">Тёмная</span>
    </button>
  </div>

  <div class="settings-block" id="tempRow">
    <div class="temp-label"><span> Температура</span><span id="tempValue">0.7</span></div>
    <input type="range" id="tempSlider" min="0" max="1" step="0.1" value="0.7">
    <div class="temp-hint">Ниже — точнее и строже, выше — креативнее.</div>
  </div>

  <div class="menu-section-label" style="margin-top:6px;">Функции ИИ</div>

  <div class="settings-block">
    <div class="switch-row">
      <div class="switch-text">
        <div class="switch-title"> Озвучивать ответы</div>
        <div class="switch-desc">Историк-ИИ будет читать новые ответы вслух.</div>
      </div>
      <button class="switch-toggle" id="ttsToggle" role="switch" aria-checked="false"></button>
    </div>
    <div class="switch-row">
      <div class="switch-text">
        <div class="switch-title"> Похожие вопросы</div>
        <div class="switch-desc">Показывать подсказки для продолжения разговора после ответа.</div>
      </div>
      <button class="switch-toggle" id="followupToggle" role="switch" aria-checked="false"></button>
    </div>
    <div class="switch-row">
      <div class="switch-text">
        <div class="switch-title"> Простыми словами</div>
        <div class="switch-desc">Объяснять без сложных терминов — как для новичка.</div>
      </div>
      <button class="switch-toggle" id="eli5Toggle" role="switch" aria-checked="false"></button>
    </div>
    <div class="switch-row">
      <div class="switch-text">
        <div class="switch-title"> Упоминать источники</div>
        <div class="switch-desc">Ссылаться на историков и историографию, где уместно.</div>
      </div>
      <button class="switch-toggle" id="sourcesToggle" role="switch" aria-checked="false"></button>
    </div>
    <div class="switch-row">
      <div class="switch-text">
        <div class="switch-title"> Эмодзи в ответах</div>
        <div class="switch-desc">Добавлять уместные эмодзи для наглядности.</div>
      </div>
      <button class="switch-toggle" id="emojiToggle" role="switch" aria-checked="false"></button>
    </div>
  </div>

  <div class="settings-block">
    <div class="s-label">🔠 Размер текста</div>
    <div class="segmented" id="fontSizeSeg">
      <button data-size="small">Мелкий</button>
      <button data-size="medium" class="active">Средний</button>
      <button data-size="large">Крупный</button>
    </div>
  </div>

  <button class="panel-btn" id="exportBtn">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 3v12"/><path d="m7 10 5 5 5-5"/><path d="M5 21h14"/></svg>
    Скачать этот чат
  </button>
  <button class="panel-btn danger" id="clearHistoryBtn">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 6h18"/><path d="M8 6V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"/><path d="M19 6l-1 14a2 2 0 0 1-2 2H8a2 2 0 0 1-2-2L5 6"/></svg>
    <span id="clearHistoryLabel">Очистить все чаты</span>
  </button>
</div>

<div id="welcomeScreen">
  <svg id="sealIcon" viewBox="0 0 48 48" fill="none">
    <defs>
      <linearGradient id="sealGrad" x1="0" y1="0" x2="48" y2="48">
        <stop offset="0%" stop-color="#f3d795"/>
        <stop offset="100%" stop-color="#b8752f"/>
      </linearGradient>
    </defs>
    <g stroke="url(#sealGrad)" stroke-width="1.6" fill="none">
      <circle cx="24" cy="24" r="20"/>
      <circle cx="24" cy="24" r="14"/>
    </g>
    <path d="M24 6 L27 20 L41 24 L27 28 L24 42 L21 28 L7 24 L21 20 Z" fill="url(#sealGrad)"/>
  </svg>
  <h2 id="welcomeGreeting" class="serif">О какой эпохе поговорим?</h2>
  <p id="welcomeSub">Спрашивайте о любых событиях, эпохах и личностях мировой истории — от древности до наших дней.</p>
  <div id="welcomeChips"></div>
</div>

<div id="chatContainer"></div>

<div id="previewWrap">
  <img id="previewImg" alt="preview">
  <button id="removePreviewBtn">✕</button>
</div>

<div id="inputBarWrap">
  <div id="inputBar">
    <button id="imgBtn" class="icon-btn" aria-label="Прикрепить изображение">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21.44 11.05 12.25 20.24a5.5 5.5 0 0 1-7.78-7.78l9.19-9.19a3.67 3.67 0 0 1 5.19 5.19L9.66 17.65a1.83 1.83 0 0 1-2.6-2.6l8.49-8.49"/></svg>
    </button>
    <input type="file" id="fileInput" accept="image/*" style="display:none">
    <textarea id="textInput" rows="1" placeholder="Спросите об истории..."></textarea>
    <button id="micBtn" class="icon-btn" title="Голосовой ввод" aria-label="Голосовой ввод">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 1a3 3 0 0 0-3 3v8a3 3 0 0 0 6 0V4a3 3 0 0 0-3-3Z"/><path d="M19 10v2a7 7 0 0 1-14 0v-2"/><path d="M12 19v4"/><path d="M8 23h8"/></svg>
    </button>
    <button id="sendBtn" aria-label="Отправить">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m5 12 14-7-7 14-2-5-5-2Z"/></svg>
    </button>
  </div>
</div>

<div id="toast"></div>

<script>
const GEMINI_API_KEY = "AQ.Ab8RN6J60WV_hTQSZ44Vv-rMVn4brKtuWf5vmrhzIurk3icBAA";
const GEMINI_MODEL = "gemini-3.1-flash-lite";

const BASE_PROMPT =
  "Ты — эксперт по всемирной истории: от древнего мира до новейшего времени, " +
  "включая историю Украины, Европы, Азии, Америки, Африки и всех других регионов. " +
  "Ты одинаково хорошо разбираешься во всех эпохах и странах. " +
  "Стремись быть точным и взвешенным, особенно в спорных или политически " +
  "чувствительных темах — в таких случаях излагай признанные научным " +
  "сообществом факты и, если существуют разные историографические трактовки, " +
  "упоминай это нейтрально, без навязывания своей позиции. Если вопрос не " +
  "связан с историей, вежливо укажи, что специализируешься на истории. ";

const PROMPT_FACTS = BASE_PROMPT +
  "Отвечай кратко и сухо: только даты, имена, события и проверенные факты, " +
  "без развёрнутых рассуждений, оценок причин и следствий, без художественных " +
  "отступлений. Формат — сжатая справка.";

const PROMPT_ANALYSIS = BASE_PROMPT +
  "Отвечай развёрнуто и увлекательно: раскрывай исторический контекст, " +
  "причины и последствия событий, взаимосвязи с другими процессами эпохи, " +
  "приводи примеры и, где уместно, разные точки зрения историков. " +
  "Формат — подробный аналитический разбор.";

let currentMode = localStorage.getItem('historik_mode') || 'facts';
let userName = (localStorage.getItem('historik_username') || '').trim();

let ttsEnabled = localStorage.getItem('historik_tts') === '1';
let followupEnabled = localStorage.getItem('historik_followup') === '1';
let eli5Enabled = localStorage.getItem('historik_eli5') === '1';
let sourcesEnabled = localStorage.getItem('historik_sources') === '1';
let emojiEnabled = localStorage.getItem('historik_emoji') === '1';
let fontSize = localStorage.getItem('historik_fontsize') || 'medium';

function getSystemPrompt() {
  const base = currentMode === 'analysis' ? PROMPT_ANALYSIS : PROMPT_FACTS;
  const nameNote = userName
    ? ` Пользователя зовут ${userName}. Обращайся к нему по имени естественно и уместно ` +
      `(например, в начале ответа или в обращении), но не в каждом предложении.`
    : '';
  const eli5Note = eli5Enabled
    ? ' Объясняй максимально простыми словами, как новичку без предварительных знаний: избегай ' +
      'сложных терминов, а если термин необходим — сразу поясняй его простыми словами, используй короткие предложения и понятные сравнения.'
    : '';
  const sourcesNote = sourcesEnabled
    ? ' Где уместно, упоминай конкретных историков, исторические источники или ' +
      'историографические школы, на которые опирается изложение.'
    : '';
  const emojiNote = emojiEnabled
    ? ' Используй уместные эмодзи (в меру, 1-2 на абзац), чтобы сделать ответ нагляднее.'
    : ' Не используй эмодзи в ответах.';
  return base + nameNote + eli5Note + sourcesNote + emojiNote + ' ' + getCurrentDateTimeNote();
}

function getCurrentDateTimeNote() {
  const now = new Date();
  const weekdays = ['воскресенье','понедельник','вторник','среда','четверг','пятница','суббота'];
  const months = ['января','февраля','марта','апреля','мая','июня','июля','августа','сентября','октября','ноября','декабря'];
  const dateStr = `${now.getDate()} ${months[now.getMonth()]} ${now.getFullYear()} года (${weekdays[now.getDay()]})`;
  const timeStr = now.toLocaleTimeString('ru-RU', { hour: '2-digit', minute: '2-digit' });
  return `Важно: сегодняшняя дата — ${dateStr}, текущее время — ${timeStr}. ` +
    `Используй эту информацию как точку отсчёта "сейчас" при ответах о датах, ` +
    `возрасте событий, годовщинах и любых расчётах "сколько лет назад" — не полагайся на своё внутреннее представление о текущей дате.`;
}

const TRASH_ICON_SVG = '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 6h18"/><path d="M8 6V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"/><path d="M19 6l-1 14a2 2 0 0 1-2 2H8a2 2 0 0 1-2-2L5 6"/><path d="M10 11v6M14 11v6"/></svg>';
const PERSON_ICON_SVG = '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="8" r="4"/><path d="M4 21c0-4 4-6 8-6s8 2 8 6"/></svg>';
const COPY_ICON_SVG = '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="9" y="9" width="12" height="12" rx="2"/><path d="M5 15H4a1 1 0 0 1-1-1V4a1 1 0 0 1 1-1h10a1 1 0 0 1 1 1v1"/></svg>';
const CHECK_ICON_SVG = '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M20 6 9 17l-5-5"/></svg>';
const SPEAKER_ICON_SVG = '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M11 5 6 9H2v6h4l5 4V5Z"/><path d="M19.07 4.93a10 10 0 0 1 0 14.14M15.54 8.46a5 5 0 0 1 0 7.07"/></svg>';
const SPEAKER_OFF_ICON_SVG = '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M11 5 6 9H2v6h4l5 4V5Z"/><path d="M23 9l-6 6M17 9l6 6"/></svg>';
const ICON_TEMPLE = '<svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 10l9-6 9 6"/><path d="M5 10v10M9 10v10M15 10v10M19 10v10"/><path d="M3 21h18"/></svg>';
const ICON_SHIELD = '<svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 2 4 5v6c0 5 3.6 9.4 8 11 4.4-1.6 8-6 8-11V5Z"/></svg>';
const ICON_PALETTE = '<svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 2a10 10 0 1 0 3.3 19.4c.9-.3 1.1-1.5.4-2.2l-.3-.3a2 2 0 0 1 1.4-3.4H19a3 3 0 0 0 3-3c0-6-4.5-10.5-10-10.5Z"/><circle cx="7.5" cy="10.5" r="1" fill="currentColor" stroke="none"/><circle cx="10.5" cy="6.5" r="1" fill="currentColor" stroke="none"/><circle cx="15" cy="7" r="1" fill="currentColor" stroke="none"/><circle cx="17" cy="11.5" r="1" fill="currentColor" stroke="none"/></svg>';

const ERA_TOPICS = [
  { icon: ICON_TEMPLE, label: 'Древний мир', prompt: 'Расскажи о ключевых событиях Древнего мира' },
  { icon: ICON_SHIELD, label: 'Вторая мировая', prompt: 'Расскажи о Второй мировой войне' },
  { icon: '🇺🇦', label: 'История Украины', prompt: 'Расскажи об истории Украины', emoji: true },
  { icon: ICON_PALETTE, label: 'Возрождение', prompt: 'Расскажи об эпохе Возрождения в Европе' }
];

let sessions = [];
let currentSessionId = null;
let messages = [];
let pendingImage = null;
let currentSearchQuery = '';
let activeSheet = null;

const welcomeScreen = document.getElementById('welcomeScreen');
const welcomeGreeting = document.getElementById('welcomeGreeting');
const chatContainer = document.getElementById('chatContainer');
const textInput = document.getElementById('textInput');
const sendBtn = document.getElementById('sendBtn');
const imgBtn = document.getElementById('imgBtn');
const fileInput = document.getElementById('fileInput');
const previewWrap = document.getElementById('previewWrap');
const previewImg = document.getElementById('previewImg');
const removePreviewBtn = document.getElementById('removePreviewBtn');
const menuBtn = document.getElementById('menuBtn');
const sidePanel = document.getElementById('sidePanel');
const overlay = document.getElementById('overlay');
const panelCloseBtn = document.getElementById('panelCloseBtn');
const newSessionBtn = document.getElementById('newSessionBtn');
const clearHistoryBtn = document.getElementById('clearHistoryBtn');
const themeToggleBtn = document.getElementById('themeToggleBtn');
const toast = document.getElementById('toast');
const modeFactsBtn = document.getElementById('modeFactsBtn');
const modeAnalysisBtn = document.getElementById('modeAnalysisBtn');
const headerClearBtn = document.getElementById('headerClearBtn');
const tempSlider = document.getElementById('tempSlider');
const tempValue = document.getElementById('tempValue');
const micBtn = document.getElementById('micBtn');
const exportBtn = document.getElementById('exportBtn');
const nameInput = document.getElementById('nameInput');
const searchToggleBtn = document.getElementById('searchToggleBtn');
const searchInputWrap = document.getElementById('searchInputWrap');
const searchInput = document.getElementById('searchInput');
const sessionListEl = document.getElementById('sessionList');
const recentLabel = document.getElementById('recentLabel');
const profileName = document.getElementById('profileName');
const avatarCircle = document.getElementById('avatarCircle');
const settingsBtn = document.getElementById('settingsBtn');
const librarySheet = document.getElementById('librarySheet');
const libraryBtn = document.getElementById('libraryBtn');
const libraryCloseBtn = document.getElementById('libraryCloseBtn');
const libraryChips = document.getElementById('libraryChips');
const notebookSheet = document.getElementById('notebookSheet');
const notebookBtn = document.getElementById('notebookBtn');
const notebookCloseBtn = document.getElementById('notebookCloseBtn');
const notebookArea = document.getElementById('notebookArea');
const notebookSavedHint = document.getElementById('notebookSavedHint');
const settingsSheet = document.getElementById('settingsSheet');
const settingsCloseBtn = document.getElementById('settingsCloseBtn');
const clearHistoryLabel = document.getElementById('clearHistoryLabel');
const devMeBtn = document.getElementById('devMeBtn');
const devFriendBtn = document.getElementById('devFriendBtn');
const ttsToggle = document.getElementById('ttsToggle');
const followupToggle = document.getElementById('followupToggle');
const eli5Toggle = document.getElementById('eli5Toggle');
const sourcesToggle = document.getElementById('sourcesToggle');
const emojiToggle = document.getElementById('emojiToggle');
const fontSizeSeg = document.getElementById('fontSizeSeg');

let currentTemperature = parseFloat(localStorage.getItem('historik_temp')) || 0.7;

let responseCache = {};
try { responseCache = JSON.parse(localStorage.getItem('historik_cache') || '{}'); } catch (e) { responseCache = {}; }
function getCacheKey(text) { return `${currentMode}|${currentTemperature}|${eli5Enabled}|${sourcesEnabled}|${emojiEnabled}|${text.trim().toLowerCase()}`; }
function saveCache() { try { localStorage.setItem('historik_cache', JSON.stringify(responseCache)); } catch (e) {} }

function genId() { return 's_' + Date.now().toString(36) + Math.random().toString(36).slice(2, 8); }

function loadSessions() {
  let arr = [];
  try { arr = JSON.parse(localStorage.getItem('historik_sessions') || '[]'); } catch (e) { arr = []; }
  if (arr.length === 0) {
    const oldMsgs = localStorage.getItem('historik_messages');
    if (oldMsgs) {
      try {
        const parsed = JSON.parse(oldMsgs);
        if (Array.isArray(parsed) && parsed.length > 0) {
          const firstUser = parsed.find(m => m.role === 'user');
          const title = firstUser && firstUser.text ? firstUser.text.slice(0, 44) : 'Новый чат';
          arr = [{ id: genId(), title, messages: parsed, updatedAt: Date.now() }];
        }
      } catch (e) {}
      localStorage.removeItem('historik_messages');
    }
  }
  return arr;
}

function persistSessions() { try { localStorage.setItem('historik_sessions', JSON.stringify(sessions)); } catch (e) {} }

function renderSessionList(filter) {
  filter = (filter || '').trim().toLowerCase();
  sessionListEl.innerHTML = '';
  const sorted = [...sessions].sort((a, b) => b.updatedAt - a.updatedAt);
  const filtered = filter
    ? sorted.filter(s => s.title.toLowerCase().includes(filter) || s.messages.some(m => m.text && m.text.toLowerCase().includes(filter)))
    : sorted;

  if (sessions.length === 0) { recentLabel.style.display = 'none'; return; }
  recentLabel.style.display = '';

  if (filtered.length === 0) {
    const empty = document.createElement('div');
    empty.className = 'session-empty-hint';
    empty.textContent = 'Ничего не найдено';
    sessionListEl.appendChild(empty);
    return;
  }

  filtered.forEach(s => {
    const row = document.createElement('div');
    row.className = 'session-row' + (s.id === currentSessionId ? ' active' : '');
    const title = document.createElement('span');
    title.className = 'session-title';
    title.textContent = s.title || 'Новый чат';
    row.appendChild(title);
    const delBtn = document.createElement('button');
    delBtn.className = 'session-del';
    delBtn.innerHTML = TRASH_ICON_SVG;
    delBtn.setAttribute('aria-label', 'Удалить чат');
    delBtn.addEventListener('click', (e) => { e.stopPropagation(); armConfirm(delBtn, () => deleteSession(s.id)); });
    row.appendChild(delBtn);
    row.addEventListener('click', () => switchToSession(s.id));
    sessionListEl.appendChild(row);
  });
}

function saveMessages() {
  if (currentSessionId === null) {
    currentSessionId = genId();
    sessions.unshift({ id: currentSessionId, title: 'Новый чат', messages: [], updatedAt: Date.now() });
  }
  const session = sessions.find(s => s.id === currentSessionId);
  if (session) {
    session.messages = messages;
    if (session.title === 'Новый чат') {
      const firstUser = messages.find(m => m.role === 'user' && m.text);
      if (firstUser) session.title = firstUser.text.length > 44 ? firstUser.text.slice(0, 44) + '…' : firstUser.text;
    }
    session.updatedAt = Date.now();
  }
  persistSessions();
  renderSessionList(currentSearchQuery);
}

function switchToSession(id) {
  const session = sessions.find(s => s.id === id);
  if (!session) return;
  currentSessionId = id;
  messages = session.messages.slice();
  localStorage.setItem('historik_current_session', id);
  renderAllMessages();
  if (messages.length > 0) showChat(); else showWelcome();
  closeMenu();
  renderSessionList(currentSearchQuery);
}

function startNewChat() {
  currentSessionId = null;
  messages = [];
  localStorage.removeItem('historik_current_session');
  chatContainer.innerHTML = '';
  showWelcome();
  closeMenu();
  renderSessionList(currentSearchQuery);
}

function deleteSession(id) {
  sessions = sessions.filter(s => s.id !== id);
  persistSessions();
  if (id === currentSessionId) startNewChat(); else renderSessionList(currentSearchQuery);
}

function armConfirm(btn, onConfirm) {
  if (btn.dataset.confirming === '1') {
    onConfirm();
    btn.dataset.confirming = '0';
    btn.classList.remove('confirming');
    clearTimeout(btn._confirmTimer);
    return;
  }
  btn.dataset.confirming = '1';
  btn.classList.add('confirming');
  clearTimeout(btn._confirmTimer);
  btn._confirmTimer = setTimeout(() => { btn.dataset.confirming = '0'; btn.classList.remove('confirming'); }, 2500);
}

function init() {
  applyTheme(localStorage.getItem('historik_theme') || 'dark');
  applyMode(currentMode);

  tempSlider.value = currentTemperature;
  tempValue.textContent = currentTemperature.toFixed(1);

  nameInput.value = userName;
  updateWelcomeGreeting();
  updateProfileFooter();

  setToggleState(ttsToggle, ttsEnabled);
  setToggleState(followupToggle, followupEnabled);
  setToggleState(eli5Toggle, eli5Enabled);
  setToggleState(sourcesToggle, sourcesEnabled);
  setToggleState(emojiToggle, emojiEnabled);
  applyFontSize(fontSize);

  sessions = loadSessions();
  const savedCurrent = localStorage.getItem('historik_current_session');
  const savedSession = savedCurrent && sessions.find(s => s.id === savedCurrent);
  if (savedSession) {
    currentSessionId = savedSession.id;
    messages = savedSession.messages.slice();
  } else if (sessions.length > 0) {
    const latest = [...sessions].sort((a, b) => b.updatedAt - a.updatedAt)[0];
    currentSessionId = latest.id;
    messages = latest.messages.slice();
  } else {
    currentSessionId = null;
    messages = [];
  }

  renderAllMessages();
  if (messages.length > 0) showChat(); else showWelcome();

  renderTopicChips(document.getElementById('welcomeChips'));
  renderTopicChips(libraryChips);
  renderSessionList();

  notebookArea.value = localStorage.getItem('historik_note') || '';
}

function renderTopicChips(container) {
  if (!container) return;
  container.innerHTML = '';
  ERA_TOPICS.forEach(t => {
    const btn = document.createElement('button');
    btn.className = 'quick-btn';
    if (t.emoji) btn.innerHTML = `<span class="chip-emoji">${t.icon}</span><span>${t.label}</span>`;
    else btn.innerHTML = `${t.icon}<span>${t.label}</span>`;
    btn.addEventListener('click', () => { closeSheetEl(librarySheet); sendMessage(t.prompt); });
    container.appendChild(btn);
  });
}

function showWelcome() { welcomeScreen.style.display = 'flex'; chatContainer.style.display = 'none'; }
function showChat() { welcomeScreen.style.display = 'none'; chatContainer.style.display = 'flex'; }

function updateWelcomeGreeting() { welcomeGreeting.textContent = userName ? `${userName}, вам слово` : 'О какой эпохе поговорим?'; }

function updateProfileFooter() {
  profileName.textContent = userName || 'Гость';
  avatarCircle.innerHTML = '';
  if (userName) { avatarCircle.classList.remove('avatar-generic'); avatarCircle.textContent = userName.trim().charAt(0).toUpperCase(); }
  else { avatarCircle.classList.add('avatar-generic'); avatarCircle.innerHTML = PERSON_ICON_SVG; }
}

function applyTheme(theme) {
  document.documentElement.setAttribute('data-theme', theme);
  localStorage.setItem('historik_theme', theme);
  const isDark = theme === 'dark';
  document.getElementById('themeIconMoon').style.display = isDark ? '' : 'none';
  document.getElementById('themeIconSun').style.display = isDark ? 'none' : '';
  document.getElementById('themeLabel').textContent = isDark ? 'Тёмная' : 'Светлая';
}

function renderAllMessages() {
  chatContainer.innerHTML = '';
  messages.forEach(m => renderMessage(m.role, m.text, m.image));
  scrollToBottom();
}

function renderMessage(role, text, image) {
  const div = document.createElement('div');
  div.className = 'msg ' + role;
  const textNode = document.createElement('div');
  textNode.textContent = text;
  div.appendChild(textNode);
  if (image) { const img = document.createElement('img'); img.src = image; div.appendChild(img); }
  if (role === 'bot') appendMessageActions(div, textNode);
  chatContainer.appendChild(div);
  return div;
}

async function copyMessageText(textNode, btn) {
  const text = textNode.textContent;
  try {
    await navigator.clipboard.writeText(text);
    const original = btn.innerHTML;
    btn.innerHTML = CHECK_ICON_SVG + 'Скопировано';
    setTimeout(() => { btn.innerHTML = original; }, 1500);
  } catch (e) { showToast('Не удалось скопировать текст.'); }
}

function speakText(text, btn) {
  if (!('speechSynthesis' in window)) { showToast('Озвучивание не поддерживается в этом браузере.'); return; }
  if (window.speechSynthesis.speaking) {
    window.speechSynthesis.cancel();
    document.querySelectorAll('.copy-btn.speaking').forEach(b => { b.innerHTML = SPEAKER_ICON_SVG + 'Слушать'; b.classList.remove('speaking'); });
    if (btn && btn.dataset.wasSpeaking === '1') { btn.dataset.wasSpeaking = '0'; return; }
  }
  const utter = new SpeechSynthesisUtterance(text);
  utter.lang = 'ru-RU';
  if (btn) {
    btn.dataset.wasSpeaking = '1';
    btn.innerHTML = SPEAKER_OFF_ICON_SVG + 'Стоп';
    btn.classList.add('speaking');
    utter.onend = () => { btn.innerHTML = SPEAKER_ICON_SVG + 'Слушать'; btn.classList.remove('speaking'); btn.dataset.wasSpeaking = '0'; };
    utter.onerror = () => { btn.innerHTML = SPEAKER_ICON_SVG + 'Слушать'; btn.classList.remove('speaking'); btn.dataset.wasSpeaking = '0'; };
  }
  window.speechSynthesis.speak(utter);
}

function appendMessageActions(botDiv, textNode) {
  const actions = document.createElement('div');
  actions.className = 'msg-actions';

  const copyBtn = document.createElement('button');
  copyBtn.className = 'copy-btn';
  copyBtn.innerHTML = COPY_ICON_SVG + 'Копировать';
  copyBtn.addEventListener('click', () => copyMessageText(textNode, copyBtn));
  actions.appendChild(copyBtn);

  if ('speechSynthesis' in window) {
    const speakBtn = document.createElement('button');
    speakBtn.className = 'copy-btn';
    speakBtn.innerHTML = SPEAKER_ICON_SVG + 'Слушать';
    speakBtn.addEventListener('click', () => speakText(textNode.textContent, speakBtn));
    actions.appendChild(speakBtn);
  }

  botDiv.appendChild(actions);
}

function addMessage(role, text, image, save = true) {
  messages.push({ role, text, image: image || null });
  renderMessage(role, text, image);
  scrollToBottom();
  if (save) saveMessages();
}

function scrollToBottom() { chatContainer.scrollTop = chatContainer.scrollHeight; }

imgBtn.addEventListener('click', () => fileInput.click());

fileInput.addEventListener('change', (e) => {
  const file = e.target.files[0];
  if (!file) return;
  compressImage(file, (dataUrl, mimeType) => {
    const base64 = dataUrl.split(',')[1];
    pendingImage = { mimeType, base64, dataUrl };
    previewImg.src = dataUrl;
    previewWrap.classList.add('active');
  });
  fileInput.value = '';
});

function compressImage(file, callback) {
  const reader = new FileReader();
  reader.onload = (ev) => {
    const img = new Image();
    img.onload = () => {
      const MAX_SIZE = 1024;
      let { width, height } = img;
      if (width > MAX_SIZE || height > MAX_SIZE) {
        if (width > height) { height = Math.round(height * (MAX_SIZE / width)); width = MAX_SIZE; }
        else { width = Math.round(width * (MAX_SIZE / height)); height = MAX_SIZE; }
      }
      const canvas = document.createElement('canvas');
      canvas.width = width; canvas.height = height;
      const ctx = canvas.getContext('2d');
      ctx.drawImage(img, 0, 0, width, height);
      callback(canvas.toDataURL('image/jpeg', 0.8), 'image/jpeg');
    };
    img.onerror = () => callback(ev.target.result, file.type);
    img.src = ev.target.result;
  };
  reader.readAsDataURL(file);
}

removePreviewBtn.addEventListener('click', () => { pendingImage = null; previewWrap.classList.remove('active'); });

textInput.addEventListener('input', () => {
  textInput.style.height = 'auto';
  textInput.style.height = Math.min(textInput.scrollHeight, 120) + 'px';
});
textInput.addEventListener('keydown', (e) => { if (e.key === 'Enter' && !e.shiftKey) { e.preventDefault(); sendMessage(); } });
sendBtn.addEventListener('click', () => sendMessage());

async function sendMessage(overrideText) {
  const text = (overrideText !== undefined ? overrideText : textInput.value).trim();
  if (!text && !pendingImage) { showToast('Введите вопрос.'); textInput.focus(); return; }
  if (welcomeScreen.style.display !== 'none') showChat();

  const imageForDisplay = pendingImage ? pendingImage.dataUrl : null;
  addMessage('user', text || '(изображение)', imageForDisplay);

  const imageToSend = pendingImage;
  if (overrideText === undefined) { textInput.value = ''; textInput.style.height = 'auto'; }
  pendingImage = null;
  previewWrap.classList.remove('active');

  const botDiv = renderMessage('bot', '', null);
  botDiv.innerHTML = '';
  const typingIndicator = document.createElement('div');
  typingIndicator.className = 'typing-dots';
  typingIndicator.innerHTML = '<span></span><span></span><span></span>';
  botDiv.appendChild(typingIndicator);
  scrollToBottom();

  const cacheKey = !imageToSend && text ? getCacheKey(text) : null;
  if (cacheKey && responseCache[cacheKey]) {
    botDiv.innerHTML = '';
    const textNode = document.createElement('div');
    botDiv.appendChild(textNode);
    await typeText(textNode, responseCache[cacheKey]);
    appendMessageActions(botDiv, textNode);
    messages.push({ role: 'bot', text: responseCache[cacheKey], image: null });
    saveMessages();
    if (ttsEnabled) speakText(responseCache[cacheKey], null);
    if (followupEnabled) fetchFollowups(text, responseCache[cacheKey], botDiv);
    textInput.focus();
    return;
  }

  try {
    botDiv.innerHTML = '';
    const textNode = document.createElement('div');
    botDiv.appendChild(textNode);
    const responseText = await callGeminiAPIStreaming(text, imageToSend, textNode);
    appendMessageActions(botDiv, textNode);
    messages.push({ role: 'bot', text: responseText, image: null });
    saveMessages();
    if (cacheKey) { responseCache[cacheKey] = responseText; saveCache(); }
    if (ttsEnabled) speakText(responseText, null);
    if (followupEnabled) fetchFollowups(text, responseText, botDiv);
  } catch (err) {
    botDiv.remove();
    showToast(err.message || 'Ошибка при обращении к Gemini API');
  } finally {
    textInput.focus();
  }
}

function typeText(node, fullText) {
  return new Promise((resolve) => {
    let i = 0;
    const speed = 12;
    function step() {
      if (i <= fullText.length) { node.textContent = fullText.slice(0, i); i++; scrollToBottom(); setTimeout(step, speed); }
      else resolve();
    }
    step();
  });
}

async function fetchFollowups(userText, botText, botDiv) {
  if (!GEMINI_API_KEY || GEMINI_API_KEY.includes('ВСТАВЬТЕ')) return;
  try {
    const prompt = `Вопрос пользователя: "${userText}"\nОтвет историка: "${botText.slice(0, 800)}"\n\n` +
      `Предложи ровно 3 коротких (до 8 слов) уточняющих вопроса по истории, которые логично задать дальше. ` +
      `Ответь строго в формате JSON-массива строк, без пояснений и без markdown, например: ["...","...","..."]`;
    const url = `https://generativelanguage.googleapis.com/v1beta/models/${GEMINI_MODEL}:generateContent?key=${GEMINI_API_KEY}`;
    const resp = await fetch(url, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ contents: [{ role: 'user', parts: [{ text: prompt }] }], generationConfig: { temperature: 0.4 } })
    });
    if (!resp.ok) return;
    const data = await resp.json();
    const raw = data.candidates && data.candidates[0] && data.candidates[0].content && data.candidates[0].content.parts
      ? data.candidates[0].content.parts.map(p => p.text || '').join('') : '';
    const cleaned = raw.replace(/```json|```/g, '').trim();
    const arr = JSON.parse(cleaned);
    if (!Array.isArray(arr) || arr.length === 0) return;
    const wrap = document.createElement('div');
    wrap.className = 'followup-chips';
    arr.slice(0, 3).forEach(q => {
      if (typeof q !== 'string' || !q.trim()) return;
      const chip = document.createElement('button');
      chip.className = 'followup-chip';
      chip.textContent = q.trim();
      chip.addEventListener('click', () => sendMessage(q.trim()));
      wrap.appendChild(chip);
    });
    botDiv.appendChild(wrap);
    scrollToBottom();
  } catch (e) { /* тихо игнорируем — подсказки необязательны */ }
}

async function callGeminiAPIStreaming(text, image, textNode) {
  if (!GEMINI_API_KEY || GEMINI_API_KEY.includes('ВСТАВЬТЕ')) {
    throw new Error('API-ключ не задан. Откройте код и вставьте свой ключ в переменную GEMINI_API_KEY (см. комментарий вверху скрипта).');
  }
  const parts = [];
  if (text) parts.push({ text });
  if (image) parts.push({ inlineData: { mimeType: image.mimeType, data: image.base64 } });

  const body = {
    contents: [{ role: 'user', parts }],
    systemInstruction: { parts: [{ text: getSystemPrompt() }] },
    generationConfig: { temperature: currentTemperature }
  };

  const streamUrl = `https://generativelanguage.googleapis.com/v1beta/models/${3.6-flash}:streamGenerateContent?alt=sse&key=${GEMINI_API_KEY}`;

  let response;
  try {
    response = await fetch(streamUrl, { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify(body) });
  } catch (networkErr) { throw new Error('Сетевая ошибка: не удалось связаться с Gemini API.'); }

  if (!response.ok) {
    let msg = `Ошибка API (код ${response.status}).`;
    try {
      const errJson = await response.json();
      if (errJson.error && errJson.error.message) {
        if (response.status === 400 && /API key/i.test(errJson.error.message)) msg = 'Неверный API-ключ. Проверьте значение GEMINI_API_KEY в коде.';
        else if (response.status === 429) msg = 'Превышен лимит запросов к Gemini API. Попробуйте позже.';
        else msg = 'Ошибка API: ' + errJson.error.message;
      }
    } catch (e) {}
    throw new Error(msg);
  }

  const reader = response.body.getReader();
  const decoder = new TextDecoder();
  let buffer = '';
  let fullText = '';

  while (true) {
    const { done, value } = await reader.read();
    if (done) break;
    buffer += decoder.decode(value, { stream: true });
    const lines = buffer.split('\n');
    buffer = lines.pop();
    for (const line of lines) {
      const trimmed = line.trim();
      if (!trimmed.startsWith('data:')) continue;
      const jsonStr = trimmed.slice(5).trim();
      if (!jsonStr || jsonStr === '[DONE]') continue;
      try {
        const chunk = JSON.parse(jsonStr);
        const candidate = chunk.candidates && chunk.candidates[0];
        const chunkText = candidate && candidate.content && candidate.content.parts
          ? candidate.content.parts.map(p => p.text || '').join('') : '';
        if (chunkText) { fullText += chunkText; textNode.textContent = fullText; scrollToBottom(); }
      } catch (e) {}
    }
  }
  if (!fullText) throw new Error('Модель не вернула ответ. Попробуйте переформулировать вопрос.');
  return fullText;
}

function showToast(msg) {
  toast.textContent = msg;
  toast.style.display = 'block';
  setTimeout(() => { toast.style.display = 'none'; }, 4500);
}

menuBtn.addEventListener('click', () => { sidePanel.classList.add('active'); overlay.classList.add('active'); });
panelCloseBtn.addEventListener('click', closeMenu);
overlay.addEventListener('click', () => { closeMenu(); if (activeSheet) closeSheetEl(activeSheet); });

function closeMenu() { sidePanel.classList.remove('active'); if (!activeSheet) overlay.classList.remove('active'); }

newSessionBtn.addEventListener('click', startNewChat);
headerClearBtn.addEventListener('click', startNewChat);

function openSheetEl(el) {
  sidePanel.classList.remove('active');
  if (activeSheet && activeSheet !== el) activeSheet.classList.remove('active');
  activeSheet = el;
  el.classList.add('active');
  overlay.classList.add('active');
}
function closeSheetEl(el) {
  el.classList.remove('active');
  if (activeSheet === el) activeSheet = null;
  if (!sidePanel.classList.contains('active') && !activeSheet) overlay.classList.remove('active');
}

libraryBtn.addEventListener('click', () => openSheetEl(librarySheet));
libraryCloseBtn.addEventListener('click', () => closeSheetEl(librarySheet));

notebookBtn.addEventListener('click', () => { notebookArea.value = localStorage.getItem('historik_note') || ''; openSheetEl(notebookSheet); });
notebookCloseBtn.addEventListener('click', () => closeSheetEl(notebookSheet));
notebookArea.addEventListener('input', () => {
  localStorage.setItem('historik_note', notebookArea.value);
  notebookSavedHint.textContent = 'Сохранено';
  clearTimeout(notebookArea._hintTimer);
  notebookArea._hintTimer = setTimeout(() => { notebookSavedHint.textContent = ''; }, 1200);
});

devMeBtn.addEventListener('click', () => window.open('https://www.tiktok.com/@thr_u1', '_blank'));
devFriendBtn.addEventListener('click', () => window.open('https://www.tiktok.com/@mixvin1', '_blank'));

settingsBtn.addEventListener('click', () => openSheetEl(settingsSheet));
settingsCloseBtn.addEventListener('click', () => closeSheetEl(settingsSheet));

searchToggleBtn.addEventListener('click', () => {
  searchInputWrap.classList.toggle('active');
  if (searchInputWrap.classList.contains('active')) searchInput.focus();
  else { searchInput.value = ''; currentSearchQuery = ''; renderSessionList(); }
});
searchInput.addEventListener('input', () => { currentSearchQuery = searchInput.value; renderSessionList(currentSearchQuery); });

themeToggleBtn.addEventListener('click', () => {
  const current = document.documentElement.getAttribute('data-theme');
  applyTheme(current === 'dark' ? 'light' : 'dark');
});

nameInput.addEventListener('input', () => {
  userName = nameInput.value.trim();
  localStorage.setItem('historik_username', userName);
  updateWelcomeGreeting();
  updateProfileFooter();
});

function applyMode(mode) {
  currentMode = mode;
  localStorage.setItem('historik_mode', mode);
  modeFactsBtn.classList.toggle('active', mode === 'facts');
  modeAnalysisBtn.classList.toggle('active', mode === 'analysis');
}
modeFactsBtn.addEventListener('click', () => applyMode('facts'));
modeAnalysisBtn.addEventListener('click', () => applyMode('analysis'));

tempSlider.addEventListener('input', () => {
  currentTemperature = parseFloat(tempSlider.value);
  tempValue.textContent = currentTemperature.toFixed(1);
  localStorage.setItem('historik_temp', currentTemperature);
});

function setToggleState(el, on) {
  el.classList.toggle('on', on);
  el.setAttribute('aria-checked', on ? 'true' : 'false');
}

function wireToggle(el, storageKey, setter) {
  el.addEventListener('click', () => {
    const next = !el.classList.contains('on');
    setToggleState(el, next);
    setter(next);
    localStorage.setItem(storageKey, next ? '1' : '0');
  });
}

wireToggle(ttsToggle, 'historik_tts', (v) => {
  ttsEnabled = v;
  if (!v && 'speechSynthesis' in window) window.speechSynthesis.cancel();
});
wireToggle(followupToggle, 'historik_followup', (v) => { followupEnabled = v; });
wireToggle(eli5Toggle, 'historik_eli5', (v) => { eli5Enabled = v; });
wireToggle(sourcesToggle, 'historik_sources', (v) => { sourcesEnabled = v; });
wireToggle(emojiToggle, 'historik_emoji', (v) => { emojiEnabled = v; });

function applyFontSize(size) {
  fontSize = size;
  const px = size === 'small' ? 13.5 : size === 'large' ? 17 : 15;
  document.documentElement.style.setProperty('--msg-font-size', px + 'px');
  document.querySelectorAll('.msg').forEach(m => { m.style.fontSize = px + 'px'; });
  fontSizeSeg.querySelectorAll('button').forEach(b => b.classList.toggle('active', b.dataset.size === size));
  localStorage.setItem('historik_fontsize', size);
}
fontSizeSeg.querySelectorAll('button').forEach(b => b.addEventListener('click', () => applyFontSize(b.dataset.size)));

const origRenderMessage = renderMessage;
renderMessage = function(role, text, image) {
  const div = origRenderMessage(role, text, image);
  const px = fontSize === 'small' ? 13.5 : fontSize === 'large' ? 17 : 15;
  div.style.fontSize = px + 'px';
  return div;
};

const SpeechRecognitionAPI = window.SpeechRecognition || window.webkitSpeechRecognition;
let recognition = null;
let isRecording = false;
if (SpeechRecognitionAPI) {
  recognition = new SpeechRecognitionAPI();
  recognition.lang = 'ru-RU';
  recognition.continuous = false;
  recognition.interimResults = false;
  recognition.onresult = (e) => {
    const transcript = e.results[0][0].transcript;
    textInput.value = (textInput.value ? textInput.value + ' ' : '') + transcript;
    textInput.dispatchEvent(new Event('input'));
  };
  recognition.onerror = () => showToast('Не удалось распознать речь. Попробуйте ещё раз.');
  recognition.onend = () => { isRecording = false; micBtn.classList.remove('recording'); };
  micBtn.addEventListener('click', () => {
    if (isRecording) { recognition.stop(); return; }
    isRecording = true; micBtn.classList.add('recording'); recognition.start();
  });
} else { micBtn.style.display = 'none'; }

exportBtn.addEventListener('click', () => {
  if (messages.length === 0) { showToast('В этом чате пока нет сообщений.'); return; }
  const lines = messages.map(m => `${m.role === 'user' ? (userName || 'Вы') : 'Историк-ИИ'}: ${m.text}`);
  const content = '📜 Переписка с Историком-ИИ\n' + '='.repeat(30) + '\n\n' + lines.join('\n\n');
  const blob = new Blob([content], { type: 'text/plain;charset=utf-8' });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = `istorik-ai-chat-${new Date().toISOString().slice(0, 10)}.txt`;
  document.body.appendChild(a);
  a.click();
  document.body.removeChild(a);
  URL.revokeObjectURL(url);
});

clearHistoryBtn.addEventListener('click', () => {
  armConfirm(clearHistoryBtn, () => {
    localStorage.removeItem('historik_sessions');
    localStorage.removeItem('historik_current_session');
    localStorage.removeItem('historik_cache');
    responseCache = {};
    sessions = [];
    startNewChat();
    closeSheetEl(settingsSheet);
  });
  clearHistoryLabel.textContent = clearHistoryBtn.dataset.confirming === '1' ? 'Нажмите ещё раз для подтверждения' : 'Очистить все чаты';
});

init();
</script>
</body>
</html>
