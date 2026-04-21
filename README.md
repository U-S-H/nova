<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA OMNI | Worldwide Institutional Terminal</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --bg: #010409; --card: #0d1117; --accent: #00ff88; --text: #f0f6fc; --dim: #8b949e; --gold: #f2cc60; --danger: #ff4d4d; }
        body { margin: 0; font-family: 'Plus Jakarta Sans', sans-serif; background: var(--bg); color: var(--text); overflow-x: hidden; }
        
        /* 1. Elite Login Experience */
        #auth-screen { min-height: 100vh; display: flex; flex-direction: column; align-items: center; justify-content: center; padding: 20px; background: radial-gradient(circle at top right, #0a2e1f, #010409); }
        .trusted-badge { font-size: 10px; color: var(--accent); border: 1px solid var(--accent); padding: 4px 12px; border-radius: 50px; text-transform: uppercase; letter-spacing: 2px; margin-bottom: 15px; background: rgba(0,255,136,0.05); }
        .welcome-msg { text-align: center; margin-bottom: 30px; }
        .welcome-msg h1 { font-size: 3.5rem; margin: 0; letter-spacing: -3px; cursor: pointer; user-select: none; }
        
        /* Hidden Admin Field */
        #secret-key-field { display: none; margin-bottom: 15px; border-color: var(--gold); }

        .glass { background: var(--card); border: 1px solid rgba(255,255,255,0.05); border-radius: 35px; padding: 30px; width: 100%; max-width: 420px; box-shadow: 0 20px 50px rgba(0,0,0,0.5); }
        input { width: 100%; padding: 18px; background: #161b22; border: 1px solid #30363d; color: #fff; border-radius: 20px; margin-bottom: 15px; outline: none; font-size: 15px; transition: 0.3s; }
        input:focus { border-color: var(--accent); box-shadow: 0 0 15px rgba(0,255,136,0.1); }
        .btn-prime { width: 100%; padding: 18px; background: var(--accent); color: #000; border: none; border-radius: 20px; font-weight: 800; cursor: pointer; text-transform: uppercase; letter-spacing: 1px; }

        /* 2. Professional Dashboard */
        .app-header { padding: 15px 25px; background: rgba(1,4,9,0.9); backdrop-filter: blur(15px); display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #30363d; position: sticky; top: 0; z-index: 100; }
        .status-dot { width: 8px; height: 8px; background: var(--accent); border-radius: 50%; box-shadow: 0 0 8px var(--accent); display: inline-block; margin-right: 5px; }

        /* 3. Unlimited Features Styling */
        .feature-chip { display: inline-block; background: #161b22; padding: 6px 12px; border-radius: 10px; font-size: 10px; margin: 4px; color: var(--dim); border: 1px solid #30363d; }
        .mining-card { background: linear-gradient(135deg, #00ff88, #008f4c); border-radius: 35px; padding: 25px; margin: 15px; color: #000; }
        
        /* 4. Secret Admin Panel UI */
        #admin-panel { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: #000; z-index: 2000; overflow-y: auto; padding: 20px; }
        .adm-row { background: #161b22; padding: 15px; border-radius: 20px; margin-bottom: 12px; border: 1px solid #30363d; }

        /* Navigation */
        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: #0d1117; display: flex; justify-content: space-around; padding: 20px 0; border-top: 1px solid #30363d; }
        .nav-link { color: var(--dim); font-size: 12px; text-align: center; }
        .nav-link.active { color: var(--accent); }
    </style>
</head>
<body>

    <div id="auth-screen">
        <div class="trusted-badge"><i class="fa-solid fa-shield-check"></i> Certified Global Provider</div>
        <div class="welcome-msg">
            <h1 id="logo-trigger" onclick="handleTap()">NOVA<span style="color:var(--accent)">.</span></h1>
            <p style="color:var(--dim); font-size: 11px; letter-spacing: 2px;">WELCOME TO THE FUTURE OF CAPITAL</p>
        </div>

        <div class="glass">
            <div id="login-fields">
                <input type="text" id="username" placeholder="Asset Management ID">
                <input type="password" id="password" placeholder="Secure Passkey">
                <input type="text" id="secret-key-field" placeholder="System Master Key">
                
                <button class="btn-prime" onclick="processLogin()">Initialize Access</button>
                <div style="margin-top:25px; text-align:center; font-size:12px; color:var(--dim);">
                    <i class="fa-solid fa-lock"></i> 256-bit AES Secure Terminal
                </div>
            </div>
        </div>
        <p style="color:var(--dim); font-size:10px; margin-top:40px;">© 2026 NOVA OMNI HOLDINGS LLC. ALL RIGHTS RESERVED.</p>
    </div>

    <div id="admin-panel">
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:30px;">
            <h2 style="color:var(--gold); margin:0;"><i class="fa-solid fa-gears"></i> SYSTEM MASTER</h2>
            <button onclick="location.reload()" style="background:var(--danger); border:none; padding:10px 20px; border-radius:10px; color:white; font-weight:800;">EXIT</button>
        </div>
        <div id="adm-list"></div>
    </div>

    <div id="app-main" style="display:none;">
        <header class="app-header">
            <div style="display:flex; align-items:center; gap:10px;">
                <div style="background:var(--accent); width:35px; height:35px; border-radius:10px; display:flex; align-items:center; justify-content:center; color:#000; font-weight:900;">N</div>
                <div style="font-size:13px;"><span id="userDisp">...</span><br><small style="color:var(--accent); font-weight:800;">INSTITUTIONAL</small></div>
            </div>
            <div style="font-size:12px; color:var(--dim);"><span class="status-dot"></span> SECURE</div>
        </header>

        <div class="mining-card">
            <p style="font-weight:800; opacity:0.8; margin:0;">GLOBAL PORTFOLIO VALUE</p>
            <h1 id="mainBal" style="font-size:3.5rem; margin:10px 0; letter-spacing:-2px;">$0.00</h1>
            <div style="background:rgba(0,0,0,0.2); height:6px; border-radius:10px; overflow:hidden;">
                <div id="mBar" style="height:100%; width:0%; background:#fff; transition:0.1s linear;"></div>
            </div>
            <div style="display:flex; justify-content:space-between; margin-top:15px; font-size:11px;">
                <span>ACTIVE NODES: <b>14</b></span>
                <span>SYSTEM APY: <b>12.5%</b></span>
            </div>
        </div>

        <div style="padding:0 15px;">
            <span class="feature-chip">Auto-Harvesting</span>
            <span class="feature-chip">Instant Extraction</span>
            <span class="feature-chip">VIP Insurance</span>
            <span class="feature-chip">24/7 Desk</span>
        </div>

        <div id="nodeGrid" style="display:grid; grid-template-columns:1fr 1fr; gap:15px; padding:15px;"></div>
    </div>

    <nav class="bottom-nav" id="nav-bar" style="display:none;">
        <div class="nav-link active"><i class="fa-solid fa-house-user" style="font-size:1.5rem"></i><br>Dashboard</div>
        <div class="nav-link"><i class="fa-solid fa-chart-line" style="font-size:1.5rem"></i><br>Market</div>
        <div class="nav-link"><i class="fa-solid fa-wallet" style="font-size:1.5rem"></i><br>Vault</div>
        <div class="nav-link"><i class="fa-solid fa-headset" style="font-size:1.5rem"></i><br>Support</div>
    </nav>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo",
            authDomain: "home-94d45.firebaseapp.com",
            projectId: "home-94d45",
            appId: "1:964390949419:web:589840cb91b7e42ecd506e"
        };
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        let tapCount = 0; let user = null; let bal = 0;

        // 1. Tap Tap Secret Logic
        window.handleTap = () => {
            tapCount++;
            if(tapCount >= 5) {
                document.getElementById('secret-key-field').style.display = 'block';
                document.getElementById('logo-trigger').style.color = '#f2cc60';
            }
        };

        // 2. Login Logic
        window.processLogin = async () => {
            const u = document.getElementById('username').value.toLowerCase().trim();
            const p = document.getElementById('password').value;
            const sk = document.getElementById('secret-key-field').value;

            // ADMIN ACCESS
            if(sk === "nov786") {
                document.getElementById('admin-panel').style.display = 'block';
                loadAdminPanel();
                return;
            }

            if(!u || !p) return alert("System Identity Required.");
            
            const ref = doc(db, "users", u);
            const snap = await getDoc(ref);
            if(snap.exists()) {
                if(snap.data().password === p) { sessionStorage.setItem("nova_u", u); startApp(); }
                else alert("Access Denied: Invalid Key.");
            } else {
                await setDoc(ref, { password: p, balance: 0 });
                sessionStorage.setItem("nova_u", u); startApp();
            }
        };

        function startApp() {
            user = sessionStorage.getItem("nova_u");
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app-main').style.display = 'block';
            document.getElementById('nav-bar').style.display = 'flex';
            document.getElementById('userDisp').innerText = user.toUpperCase();
            
            onSnapshot(doc(db, "users", user), (d) => {
                bal = d.data().balance;
                document.getElementById('mainBal').innerText = "$" + bal.toLocaleString();
            });

            renderNodes();
            let p = 0; setInterval(() => { p = (p + 0.5) % 100; document.getElementById('mBar').style.width = p + "%"; }, 100);
        }

        // 3. Unlimited Admin Control
        function loadAdminPanel() {
            onSnapshot(collection(db, "pending_tx"), (snap) => {
                const list = document.getElementById('adm-list'); list.innerHTML = "";
                snap.forEach(d => {
                    const tx = d.data();
                    if(tx.status === "Pending") {
                        list.innerHTML += `<div class="adm-row">
                            <p><b>ENTITY:</b> ${tx.user} | <b>VALUE:</b> $${tx.amount}</p>
                            <button onclick="approveTx('${d.id}','${tx.user}',${tx.amount})" style="background:var(--accent); border:none; padding:10px; width:100%; border-radius:10px; font-weight:800;">CONFIRM ASSETS</button>
                        </div>`;
                    }
                });
            });
        }

        window.approveTx = async (id, u, amt) => {
            const uRef = doc(db, "users", u);
            const uSnap = await getDoc(uRef);
            await updateDoc(uRef, { balance: (uSnap.data().balance || 0) + amt });
            await updateDoc(doc(db, "pending_tx", id), { status: "Approved" });
            alert("Master Protocol: Assets Verified.");
        };

        function renderNodes() {
            const grid = document.getElementById('nodeGrid'); grid.innerHTML = "";
            const prices = [30, 100, 500, 1000, 5000, 10000, 20000, 30000];
            prices.forEach((p, i) => {
                grid.innerHTML += `<div style="background:var(--card); border:1px solid #30363d; padding:20px; border-radius:25px; text-align:center;">
                    <div style="font-size:9px; color:var(--accent); font-weight:800; margin-bottom:5px;">SERIES V.${i+1}</div>
                    <h2 style="margin:0">$${p.toLocaleString()}</h2>
                    <button style="background:rgba(0,255,136,0.1); color:var(--accent); border:none; padding:8px 15px; border-radius:10px; font-size:10px; font-weight:900; margin-top:10px;">DEPLOY NODE</button>
                </div>`;
            });
        }

        if(sessionStorage.getItem("nova_u")) startApp();
    </script>
</body>
</html>
