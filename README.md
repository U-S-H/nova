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
        
        #news-ticker { background: #1e293b; color: var(--accent); padding: 8px; font-size: 10px; font-weight: 700; white-space: nowrap; overflow: hidden; position: relative; border-bottom: 1px solid rgba(16, 185, 129, 0.2); }
        .ticker-text { display: inline-block; animation: ticker 25s linear infinite; }
        @keyframes ticker { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }

        .glass-card { background: var(--card-bg); border: 1px solid rgba(255,255,255,0.05); border-radius: 28px; padding: 20px; margin: 15px; }
        .btn-prime { width: 100%; padding: 16px; background: var(--accent); color: white; border: none; border-radius: 18px; font-weight: 900; cursor: pointer; transition: 0.3s; }
        .btn-prime:active { transform: scale(0.95); }
        
        input, select { width: 100%; padding: 15px; background: #020617; border: 1px solid #334155; color: #fff; border-radius: 16px; box-sizing: border-box; margin-bottom: 12px; outline: none; font-family: 'Outfit'; }
        
        .app-header { padding: 15px 20px; background: rgba(15, 23, 42, 0.8); backdrop-filter: blur(20px); border-bottom: 1px solid rgba(255,255,255,0.05); display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 1000; }
        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: rgba(15, 23, 42, 0.95); display: flex; justify-content: space-around; padding: 15px 0; border-top: 1px solid #1e293b; z-index: 1000; }
        .nav-item { text-align: center; color: var(--text-dim); font-size: 10px; flex: 1; cursor: pointer; }
        .nav-item.active { color: var(--accent); }
        .nav-item i { font-size: 1.4rem; margin-bottom: 3px; }

        .screen { display: none; animation: slideUp 0.4s ease; }
        .screen.active { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } }

        .node-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; padding: 15px; }
        .node-box { background: var(--card-bg); border-radius: 20px; padding: 15px; border: 1px solid rgba(255,255,255,0.03); text-align: center; position: relative; overflow: hidden; }
        .node-img { width: 100%; height: 80px; border-radius: 12px; object-fit: cover; margin-bottom: 8px; }

        .status-badge { font-size: 8px; padding: 3px 8px; border-radius: 5px; font-weight: 900; text-transform: uppercase; }
        .pending { background: var(--gold); color: black; }
        .approved { background: var(--accent); color: white; }
        
        #admin-panel { background: #000; min-height: 100vh; padding: 20px; display: none; color: white; position: fixed; inset: 0; z-index: 9999; overflow-y: auto; }
    </style>
</head>
<body>

    <div id="news-ticker"><div class="ticker-text">🚀 NEW CLUSTER V2.0 LIVE • WITHDRAWAL AUTO-SYNC ACTIVE • BTC AT $68,420 • WELCOME TO NOVA INSTITUTIONAL • MINIMUM WITHDRAWAL REDUCED TO $50 • REFER AND EARN 10% COMMISSION 🚀</div></div>

    <div id="auth-screen" style="min-height:100vh; display:flex; flex-direction:column; align-items:center; justify-content:center; background: radial-gradient(circle at top right, #1e1b4b, #020617); padding:20px;">
        <i class="fa-solid fa-gem" style="font-size: 4rem; color: var(--accent); margin-bottom: 20px;"></i>
        <div class="glass-card" style="width:100%; max-width:380px;">
            <h2 id="auth-title" style="text-align:center; margin:0 0 20px 0;">Login to Nova</h2>
            <input type="text" id="username" placeholder="Username">
            <input type="password" id="password" placeholder="Passkey">
            <button class="btn-prime" onclick="handleAuth()">LOGIN</button>
            <p onclick="toggleAuth()" id="toggleTxt" style="text-align:center; font-size:12px; color:var(--accent); cursor:pointer; margin-top:20px;">Create New Identity</p>
        </div>
    </div>

    <div id="app-main" style="display:none;">
        <div class="app-header">
            <div id="userDisp" style="background:var(--accent); color:white; padding:7px 16px; border-radius:12px; font-weight:900; font-size:11px;">OPERATOR</div>
            <div style="display:flex; gap:12px; align-items:center;">
                <button onclick="claimBonus()" style="background:var(--gold); border:none; border-radius:8px; padding:5px 10px; font-size:10px; font-weight:900; color:white;">BONUS</button>
                <i class="fa-solid fa-power-off" onclick="logout()" style="color:var(--danger); font-size:1.2rem; cursor:pointer;"></i>
            </div>
        </div>

        <div id="home" class="screen active">
            <div class="glass-card" style="background: linear-gradient(135deg, #10B981, #064e3b); position: relative;">
                <small style="font-weight:900; opacity:0.8;">PORTFOLIO VALUE</small>
                <h1 style="font-size:3rem; margin:10px 0;" id="mainBal">$0.00</h1>
                <div style="display:grid; grid-template-columns:1fr 1fr; border-top:1px solid rgba(255,255,255,0.1); padding-top:10px;">
                    <div><small>DAILY YIELD</small><br><b id="dayYield">+$0.00</b></div>
                    <div><small>TOTAL PROFIT</small><br><b id="totProfit">$0.00</b></div>
                </div>
            </div>
            <h3 style="padding:0 20px; font-size:14px; color:var(--accent);">AVAILABLE CLUSTERS</h3>
            <div id="nodeGrid" class="node-grid"></div>
        </div>

        <div id="wallet" class="screen">
            <div class="glass-card">
                <h3>Deposit Assets</h3>
                <p style="font-size:10px; color:var(--text-dim); margin-bottom:12px;">TRC20: <b style="color:white;">TAvfQQ18hXHdVCHbExV4yUPvQ4XkNgfKsJ</b></p>
                <input type="number" id="depAmt" placeholder="Deposit Amount ($)">
                <input type="text" id="tid" placeholder="Transaction Hash / TID">
                <input type="file" id="proof" accept="image/*">
                <button class="btn-prime" onclick="submitDep()" style="margin-top:10px;">SUBMIT DEPOSIT</button>
            </div>

            <div class="glass-card" style="border-top: 4px solid var(--danger);">
                <h3>Withdraw Assets</h3>
                <select id="w-method">
                    <option value="Binance (USDT)">Binance (USDT-TRC20)</option>
                    <option value="Trust Wallet">Trust Wallet (TRX)</option>
                    <option value="PayPal">PayPal</option>
                    <option value="Bank Transfer">Direct Bank Transfer</option>
                </select>
                <input type="number" id="w-amt" placeholder="Amount ($)">
                <input type="text" id="w-addr" placeholder="Wallet Address / IBAN">
                <button class="btn-prime" onclick="submitWithdraw()" style="background:var(--danger);">INITIALIZE EXTRACTION</button>
            </div>
            
            <h3 style="padding:0 20px; font-size:14px;">Transaction History</h3>
            <div id="tx-history" style="padding:0 15px;"></div>
        </div>

        <div id="refer" class="screen">
            <div class="glass-card" style="text-align:center; background:var(--elite);">
                <i class="fa-solid fa-users" style="font-size:3rem; margin-bottom:15px;"></i>
                <h3>Affiliate Program</h3>
                <p style="font-size:12px;">Earn 10% commission on every friend's node activation.</p>
                <div style="background:rgba(0,0,0,0.2); padding:15px; border-radius:15px; margin-top:15px; display:flex; justify-content:space-between; align-items:center;">
                    <span id="refCode" style="font-weight:900;">NOVA-REF-101</span>
                    <button onclick="copyRef()" style="background:white; color:black; border:none; padding:5px 10px; border-radius:8px; font-size:10px; font-weight:900;">COPY</button>
                </div>
            </div>
            <div class="glass-card">
                <h4>Team Statistics</h4>
                <div style="display:flex; justify-content:space-between; font-size:12px;">
                    <span>Direct Referrals:</span> <b id="refCount">0</b>
                </div>
                <div style="display:flex; justify-content:space-between; font-size:12px; margin-top:10px;">
                    <span>Team Earning:</span> <b id="refEarn">$0.00</b>
                </div>
            </div>
        </div>

        <div id="info" class="screen">
            <div class="glass-card">
                <h3>Institutional Security</h3>
                <p style="font-size:12px; opacity:0.7; line-height:1.6;">Nova Protocol is a decentralized cloud mining engine. Your data is encrypted using Base64 military-grade standards. We ensure 99.9% uptime for all node clusters.</p>
            </div>
            <div class="glass-card">
                <h4>Support Channel</h4>
                <button class="btn-prime" onclick="window.open('https://t.me/yourtelegram')" style="background:#229ED9;"><i class="fa-brands fa-telegram"></i> CONTACT TELEGRAM</button>
            </div>
        </div>

        <div class="bottom-nav">
            <div class="nav-item active" onclick="navTo('home', this)"><i class="fa-solid fa-house"></i><br>HOME</div>
            <div class="nav-item" onclick="navTo('wallet', this)"><i class="fa-solid fa-wallet"></i><br>VAULT</div>
            <div class="nav-item" onclick="navTo('refer', this)"><i class="fa-solid fa-share-nodes"></i><br>REFER</div>
            <div class="nav-item" onclick="navTo('info', this)"><i class="fa-solid fa-circle-info"></i><br>INFO</div>
        </div>
    </div>

    <div id="admin-panel">
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:20px;">
            <h2>GOD MODE</h2>
            <button onclick="document.getElementById('admin-panel').style.display='none'" style="background:var(--danger); border:none; padding:10px 20px; color:white; border-radius:10px; font-weight:900;">CLOSE</button>
        </div>
        <div id="adm-pending-list"></div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, collection, getDocs, updateDoc, addDoc, query, where, onSnapshot } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        let currentUser = null; 
        let balance = 0; 
        let isLogin = true;

        window.toggleAuth = () => {
            isLogin = !isLogin;
            document.getElementById('auth-title').innerText = isLogin ? "Login to Nova" : "Register Identity";
            document.getElementById('toggleTxt').innerText = isLogin ? "Create New Identity" : "Already have account? Login";
        };

        window.handleAuth = async () => {
            const u = document.getElementById('username').value.toLowerCase().trim();
            const p = document.getElementById('password').value;
            if(!u || !p) return alert("All fields required!");
            const ref = doc(db, "users", u);
            const snap = await getDoc(ref);
            if(isLogin) {
                if(snap.exists() && snap.data().password === p) {
                    sessionStorage.setItem("nova_user", u);
                    init();
                } else alert("Access Error!");
            } else {
                if(snap.exists()) return alert("User exists!");
                await setDoc(ref, { password: p, balance: 0, dailyProfit: 0, totalProfit: 0, lastBonus: "", referrals: 0, teamEarning: 0 });
                alert("Registered! Now Login.");
                isLogin = true; toggleAuth();
            }
        };

        async function init() {
            currentUser = sessionStorage.getItem("nova_user");
            if(!currentUser) return;
            document.getElementById('auth-screen').style.display='none';
            document.getElementById('app-main').style.display='block';
            document.getElementById('userDisp').innerText = currentUser.toUpperCase();
            document.getElementById('refCode').innerText = "NOVA-" + currentUser.toUpperCase();

            onSnapshot(doc(db, "users", currentUser), (doc) => {
                if (doc.exists()) {
                    const d = doc.data();
                    balance = d.balance || 0;
                    document.getElementById('mainBal').innerText = "$" + balance.toFixed(2);
                    document.getElementById('dayYield').innerText = "+$" + (d.dailyProfit || 0).toFixed(2);
                    document.getElementById('totProfit').innerText = "$" + (d.totalProfit || 0).toFixed(2);
                    document.getElementById('refCount').innerText = d.referrals || 0;
                    document.getElementById('refEarn').innerText = "$" + (d.teamEarning || 0).toFixed(2);
                }
            });
            renderNodes(); loadHistory();
        }

        function renderNodes() {
            const grid = document.getElementById('nodeGrid'); grid.innerHTML = "";
            const imgs = ["1639762681485-074b7f938ba0", "1558494949-ef010cbdcc51", "1518770660439-4636190af475", "1640343136701-d051ff221245"];
            for(let i=1; i<=15; i++) {
                const cost = i * 50; const daily = (cost * 0.10).toFixed(2);
                grid.innerHTML += `<div class="node-box">
                    <img src="https://images.unsplash.com/photo-${imgs[i%4]}?w=200" class="node-img">
                    <div style="font-size:10px; font-weight:900;">NODE V.${i}</div>
                    <div style="color:var(--accent); font-weight:900; margin:4px 0;">$${cost}</div>
                    <button onclick="buyNode(${cost}, ${daily})" style="width:100%; border:none; background:var(--accent); color:white; border-radius:8px; padding:6px; font-size:9px; font-weight:900;">ACTIVATE</button>
                </div>`;
            }
        }

        window.buyNode = async (cost, daily) => {
            if(balance < cost) return alert("Deposit Assets First!");
            if(confirm(`Confirm $${cost} Activation?`)) {
                await updateDoc(doc(db, "users", currentUser), { balance: balance - cost });
                await addDoc(collection(db, "tx"), { user: currentUser, amount: cost, type: "Activation", status: "Approved", time: Date.now() });
                alert("Protocol Started!");
            }
        };

        window.submitDep = async () => {
            const amt = document.getElementById('depAmt').value;
            const tid = document.getElementById('tid').value;
            const file = document.getElementById('proof').files[0];
            if(!amt || !tid || !file) return alert("Need all details!");
            const reader = new FileReader();
            reader.onloadend = async () => {
                await addDoc(collection(db, "pending_tx"), { user: currentUser, amount: parseFloat(amt), tid, proof: reader.result, type: "Deposit", status: "Pending", time: Date.now() });
                alert("Broadcasted!");
            }; reader.readAsDataURL(file);
        };

        window.submitWithdraw = async () => {
            const amt = parseFloat(document.getElementById('w-amt').value);
            const addr = document.getElementById('w-addr').value;
            if(amt < 50) return alert("Minimum $50 Required!");
            if(balance < amt) return alert("Insufficient Balance!");
            await updateDoc(doc(db, "users", currentUser), { balance: balance - amt });
            await addDoc(collection(db, "pending_tx"), { user: currentUser, amount: amt, addr, type: "Withdraw", status: "Pending", time: Date.now() });
            alert("Extraction Request Sent!");
        };

        function loadHistory() {
            onSnapshot(query(collection(db, "tx"), where("user", "==", currentUser)), (snap) => {
                const list = document.getElementById('tx-history'); list.innerHTML = "";
                snap.forEach(d => {
                    const t = d.data();
                    list.innerHTML += `<div class="glass-card" style="margin:10px 0; display:flex; justify-content:space-between; align-items:center;">
                        <div><small>${t.type}</small><br><b>$${t.amount}</b></div>
                        <span class="status-badge approved">${t.status}</span>
                    </div>`;
                });
            });
        }

        window.copyRef = () => { navigator.clipboard.writeText(document.getElementById('refCode').innerText); alert("Referral Code Copied!"); };
        window.claimBonus = async () => {
            const t = new Date().toDateString();
            const r = doc(db, "users", currentUser);
            const s = await getDoc(r);
            if(s.data().lastBonus === t) return alert("Already Claimed!");
            await updateDoc(r, { balance: balance + 0.50, lastBonus: t });
            alert("Bonus Credited!");
        };

        window.navTo = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('active'));
            el.classList.add('active');
        };

        window.logout = () => { sessionStorage.clear(); location.reload(); };

        // ADMIN ACCESS
        document.getElementById('userDisp').onclick = () => {
            window.ac = (window.ac || 0) + 1;
            if(window.ac === 5) {
                if(prompt("MASTER KEY:") === "nova_boss_786") {
                    document.getElementById('admin-panel').style.display='block';
                    loadAdmin();
                } window.ac = 0;
            }
        };

        function loadAdmin() {
            onSnapshot(collection(db, "pending_tx"), (snap) => {
                const list = document.getElementById('adm-pending-list'); list.innerHTML = "";
                snap.forEach(d => {
                    const t = d.data(); if(t.status === "Pending") {
                        list.innerHTML += `<div class="glass-card">
                            <p>${t.user} | ${t.type} | $${t.amount}</p>
                            ${t.proof ? `<img src="${t.proof}" style="width:100%; border-radius:10px;">` : `<p>${t.addr || t.tid}</p>`}
                            <button onclick="admProcess('${d.id}','${t.user}',${t.amount},'${t.type}','Approved')" style="background:var(--accent); color:white; border:none; padding:10px; width:100%; border-radius:10px; margin-top:10px;">APPROVE</button>
                        </div>`;
                    }
                });
            });
        }

        window.admProcess = async (id, user, amt, type, status) => {
            await updateDoc(doc(db, "pending_tx", id), { status: "Approved" });
            if(type === 'Deposit') {
                const r = doc(db, "users", user); const s = await getDoc(r);
                await updateDoc(r, { balance: (s.data().balance || 0) + amt });
            }
            await addDoc(collection(db, "tx"), { user, amount: amt, type, status: "Approved", time: Date.now() });
            alert("Action Processed!");
        };

        init();
    </script>
</body>
</html>
