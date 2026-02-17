# PharmaTrust: A Blockchain-IoT Solution for Pharmaceutical Supply Chain Integrity

> Ensuring medication authenticity and safety through decentralized verification and real-time cold-chain monitoring on the Stellar network.

---

## Executive Summary

PharmaTrust is a trustless pharmaceutical supply chain platform that leverages Stellar blockchain technology, Soroban smart contracts, and IoT sensors to create an immutable, transparent record of medication provenance from manufacturer to patient. By utilizing Stellar's fast, low-cost transactions and eliminating intermediary trust requirements through real-time environmental monitoring, PharmaTrust addresses critical challenges in pharmaceutical distribution: counterfeiting, opacity in multi-party logistics, and temperature-sensitive cargo failures.

---

## The Problem Statement

The global pharmaceutical supply chain faces three critical vulnerabilities:

### 1. Counterfeiting & Fraud
- According to the [World Health Organization](https://www.who.int/news-room/fact-sheets/detail/substandard-and-falsified-medical-products), at least 1 in 10 medical products in low and middle-income countries are substandard or falsified
- The global counterfeit drug market is valued between $200-432 billion annually
- WHO estimates approximately 1 million deaths annually are attributable to counterfeit medications
- In some developing regions, 30-60% of drugs in the market may be counterfeit
- Traditional paper-based verification systems are easily manipulated and provide no real-time validation

### 2. Lack of Transparency
- Multi-tier distribution networks (manufacturer → wholesaler → distributor → pharmacy) create information silos
- No single source of truth for medication provenance across supply chain participants
- Countries spend an estimated $30.5 billion per year on substandard and falsified medical products
- Recalls are slow and inefficient due to poor traceability and fragmented record-keeping
- Patients and healthcare providers have limited ability to verify medication authenticity

### 3. Cold-Chain Failures
- Temperature-sensitive medications (vaccines, biologics, insulin) require strict storage conditions (typically 2-8°C)
- The biopharmaceutical industry loses approximately [$35 billion annually](https://www.supplychaindive.com/news/coronavirus-vaccine-cold-chain-tracking-iot-sensor-technology/583168/) due to temperature control failures in supply chains
- Up to 50% of vaccines globally may be discarded due to cold chain issues
- Studies show that over 75% of vaccine shipments are exposed to freezing temperatures at some point in transit
- Around one-third of cold chain pharmaceuticals are put at risk by temperature excursions annually
- Manual temperature logging is unreliable, non-verifiable, and often incomplete
- Patients and providers have no visibility into storage conditions during transit

These failures result in billions of dollars in waste, compromised patient safety, preventable deaths, and eroded trust in pharmaceutical systems worldwide.

---

## Proposed Solution

PharmaTrust creates an end-to-end trustless verification system by integrating three core technologies:

### Stellar Blockchain Ledger
- **Immutable Record**: Every batch of medication is registered as a unique asset with embedded metadata (manufacturer, expiration, composition)
- **Transparent Provenance**: All ownership transfers are recorded on-chain, creating an auditable chain of custody
- **Fast & Low-Cost**: Stellar's 3-5 second finality and minimal transaction fees (~$0.00001) enable real-time tracking
- **Decentralized Trust**: No single entity controls the data; Stellar Consensus Protocol ensures integrity

### Soroban Smart Contracts
- **Automated Verification**: Soroban smart contracts enforce business logic (e.g., only licensed distributors can receive transfers)
- **Conditional Transfers**: Ownership changes only execute if temperature logs are within acceptable ranges
- **Spoilage Alerts**: Contracts automatically flag batches that have experienced temperature excursions
- **Rust-Based Security**: Soroban contracts written in Rust provide memory safety and performance

### IoT Sensors & IPFS
- **Real-Time Monitoring**: IoT devices continuously log temperature, humidity, and location data
- **Decentralized Storage**: Sensor data is stored on IPFS, with content hashes anchored on-chain
- **Tamper-Proof Logs**: Cryptographic hashing ensures sensor data cannot be altered retroactively

### Patient Verification
- Patients scan a QR code on medication packaging to verify authenticity and view complete supply chain history
- Empowers end-users to make informed decisions about medication safety

---

## System Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                           PHARMATRUST ECOSYSTEM                                  │
└─────────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────────────────┐
│  TIER 1: HARDWARE & IoT LAYER                                                    │
├──────────────────────────────────────────────────────────────────────────────────┤
│                                                                                   │
│  ┌─────────────┐      ┌─────────────┐      ┌─────────────┐                     │
│  │  ESP32/RPi  │      │  ESP32/RPi  │      │  ESP32/RPi  │                     │
│  │  + DHT22    │      │  + DHT22    │      │  + DHT22    │                     │
│  │  + GPS      │      │  + GPS      │      │  + GPS      │                     │
│  └──────┬──────┘      └──────┬──────┘      └──────┬──────┘                     │
│         │                    │                    │                              │
│         │ MQTT Protocol      │                    │                              │
│         └────────────────────┴────────────────────┘                              │
│                              │                                                    │
│                    ┌─────────▼─────────┐                                         │
│                    │   MQTT Broker     │                                         │
│                    │  (Mosquitto/AWS)  │                                         │
│                    └─────────┬─────────┘                                         │
│                              │                                                    │
│                    ┌─────────▼─────────┐                                         │
│                    │  IoT Gateway      │                                         │
│                    │  - Aggregation    │                                         │
│                    │  - Hashing        │                                         │
│                    │  - Validation     │                                         │
│                    └─────────┬─────────┘                                         │
│                              │                                                    │
└──────────────────────────────┼───────────────────────────────────────────────────┘
                               │
                               │ Temperature Data + Hash
                               │
┌──────────────────────────────▼───────────────────────────────────────────────────┐
│  TIER 2: BLOCKCHAIN & STORAGE LAYER                                              │
├──────────────────────────────────────────────────────────────────────────────────┤
│                                                                                   │
│  ┌────────────────────────────────────────────────────────────────────┐         │
│  │                    STELLAR BLOCKCHAIN                               │         │
│  │  ┌──────────────────────────────────────────────────────────────┐  │         │
│  │  │           SOROBAN SMART CONTRACTS (Rust)                     │  │         │
│  │  │                                                               │  │         │
│  │  │  ┌─────────────────┐  ┌──────────────────┐                  │  │         │
│  │  │  │ Batch Registry  │  │ Transfer Logic   │                  │  │         │
│  │  │  │ - register()    │  │ - transfer()     │                  │  │         │
│  │  │  │ - verify()      │  │ - authorize()    │                  │  │         │
│  │  │  └─────────────────┘  └──────────────────┘                  │  │         │
│  │  │                                                               │  │         │
│  │  │  ┌─────────────────┐  ┌──────────────────┐                  │  │         │
│  │  │  │ Temperature Log │  │ Compliance Check │                  │  │         │
│  │  │  │ - log_temp()    │  │ - flag_batch()   │                  │  │         │
│  │  │  │ - check_range() │  │ - verify_license()│                 │  │         │
│  │  │  └─────────────────┘  └──────────────────┘                  │  │         │
│  │  │                                                               │  │         │
│  │  └───────────────────────────┬───────────────────────────────────┘  │         │
│  │                              │                                       │         │
│  │  ┌───────────────────────────▼───────────────────────────────────┐  │         │
│  │  │              STELLAR CUSTOM ASSETS                            │  │         │
│  │  │  - Each batch = unique asset with metadata                   │  │         │
│  │  │  - AUTH_REQUIRED & AUTH_REVOCABLE flags                      │  │         │
│  │  │  - Trustline-based ownership transfer                        │  │         │
│  │  └───────────────────────────────────────────────────────────────┘  │         │
│  └────────────────────────────────────────────────────────────────────┘         │
│                                   │                                               │
│                                   │ IPFS Hash References                          │
│                                   │                                               │
│  ┌────────────────────────────────▼──────────────────────────────────┐           │
│  │                         IPFS STORAGE                               │           │
│  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────────────┐    │           │
│  │  │ Certificates │  │ Temperature  │  │ Product Images       │    │           │
│  │  │ of Analysis  │  │ Logs (JSON)  │  │ & Documentation      │    │           │
│  │  └──────────────┘  └──────────────┘  └──────────────────────┘    │           │
│  └────────────────────────────────────────────────────────────────────┘           │
│                                                                                   │
└──────────────────────────────┬───────────────────────────────────────────────────┘
                               │
                               │ Horizon API / Soroban RPC
                               │
┌──────────────────────────────▼───────────────────────────────────────────────────┐
│  TIER 3: APPLICATION LAYER                                                       │
├──────────────────────────────────────────────────────────────────────────────────┤
│                                                                                   │
│  ┌────────────────────────────────────────────────────────────────────┐         │
│  │                  NEXT.JS DAPP (Frontend + API)                      │         │
│  │                                                                     │         │
│  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐            │         │
│  │  │Manufacturer  │  │ Distributor  │  │  Pharmacy    │            │         │
│  │  │ Dashboard    │  │  Dashboard   │  │  Dashboard   │            │         │
│  │  │ (SSR)        │  │  (SSR)       │  │  (SSR)       │            │         │
│  │  │              │  │              │  │              │            │         │
│  │  │- Register    │  │- Request     │  │- Receive     │            │         │
│  │  │  Batch       │  │  Transfer    │  │  Batch       │            │         │
│  │  │- Issue Asset │  │- View        │  │- Verify      │            │         │
│  │  │- Upload Docs │  │  Inventory   │  │  Authenticity│            │         │
│  │  └──────────────┘  └──────────────┘  └──────────────┘            │         │
│  │                                                                   │         │
│  │  ┌──────────────┐  ┌──────────────┐                               │         │
│  │  │   Patient    │  │  Regulator   │                               │         │
│  │  │   Portal     │  │  Dashboard   │                               │         │
│  │  │ (SSG/ISR)    │  │  (SSR)       │                               │         │
│  │  │              │  │              │                               │         │
│  │  │- Scan QR     │  │- Compliance  │                               │         │
│  │  │- View History│  │  Analytics   │                               │         │
│  │  │- Report Issue│  │- Flag Batch  │                               │         │
│  │  └──────────────┘  └──────────────┘                               │         │
│  │                                                                   │         │
│  │  ┌──────────────────────────────────────────────────────────────┐ │         │
│  │  │         Freighter/Albedo Wallet Integration                  │ │         │
│  │  │         stellar-sdk.js | Soroban RPC Client                  │ │         │
│  │  │         Next.js API Routes for caching & optimization        │ │         │
│  │  └──────────────────────────────────────────────────────────────┘ │         │
│  └────────────────────────────────────────────────────────────────────┘         │
│                                                                                 │
│  ┌────────────────────────────────────────────────────────────────────┐         │
│  │              BACKEND API (Optional - Node.js)                      │         │
│  │  - Indexed blockchain data (PostgreSQL)                            │         │
│  │  - Analytics & reporting                                           │         │
│  │  - Push notifications                                              │         │
│  │  - Horizon API caching                                             │         │
│  └────────────────────────────────────────────────────────────────────┘         │
│                                                                                   │
└───────────────────────────────────────────────────────────────────────────────────┘
```

## Data Flow: Batch Lifecycle

```
STEP 1: BATCH REGISTRATION
┌─────────────┐
│Manufacturer │
└──────┬──────┘
       │ 1. Create batch metadata
       │    (NDC, lot#, expiry, qty)
       │
       │ 2. Upload Certificate of Analysis to IPFS
       │    → Returns IPFS hash (QmXxx...)
       │
       ▼
┌─────────────────────┐
│ Soroban Contract    │
│ register_batch()    │
└──────┬──────────────┘
       │ 3. Issue Stellar custom asset
       │    Asset Code: BATCH_12345
       │    Metadata: {ndc, expiry, ipfs_hash}
       │    Flags: AUTH_REQUIRED, AUTH_REVOCABLE
       │
       ▼
┌─────────────────────┐
│ Stellar Ledger      │
│ Asset Created ✓     │
└─────────────────────┘


STEP 2: COLD-CHAIN MONITORING
┌─────────────┐
│ IoT Sensor  │ (attached to shipment)
└──────┬──────┘
       │ Every 5 minutes:
       │ - Read temperature: 4.2°C
       │ - Read GPS: 40.7128°N, 74.0060°W
       │ - Timestamp: 1645123456
       │
       ▼
┌─────────────────────┐
│ IoT Gateway         │
└──────┬──────────────┘
       │ 1. Aggregate readings
       │ 2. Create JSON log
       │ 3. Upload to IPFS → QmYyy...
       │ 4. Generate hash of data
       │
       ▼
┌─────────────────────┐
│ Soroban Contract    │
│ log_temperature()   │
└──────┬──────────────┘
       │ 5. Store IPFS hash on-chain
       │ 6. Check: 4.2°C within range (2-8°C)? ✓
       │ 7. Update batch status: COMPLIANT
       │
       ▼
┌─────────────────────┐
│ Stellar Ledger      │
│ Temp Log Recorded ✓ │
└─────────────────────┘


STEP 3: OWNERSHIP TRANSFER
┌─────────────┐              ┌─────────────┐
│Manufacturer │              │ Distributor │
└──────┬──────┘              └──────┬──────┘
       │                            │
       │ 1. Initiate transfer       │
       │    to Distributor          │
       │                            │
       ▼                            │
┌─────────────────────┐             │
│ Soroban Contract    │             │
│ transfer_ownership()│             │
└──────┬──────────────┘             │
       │ 2. Verify:                 │
       │    - Distributor has DEA#  │
       │    - Temp logs compliant   │
       │    - Batch not expired     │
       │                            │
       │ 3. Require trustline       │
       │    from Distributor        │
       │                            ▼
       │                   ┌─────────────────────┐
       │                   │ Distributor accepts │
       │                   │ (establishes        │
       │                   │  trustline)         │
       │                   └──────┬──────────────┘
       │                          │
       ▼                          ▼
┌──────────────────────────────────────┐
│ Stellar Ledger                       │
│ Asset transferred ✓                  │
│ Chain of custody updated             │
│ Previous owner: Manufacturer         │
│ New owner: Distributor               │
└──────────────────────────────────────┘


STEP 4: PATIENT VERIFICATION
┌─────────────┐
│   Patient   │
└──────┬──────┘
       │ 1. Scan QR code on medication package
       │    QR contains: batch_id=BATCH_12345
       │
       ▼
┌─────────────────────┐
│ Next.js DApp        │
│ (Patient Portal)    │
└──────┬──────────────┘
       │ 2. Query Stellar ledger
       │    verify_authenticity(BATCH_12345)
       │
       ▼
┌─────────────────────┐
│ Soroban Contract    │
│ Returns:            │
│ - Authentic: ✓      │
│ - Manufacturer: XYZ │
│ - Current Owner:    │
│   Local Pharmacy    │
│ - Temp Compliant: ✓ │
│ - IPFS History: Qm..│
└──────┬──────────────┘
       │
       ▼
┌─────────────────────┐
│ Patient sees:       │
│ ✓ VERIFIED          │
│ ✓ Safe to use       │
│ 📊 Full history     │
│ 🌡️ Temp: Always OK  │
└─────────────────────┘


ALERT SCENARIO: TEMPERATURE EXCURSION
┌─────────────┐
│ IoT Sensor  │
└──────┬──────┘
       │ Reading: 12.5°C (ABOVE 8°C THRESHOLD!)
       │
       ▼
┌─────────────────────┐
│ Soroban Contract    │
│ log_temperature()   │
└──────┬──────────────┘
       │ 1. Detect violation
       │ 2. Flag batch: COMPROMISED
       │ 3. Revoke AUTH flag
       │ 4. Emit alert event
       │
       ▼
┌──────────────────────────────────────┐
│ Notifications sent to:               │
│ - Current owner (Distributor)        │
│ - Downstream parties (Pharmacy)      │
│ - Regulatory dashboard               │
│                                      │
│ ⚠️ BATCH_12345 COMPROMISED           │
│ ⚠️ TRANSFER BLOCKED                  │
└──────────────────────────────────────┘
```

---

## Technical Architecture

PharmaTrust employs a 3-tier architecture:

### Tier 1: Hardware & IoT Layer
```
IoT Sensors (ESP32/Raspberry Pi)
    ↓
Temperature/Humidity/GPS Data
    ↓
MQTT Broker / Edge Gateway
    ↓
Data Aggregation & Hashing
```

- **Devices**: Low-cost ESP32 or Raspberry Pi modules with DHT22 temperature sensors and GPS
- **Communication**: MQTT protocol for real-time data streaming
- **Edge Processing**: Local gateways aggregate sensor readings and generate cryptographic hashes

### Tier 2: Blockchain & Smart Contract Layer
```
Soroban Smart Contracts (Rust)
    ↓
Stellar Network
    ↓
IPFS (Document & Sensor Data Storage)
```

- **Smart Contracts**: Soroban contracts handle core business logic for batch registration, transfers, and compliance checks
- **Network**: Stellar mainnet with 3-5 second finality and sub-cent transaction costs
- **Assets**: Custom Stellar assets represent medication batches with built-in authorization controls
- **IPFS**: Decentralized storage for certificates of analysis, temperature logs, and product images

### Tier 3: Frontend DApp Layer
```
Next.js Web Application (App Router)
    ↓
Stellar SDK (stellar-sdk.js)
    ↓
Freighter/Albedo Wallet Integration
    ↓
User Interfaces (Manufacturer, Distributor, Pharmacy, Patient)
```

- **Framework**: Next.js 14+ with App Router and TypeScript for type safety and SSR/SSG capabilities
- **Stellar Integration**: stellar-sdk for blockchain interactions and Soroban contract invocation
- **Wallet**: Freighter or Albedo for transaction signing and identity management
- **Role-Based UI**: Different dashboards for each supply chain participant with server-side rendering
- **API Routes**: Built-in API endpoints for backend integration and data caching

---

## Core Features

### 1. Batch Registration & Asset Issuance
- Manufacturers issue custom Stellar assets representing medication batches with metadata: NDC code, lot number, expiration date, quantity
- Each batch is a unique asset with authorization flags (AUTH_REQUIRED, AUTH_REVOCABLE) for supply chain control
- Certificates of analysis and product images stored on IPFS, with CID recorded in asset metadata

### 2. Ownership Transfer with Verification
- Trustline-based transfers requiring receiver to establish trustline with batch asset
- Soroban contracts enforce automated compliance checks (e.g., receiver must have valid DEA license)
- Transfer history creates immutable chain of custody on Stellar ledger
- Low transaction costs (~$0.00001) enable granular tracking without economic barriers

### 3. Automated Spoilage Alerts
- Soroban contracts monitor temperature data hashes submitted by IoT devices
- If temperature exceeds thresholds (e.g., >8°C for vaccines), batch is flagged as "compromised"
- Asset authorization can be revoked for compromised batches, preventing further transfers
- Automatic notifications sent to all downstream parties via Stellar transaction memos
- Compromised batches cannot be transferred without explicit override and documentation

### 4. Patient Verification via QR Code
- Each medication package has a unique QR code linked to its on-chain Stellar asset
- Patients scan to view: manufacturer, distribution path, temperature history, authenticity status
- Public Stellar ledger enables verification without requiring wallet or account
- Empowers patients to report suspected counterfeits directly through the DApp

### 5. Regulatory Compliance Dashboard
- Real-time analytics for regulators (FDA, state boards of pharmacy)
- Track recall efficiency, identify distribution bottlenecks
- Audit trail for DSCSA compliance verification

---

## Smart Contract Logic (Soroban)

### Core Functions

#### `register_batch()`
```rust
pub fn register_batch(
    env: Env,
    manufacturer: Address,
    batch_id: String,
    ndc_code: String,
    quantity: u32,
    expiration_date: u64,
    ipfs_certificate_hash: String
) -> Result<(), Error>
```
- Issues a new Stellar asset representing a medication batch
- Stores metadata in contract storage and links to IPFS documents
- Sets AUTH_REQUIRED and AUTH_REVOCABLE flags for supply chain control
- Emits `BatchRegistered` event

#### `transfer_ownership()`
```rust
pub fn transfer_ownership(
    env: Env,
    batch_id: String,
    current_owner: Address,
    new_owner: Address,
    ipfs_transfer_doc_hash: String
) -> Result<(), Error>
```
- Transfers batch asset to licensed entity
- Validates receiver credentials (e.g., DEA license status from contract storage)
- Records transfer timestamp and documentation in contract state
- Requires both parties to sign transaction

#### `log_temperature()`
```rust
pub fn log_temperature(
    env: Env,
    batch_id: String,
    temperature: i32,
    timestamp: u64,
    ipfs_data_hash: String,
    sensor_id: Address
) -> Result<(), Error>
```
- Records temperature reading hash in contract storage
- Checks against acceptable range (configurable per medication type)
- Flags batch and revokes authorization if threshold violated
- Only authorized sensor addresses can invoke

#### `verify_authenticity()`
```rust
pub fn verify_authenticity(
    env: Env,
    batch_id: String
) -> Result<BatchStatus, Error>
```
- Public function for patient/pharmacy verification
- Returns: authenticity status, current owner, temperature compliance, IPFS history hash
- No authentication required - enables public verification

#### `flag_counterfeit()`
```rust
pub fn flag_counterfeit(
    env: Env,
    batch_id: String,
    regulator: Address,
    evidence_hash: String
) -> Result<(), Error>
```
- Allows regulatory authorities to mark batches as counterfeit
- Revokes asset authorization, preventing further transfers
- Stores evidence hash for audit trail
- Triggers investigation workflow

---

## Tech Stack

### Blockchain & Smart Contracts
- **Rust** - Soroban smart contract language
- **Soroban SDK** - Smart contract development framework
- **Stellar CLI** - Command-line tools for deployment and testing
- **Stellar Network** (Testnet → Mainnet) - Fast, low-cost blockchain
- **Stellar Assets** - Custom assets for batch representation with built-in authorization
- **Soroban RPC** - Interface for contract invocation

### Decentralized Storage
- **IPFS** - Document and sensor data storage
- **Pinata** / **Web3.Storage** - IPFS pinning services

### IoT & Sensors
- **ESP32** / **Raspberry Pi** - Microcontrollers
- **DHT22** - Temperature/humidity sensors
- **MQTT** - Lightweight messaging protocol
- **Node-RED** (optional) - IoT flow programming

### Frontend DApp
- **Next.js** 14+ with **App Router** and **TypeScript**
- **stellar-sdk** (js-stellar-sdk) - Stellar JavaScript library
- **@stellar/freighter-api** - Freighter wallet integration
- **Albedo** - Alternative wallet option
- **TailwindCSS** - Styling framework
- **React Query** / **SWR** - Data fetching and caching
- **Recharts** - Data visualization for temperature graphs
- **Next.js API Routes** - Backend API integration

### Backend Services (Optional)
- **Node.js** + **Express** - API gateway for off-chain data
- **PostgreSQL** - Indexed blockchain data for faster queries
- **Horizon API** - Stellar's REST API for ledger queries
- **Stellar Expert** - Block explorer integration

### Development & Testing
- **Stellar Standalone Network** - Local blockchain for testing
- **Soroban CLI** - Contract testing and simulation
- **Rust Test Framework** - Unit and integration testing
- **cargo-audit** - Security vulnerability scanning
- **GitHub Actions** - CI/CD pipeline

---

## Compliance & Ethics

### DSCSA Alignment (Drug Supply Chain Security Act)
PharmaTrust directly addresses DSCSA requirements for pharmaceutical traceability:

- **Serialization**: Each batch is uniquely identified via Stellar asset code
- **Verification**: Soroban contracts enable automated verification of trading partner legitimacy
- **Traceability**: Complete chain of custody recorded on immutable Stellar ledger
- **Recall Efficiency**: Instant identification of affected batches and downstream locations
- **Cost-Effective Compliance**: Stellar's low transaction fees make compliance economically viable for all participants

### Patient Data Privacy
- **No PII on-chain**: Patient identities are never recorded on the Stellar blockchain
- **Pseudonymous Verification**: QR code scans do not require patient identification or Stellar account
- **Public Ledger Benefits**: Stellar's transparent ledger enables verification without compromising privacy
- **HIPAA Compliance**: Off-chain systems handling patient data follow HIPAA guidelines
- **Consent-Based Reporting**: Patients opt-in to report issues; no mandatory data collection

### Ethical Considerations
- **Accessibility**: Low-cost IoT sensors and Stellar's minimal fees ensure solution is viable for developing markets
- **Financial Inclusion**: Stellar's mission aligns with making pharmaceutical verification accessible globally
- **Open Source**: Core Soroban contracts published for community audit and trust
- **Regulatory Collaboration**: Designed to complement, not replace, existing regulatory frameworks
- **Equitable Access**: Verification tools available to all patients regardless of technical literacy

---

## Getting Started

### Prerequisites
```bash
# Install Rust
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# Install Stellar CLI
cargo install --locked stellar-cli --features opt

# Install Node.js
node >= 18.0.0
npm >= 9.0.0

# Install Freighter wallet browser extension
# https://www.freighter.app/
```

### Installation
```bash
# Clone repository
git clone https://github.com/your-org/pharmatrust.git
cd pharmatrust

# Install contract dependencies
cd contracts
cargo build --target wasm32-unknown-unknown --release

# Configure environment
cp .env.example .env
# Add your Stellar network passphrase, secret key, IPFS credentials

# Run contract tests
cargo test

# Deploy to testnet
stellar contract deploy \
  --wasm target/wasm32-unknown-unknown/release/pharmatrust.wasm \
  --source ACCOUNT_SECRET_KEY \
  --network testnet
```

### Running the DApp
```bash
cd frontend
npm install
npm run dev
# Access at http://localhost:3000

# Build for production
npm run build
npm run start

# Run with Turbopack (faster)
npm run dev --turbo
```

---


## Contributing

We welcome contributions from the community. Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

---

## License

This project is licensed under the MIT License - see [LICENSE](LICENSE) file for details.



*PharmaTrust is committed to improving global health outcomes through transparent, verifiable pharmaceutical supply chains.*
