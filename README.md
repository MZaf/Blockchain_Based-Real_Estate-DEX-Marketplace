# Blockchain_Based-Real_Estate-DEX-Marketplace
Blockchain based Real estate Marketplace. using AI and NFT features. House and property Sale Purchase, rental, property listings website.  Blockchain, Node.js React/Next.js stack 


## Project Structure

```text
Real-Estate-Website/
├── frontend/          → User-facing website (React + TypeScript + Vite)
├── admin/             → Admin dashboard (React + Vite)
├── backend/           → REST API server (Node.js + Express)
├── Image/             → README screenshots
└── .github/           → Issue templates, PR template, CODEOWNERS
```

**Frontend src/**

```text
├── components/
│   ├── ai-hub/            → AI Property Hub (search form, results, trends)
│   ├── common/            → Navbar, Footer, SEO, PageTransition
│   ├── home/              → Homepage sections
│   ├── properties/        → Filter sidebar, property cards
│   ├── property-details/  → Gallery, amenities, booking form
│   ├── about/             → About page sections
│   └── contact/           → Contact page sections
├── contexts/              → AuthContext (JWT state management)
├── hooks/                 → useSEO
├── pages/                 → All pages (lazy loaded via React.lazy)
└── services/              → api.ts (Axios client + API key injection)
```

**Backend**

```text
├── config/         → MongoDB, ImageKit, Nodemailer config
├── controller/     → Route handlers (property, appointment, AI search)
├── middleware/      → JWT auth, Multer uploads, stats tracking, request transform
├── models/         → Mongoose schemas (Property, User, Appointment, Stats)
├── routes/         → Express route definitions
├── services/
│   ├── firecrawlService.js  → Smart scraping (30+ cities, URL construction, retry logic)
│   └── aiService.js         → GPT-4.1 property analysis + location trends
├── utils/          → AI response validation & safe parsing
└── server.js       → Entry point (Helmet, CORS, rate limiting)
```

**Admin src/**

```text
├── components/     → Login, Navbar, ProtectedRoute
├── config/         → Property types, amenities constants
├── contexts/       → AuthContext (admin JWT state)
└── pages/          → Dashboard, Add, List, Update, Appointments
```

## Technology Stack

<div align="center">

### Frontend
[![React](https://img.shields.io/badge/React_18-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)](https://react.dev)
[![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)](https://www.typescriptlang.org)
[![Vite](https://img.shields.io/badge/Vite_6-646CFF?style=for-the-badge&logo=vite&logoColor=white)](https://vitejs.dev)
[![Tailwind](https://img.shields.io/badge/Tailwind_v4-0EA5E9?style=for-the-badge&logo=tailwindcss&logoColor=white)](https://tailwindcss.com)
[![Framer Motion](https://img.shields.io/badge/Framer_Motion-0055FF?style=for-the-badge&logo=framer&logoColor=white)](https://www.framer.com/motion)
[![React Router](https://img.shields.io/badge/React_Router_v7-CA4245?style=for-the-badge&logo=reactrouter&logoColor=white)](https://reactrouter.com)

### Backend
[![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)](https://nodejs.org)
[![Express](https://img.shields.io/badge/Express.js-000000?style=for-the-badge&logo=express&logoColor=white)](https://expressjs.com)
[![MongoDB](https://img.shields.io/badge/MongoDB_Atlas-47A248?style=for-the-badge&logo=mongodb&logoColor=white)](https://www.mongodb.com)
[![JWT](https://img.shields.io/badge/JWT-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white)](https://jwt.io)
[![Nodemailer](https://img.shields.io/badge/Nodemailer-0F9DCE?style=for-the-badge&logo=gmail&logoColor=white)](https://nodemailer.com)

### AI & Infrastructure
[![GPT-4.1](https://img.shields.io/badge/GPT--4.1-412991?style=for-the-badge&logo=openai&logoColor=white)](https://github.com/marketplace/models)
[![Firecrawl](https://img.shields.io/badge/Firecrawl-FF6600?style=for-the-badge&logo=fire&logoColor=white)](https://firecrawl.dev)
[![ImageKit](https://img.shields.io/badge/ImageKit_CDN-FF6F00?style=for-the-badge&logoColor=white)](https://imagekit.io)
[![Render](https://img.shields.io/badge/Render-46E3B7?style=for-the-badge&logo=render&logoColor=black)](https://render.com)

</div>

<br/>

## Architecture

**Complete System Architecture:**

```mermaid
flowchart LR
    subgraph Client["CLIENT LAYER"]
        FE["Frontend<br/>React 18 + TS<br/>Vercel"]
      AD["Admin Panel<br/>React + JS<br/>Vercel"]
    end
    
    subgraph API["API LAYER (Render)"]
        BE["Express.js<br/>Helmet + CORS<br/>Rate Limiter"]
    end
    
    subgraph Data["DATA & SERVICES"]
        DB[("MongoDB Atlas<br/>Database")]
        IK["ImageKit CDN<br/>Images"]
        FC["Firecrawl API<br/>Web Scraping<br/>Multi-source"]
        AI["GitHub Models<br/>GPT-4.1<br/>AI Ranking"]
        EMAIL["Brevo SMTP<br/>Email Service"]
    end
    
    FE -->|Axios| BE
    AD -->|Axios| BE
    BE -->|JWT Auth| DB
    BE -->|Upload| IK
    BE -->|Scrape| FC
    BE -->|Rank Props| AI
    BE -->|Send Mail| EMAIL
    
    style Client fill:#E8F4F8
    style API fill:#F0E8FF
    style Data fill:#FFF4E8
```

<br/>

**Multi-source AI Property Search Pipeline:**

```mermaid
graph TD
    A["React Frontend<br/>(TypeScript + Vite)"] -->|POST /api/ai/search| B["Express Backend<br/>(Node.js + Helmet + CORS)"]
    
    B -->|Build 3 parallel queries| C["Firecrawl API"]
    
    C -->|Query 1:<br/>site:airbnb.com| D1["Airbnb.com"]  
    C -->|Query 2:<br/>site:booking.com.com| D2["Booking.com"]
    C -->|Query 3:<br/>site:zillow.com| D3["Zillow.com"]
    
    D1 --> E["Parallel<br/>Scraping"]
    D2 --> E
    D3 --> E
    
    E -->|Full browser render<br/>per property| F["Firecrawl<br/>scrapeUrl"] 
    F -->|Structured JSON| G["Backend<br/>Processing"]
    
    G -->|Deduplicate<br/>by address| H["Code-side Filter<br/>Reject rentals/PG"]
    H -->|Clean properties| I["GitHub Models<br/>GPT-4.1"]
    
    I -->|Ranked + Insights| J["Response<br/>to Frontend"]
    
    J -->|Display with<br/>source badges| K["Rich Property<br/>Cards"]
    
    B --> L[("MongoDB<br/>Atlas")]
    C --> M["User API Keys<br/>(localStorage)"]
    F --> N["ImageKit CDN<br/>(Images)"]
    
    style B fill:#4A90E2
    style E fill:#FF6B6B
    style I fill:#7C3AED
    style K fill:#10B981
```
