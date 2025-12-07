# Architecture and Design

## System Overview

```
┌────────────────────────┐
│   Zepp OS Device (Amazfit Balance)    │
├────────────────────────┤
│                                      │
│  ┌──────────────────┐  │
│  │  Device App (UI Layer)      │  │
│  ├──────────────────┤  │
│  │ Pages                      │  │
│  │ - Home/Dashboard           │  │
│  │ - Add Question             │  │
│  │ - Add Alarm                │  │
│  │ - Manage Alarms            │  │
│  │ - Manage Questions         │  │
│  │ - Alarm Response           │  │
│  │ - Affirmation Display      │  │
│  │ - Points Summary           │  │
│  │ - Settings                 │  │
│  └──────────────────┘  │
│                                      │
│  ┌──────────────────┐  │
│  │  App Service (Background)  │  │
│  ├──────────────────┤  │
│  │ - Alarm Manager           │  │
│  │ - Vibration Control       │  │
│  │ - Time Tracking           │  │
│  │ - Notification Handler    │  │
│  └──────────────────┘  │
│                                      │
│  ┌──────────────────┐  │
│  │  Local Storage (Device)    │  │
│  ├──────────────────┤  │
│  │ - Questions Store         │  │
│  │ - Alarms Store            │  │
│  │ - Affirmations Store      │  │
│  │ - User Points Record      │  │
│  │ - Settings/Preferences    │  │
│  └──────────────────┘  │
│                                      │
└────────────────────────┘
```

## Data Flow

### Alarm Creation Flow

1. User navigates to "Add Alarm" page
2. User enters: time, repeat days, question count
3. Page validates input (time format, question count 5-30)
4. Alarm object created with unique ID
5. Alarm saved to device storage
6. Background service notified of new alarm
7. User returns to home page

### Alarm Trigger Flow

1. Device system time matches alarm time
2. App Service receives system event
3. App Service validates alarm is enabled
4. Random question selection from library
5. App Service initiates vibration (continuous)
6. Device App launched to alarm-response page
7. User sees selected questions
8. User answers each question
9. Points calculated and recorded
10. Vibration stops
11. Random affirmation displayed for 30 seconds
12. App returns to home page

### Points Management Flow

1. User answers a question correctly/incorrectly
2. Points value retrieved from question data
3. Points added/subtracted from total
4. Points record updated in storage
5. Points summary page shows updated total
6. User can initiate redemption (minimum 5,000 points)
7. Points deducted upon redemption

## Modules

### Pages Module (`src/pages/`)

Each page is a standalone interface:

#### index.ts - Home Dashboard
- Display user statistics
- Show next scheduled alarm
- Quick access to main features
- Display current total points

#### add-question.ts - Add Question Interface
- Form to enter question text
- Points input field (-100 to +100)
- Validation before save
- Success confirmation

#### add-alarm.ts - Create New Alarm
- Time picker (hour, minute)
- Repeat day selector (checkboxes)
- Question count selector (5-30)
- Validation (max 6 alarms per day)
- Save and return to home

#### manage-alarms.ts - View/Edit Alarms
- List of all alarms
- Enable/disable toggle for each
- Edit or delete options
- Sort by time

#### manage-questions.ts - Manage Questions
- List all questions
- Delete individual questions
- View points value for each
- Search/filter functionality

#### alarm-response.ts - Alarm Question Interface
- Display each question one at a time
- Yes/No answer buttons
- Progress indicator (X of Y questions)
- Submit button after all questions
- 2-minute timeout with vibration

#### affirmation-display.ts - Show Affirmation
- Random affirmation from library
- Large readable text
- 30-second auto-timeout
- Tap to skip to home

#### points-summary.ts - Points Tracking
- Total points display
- Points history (last 10 sessions)
- Redemption form (minimum 5,000)
- Confirmation dialog for redemption

#### settings.ts - App Settings
- Enable/disable app notifications
- Vibration intensity settings
- Data reset confirmation
- About and version info

### Services Module (`src/services/`)

#### alarm-service.ts - Background Alarm Service

Handles background operations:
- Time monitoring
- Alarm triggering at scheduled times
- Vibration control during alarm
- Timeout management (2 minutes)
- Question selection and shuffle
- Communication with Device App

### Utilities Module (`src/utils/`)

#### storage.ts - Local Storage Management

```typescript
interface StorageKey {
  questions: 'questions';
  alarms: 'alarms';
  affirmations: 'affirmations';
  userPoints: 'userPoints';
  settings: 'settings';
}

// Functions
save(key: StorageKey, data: any): void
load(key: StorageKey): any
remove(key: StorageKey): void
clear(): void
```

#### vibration.ts - Vibration Control

```typescript
// Vibration patterns
startAlarmVibration(): void      // Continuous vibration for alarm
stopVibration(): void             // Stop current vibration
successVibration(): void          // Short success vibration
errorVibration(): void            // Error/wrong answer vibration
```

#### time-utils.ts - Time and Scheduling

```typescript
// Time formatting
formatTime(hour: number, minute: number): string
getCurrentHour(): number
getCurrentMinute(): number
getCurrentTime(): { hour: number; minute: number }

// Alarm scheduling
getAlarmTimeInMs(hour: number, minute: number): number
getTimeUntilAlarm(alarmTime: { h: number; m: number }): number
shouldholdAlarmNow(alarmTime: { h: number; m: number }): boolean
```

#### random.ts - Random Selection

```typescript
// Random question selection
selectRandomQuestions(allQuestions: Question[], count: number): Question[]
getRandomAffirmation(affirmations: Affirmation[]): Affirmation
shuffle<T>(array: T[]): T[]  // Fisher-Yates shuffle
```

#### validators.ts - Input Validation

```typescript
// Validation functions
isValidTime(hour: number, minute: number): boolean
isValidQuestionCount(count: number): boolean
isValidPointsValue(points: number): boolean
isValidRedemptionAmount(amount: number): boolean
isValidAlarmCount(existingAlarms: Alarm[]): boolean
```

### Types Module (`src/types/`)

TypeScript type definitions:

```typescript
interface Question {
  id: string;
  text: string;
  points: number;
  createdAt: number;
}

interface Alarm {
  id: string;
  hour: number;
  minute: number;
  enabled: boolean;
  repeatDays: boolean[];  // [Sun, Mon, Tue, Wed, Thu, Fri, Sat]
  questionCount: number;  // 5-30
  createdAt: number;
}

interface Affirmation {
  id: string;
  text: string;
  createdAt: number;
}

interface UserPoints {
  total: number;
  sessions: SessionRecord[];
}

interface SessionRecord {
  id: string;
  alarmId: string;
  points: number;
  timestamp: number;
  questionCount: number;
}

interface AppSettings {
  notificationsEnabled: boolean;
  vibrationIntensity: 'low' | 'medium' | 'high';
  affirmationsEnabled: boolean;
}
```

### Styles Module (`src/styles/`)

#### constants.ts - Theme and Colors

```typescript
// Color palette
const COLORS = {
  primary: '#2B8DAD',      // Teal
  secondary: '#FF6B6B',    // Red
  success: '#2ECC71',      // Green
  warning: '#F39C12',      // Orange
  background: '#F5F7FA',   // Light gray
  surface: '#FFFFFF',      // White
  text: '#2C3E50',         // Dark gray
  textLight: '#7F8C8D',    // Medium gray
};

// Typography
const FONT_SIZES = {
  xs: 12,
  sm: 14,
  base: 16,
  lg: 18,
  xl: 24,
  xxl: 32,
};

// Spacing
const SPACING = {
  xs: 4,
  sm: 8,
  md: 12,
  lg: 16,
  xl: 24,
  xxl: 32,
};
```

## API Limitations

### App Service Limitations

The background service (`app-service`) has limited API access:

**Available in App Service:**
- `@zos/notification` - Send notifications
- `@zos/sensor` - Low-power sensors (Time, HeartRate)
- `@zos/media` - Audio playback
- `@zos/ble` - Bluetooth communication
- `@zos/app-service` - Service management
- `@zos/fs` - File system operations

**NOT Available in App Service:**
- `@zos/ui` - UI components (no visual output)
- `@zos/settings` (write operations)
- Timer functions (`setTimeout`, `setInterval`)
- High-power sensors

## Configuration Constants

```typescript
// Limits
const MAX_QUESTIONS = 30;
const MAX_ALARMS_PER_DAY = 6;
const MIN_QUESTIONS_PER_ALARM = 5;
const MAX_QUESTIONS_PER_ALARM = 30;

// Timings
const ALARM_TIMEOUT_SECONDS = 120;  // 2 minutes
const AFFIRMATION_DISPLAY_SECONDS = 30;
const VIBRATION_INTENSITY = 'VIBRATOR_SCENE_STRONG_REMINDER';

// Points
const MIN_REDEMPTION_POINTS = 5000;
const MAX_POINTS_PER_QUESTION = 100;
const MIN_POINTS_PER_QUESTION = -100;

// Storage
const STORAGE_KEY_QUESTIONS = 'questions';
const STORAGE_KEY_ALARMS = 'alarms';
const STORAGE_KEY_AFFIRMATIONS = 'affirmations';
const STORAGE_KEY_USER_POINTS = 'userPoints';
const STORAGE_KEY_SETTINGS = 'settings';
```

## State Management

No external state management library is used. Instead:

1. **Device App**: State stored in page variables and objects
2. **App Service**: State maintained during service lifetime
3. **Persistent State**: Saved to device storage (JSON files)
4. **Communication**: Page to service via message events

## Performance Considerations

1. **UI Responsiveness**: All heavy operations in background service
2. **Memory Usage**: Limit loaded data; stream large datasets
3. **Battery**: Minimize vibration time; use efficient timers
4. **Storage**: Compress old session data; archive history

## Security Considerations

1. **Data Privacy**: No cloud sync (local storage only)
2. **Input Validation**: All user inputs validated
3. **No External API Calls**: No network communication
4. **Permissions**: Minimal required permissions

---

For implementation details, see source code in `src/`.
For setup and building, see [SETUP.md](./SETUP.md) and [BUILD.md](./BUILD.md).
