# 🏠 Airbnb Clone — Use Case Diagram

## 📘 Overview

This diagram represents the **Use Case Diagram** for the **Airbnb Clone Backend System**.  
It visualizes how different actors interact with the system’s core functionalities such as user registration, property management, booking, and payment processing.

---

## 🎯 Objective

To illustrate the **interactions between system actors and use cases**, showing how users (Guest, Host, Admin) engage with the backend system to accomplish their respective goals.

---

## 👥 Actors

| Actor                          | Description                                                                                     |
| ------------------------------ | ----------------------------------------------------------------------------------------------- |
| **Guest**                      | A user who browses listings, books properties, makes payments, and leaves reviews.              |
| **Host**                       | A property owner who manages listings, bookings, and receives payments from guests.             |
| **Admin**                      | A system administrator who oversees user activities, listings, payments, and resolves disputes. |
| **Payment Gateway (optional)** | An external service responsible for processing transactions securely.                           |

---

## ⚙️ Key Use Cases

### For Guest

- Register / Login
- View Listings
- Search Properties
- Book Property
- Make Payment
- Write Review
- Receive Notifications

### For Host

- Register / Login
- Add Property Listing
- Manage Property (Edit/Delete)
- Manage Bookings
- Receive Payment
- Respond to Reviews

### For Admin

- Manage Users
- Manage Listings
- Manage Payments
- Monitor System Activities
- Resolve Disputes

---

## 🔗 Relationships

| Relationship Type           | Example                                                               |
| --------------------------- | --------------------------------------------------------------------- |
| **Include (`<<include>>`)** | `Book Property` → `Make Payment`                                      |
| **Extend (`<<extend>>`)**   | `Book Property` → `Cancel Booking` (optional)                         |
| **Association**             | Actors connected to use cases via lines represent direct interactions |

---

## 🧩 System Boundary

The **Airbnb Clone System** is the boundary enclosing all backend operations.  
Actors interact with this boundary through various use cases.

---

## 🗂️ Repository Structure

```

alx-airbnb-project-documentation/
│
├── use-case-diagram/
│   ├── use-case-diagram.xml
│   ├── use-case-diagram.png
│   └── README.md

```

---

## 🧭 How to View and Export the Diagram

### Step 1: Open Draw.io

Go to [https://app.diagrams.net/](https://app.diagrams.net/)

### Step 2: Import the XML File

1. Click **File → Import From → Device**
2. Select `use-case-diagram.xml` from your local folder
3. The use case diagram will appear automatically

### Step 3: Export as PNG

1. Go to **File → Export as → PNG**
2. Save as `use-case-diagram.png`
3. Place it in your directory:

```

alx-airbnb-project-documentation/use-case-diagram/

```

### Step 4: Commit and Push

Run the following commands:

```bash
git add use-case-diagram/
git commit -m "Add Airbnb Clone Use Case Diagram"
git push origin main
```

---

## 🖼️ Diagram Preview (Overview)

The diagram includes:

- **Actors:** Guest, Host, Admin
- **Use Cases:** Core backend functionalities (register, manage property, booking, payment)
- **Relationships:** Includes and associations
- **System Boundary:** Airbnb Clone System

---
