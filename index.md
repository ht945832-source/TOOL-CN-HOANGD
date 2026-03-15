<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=0"/> 
<title>HOANGDZ AI PREDICT v5.0 - AUTO MD5</title>
<style>
:root{
  --tx-color:#00f2ff; --md5-color:#facc15; --neon: #00ffcc;
  --bg-panel: rgba(5, 15, 25, 0.98);
}
*{box-sizing:border-box; touch-action: none; -webkit-tap-highlight-color: transparent;}
body { margin: 0; background: #000; overflow: hidden; font-family: 'Segoe UI', sans-serif; color: #fff; }

/* SETUP SCREEN */
#setupOverlay {
  position: fixed; inset: 0; background: radial-gradient(circle, #002b20 0%, #000 100%);
  z-index: 20000; display: flex; flex-direction: column; align-items: center; justify-content: center;
}
.setup-box {
  background: var(--bg-panel); backdrop-filter: blur(20px);
  padding: 25px; border-radius: 25px; border: 1px solid var(--neon);
  width: 380px; text-align: center; box-shadow: 0 0 50px rgba(0, 255, 204, 0.3);
}
.hub-logo { font-size: 28px; font-weight: 900; color: var(--neon); text-shadow: 0 0 15px var(--neon); margin-bottom: 15px; letter-spacing: 2px; }
.game-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 12px; margin-top: 10px; }
.game-card { background: rgba(0, 40, 30, 0.8); border: 1px solid var(--neon); border-radius: 15px; padding: 12px; cursor: pointer; transition: 0.3s; font-size: 11px; font-weight: bold; }
.game-card:hover { transform: scale(1.05); background: rgba(0, 255, 204, 0.1); }
.game-card img { width: 50px; height: 50px; border-radius: 10px; margin-bottom: 8px; box-shadow: 0 0 10px rgba(0,0,0,0.5); }

/* MAIN UI */
#gameFrame { position: fixed; inset: 0; width: 100vw; height: 100vh; border: none; z-index: 1; display: none; }
#exitBtn { position: fixed; top: 15px; left: 15px; z-index: 10005; background: rgba(255,0,0,0.5); color: white; border: 1px solid #ff4444; padding: 8px 15px; border-radius: 10px; cursor: pointer; display: none; font-weight: bold; font-size: 12px; backdrop-filter: blur(5px); }

/* TOOL 90 ĐỘ NẰM NGANG */
.bot-wrap { 
  position: fixed; z-index: 9999; display: none; align-items: center; gap: 15px; 
  transform: rotate(90deg); transform-origin: center; scale: 0.9;
}
.robot-img { 
  width: 110px; /* Robot to hơn */
  filter: drop-shadow(0 0 15px var(--neon)); 
  animation: bounce 2s infinite alternate; 
}
@keyframes bounce { from {transform: translateY(0);} to {transform: translateY(-10px);} }

.panel { 
  min-width: 280px; background: var(--bg-panel); backdrop-filter: blur(15px);
  border-radius: 20px; padding: 15px; border: 1.5px solid var(--neon);
  box-shadow: 0 15px 40px rgba(0,0,0,0.8);
}
.header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 10px; border-bottom: 1px solid rgba(0,255,204,0.2); padding-bottom: 5px; }
.title { font-size: 10px; font-weight: 900; letter-spacing: 1px; color: var(--neon); }
.zoom-btn { width: 26px; height: 26px; background: rgba(255,255,255,0.1); border: none; color: white; border-radius: 6px; cursor: pointer; }

/* Input ẩn khi phân tích */
.input-container { position: relative; margin-bottom: 10px; }
input.cau-input { width: 100%; background: #000; border: 1px solid var(--neon); color: var(--neon); padding: 10px; border-radius: 10px; font-size: 11px; outline: none; text-align: center; }

/* Vùng hiển thị kết quả gọn đẹp */
.res-box { text-align: left; padding: 10px; background: rgba(0,0,0,0.3); border-radius: 12px; }
.res-line { font-size: 13px; margin: 5px 0; font-weight: bold; display: flex; justify-content: space-between; }
.val-big { font-size: 20px; font-weight: 900; color: #fff; text-shadow: 0 0 10px var(--neon); }
.rate-val { color: #00ff00; }
.advice-val { color: #00f2ff; font-style: italic; font-size: 12px; }

/* Nút Đúng/Sai */
.check-btns { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; margin-top: 10px; }
.btn-check { padding: 8px; border: none; border-radius: 8px; font-size: 10px; font-weight: 900; cursor: pointer; transition: 0.2s; }
.btn-win { background: #fff; color: #000; }
.btn-lose { background: #ff4444; color: #fff; }

/* Thông báo Phân tích */
#loadingOverlay { display: none; text-align: center; padding: 10px; color: var(--neon); font-weight: bold; font-size: 12px; animation: pulse 1s infinite; }
@keyframes pulse { 0% {opacity:1} 50% {opacity:0.4} 100% {opacity:1} }

/* Popup thông báo */
#msgPopup {
    display: none; position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%);
    z-index: 100000; padding: 25px; border-radius: 20px; text-align: center;
    font-weight: 900; border: 4px solid #fff; box-shadow: 0 0 50px rgba(0,0,0,0.8);
}
</style>
</head>
<body>

<div id="setupOverlay">
  <div class="setup-box">
    <div class="hub-logo">HOANGDZ VIPP</div>
    <div style="font-size: 12px; color: #aaa; margin-bottom: 15px;">AUTO MD5 ANALYSIS SYSTEM</div>
    <div class="game-grid">
        <div class="game-card" onclick="launch('https://play.betvip.fit/?utm_source=betvip.ceo&utm_campaign=betvip.ceo&utm_medium=betvip.ceo&utm_term=betvip.ceo')">
            <img src="https://i.postimg.cc/ZqqrCCjQ/A3EFE26C-7E67-4AFF-A21F-130E42D235EE.png"><br>BETVIP
        </div>
        <div class="game-card" onclick="launch('https://lc79b.bet')">
            <img src="https://i.postimg.cc/JnLWCgjm/3E497BE2-D59F-46D3-BCCF-FDC2E3A9929C.png"><br>LC79
        </div>
        <div class="game-card" onclick="launch('https://68gbvn88.bar')">
            <img src="https://i.postimg.cc/mD7XFp9t/68gamebai.png"><br>68GB
        </div>
    </div>
  </div>
</div>

<button id="exitBtn" onclick="location.reload()">THOÁT</button>
<iframe id="gameFrame"></iframe>

<div id="wrapMD5" class="bot-wrap" onmousedown="dragS(event, this)" ontouchstart="dragS(event, this)">
  <img class="robot-img" src="https://i.postimg.cc/63bdy9D9/robotics-1.gif">
  <div class="panel">
    <div class="header">
        <span class="title">AI AUTO DECODE 90°</span>
        <div><button class="zoom-btn" onclick="zm(this,-0.1)">-</button><button class="zoom-btn" onclick="zm(this,0.1)">+</button></div>
    </div>
    
    <div class="input-container">
        <input type="text" id="md5Input" class="cau-input" placeholder="DÁN MÃ MD5 TẠI ĐÂY..." autocomplete="off">
    </div>

    <div id="loadingOverlay">ĐANG PHÂN TÍCH MD5...</div>

    <div class="res-box" id="resultArea">
        <div class="res-line">Dự đoán: <span id="resVal" class="val-big">---</span></div>
        <div class="res-line">Tỉ lệ: <span id="rateVal" class="rate-val">0%</span></div>
        <div class="res-line">Khuyên: <span id="adviceVal" class="advice-val">Đang đợi mã...</span></div>
        
        <div class="check-btns">
            <button class="btn-check btn-win" onclick="verify(true)">ĐÚNG</button>
            <button class="btn-check btn-lose" onclick="verify(false)">SAI (XL)</button>
        </div>
    </div>
  </div>
</div>

<div id="msgPopup"></div>

<script>
let lastResult = "";
let currentScale = 0.9;

function launch(url) {
  document.getElementById('gameFrame').src = url;
  document.getElementById('gameFrame').style.display = 'block';
  document.getElementById('setupOverlay').style.display = 'none';
  document.getElementById('exitBtn').style.display = 'block';
  const t = document.getElementById('wrapMD5');
  t.style.display = 'flex';
  t.style.top = '50%';
  t.style.left = '120px';
}

// Tự động phân tích khi dán mã
document.getElementById('md5Input').addEventListener('input', function() {
    let val = this.value.trim();
    if (val.length >= 10) { // Tự chạy khi mã đủ dài (thường là 32 ký tự)
        autoAnalyze(val);
    }
});

function autoAnalyze(hash) {
    const loader = document.getElementById('loadingOverlay');
    const input = document.getElementById('md5Input');
    const resTxt = document.getElementById('resVal');
    const rateTxt = document.getElementById('rateVal');
    const adviceTxt = document.getElementById('adviceVal');

    loader.style.display = 'block';
    input.style.opacity = '0.3';
    input.disabled = true;

    setTimeout(() => {
        let sum = 0;
        for(let i=0; i<hash.length; i++) {
            if(!isNaN(parseInt(hash[i], 16))) sum += parseInt(hash[i], 16);
        }
        
        // Thuật toán AI giả lập
        const isTai = sum % 2 === 0;
        const rate = (Math.random() * (96 - 82) + 82).toFixed(1);
        
        lastResult = isTai ? "TÀI" : "XỈU";
        resTxt.textContent = lastResult;
        resTxt.style.color = isTai ? "#ffffff" : "#ffcc00";
        rateTxt.textContent = rate + "%";
        
        if(parseFloat(rate) > 90) {
            adviceTxt.textContent = "VÀO MẠNH TAY";
        } else {
            adviceTxt.textContent = "ĐI ĐỀU VỐN";
        }

        loader.style.display = 'none';
        input.style.opacity = '1';
        input.disabled = false;
        input.value = ""; // Xóa mã cũ để dán mã mới
        input.focus();
    }, 1500);
}

function verify(isCorrect) {
    if (!lastResult) return;
    const pop = document.getElementById('msgPopup');
    if (isCorrect) {
        pop.innerHTML = "<h2 style='margin:0'>CHÚC MỪNG!</h2><p>DỰ ĐOÁN CHÍNH XÁC</p>";
        pop.style.background = "rgba(0, 255, 150, 0.95)";
        pop.style.color = "#000";
    } else {
        pop.innerHTML = "<h2 style='margin:0'>RẤT TIẾC (XL)</h2><p>HẸN BẠN PHIÊN SAU</p>";
        pop.style.background = "rgba(255, 50, 50, 0.95)";
        pop.style.color = "#fff";
    }
    pop.style.display = 'block';
    setTimeout(() => { pop.style.display = 'none'; }, 1500);
}

function zm(btn, v) {
  let w = btn.closest('.bot-wrap');
  currentScale = Math.min(Math.max(currentScale + v, 0.4), 1.5);
  w.style.scale = currentScale;
}

let act = null; let sx, sy, ix, iy;
function dragS(e, el) {
  if (e.target.tagName === 'INPUT' || e.target.tagName === 'BUTTON') return;
  act = el; const v = e.type === 'touchstart' ? e.touches[0] : e;
  sx = v.clientX; sy = v.clientY;
  ix = act.offsetLeft; iy = act.offsetTop;
  document.addEventListener('mousemove', dragM); document.addEventListener('touchmove', dragM, {passive: false});
  document.addEventListener('mouseup', dragE); document.addEventListener('touchend', dragE);
}
function dragM(e) {
  if (!act) return; const v = e.type === 'touchmove' ? e.touches[0] : e;
  if(e.type === 'touchmove') e.preventDefault();
  act.style.left = (ix + (v.clientX - sx)) + 'px';
  act.style.top = (iy + (v.clientY - sy)) + 'px';
}
function dragE() { act = null; }
</script>
</body>
</html>
