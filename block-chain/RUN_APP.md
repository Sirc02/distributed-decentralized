# Panduan Menjalankan Aplikasi Blockchain Sui

## ⚡ Quick Start (5 Menit)

### 1️⃣ Download Project
```bash
# Buka link Google Drive dari README.md:
# https://drive.google.com/drive/folders/14zD5VKyev8gp7bceCZKrOBxnR7vXpIQt?usp=sharing

# Download ZIP → Extract ke folder project Anda
```

### 2️⃣ Buka Terminal & Navigasi
```bash
cd c:\Users\user\Documents\STDT\distributed-decentrarized\block-chain

# Jika ada subfolder, masuk ke sana:
cd src
```

### 3️⃣ Install Dependencies
```bash
npm install

# Tunggu sampai selesai (biasanya 2-5 menit)
```

### 4️⃣ Setup Environment Variables
Buat file `.env.local` di root folder dengan isi:

```env
VITE_NETWORK=testnet
VITE_PACKAGE_ID=0x1a2b3c4d5e6f7g8h9i0j1k2l3m4n5o6p
VITE_SUI_RPC_URL=https://fullnode.testnet.sui.io:443
VITE_SUI_EXPLORER=https://explorer.sui.io
```

> **Catatan:** Ganti `VITE_PACKAGE_ID` dengan PackageID dari smart contract Anda

### 5️⃣ Jalankan Aplikasi
```bash
npm run dev
```

**Output:**
```
VITE v5.0.0  ready in 432 ms
➜  Local:   http://localhost:5173/
➜  press h to show help
```

### 6️⃣ Buka di Browser
```
http://localhost:5173
```

✅ **Aplikasi siap!** Sekarang Anda bisa:
- Click "Connect Wallet"
- Click "Create Greeting"
- Lihat greeting muncul di blockchain

---

## 🔑 Persiapan Wallet (PENTING!)

Sebelum bisa interact dengan blockchain, pastikan sudah setup:

### 1. Install Sui Wallet Extension
```
Chrome/Firefox → Extensions → Search "Sui Wallet"
→ Install dari Mysten Labs (official)
```

### 2. Create atau Import Wallet
- Buka extension
- Click "Create Wallet" atau "Import Wallet"
- Save recovery phrase (jangan share!)

### 3. Switch Network ke Testnet
- Di wallet extension
- Network selection → Testnet

### 4. Get Testnet Tokens (Free)
```
Buka: https://faucet.testnet.sui.io/
Paste: Wallet address Anda
Klik: Request SUI
```

Tunggu ~1 menit, Anda dapat 1 SUI (gratis untuk testing)

---

## 🚀 Menjalankan Step by Step

### Step 1: Terminal → Navigasi
```powershell
# PowerShell
cd c:\Users\user\Documents\STDT\distributed-decentrarized\block-chain

# Atau drag folder ke terminal
```

### Step 2: Install Dependencies
```bash
npm install

# Output:
# added 1024 packages in 2m
# up to date in 500ms
```

### Step 3: Buat .env.local

**Option A - Menggunakan VS Code:**
```
1. Klik "Explorer" (file icon di sidebar kiri)
2. Right-click di root folder → New File
3. Nama: .env.local
4. Paste isi di bawah:
```

**.env.local contents:**
```env
VITE_NETWORK=testnet
VITE_PACKAGE_ID=0x1a2b3c4d5e6f7g8h9i0j1k2l3m4n5o6p
VITE_SUI_RPC_URL=https://fullnode.testnet.sui.io:443
VITE_SUI_EXPLORER=https://explorer.sui.io
```

**Option B - Menggunakan Terminal:**
```powershell
# Di folder project
New-Item -Path .\.env.local -Type File

# Atau gunakan Notepad
notepad .env.local
# Paste isi, Save, Close
```

### Step 4: Start Development Server
```bash
npm run dev
```

**Tunggu output seperti ini:**
```
  VITE v5.0.0  ready in 432 ms

  ➜  Local:   http://localhost:5173/
  ➜  press h to show help
```

### Step 5: Buka Browser
```
1. Buka Chrome/Firefox
2. Go to: http://localhost:5173
3. Akan muncul aplikasi React
```

---

## 🎯 Pertama Kali Gunakan Aplikasi

### 1. Click "Connect Wallet"
```
Button akan trigger Sui Wallet extension
Extension popup muncul → Click "Approve"
Wallet sekarang connected dengan aplikasi
```

### 2. Click "Create Greeting"
```
Form akan muncul → Type teks greeting
→ Click "Submit"
→ Wallet extension popup untuk confirm
→ Click "Approve"
```

### 3. Lihat Greeting Muncul
```
Aplikasi akan fetch data dari blockchain
Greeting baru tampil di list
Text: "Hello world!" (atau text yang Anda masukkan)
Creator: Wallet address Anda
```

### 4. Edit Greeting (Opsional)
```
Click tombol "Edit" di salah satu greeting
Ubah text
Approve di wallet
Lihat perubahan instantly
```

---

## 📱 Interface Aplikasi

```
┌─────────────────────────────────┐
│   Blockchain Greeting App       │
├─────────────────────────────────┤
│                                 │
│ Status: Connected (0x123...)    │
│ Balance: 1.5 SUI               │
│                                 │
├─────────────────────────────────┤
│                                 │
│ [Create Greeting Button]        │
│                                 │
├─────────────────────────────────┤
│                                 │
│ Daftar Greetings:              │
│                                 │
│ ✓ Hello world!                 │
│   Creator: 0x789...            │
│   [Edit] [Delete?]             │
│                                 │
│ ✓ Greeting ke-2                │
│   Creator: 0x456...            │
│   [Edit] [Delete?]             │
│                                 │
└─────────────────────────────────┘
```

---

## 🔄 Flow Aplikasi

```
┌─────────────────┐
│  User di Browser │
└────────┬────────┘
         │
         │ Click button
         ▼
┌─────────────────────────┐
│  React Component        │
│  - Read input form      │
│  - Prepare transaction  │
└────────┬────────────────┘
         │
         │ Call suiService.ts
         ▼
┌─────────────────────────┐
│  Sui Service            │
│  - Check wallet connect │
│  - Build transaction    │
│  - Get PackageID from   │
│    constants.ts         │
└────────┬────────────────┘
         │
         │ Request sign
         ▼
┌─────────────────────────┐
│  Sui Wallet Extension   │
│  - Show approve popup   │
│  - User click Approve   │
│  - Sign dengan private  │
│    key                  │
└────────┬────────────────┘
         │ Signed transaction
         ▼
┌─────────────────────────┐
│  Sui Blockchain         │
│  - Execute smart        │
│    contract             │
│  - Create/Update        │
│    Greeting object      │
│  - Save to ledger       │
└────────┬────────────────┘
         │ Response + object ID
         ▼
┌─────────────────────────┐
│  Display in UI          │
│  Greeting muncul di     │
│  React component        │
└─────────────────────────┘
```

---

## ✅ Verifikasi Aplikasi Berjalan

### Check 1: Server Running
```bash
# Di terminal Anda, harus lihat:
✓ localhost:5173 active
✓ VITE ready message
✓ Tidak ada error merah
```

### Check 2: Browser Accessible
```
http://localhost:5173 → Bisa diakses
Aplikasi render dengan baik
No console errors (F12 → Console)
```

### Check 3: Wallet Connected
```
Wallet extension icon menunjukkan connected
Atau text "Connected: 0x..." di aplikasi
```

### Check 4: Create Greeting Works
```
1. Click "Create Greeting"
2. Wallet popup muncul (approve)
3. Greeting muncul di list
4. Check di Sui Explorer (optional):
   https://explorer.sui.io
   Search dengan transaction hash
```

---

## 🐛 Common Issues & Solutions

### ❌ Error: "localhost:5173 refused to connect"
```
✓ Check apakah 'npm run dev' running
✓ Terminal harus show "ready" message
✓ Coba refresh browser (F5)
✓ Coba port lain: http://localhost:5174
```

### ❌ Error: "Wallet not connected"
```
✓ Sui Wallet extension sudah install?
✓ Wallet sudah create/import?
✓ Network di wallet = testnet?
✓ Click "Connect Wallet" button di aplikasi
✓ Approve popup di extension
```

### ❌ Error: "VITE_PACKAGE_ID not found"
```
✓ Buat file .env.local
✓ Paste PackageID (format: 0x...)
✓ Save file
✓ Refresh aplikasi (Ctrl+Shift+R)
```

### ❌ Error: "Insufficient balance for gas"
```
✓ Buka faucet: https://faucet.testnet.sui.io/
✓ Paste wallet address
✓ Request SUI
✓ Tunggu ~1 menit
✓ Check balance: wallet extension → Balance
```

### ❌ Error: "Smart contract not found"
```
✓ PackageID salah?
✓ Smart contract belum published ke blockchain?
✓ Network mismatch (testnet vs mainnet)?
✓ Check PackageID di constants.ts file
```

### ❌ Aplikasi blank/white screen
```
✓ Open browser console: F12
✓ Lihat error message
✓ Check .env.local configuration
✓ Refresh: Ctrl+Shift+R (hard refresh)
```

---

## 📊 File Structure (Expected)

```
project-folder/
├── src/
│   ├── App.tsx              (Main component)
│   ├── App.css
│   ├── main.tsx
│   ├── constants.ts         ← PENTING: PackageID disini
│   ├── components/
│   │   ├── WalletConnect.tsx
│   │   ├── CreateGreeting.tsx
│   │   ├── GreetingList.tsx
│   │   └── EditGreeting.tsx
│   └── services/
│       └── suiService.ts    ← Blockchain interaction
│
├── .env.local               ← PENTING: Create ini
├── package.json
├── vite.config.ts
├── tsconfig.json
└── README.md
```

---

## 🚀 Perintah Penting

| Perintah | Fungsi |
|----------|--------|
| `npm run dev` | Start dev server (localhost:5173) |
| `npm install` | Install dependencies |
| `npm run build` | Build untuk production |
| `Ctrl+C` | Stop server |
| `F5` | Refresh browser |
| `F12` | Open browser developer tools |
| `Ctrl+Shift+R` | Hard refresh (clear cache) |

---

## 🔗 Useful Links

| Link | Fungsi |
|------|--------|
| http://localhost:5173 | Aplikasi local Anda |
| https://faucet.testnet.sui.io | Get free testnet tokens |
| https://explorer.sui.io | View transactions |
| https://docs.sui.io | Sui documentation |
| Sui Wallet Extension | Install dari Chrome/Firefox store |

---

## 💾 Package.json Commands

Lihat di `package.json` untuk melihat semua available commands:

```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "lint": "eslint src/"
  }
}
```

---

## ✨ Workflow Harian

**Pagi:** Start aplikasi
```bash
npm run dev
# Open http://localhost:5173
```

**Development:** Edit kode baik di `src/` folder
```
React hot reload otomatis
Perubahan langsung terlihat di browser
Tidak perlu restart server
```

**Testing:** Test di browser
```
Interact dengan UI
Check console untuk errors (F12)
Verify blockchain transactions
```

**Malam:** Stop aplikasi
```bash
Ctrl+C di terminal
(Atau close terminal)
```

---

## 🎓 Next Steps

1. ✅ Install & run aplikasi (`npm run dev`)
2. ✅ Setup wallet (Sui Wallet extension)
3. ✅ Connect wallet ke aplikasi
4. ✅ Create greeting pertama Anda
5. ✅ Verify di blockchain explorer
6. ✅ Call teman untuk edit greeting Anda
7. ✅ Explore smart contract di Google Drive folder
8. ✅ Deploy ke production (nanti)

---

## 📞 Support

Jika ada issue:

1. **Check console** → F12 → Console tab
2. **Check terminal** → npm run dev output
3. **Check .env.local** → Format benar?
4. **Check wallet** → Connected? Network testnet?
5. **Check blockchain** → https://explorer.sui.io
6. **Check docs** → https://docs.sui.io

Good luck! 🚀
