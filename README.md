# ♻️ IndusCycle

**The Intelligent B2B Marketplace for Industrial Waste Exchange**

> Turn factory byproducts into raw materials. Save costs, reduce landfills, and automate compliance — powered by AI.

---

## 🌟 Overview

IndusCycle is a full-stack web platform that connects factories producing industrial waste with businesses that can reuse those materials. It features **AI-powered matching** (Google Gemini), **real-time deal negotiation** (Socket.io), **waste forecasting analytics**, **logistics coordination** with map integration, and **auto-generated compliance documents**.

---

## 🚀 Features

### 🔐 Authentication & Onboarding
- Multi-step factory registration with industry type, location, and license upload
- JWT-based authentication with role-based access (`factory_manager`)
- Protected routes with automatic token refresh

### 📊 Dashboard — The Command Center

The Dashboard is the core hub where producers and buyers manage every aspect of their waste exchange operations:

- **KPI Overview Cards** — At-a-glance metrics for Active Listings, Open Deals, cumulative CO₂ Offset (auto-calculated from deals), and Missing Compliance Docs — with real-time Socket.io updates
- **My Requirements** — Full CRUD management for raw materials your factory needs (material type, monthly quantity, priority level). Each requirement has an **"AI Match"** button that triggers Gemini-powered supplier discovery
- **🤖 AI-Powered Matching** — One-click Gemini AI analysis that scores every marketplace listing against your requirement on material match, volume alignment, location proximity, and timing compatibility. Results show a ranked list with **match percentage rings**, supplier details, CO₂ savings per ton, and a **"Fast Deal"** button to instantly initiate a deal from the match results
- **Your Listings (Selling)** — View all your active waste listings with material type, quantity, unit, and creation date. Quickly add new listings via the Create Listing page
- **Marketplace (Buying)** — Browse all industrial byproducts listed by other factories. Each listing shows supplier name, location, average monthly quantity, and potential CO₂ savings. Initiate a deal directly with one click
- **Open Deals** — Track all active deals (as buyer or seller). Sellers can **approve** pending deals. Once approved, mark **transport as complete** and then download the auto-generated **Compliance PDF**
- **💬 Real-Time Chat** — Socket.io-powered in-deal messaging. Join a deal chat room, send messages with optimistic UI updates, and receive instant replies. Chat history is persisted via the Messages API
- **Logistics Card** — Quick-access widget linking to the full Logistics page with transporter discovery

#### Role-Specific Dashboards:
- **Producer Dashboard** (`/producer`) — Focused view for monitoring your listed waste materials and production output
- **Buyer Dashboard** (`/buyer`) — Focused view for tracking incoming material requirements and deal statuses

### 🏭 Waste Management
- **Create Listings** — List factory waste with type, quantity, unit, hazard level, and availability
- **Waste Profiles** — Manage detailed waste output profiles for each factory
- **Waste Ratios** — Configure production-to-waste conversion ratios
- **Emission Factors** — Track virgin vs. recycled emission factors per material for CO₂ savings calculations

### 🤝 Deal Flow
- **Initiate Deals** — Buyers select listings and specify quantity to start negotiations
- **Real-Time Chat** — Socket.io-powered messaging within each deal room
- **Deal Approval** — Sellers can approve/reject deals with instant notifications
- **CO₂ & Landfill Impact** — Automatic calculation of CO₂ saved and landfill diverted per deal
- **Compliance PDF** — Auto-generated PDF documents with producer/buyer details, material info, environmental impact, and signature blocks

### 📈 Waste Forecast
- Interactive line charts with historical data and predicted waste output (3-month & 12-month views)
- Confidence interval bands (upper/lower bounds)
- KPI cards: predicted waste, CO₂ savings potential, revenue forecast, pre-match buyer opportunities
- Drag-and-drop data upload support

### 🚚 Logistics
- **Mappls Maps Integration** — Nearby transporter discovery using geolocation APIs
- Known courier brand contact matching (DTDC, FedEx, BlueDart, Delhivery, etc.)
- Deal-based route visualization between source and destination factories

### 📝 Compliance Documents
- Regulatory document management and tracking
- PDF generation for completed deals with full audit trail

### 🌗 Theme
- Dark/Light mode toggle with system preference detection
- Persistent theme via localStorage

---

## 🛠️ Tech Stack

| Layer        | Technology                                                     |
|:-------------|:---------------------------------------------------------------|
| **Frontend** | React 19, Vite 7, Tailwind CSS 4, Framer Motion, Recharts     |
| **Backend**  | Node.js, Express 5, Socket.io                                  |
| **Database** | MongoDB Atlas (Mongoose ODM)                                   |
| **AI**       | Google Gemini (via `@google/genai` SDK)                        |
| **Auth**     | JWT (jsonwebtoken), bcryptjs                                   |
| **Maps**     | Mappls (MapMyIndia) API                                        |
| **PDF**      | PDFKit                                                         |
| **Uploads**  | Multer                                                         |

---

## 📁 Project Structure

```
codelitehacks/
├── client/                    # React frontend (Vite)
│   ├── src/
│   │   ├── components/        # Reusable UI (Button, Card, Layout, ThemeToggle)
│   │   ├── context/           # AuthContext (JWT state management)
│   │   ├── lib/               # API helper & utilities
│   │   ├── pages/             # All page components
│   │   │   ├── Landing.jsx         # Public homepage
│   │   │   ├── Login.jsx           # Login form
│   │   │   ├── Register.jsx        # Multi-step factory registration
│   │   │   ├── Dashboard.jsx       # Main dashboard (listings, deals, AI matching, chat)
│   │   │   ├── ProducerDashboard.jsx
│   │   │   ├── BuyerDashboard.jsx
│   │   │   ├── CreateListing.jsx   # Create waste listing form
│   │   │   ├── DealFlow.jsx        # Deal management
│   │   │   ├── WasteForecast.jsx   # Forecasting charts & analytics
│   │   │   ├── Logistics.jsx       # Map-based transporter discovery
│   │   │   └── ComplianceDocs.jsx  # Compliance document management
│   │   ├── App.jsx            # Routes & providers
│   │   └── main.jsx           # Entry point
│   ├── vite.config.js         # Vite config with Mappls API proxy
│   └── package.json
│
├── server/                    # Express backend
│   ├── config/
│   │   └── database.js        # MongoDB Atlas connection
│   ├── middleware/
│   │   └── auth.js            # JWT verification middleware
│   ├── models/                # Mongoose schemas
│   │   ├── users.model.js
│   │   ├── factories.model.js
│   │   ├── factoryWasteProfiles.model.js
│   │   ├── deals.model.js
│   │   ├── messages.model.js
│   │   ├── requirements.model.js
│   │   ├── emissionFactors.model.js
│   │   ├── formulas.model.js
│   │   ├── productionSchedules.model.js
│   │   └── wasteRatios.model.js
│   ├── routes/                # Express route handlers
│   │   ├── auth.js            # Register, Login, Me
│   │   ├── factories.js       # Factory CRUD
│   │   ├── wasteProfiles.js   # Waste listing CRUD
│   │   ├── requirements.js    # Requirements CRUD + AI matching (Gemini)
│   │   ├── deals.js           # Deal lifecycle + compliance PDF generation
│   │   ├── marketplace.js     # Public marketplace search
│   │   ├── messages.js        # Chat message history
│   │   ├── emissionFactors.js
│   │   ├── productionSchedules.js
│   │   └── wasteRatios.js
│   ├── uploads/               # Multer file storage
│   ├── index.js               # Server entry + Socket.io setup
│   └── package.json
│
└── package.json               # Root dependencies
```

---

## ⚡ Getting Started

### Prerequisites

- **Node.js** v18+
- **npm** v9+
- **MongoDB Atlas** account (or a local MongoDB instance)

### 1. Clone the Repository

```bash
git clone https://github.com/sethiakshad/codelitehacks.git
cd codelitehacks
```

### 2. Configure Environment Variables

**Server** (`server/.env`):

```env
PORT=4000
JWT_SECRET=your_jwt_secret_here
JWT_EXPIRY=1d
MONGO_URI=mongodb+srv://<user>:<password>@<cluster>.mongodb.net/codelite?retryWrites=true&w=majority
GEMINI_API_KEY=your_google_gemini_api_key
```

**Client** (`client/.env`):

```env
VITE_API_URL=http://localhost:4000
```

### 3. Install Dependencies

```bash
# Server
cd server
npm install

# Client
cd ../client
npm install
```

### 4. Run the Application

```bash
# Terminal 1 — Start the backend
cd server
npm run dev

# Terminal 2 — Start the frontend
cd client
npm run dev
```

- **Frontend**: http://localhost:5173
- **Backend API**: http://localhost:4000

---

## 📡 API Endpoints

| Method   | Endpoint                              | Description                        | Auth |
|:---------|:--------------------------------------|:-----------------------------------|:-----|
| `GET`    | `/api/auth/me`                        | Get current user profile           | ✅    |
| `GET`    | `/api/factories`                      | List factories                     | ✅    |
| `POST`   | `/api/factories`                      | Create a factory profile           | ✅    |
| `GET`    | `/api/waste-profiles`                 | List waste profiles                | ✅    |
| `POST`   | `/api/waste-profiles`                 | Create a waste listing             | ✅    |
| `GET`    | `/api/marketplace`                    | Browse all marketplace listings    | ✅    |
| `GET`    | `/api/requirements`                   | List user's material requirements  | ✅    |
| `POST`   | `/api/requirements`                   | Create a new requirement           | ✅    |
| `GET`    | `/api/requirements/:id/matches`       | AI-powered supplier matching       | ✅    |
| `POST`   | `/api/deals`                          | Initiate a new deal                | ✅    |
| `GET`    | `/api/deals`                          | Get user's deals (buyer + seller)  | ✅    |
| `PUT`    | `/api/deals/:id/status`               | Update deal status (approve/reject)| ✅    |
| `GET`    | `/api/deals/:id/compliance-pdf`       | Download compliance PDF            | ✅    |
| `GET`    | `/api/messages/:dealId`               | Get chat history for a deal        | ✅    |
| `GET`    | `/api/emission-factors`               | List emission factors              | ✅    |
| `GET`    | `/api/waste-ratios`                   | List waste ratios                  | ✅    |
| `GET`    | `/api/production-schedules`           | List production schedules          | ✅    |

---

## 🔌 Real-Time Events (Socket.io)

| Event            | Direction       | Description                               |
|:-----------------|:----------------|:------------------------------------------|
| `identify`       | Client → Server | Join user's personal notification room    |
| `join_chat`      | Client → Server | Join a deal-specific chat room            |
| `leave_chat`     | Client → Server | Leave a deal chat room                    |
| `send_message`   | Client → Server | Send a message in a deal chat             |
| `chat_message`   | Server → Client | Broadcast new message to deal participants|
| `new_deal`       | Server → Client | Notify seller of a new incoming deal      |

---

## 🤖 AI Matching (Gemini)

The AI matching engine uses **Google Gemini Flash** to evaluate buyer requirements against all marketplace listings. It considers:

1. **Material Match** — Exact material type alignment
2. **Volume Match** — Quantity and unit compatibility
3. **Location Proximity** — Geographic distance between factories
4. **Timing Compatibility** — Buyer priority vs. seller availability

Returns a ranked list of matches with percentage scores and human-readable reasoning.

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
