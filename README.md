# 🎫 MiniHelpDesk Ticket System

> A full-stack web application for managing support tickets.

**Group Members:** Abdul Rehman Shaikh · Mohsin Mustafa  
**Section:** BSCS VIB  
**Course:** Web Technologies I

---

## Tech Stack

| Layer      | Technology                                      |
|------------|-------------------------------------------------|
| Frontend   | React 18 · TypeScript · Vite · react-router-dom · Axios |
| Styling    | Tailwind CSS                                    |
| Backend    | Node.js · Express · TypeScript                  |
| Database   | MongoDB · Mongoose                              |

---

## Project Structure

```
helpdesk/
├── server/                     # Express + TypeScript backend
│   ├── src/
│   │   ├── controllers/
│   │   │   └── ticketController.ts   # GET, POST, DELETE logic
│   │   ├── models/
│   │   │   └── Ticket.ts             # Mongoose schema + ITicket interface
│   │   ├── routes/
│   │   │   └── ticketRoutes.ts       # Express router
│   │   └── server.ts                 # App entry point
│   ├── .env.example
│   ├── package.json
│   └── tsconfig.json
│
└── client/                     # React + TypeScript + Vite frontend
    ├── src/
    │   ├── api/
    │   │   └── axiosInstance.ts      # Axios base config
    │   ├── components/
    │   │   ├── ErrorBoundary.tsx     # React Error Boundary
    │   │   ├── GlobalErrorPage.tsx   # Friendly error UI
    │   │   ├── PriorityFilter.tsx    # Filter dropdown
    │   │   ├── TicketForm.tsx        # Controlled create form
    │   │   └── TicketList.tsx        # Ticket cards + delete
    │   ├── pages/
    │   │   └── Home.tsx              # Main page — all features wired
    │   ├── types/
    │   │   └── ticket.ts             # Shared TypeScript interfaces
    │   ├── App.tsx                   # Router + ErrorBoundary root
    │   └── main.tsx                  # React DOM entry
    ├── package.json
    └── vite.config.ts
```

---

## MongoDB Setup

1. Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) and create a free cluster.
2. Create a database user with read/write permissions.
3. Whitelist your IP (or use `0.0.0.0/0` for development).
4. Copy the connection string from **Connect → Drivers**.
5. It looks like:
   ```
   mongodb+srv://<username>:<password>@cluster0.xxxxx.mongodb.net/<dbname>?retryWrites=true&w=majority
   ```

**Local MongoDB alternative:**
```
mongodb://localhost:27017/helpdesk
```

---

## Installation & Setup

### 1. Backend (server)

```bash
# Navigate to the server directory
cd server

# Install dependencies
npm install

# Create your .env file from the example
cp .env.example .env

# Open .env and fill in your actual MONGO_URI:
# MONGO_URI=mongodb+srv://...
# PORT=5000
```

### 2. Frontend (client)

```bash
# Navigate to the client directory
cd client

# Install dependencies
npm install
```

---

## Running the Application

### Start the Backend

```bash
cd server
npm run dev
```

The API will be available at: **http://localhost:5000**

You should see:
```
✅  MongoDB connected successfully.
🚀  Server running on http://localhost:5000
```

### Start the Frontend

```bash
cd client
npm run dev
```

The app will be available at: **http://localhost:3000**

> ⚠️ Make sure the backend is running before starting the frontend.

---

## API Endpoints

| Method | Endpoint           | Description                              |
|--------|--------------------|------------------------------------------|
| GET    | `/tickets`         | Get all tickets                          |
| GET    | `/tickets?priority=High` | Filter tickets by priority         |
| POST   | `/tickets`         | Create a new ticket                      |
| DELETE | `/tickets/:id`     | Delete a ticket by MongoDB `_id`         |
| GET    | `/health`          | Health check                             |

### POST /tickets — Request Body

```json
{
  "subject": "Login page not loading",
  "description": "The login page shows a blank screen on Chrome.",
  "priority": "High"
}
```

### Error Response Format

All errors return:
```json
{
  "message": "Human-readable error description"
}
```

---

## Ticket Model

| Field         | Type     | Values                        | Notes              |
|---------------|----------|-------------------------------|--------------------|
| `ticketId`    | String   | Auto-generated `TKT-<timestamp>` | Unique            |
| `subject`     | String   | Max 120 chars                 | Required           |
| `description` | String   | Max 1000 chars                | Required           |
| `priority`    | Enum     | `Low` · `Medium` · `High`    | Required           |
| `status`      | Enum     | `Open` · `In Progress` · `Closed` | Default: `Open` |
| `createdAt`   | Date     | Auto-set by MongoDB           |                    |

---

## Implemented Features

### Base Functionality
- ✅ **GET /tickets** — Fetch and display all tickets from MongoDB
- ✅ **POST /tickets** — Create ticket via controlled React form with validation
- ✅ **DELETE /tickets/:id** — Delete ticket with confirmation spinner
- ✅ **Loading state** — Skeleton cards during fetch; spinner on submit/delete
- ✅ **Error state** — Inline error messages for list and form failures

### Assigned Product Feature — Filter by Priority
- ✅ **Frontend dropdown** — `All / Low / Medium / High` above ticket list
- ✅ **Backend query param** — `GET /tickets?priority=High` with validation
- ✅ **MongoDB filtering** — `Ticket.find({ priority })` or `Ticket.find({})`
- ✅ Ticket count updates dynamically with the active filter

### Assigned Engineering Feature — Global Error Page / Error UI
- ✅ **React Error Boundary** — Catches JS runtime crashes and renders fallback UI
- ✅ **GlobalErrorPage component** — Friendly "Something went wrong 😕" with Reload button
- ✅ **Network error detection** — Shown when backend is offline or fetch fails entirely
- ✅ **Proper HTTP status codes** — 400, 404, 500 with `{ message }` JSON on backend
- ✅ **App wrapped** — `<ErrorBoundary>` wraps `<App>` in `main.tsx`

---

## Environment Variables

```env
# server/.env
MONGO_URI=mongodb+srv://<username>:<password>@cluster0.xxxxx.mongodb.net/helpdesk
PORT=5000
```

---

*MiniHelpDesk · Web Technologies I · BSCS VIB*
