<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Business Management System</title>

<style>
    body {
        font-family: Arial, sans-serif;
        min-height: 100vh;
        margin: 0;
        padding: 20px;
        background: linear-gradient(135deg, #667eea, #764ba2);
    }

    h1 {
        text-align: center;
        color: #fff;
    }

    .container {
        max-width: 900px;
        margin: auto;
        background: #ffffff;
        padding: 20px;
        border-radius: 10px;
        box-shadow: 0 10px 25px rgba(0,0,0,0.2);
    }

    label {
        font-weight: bold;
    }

    input, button {
        width: 100%;
        padding: 10px;
        margin: 6px 0;
    }

    button {
        border: none;
        cursor: pointer;
        border-radius: 5px;
    }

    .add-btn {
        background: #007bff;
        color: white;
    }

    .add-btn:hover {
        background: #0056b3;
    }

    table {
        width: 100%;
        border-collapse: collapse;
        margin-top: 15px;
    }

    th, td {
        border: 1px solid #ddd;
        padding: 8px;
        text-align: center;
    }

    th {
        background: #007bff;
        color: white;
    }

    .edit {
        background: #ffc107;
        color: black;
        padding: 5px;
    }

    .delete {
        background: #dc3545;
        color: white;
        padding: 5px;
    }

    .error {
        color: red;
        margin-top: 10px;
    }

    .success {
        color: green;
        margin-top: 10px;
    }

    .total {
        font-weight: bold;
        margin-top: 15px;
    }
</style>
</head>

<body>

<h1>Business Management System</h1>

<div class="container">

    <label>Product Name</label>
    <input type="text" id="name">

    <label>Price ($)</label>
    <input type="number" id="price">

    <label>Quantity</label>
    <input type="number" id="quantity">

    <button class="add-btn" onclick="saveProduct()">Save Product</button>

    <p id="message"></p>

    <table>
        <thead>
            <tr>
                <th>#</th>
                <th>Product</th>
                <th>Price</th>
                <th>Qty</th>
                <th>Total</th>
                <th>Actions</th>
            </tr>
        </thead>
        <tbody id="tableBody"></tbody>
    </table>

    <p class="total">Grand Total: $<span id="grandTotal">0</span></p>

</div>

<script>
    let products = [];
    let editIndex = null;

    function saveProduct() {
        let name = document.getElementById("name").value.trim();
        let price = document.getElementById("price").value;
        let quantity = document.getElementById("quantity").value;
        let message = document.getElementById("message");

        if (name === "" || price <= 0 || quantity <= 0) {
            message.textContent = "❌ Please enter valid product data.";
            message.className = "error";
            return;
        }

        let product = {
            name: name,
            price: Number(price),
            quantity: Number(quantity)
        };

        if (editIndex === null) {
            products.push(product);
            message.textContent = "✅ Product added successfully!";
        } else {
            products[editIndex] = product;
            message.textContent = "✏️ Product updated successfully!";
            editIndex = null;
        }

        message.className = "success";
        clearInputs();
        displayProducts();
    }

    function displayProducts() {
        let tableBody = document.getElementById("tableBody");
        tableBody.innerHTML = "";
        let grandTotal = 0;

        products.forEach((product, index) => {
            let total = product.price * product.quantity;
            grandTotal += total;

            tableBody.innerHTML += `
                <tr>
                    <td>${index + 1}</td>
                    <td>${product.name}</td>
                    <td>$${product.price}</td>
                    <td>${product.quantity}</td>
                    <td>$${total}</td>
                    <td>
                        <button class="edit" onclick="editProduct(${index})">Edit</button>
                        <button class="delete" onclick="deleteProduct(${index})">Delete</button>
                    </td>
                </tr>
            `;
        });

        document.getElementById("grandTotal").textContent = grandTotal;
    }

    function editProduct(index) {
        let product = products[index];
        document.getElementById("name").value = product.name;
        document.getElementById("price").value = product.price;
        document.getElementById("quantity").value = product.quantity;
        editIndex = index;

        let message = document.getElementById("message");
        message.textContent = "✏️ Editing product...";
        message.className = "success";
    }

    function deleteProduct(index) {
        if (confirm("Are you sure you want to delete this product?")) {
            products.splice(index, 1);
            displayProducts();
        }
    }

    function clearInputs() {
        document.getElementById("name").value = "";
        document.getElementById("price").value = "";
        document.getElementById("quantity").value = "";
    }
</script>

</body>
</html>
