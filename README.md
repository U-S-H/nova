<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Institutional Asset Management</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap" rel="stylesheet">
    <style>
        :root { --primary: #020617; --accent: #10B981; --card-bg: #0F172A; --text-main: #F8FAFC; --text-dim: #94A3B8; --gold: #F59E0B; --danger: #EF4444; --elite: #8B5CF6; }
        body { margin: 0; font-family: 'Outfit', sans-serif; background: var(--primary); color: var(--text-main); overflow-x: hidden; padding-bottom: 90px; }
        
        /* MODERN SCROLLBAR */
        ::-webkit-scrollbar { width: 5px; }
        ::-webkit-scrollbar-thumb { background: var(--accent); border-radius: 10px; }

        /* MAINTENANCE & BROADCAST */
        #maintenance-screen { display: none; position: fixed; inset: 0; background: var(--primary); z-index: 10000; flex-direction: column; align-items: center; justify-content: center; text-align: center; padding: 20px; }
        #broadcast-bar { display: none; background: var(--elite); color: white; padding: 10px; font-size: 11px; text-align: center; position: sticky; top: 0; z-index: 2000; font-weight: 700; }

        /* UI ELEMENTS */
        .glass { background: rgba(15, 23, 42, 0.7); backdrop-filter: blur(15px); border: 1px solid rgba(255,255,255,0.05); border-radius: 28px; }
        .glass-card { @extend .glass; padding: 20px; margin: 15px; }
        .btn-prime { width: 100%; padding: 16px; background: var(--accent); color: white; border: none; border-radius: 18px; font-weight: 900; cursor: pointer; transition: 0.3s; }
        .btn-prime:active { transform: scale(0.95); }
        input, select, textarea { width: 100%; padding: 15px; background: #020617; border: 1px solid #334155; color: #fff; border-radius: 16px; box-sizing: border-box; margin-bottom: 12px; outline: none; }
        
        /* HEADER & NAV */
        .app-header { padding: 15px 20px; background: rgba(15, 23, 42, 0.8); backdrop-filter: blur(20px); border-bottom: 1px solid rgba(255,255,255,0.05); display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 1000; }
        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: rgba(15, 23, 42, 0.95); display: flex; justify-content: space-around; padding: 15px 0; border-top: 1px solid #1e293b; z-index: 1000; }
        .nav-item { text-align: center; color: var(--text-dim); font-size: 10px; flex: 1; cursor: pointer; }
        .nav-item.active { color: var(--accent); }
        .nav-item i { font-size: 1.4rem; margin-bottom: 3px; }

        .screen { display: none; animation: slideUp 0.4s cubic-bezier(0.165, 0.84, 0.44, 1); }
        .screen.active { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(30px); } }

        /* NODES GRID */
        .node-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; padding: 15px; }
        .node-box { background: var(--card-bg); border-radius: 20px; padding: 12px; border: 1px solid rgba(255,255,255,0.03); text-align: center; }
        .node-img { width: 100%; height: 80px; border-radius: 12px; object-fit: cover; margin-bottom: 8px; }

        /* ADMIN PANEL */
        #admin-panel { background: #000; min-height: 100vh; padding: 20px; display: none; color: white; position: absolute; top: 0; width: 100%; z-index: 9999; box-sizing: border-box; }
        .admin-section { background: #111; border-radius: 20px; padding: 15px; margin-bottom: 15px; border: 1px solid #222; }
        
        /* NOTIFICATION */
        #notif-box { position: fixed; bottom: 100px; left: 15px; z-index: 9999; background: var(--card-bg); border: 1px solid var(--accent); border-radius: 12px; padding: 10px 15px; display: flex; align-items: center; gap: 10px; transform: translateX(-200%); transition: 0.5s; }
        #notif-box.show { transform: translateX(0); }
    </style>
</head>
<body>

    <div id="maintenance-screen">
        <i class="fa-solid fa-screwdriver-wrench" style="font-size:4rem; color:var(--gold);"></i>
        <h2>System Tuning...</h2>
        <p>Nova is updating core nodes. Back shortly, sweetie!</p>
    </div>

    <div id="broadcast-bar"></div>

    <div id="notif-box">
        <div style="color:var(--accent);"><i class="fa-solid fa-circle-check"></i></div>
        <div id="notif-text" style="font-size:10px; font-weight:700;"></div>
    </div>

    <div id="auth-screen" style="min-height:100vh; display:flex; flex-direction:column; align-items:center; justify-content:center; background: radial-gradient(circle at top right, #1e1b4b, #020617); padding:20px;">
        <div style="text-align:center; margin-bottom:30px;">
            <i class="fa-solid fa-gem" style="font-size: 4.5rem; color: var(--accent); filter: drop-shadow(0 0 15px var(--accent));"></i>
            <h1 style="letter-spacing:12px; font-weight: 900; margin:10px 0 0 0;">NOVA</h1>
            <small style="color:var(--accent); letter-spacing:2px; font-weight:600;">INSTITUTIONAL GRADE</small>
        </div>

        <div class="glass-card" style="width:100%; max-width:400px; margin:0;">
            <div id="auth-header" style="text-align:center; margin-bottom:20px;">
                <h2 style="margin:0;">Welcome to Nova</h2>
                <small style="color:var(--text-dim);">Global Asset Security Protocol</small>
            </div>
            <input type="text" id="username" placeholder="Operator Identity">
            <input type="password" id="password" placeholder="Access Passkey">
            <button class="btn-prime" onclick="handleAuth()">INITIALIZE SESSION</button>
            <p onclick="toggleAuth()" id="toggleTxt" style="text-align:center; font-size:12px; color:var(--accent); cursor:pointer; margin-top:20px; font-weight:700;">CREATE NEW ACCOUNT</p>
        </div>
        
        <div style="margin-top:40px; text-align:center; font-size:10px; color:var(--text-dim); line-height:1.5;">
            <i class="fa-solid fa-shield-check"></i> 256-Bit SSL Secured<br>
            Official Corporate Registration #SG-944-NOVA
        </div>
    </div>

    <div id="app-main" style="display:none;">
        <div class="app-header">
            <div id="userDisp" style="background:var(--accent); color:white; padding:7px 16px; border-radius:14px; font-weight:900; font-size:12px; letter-spacing:1px;">USER</div>
            <div style="display:flex; gap:12px; align-items:center;">
                <button onclick="claimBonus()" style="background:var(--gold); border:none; border-radius:10px; padding:6px 12px; font-size:10px; font-weight:900; color:#000;">BONUS</button>
                <i class="fa-solid fa-power-off" onclick="location.reload()" style="color:var(--danger); font-size:1.3rem;"></i>
            </div>
        </div>

        <div id="home" class="screen active">
            <div class="glass-card" style="background: linear-gradient(135deg, #10B981, #064e3b); color:white; box-shadow: 0 15px 35px rgba(16,185,129,0.2);">
                <small style="text-transform:uppercase; font-weight:900; opacity:0.8; letter-spacing:1px;">Portfolio Valuation</small>
                <h1 style="font-size:3.5rem; margin:8px 0; font-weight:900;" id="mainBal">$0.00</h1>
                <div style="display:grid; grid-template-columns:1fr 1fr; gap:20px; border-top:1px solid rgba(255,255,255,0.15); padding-top:15px; margin-top:10px;">
                    <div><small style="display:block; font-size:10px; font-weight:700;">DAILY REVENUE</small><b id="dayYield" style="font-size:1.3rem;">+$0.00</b></div>
                    <div><small style="display:block; font-size:10px; font-weight:700;">TOTAL PROFIT</small><b id="totProfit" style="font-size:1.3rem;">$0.00</b></div>
                </div>
            </div>

            <h3 style="padding:10px 20px 0;"><i class="fa-solid fa-server" style="color:var(--accent);"></i> Cluster Nodes (20)</h3>
            <div id="nodeGrid" class="node-grid"></div>
        </div>

        <div id="wallet" class="screen">
            <div class="glass-card">
                <h3 style="margin-top:0; color:var(--accent);"><i class="fa-solid fa-arrow-down"></i> Institutional Deposit</h3>
                <select id="depMethod">
                    <option value="">Select Deposit Method</option>
                    <option value="USDT (BEP20)">USDT (BEP20)</option>
                    <option value="TRX (TRC20)">TRON (TRX)</option>
                    <option value="Binance Pay">Binance Pay ID</option>
                    <option value="Easypaisa/Jazzcash">Easypaisa / Jazzcash</option>
                </select>
                <input type="number" id="depAmt" placeholder="Investment Amount ($)">
                <input type="text" id="tid" placeholder="Transaction ID (TID)">
                <input type="text" id="depNum" placeholder="Sender Account Number">
                <div style="background:#020617; padding:12px; border-radius:15px; border:1px solid #334155;">
                    <small style="color:var(--text-dim);">Upload Transfer Proof:</small>
                    <input type="file" id="proof" style="margin:0; font-size:11px; padding:10px 0;">
                </div>
                <button class="btn-prime" style="margin-top:15px;" onclick="submitDep()">INITIALIZE DEPOSIT</button>
            </div>

            <div class="glass-card">
                <h3 style="margin-top:0; color:var(--danger);"><i class="fa-solid fa-arrow-up-from-bracket"></i> Assets Exit</h3>
                <select id="witMethod">
                    <option value="">Withdrawal Method</option>
                    <option value="USDT">USDT (BEP20)</option>
                    <option value="TRX">TRX (TRC20)</option>
                    <option value="Local Bank">Local Bank Transfer</option>
                </select>
                <input type="number" id="witAmt" placeholder="Minimum $10">
                <input type="text" id="witAcc" placeholder="Wallet Address / Account Number">
                <button class="btn-prime" style="background:var(--danger);" onclick="submitWit()">REQUEST WITHDRAWAL</button>
            </div>
        </div>

        <div id="info" class="screen">
            <div class="glass-card" style="border-left:5px solid var(--accent);">
                <h3 style="margin:0 0 10px 0;">About Nova Company</h3>
                <p style="font-size:12px; color:var(--text-dim); line-height:1.7;">
                    Nova is a globally decentralized asset protocol founded in 2021. We specialize in high-frequency node management and liquidity protection. Our servers are hosted in <b>Tier-4 Data Centers</b> in Singapore and London. 
                    Nova provides 100% capital insurance through our <b>Liquidity Protection Fund (LPF)</b>.
                </p>
            </div>
            <div class="glass-card">
                <h3 style="margin:0 0 10px 0;">Privacy & Policy</h3>
                <p style="font-size:11px; color:var(--text-dim); line-height:1.6;">
                    1. <b>Data:</b> All user data is encrypted via AES-256.<br>
                    2. <b>Withdrawals:</b> Processed within 1 to 12 working hours.<br>
                    3. <b>Security:</b> Multiple accounts from one IP are strictly prohibited.
                </p>
            </div>
            <div class="glass-card">
                <h3>Common FAQ</h3>
                <details style="margin-bottom:10px;"><summary style="font-size:13px; font-weight:700;">How long is node cycle?</summary><p style="font-size:11px;">Each node cluster runs for 30 days, providing daily profit.</p></details>
                <details><summary style="font-size:13px; font-weight:700;">Is my money trusted?</summary><p style="font-size:11px;">Yes, Nova is a registered corporate entity with institutional backing.</p></details>
            </div>
            <p style="text-align:center; font-size:10px; color:var(--text-dim);">© 2026 NOVA OMNIVERSAL | SINGAPORE</p>
        </div>

        <div class="bottom-nav">
            <div class="nav-item active" onclick="navTo('home', this)"><i class="fa-solid fa-chart-line"></i><br>DASHBOARD</div>
            <div class="nav-item" onclick="navTo('wallet', this)"><i class="fa-solid fa-wallet"></i><br>WALLET</div>
            <div class="nav-item" onclick="navTo('info', this)"><i class="fa-solid fa-shield-heart"></i><br>TRUST</div>
        </div>
    </div>

    <div id="admin-panel">
        <div style="display:flex; justify-content:space-between; align-items:center;">
            <h2><i class="fa-solid fa-crown" style="color:var(--gold);"></i> GOD COMMAND</h2>
            <button onclick="location.reload()" style="background:var(--danger); border:none; padding:10px 15px; border-radius:12px; color:white;">EXIT</button>
        </div>

        <div style="display:grid; grid-template-columns:1fr 1fr; gap:10px; margin:20px 0;">
            <div class="admin-section" style="text-align:center;"><small>ACTIVE USERS</small><h2 id="adm-users">0</h2></div>
            <div class="admin-section" style="text-align:center;"><small>SYSTEM NODES</small><h2>20</h2></div>
        </div>

        <div class="admin-section">
            <h4><i class="fa-solid fa-bolt"></i> SYSTEM CONTROL</h4>
            <button class="btn-prime" style="padding:10px; margin-bottom:10px; background:var(--elite);" onclick="window.setBroadcast()">SET BROADCAST MESSAGE</button>
            <button class="btn-prime" style="padding:10px; margin-bottom:10px; background:var(--gold);" onclick="window.toggleMaintenance()">TOGGLE MAINTENANCE MODE</button>
            <button class="btn-prime" style="padding:10px; background:var(--accent);" onclick="window.createPromo()">GENERATE PROMO CODE</button>
        </div>

        <div class="admin-section">
            <h4><i class="fa-solid fa-user-pen"></i> BALANCE INJUNCTION</h4>
            <input id="adj-user" placeholder="Target Username">
            <input id="adj-amt" type="number" placeholder="Amount (+ or -)">
            <button class="btn-prime" style="padding:10px;" onclick="window.adjustBalance()">EXECUTE INJUNCTION</button>
        </div>

        <div class="admin-section">
            <h4><i class="fa-solid fa-list-check"></i> PENDING REQUESTS</h4>
            <div id="adm-pending-list"></div>
        </div>

        <div class="admin-section">
            <h4><i class="fa-solid fa-users"></i> ALL USERS</h4>
            <div id="adm-user-list" style="font-size:11px;"></div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, collection, getDocs, query, where, updateDoc, deleteDoc } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        let user = null; let bal = 0; let isLogin = true;

        // FAKE POPUPS (20+ NAMES)
        const fakeNames = ["Ali", "Sara", "Zubair", "John", "Fatima", "Emma", "David", "Ayesha", "Viktor", "Zoe", "Musa", "Isabella", "Chen", "Yuki", "Kevin", "Elena", "Lucas", "Sofia", "Amir", "Linda"];
        setInterval(() => {
            const n = fakeNames[Math.floor(Math.random()*fakeNames.length)];
            const a = (Math.random()*400 + 10).toFixed(0);
            document.getElementById('notif-text').innerText = `${n} withdrew $${a} successfully!`;
            document.getElementById('notif-box').classList.add('show');
            setTimeout(() => document.getElementById('notif-box').classList.remove('show'), 4000);
        }, 18000);

        window.toggleAuth = () => { isLogin = !isLogin; document.getElementById('authBtn').innerText = isLogin ? "INITIALIZE SESSION" : "REGISTER OPERATOR"; };

        window.handleAuth = async () => {
            const u = document.getElementById('username').value.toLowerCase().trim();
            const p = document.getElementById('password').value;
            if(!u || !p) return;
            const s = await getDoc(doc(db, "users", u));
            if(isLogin) {
                if(s.exists() && !s.data().banned && s.data().password === p) { sessionStorage.setItem("nova_user", u); location.reload(); }
                else alert("Access Denied!");
            } else {
                if(s.exists()) return alert("Exists!");
                await setDoc(doc(db, "users", u), { password: p, balance: 0, dailyProfit: 0, totalProfit: 0, banned: false });
                alert("Operator Ready!"); toggleAuth();
            }
        };

        async function init() {
            // Check System Config
            const config = await getDoc(doc(db, "system", "config"));
            if(config.exists()) {
                if(config.data().maintenance) document.getElementById('maintenance-screen').style.display='flex';
                if(config.data().broadcast) {
                    const b = document.getElementById('broadcast-bar');
                    b.innerText = config.data().broadcast; b.style.display = 'block';
                }
            }

            user = sessionStorage.getItem("nova_user");
            if(!user) return;
            document.getElementById('auth-screen').style.display='none';
            document.getElementById('app-main').style.display='block';
            document.getElementById('userDisp').innerText = user.toUpperCase();
            
            const s = await getDoc(doc(db, "users", user));
            if(s.data().banned) { sessionStorage.clear(); location.reload(); }
            bal = s.data().balance;
            document.getElementById('mainBal').innerText = "$" + bal.toFixed(2);
            document.getElementById('dayYield').innerText = "+$" + (s.data().dailyProfit || 0).toFixed(2);
            document.getElementById('totProfit').innerText = "$" + (s.data().totalProfit || 0).toFixed(2);
            
            renderNodes();
        }

        function renderNodes() {
            const grid = document.getElementById('nodeGrid');
            for(let i=1; i<=20; i++) {
                const cost = i * 15;
                grid.innerHTML += `<div class="node-box">
                    <img src="https://images.unsplash.com/photo-1639762681485-074b7f938ba0?w=200" class="node-img">
                    <div style="font-size:10px; font-weight:900;">NODE CLUSTER ${i}</div>
                    <div style="color:var(--accent); font-size:11px; margin:4px 0;">$${cost}</div>
                    <button onclick="buyNode(${cost})" style="width:100%; border:none; background:var(--accent); color:white; font-size:9px; padding:5px; border-radius:5px;">ACTIVATE</button>
                </div>`;
            }
        }

        window.submitDep = async () => {
            const amt = document.getElementById('depAmt').value;
            const tid = document.getElementById('tid').value;
            const num = document.getElementById('depNum').value;
            const meth = document.getElementById('depMethod').value;
            const file = document.getElementById('proof').files[0];
            if(!file) return alert("Select Proof!");
            const reader = new FileReader();
            reader.onloadend = async () => {
                await setDoc(doc(db, "transactions", "tx_"+Date.now()), { 
                    user, type: "Deposit", amount: amt, tid, sender: num, method: meth, 
                    proof: reader.result, status: "Pending", date: new Date().toLocaleString() 
                });
                alert("Log Submitted!"); location.reload();
            };
            reader.readAsDataURL(file);
        };

        window.submitWit = async () => {
            const amt = document.getElementById('witAmt').value;
            const acc = document.getElementById('witAcc').value;
            const meth = document.getElementById('witMethod').value;
            if(bal < amt || amt < 10) return alert("Invalid!");
            await updateDoc(doc(db, "users", user), { balance: bal - amt });
            await setDoc(doc(db, "transactions", "wit_"+Date.now()), { 
                user, type: "Withdrawal", amount: amt, account: acc, method: meth, 
                status: "Pending", date: new Date().toLocaleString() 
            });
            alert("Requested!"); location.reload();
        };

        window.navTo = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('active'));
            el.classList.add('active');
        };

        // ADMIN GOD MODE
        document.getElementById('userDisp').onclick = async () => {
            window.ac = (window.ac || 0) + 1;
            if(window.ac === 5) {
                const p = prompt("GOD KEY:");
                if(p === "nova_boss_786") {
                    document.getElementById('admin-panel').style.display='block';
                    loadAdmin();
                } window.ac = 0;
            }
        };

        async function loadAdmin() {
            const users = await getDocs(collection(db, "users"));
            const txs = await getDocs(collection(db, "transactions"));
            document.getElementById('adm-users').innerText = users.size;
            
            const uList = document.getElementById('adm-user-list'); uList.innerHTML = "";
            users.forEach(u => {
                const d = u.data();
                uList.innerHTML += `<div style="border-bottom:1px solid #222; padding:8px; display:flex; justify-content:space-between;">
                    <span>${u.id} | $${d.balance}</span>
                    <button onclick="window.banUser('${u.id}', ${!d.banned})" style="color:red; background:none; border:1px solid red; font-size:9px;">${d.banned?'UNBAN':'BAN'}</button>
                </div>`;
            });

            const pList = document.getElementById('adm-pending-list'); pList.innerHTML = "";
            txs.forEach(t => {
                const d = t.data();
                if(d.status === 'Pending') {
                    pList.innerHTML += `<div class="glass-card" style="background:#111; font-size:10px;">
                        ${d.user} | ${d.type} | $${d.amount} | Via: ${d.method}<br>
                        TID: ${d.tid || 'N/A'} | Sender: ${d.sender || d.account}<br>
                        ${d.proof?`<img src="${d.proof}" width="100%" style="border-radius:10px; margin:10px 0;" onclick="window.open(this.src)">`:''}
                        <button class="btn-prime" style="padding:8px; background:green;" onclick="window.apprTx('${d.user}', '${d.amount}', '${t.id}', '${d.type}', 'Success')">APPROVE</button>
                        <button class="btn-prime" style="padding:8px; background:red; margin-top:5px;" onclick="window.apprTx('${d.user}', '${d.amount}', '${t.id}', '${d.type}', 'Rejected')">REJECT</button>
                    </div>`;
                }
            });
        }

        window.apprTx = async (uid, amt, tid, type, stat) => {
            if(stat === 'Success' && type === 'Deposit') {
                const r = doc(db, "users", uid); const s = await getDoc(r);
                await updateDoc(r, { balance: s.data().balance + parseFloat(amt) });
            }
            if(stat === 'Rejected' && type === 'Withdrawal') {
                const r = doc(db, "users", uid); const s = await getDoc(r);
                await updateDoc(r, { balance: s.data().balance + parseFloat(amt) });
            }
            await updateDoc(doc(db, "transactions", tid), { status: stat });
            alert(stat); loadAdmin();
        };

        window.adjustBalance = async () => {
            const u = document.getElementById('adj-user').value.trim();
            const a = parseFloat(document.getElementById('adj-amt').value);
            const r = doc(db, "users", u); const s = await getDoc(r);
            if(s.exists()) { await updateDoc(r, { balance: s.data().balance + a }); alert("Done!"); loadAdmin(); }
        };

        window.banUser = async (uid, s) => { await updateDoc(doc(db, "users", uid), { banned: s }); loadAdmin(); };
        window.toggleMaintenance = async () => {
            const s = await getDoc(doc(db, "system", "config"));
            await updateDoc(doc(db, "system", "config"), { maintenance: !s.data().maintenance }); alert("Toggled!");
        };
        window.setBroadcast = async () => {
            const m = prompt("Enter Broadcast:");
            await updateDoc(doc(db, "system", "config"), { broadcast: m }); alert("Sent!");
        };

        init();
    </script>
</body>
</html>
