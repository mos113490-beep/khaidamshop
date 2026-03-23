<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <title>Khaidam Gamingshop</title>
  <style>
    body {
      margin: 0;
      font-family: sans-serif;
      background: #0f172a;
      color: white;
    }
    header {
      background: #020617;
      padding: 15px;
      display: flex;
      justify-content: space-between;
    }
    .logo {
      font-size: 20px;
      font-weight: bold;
      color: #38bdf8;
    }
    .container {
      padding: 20px;
    }
    .products {
      display: grid;
      grid-template-columns: repeat(auto-fit,minmax(220px,1fr));
      gap: 20px;
    }
    .card {
      background: #1e293b;
      padding: 20px;
      border-radius: 10px;
    }
    button {
      padding: 10px;
      border: none;
      border-radius: 6px;
      margin-top: 10px;
      cursor: pointer;
    }
    .buy {
      background: #22c55e;
      color: white;
    }
  </style>
</head>

<body>

<header>
  <div class="logo">Khaidam Shop</div>
</header>

<div class="container">
  <h2>สินค้า</h2>

  <div class="products">
    <div class="card">
      <h3>ไอดีเกม 1</h3>
      <p>ราคา: 100 บาท</p>
      <button class="buy">ซื้อ</button>
    </div>

    <div class="card">
      <h3>ไอดีเกม 2</h3>
      <p>ราคา: 200 บาท</p>
      <button class="buy">ซื้อ</button>
    </div>
  </div>
</div>

</body>
</html>
