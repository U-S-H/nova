<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA OMNI | Worldwide Institutional Terminal</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --bg: #010409; --card: #0d1117; --accent: #00ff88; --text: #f0f6fc; --dim: #8b949e; --gold: #f2cc60; --danger: #ff4d4d; }
        body { margin: 0; font-family: 'Plus Jakarta Sans', sans-serif; background: var(--bg); color: var(--text); overflow-x: hidden; padding-bottom: 100px; }
        
        /* Global Ticker */
        #global-ticker { background: #161b22; color: var(--accent); padding: 10px; font-size: 10px; font-weight: 800; white-space: nowrap; overflow: hidden; border-bottom: 1px solid #30363d; }
        .ticker-wrap { display: inline-block; animation: drift 30s linear infinite; }
        @keyframes drift { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }

        /* Login System */
        #auth-screen { min-height: 100vh; display: flex; flex-direction: column; align-items: center; justify-content: center; padding: 25px; background: radial-gradient(circle at top, #0a2e1f, #010409); }
        .logo-box h1 { font-size: 4rem; margin: 0; letter-spacing: -4px; cursor: pointer; user-select: none; transition: 0.3s; }
        .glass { background: var(--card); border: 1px solid rgba(255,255,255,0.05); border-radius: 40px; padding: 35px; width: 100%; max-width: 400px; box-shadow: 0 25px 60px rgba(0,0,0,0.6); }
        input { width: 100%; padding: 18px; background: #161b22; border: 1px solid #30363d; color: #fff; border-radius: 22px; margin-bottom: 15px; outline: none; }
        .btn-prime { width: 100%; padding: 20px; background: var(--accent); color: #000; border: none; border-radius: 22px; font-weight: 900; cursor: pointer; text-transform: uppercase; box-shadow: 0 10px 20px rgba(0,255,136,0.2); }

        /* Hero & Dashboard */
        .mining-hero { background: linear-gradient(135deg, #00ff88, #008f4c); border-radius: 40px; padding: 30px; margin: 15px; color: #000; position: relative; overflow: hidden; }
        .reward-btn { position: absolute; top: 15px; right: 15px; background: #000; color: #fff; border: none; padding: 8px 12px; border-radius: 50px; font-size: 10px; font-weight: 900; cursor: pointer; animation: pulse 2s infinite; }
        @keyframes pulse { 0% { transform: scale(1); } 50% { transform: scale(1.1); } 100% { transform: scale(1); } }
        
        .m-bar-bg { background: rgba(0,0,0,0.1); height: 8px; border-radius: 10px; overflow: hidden; margin: 15px 0; }
        .m-bar-fill { height: 100%; width: 0%; background: #fff; box-shadow: 0 0 15px #fff; }

        /* Admin UI */
        #admin-panel { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: #000; z-index: 9999; padding: 20px; overflow-y: auto; }
        .adm-card { background: #161b22; border-left: 4px solid var(--accent); padding: 15px; border-radius: 15px; margin-bottom: 15px; }

        /* Nav & Screens */
        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: rgba(13, 17, 23, 0.98); display: flex; justify-content: space-around; padding: 20px 0; border-top: 1px solid #30363d; z-index: 1000; }
        .nav-item { color: var(--dim); font-size: 10px; text-align: center; font-weight: 800; }
        .nav-item.active { color: var(--accent); }
        .screen { display: none; padding-top: 10px; }
        .active-screen { display: block; animation: fadeIn 0.4s ease; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body>

    <div id="global-ticker">
        <div class="ticker-wrap">🌐 NOVA OMNI: MARKET STATUS OPTIMAL • BTC $64,210 • TOTAL ASSETS $14,502,192 • LIVE PAYOUTS ACTIVE • 🌐</div>
    </div>

    <div id="auth-screen">
        <div class="logo-box">
            <h1 id="tap-logo" onclick="handleTaps()">NOVA<span style="color:var(--accent)">.</span></h1>
            <p style="color:var(--dim); font-size:9px; letter-spacing:4px; font-weight:800; margin-top:10px;">WORLDWIDE ASSET MANAGEMENT</p>
        </div>
        <div class="glass">
            <input type="text" id="u-id" placeholder="Identity Name">
            <input type="password" id="u-pass" placeholder="Security Passkey">
            <input type="text" id="secret-key" style="display:none; border:1px solid var(--gold);" placeholder="Master Admin Key">
            <button class="btn-prime" onclick="processLogin()">Secure Access</button>
        </div>
        <p style="color:var(--dim); font-size:9px; margin-top:30px;"><i class="fa-solid fa-lock"></i> AES-256 BIT ENCRYPTED TERMINAL</p>
    </div>

    <div id="admin-panel">
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:30px;">
            <h2 style="color:var(--gold); margin:0;"><i class="fa-solid fa-crown"></i> MASTER CONTROL</h2>
            <button onclick="location.reload()" style="background:var(--danger); border:none; padding:10px 20px; border-radius:10px; color:white; font-weight:800;">EXIT</button>
        </div>
        <div id="admin-tx-list"></div>
    </div>

    <div id="app-main" style="display:none;">
        <header style="padding:20px; display:flex; justify-content:space-between; align-items:center;">
            <div style="display:flex; align-items:center; gap:12px;">
                <div style="background:var(--accent); width:38px; height:38px; border-radius:12px; display:flex; align-items:center; justify-content:center; color:#000; font-weight:900;">N</div>
                <div style="font-size:12px;"><span id="u-name">...</span><br><span style="color:var(--accent); font-size:9px; font-weight:900;">VERIFIED ELITE</span></div>
            </div>
            <i class="fa-solid fa-power-off" onclick="logout()" style="color:var(--danger);"></i>
        </header>

        <div id="home" class="screen active-screen">
            <div class="mining-hero">
                <button class="reward-btn" onclick="claimDaily()"><i class="fa-solid fa-gift"></i> BONUS</button>
                <small style="font-weight:900; opacity:0.9;">INSTITUTIONAL BALANCE</small>
                <h1 id="mainBal" style="font-size:3.5rem; margin:10px 0; letter-spacing:-3px;">$0.00</h1>
                <div class="m-bar-bg"><div id="p-bar" class="m-bar-fill"></div></div>
                <div style="display:flex; justify-content:space-between; font-size:11px; font-weight:900;">
                    <span>DAILY: <b id="d-prof">+$0.00</b></span>
                    <span>TOTAL: <b id="t-prof">$0.00</b></span>
                </div>
            </div>
            <div id="node-grid" style="display:grid; grid-template-columns:1fr 1fr; gap:15px; padding:15px;"></div>
        </div>

        <div id="vault" class="screen">
            <div class="glass" style="margin:15px auto;">
                <h3 style="margin:0 0 15px 0;">Deposit Assets</h3>
                <p style="font-size:10px; color:var(--accent);">USDT Wallet (TRC20):<br><b>0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2</b></p>
                <input type="number" id="depAmt" placeholder="Amount ($)">
                <input type="text" id="tid" placeholder="Transaction Hash">
                <button class="btn-prime" onclick="submitDep()">Submit to Admin</button>
            </div>
        </div>

        <div id="support" class="screen">
            <div class="glass" style="margin:15px auto; text-align:center;">
                <i class="fa-solid fa-headset" style="font-size:3rem; color:var(--accent); margin-bottom:15px;"></i>
                <h2>Support Portal</h2>
                <p style="color:var(--dim); font-size:12px;">Contact global agents 24/7.</p>
                <button class="btn-prime" style="background:#0088cc; color:#fff; margin-top:10px;" onclick="window.open('https://t.me/your_telegram')">Telegram Support</button>
            </div>
        </div>
    </div>

    <nav class="bottom-nav" id="nav-bar" style="display:none;">
        <div class="nav-item active" onclick="nav('home', this)"><i class="fa-solid fa-house-user"></i><br>HOME</div>
        <div class="nav-item" onclick="nav('vault', this)"><i class="fa-solid fa-vault"></i><br>VAULT</div>
        <div class="nav-item" onclick="nav('support', this)"><i class="fa-solid fa-headset"></i><br>SUPPORT</div>
    </nav>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        let user = null; let bal = 0; let taps = 0;

        // Secret Admin Access
        window.handleTaps = () => {
            taps++; if(taps >= 5) { document.getElementById('secret-key').style.display = 'block'; document.getElementById('tap-logo').style.color = 'var(--gold)'; }
        };

        window.processLogin = async () => {
            const u = document.getElementById('u-id').value.toLowerCase().trim();
            const p = document.getElementById('u-pass').value;
            const sk = document.getElementById('secret-key').value;

            if(sk === "nov786") { document.getElementById('admin-panel').style.display = 'block'; loadAdmin(); return; }
            if(!u || !p) return alert("Credentials Missing");

            const ref = doc(db, "users", u);
            const snap = await getDoc(ref);
            if(snap.exists()){
                if(snap.data().password === p) { sessionStorage.setItem("nova_user", u); boot(); }
                else alert("Invalid Passkey");
            } else {
                await setDoc(ref, { password: p, balance: 0, daily: 0, total: 0, lastReward: 0 });
                sessionStorage.setItem("nova_user", u); boot();
            }
        };

        function boot() {
            user = sessionStorage.getItem("nova_user");
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app-main').style.display = 'block';
            document.getElementById('nav-bar').style.display = 'flex';
            document.getElementById('u-name').innerText = user.toUpperCase();

            onSnapshot(doc(db, "users", user), (d) => {
                const data = d.data(); bal = data.balance;
                document.getElementById('mainBal').innerText = "$" + bal.toLocaleString();
                document.getElementById('d-prof').innerText = "+$" + (data.daily || 0);
                document.getElementById('t-prof').innerText = "$" + (data.total || 0);
            });
            renderNodes(); startMiningEffect();
        }

        window.claimDaily = async () => {
            const ref = doc(db, "users", user); const snap = await getDoc(ref);
            const now = Date.now();
            if(now - (snap.data().lastReward || 0) < 86400000) return alert("Already claimed today!");
            await updateDoc(ref, { balance: bal + 0.50, lastReward: now });
            alert("Daily Bonus $0.50 added!");
        };

        function loadAdmin() {
            onSnapshot(collection(db, "pending_tx"), (snap) => {
                const list = document.getElementById('admin-tx-list'); list.innerHTML = "";
                snap.forEach(d => {
                    const tx = d.data(); if(tx.status === "Pending") {
                        list.innerHTML += `<div class="adm-card">
                            <p><b>ENTITY:</b> ${tx.user} | <b>VALUE:</b> $${tx.amount}</p>
                            <button onclick="approveDep('${d.id}','${tx.user}',${tx.amount})" style="background:var(--accent); border:none; padding:10px; width:100%; border-radius:10px; font-weight:900;">APPROVE ASSETS</button>
                        </div>`;
                    }
                });
            });
        }

        window.approveDep = async (id, u, amt) => {
            const uRef = doc(db, "users", u); const uSnap = await getDoc(uRef);
            await updateDoc(uRef, { balance: (uSnap.data().balance || 0) + amt });
            await updateDoc(doc(db, "pending_tx", id), { status: "Approved" });
            alert("Verified for " + u);
        };

        function renderNodes() {
            const container = document.getElementById('node-grid'); container.innerHTML = "";
            const prices = [30, 100, 500, 1000, 5000, 10000, 20000, 30000];
            prices.forEach((p, i) => {
                container.innerHTML += `<div style="background:var(--card); border:1px solid #333; padding:20px; border-radius:30px; text-align:center;">
                    <small style="color:var(--accent); font-weight:900;">NODE V.${i+1}</small>
                    <h2 style="margin:5px 0;">$${p.toLocaleString()}</h2>
                    <button style="background:rgba(0,255,136,0.1); color:var(--accent); border:none; padding:10px; width:100%; border-radius:12px; font-weight:900; font-size:10px;">ACTIVATE</button>
                </div>`;
            });
        }

        window.submitDep = async () => {
            const amt = document.getElementById('depAmt').value;
            const t = document.getElementById('tid').value;
            if(!amt || !t) return alert("Enter all details");
            await addDoc(collection(db, "pending_tx"), { user, amount: parseFloat(amt), tid: t, status: "Pending", time: Date.now() });
            alert("Assets sent for verification!");
        };

        window.nav = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
            document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('active'));
            el.classList.add('active');
        };

        window.logout = () => { sessionStorage.clear(); location.reload(); };
        function startMiningEffect() { let p = 0; setInterval(() => { p = (p + 0.4) % 100; document.getElementById('p-bar').style.width = p + "%"; }, 100); }
        if(sessionStorage.getItem("nova_user")) boot();
    </script>
</body>
</html>
