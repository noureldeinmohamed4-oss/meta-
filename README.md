
<!DOCTYPE html>
<html lang="ar" dir="rtl" translate="no" class="notranslate">
<head>
<meta charset="UTF-8">
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<meta name="google" content="notranslate">
<title>Luma — تحسين الصور</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700;900&family=Tajawal:wght@300;400;500;700&display=swap" rel="stylesheet">
<style>
  :root{
    --bg-0:#070a10;
    --bg-1:#0d121b;
    --bg-2:#131a26;
    --gold-1:#e8c874;
    --gold-2:#a9822f;
    --teal:#12706a;
    --ink:#f3efe3;
    --ink-dim:#9aa4b4;
    --line:rgba(232,200,116,0.14);
    --radius:18px;
  }
  *{box-sizing:border-box; -webkit-tap-highlight-color:transparent;}
  html,body{margin:0;padding:0;}
  body{
    background:
      radial-gradient(1200px 700px at 85% -10%, rgba(18,112,106,0.18), transparent 60%),
      radial-gradient(900px 600px at 10% 110%, rgba(232,200,116,0.10), transparent 55%),
      var(--bg-0);
    color:var(--ink);
    font-family:'Tajawal', sans-serif;
    min-height:100vh;
    overflow-x:hidden;
  }
  .wrap{max-width:640px; margin:0 auto; padding:28px 18px 60px;}

  /* Header / seal */
  header{text-align:center; padding:10px 0 26px; position:relative;}
  .seal{
    display:inline-flex; align-items:center; gap:10px;
    padding:7px 16px; border-radius:999px;
    border:1px solid var(--line);
    background:linear-gradient(180deg, rgba(232,200,116,0.06), rgba(232,200,116,0.02));
    font-family:'Cairo', sans-serif; font-weight:600; font-size:12px;
    letter-spacing:0.5px; color:var(--gold-1);
    margin-bottom:22px;
  }
  .seal .dot{width:6px;height:6px;border-radius:50%;background:var(--gold-1); box-shadow:0 0 8px var(--gold-1);}
  h1.brand{
    font-family:'Cairo', sans-serif; font-weight:900;
    font-size:56px; margin:0; line-height:1;
    background:linear-gradient(100deg, #fff 10%, var(--gold-1) 45%, #8a6a22 60%, var(--gold-1) 75%, #fff 95%);
    background-size:250% 100%;
    -webkit-background-clip:text; background-clip:text; color:transparent;
    animation:shine 5.5s linear infinite;
    position:relative;
    display:inline-block;
  }
  @keyframes shine{
    0%{background-position:200% 0;}
    100%{background-position:-50% 0;}
  }
  p.tagline{color:var(--ink-dim); font-size:15px; margin:12px 0 0; font-weight:400;}

  /* Upload zone */
  .stage{
    border-radius:var(--radius);
    border:1px solid var(--line);
    background:linear-gradient(180deg, var(--bg-2), var(--bg-1));
    padding:16px;
    box-shadow:0 30px 60px -25px rgba(0,0,0,0.6), inset 0 1px 0 rgba(255,255,255,0.03);
  }
  #dropzone{
    border-radius:14px;
    border:1.5px dashed rgba(232,200,116,0.35);
    min-height:280px;
    display:flex; flex-direction:column; align-items:center; justify-content:center;
    gap:14px;
    padding:30px 20px;
    text-align:center;
    cursor:pointer;
    transition:border-color .25s, background .25s;
    position:relative;
    overflow:hidden;
  }
  #dropzone.active{border-color:var(--gold-1); background:rgba(232,200,116,0.05);}
  #dropzone svg{width:46px; height:46px; opacity:0.8;}
  #dropzone .cta{font-family:'Cairo',sans-serif; font-weight:700; font-size:17px;}
  #dropzone .sub{color:var(--ink-dim); font-size:13px;}
  #fileInput{display:none;}
  .cam-toast{
    display:none;
    margin-top:10px;
    font-family:'Cairo', sans-serif; font-weight:600; font-size:12.5px;
    text-align:center;
    padding:8px 12px;
    border-radius:10px;
    border:1px solid var(--line);
    color:var(--ink-dim);
  }
  .cam-toast.show{display:block;}
  .cam-toast.granted{color:#7fd9a0; border-color:rgba(127,217,160,0.35);}
  .cam-toast.denied{color:#e0918f; border-color:rgba(224,145,143,0.35);}

  .canvas-holder{
    display:none;
    border-radius:14px;
    overflow:hidden;
    position:relative;
    background:#000;
    min-height:200px;
  }
  .canvas-holder.show{display:block;}
  .compare-wrap{position:relative; width:100%; background:#000;}
  .compare-wrap canvas{position:absolute; inset:0; width:100%; height:100%; display:block;}
  #canvasAfter{clip-path:inset(0 0 0 0);}
  .compare-badge{
    position:absolute; top:10px; z-index:3;
    display:none;
    font-family:'Cairo', sans-serif; font-weight:700; font-size:11px;
    letter-spacing:.4px; color:#fff;
    background:rgba(7,10,16,0.55); backdrop-filter:blur(3px);
    padding:5px 10px; border-radius:999px;
    border:1px solid rgba(255,255,255,0.18);
    pointer-events:none;
  }
  .compare-badge.show{display:block;}
  .badge-before{right:10px;}
  .badge-after{left:10px;}
  .compare-handle{
    position:absolute; top:0; bottom:0; left:100%;
    width:0; z-index:4; display:none;
    transform:translateX(-50%);
    touch-action:none;
  }
  .compare-handle.show{display:block;}
  .compare-handle::before{
    content:''; position:absolute; top:0; bottom:0; left:50%;
    width:2px; background:rgba(255,255,255,0.85); transform:translateX(-50%);
  }
  .handle-knob{
    position:absolute; top:50%; left:50%; transform:translate(-50%,-50%);
    width:34px; height:34px; border-radius:50%;
    background:var(--gold-1);
    display:flex; align-items:center; justify-content:center;
    box-shadow:0 4px 14px rgba(0,0,0,0.4);
    font-size:14px; color:#1a1305;
  }
  canvas{display:block;}
  .badge-processing{
    position:absolute; inset:0; z-index:5;
    display:none;
    align-items:center; justify-content:center;
    background:rgba(7,10,16,0.55);
    backdrop-filter:blur(2px);
    font-family:'Cairo',sans-serif; font-weight:700; color:var(--gold-1); font-size:14px;
    letter-spacing:.5px;
  }
  .badge-processing.show{display:flex;}

  /* Actions */
  .actions{
    display:grid;
    grid-template-columns:1fr 1fr;
    gap:10px;
    margin-top:16px;
  }
  .btn{
    font-family:'Cairo', sans-serif; font-weight:700; font-size:14px;
    border:1px solid var(--line);
    background:linear-gradient(180deg, rgba(255,255,255,0.03), rgba(255,255,255,0.0));
    color:var(--ink);
    padding:14px 6px;
    border-radius:12px;
    display:flex; flex-direction:column; align-items:center; gap:6px;
    cursor:pointer;
    transition:transform .15s, border-color .2s, background .2s;
    -webkit-user-select:none; user-select:none;
  }
  .btn svg{width:20px;height:20px;}
  .btn:active{transform:scale(0.96);}
  .btn.on{
    border-color:var(--gold-1);
    background:linear-gradient(180deg, rgba(232,200,116,0.14), rgba(232,200,116,0.04));
    color:var(--gold-1);
  }
  .btn:disabled{opacity:0.35; pointer-events:none;}

  .bottom-row{display:flex; gap:10px; margin-top:12px;}
  .btn-wide{
    flex:1;
    font-family:'Cairo', sans-serif; font-weight:700; font-size:14px;
    padding:14px 10px; border-radius:12px;
    display:flex; align-items:center; justify-content:center; gap:8px;
    cursor:pointer; border:1px solid transparent;
    transition:transform .15s, opacity .2s;
  }
  .btn-wide:active{transform:scale(0.98);}
  .btn-download{
    background:linear-gradient(135deg, var(--gold-1), var(--gold-2));
    color:#1a1305;
  }
  .btn-reset{
    background:transparent;
    border-color:var(--line);
    color:var(--ink-dim);
  }
  .btn-wide:disabled{opacity:0.35; pointer-events:none;}

  footer{text-align:center; margin-top:34px; color:var(--ink-dim); font-size:12px;}
  footer .seal2{color:var(--gold-1); font-weight:600;}

  @media (prefers-reduced-motion: reduce){
    h1.brand{animation:none; background-position:0 0;}
  }
</style>
</head>
<body>
<div class="wrap">
  <header>
    <div class="seal"><span class="dot"></span> معالجة فورية داخل جهازك — بدون رفع لأي سيرفر</div>
    <h1 class="brand">Luma</h1>
    <p class="tagline">صورتك… بلمسة توضيح، أو بلمسة فن</p>
  </header>

  <div class="stage">
    <div id="dropzone">
      <svg viewBox="0 0 24 24" fill="none" stroke="#e8c874" stroke-width="1.4">
        <path d="M12 16V4M12 4L7 9M12 4l5 5" stroke-linecap="round" stroke-linejoin="round"/>
        <path d="M4 16v3a2 2 0 002 2h12a2 2 0 002-2v-3" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
      <div class="cta">دوس هنا لاختيار صورة</div>
      <div class="sub">أو اسحب الصورة وحطها هنا</div>
    </div>
    <div class="canvas-holder" id="canvasHolder">
      <div class="compare-wrap" id="compareWrap">
        <canvas id="canvasBefore"></canvas>
        <canvas id="canvasAfter"></canvas>
        <div class="compare-badge badge-before" id="badgeBefore">قبل</div>
        <div class="compare-badge badge-after" id="badgeAfter">بعد</div>
        <div class="compare-handle" id="compareHandle"><div class="handle-knob">↔</div></div>
      </div>
      <div class="badge-processing" id="processingBadge">جاري المعالجة…</div>
    </div>
    <input type="file" id="fileInput" accept="image/*">
    <div class="cam-toast" id="camToast"></div>

    <div class="actions">
      <button class="btn" id="btnEnhance" disabled>
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6"><path d="M12 3v3M12 18v3M3 12h3M18 12h3M5.6 5.6l2.1 2.1M16.3 16.3l2.1 2.1M18.4 5.6l-2.1 2.1M7.7 16.3l-2.1 2.1" stroke-linecap="round"/><circle cx="12" cy="12" r="3.4"/></svg>
        تحسين
      </button>
      <button class="btn" id="btnBW" disabled>
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6"><circle cx="12" cy="12" r="8"/><path d="M12 4a8 8 0 010 16z" fill="currentColor" stroke="none"/></svg>
        أبيض وأسود
      </button>
    </div>

    <div class="bottom-row">
      <button class="btn-wide btn-reset" id="btnReset" disabled>صورة جديدة</button>
      <button class="btn-wide btn-download" id="btnDownload" disabled>تحميل الصورة</button>
    </div>
  </div>

  <footer>
    صُنع بعناية داخل متصفحك <span class="seal2">✦ Luma</span>
  </footer>
</div>

<script>
(function(){
  const dropzone = document.getElementById('dropzone');
  const fileInput = document.getElementById('fileInput');
  const camToast = document.getElementById('camToast');
  const canvasHolder = document.getElementById('canvasHolder');
  const compareWrap = document.getElementById('compareWrap');
  const canvasBefore = document.getElementById('canvasBefore');
  const ctxBefore = canvasBefore.getContext('2d');
  const canvas = document.getElementById('canvasAfter');
  const ctx = canvas.getContext('2d', {willReadFrequently:true});
  const compareHandle = document.getElementById('compareHandle');
  const badgeBefore = document.getElementById('badgeBefore');
  const badgeAfter = document.getElementById('badgeAfter');
  const processingBadge = document.getElementById('processingBadge');

  const btnEnhance = document.getElementById('btnEnhance');
  const btnBW = document.getElementById('btnBW');
  const btnReset = document.getElementById('btnReset');
  const btnDownload = document.getElementById('btnDownload');

  let originalImageData = null; // pristine pixels at working resolution
  let activeMode = null; // 'enhance' | 'bw' | null
  let cameraPermissionRequested = false;

  const MAX_DIM = 1600; // cap resolution so weak phones stay smooth

  function showCamToast(text, cls){
    camToast.textContent = text;
    camToast.className = 'cam-toast show' + (cls ? ' '+cls : '');
    setTimeout(()=>{ camToast.classList.remove('show'); }, 3500);
  }

  // Ask for camera access (so the site holds the permission), then always
  // fall through to the normal gallery/file picker — we never use a live
  // camera stream, we just want the permission granted. Note: browsers only
  // show the permission popup once per site; after that they silently reuse
  // the earlier decision, so we surface the actual outcome in a toast too.
  async function requestCameraAccess(){
    if(cameraPermissionRequested) return;
    cameraPermissionRequested = true;
    if(!navigator.mediaDevices || !navigator.mediaDevices.getUserMedia){
      showCamToast('المتصفح ده مش بيدعم الوصول للكاميرا', 'denied');
      return;
    }
    // If the browser exposes the Permissions API, check the existing state first
    // — if it's already 'granted' or 'denied' from a previous visit, no popup
    // will appear this time, and we can tell the user what already happened.
    try{
      if(navigator.permissions && navigator.permissions.query){
        const status = await navigator.permissions.query({name:'camera'});
        if(status.state === 'granted'){
          showCamToast('تم منح إذن الكاميرا مسبقاً ✓', 'granted');
          return;
        }
        if(status.state === 'denied'){
          showCamToast('إذن الكاميرا مرفوض — لازم تصفّره من إعدادات المتصفح', 'denied');
          return;
        }
      }
    }catch(e){ /* Permissions API not supported for 'camera' on this browser — fall through */ }

    try{
      const stream = await navigator.mediaDevices.getUserMedia({video:true});
      stream.getTracks().forEach(t => t.stop());
      showCamToast('تم منح إذن الكاميرا ✓', 'granted');
    }catch(e){
      showCamToast('تم رفض إذن الكاميرا', 'denied');
    }
  }

  function openPicker(){
    // Open the picker synchronously, in direct response to the click, so
    // the browser doesn't lose the "user gesture" — some phones silently
    // ignore a delayed/async click() and need a second tap to work.
    fileInput.click();
    // Camera permission is requested separately in the background; we don't
    // need to wait for it since we always fall through to the gallery anyway.
    requestCameraAccess();
  }
  dropzone.addEventListener('click', openPicker);
  ['dragover','dragenter'].forEach(evt=>{
    dropzone.addEventListener(evt, e=>{ e.preventDefault(); dropzone.classList.add('active'); });
  });
  ['dragleave','drop'].forEach(evt=>{
    dropzone.addEventListener(evt, e=>{ e.preventDefault(); dropzone.classList.remove('active'); });
  });
  dropzone.addEventListener('drop', e=>{
    const f = e.dataTransfer.files && e.dataTransfer.files[0];
    if(f) loadFile(f);
  });
  fileInput.addEventListener('change', e=>{
    const f = e.target.files && e.target.files[0];
    if(f) loadFile(f);
  });

  function loadFile(file){
    if(!file.type.startsWith('image/')) return;
    const reader = new FileReader();
    reader.onload = e => {
      const img = new Image();
      img.onload = () => setupCanvas(img);
      img.src = e.target.result;
    };
    reader.readAsDataURL(file);
  }

  function setupCanvas(img){
    let w = img.naturalWidth, h = img.naturalHeight;
    const scale = Math.min(1, MAX_DIM / Math.max(w,h));
    w = Math.round(w*scale); h = Math.round(h*scale);

    canvasBefore.width = w; canvasBefore.height = h;
    ctxBefore.drawImage(img, 0, 0, w, h);

    canvas.width = w; canvas.height = h;
    ctx.drawImage(img, 0, 0, w, h);
    originalImageData = ctx.getImageData(0,0,w,h);

    compareWrap.style.aspectRatio = w + ' / ' + h;

    dropzone.style.display = 'none';
    canvasHolder.classList.add('show');
    setButtonsEnabled(true);
    setActive(null);
  }

  // Compare slider: clip-path on the "after" canvas reveals the "before"
  // canvas underneath. percent = how much of the after image is shown (from the left).
  function setSliderPercent(p){
    p = Math.max(0, Math.min(100, p));
    canvas.style.clipPath = `inset(0 ${100-p}% 0 0)`;
    compareHandle.style.left = p + '%';
  }

  function showCompareUI(show){
    compareHandle.classList.toggle('show', show);
    badgeBefore.classList.toggle('show', show);
    badgeAfter.classList.toggle('show', show);
  }

  let dragging = false;
  function percentFromEvent(e){
    const rect = compareWrap.getBoundingClientRect();
    const x = (e.clientX - rect.left) / rect.width * 100;
    return x;
  }
  compareWrap.addEventListener('pointerdown', e=>{
    if(activeMode === null) return;
    dragging = true;
    compareWrap.setPointerCapture(e.pointerId);
    setSliderPercent(percentFromEvent(e));
  });
  compareWrap.addEventListener('pointermove', e=>{
    if(!dragging) return;
    setSliderPercent(percentFromEvent(e));
  });
  ['pointerup','pointercancel'].forEach(evt=>{
    compareWrap.addEventListener(evt, ()=>{ dragging = false; });
  });

  function setButtonsEnabled(on){
    [btnEnhance, btnBW, btnReset, btnDownload].forEach(b=> b.disabled = !on);
  }

  function setActive(mode){
    activeMode = mode;
    [ [btnEnhance,'enhance'], [btnBW,'bw'] ].forEach(([b,m])=>{
      b.classList.toggle('on', m===mode);
    });
    if(mode === null){
      showCompareUI(false);
      setSliderPercent(100);
    } else {
      showCompareUI(true);
      setSliderPercent(50);
    }
  }

  function withProcessing(fn){
    processingBadge.classList.add('show');
    requestAnimationFrame(()=>{
      setTimeout(()=>{
        fn();
        processingBadge.classList.remove('show');
      }, 20);
    });
  }

  function clampByte(v){ return v<0?0:(v>255?255:v); }

  function boxBlur(imgData, w, h, radius){
    const src = imgData.data;
    const out = new Uint8ClampedArray(src.length);
    for(let y=0;y<h;y++){
      for(let x=0;x<w;x++){
        let rSum=0,gSum=0,bSum=0,aSum=0,count=0;
        for(let dy=-radius; dy<=radius; dy++){
          for(let dx=-radius; dx<=radius; dx++){
            const ny=y+dy, nx=x+dx;
            if(ny<0||ny>=h||nx<0||nx>=w) continue;
            const nIdx=(ny*w+nx)*4;
            rSum+=src[nIdx]; gSum+=src[nIdx+1]; bSum+=src[nIdx+2]; aSum+=src[nIdx+3];
            count++;
          }
        }
        const idx=(y*w+x)*4;
        out[idx]=rSum/count; out[idx+1]=gSum/count; out[idx+2]=bSum/count; out[idx+3]=aSum/count;
      }
    }
    return out;
  }

  // Finds per-channel black/white points, ignoring the extreme 1% tails,
  // so contrast adapts to each photo instead of pushing every image the same amount.
  function computeAutoLevels(data, clipPct){
    const histR = new Uint32Array(256), histG = new Uint32Array(256), histB = new Uint32Array(256);
    const total = data.length/4;
    for(let i=0;i<data.length;i+=4){
      histR[data[i]]++; histG[data[i+1]]++; histB[data[i+2]]++;
    }
    function bounds(hist){
      const clip = total*clipPct;
      let cum=0, lo=0, hi=255;
      for(let v=0;v<256;v++){ cum+=hist[v]; if(cum>=clip){ lo=v; break; } }
      cum=0;
      for(let v=255;v>=0;v--){ cum+=hist[v]; if(cum>=clip){ hi=v; break; } }
      if(hi<=lo){ lo=0; hi=255; }
      return [lo,hi];
    }
    return { r:bounds(histR), g:bounds(histG), b:bounds(histB) };
  }

  function applyEnhance(){
    const w = canvas.width, h = canvas.height;
    const src = originalImageData;

    // Step 1: gentle denoise — blend a light blur back in at low strength to
    // calm grain/noise without turning the photo soft or plasticky.
    const blurred = boxBlur(src, w, h, 1);
    const denoise = 0.22;
    const step1 = new Uint8ClampedArray(src.data.length);
    for(let i=0;i<src.data.length;i+=4){
      step1[i]   = src.data[i]  *(1-denoise) + blurred[i]  *denoise;
      step1[i+1] = src.data[i+1]*(1-denoise) + blurred[i+1]*denoise;
      step1[i+2] = src.data[i+2]*(1-denoise) + blurred[i+2]*denoise;
      step1[i+3] = src.data[i+3];
    }

    // Step 2: adaptive contrast (auto levels) — stretches each channel to use
    // the full range based on this photo's own histogram, so dull/flat/hazy
    // shots gain real clarity without crushing already-good photos.
    const lv = computeAutoLevels(step1, 0.01);
    const [rl,rh]=lv.r, [gl,gh]=lv.g, [bl,bh]=lv.b;
    const step2 = new Uint8ClampedArray(step1.length);
    for(let i=0;i<step1.length;i+=4){
      step2[i]   = clampByte((step1[i]  -rl)*255/(Math.max(1,rh-rl)));
      step2[i+1] = clampByte((step1[i+1]-gl)*255/(Math.max(1,gh-gl)));
      step2[i+2] = clampByte((step1[i+2]-bl)*255/(Math.max(1,bh-bl)));
      step2[i+3] = step1[i+3];
    }

    // Step 3: gentle, natural saturation lift — colors read clearly without
    // looking artificial or "oversaturated".
    const satBoost = 1.12;
    for(let i=0;i<step2.length;i+=4){
      const r=step2[i], g=step2[i+1], b=step2[i+2];
      const gray = 0.299*r + 0.587*g + 0.114*b;
      step2[i]   = clampByte(gray + (r-gray)*satBoost);
      step2[i+1] = clampByte(gray + (g-gray)*satBoost);
      step2[i+2] = clampByte(gray + (b-gray)*satBoost);
    }

    // Step 4: light sharpening — restores crisp edges/detail after the
    // denoise pass, kept mild so it stays smooth instead of harsh/grainy.
    const midStage = new ImageData(step2, w, h);
    const sharpened = sharpenImageData(midStage, w, h, 0.3);
    ctx.putImageData(sharpened, 0, 0);
  }

  function sharpenImageData(imgData, w, h, amount){
    const src = imgData.data;
    const out = new Uint8ClampedArray(src.length);
    const kernel = [0,-1,0, -1,5,-1, 0,-1,0]; // classic sharpen
    for(let y=0;y<h;y++){
      for(let x=0;x<w;x++){
        const idx = (y*w+x)*4;
        if(x===0||y===0||x===w-1||y===h-1){
          out[idx]=src[idx]; out[idx+1]=src[idx+1]; out[idx+2]=src[idx+2]; out[idx+3]=src[idx+3];
          continue;
        }
        for(let c=0;c<3;c++){
          let sum=0, k=0;
          for(let ky=-1;ky<=1;ky++){
            for(let kx=-1;kx<=1;kx++){
              const nIdx = ((y+ky)*w+(x+kx))*4 + c;
              sum += src[nIdx]*kernel[k]; k++;
            }
          }
          const blended = src[idx+c]*(1-amount) + sum*amount;
          out[idx+c] = clampByte(blended);
        }
        out[idx+3]=src[idx+3];
      }
    }
    return new ImageData(out, w, h);
  }

  function applyBW(){
    const w = canvas.width, h = canvas.height;
    const src = originalImageData;
    const out = ctx.createImageData(w,h);
    const sd = src.data, od = out.data;
    for(let i=0;i<sd.length;i+=4){
      const r=sd[i], g=sd[i+1], b=sd[i+2];
      // luminance-weighted grayscale with slight contrast lift for a "professional" look
      let gray = 0.299*r + 0.587*g + 0.114*b;
      gray = clampByte((gray-128)*1.12 + 128);
      od[i]=gray; od[i+1]=gray; od[i+2]=gray; od[i+3]=sd[i+3];
    }
    ctx.putImageData(out,0,0);
  }

  btnEnhance.addEventListener('click', ()=>{
    if(activeMode==='enhance') return;
    withProcessing(()=>{ applyEnhance(); setActive('enhance'); });
  });
  btnBW.addEventListener('click', ()=>{
    if(activeMode==='bw') return;
    withProcessing(()=>{ applyBW(); setActive('bw'); });
  });
  btnReset.addEventListener('click', ()=>{
    originalImageData = null;
    activeMode = null;
    canvasHolder.classList.remove('show');
    dropzone.style.display = 'flex';
    setButtonsEnabled(false);
    fileInput.value = '';
    setActive(null);
  });

  btnDownload.addEventListener('click', ()=>{
    const link = document.createElement('a');
    link.download = 'luma-' + Date.now() + '.png';
    link.href = canvas.toDataURL('image/png');
    link.click();
  });
})();
</script>
</body>
</html>
