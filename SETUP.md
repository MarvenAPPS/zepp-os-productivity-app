# Setup and Development Guide

## Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js**: v14.0.0 or higher (check with `node --version`)
- **npm** or **yarn**: Comes with Node.js
- **Git**: For version control

## Installation Steps

### 1. Clone the Repository

```bash
git clone https://github.com/MarvenAPPS/zepp-os-productivity-app.git
cd zepp-os-productivity-app
```

### 2. Install Node Modules

Using npm:
```bash
npm install
```

Or using yarn:
```bash
yarn install
```

### 3. Install Zeus CLI Globally

Zeus CLI is the development tool for Zepp OS projects:

```bash
npm install -g @zeppos/zeus-cli
```

Verify installation:
```bash
zeus --version
```

### 4. Setup Zepp Developer Account

Create a Zepp developer account at: https://open.zepp.com/

Login to your account:
```bash
zeus login
```

This will open your browser to authenticate and store your credentials locally.

## Development Workflow

### Option 1: Using npm Scripts (Recommended)

All commands are available through npm scripts:

```bash
# Start development with hot reload
npm run dev

# Build the app
npm run build

# Generate QR code for device preview
npm run preview

# Check login status
npm run status
```

### Option 2: Using Zeus CLI Directly

If you prefer to use Zeus CLI commands directly:

```bash
# Development
zeus dev

# Build
zeus build

# Real device preview
zeus preview

# Bridge mode for advanced debugging
zeus bridge
```

## Simulator Setup (Optional)

For testing without a physical device:

1. Download the Zepp OS Simulator from the Zepp developer platform
2. Install and launch the simulator
3. In the simulator settings, configure the IP address and port
4. Run `zeus dev` to connect the CLI to the simulator

## Project Configuration

### app.json

The main configuration file `app.json` contains:

- **App metadata**: Name, version, ID
- **Permissions**: Required system permissions
- **Modules**: Pages and services to include
- **Platforms**: Target devices and design resolution

### Key Configuration Values

- `designWidth: 480` - Design width for Amazfit Balance (480x480 square display)
- `deviceSource: 516` - Device identifier for Amazfit Balance Round
- `appId: 1111748283` - Unique app identifier from Zepp platform

## Build Artifacts

After running `npm run build`, the output will be in the `dist/` directory:

- `dist/zepp-os-productivity-app.zab` - Installation package for publishing
- Various intermediate build files

## File Structure

```
src/
├─ pages/           # Device app pages (UI)
├─ services/        # Background services
├─ components/      # Reusable components
├─ utils/           # Utility functions
├─ types/           # TypeScript definitions
├─ assets/          # Images and fonts
└─ styles/          # Theme and constants
```

## Code Organization Best Practices

### Pages

Each page file should:
1. Import necessary dependencies
2. Define page layout using ZML/UI components
3. Handle user interactions
4. Import and use utility functions
5. Export the page object

### Services

Background services should:
1. Use `AppService` constructor
2. Implement `onInit` and `onDestroy` lifecycle hooks
3. Handle permissions properly
4. Use only available APIs (see API limitations in App Service docs)

### Utils

Utility files should contain:
1. Pure functions with no side effects
2. Data transformation and validation
3. Reusable business logic
4. Type definitions and exports

## Common Development Commands

```bash
# Start development with automatic reload
npm run dev

# Build for production without publishing
npm run build

# Test on real device by scanning QR code
npm run preview

# Check if you're logged in and view status
npm run status

# Enter bridge mode for advanced debugging
npm run bridge
```

## Debugging Tips

1. **Console Output**: Use `console.log()` to output debug information
2. **Simulator Logs**: Check the simulator console for real-time logs
3. **Bridge Mode**: Use `zeus bridge` for more detailed debugging
4. **Device Preview**: Use QR code preview to test on actual device

## Troubleshooting

### Issue: "zeus: command not found"

**Solution**: Install Zeus CLI globally:
```bash
npm install -g @zeppos/zeus-cli
```

### Issue: "Module not found" errors

**Solution**: Ensure all dependencies are installed:
```bash
npm install
```

### Issue: Login errors

**Solution**: Re-login to your Zepp developer account:
```bash
zeus login
```

### Issue: Build fails with file not found

**Solution**: Check that all imports are using correct relative paths and file extensions.

See [BUILD.md](./BUILD.md) for more detailed troubleshooting.

## Next Steps

1. Review [ARCHITECTURE.md](./ARCHITECTURE.md) for system design
2. Examine existing page implementations
3. Implement new features following the patterns established
4. Test thoroughly on both simulator and device
5. Commit changes with descriptive messages

## IDE Setup (Recommended)

### VS Code

Install these extensions for better Zepp OS development:

- **Zepp Studio Extension** (if available in marketplace)
- **ES6 Syntax Highlighting**
- **Code Runner** (optional, for quick testing)
- **GitLens** (for git integration)

### JetBrains Fleet (Your Preference)

- Native JavaScript/TypeScript support
- Git integration built-in
- Excellent code completion

## Resources

- [Official Zepp OS Documentation](https://docs.zepp.com/)
- [Zepp Developer Platform](https://open.zepp.com/)
- [API Reference](https://docs.zepp.com/docs/reference/device-app-api/)
- [Component Library](https://docs.zepp.com/docs/reference/device-app-api/zml/)

---

For issues or questions, refer to [BUILD.md](./BUILD.md) or create an issue on GitHub.
