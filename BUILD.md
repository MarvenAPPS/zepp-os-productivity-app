# Build and Deployment Guide

## Building the Application

### Development Build (Hot Reload)

```bash
npm run dev
```

This command:
- Starts the Zeus development server
- Connects to the simulator (if running)
- Automatically recompiles on file changes
- Shows real-time output in terminal

### Production Build

```bash
npm run build
```

This command:
- Optimizes and minifies all code
- Generates the `.zab` installation package
- Outputs to `dist/` directory
- Ready for publishing to Zepp Market

## Output

After building, check `dist/` directory:

```
dist/
├── zepp-os-productivity-app.zab  # Main installation package
├── resources/                     # Compiled resources
└── index.js                       # Compiled application
```

## Device Testing

### Method 1: Real Device via QR Code

1. Ensure your phone is connected to the same WiFi as your development computer
2. Ensure the Amazfit Balance is paired with Zepp App on your phone
3. Run: `npm run preview`
4. A QR code will appear in the terminal
5. Open Zepp App on your phone
6. Navigate to the app or miniprogram section
7. Scan the QR code
8. The app will install automatically

### Method 2: Simulator

1. Download and install Zepp OS Emulator
2. Launch the emulator
3. Configure emulator connection (IP and port)
4. Run: `npm run dev`
5. Simulator will show the installed app
6. Test all functionality

### Method 3: Direct Installation

1. Build the app: `npm run build`
2. Find `zepp-os-productivity-app.zab` in `dist/`
3. Transfer to phone via USB or Zepp developer platform
4. Open Zepp App and install locally

## Publishing to Zepp Market

### Prerequisites

1. Developer account at https://open.zepp.com/
2. App registered on developer platform
3. App icon (256x256 PNG)
4. Screenshots (480x480 PNG, 3-5 images)
5. App description (150 characters)
6. Privacy policy URL

### Publishing Steps

1. **Build the app**:
   ```bash
   npm run build
   ```

2. **Login to Zepp Developer Platform**:
   - Go to https://open.zepp.com/
   - Login with your developer account

3. **Navigate to "My Apps"**:
   - Click on your registered app

4. **Upload Package**:
   - Go to version management
   - Click "New Version"
   - Upload `dist/zepp-os-productivity-app.zab`

5. **Add Metadata**:
   - Add version description
   - Upload screenshots
   - Set release notes

6. **Submit for Review**:
   - Click "Submit for Review"
   - Wait for approval (usually 1-3 days)

7. **Release**:
   - After approval, click "Release"
   - App becomes available in Zepp Market

## Version Management

### Updating Version Number

Edit `package.json`:

```json
{
  "version": "1.1.0"
}
```

Also update `app.json`:

```json
{
  "app": {
    "appVersion": "1.1.0"
  }
}
```

### Semantic Versioning

Follow SemVer: MAJOR.MINOR.PATCH

- **MAJOR**: Breaking changes
- **MINOR**: New features (backward compatible)
- **PATCH**: Bug fixes

Example: `1.2.3`

## Troubleshooting

### Build Errors

#### "Module not found"

```bash
# Clear cache and rebuild
rm -rf node_modules package-lock.json
npm install
npm run build
```

#### "Syntax error" or "Parse error"

1. Check file has no typos
2. Verify all imports use correct paths
3. Ensure closing braces/brackets match
4. Check for mixing spaces and tabs

### Device Connection Issues

#### QR Code Won't Scan

1. Check WiFi connection on both devices
2. Ensure firewall isn't blocking port 8080
3. Try restarting the development server
4. Check terminal for error messages

#### App Won't Install

1. Verify Amazfit Balance is properly paired in Zepp App
2. Ensure sufficient storage on watch (>10MB free)
3. Check device compatibility (Amazfit Balance required)
4. Try clearing watch cache: Settings > Apps > (Long press) > Clear Cache

### Performance Issues

#### App Crashes

1. Check browser console for JavaScript errors
2. Verify all data storage operations are correct
3. Ensure no infinite loops
4. Test memory usage with large datasets

#### Slow Performance

1. Minimize UI redraws
2. Use lazy loading for large lists
3. Optimize asset sizes (images, fonts)
4. Profile with bridge mode: `zeus bridge`

## Verification Checklist Before Deployment

- [ ] All features implemented and tested
- [ ] No console errors in development
- [ ] App runs on simulator without crashes
- [ ] Tested on physical Amazfit Balance device
- [ ] No memory leaks (tested with extended use)
- [ ] All UI elements properly aligned (no overlapping text)
- [ ] Navigation works smoothly
- [ ] Data persistence works correctly
- [ ] Version number updated
- [ ] README and docs are current
- [ ] Code is clean with no TODO comments
- [ ] Build completes without warnings
- [ ] dist/zepp-os-productivity-app.zab file exists

## Remote Verification (Post-Deployment)

Before final delivery, verify on remote (not locally):

1. **Clone fresh copy of repository**:
   ```bash
   git clone https://github.com/MarvenAPPS/zepp-os-productivity-app.git test-repo
   cd test-repo
   ```

2. **Install and build**:
   ```bash
   npm install
   npm run build
   ```

3. **Check for missing files**:
   ```bash
   ls -la dist/
   ls -la src/
   ```

4. **Verify no file corruption**:
   - Check all TypeScript/JavaScript files compile
   - Verify no syntax errors
   - Test app installation with QR code

5. **Document any issues found** and fix:
   - Record what was missing
   - Note what was corrupted
   - Provide fix instructions

## CI/CD Integration (Optional)

For automated builds on each commit:

```yaml
# .github/workflows/build.yml
name: Build and Test

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
        with:
          node-version: '14'
      - run: npm install
      - run: npm run build
      - run: npm run lint (if implemented)
```

## Documentation Requirements

All deployments must include:

1. **README.md** - Project overview and quick start
2. **SETUP.md** - Development environment setup
3. **BUILD.md** - This file, build and deployment guide
4. **ARCHITECTURE.md** - Technical design and API documentation
5. **CHANGELOG.md** - Version history and changes

## Support and Issues

For deployment issues:

1. Check [SETUP.md](./SETUP.md) for environment problems
2. Review [ARCHITECTURE.md](./ARCHITECTURE.md) for design questions
3. Create an issue on GitHub with:
   - Error message (full output)
   - Steps to reproduce
   - Environment info (OS, Node version, Zeus CLI version)
   - Screenshots if applicable

## Next Steps After Deployment

1. Monitor app usage statistics on Zepp Market
2. Collect user feedback and bug reports
3. Plan improvements for next version
4. Maintain documentation as code evolves
5. Keep dependencies updated

---

For detailed development information, see [SETUP.md](./SETUP.md).
For architecture details, see [ARCHITECTURE.md](./ARCHITECTURE.md).
