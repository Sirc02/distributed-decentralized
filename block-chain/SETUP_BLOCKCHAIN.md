# Panduan Setup & Menjalankan Blockchain Sui Application

## 📋 Prerequisites (Persyaratan)

Sebelum menjalankan aplikasi, pastikan Anda sudah install:

### 1. **Node.js & npm**
```bash
# Check if installed
node --version    # v18+ recommended
npm --version     # v9+

# Download dari: https://nodejs.org/
```

### 2. **Sui CLI Tools**
```bash
# Install Sui CLI
curl -fsSL https://github.com/MystenLabs/sui/releases/download/sui-v1.0.0/sui-ubuntu-x86_64 -o sui
chmod +x sui
mv sui /usr/local/bin/

# Verify installation
sui --version
```

### 3. **Sui Wallet Extension untuk Browser**
- Buka Chrome/Firefox Web Store
- Cari: "Sui Wallet"
- Install official extension dari Mysten Labs
- Create atau import wallet

### 4. **Git** (untuk clone project)
```bash
# Check if installed
git --version

# Download dari: https://git-scm.com/
```

---

## 🚀 Langkah-Langkah Menjalankan Aplikasi

### Step 1: Download/Clone Project dari Google Drive

```bash
# Link project ada di README.md
# https://drive.google.com/drive/folders/14zD5VKyev8gp7bceCZKrOBxnR7vXpIQt?usp=sharing

# Pilih salah satu:
# a) Download ZIP dari Google Drive
# b) Atau clone dari repository (jika ada GitHub link)

# Kalau download ZIP:
# 1. Buka link Google Drive
# 2. Klik Download (↓)
# 3. Extract folder ke workspace Anda

# Misal extract ke:
# c:\Users\user\Documents\STDT\distributed-decentrarized\block-chain\src\
```

### Step 2: Navigasi ke Folder Project

```bash
# Buka terminal PowerShell di VS Code
cd c:\Users\user\Documents\STDT\distributed-decentrarized\block-chain

# Atau jika ada subfolder:
cd .\src\  # tergantung struktur dari Google Drive
```

### Step 3: Install Frontend Dependencies (React)

```bash
# Install npm packages
npm install

# Atau gunakan yarn
yarn install
```

**Output yang diharapkan:**
```
added 1024 packages in 2m
```

### Step 4: Setup Environment Variables

Buat file `.env.local` di root folder project:

```bash
# Buat file
New-Item -Path .\.env.local -Type File

# Atau manual:
# - Di VS Code, klik "New File"
# - Nama: .env.local
# - Isi dengan konfigurasi di bawah
```

**Isi file `.env.local`:**
```env
# Sui Network Configuration
VITE_NETWORK=testnet
# atau mainnet

# Smart Contract PackageID (ganti dengan PackageID Anda)
VITE_PACKAGE_ID=0x1a2b3c4d5e6f7g8h9i0j1k2l3m4n5o6p

# Sui RPC Endpoint
VITE_SUI_RPC_URL=https://fullnode.testnet.sui.io:443

# Optional: Sui Explorer URL
VITE_SUI_EXPLORER=https://explorer.sui.io
```

**Catatan:** PackageID bisa didapat setelah deploy smart contract

### Step 5: Start Development Server

```bash
# Gunakan npm
npm run dev

# Atau gunakan yarn
yarn dev
```

**Output:**
```
VITE v5.0.0  ready in 432 ms

➜  Local:   http://localhost:5173/
➜  press h to show help
```

### Step 6: Buka Browser

```
http://localhost:5173
```

Anda sekarang bisa:
✅ Lihat React interface
✅ Klik "Connect Wallet" untuk connect Sui Wallet
✅ Klik "Create Greeting" untuk buat greeting di blockchain

---

## 📦 Untuk Deploy Smart Contract (Optional)

### Step 1: Setup Sui Dev Environment

```bash
# Install Sui Move CLI
cargo install --locked --git https://github.com/MystenLabs/sui.git sui

# Verify
sui --version
```

### Step 2: Buat Wallet untuk Deploy

```bash
# Generate new keypair
sui client new-address ed25519

# Atau import existing wallet
sui client import-address
```

### Step 3: Request Testnet Tokens (untuk gas)

```bash
# Di faucet testnet Sui
# https://faucet.testnet.sui.io/

# Atau gunakan CLI
sui client faucet
```

### Step 4: Publish Smart Contract

```bash
# Navigasi ke folder smart contract
cd path/to/smart/contract

# Publish ke testnet
sui client publish --gas-budget 10000000

# Output akan memberikan PackageID
# Copy PackageID dan paste ke .env.local
```

---

## 🔄 Struktur Project (Expected)

```
block-chain/
├── src/                          (React frontend)
│   ├── App.tsx
│   ├── App.css
│   ├── main.tsx
│   ├── constants.ts              (PackageID disini)
│   ├── components/
│   │   ├── WalletConnect.tsx
│   │   ├── CreateGreeting.tsx
│   │   ├── GreetingList.tsx
│   │   └── etc.
│   └── services/
│       └── suiService.ts         (Blockchain interaction)
│
├── smart-contract/              (Move/Sui smart contract)
│   ├── sources/
│   │   └── greeting.move        (Smart contract code)
│   └── Move.toml
│
├── .env.local                   (Environment variables)
├── package.json
├── vite.config.ts
├── tsconfig.json
└── README.md
```

---

## ⚙️ Konfigurasi Penting

### constants.ts
```typescript
// Harus dikonfigure dengan PackageID dari smart contract

export const PACKAGE_ID = "0x...";  // Ganti dengan PackageID Anda
export const NETWORK = "testnet";   // atau "mainnet"
export const SUI_RPC = "https://fullnode.testnet.sui.io:443";
```

### services/suiService.ts
```typescript
// File ini handle semua interaksi blockchain:

export async function createGreeting(text: string) {
    // Call smart contract new() function
    // Sign transaksi dengan wallet
    // Wait untuk blockchain confirm
    // Return greeting object ID
}

export async function updateGreeting(greetingId: string, newText: string) {
    // Call smart contract update_text() function
    // Similar workflow
}

export async function getGreetings() {
    // Query blockchain untuk semua greetings
    // Return array of greeting objects
}
```

---

## 🐛 Troubleshooting

### Error: "Wallet not connected"
```
✓ Pastikan Sui Wallet extension sudah install
✓ Import atau create wallet di extension
✓ Network di wallet = testnet/mainnet (sesuai .env)
✓ Klik "Connect Wallet" button di aplikasi
```

### Error: "PackageID not found"
```
✓ Pastikan PackageID di .env.local benar
✓ PackageID dari output saat publish smart contract
✓ Harus format: 0x...
```

### Error: "Insufficient balance for gas"
```
✓ Request testnet tokens dari faucet:
   https://faucet.testnet.sui.io/
✓ Tunggu beberapa detik untuk balance update
```

### Error: "Smart contract function not found"
```
✓ Verify PackageID benar
✓ Verify function name benar (case-sensitive)
✓ Verify smart contract sudah published
```

---

## 📊 Network Configuration

### Testnet (Recommended untuk development)
```env
VITE_NETWORK=testnet
VITE_SUI_RPC_URL=https://fullnode.testnet.sui.io:443
VITE_SUI_EXPLORER=https://testnet.sui.news

# Faucet untuk tokens: https://faucet.testnet.sui.io/
```

### Mainnet (Production)
```env
VITE_NETWORK=mainnet
VITE_SUI_RPC_URL=https://fullnode.mainnet.sui.io:443
VITE_SUI_EXPLORER=https://suiscan.xyz

# Butuh real SUI tokens (di exchange)
```

---

## 🚀 Build untuk Production

```bash
# Build optimized version
npm run build

# Output terletak di folder: dist/
```

Setelah build:
```bash
# Test build locally
npm run preview

# Atau deploy ke hosting:
# - Vercel: vercel deploy
# - Netlify: netlify deploy
# - AWS S3: aws s3 sync dist/ s3://bucket-name
```

---

## 📝 Useful Commands

| Command | Fungsi |
|---------|--------|
| `npm run dev` | Start dev server (localhost:5173) |
| `npm run build` | Build untuk production |
| `npm run preview` | Preview build production |
| `npm run lint` | Check code quality |
| `sui client balance` | Check wallet balance |
| `sui client faucet` | Request testnet tokens |
| `sui client objects` | List owned objects |

---

## 🔗 Useful Links

| Resource | URL |
|----------|-----|
| **Sui Docs** | https://docs.sui.io |
| **Sui Testnet Faucet** | https://faucet.testnet.sui.io |
| **Sui Explorer (Testnet)** | https://testnet.sui.news |
| **TypeScript/Sui SDK** | https://github.com/MystenLabs/sui/tree/main/sdk/typescript |
| **React + Sui Template** | https://github.com/MystenLabs/create-sui-dapp |

---

## ✅ Checklist Sebelum Deploy ke Production

- [ ] Smart contract sudah tested & audited
- [ ] PackageID di .env.local benar
- [ ] Network configuration = mainnet
- [ ] Wallet punya sufficient gas untuk operations
- [ ] Frontend error handling sudah implemented
- [ ] Build production tested locally
- [ ] Gas budget di smart contract calls sudah optimal

---

## 💡 Tips & Best Practices

### Development
```bash
# Use environment variables untuk sensitif info
# Jangan hardcode PackageID atau RPC URL

# Example:
const packageId = import.meta.env.VITE_PACKAGE_ID;
const rpcUrl = import.meta.env.VITE_SUI_RPC_URL;
```

### Testing Transactions
```bash
# Test di testnet dulu (free tokens, reverted quickly)
# Login dengan test wallet
# Create greeting beberapa kali
# Update greeting dari user lain
# Verify semua berjalan di explorer
```

### Monitoring
```bash
# Check transaction status:
# https://testnet.sui.news/

# Search dengan transaction hash
# Verify create & update functions berjalan
# Check object state changes
```

---

## 🎯 Next Steps

1. ✅ Download project dari Google Drive
2. ✅ Install dependencies (`npm install`)
3. ✅ Setup `.env.local` dengan PackageID
4. ✅ Install Sui Wallet extension
5. ✅ Run dev server (`npm run dev`)
6. ✅ Test aplikasi: connect wallet → create greeting
7. ✅ Verify transaction di Sui Explorer
8. ✅ Build untuk production (`npm run build`)
