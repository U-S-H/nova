<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Institutional Digital Terminal</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --accent: #00FFA3; --bg: #050505; --card: #0f0f0f; --text: #FFFFFF; --dim: #8b949e; --danger: #ff4d4d; }
        body { margin: 0; font-family: 'Plus Jakarta Sans', sans-serif; background: var(--bg); color: var(--text); overflow-x: hidden; -webkit-tap-highlight-color: transparent; }
        
        .glass { background: rgba(15, 15, 15, 0.98); border: 1px solid rgba(212, 175, 55, 0.2); border-radius: 30px; padding: 25px; backdrop-filter: blur(20px); }
        .input-nova { width: 100%; padding: 18px; background: #000; border: 1px solid #222; color: #fff; border-radius: 18px; margin-bottom: 12px; outline: none; box-sizing: border-box; font-family: inherit; }
        .btn-nova { background: linear-gradient(135deg, var(--gold), #F2CC60); color: #000; border: none; padding: 18px; border-radius: 18px; font-weight: 800; cursor: pointer; text-transform: uppercase; width: 100%; transition: 0.3s; box-shadow: 0 10px 20px rgba(212,175,55,0.1); }
        .btn-nova:active { transform: scale(0.96); }

        /* Auth Screen */
        #auth-screen { height: 100vh; display: flex; flex-direction: column; align-items: center; justify-content: center; padding: 30px; background: radial-gradient(circle at top, #1a1a00, #050505); }
        .auth-toggle { display: flex; gap: 10px; margin-bottom: 25px; background: #000; padding: 5px; border-radius: 15px; width: 100%; max-width: 400px; box-sizing: border-box; }
        .auth-toggle button { flex: 1; border: none; padding: 10px; border-radius: 10px; font-weight: 800; color: #fff; background: transparent; cursor: pointer; }
        .auth-toggle .active { background: var(--gold); color: #000; }

        /* Dashboard & Nodes */
        .stat-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-top: 15px; }
        .stat-box { background: #1a1a1a; padding: 15px; border-radius: 20px; border-bottom: 3px solid var(--gold); }
        .node-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 12px; padding: 15px; }
        .node-card { background: #0f0f0f; border: 1px solid #222; border-radius: 22px; overflow: hidden; position: relative; padding-bottom: 10px; box-shadow: 0 5px 15px rgba(0,0,0,0.5); }
        .node-img { width: 100%; height: 100px; object-fit: cover; filter: brightness(0.6); background: #1a1a1a; }
        .node-timer { position: absolute; top: 8px; right: 8px; background: rgba(0,0,0,0.8); color: var(--accent); padding: 3px 8px; border-radius: 50px; font-size: 8px; font-weight: 800; border: 1px solid var(--accent); }
        .node-price { font-size: 1.2rem; font-weight: 800; color: var(--gold); margin: 5px 0; text-align: center; }

        /* Navigation */
        .navbar { position: fixed; bottom: 0; width: 100%; background: rgba(0,0,0,0.98); display: flex; justify-content: space-around; padding: 18px 0; border-top: 1px solid #222; z-index: 5000; }
        .nav-link { color: #555; text-align: center; font-size: 9px; font-weight: 800; text-decoration: none; transition: 0.3s; }
        .nav-link.active { color: var(--gold); text-shadow: 0 0 10px rgba(212,175,55,0.3); }

        .screen { display: none; padding-bottom: 120px; animation: slideUp 0.4s ease; }
        .active-screen { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(15px); } to { opacity: 1; transform: translateY(0); } }

        /* History Status Colors */
        .st-verified { border-left: 3px solid var(--accent); color: var(--accent); }
        .st-pending { border-left: 3px solid var(--gold); color: var(--gold); }
        .st-rejected { border-left: 3px solid var(--danger); color: var(--danger); }

        /* Admin UI */
        #admin-panel { display: none; position: fixed; inset: 0; background: #000; z-index: 9999; padding: 25px; overflow-y: auto; }
        .adm-card { background: #111; border: 1px solid var(--gold); border-radius: 20px; padding: 15px; margin-bottom: 12px; }
        
        #payout-toast { position: fixed; top: 20px; left: 50%; transform: translateX(-50%); background: #111; border: 1px solid var(--accent); padding: 10px 20px; border-radius: 50px; font-size: 10px; z-index: 9999; display: none; white-space: nowrap; box-shadow: 0 0 20px rgba(0,255,163,0.2); }
    </style>
</head>
<body>

    <div id="payout-toast"></div>

    <div id="auth-screen">
        <div style="text-align:center; margin-bottom:40px;">
            <h1 id="logo-trigger" onclick="handleLogoTaps()" style="font-size:3.5rem; letter-spacing:-3px; font-weight:900;">NOVA<span style="color:var(--gold)">.</span></h1>
            <p style="color:var(--gold); font-size:9px; letter-spacing:5px; opacity:0.7;">QUANTUM ASSET MANAGEMENT</p>
        </div>
        <div class="auth-toggle">
            <button id="tab-login" class="active" onclick="toggleAuth('login')">LOGIN</button>
            <button id="tab-signup" onclick="toggleAuth('signup')">SIGNUP</button>
        </div>
        <div class="glass" style="width:100%; max-width:400px; box-sizing:border-box;">
            <input type="text" id="u-id" placeholder="Account Identity" class="input-nova">
            <input type="password" id="u-pass" placeholder="Security Passkey" class="input-nova">
            <div id="reg-fields" style="display:none;">
                <input type="text" id="u-whatsapp" placeholder="WhatsApp Connection" class="input-nova">
                <input type="text" id="u-ref" placeholder="Referral Code (Optional)" class="input-nova">
            </div>
            <input type="text" id="admin-key" style="display:none; border-color:var(--gold);" placeholder="Enter Master Key" class="input-nova">
            <button class="btn-nova" onclick="processAuth()">Initiate Protocol</button>
        </div>
    </div>

    <div id="admin-panel">
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:30px;">
            <h2 style="color:var(--gold);">ADMIN OVERRIDE</h2>
            <button onclick="location.reload()" style="background:var(--danger); color:#fff; border:none; padding:10px 20px; border-radius:12px; font-weight:900;">EXIT</button>
        </div>
        <div id="admin-requests"></div>
    </div>

    <div id="app-main" style="display:none;">
        <header style="padding:20px; display:flex; justify-content:space-between; align-items:center; border-bottom:1px solid #1a1a1a;">
            <div>
                <span id="user-display" style="font-weight:800; font-size:22px;">...</span><br>
                <small id="bonus-btn" onclick="claimBonus()" style="color:var(--accent); font-weight:800; cursor:pointer;"><i class="fa-solid fa-gift"></i> CLAIM DAILY REWARD</small>
            </div>
            <div style="background:var(--card); width:40px; height:40px; border-radius:12px; display:flex; align-items:center; justify-content:center; border:1px solid #333;" onclick="nav('office')">
                <i class="fa-solid fa-user-shield" style="color:var(--gold);"></i>
            </div>
        </header>

        <div id="dash" class="screen active-screen">
            <div style="padding:15px;">
                <div class="glass" style="background: linear-gradient(145deg, #111 0%, #000 100%);">
                    <p style="color:#666; font-size:10px; margin:0; letter-spacing:2px;">INSTITUTIONAL CAPITAL</p>
                    <h1 id="bal-main" style="font-size:3.5rem; margin:10px 0; letter-spacing:-3px;">$0.00</h1>
                    <div class="stat-grid">
                        <div class="stat-box"><small style="color:#555;">ACTIVE YIELD</small><br><b id="v-daily" style="color:var(--accent);">+$0.00</b></div>
                        <div class="stat-box"><small style="color:#555;">TOTAL PROFIT</small><br><b id="v-total">$0.00</b></div>
                    </div>
                </div>
            </div>

            <div style="padding:0 20px; display:flex; justify-content:space-between; align-items:center;">
                <h4 style="margin:0;">ASSET CLUSTERS</h4>
                <div style="font-size:9px; background:rgba(0,255,163,0.1); color:var(--accent); padding:4px 10px; border-radius:50px;">LIVE NODES</div>
            </div>
            <div id="nodes-grid" class="node-grid"></div>
        </div>

        <div id="finance" class="screen">
            <div style="padding:15px;">
                <div class="glass">
                    <div style="display:flex; gap:10px; margin-bottom:20px;">
                        <button class="btn-nova" style="padding:15px; font-size:12px;" onclick="setFin('dep')">DEPOSIT</button>
                        <button class="btn-nova" style="padding:15px; font-size:12px; background:#222; color:#fff;" onclick="setFin('wd')">WITHDRAW</button>
                    </div>

                    <div id="box-dep">
                        <select id="dep-method" class="input-nova">
                            <option>Binance (USDT-BEP20)</option>
                            <option>Trust Wallet (USDT)</option>
                            <option>MetaMask (USDT)</option>
                        </select>
                        <p style="font-size:9px; text-align:center; color:var(--gold); border:1px dashed #333; padding:10px; border-radius:10px;">Network Address: <br><b>0x71C7656EC7ab88b098defB751B7401B5f6d8976F</b></p>
                        <input type="number" id="dep-amt" placeholder="Value Amount ($)" class="input-nova">
                        <input type="text" id="dep-tid" placeholder="Tx Hash / TID" class="input-nova">
                        <label style="font-size:10px; color:#555;">Attach Digital Evidence (Proof):</label>
                        <input type="file" id="proof-file" accept="image/*" class="input-nova" onchange="readBase64(this)">
                        <div id="proof-preview" style="width:100%; height:120px; border:1px solid #333; border-radius:15px; background-size:contain; background-repeat:no-repeat; background-position:center; margin-bottom:15px; display:flex; align-items:center; justify-content:center; color:#333; font-size:10px;">Proof Preview</div>
                        <button class="btn-nova" onclick="submitDeposit()">Submit for Verification</button>
                    </div>

                    <div id="box-wd" style="display:none;">
                        <select id="wd-method" class="input-nova">
                            <option>EasyPaisa</option>
                            <option>Binance Pay ID</option>
                            <option>Crypto (USDT-TRC20)</option>
                            <option>PayPal</option>
                        </select>
                        <input type="number" id="wd-amt" placeholder="Withdrawal Amount" class="input-nova">
                        <input type="text" id="wd-acc" placeholder="Address / Account Details" class="input-nova">
                        <input type="text" id="wd-name" placeholder="Full Account Name" class="input-nova">
                        <button class="btn-nova" onclick="submitWithdraw()">Request Global Payout</button>
                    </div>
                </div>
            </div>
            <div style="padding:0 15px;">
                <h4>TRANSACTION LOGS</h4>
                <div id="history-list" class="glass" style="padding:15px; font-size:11px; color:#555;">Generating activity...</div>
            </div>
        </div>

        <div id="office" class="screen">
            <div style="padding:15px;">
                <div class="glass" style="margin-bottom:15px;">
                    <h3>NOVA GLOBAL</h3>
                    <p style="font-size:11px; line-height:1.6; color:#777; margin-top:10px;">Institutional-grade asset validation and node clusters. We power the next generation of decentralized finance.</p>
                    <div style="margin-top:20px; border-top:1px solid #222; padding-top:15px;">
                        <p onclick="window.open('https://t.me/Nova_Official_Support')" style="color:var(--gold); cursor:pointer; font-weight:800;"><i class="fa-brands fa-telegram"></i> Telegram Support Channel</p>
                        <p><i class="fa-solid fa-envelope"></i> corporate@nova-solutions.io</p>
                    </div>
                </div>

                <div class="glass">
                    <h3>AFFILIATE NETWORK</h3>
                    <p style="font-size:11px; color:#666;">Earn 10% on Tier 1 node activations through your link.</p>
                    <div style="background:#000; padding:15px; border-radius:15px; border:1px dashed var(--gold); word-break:break-all;">
                        <small id="ref-link" style="color:var(--gold); font-weight:800;"></small>
                    </div>
                </div>

                <div class="glass" style="margin-top:20px; border-color:var(--danger);">
                    <button onclick="secureLogout()" style="background:transparent; color:var(--danger); border:1px solid var(--danger); padding:15px; border-radius:18px; width:100%; font-weight:900; cursor:pointer;">
                        <i class="fa-solid fa-right-from-bracket"></i> TERMINATE SECURE SESSION
                    </button>
                </div>
            </div>
        </div>
    </div>

    <nav class="navbar" id="nav-bar" style="display:none;">
        <a href="#" class="nav-link active" onclick="nav('dash', this)"><i class="fa-solid fa-microchip" style="font-size:1.5rem;"></i><br>MINING</a>
        <a href="#" class="nav-link" onclick="nav('finance', this)"><i class="fa-solid fa-money-bill-transfer" style="font-size:1.5rem;"></i><br>FINANCE</a>
        <a href="#" class="nav-link" onclick="nav('office', this)"><i class="fa-solid fa-briefcase" style="font-size:1.5rem;"></i><br>OFFICE</a>
    </nav>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, orderBy } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        // CONFIG (Replace with your actual Firebase details)
        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        let user = null; let base64Data = ""; let authMode = "login"; let taps = 0;

        window.handleLogoTaps = () => { taps++; if(taps >= 6) { document.getElementById('admin-key').style.display='block'; document.getElementById('logo-trigger').style.color='var(--gold)'; }};

        window.toggleAuth = (m) => {
            authMode = m;
            document.getElementById('reg-fields').style.display = m === 'signup' ? 'block' : 'none';
            document.getElementById('tab-login').className = m === 'login' ? 'active' : '';
            document.getElementById('tab-signup').className = m === 'signup' ? 'active' : '';
        };

        window.processAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pass').value;
            const ak = document.getElementById('admin-key').value;

            if(ak === "nov786") { document.getElementById('admin-panel').style.display='block'; loadAdmin(); return; }
            if(!id || !pw) return alert("Security Protocol: Enter credentials.");

            const ref = doc(db, "users", id);
            const snap = await getDoc(ref);

            if(authMode === 'login') {
                if(snap.exists() && snap.data().password === pw) { boot(id); }
                else alert("Authentication failed.");
            } else {
                if(snap.exists()) return alert("Identity already exists.");
                await setDoc(ref, { password: pw, balance: 0, daily: 0, total: 0, lastBonus: 0, whatsapp: document.getElementById('u-whatsapp').value, refBy: document.getElementById('u-ref').value, time: Date.now() });
                boot(id);
            }
        };

        function boot(id) {
            user = id; sessionStorage.setItem("nova_u", id);
            document.getElementById('auth-screen').style.display='none';
            document.getElementById('app-main').style.display='block';
            document.getElementById('nav-bar').style.display='flex';
            document.getElementById('user-display').innerText = id.toUpperCase();
            document.getElementById('ref-link').innerText = "https://nova-global.io/join?ref=" + id;

            onSnapshot(doc(db, "users", id), (d) => {
                const data = d.data();
                document.getElementById('bal-main').innerText = "$" + (data.balance || 0).toLocaleString(undefined, {minimumFractionDigits:2});
                document.getElementById('v-daily').innerText = "+$" + (data.daily || 0).toFixed(2);
                document.getElementById('v-total').innerText = "$" + (data.total || 0).toFixed(2);
            });
            renderNodes(); loadHistory(); startVisualMining(); startFakeToasts();
        }

        function renderNodes() {
            const grid = document.getElementById('nodes-grid'); grid.innerHTML = "";
            const tiers = [30, 50, 100, 300, 500, 1000, 2500, 5000, 10000, 25000, 50000, 100000];
            tiers.forEach((p, i) => {
                grid.innerHTML += `
                    <div class="node-card">
                        <div class="node-timer">29d 23h 59m</div>
                        <img src="https://images.unsplash.com/photo-1639762681485-074b7f938ba0?q=80&w=400" class="node-img">
                        <div class="node-price">$${p.toLocaleString()}</div>
                        <div style="text-align:center; font-size:9px; color:#555; margin-bottom:10px;">Return: $${(p*0.05).toFixed(2)}/day</div>
                        <button class="btn-nova" style="padding:10px; font-size:9px; width:90%; margin:0 auto 10px; display:block;" onclick="buyNode(${p}, ${p*0.05})">Stake Node</button>
                    </div>`;
            });
        }

        window.buyNode = async (price, dProf) => {
            const ref = doc(db, "users", user);
            const s = await getDoc(ref);
            if(s.data().balance < price) return alert("Capital too low for this cluster.");
            await updateDoc(ref, { balance: s.data().balance - price, daily: (s.data().daily || 0) + dProf });
            alert("Node cluster activation successful.");
        };

        window.claimBonus = async () => {
            const ref = doc(db, "users", user); const s = await getDoc(ref);
            const now = Date.now();
            if(now - (s.data().lastBonus || 0) < 86400000) return alert("System Sync: Already claimed.");
            await updateDoc(ref, { balance: s.data().balance + 1, lastBonus: now });
            alert("Elite Dividend +$1.00 Received.");
        };

        window.readBase64 = (el) => {
            const reader = new FileReader();
            reader.onload = () => { base64Data = reader.result; document.getElementById('proof-preview').style.backgroundImage = `url(${base64Data})`; document.getElementById('proof-preview').innerText = ""; };
            reader.readAsDataURL(el.files[0]);
        };

        window.setFin = (t) => {
            document.getElementById('box-dep').style.display = t === 'dep' ? 'block' : 'none';
            document.getElementById('box-wd').style.display = t === 'wd' ? 'block' : 'none';
        };

        window.submitDeposit = async () => {
            const amt = document.getElementById('dep-amt').value; const tid = document.getElementById('dep-tid').value;
            if(!amt || !tid || !base64Data) return alert("Incomplete evidence logs.");
            await addDoc(collection(db, "logs"), { user, amount: parseFloat(amt), tid, type: 'Deposit', proof: base64Data, status: 'Pending', method: document.getElementById('dep-method').value, time: Date.now() });
            alert("Evidence sent for institutional audit.");
        };

        window.submitWithdraw = async () => {
            const amt = document.getElementById('wd-amt').value;
            if(!amt || amt < 10) return alert("Min threshold: $10.");
            await addDoc(collection(db, "logs"), { user, amount: parseFloat(amt), type: 'Withdrawal', method: document.getElementById('wd-method').value, acc: document.getElementById('wd-acc').value, status: 'Pending', time: Date.now() });
            alert("Withdrawal request logged in network.");
        };

        function startVisualMining() {
            setInterval(() => {
                const el = document.getElementById('bal-main');
                let val = parseFloat(el.innerText.replace('$','').replace(',',''));
                el.innerText = "$" + (val + 0.0001).toFixed(4);
            }, 3000);
        }

        function startFakeToasts() {
            const list = ["Alex_VIP", "Sam_Trader", "Elena_99", "Viktor_Pro", "Sarah_Crypto"];
            setInterval(() => {
                if(Math.random() > 0.7) {
                    const t = document.getElementById('payout-toast');
                    const u = list[Math.floor(Math.random()*list.length)];
                    const a = (Math.random()*500 + 50).toFixed(2);
                    t.innerText = `🔥 SIGNAL: ${u} just withdrew $${a}`;
                    t.style.display='block'; setTimeout(() => t.style.display='none', 3000);
                }
            }, 8000);
        }

        function loadHistory() {
            const q = query(collection(db, "logs"), where("user", "==", user), orderBy("time", "desc"));
            onSnapshot(q, (snap) => {
                const l = document.getElementById('history-list'); l.innerHTML = "";
                snap.forEach(d => {
                    const t = d.data();
                    let stClass = "st-pending";
                    if(t.status === 'Verified') stClass = "st-verified";
                    if(t.status === 'Rejected') stClass = "st-rejected";
                    
                    l.innerHTML += `<div class="glass ${stClass}" style="margin-bottom:10px; padding:12px; font-size:12px;">
                        <div style="display:flex; justify-content:space-between;">
                            <b>${t.type} (${t.method})</b>
                            <span style="font-weight:900;">${t.status.toUpperCase()}</span>
                        </div>
                        <div style="display:flex; justify-content:space-between; font-size:10px; color:#555; margin-top:5px;">
                            <span>$${t.amount.toLocaleString()}</span>
                            <span>${new Date(t.time).toLocaleDateString()}</span>
                        </div>
                    </div>`;
                });
            });
        }

        function loadAdmin() {
            const q = query(collection(db, "logs"), where("status", "==", "Pending"), orderBy("time", "asc"));
            onSnapshot(q, (snap) => {
                const l = document.getElementById('admin-requests'); l.innerHTML = "";
                snap.forEach(d => {
                    const r = d.data();
                    l.innerHTML += `<div class="adm-card">
                        <p>${r.user} | ${r.type}: $${r.amount} <br><small>${r.tid || 'No TID'}</small></p>
                        ${r.proof ? `<button onclick="window.open('${r.proof}')" style="background:var(--gold); border:none; padding:5px; font-size:10px; border-radius:5px; margin-bottom:10px; cursor:pointer;">View Proof</button>` : ''}
                        <div style="display:flex; gap:10px;">
                            <button onclick="adminAction('${d.id}','${r.user}',${r.amount},'${r.type}','Verified')" style="flex:1; background:var(--accent); color:#000; border:none; padding:10px; border-radius:10px; font-weight:800; cursor:pointer;">APPROVE</button>
                            <button onclick="adminAction('${d.id}','${r.user}',${r.amount},'${r.type}','Rejected')" style="flex:1; background:var(--danger); color:#fff; border:none; padding:10px; border-radius:10px; font-weight:800; cursor:pointer;">REJECT</button>
                        </div>
                    </div>`;
                });
            });
        }

        window.adminAction = async (docId, userId, amount, type, newStatus) => {
            const r = doc(db, "users", userId); const s = await getDoc(r);
            if (newStatus === 'Verified') {
                let b = type === 'Deposit' ? (s.data().balance + amount) : (s.data().balance - amount);
                await updateDoc(r, { balance: b });
            }
            await updateDoc(doc(db, "logs", docId), { status: newStatus });
            alert("Operation successful.");
        };

        window.secureLogout = () => {
            if(confirm("Terminate protocol session?")) { sessionStorage.clear(); location.reload(); }
        };

        window.nav = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.style.display='none');
            document.getElementById(id).style.display='block';
            document.querySelectorAll('.nav-link').forEach(l => l.classList.remove('active'));
            if(el) el.classList.add('active');
        };

        if(sessionStorage.getItem("nova_u")) boot(sessionStorage.getItem("nova_u"));
    </script>
</body>
</html>
