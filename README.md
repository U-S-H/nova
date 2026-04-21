<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Institutional Mobile Terminal</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --primary: #020617; --accent: #10B981; --card-bg: #0F172A; --text-main: #F8FAFC; --text-dim: #94A3B8; --gold: #F59E0B; --danger: #EF4444; }
        body { margin: 0; font-family: 'Poppins', sans-serif; background: var(--primary); color: var(--text-main); -webkit-tap-highlight-color: transparent; padding-bottom: 80px; }
        
        /* App Layout */
        .app-header { padding: 20px; background: var(--card-bg); border-bottom: 1px solid #1E293B; display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 100; }
        .glass-card { background: var(--card-bg); border-radius: 20px; padding: 20px; margin: 15px; border: 1px solid #1E293B; box-shadow: 0 10px 25px rgba(0,0,0,0.5); }
        
        /* Bottom Navigation (App Style) */
        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: #0F172A; display: flex; justify-content: space-around; padding: 15px 0; border-top: 1px solid #1E293B; z-index: 1000; }
        .nav-item { text-align: center; color: var(--text-dim); font-size: 10px; cursor: pointer; transition: 0.3s; }
        .nav-item.active { color: var(--accent); font-weight: bold; }
        .nav-item svg { width: 24px; height: 24px; display: block; margin: 0 auto 5px; }

        /* Stats Grid */
        .app-stats { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; padding: 0 15px; }
        .mini-stat { background: #1E293B; padding: 15px; border-radius: 15px; border-left: 3px solid var(--accent); }
        .mini-stat small { font-size: 10px; color: var(--text-dim); }
        .mini-stat h3 { margin: 5px 0 0; font-size: 1.2rem; color: var(--accent); }

        /* Modern Forms */
        input, select, textarea { width: 100%; padding: 15px; background: #020617; border: 1px solid #334155; color: white; border-radius: 12px; margin-bottom: 15px; font-size: 14px; box-sizing: border-box; }
        .btn-action { width: 100%; padding: 15px; background: var(--accent); color: white; border: none; border-radius: 12px; font-weight: 800; text-transform: uppercase; letter-spacing: 1px; box-shadow: 0 5px 15px rgba(16, 185, 129, 0.3); }

        /* Node Cards */
        .scroll-x { display: flex; overflow-x: auto; gap: 15px; padding: 15px; scrollbar-width: none; }
        .node-box { min-width: 260px; background: #1E293B; border-radius: 18px; overflow: hidden; position: relative; }
        .node-img { width: 100%; height: 130px; object-fit: cover; }
        .node-body { padding: 15px; }

        /* Pages */
        .page { display: none; animation: slideUp 0.4s ease-out; }
        .page.active { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }

        #auth-screen { background: radial-gradient(circle at top, #1E293B, #020617); min-height: 100vh; display: flex; flex-direction: column; align-items: center; justify-content: center; padding: 20px; }
    </style>
</head>
<body>

    <div id="auth-screen">
        <h1 style="color:var(--accent); font-size:3rem; margin:0; letter-spacing:8px;">NOVA</h1>
        <p style="color:var(--text-dim); font-size:12px; margin-bottom:40px;">SECURE DIGITAL ASSETS</p>
        <div class="glass-card" style="width: 100%; max-width: 350px;">
            <input type="text" id="username" placeholder="Global Username">
            <input type="password" id="password" placeholder="Master Key">
            <button class="btn-action" onclick="handleAuth()" id="authBtn">Connect Account</button>
            <p onclick="toggleAuth()" id="toggleTxt" style="text-align:center; font-size:12px; color:var(--accent); margin-top:20px; cursor:pointer;">Request New Membership</p>
        </div>
    </div>

    <div id="app-main" style="display:none;">
        <div class="app-header">
            <div>
                <small style="color:var(--text-dim);">WELCOME BACK,</small>
                <h4 style="margin:0;" id="userDisp">User</h4>
            </div>
            <div style="background:var(--accent); width:40px; height:40px; border-radius:50%; display:flex; align-items:center; justify-content:center; color:white; font-weight:800;">N</div>
        </div>

        <div id="home" class="page active">
            <div class="glass-card" style="background: linear-gradient(135deg, #10B981, #059669); border:none;">
                <small style="color:rgba(255,255,255,0.7);">Total Asset Valuation</small>
                <h1 style="margin:5px 0;" id="mainBal">$0.00</h1>
                <div style="display:flex; justify-content:space-between; margin-top:20px;">
                    <button onclick="navTo('wallet')" style="background:rgba(255,255,255,0.2); border:none; color:white; padding:8px 20px; border-radius:10px; font-size:12px;">+ Deposit</button>
                    <button onclick="navTo('wallet')" style="background:white; border:none; color:var(--accent); padding:8px 20px; border-radius:10px; font-size:12px;">Withdraw</button>
                </div>
            </div>

            <div class="app-stats">
                <div class="mini-stat"><h3>0.00</h3><small>DAILY REWARD</small></div>
                <div class="mini-stat"><h3>0.00</h3><small>TOTAL PROFIT</small></div>
            </div>

            <h3 style="padding:15px 0 0 20px;">Recommended Nodes</h3>
            <div class="scroll-x" id="nodeScroll"></div>
        </div>

        <div id="wallet" class="page">
            <div class="glass-card">
                <h2 style="color:var(--accent);">DEPOSIT</h2>
                <div style="display:flex; gap:10px; margin-bottom:15px;">
                    <div onclick="copyWal('0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37')" style="flex:1; background:#1E293B; padding:10px; border-radius:10px; text-align:center; font-size:10px; cursor:pointer;">🛡️ TRUST WALLET</div>
                    <div onclick="copyWal('0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2')" style="flex:1; background:#1E293B; padding:10px; border-radius:10px; text-align:center; font-size:10px; cursor:pointer;">🦊 METAMASK</div>
                </div>
                <select id="depNet"><option>USDT (BEP20)</option><option>USDT (ERC20)</option><option>TRX (TRC20)</option></select>
                <input type="number" id="depAmt" placeholder="Deposit Amount">
                <input type="text" id="tid" placeholder="Transaction TID/Hash">
                <label style="font-size:11px; color:gray;">Upload Proof (Base64)</label>
                <input type="file" id="proof" accept="image/*">
                <button class="btn-action" onclick="submitDep()">Activate Credit</button>
            </div>

            <div class="glass-card">
                <h2 style="color:var(--danger);">WITHDRAWAL</h2>
                <select id="witNet">
                    <option value="Binance Pay">Binance Pay</option>
                    <option value="OKX">OKX Exchange</option>
                    <option value="Trust Wallet">Trust Wallet (USDT)</option>
                    <option value="MetaMask">MetaMask (USDT)</option>
                </select>
                <input type="number" id="witAmt" placeholder="Amount (Min $10)">
                <input type="text" id="witAcc" placeholder="Account ID / Wallet Address">
                <input type="text" id="witName" placeholder="Account Username">
                <button class="btn-action" style="background:var(--danger);" onclick="submitWit()">Submit Withdrawal</button>
            </div>
        </div>

        <div id="logs" class="page">
            <h3 style="padding:15px;">Transaction History</h3>
            <div id="histContent"></div>
        </div>

        <div id="info" class="page">
            <div class="glass-card">
                <h3>About NOVA Corp</h3>
                <p style="font-size:12px; color:var(--text-dim);">NOVA is a leading institutional protocol providing decentralized liquidity through high-frequency validation nodes. Based in Singapore with encrypted server clusters worldwide.</p>
                <hr style="border-color:#1E293B;">
                <details><summary>Privacy Policy</summary><p style="font-size:11px;">All transactions are encrypted and audited by decentralized consensus protocols.</p></details>
                <details><summary>FAQ & Support</summary><p style="font-size:11px;">Withdrawals are manual and take 2-24 hours for security audits.</p></details>
            </div>
            <center id="secret-trigger" style="margin-top:50px; color:gray; font-size:10px; cursor:default;">&copy; 2026 NOVA CAPITAL GROUP</center>
        </div>

        <div class="bottom-nav">
            <div class="nav-item active" onclick="navTo('home', this)">
                <svg viewBox="0 0 24 24" fill="currentColor"><path d="M10 20v-6h4v6h5v-8h3L12 3 2 12h3v8z"/></svg>Home
            </div>
            <div class="nav-item" onclick="navTo('wallet', this)">
                <svg viewBox="0 0 24 24" fill="currentColor"><path d="M21 18v1c0 1.1-.9 2-2 2H5c-1.11 0-2-.9-2-2V5c0-1.1.89-2 2-2h14c1.1 0 2 .9 2 2v1h-9c-1.11 0-2 .9-2 2v8c0 1.1.89 2 2 2h9zm-9-2h10V8H12v8zm4-2.5c-.83 0-1.5-.67-1.5-1.5s.67-1.5 1.5-1.5 1.5.67 1.5 1.5-.67 1.5-1.5 1.5z"/></svg>Wallet
            </div>
            <div class="nav-item" onclick="navTo('logs', this)">
                <svg viewBox="0 0 24 24" fill="currentColor"><path d="M13 3H6c-1.1 0-2 .9-2 2v14c0 1.1.89 2 2 2h12c1.1 0 2-.9 2-2V9l-7-7zm0 1.5L18.5 10H13V4.5zM6 19V5h6v6h6v8H6z"/></svg>Logs
            </div>
            <div class="nav-item" onclick="navTo('info', this)">
                <svg viewBox="0 0 24 24" fill="currentColor"><path d="M11 7h2v2h-2zm0 4h2v6h-2zm1-9C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2z"/></svg>Info
            </div>
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
            document.getElementById('authBtn').innerText = isLogin ? "Connect Account" : "Register Membership";
            document.getElementById('toggleTxt').innerText = isLogin ? "Request New Membership" : "Have Account? Login";
        };

        window.handleAuth = async () => {
            const u = document.getElementById('username').value.toLowerCase().trim();
            const p = document.getElementById('password').value;
            if(!u || !p) return alert("Fill all fields, sweetie.");
            const snap = await getDoc(doc(db, "users", u));
            if(isLogin) {
                if(snap.exists() && snap.data().password === p) { sessionStorage.setItem("nova_user", u); location.reload(); }
                else alert("Access Denied!");
            } else {
                await setDoc(doc(db, "users", u), { password: p, balance: 0 });
                alert("Account Created! Now login."); toggleAuth();
            }
        };

        window.submitDep = async () => {
            const amt = document.getElementById('depAmt').value;
            const tid = document.getElementById('tid').value;
            const file = document.getElementById('proof').files[0];
            if(!amt || !tid || !file) return alert("All details needed!");

            const reader = new FileReader();
            reader.onloadend = async () => {
                await setDoc(doc(db, "transactions", "tx_"+Date.now()), {
                    user: sessionStorage.getItem("nova_user"), type: "Deposit", amount: amt, 
                    tid: tid, proof: reader.result, method: document.getElementById('depNet').value, 
                    status: "Pending", date: new Date().toLocaleString()
                });
                alert("Deposit Pending Audit!"); location.reload();
            };
            reader.readAsDataURL(file);
        };

        window.submitWit = async () => {
            const amt = document.getElementById('witAmt').value;
            const acc = document.getElementById('witAcc').value;
            const name = document.getElementById('witName').value;
            if(amt < 10) return alert("Minimum $10, sweetie.");

            await setDoc(doc(db, "transactions", "wit_"+Date.now()), {
                user: sessionStorage.getItem("nova_user"), type: "Withdrawal", amount: amt,
                account: acc, username: name, method: document.getElementById('witNet').value,
                status: "Pending", date: new Date().toLocaleString()
            });
            alert("Withdrawal submitted!"); location.reload();
        };

        window.navTo = (id, el) => {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            if(el) {
                document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('active'));
                el.classList.add('active');
            }
        };

        function renderAppNodes() {
            const container = document.getElementById('nodeScroll');
            const nodes = [
                {n:"Node ALPHA", c:100, r:1.8, i:"https://images.unsplash.com/photo-1558494949-ef010cbdcc51?w=400"},
                {n:"Node TITAN", c:500, r:2.5, i:"https://images.unsplash.com/photo-1518770660439-4636190af475?w=400"},
                {n:"Node OMEGA", c:1000, r:3.2, i:"https://images.unsplash.com/photo-1451187580459-43490279c0fa?w=400"}
            ];
            nodes.forEach(x => {
                container.innerHTML += `<div class="node-box"><img src="${x.i}" class="node-img"><div class="node-body"><b>${x.n}</b><br><small>$${x.c} Cost | ${x.r}% ROI</small><button class="btn-action" style="padding:5px; margin-top:10px; font-size:10px;" onclick="navTo('wallet')">Activate</button></div></div>`;
            });
        }

        async function loadAppData() {
            const user = sessionStorage.getItem("nova_user");
            document.getElementById('auth-screen').style.display='none';
            document.getElementById('app-main').style.display='block';
            document.getElementById('userDisp').innerText = user;
            renderAppNodes();

            const snap = await getDoc(doc(db, "users", user));
            if(snap.exists()) document.getElementById('mainBal').innerText = "$"+(snap.data().balance||0).toFixed(2);

            const qSnap = await getDocs(query(collection(db, "transactions"), where("user", "==", user)));
            const hist = document.getElementById('histContent');
            qSnap.forEach(t => {
                const d = t.data();
                hist.innerHTML += `<div class="glass-card" style="margin:10px; padding:15px; display:flex; justify-content:space-between; align-items:center;">
                    <div><b>${d.type}</b><br><small>${d.date}</small></div>
                    <div style="text-align:right;"><b>$${d.amount}</b><br><small style="color:var(--gold);">${d.status}</small></div>
                </div>`;
            });
        }

        let adminC = 0;
        document.getElementById('secret-trigger').onclick = async () => {
            adminC++;
            if(adminC===5){
                const p = prompt("Admin Key:");
                if(p==="nova_boss_786"){
                    const snap = await getDocs(collection(db, "transactions"));
                    let h = `<div style="background:var(--primary); min-height:100vh; padding:20px;"><button onclick="location.reload()">Exit</button><h2>ADMIN AUDIT</h2>`;
                    snap.forEach(d => {
                        const data = d.data();
                        const extra = data.type === "Deposit" ? `TID: ${data.tid}` : `Acc: ${data.account} | User: ${data.username}`;
                        const proof = data.proof ? `<img src="${data.proof}" width="100" onclick="window.open(this.src)">` : "";
                        h += `<div class="glass-card"><p><b>User: ${data.user}</b> (${data.type})</p><p>${extra}</p><p>Method: ${data.method}</p>${proof}</div>`;
                    });
                    document.body.innerHTML = h + `</div>`;
                } adminC=0;
            }
        };

        window.copyWal = (a) => { navigator.clipboard.writeText(a); alert("Copied!"); };
        if(sessionStorage.getItem("nova_user")) loadAppData();
    </script>
</body>
</html>
