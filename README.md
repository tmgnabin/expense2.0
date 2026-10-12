# Expense Tracker 2.0 — Final Year Project

A production-grade, monolithic full-stack personal finance web application built from scratch for academic final-year project evaluation. Engineered with code quality, security, clear separation of concerns, and documentation at its core.

---

## 🌟 Key Features

1. **Secure Authentication & Data Isolation**
   - User registration and login using **JWT stored in `httpOnly`, `SameSite=Lax` cookies**.
   - Passwords hashed with **bcrypt (12 salt rounds)**.
   - Strict multi-tenant data isolation: users can never read, modify, or delete another user's financial records.
   - Rate limiting on authentication routes to prevent brute-force attacks.

2. **Full-Featured Expense Management**
   - Create, read, update, and delete expenses with monetary amounts, category, date, payment method (`cash`, `card`, `upi`, `bank_transfer`, `other`), and note memos.
   - Financial precision: safe 2-decimal monetary handling (`roundMoney`) with SQLite `REAL` check constraints.
   - Interactive data table with sortable columns (by Date or Amount), pagination, and instant CRUD modals.

3. **Multi-Criteria Filtering & Text Search**
   - Real-time debounced text search across notes and category names.
   - Filter by date range (`startDate`, `endDate`), category, payment method, and amount range.
   - Reset filters with a single click.

4. **Category Management**
   - 9 default system categories (Food, Transport, Rent, Shopping, Bills, Health, Entertainment, Education, Travel) each with distinct colors and icons.
   - User-defined custom categories with custom hex colors.
   - Protection against deleting or altering default categories, and safeguards against deleting categories with active linked expenses.

5. **Analytics Dashboard with Visualizations**
   - KPI metric cards: This Month's Outlay, This Year's Total, Top Spending Category, and Budget Alerts.
   - **Offline Chart.js** visualizations:
     - Interactive **Category Doughnut Chart** showing percentage breakdown.
     - **6-Month Spending Trend Bar Chart** showing historical monthly comparisons.
     - Seamless dynamic light/dark mode adaptation for charts.
   - Top 5 recent transactions quick-view.

6. **Smart Budgets & Threshold Warnings**
   - Set monthly spending limits per category.
   - Visual progress bars with dynamic status indicators:
     - 🟢 **Normal / Within Budget** (< 80%)
     - 🟡 **Warning Near Limit** (80% – 99%)
     - 🔴 **Budget Exceeded Alert** (≥ 100%)
   - Monthly calendar navigation (previous/next month reviews).

7. **Export & Printable Reports**
   - One-click **CSV Export** with properly escaped comma and quote fields.
   - **Printable Monthly Statement of Expenses** with clean `@media print` layout suitable for printing or saving as PDF.

8. **Profile & Account Management**
   - Update full name.
   - Change password with current password verification.
   - Danger zone: Permanent account deletion with password re-verification, cleanly cascading all related records.

9. **Design Aesthetics & Theming**
   - Single shared CSS design token system (`variables.css`).
   - Seamless **Light & Dark mode** switching with persistent localStorage sync.
   - Responsive layout designed for mobile, tablet, and desktop screens.
   - No external CSS frameworks (clean Vanilla CSS and ES modules).

---

## 🛠️ Technology Stack

- **Backend**:
  - Node.js (LTS v24+)
  - **Express v5** (`^5.0.0`)
  - **SQLite** via `better-sqlite3` (synchronous, transactions, WAL mode, foreign keys enabled)
  - `jsonwebtoken` & `cookie-parser` for secure auth
  - `bcrypt` for password hashing
  - `zod` for declarative request validation (stores coerced data in `req.validated`)
  - `helmet`, `cors`, `express-rate-limit`, `dotenv`, `morgan`
- **Frontend**:
  - Vanilla HTML5 & Modern CSS3 (Custom Properties / Design Tokens, Glassmorphism, Micro-animations)
  - Vanilla JavaScript (ES Modules)
  - **Chart.js** (bundled locally in `/public/js/vendor` for 100% offline functionality)
- **Testing**:
  - **Jest** + **Supertest** (runs against isolated in-memory SQLite instances)
- **Tooling**:
  - **ESLint** (v9 flat config) + **Prettier**
  - **nodemon** for live reload development

---

## 🚀 Quick Start Guide

### 1. Prerequisites
- **Node.js**: v20+ or v24+ LTS installed
- **Git** (optional for version control)

### 2. Installation
```powershell
# Clone or navigate to the workspace
cd e:\expense2.0

# Install dependencies
npm install
```

### 3. Environment Configuration
The `.env` file is already pre-configured for development. To customize, copy from `.env.example`:
```powershell
cp .env.example .env
```
Key variables:
```ini
PORT=3000
NODE_ENV=development
DB_PATH=./data/expense_tracker.db
JWT_SECRET=super_secret_jwt_key_at_least_32_characters_long_for_security
JWT_EXPIRES_IN=1d
CORS_ORIGIN=http://localhost:3000
RATE_LIMIT_WINDOW_MS=900000
RATE_LIMIT_MAX=200
AUTH_RATE_LIMIT_MAX=15
```

### 4. Database Setup & Seeding
```powershell
# Run versioned SQL migrations
npm run migrate

# Seed with realistic dummy data
npm run seed
```

To wipe and reseed cleanly at any time:
```powershell
npm run seed:reset
```

### 5. Start the Server
```powershell
# For development with nodemon:
npm run dev

# Or for production:
npm start
```
Open **[http://localhost:3000](http://localhost:3000)** in your browser.

---

## 🔑 Demo Account Credentials

Use these credentials to test the application immediately:

| Role | Email | Password | Details |
|---|---|---|---|
| **Demo User** | `demo@example.com` | `Demo@1234` | ~250 realistic expenses over 6 months, calibrated budgets (<80%, ~85%, >100%) |
| **Secondary User** | `alex@example.com` | `Alex@1234` | Distinct isolated records to verify multi-tenant privacy |
| **Tertiary User** | `sarah@example.com` | `Sarah@1234` | Distinct isolated records |

> **Pro Tip**: On the login page ([http://localhost:3000/pages/login.html](http://localhost:3000/pages/login.html)), click the **Auto-Fill** button to instantly populate the demo credentials.

---

## 🧪 Testing & Code Quality

Run the automated test suite with Supertest:
```powershell
npm test
```
*Current test suite results*: **57 passed across 8 test suites** (`health`, `database`, `auth`, `categories`, `expenses`, `budgets`, `reports`, `user`).

Run code formatting and linting:
```powershell
npm run lint
npm run format
```

---

## 📁 Project Directory Structure

```text
expense2.0/
├── .env                  # Local environment secrets
├── .env.example          # Committed environment blueprint
├── .gitignore            # Git ignore rules
├── .prettierrc           # Prettier configuration
├── .prettierignore       # Prettier vendor ignore
├── eslint.config.js      # ESLint flat configuration
├── package.json          # Dependencies and scripts
├── README.md             # Project documentation
├── docs/
│   ├── api.md            # Comprehensive API endpoint reference
│   └── architecture.md   # Mermaid system & ER architecture diagrams
├── tests/
│   ├── setup.js          # Jest test environment setup
│   ├── health.test.js    # Health route tests
│   ├── database.test.js  # SQLite constraint & migration tests
│   ├── auth.test.js      # Authentication & cookie tests
│   ├── categories.test.js# Categories CRUD & permissions tests
│   ├── expenses.test.js  # Expenses CRUD, filters & isolation tests
│   ├── budgets.test.js   # Budgets CRUD & threshold alert tests
│   ├── reports.test.js   # CSV export & monthly statements tests
│   └── user.test.js      # Profile update & deletion tests
├── src/
│   ├── app.js            # Express application setup
│   ├── server.js         # Server bootstrap & graceful shutdown
│   ├── config/
│   │   ├── env.js        # Environment validation
│   │   └── database.js   # SQLite connection (WAL, foreign keys)
│   ├── db/
│   │   ├── migrate.js    # Migration runner
│   │   ├── migrations/   # Versioned SQL migrations (001 - 004)
│   │   └── seeds/        # Realistic dummy data generator
│   ├── middleware/       # Auth, error, rate-limit, and validation
│   ├── repositories/     # Data access layer (parameterized SQL)
│   ├── services/         # Core business logic
│   ├── controllers/      # Request/response handlers
│   ├── routes/           # Layered routing endpoints
│   └── utils/            # Money, AppError, response, and validators
└── public/
    ├── index.html        # Landing showcase page
    ├── pages/            # Login, register, dashboard, expenses, budgets, reports, profile
    ├── css/              # Shared tokens, base, layout, components
    ├── js/               # API client, auth guard, theme manager, utils, page scripts
    └── js/vendor/        # Offline Chart.js bundle
```

---

## 📋 Evaluation Checklist for Examiners

- [x] **Layered Architecture**: Strict `routes` -> `controllers` -> `services` -> `repositories` structure.
- [x] **Parameterized Queries**: 100% parameterization with `better-sqlite3` to prevent SQL injection.
- [x] **Input Validation**: Declarative Zod schemas verifying types, positive amounts, valid dates, and regex formats.
- [x] **Secure Auth**: JWT stored in `httpOnly`, `SameSite=Lax` cookies; bcrypt 12 rounds.
- [x] **Multi-Tenant Privacy**: Every expense, budget, and custom category is strictly scoped to `user_id`.
- [x] **Express 5 Compliant**: Safely handles read-only `req.query` without mutation errors.
- [x] **Offline Resilience**: Chart.js served locally from `public/js/vendor/chart.umd.js`.
- [x] **Safe Financials**: Monetary amounts rounded strictly to 2 decimal places.
- [x] **Test Coverage**: 57 automated tests covering database, auth, categories, expenses, budgets, reports, and profile.
