<!DOCTYPE html>
<html lang="th">
<head>

<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">

<title>Khaidam Gamingshop</title>

<style>

body{
margin:0;
font-family:sans-serif;
background:#0f172a;
color:white;
}

header{
background:#020617;
padding:15px;
display:flex;
justify-content:space-between;
align-items:center;
}

.logo{
font-size:20px;
font-weight:bold;
color:#38bdf8;
}

.container{
padding:20px;
max-width:1100px;
margin:auto;
}

.products{
display:grid;
grid-template-columns:repeat(auto-fit,minmax(220px,1fr));
gap:20px;
}

.card{
background:#1e293b;
padding:20px;
border-radius:10px;
}

button{
padding:10px;
border:none;
border-radius:6px;
cursor:pointer;
margin-top:10px;
}

.buy{background:#22c55e;color:white}
.edit{background:#eab308;color:black}
.delete{background:#ef4444;color:white}

.panel{
background:#1e293b;
padding:20px;
border-radius:10px;
margin-top:20px;
}

input,select{
width:100%;
padding:10px;
margin-top:10px;
border-radius:6px;
border:none;
}

.order{
border-bottom:1px solid #334155;
padding:10px;
}

.paybox{
background:#1e293b;
padding:15px;
border-radius:8px;
margin-top:10px;
}

.status1{color:#38bdf8}
.status2{color:#2563eb}
.status3{color:#22c55e}
.status4{color:#ef4444}

</style>
</head>

<body>

<header>

<div class="logo">Khaidam Gamingshop</div>

<div id="userpanel"></div>

</header>

<div class="container">

<div id="login"></div>

<div id="shop"></div>

<div id="pay"></div>

<div id="orders"></div>

<div id="admin"></div>

</div>

<script>

let products = JSON.parse(localStorage.getItem("products")) || [
{name:"ไก่ตันเงิน 1M",price:120},
{name:"ไก่ตันเงิน 100K",price:12}
]

let users = JSON.parse(localStorage.getItem("users")) || []

let orders = JSON.parse(localStorage.getItem("orders")) || []

let currentUser = null

function save(){

localStorage.setItem("products",JSON.stringify(products))
localStorage.setItem("users",JSON.stringify(users))
localStorage.setItem("orders",JSON.stringify(orders))

}

function showLogin(){

document.getElementById("login").innerHTML=`

<div class="panel">

<h2>เข้าสู่ระบบ</h2>

<input id="loginUser" placeholder="username">

<input id="loginPass" type="password" placeholder="password">

<button onclick="login()">Login</button>

<p onclick="showRegister()">สมัครสมาชิก</p>

</div>

`

}

function showRegister(){

document.getElementById("login").innerHTML=`

<div class="panel">

<h2>สมัครสมาชิก</h2>

<input id="regUser">

<input id="regPass" type="password">

<button onclick="register()">สมัคร</button>

</div>

`

}

function register(){

let u=document.getElementById("regUser").value
let p=document.getElementById("regPass").value

users.push({u,p})

save()

alert("สมัครสำเร็จ")

showLogin()

}

function login(){

let u=document.getElementById("loginUser").value
let p=document.getElementById("loginPass").value

if(u=="danainat" && p=="danainat"){

currentUser={u:"danainat",admin:true}

}

else{

let user=users.find(x=>x.u==u && x.p==p)

if(!user)return alert("ข้อมูลผิด")

currentUser={u:user.u}

}

loadUser()

}

function loadUser(){

document.getElementById("login").innerHTML=""

document.getElementById("userpanel").innerHTML=`

${currentUser.u}

<button onclick="logout()">Logout</button>

`

loadShop()

showPay()

if(currentUser.admin){

showAdmin()

}else{

showOrders()

}

}

function logout(){

location.reload()

}

function loadShop(){

let html="<h2>สินค้า</h2><div class='products'>"

products.forEach((p,i)=>{

html+=`

<div class="card">

<h3>${p.name}</h3>

ราคา ${p.price} บาท

<button class="buy" onclick="buy(${i})">สั่งซื้อ</button>

${currentUser.admin?`

<button class="edit" onclick="editProduct(${i})">แก้ไข</button>

<button class="delete" onclick="deleteProduct(${i})">ลบ</button>

`:""}

</div>

`

})

html+="</div>"

document.getElementById("shop").innerHTML=html

}

function showPay(){

document.getElementById("pay").innerHTML=`

<h2>ช่องทางชำระเงิน</h2>

<div class="paybox">
TrueMoney : 096-817-7154
</div>

<div class="paybox">
KBank : 201-3-16151-0
</div>

`

}

function buy(i){

let file=document.createElement("input")

file.type="file"

file.accept="image/*"

file.onchange=e=>{

let reader=new FileReader()

reader.onload=function(){

orders.push({

user:currentUser.u,

product:products[i].name,

price:products[i].price,

slip:reader.result,

status:"รอดำเนินการ",

msg:""

})

save()

alert("ส่งออเดอร์สำเร็จ")

}

reader.readAsDataURL(e.target.files[0])

}

file.click()

}

function showOrders(){

let my=orders.filter(o=>o.user==currentUser.u)

let html="<div class='panel'><h2>ประวัติการสั่งซื้อ</h2>"

my.forEach(o=>{

html+=`

<div class="order">

สินค้า: ${o.product}

<br>

ราคา: ${o.price}

<br>

สถานะ: ${o.status}

<br>

ข้อความแอดมิน: ${o.msg}

<br>

<img src="${o.slip}" width="200">

</div>

`

})

html+="</div>"

document.getElementById("orders").innerHTML=html

}

function showAdmin(){

let html=`

<div class="panel">

<h2>Admin Dashboard</h2>

ออเดอร์ทั้งหมด ${orders.length}

<h3>เพิ่มสินค้า</h3>

<input id="pname">

<input id="pprice">

<button onclick="addProduct()">เพิ่มสินค้า</button>

</div>

<div class="panel">

<h3>รายการออเดอร์</h3>

`

orders.forEach((o,i)=>{

html+=`

<div class="order">

${o.user} | ${o.product} | ${o.price}

<br>

<img src="${o.slip}" width="200">

<select onchange="setStatus(${i},this.value)">

<option>รอดำเนินการ</option>
<option>กำลังดำเนินการ</option>
<option>ดำเนินการเสร็จสิ้น</option>
<option>ดำเนินการไม่สำเร็จ</option>

</select>

<input placeholder="ข้อความถึงลูกค้า" onchange="setMsg(${i},this.value)">

</div>

`

})

html+="</div>"

document.getElementById("admin").innerHTML=html

}

function addProduct(){

let n=document.getElementById("pname").value
let p=document.getElementById("pprice").value

products.push({name:n,price:p})

save()

loadShop()

}

function editProduct(i){

let name=prompt("ชื่อสินค้าใหม่",products[i].name)
let price=prompt("ราคาใหม่",products[i].price)

products[i].name=name
products[i].price=price

save()

loadShop()

}

function deleteProduct(i){

if(confirm("ลบสินค้า?")){

products.splice(i,1)

save()

loadShop()

}

}

function setStatus(i,v){

orders[i].status=v

save()

}

function setMsg(i,v){

orders[i].msg=v

save()

}

showLogin()

</script>

</body>
</html>
