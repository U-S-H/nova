<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA PROTOCOL | Institutional Mining</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;600;900&family=Space+Grotesk:wght@700&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --bg: #050505; --glass: rgba(255, 255, 255, 0.03); }
        body { background: var(--bg); color: white; font-family: 'Outfit', sans-serif; overflow-x: hidden; -webkit-tap-highlight-color: transparent; }
        .heading-font { font-family: 'Space+Grotesk', sans-serif; }
        .glass-card { background: var(--glass); border: 1px solid rgba(255,255,255,0.06); border-radius: 32px; backdrop-filter: blur(15px); }
        .gold-gradient { background: linear-gradient(135deg, #D4AF37 0%, #FBE795 100%); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        .btn-main { background: linear-gradient(135deg, #D4AF37 0%, #FBE795 100%); color: black; font-weight: 900; border-radius: 18px; transition: 0.3s; text-transform: uppercase; font-size: 11px; letter-spacing: 1px; }
        .btn-main:active { transform: scale(0.96); }
        .nova-input { background: #0c0c0c; border: 1px solid #1a1a1a; width: 100%; padding: 18px; border-radius: 20px; color: white; font-size: 14px; outline: none; margin-bottom: 15px; transition: 0.3s; }
        .nova-input:focus { border-color: var(--gold); }
        .screen { display: none; padding-bottom: 130px; animation: slideIn 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275); }
        .active-screen { display: block; }
        @keyframes slideIn { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; left: 0; right: 0; background: rgba(5,5,5,0.9); border-top: 1px solid #111; display: flex; justify-content: space-around; padding: 22px; z-index: 1000; backdrop-filter: blur(20px); }
        .nav-item { color: #444; font-size: 10px; font-weight: 900; display: flex; flex-direction: column; align-items: center; gap: 6px; }
        .nav-item.active { color: var(--gold); }
        .modal { display: none; position: fixed; inset: 0; background: #000; z-index: 2000; padding: 25px; overflow-y: auto; }
        .status-pill { background: rgba(0,255,136,0.1); color: #00FF88; font-size: 8px; padding: 4px 10px; border-radius: 20px; font-weight: 900; }
    </style>
</head>
<body>

    <div id="auth-screen" class="min-h-screen flex flex-col p-10 justify-center active-screen">
        <h1 onclick="handleTaps()" class="heading-font text-6xl font-black mb-2 tracking-tighter text-white">NOVA<span class="gold-gradient">.</span></h1>
        <p class="text-[10px] text-gray-600 font-black tracking-[7px] uppercase mb-16">Institutional Access Only</p>
        <div class="space-y-3">
            <input type="text" id="u-email" placeholder="Terminal ID" class="nova-input">
            <input type="password" id="u-pass" placeholder="Passkey" class="nova-input">
            <input type="text" id="adm-key" placeholder="Root Override" class="hidden nova-input border-[#D4AF37]">
            <button onclick="auth()" class="w-full btn-main py-5 shadow-lg shadow-[#D4AF37]/10">Access Terminal</button>
            <p onclick="toggleM()" id="m-text" class="text-center text-[10px] mt-10 text-gray-500 font-black uppercase cursor-pointer">Register New Entity</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-6 flex justify-between items-center sticky top-0 bg-black/50 backdrop-blur-md z-50">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 glass-card flex items-center justify-center text-[#D4AF37] font-black border-[#D4AF37]/20">N</div>
                <p id="user-display" class="text-[10px] font-black uppercase tracking-widest text-gray-400"></p>
            </div>
            <i onclick="location.reload()" class="fa-solid fa-power-off text-gray-800 text-lg"></i>
        </header>

        <div id="dash-screen" class="screen active-screen px-6">
            <div class="glass-card p-10 mb-8 border-b-2 border-[#D4AF37]/30 text-center relative overflow-hidden">
                <p class="text-[9px] text-gray-500 font-black tracking-[5px] uppercase mb-4">Current Valuation</p>
                <h1 id="main-bal" class="text-6xl font-black tracking-tighter gold-gradient">$0.00</h1>
                <div class="mt-6 flex items-center justify-center gap-2">
                    <span class="status-pill">SECURED PROTOCOL</span>
                    <p id="v-daily" class="text-[9px] font-black text-green-500 uppercase">+$0.00 Daily</p>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-10">
                <button onclick="openModal('dep-modal')" class="glass-card p-6 flex flex-col items-center gap-2 text-green-500"><i class="fa-solid fa-plus text-lg"></i><span class="text-[9px] font-black uppercase">Deposit</span></button>
                <button onclick="openModal('wd-modal')" class="glass-card p-6 flex flex-col items-center gap-2 text-red-500"><i class="fa-solid fa-minus text-lg"></i><span class="text-[9px] font-black uppercase">Withdraw</span></button>
            </div>

            <div class="glass-card p-8">
                <h3 class="text-xs font-black mb-6 uppercase tracking-widest gold-gradient">Company Intelligence</h3>
                <p class="text-[11px] text-gray-500 leading-relaxed font-medium italic mb-6">"Founded in 2021, Nova Institutional specializes in distributed SHA-256 computation. Headquartered in Singapore with active hash-centers across Northern Europe."</p>
                <div class="grid grid-cols-3 gap-4 border-t border-white/5 pt-6">
                    <div class="text-center"><p class="text-[7px] text-gray-600 font-black uppercase">License</p><p class="text-[10px] font-bold">MSB-740</p></div>
                    <div class="text-center"><p class="text-[7px] text-gray-600 font-black uppercase">Audited</p><p class="text-[10px] font-bold text-green-500">Verified</p></div>
                    <div class="text-center"><p class="text-[7px] text-gray-600 font-black uppercase">Insured</p><p class="text-[10px] font-bold">$100M</p></div>
                </div>
            </div>
        </div>

        <div id="mine-screen" class="screen px-6">
            <h2 class="heading-font text-3xl font-black mb-2 uppercase gold-gradient">Hardware Fleet</h2>
            <p class="text-[9px] text-gray-600 mb-10 font-black uppercase tracking-[4px]">Active Node Distribution</p>
            <div id="nodes-container" class="space-y-6"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('dash-screen')" class="nav-item active" id="nav-dash"><i class="fa-solid fa-terminal text-xl"></i><span>CORE</span></div>
            <div onclick="nav('mine-screen')" class="nav-item" id="nav-mine"><i class="fa-solid fa-server text-xl"></i><span>FARM</span></div>
        </nav>
    </div>

    <div id="dep-modal" class="modal">
        <div onclick="closeModal('dep-modal')" class="mb-10 text-gray-600 font-black text-xs uppercase tracking-widest">Back</div>
        <h2 class="text-4xl font-black mb-10 heading-font gold-gradient">DEPOSIT</h2>
        <select id="dep-method" class="nova-input" onchange="updateAddr()">
            <option value="Trust">Trust Wallet (BEP20)</option>
            <option value="Meta">MetaMask (ERC20)</option>
            <option value="Binance">Binance (TRC20)</option>
            <option value="Jazz">JazzCash / EasyPaisa</option>
        </select>
        <div class="glass-card p-6 mb-8 text-center border-dashed border-gray-800">
            <p class="text-[8px] text-gray-600 font-black uppercase mb-2">Vault Address</p>
            <p id="dep-addr" class="text-[11px] font-mono break-all text-gray-400">0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37</p>
        </div>
        <input type="number" id="dep-amt" placeholder="Deposit Amount ($)" class="nova-input">
        <input type="text" id="dep-user" placeholder="Your Full Name" class="nova-input">
        <input type="text" id="dep-tid" placeholder="Transaction TID / ID" class="nova-input">
        <div class="glass-card p-5 mb-10">
            <p class="text-[10px] font-black text-gray-600 uppercase mb-3">Upload Transfer Proof</p>
            <input type="file" id="dep-proof" accept="image/*" class="text-[10px] text-gray-400">
        </div>
        <button onclick="requestFin('Deposit')" class="w-full btn-main py-5">Initiate Vault Deposit</button>
    </div>

    <div id="wd-modal" class="modal">
        <div onclick="closeModal('wd-modal')" class="mb-10 text-gray-600 font-black text-xs uppercase tracking-widest">Back</div>
        <h2 class="text-4xl font-black mb-10 heading-font text-red-500">WITHDRAW</h2>
        <select id="wd-method" class="nova-input">
            <option value="JazzCash">JazzCash</option>
            <option value="EasyPaisa">EasyPaisa</option>
            <option value="Binance Pay">Binance Pay</option>
            <option value="MetaMask">MetaMask</option>
            <option value="Trust Wallet">Trust Wallet</option>
        </select>
        <input type="number" id="wd-amt" placeholder="Extraction Amount ($)" class="nova-input">
        <input type="text" id="wd-user" placeholder="Account Title" class="nova-input">
        <input type="text" id="wd-acc" placeholder="Account Number / Wallet" class="nova-input">
        <button onclick="requestFin('Withdrawal')" class="w-full bg-white text-black font-black py-5 rounded-2xl uppercase text-[11px]">Authorize Extract</button>
    </div>

    <div id="admin-panel" class="hidden fixed inset-0 bg-black z-[9999] p-6 overflow-y-auto">
        <div class="flex justify-between items-center mb-10 border-b border-white/5 pb-5">
            <h2 class="gold-gradient font-black uppercase tracking-widest text-sm">System Admin</h2>
            <button onclick="location.reload()" class="text-red-500 font-black text-xs">EXIT</button>
        </div>
        <div id="adm-list" class="space-y-4"></div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);
        let activeUser = null; let isLogin = true;

        const addrs = { "Trust": "0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37", "Meta": "0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2", "Binance": "0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37", "Jazz": "0300-XXXXXXX" };
        window.updateAddr = () => { document.getElementById('dep-addr').innerText = addrs[document.getElementById('dep-method').value]; };

        window.auth = async () => {
            const id = document.getElementById('u-email').value.toLowerCase().trim();
            const pw = document.getElementById('u-pass').value;
            const ak = document.getElementById('adm-key').value;
            if(ak === "nov786") { document.getElementById('admin-panel').classList.remove('hidden'); loadAdmin(); return; }
            if(!id || !pw) return alert("Empty Fields");
            const uRef = doc(db, "users", id); const s = await getDoc(uRef);
            if(!isLogin) {
                if(s.exists()) return alert("Identity Exists");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, status: 'Active' });
                start(id);
            } else if(s.exists() && s.data().password === pw) {
                start(id);
            } else alert("Invalid Passkey");
        };

        function start(id) {
            activeUser = id; document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app').classList.remove('hidden');
            document.getElementById('user-display').innerText = "ID: " + id;
            onSnapshot(doc(db, "users", id), d => {
                document.getElementById('main-bal').innerText = "$" + (d.data().balance || 0).toFixed(2);
                document.getElementById('v-daily').innerText = "+$" + (d.data().daily || 0).toFixed(2) + " Daily";
            });
            renderNodes();
        }

        window.requestFin = async (type) => {
            const uRef = doc(db, "users", activeUser); const s = await getDoc(uRef);
            let payload = { user: activeUser, type, status: 'Pending', time: Date.now() };
            if(type === 'Deposit') {
                payload.amount = parseFloat(document.getElementById('dep-amt').value);
                payload.method = document.getElementById('dep-method').value;
                payload.username = document.getElementById('dep-user').value;
                payload.tid = document.getElementById('dep-tid').value;
                const file = document.getElementById('dep-proof').files[0];
                if(file) {
                    const reader = new FileReader();
                    reader.onloadend = async () => { payload.proof = reader.result; await addDoc(collection(db, "logs"), payload); alert("Transmitted to Ledger"); closeModal('dep-modal'); };
                    reader.readAsDataURL(file);
                } else { await addDoc(collection(db, "logs"), payload); alert("Transmitted"); closeModal('dep-modal'); }
            } else {
                payload.amount = parseFloat(document.getElementById('wd-amt').value);
                payload.method = document.getElementById('wd-method').value;
                payload.acc = document.getElementById('wd-acc').value;
                if(s.data().balance < payload.amount) return alert("Liquidity Error");
                await updateDoc(uRef, { balance: s.data().balance - payload.amount });
                await addDoc(collection(db, "logs"), payload); alert("Extraction Authorized"); closeModal('wd-modal');
            }
        };

        function renderNodes() {
            const c = document.getElementById('nodes-container'); c.innerHTML = "";
            const names = ["Antminer S21", "Whatsminer M60", "Avalon Nano", "IceRiver KS3", "Quantum Core"];
            for(let i=1; i<=20; i++) {
                let p = i===1?15:(i*90); let y = (p*0.06).toFixed(2);
                c.innerHTML += `<div class="glass-card p-6 flex flex-col gap-6 relative overflow-hidden">
                    <div class="flex justify-between items-start">
                        <div><p class="text-[8px] font-black text-[#D4AF37] uppercase mb-1">${names[i%5]}</p><h4 class="text-lg font-black uppercase">RIG ALPHA v.${i}</h4></div>
                        <span class="status-pill">ONLINE</span>
                    </div>
                    <div class="flex justify-between items-end border-t border-white/5 pt-4">
                        <div><p class="text-[8px] text-gray-600 font-black uppercase">Lease Price</p><p class="text-2xl font-black">$${p}</p></div>
                        <button onclick="buyNode(${p},${y})" class="btn-main px-8 py-3">Deploy</button>
                    </div>
                </div>`;
            }
        }

        window.buyNode = async (p, d) => {
            const uRef = doc(db, "users", activeUser); const s = await getDoc(uRef);
            if(s.data().balance < p) return alert("Insufficient Liquidity");
            await updateDoc(uRef, { balance: s.data().balance - p, daily: (s.data().daily || 0) + parseFloat(d) });
            alert("Rig Online!");
        };

        function loadAdmin() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const al = document.getElementById('adm-list'); al.innerHTML = "";
                s.forEach(d => {
                    const r = d.data();
                    al.innerHTML += `<div class="glass-card p-6 border-l-4 ${r.type=='Deposit'?'border-green-500':'border-red-500'}">
                        <div class="flex justify-between mb-4"><span class="text-[10px] font-black uppercase text-gray-500">${r.type}</span><span class="text-xl font-black">$${r.amount}</span></div>
                        <p class="text-[9px] text-gray-400 mb-4 uppercase">USER: ${r.user} | TID: ${r.tid || r.acc}</p>
                        ${r.proof ? `<button onclick="window.open('${r.proof}')" class="text-blue-500 text-[10px] underline mb-4 block">View Proof Image</button>` : ''}
                        <div class="flex gap-2">
                            <button onclick="approve('${d.id}','${r.user}',${r.amount},'${r.type}')" class="flex-1 bg-green-600 py-3 rounded-xl text-[9px] font-black uppercase">Approve</button>
                            <button onclick="reject('${d.id}','${r.user}',${r.amount},'${r.type}')" class="flex-1 bg-red-600 py-3 rounded-xl text-[9px] font-black uppercase">Reject</button>
                        </div>
                    </div>`;
                });
            });
        }

        window.approve = async (id, u, a, t) => { if(t === 'Deposit') { const uR = doc(db, "users", u); const us = await getDoc(uR); await updateDoc(uR, { balance: (us.data().balance || 0) + a }); } await updateDoc(doc(db, "logs", id), { status: 'Success' }); };
        window.reject = async (id, u, a, t) => { if(t === 'Withdrawal') { const uR = doc(db, "users", u); const us = await getDoc(uR); await updateDoc(uR, { balance: (us.data().balance || 0) + a }); } await updateDoc(doc(db, "logs", id), { status: 'Rejected' }); };
        window.handleTaps = () => { window.taps = (window.taps || 0) + 1; if(window.taps >= 6) document.getElementById('adm-key').classList.remove('hidden'); };
        window.toggleM = () => { isLogin = !isLogin; document.getElementById('m-text').innerText = isLogin ? "Register New Entity" : "Back to Terminal"; };
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
