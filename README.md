<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Premium - YURI MUSIC</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@700;900&family=Roboto:wght@300;400;700&display=swap" rel="stylesheet">
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }

    body {
      min-height: 100vh;
      background: #000000;
      font-family: 'Roboto', system-ui, sans-serif;
      overflow-x: hidden;
      overflow-y: auto;
      position: relative;
      color: #fff;
      opacity: 0;
      transition: opacity 0.6s ease;
    }

    body.loaded { opacity: 1; }

    body::before {
      content: '';
      position: fixed;
      inset: 0;
      background-image:
        linear-gradient(rgba(100, 80, 255, 0.04) 1px, transparent 1px),
        linear-gradient(90deg, rgba(100, 80, 255, 0.04) 1px, transparent 1px);
      background-size: 50px 50px;
      pointer-events: none;
      z-index: -2;
      opacity: 0.35;
    }

    /* PRELOADER */
    #preloader {
      position: fixed;
      inset: 0;
      background: #000;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      z-index: 9999;
      transition: opacity 0.5s ease, visibility 0.5s ease;
    }

    #preloader.hidden { opacity: 0; visibility: hidden; }

    .loader-logo {
      font-family: 'Orbitron', sans-serif;
      font-size: 5rem;
      font-weight: 900;
      background: linear-gradient(90deg, #00f0ff, #00aaff, #0066ff);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      animation: pulse 1.5s infinite alternate;
      margin-bottom: 16px;
    }

    @keyframes pulse {
      0% { transform: scale(1); opacity: 0.8; }
      100% { transform: scale(1.06); opacity: 1; }
    }

    .loader-text {
      font-size: 1.4rem;
      color: #a78bfa;
      letter-spacing: 4px;
      margin-bottom: 24px;
    }

    .loader-bar {
      width: 280px;
      height: 5px;
      background: rgba(168, 139, 250, 0.2);
      border-radius: 3px;
      overflow: hidden;
    }

    .loader-progress {
      width: 0;
      height: 100%;
      background: linear-gradient(90deg, #a78bfa, #c084fc);
      animation: loading 1.4s ease-in-out forwards;
    }

    @keyframes loading {
      0% { width: 0; }
      100% { width: 100%; }
    }

    /* Navbar */
    .navbar {
      position: fixed;
      top: 0; left: 0; right: 0;
      height: 70px;
      background: rgba(5, 5, 15, 0.88);
      backdrop-filter: blur(12px);
      border-bottom: 1px solid rgba(120, 100, 255, 0.12);
      z-index: 1000;
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 0 50px;
      opacity: 0;
      transform: translateY(-20px);
      transition: all 0.5s ease 1.6s;
    }

    .navbar.loaded { opacity: 1; transform: translateY(0); }

    .logo {
      font-family: 'Orbitron', sans-serif;
      font-size: 1.6rem;
      font-weight: 900;
      background: linear-gradient(90deg, #c084fc, #7c3aed, #a78bfa);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      letter-spacing: 3px;
      text-decoration: none;
    }

    .nav-menu { display: flex; gap: 40px; align-items: center; }

    .nav-menu a {
      color: #d0d0ff;
      text-decoration: none;
      font-size: 1rem;
      font-weight: 500;
      transition: all 0.3s ease;
      position: relative;
    }

    .nav-menu a:hover, .nav-menu a.active {
      color: #ffffff;
      text-shadow: 0 0 10px rgba(255, 255, 255, 0.7);
    }

    .nav-menu a::after {
      content: '';
      position: absolute;
      width: 0; height: 2px;
      bottom: -8px; left: 0;
      background: linear-gradient(90deg, #c084fc, #a78bfa);
      transition: width 0.4s ease;
    }

    .nav-menu a:hover::after, .nav-menu a.active::after { width: 100%; }

    .nav-actions { display: flex; gap: 16px; }

    .btn-action {
      padding: 9px 22px;
      border-radius: 50px;
      font-size: 0.95rem;
      font-weight: 600;
      text-decoration: none;
      transition: all 0.3s ease;
    }

    .btn-premium {
      background: linear-gradient(135deg, #7c3aed, #c084fc);
      color: white;
      box-shadow: 0 0 20px rgba(168, 85, 247, 0.5);
    }

    .btn-premium:hover {
      transform: translateY(-3px);
      box-shadow: 0 0 35px rgba(168, 85, 247, 0.8);
    }

    .btn-support {
      background: transparent;
      color: #d0d0ff;
      border: 1px solid rgba(168, 85, 247, 0.5);
    }

    .btn-support:hover {
      background: rgba(168, 85, 247, 0.12);
      color: white;
      border-color: #c084fc;
    }

    /* Dropdown */
    .nav-dropdown { position: relative; display: flex; align-items: center; }

    .nav-dropbtn {
      background: transparent; border: none;
      color: #d0d0ff; font-size: 1rem; font-weight: 500;
      cursor: pointer; transition: all 0.3s ease;
      position: relative; padding: 0;
      display: flex; align-items: center; gap: 8px;
    }

    .nav-dropbtn:hover { color: #ffffff; text-shadow: 0 0 10px rgba(255,255,255,0.7); }

    .nav-dropbtn::after {
      content: ''; position: absolute;
      width: 0; height: 2px; bottom: -8px; left: 0;
      background: linear-gradient(90deg, #c084fc, #a78bfa);
      transition: width 0.4s ease;
    }

    .nav-dropdown:hover .nav-dropbtn::after { width: 100%; }

    .dropdown-panel {
      position: absolute;
      top: calc(100% + 18px); left: 0;
      width: 320px; padding: 12px;
      border-radius: 14px;
      background: rgba(10, 10, 18, 0.92);
      border: 1px solid rgba(168, 85, 247, 0.20);
      box-shadow: 0 25px 80px rgba(0,0,0,0.70);
      backdrop-filter: blur(14px);
      transform: translateY(-10px);
      opacity: 0; visibility: hidden;
      transition: all 0.25s ease;
      z-index: 2000;
    }

    .nav-dropdown.open .dropdown-panel { transform: translateY(0); opacity: 1; visibility: visible; }

    .dropdown-item {
      display: flex; align-items: center; gap: 12px;
      padding: 11px 12px; border-radius: 10px;
      text-decoration: none; transition: all 0.25s ease;
      border: 1px solid transparent;
    }

    .dropdown-item:hover { background: rgba(168, 85, 247, 0.10); border-color: rgba(168, 85, 247, 0.18); }

    .dropdown-ico {
      width: 36px; height: 36px; border-radius: 9px;
      background: rgba(168, 85, 247, 0.12);
      border: 1px solid rgba(168, 85, 247, 0.22);
      display: flex; align-items: center; justify-content: center;
      font-size: 1rem; flex: 0 0 auto;
    }

    .dropdown-text { display: flex; flex-direction: column; gap: 2px; }
    .dropdown-title { font-weight: 700; color: #eaeaff; font-size: 1rem; }
    .dropdown-sub { color: #9aa0c6; font-size: 0.88rem; }

    /* Hamburger */
    .hamburger {
      display: none; flex-direction: column; gap: 5px;
      cursor: pointer; padding: 8px;
      border: none; background: transparent;
    }

    .hamburger span {
      display: block; width: 24px; height: 2px;
      background: #d0d0ff; border-radius: 2px;
      transition: all 0.3s ease;
    }

    .hamburger.open span:nth-child(1) { transform: translateY(7px) rotate(45deg); }
    .hamburger.open span:nth-child(2) { opacity: 0; }
    .hamburger.open span:nth-child(3) { transform: translateY(-7px) rotate(-45deg); }

    .mobile-menu {
      display: none;
      position: fixed;
      top: 70px; left: 0; right: 0;
      background: rgba(5, 5, 15, 0.97);
      backdrop-filter: blur(16px);
      border-bottom: 1px solid rgba(168, 85, 247, 0.2);
      padding: 24px 30px;
      flex-direction: column; gap: 20px;
      z-index: 999;
      transform: translateY(-20px); opacity: 0;
      transition: all 0.3s ease;
    }

    .mobile-menu.open { transform: translateY(0); opacity: 1; display: flex; }

    .mobile-menu a {
      color: #d0d0ff; text-decoration: none;
      font-size: 1.1rem; font-weight: 500;
      padding: 12px 0;
      border-bottom: 1px solid rgba(168, 85, 247, 0.1);
      transition: color 0.3s ease;
    }

    .mobile-menu a:hover, .mobile-menu a.active { color: #fff; }

    .mobile-menu-actions { display: flex; gap: 12px; padding-top: 8px; }
    .mobile-menu-actions .btn-action { flex: 1; text-align: center; }

    /* Stars & Meteor */
    .stars { position: fixed; inset: 0; pointer-events: none; z-index: -1; }

    .star {
      position: absolute; background: #ffffff; border-radius: 50%;
      box-shadow: 0 0 8px rgba(255,255,255,0.8), 0 0 16px rgba(255,255,255,0.4);
      animation: twinkle 6s infinite alternate; opacity: 0.9;
    }

    @keyframes twinkle {
      0%,100% { opacity: 0.5; transform: scale(0.8); }
      50% { opacity: 1; transform: scale(1.2); }
    }

    .meteor {
      position: absolute; width: 5px; height: 5px;
      background: #ffffff; border-radius: 50%;
      box-shadow: 0 0 20px 8px rgba(255,255,255,0.95), 0 0 50px 15px rgba(255,255,255,0.7);
      transform: translate(-50%, -50%) rotate(45deg);
      pointer-events: none; animation: meteor linear forwards;
    }

    .meteor::after {
      content: ''; position: absolute;
      top: 50%; left: -220px; width: 220px; height: 3.5px;
      background: linear-gradient(to left, transparent, rgba(255,255,255,0.9));
      transform: translateY(-50%); filter: blur(1.8px);
    }

    @keyframes meteor {
      0% { transform: translate(-50%, -50%) translate(0,0) rotate(45deg); opacity: 1; }
      100% { transform: translate(-50%, -50%) translate(-1200px,1200px) rotate(45deg); opacity: 0; }
    }

    /* Hero */
    .hero-section {
      padding: 160px 60px 80px;
      text-align: center;
      position: relative; z-index: 5;
    }

    .hero-badge {
      display: inline-block;
      padding: 11px 26px;
      background: linear-gradient(135deg, #fbbf24, #f59e0b);
      color: #000;
      font-weight: 700;
      border-radius: 50px;
      font-size: 1.05rem;
      margin-bottom: 28px;
      box-shadow: 0 0 30px rgba(251, 191, 36, 0.6);
    }

    .hero-title {
      font-family: 'Orbitron', sans-serif;
      font-size: 5rem;
      font-weight: 900;
      letter-spacing: 6px;
      background: linear-gradient(90deg, #fbbf24, #f59e0b, #fbbf24);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-size: 200% auto;
      animation: gradientFlow 8s linear infinite;
    }

    @keyframes gradientFlow {
      0% { background-position: 0% center; }
      100% { background-position: 200% center; }
    }

    .hero-subtitle {
      font-size: 1.4rem;
      color: #b0b0ff;
      max-width: 800px;
      margin: 24px auto 60px;
      line-height: 1.7;
    }

    /* Pricing */
    .pricing-container {
      max-width: 1300px;
      margin: 0 auto;
      padding: 0 60px 120px;
      position: relative; z-index: 2;
    }

    .pricing-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 40px;
      margin-bottom: 100px;
    }

    .pricing-card {
      background: rgba(15, 10, 45, 0.65);
      backdrop-filter: blur(18px);
      border: 1px solid rgba(168,85,247,0.35);
      border-radius: 28px;
      padding: 48px 40px;
      transition: all 0.6s cubic-bezier(0.22, 1, 0.36, 1);
      position: relative;
      overflow: hidden;
      opacity: 0;
      transform: translateY(80px);
    }

    .pricing-card.visible { opacity: 1; transform: translateY(0); }

    .pricing-card:hover {
      transform: translateY(-16px) scale(1.015);
      box-shadow: 0 35px 90px rgba(168,85,247,0.45);
    }

    .pricing-card.featured {
      border: 2px solid rgba(251, 191, 36, 0.6);
      background: rgba(20, 15, 50, 0.75);
      box-shadow: 0 0 40px rgba(251, 191, 36, 0.3);
    }

    .pricing-card.featured:hover {
      border-color: rgba(251, 191, 36, 0.95);
      box-shadow: 0 40px 110px rgba(251, 191, 36, 0.55);
    }

    .pricing-card::before {
      content: ''; position: absolute; inset: 0;
      background: radial-gradient(circle at 50% 50%, rgba(168,85,247,0.28), transparent 70%);
      opacity: 0; transition: opacity 0.6s ease;
    }

    .pricing-card:hover::before { opacity: 1; }

    .pricing-card.featured::before {
      background: radial-gradient(circle at 50% 50%, rgba(251, 191, 36, 0.2), transparent 70%);
    }

    .featured-badge {
      position: absolute;
      top: -14px; right: 28px;
      background: linear-gradient(135deg, #fbbf24, #f59e0b);
      color: #000;
      padding: 9px 22px;
      border-radius: 50px;
      font-weight: 800;
      font-size: 0.92rem;
      box-shadow: 0 8px 25px rgba(251, 191, 36, 0.55);
      transform: rotate(6deg);
    }

    .pricing-icon {
      font-size: 4rem;
      margin-bottom: 24px;
      display: block;
    }

    .pricing-name {
      font-family: 'Orbitron', sans-serif;
      font-size: 2.1rem;
      font-weight: 900;
      color: #ffffff;
      margin-bottom: 12px;
    }

    .pricing-description {
      font-size: 1.05rem;
      color: #b0b0d0;
      margin-bottom: 28px;
      line-height: 1.6;
    }

    .pricing-price { margin-bottom: 32px; }

    .price-amount {
      font-family: 'Orbitron', sans-serif;
      font-size: 4.2rem;
      font-weight: 900;
      color: #ffffff;
      line-height: 1;
    }

    .price-currency { font-size: 2rem; color: #a78bfa; }
    .price-period { font-size: 1.1rem; color: #9090b0; margin-top: 6px; }

    .pricing-features { list-style: none; margin-bottom: 36px; }

    .pricing-features li {
      padding: 13px 0;
      font-size: 1.05rem;
      color: #d0d0ff;
      display: flex;
      align-items: center;
      gap: 12px;
      border-bottom: 1px solid rgba(168,85,247,0.08);
    }

    .pricing-features li:last-child { border-bottom: none; }

    .pricing-features li::before {
      content: '✓';
      color: #4ade80;
      font-weight: bold;
      font-size: 1.3rem;
      flex-shrink: 0;
    }

    .pricing-features li.disabled { color: #707090; }
    .pricing-features li.disabled::before { content: '✗'; color: #707090; }

    .pricing-btn {
      width: 100%;
      padding: 18px 36px;
      font-size: 1.2rem;
      font-weight: 700;
      border-radius: 50px;
      border: none;
      cursor: pointer;
      transition: all 0.4s ease;
      text-decoration: none;
      display: inline-block;
      text-align: center;
    }

    .btn-free {
      background: rgba(168, 85, 247, 0.15);
      color: #c084fc;
      border: 2px solid rgba(168, 85, 247, 0.45);
    }

    .btn-free:hover {
      background: rgba(168, 85, 247, 0.3);
      border-color: #c084fc;
      transform: translateY(-3px);
    }

    .btn-premium-plan {
      background: linear-gradient(135deg, #7c3aed, #c084fc);
      color: white;
      box-shadow: 0 0 30px rgba(168, 85, 247, 0.55);
    }

    .btn-premium-plan:hover {
      transform: translateY(-4px);
      box-shadow: 0 0 50px rgba(168, 85, 247, 0.85);
    }

    .btn-featured {
      background: linear-gradient(135deg, #fbbf24, #f59e0b);
      color: #000;
      box-shadow: 0 0 30px rgba(251, 191, 36, 0.55);
    }

    .btn-featured:hover {
      transform: translateY(-4px);
      box-shadow: 0 0 50px rgba(251, 191, 36, 0.85);
    }

    /* Benefits */
    .benefits-section {
      padding: 80px 0;
      text-align: center;
      position: relative; z-index: 2;
    }

    .benefits-title {
      font-family: 'Orbitron', sans-serif;
      font-size: 3rem;
      font-weight: 900;
      background: linear-gradient(90deg, #a78bfa, #c084fc, #7c3aed);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      margin-bottom: 60px;
    }

    .benefits-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 32px;
    }

    .benefit-card {
      background: rgba(15, 10, 45, 0.65);
      backdrop-filter: blur(18px);
      border: 1px solid rgba(168,85,247,0.28);
      border-radius: 24px;
      padding: 40px 34px;
      transition: all 0.5s ease;
      opacity: 0;
      transform: translateY(60px);
    }

    .benefit-card.visible { opacity: 1; transform: translateY(0); }

    .benefit-card:hover {
      transform: translateY(-12px);
      border-color: rgba(168,85,247,0.75);
      box-shadow: 0 22px 60px rgba(168,85,247,0.35);
    }

    .benefit-icon {
      font-size: 3.5rem;
      margin-bottom: 20px;
      display: block;
    }

    .benefit-title {
      font-family: 'Orbitron', sans-serif;
      font-size: 1.6rem;
      font-weight: 800;
      color: #ffffff;
      margin-bottom: 14px;
    }

    .benefit-description { font-size: 1.05rem; color: #d0d0ff; line-height: 1.7; }

    /* Footer */
    .footer-section {
      position: relative;
      background: rgba(10, 8, 20, 0.95);
      border-top: 1px solid rgba(168,85,247,0.2);
      padding: 60px 60px 40px;
      z-index: 10;
    }

    .footer-top {
      display: flex; justify-content: space-between;
      align-items: center; margin-bottom: 50px;
    }

    .footer-brand { display: flex; align-items: center; gap: 16px; }

    .footer-logo-container {
      width: 60px; height: 60px; border-radius: 16px;
      overflow: hidden;
      border: 2px solid rgba(168,85,247,0.3);
      box-shadow: 0 0 20px rgba(168,85,247,0.3);
    }

    .footer-logo-img { width: 100%; height: 100%; object-fit: cover; }

    .footer-brand-name {
      font-family: 'Orbitron', sans-serif; font-size: 1.6rem; font-weight: 900;
      background: linear-gradient(90deg, #a78bfa, #c084fc);
      -webkit-background-clip: text; -webkit-text-fill-color: transparent;
      letter-spacing: 2px;
    }

    .footer-brand-tagline { font-size: 0.9rem; color: #9090b0; }

    .footer-stats { display: flex; gap: 50px; }
    .footer-stat { text-align: center; }

    .footer-stat-number {
      font-size: 2rem; font-weight: 700; color: #ffffff;
      font-family: 'Orbitron', sans-serif; margin-bottom: 6px;
    }

    .footer-stat-label { font-size: 0.85rem; color: #9090b0; text-transform: uppercase; letter-spacing: 1px; }

    .footer-divider {
      width: 100%; height: 1px;
      background: linear-gradient(90deg, transparent, rgba(168,85,247,0.3), transparent);
      margin: 40px 0;
    }

    .footer-content {
      display: flex; justify-content: space-between;
      gap: 60px; margin-bottom: 40px; max-width: 800px;
    }

    .footer-column { flex: 1; }

    .footer-heading {
      font-family: 'Orbitron', sans-serif; font-size: 1.1rem; font-weight: 700;
      color: #ffffff; margin-bottom: 20px; letter-spacing: 1px;
    }

    .footer-links { list-style: none; display: flex; flex-direction: column; gap: 13px; }

    .footer-links li a {
      color: #b0b0d0; text-decoration: none;
      font-size: 1rem; transition: all 0.3s ease; display: inline-block;
    }

    .footer-links li a:hover { color: #ffffff; transform: translateX(5px); }

    .footer-bottom {
      display: flex; justify-content: space-between; align-items: center;
      padding-top: 30px;
      border-top: 1px solid rgba(168,85,247,0.15);
    }

    .footer-copyright { font-size: 0.95rem; color: #8080a0; }

    .footer-socials { display: flex; gap: 16px; }

    .footer-social-link {
      width: 42px; height: 42px; border-radius: 50%;
      background: rgba(168,85,247,0.1);
      border: 1px solid rgba(168,85,247,0.3);
      display: flex; align-items: center; justify-content: center;
      color: #b0b0d0; transition: all 0.35s ease; text-decoration: none;
    }

    .footer-social-link:hover {
      background: rgba(168,85,247,0.25); border-color: rgba(168,85,247,0.6);
      color: #ffffff; transform: translateY(-4px);
      box-shadow: 0 8px 25px rgba(168,85,247,0.4);
    }

    /* Responsive */
    @media (max-width: 1100px) {
      .pricing-grid { grid-template-columns: 1fr; max-width: 520px; margin: 0 auto 100px; }
      .benefits-grid { grid-template-columns: repeat(2, 1fr); }
    }

    @media (max-width: 1024px) {
      .hero-title { font-size: 3.8rem; }
      .hero-section { padding: 140px 30px 60px; }
      .pricing-container { padding: 0 30px 100px; }
      .benefits-title { font-size: 2.4rem; }
      .navbar { padding: 0 30px; }
      .nav-menu { gap: 24px; }
      .footer-top { flex-direction: column; gap: 30px; text-align: center; }
      .footer-stats { gap: 30px; justify-content: center; flex-wrap: wrap; }
      .footer-content { flex-direction: column; gap: 36px; max-width: 100%; }
      .footer-bottom { flex-direction: column; gap: 16px; text-align: center; }
    }

    @media (max-width: 768px) {
      .navbar { padding: 0 20px; }
      .nav-menu, .nav-actions { display: none; }
      .hamburger { display: flex; }

      .hero-title { font-size: 2.6rem; letter-spacing: 2px; }
      .hero-subtitle { font-size: 1.05rem; }
      .hero-section { padding: 120px 20px 50px; }

      .pricing-container { padding: 0 20px 80px; }
      .pricing-grid { max-width: 100%; }
      .benefits-grid { grid-template-columns: 1fr; }

      .footer-section { padding: 40px 20px 30px; }
      .footer-stat-number { font-size: 1.6rem; }
      .footer-stats { gap: 24px; }
    }

    @media (max-width: 480px) {
      .hero-title { font-size: 2rem; }
      .loader-logo { font-size: 3rem; }
      .loader-text { font-size: 1rem; }
      .price-amount { font-size: 3.2rem; }
    }
  </style>
</head>
<body>

  <div id="preloader">
    <div class="loader-logo">YURI MUSIC</div>
    <div class="loader-text">LOADING PREMIUM...</div>
    <div class="loader-bar">
      <div class="loader-progress"></div>
    </div>
  </div>

  <nav class="navbar" id="navbar">
    <a href="index.html" class="logo">YURI MUSIC</a>

    <div class="nav-menu">
      <a href="index.html">Home</a>
      <a href="fitur.html">Fitur</a>
      <a href="command.html">Command</a>
      <a href="support.html">Support</a>
      <a href="premium.html" class="active">Premium</a>

      <div class="nav-dropdown" id="resourcesDropdown">
        <button class="nav-dropbtn" type="button">
          Sumber Daya <span>▾</span>
        </button>
        <div class="dropdown-panel">
          <a class="dropdown-item" href="team.html">
            <span class="dropdown-ico">👥</span>
            <span class="dropdown-text"><span class="dropdown-title">Team</span><span class="dropdown-sub">Kenali tim kami</span></span>
          </a>
          <a class="dropdown-item" href="tos.html">
            <span class="dropdown-ico">📜</span>
            <span class="dropdown-text"><span class="dropdown-title">TOS</span><span class="dropdown-sub">Syarat & ketentuan</span></span>
          </a>
          <a class="dropdown-item" href="privacy.html">
            <span class="dropdown-ico">🔒</span>
            <span class="dropdown-text"><span class="dropdown-title">Privacy Policy</span><span class="dropdown-sub">Privasi & data</span></span>
          </a>
          <a class="dropdown-item" href="dokumentasi.html">
            <span class="dropdown-ico">📚</span>
            <span class="dropdown-text"><span class="dropdown-title">Dokumentasi</span><span class="dropdown-sub">Belajar & tutorial</span></span>
          </a>
        </div>
      </div>
    </div>

    <div class="nav-actions">
      <a href="premium.html" class="btn-action btn-premium">Premium</a>
      <a href="support.html" class="btn-action btn-support">Support</a>
    </div>

    <button class="hamburger" id="hamburger" aria-label="Menu">
      <span></span><span></span><span></span>
    </button>
  </nav>

  <!-- Mobile Menu -->
  <div class="mobile-menu" id="mobileMenu">
    <a href="index.html">Home</a>
    <a href="fitur.html">Fitur</a>
    <a href="command.html">Command</a>
    <a href="support.html">Support</a>
    <a href="premium.html" class="active">Premium</a>
    <a href="team.html">Team</a>
    <a href="tos.html">Terms of Service</a>
    <a href="privacy.html">Privacy Policy</a>
    <a href="dokumentasi.html">Dokumentasi</a>
    <div class="mobile-menu-actions">
      <a href="premium.html" class="btn-action btn-premium">Premium</a>
      <a href="support.html" class="btn-action btn-support">Support</a>
    </div>
  </div>

  <div class="stars" id="stars"></div>

  <!-- Hero -->
  <section class="hero-section">
    <div class="hero-badge">👑 PREMIUM PLANS</div>
    <h1 class="hero-title">UPGRADE NOW</h1>
    <p class="hero-subtitle">
      Buka semua fitur eksklusif dan jadikan pengalaman musik Discord kamu jauh lebih powerful, jernih, dan tak terbatas dengan YURI MUSIC Premium!
    </p>
  </section>

  <!-- Pricing -->
  <div class="pricing-container">

    <div class="pricing-grid">

      <!-- Free -->
      <div class="pricing-card">
        <span class="pricing-icon">🎯</span>
        <h3 class="pricing-name">Free</h3>
        <p class="pricing-description">Cocok untuk server kecil atau yang baru mulai</p>
        <div class="pricing-price">
          <span class="price-currency">Rp</span>
          <span class="price-amount">0</span>
          <div class="price-period">selamanya gratis</div>
        </div>
        <ul class="pricing-features">
          <li>Semua command musik dasar</li>
          <li>Play dari YouTube & SoundCloud</li>
          <li>Queue & playlist standar</li>
          <li>Loop & shuffle</li>
          <li>Audio filter dasar</li>
          <li>Volume control</li>
          <li class="disabled">Mode 24/7 non-stop</li>
          <li class="disabled">Spotify support</li>
          <li class="disabled">Playlist tak terbatas</li>
          <li class="disabled">Support prioritas</li>
        </ul>
        <a href="#" class="pricing-btn btn-free">Mulai Gratis</a>
      </div>

      <!-- Premium -->
      <div class="pricing-card featured">
        <span class="featured-badge">⭐ PALING POPULER</span>
        <span class="pricing-icon">💎</span>
        <h3 class="pricing-name">Premium</h3>
        <p class="pricing-description">Ideal untuk server yang ingin musik tanpa batas</p>
        <div class="pricing-price">
          <span class="price-currency">Rp</span>
          <span class="price-amount">49K</span>
          <div class="price-period">per bulan</div>
        </div>
        <ul class="pricing-features">
          <li>Semua fitur Free</li>
          <li>Mode 24/7 non-stop</li>
          <li>Spotify & SoundCloud full</li>
          <li>Playlist tak terbatas</li>
          <li>Semua audio filter premium</li>
          <li>Kualitas audio HQ</li>
          <li>Tanpa cooldown command</li>
          <li>Autoplay cerdas</li>
          <li>Support prioritas 24/7</li>
          <li>Akses awal fitur baru</li>
        </ul>
        <a href="#" class="pricing-btn btn-featured">Berlangganan Sekarang</a>
      </div>

      <!-- Lifetime -->
      <div class="pricing-card">
        <span class="pricing-icon">🏆</span>
        <h3 class="pricing-name">Lifetime</h3>
        <p class="pricing-description">Bayar sekali, nikmati musik selamanya!</p>
        <div class="pricing-price">
          <span class="price-currency">Rp</span>
          <span class="price-amount">499K</span>
          <div class="price-period">sekali bayar</div>
        </div>
        <ul class="pricing-features">
          <li>Semua fitur Premium</li>
          <li>Akses seumur hidup</li>
          <li>Tanpa biaya bulanan</li>
          <li>Bass boost & EQ eksklusif</li>
          <li>Badge Lifetime eksklusif</li>
          <li>Support VIP selamanya</li>
          <li>Role khusus di server support</li>
          <li>Akses beta tester</li>
          <li>Hak suara fitur baru</li>
        </ul>
        <a href="#" class="pricing-btn btn-premium-plan">Dapatkan Lifetime</a>
      </div>

    </div>

    <!-- Benefits -->
    <div class="benefits-section">
      <h2 class="benefits-title">KENAPA HARUS PREMIUM?</h2>
      <div class="benefits-grid">

        <div class="benefit-card">
          <span class="benefit-icon">⏰</span>
          <h3 class="benefit-title">Mode 24/7</h3>
          <p class="benefit-description">Bot tetap stay di voice channel tanpa disconnect — musik non-stop tanpa perlu reconnect setiap saat.</p>
        </div>

        <div class="benefit-card">
          <span class="benefit-icon">🎵</span>
          <h3 class="benefit-title">Audio HQ</h3>
          <p class="benefit-description">Nikmati suara lebih jernih dan kencang. Filter bass boost, 8D, nightcore, dan efek premium lainnya.</p>
        </div>

        <div class="benefit-card">
          <span class="benefit-icon">📁</span>
          <h3 class="benefit-title">Playlist Tak Terbatas</h3>
          <p class="benefit-description">Simpan dan muat playlist sebanyak yang kamu mau. Kelola koleksi musik favoritmu tanpa batasan.</p>
        </div>

        <div class="benefit-card">
          <span class="benefit-icon">⚡</span>
          <h3 class="benefit-title">Tanpa Cooldown</h3>
          <p class="benefit-description">Gunakan semua command musik sepuasnya tanpa jeda. Tidak ada batas antrian, skip, atau filter.</p>
        </div>

        <div class="benefit-card">
          <span class="benefit-icon">🎧</span>
          <h3 class="benefit-title">Multi Platform</h3>
          <p class="benefit-description">Putar dari YouTube, Spotify, dan SoundCloud secara penuh. Akses katalog musik terlengkap.</p>
        </div>

        <div class="benefit-card">
          <span class="benefit-icon">👑</span>
          <h3 class="benefit-title">Support VIP</h3>
          <p class="benefit-description">Dapatkan bantuan prioritas dari tim developer dengan respons cepat dan solusi terbaik.</p>
        </div>

      </div>
    </div>

  </div>

  <!-- Footer -->
  <footer class="footer-section">
    <div class="footer-top">
      <div class="footer-brand">
        <div class="footer-logo-container">
          <img src="pp.jpg" alt="YURI MUSIC Logo" class="footer-logo-img">
        </div>
        <div>
          <div class="footer-brand-name">YURI MUSIC</div>
          <div class="footer-brand-tagline">The Ultimate Discord Music Bot</div>
        </div>
      </div>
      <div class="footer-stats">
        <div class="footer-stat">
          <div class="footer-stat-number">--</div>
          <div class="footer-stat-label">Servers</div>
        </div>
        <div class="footer-stat">
          <div class="footer-stat-number">25+</div>
          <div class="footer-stat-label">Commands</div>
        </div>
        <div class="footer-stat">
          <div class="footer-stat-number">24/7</div>
          <div class="footer-stat-label">Uptime</div>
        </div>
      </div>
    </div>

    <div class="footer-divider"></div>

    <div class="footer-content">
      <div class="footer-column">
        <h4 class="footer-heading">YURI MUSIC</h4>
        <ul class="footer-links">
          <li><a href="command.html">Commands</a></li>
          <li><a href="dokumentasi.html">Documentation</a></li>
          <li><a href="team.html">Team</a></li>
        </ul>
      </div>
      <div class="footer-column">
        <h4 class="footer-heading">Community</h4>
        <ul class="footer-links">
          <li><a href="support.html">Support Server</a></li>
          <li><a href="premium.html">Premium</a></li>
        </ul>
      </div>
      <div class="footer-column">
        <h4 class="footer-heading">Legal</h4>
        <ul class="footer-links">
          <li><a href="tos.html">Terms of Service</a></li>
          <li><a href="privacy.html">Privacy Policy</a></li>
        </ul>
      </div>
    </div>

    <div class="footer-bottom">
      <div class="footer-copyright">© 2025 YURI MUSIC. All rights reserved</div>
      <div class="footer-socials">
        <a href="mailto:contact@yurimusic.com" class="footer-social-link" aria-label="Email">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <circle cx="12" cy="12" r="4"/>
            <path d="M16 8v5a3 3 0 0 0 6 0v-1a10 10 0 1 0-3.92 7.94"/>
          </svg>
        </a>
        <a href="https://discord.gg/xxxxxxxxxx" class="footer-social-link" aria-label="Discord">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="currentColor">
            <path d="M20.317 4.37a19.791 19.791 0 0 0-4.885-1.515a.074.074 0 0 0-.079.037c-.21.375-.444.864-.608 1.25a18.27 18.27 0 0 0-5.487 0a12.64 12.64 0 0 0-.617-1.25a.077.077 0 0 0-.079-.037A19.736 19.736 0 0 0 3.677 4.37a.07.07 0 0 0-.032.027C.533 9.046-.32 13.58.099 18.057a.082.082 0 0 0 .031.057a19.9 19.9 0 0 0 5.993 3.03a.078.078 0 0 0 .084-.028a14.09 14.09 0 0 0 1.226-1.994a.076.076 0 0 0-.041-.106a13.107 13.107 0 0 1-1.872-.892a.077.077 0 0 1-.008-.128a10.2 10.2 0 0 0 .372-.292a.074.074 0 0 1 .077-.01c3.928 1.793 8.18 1.793 12.062 0a.074.074 0 0 1 .078.01c.12.098.246.198.373.292a.077.077 0 0 1-.006.127a12.299 12.299 0 0 1-1.873.892a.077.077 0 0 0-.041.107c.36.698.772 1.362 1.225 1.993a.076.076 0 0 0 .084.028a19.839 19.839 0 0 0 6.002-3.03a.077.077 0 0 0 .032-.054c.5-5.177-.838-9.674-3.549-13.66a.061.061 0 0 0-.031-.03z"/>
          </svg>
        </a>
      </div>
    </div>
  </footer>

  <script>
    // Preloader cepat 1.6s
    window.addEventListener('load', () => {
      document.body.classList.add('loaded');
      setTimeout(() => {
        document.getElementById('preloader').classList.add('hidden');
        document.getElementById('navbar').classList.add('loaded');
      }, 1600);
    });

    // Stars
    function createStars() {
      const container = document.getElementById('stars');
      for (let i = 0; i < 180; i++) {
        const star = document.createElement('div');
        star.className = 'star';
        const size = Math.random() * 3 + 1;
        star.style.cssText = `width:${size}px;height:${size}px;left:${Math.random()*100}vw;top:${Math.random()*100}vh;animation-delay:${Math.random()*7}s`;
        container.appendChild(star);
      }
    }

    function createMeteor() {
      const meteor = document.createElement('div');
      meteor.className = 'meteor';
      meteor.style.left = (80 + Math.random() * 20) + 'vw';
      meteor.style.top = (-30 - Math.random() * 40) + 'vh';
      const dur = 3.5 + Math.random() * 4;
      meteor.style.animationDuration = dur + 's';
      document.body.appendChild(meteor);
      setTimeout(() => meteor.remove(), dur * 1000 + 2000);
    }

    createStars();
    setInterval(createMeteor, 900 + Math.random() * 1100);
    for (let i = 0; i < 6; i++) setTimeout(createMeteor, i * 350 + Math.random() * 600);

    // Scroll animation
    const observer = new IntersectionObserver((entries) => {
      entries.forEach((entry, idx) => {
        if (entry.isIntersecting) {
          setTimeout(() => entry.target.classList.add('visible'), idx * 80);
        }
      });
    }, { threshold: 0.12 });

    document.querySelectorAll('.pricing-card, .benefit-card').forEach(el => observer.observe(el));

    // Dropdown
    (() => {
      const wrap = document.getElementById("resourcesDropdown");
      if (!wrap) return;
      const btn = wrap.querySelector(".nav-dropbtn");
      wrap.addEventListener("mouseenter", () => wrap.classList.add("open"));
      wrap.addEventListener("mouseleave", () => wrap.classList.remove("open"));
      btn.addEventListener("click", (e) => { e.stopPropagation(); wrap.classList.toggle("open"); });
      document.addEventListener("click", () => wrap.classList.remove("open"));
      document.addEventListener("keydown", (e) => { if (e.key === "Escape") wrap.classList.remove("open"); });
    })();

    // Hamburger
    const hamburger = document.getElementById('hamburger');
    const mobileMenu = document.getElementById('mobileMenu');

    hamburger.addEventListener('click', () => {
      hamburger.classList.toggle('open');
      if (mobileMenu.classList.contains('open')) {
        mobileMenu.classList.remove('open');
        setTimeout(() => { mobileMenu.style.display = 'none'; }, 300);
      } else {
        mobileMenu.style.display = 'flex';
        requestAnimationFrame(() => mobileMenu.classList.add('open'));
      }
    });

    document.addEventListener('click', (e) => {
      if (!hamburger.contains(e.target) && !mobileMenu.contains(e.target)) {
        hamburger.classList.remove('open');
        mobileMenu.classList.remove('open');
        setTimeout(() => { mobileMenu.style.display = 'none'; }, 300);
      }
    });
  </script>
</body>
</html>