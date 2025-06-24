# Rebooker-DBMS


-- 📍 Address Table
CREATE TABLE Address 
    address_id INT PRIMARY KEY AUTO_INCREMENT,
    line1 VARCHAR(255),
    line2 VARCHAR(255),
    city VARCHAR(100),
    state VARCHAR(100),
    country VARCHAR(100),
    pincode VARCHAR(20)


-- 👤 Users Table
CREATE TABLE Users (
    user_id INT PRIMARY KEY,
    username VARCHAR(255) NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    last_password_attempt VARCHAR(255),
    login_attempts INT DEFAULT 0,
    login_successful BOOLEAN DEFAULT FALSE,
    is_blocked BOOLEAN DEFAULT FALSE,
    address_id INT,
    paypal_coins INT DEFAULT 0,
    product_preferences TEXT,
    cart TEXT,
    is_vendor BOOLEAN DEFAULT FALSE,
    FOREIGN KEY (address_id) REFERENCES Address(address_id)
);

-- 🛍️ Vendor Table
CREATE TABLE Vendor (
    vendor_id INT PRIMARY KEY,
    vendor_name VARCHAR(255),
    address_id INT,
    FOREIGN KEY (address_id) REFERENCES Address(address_id)
);

-- 🛠 Refurbishment Center Table
CREATE TABLE RefurbishmentCenter (
    center_id INT PRIMARY KEY,
    address_id INT,
    contact_info VARCHAR(100),
    FOREIGN KEY (address_id) REFERENCES Address(address_id)
);

-- 📚 Catalogue Table
CREATE TABLE Catalogue (
    book_id INT PRIMARY KEY,
    book_name VARCHAR(255) NOT NULL,
    quantity_available INT,
    vendor_id INT,
    book_status VARCHAR(100),
    FOREIGN KEY (vendor_id) REFERENCES Vendor(vendor_id)
);

-- 🧾 Ordered History Table
CREATE TABLE OrderedHistory (
    transaction_id INT PRIMARY KEY,
    cart_id INT,
    user_id INT,
    order_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    book_id INT,
    book_quantity INT,
    total_cost DECIMAL(10, 2),
    order_status VARCHAR(100),
    FOREIGN KEY (user_id) REFERENCES Users(user_id),
    FOREIGN KEY (book_id) REFERENCES Catalogue(book_id)
);

-- 🎁 Donated History Table
CREATE TABLE DonatedHistory (
    transaction_id INT PRIMARY KEY,
    user_id INT,
    donation_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    book_id INT,
    book_quantity INT,
    estimated_value DECIMAL(10, 2),
    donation_status VARCHAR(100),
    FOREIGN KEY (user_id) REFERENCES Users(user_id),
    FOREIGN KEY (book_id) REFERENCES Catalogue(book_id)
);
