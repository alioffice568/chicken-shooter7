<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SMM HUB Panel</title>
<style>
*{box-sizing:border-box}
body{margin:0;font-family:Arial;background:#f1faf7;color:#24434a}
header{background:white;padding:18px;color:#08786d;font-size:25px;font-weight:bold;border-bottom:2px solid #bde7df}
nav{display:flex;flex-wrap:wrap;gap:8px;padding:12px;background:white}
button{cursor:pointer;border:0;border-radius:12px;padding:12px;font-size:15px}
nav button{background:#e9f7f4;color:#176b64}
main{max-width:700px;margin:auto;padding:14px}
.card{background:white;border:1px solid #dce9e8;border-radius:22px;padding:18px;margin-bottom:16px;box-shadow:0 5px 15px #00000008}
.stats{display:grid;gap:10px}
.stat{background:#112f50;color:white;padding:18px;border-radius:18px}
.stat b{display:block;color:#f8bf45;margin-top:8px;font-size:22px}
h2{color:#08786d;font-size:20px}
label{display:block;margin:16px 0 8px;font-weight:bold;color:#315d5b}
input,select,textarea{width:100%;padding:14px;border:1px solid #d8e5e9;border-radius:12px;background:#fbfdff;font-size:16px}
.primary{width:100%;margin-top:16px;background:linear-gradient(90deg,#ff592c,#ff803c);color:white;font-weight:bold;padding:16px}
.green{background:#08796b;color:white}
.page{display:none}.page.active{display:block}
.note{background:#fff4d9;padding:12px;border-radius:12px;font-size:13px;line-height:1.5}
.item{padding:12px 0;border-bottom:1px solid #e4eeee;overflow-wrap:anywhere}
.small{color:#829196;font-size:13px}
</style>
</head>
<body>

<header>◢ SMM HUB 👑</header>

<nav>
<button onclick="show('home')">Dashboard</button>
<button onclick="show('order')">New Order</button>
<button onclick="show('funds')">Add Funds</button>
<button onclick="show('orders')">Orders</button>
<button onclick="show('transactions')">Transactions</button>
<button onclick="show('admin')">Admin Demo</button>
</nav>

<main>
<section id="home" class="page active">
<div class="card">
<center>IN THE NAME OF<br><h2>Allah Almighty</h2></center>
<div class="stats">
<div class="stat">ACCOUNT USERNAME<b id="username">demo_user</b></div>
<div class="stat">TOTAL SPENT<b id="spent">PKR 0.00</b></div>
<div class="stat">AVAILABLE BALANCE<b id="balance">PKR 0.00</b></div>
</div>
<button class="primary" onclick="show('funds')">＋ ADD FUND</button>
</div>
<div class="card">
<h2>Welcome to SMM HUB</h2>
<p>Social media services panel demo.</p>
<div class="note">Demo only: no real payment or social-media delivery is connected.</div>
<button class="primary" onclick="show('order')">Create New Order</button>
</div>
</section>

<section id="order" class="page">
<div class="card">
<h2>New Order</h2>
<label>SEARCH SERVICE</label>
<input id="search" placeholder="Search services..." oninput="filterServices()">

<label>CHECK CATEGORY</label>
<select id="category" onchange="chooseCategory()">
<option>TikTok Likes</option>
<option>TikTok Followers</option>
<option>TikTok Views</option>
<option>Instagram Likes</option>
<option>Instagram Followers</option>
<option>YouTube Views</option>
<option>Facebook Page Likes</option>
<option>WhatsApp Channel</option>
</select>

<label>SELECT SERVICE</label>
<select id="service" onchange="calculate()">
<option value="4173|277.035|1000">4173 - TikTok Likes - PKR 277.035 / 1000</option>
<option value="4174|350.8898|1000">4174 - TikTok Likes + Views - PKR 350.8898 / 1000</option>
<option value="2101|180|100">2101 - Instagram Likes - PKR 180 / 1000</option>
<option value="2102|400|100">2102 - Instagram Followers - PKR 400 / 1000</option>
<option value="3101|420|100">3101 - YouTube Views - PKR 420 / 1000</option>
<option value="5101|250|50">5101 - Facebook Likes - PKR 250 / 1000</option>
</select>

<label>DESCRIPTION</label>
<div class="note">Check your link and quantity before submitting. This demo does not send real orders.</div>

<label>LINK</label>
<input id="link" placeholder="https://...">

<label>QUANTITY</label>
<input id="quantity" type="number" placeholder="Minimum 1000" oninput="calculate()">

<label>CHARGE (PKR)</label>
<input id="charge" readonly value="0.00">

<button class="primary" onclick="submitOrder()">Submit Order</button>
</div>
</section>

<section id="funds" class="page">
<div class="card">
<h2>Add Funds</h2>
<div class="note">Demo deposit request only. No money is transferred and balance is not credited automatically.</div>
<label>PAYMENT METHOD</label>
<select id="method">
<option>Easypaisa</option>
<option>JazzCash</option>
<option>Bank Transfer</option>
<option>PayFast</option>
<option>UPaisa</option>
</select>
<label>AMOUNT (PKR)</label>
<input id="amount" type="number" placeholder="Enter amount">
<label>TRANSACTION REFERENCE</label>
<input id="reference" placeholder="Optional reference">
<button class="primary" onclick="deposit()">Create Deposit Request</button>
</div>
</section>

<section id="orders" class="page">
<div class="card"><h2>My Orders</h2><div id="orderList">No orders yet.</div></div>
</section>

<section id="transactions" class="page">
<div class="card"><h2>Transactions</h2><div id="transactionList">No transactions yet.</div></div>
</section>

<section id="admin" class="page">
<div class="card">
<h2>Admin Demo</h2>
<div class="note">This is not a secure admin panel. Do not use it for real customer funds.</div>
<label>USERNAME</label>
<input id="newUsername" placeholder="Enter username">
<button class="primary" onclick="saveUsername()">Save Username</button>
<label>DEMO BALANCE ADJUSTMENT (PKR)</label>
<input id="adjustment" type="number" placeholder="Amount to add">
<button class="primary" onclick="adjustBalance()">Adjust Demo Balance</button>
</div>
</section>
</main>

<script>
let balance=0,spent=0,orders=[],transactions=[];

function show(id){
 document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
 document.getElementById(id).classList.add('active');
 if(id==='orders')renderOrders();
 if(id==='transactions')renderTransactions();
 window.scrollTo(0,0);
}
function money(n){return 'PKR '+Number(n).toFixed(2)}
function updateStats(){
 document.getElementById('balance').textContent=money(balance);
 document.getElementById('spent').textContent=money(spent);
}
function calculate(){
 let parts=document.getElementById('service').value.split('|');
 let qty=Number(document.getElementById('quantity').value)||0;
 document.getElementById('charge').value=(qty*Number(parts[1])/1000).toFixed(2);
}
function chooseCategory(){
 let c=document.getElementById('category').value;
 let s=document.getElementById('service');
 if(c.includes('Instagram Likes'))s.selectedIndex=2;
 else if(c.includes('Instagram Followers'))s.selectedIndex=3;
 else if(c.includes('YouTube'))s.selectedIndex=4;
 else if(c.includes('Facebook'))s.selectedIndex=5;
 else s.selectedIndex=0;
 calculate();
}
function filterServices(){
 let q=document.getElementById('search').value.toLowerCase();
 let s=document.getElementById('service');
 for(let o of s.options)o.hidden=!o.text.toLowerCase().includes(q);
 let found=[...s.options].find(o=>!o.hidden);
 if(found)s.value=found.value;
 calculate();
}
function submitOrder(){
 let link=document.getElementById('link').value.trim();
 let qty=Number(document.getElementById('quantity').value);
 let charge=Number(document.getElementById('charge').value);
 let service=document.getElementById('service');
 if(!/^https?:\/\/\S+/i.test(link)){alert('Enter a valid link starting with https://');return}
 if(!Number.isInteger(qty)||qty<1000){alert('Quantity must be at least 1000');return}
 if(charge>balance){alert('Insufficient demo balance. Deposit requests do not credit funds automatically.');return}
 balance-=charge;spent+=charge;
 orders.unshift({service:service.options[service.selectedIndex].text,link,qty,charge,status:'Demo - Not Sent'});
 updateStats();
 alert('Demo order saved. No real order was sent.');
 document.getElementById('link').value='';
 document.getElementById('quantity').value='';
 calculate();show('orders');
}
function deposit(){
 let amount=Number(document.getElementById('amount').value);
 if(amount<100){alert('Enter an amount of at least PKR 100');return}
 transactions.unshift({
 type:'Deposit request',amount,
 method:document.getElementById('method').value,
 ref:document.getElementById('reference').value||'Not provided',
 status:'Pending - Not Paid'
 });
 alert('Demo deposit request saved. Balance has not been credited.');
 document.getElementById('amount').value='';
 document.getElementById('reference').value='';
 show('transactions');
}
function renderOrders(){
 let el=document.getElementById('orderList');
 el.innerHTML=orders.length?orders.map((o,i)=>
 `<div class="item"><b>Order #${i+1}</b><p>${o.service}</p>
 <p>Quantity: ${o.qty}</p><p>Charge: ${money(o.charge)}</p>
 <p class="small">${o.link}</p><b>${o.status}</b></div>`
 ).join(''):'No orders yet.';
}
function renderTransactions(){
 let el=document.getElementById('transactionList');
 el.innerHTML=transactions.length?transactions.map(t=>
 `<div class="item"><b>${t.type}</b><p>${money(t.amount)}</p>
 <p>${t.method}</p><p>Reference: ${t.ref}</p><b>${t.status}</b></div>`
 ).join(''):'No transactions yet.';
}
function saveUsername(){
 let n=document.getElementById('newUsername').value.trim();
 if(!n){alert('Enter a username');return}
 document.getElementById('username').textContent=n;
 alert('Demo username changed.');
}
function adjustBalance(){
 let n=Number(document.getElementById('adjustment').value);
 if(!n){alert('Enter an adjustment amount');return}
 balance=Math.max(0,balance+n);
 transactions.unshift({type:'Admin demo adjustment',amount:Math.abs(n),method:'Local demo',ref:'Demo only',status:'Not real money'});
 updateStats();alert('Demo balance adjusted locally.');
}
</script>
</body>
</html>
