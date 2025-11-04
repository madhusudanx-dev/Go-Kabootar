# GoKabootar - How to Run

## Prerequisites
- Node.js >= 18
- npm >= 8

## Step-by-Step Setup

### 1. Install Root Dependencies
```bash
npm install
```

### 2. Install App Dependencies
```bash
cd app
npm install
cd ..
```

### 3. Build Frontend (Required before running)
```bash
cd app
npm run build
cd ..
```

### 4. Run the Application

#### Development Mode (with auto-reload)
```bash
npm run dev
```

#### Production Mode
```bash
npm start
```

#### Debug Mode
```bash
npm run debug
```

## Quick Start (All in One)

```bash
# Install all dependencies
npm install
cd app && npm install && cd ..

# Build frontend
cd app && npm run build && cd ..

# Run in development mode
npm run dev
```

## Alternative: Development with Hot Reload

If you want to develop with hot reload for the frontend:

```bash
# Terminal 1: Run frontend dev server
cd app
npm run dev

# Terminal 2: Run backend server
cd ..
npm run dev
```

## Default Access
- Application: http://localhost:3000
- Admin Panel: http://localhost:3000/admin (if admin password is set)

## Configuration
- Edit `config.js` or create `config.dev.js` for development settings
- Set environment variables like `GoKabootar_UPLOAD_PASS` for upload password protection

## Troubleshooting

### Port Already in Use
Change the port in `config.js` or set `GoKabootar_PORT` environment variable:
```bash
GoKabootar_PORT=3001 npm run dev
```

### Build Errors
Make sure you've installed all dependencies:
```bash
npm install
cd app && npm install && cd ..
```

### Missing Frontend Files
If you see errors about missing JS files, make sure you've built the frontend:
```bash
cd app && npm run build && cd ..
```

