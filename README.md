Berikut adalah kode HTML lengkap (disertai CSS dan JavaScript interaktif) untuk Aplikasi Kasir / Manajemen Supermarket. Kode ini sudah dikemas dalam satu file (index.html) sehingga sangat mudah untuk langsung Anda unggah (upload) dan jalankan di GitHub / GitHub Pages.

Kode: index.html
HTML
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aplikasi Supermarket - Kasir & Stok</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: #f4f6f9;
            color: #333;
            display: flex;
            flex-direction: column;
            min-height: 100vh;
        }

        header {
            background-color: #2c3e50;
            color: #fff;
            padding: 1rem 2rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }

        header h1 {
            font-size: 1.5rem;
        }

        .container {
            display: flex;
            flex: 1;
            padding: 20px;
            gap: 20px;
            flex-wrap: wrap;
        }

        .card {
            background: #fff;
            border-radius: 8px;
            padding: 20px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.05);
        }

        .left-panel {
            flex: 2;
            min-width: 300px;
        }

        .right-panel {
            flex: 1;
            min-width: 300px;
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        h2 {
            font-size: 1.2rem;
            margin-bottom: 15px;
            color: #2c3e50;
            border-bottom: 2px solid #ecf0f1;
            padding-bottom: 5px;
        }

        /* Product Grid */
        .product-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
            gap: 15px;
            margin-bottom: 20px;
        }

        .product-card {
            border: 1px solid #e0e0e0;
            border-radius: 6px;
            padding: 15px;
            text-align: center;
            cursor: pointer;
            transition: transform 0.2s, box-shadow 0.2s;
            background: #fafafa;
        }

        .product-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
            border-color: #3498db;
        }

        .product-title {
            font-weight: bold;
            margin-bottom: 5px;
        }

        .product-price {
            color: #27ae60;
            font-weight: bold;
        }

        .product-stock {
            font-size: 0.85rem;
            color: #7f8c8d;
        }

        /* Table Cart */
        table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 15px;
        }

        th, td {
            text-align: left;
            padding: 10px;
            border-bottom: 1px solid #eee;
        }

        th {
            background-color: #f8f9fa;
            color: #555;
        }

        .btn-btn {
            padding: 5px 10px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-weight: bold;
        }

        .btn-danger {
            background-color: #e74c3c;
            color: white;
        }

        .btn-success {
            background-color: #2ecc71;
            color: white;
            width: 100%;
            padding: 12px;
            font-size: 1rem;
        }

        .btn-primary {
            background-color: #3498db;
            color: white;
            width: 100%;
            padding: 10px;
        }

        .total-section {
            font-size: 1.2rem;
            font-weight: bold;
            display: flex;
            justify-content: space-between;
            margin-top: 15px;
            padding-top: 10px;
            border-top: 2px solid #eee;
        }

        /* Input Form */
        .form-group {
            margin-bottom: 10px;
        }

        .form-group label {
            display: block;
            font-size: 0.9rem;
            margin-bottom: 5px;
        }

        .form-group input {
            width: 100%;
            padding: 8px;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
    </style>
</head>
<body>

    <header>
        <h1>Supermarket POS System</h1>
        <span id="datetime"></span>
    </header>

    <div class="container">
        <!-- Panel Kiri: Daftar Produk & Keranjang -->
        <div class="left-panel card">
            <h2>Pilih Produk</h2>
            <div class="product-grid" id="productGrid">
                <!-- Produk akan dimuat di sini secara otomatis -->
            </div>

            <h2>Keranjang Belanja</h2>
            <table>
                <thead>
                    <tr>
                        <th>Produk</th>
                        <th>Harga</th>
                        <th>Jumlah</th>
                        <th>Subtotal</th>
                        <th>Aksi</th>
                    </tr>
                </thead>
                <tbody id="cartTable">
                    <!-- Barang belanjaan masuk di sini -->
                </tbody>
            </table>

            <div class="total-section">
                <span>Total Bayar:</span>
                <span id="totalPrice">Rp 0</span>
            </div>
            <br>
            <button class="btn-btn btn-success" onclick="checkout()">Bayar Sekarang</button>
        </div>

        <!-- Panel Kanan: Tambah Produk Baru -->
        <div class="right-panel">
            <div class="card">
                <h2>Tambah Produk Baru</h2>
                <form id="addProductForm">
                    <div class="form-group">
                        <label for="prodName">Nama Produk</label>
                        <input type="text" id="prodName" required placeholder="Contoh: Susu Kotak">
                    </div>
                    <div class="form-group">
                        <label for="prodPrice">Harga (Rp)</label>
                        <input type="number" id="prodPrice" required placeholder="15000">
                    </div>
                    <div class="form-group">
                        <label for="prodStock">Stok</label>
                        <input type="number" id="prodStock" required placeholder="50">
                    </div>
                    <button type="submit" class="btn-btn btn-primary">Tambah ke Daftar</button>
                </form>
            </div>
        </div>
    </div>

    <script>
        // Data awal produk
        let products = [
            { id: 1, name: "Beras 5kg", price: 68000, stock: 20 },
            { id: 2, name: "Minyak Goreng 2L", price: 34000, stock: 15 },
            { id: 3, name: "Gula Pasir 1kg", price: 16000, stock: 30 },
            { id: 4, name: "Mie Instan", price: 3000, stock: 100 },
            { id: 5, name: "Susu UHT 1L", price: 18000, stock: 25 }
        ];

        let cart = [];

        // Menampilkan produk
        function renderProducts() {
            const grid = document.getElementById('productGrid');
            grid.innerHTML = '';

            products.forEach(p => {
                grid.innerHTML += `
                    <div class="product-card" onclick="addToCart(${p.id})">
                        <div class="product-title">${p.name}</div>
                        <div class="product-price">Rp ${p.price.toLocaleString()}</div>
                        <div class="product-stock">Stok: ${p.stock}</div>
                    </div>
                `;
            });
        }

        // Tambah ke keranjang
        function addToCart(productId) {
            const product = products.find(p => p.id === productId);

            if (product.stock <= 0) {
                alert('Stok barang habis!');
                return;
            }

            const cartItem = cart.find(item => item.id === productId);

            if (cartItem) {
                if (cartItem.qty < product.stock) {
                    cartItem.qty++;
                } else {
                    alert('Jumlah melebihi stok yang tersedia!');
                }
            } else {
                cart.push({ ...product, qty: 1 });
            }

            renderCart();
        }

        // Menampilkan Keranjang
        function renderCart() {
            const tbody = document.getElementById('cartTable');
            tbody.innerHTML = '';
            let total = 0;

            cart.forEach((item, index) => {
                const subtotal = item.price * item.qty;
                total += subtotal;

                tbody.innerHTML += `
                    <tr>
                        <td>${item.name}</td>
                        <td>Rp ${item.price.toLocaleString()}</td>
                        <td>${item.qty}</td>
                        <td>Rp ${subtotal.toLocaleString()}</td>
                        <td>
                            <button class="btn-btn btn-danger" onclick="removeFromCart(${index})">Hapus</button>
                        </td>
                    </tr>
                `;
            });

            document.getElementById('totalPrice').innerText = `Rp ${total.toLocaleString()}`;
        }

        // Hapus barang dari keranjang
        function removeFromCart(index) {
            cart.splice(index, 1);
            renderCart();
        }

        // Proses Checkout / Pembayaran
        function checkout() {
            if (cart.length === 0) {
                alert('Keranjang belanja masih kosong!');
                return;
            }

            // Kurangi stok produk
            cart.forEach(item => {
                const prod = products.find(p => p.id === item.id);
                if (prod) {
                    prod.stock -= item.qty;
                }
            });

            alert('Transaksi Berhasil! Terima kasih.');
            cart = [];
            renderCart();
            renderProducts();
        }

        // Tambah Produk Baru lewat Form
        document.getElementById('addProductForm').addEventListener('submit', function(e) {
            e.preventDefault();

            const name = document.getElementById('prodName').value;
            const price = parseInt(document.getElementById('prodPrice').value);
            const stock = parseInt(document.getElementById('prodStock').value);

            const newProduct = {
                id: products.length + 1,
                name: name,
                price: price,
                stock: stock
            };

            products.push(newProduct);
            renderProducts();

            // Reset form
            this.reset();
        });

        // Inisialisasi Tampilan
        renderProducts();

        // Tanggal & Waktu Realtime
        setInterval(() => {
            const now = new Date();
            document.getElementById('datetime').innerText = now.toLocaleString('id-ID');
        }, 1000);
    </script>
</body>
</html>
Cara Mengunggah (Upload) Kode ini ke GitHub & Menjalankannya Grat
