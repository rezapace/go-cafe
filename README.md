# Cafe App Documentation

## Sebelum Memulai
1. Inisialisasi Go Modules:
    ```sh
    go mod init
    go mod tidy
    ```

## User Features
- [x] User dapat registrasi dengan role sebagai customer atau kasir
- [x] User dapat login dengan email dan password
- [x] User dapat melihat daftar makanan
- [x] User dapat menambahkan pesanan baru (order)

## Kasir Features
- [x] Kasir dapat menambahkan produk makanan baru
- [x] Kasir dapat melihat detail pesanan berdasarkan ID pesanan
- [x] Kasir dapat mengupdate order dalam sistem
- [x] Kasir dapat menghapus order dalam sistem
- [x] Kasir dapat mengubah data makanan berdasarkan ID makanan
- [x] Kasir dapat menghapus data makanan berdasarkan ID makanan
- [x] Kasir dapat melihat status pesanan berdasarkan ID pesanan

## Docker Build
```sh
docker build -t cafe-app:v1.0 .
```
- `-t`: Tag atau memberikan nama pada image yang akan dibuat
- `nama_image`: Nama yang diberikan pada image
- `tag`: Versi dari image yang dibuat
- `.`: Menunjukkan bahwa Dockerfile yang digunakan berada pada direktori yang sama dengan terminal saat ini

## FIXME
- Ketika menggunakan JWT, outputnya selalu error. Disarankan mengganti versi JWT dan Echo.
- Ketika di-build, Docker tidak bisa include GORM, terjadi error.
- Pada routes belum ditambahkan user role-nya berdasarkan siapa saja yang dapat mengakses karena Echo dan JWT-nya.

## Installation
1. Inisialisasi Go Modules:
    ```sh
    go mod init
    go mod tidy
    ```
2. Set Up Database:
    ```sql
    CREATE DATABASE cafe;
    USE cafe;

    CREATE TABLE users (
        id INT AUTO_INCREMENT PRIMARY KEY,
        name VARCHAR(255) NOT NULL,
        email VARCHAR(255) UNIQUE NOT NULL,
        password VARCHAR(255) NOT NULL,
        userrole VARCHAR(255) NOT NULL
    );

    CREATE TABLE food (
        id INT AUTO_INCREMENT PRIMARY KEY,
        name VARCHAR(255) NOT NULL,
        description VARCHAR(255),
        price DECIMAL(10, 2) NOT NULL
    );

    CREATE TABLE orders (
        id INT AUTO_INCREMENT PRIMARY KEY,
        user_id INT NOT NULL,
        total_price DECIMAL(10, 2) NOT NULL,
        status ENUM('proses', 'selesai') NOT NULL,
        order_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
        FOREIGN KEY (user_id) REFERENCES users(id)
    );

    CREATE TABLE order_details (
        id INT AUTO_INCREMENT PRIMARY KEY,
        order_id INT NOT NULL,
        food_id INT NOT NULL,
        quantity INT NOT NULL,
        price DECIMAL(10, 2) NOT NULL,
        FOREIGN KEY (order_id) REFERENCES orders(id),
        FOREIGN KEY (food_id) REFERENCES food(id)
    );
    ```
3. Run the Server:
    ```sh
    go run main.go
    ```

## Postman Usage

### Register User
- **Endpoint**: `POST /register`
- **Body**:
    ```json
    {
        "name": "reza",
        "email": "reza@gmail.com",
        "password": "123",
        "role": "kasir"
    }
    ```

### Login User
- **Endpoint**: `POST /login`
- **Body**:
    ```json
    {
        "email": "reza@gmail.com",
        "password": "123"
    }
    ```

### Add Food Item
- **Endpoint**: `POST /foods`
- **Body**:
    ```json
    {
        "name": "Nasi Goreng",
        "description": "Nasi goreng istimewa dengan telur, ayam, dan bumbu rempah pilihan.",
        "price": 15000
    }
    ```

### List Food Items
- **Endpoint**: `GET /foods`

### Add Order
- **Endpoint**: `POST /orders`
- **Body**:
    ```json
    {
        "user_id": 1,
        "food_id": 1,
        "quantity": 2,
        "price": 1000.00,
        "status": "proses"
    }
    ```

### View Order by ID
- **Endpoint**: `GET /orders/1`

### Update Order
- **Endpoint**: `PUT /orders/1`
- **Body**:
    ```json
    {
        "user_id": 1,
        "food_id": 1,
        "quantity": 3,
        "price": 10.50,
        "status": "selesai"
    }
    ```

### Delete Order
- **Endpoint**: `DELETE /orders/1`

### Update Food Item
- **Endpoint**: `PUT /food/1`
- **Body**:
    ```json
    {
        "name": "Nasi Goreng Spesial",
        "description": "Nasi goreng dengan telur, ayam, sayuran, dan rempah-rempah khas Indonesia.",
        "price": 225000
    }
    ```

### Delete Food Item
- **Endpoint**: `DELETE /food/1`

### View Order Status by ID
- **Endpoint**: `GET /orders/status/1`
Endpoint: GET /orders/status/1
