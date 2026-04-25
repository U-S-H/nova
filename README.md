<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | Quantum Ecosystem</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;700;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --bg: #0F172A; }
        body { background: #0b0f1a; font-family: 'Plus Jakarta Sans', sans-serif; color: white; -webkit-tap-highlight-color: transparent; }
        .glass { background: rgba(255, 255, 255, 0.03); backdrop-filter: blur(10px); border: 1px solid rgba(255,255,255,0.05); border-radius: 24px; }
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 16px; font-weight: 800; border: none; box-shadow: 0 4px 15px rgba(212, 175, 55, 0.3); }
        .screen { display: none; padding-bottom: 110px; }
        .active-screen { display: block !important; animation: slideUp 0.4s ease; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(15, 23, 42, 0.9); backdrop-filter: blur(15px); border-top: 1px solid rgba(255,255,255,0.1); display: flex; justify-content: space-around; padding: 15px 5px; z-index: 1000; }
        .nav-item { color: #64748b; font-size: 9px; font-weight: 700; display: flex; flex-direction: column; align-items: center; gap: 4px; }
        .nav-item.active { color: var(--gold); }
        input { background: rgba(255,255,255,0.05) !important; border: 1px solid rgba(255,255,255,0.1) !important; color: white !important; border-radius: 14px !important; }
        .node-img { width: 100%; height: 120px; object-fit: cover; border-radius: 18px; margin-bottom: 12px; }
    </style>
</head>
<body>

    <div id="auth-screen" class="flex flex-col justify-center items-center min-h-screen p-8 text-center bg-[#0b0f1a]">
        <img src="https://images.unsplash.com/photo-1639762681485-074b7f938ba0?auto=format&fit=crop&q=80&w=200" class="w-24 h-24 rounded-full mb-6 border-2 border-[#D4AF37] p-1">
        <h1 class="text-4xl font-black tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
        <p class="text-[9px] text-slate-500 font-bold uppercase tracking-[0.3em] mb-10">Quantum Asset Management</p>
        
        <div class="space-y-4 w-full max-w-xs">
            <input type="text" id="u-id" placeholder="Username" class="w-full p-4 outline-none">
            <input type="password" id="u-pw" placeholder="Password" class="w-full p-4 outline-none">
            <div id="ref-field" class="hidden">
                <input type="text" id="u-ref" placeholder="Referral Code" class="w-full p-4 outline-none text-[#D4AF37]">
            </div>
            <button onclick="handleAuth()" class="w-full btn-gold py-4 uppercase text-xs tracking-widest mt-2">Access Terminal</button>
            <p onclick="toggleAuth()" id="auth-btn-text" class="text-[10px] text-slate-500 font-bold uppercase cursor-pointer underline mt-6">Initialize Corporate Account</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center sticky top-0 bg-[#0b0f1a]/80 backdrop-blur-md z-[100] border-b border-white/5">
            <div>
                <h2 id="logo-main" class="font-black text-xl tracking-tighter cursor-pointer">NO<span class="text-[#D4AF37]">VA</span></h2>
                <p class="text-[8px] font-bold text-slate-500 uppercase">Worldwide Nodes</p>
            </div>
            <div class="text-right">
                <p id="user-display" class="text-[10px] font-black text-[#D4AF37]">@USER</p>
                <div class="flex items-center gap-1 justify-end"><div class="w-1.5 h-1.5 rounded-full bg-green-500 shadow-[0_0_8px_#22c55e]"></div><span class="text-[8px] font-bold text-slate-400 uppercase">Live</span></div>
            </div>
        </header>

        <div id="page-dash" class="screen active-screen px-4 pt-6">
            <div class="glass p-8 mb-6 relative overflow-hidden bg-gradient-to-br from-slate-900 to-black">
                <div class="absolute -right-10 -top-10 w-32 h-32 bg-[#D4AF37] opacity-10 rounded-full blur-3xl"></div>
                <p class="text-[8px] text-slate-500 font-bold uppercase tracking-widest mb-1">Portfolio Value</p>
                <h1 id="b-main" class="text-5xl font-extrabold tracking-tighter mb-8">$0.00</h1>
                <div class="grid grid-cols-2 gap-4 border-t border-white/5 pt-6">
                    <div><p class="text-[7px] text-slate-500 font-bold uppercase">Today's Earnings</p><p id="b-daily" class="text-lg font-black text-green-400">+$0.00</p></div>
                    <div class="text-right"><p class="text-[7px] text-slate-500 font-bold uppercase">Accumulated Profit</p><p id="b-total" class="text-lg font-black text-[#D4AF37]">$0.00</p></div>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-6">
                <div class="glass p-4">
                    <p class="text-[8px] font-bold text-slate-500 mb-2 uppercase">Promo System</p>
                    <input type="text" id="promo-input" placeholder="CODE" class="w-full p-2 text-[10px] mb-2 bg-white/5 border-none">
                    <button onclick="applyPromo()" class="w-full btn-gold py-2 text-[9px]">APPLY</button>
                </div>
                <div class="glass p-4 text-center flex flex-col justify-center">
                    <p class="text-[8px] font-bold text-slate-500 mb-1 uppercase">Your Referrals</p>
                    <h3 id="team-count" class="text-xl font-black text-[#D4AF37]">0</h3>
                    <button onclick="copyRef()" class="text-[8px] font-bold text-white/50 underline">COPY LINK</button>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-6">
                <button onclick="openModal('modal-dep')" class="glass p-6 flex flex-col items-center gap-2 border-green-500/20"><i class="fa-solid fa-arrow-down text-green-500"></i><span class="text-[10px] font-black">DEPOSIT</span></button>
                <button onclick="openModal('modal-wd')" class="glass p-6 flex flex-col items-center gap-2 border-red-500/20"><i class="fa-solid fa-arrow-up text-red-500"></i><span class="text-[10px] font-black">WITHDRAW</span></button>
            </div>
        </div>

        <div id="page-nodes" class="screen px-4 pt-6">
            <h2 class="text-xl font-black mb-6 px-2">QUANTUM <span class="text-[#D4AF37]">NODES</span></h2>
            <div id="nodes-grid" class="grid grid-cols-1 gap-4">
                </div>
        </div>

        <div id="page-logs" class="screen px-4 pt-6">
            <h2 class="text-xl font-black mb-6 px-2 text-slate-400">LEDGER <span class="text-white">ACTIVITY</span></h2>
            <div id="logs-list" class="space-y-3"></div>
        </div>

        <div id="page-info" class="screen px-4 pt-6">
            <div class="glass p-6 mb-4">
                <h3 class="font-black text-[#D4AF37] mb-2">Corporate Profile</h3>
                <p class="text-[11px] text-slate-400 leading-relaxed">Nova Corp is a global liquidity provider utilizing quantum computing for automated asset yield generation across 48 countries.</p>
            </div>
            <div class="glass p-4 space-y-4">
                <details class="text-[11px]"><summary class="font-bold cursor-pointer">What is the Minimum Limit?</summary><p class="mt-2 text-slate-500">Min Deposit: $10 | Min Withdraw: $5.</p></details>
                <details class="text-[11px]"><summary class="font-bold cursor-pointer">Privacy & Security</summary><p class="mt-2 text-slate-500">All transactions are end-to-end encrypted and managed by decentralized protocols.</p></details>
            </div>
            <button onclick="nav('page-chat', this)" class="w-full btn-gold py-4 mt-6 uppercase text-[10px]">Open Real-time Support</button>
        </div>

        <div id="page-chat" class="screen px-4 pt-6">
            <div class="glass flex flex-col h-[70vh]">
                <div id="chat-box" class="flex-1 p-4 space-y-4 overflow-y-auto text-[12px]"></div>
                <div class="p-4 border-t border-white/10 flex gap-2">
                    <input type="text" id="chat-msg" placeholder="Message agent..." class="flex-1 p-3 text-xs">
                    <button onclick="sendChat()" class="btn-gold px-4"><i class="fa-solid fa-paper-plane"></i></button>
                </div>
            </div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('page-dash', this)" class="nav-item active"><i class="fa-solid fa-house-chimney-window text-base"></i><span>HOME</span></div>
            <div onclick="nav('page-nodes', this)" class="nav-item"><i class="fa-solid fa-microchip text-base"></i><span>NODES</span></div>
            <div onclick="nav('page-logs', this)" class="nav-item"><i class="fa-solid fa-fingerprint text-base"></i><span>LOGS</span></div>
            <div onclick="nav('page-info', this)" class="nav-item"><i class="fa-solid fa-circle-info text-base"></i><span>INFO</span></div>
            <div id="nav-admin-btn" onclick="nav('page-admin', this)" class="nav-item text-red-500 hidden"><i class="fa-solid fa-user-shield text-base"></i><span>ADMIN</span></div>
        </nav>
    </div>

    <div id="modal-dep" class="fixed inset-0 bg-[#0b0f1a] z-[2000] p-8 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-10"><h2 class="font-black text-2xl tracking-tighter">DEPOSIT <span class="text-[#D4AF37]">ASSETS</span></h2><i onclick="closeModal('modal-dep')" class="fa-solid fa-xmark text-xl"></i></div>
        <p class="text-[10px] font-bold text-slate-500 mb-4">SELECT NETWORK</p>
        <div class="grid grid-cols-2 gap-3 mb-8">
            <button onclick="setDep('Binance Pay')" class="glass p-4 text-[10px] font-bold">Binance Pay</button>
            <button onclick="setDep('EasyPaisa')" class="glass p-4 text-[10px] font-bold">EasyPaisa</button>
            <button onclick="setDep('JazzCash')" class="glass p-4 text-[10px] font-bold">JazzCash</button>
            <button onclick="setDep('SadaPay')" class="glass p-4 text-[10px] font-bold">SadaPay</button>
            <button onclick="setDep('USDT BEP20')" class="glass p-4 text-[10px] font-bold col-span-2">USDT (Trust/MetaMask)</button>
        </div>
        <div id="dep-details" class="hidden mb-8">
            <div class="glass p-5 text-center mb-6"><p class="text-[9px] text-slate-500 mb-1 uppercase">Transfer to:</p><h4 id="dep-address" class="font-black text-[#D4AF37] break-all text-sm"></h4></div>
            <input type="number" id="dep-amt" placeholder="Amount ($)" class="w-full p-4 mb-4 outline-none">
            <input type="text" id="dep-txid" placeholder="Transaction ID / Hash" class="w-full p-4 mb-8 outline-none">
            <button onclick="submitFin('Deposit')" class="w-full btn-gold py-4 text-xs">CONFIRM DEPOSIT</button>
        </div>
    </div>

    <div id="modal-wd" class="fixed inset-0 bg-[#0b0f1a] z-[2000] p-8 hidden">
        <div class="flex justify-between items-center mb-10"><h2 class="font-black text-2xl tracking-tighter">WITHDRAW <span class="text-red-500">FUNDS</span></h2><i onclick="closeModal('modal-wd')" class="fa-solid fa-xmark text-xl"></i></div>
        <div class="glass p-5 mb-8 border-red-500/10">
            <p class="text-[10px] text-slate-500 font-bold mb-2">NETWORK TAX (Auto-Cut)</p>
            <div class="flex justify-between items-end"><h4 id="tax-display" class="text-2xl font-black">5%</h4><p class="text-[9px] text-slate-400">Min: $5.00</p></div>
        </div>
        <input type="number" id="wd-amt" placeholder="Amount ($)" class="w-full p-4 mb-4 outline-none">
        <input type="text" id="wd-addr" placeholder="Wallet Address / Number" class="w-full p-4 mb-8 outline-none">
        <button onclick="submitFin('Withdraw')" class="w-full btn-gold py-4 text-xs">REQUEST DISBURSEMENT</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, orderBy } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);

        let user = localStorage.getItem('nova_session');
        let mode = 'login';
        const nodeTemplates = [
            {id:1, n:"Nano-Core V1", c:10, y:0.8, d:30, img:"https://images.unsplash.com/photo-1518770660439-4636190af475?w=400"},
            {id:2, n:"Quantum Rack", c:50, y:4.5, d:60, img:"https://images.unsplash.com/photo-1550751827-4bd374c3f58b?w=400"},
            {id:3, n:"Fusion Node", c:200, y:20, d:90, img:"https://images.unsplash.com/photo-1451187580459-43490279c0fa?w=400"}
        ];

        function checkSession() {
            if (user) {
                document.getElementById('auth-screen').classList.add('hidden');
                document.getElementById('app').classList.remove('hidden');
                initApp(user);
            }
        }

        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            const ref = document.getElementById('u-ref').value;
            if(!id || !pw) return alert("Missing credentials");

            const uRef = doc(db, "users", id); const snap = await getDoc(uRef);
            if(mode === 'login') {
                if(snap.exists() && snap.data().password === pw) { localStorage.setItem('nova_session', id); location.reload(); }
                else alert("Unauthorized");
            } else {
                if(snap.exists()) return alert("Username taken");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, total_profit: 0, nodes: [], team: 0, refBy: ref || 'direct' });
                localStorage.setItem('nova_session', id); location.reload();
            }
        };

        function initApp(id) {
            document.getElementById('user-display').innerText = `@${id}`;
            onSnapshot(doc(db, "users", id), d => {
                const u = d.data(); if(!u) return;
                document.getElementById('b-main').innerText = `$${u.balance.toFixed(2)}`;
                document.getElementById('b-daily').innerText = `+$${u.daily.toFixed(2)}`;
                document.getElementById('b-total').innerText = `$${u.total_profit.toFixed(2)}`;
                document.getElementById('team-count').innerText = u.team || 0;
                renderNodes(u.nodes || []);
            });
            loadLogs(id);
        }

        function renderNodes(activeNodes) {
            const grid = document.getElementById('nodes-grid'); grid.innerHTML = "";
            nodeTemplates.forEach(t => {
                const isOwned = activeNodes.find(an => an.id === t.id);
                grid.innerHTML += `<div class="glass p-5">
                    <img src="${t.img}" class="node-img">
                    <div class="flex justify-between items-start mb-4">
                        <div><h4 class="font-black uppercase text-xs">${t.n}</h4><p class="text-[9px] text-slate-500">Term: ${t.d} Days</p></div>
                        <div class="text-right"><p class="text-[10px] font-black text-[#D4AF37]">$${t.c}</p><p class="text-[8px] text-green-400">+$${t.y}/day</p></div>
                    </div>
                    ${isOwned ? `<button class="w-full py-3 bg-green-500/10 text-green-500 rounded-xl font-bold text-[9px] uppercase tracking-widest border border-green-500/20">ACTIVE & MINING</button>` : 
                    `<button onclick="buyNode(${t.id}, ${t.c}, ${t.y})" class="w-full btn-gold py-3 text-[9px] uppercase tracking-widest">DEPLOY NODE</button>`}
                </div>`;
            });
        }

        window.buyNode = async (id, cost, yieldVal) => {
            const uRef = doc(db, "users", user); const snap = await getDoc(uRef);
            if(snap.data().balance < cost) return alert("Insufficient Liquidity");
            await updateDoc(uRef, { 
                balance: snap.data().balance - cost, 
                nodes: [...(snap.data().nodes || []), {id, yield: yieldVal, date: Date.now()}],
                daily: snap.data().daily + yieldVal
            });
            await addDoc(collection(db, "logs"), { user, type: 'Node Purchase', amount: cost, status: 'Completed', time: Date.now() });
            alert("Hardware Successfully Deployed!");
        };

        window.setDep = (method) => {
            const addr = { 'EasyPaisa': '0300-XXXXXXX', 'JazzCash': '0312-XXXXXXX', 'Binance Pay': 'ID: 8872661', 'USDT BEP20': '0x7a...992' };
            document.getElementById('dep-address').innerText = addr[method] || "Contact Support";
            document.getElementById('dep-details').classList.remove('hidden');
        };

        window.submitFin = async (type) => {
            const amt = parseFloat(document.getElementById(type === 'Deposit' ? 'dep-amt' : 'wd-amt').value);
            const ref = document.getElementById(type === 'Deposit' ? 'dep-txid' : 'wd-addr').value;
            if(!amt || !ref) return alert("Fill data");
            await addDoc(collection(db, "logs"), { user, type, amount: amt, ref, status: 'Pending', time: Date.now() });
            alert("Request broadcasted to network."); closeModal(type === 'Deposit' ? 'modal-dep' : 'modal-wd');
        };

        function loadLogs(id) {
            onSnapshot(query(collection(db, "logs"), where("user", "==", id), orderBy("time", "desc")), s => {
                const box = document.getElementById('logs-list'); box.innerHTML = "";
                s.forEach(d => {
                    const l = d.data();
                    box.innerHTML += `<div class="glass p-4 flex justify-between items-center text-[10px]">
                        <div><p class="font-bold uppercase">${l.type}</p><p class="text-slate-500 text-[8px]">${new Date(l.time).toLocaleString()}</p></div>
                        <div class="text-right"><p class="font-black">$${l.amount.toFixed(2)}</p><p class="${l.status==='Pending'?'text-orange-500':'text-green-500'} font-bold uppercase text-[8px]">${l.status}</p></div>
                    </div>`;
                });
            });
        }

        window.nav = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            el.classList.add('active');
        };

        window.openModal = (id) => document.getElementById(id).classList.remove('hidden');
        window.closeModal = (id) => document.getElementById(id).classList.add('hidden');
        window.toggleAuth = () => { mode = mode === 'login' ? 'reg' : 'login'; document.getElementById('auth-btn-text').innerText = mode === 'login' ? "Initialize Corporate Account" : "Access Terminal"; document.getElementById('ref-field').classList.toggle('hidden'); };
        window.copyRef = () => { navigator.clipboard.writeText(window.location.href + "?ref=" + user); alert("Referral link copied!"); };

        checkSession();
    </script>
</body>
</html>
