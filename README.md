<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Icon-Based Pro Terminal</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --primary: #020617; --accent: #10B981; --card-bg: #0F172A; --text-main: #F8FAFC; --text-dim: #94A3B8; --gold: #F59E0B; --danger: #EF4444; }
        body { margin: 0; font-family: 'Poppins', sans-serif; background: var(--primary); color: var(--text-main); -webkit-tap-highlight-color: transparent; padding-bottom: 80px; }
        
        .glass-card { background: var(--card-bg); border-radius: 24px; padding: 20px; margin: 15px; border: 1px solid #1E293B; box-shadow: 0 10px 30px rgba(0,0,0,0.5); }
        
        /* App Bar Icons */
        .app-header { padding: 20px; background: var(--card-bg); border-bottom: 1px solid #1E293B; display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 100; }
        .icon-circle { background: rgba(16, 185, 129, 0.1); color: var(--accent); width: 40px; height: 40px; border-radius: 50%; display: flex; align-items: center; justify-content: center; }

        /* Icon Grid for Balance */
        .bal-display { background: linear-gradient(135deg, #059669, #064e3b); border-radius: 24px; padding: 25px; margin: 15px; position: relative; overflow: hidden; }
        .bal-display i { position: absolute; right: -10px; bottom: -10px; font-size: 8rem; opacity: 0.1; color: white; }

        /* Stats with Icons */
        .stat-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; padding: 0 15px; }
        .icon-stat { background: #1E293B; padding: 15px; border-radius: 18px; display: flex; align-items: center; gap: 10px; border: 1px solid rgba(255,255,255,0.05); }
        .icon-stat i { font-size: 1.2rem; color: var(--accent); }

        /* Modern Navigation Icons */
        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: rgba(15, 23, 42, 0.95); backdrop-filter: blur(10px); display: flex; justify-content: space-around; padding: 12px 0; border-top: 1px solid #1E293B; z-index: 1000; }
        .nav-item { text-align: center; color: var(--text-dim); font-size: 10px; cursor: pointer; transition: 0.3s; flex: 1; }
        .nav-item i { font-size: 1.4rem; margin-bottom: 4px; display: block; }
        .nav-item.active { color: var(--accent); }

        /* Input Icons Wrapper */
        .input-group { position: relative; margin-bottom: 15px; }
        .input-group i { position: absolute; left: 15px; top: 16px; color: var(--text-dim); }
        input, select { width: 100%; padding: 14px 14px 14px 45px; background: #020617; border: 1px solid #334155; color: white; border-radius: 14px; font-size: 14px; box-sizing: border-box; transition: 0.3s; }
        input:focus { border-color: var(--accent); outline: none; }

        .btn-action { width: 100%; padding: 15px; background: var(--accent); color: white; border: none; border-radius: 14px; font-weight: 800; cursor: pointer; display: flex; align-items: center; justify-content: center; gap: 10px; box-shadow: 0 5px 15px rgba(16, 185, 129, 0.3); }

        /* Pages */
        .page { display: none; padding-top: 10px; animation: fadeIn 0.4s ease; }
        .page.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        #auth-screen { min-height: 100vh; display: flex; flex-direction: column; align-items: center; justify-content: center; background: radial-gradient(circle at top, #1E293B, #020617); padding: 20px; }
    </style>
</head>
<body>

    <div id="auth-screen">
        <i class="fa-solid fa-shield-halved" style="font-size: 4rem; color: var(--accent); margin-bottom: 20px;"></i>
        <h1 style="color:var(--accent); font-size:2.5rem; margin:0; letter-spacing:6px;">NOVA</h1>
        <p style="color:var(--text-dim); font-size:10px; margin-bottom:40px;">SECURE ACCESS PROTOCOL</p>
        
        <div class="glass-card" style="width: 100%; max-width: 350px;">
            <div class="input-group">
                <i class="fa-solid fa-user"></i>
                <input type="text" id="username" placeholder="Username">
            </div>
            <div class="input-group">
                <i class="fa-solid fa-lock"></i>
                <input type="password" id="password" placeholder="Master Key">
            </div>
            <button class="btn-action" onclick="handleAuth()" id="authBtn">
                <i class="fa-solid fa-paper-plane"></i> Establish Link
            </button>
            <p onclick="toggleAuth()" id="toggleTxt" style="text-align:center; font-size:11px; color:var(--accent); margin-top:20px; cursor:pointer;">
                <i class="fa-solid fa-user-plus"></i> New Membership
            </p>
        </div>
    </div>

    <div id="app-main" style="display:none;">
        <div class="app-header">
            <div style="display:flex; align-items:center; gap:10px;">
                <div class="icon-circle"><i class="fa-solid fa-fingerprint"></i></div>
                <div><small style="color:var(--text-dim); display:block; font-size:9px;">ENCRYPTED SESSION</small><b id="userDisp">User</b></div>
            </div>
            <i class="fa-solid fa-right-from-bracket" onclick="location.reload()" style="color:var(--text-dim); font-size:1.2rem;"></i>
        </div>

        <div id="home" class="page active">
            <div class="bal-display">
                <i class="fa-solid fa-wallet"></i>
                <small style="color:rgba(255,255,255,0.7); display:block;">Assets Valuation</small>
                <h1 style="margin:5px 0; font-size:2.5rem;" id="mainBal">$0.00</h1>
                <div style="display:flex; gap:10px; margin-top:20px;">
                    <button onclick="navTo('wallet')" style="background:rgba(255,255,255,0.2); color:white; border:none; padding:8px 15px; border-radius:10px; font-size:11px; display:flex; align-items:center; gap:5px;"><i class="fa-solid fa-plus-circle"></i> DEPOSIT</button>
                    <button onclick="navTo('wallet')" style="background:white; color:var(--accent); border:none; padding:8px 15px; border-radius:10px; font-size:11px; display:flex; align-items:center; gap:5px;"><i class="fa-solid fa-arrow-up-from-bracket"></i> WITHDRAW</button>
                </div>
            </div>

            <div class="stat-grid">
                <div class="icon-stat"><i class="fa-solid fa-chart-line"></i><div><small style="color:var(--text-dim); display:block; font-size:9px;">DAILY ROI</small><b>$0.00</b></div></div>
                <div class="icon-stat"><i class="fa-solid fa-coins"></i><div><small style="color:var(--text-dim); display:block; font-size:9px;">TOTAL EARN</small><b>$0.00</b></div></div>
            </div>

            <h4 style="padding:15px 0 0 20px;"><i class="fa-solid fa-server" style="color:var(--accent);"></i> Activation Marketplace</h4>
            <div id="nodeGrid" style="padding:15px; display:grid; gap:15px;"></div>
        </div>

        <div id="wallet" class="page">
            <div class="glass-card">
                <h3 style="color:var(--accent); margin-top:0;"><i class="fa-solid fa-circle-down"></i> Funding Gateway</h3>
                <div style="display:flex; gap:10px; margin-bottom:15px;">
                    <div onclick="copyWal('0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37')" style="flex:1; background:#020617; padding:12px; border-radius:12px; text-align:center; border:1px solid #1E293B;"><i class="fa-solid fa-shield-heart" style="color:#3375BB;"></i><br><small>TRUST</small></div>
                    <div onclick="copyWal('0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2')" style="flex:1; background:#020617; padding:12px; border-radius:12px; text-align:center; border:1px solid #1E293B;"><i class="fa-solid fa-mask" style="color:#F6851B;"></i><br><small>METAMASK</small></div>
                </div>
                <div class="input-group"><i class="fa-solid fa-network-wired"></i><select id="depNet"><option>USDT (BEP20)</option><option>TRX (TRC20)</option></select></div>
                <div class="input-group"><i class="fa-solid fa-hashtag"></i><input type="text" id="tid" placeholder="Transaction TID"></div>
                <div class="input-group"><i class="fa-solid fa-image"></i><input type="file" id="proof" accept="image/*"></div>
                <button class="btn-action" onclick="submitDep()"><i class="fa-solid fa-circle-check"></i> Submit Deposit</button>
            </div>

            <div class="glass-card">
                <h3 style="color:var(--danger); margin-top:0;"><i class="fa-solid fa-circle-up"></i> Exit Liquidity</h3>
                <div class="input-group"><i class="fa-solid fa-building-columns"></i><select id="witNet"><option>Binance Pay</option><option>OKX</option><option>Trust Wallet</option></select></div>
                <div class="input-group"><i class="fa-solid fa-money-bill-wave"></i><input type="number" id="witAmt" placeholder="Amount (Min $10)"></div>
                <div class="input-group"><i class="fa-solid fa-address-card"></i><input type="text" id="witAcc" placeholder="Wallet Address/ID"></div>
                <button class="btn-action" style="background:var(--danger);" onclick="submitWit()"><i class="fa-solid fa-bolt"></i> Confirm Withdrawal</button>
            </div>
        </div>

        <div id="logs" class="page">
            <h3 style="padding:15px;"><i class="fa-solid fa-list-check"></i> Protocol Logs</h3>
            <div id="histContent" style="padding:0 15px;"></div>
        </div>

        <div class="bottom-nav">
            <div class="nav-item active" onclick="navTo('home', this)"><i class="fa-solid fa-house-chimney"></i>Home</div>
            <div class="nav-item" onclick="navTo('wallet', this)"><i class="fa-solid fa-wallet"></i>Wallet</div>
            <div class="nav-item" onclick="navTo('logs', this)"><i class="fa-solid fa-clock-rotate-left"></i>Logs</div>
            <div class="nav-item" onclick="navTo('info', this)"><i class="fa-solid fa-circle-info"></i>Info</div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, collection, getDocs, query, where } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        let isLogin = true;
        window.toggleAuth = () => {
            isLogin = !isLogin;
            document.getElementById('authBtn').innerHTML = isLogin ? '<i class="fa-solid fa-paper-plane"></i> Establish Link' : '<i class="fa-solid fa-user-plus"></i> Join Protocol';
            document.getElementById('toggleTxt').innerHTML = isLogin ? '<i class="fa-solid fa-user-plus"></i> New Membership' : '<i class="fa-solid fa-right-to-bracket"></i> Login Portal';
        };

        window.handleAuth = async () => {
            const u = document.getElementById('username').value.toLowerCase().trim();
            const p = document.getElementById('password').value;
            if(!u || !p) return alert("Credentials required!");
            const snap = await getDoc(doc(db, "users", u));
            if(isLogin) {
                if(snap.exists() && snap.data().password === p) { sessionStorage.setItem("nova_user", u); location.reload(); }
                else alert("Encryption mismatch!");
            } else {
                await setDoc(doc(db, "users", u), { password: p, balance: 0 });
                alert("ID Registered! Accessing Portal..."); toggleAuth();
            }
        };

        window.navTo = (id, el) => {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            if(el) {
                document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('active'));
                el.classList.add('active');
            }
        };

        window.submitDep = async () => {
            const tid = document.getElementById('tid').value;
            const file = document.getElementById('proof').files[0];
            if(!tid || !file) return alert("Data incomplete!");
            const reader = new FileReader();
            reader.onloadend = async () => {
                await setDoc(doc(db, "transactions", "tx_"+Date.now()), {
                    user: sessionStorage.getItem("nova_user"), type: "Deposit", tid: tid, 
                    proof: reader.result, status: "Pending", date: new Date().toLocaleString()
                });
                alert("Audit in progress!"); location.reload();
            };
            reader.readAsDataURL(file);
        };

        window.submitWit = async () => {
            const amt = document.getElementById('witAmt').value;
            const acc = document.getElementById('witAcc').value;
            if(amt < 10) return alert("Min $10 required!");
            await setDoc(doc(db, "transactions", "wit_"+Date.now()), {
                user: sessionStorage.getItem("nova_user"), type: "Withdrawal", amount: amt,
                account: acc, status: "Pending", date: new Date().toLocaleString()
            });
            alert("Request sent to network!"); location.reload();
        };

        function loadAppData() {
            const user = sessionStorage.getItem("nova_user");
            document.getElementById('auth-screen').style.display='none';
            document.getElementById('app-main').style.display='block';
            document.getElementById('userDisp').innerText = user;

            // Render Nodes with Icons
            const grid = document.getElementById('nodeGrid');
            const nodes = [{n:"GENESIS-V1", c:100, r:1.8, i:"fa-microchip"}, {n:"QUANTUM-V2", c:500, r:2.5, i:"fa-atom"}, {n:"CORE-MAX", c:1000, r:3.2, i:"fa-brain"}];
            nodes.forEach(x => {
                grid.innerHTML += `<div class="icon-stat" style="justify-content:space-between; padding:20px;">
                    <div style="display:flex; gap:15px; align-items:center;"><i class="fa-solid ${x.i}" style="font-size:1.5rem;"></i><div><b>${x.n}</b><br><small>$${x.c} Cost</small></div></div>
                    <button onclick="navTo('wallet')" style="background:var(--accent); color:white; border:none; padding:8px 15px; border-radius:10px; font-size:10px;">ACTIVATE</button>
                </div>`;
            });
        }

        window.copyWal = (a) => { navigator.clipboard.writeText(a); alert("Copied!"); };
        if(sessionStorage.getItem("nova_user")) loadAppData();
    </script>
</body>
</html>
