# Deployment Guide

## cPanel Deployment

This app can be deployed to cPanel in two parts:

- Frontend: build the Vite app and upload the generated files to your public_html folder.
- Backend: deploy the Express API as a Node.js application using cPanel’s Setup Node.js App feature.

### Frontend build

```bash
npm install
npm run build
```

If your API lives on a different domain, build the frontend with:

```bash
VITE_API_BASE=https://api.yourdomain.com/api npm run build
```

Upload the contents of the `dist/` folder to your cPanel document root.

### Backend deployment

1. Upload the contents of the `server/` folder to a Node.js app directory in cPanel.
2. In cPanel, open Setup Node.js App.
3. Set the application root to the folder containing `package.json`.
4. Use `index.js` as the startup file.
5. Add the environment variables below:

```env
MYSQL_HOST=your_mysql_host
MYSQL_USER=your_mysql_user
MYSQL_PASSWORD=your_mysql_password
MYSQL_DATABASE=your_database_name
MYSQL_PORT=3306
JWT_SECRET=your_secure_secret
PORT=3000
```

6. Install dependencies and start the app.

The backend listens on `0.0.0.0` so it works correctly in cPanel’s Node.js environment.

---

# Option A: Two-Service Deployment on Railway

This guide shows how to deploy frontend and backend as separate Railway services.

## Frontend Service (Static)

### Steps
1. Create a new Railway project (or use existing)
2. Select "Deploy from GitHub"
3. Point to this repo
4. In the **environment variables**, set:
   ```
   VITE_API_BASE=https://<backend-service-url>/api
   ```
   Replace `<backend-service-url>` with your backend service URL (e.g., `timekeeping-backend.up.railway.app`)

5. Set the **build command** to: `npm run build`
6. Set the **start command** to: `npx serve dist` or configure for static hosting
7. Point to the `dist/` directory as the public folder

**OR** use Railway's static site feature (no Node runtime needed for frontend only).

---

## Backend Service (Node.js)

### Steps
1. In the same or new Railway project, create another service
2. Select "Deploy from GitHub"
3. Point to this repo
4. In the **environment variables**, set:
   ```
   MYSQL_HOST=<mysql-host>
   MYSQL_USER=<mysql-user>
   MYSQL_PASSWORD=<mysql-password>
   MYSQL_DATABASE=railway
   MYSQL_PORT=<mysql-port>
   JWT_SECRET=your-secret-key
   PORT=4000
   ```

   Get these from the **MySQL plugin** in Railway.

5. Set the **build command** to: `npm install --prefix server`
6. Set the **start command** to: `node server/index.js`
7. Deploy

---

## MySQL Database

1. In your Railway project, go to **Plugins**
2. Select **MySQL**
3. Railway will provision a database and auto-fill the connection details
4. Copy these values into the backend service **environment variables**

---

## After Deployment

- **Frontend**: deployed to `https://<frontend-url>`
- **Backend**: deployed to `https://<backend-url>`
- Frontend API calls will go to `https://<backend-url>/api/*`

Your deployed app will now be fully functional with database connectivity.

---

## Local Development

For local testing (already set up):
- Frontend: `npm run dev` (port 5173)
- Backend: `npm start` in `server/` folder (port 4000)
- MySQL: via XAMPP or local service

The frontend automatically detects localhost and uses `http://localhost:4000/api`.
