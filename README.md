# FindKia 
https://findkia.com

**FindKia** is a cross-platform app, digital platform developed by Hudaz Tech and officially launched in 2022. The platform is designed to bridge the gap between the general public and a wide range of service providers offering services and products across multiple categories. These categories include marriage halls, restaurants, catering services, vehicles such as cars and pickups, and various other commercial and household services.

**In addition**, Findkia provides a convenient solution for connecting users with skilled manpower and human resources from different professions, including electricians, plumbers, mechanics, technicians, and other service specialists.

- The platform enables service providers to register online through the Findkia mobile application using their registered mobile number along with National Identity Card (NIC) verification. With a highly economical registration model, businesses and individuals can market their services by creating their own dedicated mobile application pages. Service providers can easily upload their business details, service descriptions, pricing information, images, and videos to showcase their offerings professionally.

- Once the registration and verification process is completed and approved, the service provider’s profile becomes publicly visible on the Findkia platform. Registered providers can further promote their business by sharing their personalized page links through integrated social media sharing options available within the Findkia application. Users receiving these links can directly access the provider’s page through either the Findkia mobile application or the web platform.

- For customers and the general public, Findkia offers a convenient and user-friendly experience to search for required services, vehicles, products, or manpower from the comfort of their homes. Users can browse detailed provider profiles, view product and service rates, examine photos and videos, access location information, and directly contact service providers using the contact details available on the platform.

- Findkia supports multiple platforms, including Android applications, iOS applications, and web browsers, ensuring accessibility and convenience for a broad range of users across different devices.


## Screenshots

| SideMenu Screen | Services Home Screen | Taxi Service | App Icon |
|----------------|----------------|----------------|----------------|
| ![1](Images/Mobile/SideMenu.jpeg) | ![2](Images/Mobile/Home.jpeg) | ![3](Images/Mobile/SpTaxiService.jpeg) | ![4](Images/Mobile/AppIcon.jpeg) |
| Taxi Search Card View | Marriage Hall Search List View | Search Details | Service Provider Photo |
| ![5](Images/Mobile/SearchList.jpeg) | ![6](Images/Mobile/SearchList2.jpeg) | ![7](Images/Mobile/SearchDetails.jpeg) | ![8](Images/Mobile/SpPhoto.jpeg) |
| Share As WhatsAppAdd  | Map Taxi Hiring Plan | Trip Plan | Route Of Trip |
| ![](Images/Mobile/ShareAsAdd.jpeg) | ![](Images/Mobile/MapTaxiHiringPlan.jpeg) | ![](Images/Mobile/TripPlan.jpeg) | ![](Images/Mobile/Routre.jpeg) |
| Login | Signup | Help Tutorial | Help Tutorial |
| ![](Images/Mobile/Login.jpeg) | ![](Images/Mobile/Signup.jpeg) | ![](Images/Mobile/HelpTutorial.jpeg) | ![](Images/Mobile/HelpTutorial2.jpeg) |
| About | CompanyPolicy | Contact | Multi Language Support |
| ![](Images/Mobile/About.jpeg) | ![](Images/Mobile/CompanyPolicy.jpeg) | ![](Images/Mobile/Contact.jpeg) | ![](Images/Mobile/HomeUrdu.jpeg) |




## Website Screenshots

| Services Home Screen | Marriage Hall Search | Radial Search From Location | Chat With Service Provider |
|----------------|----------------|----------------|----------------|
| ![1](Images/Web/Home.png) | ![2](Images/Web/MarriageHallSearch.png) | ![3](Images/Web/RadialSearchFromLocation.png) | ![4](Images/Web/SendMessageOrChatWithServiceProvider.png) |


# System Architecture Overview

The **Findkia platform** is designed as a large-scale, enterprise-grade system capable of supporting millions of end users and hundreds of thousands of service providers. The architecture is built with high availability, scalability, and fault tolerance as core principles.

## High-Level Architecture Design

The system is deployed on a cloud-based infrastructure and follows a distributed, load-balanced architecture across multiple layers.

### 1. Load Balancing & High Availability Layer

* Two **Nginx-based Linux servers** are deployed as the primary load balancers.
* **Keepalived** is implemented to manage a **Virtual IP (VIP)** for failover and high availability.
* In case of failure of one load balancer, the second node automatically takes over without service disruption.

### 2. Web Application Layer

* A **cluster of multiple web servers** hosts the client-facing web application.
* All web servers operate in an **active-active configuration**.
* Web traffic is routed through the Virtual IP (VIP), ensuring seamless failover and load distribution.
* If any web server becomes unavailable, traffic is automatically routed to the remaining healthy servers.

### 3. API Service Layer

* A separate **cluster of Web API servers** is deployed to handle backend services.
* These APIs are consumed by:

  * Web application
  * Mobile applications (clients, service providers, administrators)
* The API layer also operates in an **active-active configuration** behind a Virtual IP (VIP).
* This ensures uninterrupted service even if one or more API servers fail.

### 4. Data Layer

* The database system is hosted on a **cloud-managed database server** to ensure scalability, reliability, and backup management.
* All application data is centralized and optimized for high-volume transactional operations.

### 5. Storage Layer

* Static content, media files, and user uploads are stored in **cloud-based S3-compatible object storage**.
* This ensures high durability, scalability, and global accessibility.

### 6. External Service Integrations

The platform integrates with multiple third-party services to enhance functionality:

* **Google Maps API**

  * Location search
  * Route planning
  * Real-time tracking of service providers

* **Google Image API**

  * Used to fetch and display landmark or service provider-related imagery for better user experience and identification

## Key Architectural Benefits

* Highly scalable to support millions of users
* Zero single point of failure (SPOF) in load balancers and application layers
* Active-active clustering for maximum availability
* Cloud-native storage and database integration
* Seamless integration with third-party mapping and imaging services
* Designed for real-time, high-volume service marketplace operations



# System Architecture Diagram

```mermaid
flowchart TB

%% ================= LOAD BALANCER LAYER =================
subgraph LB[Load Balancer Layer]
    LB1[Nginx Server 1]
    LB2[Nginx Server 2]
    VIP[Virtual IP (Keepalived)]
    LB1 <--> VIP
    LB2 <--> VIP
end

%% ================= WEB LAYER =================
subgraph WEB[Web Application Cluster (Active-Active)]
    W1[Web Server 1]
    W2[Web Server 2]
    W3[Web Server N]
end

%% ================= API LAYER =================
subgraph API[Web API Cluster (Active-Active)]
    A1[API Server 1]
    A2[API Server 2]
    A3[API Server N]
end

%% ================= CLIENTS =================
subgraph CLIENTS[Clients]
    WEBAPP[Web Users]
    MOBILE[Mobile Apps]
    ADMIN[Admin Panel]
end

%% ================= EXTERNAL SERVICES =================
subgraph EXT[External Services]
    MAPS[Google Maps API]
    IMG[Google Image API]
end

%% ================= DATA LAYER =================
DB[(Cloud Database Server)]
S3[(Cloud S3 Storage)]

%% ================= FLOW =================
WEBAPP --> VIP
MOBILE --> VIP
ADMIN --> VIP

VIP --> W1
VIP --> W2
VIP --> W3

W1 --> A1
W2 --> A2
W3 --> A3

A1 --> DB
A2 --> DB
A3 --> DB

A1 --> S3
A2 --> S3
A3 --> S3

A1 --> MAPS
A2 --> MAPS
A3 --> MAPS

A1 --> IMG
A2 --> IMG
A3 --> IMG
```




One codebase ships to **web**, **Android**, and **iOS** using **Ionic 6**, **Angular 12**, and **Capacitor 4**, backed by a REST API and JWT authentication.

| | |
|---|---|
| **Product** | [findkia.com](https://findkia.com) |
| **Stack** | Ionic 6 · Angular 12 · Capacitor 4 |
| **Node.js** | 18.x (see `.nvmrc`) |
| **Languages** | English · Urdu |

---

## Project summary

FindKia connects **customers**, **service providers**, and **administrators** in a single application:

| Persona | What they do in the app |
|---------|-------------------------|
| **Customer / guest** | Browse services by role and city, search with live location, view maps, add items to basket, checkout, manage bookings, favourites, and profile |
| **Service provider (SP)** | Manage business profile, menus, images/videos, subscription packages, orders, payments, online/offline status, and live location (drivers) |
| **Administrator** | Dashboard analytics, user & SP management, approvals, payment requests, image/video moderation, maps, addresses, and support chat |

The app is **multi-role**: the home screen and search flows adapt based on the logged-in role (e.g. hotels, cars, activities, wedding halls, transport). Permissions are enforced server-side and reflected in the UI via JWT claims and role maps.

### High-level architecture

```mermaid
flowchart TB
  subgraph clients [Clients]
    Web[Web PWA / Browser]
    Android[Android App]
    iOS[iOS App]
  end

  subgraph app [FindKia Frontend]
    Ionic[Ionic 6 UI]
    Angular[Angular 12 Modules]
    Cap[Capacitor / Cordova Plugins]
    Ionic --> Angular
    Angular --> Cap
  end

  subgraph services [External Services]
    API[REST API]
    Maps[Google Maps]
    FCM[Firebase Cloud Messaging]
    Ads[AdMob]
  end

  Web --> app
  Android --> app
  iOS --> app
  Angular --> API
  Cap --> Maps
  Cap --> FCM
  Cap --> Ads
```

### Request flow (simplified)

1. User authenticates → JWT stored via Ionic Storage  
2. `HttpInterceptor` attaches token to API calls (`environment.siteUrl`)  
3. Role-based guards route users to customer, SP, or admin areas  
4. Push notifications, maps, camera, and file plugins run through Capacitor/Cordova on native builds  

---

## Features

### Customer & booking

- **Home & discovery** — role-based service grid, promotional banners, recommended listings  
- **Search** — city/area filters, date ranges, live GPS search with radius, grid/list/map results  
- **Detail & basket** — service menus, scheduling, item modal, checkout  
- **Verticals** — hotels, rent-a-car, trip activities, wedding halls  
- **Account** — register, login, SMS verification, password reset, edit profile, messages  

### Service provider

- **Dashboard** — subscription status, online/offline toggle, account summary  
- **Business** — profile, menus, menu images, SP images, video upload  
- **Commerce** — packages, payment flow, payment status, orders  
- **Location** — live location sharing for eligible roles (e.g. drivers)  

### Administration

- **Dashboard** — KPI cards and charts  
- **Users & SPs** — CRUD, approvals, datatable filters, date-range search  
- **Moderation** — image requests, video requests, SP comments  
- **Operations** — payment requests, SP packages, Google map tools, map addresses, admin chat  

### Platform

- **Maps** — Google Maps (web AGM), native Google Maps, TPL map variants  
- **i18n** — `@ngx-translate` (`src/assets/i18n/en.json`, `ur.json`)  
- **Notifications** — Firebase Cloud Messaging  
- **Deep links** — `findkia://` / `https://findkia.com` (Cordova deeplinks plugin)  
- **UI theme** — dark glassmorphism design system ([docs/DESIGN_SYSTEM.md](docs/DESIGN_SYSTEM.md))  

---

## Tech stack

| Layer | Technology |
|-------|------------|
| UI framework | Ionic 6 + Angular 12 |
| Native runtime | Capacitor 4 (+ Cordova plugins for legacy features) |
| Language | TypeScript 4.3 |
| Styling | SCSS, Ionic CSS variables, custom glass theme |
| State / storage | Ionic Storage, RxJS 6 |
| Backend | REST API (`environment.siteUrl`) |
| Authentication | JWT (`@auth0/angular-jwt`) |
| Maps | `@agm/core`, `@capacitor/google-maps`, ngx-google-places-autocomplete |
| Charts / tables | `@swimlane/ngx-charts`, `@swimlane/ngx-datatable` |
| Push / analytics | Firebase 7, `@angular/fire` |
| i18n | `@ngx-translate/core` |
| Testing | Karma + Jasmine |
| Hosting (web) | Firebase Hosting (`npm run deploy`) |

---

## Prerequisites

- **Node.js 18.x** (required — see `.nvmrc`)  
- **npm 8+**  
- **Ionic CLI** (optional; invoked via npm scripts)  
- **Android Studio** / **Xcode** — native builds  
- **Java JDK 11+** — Android  

### Install Node.js 18

**Windows (nvm-windows):**
```bash
nvm install 18
nvm use 18
```

**macOS / Linux (nvm):**
```bash
nvm install
nvm use
```

Verify:
```bash
node -v   # v18.x.x
npm -v    # 8.x or higher
```

---

## Getting started

### 1. Clone the repository

```bash
git clone <repository-url>
cd Connected.App.New
```

### 2. Install dependencies

```bash
npm install
```

> `postinstall` runs `scripts/setup-env.js`, which creates `environment.ts` and `environment.prod.ts` from `environment.example.ts` when missing.

### 3. Configure environment

Copy and edit environment files:

```bash
cp src/environments/environment.example.ts src/environments/environment.ts
cp src/environments/environment.example.ts src/environments/environment.prod.ts
```

| Variable | Description |
|----------|-------------|
| `siteUrl` | Backend API base URL (must end with `/api/`) |
| `mapKey` | Google Maps JavaScript API key |
| `mapKeyStatic` | Google Maps Static API key |
| `firebase` | Firebase project credentials (FCM, hosting) |
| `production` | `true` in `environment.prod.ts` for release builds |
| `versionCode` / `versionNumber` | App version shown in About screen |

> `environment.ts` and `environment.prod.ts` are **gitignored** — never commit API keys or secrets.

### 4. Run in the browser

```bash
npm start
# or
npm run serve
```

Open [http://localhost:8100](http://localhost:8100).

### 5. Production build

```bash
npm run build
```

Output: `www/`

---

## NPM scripts

| Command | Description |
|---------|-------------|
| `npm start` | Dev server (`ng serve` with OpenSSL legacy provider on Node 17+) |
| `npm run serve` | Ionic dev server |
| `npm run lab` | Ionic Lab — multi-platform preview |
| `npm run build` | Production build |
| `npm run build:dev` | Development build |
| `npm test` | Unit tests (Karma) |
| `npm run lint` | TSLint |
| `npm run deploy` | Deploy `www/` to Firebase Hosting |
| `npm run android` | Run on Android (Cordova) |
| `npm run ios` | Run on iOS simulator (Cordova) |

---

## Native mobile builds

### Capacitor (recommended)

```bash
npm run build
npx cap sync
npx cap open android   # or ios (macOS only)
```

Build and run from Android Studio or Xcode.

### Cordova (legacy scripts)

```bash
npm run android
npm run ios
```

---

## Project structure

```
Connected.App.New/
├── src/
│   ├── app/
│   │   ├── Admin/              # Admin dashboard, users, SP approvals, moderation, maps
│   │   ├── pages/              # Feature modules (lazy-loaded routes)
│   │   │   ├── main/           # Home / service discovery
│   │   │   ├── sp-search/      # Search, filters, results (grid/list/map)
│   │   │   ├── sp-detail/      # Service provider detail
│   │   │   ├── basket/         # Cart & checkout
│   │   │   ├── dashboard-sp/   # SP dashboard
│   │   │   ├── login/          # Auth flows
│   │   │   └── …               # Hotels, cars, activities, settings, etc.
│   │   ├── services/           # API & business logic
│   │   ├── shared/             # Components, guards, interceptors, global provider
│   │   └── model/              # TypeScript interfaces / models
│   ├── assets/
│   │   └── i18n/               # en.json, ur.json
│   ├── environments/           # App config (gitignored)
│   └── theme/
│       ├── variables.scss      # Design tokens, Ionic colors, dark mode
│       ├── glassmorphism.scss  # Glass cards, buttons, layout
│       ├── dark-global.scss    # Global dark theme overrides
│       ├── admin-pages.scss    # Admin datatables, modals
│       └── sp-results.scss     # Search result grid & list
├── docs/
│   └── DESIGN_SYSTEM.md        # UI guidelines & component patterns
├── scripts/
│   ├── run.js                  # OpenSSL legacy provider wrapper
│   └── setup-env.js            # Environment file bootstrap
├── .nvmrc                      # Node 18
├── .npmrc                      # legacy-peer-deps
└── capacitor.config.json
```

### Key routes (examples)

| Path | Module |
|------|--------|
| `/`, `/main` | Home & service discovery |
| `/spsearch` | Service provider search |
| `/sp-detail/:id` | SP detail page |
| `/basket` | Shopping basket |
| `/dashboard-sp` | SP dashboard |
| `/dashboard` | Admin dashboard |
| `/users`, `/sps` | Admin user & SP management |
| `/howto` | Video tutorials |
| `/privacypolicy` | Privacy policy |

Full route table: `src/app/app-routing.module.ts`

---

## UI & design system

The app uses a **dark-first glassmorphism** theme (indigo primary, cyan accent, Inter typography). Shared patterns:

- `glass-header` — frosted toolbars  
- `app-shell` + `app-shell-bg` — page background mesh  
- `glass-card` — cards, panels, modals  
- `btn-search` / `btn-glass` — primary and secondary actions  

See **[docs/DESIGN_SYSTEM.md](docs/DESIGN_SYSTEM.md)** for tokens, spacing, and component rules.

---

## Node.js 18 compatibility

This repo targets **Node.js 18**. Notable adjustments:

- Removed legacy `@ionic/app-scripts` (avoided `node-sass` failures)  
- Pinned packages for Angular 12 compatibility (`@auth0/angular-jwt@5.0.2`, `sass@1.54.9`, etc.)  
- `scripts/run.js` sets `--openssl-legacy-provider` on Node 17+ (Webpack / Angular 12)  
- `.npmrc` uses `legacy-peer-deps` for older peer trees  
- Browser-safe helpers replace Node `util` imports where needed  

---

## Troubleshooting

### `ERR_OSSL_EVP_UNSUPPORTED` on Node 18+

Use npm scripts (`npm start`, `npm run build`). They run through `scripts/run.js`.

### Clean reinstall

```bash
rm -rf node_modules package-lock.json
npm install
```

### Missing environment files

```bash
node scripts/setup-env.js
```

### Port in use

```bash
npx ionic serve --port 8200
```

---

## Deployment

### Firebase Hosting (web)

```bash
npm run build
npm run deploy
```

Requires Firebase CLI login and project configuration.

### Android release keystore (one-time)

```bash
keytool -genkey -v -keystore my-release-key.keystore -alias alias_name -keyalg RSA -keysize 2048 -validity 10000
```

---

## Documentation

| Document | Description |
|----------|-------------|
| [README.md](README.md) | This file — overview, setup, structure |
| [docs/DESIGN_SYSTEM.md](docs/DESIGN_SYSTEM.md) | Colors, typography, glass UI patterns |

---

## License

Private project. Contact the repository owner for licensing terms.

---

## Version

App version is defined in `src/environments/environment.prod.ts` (`versionCode`, `versionNumber`).  
Package version: see `package.json`.
