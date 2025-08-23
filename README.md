<!DOCTYPE html>
<html lang="en">
<head>
    <!-- FOUC Prevention - Add this FIRST in head -->
    <style>
        html { background-color: #000; }
        body {
            visibility: hidden; opacity: 0; margin: 0; padding: 0; height: 100%;
            overflow: hidden; background: #000; font-family: 'Times New Roman', serif;
        }
        body.loaded { visibility: visible; opacity: 1; transition: opacity 0.5s ease; }
    </style>

    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <title>dread.services</title>

    <!-- ====== Styles (kept your original look & feel) ====== -->
    <style>
        .youtube-container{position:fixed;top:0;left:0;width:100%;height:145%;opacity:0;transition:opacity 1s ease-in-out;z-index:1;pointer-events:none}
        .youtube-container.playing{opacity:1}
        .youtube-iframe{width:100%;height:100%;border:none;transform:translateY(-15%)}
        .overlay{position:fixed;top:0;left:0;width:100%;height:100%;background:#000;display:flex;flex-direction:column;justify-content:center;align-items:center;z-index:2}
        .overlay.hidden{opacity:0;pointer-events:none;transition:opacity 1s ease-in-out}
        .title-text{color:#fff;font-size:min(5vw,36px);text-transform:uppercase;letter-spacing:4px;margin-bottom:20px;text-shadow:0 0 8px rgba(255,255,255,.5)}
        .subtitle-text{color:#FF0;font-size:min(3vw,24px);letter-spacing:2px;margin-bottom:40px;text-shadow:0 0 8px rgba(255,255,0,.7)}
        .enter-button,.test-button,.btn{padding:20px 40px;font-size:min(5vw,28px);font-weight:700;color:#fff;background:linear-gradient(145deg,#1a1a1a,#333);border:2px solid #444;border-top:2px solid #666;border-bottom:2px solid #222;border-radius:0;text-transform:uppercase;letter-spacing:4px;cursor:pointer;position:relative;overflow:hidden;box-shadow:0 0 20px rgba(255,0,0,.7);transition:all .3s ease;text-shadow:0 0 8px rgba(255,255,255,.5);min-width:250px;text-align:center}
        .enter-button:hover,.test-button:hover,.btn:hover{box-shadow:0 0 30px rgba(255,0,0,.9);transform:scale(1.05);text-shadow:0 0 12px rgba(255,255,255,.8)}
        .click-animation{position:absolute;width:100px;height:100px;background:radial-gradient(circle,rgba(255,0,0,.8) 0,rgba(255,0,0,0) 70%);border-radius:50%;transform:scale(0);opacity:0;pointer-events:none;mix-blend-mode:screen}
        .animate{animation:ripple 1s ease-out}
        @keyframes ripple{to{transform:scale(3);opacity:0}}
        .login-button,.logout-button{position:fixed;bottom:50px;right:20px;padding:10px 20px;font-size:16px;font-weight:700;color:#fff;background:linear-gradient(145deg,#1a1a1a,#333);border:2px solid #444;border-top:2px solid #666;border-bottom:2px solid #222;border-radius:0;text-transform:uppercase;letter-spacing:2px;cursor:pointer;z-index:1002;box-shadow:0 0 10px rgba(255,0,0,.5);transition:all .3s ease;text-shadow:0 0 5px rgba(255,255,255,.5);opacity:0}
        .login-button.visible,.logout-button.visible{opacity:1}
        .login-button:hover,.logout-button:hover{box-shadow:0 0 15px rgba(255,0,0,.8);transform:scale(1.05)}
        .volume-container{position:fixed;bottom:50px;left:20px;display:flex;align-items:center;z-index:100;opacity:0;transition:opacity .5s ease}
        .volume-container.visible{opacity:1}
        .volume-slider{-webkit-appearance:none;width:120px;height:8px;background:linear-gradient(145deg,#1a1a1a,#333);border:2px solid #444;border-top:2px solid #666;border-bottom:2px solid #222;outline:0;margin-left:10px;box-shadow:0 0 10px rgba(255,0,0,.5)}
        .volume-slider::-webkit-slider-thumb{-webkit-appearance:none;appearance:none;width:18px;height:18px;background:#fff;cursor:pointer;border:2px solid #444;box-shadow:0 0 5px rgba(255,0,0,.7)}
        .volume-slider::-moz-range-thumb{width:18px;height:18px;background:#fff;cursor:pointer;border:2px solid #444;box-shadow:0 0 5px rgba(255,0,0,.7)}
        .volume-icon{color:#fff;font-size:24px;text-shadow:0 0 5px rgba(255,0,0,.7)}
        .login-page{position:fixed;top:0;left:0;width:100%;height:100%;background:#000;display:flex;flex-direction:column;justify-content:center;align-items:center;z-index:1000;opacity:0;pointer-events:none;transition:opacity .5s ease}
        .login-page.visible{opacity:1;pointer-events:all}
        .login-title{color:#fff;font-size:min(5vw,36px);text-transform:uppercase;letter-spacing:4px;margin-bottom:40px;text-shadow:0 0 8px rgba(255,255,255,.5)}
        .input-container{margin-bottom:20px;text-align:center}
        .input-label{display:block;color:#FF0;font-size:min(3vw,18px);margin-bottom:8px;text-shadow:0 0 8px rgba(255,255,0,.7)}
        .input-field{padding:15px 25px;font-size:min(4vw,20px);color:#fff;background:linear-gradient(145deg,#1a1a1a,#333);border:2px solid #444;border-top:2px solid #666;border-bottom:2px solid #222;border-radius:0;text-transform:uppercase;letter-spacing:2px;min-width:250px;text-align:center;box-shadow:0 0 10px rgba(255,0,0,.5);transition:all .3s ease}
        .input-field:focus{outline:0;box-shadow:0 0 15px rgba(255,0,0,.8)}
        .login-submit{margin-top:30px;padding:15px 30px;font-size:min(4vw,20px);font-weight:700;color:#fff;background:linear-gradient(145deg,#1a1a1a,#333);border:2px solid #444;border-top:2px solid #666;border-bottom:2px solid #222;border-radius:0;text-transform:uppercase;letter-spacing:2px;cursor:pointer;box-shadow:0 0 10px rgba(255,0,0,.5);transition:all .3s ease}
        .login-submit:hover{box-shadow:0 0 20px rgba(255,0,0,.9);transform:scale(1.05)}
        .login-message{margin-top:20px;color:#F00;font-size:min(3vw,16px);text-shadow:0 0 8px rgba(255,0,0,.7);height:20px}
        .close-login{position:absolute;top:20px;right:20px;color:#fff;font-size:24px;cursor:pointer;text-shadow:0 0 8px rgba(255,0,0,.7)}
        .logged-in-page{position:fixed;top:0;left:0;width:100%;height:100%;background:#000;display:flex;justify-content:center;align-items:center;z-index:1000;color:#fff;font-size:min(10vw,72px);text-transform:uppercase;letter-spacing:8px;text-shadow:0 0 20px rgba(255,0,0,.9);opacity:0;pointer-events:none;transition:opacity 1s ease}
        .logged-in-page.visible{opacity:1;pointer-events:all}

        /* === Flipper page (replaces test page) === */
        .flipper-page{position:fixed;top:0;left:0;width:100%;height:100%;background:#000;display:flex;flex-direction:column;z-index:1001;opacity:0;pointer-events:none;transition:opacity 1s ease}
        .flipper-page.visible{opacity:1;pointer-events:all}
        .flipper-header{display:flex;align-items:center;justify-content:space-between;padding:16px 20px;background:rgba(20,20,20,.9);border-bottom:1px solid #333}
        .pill{color:#000;background:#FF0;padding:6px 10px;font-weight:700;letter-spacing:1px}
        .controls{display:flex;flex-wrap:wrap;gap:10px}
        .control{display:flex;align-items:center;gap:8px;color:#fff}
        .control input,.control select{background:#111;color:#fff;border:1px solid #333;padding:6px 10px}
        .flipper-table{width:100%;border-collapse:collapse}
        .flipper-table th,.flipper-table td{padding:10px;border-bottom:1px solid #222;color:#ddd;text-align:right}
        .flipper-table th{position:sticky;top:0;background:#0b0b0b;z-index:1;text-align:right}
        .flipper-table td:first-child,.flipper-table th:first-child{text-align:left}
        .green{color:#0f0}
        .red{color:#f33}
        .tag{display:inline-block;padding:2px 6px;border:1px solid #444;background:#111;color:#aaa;font-size:12px;margin-left:6px}
        .table-wrap{flex:1;overflow:auto}
        .footer{padding:8px 16px;color:#999;background:#0b0b0b;border-top:1px solid #222}
        .linkish{cursor:pointer;text-decoration:underline}
    </style>
</head>
<body>
    <div id="preloader" style="position:fixed;top:0;left:0;width:100%;height:100%;background:#000;z-index:9999;display:none;"></div>
    <div id="youtubeContainer" class="youtube-container"></div>
    <audio id="backgroundMusic"></audio>

    <div class="overlay" id="overlay">
        <div class="title-text">dread.services</div>
        <div class="subtitle-text">Work In Progress</div>
        <button class="enter-button" id="enterButton">CLICK TO ENTER</button>
    </div>

    <div class="volume-container" id="volumeContainer">
        <div class="volume-icon">🔊</div>
        <input type="range" min="0" max="1" step="0.01" value="0.5" class="volume-slider" id="volumeSlider">
    </div>

    <button class="login-button" id="loginButton">LOGIN</button>
    <button class="logout-button" id="logoutButton">LOGOUT</button>

    <!-- Login modal -->
    <div class="login-page" id="loginPage">
        <span class="close-login" id="closeLogin">&times;</span>
        <div class="login-title">LOGIN</div>
        <div class="input-container"><label class="input-label" for="username">ENTER USERNAME</label><input type="text" id="username" class="input-field"></div>
        <div class="input-container"><label class="input-label" for="password">ENTER PASSWORD</label><input type="password" id="password" class="input-field"></div>
        <button class="login-submit" id="loginSubmit">LOGIN</button>
        <div class="login-message" id="loginMessage"></div>
    </div>

    <div class="logged-in-page" id="loggedInPage">LOGGED IN</div>

    <!-- ===== Bazaar Flipper (replaces test page) ===== -->
    <div class="flipper-page" id="flipperPage">
        <div class="flipper-header">
            <div style="display:flex;gap:10px;align-items:center;flex-wrap:wrap">
                <div class="title-text" style="margin:0">Bazaar Flipper</div>
                <span class="pill" id="mayorPill">Mayor: …</span>
                <span class="pill" id="taxPill">Effective Tax: …</span>
                <span class="tag" id="statusTag">Idle</span>
            </div>
            <div class="controls">
                <div class="control"><label>Perk:</label>
                    <select id="perkSelect">
                        <option value="1.25">1.25% (no perk)</option>
                        <option value="1.00">1.0% (Flipper perk max)</option>
                    </select>
                </div>
                <div class="control"><label>Min Net Margin (coins):</label><input id="minMargin" type="number" value="1000" step="1"></div>
                <div class="control"><label>Min Fills / min:</label><input id="minVpm" type="number" value="10" step="1"></div>
                <div class="control"><label>Max ETA (min):</label><input id="maxEta" type="number" value="5" step="0.1"></div>
                <div class="control"><label>PPM Floor:</label><input id="ppmFloor" type="number" value="0" step="1"></div>
                <button class="btn" id="refreshBtn">Refresh</button>
            </div>
        </div>
        <div class="table-wrap">
            <table class="flipper-table" id="flipperTable">
                <thead>
                    <tr>
                        <th>Item</th>
                        <th>Best Sell</th>
                        <th>Best Buy</th>
                        <th>Spread %</th>
                        <th>Net Margin (u)</th>
                        <th>Vol/min</th>
                        <th>ETA (m)</th>
                        <th>PPM</th>
                        <th>PPH</th>
                        <th>Qty</th>
                        <th>Bid / Ask</th>
                        <th>Stability</th>
                    </tr>
                </thead>
                <tbody id="flipperBody"></tbody>
            </table>
        </div>
        <div class="footer">
            Data: Hypixel Bazaar & Election endpoints • Sorted by Profit/Minute • Filters applied client-side • Conservative pricing to avoid red flips.
            <span class="linkish" id="resetFilters" style="margin-left:10px">Reset Filters</span>
        </div>
    </div>

    <script>
        // ===== Secure credential storage (as you had) =====
        const SECURE = { user: '1', pass: '1', token: 'dread_' + btoa('r00do:admin').split('').reverse().join('') + '_' + Math.random().toString(36).substring(2, 15) };

        // ===== Music setup (unchanged) =====
        const songs = ['Scars.mp3','Rip.mp3','Zero.mp3','Plata.mp3','Peach.mp3','Leave.mp3','Peace.mp3','Foreal.mp3','Prove.mp3','Newer.mp3','Hair.mp3'];
        let youtubeIframe = null;
        function shuffleArray(a){for(let i=a.length-1;i>0;i--){const j=Math.floor(Math.random()*(i+1));[a[i],a[j]]=[a[j],a[i]]}return a}
        function playShuffledSongs(){const music=document.getElementById('backgroundMusic');let s=shuffleArray([...songs]);let i=0;function next(){if(i>=s.length){s=shuffleArray([...songs]);i=0}const song=s[i];music.src=`music/${song}`;music.play();i++}music.addEventListener('ended',next);next()}
        function fadeOutMusic(d=2000){const m=document.getElementById('backgroundMusic');const sv=m.volume;const iv=50;const steps=d/iv;const step=sv/steps;const t=setInterval(()=>{if(m.volume>step){m.volume-=step}else{m.volume=0;clearInterval(t);m.pause()}},iv)}
        function pauseAllMedia(){fadeOutMusic();if(youtubeIframe){youtubeIframe.contentWindow.postMessage('{"event":"command","func":"pauseVideo","args":""}','*')}}
        function resumeAllMedia(){const m=document.getElementById('backgroundMusic');m.volume=document.getElementById('volumeSlider').value;m.play();if(youtubeIframe){youtubeIframe.contentWindow.postMessage('{"event":"command","func":"playVideo","args":""}','*')}}

        // ===== Auth helpers =====
        function setAuth(){const now=new Date();now.setTime(now.getTime()+60*60*1000);document.cookie=`dread_auth=${SECURE.token}; expires=${now.toUTCString()}; path=/; SameSite=Strict`;localStorage.setItem('dread_auth',SECURE.token)}
        function checkAuth(){const cookieAuth=document.cookie.split(';').some(i=>i.trim().startsWith('dread_auth='+SECURE.token));const storageAuth=localStorage.getItem('dread_auth')===SECURE.token;return cookieAuth||storageAuth}
        function clearAuth(){document.cookie='dread_auth=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/';localStorage.removeItem('dread_auth')}

        // ===== DOM refs =====
        const youtubeContainer=document.getElementById('youtubeContainer');
        const music=document.getElementById('backgroundMusic');
        const overlay=document.getElementById('overlay');
        const enterButton=document.getElementById('enterButton');
        const loginButton=document.getElementById('loginButton');
        const logoutButton=document.getElementById('logoutButton');
        const loginPage=document.getElementById('loginPage');
        const closeLogin=document.getElementById('closeLogin');
        const loginSubmit=document.getElementById('loginSubmit');
        const usernameInput=document.getElementById('username');
        const passwordInput=document.getElementById('password');
        const loginMessage=document.getElementById('loginMessage');
        const loggedInPage=document.getElementById('loggedInPage');
        const volumeContainer=document.getElementById('volumeContainer');
        const volumeSlider=document.getElementById('volumeSlider');

        // ===== Flipper refs =====
        const flipperPage=document.getElementById('flipperPage');
        const flipperBody=document.getElementById('flipperBody');
        const mayorPill=document.getElementById('mayorPill');
        const taxPill=document.getElementById('taxPill');
        const statusTag=document.getElementById('statusTag');
        const perkSelect=document.getElementById('perkSelect');
        const minMargin=document.getElementById('minMargin');
        const minVpm=document.getElementById('minVpm');
        const maxEta=document.getElementById('maxEta');
        const ppmFloor=document.getElementById('ppmFloor');
        const refreshBtn=document.getElementById('refreshBtn');
        const resetFilters=document.getElementById('resetFilters');

        // ===== General UI =====
        volumeSlider.addEventListener('input', e=>{music.volume=e.target.value});
        music.volume=volumeSlider.value;
        function loadYouTubeVideo(){youtubeContainer.innerHTML=`<iframe class="youtube-iframe" src="https://www.youtube.com/embed/sKu8Zg7Hplc?autoplay=1&controls=0&disablekb=1&fs=0&loop=1&modestbranding=1&playsinline=1&rel=0&showinfo=0&mute=1&iv_load_policy=3&cc_load_policy=0&playlist=sKu8Zg7Hplc&enablejsapi=1" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>`;youtubeIframe=youtubeContainer.querySelector('.youtube-iframe');setTimeout(()=>youtubeContainer.classList.add('playing'),300)}

        // ===== App lifecycle =====
        document.addEventListener('DOMContentLoaded',()=>{
            document.body.classList.add('loaded');
            loginButton.style.display='none';
            logoutButton.style.display='none';
            volumeContainer.style.display='none';
            if(checkAuth()){
                loggedInPage.classList.add('visible');
                overlay.style.display='none';
                loadYouTubeVideo();
                playShuffledSongs();
                logoutButton.style.display='block';
                logoutButton.classList.add('visible');
                volumeContainer.style.display='flex';
                volumeContainer.classList.add('visible');
                setTimeout(()=>{loggedInPage.classList.remove('visible'); flipperPage.classList.add('visible'); fadeOutMusic(); startFlipper();},3000);
            }
        });

        enterButton.addEventListener('click', (e)=>{
            const ripple=document.createElement('div');ripple.classList.add('click-animation');const rect=enterButton.getBoundingClientRect();ripple.style.left=(e.clientX-rect.left-50)+'px';ripple.style.top=(e.clientY-rect.top-50)+'px';enterButton.appendChild(ripple);setTimeout(()=>ripple.classList.add('animate'),10);setTimeout(()=>ripple.remove(),1000);
            loadYouTubeVideo();
            playShuffledSongs();
            enterButton.style.display='none';
            loginButton.style.display='block';
            volumeContainer.style.display='flex';
            setTimeout(()=>{loginButton.classList.add('visible');volumeContainer.classList.add('visible');},500);
            overlay.classList.add('hidden');
        });
        loginButton.addEventListener('click',()=>{pauseAllMedia();loginPage.classList.add('visible')});
        closeLogin.addEventListener('click',()=>{loginPage.classList.remove('visible');loginMessage.textContent='';resumeAllMedia()});
        passwordInput.addEventListener('keypress',e=>{if(e.key==='Enter')loginSubmit.click()});
        loginSubmit.addEventListener('click',()=>{
            const u=usernameInput.value.trim(); const p=passwordInput.value.trim();
            if(!u||!p){loginMessage.textContent='Please enter both username and password';return}
            if(u===SECURE.user && p===SECURE.pass){
                loginMessage.textContent='Login successful!'; loginMessage.style.color='#0F0'; setAuth();
                setTimeout(()=>{loginPage.classList.remove('visible'); loginButton.classList.remove('visible');
                    setTimeout(()=>{loggedInPage.classList.add('visible'); setTimeout(()=>{loggedInPage.classList.remove('visible'); flipperPage.classList.add('visible'); fadeOutMusic(); logoutButton.style.display='block'; setTimeout(()=>logoutButton.classList.add('visible'),100); startFlipper();},3000);},1000);
                    usernameInput.value=''; passwordInput.value='';
                },1000);
            } else { loginMessage.textContent='Invalid credentials'; loginMessage.style.color='#F00'; }
        });
        logoutButton.addEventListener('click',()=>{resetToInitialState()});
        function resetToInitialState(){loginPage.classList.remove('visible'); loggedInPage.classList.remove('visible'); flipperPage.classList.remove('visible'); overlay.classList.remove('hidden'); enterButton.style.display='block'; loginButton.style.display='none'; loginButton.classList.remove('visible'); logoutButton.style.display='none'; logoutButton.classList.remove('visible'); volumeContainer.style.display='none'; volumeContainer.classList.remove('visible'); clearAuth(); pauseAllMedia(); youtubeContainer.innerHTML=''; youtubeContainer.classList.remove('playing'); youtubeIframe=null; stopFlipper()}

        // ===== Anti-tamper (as you had) =====
        document.addEventListener('contextmenu', e=>e.preventDefault());
        document.addEventListener('keydown', e=>{
            if(e.key==='F12'||(e.ctrlKey&&e.shiftKey&&['I','J','C'].includes(e.key))){e.preventDefault();alert('Access restricted');return false}
            if(e.ctrlKey&&e.key==='u'){e.preventDefault();return false}
        });
        setInterval(()=>{if(window.console&&console.log){console.log=function(){};console.warn=function(){};console.error=function(){}}},1000);

        // ===== Bazaar Flipper core =====
        const API_BAZAAR='https://api.hypixel.net/v2/skyblock/bazaar';
        const API_ELECTION='https://api.hypixel.net/v2/resources/skyblock/election';
        const TICK=0.1; // min price increment
        let loopHandle=null, mayorHandle=null;
        let effectiveTax=0.0125; // default
        let mayorName='Unknown';

        async function getEffectiveTax(){
            try{
                const res=await fetch(API_ELECTION,{cache:'no-store'}); const json=await res.json();
                mayorName=(json && json.mayor && json.mayor.name) ? json.mayor.name : 'None';
                const base=parseFloat(perkSelect.value)/100; // 0.0125 or 0.01
                const mult = (mayorName==='Derpy')?4.0 : (mayorName==='Dante'?2.0:1.0);
                effectiveTax = base * mult;
                mayorPill.textContent = `Mayor: ${mayorName}`;
                taxPill.textContent = `Effective Tax: ${(effectiveTax*100).toFixed(2)}%`;
            }catch(e){ statusTag.textContent='Election fetch failed'; }
        }

        async function fetchBazaar(){
            const res=await fetch(API_BAZAAR,{cache:'no-store'}); return (await res.json()).products || {};
        }

        function depthQtyFromTop(summary, maxWorsenPct){
            // Sum amount until price moves against us by >= maxWorsenPct
            if(!Array.isArray(summary)||summary.length===0) return 0;
            const top=summary[0].pricePerUnit; let total=0; for(const lvl of summary){
                const worsen = Math.abs((lvl.pricePerUnit - top)/top);
                if(worsen >= maxWorsenPct) break; total += (lvl.amount||0);
            }
            return total;
        }

        function scoreItem(pid, data){
            const sellSum=data.sell_summary||[]; const buySum=data.buy_summary||[]; if(!sellSum.length||!buySum.length) return null;
            const bestSell=sellSum[0].pricePerUnit; // what you pay if insta-buy
            const bestBuy=buySum[0].pricePerUnit;   // what you get if insta-sell
            if(!(bestSell>0 && bestBuy>0)) return null;

            // Conservative order prices
            const slip=Math.max(0.1, 0.001*bestSell);
            const bid=Math.min(bestSell - slip, bestBuy + TICK);
            const ask=Math.max(bestBuy + slip, bestSell - TICK);
            if(ask <= bid) return null;

            const rawMargin=ask - bid;
            const netMargin=rawMargin * (1 - effectiveTax);
            const spreadPct = netMargin / bid;

            const qs=data.quick_status||{};
            const buyVpm=Math.max(1,(qs.buyVolume||0)/60);
            const sellVpm=Math.max(1,(qs.sellVolume||0)/60);
            const fillsVpm=Math.min(buyVpm, sellVpm);

            // Guardrails
            const volFloor=parseFloat(minVpm.value||'10');
            const nmFloor=parseFloat(minMargin.value||'1000');
            const etaMax=parseFloat(maxEta.value||'5');
            if(!(netMargin>nmFloor)) return {reject:true, reason:'low-margin'};
            if(!(fillsVpm>=volFloor)) return {reject:true, reason:'low-volume'};

            // Depth-based sizing (0.3% tolerance, take 35%)
            const safeDepth = Math.min(
                depthQtyFromTop(sellSum, 0.003), // for our buy order
                depthQtyFromTop(buySum, 0.003)   // for our sell order
            );
            const orderQty = Math.floor(Math.max(1, safeDepth * 0.35));
            const etaBuy = orderQty / sellVpm;
            const etaSell = orderQty / buyVpm;
            const etaTotal = etaBuy + etaSell;

            const ppm = (netMargin * orderQty) / Math.max(etaTotal,1);
            const pph = ppm * 60;

            const ppmMin=parseFloat(ppmFloor.value||'0');
            if(etaTotal > etaMax && ppm < ppmMin) return {reject:true, reason:'slow-eta'};

            // Stability tag
            let stability='STEADY';
            if ((sellSum[0].amount||0)<5 || (buySum[0].amount||0)<5) stability='UNSTABLE';

            return { id: pid, bestSell, bestBuy, spreadPct, netMargin, fillsVpm, etaTotal, ppm, pph, orderQty, bid, ask, stability };
        }

        function renderRows(items){
            flipperBody.innerHTML='';
            const fmt=n=>Number.isFinite(n)?n.toLocaleString(undefined,{maximumFractionDigits:1}):'—';
            const fm2=n=>Number.isFinite(n)?n.toLocaleString(undefined,{maximumFractionDigits:2}):'—';
            for(const it of items){
                const tr=document.createElement('tr');
                tr.innerHTML=`
                    <td>${it.id.replaceAll('_',' ')}${it.stability!=='STEADY'?` <span class="tag">${it.stability}</span>`:''}</td>
                    <td>${fmt(it.bestSell)}</td>
                    <td>${fmt(it.bestBuy)}</td>
                    <td>${fm2(it.spreadPct*100)}%</td>
                    <td class="${it.netMargin>0?'green':'red'}">${fm2(it.netMargin)}</td>
                    <td>${fmt(it.fillsVpm)}</td>
                    <td>${fm2(it.etaTotal)}</td>
                    <td class="green">${fm2(it.ppm)}</td>
                    <td class="green">${fmt(Math.floor(it.pph))}</td>
                    <td>${fmt(it.orderQty)}</td>
                    <td>${fm2(it.bid)} / ${fm2(it.ask)}</td>
                    <td>${it.stability}</td>`;
                flipperBody.appendChild(tr);
            }
        }

        async function tick(){
            statusTag.textContent='Updating…';
            try{
                const products=await fetchBazaar();
                const scored=[];
                for(const [pid,data] of Object.entries(products)){
                    const s=scoreItem(pid,data);
                    if(s && !s.reject) scored.push(s);
                }
                // Sort by best Profit per Minute
                scored.sort((a,b)=> b.ppm - a.ppm);
                renderRows(scored);
                statusTag.textContent=`${scored.length} items`;
            }catch(e){ statusTag.textContent='API error'; }
        }

        function startFlipper(){ getEffectiveTax(); tick();
            loopHandle=setInterval(tick, 5000); // 2–5s recommended; we use 5s
            mayorHandle=setInterval(getEffectiveTax, 60000);
        }
        function stopFlipper(){ if(loopHandle){clearInterval(loopHandle); loopHandle=null} if(mayorHandle){clearInterval(mayorHandle); mayorHandle=null} }

        // Controls
        refreshBtn.addEventListener('click',()=>tick());
        resetFilters.addEventListener('click',()=>{perkSelect.value='1.25'; minMargin.value='1000'; minVpm.value='10'; maxEta.value='5'; ppmFloor.value='0'; getEffectiveTax(); tick();});
        perkSelect.addEventListener('change',()=>getEffectiveTax());

        // Responsive font sizing (kept)
        window.addEventListener('resize',()=>{const w=window.innerWidth; enterButton.style.fontSize=`${Math.min(w*0.05,28)}px`}); window.dispatchEvent(new Event('resize'));
    </script>
</body>
</html>
