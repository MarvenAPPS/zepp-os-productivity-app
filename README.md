# Zepp OS Productivity App

## Project Overview

A comprehensive Zepp OS V3 productivity application for the **Amazfit Balance Round** smartwatch. This app delivers a powerful question-based alarm system with points tracking and daily affirmations to boost productivity and mindfulness.

### Key Features

- **Multi-Question Alarms**: Up to 6 alarms per day, each customizable with 5-30 questions
- **Dynamic Question Selection**: Questions are randomly selected from your library for each alarm
- **Points System**: Earn or lose points based on answers, redeemable in blocks of 5,000 points
- **Affirmations**: Random affirmations display for 30 seconds after completing questions
- **Vibration Feedback**: Continuous vibration during alarm that stops only on timeout (2 minutes) or after answering
- **Flexible Scheduling**: Set alarms with custom times and recurrence patterns
- **Smooth UI**: Production-ready interface with no overlapping text, nice color theme
- **Persistent Storage**: Local data management with sync capabilities

## Technical Stack

- **Platform**: Zepp OS V3 API
- **Device**: Amazfit Balance Round (Square Screen, 480x480px)
- **Framework**: Zeus CLI (Node.js based build tool)
- **Language**: JavaScript ES6+
- **Architecture**: Device App + App Service (Background Service)
- **Version Control**: Single branch development

## Project Structure

```
zepp-os-productivity-app/
├── README.md                           # Project documentation
├── SETUP.md                            # Setup and development guide
├── BUILD.md                            # Build and deployment guide
├── ARCHITECTURE.md                     # Architecture and design decisions
├── app.json                            # Main application configuration
├── package.json                        # Dependencies and build scripts
├── tsconfig.json                       # TypeScript configuration (optional)
├── .github/
│   └── workflows/                      # CI/CD workflows (if needed)
├── src/
│   ├── pages/                          # Device App pages
│   │   ├── index.ts                    # Home/Dashboard page
│   │   ├── add-question.ts             # Add question interface
│   │   ├── add-alarm.ts                # Create alarm interface
│   │   ├── manage-alarms.ts            # View/edit alarms
│   │   ├── manage-questions.ts         # View/edit questions
│   │   ├── alarm-response.ts           # Alarm question answering interface
│   │   ├── affirmation-display.ts      # Affirmation display page
│   │   ├── points-summary.ts           # Points tracking and redemption
│   │   └── settings.ts                 # App settings
│   ├── services/
│   │   └── alarm-service.ts            # Background alarm service
│   ├── components/
│   │   ├── alarm-item.ts               # Reusable alarm item component
│   │   ├── question-item.ts            # Reusable question item component
│   │   └── navigation-footer.ts        # Navigation component
│   ├── utils/
│   │   ├── storage.ts                  # Local storage management
│   │   ├── vibration.ts                # Vibration utilities
│   │   ├── time-utils.ts               # Time/scheduling utilities
│   │   ├── random.ts                   # Random selection utilities
│   │   └── validators.ts               # Input validation
│   ├── types/
│   │   └── index.ts                    # TypeScript type definitions
│   ├── assets/
│   │   ├── fonts/                      # Custom fonts if needed
│   │   └── images/                     # Icons and images
│   └── styles/
│       └── constants.ts                # Color constants and theme
├── dist/                               # Build output directory
└── .gitignore                          # Git ignore rules
```

## Quick Start

### Prerequisites

- Node.js 14.x or higher
- npm or yarn
- Zeus CLI installed globally
- Zepp OS emulator (optional, for development)

### Installation

```bash
# Clone the repository
git clone https://github.com/MarvenAPPS/zepp-os-productivity-app.git
cd zepp-os-productivity-app

# Install dependencies
npm install

# Install Zeus CLI globally (if not already installed)
npm install -g @zeppos/zeus-cli
```

### Development

```bash
# Start development server with hot reload
npm run dev

# Or using Zeus CLI directly
zeus dev
```

### Building

```bash
# Build for production
npm run build

# Or using Zeus CLI directly
zeus build
```

### Testing on Device

```bash
# Generate QR code for real device preview
npm run preview

# Or using Zeus CLI directly
zeus preview
```

## Data Models

### Question
```typescript
interface Question {
  id: string;                    // Unique identifier
  text: string;                  // Question text (max 100 chars)
  points: number;                // Points for correct answer (range: -100 to +100)
  createdAt: number;             // Timestamp
}
```

### Alarm
```typescript
interface Alarm {
  id: string;                    // Unique identifier
  hour: number;                  // Hour (0-23)
  minute: number;                // Minute (0-59)
  enabled: boolean;              // Is alarm active
  repeatDays: boolean[];         // [Sun, Mon, Tue, Wed, Thu, Fri, Sat]
  questionCount: number;         // Questions per alarm (5-30)
  createdAt: number;             // Timestamp
}
```

### Affirmation
```typescript
interface Affirmation {
  id: string;                    // Unique identifier
  text: string;                  // Affirmation text (max 200 chars)
  createdAt: number;             // Timestamp
}
```

### AlarmSession
```typescript
interface AlarmSession {
  id: string;                    // Unique session identifier
  alarmId: string;               // Reference to alarm
  questions: Question[];         // Selected questions for this session
  answers: Map<string, boolean>; // Question ID -> Answer
  startTime: number;             // Session start timestamp
  endTime?: number;              // Session end timestamp
  pointsEarned: number;          // Total points for this session
}
```

## API Reference

See [ARCHITECTURE.md](./ARCHITECTURE.md) for detailed API documentation.

## Configuration

Key limits and constants:
- **Max Questions**: 30
- **Max Alarms**: 6 per day
- **Min Questions per Alarm**: 5
- **Max Questions per Alarm**: 30
- **Alarm Timeout**: 2 minutes (120 seconds)
- **Affirmation Display Duration**: 30 seconds
- **Points Redemption Minimum**: 5,000
- **Max Total Points**: No limit (tracks cumulatively)

## Building from Scratch

This repository is ready-to-use. However, if you need to rebuild from scratch:

```bash
zeus create zepp-os-productivity-app
cd zepp-os-productivity-app
# Follow SETUP.md for configuration
```

## Deployment

### To Zepp Market
1. Build the app: `npm run build`
2. Upload `dist/zepp-os-productivity-app.zab` to Zepp developer platform
3. Fill in required metadata and submit for review

### To Device via Zepp App
1. Generate QR code: `npm run preview`
2. Scan QR code in Zepp App on your phone
3. Install app to your Amazfit Balance

## Troubleshooting

See [BUILD.md](./BUILD.md) for common issues and solutions.

## Contributing

Development guidelines:
1. All code must be production-ready (no TODOs or placeholders)
2. Complete documentation required for all modules
3. Test on simulator and real device before committing
4. Follow Zepp OS design guidelines
5. Maintain code quality and readability

## License

MIT License - See LICENSE file for details

## Author

Marven (MarvenAPPS) - Full-Stack Developer

## Support

For issues, bugs, or feature requests, please create an issue on GitHub.

## Changelog

See [CHANGELOG.md](./CHANGELOG.md) for version history.

---

**Status**: Early Development
**Last Updated**: 2025-12-07
**Target Device**: Amazfit Balance Round (API Level 3.0+)
