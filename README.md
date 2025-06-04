<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>متجر مثل الفيديو</title>
<style>
  /* الخط العام */
  body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: #f5f7fa;
    margin: 0;
    padding: 20px;
    color: #333;
  }

  h1 {
    text-align: center;
    font-weight: 700;
    margin-bottom: 30px;
    color: #222;
  }

  /* شبكة المنتجات */
  .products {
    display: grid;
    grid-template-columns: repeat(auto-fill,minmax(220px,1fr));
    gap: 25px;
  }

  /* كل منتج */
  .product {
    background: #fff;
    border-radius: 12px;
    box-shadow: 0 3px 10px rgb(0 0 0 / 0.1);
    padding: 15px;
    display: flex;
    flex-direction: column;
    align-items: center;
    transition: transform 0.2s ease;
  }
  .product:hover {
    transform: translateY(-5px);
    box-shadow: 0 6px 20px rgb(0 0 0 / 0.15);
  }

  /* صورة المنتج */
  .product img {
    width: 180px;
    height: 180px;
    object-fit: cover;
    border-radius: 12px;
    margin-bottom: 12px;
    box-shadow: 0 2px 6px rgba(0,0,0,0.1);
  }

  /* اسم المنتج */
  .product h3 {
    font-size: 1.15rem;
    margin: 8px 0 6px;
    color: #222;
  }

  /* سعر المنتج */
  .product p {
    color: #1e88e5;
    font-weight: 700;
    font-size: 1.05rem;
    margin: 0 0 15px;
  }

  /* زر إضافة للسلة */
  button {
    background-color: #1e88e5;
    border: none;
    color: white;
    padding: 10px 20px;
    font-weight: 700;
    border-radius: 7px;
    cursor: pointer;
    box-shadow: 0 3px 6px rgb(30 136 229 / 0.5);
    transition: background-color 0.3s ease, box-shadow 0.3s ease;
  }
  button:hover {
    background-color: #1565c0;
    box-shadow: 0 6px 12px rgb(21 101 192 / 0.7);
  }

  /* سلة التسوق */
  .cart {
    margin-top: 45px;
    background: #fff;
    border-radius: 12px;
    box-shadow: 0 3px 10px rgb(0 0 0 / 0.1);
    padding: 20px;
  }
  .cart h2 {
    font-weight: 700;
    margin-top: 0;
    margin-bottom: 18px;
    color: #222;
  }

  /* عناصر السلة */
  .cart-items {
    max-height: 250px;
    overflow-y: auto;
    margin-bottom: 18px;
  }
  .cart-item {
    display: flex;
    justify-content: space-between;
    padding: 8px 0;
    border-bottom: 1px solid #eee;
  }
  .cart-item span:first-child {
    font-weight: 700;
    color: #444;
  }
  .cart-item span:last-child {
    color: #1e88e5;
    font-weight: 700;
  }

  /* السعر الإجمالي */
  .total {
    font-size: 1.25rem;
    font-weight: 800;
    color: #1e88e5;
    text-align: right;
  }

  /* تصميم متجاوب للجوال */
  @media (max-width: 600px) {
    .products {
      grid-template-columns: repeat(auto-fill,minmax(150px,1fr));
      gap: 15px;
    }
    .product img {
      width: 140px;
      height: 140px;
    }
    button {
      padding: 8px 12px;
      font-size: 0.9rem;
    }
  }
</style>
</head>
<body>

  <h1>متجر مثل الفيديو</h1>

  <div class="products" id="products"></div>

  <div class="cart" id="cart">
    <h2>سلة التسوق</h2>
    <div class="cart-items" id="cart-items">لا يوجد منتجات في السلة.</div>
    <div class="total" id="total-price"></div>
  </div>

<script>
  const products = [
    {id: 1, name: "كريم ترطيب", price: 55, image: "https://via.placeholder.com/180?text=كريم+ترطيب"},
    {id: 2, name: "صابون طبيعي", price: 30, image: "https://via.placeholder.com/180?text=صابون+طبيعي"},
    {id: 3, name: "زيت عطري", price: 45, image: "https://via.placeholder.com/180?text=زيت+عطري"},
    {id: 4, name: "مصل فيتامين", price: 90, image: "https://via.placeholder.com/180?text=مصل+فيتامين"}
  ];

  let cart = [];

  function displayProducts() {
    const container = document.getElementById("products");
    container.innerHTML = "";
    products.forEach(p => {
      const prod = document.createElement("div");
      prod.className = "product";
      prod.innerHTML = `
        <img src="${p.image}" alt="${p.name}" />
        <h3>${p.name}</h3>
        <p>${p.price} ريال</p>
        <button onclick="addToCart(${p.id})">أضف للسلة</button>
      `;
      container.appendChild(prod);
    });
  }

  function addToCart(id) {
    const product = products.find(p => p.id === id);
    const found = cart.find(item => item.id === id);
    if (found) {
      found.qty++;
    } else {
      cart.push({...product, qty: 1});
    }
    displayCart();
  }

  function displayCart() {
    const cartItems = document.getElementById("cart-items");
    const totalPrice = document.getElementById("total-price");

    if (cart.length === 0) {
      cartItems.innerHTML = "لا يوجد منتجات في السلة.";
      totalPrice.innerHTML = "";
      return;
    }

    cartItems.innerHTML = "";
    let total = 0;
    cart.forEach(item => {
      total += item.price * item.qty;
      const div = document.createElement("div");
      div.className = "cart-item";
      div.innerHTML = `
        <span>${item.name} (x${item.qty})</span>
        <span>${item.price * item.qty} ريال</span>
      `;
      cartItems.appendChild(div);
    });

    totalPrice.innerHTML = `الإجمالي: ${total} ريال`;
  }

  window.onload = () => {
    displayProducts();
    displayCart();
  }
</script>

</body>
</html>
