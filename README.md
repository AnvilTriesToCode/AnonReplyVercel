# 🛡️ AnonReply — Vercel Edition

> **AnonReply** is a modern, lightweight, and strictly confidential anonymous support and real-time live conversation platform built for organizations, student communities, and support teams.

Designed for seamless **1-click deployment on Vercel** paired with a free cloud database on **MongoDB Atlas**.

---

## ✨ Features

* **🔒 100% Anonymous Communication:** Users can generate instant disposable handles with 1 click without entering passwords, phone numbers, or emails.
* **⚡ Real-Time Live Conversations:** Private 1-on-1 live chat sessions with polling-assisted streaming for instant assistance.
* **⏰ Native 72-Hour TTL Auto-Deletion:** All conversations, tickets, chats, and messages automatically purge from MongoDB precisely 72 hours after creation via native database indexes.
* **👥 Multi-Admin Architecture:** Team moderators log in using individual seats (`admin`, `moderator1`, `moderator2`, etc.), while all outgoing replies and live chat greetings are unified under the public brand **`SupportTeam`**.
* **🔍 Internal Team Attribution:** Internal moderators see which teammate wrote each response (`SupportTeam via @admin`), preventing duplicate replies while keeping moderator identities hidden from the public.
* **📧 Email Routing & Instant Alerts:** Incoming user requests and live chats are routed to your organization's notification inbox (`notifications@example.com`) with ticket descriptions and 1-click review links.

---

## 🚀 Complete Step-by-Step Vercel Setup Guide

### Step 1: Set Up Free MongoDB Atlas Cloud Database
AnonReply uses MongoDB with a **native 72-hour TTL (Time-To-Live) index** that automatically purges all tickets, chats, and messages 3 days after creation.

1. **Create Free Cluster:**
   - Sign up at [MongoDB Atlas](https://cloud.mongodb.com/).
   - Click **Create Deployment** &rarr; Select the **M0 (Free)** tier.
   - Choose a region closest to your users.
2. **Create Database User:**
   - Under **Security** in the left sidebar, click **Database Access** &rarr; **Add New Database User**.
   - Set **Authentication Method** to Password (e.g. `anonreply_admin` and a strong password).
   - Under **Database User Privileges**, select **Read and write to any database**.
3. **Configure Network Access (Crucial for Vercel):**
   - Under **Security**, click **Network Access** &rarr; **Add IP Address**.
   - Click **Allow Access from Anywhere** (`0.0.0.0/0`).
   - Click **Confirm** and wait ~30 seconds for the status to show **Active**.
4. **Copy Connection String:**
   - Go to **Database** &rarr; click **Connect** &rarr; choose **Drivers** (Node.js).
   - Copy the string and append `/anonreply` database name:
     ```text
     mongodb+srv://<username>:<password>@cluster0.example.mongodb.net/anonreply?retryWrites=true&w=majority
     ```

---

### Step 2: Push to Your GitHub Repository
Initialize a git repository in this folder and push to your GitHub account:
```bash
git init
git add .
git commit -m "feat: initial commit of AnonReply"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
git push -u origin main
```

---

### Step 3: Import Project into Vercel
1. Log in to your [Vercel Dashboard](https://vercel.com/dashboard).
2. Click **Add New...** &rarr; **Project**.
3. Locate your GitHub repository and click **Import**.
4. Leave the **Framework Preset** as `Next.js` and **Root Directory** as `./`.

---

### Step 4: Configure Environment Variables in Vercel
Expand the **Environment Variables** section on Vercel and add the following keys. **Make sure to check all three environments: Production, Preview, and Development:**

| Key | Example Value | Description |
| :--- | :--- | :--- |
| `MONGODB_URI` | `mongodb+srv://user:pass@cluster0.example.mongodb.net/anonreply?retryWrites=true&w=majority` | Your MongoDB Atlas connection string |
| `NEXTAUTH_SECRET` | `openssl rand -base64 32` | 32+ character random secret used to sign session JWTs |
| `NEXTAUTH_URL` | `https://your-project.vercel.app` | Your production Vercel domain |
| `ADMIN_PASSWORD` | `ChangeMe123!` | Initial password for the 5 default moderator seats |
| `ADMIN_EMAIL` | `admin@example.com` | Primary administrator contact email |
| `NOTIFICATION_ROUTING_EMAIL` | `notifications@example.com` | Destination inbox for incoming user tickets & live chats |
| `RESEND_API_KEY` | `re_123456789...` | *(Optional)* Resend API key for real email notifications |
| `EMAIL_FROM` | `onboarding@resend.dev` | *(Optional)* Sender email address |

Click **Deploy**. Your application will build and deploy in ~60 seconds.

---

### Step 5: Admin Team Accounts & Initial Access

Once deployed, visit your admin sign-in portal:
`https://your-project.vercel.app/admin/login`

Default initial credentials:
| Seat | Admin Username | Role | Initial Password |
| :---: | :--- | :--- | :--- |
| **1** | **`admin`** | Primary Administrator (Owner) | `ChangeMe123!` |
| **2** | **`moderator1`** | Support Moderator | `ChangeMe123!` |
| **3** | **`moderator2`** | Support Moderator | `ChangeMe123!` |
| **4** | **`moderator3`** | Support Moderator | `ChangeMe123!` |
| **5** | **`moderator4`** | Support Moderator | `ChangeMe123!` |

*(Once signed in, open the **Admin Suite** at `/admin` to customize role titles and update passwords for each seat).*

---

## 📖 Additional Documentation
For a dedicated standalone guide, see:
* **[DEPLOY_VERCEL.md](./DEPLOY_VERCEL.md)**

---

## 📦 Tech Stack
- **Framework:** Next.js 15 (App Router, React 19, Server Actions)
- **Database:** MongoDB & Mongoose (with native 72-hour TTL indexing)
- **Authentication:** NextAuth.js (JWT session strategy, bcrypt password hashing for admins)
- **Styling:** Tailwind CSS + Radix UI components (Creative Tim Dark PRO theme)
- **Email:** Resend API (optional)

---

## 📄 License
MIT License. Free for personal, organizational, and educational use.
