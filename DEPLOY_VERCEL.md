# 🚀 Deploying AnonReply to Vercel (Step-by-Step Guide)

This guide walks you through deploying **AnonReply** to [Vercel](https://vercel.com/) paired with a free cloud database on [MongoDB Atlas](https://cloud.mongodb.com/).

---

## 📋 Prerequisites
Before you start, make sure you have:
1. A **[GitHub](https://github.com/)** account.
2. A free **[MongoDB Atlas](https://www.mongodb.com/cloud/atlas)** account.
3. A free **[Vercel](https://vercel.com/)** account.
4. *(Optional)* A free **[Resend](https://resend.com/)** account for email delivery to your notification inbox.

---

## 🗄️ Step 1: Set Up MongoDB Atlas (Cloud Database)

AnonReply uses MongoDB with a **native 72-hour TTL (Time-To-Live) index** that automatically purges all tickets, chats, and messages 3 days after creation.

### 1.1 Create a Free Cluster
1. Sign in to [MongoDB Atlas](https://cloud.mongodb.com/).
2. Click **Create** &rarr; Select the **M0 (Free)** tier.
3. Choose a cloud provider and region closest to your users.
4. Click **Create Deployment**.

### 1.2 Create Database User & Password
1. Under **Security** in the left sidebar, click **Database Access**.
2. Click **Add New Database User**.
3. Choose **Password** authentication:
   - **Username:** e.g., `anonreply_admin`
   - **Password:** e.g., `SecureDbPass123!` *(avoid special symbols like `@`, `:`, `/`, or `%` in the password, or URL-encode them)*.
4. Under **Database User Privileges**, select **Read and write to any database**.
5. Click **Add User**.

### 1.3 Configure Network Access (CRITICAL)
> [!IMPORTANT]
> Because Vercel serverless functions connect from dynamic cloud IP addresses, you **must allow connections from anywhere (`0.0.0.0/0`)** in MongoDB Atlas. If you skip this, your Vercel deployment will fail with connection timeouts!

1. In MongoDB Atlas, click **Network Access** under **Security** in the left sidebar.
2. Click **Add IP Address**.
3. Click **Allow Access from Anywhere** (this automatically fills `0.0.0.0/0`).
4. Click **Confirm**. Wait ~30 seconds until the status shows **Active**.

### 1.4 Get Your Connection String
1. Go to **Database** &rarr; click **Connect** on your cluster.
2. Select **Drivers** (Node.js).
3. Copy the connection string:
   ```text
   mongodb+srv://<username>:<password>@cluster0.example.mongodb.net/?retryWrites=true&w=majority
   ```
4. Insert your actual password and append the database name `/anonreply`:
   ```text
   mongodb+srv://anonreply_admin:SecureDbPass123!@cluster0.example.mongodb.net/anonreply?retryWrites=true&w=majority
   ```
   *(Keep this string handy for Vercel Step 4).*

---

## 🚢 Step 2: Push This Code to GitHub
1. Create a new repository on your GitHub account (e.g. `anonreply`).
2. Initialize and push this folder:
   ```bash
   git init
   git add .
   git commit -m "feat: initial commit of AnonReply Vercel Edition"
   git branch -M main
   git remote add origin https://github.com/AnvilTriesToCode/AnonReplyVercel.git
   git push -u origin main
   ```

---

## ⚡ Step 3: Import Project into Vercel

1. Log in to your [Vercel Dashboard](https://vercel.com/dashboard).
2. Click **Add New...** &rarr; **Project**.
3. Locate your GitHub repository and click **Import**.
4. In the configuration window:
   - **Framework Preset:** `Next.js` (auto-detected)
   - **Root Directory:** `./`

---

## 🔐 Step 4: Add Environment Variables in Vercel

Expand the **Environment Variables** section on the Vercel import screen (or go to **Project Settings &rarr; Environment Variables**):

| Key | Example Value | Description |
| :--- | :--- | :--- |
| `MONGODB_URI` | `mongodb+srv://user:pass@cluster0.example.mongodb.net/anonreply?retryWrites=true&w=majority` | Your MongoDB Atlas connection string |
| `NEXTAUTH_SECRET` | `openssl rand -base64 32` | 32+ character string used to encrypt JWT sessions |
| `NEXTAUTH_URL` | `https://your-project.vercel.app` | Your canonical Vercel production domain |
| `ADMIN_PASSWORD` | `ChangeMe123!` | Initial password for the 5 default umbrella admin seats |
| `ADMIN_EMAIL` | `admin@example.com` | Primary administrator contact email |
| `NOTIFICATION_ROUTING_EMAIL` | `notifications@example.com` | Destination inbox for incoming user tickets & live chats |
| `RESEND_API_KEY` | `re_123456789...` | *(Optional)* Resend API key for real email delivery |
| `EMAIL_FROM` | `onboarding@resend.dev` | *(Optional)* Sender email address |

> [!TIP]
> Ensure all three environments (**Production**, **Preview**, and **Development**) are checked for each environment variable!

Click **Deploy**. Vercel will build and deploy your project in ~60 seconds.

---

## 👥 Step 5: Initializing Your Admin Seats

Once deployed, visit your admin sign-in portal:
`https://your-project.vercel.app/admin/login`

Default initial credentials:
- **Username:** `admin` (or `moderator1`, `moderator2`, `moderator3`, `moderator4`)
- **Password:** The `ADMIN_PASSWORD` you configured in Step 4 (default: `ChangeMe123!`)

Once logged in, open the **Admin Suite** at `/admin` to customize role titles and update passwords for each seat.

---

## 🛠️ Post-Deployment Verification
- Open your homepage (`/`) &rarr; Click **⚡ Use Services Immediately** to test anonymous instant user generation.
- Submit a test ticket and verify that it appears in your `/admin` queue.
- Test a 1-on-1 live chat request at `/live`.
