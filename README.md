<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA ENTERPRISE | Quantum Mining</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --bg: #F9FAFB; --card: #FFFFFF; --text: #111827; }
        body { background: var(--bg); font-family: 'Plus Jakarta Sans', sans-serif; color: var(--text); overflow-x: hidden; touch-action: manipulation; }
        
        .premium-card { background: var(--card); border: 1px solid #E5E7EB; border-radius: 24px; box-shadow: 0 4px 25px rgba(0,0,0,0.02); transition: 0.3s; }
        .premium-card:active { transform: scale(0.98); }
        .node-img { width: 100%; height: 160px; object-fit: cover; border-radius: 20px; margin-bottom: 15px; }
        
        .btn-main { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 16px; font-weight: 800; transition: 0.3s; box-shadow: 0 10px 20px rgba(212, 175, 55, 0.2); }
        .timer-box { font-family: 'Courier New', Courier, monospace; background: #F3F4F6; padding: 6px 12px; border-radius: 10px; color: #D4AF37; font-size: 13px; font-weight: 800; border: 1px solid #E5E7EB; }
        
        .screen { display: none; animation: slideUp 0.5s cubic-bezier(0.16, 1, 0.3, 1); padding-bottom: 120px; }
        .active-screen { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }

        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255, 255, 255, 0.95); backdrop-filter: blur(20px); border-top: 1px solid #E5E7EB; display: flex; justify-content: space-around; padding: 20px; z-index: 1000; }
        .nav-item { color: #9CA3AF; font-size: 10px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 6px; }
        .nav-item.active { color: var(--gold); }
        
        .input-field { background: #FFFFFF; border: 2px solid #F3F4F6; width: 100%; padding: 18px; border-radius: 18px; outline: none; margin-bottom: 15px; color: #111827; font-weight: 600; }
        .input-field:focus { border-color: var(--gold); }
    </style>
</head>
<body>

    <div id="auth-screen" class="min-h-screen flex flex-col p-8 justify-center active-screen bg-white">
        <div class="text-center mb-12">
            <h1 id="logo-trigger" class="text-6xl font-black text-gray-900 tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <p class="text-[10px] text-gray-400 font-bold uppercase tracking-[10px] mt-3">Quantum Core Hub</p>
        </div>
        <div class="space-y-4">
            <input type="text" id="u-id" placeholder="Partner ID" class="input-field">
            <input type="password" id="u-pw" placeholder="Security Pass" class="input-field">
            <div id="adm-box" class="hidden"><input type="password" id="adm-key" placeholder="ROOT ACCESS KEY" class="input-field border-red-100"></div>
            <button onclick="handleAuth()" class="w-full btn-main py-5 uppercase tracking-widest">Establish Session</button>
            <p onclick="toggleMode()" id="mode-text" class="text-center text-[10px] text-gray-400 font-black uppercase mt-8 cursor-pointer tracking-widest">Register New Identity</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-6 flex justify-between items-center bg-white/90 backdrop-blur-md sticky top-0 z-50 border-b border-gray-100">
            <h2 class="font-black text-2xl tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div id="user-tag" class="text-[9px] font-black bg-gray-900 text-white px-5 py-2 rounded-full uppercase">@PARTNER</div>
        </header>

        <div id="page-dash" class="screen active-screen px-4 pt-6">
            <div class="bg-gray-900 rounded-[35px] p-10 mb-8 text-white shadow-2xl relative overflow-hidden">
                <div class="absolute top-0 right-0 p-6 opacity-10"><i class="fa-solid fa-microchip text-8xl"></i></div>
                <p class="text-[11px] text-gray-400 font-black uppercase tracking-[5px] mb-3">Available Liquidity</p>
                <h1 id="b-main" class="text-6xl font-black tracking-tighter mb-12">$0.00</h1>
                <div class="grid grid-cols-2 gap-6 pt-6 border-t border-white/5">
                    <div><p class="text-[9px] text-gray-500 uppercase font-bold mb-1">Daily Yield</p><p id="b-daily" class="text-2xl font-black text-green-400">+$0.00</p></div>
                    <div class="text-right"><p class="text-[9px] text-gray-500 uppercase font-bold mb-1">Total Profit</p><p id="b-total" class="text-2xl font-black text-[#D4AF37]">$0.00</p></div>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-5 mb-8">
                <button onclick="openModal('modal-dep')" class="premium-card p-8 flex flex-col items-center gap-4"><i class="fa-solid fa-circle-plus text-3xl text-[#D4AF37]"></i><span class="text-[11px] font-black uppercase">Deposit</span></button>
                <button onclick="openModal('modal-wd')" class="premium-card p-8 flex flex-col items-center gap-4"><i class="fa-solid fa-arrow-up-right-from-square text-3xl text-gray-400"></i><span class="text-[11px] font-black uppercase">Extract</span></button>
            </div>
            
            <div class="premium-card p-6 flex items-center gap-5 border-l-4 border-green-500">
                <i class="fa-solid fa-shield-check text-2xl text-green-500"></i>
                <p class="text-[10px] font-bold text-gray-500 uppercase italic">All mining nodes are currently optimized and operational.</p>
            </div>
        </div>

        <div id="page-nodes" class="screen px-4 pt-6">
            <h2 class="text-2xl font-black mb-8 uppercase tracking-tighter">Hardware <span class="text-[#D4AF37]">Catalogue</span></h2>
            <div id="nodes-grid" class="grid grid-cols-1 gap-8 pb-10"></div>
        </div>

        <div id="page-logs" class="screen px-4 pt-6">
            <h2 class="text-2xl font-black mb-8 uppercase">Network <span class="text-[#D4AF37]">Ledger</span></h2>
            <div id="logs-list" class="space-y-4"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('page-dash', this)" class="nav-item active"><i class="fa-solid fa-house-chimney text-xl"></i><span>HOME</span></div>
            <div onclick="nav('page-nodes', this)" class="nav-item"><i class="fa-solid fa-server text-xl"></i><span>NODES</span></div>
            <div onclick="nav('page-logs', this)" class="nav-item"><i class="fa-solid fa-receipt text-xl"></i><span>LOGS</span></div>
            <div onclick="logout()" class="nav-item"><i class="fa-solid fa-power-off text-xl text-red-500"></i><span>EXIT</span></div>
        </nav>
    </div>

    <div id="modal-dep" class="fixed inset-0 bg-white z-[2000] p-8 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-10"><h2 class="text-3xl font-black">FUNDING</h2><i onclick="closeModal('modal-dep')" class="fa-solid fa-xmark text-3xl"></i></div>
        
        <div class="space-y-5 mb-10">
            <div class="premium-card p-6 border-l-4 border-blue-600 bg-blue-50/30">
                <p class="text-[11px] font-black uppercase text-blue-600 mb-2">Trust Wallet (BEP20)</p>
                <p class="text-[12px] font-mono font-bold break-all text-gray-700">0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37</p>
                <button onclick="copy('0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37')" class="mt-4 text-[10px] font-black text-blue-700 underline uppercase">Copy Channel 1</button>
            </div>
            <div class="premium-card p-6 border-l-4 border-orange-600 bg-orange-50/30">
                <p class="text-[11px] font-black uppercase text-orange-600 mb-2">MetaMask (BEP20)</p>
                <p class="text-[12px] font-mono font-bold break-all text-gray-700">0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2</p>
                <button onclick="copy('0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2')" class="mt-4 text-[10px] font-black text-orange-700 underline uppercase">Copy Channel 2</button>
            </div>
        </div>

        <input type="number" id="in-amt" placeholder="Deposit Amount (USDT)" class="input-field">
        <input type="text" id="in-ref" placeholder="Transaction Hash / TxID" class="input-field">
        <button onclick="submitFin('Deposit')" class="w-full btn-main py-5 mt-4">SUBMIT FOR APPROVAL</button>
    </div>

    <div id="modal-wd" class="fixed inset-0 bg-white z-[2000] p-8 hidden">
        <div class="flex justify-between items-center mb-10"><h2 class="text-3xl font-black">WITHDRAW</h2><i onclick="closeModal('modal-wd')" class="fa-solid fa-xmark text-3xl"></i></div>
        <input type="number" id="out-amt" placeholder="Payout Amount" class="input-field">
        <input type="text" id="out-addr" placeholder="Destination Wallet Address" class="input-field">
        <button onclick="submitFin('Withdrawal')" class="w-full btn-main py-5 bg-gray-900 !text-white mt-4 uppercase">Request Extraction</button>
    </div>

    <div id="admin-panel" class="fixed inset-0 bg-gray-50 z-[5000] p-8 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-12"><h1 class="text-3xl font-black italic">SYSTEM ROOT</h1><button onclick="location.reload()" class="bg-black text-white px-8 py-3 rounded-2xl font-black uppercase text-[10px]">Close Hub</button></div>
        <div class="mb-12">
            <h3 class="text-xs font-black uppercase text-red-500 mb-6 tracking-widest border-b pb-2">Blockchain Queue (<span id="req-count">0</span>)</h3>
            <div id="admin-req-list" class="space-y-6"></div>
        </div>
        <div class="premium-card p-8 bg-white">
            <h3 class="text-xs font-black uppercase text-gray-400 mb-6">Manual Injection</h3>
            <input type="text" id="adm-target" placeholder="User ID" class="input-field !bg-gray-50">
            <input type="number" id="adm-val" placeholder="Value" class="input-field !bg-gray-50">
            <button onclick="injectFunds()" class="w-full bg-gray-900 text-white py-5 rounded-2xl font-black uppercase">Execute Injection</button>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        // Your specific Firebase configuration
        const firebaseConfig = {
            apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo",
            authDomain: "home-94d45.firebaseapp.com",
            databaseURL: "https://home-94d45-default-rtdb.firebaseio.com",
            projectId: "home-94d45",
            storageBucket: "home-94d45.firebasestorage.app",
            messagingSenderId: "964390949419",
            appId: "1:964390949419:web:589840cb91b7e42ecd506e"
        };

        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);
        let user = localStorage.getItem('nova_user'); 
        let isLogin = true;

        if(user) startApp(user);

        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(document.getElementById('adm-key').value === "NOVA786") { openAdmin(); return; }
            if(!id || !pw) return alert("Fill all security fields");
            
            const uR = doc(db, "users", id); const s = await getDoc(uR);
            if(!isLogin) {
                if(s.exists()) return alert("Partner ID already active");
                await setDoc(uR, { balance: 0, password: pw, daily: 0, total_profit: 0, active_nodes: [] });
                localStorage.setItem('nova_user', id); startApp(id);
            } else if(s.exists() && s.data().password === pw) {
                localStorage.setItem('nova_user', id); startApp(id);
            } else alert("Invalid Credentials");
        };

        function startApp(id) {
            user = id; document.getElementById('auth-screen').style.display='none'; document.getElementById('app').classList.remove('hidden');
            document.getElementById('user-tag').innerText = `@${id.toUpperCase()}`;
            onSnapshot(doc(db, "users", id), d => {
                const data = d.data();
                document.getElementById('b-main').innerText = "$" + (data.balance || 0).toFixed(2);
                document.getElementById('b-daily').innerText = "+$" + (data.daily || 0).toFixed(2);
                document.getElementById('b-total').innerText = "$" + (data.total_profit || 0).toFixed(2);
                genNodes(data.active_nodes || []);
            });
            loadHistory();
            setInterval(syncMining, 1000);
        }

        // 20+ Modern Hardware Nodes ($20 - $25,000)
        const nodeMarket = [
            { id: 1, name: "Core Alpha", cost: 20, yield: 1.5, img: "https://images.unsplash.com/photo-1558494949-ef010cbdcc51?q=80&w=400" },
            { id: 2, name: "Neon Rig", cost: 50, yield: 4.2, img: "https://images.unsplash.com/photo-1639322537228-f710d846310a?q=80&w=400" },
            { id: 3, name: "Bronze Blade", cost: 100, yield: 9.0, img: "https://images.unsplash.com/photo-1518770660439-4636190af475?q=80&w=400" },
            { id: 4, name: "Silver Server", cost: 250, yield: 24.5, img: "https://images.unsplash.com/photo-1591844339942-3064e9042ce5?q=80&w=400" },
            { id: 5, name: "Gold Processor", cost: 500, yield: 52.0, img: "https://images.unsplash.com/photo-1620712943543-bcc4688e7485?q=80&w=400" },
            { id: 6, name: "Titan X1", cost: 1000, yield: 110.0, img: "https://images.unsplash.com/photo-1558494949-ef010cbdcc51?q=80&w=400" },
            { id: 7, name: "Quantum Rig", cost: 2000, yield: 240.0, img: "https://images.unsplash.com/photo-1639322537228-f710d846310a?q=80&w=400" },
            { id: 8, name: "Nexus Hub", cost: 3500, yield: 450.0, img: "https://images.unsplash.com/photo-1518770660439-4636190af475?q=80&w=400" },
            { id: 9, name: "Apex Miner", cost: 5000, yield: 680.0, img: "https://images.unsplash.com/photo-1591844339942-3064e9042ce5?q=80&w=400" },
            { id: 10, name: "Nova Master", cost: 7500, yield: 1050.0, img: "https://images.unsplash.com/photo-1620712943543-bcc4688e7485?q=80&w=400" },
            { id: 11, name: "Ultra Core", cost: 10000, yield: 1450.0, img: "https://images.unsplash.com/photo-1558494949-ef010cbdcc51?q=80&w=400" },
            { id: 12, name: "Empire Blade", cost: 15000, yield: 2300.0, img: "https://images.unsplash.com/photo-1639322537228-f710d846310a?q=80&w=400" },
            { id: 13, name: "Zenith Node", cost: 20000, yield: 3100.0, img: "https://images.unsplash.com/photo-1518770660439-4636190af475?q=80&w=400" },
            { id: 14, name: "Quantum Galaxy", cost: 25000, yield: 4000.0, img: "https://images.unsplash.com/photo-1591844339942-3064e9042ce5?q=80&w=400" }
        ];

        function genNodes(activeList) {
            const c = document.getElementById('nodes-grid'); c.innerHTML = "";
            nodeMarket.forEach(n => {
                const isActive = activeList.find(a => a.nodeId === n.id);
                c.innerHTML += `
                <div class="premium-card p-6 relative">
                    <img src="${n.img}" class="node-img">
                    <div class="flex justify-between items-center">
                        <div>
                            <h4 class="font-black text-gray-900 uppercase text-sm">${n.name}</h4>
                            <p class="text-green-600 font-bold text-[11px]">Daily: +$${n.yield.toFixed(2)}</p>
                        </div>
                        ${isActive ? `<div class="timer-box" id="timer-${n.id}">24:00:00</div>` : `<button onclick="buyNode(${n.id}, ${n.cost}, ${n.yield})" class="btn-main px-8 py-3 text-[10px] uppercase font-black">$${n.cost}</button>`}
                    </div>
                </div>`;
            });
        }

        window.buyNode = async (nodeId, cost, y) => {
            const uR = doc(db, "users", user); const s = await getDoc(uR);
            if(s.data().balance < cost) return alert("Insufficient Capital");
            const entry = { nodeId, yield: y, lastClaim: Date.now() };
            await updateDoc(uR, { balance: s.data().balance - cost, active_nodes: [...(s.data().active_nodes || []), entry], daily: (s.data().daily || 0) + y });
            alert("Hardware extraction started!");
        };

        async function syncMining() {
            const uR = doc(db, "users", user); const s = await getDoc(uR); if(!s.exists()) return;
            let d = s.data(); let nodes = d.active_nodes || []; let sync = false;
            nodes.forEach(n => {
                const diff = Date.now() - n.lastClaim; const rem = (24*60*60*1000) - diff;
                const el = document.getElementById(`timer-${n.nodeId}`);
                if(el){
                    if(rem > 0){
                        const h = Math.floor(rem/3600000).toString().padStart(2,'0');
                        const m = Math.floor((rem%3600000)/60000).toString().padStart(2,'0');
                        const sc = Math.floor((rem%60000)/1000).toString().padStart(2,'0');
                        el.innerText = `${h}:${m}:${sc}`;
                    } else {
                        n.lastClaim = Date.now(); d.balance += n.yield; d.total_profit += n.yield; sync = true;
                    }
                }
            });
            if(sync) await updateDoc(uR, { balance: d.balance, total_profit: d.total_profit, active_nodes: nodes });
        }

        window.submitFin = async (t) => {
            const a = parseFloat(document.getElementById(t=='Deposit'?'in-amt':'out-amt').value);
            const r = document.getElementById(t=='Deposit'?'in-ref':'out-addr').value;
            if(!a || !r) return alert("Validation error");
            await addDoc(collection(db, "logs"), { user, type: t, amount: a, status: 'Processing', ref: r, time: Date.now() });
            alert("Sent to global ledger for verification!"); closeModal(t=='Deposit'?'modal-dep':'modal-wd');
        };

        function loadHistory() {
            onSnapshot(query(collection(db, "logs"), where("user", "==", user)), s => {
                const c = document.getElementById('logs-list'); c.innerHTML = "";
                s.forEach(doc => {
                    const l = doc.data();
                    c.innerHTML += `<div class="premium-card p-5 flex justify-between items-center border-l-4 ${l.status=='Success'?'border-green-500':'border-yellow-500'}"><div><p class="text-[9px] font-black text-gray-400 uppercase">${l.type}</p><p class="text-sm font-black">$${l.amount}</p></div><span class="text-[10px] font-bold uppercase text-yellow-600">${l.status}</span></div>`;
                });
            });
        }

        // ADMIN HUB FUNCTIONS
        window.openAdmin = () => { document.getElementById('admin-panel').classList.remove('hidden'); loadAdmin(); };
        function loadAdmin() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Processing")), s => {
                const c = document.getElementById('admin-req-list'); c.innerHTML = "";
                document.getElementById('req-count').innerText = s.size;
                s.forEach(doc => {
                    const l = doc.data(); const id = doc.id;
                    c.innerHTML += `<div class="bg-white p-6 border rounded-[25px] flex justify-between items-center shadow-sm"><div><p class="font-bold text-[10px] text-gray-400">@${l.user.toUpperCase()}</p><p class="font-black text-xl text-blue-600">$${l.amount}</p><p class="text-[8px] text-gray-400 mt-1">${l.type}</p></div><div class="flex gap-2"><button onclick="admOk('${id}','${l.user}',${l.amount},'${l.type}')" class="bg-green-500 text-white px-5 py-3 rounded-xl font-black text-[10px]">OK</button><button onclick="rej('${id}')" class="bg-gray-100 text-gray-400 px-5 py-3 rounded-xl font-black text-[10px]">X</button></div></div>`;
                });
            });
        }

        window.admOk = async (id, target, amt, type) => {
            const uR = doc(db, "users", target); const s = await getDoc(uR);
            if(s.exists()){
                let b = s.data().balance; if(type === 'Deposit') b += amt; else b -= amt;
                await updateDoc(uR, { balance: b }); await updateDoc(doc(db, "logs", id), { status: 'Success' });
            }
        };
        window.rej = async (id) => await updateDoc(doc(db, "logs", id), { status: 'Rejected' });
        window.injectFunds = async () => {
            const t = document.getElementById('adm-target').value.toLowerCase().trim();
            const v = parseFloat(document.getElementById('adm-val').value);
            const uR = doc(db, "users", t); const s = await getDoc(uR);
            if(s.exists()){ await updateDoc(uR, { balance: s.data().balance + v }); alert("Asset Injected Successfully"); }
        };

        window.nav = (id, el) => { document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen')); document.getElementById(id).classList.add('active-screen'); document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active')); el.classList.add('active'); };
        window.toggleMode = () => { isLogin = !isLogin; document.getElementById('mode-text').innerText = isLogin ? "Register New Identity" : "Authorize Existing Partner"; };
        window.openModal = (id) => document.getElementById(id).style.display = 'block';
        window.closeModal = (id) => document.getElementById(id).style.display = 'none';
        window.logout = () => { localStorage.removeItem('nova_user'); location.reload(); };
        window.copy = (t) => { navigator.clipboard.writeText(t); alert("Copy Successful!"); };
        let count = 0; document.getElementById('logo-trigger').onclick = () => { count++; if(count >= 6) document.getElementById('adm-box').classList.remove('hidden'); };
    </script>
</body>
</html>
