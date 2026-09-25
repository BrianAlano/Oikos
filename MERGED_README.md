# Oikos Technologies + Time Keeping — Merged Project

This project combines the Oikos Technologies landing page and the Time Keeping application into one Vite/React project.

## Flow
1. The app opens on the Oikos Technologies landing page.
2. Click **Launch App** under **Time Keeping App** to open the existing timekeeping system.
3. A **← Back to Oikos** button appears while the timekeeping system is open.
4. The original timekeeping backend remains in `server/`.

## Run
Install dependencies:
```bash
npm install
```

Start the frontend:
```bash
npm run dev
```

Start the backend from the `server` folder:
```bash
cd server
npm install
npm start
```

If the backend is hosted separately, set `VITE_API_BASE` to its API URL.

## Important
The original timekeeping logic, authentication, work schedule, users, time entries, and PDF reporting code were retained. The Oikos landing page was integrated as the entry screen rather than as a separate application.
