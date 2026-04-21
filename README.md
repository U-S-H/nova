<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Institutional Asset Protocol</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap" rel="stylesheet">
    <style>
        :root { --primary: #020617; --accent: #10B981; --card-bg: #0F172A; --text-main: #F8FAFC; --text-dim: #94A3B8; --gold: #F59E0B; --danger: #EF4444; --elite: #8B5CF6; }
        body { margin: 0; font-family: 'Outfit', sans-serif; background: var(--primary); color: var(--text-main); -webkit-tap-highlight-color: transparent; overflow-x: hidden; padding-bottom: 100px; }
        
        .glass-card { background: var(--card-bg); border-radius: 28px; padding: 22px; margin: 15px; border: 1px solid rgba(255,255,255,0.05); box-shadow: 0 20px 40px rgba(0,0,0,0.6); }
        .ticker { background: #000; color: var(--accent); padding: 10px; font-size: 11px; font-weight: 600; text-align: center; border-bottom: 1px solid #1e293b; }
        
        .app-header { padding: 20px; background: rgba(15, 23, 42, 0.8); backdrop-filter: blur(20px); border-bottom: 1px solid rgba(255,255,255,0.05); display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 1000; }
        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: rgba(15, 23, 42, 0.95); backdrop-filter: blur(20px); display: flex; justify-content: space-around; padding: 15px 0; border-top: 1px solid #1e293b; z-index: 1000; }
        .nav-item { text-align: center; color: var(--text-dim); font-size: 11px; cursor: pointer; flex: 1; transition: 0.4s; }
        .nav-item i { font-size: 1.6rem; margin-bottom: 5px; display: block; }
        .nav-item.active { color: var(--accent); }

        .bal-hero { background: linear-gradient(135deg, #10B981, #064e3b); border-radius: 32px; padding: 35px; margin: 15px; position: relative; overflow: hidden; }
        .bal-hero i { position: absolute; right: -30px; bottom: -30px; font-size: 10rem; opacity: 0.1; color: white; }
        
        .node-grid { display: grid; gap: 15px; padding: 15px; }
        .node-card { background: #1e293b; border-radius: 24px; padding: 15px; border: 1px solid rgba(255,255,255,0.03); display: flex; flex-direction: column; gap: 12px; transition: 0.3s; position: relative; }
        .node-img { width: 100%; height: 140px; border-radius: 18px; object-fit: cover; background: #020617; }
        .elite-node { border: 1.5px solid var(--elite); background: linear-gradient(to bottom, #1e293b, #1e1b4b); }
        .badge { position: absolute; top: 10px; right: 15px; background: var(--elite); color: #fff; font-size: 9px; padding: 4px 12px; border-radius: 50px; font-weight: 900; }

        .btn-buy { width: 100%; padding: 14px; background: var(--accent); color: white; border: none; border-radius: 16px; font-weight: 800; cursor: pointer; }
        .input-wrap { position: relative; margin-bottom: 15px; }
        .input-wrap i { position: absolute; left: 18px; top: 18px; color: var(--text-dim); }
        input, select { width: 100%; padding: 16px 16px 16px 50px; background: #020617; border: 1px solid #334155; color: #fff; border-radius: 18px; box-sizing: border-box; outline: none; }

        .screen { display: none; animation: slideIn 0.4s ease; }
        .screen.active { display: block; }
        @keyframes slideIn { from { opacity:0; transform:translateY(20px); } to { opacity:1; transform:translateY(0); } }

        #auth-screen { min-height: 100vh; display: flex; flex-direction: column; align-items: center; justify-content: center; background: radial-gradient(circle at top right, #1e1b4b, #020617); padding: 20px; }
    </style>
</head>
<body>

    <div class="ticker"><marquee>NOVA WORLDWIDE v10.0 | TRUSTED INSTITUTIONAL PARTNER | NODES ACTIVATION LIVE | ISO 27001 | 256-BIT ENCRYPTION</marquee></div>

    <div id="auth-screen">
        <div style="text-align: center; margin-bottom: 25px;">
            <i class="fa-solid fa-gem" style="font-size: 4rem; color: var(--accent); filter: drop-shadow(0 0 15px var(--accent));"></i>
            <h1 style="letter-spacing:10px; font-weight:900; margin:5px 0;">NOVA</h1>
            <p style="font-size:10px; color:var(--text-dim); text-transform: uppercase;">Global Asset Infrastructure</p>
        </div>

        <div class="glass-card" style="width: 100%; max-width: 400px;">
            <h3 id="authTitle" style="margin-top: 0; text-align: center;"><i class="fa-solid fa-user-shield"></i> Operator Login</h3>
            <div class="input-wrap"><i class="fa-solid fa-user-tie"></i><input type="text" id="username" placeholder="Operator ID"></div>
            <div class="input-wrap"><i class="fa-solid fa-fingerprint"></i><input type="password" id="password" placeholder="Passkey"></div>
            <button class="btn-buy" onclick="handleAuth()" id="authBtn">INITIALIZE SESSION</button>
            <p onclick="toggleAuth()" id="toggleTxt" style="text-align:center; font-size:12px; color:var(--accent); margin-top:20px; cursor:pointer;">New Registration</p>
        </div>

        <div style="width: 100%; max-width: 400px; margin-top: 30px; color: var(--text-dim); font-size: 11px;">
            <div style="border-left: 2px solid var(--accent); padding-left: 15px; margin-bottom: 15px;">
                <b style="color:white;">Corporate Profile:</b> Registered digital management firm (Reg: #SG-94419). Serving 120+ countries with institutional liquidity.
            </div>
            <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px; text-align: center;">
                <div class="glass-card" style="margin:0; padding:10px;"><i class="fa-solid fa-certificate"></i> ISO Certified</div>
                <div class="glass-card" style="margin:0; padding:10px;"><i class="fa-solid fa-shield-halved"></i> Secured</div>
            </div>
        </div>
    </div>

    <div id="app-main" style="display:none;">
        <div class="app-header">
            <div style="display:flex; align-items:center; gap:12px;">
                <div style="background:var(--accent); width:40px; height:40px; border-radius:12px; display:flex; align-items:center; justify-content:center; color:#fff;"><i class="fa-solid fa-bolt"></i></div>
                <div><small style="color:var(--text-dim); font-size:10px; display:block;">STATUS: ONLINE</small><b id="userDisp">User</b></div>
            </div>
            <i class="fa-solid fa-power-off" onclick="location.reload()" style="color:var(--danger);"></i>
        </div>

        <div id="home" class="screen active">
            <div class="bal-hero">
                <i class="fa-solid fa-vault"></i>
                <small style="font-weight:600;">Total Assets Valuation</small>
                <h1 style="margin:10px 0; font-size:3rem; font-weight:900;" id="mainBal">$0.00</h1>
                <div style="display:flex; gap:12px; margin-top:20px;">
                    <button onclick="navTo('wallet')" style="flex:1; background:rgba(255,255,255,0.2); border:none; color:white; padding:15px; border-radius:20px; font-weight:900;">DEPOSIT</button>
                    <button onclick="navTo('wallet')" style="flex:1; background:white; border:none; color:var(--accent); padding:15px; border-radius:20px; font-weight:900;">WITHDRAW</button>
                </div>
            </div>

            <h3 style="padding:10px 25px 0;"><i class="fa-solid fa-layer-group" style="color:var(--accent);"></i> Node Market</h3>
            <div class="node-grid" id="nodeGrid"></div>
        </div>

        <div id="wallet" class="screen">
            <div class="glass-card">
                <h2 style="color:var(--accent); margin-top:0;">DEPOSIT ASSETS</h2>
                <div style="display:flex; gap:10px; margin-bottom:20px;">
                    <div onclick="copyWal('0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37')" style="flex:1; background:#020617; padding:15px; border-radius:20px; text-align:center; border:1px solid #1e293b;"><i class="fa-solid fa-shield-heart" style="color:#3375BB; font-size:1.8rem;"></i><br><small>TRUST</small></div>
                    <div onclick="copyWal('0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2')" style="flex:1; background:#020617; padding:15px; border-radius:20px; text-align:center; border:1px solid #1e293b;"><i class="fa-solid fa-mask" style="color:#F6851B; font-size:1.8rem;"></i><br><small>METAMASK</small></div>
                </div>
                <div class="input-wrap"><i class="fa-solid fa-dollar-sign"></i><input type="number" id="depAmt" placeholder="Amount"></div>
                <div class="input-wrap"><i class="fa-solid fa-hashtag"></i><input type="text" id="tid" placeholder="Transaction Hash/TID"></div>
                <div class="input-wrap"><i class="fa-solid fa-image"></i><input type="file" id="proof" accept="image/*"></div>
                <button class="btn-buy" onclick="submitDep()">SUBMIT PROOF</button>
            </div>

            <div class="glass-card">
                <h2 style="color:var(--danger); margin-top:0;">WITHDRAW</h2>
                <div class="input-wrap"><i class="fa-solid fa-wallet"></i><input type="number" id="witAmt" placeholder="Min $10"></div>
                <div class="input-wrap"><i class="fa-solid fa-id-card"></i><input type="text" id="witAcc" placeholder="Wallet Address"></div>
                <button class="btn-action" style="background:var(--danger); color:white; width:100%; padding:15px; border-radius:15px; border:none; font-weight:900;" onclick="submitWit()">CONFIRM EXIT</button>
            </div>
        </div>

        <div id="logs" class="screen">
            <h3 style="padding:20px;"><i class="fa-solid fa-history"></i> Activity Logs</h3>
            <div id="histContent" style="padding:0 15px;"></div>
        </div>

        <div id="info" class="screen">
            <div class="glass-card">
                <h3><i class="fa-solid fa-info-circle"></i> Institutional Info</h3>
                <p style="font-size:12px; color:var(--text-dim);">Global Office: 12 Marina Blvd, Singapore. Secured with AES-256 Protocol.</p>
                <div style="background:#020617; padding:15px; border-radius:15px; font-size:11px;">
                    <b style="color:var(--accent);">REGISTRATION:</b> #SG-94419<br>
                    <b style="color:var(--accent);">SECURITY:</b> Tier 4 Data Center
                </div>
            </div>
            <center id="admin-trigger" style="margin-top:60px; color:gray; font-size:11px;">&copy; 2026 NOVA ASSET GROUP | SECURE</center>
        </div>

        <div class="bottom-nav">
            <div class="nav-item active" onclick="navTo('home', this)"><i class="fa-solid fa-house"></i>HOME</div>
            <div class="nav-item" onclick="navTo('wallet', this)"><i class="fa-solid fa-wallet"></i>WALLET</div>
            <div class="nav-item" onclick="navTo('logs', this)"><i class="fa-solid fa-list-ul"></i>LOGS</div>
            <div class="nav-item" onclick="navTo('info', this)"><i class="fa-solid fa-shield-halved"></i>INFO</div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, collection, getDocs, query, where, updateDoc, deleteDoc } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        let currentUser = null;
        let userBalance = 0;
        let isLogin = true;

        window.toggleAuth = () => {
            isLogin = !isLogin;
            document.getElementById('authTitle').innerText = isLogin ? "Operator Login" : "Create Operator ID";
            document.getElementById('authBtn').innerText = isLogin ? "INITIALIZE SESSION" : "REGISTER OPERATOR";
            document.getElementById('toggleTxt').innerText = isLogin ? "New Registration" : "Back to Login";
        };

        window.handleAuth = async () => {
            const u = document.getElementById('username').value.toLowerCase().trim();
            const p = document.getElementById('password').value;
            if(!u || !p) return alert("Credentials required!");
            
            const snap = await getDoc(doc(db, "users", u));
            if(isLogin) {
                if(snap.exists() && snap.data().password === p) { sessionStorage.setItem("nova_user", u); location.reload(); }
                else alert("Access Denied!");
            } else {
                if(snap.exists()) return alert("User already exists!");
                await setDoc(doc(db, "users", u), { password: p, balance: 0, status: "Active" });
                alert("Operator Registered!"); toggleAuth();
            }
        };

        window.buyNode = async (cost, name) => {
            if(userBalance < cost) { alert("Insufficient Balance!"); navTo('wallet'); return; }
            if(confirm(`Confirm activation of ${name} for $${cost}?`)) {
                userBalance -= cost;
                await updateDoc(doc(db, "users", currentUser), { balance: userBalance });
                await setDoc(doc(db, "transactions", "buy_"+Date.now()), {
                    user: currentUser, type: "Activation", amount: cost, node: name, status: "Success", date: new Date().toLocaleString()
                });
                alert("Node Activated! 🚀"); location.reload();
            }
        };

        function renderNodes() {
            const grid = document.getElementById('nodeGrid');
            const imgs = [
                "https://images.unsplash.com/photo-1639762681485-074b7f938ba0?w=400&q=80",
                "https://images.unsplash.com/photo-1644088379091-d574269d422f?w=400&q=80",
                "https://images.unsplash.com/photo-1639322537228-f710d846310a?w=400&q=80"
            ];
            
            for(let i=1; i<=15; i++){
                let cost = i===1 ? 30 : Math.round(30 * Math.pow(1.3, i));
                if(cost > 2000) cost = 2000 + (i*50);
                grid.innerHTML += `<div class="node-card">
                    <img src="${imgs[i%3]}" class="node-img">
                    <div style="display:flex; justify-content:space-between;"><b>Nova-X${i}</b> <b>$${cost}</b></div>
                    <button class="btn-buy" onclick="buyNode(${cost}, 'Nova-X${i}')">ACTIVATE</button>
                </div>`;
            }
            
            const elite = [5000, 10000, 15000, 20000, 30000];
            elite.forEach((c, idx) => {
                grid.innerHTML += `<div class="node-card elite-node">
                    <span class="badge">ELITE</span>
                    <img src="${imgs[2]}" class="node-img">
                    <div style="display:flex; justify-content:space-between;"><b>TITAN-0${idx+1}</b> <b style="color:var(--elite)">$${c}</b></div>
                    <button class="btn-buy" style="background:var(--elite)" onclick="buyNode(${c}, 'Titan-0${idx+1}')">ACTIVATE</button>
                </div>`;
            });
        }

        window.submitDep = async () => {
            const amt = document.getElementById('depAmt').value;
            const tid = document.getElementById('tid').value;
            const file = document.getElementById('proof').files[0];
            if(!amt || !tid || !file) return alert("All fields required!");
            const reader = new FileReader();
            reader.onloadend = async () => {
                await setDoc(doc(db, "transactions", "dep_"+Date.now()), {
                    user: currentUser, type: "Deposit", amount: amt, tid: tid, proof: reader.result, status: "Pending", date: new Date().toLocaleString()
                });
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
            currentUser = sessionStorage.getItem("nova_user");
            if(!currentUser) return;
            document.getElementById('auth-screen').style.display='none';
            document.getElementById('app-main').style.display='block';
            document.getElementById('userDisp').innerText = currentUser;
            
            const uSnap = await getDoc(doc(db, "users", currentUser));
            userBalance = uSnap.data().balance || 0;
            document.getElementById('mainBal').innerText = "$" + userBalance.toFixed(2);
            renderNodes();

            const qSnap = await getDocs(query(collection(db, "transactions"), where("user", "==", currentUser)));
            const hist = document.getElementById('histContent');
            qSnap.forEach(t => {
                const d = t.data();
                hist.innerHTML += `<div class="glass-card" style="margin:10px 0;">
                    <div style="display:flex; justify-content:space-between;"><b>${d.type}</b> <b>$${d.amount}</b></div>
                    <small>${d.date} | <span style="color:var(--gold)">${d.status}</span></small>
                </div>`;
            });
        }

        /* EXTRA EXTRA ADMIN POWERS (5 CLICKS ON COPYRIGHT) */
        let adClicks = 0;
        document.getElementById('admin-trigger').onclick = async () => {
            adClicks++;
            if(adClicks === 5) {
                const p = prompt("ENTER MASTER KEY:");
                if(p === "nova_boss_786") {
                    const txs = await getDocs(collection(db, "transactions"));
                    let h = `<div style="background:#000; min-height:100vh; padding:20px; font-size:12px; color:white;">
                    <button onclick="location.reload()" style="background:red; color:white; border:none; padding:10px; border-radius:10px; margin-bottom:20px;">X EXIT PANEL</button>
                    <h1>ULTIMATE ADMIN TERMINAL</h1><div style="display:grid; gap:15px;">`;
                    txs.forEach(d => {
                        const data = d.data();
                        h += `<div class="glass-card" style="background:#111;">
                            <p>USER: ${data.user} | ${data.type} | $${data.amount}</p>
                            ${data.proof ? `<img src="${data.proof}" style="width:100px; border-radius:10px;" onclick="window.open(this.src)">` : ''}
                            <div style="margin-top:10px;">
                                <button onclick="window.modUserBal('${data.user}')" style="background:green; color:white; padding:8px; border-radius:8px; border:none;">ADD BALANCE</button>
                                <button onclick="window.delRecord('${d.id}')" style="background:red; color:white; padding:8px; border-radius:8px; border:none;">DELETE</button>
                            </div>
                        </div>`;
                    });
                    document.body.innerHTML = h + `</div></div>`;
                }
                adClicks = 0;
            }
        };

        window.modUserBal = async (uid) => {
            const val = prompt("Enter amount to ADD to " + uid);
            if(val) {
                const uRef = doc(db, "users", uid);
                const s = await getDoc(uRef);
                await updateDoc(uRef, { balance: (s.data().balance || 0) + parseFloat(val) });
                alert("Success!"); location.reload();
            }
        };

        window.delRecord = async (tid) => { if(confirm("Delete record?")) { await deleteDoc(doc(db, "transactions", tid)); location.reload(); } };
        window.copyWal = (a) => { navigator.clipboard.writeText(a); alert("Copied!"); };
        init();
    </script>
</body>
</html>
