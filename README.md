# StellarAjo-API


Supporting infrastructure for notifications, identity, and off-chain services.
The StellarSusu Backend provides optional off-chain services that enhance the user experience of the StellarSusu ecosystem. While all financial logic lives on-chain in the smart contract, this backend handles **real-time communication, user experience, and integrations**.

---

## 🌍 Purpose

The backend exists to support — not replace — the trustless system powered by the smart contract.

It is responsible for:

* 🔔 Delivering notifications (contributions, payouts, reminders)
* 👤 Managing user profiles and contacts
* 🔗 Syncing off-chain metadata with on-chain activity
* 📡 Enabling real-time updates for mobile clients

---

## 🧱 Architecture Overview

```
Mobile App (React Native)
        │
        ▼
Backend API (Supabase / Node.js)
        │
        ▼
Event Listener / Indexer
        │
        ▼
Stellar Network (Soroban Smart Contract)
```

---

## 🔑 Key Responsibilities

### 🔔 Notifications System

* Push notifications for:

  * Contribution reminders
  * Payout alerts
  * Group activity
* Scheduled jobs for recurring reminders

---

### 📊 Event Indexing

* Listen to on-chain contract events
* Parse and store:

  * Contributions
  * Disbursements
  * Group state updates
* Provide fast query access for the frontend

---

### 👤 User & Social Layer

* Store user profiles (username, avatar, preferences)
* Contact syncing (optional)
* Group metadata (names, descriptions)

---

### 🔐 Authentication

* Wallet-based authentication
* Optional session management
* Passkey / OAuth extensions (future-ready)

---

## ⚙️ Tech Stack

| Layer              | Technology                           |
| ------------------ | ------------------------------------ |
| Backend Platform   | Supabase (Postgres + Edge Functions) |
| Runtime (optional) | Node.js (Express / Fastify)          |
| Database           | PostgreSQL                           |
| Realtime           | Supabase Realtime                    |
| Queue / Jobs       | Cron / Background workers            |
| Blockchain SDK     | @stellar/stellar-sdk                 |
| Event Processing   | Custom indexer service               |

---

## 🚀 Getting Started

### Prerequisites

* Node.js (v18+)
* Supabase account
* Stellar testnet access

---

## 📦 Installation

```bash
git clone https://github.com/stellarsusu/stellarsusu-backend.git
cd stellarsusu-backend

npm install
```

---

## 🔧 Environment Variables

Create a `.env` file:

```env
# Supabase
SUPABASE_URL=
SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=

# Stellar
STELLAR_RPC_URL=
STELLAR_NETWORK_PASSPHRASE=

# Notifications
EXPO_PUSH_KEY=

# App
APP_ENV=development
```

---

## ▶️ Run the Backend

```bash
npm run dev
```

---

## 🧩 Core Services

### 1. Event Listener / Indexer

Listens to the smart contract and processes events:

* `create_group`
* `join_group`
* `contribute`
* `disburse`

**Responsibilities:**

* Decode contract events
* Store structured data in database
* Trigger downstream actions (notifications, updates)

---

### 2. Notifications Service

* Sends push notifications via Expo
* Supports:

  * Scheduled reminders
  * Instant alerts from contract events

---

### 3. API Layer

Provides endpoints for:

* User profiles
* Group metadata
* Transaction history (indexed)
* Notifications

Example endpoints:

```
GET   /groups
GET   /groups/:id
GET   /users/:id
GET   /transactions
POST  /notifications/register
```

---

### 4. Realtime Updates

* WebSocket subscriptions via Supabase
* Live updates for:

  * Contributions
  * Payouts
  * Group changes

---

## 🗄️ Database Schema (Simplified)

```
users
- id
- wallet_address
- username
- avatar_url

groups
- id
- contract_id
- name
- contribution_amount
- cycle_length

members
- id
- group_id
- user_id
- payout_order

transactions
- id
- group_id
- type (contribution | disbursement)
- amount
- tx_hash

notifications
- id
- user_id
- type
- message
- read
```

---

## 🔗 Blockchain Integration

The backend **does not control funds**.

Instead, it:

* Reads contract state
* Listens to emitted events
* Enhances UX with indexed data

All financial actions are executed directly by the client via the SDK.

---

## 🧪 Testing

* Unit tests for services
* Integration tests for event processing
* Manual testing with testnet transactions

---

## 📈 Scaling Considerations

* Horizontal scaling for event indexer
* Queue-based processing for high throughput
* Caching frequently accessed data
* Rate limiting for API endpoints

---

## 🔒 Security Considerations

* No private keys stored
* Validate all incoming requests
* Rate-limit APIs
* Secure environment variables
* Use service role keys only on trusted servers

---

## 🗺️ Roadmap

* 🔄 Improved event indexing reliability
* 🔔 Advanced notification preferences
* 🌍 Multi-region deployment
* 📊 Analytics & reporting
* 🔮 Fraud detection signals (off-chain insights only)

---

## 🤝 Contributing

We welcome contributors:

1. Fork the repo
2. Create a feature branch
3. Submit a pull request

