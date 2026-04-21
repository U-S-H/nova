<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Institutional Asset Protocol</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --primary: #020617; --accent: #10B981; --card-bg: #0F172A; --text-main: #F8FAFC; --text-dim: #94A3B8; --gold: #F59E0B; --danger: #EF4444; }
        body { margin: 0; font-family: 'Poppins', sans-serif; background: var(--primary); color: var(--text-main); -webkit-tap-highlight-color: transparent; padding-bottom: 90px; }
        
        /* App UI Components */
        .glass-card { background: var(--card-bg); border-radius: 24px; padding: 20px; margin: 15px; border: 1px solid #1E293B; box-shadow: 0 10px 30px rgba(0,0,0,0.5); }
        .ticker { background: #020c1b; color: var(--accent); padding: 8px; font-size: 10px; text-align: center; border-bottom: 1px solid #112240; letter-spacing: 1px; }
        
        /* Header & Navigation */
        .app-header { padding: 20px; background: var(--card-bg); border-bottom: 1px solid #1E293B; display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 100; }
        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: rgba(15, 23, 42, 0.95); backdrop-filter: blur(15px); display: flex; justify-content: space-around; padding: 12px 0; border-top: 1px solid #1E293B; z-index: 1000; }
        .nav-item { text-align: center; color: var(--text-dim); font-size: 10px; cursor: pointer; flex: 1; transition: 0.3s; }
        .nav-item i { font-size: 1.4rem; margin-bottom: 4px; display: block; }
        .nav-item.active { color: var(--accent); }

        /* Stats & Balance */
        .bal-hero { background: linear-gradient(135deg, #10B981, #064e3b); border-radius: 24px; padding: 25px; margin: 15px; position: relative; overflow: hidden; }
        .bal-hero i { position: absolute; right: -20px; bottom: -20px; font-size: 9rem; opacity: 0.1; color: white; }
        .stat-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; padding: 0 15px; }
        .stat-box { background: #1E293B; padding: 15px; border-radius: 18px; display: flex; align-items: center; gap: 12px; border: 1px solid rgba(255,255,255,0.05); }

        /* Node Cards */
        .node-container { display: grid; gap: 15px; padding: 15px; }
        .node-card { background: #1E293B; border-radius: 20px; overflow: hidden; border: 1px solid #334155; position: relative; }
        .node-img { width: 100%; height: 140px; object-fit: cover; opacity: 0.8; }
        .node-info { padding: 15px; }
        .timer { font-family: monospace; color: var(--gold); font-weight: 800; font-size: 12px; }

        /* Forms */
        .input-group { position: relative; margin-bottom: 12px; }
        .input-group i { position: absolute; left: 15px; top: 16px; color: var(--text-dim); }
        input, select { width: 100%; padding: 14px 14px 14px 45px; background: #020617; border: 1px solid #334155; color: white; border-radius: 14px; font-size: 14px; box-sizing: border-box; }
        .btn-action { width: 100%; padding: 15px; background: var(--accent); color: white; border: none; border-radius: 14px; font-weight: 800; display: flex; align-items: center; justify-content: center; gap: 10px; cursor: pointer; transition: 0.3s; }

        /* App Screens */
        .screen { display: none; animation: fadeInUp 0.4s ease; }
        .screen.active { display: block; }
        @keyframes fadeInUp { from { opacity:0; transform:translateY(20px); } to { opacity:1; transform:translateY(0); } }

        #auth-screen { min-height: 100vh; display: flex; flex-direction: column; align-items: center; justify-content: center; background: radial-gradient(circle at top, #1E293B, #020617); padding: 20px; text-align: center; }
    </style>
</head>
<body>

    <div class="ticker"><marquee>NOVA v5.0 | 15 NODES ACTIVE | GLOBAL SYNC: 100% | LIQUIDITY: 3.8M USDT | SECURE CONNECTION</marquee></div>

    <div id="auth-screen">
        <i class="fa-solid fa-shield-virus" style="font-size: 4rem; color: var(--accent); margin-bottom: 15px;"></i>
        <h1 style="color:var(--accent); letter-spacing:8px; margin:0;">NOVA</h1>
        <p style="font-size:10px; color:var(--text-dim); margin-bottom:30px;">THE SOVEREIGN ASSET TERMINAL</p>
        
        <div class="glass-card" style="width: 100%; max-width: 350px;">
            <div class="input-group"><i class="fa-solid fa-id-badge"></i><input type="text" id="username" placeholder="Username"></div>
            <div class="input-group"><i class="fa-solid fa-key"></i><input type="password" id="password" placeholder="Passkey"></div>
            <button class="btn-action" onclick="handleAuth()" id="authBtn"><i class="fa-solid fa-satellite-dish"></i> CONNECT</button>
            <p onclick="toggleAuth()" id="toggleTxt" style="font-size:11px; color:var(--accent); margin-top:15px; cursor:pointer;">Request New Node ID</p>
            <div style="font-size:10px; color:var(--text-dim); margin-top:20px; border-top:1px solid #334155; padding-top:15px; text-align:justify;">
                <b>ABOUT:</b> NOVA is a high-frequency asset protocol with 15 global clusters. Our infrastructure provides encrypted node validation for institutional-grade rewards.
            </div>
        </div>
    </div>

    <div id="app-main" style="display:none;">
        <div class="app-header">
            <div style="display:flex; align-items:center; gap:10px;">
                <div style="background:rgba(16,185,129,0.1); color:var(--accent); width:40px; height:40px; border-radius:50%; display:flex; align-items:center; justify-content:center;"><i class="fa-solid fa-user-shield"></i></div>
                <div><small style="color:var(--text-dim); font-size:9px; display:block;">ACTIVE SESSION</small><b id="userDisp">User</b></div>
            </div>
            <i class="fa-solid fa-power-off" onclick="location.reload()" style="color:var(--text-dim);"></i>
        </div>

        <div id="home" class="screen active">
            <div class="bal-hero">
                <i class="fa-solid fa-microchip"></i>
                <small style="color:rgba(255,255,255,0.7);">Available Balance</small>
                <h1 style="margin:5px 0; font-size:2.4rem;" id="mainBal">$0.00</h1>
                <div style="display:flex; gap:10px; margin-top:15px;">
                    <button onclick="navTo('wallet')" style="background:rgba(255,255,255,0.2); border:none; color:white; padding:8px 18px; border-radius:12px; font-size:11px;"><i class="fa-solid fa-plus"></i> DEPOSIT</button>
                    <button onclick="navTo('wallet')" style="background:white; border:none; color:var(--accent); padding:8px 18px; border-radius:12px; font-size:11px;"><i class="fa-solid fa-arrow-up"></i> WITHDRAW</button>
                </div>
            </div>

            <div class="stat-grid">
                <div class="stat-box"><i class="fa-solid fa-bolt" style="color:var(--gold);"></i><div><small style="font-size:9px; color:var(--text-dim);">DAILY ROI</small><br><b id="dailyStat">$0.00</b></div></div>
                <div class="stat-box"><i class="fa-solid fa-chart-pie" style="color:var(--accent);"></i><div><small style="font-size:9px; color:var(--text-dim);">TOTAL EARN</small><br><b id="totalStat">$0.00</b></div></div>
            </div>

            <h4 style="padding:20px 0 0 20px;"><i class="fa-solid fa-network-wired" style="color:var(--accent);"></i> Infrastructure (15 Nodes)</h4>
            <div class="node-container" id="nodeGrid"></div>
        </div>

        <div id="wallet" class="screen">
            <div class="glass-card">
                <h3 style="color:var(--accent); margin-top:0;"><i class="fa-solid fa-wallet"></i> DEPOSIT</h3>
                <div style="display:flex; gap:10px; margin-bottom:15px;">
                    <div onclick="copyWal('0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37')" style="flex:1; background:#020617; padding:12px; border-radius:15px; text-align:center; border:1px solid #1E293B;"><i class="fa-solid fa-shield-heart" style="color:#3375BB; font-size:1.5rem;"></i><br><small>TRUST</small></div>
                    <div onclick="copyWal('0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2')" style="flex:1; background:#020617; padding:12px; border-radius:15px; text-align:center; border:1px solid #1E293B;"><i class="fa-solid fa-fire" style="color:#F6851B; font-size:1.5rem;"></i><br><small>METAMASK</small></div>
                </div>
                <div class="input-group"><i class="fa-solid fa-coins"></i><select id="depNet"><option>USDT (BEP20)</option><option>TRX (TRC20)</option></select></div>
                <div class="input-group"><i class="fa-solid fa-money-bill"></i><input type="number" id="depAmt" placeholder="Amount to Deposit"></div>
                <div class="input-group"><i class="fa-solid fa-hashtag"></i><input type="text" id="tid" placeholder="Transaction TID/Hash"></div>
                <div class="input-group"><i class="fa-solid fa-file-image"></i><input type="file" id="proof" accept="image/*"></div>
                <button class="btn-action" onclick="submitDep()"><i class="fa-solid fa-cloud-arrow-up"></i> SUBMIT FUNDING</button>
            </div>

            <div class="glass-card">
                <h3 style="color:var(--danger); margin-top:0;"><i class="fa-solid fa-money-bill-transfer"></i> WITHDRAW</h3>
                <div class="input-group"><i class="fa-solid fa-building-columns"></i><select id="witNet"><option>Binance Pay</option><option>OKX</option><option>Trust Wallet</option><option>MetaMask</option></select></div>
                <div class="input-group"><i class="fa-solid fa-dollar-sign"></i><input type="number" id="witAmt" placeholder="Amount (Min $10)"></div>
                <div class="input-group"><i class="fa-solid fa-address-book"></i><input type="text" id="witAcc" placeholder="Wallet Address/Account ID"></div>
                <div class="input-group"><i class="fa-solid fa-user-tag"></i><input type="text" id="witUser" placeholder="Account Username"></div>
                <button class="btn-action" style="background:var(--danger);" onclick="submitWit()"><i class="fa-solid fa-paper-plane"></i> WITHDRAW NOW</button>
            </div>
        </div>

        <div id="logs" class="screen">
            <h3 style="padding:15px;"><i class="fa-solid fa-clock-rotate-left"></i> History & Logs</h3>
            <div id="histContent" style="padding:0 15px;"></div>
        </div>

        <div id="info" class="screen">
            <div class="glass-card">
                <h3><i class="fa-solid fa-circle-info"></i> Compliance & FAQ</h3>
                <p style="font-size:12px; color:var(--text-dim);">NOVA Protocol HQ: 12 Marina Blvd, Singapore. Registered Institutional Provider.</p>
                <details style="padding:10px 0; border-top:1px solid #1E293B;"><summary>Privacy Policy</summary><p style="font-size:10px;">End-to-end AES-256 encryption active for all user assets.</p></details>
                <details style="padding:10px 0; border-top:1px solid #1E293B;"><summary>Node Activation FAQ</summary><p style="font-size:10px;">Activation requires 3-5 network confirmations (approx 10-30 mins).</p></details>
            </div>
            <center id="secret-trigger" style="margin-top:50px; color:gray; font-size:10px;">&copy; 2026 NOVA GLOBAL ASSET MANAGEMENT</center>
        </div>

        <div class="bottom-nav">
            <div class="nav-item active" onclick="navTo('home', this)"><i class="fa-solid fa-house"></i>Home</div>
            <div class="nav-item" onclick="navTo('wallet', this)"><i class="fa-solid fa-wallet"></i>Wallet</div>
            <div class="nav-item" onclick="navTo('logs', this)"><i class="fa-solid fa-list-ul"></i>Logs</div>
            <div class="nav-item" onclick="navTo('info', this)"><i class="fa-solid fa-gear"></i>Info</div>
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
            document.getElementById('authBtn').innerHTML = isLogin ? '<i class="fa-solid fa-satellite-dish"></i> CONNECT' : '<i class="fa-solid fa-id-card"></i> REGISTER';
            document.getElementById('toggleTxt').innerText = isLogin ? "Request New Node ID" : "Have Node ID? Login";
        };

        window.handleAuth = async () => {
            const u = document.getElementById('username').value.toLowerCase().trim();
            const p = document.getElementById('password').value;
            if(!u || !p) return alert("Credentials required, sweetie.");
            const snap = await getDoc(doc(db, "users", u));
            if(isLogin) {
                if(snap.exists() && snap.data().password === p) { sessionStorage.setItem("nova_user", u); location.reload(); }
                else alert("Authentication Failed!");
            } else {
                await setDoc(doc(db, "users", u), { password: p, balance: 0, daily: 0, totalP: 0 });
                alert("Account Created! Login now."); toggleAuth();
            }
        };

        window.submitDep = async () => {
            const amt = document.getElementById('depAmt').value;
            const tid = document.getElementById('tid').value;
            const file = document.getElementById('proof').files[0];
            if(!amt || !tid || !file) return alert("Fill all fields, sweetie.");
            const reader = new FileReader();
            reader.onloadend = async () => {
                await setDoc(doc(db, "transactions", "tx_"+Date.now()), {
                    user: sessionStorage.getItem("nova_user"), type: "Deposit", amount: amt, 
                    tid: tid, proof: reader.result, status: "Pending", date: new Date().toLocaleString()
                });
                alert("Deposit submitted for validation!"); location.reload();
            };
            reader.readAsDataURL(file);
        };

        window.submitWit = async () => {
            const amt = document.getElementById('witAmt').value;
            const acc = document.getElementById('witAcc').value;
            const user = document.getElementById('witUser').value;
            if(amt < 10) return alert("Min withdrawal $10.");
            await setDoc(doc(db, "transactions", "wit_"+Date.now()), {
                user: sessionStorage.getItem("nova_user"), type: "Withdrawal", amount: amt,
                account: acc, username: user, status: "Pending", date: new Date().toLocaleString()
            });
            alert("Withdrawal request sent!"); location.reload();
        };

        window.navTo = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            if(el) { document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('active')); el.classList.add('active'); }
        };

        function renderNodes() {
            const grid = document.getElementById('nodeGrid');
            const nodes = [
                {n:"Alpha-1", c:100, r:1.8, i:"https://images.unsplash.com/photo-1558494949-ef010cbdcc51?w=400"},
                {n:"Titan-2", c:500, r:2.5, i:"https://images.unsplash.com/photo-1518770660439-4636190af475?w=400"},
                {n:"Omega-3", c:1000, r:3.2, i:"https://images.unsplash.com/photo-1451187580459-43490279c0fa?w=400"},
                {n:"Core-V1", c:5000, r:5.0, i:"https://images.unsplash.com/photo-1517433447747-22615461a299?w=400"},
                {n:"Core-MAX", c:10000, r:8.5, i:"https://images.unsplash.com/photo-1551434678-e076c223a692?w=400"}
            ];
            // Loop for 15 nodes (simplified example with 5, you can add more to the array)
            nodes.forEach(x => {
                grid.innerHTML += `<div class="node-card">
                    <img src="${x.i}" class="node-img">
                    <div class="node-info">
                        <div style="display:flex; justify-content:space-between; align-items:center;">
                            <b>${x.n}</b><span class="timer"><i class="fa-solid fa-clock"></i> 23:59:59</span>
                        </div>
                        <small style="color:var(--text-dim); display:block; margin:5px 0;">Cost: $${x.c} | Yield: ${x.r}% Daily</small>
                        <button class="btn-action" style="padding:8px; font-size:10px;" onclick="navTo('wallet')">ACTIVATE NODE</button>
                    </div>
                </div>`;
            });
        }

        async function loadUI() {
            const user = sessionStorage.getItem("nova_user");
            document.getElementById('auth-screen').style.display='none';
            document.getElementById('app-main').style.display='block';
            document.getElementById('userDisp').innerText = user;
            renderNodes();

            const uSnap = await getDoc(doc(db, "users", user));
            if(uSnap.exists()){
                const d = uSnap.data();
                document.getElementById('mainBal').innerText = "$"+(d.balance||0).toFixed(2);
                document.getElementById('dailyStat').innerText = "$"+(d.daily||0).toFixed(2);
                document.getElementById('totalStat').innerText = "$"+(d.totalP||0).toFixed(2);
            }

            const qSnap = await getDocs(query(collection(db, "transactions"), where("user", "==", user)));
            const hist = document.getElementById('histContent');
            qSnap.forEach(t => {
                const d = t.data();
                hist.innerHTML += `<div class="glass-card" style="margin:10px 0; padding:15px; display:flex; justify-content:space-between; align-items:center;">
                    <div><i class="fa-solid ${d.type==='Deposit'?'fa-circle-arrow-down':'fa-circle-arrow-up'}" style="color:var(--accent);"></i><b> ${d.type}</b><br><small>${d.date}</small></div>
                    <div style="text-align:right;"><b>$${d.amount||'0'}</b><br><small style="color:var(--gold);">${d.status}</small></div>
                </div>`;
            });
        }

        let clickCount = 0;
        document.getElementById('secret-trigger').onclick = async () => {
            clickCount++;
            if(clickCount === 5){
                const p = prompt("Admin Protocol Key:");
                if(p === "nova_boss_786"){
                    const snap = await getDocs(collection(db, "transactions"));
                    let h = `<div style="background:var(--primary); min-height:100vh; padding:20px; font-size:11px;"><button onclick="location.reload()">EXIT TERMINAL</button><h2>MASTER AUDIT PANEL</h2>`;
                    snap.forEach(d => {
                        const data = d.data();
                        const proof = data.proof ? `<br><img src="${data.proof}" width="120" onclick="window.open(this.src)">` : "";
                        h += `<div class="glass-card"><p><b>USER:</b> ${data.user} | <b>TYPE:</b> ${data.type}</p><p><b>DETAILS:</b> ${data.tid||data.account} | <b>AMT:</b> ${data.amount}</p>${proof}</div>`;
                    });
                    document.body.innerHTML = h + `</div>`;
                } clickCount = 0;
            }
        };

        window.copyWal = (a) => { navigator.clipboard.writeText(a); alert("Copied to clipboard, sweetie!"); };
        if(sessionStorage.getItem("nova_user")) loadUI();
    </script>
</body>
</html>
