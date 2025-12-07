# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2025-12-07

### Added

- Initial release of Zepp OS Productivity App for Amazfit Balance Round
- Multi-question alarm system with up to 6 alarms per day
- Question management: add, edit, delete up to 30 questions
- Customizable questions with point values (-100 to +100)
- Points tracking and points-based redemption system
- Affirmations display after alarm completion (30 seconds)
- Continuous vibration during alarm (stops on answer or 2-minute timeout)
- Flexible alarm scheduling with repeat day selection
- Smooth, non-overlapping UI with consistent color theme
- Local data persistence (no cloud sync)
- Background alarm service for reliable scheduling
- Complete documentation (README, SETUP, BUILD, ARCHITECTURE)
- Project structure following Zepp OS V3 best practices
- Device App with 9 pages for comprehensive functionality
- App Service for background alarm management
- Utility modules for storage, vibration, time, and validation
- Type definitions for all data models
- Theme constants and color palette
- GitHub repository with comprehensive setup

### Technical Details

- **Target Device**: Amazfit Balance Round (480x480 square display)
- **API Level**: V3
- **Framework**: Zeus CLI with Node.js
- **Language**: JavaScript ES6+
- **State Management**: Local storage only
- **Architecture**: Device App + Background Service

### Features by Module

#### Pages
- Home Dashboard with statistics and quick access
- Add Question form with validation
- Add Alarm interface with time and repeat settings
- Manage Alarms list with enable/disable toggles
- Manage Questions list with full CRUD operations
- Alarm Response page for answering questions
- Affirmation Display with auto-timeout
- Points Summary with tracking and redemption
- Settings page for configuration

#### Services
- Background Alarm Service for reliable scheduling
- Time monitoring and alarm triggering
- Vibration control with configurable patterns
- Question randomization and shuffling

#### Utilities
- Local storage management (CRUD operations)
- Vibration patterns and control
- Time utilities for scheduling
- Random selection algorithms
- Input validation functions

### Limitations

- No cloud synchronization (local storage only)
- No companion app (watch-only functionality)
- Alarms work while app is installed (may need device settings)
- Maximum 30 questions total
- Maximum 6 alarms per day
- Background service available only on API Level 3.0+

### Known Issues

None at release.

---

## Future Versions

Planned features for upcoming releases:

- Cloud backup and sync
- Companion app on phone
- Advanced statistics and analytics
- Custom vibration patterns
- Voice feedback integration
- Widget support
- Multi-language support
- Dark mode theme option
- Performance optimizations
- Extended question categories

---

For detailed version information, see [BUILD.md](./BUILD.md).
