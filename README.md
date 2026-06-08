
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>O Futuro da Terra — PJ5</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Playfair+Display:ital,wght@0,400;0,600;0,700;1,400&display=swap" rel="stylesheet">

  <style>
    /* ── RESET & BASE ── */
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --bg:       #f5f0e8;
      --fg:       #1e2e1e;
      --card:     #fdfbf7;
      --border:   #d4dbc8;
      --primary:  #2e6b3e;
      --primary-light: #e8f2ec;
      --secondary:#7a5230;
      --muted:    #5a6e5a;
      --muted-bg: #ede8df;
      --dark-card:#1f3a52;
      --white:    #ffffff;
      --font-sans:'Inter', sans-serif;
      --font-serif:'Playfair Display', Georgia, serif;
      --radius:   12px;
      --shadow:   0 2px 16px rgba(30,46,30,.08);
      --shadow-md:0 4px 24px rgba(30,46,30,.12);
    }

    html { scroll-behavior: smooth; }

    body {
      font-family: var(--font-sans);
      background: var(--bg);
      color: var(--fg);
      line-height: 1.6;
    }

    h1,h2,h3,h4 { font-family: var(--font-serif); line-height: 1.2; }

    img { max-width: 100%; }

    /* ── REVEAL ANIMATION ── */
    .reveal {
      opacity: 0;
      transform: translateY(24px);
      transition: opacity .6s ease, transform .6s ease;
    }
    .reveal.visible { opacity: 1; transform: none; }
    .reveal-left  { opacity: 0; transform: translateX(-30px); transition: opacity .7s ease, transform .7s ease; }
    .reveal-right { opacity: 0; transform: translateX(30px);  transition: opacity .7s ease, transform .7s ease; }
    .reveal-left.visible, .reveal-right.visible { opacity: 1; transform: none; }

    /* ── NAVBAR ── */
    #navbar {
      position: fixed;
      top: 0; left: 0; right: 0;
      z-index: 100;
      background: rgba(245,240,232,.85);
      backdrop-filter: blur(12px);
      border-bottom: 1px solid transparent;
      transition: border-color .3s, box-shadow .3s;
    }
    #navbar.scrolled {
      border-color: var(--border);
      box-shadow: var(--shadow);
    }
    .nav-inner {
      max-width: 1100px;
      margin: 0 auto;
      padding: 0 24px;
      height: 64px;
      display: flex;
      align-items: center;
      justify-content: space-between;
    }
    .nav-logo {
      display: flex;
      align-items: center;
      gap: 8px;
      font-family: var(--font-serif);
      font-size: 1.15rem;
      font-weight: 700;
      color: var(--fg);
      text-decoration: none;
      cursor: pointer;
      background: none;
      border: none;
    }
    .nav-logo .logo-icon {
      font-size: 1.4rem;
      color: var(--primary);
    }
    .nav-logo span.green { color: var(--primary); }
    .nav-links {
      display: flex;
      align-items: center;
      gap: 8px;
    }
    .nav-links button {
      background: none;
      border: none;
      color: var(--muted);
      font-size: .875rem;
      font-weight: 500;
      padding: 8px 16px;
      cursor: pointer;
      border-radius: 8px;
      transition: color .2s, background .2s;
    }
    .nav-links button:hover { color: var(--primary); background: var(--primary-light); }
    .btn-primary {
      background: var(--primary);
      color: #fff;
      border: none;
      border-radius: 10px;
      padding: 10px 22px;
      font-size: .875rem;
      font-weight: 600;
      cursor: pointer;
      transition: opacity .2s;
    }
    .btn-primary:hover { opacity: .88; }
    .nav-hamburger {
      display: none;
      background: none;
      border: none;
      font-size: 1.5rem;
      cursor: pointer;
      color: var(--fg);
    }
    #mobile-menu {
      display: none;
      flex-direction: column;
      background: rgba(245,240,232,.97);
      border-top: 1px solid var(--border);
      padding: 8px 24px 16px;
    }
    #mobile-menu button {
      background: none;
      border: none;
      text-align: left;
      padding: 12px 0;
      font-size: 1rem;
      font-weight: 500;
      color: var(--muted);
      border-bottom: 1px solid var(--border);
      cursor: pointer;
    }
    #mobile-menu button:last-child { border-bottom: none; color: var(--primary); font-weight: 700; }

    /* ── SECTION WRAPPER ── */
    .section { padding: 96px 24px; }
    .section-sm { padding: 64px 24px; }
    .container { max-width: 1100px; margin: 0 auto; }
    .container-sm { max-width: 760px; margin: 0 auto; }

    .section-header { text-align: center; margin-bottom: 56px; }
    .section-header h2 { font-size: clamp(2rem, 5vw, 3rem); margin-bottom: 16px; }
    .section-header p { font-size: 1.1rem; color: var(--muted); max-width: 580px; margin: 0 auto; }

    /* ── HERO ── */
    #home {
      min-height: 90vh;
      display: flex;
      align-items: center;
      padding: 120px 24px 72px;
      background: radial-gradient(ellipse at top right, rgba(46,107,62,.06) 0%, var(--bg) 65%);
    }
    .hero-grid {
      max-width: 1100px;
      margin: 0 auto;
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 56px;
      align-items: center;
      width: 100%;
    }
    .hero-badge {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      background: var(--primary-light);
      color: var(--primary);
      border-radius: 999px;
      padding: 6px 16px;
      font-size: .75rem;
      font-weight: 700;
      letter-spacing: .08em;
      text-transform: uppercase;
      margin-bottom: 24px;
    }
    .hero-title {
      font-size: clamp(2.8rem, 6vw, 4.2rem);
      font-weight: 700;
      margin-bottom: 20px;
      letter-spacing: -.02em;
      color: var(--fg);
    }
    .hero-title .earth { color: var(--secondary); }
    .hero-desc {
      font-size: 1.1rem;
      color: var(--muted);
      max-width: 480px;
      margin-bottom: 36px;
      line-height: 1.7;
    }
    .hero-btns {
      display: flex;
      gap: 12px;
      flex-wrap: wrap;
      margin-bottom: 28px;
    }
    .btn-dark {
      background: var(--fg);
      color: var(--bg);
      border: none;
      border-radius: 999px;
      padding: 14px 32px;
      font-size: 1rem;
      font-weight: 600;
      cursor: pointer;
      transition: opacity .2s;
    }
    .btn-dark:hover { opacity: .85; }
    .btn-outline {
      background: transparent;
      color: var(--fg);
      border: 1.5px solid var(--border);
      border-radius: 999px;
      padding: 14px 32px;
      font-size: 1rem;
      font-weight: 600;
      cursor: pointer;
      transition: background .2s;
    }
    .btn-outline:hover { background: var(--muted-bg); }
    .weather-widget {
      display: inline-flex;
      align-items: center;
      gap: 10px;
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: 16px;
      padding: 10px 20px;
      font-size: .875rem;
      color: var(--muted);
      box-shadow: var(--shadow);
    }
    .weather-widget .icon { color: var(--primary); font-size: 1rem; }

    /* Hero right card */
    .hero-card {
      background: linear-gradient(135deg, #fdfbf7, #f4eee6);
      border: 1px solid var(--border);
      border-radius: 28px;
      padding: 40px;
      box-shadow: var(--shadow-md);
      position: relative;
      overflow: hidden;
    }
    .hero-card::after {
      content: "🌿";
      position: absolute;
      top: -20px; right: -20px;
      font-size: 9rem;
      opacity: .06;
      pointer-events: none;
    }
    .hero-card h3 { font-size: 1.6rem; margin-bottom: 16px; }
    .hero-card p { color: var(--muted); line-height: 1.7; margin-bottom: 28px; }
    .hero-stats {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 12px;
    }
    .stat-box {
      background: #fff;
      border: 1px solid var(--border);
      border-radius: 14px;
      padding: 16px;
      box-shadow: 0 1px 4px rgba(0,0,0,.05);
    }
    .stat-box h4 { color: var(--primary); font-size: 1rem; font-weight: 700; margin-bottom: 4px; }
    .stat-box p { font-size: .78rem; color: var(--muted); }

    /* ── QUICK LINKS ── */
    #quick-links {
      padding: 40px 24px;
      background: var(--bg);
      margin-top: -32px;
      position: relative;
      z-index: 10;
    }
    .quick-grid {
      max-width: 1100px;
      margin: 0 auto;
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 20px;
    }
    .quick-card {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: 20px;
      padding: 24px;
      cursor: pointer;
      transition: transform .25s, box-shadow .25s;
      box-shadow: var(--shadow);
      text-align: left;
    }
    .quick-card:hover { transform: translateY(-4px); box-shadow: var(--shadow-md); }
    .quick-icon {
      width: 48px; height: 48px;
      border-radius: 14px;
      display: flex; align-items: center; justify-content: center;
      font-size: 1.3rem;
      margin-bottom: 14px;
    }
    .icon-amber { background: #fef3c7; color: #b45309; }
    .icon-green { background: var(--primary-light); color: var(--primary); }
    .icon-blue  { background: #dbeafe; color: #1d4ed8; }
    .quick-card h3 { font-size: 1.05rem; font-weight: 700; margin-bottom: 6px; }
    .quick-card p  { font-size: .85rem; color: var(--muted); }

    /* ── POR QUE MUDAR ── */
    #porque { background: var(--bg); text-align: center; }
    #porque p { font-size: 1.1rem; color: var(--muted); line-height: 1.8; max-width: 680px; margin: 0 auto; }

    /* ── GUIA DO CONSUMIDOR ── */
    #guia { background: var(--muted-bg); }

    /* Table */
    .table-wrap { overflow-x: auto; margin-bottom: 48px; }
    .comparison-table {
      width: 100%;
      min-width: 760px;
      border-collapse: collapse;
      background: var(--card);
      border-radius: var(--radius);
      overflow: hidden;
      box-shadow: var(--shadow);
      border: 1px solid var(--border);
    }
    .comparison-table thead tr {
      background: #ede8df;
      border-bottom: 1px solid var(--border);
    }
    .comparison-table th {
      padding: 14px 16px;
      text-align: left;
      font-size: .875rem;
      font-weight: 700;
      color: var(--fg);
    }
    .comparison-table td {
      padding: 14px 16px;
      font-size: .875rem;
      color: var(--muted);
      vertical-align: top;
    }
    .comparison-table tbody tr {
      border-bottom: 1px solid var(--border);
      transition: background .2s;
    }
    .comparison-table tbody tr:last-child { border-bottom: none; }
    .comparison-table tbody tr:hover { background: rgba(0,0,0,.02); }
    .comparison-table tbody tr.highlight { background: rgba(46,107,62,.04); }
    .comparison-table tbody tr.highlight td { color: var(--fg); font-weight: 500; }
    .row-label {
      display: flex;
      align-items: center;
      gap: 8px;
      font-weight: 700;
      color: var(--fg);
    }
    .row-label.green { color: var(--primary); }
    .badge-icon {
      font-size: 1rem;
    }

    /* Ciência à Mesa */
    .ciencia-card {
      background: var(--dark-card);
      border-radius: 24px;
      padding: 48px;
      color: #fff;
      position: relative;
      overflow: hidden;
      box-shadow: var(--shadow-md);
    }
    .ciencia-card::before {
      content: "";
      position: absolute;
      top: -60px; right: -60px;
      width: 260px; height: 260px;
      border-radius: 50%;
      background: rgba(46,107,62,.12);
      filter: blur(40px);
      pointer-events: none;
    }
    .ciencia-label {
      font-size: .72rem;
      font-weight: 700;
      letter-spacing: .12em;
      text-transform: uppercase;
      color: #6fcf97;
      margin-bottom: 16px;
    }
    .ciencia-card h3 { font-size: 1.7rem; color: #fff; margin-bottom: 14px; }
    .ciencia-card > p { font-size: .875rem; color: rgba(255,255,255,.65); line-height: 1.7; margin-bottom: 32px; max-width: 640px; }
    .ciencia-steps {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 20px;
    }
    .ciencia-step {
      background: rgba(255,255,255,.05);
      border: 1px solid rgba(255,255,255,.1);
      border-radius: 14px;
      padding: 20px;
    }
    .step-num {
      width: 32px; height: 32px;
      background: var(--primary);
      border-radius: 8px;
      display: flex; align-items: center; justify-content: center;
      font-size: .85rem;
      font-weight: 700;
      color: #fff;
      margin-bottom: 10px;
    }
    .ciencia-step h4 { font-size: .9rem; color: #fff; margin-bottom: 6px; font-family: var(--font-sans); font-weight: 600; }
    .ciencia-step p  { font-size: .78rem; color: rgba(255,255,255,.55); line-height: 1.6; }

    /* ── AGROTECH ── */
    #agrotech { background: var(--bg); }
    .cards-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 24px;
      margin-bottom: 64px;
    }
    .agrotech-card {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: 20px;
      padding: 28px;
      box-shadow: var(--shadow);
      transition: transform .25s, box-shadow .25s;
    }
    .agrotech-card:hover { transform: translateY(-4px); box-shadow: var(--shadow-md); }
    .card-icon {
      width: 56px; height: 56px;
      background: var(--primary-light);
      color: var(--primary);
      border-radius: 16px;
      display: flex; align-items: center; justify-content: center;
      font-size: 1.4rem;
      margin-bottom: 18px;
    }
    .agrotech-card h3 { font-size: 1.15rem; font-weight: 700; margin-bottom: 10px; }
    .agrotech-card p { font-size: .9rem; color: var(--muted); line-height: 1.7; }

    /* Simulador */
    .simulador-wrap {
      max-width: 780px;
      margin: 0 auto;
    }
    .simulador-card {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: 24px;
      overflow: hidden;
      box-shadow: var(--shadow-md);
    }
    .simulador-header {
      background: var(--muted-bg);
      border-bottom: 1px solid var(--border);
      padding: 28px 32px;
    }
    .simulador-header h3 {
      display: flex;
      align-items: center;
      gap: 12px;
      font-size: 1.35rem;
    }
    .simulador-header .h-icon {
      background: var(--primary-light);
      color: var(--primary);
      width: 40px; height: 40px;
      border-radius: 12px;
      display: flex; align-items: center; justify-content: center;
      font-size: 1.1rem;
    }
    .simulador-header p { color: var(--muted); font-size: .95rem; margin-top: 6px; }
    .simulador-body { padding: 32px; }
    .simulador-body select {
      width: 100%;
      padding: 14px 18px;
      font-size: 1rem;
      font-family: var(--font-sans);
      border: 1.5px solid var(--border);
      border-radius: 14px;
      background: var(--bg);
      color: var(--fg);
      appearance: none;
      background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='16' height='16' viewBox='0 0 24 24' fill='none' stroke='%235a6e5a' stroke-width='2'%3E%3Cpath d='m6 9 6 6 6-6'/%3E%3C/svg%3E");
      background-repeat: no-repeat;
      background-position: right 16px center;
      cursor: pointer;
    }
    .simulador-body select:focus { outline: 2px solid var(--primary); border-color: var(--primary); }
    #pest-result {
      margin-top: 24px;
      background: rgba(46,107,62,.05);
      border: 1px solid rgba(46,107,62,.2);
      border-radius: 18px;
      padding: 24px;
      display: none;
      animation: fadeIn .4s ease;
    }
    #pest-result.show { display: flex; gap: 16px; }
    .result-icon {
      background: var(--primary);
      color: #fff;
      width: 48px; height: 48px;
      border-radius: 50%;
      display: flex; align-items: center; justify-content: center;
      font-size: 1.2rem;
      flex-shrink: 0;
    }
    #pest-result h4 { font-size: 1.1rem; color: var(--primary); margin-bottom: 6px; font-family: var(--font-serif); }
    #pest-result p  { font-size: .9rem; color: var(--fg); line-height: 1.7; }

    @keyframes fadeIn { from { opacity: 0; transform: translateY(8px); } to { opacity: 1; transform: none; } }

    /* ── TIMELINE ── */
    #timeline { background: var(--muted-bg); }
    .timeline-list {
      position: relative;
      padding-left: 48px;
    }
    .timeline-list::before {
      content: "";
      position: absolute;
      left: 15px; top: 0; bottom: 0;
      width: 2px;
      background: rgba(46,107,62,.3);
    }
    .timeline-item {
      position: relative;
      margin-bottom: 48px;
    }
    .timeline-item:last-child { margin-bottom: 0; }
    .timeline-dot {
      position: absolute;
      left: -40px;
      top: 2px;
      width: 32px; height: 32px;
      background: var(--primary);
      border-radius: 50%;
      display: flex; align-items: center; justify-content: center;
      color: #fff;
      font-size: .8rem;
      box-shadow: 0 0 0 4px var(--muted-bg);
    }
    .timeline-period {
      display: inline-block;
      background: var(--primary-light);
      color: var(--primary);
      font-size: .8rem;
      font-weight: 700;
      border-radius: 999px;
      padding: 4px 14px;
      margin-bottom: 10px;
    }
    .timeline-card {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: 16px;
      padding: 20px 24px;
      box-shadow: var(--shadow);
    }
    .timeline-card h3 { font-size: 1.2rem; margin-bottom: 8px; }
    .timeline-card p  { font-size: .9rem; color: var(--muted); line-height: 1.7; }

    /* ── CONTATO ── */
    #contato { background: var(--bg); }
    .contato-card {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: 28px;
      padding: 64px 48px;
      box-shadow: var(--shadow-md);
      text-align: center;
      position: relative;
      overflow: hidden;
      max-width: 780px;
      margin: 0 auto;
    }
    .contato-card::before {
      content: "🌿";
      position: absolute;
      top: 16px; right: 24px;
      font-size: 8rem;
      opacity: .03;
      pointer-events: none;
    }
    .contato-card h2 { font-size: clamp(1.8rem, 4vw, 2.8rem); margin-bottom: 14px; }
    .contato-card > p { color: var(--muted); font-size: 1.05rem; max-width: 500px; margin: 0 auto 40px; }
    .form-group { margin-bottom: 20px; text-align: left; }
    .form-group label { display: block; font-size: .875rem; font-weight: 600; margin-bottom: 8px; }
    .form-group input {
      width: 100%;
      padding: 14px 18px;
      font-size: 1rem;
      font-family: var(--font-sans);
      border: 1.5px solid var(--border);
      border-radius: 14px;
      background: var(--bg);
      color: var(--fg);
      transition: border-color .2s;
    }
    .form-group input:focus { outline: none; border-color: var(--primary); }
    .form-error { color: #c0392b; font-size: .8rem; margin-top: 4px; display: none; }
    .form-error.show { display: block; }
    .btn-submit {
      width: 100%;
      background: var(--primary);
      color: #fff;
      border: none;
      border-radius: 999px;
      padding: 16px;
      font-size: 1rem;
      font-weight: 600;
      font-family: var(--font-sans);
      cursor: pointer;
      margin-top: 8px;
      transition: opacity .2s;
    }
    .btn-submit:hover { opacity: .88; }
    .form-success {
      display: none;
      background: rgba(46,107,62,.08);
      border: 1px solid rgba(46,107,62,.25);
      border-radius: 14px;
      padding: 16px;
      color: var(--primary);
      font-weight: 600;
      margin-top: 16px;
    }
    .form-success.show { display: block; }
    .form-max { max-width: 480px; margin: 0 auto; }

    /* ── FOOTER ── */
    footer {
      background: var(--fg);
      color: rgba(245,240,232,.9);
      padding: 64px 24px;
      text-align: center;
    }
    footer .f-icon { font-size: 2.2rem; opacity: .3; margin-bottom: 20px; }
    footer p.tagline { font-family: var(--font-serif); font-size: 1.3rem; margin-bottom: 12px; }
    footer p.copy { font-size: .8rem; opacity: .4; }

    /* ── RESPONSIVE ── */
    @media (max-width: 900px) {
      .hero-grid { grid-template-columns: 1fr; gap: 36px; }
      .hero-card { display: none; }
      .quick-grid, .cards-grid { grid-template-columns: 1fr 1fr; }
      .ciencia-steps { grid-template-columns: 1fr; }
    }
    @media (max-width: 600px) {
      .section { padding: 64px 20px; }
      .quick-grid, .cards-grid { grid-template-columns: 1fr; }
      .nav-links { display: none; }
      .nav-hamburger { display: flex; align-items: center; justify-content: center; }
      #mobile-menu { display: none; }
      #mobile-menu.open { display: flex; }
      .contato-card { padding: 36px 20px; }
      .ciencia-card { padding: 28px 20px; }
    }
  </style>
</head>
<body>

<!-- ══ NAVBAR ══ -->
<header id="navbar">
  <div class="nav-inner">
    <button class="nav-logo" onclick="scrollTo('home')">
      <span class="logo-icon">🌱</span>
      Futuro da <span class="green">&nbsp;Terra</span>
    </button>
    <div class="nav-links">
      <button onclick="scrollTo('home')">Home</button>
      <button onclick="scrollTo('guia')">Guia do Consumidor</button>
      <button onclick="scrollTo('agrotech')">AgroTech</button>
      <button class="btn-primary" onclick="scrollTo('contato')">Faça Parte</button>
    </div>
    <button class="nav-hamburger" id="hamburger-btn" onclick="toggleMenu()" aria-label="Menu">☰</button>
  </div>
  <nav id="mobile-menu">
    <button onclick="scrollTo('home')">Home</button>
    <button onclick="scrollTo('guia')">Guia do Consumidor</button>
    <button onclick="scrollTo('agrotech')">AgroTech</button>
    <button onclick="scrollTo('contato')">Faça Parte</button>
  </nav>
</header>


<!-- ══ HERO ══ -->
<section id="home">
  <div class="hero-grid">

    <!-- Esquerda -->
    <div class="reveal-left visible">
      <div class="hero-badge">🌱 Consciência &amp; Tecnologia</div>
      <h1 class="hero-title">O Futuro da <br><span class="earth">Nossa Terra</span></h1>
      <p class="hero-desc">
        A transição para uma agricultura equilibrada não é apenas uma escolha ecológica, mas a garantia de segurança alimentar e saúde para as próximas gerações.
      </p>
      <div class="hero-btns">
        <button class="btn-dark" onclick="scrollTo('agrotech')">Conhecer Soluções</button>
        <button class="btn-outline" onclick="scrollTo('guia')">Aprender a Higienizar</button>
      </div>
      <div class="weather-widget">
        <span class="icon">🌧</span>
        <span>Rio Negro – PR &nbsp;|&nbsp; 01/06/2026 &nbsp;|&nbsp; 14 °C · Nublado</span>
      </div>
    </div>

    <!-- Direita -->
    <div class="hero-card reveal-right visible">
      <h3>O Cenário Atual</h3>
      <p>O uso intensivo de defensivos químicos tradicionais desafia a biodiversidade e a resiliência do solo. A urgência global exige práticas que integrem a sabedoria da natureza com a inovação digital de ponta.</p>
      <div class="hero-stats">
        <div class="stat-box">
          <h4>Bioinsumos</h4>
          <p>Substitutos naturais eficientes</p>
        </div>
        <div class="stat-box">
          <h4>M.I.P.</h4>
          <p>Manejo inteligente de pragas</p>
        </div>
      </div>
    </div>

  </div>
</section>


<!-- ══ QUICK LINKS ══ -->
<section id="quick-links">
  <div class="quick-grid">
    <div class="quick-card reveal" onclick="scrollTo('guia')">
      <div class="quick-icon icon-amber">🛒</div>
      <h3>Guia do Consumidor</h3>
      <p>Aprenda a diferenciar os tipos de alimentos e proteja sua mesa.</p>
    </div>
    <div class="quick-card reveal" style="transition-delay:.1s" onclick="scrollTo('agrotech')">
      <div class="quick-icon icon-green">💻</div>
      <h3>AgroTech &amp; Bio</h3>
      <p>Descubra as alternativas tecnológicas e biológicas ao modelo químico tradicional.</p>
    </div>
    <div class="quick-card reveal" style="transition-delay:.2s" onclick="scrollTo('simulador')">
      <div class="quick-icon icon-blue">📊</div>
      <h3>Simulador de Combate</h3>
      <p>Teste cenários agrícolas reais e veja o controle biológico em ação.</p>
    </div>
  </div>
</section>


<!-- ══ POR QUE MUDAR ══ -->
<section id="porque" class="section">
  <div class="container-sm">
    <div class="reveal">
      <div class="section-header">
        <h2>Por que mudar?</h2>
      </div>
      <p>
        O uso intensivo de agrotóxicos ameaça a saúde humana e o equilíbrio ambiental. A contaminação do solo, da água e dos alimentos afeta toda a cadeia alimentar. A transição para práticas sustentáveis não é apenas desejável — é urgente e necessária para garantir o futuro das próximas gerações.
      </p>
    </div>
  </div>
</section>


<!-- ══ GUIA DO CONSUMIDOR ══ -->
<section id="guia" class="section" style="background:var(--muted-bg)">
  <div class="container">

    <div class="section-header reveal">
      <h2>Guia do Consumidor</h2>
      <p>Entenda as diferenças entre os métodos de cultivo e faça escolhas mais conscientes para você e para o planeta.</p>
    </div>

    <!-- Tabela comparativa -->
    <div class="table-wrap reveal">
      <table class="comparison-table">
        <thead>
          <tr>
            <th>Categoria</th>
            <th>Uso de Defensivos</th>
            <th>Foco do Modelo</th>
            <th>Impacto Social/Ambiental</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td><div class="row-label"><span class="badge-icon">⚠️</span> Convencionais</div></td>
            <td>Químicos sintéticos em larga escala.</td>
            <td>Alta produtividade por hectare.</td>
            <td>Risco de contaminação de lençóis freáticos e perda de biodiversidade local.</td>
          </tr>
          <tr>
            <td><div class="row-label"><span class="badge-icon">✅</span> Orgânicos</div></td>
            <td>Proibido o uso de sintéticos. Defensivos biológicos permitidos.</td>
            <td>Saúde do consumidor e exclusão de insumos químicos externos.</td>
            <td>Preserva a fauna local, mas pode exigir maior área territorial dependendo do cultivo.</td>
          </tr>
          <tr class="highlight">
            <td><div class="row-label green"><span class="badge-icon">⭐</span> Agroecológicos</div></td>
            <td>Zero químicos. Baseado em consórcios de plantas e equilíbrio natural.</td>
            <td>Sistemas biodiversos e regeneração ativa do bioma.</td>
            <td>Fortalece a agricultura familiar, fixa carbono no solo e recupera ecossistemas degradados.</td>
          </tr>
        </tbody>
      </table>
    </div>

    <!-- Ciência à Mesa -->
    <div class="ciencia-card reveal">
      <p class="ciencia-label">Ciência à Mesa</p>
      <h3>Como reduzir defensivos na sua rotina?</h3>
      <p>Embora a lavagem doméstica não elimine defensivos sistêmicos (absorvidos por dentro da planta), a ciência comprova que a higienização correta elimina resíduos de superfície e microrganismos patogênicos.</p>
      <div class="ciencia-steps">
        <div class="ciencia-step">
          <div class="step-num">1</div>
          <h4>Água Corrente</h4>
          <p>Esfregue os alimentos em água corrente por pelo menos 30 segundos antes do consumo.</p>
        </div>
        <div class="ciencia-step">
          <div class="step-num">2</div>
          <h4>Bicarbonato</h4>
          <p>Deixe de molho em solução de bicarbonato de sódio (1 colher por litro) por 15 minutos.</p>
        </div>
        <div class="ciencia-step">
          <div class="step-num">3</div>
          <h4>Descascar</h4>
          <p>Remova a casca sempre que possível, especialmente em alimentos convencionais.</p>
        </div>
      </div>
    </div>

  </div>
</section>


<!-- ══ AGROTECH ══ -->
<section id="agrotech" class="section">
  <div class="container">

    <div class="section-header reveal">
      <h2>AgroTech &amp; Bioinsumos</h2>
      <p>A tecnologia a serviço da natureza — conheça as inovações que estão transformando a agricultura brasileira.</p>
    </div>

    <div class="cards-grid">
      <div class="agrotech-card reveal">
        <div class="card-icon">🐛</div>
        <h3>Manejo Integrado de Pragas</h3>
        <p>Combinação de técnicas biológicas, culturais e químicas seletivas para reduzir o uso de defensivos ao mínimo necessário.</p>
      </div>
      <div class="agrotech-card reveal" style="transition-delay:.1s">
        <div class="card-icon">🧪</div>
        <h3>Bioinsumos</h3>
        <p>Uso de microrganismos, extratos vegetais e compostos orgânicos para substituir ou complementar insumos químicos na lavoura.</p>
      </div>
      <div class="agrotech-card reveal" style="transition-delay:.2s">
        <div class="card-icon">🚁</div>
        <h3>Drones Agrícolas</h3>
        <p>Monitoramento inteligente por imagem e aplicação ultra-precisa de defensivos, reduzindo desperdício e contaminação.</p>
      </div>
    </div>

    <!-- Simulador -->
    <div class="simulador-wrap" id="simulador">
      <div class="simulador-card reveal">
        <div class="simulador-header">
          <h3>
            <span class="h-icon">🐛</span>
            Simulador de Soluções Biológicas
          </h3>
          <p>Selecione uma praga ou patógeno e descubra a solução natural recomendada.</p>
        </div>
        <div class="simulador-body">
          <select id="pest-select" onchange="showPestResult(this.value)">
            <option value="">Selecione uma praga...</option>
            <option value="pulgoes">Pulgões</option>
            <option value="lagartas">Lagartas</option>
            <option value="mosca">Mosca-branca</option>
            <option value="acaros">Ácaros</option>
            <option value="percevejos">Percevejos</option>
            <option value="fungo">Fungo</option>
          </select>
          <div id="pest-result">
            <div class="result-icon">🌱</div>
            <div>
              <h4 id="pest-title"></h4>
              <p id="pest-desc"></p>
            </div>
          </div>
        </div>
      </div>
    </div>

  </div>
</section>


<!-- ══ LINHA DO TEMPO ══ -->
<section id="timeline" class="section" style="background:var(--muted-bg)">
  <div class="container-sm">

    <div class="section-header reveal">
      <h2>Linha do Tempo da Agricultura</h2>
      <p>De onde viemos e para onde vamos — a evolução das práticas agrícolas ao longo das décadas.</p>
    </div>

    <ol class="timeline-list">
      <li class="timeline-item reveal">
        <div class="timeline-dot">🕐</div>
        <span class="timeline-period">1950–1970</span>
        <div class="timeline-card">
          <h3>Expansão dos Agrotóxicos</h3>
          <p>Uso intensivo de químicos sintéticos para aumentar a produtividade agrícola durante a Revolução Verde.</p>
        </div>
      </li>
      <li class="timeline-item reveal" style="transition-delay:.1s">
        <div class="timeline-dot">🕐</div>
        <span class="timeline-period">1980–2000</span>
        <div class="timeline-card">
          <h3>Consciência Ambiental</h3>
          <p>Primeiros movimentos orgânicos e agroecológicos surgem, questionando os impactos da agricultura química.</p>
        </div>
      </li>
      <li class="timeline-item reveal" style="transition-delay:.2s">
        <div class="timeline-dot">🕐</div>
        <span class="timeline-period">2000–2020</span>
        <div class="timeline-card">
          <h3>Tecnologia e Transição</h3>
          <p>Crescimento acelerado do mercado orgânico, bioinsumos e ferramentas digitais para monitoramento agrícola.</p>
        </div>
      </li>
      <li class="timeline-item reveal" style="transition-delay:.3s">
        <div class="timeline-dot">🕐</div>
        <span class="timeline-period">2020–Futuro</span>
        <div class="timeline-card">
          <h3>Agricultura Regenerativa</h3>
          <p>Integração de IA, drones e práticas regenerativas para produzir alimentos saudáveis sem comprometer o planeta.</p>
        </div>
      </li>
    </ol>

  </div>
</section>


<!-- ══ CONTATO / NEWSLETTER ══ -->
<section id="contato" class="section">
  <div class="container">
    <div class="contato-card reveal">
      <h2>Receba Atualizações</h2>
      <p>Junte-se à nossa comunidade e receba novidades sobre inovações na agricultura ecológica diretamente no seu e-mail.</p>
      <div class="form-max">
        <form id="newsletter-form" onsubmit="submitForm(event)" novalidate>
          <div class="form-group">
            <label for="f-name">Nome completo</label>
            <input type="text" id="f-name" placeholder="Seu nome" autocomplete="name">
            <div class="form-error" id="err-name">Nome deve ter pelo menos 2 caracteres.</div>
          </div>
          <div class="form-group">
            <label for="f-email">E-mail</label>
            <input type="email" id="f-email" placeholder="seu@email.com" autocomplete="email">
            <div class="form-error" id="err-email">Digite um e-mail válido.</div>
          </div>
          <button type="submit" class="btn-submit">📩 &nbsp;Inscrever-se</button>
        </form>
        <div class="form-success" id="form-success">
          ✅ &nbsp;Obrigado! Você se inscreveu com sucesso na Newsletter Sustentável.
        </div>
      </div>
    </div>
  </div>
</section>


<!-- ══ FOOTER ══ -->
<footer>
  <div class="f-icon">🌿</div>
  <p class="tagline">Cultivar o futuro é uma responsabilidade compartilhada.</p>
  <p class="copy">&copy; <span id="year"></span> Futuro da Terra — Todos os direitos reservados.</p>
</footer>


<script>
  /* ── Ano dinâmico ── */
  document.getElementById('year').textContent = new Date().getFullYear();

  /* ── Navbar scroll ── */
  const navbar = document.getElementById('navbar');
  window.addEventListener('scroll', () => {
    navbar.classList.toggle('scrolled', window.scrollY > 40);
  });

  /* ── Scroll suave ── */
  function scrollTo(id) {
    closeMobileMenu();
    const el = document.getElementById(id);
    if (!el) return;
    const offset = 72;
    const top = el.getBoundingClientRect().top + window.scrollY - offset;
    window.scrollTo({ top, behavior: 'smooth' });
  }

  /* ── Mobile menu ── */
  const mobileMenu = document.getElementById('mobile-menu');
  const hamburger  = document.getElementById('hamburger-btn');
  function toggleMenu() {
    const open = mobileMenu.classList.toggle('open');
    hamburger.textContent = open ? '✕' : '☰';
  }
  function closeMobileMenu() {
    mobileMenu.classList.remove('open');
    hamburger.textContent = '☰';
  }

  /* ── IntersectionObserver — reveal animations ── */
  const revealEls = document.querySelectorAll('.reveal, .reveal-left, .reveal-right');
  const observer  = new IntersectionObserver((entries) => {
    entries.forEach(e => { if (e.isIntersecting) { e.target.classList.add('visible'); } });
  }, { rootMargin: '-60px', threshold: 0.1 });
  revealEls.forEach(el => observer.observe(el));

  /* ── Pest Simulator ── */
  const pests = {
    pulgoes:    { title: 'Joaninhas (Controle Biológico)',               desc: 'As joaninhas são predadoras naturais vorazes de pulgões, ajudando a controlar a população de forma orgânica e eficaz, sem necessidade de defensivos químicos.' },
    lagartas:   { title: 'Bacillus thuringiensis (Controle Biológico)', desc: 'Uma bactéria de ocorrência natural que afeta o sistema digestivo de lagartas específicas, sendo inofensiva para humanos, pássaros e insetos benéficos.' },
    mosca:      { title: 'Vespas parasitoides (Controle Biológico)',     desc: 'Pequenas vespas que depositam seus ovos dentro das ninfas da mosca-branca. Uma solução natural e altamente específica.' },
    acaros:     { title: 'Ácaros predadores (Controle Biológico)',       desc: 'Alguns ácaros são benéficos e se alimentam dos ácaros-praga, estabelecendo um equilíbrio natural no ecossistema da plantação.' },
    percevejos: { title: 'Inimigos naturais específicos (Controle Biológico)', desc: 'O uso de predadores e parasitoides nativos ajuda a combater percevejos sem desequilibrar a cadeia alimentar local.' },
    fungo:      { title: 'Trichoderma spp. (Controle Biológico)',        desc: 'Um fungo benéfico que atua como antagonista de fungos patogênicos, colonizando o solo e protegendo as raízes das plantas de forma natural e sustentável.' },
  };

  function showPestResult(val) {
    const box = document.getElementById('pest-result');
    if (!val || !pests[val]) { box.classList.remove('show'); return; }
    document.getElementById('pest-title').textContent = pests[val].title;
    document.getElementById('pest-desc').textContent  = pests[val].desc;
    box.classList.remove('show');
    void box.offsetWidth; /* force reflow for re-animation */
    box.classList.add('show');
  }

  /* ── Newsletter form ── */
  function submitForm(e) {
    e.preventDefault();
    const name  = document.getElementById('f-name');
    const email = document.getElementById('f-email');
    const errN  = document.getElementById('err-name');
    const errE  = document.getElementById('err-email');
    let valid   = true;

    errN.classList.remove('show');
    errE.classList.remove('show');

    if (name.value.trim().length < 2) { errN.classList.add('show'); valid = false; }
    if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email.value.trim())) { errE.classList.add('show'); valid = false; }

    if (valid) {
      document.getElementById('newsletter-form').style.display = 'none';
      document.getElementById('form-success').classList.add('show');
    }
  }
</script>
</body>
</html>
