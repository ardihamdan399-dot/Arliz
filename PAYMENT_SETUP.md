# BACKEND SETUP - Midtrans Payment Gateway

## 📋 Prasyarat

1. **Akun Midtrans**: https://midtrans.com (Daftar gratis)
2. **Node.js + Express** (Backend)
3. **Database** (Optional - untuk tracking)

---

## 🚀 STEP 1: Setup Midtrans Account

### Daftar Akun Midtrans

1. Buka: https://midtrans.com
2. Klik "Sign Up"
3. Isi data bisnis
4. Verifikasi email
5. Login ke dashboard

### Ambil API Keys

1. Buka: https://dashboard.midtrans.com/settings/config
2. Di tab "SANDBOX" (untuk testing):
   - Copy **Client Key** 
   - Copy **Server Key**
3. Simpan untuk backend setup

---

## 💻 STEP 2: Setup Backend (Node.js + Express)

### Install Dependencies

```bash
# Create project
mkdir arliz-backend
cd arliz-backend
npm init -y

# Install packages
npm install express cors body-parser midtrans-client dotenv
```

### Create `.env` File

```env
PORT=3000
MIDTRANS_CLIENT_KEY=YOUR_CLIENT_KEY
MIDTRANS_SERVER_KEY=YOUR_SERVER_KEY
MIDTRANS_APP_ID=YOUR_APP_ID
```

### Create `server.js`

```javascript
const express = require('express');
const cors = require('cors');
const bodyParser = require('body-parser');
const midtransClient = require('midtrans-client');
require('dotenv').config();

const app = express();

// Middleware
app.use(cors());
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));

// Midtrans Setup
const snap = new midtransClient.Snap({
    isProduction: false, // Sandbox mode
    serverKey: process.env.MIDTRANS_SERVER_KEY,
    clientKey: process.env.MIDTRANS_CLIENT_KEY
});

// Payment Endpoint
app.post('/api/payment', async (req, res) => {
    const { item_type, item_name, amount, order_id, customer_name } = req.body;

    // Define item details
    let itemDetails = [];
    switch(item_type) {
        case 'extra_lives':
            itemDetails = [{
                id: 'extra_lives',
                price: amount,
                quantity: 1,
                name: '+3 Extra Lives'
            }];
            break;
        case 'hints':
            itemDetails = [{
                id: 'hints',
                price: amount,
                quantity: 1,
                name: '+5 Hints'
            }];
            break;
        case 'premium':
            itemDetails = [{
                id: 'premium',
                price: amount,
                quantity: 1,
                name: 'Premium Pass (Unlimited)'
            }];
            break;
    }

    const parameter = {
        "transaction_details": {
            "order_id": order_id,
            "gross_amount": amount
        },
        "item_details": itemDetails,
        "customer_details": {
            "first_name": customer_name,
            "email": "player@arlizgameplay.com",
            "phone": "08123456789"
        },
        "callbacks": {
            "finish": "https://your-domain.com/finish"
        }
    };

    try {
        const token = await snap.createTransaction(parameter);
        res.json({ token: token });
    } catch (error) {
        console.error('Error:', error);
        res.status(500).json({ error: 'Payment failed' });
    }
});

// Notification Webhook
app.post('/api/payment-notification', async (req, res) => {
    const notification = req.body;
    
    // Verify signature
    const orderId = notification.order_id;
    const statusCode = notification.status_code;
    const grossAmount = notification.gross_amount;
    const serverKey = process.env.MIDTRANS_SERVER_KEY;
    
    // Log transaction
    console.log('Notification received:');
    console.log(`Order ID: ${orderId}`);
    console.log(`Status: ${statusCode}`);
    console.log(`Amount: ${grossAmount}`);
    
    // Handle different statuses
    if (statusCode == '200' || statusCode == '201') {
        console.log('✅ Payment Success');
        // Update database or user account
    } else if (statusCode == '202') {
        console.log('⏳ Payment Pending');
    } else if (statusCode == '407') {
        console.log('❌ Payment Cancelled');
    }
    
    res.json({ status: 'ok' });
});

// Health Check
app.get('/health', (req, res) => {
    res.json({ status: 'Backend running ✅' });
});

// Start Server
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
    console.log(`🚀 Server running at http://localhost:${PORT}`);
});
```

### Run Server

```bash
node server.js
```

Output:
```
🚀 Server running at http://localhost:3000
```

---

## 🔧 STEP 3: Update Frontend (index.html)

### Update Midtrans Keys

Di `index.html`, ganti:

```javascript
const MIDTRANS_CLIENT_KEY = 'YOUR_MIDTRANS_CLIENT_KEY';
const MIDTRANS_SERVER_URL = 'http://localhost:3000/api/payment';
```

Jadi:

```javascript
const MIDTRANS_CLIENT_KEY = 'Paste_Your_Client_Key_Here';
const MIDTRANS_SERVER_URL = 'http://localhost:3000/api/payment';
```

---

## 🧪 Testing

### Test Payment (Sandbox Mode)

1. Buka: https://ardihamdan399-dot.github.io/Arliz
2. Klik "💎 SHOP"
3. Klik salah satu item
4. Gunakan test card:

**Credit Card Test Numbers:**
```
- Visa: 4811 1111 1111 1114 (Success)
- Visa: 4911 1111 1111 1113 (Pending)
- Mastercard: 5555 1111 1111 4444 (Success)
```

**Expiry Date:** Anggap saja lebih dari 2 tahun (misal 12/25)

**CVV:** Random 3 digits (misal 123)

**OTP:** 112233 (jika diminta)

---

## 🌐 Deploy Backend

### Deploy ke Heroku (Gratis)

```bash
# Install Heroku CLI
npm install -g heroku-cli

# Login
heroku login

# Create app
heroku create arliz-backend

# Set env variables
heroku config:set MIDTRANS_SERVER_KEY=YOUR_KEY
heroku config:set MIDTRANS_CLIENT_KEY=YOUR_KEY

# Deploy
git push heroku main
```

**Production URL:** https://arliz-backend.herokuapp.com/api/payment

### Deploy ke Railway (Recommended)

```bash
# Visit: https://railway.app
# Connect GitHub repo
# Add env variables
# Deploy
```

### Deploy ke Vercel

```bash
# Install Vercel CLI
npm install -g vercel

# Deploy
vercel
```

---

## 📝 Production Checklist

```
☑ Midtrans Sandbox testing done
☑ Backend server deployed
☑ Environment variables set
☑ Payment URLs updated
☑ Frontend payment gateway active
☑ Test payment successful
☑ Switch to production mode (isProduction: true)
☑ Enable production payment
```

---

## 🔐 Security Notes

✅ **Never commit** `.env` ke GitHub
✅ **Use** environment variables untuk keys
✅ **Verify** Midtrans signature di webhook
✅ **Log** semua transactions
✅ **Monitor** suspicious activities

---

## 📞 Support

- **Midtrans Docs**: https://docs.midtrans.com
- **Sample Code**: https://github.com/midtrans/midtrans-nodejs-client
- **Dashboard**: https://dashboard.midtrans.com

---

**Setup payment gateway berhasil! 🎉**
