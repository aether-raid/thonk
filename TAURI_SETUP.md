# Tauri Desktop App Setup

Your project is now configured as a Tauri desktop application! Here's what was done:

## What Changed

### 1. **Tauri Structure Added** (`frontend/src-tauri/`)
- Rust backend for native OS integration
- Automatic Python subprocess spawning
- Native window management

### 2. **Vite Config Updated** (`frontend/vite.config.ts`)
- Build output set to `dist/` directory (required by Tauri)

### 3. **Rust Backend Code** (`frontend/src-tauri/src/lib.rs`)
- Automatically spawns your Python Flask backend when the app starts
- Looks for `backend/app.py` relative to the application
- Logs startup status and errors

### 4. **Tauri Config** (`frontend/src-tauri/tauri.conf.json`)
- Frontend dev URL: `http://localhost:5173`
- Frontend build command: `npm run build`
- Window: 800x600, resizable
- Product name: "thonk"

### 5. **New npm Scripts** (`frontend/package.json`)
- `npm run tauri:dev` - Run app in development mode
- `npm run tauri:build` - Build production binary
- `npm run tauri` - Direct tauri CLI access

## How to Run

### Development Mode
```bash
cd frontend
npm run tauri:dev
```

This will:
1. Start Vite dev server (http://localhost:5173)
2. Build Rust backend
3. Launch Tauri desktop app
4. Spawn Python backend process at http://localhost:8000

### Production Build
```bash
cd frontend
npm run tauri:build
```

Creates a native:
- **macOS**: `frontend/src-tauri/target/release/bundle/dmg/thonk_*.dmg`
- **Windows**: `.exe` installer
- **Linux**: `.AppImage`

## Important Notes

1. **Python Must Be Installed**
   - The app calls `python3 app.py` from the backend directory
   - Make sure your system has Python 3 installed

2. **Backend Must Be Installed**
   - Run `pip install -r requirements.txt` in the `backend/` directory first
   - Or however you normally set up your backend dependencies

3. **Architecture**
   ```
   User clicks desktop app icon
   ↓
   Tauri window opens
   ↓
   Rust code spawns: python3 backend/app.py
   ↓
   Frontend connects to localhost:8000
   (Same as before, but now all packaged as one app)
   ```

4. **Frontend Code Unchanged**
   - Your React code stays the same
   - API calls still use `http://localhost:8000` or wherever the backend runs
   - No need to change WebSocket/HTTP communications

## Troubleshooting

**"Python not found"** error?
- Make sure `python3` is in your PATH
- Run `which python3` to verify it's installed

**Backend fails to start?**
- Check that `backend/app.py` exists
- Install backend dependencies: `cd backend && pip install -r requirements.txt`
- Check logs with: `npm run tauri:dev` (stderr shows backend startup)

**Port 8000 already in use?**
- Change `app.py` to use a different port
- Or kill the process: `lsof -ti:8000 | xargs kill -9`

## Next Steps

1. Test development mode: `npm run tauri:dev`
2. Test that frontend connects to backend properly
3. Build production app: `npm run tauri:build`
4. Distribute the `.dmg` / `.exe` file to users

Enjoy your lightweight, secure, desktop BCI application! 🚀
