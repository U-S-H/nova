<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | ULTRA-PRO MAX</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --dark: #0F172A; }
        body { background: #F0F4F8; font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; -webkit-tap-highlight-color: transparent; }
        .glass-card { background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(10px); border: 1px solid rgba(255, 255, 255, 0.2); border-radius: 30px; box-shadow: 0 10px 30px rgba(0,0,0,0.03); }
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 20px; font-weight: 800; cursor: pointer; border: none; transition: 0.4s; }
        .screen { display: none; min-height: 100vh; width: 100%; padding-bottom: 120px; }
        .active-screen { display: block; animation: fadeIn 0.5s ease; }
        @keyframes fadeIn { from { opacity: 0; transform: scale(0.98); } to { opacity: 1; transform: scale(1); } }
        .nav-bar { position: fixed; bottom: 15px; left: 50%; transform: translateX(-50%); width: 92%; background: #0F172A; border-radius: 25px; display: flex; justify-content: space-around; padding: 15px 0; z-index: 1000; box-shadow: 0 20px 40px rgba(0,0,0,0.2); }
        .nav-item { color: #64748b; font-size: 9px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 5px; }
        .nav-item.active { color: var(--gold); }
        .method-card { border: 2px solid #f1f5f9; border-radius: 20px; padding: 15px; transition: 0.3s; cursor: pointer; }
        .method-card:active { border-color: var(--gold); background: #fffcf0; }
        .ticker-wrap { background: #D4AF37; color: #0F172A; padding: 6px 0; font-size: 10px; font-weight: 900; overflow: hidden; text-transform: uppercase; }
        .ticker-move { display: inline-block; white-space: nowrap; animation: ticker 25s linear infinite; }
        @keyframes ticker { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }
    </style>
</head>
<body>

    <div id="auth-screen" class="active-screen min-h-screen flex flex-col justify-center p-8 bg-white">
        <div class="text-center mb-12">
            <h1 class="text-7xl font-black italic tracking-tighter">NO<span class="text-gold">VA</span></h1>
            <p class="text-[10px] font-bold text-slate-400 tracking-[0.5em] mt-2 uppercase">Global Trading Infrastructure</p>
        </div>
        <div class="space-y-4 max-w-xs mx-auto w-full">
            <input type="text" id="u-id" placeholder="Username" class="w-full p-5 rounded-2xl bg-slate-50 border-none outline-none font-bold">
            <input type="password" id="u-pw" placeholder="Security Password" class="w-full p-5 rounded-2xl bg-slate-50 border-none outline-none font-bold">
            <input type="text" id="u-ref" placeholder="Referral Code (Optional)" class="w-full p-5 rounded-2xl bg-slate-50 border-none outline-none font-bold text-xs">
            <button onclick="handleAuth()" class="w-full btn-gold py-5 shadow-2xl text-xs tracking-widest uppercase">Start Session</button>
            <p onclick="toggleAuth()" id="auth-btn-text" class="text-[10px] text-center text-slate-400 font-black uppercase cursor-pointer underline">New Node? Register Here</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <div class="ticker-wrap"><div class="ticker-move">Secure Link Established • Multi-Currency Gateway Active • Withdrawals Processing in 5mins • V21 Nodes Yielding 15% Bonus • </div></div>
        
        <header class="p-6 flex justify-between items-center bg-white/80 backdrop-blur sticky top-0 z-[100]">
            <h2 id="logo-main" class="font-black text-2xl italic tracking-tighter">NO<span class="text-gold">VA</span></h2>
            <div class="text-right">
                <p id="user-tag" class="text-[8px] font-black uppercase text-slate-400">@DEV_NODE</p>
                <p id="b-header" class="text-xl font-black text-slate-950 tracking-tighter">$0.00</p>
            </div>
        </header>

        <div id="scr-dash" class="screen active-screen px-5 pt-4">
            <div class="bg-slate-950 rounded-[40px] p-8 text-white mb-6 shadow-2xl border-b-[10px] border-gold">
                <p class="text-[9px] text-slate-500 uppercase font-black mb-1">Available Liquidity</p>
                <h1 id="b-main" class="text-6xl font-black mb-10 tracking-tighter">$0.00</h1>
                <div class="flex justify-between items-end">
                    <div><p class="text-[8px] text-slate-500 uppercase font-black">24h Profit</p><p id="b-daily" class="text-2xl font-black text-green-400">+$0.00</p></div>
                    <div class="text-right"><p class="text-[8px] text-slate-500 uppercase font-black">Total Earnings</p><p id="b-total-p" class="text-2xl font-black text-gold">$0.00</p></div>
                </div>
            </div>

            <div class="grid grid-cols-3 gap-4 mb-8">
                <div onclick="openModal('mod-dep')" class="glass-card p-5 text-center"><i class="fa-solid fa-plus-circle text-gold text-2xl mb-2"></i><p class="text-[9px] font-black uppercase">Deposit</p></div>
                <div onclick="openModal('mod-wd')" class="glass-card p-5 text-center"><i class="fa-solid fa-arrow-up-right-from-square text-slate-400 text-2xl mb-2"></i><p class="text-[9px] font-black uppercase">Withdraw</p></div>
                <div onclick="claimDaily()" class="glass-card p-5 text-center"><i class="fa-solid fa-gift text-red-500 text-2xl mb-2"></i><p class="text-[9px] font-black uppercase">Claim</p></div>
            </div>

            <div class="glass-card p-6 border-dashed border-2 border-slate-300">
                <div class="flex items-center justify-between">
                    <div><h4 class="text-[9px] font-black text-slate-400 uppercase">Your Network ID</h4><p id="ref-link" class="text-[11px] font-bold mt-1 text-slate-600">nova.app/ref=786</p></div>
                    <button onclick="copyRef()" class="btn-gold p-3 rounded-xl"><i class="fa-solid fa-copy"></i></button>
                </div>
            </div>
        </div>

        <div id="scr-nodes" class="screen px-5 pt-6">
            <h3 class="font-black text-2xl mb-8 italic uppercase">High-Yield Nodes</h3>
            <div id="nodes-grid" class="space-y-8"></div>
        </div>

        <div id="scr-team" class="screen px-5 pt-6">
            <h3 class="font-black text-2xl mb-8 italic uppercase">Global Network</h3>
            <div class="grid grid-cols-2 gap-4 mb-8">
                <div class="glass-card p-8 bg-slate-900 text-white text-center"><h5 id="team-count" class="text-4xl font-black text-gold">0</h5><p class="text-[8px] font-black uppercase mt-2">Active Partners</p></div>
                <div class="glass-card p-8 text-center"><h5 id="team-comm" class="text-4xl font-black">$0</h5><p class="text-[8px] font-black text-slate-400 uppercase mt-2">Total Bonus</p></div>
            </div>
            <div id="team-list" class="space-y-4"></div>
        </div>

        <div id="scr-admin" class="screen px-5 pt-6">
            <h3 class="text-2xl font-black mb-8 text-red-600 italic uppercase">Central Command</h3>
            <div class="glass-card p-6 bg-slate-950 text-white mb-6">
                <p class="text-[9px] font-black text-slate-500 mb-4 uppercase">Gateway Configuration</p>
                <div class="space-y-3">
                    <input type="text" id="adm-ep" placeholder="EasyPaisa" class="w-full p-4 rounded-xl text-black font-bold">
                    <input type="text" id="adm-jc" placeholder="JazzCash" class="w-full p-4 rounded-xl text-black font-bold">
                    <input type="text" id="adm-bin" placeholder="Binance Pay ID" class="w-full p-4 rounded-xl text-black font-bold">
                    <input type="number" id="adm-tax" placeholder="Withdraw Tax %" class="w-full p-4 rounded-xl text-black font-bold">
                    <button onclick="saveAdminSettings()" class="w-full btn-gold py-4 text-xs uppercase">Save System Config</button>
                </div>
            </div>
            <div id="adm-pending" class="space-y-4"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('scr-dash', this)" class="nav-item active"><i class="fa-solid fa-compass text-2xl"></i><span>TERMINAL</span></div>
            <div onclick="nav('scr-nodes', this)" class="nav-item"><i class="fa-solid fa-microchip text-2xl"></i><span>NODES</span></div>
            <div onclick="nav('scr-team', this)" class="nav-item"><i class="fa-solid fa-users-rays text-2xl"></i><span>NETWORK</span></div>
            <div id="nav-adm" onclick="nav('scr-admin', this)" class="nav-item text-red-500" style="display:none;"><i class="fa-solid fa-lock text-2xl"></i><span>MASTER</span></div>
        </nav>
    </div>

    <div id="mod-dep" class="fixed inset-0 bg-white z-[2000] p-8 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-8"><h2 class="font-black text-2xl uppercase italic">Deposit Hub</h2><i onclick="closeModal('mod-dep')" class="fa-solid fa-times text-3xl text-slate-200"></i></div>
        <p class="text-[10px] font-black text-slate-400 uppercase mb-4">Select Gateway</p>
        <div class="grid grid-cols-2 gap-4 mb-8">
            <div onclick="showWay('ep')" class="method-card text-center"><i class="fa-solid fa-mobile-screen text-green-500 mb-2"></i><p class="text-[9px] font-black uppercase">EasyPaisa</p></div>
            <div onclick="showWay('jc')" class="method-card text-center"><i class="fa-solid fa-wallet text-red-500 mb-2"></i><p class="text-[9px] font-black uppercase">JazzCash</p></div>
            <div onclick="showWay('bin')" class="method-card text-center"><i class="fa-brands fa-bitcoin text-yellow-500 mb-2"></i><p class="text-[9px] font-black uppercase">Binance</p></div>
            <div onclick="showWay('tw')" class="method-card text-center"><i class="fa-solid fa-shield-halved text-blue-500 mb-2"></i><p class="text-[9px] font-black uppercase">Trust Wallet</p></div>
        </div>
        <div id="way-info" class="bg-slate-950 text-gold p-8 rounded-[35px] mb-8 hidden text-center shadow-2xl border-b-4 border-gold">
            <p id="way-label" class="text-[8px] uppercase mb-2">Address / Number</p>
            <p id="way-num" class="text-xl font-black tracking-widest">0000000000</p>
        </div>
        <input type="number" id="dep-amt" placeholder="Amount ($)" class="w-full p-5 bg-slate-50 rounded-2xl mb-4 font-bold outline-none">
        <input type="text" id="dep-tx" placeholder="TRX / Hash ID" class="w-full p-5 bg-slate-50 rounded-2xl mb-8 font-bold outline-none uppercase">
        <button onclick="handleFinance('Deposit')" class="w-full btn-gold py-6 uppercase tracking-widest text-sm shadow-xl">Broadcast Deposit</button>
    </div>

    <div id="mod-wd" class="fixed inset-0 bg-white z-[2000] p-8 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-8"><h2 class="font-black text-2xl uppercase italic">Withdrawal Portal</h2><i onclick="closeModal('mod-wd')" class="fa-solid fa-times text-3xl text-slate-200"></i></div>
        <div class="space-y-4 mb-8">
            <div class="glass-card p-4 flex gap-4 items-center">
                <select id="wd-method" class="w-full bg-transparent font-bold outline-none">
                    <option value="EasyPaisa">EasyPaisa</option>
                    <option value="JazzCash">JazzCash</option>
                    <option value="Binance (USDT)">Binance (USDT)</option>
                    <option value="Trust Wallet">Trust Wallet</option>
                    <option value="MetaMask">MetaMask</option>
                    <option value="Bank Transfer">Bank Transfer</option>
                    <option value="Nayapay">Nayapay</option>
                    <option value="SadaPay">SadaPay</option>
                </select>
            </div>
            <input type="number" id="wd-amt" placeholder="Amount ($)" class="w-full p-5 bg-slate-50 rounded-2xl font-bold">
            <input type="text" id="wd-acc" placeholder="Account / Wallet Address" class="w-full p-5 bg-slate-50 rounded-2xl font-bold">
        </div>
        <button onclick="handleFinance('Withdraw')" class="w-full btn-gold py-6 uppercase tracking-widest text-sm">Request Withdrawal</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const fbCfg = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(fbCfg); const db = getFirestore(app);

        let user = localStorage.getItem('nova_user');
        let mode = 'login'; let tCount = 0;
        let sys = { ep: "03279177196", jc: "03279177196", bin: "PAY-ID-78601", tax: 5 };

        // 21 MODERN NODES
        const nodes = [];
        const imgs = [
            "https://images.unsplash.com/photo-1639762681485-074b7f938ba0?w=500",
            "https://images.unsplash.com/photo-1620712943543-bcc4628c6bb5?w=500",
            "https://images.unsplash.com/photo-1642104704074-907c0698bcd9?w=500",
            "https://images.unsplash.com/photo-1558494949-ef010cbdcc51?w=500"
        ];
        for(let i=0; i<21; i++) {
            nodes.push({ id: i+1, n: `QUANTUM NODE X${i+1}`, c: 10 + (i * 25), y: 1.2 + (i * 2.1), days: 30 + (i*2), img: imgs[i%4] });
        }

        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            const ref = document.getElementById('u-ref').value.toLowerCase().trim() || 'none';
            if(!id || !pw) return alert("Fill all details");
            
            const uRef = doc(db, "users", id); const snap = await getDoc(uRef);
            if(mode === 'login') {
                if(snap.exists() && snap.data().pw === pw) {
                    if(snap.data().status === 'Banned') return alert("Account Banned!");
                    localStorage.setItem('nova_user', id); location.reload();
                } else alert("Invalid Credentials");
            } else {
                if(snap.exists()) return alert("Username Taken");
                await setDoc(uRef, { bal: 0, tp: 0, pw: pw, daily: 0, team_bal: 0, refBy: ref, active: [], lastClaim: 0, status: 'Active' });
                localStorage.setItem('nova_user', id); location.reload();
            }
        };

        function initApp(id) {
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app').classList.remove('hidden');

            onSnapshot(doc(db, "users", id), d => {
                const u = d.data(); if(!u || u.status === 'Banned') { localStorage.clear(); location.reload(); }
                document.getElementById('user-tag').innerText = `@${id}`;
                document.getElementById('b-main').innerText = `$${u.bal.toFixed(2)}`;
                document.getElementById('b-header').innerText = `$${u.bal.toFixed(2)}`;
                document.getElementById('b-daily').innerText = `+$${u.daily.toFixed(2)}`;
                document.getElementById('b-total-p').innerText = `$${(u.tp || 0).toFixed(2)}`;
                renderNodes(u.active || []);
            });

            onSnapshot(query(collection(db, "users"), where("refBy", "==", id)), s => {
                document.getElementById('team-count').innerText = s.size;
                const list = document.getElementById('team-list'); list.innerHTML = "";
                s.forEach(doc => { list.innerHTML += `<div class="glass-card p-5 flex justify-between items-center text-xs"><b>@${doc.id}</b><span class="text-green-500 font-black">ONLINE</span></div>`; });
            });

            onSnapshot(doc(db, "config", "master"), d => { if(d.exists()) sys = d.data(); });
        }

        function renderNodes(active) {
            const grid = document.getElementById('nodes-grid'); grid.innerHTML = "";
            nodes.forEach(n => {
                const isRun = active.includes(n.id);
                grid.innerHTML += `<div class="glass-card overflow-hidden">
                    <img src="${n.img}" class="w-full h-44 object-cover">
                    <div class="p-6">
                        <div class="flex justify-between items-center mb-6">
                            <div><h4 class="font-black text-sm uppercase italic">${n.n}</h4><p class="text-[10px] text-green-500 font-bold">$${n.y.toFixed(2)} Hourly Yield</p></div>
                            ${isRun ? `<span class="bg-green-100 text-green-600 px-4 py-2 rounded-full text-[10px] font-black animate-pulse">MINING</span>` : `<button onclick="buyNode(${n.id}, ${n.c}, ${n.y})" class="btn-gold px-6 py-3 text-[10px] uppercase">$${n.c}</button>`}
                        </div>
                        <div class="flex justify-between text-[8px] font-black text-slate-400 uppercase"><span>Cycles: ${n.days} Days</span><span>Status: Operational</span></div>
                    </div>
                </div>`;
            });
        }

        window.claimDaily = async () => {
            const uRef = doc(db, "users", user); const s = await getDoc(uRef);
            if(Date.now() - (s.data().lastClaim || 0) < 86400000) return alert("Come back in 24 hours!");
            await updateDoc(uRef, { bal: s.data().bal + 0.25, tp: (s.data().tp || 0) + 0.25, lastClaim: Date.now() });
            alert("Node Bonus $0.25 Added!");
        };

        window.buyNode = async (id, c, y) => {
            const uRef = doc(db, "users", user); const s = await getDoc(uRef);
            if(s.data().bal < c) return alert("Liquidity Error: Low Balance");
            await updateDoc(uRef, { bal: s.data().bal - c, active: [...s.data().active, id], daily: s.data().daily + y });
            
            if(s.data().refBy && s.data().refBy !== 'none') {
                const rRef = doc(db, "users", s.data().refBy); const rs = await getDoc(rRef);
                if(rs.exists()) await updateDoc(rRef, { bal: rs.data().bal + (c*0.1), team_bal: (rs.data().team_bal || 0) + (c*0.1), tp: (rs.data().tp || 0) + (c*0.1) });
            }
            alert("Node successfully integrated into the network!");
        };

        window.handleFinance = async (type) => {
            const amt = parseFloat(document.getElementById(type === 'Deposit' ? 'dep-amt' : 'wd-amt').value);
            const ref = document.getElementById(type === 'Deposit' ? 'dep-tx' : 'wd-acc').value;
            if(!amt || !ref) return alert("Credentials Missing");
            await addDoc(collection(db, "logs"), { user, type, amount: amt, ref, status: 'Pending', method: type === 'Withdraw' ? document.getElementById('wd-method').value : 'Direct' });
            alert("Network Ticket Submitted!"); 
            closeModal('mod-dep'); closeModal('mod-wd');
        };

        document.getElementById('logo-main').onclick = () => {
            tCount++; if(tCount >= 5) {
                if(prompt("System Override Key:") === "nov786") { document.getElementById('nav-adm').style.display="flex"; loadAdm(); }
                tCount = 0;
            }
        };

        onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const list = document.getElementById('adm-pending'); list.innerHTML = "";
                s.forEach(d => {
                    list.innerHTML += `<div class="glass-card p-5 text-[10px] flex justify-between items-center bg-white">
                        <div><b>${d.data().user}</b> [${d.data().type}]<br>$${d.data().amount} via ${d.data().method || 'Direct'}<br>REF: ${d.data().ref}</div>
                        <div class="flex gap-2">
                            <button onclick="approveLog('${d.id}', '${d.data().user}', ${d.data().amount}, '${d.data().type}')" class="bg-green-500 text-white p-3 rounded-xl">OK</button>
                            <button onclick="banUser('${d.data().user}')" class="bg-red-600 text-white p-3 rounded-xl">BAN</button>
                        </div>
                    </div>`;
                });
            });
        }

        window.approveLog = async (lid, uid, amt, type) => {
            const uRef = doc(db, "users", uid); const us = await getDoc(uRef);
            if(type === 'Deposit') await updateDoc(uRef, { bal: us.data().bal + amt });
            await updateDoc(doc(db, "logs", lid), { status: 'Approved' }); alert("Broadcast Successful");
        };

        window.banUser = async (uid) => { if(confirm(`Suspend ${uid}?`)) await updateDoc(doc(db, "users", uid), { status: 'Banned' }); };
        
        window.saveAdminSettings = async () => {
            const up = { ep: document.getElementById('adm-ep').value, jc: document.getElementById('adm-jc').value, bin: document.getElementById('adm-bin').value, tax: parseFloat(document.getElementById('adm-tax').value) };
            await setDoc(doc(db, "config", "master"), up, { merge: true }); alert("Mainframe Updated");
        };

        window.showWay = (t) => { 
            document.getElementById('way-info').classList.remove('hidden'); 
            document.getElementById('way-num').innerText = sys[t] || "ADDRESS_PENDING";
            document.getElementById('way-label').innerText = t === 'bin' ? 'Binance Pay ID' : (t === 'tw' ? 'Wallet Address' : 'Account Number');
        };
        window.nav = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active')); el.classList.add('active');
        };
        window.openModal = (id) => document.getElementById(id).classList.remove('hidden');
        window.closeModal = (id) => document.getElementById(id).classList.add('hidden');
        window.toggleAuth = () => { mode = mode === 'login' ? 'reg' : 'login'; document.getElementById('auth-btn-text').innerText = mode === 'login' ? "New Node? Register Here" : "Existing Node? Login"; };
        window.copyRef = () => { navigator.clipboard.writeText(document.getElementById('ref-link').innerText); alert("ID Copied to Clipboard"); };

        if(user) initApp(user);
    </script>
</body>
</html>
