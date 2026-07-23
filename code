<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NOCHTRA — INNER TERRAIN // AFTER LIGHT</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght@0,9..144,400;0,9..144,500;1,9..144,400;1,9..144,500&family=Inter:wght@300;400;500&family=IBM+Plex+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
  :root{
    --void:#050505;
    --void-2:#0c0b0a;
    --bone:#D9D3C4;
    --ash:#726f66;
    --red:#8a1c22;
    --red-dim:#3a0f12;
    --red-glow:rgba(138,28,34,0.18);
    --line:rgba(217,211,196,0.1);
  }
  *{box-sizing:border-box; margin:0; padding:0;}
  html{ scroll-behavior:smooth; }
  body{
    background:var(--void);
    color:var(--bone);
    font-family:'Inter', sans-serif;
    font-weight:300;
    line-height:1.75;
    overflow-x:hidden;
  }
  ::selection{ background:var(--red); color:var(--bone); }
  a{ color:inherit; }
  em{ font-style:italic; }

  .mono{ font-family:'IBM Plex Mono', monospace; letter-spacing:0.14em; text-transform:uppercase; }
  .serif{ font-family:'Fraunces', serif; font-style:italic; font-weight:400; }

  .wrap{ max-width:760px; margin:0 auto; padding:0 30px; }
  @media (max-width:600px){ .wrap{ padding:0 24px; } }

  canvas#terrain{ position:fixed; inset:0; width:100%; height:100%; pointer-events:none; z-index:0; opacity:0.75; }
  .grain{
    position:fixed; inset:0; pointer-events:none; z-index:1; opacity:0.9;
    background:repeating-linear-gradient(to bottom, rgba(255,255,255,0.025) 0px, transparent 1px, transparent 3px);
  }
  .vignette{
    position:fixed; inset:0; pointer-events:none; z-index:2;
    background:radial-gradient(ellipse at center, transparent 32%, rgba(0,0,0,0.8) 100%);
  }
  .core-glow{
    position:fixed; inset:0; pointer-events:none; z-index:1; opacity:0;
    background:radial-gradient(ellipse at 50% 100%, rgba(138,28,34,0.35), transparent 55%);
    transition:opacity .4s ease;
  }

  nav{
    position:fixed; top:0; left:0; right:0; z-index:41;
    display:flex; justify-content:space-between; align-items:center;
    padding:22px 30px; font-size:0.66rem; color:var(--ash);
  }
  @media (max-width:420px){ nav{ padding:16px 20px; font-size:0.58rem; } }
  .depth-readout{ display:flex; align-items:center; gap:12px; }
  .depth-pulse{ width:6px; height:6px; border-radius:50%; background:var(--red); animation:heartbeat 1.8s ease-in-out infinite; }
  @keyframes heartbeat{ 0%,100%{ transform:scale(0.7); opacity:0.5; } 50%{ transform:scale(1.15); opacity:1; } }
  .depth-val{ font-family:'IBM Plex Mono', monospace; color:var(--bone); font-size:0.68rem; min-width:74px; text-align:right; }
  .depth-track{ width:70px; height:1px; background:var(--line); position:relative; }
  .depth-fill{ position:absolute; top:0; left:0; height:1px; background:var(--red); width:0%; transition:width .1s linear; }
  @media (max-width:520px){ .depth-track{ width:44px; } }

  .hero{
    position:relative; z-index:3; min-height:100svh;
    display:flex; flex-direction:column; align-items:center; justify-content:center;
    text-align:center; padding:0 24px; cursor:pointer; user-select:none;
  }
  .hero .coll{ font-size:0.66rem; color:var(--ash); margin-bottom:10px; transition:opacity 2.2s ease; }
  .hero .chapter{ font-size:0.66rem; color:var(--ash); margin-bottom:30px; transition:opacity 2.2s ease; }
  .hero h1{
    font-family:'Fraunces', serif; font-weight:500; font-style:normal;
    font-size:clamp(2.6rem, 9vw, 5.6rem); letter-spacing:-0.01em; line-height:1.02;
    position:relative; transition:opacity 2.2s ease;
  }
  .hero h1.glitching{ animation:glitchPulse 0.28s steps(3) both; }
  @keyframes glitchPulse{
    0%{ transform:translate(0,0) skewX(0); text-shadow:none; }
    20%{ transform:translate(-2px,1px) skewX(-2deg); text-shadow:2px 0 var(--red), -2px 0 rgba(217,211,196,0.4); }
    40%{ transform:translate(2px,-1px) skewX(2deg); text-shadow:-3px 0 var(--red), 3px 0 rgba(217,211,196,0.4); }
    60%{ transform:translate(-1px,0) skewX(-1deg); text-shadow:1px 0 var(--red); }
    100%{ transform:translate(0,0) skewX(0); text-shadow:none; }
  }
  .hero .quote{ margin-top:34px; max-width:32ch; font-size:clamp(1rem, 2.1vw, 1.25rem); color:rgba(217,211,196,0.82); transition:opacity 2.2s ease; }

  .hero.fading .coll, .hero.fading .chapter, .hero.fading .quote{ opacity:0.05; }
  .hero.fading h1{ opacity:0.06; }
  .hero.fading .scroll-cue{ opacity:0.05; }
  .hero::before{
    content:''; position:absolute; inset:0; pointer-events:none; z-index:-1;
    background:radial-gradient(ellipse at center, transparent 20%, rgba(138,28,34,0.16) 100%);
    opacity:0; transition:opacity 2.4s ease;
  }
  .hero.fading::before{ opacity:1; }

  .hero-reveal{
    position:absolute; top:50%; left:50%; transform:translate(-50%,-50%);
    font-family:'Fraunces', serif; font-style:italic; font-weight:400;
    font-size:clamp(1.3rem, 3.4vw, 2.2rem); color:var(--bone);
    opacity:0; transition:opacity 2s ease .4s; pointer-events:none; white-space:nowrap; text-align:center;
  }
  .hero.fading .hero-reveal{ opacity:0.92; }

  .hold-hint{
    position:absolute; bottom:88px; left:50%; transform:translateX(-50%);
    font-size:0.58rem; color:var(--ash); letter-spacing:0.18em;
    opacity:0; transition:opacity 1s ease; pointer-events:none;
  }
  .hold-hint.show{ opacity:0.6; }
  .hold-hint.hide{ opacity:0; }

  .scroll-cue{
    position:absolute; bottom:32px; left:50%; transform:translateX(-50%);
    font-size:0.6rem; color:var(--ash); letter-spacing:0.22em;
    display:flex; flex-direction:column; align-items:center; gap:8px;
    transition:opacity 2.2s ease;
  }
  .scroll-cue .stem{ width:1px; height:30px; background:var(--ash); animation:stem 2.6s ease-in-out infinite; transform-origin:top; }
  @keyframes stem{ 0%,100%{transform:scaleY(.4);} 50%{transform:scaleY(1);} }

  .touch-hint{
    position:fixed; bottom:24px; right:26px; z-index:20;
    font-size:0.58rem; color:var(--ash); letter-spacing:0.18em;
    opacity:0; transition:opacity 1.2s ease; pointer-events:none;
  }
  .touch-hint.show{ opacity:0.7; }
  .touch-hint.hide{ opacity:0; }
  @media (max-width:600px){ .touch-hint{ display:none; } }

  /* ---------------- BREATHING GUIDE — persistent grounding tool ---------------- */
  .breathe-guide{
    position:fixed; bottom:24px; left:26px; z-index:44;
    display:flex; align-items:center; gap:10px; cursor:pointer;
  }
  .breathe-ring{
    width:20px; height:20px; border-radius:50%;
    border:1px solid rgba(217,211,196,0.5);
    transition:transform 4s ease-in-out, border-color 4s ease-in-out;
  }
  .breathe-ring.exhale{ transform:scale(0.55); border-color:rgba(217,211,196,0.28); }
  .breathe-ring.inhale{ transform:scale(1.15); border-color:rgba(138,28,34,0.6); }
  .breathe-label{
    font-family:'IBM Plex Mono', monospace; font-size:0.58rem; color:var(--ash);
    letter-spacing:0.14em; text-transform:uppercase;
  }
  @media (max-width:600px){ .breathe-guide{ bottom:18px; left:18px; } .breathe-label{ display:none; } }

  .ground-overlay{
    position:fixed; inset:0; z-index:43; background:var(--void);
    opacity:0; pointer-events:none; transition:opacity 1.4s ease;
  }
  .ground-overlay.active{ opacity:0.88; pointer-events:auto; }
  .ground-overlay .ground-msg{
    position:absolute; top:50%; left:50%; transform:translate(-50%,-50%);
    font-family:'Fraunces', serif; font-style:italic; font-size:clamp(1.1rem,2.6vw,1.6rem);
    color:var(--bone); opacity:0; transition:opacity 1s ease .6s; text-align:center; width:80%; max-width:32ch;
  }
  .ground-overlay.active .ground-msg{ opacity:0.85; }

  section{ position:relative; z-index:3; padding:130px 0; }
  @media (max-width:600px){ section{ padding:90px 0; } }

  .layer-label{ font-size:0.64rem; color:var(--ash); margin-bottom:26px; display:flex; align-items:baseline; gap:14px; }
  .layer-label .num{ color:var(--red); }
  .layer-label::after{ content:''; flex:1; height:1px; background:var(--line); }

  section p{ font-size:1.02rem; color:rgba(217,211,196,0.86); max-width:56ch; }
  section p + p{ margin-top:18px; }

  .lede{
    font-family:'Fraunces', serif; font-style:italic; font-weight:400;
    font-size:clamp(1.25rem, 2.4vw, 1.6rem); max-width:22ch; margin-bottom:32px; color:var(--bone);
  }

  blockquote{
    margin:38px 0; padding-left:24px; border-left:1px solid var(--red);
    font-family:'Fraunces', serif; font-style:italic; font-weight:400;
    font-size:clamp(1.1rem, 2vw, 1.35rem); color:var(--bone); max-width:44ch;
  }

  /* ---------------- NOISE — scattered overthinking clutter ---------------- */
  .noise-field{
    position:relative; margin-top:36px; padding:34px 6px;
    border-top:1px solid var(--line); border-bottom:1px solid var(--line);
    display:flex; flex-wrap:wrap; gap:14px 20px; align-items:center;
  }
  .noise-field::before{
    content:''; position:absolute; inset:-40px; z-index:-1; pointer-events:none;
    background:radial-gradient(ellipse at 30% 40%, var(--red-glow), transparent 60%);
    opacity:0.6;
  }
  .noise-field::after{
    content:''; position:absolute; inset:0; z-index:-1; pointer-events:none;
    background:radial-gradient(circle 220px at var(--mx,50%) var(--my,50%), rgba(138,28,34,0.16), transparent 70%);
    opacity:0; transition:opacity .3s ease;
  }
  .noise-field:hover::after{ opacity:1; }
  .n-frag{
    font-family:'IBM Plex Mono', monospace; font-size:0.78rem;
    color:rgba(217,211,196,0.55); padding:6px 10px; border:1px solid var(--line);
    background:rgba(217,211,196,0.02); white-space:nowrap;
    transition:transform .18s ease, color .18s ease, border-color .18s ease, background .18s ease;
    cursor:default;
  }
  .n-frag:hover{
    color:var(--bone); border-color:rgba(138,28,34,0.55); background:rgba(138,28,34,0.1);
    transform:rotate(0deg) scale(1.06) !important;
  }
  .n-frag.strike{ text-decoration:line-through; text-decoration-color:rgba(217,211,196,0.3); }
  .n-frag.hot{ color:var(--bone); border-color:rgba(138,28,34,0.5); background:rgba(138,28,34,0.08); }
  .n-frag.joke{ color:var(--bone); border-color:rgba(217,211,196,0.28); font-style:italic; font-family:'Fraunces',serif; text-transform:none; letter-spacing:0; white-space:normal; }
  .n-frag:nth-child(2n){ transform:rotate(-1.4deg); }
  .n-frag:nth-child(3n){ transform:rotate(1.8deg); }
  .n-frag:nth-child(5n){ transform:rotate(-0.6deg); }

  .meaning-grid{ display:grid; grid-template-columns:1fr 1fr; gap:1px; background:var(--line); border:1px solid var(--line); margin-top:40px; }
  @media (max-width:640px){ .meaning-grid{ grid-template-columns:1fr; } }
  .meaning-col{ background:var(--void); padding:36px 32px; }
  .meaning-col h3{ font-family:'IBM Plex Mono', monospace; font-size:0.72rem; letter-spacing:0.1em; text-transform:uppercase; color:var(--bone); margin-bottom:20px; }
  .meaning-col ul{ list-style:none; }
  .meaning-col li{ font-size:0.92rem; color:rgba(217,211,196,0.72); padding:9px 0; border-top:1px solid var(--line); }
  .meaning-col li:first-child{ border-top:none; }

  .panel-block{ display:grid; grid-template-columns:0.9fr 1.6fr; gap:0; align-items:stretch; margin-top:10px; border:1px solid var(--line); }
  @media (max-width:680px){ .panel-block{ grid-template-columns:1fr; } }
  .panel-curve{ position:relative; background:var(--void-2); min-height:220px; display:flex; align-items:center; justify-content:center; }
  .panel-curve svg{ width:70%; height:70%; }
  .panel-text{ padding:36px 34px; }
  .panel-text p{ font-size:1rem; }

  .why-black{
    position:relative; z-index:3; padding:130px 0; text-align:center;
    background:linear-gradient(180deg, var(--void-2), var(--void));
    border-top:1px solid var(--line); border-bottom:1px solid var(--line);
  }
  .why-black .layer-label{ justify-content:center; }
  .why-black .layer-label::after{ display:none; }
  .why-black .statement{ font-family:'Fraunces', serif; font-style:italic; font-weight:400; font-size:clamp(1.4rem, 3.2vw, 2.1rem); max-width:22ch; margin:0 auto; }
  .why-black .statement + .statement{ margin-top:14px; color:rgba(217,211,196,0.6); font-size:clamp(1rem,2vw,1.2rem); font-style:normal; font-family:'Inter',sans-serif; max-width:36ch; }

  .message{ text-align:center; }
  .message .msg-text{ font-family:'Fraunces', serif; font-style:italic; font-weight:400; font-size:clamp(1.2rem, 2.6vw, 1.7rem); max-width:26ch; margin:0 auto; line-height:1.5; }
  .message .msg-text + .msg-text{ margin-top:22px; font-size:clamp(1rem,1.8vw,1.15rem); font-style:normal; font-family:'Inter',sans-serif; color:rgba(217,211,196,0.7); max-width:40ch; }

  .primary-tag{ text-align:center; padding:100px 0; }
  .primary-tag .tagline{ font-family:'Fraunces', serif; font-style:italic; font-weight:500; font-size:clamp(1.6rem, 4.4vw, 2.6rem); max-width:20ch; margin:0 auto; }

  /* ---------------- GARMENT CARE ---------------- */
  .care-lede{ color:var(--bone); }
  .care-list{ list-style:none; margin-top:22px; display:grid; grid-template-columns:1fr 1fr; gap:10px 22px; }
  @media (max-width:560px){ .care-list{ grid-template-columns:1fr; } }
  .care-list li{
    font-family:'IBM Plex Mono', monospace; font-size:0.74rem; letter-spacing:0.04em;
    color:rgba(217,211,196,0.78); padding-left:16px; position:relative; text-transform:none;
  }
  .care-list li::before{ content:'—'; position:absolute; left:0; color:var(--red); }
  .care-sub{ margin-top:46px; }
  .care-sub h4{ font-family:'IBM Plex Mono', monospace; font-size:0.72rem; letter-spacing:0.1em; text-transform:uppercase; color:var(--bone); margin-bottom:12px; }

  .ending{ text-align:center; padding:70px 0 60px; }
  .ending .em-line{ font-family:'Fraunces', serif; font-style:italic; font-weight:400; font-size:clamp(1.1rem, 2.2vw, 1.4rem); color:var(--bone); max-width:34ch; margin:0 auto 14px; }
  .ending .em-line.dim{ color:rgba(217,211,196,0.6); font-size:clamp(0.95rem,1.8vw,1.1rem); font-style:normal; font-family:'Inter',sans-serif; }
  .ending .em-line.final{ margin-top:26px; color:var(--bone); }

  .closing{ text-align:center; padding:120px 0 100px; }
  .closing .brand{ font-family:'IBM Plex Mono', monospace; font-weight:500; font-size:clamp(1.4rem, 4.4vw, 2.2rem); letter-spacing:3px; }
  .closing .coll{ margin-top:14px; font-size:0.66rem; color:var(--ash); }
  .cta-row{ margin-top:40px; display:flex; justify-content:center; gap:14px; flex-wrap:wrap; }
  button, .cta{
    font-family:'IBM Plex Mono', monospace; font-size:0.68rem; letter-spacing:0.12em; text-transform:uppercase;
    padding:14px 26px; border:1px solid rgba(217,211,196,0.4); background:transparent; color:var(--bone);
    cursor:pointer; text-decoration:none; display:inline-block; transition:all .2s ease;
  }
  button:hover, .cta:hover{ background:var(--bone); color:var(--void); border-color:var(--bone); }
  button:active{ transform:scale(0.97); }
  button:focus-visible, .cta:focus-visible{ outline:2px solid var(--red); outline-offset:3px; }

  footer{ position:relative; z-index:3; text-align:center; padding:30px 0 50px; font-size:0.58rem; color:var(--ash); letter-spacing:0.14em; }

  .reveal{ opacity:0; transform:translateY(18px); transition:opacity .9s ease, transform .9s ease; }
  .reveal.revealed{ opacity:1; transform:translateY(0); }

  @media (prefers-reduced-motion: reduce){
    *{ animation:none !important; transition:none !important; }
    .reveal{ opacity:1 !important; transform:none !important; }
  }
</style>
</head>
<body>

<canvas id="terrain"></canvas>
<div class="grain"></div>
<div class="core-glow" id="coreGlow"></div>
<div class="vignette"></div>

<nav class="mono">
  <span>NOCHTRA</span>
  <div class="depth-readout">
    <span class="depth-pulse" id="depthPulse"></span>
    <span id="depthLabel">SURFACE</span>
    <span class="depth-track"><span class="depth-fill" id="depthFill"></span></span>
    <span class="depth-val" id="depthVal">0m</span>
  </div>
</nav>

<div class="hero" id="heroSection">
  <div class="coll mono">NOCHTRA COLLECTION DROP 3.1</div>
  <div class="chapter mono">INNER TERRAIN CHAPTER I</div>
  <h1>After Light</h1>
  <p class="quote serif">"When the light fades, the terrain remains."</p>
  <div class="scroll-cue"><span class="mono">Teken layar dulu baru scroll</span><span class="stem"></span></div>
  <div class="hero-reveal serif" id="heroReveal">scroll terus, siapa tau nemu jawaban hidup.</div>
  <div class="hold-hint mono" id="holdHint">refresh halaman tidak menghapus masa lalu</div>
</div>

<div class="touch-hint mono" id="touchHint">touch the surface</div>

<div class="breathe-guide mono" id="breatheGuide" role="button" tabindex="0" aria-label="Ikuti panduan napas">
  <span class="breathe-ring" id="breatheRing"></span>
  <span class="breathe-label" id="breatheLabel">breathe</span>
</div>

<div class="ground-overlay" id="groundOverlay">
  <div class="ground-msg serif" id="groundMsg">Refresh halaman tidak menghapus masa lalu.</div>
</div>

<section>
  <div class="wrap">
    <div class="layer-label mono"><span class="num">L.01</span> Concept</div>
    <p class="lede reveal">Ruang yang tidak pernah benar-benar diperlihatkan kepada siapa pun.</p>
    <div class="reveal">
      <p>Setiap orang memiliki ruang yang tidak pernah benar-benar diperlihatkan kepada siapa pun. Sebuah ruang yang dipenuhi kenangan, kecemasan, luka, harapan, dan pikiran yang terus bergerak.</p>
      <p>NOCHTRA menyebut ruang itu sebagai <em class="serif">Inner Terrain</em> — lanskap batin yang membentuk siapa kita, meskipun tidak pernah terlihat dari luar.</p>
      <p>AFTER LIGHT adalah awal dari perjalanan itu. Bukan tentang gelap. Bukan tentang kehilangan. Tetapi tentang momen ketika semua distraksi menghilang, dan seseorang akhirnya berhadapan dengan dirinya sendiri.</p>
    </div>
  </div>
</section>

<section>
  <div class="wrap">
    <div class="layer-label mono"><span class="num">L.02</span> Story</div>
    <div class="reveal">
      <p>Saat cahaya perlahan menghilang, dunia ikut menjadi sunyi. Tidak ada lagi suara yang harus didengar. Tidak ada lagi wajah yang harus dipuaskan.</p>
      <p>Yang tersisa hanyalah dirimu. Dan seluruh pikiran yang selama ini kamu simpan.</p>
    </div>
    <blockquote class="reveal">Body hitam melambangkan ruang tanpa gangguan. Tempat di mana seseorang berhenti berlari dari dirinya sendiri.</blockquote>
    <div class="reveal">
      <p>Panel camo hitam bukan dibuat untuk menarik perhatian. Ia sengaja dibuat hampir menyatu. Karena tidak semua luka memiliki bentuk yang jelas. Tidak semua rasa takut terlihat oleh mata.</p>
      <p>Ada cerita yang hanya bisa dirasakan. Bukan diperlihatkan.</p>
    </div>
  </div>
</section>

<section id="noiseSection">
  <div class="wrap">
    <div class="layer-label mono"><span class="num">L.03</span> Noise</div>
    <p class="reveal">Lanjut terus, siapa tau hidup ikut berubah.</p>
    <div class="noise-field reveal">
      <span class="n-frag">yo ndak tau kok tanya saya (YNTKTS)</span>
      <span class="n-frag hot">anjay masih idup</span>
      <span class="n-frag strike">semoga harimu lebih stabil dari koneksi wifi</span>
      <span class="n-frag">lagi ngumpulin aura</span>
      <span class="n-frag">hidup emang suka ngide, jangan heran dah</span>
      <span class="n-frag hot">yang penting napas</span>
      <span class="n-frag joke">mending ngopi kita.</span>
      <span class="n-frag">lagi farming sabar</span>
      <span class="n-frag strike">meta lagi jelek</span>
      <span class="n-frag">kalo nemu jawaban hidup kabarin</span>
    </div>
  </div>
</section>

<div class="why-black">
  <div class="wrap">
    <div class="layer-label mono reveal"><span class="num">L.04</span> Why Black</div>
    <div class="statement reveal">Hitam bukan simbol kesedihan.<br>Hitam adalah keheningan.</div>
    <div class="statement reveal">Di dalam keheningan, manusia tidak lagi sibuk menyembunyikan dirinya. Ia mulai mendengar isi kepalanya sendiri. Di situlah perjalanan dimulai.</div>
  </div>
</div>

<section>
  <div class="wrap">
    <div class="layer-label mono reveal"><span class="num">L.05</span> Visual Meaning</div>
    <p class="reveal">Dua lapisan yang sama-sama gelap, tapi menyimpan makna yang berbeda.</p>
    <div class="meaning-grid reveal">
      <div class="meaning-col">
        <h3>Black Body</h3>
        <ul><li>The Self</li><li>Silence</li><li>Acceptance</li><li>Depth</li><li>Unknown</li></ul>
      </div>
      <div class="meaning-col">
        <h3>Hidden Camo</h3>
        <ul><li>Fear</li><li>Memory</li><li>Trauma</li><li>Growth</li><li>Everything carried within</li></ul>
      </div>
    </div>
  </div>
</section>

<section>
  <div class="wrap">
    <div class="layer-label mono reveal"><span class="num">L.06</span> Curved Side Panel</div>
    <div class="panel-block reveal">
      <div class="panel-curve" aria-hidden="true">
        <svg viewBox="0 0 100 100" fill="none">
          <path d="M 20 0 C 45 25, 45 75, 20 100" stroke="rgba(217,211,196,0.5)" stroke-width="0.6"/>
          <path d="M 32 0 C 55 25, 55 75, 32 100" stroke="rgba(217,211,196,0.28)" stroke-width="0.6"/>
          <path d="M 44 0 C 65 25, 65 75, 44 100" stroke="rgba(138,28,34,0.4)" stroke-width="0.6"/>
        </svg>
      </div>
      <div class="panel-text">
        <p>Batas antara dunia yang terlihat. Dan dunia yang membentukmu.</p>
        <p>Panel berada di samping. Karena bagian terpenting dari diri seseorang sering kali berada di tempat yang tidak pernah diperhatikan.</p>
      </div>
    </div>
  </div>
</section>

<section>
  <div class="wrap">
    <div class="layer-label mono reveal"><span class="num">L.07</span> Philosophy of Construction</div>
    <div class="reveal">
      <p>Setiap potongan panel dibuat dengan sengaja. Setiap garis jahitan adalah simbol bahwa manusia tidak pernah benar-benar utuh sejak awal.</p>
      <p>Kita dibentuk oleh pengalaman. Disatukan kembali oleh waktu. Dan terus membawa bekasnya ke mana pun kita pergi.</p>
    </div>
  </div>
</section>

<section class="message">
  <div class="wrap">
    <div class="layer-label mono reveal" style="justify-content:center;"><span class="num">L.08</span> Collection Message</div>
    <p class="msg-text reveal">Kita hidup di dunia yang selalu meminta kita terlihat baik-baik saja. Namun tidak pernah mengajarkan bagaimana menerima bagian diri yang tidak pernah terlihat.</p>
    <p class="msg-text reveal">AFTER LIGHT bukan tentang menjadi sempurna. Ia adalah tentang berhenti melawan bagian dirimu yang sudah lama ada. Karena menerima bukan berarti menyerah. Menerima berarti memahami.</p>
  </div>
</section>

<div class="primary-tag">
  <div class="wrap"><p class="tagline serif reveal">"When the light fades, the terrain remains."</p></div>
</div>

<section>
  <div class="wrap">
    <div class="layer-label mono reveal"><span class="num">L.09</span> Garment Care</div>
    <p class="lede care-lede reveal">Designed to age with you.</p>
    <div class="reveal">
      <p>Setiap jahitan, panel, dan konstruksi pada garment ini dibuat untuk digunakan dalam jangka panjang. Perawatan yang tepat akan menjaga bentuk, warna, dan karakter kain seiring waktu.</p>
    </div>
    <ul class="care-list reveal">
      <li>Cuci dengan mode lembut</li>
      <li>Balik pakaian sebelum mencuci</li>
      <li>Gunakan air dingin (Max. 30°C)</li>
      <li>Gunakan daterjen lembut</li>
      <li>Jangan gunakan pemutih</li>
      <li>Jangan gunakan mesin pengering</li>
      <li>Jemur di tempat teduh</li>
      <li>Setrika dengan suhu rendah</li>
      <li>Setrika dari bagian dalam pakaian</li>
      <li>Tidak disarankan menggunakan dry clean</li>
    </ul>
    <div class="care-sub reveal">
      <h4>Wear With Care</h4>
      <p>Panel camo dan konstruksi garment dirancang sebagai bagian dari identitas produk. Hindari gesekan berlebih dengan permukaan kasar atau benda tajam agar tekstur dan bentuk panel tetap terjaga.</p>
    </div>
    <div class="care-sub reveal">
      <h4>Aging Process</h4>
      <p>Kami percaya bahwa pakaian yang baik akan berkembang bersama pemiliknya. Perubahan karakter kain, fading alami, dan lipatan akibat penggunaan adalah bagian dari perjalanan garment ini.</p>
      <p>Setiap bekas pemakaian menjadi bagian dari cerita yang hanya dimiliki oleh pemiliknya.</p>
    </div>
    <blockquote class="reveal">Care for it. Like every landscape, it becomes more meaningful with time.</blockquote>
  </div>
</section>

<section class="ending">
  <div class="wrap">
    <div class="layer-label mono reveal" style="justify-content:center;"><span class="num">L.10</span> Ending Manifesto</div>
    <p class="em-line reveal">Every person carries a landscape invisible to the world.</p>
    <p class="em-line dim reveal">Some hide it.</p>
    <p class="em-line dim reveal">Some fight it.</p>
    <p class="em-line dim reveal">Some spend their entire lives running from it.</p>
    <p class="em-line final reveal">This collection is for those who choose to face it.</p>
  </div>
</section>

<section class="closing">
  <div class="wrap">
    <div class="brand reveal">NOCHTRA</div>
    <div class="coll mono reveal">Collection 3.1 — Inner Terrain · Chapter I — After Light</div>
    <div class="cta-row reveal">
      <a class="cta" href="https://instagram.com/nochtra.co" target="_blank" rel="noopener">Ikuti @nochtra.co</a>
      <button id="descendBtn">↓ ENTER THE TERRAIN</button>
    </div>
  </div>
</section>

<footer class="mono">NOCHTRA — INNER TERRAIN — CHAPTER I — AFTER LIGHT</footer>

<script>
  const reduceMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

  const canvas = document.getElementById('terrain');
  const ctx = canvas.getContext('2d');
  let cw, ch, t = 0;
  function resize(){ cw = canvas.width = innerWidth; ch = canvas.height = innerHeight; }
  resize();
  addEventListener('resize', resize);

  const LAYERS = 9;
  const glyphs = ['?','...','!','//','—'];
  let particles = [];
  function buildParticles(){
    particles = [];
    for(let i=0;i<26;i++){
      particles.push({
        x: Math.random(), y: Math.random(),
        s: 0.3 + Math.random()*0.5,
        vy: 0.00006 + Math.random()*0.00012,
        g: glyphs[Math.floor(Math.random()*glyphs.length)],
        red: Math.random() < 0.22
      });
    }
  }
  buildParticles();

  /* ---------- mouse parallax + soft cursor trail ---------- */
  let mouseNX = 0, mouseNY = 0; // normalized -1..1
  let trail = [];
  addEventListener('mousemove', (e)=>{
    mouseNX = (e.clientX / innerWidth) * 2 - 1;
    mouseNY = (e.clientY / innerHeight) * 2 - 1;
    trail.push({ x:e.clientX, y:e.clientY, life:1 });
    if(trail.length > 40) trail.shift();
  });

  /* ---------- click cracks — the calm surface fractures when touched ---------- */
  let cracks = [];
  function spawnCrack(cx, cy){
    const branches = 5 + Math.floor(Math.random()*3);
    for(let b=0; b<branches; b++){
      const angle = (Math.PI*2/branches)*b + (Math.random()-0.5)*0.6;
      const len = 60 + Math.random()*140;
      cracks.push({
        x1:cx, y1:cy,
        x2: cx + Math.cos(angle)*len,
        y2: cy + Math.sin(angle)*len,
        life: 1
      });
    }
  }
  addEventListener('click', (e)=>{
    if(e.target.closest('button, a, .n-frag, #breatheGuide, #groundOverlay')) return;
    spawnCrack(e.clientX, e.clientY);
    if(cracks.length > 220) cracks.splice(0, cracks.length-220);
    const hint = document.getElementById('touchHint');
    if(hint){ hint.classList.add('hide'); hint.classList.remove('show'); }
  });

  const touchHint = document.getElementById('touchHint');
  if(touchHint && !reduceMotion){
    setTimeout(()=> touchHint.classList.add('show'), 2400);
    setTimeout(()=> touchHint.classList.remove('show'), 7000);
  }

  function drawTerrain(){
    ctx.clearRect(0,0,cw,ch);
    for(let l=0; l<LAYERS; l++){
      const parallax = mouseNX * (l - LAYERS/2) * 2.2;
      const baseY = ch * (0.08 + l * 0.105) + mouseNY * (l - LAYERS/2) * 1.6;
      const amp = 24 + l*7;
      const speed = 0.16 + l*0.045;
      const isRedLayer = (l % 3 === 2);
      const alpha = isRedLayer ? 0.07 + (l/LAYERS)*0.06 : 0.075 + (l/LAYERS)*0.09;
      ctx.beginPath();
      for(let x=0; x<=cw; x+=8){
        const y = baseY + Math.sin(((x+parallax*20)*0.004) + t*speed + l*1.7) * amp
                        + Math.sin(((x+parallax*20)*0.011) + t*speed*0.6 + l) * (amp*0.4);
        if(x===0) ctx.moveTo(x,y); else ctx.lineTo(x,y);
      }
      ctx.strokeStyle = isRedLayer ? 'rgba(138,28,34,'+alpha+')' : 'rgba(217,211,196,'+alpha+')';
      ctx.lineWidth = 1;
      ctx.stroke();
    }

    ctx.font = '13px IBM Plex Mono, monospace';
    particles.forEach(p=>{
      p.y -= p.vy;
      if(p.y < -0.05) p.y = 1.05;
      const px = p.x*cw + mouseNX*10, py = p.y*ch;
      ctx.fillStyle = p.red ? 'rgba(138,28,34,'+ (0.18*p.s+0.06) +')' : 'rgba(217,211,196,'+ (0.1*p.s+0.03) +')';
      ctx.fillText(p.g, px, py);
    });

    if(cracks.length){
      cracks = cracks.filter(c=> c.life > 0.02);
      cracks.forEach(c=>{
        ctx.strokeStyle = 'rgba(138,28,34,'+(c.life*0.5)+')';
        ctx.lineWidth = 0.8;
        ctx.beginPath();
        ctx.moveTo(c.x1, c.y1);
        ctx.lineTo(c.x2, c.y2);
        ctx.stroke();
        c.life -= 0.012;
      });
    }

    if(trail.length){
      trail.forEach((p,i)=>{
        p.life -= 0.045;
        if(p.life <= 0) return;
        const r = 2.2 * p.life;
        ctx.beginPath();
        ctx.arc(p.x, p.y, r, 0, Math.PI*2);
        ctx.fillStyle = 'rgba(138,28,34,'+ (0.16*p.life) +')';
        ctx.fill();
      });
      trail = trail.filter(p=> p.life > 0);
    }
  }
  function loop(){
    t += 0.006;
    drawTerrain();
    if(!reduceMotion) requestAnimationFrame(loop);
  }
  if(!reduceMotion) loop(); else drawTerrain();

  const depthFill = document.getElementById('depthFill');
  const depthVal = document.getElementById('depthVal');
  const depthLabel = document.getElementById('depthLabel');
  const depthPulse = document.getElementById('depthPulse');
  const coreGlow = document.getElementById('coreGlow');
  let ticking = false;
  function updateDepth(){
    ticking = false;
    const scrollable = document.documentElement.scrollHeight - innerHeight;
    const progress = scrollable > 0 ? Math.min(1, window.scrollY / scrollable) : 0;
    depthFill.style.width = (progress*100).toFixed(1)+'%';
    depthVal.textContent = '-'+Math.round(progress*140)+'m';
    let label;
    if(progress < 0.05) label = 'SURFACE';
    else if(progress < 0.3) label = 'DESCENDING';
    else if(progress < 0.5) label = 'NOISE RISING';
    else if(progress < 0.78) label = 'INNER TERRAIN';
    else if(progress < 0.95) label = 'APPROACHING CORE';
    else label = 'CORE REACHED';
    depthLabel.textContent = label;
    if(!reduceMotion) depthPulse.style.animationDuration = (1.8 - progress*1.1) + 's';
    coreGlow.style.opacity = (progress*0.9).toFixed(2);
  }
  addEventListener('scroll', ()=>{ if(!ticking){ ticking = true; requestAnimationFrame(updateDepth); } });
  updateDepth();

  /* ---------- periodic hero glitch — a wink at the "noise" theme ---------- */
  const heroTitle = document.querySelector('.hero h1');
  function scheduleGlitch(){
    const delay = 6000 + Math.random()*6000;
    setTimeout(()=>{
      if(!reduceMotion){
        heroTitle.classList.add('glitching');
        setTimeout(()=> heroTitle.classList.remove('glitching'), 300);
      }
      scheduleGlitch();
    }, delay);
  }
  if(!reduceMotion) scheduleGlitch();

  heroTitle.style.cursor = 'default';
  heroTitle.addEventListener('mouseenter', ()=>{
    if(reduceMotion) return;
    heroTitle.classList.remove('glitching');
    void heroTitle.offsetWidth; /* restart animation */
    heroTitle.classList.add('glitching');
    setTimeout(()=> heroTitle.classList.remove('glitching'), 300);
  });

  /* ---------- mouse spotlight inside the noise field ---------- */
  const noiseField = document.querySelector('.noise-field');
  if(noiseField){
    noiseField.addEventListener('mousemove', (e)=>{
      const rect = noiseField.getBoundingClientRect();
      const mx = ((e.clientX - rect.left) / rect.width) * 100;
      const my = ((e.clientY - rect.top) / rect.height) * 100;
      noiseField.style.setProperty('--mx', mx+'%');
      noiseField.style.setProperty('--my', my+'%');
    });
  }

  let audioCtx;
  function getCtx(){
    if(!audioCtx) audioCtx = new (window.AudioContext||window.webkitAudioContext)();
    if(audioCtx.state==='suspended') audioCtx.resume();
    return audioCtx;
  }
  function playTone(){
    try{
      const ctx2 = getCtx();
      const osc = ctx2.createOscillator();
      const gain = ctx2.createGain();
      osc.type = 'sine';
      osc.frequency.setValueAtTime(110, ctx2.currentTime);
      osc.frequency.exponentialRampToValueAtTime(70, ctx2.currentTime + 0.5);
      gain.gain.setValueAtTime(0.0001, ctx2.currentTime);
      gain.gain.exponentialRampToValueAtTime(0.12, ctx2.currentTime + 0.05);
      gain.gain.exponentialRampToValueAtTime(0.0001, ctx2.currentTime + 0.6);
      osc.connect(gain); gain.connect(ctx2.destination);
      osc.start(); osc.stop(ctx2.currentTime + 0.6);
    }catch(e){}
  }
  document.querySelectorAll('button, .cta').forEach(el=> el.addEventListener('click', playTone));

  document.getElementById('descendBtn').addEventListener('click', ()=>{
    window.scrollTo({ top:0, behavior: reduceMotion ? 'auto' : 'smooth' });
  });

  const io = new IntersectionObserver((entries)=>{
    entries.forEach(e=>{ if(e.isIntersecting) e.target.classList.add('revealed'); });
  }, { threshold:0.16 });
  document.querySelectorAll('.reveal').forEach(el=> io.observe(el));

  /* ---------- breathing guide: ambient cycle + click-to-ground ---------- */
  const breatheRing = document.getElementById('breatheRing');
  const breatheLabel = document.getElementById('breatheLabel');
  const breatheGuide = document.getElementById('breatheGuide');
  const groundOverlay = document.getElementById('groundOverlay');
  const groundMessages = [
    'Kalo ketawa berarti masih sehat. kayanya.',
    'Masih hidup? Keren juga lu bro.',
    'Hiu emang suka bercanda.'
  ];

  function breatheCycle(){
    breatheRing.classList.remove('exhale');
    breatheRing.classList.add('inhale');
    breatheLabel.textContent = 'in';
    setTimeout(()=>{
      breatheRing.classList.remove('inhale');
      breatheRing.classList.add('exhale');
      breatheLabel.textContent = 'out';
    }, 4000);
  }
  if(!reduceMotion){
    breatheCycle();
    setInterval(breatheCycle, 8000);
  } else {
    breatheLabel.textContent = 'breathe';
  }

  let groundBusy = false;
  function enterGroundPause(){
    if(groundBusy) return;
    groundBusy = true;
    document.getElementById('groundMsg').textContent =
      groundMessages[Math.floor(Math.random()*groundMessages.length)];
    groundOverlay.classList.add('active');
    setTimeout(()=>{
      groundOverlay.classList.remove('active');
      groundBusy = false;
    }, 5200);
  }
  breatheGuide.addEventListener('click', enterGroundPause);
  breatheGuide.addEventListener('keydown', (e)=>{
    if(e.key === 'Enter' || e.key === ' '){ e.preventDefault(); enterGroundPause(); }
  });

  /* ---------- hero: press & hold to let the light fade ---------- */
  const heroSection = document.getElementById('heroSection');
  const holdHint = document.getElementById('holdHint');
  let holdTimer = null;

  if(!reduceMotion){
    setTimeout(()=> holdHint.classList.add('show'), 3200);
  } else {
    holdHint.style.display = 'none';
  }

  function startFade(){
    if(reduceMotion) return;
    heroSection.classList.add('fading');
    holdHint.classList.remove('show');
    holdHint.classList.add('hide');
  }
  function endFade(){
    heroSection.classList.remove('fading');
  }
  heroSection.addEventListener('mousedown', startFade);
  heroSection.addEventListener('touchstart', startFade, { passive:true });
  ['mouseup','mouseleave','touchend','touchcancel'].forEach(evt=>{
    heroSection.addEventListener(evt, endFade);
  });
</script>
</body>
</html>
