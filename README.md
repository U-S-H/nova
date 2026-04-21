<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA TITAN | Institutional Terminal</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --bg: #020202; --card: #0d0d0d; --accent: #00ff88; --gold: #f2cc60; --text: #ffffff; --dim: #8b949e; --danger: #ff4d4d; }
        body { margin: 0; font-family: 'Plus Jakarta Sans', sans-serif; background: var(--bg); color: var(--text); overflow-x: hidden; padding-bottom: 100px; }
        
        /* Layout */
        .glass { background: var(--card); border: 1px solid #222; border-radius: 30px; padding: 25px; box-shadow: 0 20px 40px rgba(0,0,0,0.8); }
        input { width: 100%; padding: 18px; background: #161b22; border: 1px solid #30363d; color: #fff; border-radius: 20px; margin-bottom: 12px; outline: none; box-sizing: border-box; }
        .btn-prime { width: 100%; padding: 18px; background: var(--accent); color: #000; border: none; border-radius: 20px; font-weight: 800; cursor: pointer; text-transform: uppercase; transition: 0.3s; }
        .btn-prime:active { transform: scale(0.95); }

        /* Nodes Grid */
        .node-card { background: var(--card); border: 1px solid #222; border-radius: 25px; padding: 15px; position: relative; }
        .node-img { width: 100%; height: 120px; border-radius: 20px; object-fit: cover; opacity: 0.7; background: #161b22; }
        .timer-badge { position: absolute; top: 10px; right: 10px; background: rgba(0,0,0,0.8); color: var(--gold); padding: 5px 12px; border-radius: 50px; font-size: 10px; font-weight: 800; border: 1px solid var(--gold); }

        /* Payout Toast */
        #payout-toast { position: fixed; top: 20px; left: 50%; transform: translateX(-50%); background: #111; border: 1px solid var(--accent); padding: 12px 25px; border-radius: 50px; font-size: 10px; z-index: 9999; display: none; box-shadow: 0 0 30px rgba(0,255,136,0.2); }

        /* Navigation */
        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: rgba(2,2,2,0.95); display: flex; justify-content: space-around; padding: 20px 0; border-top: 1px solid #222; z-index: 2000; backdrop-filter: blur(10px); }
        .nav-item { color: var(--dim); text-align: center; font-size: 10px; font-weight: 800; }
        .nav-item.active { color: var(--accent); }
        .screen { display: none; padding: 20px; }
        .active-screen { display: block; animation: fadeInUp 0.4s ease; }
        @keyframes fadeInUp { from { opacity:0; transform: translateY(20px); } to { opacity:1; transform: translateY(0); } }

        #admin-panel { display: none; position: fixed; inset: 0; background: #000; z-index: 10000; padding: 25px; overflow-y: auto; }
    </style>
</head>
<body>

    <div id="payout-toast"></div>

    <div id="auth-screen" style="height:90vh; display:flex; flex-direction:column; align-items:center; justify-content:center; padding:30px;">
        <h1 id="logo" onclick="adminTap()" style="font-size:4.5rem; margin:0; letter-spacing:-5px;">NOVA<span style="color:var(--accent)">.</span></h1>
        <p style="color:var(--dim); font-size:10px; letter-spacing:6px; margin-bottom:40px;">QUANTUM ASSET MANAGEMENT</p>
        <div class="glass" style="width:100%; max-width:400px;">
            <input type="text" id="u-id" placeholder="User Identifier">
            <input type="password" id="u-pass" placeholder="Security Passkey">
            <input type="text" id="adm-key" style="display:none; border-color:var(--gold);" placeholder="Enter Master Key">
            <button class="btn-prime" onclick="processAuth()">Access Terminal</button>
        </div>
    </div>

    <div id="admin-panel">
        <div style="display:flex; justify-content:space-between; align-items:center;">
            <h2 style="color:var(--gold);">SYSTEM OVERRIDE</h2>
            <button onclick="location.reload()" style="background:var(--danger); border:none; padding:10px 20px; border-radius:10px; color:#fff;">EXIT</button>
        </div>
        <div id="adm-list" style="margin-top:40px;"></div>
    </div>

    <div id="app-main" style="display:none;">
        <header style="padding:20px; display:flex; justify-content:space-between; align-items:center;">
            <div>
                <span id="user-display" style="font-weight:900; font-size:22px;">...</span><br>
                <small style="color:var(--accent); font-weight:800;"><i class="fa-solid fa-circle-check"></i> GLOBAL VERIFIED</small>
            </div>
            <div style="background:var(--card); width:45px; height:45px; border-radius:15px; display:flex; align-items:center; justify-content:center; border:1px solid #333;" onclick="nav('wallet')">
                <i class="fa-solid fa-wallet" style="color:var(--gold);"></i>
            </div>
        </header>

        <div id="home" class="screen active-screen">
            <div class="glass" style="background: linear-gradient(135deg, #111, #000); margin-bottom:25px; border:1px solid #333;">
                <p style="color:var(--dim); font-size:11px; margin:0; letter-spacing:1px;">AVAILABLE PORTFOLIO</p>
                <h1 id="main-bal" style="font-size:3.8rem; margin:10px 0; letter-spacing:-3px;">$0.00</h1>
                <div style="display:flex; justify-content:space-between; font-size:11px; font-weight:800;">
                    <span style="color:var(--accent);">YIELD: +$<span id="d-prof">0.00</span></span>
                    <span style="color:var(--gold);">REBATE: $<span id="r-prof">0.00</span></span>
                </div>
            </div>

            <div style="display:flex; justify-content:space-between; margin-bottom:20px; align-items:center;">
                <h4 style="margin:0;">ACTIVE MINING NODES</h4>
                <div style="font-size:10px; background:rgba(0,255,136,0.1); color:var(--accent); padding:5px 12px; border-radius:50px;">LIVE ENGINE</div>
            </div>
            <div id="nodes-grid" style="display:grid; grid-template-columns:1fr 1fr; gap:15px;"></div>
        </div>

        <div id="wallet" class="screen">
            <div class="glass">
                <div style="display:flex; gap:10px; margin-bottom:25px;">
                    <button class="btn-prime" style="font-size:12px; padding:15px;" onclick="setMode('Deposit')">Deposit</button>
                    <button class="btn-prime" style="font-size:12px; padding:15px; background:#222; color:#fff;" onclick="setMode('Withdraw')">Withdraw</button>
                </div>
                <div id="finance-form">
                    <input type="number" id="f-amt" placeholder="Amount ($)">
                    <input type="text" id="f-tx" placeholder="Hash / Wallet Address">
                    <button class="btn-prime" onclick="submitFinance()">Submit Request</button>
                </div>
            </div>
            
            <h4 style="margin:30px 0 10px 0;">INVITATION SYSTEM</h4>
            <div class="glass" style="padding:20px; font-size:12px; border:1px dashed #444;">
                <p style="margin:0; color:var(--dim);">Your Personal Referral Link:</p>
                <p id="ref-link" style="color:var(--accent); font-weight:800; margin-top:10px;"></p>
            </div>
        </div>
    </div>

    <nav class="bottom-nav" id="nav-bar" style="display:none;">
        <div class="nav-item active" onclick="nav('home', this)"><i class="fa-solid fa-chart-line" style="font-size:1.5rem;"></i><br>MARKETS</div>
        <div class="nav-item" onclick="nav('wallet', this)"><i class="fa-solid fa-vault" style="font-size:1.5rem;"></i><br>WALLET</div>
        <div class="nav-item" onclick="window.open('https://t.me/your_telegram')"><i class="fa-solid fa-headset" style="font-size:1.5rem;"></i><br>SUPPORT</div>
    </nav>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        let user = null; let taps = 0; let fMode = "Deposit";
        const b64 = "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAOAAAADhCAMAAAD66L79AAAAA1BMVEWAgICQpREbAAAAR0lEQVR4nO3BAQEAAACAkP6v7ggKAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAgB8M4AAHL750VAAAAAElFTkSuQmCC";

        window.adminTap = () => { taps++; if(taps >= 6) { document.getElementById('adm-key').style.display='block'; document.getElementById('logo').style.color='var(--gold)'; } };

        window.processAuth = async () => {
            const u = document.getElementById('u-id').value.toLowerCase().trim();
            const p = document.getElementById('u-pass').value;
            if(document.getElementById('adm-key').value === "nov786") { document.getElementById('admin-panel').style.display='block'; loadAdmin(); return; }
            if(!u || !p) return alert("All fields required");

            const ref = doc(db, "users", u);
            const snap = await getDoc(ref);
            if(snap.exists()){
                if(snap.data().password === p) { startApp(u); } else alert("Wrong Passkey");
            } else {
                await setDoc(ref, { password: p, balance: 0, daily: 0, rebate: 0, total: 0 });
                startApp(u);
            }
        };

        function startApp(u) {
            user = u; sessionStorage.setItem("nova_user", u);
            document.getElementById('auth-screen').style.display='none';
            document.getElementById('app-main').style.display='block';
            document.getElementById('nav-bar').style.display='flex';
            document.getElementById('user-display').innerText = u.toUpperCase();
            document.getElementById('ref-link').innerText = "https://u-s-h.github.io/Coin/?id=" + u;
            
            onSnapshot(doc(db, "users", u), (d) => {
                const data = d.data();
                document.getElementById('main-bal').innerText = "$" + data.balance.toFixed(2);
                document.getElementById('d-prof').innerText = (data.daily || 0).toFixed(2);
                document.getElementById('r-prof').innerText = (data.rebate || 0).toFixed(2);
            });
            renderNodes();
            startToasts();
            // Visual Mining Engine
            setInterval(() => {
                const balEl = document.getElementById('main-bal');
                let val = parseFloat(balEl.innerText.replace('$',''));
                balEl.innerText = "$" + (val + 0.0001).toFixed(4);
            }, 5000);
        }

        function renderNodes() {
            const grid = document.getElementById('nodes-grid'); grid.innerHTML = "";
            const tiers = [30, 100, 300, 500, 1000, 2500, 5000, 10000, 25000, 50000];
            tiers.forEach((t, i) => {
                grid.innerHTML += `
                    <div class="node-card">
                        <div class="timer-badge">29:23:59:59</div>
                        <img src="${b64}" class="node-img">
                        <div style="font-weight:900; font-size:20px; margin-top:10px;">$${t.toLocaleString()}</div>
                        <div style="font-size:10px; color:var(--accent);">Daily Payout: $${(t*0.05).toFixed(2)}</div>
                        <button class="btn-prime" style="padding:10px; font-size:10px; margin-top:12px;">Stake Node</button>
                    </div>`;
            });
        }

        window.setMode = (m) => { fMode = m; alert("Selected Mode: " + m); };

        window.submitFinance = async () => {
            const a = document.getElementById('f-amt').value; const tx = document.getElementById('f-tx').value;
            if(!a || !tx) return alert("Missing Info");
            await addDoc(collection(db, "requests"), { user, amount: parseFloat(a), txid: tx, type: fMode, status: "Pending", time: Date.now() });
            alert(fMode + " request logged!");
        };

        function startToasts() {
            const users = ["X-Crypto", "Zaid_99", "Elite_King", "Sam_Trader", "VIP_User"];
            setInterval(() => {
                if(Math.random() > 0.7) {
                    const t = document.getElementById('payout-toast');
                    const u = users[Math.floor(Math.random()*users.length)];
                    const a = (Math.random()*1000).toFixed(2);
                    t.innerText = `🔥 LIVE: ${u} successfully withdrew $${a}`;
                    t.style.display = 'block';
                    setTimeout(() => t.style.display='none', 4000);
                }
            }, 8000);
        }

        function loadAdmin() {
            onSnapshot(collection(db, "requests"), (snap) => {
                const l = document.getElementById('adm-list'); l.innerHTML = "";
                snap.forEach(d => {
                    const r = d.data(); if(r.status === "Pending") {
                        l.innerHTML += `<div class="glass" style="margin-bottom:15px; border-left:5px solid var(--accent);">
                            <p>${r.user} | ${r.type}: $${r.amount} <br><small>${r.txid}</small></p>
                            <button onclick="approveReq('${d.id}','${r.user}',${r.amount},'${r.type}')" style="background:var(--accent); border:none; padding:12px; width:100%; border-radius:15px; font-weight:800;">CONFIRM</button>
                        </div>`;
                    }
                });
            });
        }

        window.approveReq = async (id, u, amt, t) => {
            const r = doc(db, "users", u); const s = await getDoc(r);
            let b = t === "Deposit" ? (s.data().balance + amt) : (s.data().balance - amt);
            await updateDoc(r, { balance: b });
            await updateDoc(doc(db, "requests", id), { status: "Verified" });
            alert("Action Executed!");
        };

        window.nav = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
            document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('active'));
            el.classList.add('active');
        };

        if(sessionStorage.getItem("nova_user")) startApp(sessionStorage.getItem("nova_user"));
    </script>
</body>
</html>
