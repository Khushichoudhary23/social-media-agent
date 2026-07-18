<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Social Media Agent — IBM watsonx</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: -apple-system, "Segoe UI", system-ui, sans-serif;
      background: #0d0e1a;
      color: #e8eaf6;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
    }

    /* ── Header ── */
    header {
      background: #13152a;
      border-bottom: 1px solid #2a2d4a;
      padding: 0 32px;
      height: 64px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      position: sticky;
      top: 0;
      z-index: 100;
    }

    .header-left { display: flex; align-items: center; gap: 14px; }

    .logo-icon {
      width: 38px; height: 38px;
      background: linear-gradient(135deg, #4f8ef7, #8b5cf6);
      border-radius: 10px;
      display: flex; align-items: center; justify-content: center;
      flex-shrink: 0;
    }
    .logo-icon svg { display: block; }

    .brand-name { font-size: 16px; font-weight: 700; color: #e8eaf6; letter-spacing: -0.3px; }
    .brand-sub  { font-size: 11px; color: #7b80a8; margin-top: 1px; }

    .header-right { display: flex; align-items: center; gap: 12px; }

    /* IBM logo in header */
    .ibm-badge {
      display: flex; align-items: center; gap: 8px;
      background: rgba(255,255,255,0.05);
      border: 1px solid #2a2d4a;
      border-radius: 8px;
      padding: 5px 12px;
      font-size: 11px; color: #7b80a8; font-weight: 500;
    }
    .ibm-badge svg { display: block; flex-shrink: 0; }

    .status-badge {
      display: flex; align-items: center; gap: 7px;
      background: rgba(79,142,247,0.12);
      border: 1px solid rgba(79,142,247,0.35);
      border-radius: 20px;
      padding: 5px 14px;
      font-size: 12px; color: #4f8ef7; font-weight: 500;
    }
    .status-dot {
      width: 7px; height: 7px;
      background: #4f8ef7; border-radius: 50%;
      box-shadow: 0 0 6px #4f8ef7;
    }

    /* ── Hero banner ── */
    .hero {
      background: #13152a;
      border-bottom: 1px solid #2a2d4a;
      padding: 28px 32px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 24px;
      max-width: 100%;
    }

    .hero-text { flex: 1; }
    .hero-title {
      font-size: 26px; font-weight: 800;
      color: #e8eaf6; letter-spacing: -0.5px; line-height: 1.25;
    }
    .hero-title span {
      background: linear-gradient(90deg, #4f8ef7, #8b5cf6);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }
    .hero-sub {
      margin-top: 8px; font-size: 14px; color: #7b80a8; line-height: 1.6; max-width: 520px;
    }

    /* Platform logo strip */
    .platform-strip {
      display: flex; align-items: center; gap: 10px; margin-top: 18px; flex-wrap: wrap;
    }
    .platform-strip-label { font-size: 11px; color: #7b80a8; font-weight: 600; text-transform: uppercase; letter-spacing: 0.8px; margin-right: 4px; }
    .plat-logo {
      width: 36px; height: 36px; border-radius: 10px;
      display: flex; align-items: center; justify-content: center;
      border: 1px solid #2a2d4a;
    }
    .plat-logo svg { display: block; }

    /* Hero illustration */
    .hero-illustration { flex-shrink: 0; }

    /* ── Layout ── */
    .page {
      display: flex;
      flex: 1;
      max-width: 1240px;
      width: 100%;
      margin: 0 auto;
      padding: 24px 24px;
      gap: 24px;
      align-items: flex-start;
    }

    /* ── Sidebar ── */
    .sidebar {
      width: 252px; flex-shrink: 0;
      display: flex; flex-direction: column; gap: 16px;
      position: sticky; top: 80px;
    }

    .card {
      background: #13152a;
      border: 1px solid #2a2d4a;
      border-radius: 12px;
      padding: 18px;
    }

    .card-title {
      font-size: 10px; font-weight: 700;
      text-transform: uppercase; letter-spacing: 1px;
      color: #7b80a8; margin-bottom: 14px;
    }

    /* Capabilities */
    .capability-list { list-style: none; display: flex; flex-direction: column; gap: 10px; }
    .capability-list li {
      display: flex; align-items: flex-start; gap: 10px;
      font-size: 13px; color: #c5c9e8; line-height: 1.45;
    }
    .cap-icon {
      width: 22px; height: 22px; border-radius: 6px;
      background: rgba(79,142,247,0.15);
      border: 1px solid rgba(79,142,247,0.25);
      display: flex; align-items: center; justify-content: center;
      flex-shrink: 0; margin-top: 1px;
    }
    .cap-icon svg { display: block; }

    /* Platforms in sidebar */
    .platform-list { display: flex; flex-direction: column; gap: 8px; }
    .platform-item {
      display: flex; align-items: center; gap: 10px;
      padding: 8px 12px; border-radius: 8px;
      border: 1px solid #2a2d4a; background: #1a1d36;
      font-size: 13px; color: #c5c9e8;
    }
    .platform-icon {
      width: 24px; height: 24px; border-radius: 6px;
      display: flex; align-items: center; justify-content: center;
      flex-shrink: 0;
    }
    .platform-icon svg { display: block; }

    .tip-box {
      background: rgba(139,92,246,0.1);
      border: 1px solid rgba(139,92,246,0.3);
      border-left: 3px solid #8b5cf6;
      border-radius: 8px; padding: 12px 14px;
      font-size: 12px; color: #c4b5fd; line-height: 1.6;
    }
    .tip-label { font-weight: 700; margin-bottom: 5px; color: #a78bfa; }

    /* ── Main ── */
    .main { flex: 1; min-width: 0; display: flex; flex-direction: column; gap: 16px; }

    .panel-header {
      display: flex; align-items: center; justify-content: space-between;
      padding: 16px 20px;
      background: #13152a;
      border: 1px solid #2a2d4a;
      border-radius: 12px;
      border-left: 3px solid #4f8ef7;
    }
    .panel-header-left { display: flex; align-items: center; gap: 12px; }
    .agent-avatar {
      width: 40px; height: 40px; border-radius: 10px;
      background: linear-gradient(135deg, #4f8ef7, #8b5cf6);
      display: flex; align-items: center; justify-content: center;
      flex-shrink: 0;
    }
    .agent-avatar svg { display: block; }
    .panel-title   { font-size: 17px; font-weight: 700; color: #e8eaf6; }
    .panel-desc    { font-size: 12px; color: #7b80a8; margin-top: 2px; }

    /* Prompt chips */
    .prompts-row { display: flex; flex-wrap: wrap; gap: 8px; }
    .prompt-chip {
      background: #1a1d36; border: 1px solid #2a2d4a;
      border-radius: 20px; padding: 6px 16px;
      font-size: 12px; color: #a5b4fc;
      cursor: default; white-space: nowrap;
      transition: background 0.15s, border-color 0.15s, color 0.15s;
    }
    .prompt-chip:hover { background: rgba(79,142,247,0.15); border-color: #4f8ef7; color: #4f8ef7; }

    /* ── Chat wrapper — the critical part ── */
    .chat-wrapper {
      background: #13152a;
      border: 1px solid #2a2d4a;
      border-radius: 12px;
      overflow: hidden;
      height: calc(100vh - 330px);
      min-height: 440px;
      position: relative;
      display: flex;
      flex-direction: column;
    }

    /* #root must fill the entire wrapper so the agent UI has space */
    #root {
      flex: 1;
      width: 100%;
      min-height: 0;
      /* Force any iframe / div the loader injects to fill fully */
    }
    #root > * { width: 100% !important; height: 100% !important; }

    /* Loading overlay */
    .chat-loading {
      position: absolute; inset: 0;
      display: flex; flex-direction: column;
      align-items: center; justify-content: center; gap: 14px;
      color: #7b80a8; font-size: 13px;
      background: #13152a;
      pointer-events: none; transition: opacity 0.4s;
    }
    .spinner {
      width: 32px; height: 32px;
      border: 3px solid #2a2d4a;
      border-top-color: #4f8ef7;
      border-radius: 50%;
      animation: spin 0.75s linear infinite;
    }
    @keyframes spin { to { transform: rotate(360deg); } }

    /* ── Footer ── */
    footer {
      text-align: center; font-size: 11px; color: #4a4f72;
      padding: 14px 0; border-top: 1px solid #2a2d4a;
      background: #13152a;
    }

    @media (max-width: 900px) {
      .hero { padding: 20px 20px; }
      .hero-illustration { display: none; }
    }
    @media (max-width: 768px) {
      header { padding: 0 16px; }
      .ibm-badge { display: none; }
      .page { flex-direction: column; padding: 16px; }
      .sidebar { width: 100%; position: static; }
      .chat-wrapper { height: 65vh; min-height: 380px; }
    }
  </style>
</head>
<body>

<!-- ── Header ── -->
<header>
  <div class="header-left">
    <div class="logo-icon">
      <svg width="20" height="20" viewBox="0 0 20 20" fill="none">
        <path d="M4 10h12M4 7h12M4 13h12M7 4v12M13 4v12" stroke="#fff" stroke-width="1.6" stroke-linecap="round"/>
      </svg>
    </div>
    <div>
      <div class="brand-name">Social Media Agent</div>
      <div class="brand-sub">Powered by IBM watsonx Orchestrate</div>
    </div>
  </div>
  <div class="header-right">
    <!-- IBM wordmark -->
    <div class="ibm-badge">
      <svg width="32" height="13" viewBox="0 0 32 13" fill="none" xmlns="http://www.w3.org/2000/svg">
        <rect x="0" y="0" width="4" height="2" fill="#4f8ef7"/>
        <rect x="0" y="3" width="4" height="2" fill="#4f8ef7"/>
        <rect x="0" y="6" width="4" height="2" fill="#4f8ef7"/>
        <rect x="0" y="9" width="4" height="2" fill="#4f8ef7"/>
        <rect x="0" y="12" width="4" height="1" fill="#4f8ef7"/>
        <rect x="6" y="0" width="8" height="1" fill="#4f8ef7"/>
        <rect x="6" y="3" width="8" height="2" fill="#4f8ef7"/>
        <rect x="6" y="6" width="8" height="2" fill="#4f8ef7"/>
        <rect x="6" y="9" width="8" height="2" fill="#4f8ef7"/>
        <rect x="6" y="12" width="8" height="1" fill="#4f8ef7"/>
        <rect x="16" y="0" width="3" height="13" fill="#4f8ef7"/>
        <rect x="21" y="0" width="3" height="13" fill="#4f8ef7"/>
        <rect x="27" y="0" width="3" height="13" fill="#4f8ef7"/>
        <path d="M19 3 L25 6.5 L19 10" fill="#4f8ef7"/>
      </svg>
      <span>watsonx</span>
    </div>
    <div class="status-badge">
      <div class="status-dot"></div>
      Agent Online
    </div>
  </div>
</header>

<!-- ── Hero Banner ── -->
<div class="hero">
  <div class="hero-text">
    <div class="hero-title">Your AI-Powered<br><span>Social Media Agent</span></div>
    <div class="hero-sub">Craft posts, generate hashtags, schedule content, and analyse performance across all your platforms — just by chatting.</div>
    <div class="platform-strip">
      <span class="platform-strip-label">Works with</span>
      <!-- Facebook -->
      <div class="plat-logo" style="background:#1877f2;">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="#fff"><path d="M18 2h-3a5 5 0 00-5 5v3H7v4h3v8h4v-8h3l1-4h-4V7a1 1 0 011-1h3z"/></svg>
      </div>
      <!-- X / Twitter -->
      <div class="plat-logo" style="background:#000;">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="#fff"><path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/></svg>
      </div>
      <!-- Instagram -->
      <div class="plat-logo" style="background:linear-gradient(45deg,#f09433,#e6683c,#dc2743,#cc2366,#bc1888);">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#fff" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="2" width="20" height="20" rx="5"/><circle cx="12" cy="12" r="5"/><circle cx="17.5" cy="6.5" r="1" fill="#fff" stroke="none"/></svg>
      </div>
      <!-- LinkedIn -->
      <div class="plat-logo" style="background:#0a66c2;">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="#fff"><path d="M16 8a6 6 0 016 6v7h-4v-7a2 2 0 00-2-2 2 2 0 00-2 2v7h-4v-7a6 6 0 016-6zM2 9h4v12H2z"/><circle cx="4" cy="4" r="2"/></svg>
      </div>
      <!-- TikTok -->
      <div class="plat-logo" style="background:#010101; border-color:#333;">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="#fff"><path d="M19.59 6.69a4.83 4.83 0 01-3.77-4.25V2h-3.45v13.67a2.89 2.89 0 01-2.88 2.5 2.89 2.89 0 01-2.89-2.89 2.89 2.89 0 012.89-2.89c.28 0 .54.04.79.1V9.01a6.3 6.3 0 00-.79-.05 6.34 6.34 0 00-6.34 6.34 6.34 6.34 0 006.34 6.34 6.34 6.34 0 006.33-6.34V8.69a8.18 8.18 0 004.79 1.54V6.79a4.85 4.85 0 01-1.02-.1z"/></svg>
      </div>
    </div>
  </div>

  <!-- Hero illustration (inline SVG) -->
  <div class="hero-illustration">
    <svg width="220" height="130" viewBox="0 0 220 130" fill="none" xmlns="http://www.w3.org/2000/svg">
      <!-- phone outline -->
      <rect x="70" y="8" width="80" height="114" rx="12" fill="#1a1d36" stroke="#2a2d4a" stroke-width="2"/>
      <rect x="78" y="20" width="64" height="90" rx="6" fill="#0d0e1a"/>
      <!-- chat bubbles inside phone -->
      <rect x="84" y="28" width="42" height="10" rx="5" fill="#4f8ef7" opacity="0.85"/>
      <rect x="94" y="44" width="42" height="10" rx="5" fill="#8b5cf6" opacity="0.85"/>
      <rect x="84" y="60" width="32" height="10" rx="5" fill="#4f8ef7" opacity="0.7"/>
      <rect x="94" y="76" width="38" height="10" rx="5" fill="#8b5cf6" opacity="0.7"/>
      <!-- sparkle left -->
      <circle cx="38" cy="30" r="4" fill="#4f8ef7" opacity="0.5"/>
      <line x1="38" y1="22" x2="38" y2="18" stroke="#4f8ef7" stroke-width="1.5" opacity="0.5"/>
      <line x1="38" y1="38" x2="38" y2="42" stroke="#4f8ef7" stroke-width="1.5" opacity="0.5"/>
      <line x1="30" y1="30" x2="26" y2="30" stroke="#4f8ef7" stroke-width="1.5" opacity="0.5"/>
      <line x1="46" y1="30" x2="50" y2="30" stroke="#4f8ef7" stroke-width="1.5" opacity="0.5"/>
      <!-- sparkle right -->
      <circle cx="186" cy="80" r="4" fill="#8b5cf6" opacity="0.5"/>
      <line x1="186" y1="72" x2="186" y2="68" stroke="#8b5cf6" stroke-width="1.5" opacity="0.5"/>
      <line x1="186" y1="88" x2="186" y2="92" stroke="#8b5cf6" stroke-width="1.5" opacity="0.5"/>
      <line x1="178" y1="80" x2="174" y2="80" stroke="#8b5cf6" stroke-width="1.5" opacity="0.5"/>
      <line x1="194" y1="80" x2="198" y2="80" stroke="#8b5cf6" stroke-width="1.5" opacity="0.5"/>
      <!-- signal waves -->
      <path d="M24 65 Q32 55 24 45" stroke="#4f8ef7" stroke-width="2" stroke-linecap="round" fill="none" opacity="0.4"/>
      <path d="M16 70 Q28 55 16 40" stroke="#4f8ef7" stroke-width="1.5" stroke-linecap="round" fill="none" opacity="0.25"/>
      <path d="M196 65 Q188 55 196 45" stroke="#8b5cf6" stroke-width="2" stroke-linecap="round" fill="none" opacity="0.4"/>
      <path d="M204 70 Q192 55 204 40" stroke="#8b5cf6" stroke-width="1.5" stroke-linecap="round" fill="none" opacity="0.25"/>
    </svg>
  </div>
</div>

<!-- ── Page body ── -->
<div class="page">

  <!-- Sidebar -->
  <aside class="sidebar">

    <div class="card">
      <div class="card-title">Capabilities</div>
      <ul class="capability-list">
        <li>
          <div class="cap-icon">
            <svg width="12" height="12" viewBox="0 0 12 12" fill="none">
              <path d="M6 1L7.5 4.5H11L8.5 6.8L9.5 10.5L6 8.5L2.5 10.5L3.5 6.8L1 4.5H4.5L6 1Z" fill="#4f8ef7"/>
            </svg>
          </div>
          Draft &amp; schedule posts
        </li>
        <li>
          <div class="cap-icon">
            <svg width="12" height="12" viewBox="0 0 12 12" fill="none">
              <circle cx="6" cy="6" r="4.5" stroke="#4f8ef7" stroke-width="1.5"/>
              <path d="M4 6l1.5 1.5L8 4" stroke="#4f8ef7" stroke-width="1.5" stroke-linecap="round"/>
            </svg>
          </div>
          Generate captions &amp; hashtags
        </li>
        <li>
          <div class="cap-icon">
            <svg width="12" height="12" viewBox="0 0 12 12" fill="none">
              <rect x="1" y="3" width="10" height="7" rx="1.5" stroke="#8b5cf6" stroke-width="1.5"/>
              <path d="M4 3V2a2 2 0 014 0v1" stroke="#8b5cf6" stroke-width="1.5" stroke-linecap="round"/>
            </svg>
          </div>
          Repurpose existing content
        </li>
        <li>
          <div class="cap-icon">
            <svg width="12" height="12" viewBox="0 0 12 12" fill="none">
              <path d="M2 9.5L6 2.5L10 9.5" stroke="#4f8ef7" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
              <path d="M3.5 7.5h5" stroke="#4f8ef7" stroke-width="1.5" stroke-linecap="round"/>
            </svg>
          </div>
          Analyse performance trends
        </li>
        <li>
          <div class="cap-icon">
            <svg width="12" height="12" viewBox="0 0 12 12" fill="none">
              <circle cx="4" cy="4" r="2" stroke="#8b5cf6" stroke-width="1.5"/>
              <circle cx="9" cy="8" r="2" stroke="#8b5cf6" stroke-width="1.5"/>
              <path d="M6 4h2M4 6v2" stroke="#8b5cf6" stroke-width="1.2" stroke-linecap="round"/>
            </svg>
          </div>
          Competitor monitoring
        </li>
      </ul>
    </div>

    <div class="card">
      <div class="card-title">Platforms</div>
      <div class="platform-list">
        <div class="platform-item">
          <div class="platform-icon" style="background:#1877f2;">
            <svg width="14" height="14" viewBox="0 0 24 24" fill="#fff"><path d="M18 2h-3a5 5 0 00-5 5v3H7v4h3v8h4v-8h3l1-4h-4V7a1 1 0 011-1h3z"/></svg>
          </div>
          Facebook
        </div>
        <div class="platform-item">
          <div class="platform-icon" style="background:#111;">
            <svg width="12" height="12" viewBox="0 0 24 24" fill="#fff"><path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/></svg>
          </div>
          X / Twitter
        </div>
        <div class="platform-item">
          <div class="platform-icon" style="background:linear-gradient(45deg,#f09433,#e6683c,#dc2743,#cc2366,#bc1888);">
            <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="#fff" stroke-width="2.2" stroke-linecap="round"><rect x="2" y="2" width="20" height="20" rx="5"/><circle cx="12" cy="12" r="5"/><circle cx="17.5" cy="6.5" r="1" fill="#fff" stroke="none"/></svg>
          </div>
          Instagram
        </div>
        <div class="platform-item">
          <div class="platform-icon" style="background:#0a66c2;">
            <svg width="14" height="14" viewBox="0 0 24 24" fill="#fff"><path d="M16 8a6 6 0 016 6v7h-4v-7a2 2 0 00-2-2 2 2 0 00-2 2v7h-4v-7a6 6 0 016-6zM2 9h4v12H2z"/><circle cx="4" cy="4" r="2"/></svg>
          </div>
          LinkedIn
        </div>
        <div class="platform-item">
          <div class="platform-icon" style="background:#010101; border:1px solid #333;">
            <svg width="12" height="12" viewBox="0 0 24 24" fill="#fff"><path d="M19.59 6.69a4.83 4.83 0 01-3.77-4.25V2h-3.45v13.67a2.89 2.89 0 01-2.88 2.5 2.89 2.89 0 01-2.89-2.89 2.89 2.89 0 012.89-2.89c.28 0 .54.04.79.1V9.01a6.3 6.3 0 00-.79-.05 6.34 6.34 0 00-6.34 6.34 6.34 6.34 0 006.34 6.34 6.34 6.34 0 006.33-6.34V8.69a8.18 8.18 0 004.79 1.54V6.79a4.85 4.85 0 01-1.02-.1z"/></svg>
          </div>
          TikTok
        </div>
      </div>
    </div>

    <div class="tip-box">
      <div class="tip-label">💡 Tip</div>
      Tell the agent your brand voice, target audience, and platform — it will craft the perfect post.
    </div>

  </aside>

  <!-- Main content -->
  <main class="main">

    <!-- Panel header with agent avatar -->
    <div class="panel-header">
      <div class="panel-header-left">
        <div class="agent-avatar">
          <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="#fff" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M12 2a4 4 0 014 4v2a4 4 0 01-8 0V6a4 4 0 014-4z"/>
            <path d="M4 20c0-4 3.6-7 8-7s8 3 8 7"/>
            <circle cx="18" cy="6" r="2" fill="#4f8ef7" stroke="none"/>
          </svg>
        </div>
        <div>
          <div class="panel-title">Chat with your Agent</div>
          <div class="panel-desc">Ask it to create, schedule, or analyse your social media content.</div>
        </div>
      </div>
    </div>

    <!-- Prompt chips -->
    <div class="prompts-row">
      <div class="prompt-chip">Write a LinkedIn post about AI trends</div>
      <div class="prompt-chip">Draft 3 Instagram captions for a product launch</div>
      <div class="prompt-chip">Suggest hashtags for my fitness brand</div>
      <div class="prompt-chip">Create a weekly content calendar</div>
      <div class="prompt-chip">Analyse my top-performing posts</div>
    </div>

    <!-- Chat embed — #root fills entire wrapper -->
    <div class="chat-wrapper">
      <div class="chat-loading" id="loadingOverlay">
        <div class="spinner"></div>
        Welcome to the Social Media Agent
      </div>
      <div id="root"></div>
    </div>

  </main>
</div>

<footer>Made with IBM Bob</footer>

<script>
  window.wxOConfiguration = {
    orchestrationID: "1ff6b1394e384f2eaadf8e40aa1a500d_27b81735-a4f3-4564-b8cd-d9d3e7da487a",
    hostURL: "https://au-syd.watson-orchestrate.cloud.ibm.com",
    rootElementID: "root",
    deploymentPlatform: "ibmcloud",
    crn: "crn:v1:bluemix:public:watsonx-orchestrate:au-syd:a/1ff6b1394e384f2eaadf8e40aa1a500d:27b81735-a4f3-4564-b8cd-d9d3e7da487a::",
    chatOptions: {
      agentId: "aa821ca0-3ded-45ae-a47e-388560342d48",
      welcomeMessage: "Welcome to the Social Media Agent"
    }
  };

  setTimeout(function () {
    var script = document.createElement('script');
    script.src = window.wxOConfiguration.hostURL + '/wxochat/wxoLoader.js?embed=true';
    script.addEventListener('load', function () {
      wxoLoader.init();
      var overlay = document.getElementById('loadingOverlay');
      if (overlay) { overlay.style.opacity = '0'; }
      setTimeout(function () {
        if (overlay) { overlay.style.display = 'none'; }
      }, 400);
    });
    script.addEventListener('error', function () {
      var overlay = document.getElementById('loadingOverlay');
      if (overlay) {
        overlay.innerHTML = '<p style="color:#f87171;font-size:13px;padding:0 24px;text-align:center;">Could not reach the agent.<br>Check your network or IBM Cloud credentials.</p>';
      }
    });
    document.head.appendChild(script);
  }, 0);
</script>

</body>
</html>
