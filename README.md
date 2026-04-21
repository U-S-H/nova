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
        
        .glass-card { background: var(--card-bg); border-radius: 28px; padding: 20px; margin: 15px; border: 1px solid rgba(255,255,255,0.05); box-shadow: 0 20px 40px rgba(0,0,0,0.6); }
        .ticker { background: #000; color: var(--accent); padding: 8px; font-size: 10px; font-weight: 600; text-align: center; border-bottom: 1px solid #1e293b; }
        
        .app-header { padding: 15px 20px; background: rgba(15, 23, 42, 0.8); backdrop-filter: blur(20px); border-bottom: 1px solid rgba(255,255,255,0.05); display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 1000; }
        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: rgba(15, 23, 42, 0.95); backdrop-filter: blur(20px); display: flex; justify-content: space-around; padding: 12px 0; border-top: 1px solid #1e293b; z-index: 1000; }
        .nav-item { text-align: center; color: var(--text-dim); font-size: 10px; cursor: pointer; flex: 1; transition: 0.3s; }
        .nav-item i { font-size: 1.4rem; margin-bottom: 4px; display: block; }
        .nav-item.active { color: var(--accent); transform: translateY(-3px); }

        .bal-hero { background: linear-gradient(135deg, #10B981, #064e3b); border-radius: 32px; padding: 30px; margin: 15px; position: relative; overflow: hidden; }
        .bal-hero i { position: absolute; right: -20px; bottom: -20px; font-size: 8rem; opacity: 0.1; color: white; }
        
        .node-grid { display: grid; gap: 15px; padding: 15px; }
        .node-card { background: #1e293b; border-radius: 24px; padding: 12px; border: 1px solid rgba(255,255,255,0.03); display: flex; flex-direction: column; gap: 10px; position: relative; }
        .node-img { width: 100%; height: 130px; border-radius: 18px; object-fit: cover; }
        
        .btn-prime { width: 100%; padding: 14px; background: var(--accent); color: white; border: none; border-radius: 16px; font-weight: 800; cursor: pointer; box-shadow: 0 8px 15px rgba(16, 185, 129, 0.2); }
        .input-wrap { position: relative; margin-bottom: 12px; }
        .input-wrap i { position: absolute; left: 16px; top: 16px; color: var(--text-dim); }
        input, select { width: 100%; padding: 16px 16px 16px 45px; background: #020617; border: 1px solid #334155; color: #fff; border-radius: 18px; box-sizing: border-box; outline: none; }

        .screen { display: none; animation: fadeIn 0.4s ease; }
        .screen.active { display: block; }
        @keyframes fadeIn { from { opacity:0; } to { opacity:1; } }

        #auth-screen { min-height: 100vh; display: flex; flex-direction: column; align-items: center; justify-content: center; background: radial-gradient(circle at top right, #1e1b4b, #020617); padding: 20px; }
    </style>
</head>
<body>

    <div class="ticker"><marquee>NOVA OMNIVERSAL v12.0 | SECURE DEPOSITS VIA USDT/TRX | DAILY BONUS SYSTEM ACTIVE | PROMO CODE PROTOCOL READY</marquee></div>

    <div id="auth-screen">
        <div style="text-align: center; margin-bottom: 20px;">
            <i class="fa-solid fa-gem" style="font-size: 3.5rem; color: var(--accent);"></i>
            <h1 style="letter-spacing:8px; font-weight:900; margin:5px 0;">NOVA</h1>
            <p style="font-size:10px; color:var(--text-dim);">INSTITUTIONAL GATEWAY</p>
        </div>
        <div class="glass-card" style="width: 100%; max-width: 380px;">
            <div class="input-wrap"><i class="fa-solid fa-user-tie"></i><input type="text" id="username" placeholder="Operator ID"></div>
            <div class="input-wrap"><i class="fa-solid fa-lock"></i><input type="password" id="password" placeholder="Passkey"></div>
            <button class="btn-prime" onclick="handleAuth()" id="authBtn">INITIALIZE SESSION</button>
            <p onclick="toggleAuth()" id="toggleTxt" style="text-align:center; font-size:11px; color:var(--accent); margin-top:15px; cursor:pointer;">NEW REGISTRATION</p>
        </div>
    </div>

    <div id="app-main" style="display:none;">
        <div class="app-header">
            <div style="display:flex; align-items:center; gap:10px;">
                <div style="background:var(--accent); width:35px; height:35px; border-radius:10px; display:flex; align-items:center; justify-content:center; color:#fff;"><i class="fa-solid fa-user-check"></i></div>
                <div><small style="color:var(--text-dim); font-size:9px; display:block;">OPERATOR</small><b id="userDisp" style="font-size:14px;">User</b></div>
            </div>
            <div style="display:flex; gap:10px;">
                <button onclick="claimDailyBonus()" style="background:var(--gold); border:none; border-radius:8px; padding:5px 10px; font-size:10px; font-weight:900; color:black;"><i class="fa-solid fa-gift"></i> BONUS</button>
                <i class="fa-solid fa-power-off" onclick="location.reload()" style="color:var(--danger); font-size:1.2rem;"></i>
            </div>
        </div>

        <div id="home" class="screen active">
            <div class="bal-hero">
                <i class="fa-solid fa-vault"></i>
                <small>Total Liquidity</small>
                <h1 style="font-size:2.8rem; font-weight:900; margin:5px 0;" id="mainBal">$0.00</h1>
                <div style="display:grid; grid-template-columns:1fr 1fr; gap:10px; margin-top:15px; border-top:1px solid rgba(255,255,255,0.1); padding-top:15px;">
                    <div><small style="display:block; opacity:0.8;">Daily Yield</small><b id="dayYield">+$0.00</b></div>
                    <div><small style="display:block; opacity:0.8;">Total Profit</small><b id="totProfit">$0.00</b></div>
                </div>
            </div>

            <div class="glass-card" style="margin-top:-5px; padding:15px; border:1px solid var(--elite);">
                <div style="display:flex; gap:10px;">
                    <input type="text" id="promoInput" placeholder="Enter Promo Code" style="padding:12px 12px 12px 15px; font-size:12px;">
                    <button onclick="applyPromo()" style="background:var(--elite); border:none; color:white; border-radius:15px; padding:0 15px; font-weight:900;">APPLY</button>
                </div>
            </div>

            <div class="glass-card" style="padding:15px;">
                <small style="color:var(--text-dim);">Your Referral Link</small>
                <div style="display:flex; gap:10px; margin-top:5px;">
                    <input type="text" id="refLink" readonly style="padding:10px; font-size:10px; background:#000;">
                    <button onclick="copyRef()" style="background:var(--accent); border:none; color:white; border-radius:10px; padding:0 15px;"><i class="fa-solid fa-copy"></i></button>
                </div>
            </div>

            <h3 style="padding:10px 20px 0;"><i class="fa-solid fa-microchip"></i> Node Clusters</h3>
            <div class="node-grid" id="nodeGrid"></div>
        </div>

        <div id="wallet" class="screen">
            <div class="glass-card">
                <h2 style="color:var(--accent); margin-top:0;">DEPOSIT METHOD</h2>
                <p style="font-size:11px; color:var(--text-dim);">Select an institutional wallet to fund your account via USDT (BEP20) or TRX (TRC20).</p>
                <div style="display:grid; grid-template-columns: 1fr 1fr; gap:10px; margin-bottom:15px;">
                    <div onclick="copyWal('0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37')" style="background:#020617; padding:15px; border-radius:20px; text-align:center; border:1px solid #1e293b;">
                        <i class="fa-solid fa-shield-heart" style="color:#3375BB; font-size:1.5rem;"></i><br><small>TRUST</small>
                    </div>
                    <div onclick="copyWal('0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2')" style="background:#020617; padding:15px; border-radius:20px; text-align:center; border:1px solid #1e293b;">
                        <i class="fa-solid fa-mask" style="color:#F6851B; font-size:1.5rem;"></i><br><small>METAMASK</small>
                    </div>
                </div>
                <input type="number" id="depAmt" placeholder="Amount (USD)" style="margin-bottom:10px;">
                <input type="text" id="tid" placeholder="Transaction TID" style="margin-bottom:10px;">
                <input type="file" id="proof" style="padding:12px;">
                <button class="btn-prime" style="margin-top:10px;" onclick="submitDep()">SUBMIT DEPOSIT</button>
            </div>

            <div class="glass-card">
                <h2 style="color:var(--danger); margin-top:0;">WITHDRAWAL</h2>
                <input type="number" id="witAmt" placeholder="Min $10" style="margin-bottom:10px;">
                <input type="text" id="witAcc" placeholder="Wallet Address (USDT/TRX)" style="margin-bottom:10px;">
                <button class="btn-prime" style="background:var(--danger);" onclick="submitWit()">REQUEST WITHDRAWAL</button>
            </div>
        </div>

        <div id="logs" class="screen"><div id="histContent" style="padding:15px;"></div></div>

        <div class="bottom-nav">
            <div class="nav-item active" onclick="navTo('home', this)"><i class="fa-solid fa-house"></i>HOME</div>
            <div class="nav-item" onclick="navTo('wallet', this)"><i class="fa-solid fa-wallet"></i>WALLET</div>
            <div class="nav-item" onclick="navTo('logs', this)"><i class="fa-solid fa-list"></i>LOGS</div>
            <div class="nav-item" onclick="navTo('info', this)"><i class="fa-solid fa-shield"></i>INFO</div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, collection, getDocs, query, where, updateDoc, deleteDoc } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        let user = null;
        let bal = 0;
        let isLogin = true;

        window.toggleAuth = () => {
            isLogin = !isLogin;
            document.getElementById('authBtn').innerText = isLogin ? "INITIALIZE SESSION" : "REGISTER OPERATOR";
            document.getElementById('toggleTxt').innerText = isLogin ? "NEW REGISTRATION" : "BACK TO LOGIN";
        };

        window.handleAuth = async () => {
            const u = document.getElementById('username').value.toLowerCase().trim();
            const p = document.getElementById('password').value;
            if(!u || !p) return alert("Fields required!");
            const snap = await getDoc(doc(db, "users", u));
            if(isLogin) {
                if(snap.exists() && snap.data().password === p) { sessionStorage.setItem("nova_user", u); location.reload(); }
                else alert("Invalid Key!");
            } else {
                if(snap.exists()) return alert("Exists!");
                await setDoc(doc(db, "users", u), { password: p, balance: 0, lastBonus: 0, dailyProfit: 0, totalProfit: 0 });
                alert("Success! Login now."); toggleAuth();
            }
        };

        window.claimDailyBonus = async () => {
            const snap = await getDoc(doc(db, "users", user));
            const now = new Date().toDateString();
            if(snap.data().lastBonus === now) return alert("Already claimed today, sweetie!");
            const reward = 0.50;
            await updateDoc(doc(db, "users", user), { balance: bal + reward, lastBonus: now });
            alert("Daily Reward +$0.50 Added!"); location.reload();
        };

        window.applyPromo = async () => {
            const code = document.getElementById('promoInput').value.trim();
            const pSnap = await getDoc(doc(db, "promos", code));
            if(pSnap.exists() && pSnap.data().status === "Active") {
                const reward = pSnap.data().value;
                await updateDoc(doc(db, "users", user), { balance: bal + reward });
                alert(`Promo Applied! +$${reward} Added.`); location.reload();
            } else alert("Invalid or Expired Code!");
        };

        function renderNodes() {
            const grid = document.getElementById('nodeGrid');
            const imgs = ["https://images.unsplash.com/photo-1639762681485-074b7f938ba0?w=400", "https://images.unsplash.com/photo-1644088379091-d574269d422f?w=400"];
            for(let i=1; i<=15; i++){
                let cost = i===1 ? 30 : Math.round(30 * Math.pow(1.3, i));
                grid.innerHTML += `<div class="node-card"><img src="${imgs[i%2]}" class="node-img"><b>Nova-X${i}</b><p style="margin:0; font-size:12px;">Cost: $${cost}</p><button class="btn-prime" onclick="buyNode(${cost}, 'Nova-X${i}')">ACTIVATE</button></div>`;
            }
        }

        window.buyNode = async (cost, name) => {
            if(bal < cost) return alert("Insufficient Liquidity!");
            await updateDoc(doc(db, "users", user), { balance: bal - cost });
            await setDoc(doc(db, "transactions", "buy_"+Date.now()), { user, type: "Activation", amount: cost, node: name, status: "Success", date: new Date().toLocaleString() });
            alert(name + " Active!"); location.reload();
        };

        window.submitDep = async () => {
            const amt = document.getElementById('depAmt').value;
            const tid = document.getElementById('tid').value;
            const file = document.getElementById('proof').files[0];
            if(!amt || !tid || !file) return alert("Fill all!");
            const reader = new FileReader();
            reader.onloadend = async () => {
                await setDoc(doc(db, "transactions", "dep_"+Date.now()), { user, type: "Deposit", amount: amt, tid, proof: reader.result, status: "Pending", date: new Date().toLocaleString() });
                alert("Deposit Logged!"); location.reload();
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
            document.getElementById('userDisp').innerText = user;
            document.getElementById('refLink').value = window.location.href + "?ref=" + user;
            const snap = await getDoc(doc(db, "users", user));
            bal = snap.data().balance;
            document.getElementById('mainBal').innerText = "$" + bal.toFixed(2);
            document.getElementById('dayYield').innerText = "+$" + (snap.data().dailyProfit || 0).toFixed(2);
            document.getElementById('totProfit').innerText = "$" + (snap.data().totalProfit || 0).toFixed(2);
            renderNodes();
        }

        /* GOD-MODE ADMIN CONTROL */
        let clicks = 0;
        document.body.addEventListener('click', async (e) => {
            if(e.target.id === 'userDisp') {
                clicks++;
                if(clicks === 5) {
                    const pass = prompt("ADMIN KEY:");
                    if(pass === "nova_boss_786") {
                        const users = await getDocs(collection(db, "users"));
                        const txs = await getDocs(collection(db, "transactions"));
                        let h = `<div style="background:#000; color:white; min-height:100vh; padding:20px; font-size:12px;">
                        <button onclick="location.reload()" style="background:red; color:white; border:none; padding:10px; margin-bottom:20px;">X EXIT</button>
                        <h2>ADMIN GOD-PANEL</h2><hr>`;
                        
                        h += `<h3>CREATE PROMO CODE</h3>
                        <input id="pCode" placeholder="Code"> <input id="pVal" type="number" placeholder="Value">
                        <button onclick="window.genPromo()">GENERATE</button><hr><h3>TRANSACTIONS</h3>`;
                        
                        txs.forEach(t => {
                            const d = t.data();
                            h += `<div class="glass-card">USER: ${d.user} | ${d.type} | $${d.amount} | ${d.status}<br>
                            ${d.proof ? `<img src="${d.proof}" width="80" onclick="window.open(this.src)">` : ''}
                            <button onclick="window.approveDep('${d.user}', '${d.amount}', '${t.id}')">APPROVE</button>
                            <button onclick="window.delTx('${t.id}')">DEL</button></div>`;
                        });
                        document.body.innerHTML = h + `</div>`;
                    } clicks = 0;
                }
            }
        });

        window.genPromo = async () => {
            const c = document.getElementById('pCode').value;
            const v = parseFloat(document.getElementById('pVal').value);
            await setDoc(doc(db, "promos", c), { value: v, status: "Active" });
            alert("Promo Created!");
        };

        window.approveDep = async (uid, amt, tid) => {
            const r = doc(db, "users", uid);
            const s = await getDoc(r);
            await updateDoc(r, { balance: (s.data().balance || 0) + parseFloat(amt) });
            await updateDoc(doc(db, "transactions", tid), { status: "Success" });
            alert("Approved!"); location.reload();
        };

        window.delTx = async (id) => { await deleteDoc(doc(db, "transactions", id)); location.reload(); };
        window.copyWal = (a) => { navigator.clipboard.writeText(a); alert("Copied!"); };
        init();
    </script>
</body>
</html>
