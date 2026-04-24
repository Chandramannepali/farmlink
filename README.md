# 🌾 FarmLink

**Farm-to-Consumer Marketplace Platform** — A modern, full-stack web application that directly connects farmers with consumers, eliminating middlemen and ensuring fresh, traceable produce delivery.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Environment Variables](#environment-variables)
- [Authentication & Role System](#authentication--role-system)
- [Pages & Routes](#pages--routes)
- [Database Schema](#database-schema)
- [Key Integrations](#key-integrations)
- [Scripts](#scripts)
- [Contributing](#contributing)

---

## Overview

FarmLink is a role-based marketplace platform built with **React + TypeScript + Vite** and powered by **Supabase** as the backend. It supports three distinct user roles — **Farmer**, **Consumer**, and **Admin** — each with a dedicated dashboard and feature set. The platform features AI-driven pricing intelligence, real-time weather forecasting for farmers, QR-based product traceability, an integrated messaging system, and an **AI Voice Assistant** that helps users unfamiliar with digital interfaces navigate the app entirely by voice in their regional language.

---

## Features

### 🌱 Farmer Portal
| Feature | Description |
|---|---|
| **Farmer Dashboard** | Overview of today's earnings, active orders, listed products, and AI price score |
| **Product Management** | List, edit, and manage agricultural products with pricing |
| **Order Management** | View and process incoming consumer orders with status tracking |
| **AI Quality Scan** | Upload or capture crop images for AI-powered quality grading (Freshness, Color Uniformity, Size Consistency, Surface Quality) |
| **AI Price Intelligence** | Market-driven price predictions per crop with trend analysis, confidence scores, and actionable insights |
| **Climate & Weather** | Real-time weather data (temperature, humidity, wind, UV index) with 7-day forecast and farm-specific alerts |
| **Messaging** | In-app chat to communicate directly with consumers |

### 🛒 Consumer Portal
| Feature | Description |
|---|---|
| **Home / Browse** | Discover fresh produce listed by local farmers |
| **Product Detail** | View produce details, farm origin, traceability, and pricing |
| **Cart** | Add items and manage purchase cart |
| **Order Tracking** | Real-time order status with full tracking timeline |
| **Messaging** | Chat directly with farmers |

### 🛡️ Admin Portal
| Feature | Description |
|---|---|
| **Admin Dashboard** | Platform-wide statistics and key metrics |
| **User Management** | View and manage all farmers and consumers |
| **Revenue Analytics** | Track platform commissions and revenue |
| **Dispute Resolution** | Manage and resolve order disputes between farmers and consumers |
| **Platform Settings** | Configure global platform settings |

### 📦 Product Traceability
- Every product batch is assigned a unique **Lot ID**
- Scan a **QR code** to view the product's full journey: Harvested → Quality Inspected → Packed → Shipped → Delivered
- Traceability data includes farm name, location, harvest date, quality grade, certifications (Organic India, FSSAI, etc.), and cold-chain temperature

### 🎙️ AI Voice Assistant

FarmLink includes a built-in **AI Voice Assistant** designed specifically for farmers and consumers who are unfamiliar with smartphones or digital interfaces — a critical need in rural India where many users are first-generation smartphone owners.

**Problem it solves:** Many farmers and rural consumers struggle to navigate apps, read text-heavy screens, or type queries. The voice assistant removes this barrier entirely, letting users interact with FarmLink naturally by speaking in their own language — no typing or reading required.

| Capability | Description |
|---|---|
| **Multilingual Voice Input** | Speak in regional languages (Hindi, Telugu, Tamil, Marathi, Kannada, and more) using the Web Speech API |
| **Voice-Guided Navigation** | Say commands like *"Show my orders"* or *"Add tomatoes to cart"* and the app responds accordingly |
| **Text-to-Speech Responses** | The app reads out prices, order status, weather alerts, and notifications aloud for users who cannot read |
| **Farmer Onboarding by Voice** | Step-by-step voice guidance helps new farmers list their first product without needing to type anything |
| **Order Status Query** | Consumers ask *"Where is my order?"* and hear a spoken summary of their delivery status |
| **Price Query** | Farmers ask *"What is today's price for tomatoes?"* and receive AI pricing data read aloud instantly |
| **Offline-Friendly** | Gracefully degrades in low-connectivity rural areas with cached responses |

**Target Users:** First-generation smartphone users, elderly farmers, and rural consumers with limited digital or textual literacy.

**Tech:** Web Speech API (`SpeechRecognition` + `SpeechSynthesis`) integrated with **Bhashini** — India's national language AI platform — for robust regional language support across 22+ Indian languages.

---

## Tech Stack

| Category | Technology |
|---|---|
| **Frontend Framework** | React 18 + TypeScript |
| **Build Tool** | Vite 5 (with SWC plugin) |
| **Styling** | Tailwind CSS v3 + tailwindcss-animate |
| **UI Components** | shadcn/ui (Radix UI primitives) |
| **Routing** | React Router DOM v6 |
| **State Management** | React Context API |
| **Server State / Caching** | TanStack React Query v5 |
| **Backend / Auth / DB** | Supabase (PostgreSQL + Auth) |
| **Animations** | Framer Motion |
| **Charts** | Recharts |
| **Forms** | React Hook Form + Zod |
| **QR Codes** | qrcode.react |
| **Voice Assistant** | Web Speech API + Bhashini (22+ Indian languages) |
| **Testing** | Vitest + Testing Library |
| **Linting** | ESLint 9 |
| **Package Manager** | Bun (lock file) / npm |

---

## Project Structure

```
farmlink/
├── public/                    # Static assets
│   ├── favicon.ico
│   ├── placeholder.svg
│   └── robots.txt
│
├── src/
│   ├── assets/                # Image assets
│   │   ├── fresh-produce.jpg
│   │   └── hero-farm.jpg
│   │
│   ├── components/
│   │   ├── ui/                # shadcn/ui base components (40+ components)
│   │   ├── tracking/
│   │   │   ├── TraceabilityCard.tsx    # Displays lot/farm/cert info
│   │   │   └── TrackingTimeline.tsx    # Order journey timeline
│   │   ├── AppShell.tsx       # Main layout wrapper with navigation
│   │   ├── LocationSearch.tsx # Location search component (for weather)
│   │   ├── NavLink.tsx        # Sidebar/nav link component
│   │   ├── ProtectedRoute.tsx # Role-based route guard
│   │   └── StatCard.tsx       # Dashboard stat card component
│   │
│   ├── contexts/
│   │   └── AppContext.tsx     # Global auth state (user, role, session)
│   │
│   ├── data/
│   │   └── trackingMockData.ts # Mock order/traceability data
│   │
│   ├── hooks/
│   │   ├── use-mobile.tsx
│   │   ├── use-toast.ts
│   │   └── useAdminStats.ts   # Admin dashboard statistics hook
│   │
│   ├── integrations/
│   │   └── supabase/
│   │       ├── client.ts      # Supabase client initialization
│   │       └── types.ts       # Auto-generated DB types
│   │
│   ├── lib/
│   │   └── utils.ts           # cn() utility for class merging
│   │
│   ├── pages/
│   │   ├── admin/
│   │   │   ├── AdminDashboard.tsx
│   │   │   ├── AdminDisputes.tsx
│   │   │   ├── AdminRevenue.tsx
│   │   │   ├── AdminSettings.tsx
│   │   │   └── AdminUsers.tsx
│   │   ├── consumer/
│   │   │   ├── ConsumerCart.tsx
│   │   │   ├── ConsumerHome.tsx
│   │   │   ├── ConsumerOrders.tsx
│   │   │   └── ProductDetail.tsx
│   │   ├── farmer/
│   │   │   ├── FarmerClimate.tsx
│   │   │   ├── FarmerDashboard.tsx
│   │   │   ├── FarmerOrders.tsx
│   │   │   ├── FarmerPricing.tsx
│   │   │   ├── FarmerProducts.tsx
│   │   │   └── FarmerQualityScan.tsx
│   │   ├── shared/
│   │   │   └── ChatPage.tsx
│   │   ├── Index.tsx
│   │   ├── Login.tsx
│   │   ├── NotFound.tsx
│   │   ├── RoleSelect.tsx
│   │   ├── Signup.tsx
│   │   ├── TraceLot.tsx       # Public traceability page (via QR)
│   │   ├── VerifyOtp.tsx
│   │   └── Welcome.tsx
│   │
│   ├── test/
│   │   ├── example.test.ts
│   │   └── setup.ts
│   │
│   ├── App.tsx                # Root app with all routes defined
│   ├── main.tsx               # Entry point
│   └── index.css              # Global styles
│
├── .env                       # Environment variables (Supabase config)
├── .gitignore
├── components.json            # shadcn/ui config
├── eslint.config.js
├── index.html
├── package.json
├── postcss.config.js
└── vite.config.ts (implied)
```

---

## Getting Started

### Prerequisites

- **Node.js** v18+ or **Bun** runtime
- A **Supabase** account and project

### Installation

```bash
# 1. Clone the repository
git clone <your-repo-url>
cd farmlink

# 2. Install dependencies (using npm)
npm install

# or using Bun
bun install

# 3. Set up environment variables
cp .env.example .env
# Fill in your Supabase credentials (see Environment Variables section)

# 4. Start the development server
npm run dev
# App will be available at http://localhost:5173
```

---

## Environment Variables

Create a `.env` file in the project root with the following variables:

```env
VITE_SUPABASE_PROJECT_ID=your_supabase_project_id
VITE_SUPABASE_URL=https://your_project_id.supabase.co
VITE_SUPABASE_PUBLISHABLE_KEY=your_supabase_anon_key
```

> ⚠️ **Security Note:** The `.env` file contains your Supabase anon key. This key is safe to expose client-side (it has Row Level Security applied), but never commit your **service role key** to version control.

---

## Authentication & Role System

Authentication is handled entirely by **Supabase Auth**. The `AppContext` manages global auth state including session, user object, and role.

### Auth Flow

```
/ (Welcome)
  └─► /role-select       → User picks Farmer / Consumer
      └─► /login         → Email/password login
      └─► /signup        → Registration
          └─► /verify-otp → OTP verification
```

### Role-Based Access

After login, users are redirected based on their role stored in the `user_roles` Supabase table:

| Role | Default Redirect | Protected Paths |
|---|---|---|
| `farmer` | `/farmer` | `/farmer/*` |
| `consumer` | `/consumer` | `/consumer/*` |
| `admin` | `/admin` | `/admin/*` |

The `ProtectedRoute` component enforces role-based access. Authenticated users are redirected away from auth pages automatically via `AuthRoute`.

---

## Pages & Routes

| Path | Component | Access | Description |
|---|---|---|---|
| `/` | Welcome | Public | Landing/welcome screen |
| `/role-select` | RoleSelect | Public | Choose farmer or consumer role |
| `/login` | Login | Public | Login screen |
| `/signup` | Signup | Public | Registration |
| `/verify-otp` | VerifyOtp | Public | OTP verification |
| `/farmer` | FarmerDashboard | Farmer | Dashboard with earnings, orders, weather |
| `/farmer/products` | FarmerProducts | Farmer | Manage product listings |
| `/farmer/orders` | FarmerOrders | Farmer | View and process orders |
| `/farmer/climate` | FarmerClimate | Farmer | Weather & climate data |
| `/farmer/pricing` | FarmerPricing | Farmer | AI price predictions |
| `/farmer/scan` | FarmerQualityScan | Farmer | AI crop quality scanner |
| `/farmer/chat` | ChatPage | Farmer | Message consumers |
| `/consumer` | ConsumerHome | Consumer | Browse produce |
| `/consumer/browse` | ConsumerHome | Consumer | Browse produce (alias) |
| `/consumer/product/:id` | ProductDetail | Consumer | Product detail view |
| `/consumer/cart` | ConsumerCart | Consumer | Shopping cart |
| `/consumer/orders` | ConsumerOrders | Consumer | Order history & tracking |
| `/consumer/chat` | ChatPage | Consumer | Message farmers |
| `/admin` | AdminDashboard | Admin | Platform overview |
| `/admin/users` | AdminUsers | Admin | User management |
| `/admin/revenue` | AdminRevenue | Admin | Revenue analytics |
| `/admin/disputes` | AdminDisputes | Admin | Dispute management |
| `/admin/settings` | AdminSettings | Admin | Platform configuration |
| `/trace/:lotId` | TraceLot | **Public** | QR-accessible product traceability |
| `*` | NotFound | Public | 404 page |

---

## Database Schema

The Supabase PostgreSQL database contains the following core tables:

### `profiles`
| Column | Type | Description |
|---|---|---|
| `id` | UUID | Primary key |
| `user_id` | UUID | FK to auth.users |
| `full_name` | Text | User's display name |
| `phone` | Text | Contact number |
| `farm_name` | Text | (Farmers only) Farm name |
| `farm_location` | Text | (Farmers only) Farm location |
| `created_at` / `updated_at` | Timestamp | Audit fields |

### `user_roles`
| Column | Type | Description |
|---|---|---|
| `user_id` | UUID | FK to auth.users |
| `role` | Text | `farmer` / `consumer` / `admin` |

### `orders`
| Column | Type | Description |
|---|---|---|
| `id` | UUID | Primary key |
| `order_number` | Text | Human-readable order ID |
| `farmer_id` | UUID | FK to auth.users |
| `consumer_id` | UUID | FK to auth.users |
| `items` | JSON | Order line items |
| `total_amount` | Numeric | Order total |
| `commission_amount` | Numeric | Platform commission |
| `status` | Text | Order status |
| `delivery_address` | Text | Delivery location |
| `created_at` / `updated_at` | Timestamp | Audit fields |

### `disputes`
| Column | Type | Description |
|---|---|---|
| `id` | UUID | Primary key |
| `dispute_number` | Text | Human-readable dispute ID |
| `order_id` | UUID | FK to orders |
| `farmer_id` | UUID | Involved farmer |
| `consumer_id` | UUID | Involved consumer |
| `type` | Text | Dispute category |
| `description` | Text | Dispute details |
| `status` | Text | Open / Resolved / etc. |
| `resolution` | Text | Resolution notes |
| `amount` | Numeric | Disputed amount |

### `platform_settings`
| Column | Type | Description |
|---|---|---|
| `key` | Text | Setting name |
| `value` | Text | Setting value |
| `updated_by` | UUID | Last modified by |

---

## Key Integrations

### Supabase
- **Auth**: Email/password with OTP verification
- **Database**: PostgreSQL with typed client via `@supabase/supabase-js`
- **Edge Functions**: Used for AI pricing predictions and weather data (called from `FarmerPricing` and `FarmerClimate`)

### AI Features (via Supabase Edge Functions)
- **AI Price Intelligence**: Fetches market-driven price predictions for crops with trend indicators, confidence percentages, and market insights
- **AI Quality Scan**: Grades crop images across Freshness, Color Uniformity, Size Consistency, and Surface Quality

### AI Voice Assistant
- Built using the browser's **Web Speech API** (`SpeechRecognition` for voice input, `SpeechSynthesis` for spoken output)
- Integrated with **Bhashini** — India's national language AI platform — for regional language support across 22+ Indian languages including Hindi, Telugu, Tamil, Marathi, and Kannada
- Enables fully voice-driven app navigation, order tracking, price queries, and farmer onboarding for users with no digital literacy

### Weather (via Supabase Edge Function)
- Live weather data: temperature, feels-like, humidity, wind speed, UV index
- 7-day forecast with min/max temperatures and rainfall probability
- Farm-specific weather alerts (e.g., frost warning, heavy rain)
- Location search with custom `LocationSearch` component

### QR Traceability
- Each product lot gets a unique `Lot ID` (e.g., `LOT-GVF-2026-0206A`)
- Public URL `/trace/:lotId` renders a full traceability page with QR code (generated via `qrcode.react`)
- Journey steps: Harvested → Quality Inspected → Packed & Labeled → Shipped → In Transit → Delivered

---

## Scripts

```bash
# Start development server
npm run dev

# Build for production
npm run build

# Build in development mode
npm run build:dev

# Preview production build
npm run preview

# Run linter
npm run lint

# Run tests (single run)
npm run test

# Run tests in watch mode
npm run test:watch
```

---

## Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature-name`
3. Commit your changes: `git commit -m "feat: add your feature"`
4. Push to the branch: `git push origin feature/your-feature-name`
5. Open a Pull Request

### Code Style
- Use TypeScript for all new files
- Follow existing component patterns (functional components with hooks)
- Use `shadcn/ui` components wherever applicable
- Validate forms with `zod` schemas via `react-hook-form`
- Keep Supabase calls inside hooks or page components, not inside UI components

---

## License

This project is private. All rights reserved.

---

> Built with ❤️ for Indian farmers and consumers. Fresh produce, zero middlemen.
