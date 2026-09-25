# Time Keeping Web App


## Local development

### Frontend

1. Install dependencies:
   ```bash
   npm install
   ```
2. Start the development server:
   ```bash
   npm run dev
   ```
3. Open the app at http://localhost:5173

### Backend

1. Change into the server folder:
   ```bash
   cd server
   ```
2. Install backend dependencies:
   ```bash
   npm install
   ```
3. Create a `.env` file with your database and JWT settings:
   ```env
   MYSQL_HOST=127.0.0.1
   MYSQL_USER=root
   MYSQL_PASSWORD=your_password
   MYSQL_DATABASE=time_keeping_app
   MYSQL_PORT=3306
   JWT_SECRET=your_jwt_secret
   PORT=4000
   ```
4. Start the API:
   ```bash
   npm start
   ```

The backend will run on http://localhost:4000 and the frontend will call it at http://localhost:4000/api by default.

## cPanel deployment

### 1. Build the frontend

From the project root:

```bash
npm install
npm run build
```

If your frontend and backend are hosted on different domains, set the API base before building:

```bash
VITE_API_BASE=https://api.yourdomain.com/api
npm run build
```

Upload the contents of the `dist/` folder to your cPanel document root (for example `public_html/`) or to a subfolder such as `public_html/timekeeping/`.

### 2. Deploy the backend on cPanel

Use a separate Node.js application for the API.

1. Upload the contents of the `server/` folder to your cPanel Node.js app directory, or create a dedicated app folder such as `home/your-user/nodeapps/timekeeping-api`.
2. In cPanel, open Setup Node.js App.
3. Set:
   - Application Root: the folder that contains `package.json`
   - Startup File: `index.js`
   - Node.js version: 18 or newer
4. Add environment variables in cPanel:
   ```env
   MYSQL_HOST=your_mysql_host
   MYSQL_USER=your_mysql_user
   MYSQL_PASSWORD=your_mysql_password
   MYSQL_DATABASE=your_database_name
   MYSQL_PORT=3306
   JWT_SECRET=your_secure_secret
   PORT=3000
   ```
5. Install dependencies from the application root:
   ```bash
   npm install
   ```
6. Start the app.

The backend is configured to listen on `0.0.0.0` so it can be reached from cPanel’s Node.js environment.

### 3. Point the frontend to the backend

If the frontend is hosted on a different domain from the API, rebuild the frontend with:

```bash
VITE_API_BASE=https://api.yourdomain.com/api
npm run build
```

If the backend is hosted under the same main domain, you can also leave the default behavior and use the app’s same-origin API route.

## Notes

- The backend uses MySQL and will create its tables automatically on first start.
- Make sure your database user has permission to create tables and insert initial data.
- For production, use a strong `JWT_SECRET` value and a real database password.
