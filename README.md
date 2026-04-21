<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Institutional Asset Management</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@300;600;900&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --accent: #00FFA3; --bg: #050505; --card: #111111; --text: #FFFFFF; }
        body { margin: 0; font-family: 'Montserrat', sans-serif; background: var(--bg); color: var(--text); overflow-x: hidden; }
        
        /* Corporate UI Elements */
        .glass { background: rgba(20,20,20,0.9); border: 1px solid rgba(212,175,55,0.2); border-radius: 30px; padding: 25px; backdrop-filter: blur(20px); }
        .btn-nova { background: linear-gradient(135deg, var(--gold), #F2CC60); color: #000; border: none; padding: 20px; border-radius: 18px; font-weight: 900; cursor: pointer; text-transform: uppercase; letter-spacing: 2px; width: 100%; transition: 0.4s; }
        .input-nova { width: 100%; padding: 18px; background: #000; border: 1px solid #333; color: #fff; border-radius: 15px; margin-bottom: 15px; outline: none; font-family: 'Montserrat'; }
        
        /* Dashboard Stats */
        .stat-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-top: 20px; }
        .stat-box { background: var(--card); padding: 15px; border-radius: 20px; border-left: 4px solid var(--gold); }
        
        /* Node Cards */
        .node-card { background: var(--card); border: 1px solid #222; border-radius: 25px; padding: 15px; position: relative; margin-bottom: 15px; transition: 0.3s; }
        .node-img { width: 100%; height: 140px; border-radius: 20px; object-fit: cover; background: #1a1a1a; }
        .countdown { position: absolute; top: 10px; right: 10px; background: rgba(0,0,0,0.8); color: var(--accent); padding: 5px 12px; border-radius: 50px; font-size: 10px; font-weight: 800; border: 1px solid var(--accent); }

        /* Navigation */
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(0,0,0,0.95); display: flex; justify-content: space-around; padding: 20px 0; border-top: 1px solid #222; z-index: 5000; }
        .nav-item { color: #666; text-align: center; font-size: 9px; font-weight: 700; text-decoration: none; }
        .nav-item.active { color: var(--gold); }

        .screen { display: none; padding: 20px; padding-bottom: 120px; animation: fadeIn 0.4s ease; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        /* Image Preview Box */
        #preview-box { width: 100%; height: 150px; border: 2px dashed #444; border-radius: 15px; margin-top: 10px; display: flex; align-items: center; justify-content: center; background-size: cover; background-position: center; }
    </style>
</head>
<body>

    <div id="auth-screen" style="height:100vh; display:flex; flex-direction:column; align-items:center; justify-content:center; padding:30px;">
        <div style="text-align:center; margin-bottom:40px;">
            <h1 style="font-size:3.5rem; margin:0; letter-spacing:-4px; font-weight:900;">NOVA<span style="color:var(--gold)">.</span></h1>
            <p style="color:var(--gold); font-size:9px; letter-spacing:6px; opacity:0.8;">WORLDWIDE ASSET TERMINAL</p>
        </div>
        <div class="glass" style="width:100%; max-width:400px;">
            <div id="auth-toggle" style="display:flex; margin-bottom:20px; gap:10px;">
                <button onclick="toggleAuth('login')" id="btn-login" style="flex:1; background:var(--gold); color:#000; border:none; padding:10px; border-radius:10px; font-weight:900;">LOGIN</button>
                <button onclick="toggleAuth('signup')" id="btn-signup" style="flex:1; background:#222; color:#fff; border:none; padding:10px; border-radius:10px; font-weight:900;">SIGNUP</button>
            </div>
            <input type="text" id="u-id" placeholder="Account Identifier / Email" class="input-nova">
            <input type="password" id="u-pass" placeholder="Security Passkey" class="input-nova">
            <div id="signup-fields" style="display:none;">
                <input type="text" id="u-phone" placeholder="Phone Number (WhatsApp)" class="input-nova">
                <input type="text" id="u-ref" placeholder="Referral ID (Optional)" class="input-nova">
            </div>
            <button class="btn-nova" onclick="handleAuth()">Access Protocol</button>
        </div>
    </div>

    <div id="app-main" style="display:none;">
        <header style="padding:25px; display:flex; justify-content:space-between; align-items:center; border-bottom:1px solid #1a1a1a;">
            <div>
                <span id="display-user" style="font-weight:900; font-size:24px;">USER</span><br>
                <small id="bonus-status" onclick="claimDaily()" style="color:var(--accent); font-weight:800; cursor:pointer;"><i class="fa-solid fa-gift"></i> CLAIM LOGIN BONUS</small>
            </div>
            <div style="background:var(--card); padding:10px; border-radius:15px; border:1px solid #333;" onclick="nav('profile')">
                <i class="fa-solid fa-user-shield" style="color:var(--gold);"></i>
            </div>
        </header>

        <div id="dash" class="screen" style="display:block;">
            <div class="glass" style="background: linear-gradient(135deg, #111 0%, #000 100%); margin-bottom:25px;">
                <p style="color:#666; font-size:11px; margin:0; letter-spacing:2px;">LIQUID ASSETS (USDT)</p>
                <h1 id="bal-val" style="font-size:3.8rem; margin:15px 0; color:#fff; letter-spacing:-3px;">$0.00</h1>
                <div class="stat-grid">
                    <div class="stat-box"><small style="color:#666;">DAILY YIELD</small><br><b id="daily-v" style="color:var(--accent);">+$0.00</b></div>
                    <div class="stat-box"><small style="color:#666;">TOTAL PROFIT</small><br><b id="total-v">$0.00</b></div>
                </div>
            </div>

            <h4 style="margin-bottom:15px;">STAKING NODES</h4>
            <div id="nodes-grid" style="display:grid; grid-template-columns:1fr 1fr; gap:12px;"></div>
        </div>

        <div id="finance" class="screen">
            <div class="glass" style="margin-bottom:20px;">
                <div style="display:flex; gap:10px; margin-bottom:25px;">
                    <button class="btn-nova" style="padding:15px; font-size:12px;" onclick="finTab('dep')">Deposit</button>
                    <button class="btn-nova" style="padding:15px; font-size:12px; background:#222; color:#fff;" onclick="finTab('wd')">Withdraw</button>
                </div>

                <div id="dep-box">
                    <select id="dep-method" class="input-nova">
                        <option>Binance (USDT-BEP20)</option>
                        <option>Trust Wallet (USDT)</option>
                        <option>MetaMask (USDT)</option>
                    </select>
                    <p style="font-size:10px; color:var(--gold); text-align:center; background:rgba(0,0,0,0.5); padding:10px; border-radius:10px;">Company Address: <br><b>0x71C7656EC7ab88b098defB751B7401B5f6d8976F</b></p>
                    <input type="number" id="dep-amt" placeholder="Amount ($)" class="input-nova">
                    <input type="text" id="dep-tid" placeholder="Transaction ID (TID)" class="input-nova">
                    <label style="font-size:11px; color:#888;">Upload Proof (Base64):</label>
                    <input type="file" id="proof-img" onchange="previewProof(this)" class="input-nova" style="padding:10px;">
                    <div id="preview-box">Preview</div>
                    <button class="btn-nova" style="margin-top:20px;" onclick="submitDep()">Verify Assets</button>
                </div>

                <div id="wd-box" style="display:none;">
                    <select id="wd-method" class="input-nova">
                        <option>EasyPaisa</option>
                        <option>PayPal</option>
                        <option>Binance Pay ID</option>
                        <option>Bank Transfer (Worldwide)</option>
                    </select>
                    <input type="number" id="wd-amt" placeholder="Withdrawal Amount" class="input-nova">
                    <input type="text" id="wd-acc" placeholder="Account Number / Email" class="input-nova">
                    <input type="text" id="wd-name" placeholder="Account Holder Name" class="input-nova">
                    <button class="btn-nova" onclick="submitWd()">Request Payout</button>
                </div>
            </div>
            
            <h4>ACTIVITY LOG</h4>
            <div id="trans-history" class="glass" style="padding:15px; font-size:11px; color:#888;">No recent activity.</div>
        </div>

        <div id="profile" class="screen">
            <div class="glass" style="margin-bottom:20px;">
                <h3>Company Profile</h3>
                <p style="font-size:12px; line-height:1.6; color:#888;">NOVA is a registered global asset management firm specialized in high-frequency trading and node validation.</p>
                <div style="border-top:1px solid #222; padding-top:15px; margin-top:15px;">
                    <p onclick="window.open('https://t.me/NovaSupport')" style="color:var(--gold); cursor:pointer;"><i class="fa-brands fa-telegram"></i> Telegram Official</p>
                    <p><i class="fa-solid fa-envelope"></i> support@nova-global.com</p>
                </div>
            </div>

            <div class="glass">
                <h3>Affiliate Center</h3>
                <p style="font-size:11px;">Copy your referral link and earn 10% Level 1 bonus.</p>
                <div style="background:#000; padding:15px; border-radius:12px; border:1px dashed var(--gold); word-break: break-all;">
                    <small id="ref-link"></small>
                </div>
            </div>
        </div>
    </div>

    <nav class="nav-bar" id="bottom-nav" style="display:none;">
        <a href="#" class="nav-item active" onclick="nav('dash', this)"><i class="fa-solid fa-cube" style="font-size:1.5rem;"></i><br>MINING</a>
        <a href="#" class="nav-item" onclick="nav('finance', this)"><i class="fa-solid fa-money-bill-transfer" style="font-size:1.5rem;"></i><br>FINANCE</a>
        <a href="#" class="nav-item" onclick="nav('profile', this)"><i class="fa-solid fa-briefcase" style="font-size:1.5rem;"></i><br>OFFICE</a>
    </nav>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, orderBy } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        // CONFIG (Replace with your actual Firebase details)
        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        let user = null; let base64Img = ""; let authMode = "login";

        window.toggleAuth = (m) => {
            authMode = m;
            document.getElementById('signup-fields').style.display = m === 'signup' ? 'block' : 'none';
            document.getElementById('btn-login').style.background = m === 'login' ? 'var(--gold)' : '#222';
            document.getElementById('btn-signup').style.background = m === 'signup' ? 'var(--gold)' : '#222';
        };

        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pass').value;
            if(!id || !pw) return alert("Security Violation: Missing Data");

            const ref = doc(db, "users", id);
            const snap = await getDoc(ref);

            if(authMode === 'login') {
                if(snap.exists() && snap.data().password === pw) { boot(id); }
                else alert("Access Denied: Invalid Credentials");
            } else {
                if(snap.exists()) return alert("Identity already registered");
                await setDoc(ref, { 
                    password: pw, balance: 0, daily: 0, total: 0, 
                    phone: document.getElementById('u-phone').value,
                    refBy: document.getElementById('u-ref').value,
                    joined: Date.now(), lastBonus: 0
                });
                boot(id);
            }
        };

        function boot(id) {
            user = id; sessionStorage.setItem("nova_token", id);
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app-main').style.display = 'block';
            document.getElementById('bottom-nav').style.display = 'flex';
            document.getElementById('display-user').innerText = id.toUpperCase();
            document.getElementById('ref-link').innerText = "https://nova.global/join?id=" + id;

            onSnapshot(doc(db, "users", id), (d) => {
                const data = d.data();
                document.getElementById('bal-val').innerText = "$" + (data.balance || 0).toLocaleString(undefined, {minimumFractionDigits:2});
                document.getElementById('daily-v').innerText = "+$" + (data.daily || 0).toFixed(2);
                document.getElementById('total-v').innerText = "$" + (data.total || 0).toFixed(2);
            });
            renderNodes();
            loadHistory();
        }

        function renderNodes() {
            const grid = document.getElementById('nodes-grid'); grid.innerHTML = "";
            const tiers = [
                {p: 50, d: 2.5}, {p: 100, d: 5.2}, {p: 500, d: 28}, {p: 1000, d: 60},
                {p: 2500, d: 155}, {p: 5000, d: 320}, {p: 10000, d: 700}, {p: 25000, d: 1800}
            ];
            tiers.forEach((t, i) => {
                grid.innerHTML += `
                    <div class="node-card">
                        <div class="countdown">29d 23h 59m</div>
                        <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAOAAAADhCAMAAAD66L79AAAAA1BMVEWAgICQpREbAAAAR0lEQVR4nO3BAQEAAACAkP6v7ggKAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAgB8M4AAHL750VAAAAAElFTkSuQmCC" class="node-img">
                        <div style="margin-top:10px;">
                            <span style="font-weight:900; font-size:18px;">$${t.p}</span><br>
                            <small style="color:#666; font-size:9px;">Daily ROI: $${t.d}</small>
                        </div>
                        <button onclick="buyNode(${t.p}, ${t.d})" class="btn-nova" style="padding:8px; font-size:9px; margin-top:10px;">Purchase Node</button>
                    </div>`;
            });
        }

        window.buyNode = async (price, yieldAmt) => {
            const ref = doc(db, "users", user);
            const snap = await getDoc(ref);
            if(snap.data().balance < price) return alert("Insufficient Capital in Terminal");
            
            await updateDoc(ref, { 
                balance: snap.data().balance - price,
                daily: (snap.data().daily || 0) + yieldAmt 
            });
            alert("Protocol Activated: Mining Started");
        };

        window.claimDaily = async () => {
            const ref = doc(db, "users", user);
            const snap = await getDoc(ref);
            const now = Date.now();
            if(now - snap.data().lastBonus < 86400000) return alert("Bonus already synced for today");

            await updateDoc(ref, { 
                balance: snap.data().balance + 1, 
                lastBonus: now 
            });
            alert("Elite Login Bonus Claimed: +$1.00");
        };

        window.previewProof = (el) => {
            const file = el.files[0];
            const reader = new FileReader();
            reader.onloadend = () => {
                base64Img = reader.result;
                document.getElementById('preview-box').style.backgroundImage = `url(${base64Img})`;
                document.getElementById('preview-box').innerText = "";
            };
            reader.readAsDataURL(file);
        };

        window.finTab = (t) => {
            document.getElementById('dep-box').style.display = t === 'dep' ? 'block' : 'none';
            document.getElementById('wd-box').style.display = t === 'wd' ? 'block' : 'none';
        };

        window.submitDep = async () => {
            const amt = document.getElementById('dep-amt').value;
            const tid = document.getElementById('dep-tid').value;
            if(!amt || !tid || !base64Img) return alert("Submission Rejected: Evidence Missing");

            await addDoc(collection(db, "transactions"), {
                user, amount: parseFloat(amt), tid, type: 'Deposit', 
                method: document.getElementById('dep-method').value,
                proof: base64Img, status: 'Pending', time: Date.now()
            });
            alert("Assets queued for verification.");
        };

        window.submitWd = async () => {
            const amt = document.getElementById('wd-amt').value;
            if(!amt || amt < 10) return alert("Minimum withdrawal is $10");
            
            await addDoc(collection(db, "transactions"), {
                user, amount: parseFloat(amt), type: 'Withdrawal',
                method: document.getElementById('wd-method').value,
                account: document.getElementById('wd-acc').value,
                status: 'Pending', time: Date.now()
            });
            alert("Payout request submitted to worldwide network.");
        };

        function loadHistory() {
            const q = query(collection(db, "transactions"), where("user", "==", user), orderBy("time", "desc"));
            onSnapshot(q, (snap) => {
                const list = document.getElementById('trans-history');
                list.innerHTML = "";
                snap.forEach(d => {
                    const t = d.data();
                    list.innerHTML += `<div style="display:flex; justify-content:space-between; margin-bottom:10px; border-bottom:1px solid #222; padding-bottom:5px;">
                        <span>${t.type} ($${t.amount})</span>
                        <span style="color:${t.status === 'Pending' ? 'var(--gold)' : 'var(--accent)'}">${t.status}</span>
                    </div>`;
                });
            });
        }

        window.nav = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.style.display='none');
            document.getElementById(id).style.display='block';
            document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('active'));
            if(el) el.classList.add('active');
        };

        if(sessionStorage.getItem("nova_token")) boot(sessionStorage.getItem("nova_token"));
    </script>
</body>
</html>
