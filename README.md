<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA QUANTUM | Master Enterprise</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --dark-bg: #030712; --card-bg: #111827; --neon: #22c55e; }
        body { background: var(--dark-bg); font-family: 'Plus Jakarta Sans', sans-serif; color: #f8fafc; overflow-x: hidden; touch-action: manipulation; }
        
        .glass { background: rgba(17, 24, 39, 0.8); backdrop-filter: blur(12px); border: 1px solid rgba(255, 255, 255, 0.05); border-radius: 28px; }
        .node-img { width: 100%; height: 160px; object-fit: cover; border-radius: 20px; filter: contrast(1.1) brightness(0.8); }
        
        .btn-gold { background: linear-gradient(135deg, #B8860B, #D4AF37); color: #000; border-radius: 16px; font-weight: 800; transition: 0.3s; }
        .btn-gold:active { transform: scale(0.95); }
        
        .timer-box { font-family: 'monospace'; background: rgba(0,0,0,0.5); padding: 5px 12px; border-radius: 10px; color: var(--neon); font-size: 12px; border: 1px solid var(--neon); }
        
        .screen { display: none; animation: slideUp 0.6s cubic-bezier(0.16, 1, 0.3, 1); padding-bottom: 110px; }
        .active-screen { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(40px); } to { opacity: 1; transform: translateY(0); } }

        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(3, 7, 18, 0.95); backdrop-filter: blur(20px); border-top: 1px solid rgba(255,255,255,0.05); display: flex; justify-content: space-around; padding: 18px; z-index: 1000; }
        .nav-item { color: #4b5563; font-size: 10px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 6px; }
        .nav-item.active { color: var(--gold); }
        
        .p-input { background: rgba(31, 41, 55, 0.5); border: 1px solid rgba(255,255,255,0.1); width: 100%; padding: 18px; border-radius: 18px; outline: none; margin-bottom: 14px; color: white; font-weight: 600; }
        .p-input:focus { border-color: var(--gold); }
    </style>
</head>
<body>

    <div id="auth-screen" class="min-h-screen flex flex-col p-8 justify-center active-screen">
        <div class="text-center mb-12">
            <h1 id="logo-tap" class="text-7xl font-black tracking-tighter text-white">NO<span class="text-[#D4AF37]">VA</span></h1>
            <p class="text-[9px] text-slate-500 font-black uppercase tracking-[10px] mt-3 italic">Global Quantum Yields</p>
        </div>
        <div class="space-y-4">
            <input type="text" id="u-id" placeholder="Partner ID" class="p-input">
            <input type="password" id="u-pw" placeholder="Access Key" class="p-input">
            <div id="adm-box" class="hidden"><input type="password" id="adm-key" placeholder="ROOT KEY" class="p-input border-red-900 bg-red-950/20"></div>
            <button onclick="handleAuth()" class="w-full btn-gold py-5 shadow-2xl shadow-yellow-600/10 uppercase tracking-widest">Authorize Session</button>
            <p onclick="toggleMode()" id="mode-text" class="text-center text-[10px] text-slate-500 font-black uppercase mt-8 cursor-pointer tracking-widest">Establish New Identity</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-6 flex justify-between items-center sticky top-0 bg-[#030712]/90 backdrop-blur-md z-50">
            <h2 class="font-black text-2xl">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div id="user-tag" class="text-[9px] font-black bg-[#D4AF37] text-black px-4 py-2 rounded-full uppercase tracking-tighter">@PARTNER</div>
        </header>

        <div id="page-dash" class="screen active-screen px-4 pt-4">
            <div class="glass p-8 mb-6 relative overflow-hidden">
                <div class="absolute top-0 right-0 p-4 opacity-5"><i class="fa-solid fa-microchip text-9xl"></i></div>
                <p class="text-[10px] text-slate-500 font-black uppercase tracking-[5px] mb-2">Portfolio Value</p>
                <h1 id="b-main" class="text-6xl font-black tracking-tighter mb-10 text-white">$0.00</h1>
                <div class="grid grid-cols-2 gap-4">
                    <div class="bg-white/5 p-4 rounded-2xl border border-white/5"><p class="text-[8px] text-slate-500 uppercase font-bold mb-1">24H Harvest</p><p id="b-daily" class="text-xl font-black text-green-400">+$0.00</p></div>
                    <div class="bg-white/5 p-4 rounded-2xl border border-white/5"><p class="text-[8px] text-slate-500 uppercase font-bold mb-1">Total Yield</p><p id="b-total" class="text-xl font-black text-[#D4AF37]">$0.00</p></div>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-6">
                <button onclick="openModal('modal-dep')" class="glass p-6 flex flex-col items-center gap-4 hover:border-yellow-500/50"><i class="fa-solid fa-plus-circle text-3xl text-[#D4AF37]"></i><span class="text-[10px] font-black tracking-widest">DEPOSIT</span></button>
                <button onclick="openModal('modal-wd')" class="glass p-6 flex flex-col items-center gap-4 hover:border-yellow-500/50"><i class="fa-solid fa-arrow-up-from-bracket text-3xl text-slate-500"></i><span class="text-[10px] font-black tracking-widest">EXTRACT</span></button>
            </div>
            
            <div class="glass p-5 flex items-center gap-4 mb-6 border-l-4 border-blue-500">
                <i class="fa-solid fa-fingerprint text-blue-500 text-2xl"></i>
                <div><h4 class="text-[10px] font-black uppercase">Session Active</h4><p class="text-[9px] text-slate-500">Your mining hardware is protected by Nova Shield Protocol.</p></div>
            </div>
        </div>

        <div id="page-nodes" class="screen px-4 pt-4">
            <h2 class="text-2xl font-black mb-8 italic uppercase tracking-tighter">Hardware <span class="text-[#D4AF37]">Inventory</span></h2>
            <div id="nodes-grid" class="grid grid-cols-1 gap-8">
                </div>
        </div>

        <div id="page-logs" class="screen px-4 pt-4">
            <h2 class="text-2xl font-black mb-8 uppercase italic">Network <span class="text-[#D4AF37]">Logs</span></h2>
            <div id="logs-list" class="space-y-4"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('page-dash', this)" class="nav-item active"><i class="fa-solid fa-house text-xl"></i><span>HUB</span></div>
            <div onclick="nav('page-nodes', this)" class="nav-item"><i class="fa-solid fa-server text-xl"></i><span>MINING</span></div>
            <div onclick="nav('page-logs', this)" class="nav-item"><i class="fa-solid fa-list-check text-xl"></i><span>LOGS</span></div>
            <div onclick="logout()" class="nav-item"><i class="fa-solid fa-power-off text-xl text-red-900"></i><span>OUT</span></div>
        </nav>
    </div>

    <div id="modal-dep" class="fixed inset-0 bg-black/95 z-[2000] p-6 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-12"><h2 class="text-3xl font-black">FUNDING</h2><i onclick="closeModal('modal-dep')" class="fa-solid fa-xmark text-3xl"></i></div>
        <div class="glass p-6 border-l-4 border-blue-500 mb-6">
            <p class="text-[10px] font-black text-blue-500 mb-2 uppercase">MetaMask / Trust (BEP20)</p>
            <p class="text-xs font-mono break-all text-slate-300">0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37</p>
            <button onclick="copy('0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37')" class="mt-4 text-[9px] font-black bg-white/5 py-3 px-6 rounded-xl border border-white/10 uppercase">Copy Channel</button>
        </div>
        <input type="number" id="in-amt" placeholder="USDT Amount" class="p-input">
        <input type="text" id="in-ref" placeholder="Transaction Hash (TxID)" class="p-input">
        <button onclick="submitFin('Deposit')" class="w-full btn-gold py-5 mt-4">SUBMIT VERIFICATION</button>
    </div>

    <div id="modal-wd" class="fixed inset-0 bg-black/95 z-[2000] p-6 hidden">
        <div class="flex justify-between items-center mb-12"><h2 class="text-3xl font-black">EXTRACTION</h2><i onclick="closeModal('modal-wd')" class="fa-solid fa-xmark text-3xl"></i></div>
        <input type="number" id="out-amt" placeholder="Amount to Extract" class="p-input">
        <input type="text" id="out-addr" placeholder="Recipient Wallet Address" class="p-input">
        <button onclick="submitFin('Withdrawal')" class="w-full btn-gold py-5 bg-slate-900 !text-white mt-4">CONFIRM PAYOUT</button>
    </div>

    <div id="admin-panel" class="fixed inset-0 bg-slate-50 text-black z-[5000] p-6 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-12"><h1 class="text-3xl font-black italic">ROOT ADMIN HUB</h1><button onclick="location.reload()" class="bg-red-600 text-white px-8 py-3 rounded-2xl font-black uppercase text-[10px]">Close Connection</button></div>
        
        <div class="mb-12">
            <h3 class="text-xs font-black uppercase text-red-600 mb-6 tracking-widest border-b pb-2">Blockchain Queue (<span id="req-count">0</span>)</h3>
            <div id="admin-req-list" class="space-y-6"></div>
        </div>

        <div class="p-8 bg-white border-2 border-slate-200 rounded-[32px]">
            <h3 class="text-xs font-black uppercase text-slate-400 mb-6 tracking-widest">Manual Asset Injection</h3>
            <input type="text" id="adm-target" placeholder="Target User ID" class="p-input !bg-slate-100 !text-black !border-slate-200">
            <input type="number" id="adm-val" placeholder="Injection Amount" class="p-input !bg-slate-100 !text-black !border-slate-200">
            <button onclick="injectFunds()" class="w-full bg-black text-white py-5 rounded-2xl font-black uppercase tracking-widest">Execute Injection</button>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        // Firebase Configuration
        const config = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(config); const db = getFirestore(app);
        let user = localStorage.getItem('nova_user'); let isLogin = true;

        // Auth Logic
        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(document.getElementById('adm-key').value === "NOVA786") { openAdmin(); return; }
            if(!id || !pw) return alert("Credentials required");
            
            const uR = doc(db, "users", id); const s = await getDoc(uR);
            if(!isLogin) {
                if(s.exists()) return alert("User already linked");
                await setDoc(uR, { balance: 0, password: pw, daily: 0, total_profit: 0, active_nodes: [] });
                localStorage.setItem('nova_user', id); startApp(id);
            } else if(s.exists() && s.data().password === pw) {
                localStorage.setItem('nova_user', id); startApp(id);
            } else alert("Authorization Failed");
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
            loadUserLogs();
            setInterval(checkMiningProgress, 1000);
        }

        // Modern Nodes Configuration
        const hardwareConfig = [
            { id: 1, name: "Core Node Alpha", cost: 40, yield: 3.2, img: "https://images.unsplash.com/photo-1558494949-ef010cbdcc51?q=80&w=400" },
            { id: 2, name: "Titan Rig X", cost: 100, yield: 9.5, img: "https://images.unsplash.com/photo-1639322537228-f710d846310a?q=80&w=400" },
            { id: 3, name: "Quantum Blade", cost: 300, yield: 32.0, img: "https://images.unsplash.com/photo-1518770660439-4636190af475?q=80&w=400" },
            { id: 4, name: "Apex Data Center", cost: 800, yield: 95.0, img: "https://images.unsplash.com/photo-1591844339942-3064e9042ce5?q=80&w=400" },
            { id: 5, name: "Neural Network Hub", cost: 2000, yield: 260.0, img: "https://images.unsplash.com/photo-1620712943543-bcc4688e7485?q=80&w=400" }
        ];

        function genNodes(activeList) {
            const c = document.getElementById('nodes-grid'); c.innerHTML = "";
            hardwareConfig.forEach(n => {
                const activeEntry = activeList.find(a => a.nodeId === n.id);
                c.innerHTML += `
                <div class="glass p-6 relative">
                    <img src="${n.img}" class="node-img mb-6">
                    <div class="flex justify-between items-end">
                        <div>
                            <h3 class="text-lg font-black text-white uppercase mb-1">${n.name}</h3>
                            <p class="text-green-500 font-bold text-xs">Profit: +$${n.yield.toFixed(2)}/24h</p>
                        </div>
                        ${activeEntry ? 
                            `<div class="timer-box" id="timer-${n.id}"><i class="fa-solid fa-clock"></i> 24:00:00</div>` : 
                            `<button onclick="buyNode(${n.id}, ${n.cost}, ${n.yield})" class="btn-gold px-8 py-3 text-[10px] uppercase">Buy $${n.cost}</button>`
                        }
                    </div>
                    ${activeEntry ? `<div class="mt-4 text-[8px] font-black text-slate-500 tracking-widest uppercase">Node Health: 100% • Status: Mining</div>` : ""}
                </div>`;
            });
        }

        window.buyNode = async (nodeId, cost, yieldVal) => {
            const uR = doc(db, "users", user); const s = await getDoc(uR);
            if(s.data().balance < cost) return alert("Liquidity Error: Low Funds");
            const activeNode = { nodeId, yield: yieldVal, lastClaim: Date.now() };
            await updateDoc(uR, { 
                balance: s.data().balance - cost, 
                active_nodes: [...(s.data().active_nodes || []), activeNode],
                daily: (s.data().daily || 0) + yieldVal
            });
            alert("Hardware linked successfully!");
        };

        // Automated Profit Timer Logic
        async function checkMiningProgress() {
            const uR = doc(db, "users", user); const s = await getDoc(uR);
            if(!s.exists()) return;
            const data = s.data(); let nodes = data.active_nodes || []; let syncNeeded = false;

            nodes.forEach(n => {
                const diff = Date.now() - n.lastClaim;
                const remaining = (24 * 60 * 60 * 1000) - diff;
                const timerEl = document.getElementById(`timer-${n.nodeId}`);
                
                if(timerEl) {
                    if(remaining > 0) {
                        const h = Math.floor(remaining/3600000).toString().padStart(2,'0');
                        const m = Math.floor((remaining%3600000)/60000).toString().padStart(2,'0');
                        const sc = Math.floor((remaining%60000)/1000).toString().padStart(2,'0');
                        timerEl.innerHTML = `<i class="fa-solid fa-clock"></i> ${h}:${m}:${sc}`;
                    } else {
                        n.lastClaim = Date.now();
                        data.balance += n.yield;
                        data.total_profit += n.yield;
                        syncNeeded = true;
                    }
                }
            });

            if(syncNeeded) await updateDoc(uR, { balance: data.balance, total_profit: data.total_profit, active_nodes: nodes });
        }

        // Finance Handlers
        window.submitFin = async (type) => {
            const a = parseFloat(document.getElementById(type=='Deposit'?'in-amt':'out-amt').value);
            const r = document.getElementById(type=='Deposit'?'in-ref':'out-addr').value;
            if(!a || !r) return alert("Validation Failed: Empty Fields");
            await addDoc(collection(db, "logs"), { user, type, amount: a, status: 'Processing', time: Date.now(), ref: r });
            alert("Sent to Blockchain Queue!"); closeModal(type=='Deposit'?'modal-dep':'modal-wd');
        };

        function loadUserLogs() {
            onSnapshot(query(collection(db, "logs"), where("user", "==", user)), s => {
                const c = document.getElementById('logs-list'); c.innerHTML = "";
                s.forEach(d => {
                    const l = d.data();
                    c.innerHTML += `<div class="glass p-5 flex justify-between items-center border-l-2 ${l.status=='Success'?'border-green-500':'border-yellow-500'}"><div><p class="text-[9px] font-black text-slate-500 uppercase">${l.type}</p><p class="text-sm font-black">$${l.amount}</p></div><span class="text-[9px] font-black uppercase text-yellow-500">${l.status}</span></div>`;
                });
            });
        }

        // Admin Engine
        window.openAdmin = () => { document.getElementById('admin-panel').classList.remove('hidden'); loadAdminQueue(); };
        function loadAdminQueue() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Processing")), s => {
                const c = document.getElementById('admin-req-list'); c.innerHTML = "";
                document.getElementById('req-count').innerText = s.size;
                s.forEach(d => {
                    const l = d.data(); const id = d.id;
                    c.innerHTML += `
                    <div class="p-6 bg-white border border-slate-200 rounded-[28px] shadow-sm">
                        <div class="flex justify-between mb-4"><div><p class="text-[10px] font-bold text-slate-400">@${l.user.toUpperCase()}</p><p class="text-xl font-black">${l.type}</p></div><p class="text-xl font-black text-blue-600">$${l.amount}</p></div>
                        <p class="text-[10px] font-mono bg-slate-100 p-3 rounded-xl mb-6 break-all">${l.ref}</p>
                        <div class="flex gap-3"><button onclick="approveReq('${id}','${l.user}',${l.amount},'${l.type}')" class="flex-1 bg-green-500 text-white py-4 rounded-xl font-black text-[10px]">APPROVE</button><button onclick="rejectReq('${id}')" class="flex-1 bg-slate-200 py-4 rounded-xl font-black text-[10px]">REJECT</button></div>
                    </div>`;
                });
            });
        }

        window.approveReq = async (docId, target, amt, type) => {
            const uR = doc(db, "users", target); const s = await getDoc(uR);
            if(s.exists()){
                let nb = s.data().balance;
                if(type === 'Deposit') nb += amt; else nb -= amt;
                await updateDoc(uR, { balance: nb });
                await updateDoc(doc(db, "logs", docId), { status: 'Success' });
            }
        };

        window.rejectReq = async (id) => await updateDoc(doc(db, "logs", id), { status: 'Rejected' });
        window.injectFunds = async () => {
            const t = document.getElementById('adm-target').value.toLowerCase().trim();
            const v = parseFloat(document.getElementById('adm-val').value);
            const uR = doc(db, "users", t); const s = await getDoc(uR);
            if(s.exists()){ await updateDoc(uR, { balance: s.data().balance + v }); alert("Asset Injected!"); }
        };

        // UI Utilities
        window.nav = (id, el) => { document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen')); document.getElementById(id).classList.add('active-screen'); document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active')); el.classList.add('active'); };
        window.toggleMode = () => { isLogin = !isLogin; document.getElementById('mode-text').innerText = isLogin ? "Establish New Identity" : "Authorize Linked Partner"; };
        window.openModal = (id) => document.getElementById(id).style.display = 'block';
        window.closeModal = (id) => document.getElementById(id).style.display = 'none';
        window.logout = () => { localStorage.removeItem('nova_user'); location.reload(); };
        window.copy = (t) => { navigator.clipboard.writeText(t); alert("Network Address Copied!"); };
        
        let taps = 0; document.getElementById('logo-tap').onclick = () => { taps++; if(taps >= 6) document.getElementById('adm-box').classList.remove('hidden'); };
    </script>
</body>
</html>
