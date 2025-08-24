<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Confession ❤️</title>
  <style>
    :root{
      --bg1:#ffe6f1; --bg2:#e6f7ff; --card:#ffffffee; --text:#222; --accent:#ff477e; --accent2:#6c5ce7;
    }
    *{box-sizing:border-box}
    html,body{height:100%;margin:0;font-family:system-ui,-apple-system,Segoe UI,Roboto,Ubuntu,"Noto Sans",sans-serif;color:var(--text)}
    body{
      background: radial-gradient(1200px 800px at 20% 10%, var(--bg1), #fff),
                  radial-gradient(1200px 800px at 80% 90%, var(--bg2), #fff);
      overflow:hidden;
    }
    .wrap{position:relative;min-height:100%;display:grid;place-items:center;padding:24px}
    .card{
      width:min(560px, 92vw);background:var(--card);backdrop-filter:saturate(1.2) blur(6px);
      border-radius:24px;box-shadow:0 20px 40px rgba(0,0,0,.12);padding:28px 24px 20px;position:relative
    }
    .title{font-size:28px;line-height:1.25;margin:0 0 8px;font-weight:800}
    .subtitle{margin:0 0 18px;opacity:.8}
    .heart{display:inline-block;transform:translateY(2px)}
    .typed{min-height:40px;font-size:18px;line-height:1.5;white-space:pre-wrap}
    .btns{display:flex;gap:12px;flex-wrap:wrap;margin-top:18px}
    button{
      appearance:none;border:none;border-radius:999px;padding:12px 20px;font-size:16px;font-weight:700;
      cursor:pointer;transition:transform .12s ease, box-shadow .2s ease, background .2s ease;outline:none
    }
    .yes{background:var(--accent);color:white;box-shadow:0 8px 18px rgba(255,71,126,.35)}
    .yes:active{transform:scale(.98)}
    .no{background:#f1f5f9;color:#111;position:relative}
    .hint{font-size:12px;opacity:.6;margin-top:8px}

    /* Modal */
    .modal{position:fixed;inset:0;display:grid;place-items:center;background:rgba(0,0,0,.25);opacity:0;pointer-events:none;transition:opacity .25s}
    .modal.open{opacity:1;pointer-events:auto}
    .modal-card{width:min(520px, 92vw);background:white;border-radius:24px;padding:28px;box-shadow:0 24px 60px rgba(0,0,0,.25);text-align:center}
    .modal h2{margin:0 0 8px}

    /* Floating hearts */
    .confetti{position:fixed;inset:0;pointer-events:none;overflow:hidden}
    .dot{position:absolute;font-size:20px;animation:rise linear forwards}
    @keyframes rise{from{transform:translateY(100vh) rotate(0)} to{transform:translateY(-10vh) rotate(360deg);opacity:.9}}

    /* Footer */
    .footer{position:fixed;bottom:10px;left:50%;transform:translateX(-50%);font-size:12px;opacity:.6}
  </style>
</head>
<body>
  <div class="wrap">
    <div class="card" id="card">
      <h1 class="title">Boleh minta waktunya sebentar gak? <span class="heart">💖</span></h1>
      <p class="subtitle">Boleh yah. Aku mau bilang ke kamu..</p>
      <div class="typed" id="typed"></div>
      <div class="btns">
        <button class="yes" id="yesBtn">Boleh kok</button>
        <button class="no" id="noBtn">Nggak mau 🙈</button>
      </div>
      <div class="hint">Jangan tekan 'nggak mau' yah. please 🥺</div>
    </div>
  </div>

  <div class="modal" id="modal">
    <div class="modal-card">
      <h2>Jadi gini…</h2>
      <p id="finalMsg" style="font-size:18px;line-height:1.6"></p>
      <div style="margin-top:16px;display:flex;gap:10px;justify-content:center;flex-wrap:wrap">
        <button id="replyYes" class="yes">Iya, aku mau! 💘</button>
        <button id="replyMaybe" class="no">Boleh temenan dulu 😊</button>
      </div>
    </div>
  </div>

  <div class="confetti" id="confetti"></div>

  <div class="footer">Made with ♥ — edit teksnya di dalam file ini</div>

  <script>
    // =============== Helper: Typewriter ===============
    const typeText = (el, text, speed=24) => new Promise(resolve=>{
      el.textContent = "";
      let i=0; const tick=()=>{ if(i<text.length){ el.textContent += text[i++]; setTimeout(tick, speed); } else resolve(); };
      tick();
    });

    // =============== Content you can edit ==============
    const confessionIntro = "Setiap hari kita main roblox ma ngobrol bersama. Dimulai dari ketemu di sekolahan, Muncak, Ketemu teman-teman, Sampai saat ini. Aku mo bilang..";
    const confessionLine  = "好き です ❤️. 付き 合っ てく ださ い.";

    // =============== Init typing ===============
    const typed = document.getElementById('typed');
    typeText(typed, confessionIntro, 18);

    // =============== Moving "No" button ===============
    const noBtn = document.getElementById('noBtn');
    const yesBtn = document.getElementById('yesBtn');
    const card = document.getElementById('card');

    function moveNo(){
      const rect = card.getBoundingClientRect();
      const maxX = rect.width - noBtn.offsetWidth - 12;
      const maxY = rect.height - noBtn.offsetHeight - 12;
      const x = Math.max(0, Math.min(maxX, Math.random()*maxX));
      const y = Math.max(0, Math.min(maxY, Math.random()*maxY));
      noBtn.style.position = 'absolute';
      noBtn.style.left = x + 'px';
      noBtn.style.top  = (y + 140) + 'px';
    }
    noBtn.addEventListener('mouseenter', moveNo);
    noBtn.addEventListener('click', moveNo);

    // =============== Modal & Confetti ===============
    const modal = document.getElementById('modal');
    const finalMsg = document.getElementById('finalMsg');
    const confetti = document.getElementById('confetti');

    yesBtn.addEventListener('click', async () => {
      await typeText(typed, confessionLine, 16);
      openModal();
      celebrate();
    });

    function openModal(){ modal.classList.add('open'); finalMsg.textContent = confessionLine; }
    modal.addEventListener('click', (e)=>{ if(e.target===modal) modal.classList.remove('open'); });

    function celebrate(){
      for(let i=0;i<120;i++){
        const d = document.createElement('div');
        d.className='dot';
        d.textContent = Math.random()>.5 ? '💖' : (Math.random()>.5 ? '💘' : '💞');
        d.style.left = Math.random()*100 + 'vw';
        d.style.animationDuration = 3 + Math.random()*3 + 's';
        d.style.opacity = .6 + Math.random()*.4;
        confetti.appendChild(d);
        setTimeout(()=>d.remove(), 6500);
      }
    }

    // =============== Reply buttons ===============
    document.getElementById('replyYes').addEventListener('click', ()=>{
      finalMsg.textContent = 'Yasss! Makasih sudah nerima perasaanku. Awww! Chen2 jadi pacar ku sekarang~ ILY❤️ 😘 ';
      celebrate();
    });
    document.getElementById('replyMaybe').addEventListener('click', ()=>{
      finalMsg.textContent = 'Nggak apa-apa🥹. Aku senang bisa kenal kamu. Aku off dulu selama seminggu...';
    });

    // =============== Keyboard easter egg ===============
    window.addEventListener('keydown', (e)=>{
      if(e.key.toLowerCase()==='l'){ celebrate(); }
    });
  </script>
</body>
</html>
