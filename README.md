<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA GOLD | Enterprise Mining</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --bg: #F8FAFC; --card: #FFFFFF; }
        body { background: var(--bg); font-family: 'Plus Jakarta Sans', sans-serif; color: #1E293B; overflow-x: hidden; touch-action: manipulation; }
        
        .clean-card { background: var(--card); border: 1px solid #E2E8F0; border-radius: 24px; box-shadow: 0 4px 20px rgba(0,0,0,0.03); }
        .node-img { width: 100%; height: 150px; object-fit: cover; border-radius: 20px; margin-bottom: 15px; }
        
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 14px; font-weight: 800; transition: 0.3s; box-shadow: 0 4px 15px rgba(212, 175, 55, 0.3); }
        .btn-gold:active { transform: scale(0.96); }
        
        .timer-box { font-family: 'monospace'; background: #F1F5F9; padding: 6px 12px; border-radius: 10px; color: #D4AF37; font-size: 12px; font-weight: 800; border: 1px solid #E2E8F0; }
        
        .screen { display: none; animation: fadeIn 0.5s ease-out; padding-bottom: 110px; }
        .active-screen { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(15px); border-top: 1px solid #E2E8F0; display: flex; justify-content: space-around; padding: 18px; z-index: 1000; }
        .nav-item { color: #94A3B8; font-size: 10px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 6px; cursor: pointer; }
        .nav-item.active { color: var(--gold); }
        
        .p-input { background: #FFFFFF; border: 2px solid #F1F5F9; width: 100%; padding: 16px; border-radius: 16px; outline: none; margin-bottom: 12px; color: #1E293B; font-weight: 600; transition: 0.2s; }
        .p-input:focus { border-color: var(--gold); }
    </style>
</head>
<body>

    <div id="auth-screen" class="min-h-screen flex flex-col p-8 justify-center active-screen bg-white">
        <div class="text-center mb-10">
            <h1 id="logo-tap" class="text-6xl font-black text-slate-900 tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <p class="text-[9px] text-slate-400 font-bold uppercase tracking-[8px] mt-2">Professional Mining Network</p>
        </div>
        <div class="space-y-3">
            <input type="text" id="u-id" placeholder="Partner ID" class="p-input">
            <input type="password" id="u-pw" placeholder="Security Passkey" class="p-input">
            <div id="adm-box" class="hidden"><input type="password" id="adm-key" placeholder="ADMIN MASTER KEY" class="p-input border-red-200"></div>
            <button onclick="handleAuth()" class="w-full btn-gold py-5 shadow-xl uppercase">Authorize Access</button>
            <p onclick="toggleMode()" id="mode-text" class="text-center text-[10px] text-slate-400 font-black uppercase mt-6 cursor-pointer tracking-widest">Create New Partner Account</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center bg-white/80 backdrop-blur-md sticky top-0 z-50 border-b border-slate-100">
            <h2 class="font-black text-2xl tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div id="user-tag" class="text-[9px] font-black bg-slate-900 text-white px-4 py-2 rounded-full uppercase">@PARTNER</div>
        </header>

        <div id="page-dash" class="screen active-screen px-4 pt-6">
            <div class="bg-slate-900 rounded-[32px] p-8 mb-6 text-white shadow-2xl relative overflow-hidden">
                <div class="absolute -right-10 -top-10 w-40 h-40 bg-white/5 rounded-full"></div>
                <p class="text-[10px] text-slate-400 font-black uppercase tracking-[4px] mb-2">Total Balance</p>
                <h1 id="b-main" class="text-5xl font-black tracking-tighter mb-10">$0.00</h1>
                <div class="grid grid-cols-2 gap-4 border-t border-white/10 pt-6">
                    <div><p class="text-[8px] text-slate-500 uppercase font-bold mb-1">Daily Profit</p><p id="b-daily" class="text-xl font-black text-green-400">+$0.00</p></div>
                    <div class="text-right"><p class="text-[8px] text-slate-500 uppercase font-bold mb-1">Total Earned</p><p id="b-total" class="text-xl font-black text-[#D4AF37]">$0.00</p></div>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-6">
                <button onclick="openModal('modal-dep')" class="clean-card p-6 flex flex-col items-center gap-3"><i class="fa-solid fa-wallet text-2xl text-[#D4AF37]"></i><span class="text-[10px] font-black uppercase">Deposit</span></button>
                <button onclick="openModal('modal-wd')" class="clean-card p-6 flex flex-col items-center gap-3"><i class="fa-solid fa-paper-plane text-2xl text-slate-400"></i><span class="text-[10px] font-black uppercase">Withdraw</span></button>
            </div>
        </div>

        <div id="page-nodes" class="screen px-4 pt-6">
            <h2 class="text-xl font-black mb-6 uppercase tracking-tight">Active <span class="text-[#D4AF37]">Infrastructure</span></h2>
            <div id="nodes-grid" class="grid grid-cols-1 gap-6 pb-10"></div>
        </div>

        <div id="page-logs" class="screen px-4 pt-6">
            <h2 class="text-xl font-black mb-6 uppercase">Transaction <span class="text-[#D4AF37]">History</span></h2>
            <div id="logs-list" class="space-y-3"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('page-dash', this)" class="nav-item active"><i class="fa-solid fa-grid-2 text-xl"></i><span>CORE</span></div>
            <div onclick="nav('page-nodes', this)" class="nav-item"><i class="fa-solid fa-microchip text-xl"></i><span>NODES</span></div>
            <div onclick="nav('page-logs', this)" class="nav-item"><i class="fa-solid fa-list-ul text-xl"></i><span>LOGS</span></div>
            <div onclick="logout()" class="nav-item"><i class="fa-solid fa-power-off text-xl text-red-400"></i><span>OUT</span></div>
        </nav>
    </div>

    <div id="modal-dep" class="fixed inset-0 bg-white z-[2000] p-6 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-8"><h2 class="text-2xl font-black">FUNDING METHODS</h2><i onclick="closeModal('modal-dep')" class="fa-solid fa-xmark text-2xl"></i></div>
        
        <div class="space-y-4 mb-8">
            <div class="clean-card p-5 border-l-4 border-blue-500">
                <p class="text-[10px] font-black uppercase text-blue-600 mb-1">Trust Wallet (USDT BEP20)</p>
                <p class="text-[11px] font-mono font-bold break-all">0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37</p>
                <button onclick="copy('0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37')" class="mt-3 text-[9px] font-black text-blue-600 uppercase">Copy Address</button>
            </div>
            <div class="clean-card p-5 border-l-4 border-orange-500">
                <p class="text-[10px] font-black uppercase text-orange-600 mb-1">MetaMask (USDT BEP20)</p>
                <p class="text-[11px] font-mono font-bold break-all">0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2</p>
                <button onclick="copy('0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2')" class="mt-3 text-[9px] font-black text-orange-600 uppercase">Copy Address</button>
            </div>
        </div>

        <input type="number" id="in-amt" placeholder="Enter Deposit Amount" class="p-input">
        <input type="text" id="in-ref" placeholder="Enter Transaction ID / Hash" class="p-input">
        <button onclick="submitFin('Deposit')" class="w-full btn-gold py-5">Verify Payment</button>
    </div>

    <div id="modal-wd" class="fixed inset-0 bg-white z-[2000] p-6 hidden">
        <div class="flex justify-between items-center mb-8"><h2 class="text-2xl font-black">WITHDRAW ASSETS</h2><i onclick="closeModal('modal-wd')" class="fa-solid fa-xmark text-2xl"></i></div>
        <input type="number" id="out-amt" placeholder="Amount (Minimum $10)" class="p-input">
        <input type="text" id="out-addr" placeholder="Your Wallet Address" class="p-input">
        <button onclick="submitFin('Withdrawal')" class="w-full btn-gold py-5 bg-slate-900 !text-white">Submit Request</button>
    </div>

    <div id="admin-panel" class="fixed inset-0 bg-slate-50 z-[5000] p-6 hidden overflow-y-auto text-slate-900">
        <div class="flex justify-between items-center mb-10"><h1 class="text-2xl font-black italic">ROOT ADMIN HUB</h1><button onclick="location.reload()" class="bg-black text-white px-6 py-2 rounded-xl text-[10px] font-black uppercase">Logout</button></div>
        <div class="mb-10">
            <h3 class="text-xs font-black uppercase text-red-500 mb-4 tracking-widest">Pending Requests (<span id="req-count">0</span>)</h3>
            <div id="admin-req-list" class="space-y-4"></div>
        </div>
        <div class="clean-card p-6">
            <h3 class="text-xs font-black uppercase text-slate-400 mb-4">Manual User Balance</h3>
            <input type="text" id="adm-target" placeholder="User ID" class="p-input !bg-slate-100">
            <input type="number" id="adm-val" placeholder="Amount" class="p-input !bg-slate-100">
            <button onclick="injectFunds()" class="w-full bg-slate-900 text-white py-4 rounded-xl font-black">ADD BALANCE</button>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const config = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(config); const db = getFirestore(app);
        let user = localStorage.getItem('nova_user'); let isLogin = true;

        if(user) startApp(user);

        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(document.getElementById('adm-key').value === "NOVA786") { openAdmin(); return; }
            if(!id || !pw) return alert("Missing Details");
            const uR = doc(db, "users", id); const s = await getDoc(uR);
            if(!isLogin) {
                if(s.exists()) return alert("User exists");
                await setDoc(uR, { balance: 0, password: pw, daily: 0, total_profit: 0, active_nodes: [] });
                localStorage.setItem('nova_user', id); startApp(id);
            } else if(s.exists() && s.data().password === pw) {
                localStorage.setItem('nova_user', id); startApp(id);
            } else alert("Error Accessing");
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
            loadLogs();
            setInterval(checkProfits, 1000);
        }

        const nodeSystem = [
            { id: 1, name: "Starter Unit", cost: 20, yield: 1.8, img: "https://images.unsplash.com/photo-1558494949-ef010cbdcc51?q=80&w=400" },
            { id: 2, name: "Bronze Miner", cost: 50, yield: 4.5, img: "https://images.unsplash.com/photo-1639322537228-f710d846310a?q=80&w=400" },
            { id: 3, name: "Silver Server", cost: 150, yield: 14.2, img: "https://images.unsplash.com/photo-1518770660439-4636190af475?q=80&w=400" },
            { id: 4, name: "Gold Rig", cost: 500, yield: 52.0, img: "https://images.unsplash.com/photo-1591844339942-3064e9042ce5?q=80&w=400" },
            { id: 5, name: "Platinum Blade", cost: 1200, yield: 135.0, img: "https://images.unsplash.com/photo-1620712943543-bcc4688e7485?q=80&w=400" },
            { id: 6, name: "Titan Node", cost: 3000, yield: 360.0, img: "https://images.unsplash.com/photo-1558494949-ef010cbdcc51?q=80&w=400" },
            { id: 7, name: "Diamond Core", cost: 7500, yield: 950.0, img: "https://images.unsplash.com/photo-1639322537228-f710d846310a?q=80&w=400" },
            { id: 8, name: "Nova Master", cost: 15000, yield: 2100.0, img: "https://images.unsplash.com/photo-1518770660439-4636190af475?q=80&w=400" },
            { id: 9, name: "Quantum Galaxy", cost: 25000, yield: 3800.0, img: "https://images.unsplash.com/photo-1591844339942-3064e9042ce5?q=80&w=400" }
        ];

        function genNodes(activeList) {
            const c = document.getElementById('nodes-grid'); c.innerHTML = "";
            nodeSystem.forEach(n => {
                const active = activeList.find(a => a.nodeId === n.id);
                c.innerHTML += `
                <div class="clean-card p-5 relative overflow-hidden">
                    <img src="${n.img}" class="node-img">
                    <div class="flex justify-between items-center">
                        <div>
                            <h4 class="font-black text-slate-900">${n.name}</h4>
                            <p class="text-green-600 font-bold text-[10px]">+$${n.yield.toFixed(2)} Daily Profit</p>
                        </div>
                        ${active ? `<div class="timer-box" id="timer-${n.id}">24:00:00</div>` : `<button onclick="buyNode(${n.id}, ${n.cost}, ${n.yield})" class="btn-gold px-6 py-2 text-[10px] uppercase font-black">$${n.cost}</button>`}
                    </div>
                </div>`;
            });
        }

        window.buyNode = async (nodeId, cost, y) => {
            const uR = doc(db, "users", user); const s = await getDoc(uR);
            if(s.data().balance < cost) return alert("Low Liquidity");
            const nN = { nodeId, yield: y, lastClaim: Date.now() };
            await updateDoc(uR, { balance: s.data().balance - cost, active_nodes: [...(s.data().active_nodes || []), nN], daily: (s.data().daily || 0) + y });
            alert("Hardware Activated!");
        };

        async function checkProfits() {
            const uR = doc(db, "users", user); const s = await getDoc(uR); if(!s.exists()) return;
            let data = s.data(); let nodes = data.active_nodes || []; let up = false;
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
                        n.lastClaim = Date.now(); data.balance += n.yield; data.total_profit += n.yield; up = true;
                    }
                }
            });
            if(up) await updateDoc(uR, { balance: data.balance, total_profit: data.total_profit, active_nodes: nodes });
        }

        window.submitFin = async (t) => {
            const a = parseFloat(document.getElementById(t=='Deposit'?'in-amt':'out-amt').value);
            const r = document.getElementById(t=='Deposit'?'in-ref':'out-addr').value;
            if(!a || !r) return alert("Fill data");
            await addDoc(collection(db, "logs"), { user, type: t, amount: a, status: 'Processing', ref: r, time: Date.now() });
            alert("Broadcasted to Admin Hub!"); closeModal(t=='Deposit'?'modal-dep':'modal-wd');
        };

        function loadLogs() {
            onSnapshot(query(collection(db, "logs"), where("user", "==", user)), s => {
                const c = document.getElementById('logs-list'); c.innerHTML = "";
                s.forEach(d => {
                    const l = d.data();
                    c.innerHTML += `<div class="clean-card p-4 flex justify-between"><div><p class="text-[8px] font-black text-slate-400 uppercase">${l.type}</p><p class="text-sm font-black">$${l.amount}</p></div><span class="text-[9px] font-black uppercase text-yellow-600">${l.status}</span></div>`;
                });
            });
        }

        window.openAdmin = () => { document.getElementById('admin-panel').classList.remove('hidden'); loadAdm(); };
        function loadAdm() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Processing")), s => {
                const c = document.getElementById('admin-req-list'); c.innerHTML = "";
                document.getElementById('req-count').innerText = s.size;
                s.forEach(d => {
                    const l = d.data(); const id = d.id;
                    c.innerHTML += `<div class="bg-white p-5 border rounded-2xl flex justify-between items-center"><div><p class="font-bold text-xs">@${l.user} (${l.type})</p><p class="font-black text-blue-600">$${l.amount}</p></div><div class="flex gap-2"><button onclick="admOp('${id}','${l.user}',${l.amount},'${l.type}')" class="bg-green-500 text-white px-4 py-2 rounded-lg font-black text-[9px]">OK</button><button onclick="rej('${id}')" class="bg-slate-200 px-4 py-2 rounded-lg font-black text-[9px]">X</button></div></div>`;
                });
            });
        }

        window.admOp = async (id, target, amt, type) => {
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
            if(s.exists()){ await updateDoc(uR, { balance: s.data().balance + v }); alert("Success"); }
        };

        window.nav = (id, el) => { document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen')); document.getElementById(id).classList.add('active-screen'); document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active')); el.classList.add('active'); };
        window.toggleMode = () => { isLogin = !isLogin; document.getElementById('mode-text').innerText = isLogin ? "Create New Partner Account" : "Return to Login Portal"; };
        window.openModal = (id) => document.getElementById(id).style.display = 'block';
        window.closeModal = (id) => document.getElementById(id).style.display = 'none';
        window.logout = () => { localStorage.removeItem('nova_user'); location.reload(); };
        window.copy = (t) => { navigator.clipboard.writeText(t); alert("Copied to Clipboard!"); };
        let tps = 0; document.getElementById('logo-tap').onclick = () => { tps++; if(tps >= 6) document.getElementById('adm-box').classList.remove('hidden'); };
    </script>
</body>
</html>
