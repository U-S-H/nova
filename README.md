<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA OMNI | Elite Institutional Management</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --bg: #020617; --card: #0f172a; --accent: #10B981; --text: #f8fafc; --dim: #94a3b8; --gold: #fbbf24; --danger: #ef4444; }
        body { margin: 0; font-family: 'Plus Jakarta Sans', sans-serif; background: var(--bg); color: var(--text); overflow-x: hidden; padding-bottom: 100px; }
        
        /* Premium Ticker */
        #ticker { background: #1e293b; padding: 12px; font-size: 11px; font-weight: 800; white-space: nowrap; overflow: hidden; border-bottom: 1px solid rgba(16, 185, 129, 0.2); color: var(--accent); }
        .ticker-move { display: inline-block; animation: move 25s linear infinite; }
        @keyframes move { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }

        .glass { background: var(--card); border: 1px solid rgba(255,255,255,0.05); border-radius: 32px; padding: 22px; margin: 15px; box-shadow: 0 15px 35px rgba(0,0,0,0.4); }
        .btn-prime { width: 100%; padding: 18px; background: var(--accent); color: white; border: none; border-radius: 22px; font-weight: 800; cursor: pointer; transition: 0.3s; text-transform: uppercase; letter-spacing: 1px; }
        .btn-prime:active { transform: scale(0.95); }
        
        input, select { width: 100%; padding: 16px; background: #020617; border: 1px solid #334155; color: #fff; border-radius: 20px; margin-bottom: 15px; outline: none; font-family: inherit; font-weight: 600; }

        .app-header { padding: 20px; background: rgba(15, 23, 42, 0.9); backdrop-filter: blur(20px); border-bottom: 1px solid rgba(255,255,255,0.05); display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 1000; }
        .vip-tag { font-size: 10px; background: var(--gold); color: #000; padding: 4px 12px; border-radius: 50px; font-weight: 900; box-shadow: 0 0 15px rgba(251, 191, 36, 0.3); }

        /* Modern Node Cards */
        .node-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 18px; padding: 15px; }
        .node-card { background: var(--card); border-radius: 28px; padding: 18px; border: 1px solid rgba(255,255,255,0.03); text-align: center; position: relative; overflow: hidden; transition: 0.3s; }
        .node-card:hover { border-color: var(--accent); transform: translateY(-5px); }
        .node-img { width: 100%; height: 100px; border-radius: 20px; object-fit: cover; margin-bottom: 12px; filter: brightness(0.9); }
        .yield-badge { position: absolute; top: 12px; left: 12px; background: var(--accent); color: white; font-size: 9px; padding: 4px 10px; border-radius: 10px; font-weight: 900; }

        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: rgba(15, 23, 42, 0.98); display: flex; justify-content: space-around; padding: 18px 0; border-top: 1px solid #1e293b; z-index: 1000; }
        .nav-item { text-align: center; color: var(--dim); font-size: 10px; font-weight: 800; cursor: pointer; transition: 0.3s; }
        .nav-item.active { color: var(--accent); }
        .nav-item i { font-size: 1.6rem; margin-bottom: 5px; }

        .screen { display: none; animation: slideUp 0.4s ease-out; }
        .screen.active { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body>

    <div id="ticker"><div class="ticker-move">💎 NOVA OMNI: WORLDWIDE NODE CLUSTER DEPLOYED • BTC: $64,231.50 (+2.4%) • ETH: $3,452.12 • SECURE MINING PROTOCOL ACTIVE • WITHDRAWAL STATUS: AUTO-SYNC 💎</div></div>

    <div id="auth-screen" style="min-height:100vh; display:flex; flex-direction:column; align-items:center; justify-content:center; padding:25px; background: radial-gradient(circle at top, #1e293b, #020617);">
        <h1 style="font-size:4rem; font-weight:900; letter-spacing:-4px; margin-bottom:10px;">NOVA<span style="color:var(--accent);">.</span></h1>
        <p style="color:var(--dim); font-size:12px; margin-bottom:30px; letter-spacing:2px; font-weight:600;">INSTITUTIONAL GRADE</p>
        <div class="glass" style="width:100%; max-width:400px;">
            <input type="text" id="username" placeholder="Username">
            <input type="password" id="password" placeholder="Passkey">
            <button class="btn-prime" onclick="handleAuth()">Access Dashboard</button>
            <p onclick="toggleAuth()" id="toggleTxt" style="text-align:center; font-size:12px; color:var(--dim); margin-top:25px; cursor:pointer;">Identity not found? <span style="color:var(--accent)">Initialize</span></p>
        </div>
    </div>

    <div id="app-main" style="display:none;">
        <header class="app-header">
            <div style="display:flex; align-items:center; gap:12px;">
                <div style="width:40px; height:40px; background:var(--accent); border-radius:14px; display:flex; align-items:center; justify-content:center; font-weight:900; color:white; font-size:1.2rem;">N</div>
                <div><small id="userDisp" style="font-weight:900; display:block; color:white;">USER</small><span class="vip-tag" id="vipStatus">BASIC</span></div>
            </div>
            <div style="display:flex; gap:15px; align-items:center;">
                <button onclick="claimBonus()" style="background:var(--gold); border:none; padding:6px 12px; border-radius:10px; color:black; font-weight:900; font-size:10px;">BONUS</button>
                <i class="fa-solid fa-power-off" onclick="logout()" style="color:var(--danger); font-size:1.4rem; cursor:pointer;"></i>
            </div>
        </header>

        <div id="home" class="screen active">
            <div class="glass" style="background: linear-gradient(135deg, #10B981, #064e3b); border:none; position:relative; overflow:hidden;">
                <div style="position:absolute; top:-20px; right:-20px; width:100px; height:100px; background:rgba(255,255,255,0.1); border-radius:50%;"></div>
                <p style="font-size:11px; font-weight:800; opacity:0.8; text-transform:uppercase; letter-spacing:1px; color:white;">Total Available Balance</p>
                <h1 style="font-size:3.5rem; margin:12px 0; letter-spacing:-2px; color:white;" id="mainBal">$0.00</h1>
                <div style="display:flex; justify-content:space-between; border-top:1px solid rgba(255,255,255,0.1); padding-top:18px;">
                    <div><small style="color:rgba(255,255,255,0.7)">DAILY HARVEST</small><br><b id="dayYield" style="color:white;">+$0.00</b></div>
                    <div style="text-align:right;"><small style="color:rgba(255,255,255,0.7)">LIFETIME PROFIT</small><br><b id="totProfit" style="color:white;">$0.00</b></div>
                </div>
            </div>
            
            <h4 style="margin:25px 22px 12px; color:var(--dim); font-size:12px; letter-spacing:2px; font-weight:800;">CLUSTER SELECTION</h4>
            <div id="nodeGrid" class="node-grid"></div>
        </div>

        <div id="vault" class="screen">
            <div class="glass">
                <h3 style="margin-top:0;"><i class="fa-solid fa-circle-plus" style="color:var(--accent)"></i> Deposit Assets</h3>
                <select id="depMethod" onchange="updateAddr()">
                    <option value="MetaMask">MetaMask (USDT-ERC20)</option>
                    <option value="Trust">Trust Wallet (USDT-TRC20)</option>
                </select>
                <div style="background:#020617; padding:18px; border-radius:20px; border:2px dashed #1e293b; margin-bottom:18px;">
                    <small style="color:var(--dim); font-size:10px;">OFFICIAL WALLET ADDRESS:</small>
                    <p id="walletAddr" style="font-size:11px; word-break:break-all; font-weight:900; color:var(--accent); margin:8px 0; letter-spacing:0.5px;">0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2</p>
                </div>
                <input type="number" id="depAmt" placeholder="Amount in USD">
                <input type="text" id="tid" placeholder="Transaction ID (TID)">
                <button class="btn-prime" onclick="submitDep()">Initialize Verification</button>
            </div>

            <div class="glass" style="border-top:5px solid var(--danger);">
                <h3 style="margin-top:0;"><i class="fa-solid fa-paper-plane" style="color:var(--danger)"></i> Extraction</h3>
                <input type="number" id="witAmt" placeholder="Minimum $50">
                <input type="text" id="witAddr" placeholder="Recipient Address">
                <button class="btn-prime" onclick="submitWit()" style="background:var(--danger);">Withdraw Now</button>
            </div>
        </div>

        <div id="refer" class="screen">
            <div class="glass" style="text-align:center; background:linear-gradient(to bottom, #1e1b4b, #0f172a);">
                <i class="fa-solid fa-users-rays" style="font-size:3.5rem; color:var(--gold); margin-bottom:15px;"></i>
                <h2>Affiliate Program</h2>
                <p style="font-size:12px; color:var(--dim);">Earn 10% instant commission on every cluster activated by your team.</p>
                <div style="background:rgba(0,0,0,0.3); padding:20px; border-radius:22px; margin-top:20px; display:flex; justify-content:space-between; align-items:center;">
                    <span id="refLink" style="font-weight:900; color:var(--accent);">NOVA-LINK-786</span>
                    <button onclick="copyRef()" style="background:white; color:black; border:none; padding:8px 15px; border-radius:12px; font-weight:900; font-size:11px;">COPY</button>
                </div>
            </div>
        </div>
    </div>

    <nav class="bottom-nav" id="nav-bar" style="display:none;">
        <div class="nav-item active" onclick="nav('home', this)"><i class="fa-solid fa-house-chimney-window"></i><br>HOME</div>
        <div class="nav-item" onclick="nav('vault', this)"><i class="fa-solid fa-vault"></i><br>VAULT</div>
        <div class="nav-item" onclick="nav('refer', this)"><i class="fa-solid fa-users"></i><br>TEAM</div>
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
            document.getElementById('auth-title').innerText = isLogin ? "Access Terminal" : "Initialize Identity";
            document.getElementById('toggleTxt').innerHTML = isLogin ? "Identity not found? <span style='color:var(--accent)'>Initialize</span>" : "Have account? <span style='color:var(--accent)'>Login</span>";
        };

        window.handleAuth = async () => {
            const u = document.getElementById('username').value.toLowerCase().trim();
            const p = document.getElementById('password').value;
            if(!u || !p) return alert("Credentials required!");
            const ref = doc(db, "users", u);
            const snap = await getDoc(ref);
            if(isLogin) {
                if(snap.exists() && snap.data().password === p) { sessionStorage.setItem("nova_user", u); start(); }
                else alert("Unauthorized Access!");
            } else {
                if(snap.exists()) return alert("User exists!");
                await setDoc(ref, { password: p, balance: 0, daily: 0, total: 0, lastBonus: "" });
                alert("Account Ready! Please Login."); isLogin = true; toggleAuth();
            }
        };

        function start() {
            user = sessionStorage.getItem("nova_user");
            if(!user) return;
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app-main').style.display = 'block';
            document.getElementById('nav-bar').style.display = 'flex';
            document.getElementById('userDisp').innerText = user.toUpperCase();

            onSnapshot(doc(db, "users", user), (d) => {
                if(d.exists()){
                    const data = d.data(); bal = data.balance;
                    document.getElementById('mainBal').innerText = "$" + bal.toLocaleString();
                    document.getElementById('dayYield').innerText = "+$" + (data.daily || 0).toLocaleString();
                    document.getElementById('totProfit').innerText = "$" + (data.total || 0).toLocaleString();
                    
                    let rank = "BASIC";
                    if(bal > 1000) rank = "ELITE";
                    if(bal > 10000) rank = "WHALE";
                    document.getElementById('vipStatus').innerText = rank;
                }
            });
            renderNodes();
        }

        function renderNodes() {
            const grid = document.getElementById('nodeGrid'); grid.innerHTML = "";
            const prices = [30, 100, 250, 500, 1000, 2500, 5000, 10000, 20000, 30000];
            const imgs = ["1639762681485-074b7f938ba0", "1558494949-ef010cbdcc51", "1518770660439-4636190af475", "1640343136701-d051ff221245"];
            
            prices.forEach((p, i) => {
                const y = (p * 0.12).toFixed(2);
                grid.innerHTML += `<div class="node-card">
                    <span class="yield-badge">12% ROI</span>
                    <img src="https://images.unsplash.com/photo-${imgs[i%4]}?w=300" class="node-img">
                    <b style="font-size:10px; color:var(--dim); text-transform:uppercase;">Cluster V.${i+1}</b>
                    <h2 style="margin:8px 0; font-size:1.6rem; font-weight:900;">$${p.toLocaleString()}</h2>
                    <p style="font-size:10px; color:var(--accent); margin-bottom:15px; font-weight:800;">Harvest: $${y}/Day</p>
                    <button onclick="buy(${p})" style="width:100%; border:none; background:rgba(16,185,129,0.1); color:var(--accent); padding:12px; border-radius:15px; font-weight:900; font-size:10px; cursor:pointer;">ACTIVATE</button>
                </div>`;
            });
        }

        window.updateAddr = () => {
            const m = document.getElementById('depMethod').value;
            const addr = m === "MetaMask" ? "0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2" : "0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37";
            document.getElementById('walletAddr').innerText = addr;
        };

        window.submitDep = async () => {
            const amt = document.getElementById('depAmt').value;
            const t = document.getElementById('tid').value;
            if(!amt || !t) return alert("Missing Details!");
            await addDoc(collection(db, "pending_tx"), { user, amount: parseFloat(amt), tid: t, status: "Pending", type: "Deposit", time: Date.now() });
            alert("Verification Request Broadcasted!");
        };

        window.submitWit = async () => {
            const amt = parseFloat(document.getElementById('witAmt').value);
            const addr = document.getElementById('witAddr').value;
            if(amt < 50) return alert("Minimum withdrawal $50!");
            if(bal < amt) return alert("Insufficient Capital!");
            await updateDoc(doc(db, "users", user), { balance: bal - amt });
            await addDoc(collection(db, "pending_tx"), { user, amount: amt, addr, status: "Pending", type: "Withdraw", time: Date.now() });
            alert("Extraction Initialized!");
        };

        window.buy = async (p) => {
            if(bal < p) return alert("Insufficient Assets!");
            if(confirm(`Activate Cluster for $${p}?`)) {
                await updateDoc(doc(db, "users", user), { balance: bal - p });
                alert("Protocol Activated! Earnings will start soon.");
            }
        };

        window.claimBonus = async () => {
            const today = new Date().toDateString();
            const ref = doc(db, "users", user);
            const snap = await getDoc(ref);
            if(snap.data().lastBonus === today) return alert("Already Harvested!");
            await updateDoc(ref, { balance: bal + 0.50, lastBonus: today });
            alert("Daily $0.50 Credited!");
        };

        window.nav = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            el.classList.add('active');
        };

        window.logout = () => { sessionStorage.clear(); location.reload(); };
        window.copyRef = () => { navigator.clipboard.writeText(document.getElementById('refLink').innerText); alert("Referral ID Copied!"); };

        if(sessionStorage.getItem("nova_user")) start();
    </script>
</body>
</html>
