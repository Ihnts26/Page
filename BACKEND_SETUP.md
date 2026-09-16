# Contact Form Backend

A simple Node.js/Express backend for handling contact form submissions. Sends emails via Gmail and is ready to deploy on Render.

## Setup Instructions

### Step 1: Clone & Install Dependencies

```bash
git clone https://github.com/Ihnts26/contact-form-backend.git
cd contact-form-backend
npm install
```

### Step 2: Set Up Gmail App Password

1. Enable 2FA on your Google account: https://myaccount.google.com/security
2. Go to App Passwords: https://myaccount.google.com/apppasswords
3. Select "Mail" and "Windows Computer" (or your device)
4. Google will generate a 16-character password - copy it

### Step 3: Configure Environment Variables

```bash
cp .env.example .env
```

Edit `.env` and add:
```
EMAIL_USER=your-email@gmail.com
EMAIL_PASSWORD=your-16-char-google-app-password
CONTACT_EMAIL=where-emails-should-go@example.com
PORT=3000
```

### Step 4: Test Locally

```bash
npm run dev
```

Visit http://localhost:3000 - you should see `{ "message": "Contact form backend is running!" }`

### Step 5: Deploy to Render

1. Push your code to GitHub (already done!)
2. Go to https://render.com and sign up
3. Click "New +" → "Web Service"
4. Connect your GitHub repository (Ihnts26/contact-form-backend)
5. Fill in details:
   - **Name:** `contact-form-backend`
   - **Runtime:** `Node`
   - **Build Command:** `npm install`
   - **Start Command:** `npm start`
6. Click "Advanced" and add environment variables:
   - `EMAIL_USER` = your Gmail
   - `EMAIL_PASSWORD` = your app password
   - `CONTACT_EMAIL` = where to send emails
7. Deploy!

### Step 6: Update Your Frontend

In your `index.html` on your Page repository, add this JavaScript to handle form submissions:

```javascript
const form = document.querySelector('#contact-form'); // your form ID

form.addEventListener('submit', async (e) => {
  e.preventDefault();
  
  const formData = {
    name: document.querySelector('#name').value,
    email: document.querySelector('#email').value,
    message: document.querySelector('#message').value,
  };

  try {
    const response = await fetch('https://your-render-url.onrender.com/api/contact', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(formData),
    });

    const data = await response.json();
    if (data.success) {
      alert('Message sent successfully!');
      form.reset();
    } else {
      alert('Error: ' + data.error);
    }
  } catch (error) {
    alert('Error sending message');
    console.error(error);
  }
});
```

## API Endpoint

**POST** `/api/contact`

Request body:
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "message": "Hello, I'd like to get in touch!"
}
```

Response:
```json
{
  "success": true,
  "message": "Email sent successfully!"
}
```

---

**That's it!** Your contact form backend is ready to go. 🚀
