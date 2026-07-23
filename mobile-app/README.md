# HMS Patient Mobile App (React Native)

Cross-platform patient mobile application built with React Native and Expo.

## Overview

Native iOS and Android mobile application for hospital patients to manage appointments, view medical records, and communicate with healthcare providers. Built with React Native and Expo for seamless cross-platform development.

## Features

### Authentication

- Patient registration with profile creation
- Secure login with JWT tokens
- Password reset via email
- Biometric authentication (Touch ID / Face ID) - Optional
- Secure token storage with Expo SecureStore

### Appointment Management

- View available doctors with specializations
- Check doctor availability and time slots
- Book appointments with real-time slot validation
- View upcoming appointments with status tracking
- Edit appointments before scheduled time
- Cancel appointments with cancellation reason
- Push notifications for appointment reminders

### Medical Records

- View medical records (finalized records only)
- Access by appointment — Link records to visits
- Read-only access — Secure viewing of diagnosis and prescriptions
- Download/Share — Export records as PDF (future)

### Profile Management

- Update personal information (name, phone, email)
- Change password securely
- Emergency contact management
- Medical history updates

### Notifications

- Appointment reminders — 24h and 1h before appointment
- Appointment confirmations — Booking success
- Status updates — Appointment cancellations

## Architecture

```text
React Native (Expo) App
    │
    ├── Screens (UI Pages)
    ├── Components (Reusable UI)
    ├── Services (API calls)
    ├── Store (Zustand state management)
    ├── Navigation (Expo Router)
    └── Utils (Helper functions)
```

## Folder Structure

```text
mobile-app/
├── src/
│   ├── app/                # Expo Router pages
│   │   ├── (auth)/         # Auth screens (login, register)
│   │   ├── (tabs)/         # Tab navigation screens
│   │   └── _layout.tsx     # Root layout
│   ├── components/
│   │   ├── appointment/    # Appointment components
│   │   ├── common/         # Shared UI components
│   │   └── medical-record/ # Medical record components
│   ├── screens/
│   │   ├── auth/           # Authentication screens
│   │   ├── home/           # Home/Dashboard
│   │   ├── landing/        # Onboarding screens
│   │   └── patient/        # Patient profile screens
│   ├── services/           # API service layer
│   ├── store/              # Zustand stores
│   ├── hooks/              # Custom React hooks
│   ├── constants/          # Constants, enums
│   ├── config/             # App configuration
│   ├── lib/                # Third-party integrations
│   ├── styles/             # Global styles
│   └── utils/              # Utility functions
├── assets/                 # Images, fonts, icons
├── app.json                # Expo configuration
├── package.json
└── README.md
```

## Quick Start

### Prerequisites

- Node.js 14+
- Expo CLI
- iOS Simulator (Mac)
- Android Emulator
- Expo Go app (for physical device testing)

### Installation

```bash
# Install Expo CLI globally
npm install -g expo-cli

# Install dependencies
npm install
```

### Environment Configuration

Create a `.env` file in the `mobile-app/` folder:

```env
EXPO_PUBLIC_API_URL=http://192.168.1.100:5000/api
EXPO_PUBLIC_APP_NAME=HMS Patient App
EXPO_PUBLIC_APP_VERSION=1.0.0
```

**Note:** Replace `192.168.1.100` with your computer's local IP address. Mobile devices cannot access localhost directly.

### Run Development Server

```bash
npm start

# or

npx expo start
```

Scan the QR code using:

- iOS: Camera app (requires Expo Go installed)
- Android: Expo Go app

### Run on Simulator/Emulator

```bash
# iOS Simulator (Mac only)
npm run ios

# Android Emulator
npm run android
```

## App Navigation

```text
Landing Screen
    ↓
Login / Register
    ↓
Bottom Tab Navigator
    ├── Home (Dashboard)
    ├── Appointments (List + Book)
    ├── Medical Records
    └── Profile (Settings)
```

## Default Test Credentials

### Patient Login

- Email: `patient@example.com`
- Password: `Patient@123`

Register new patients through the app or create them through the web dashboard.

## UI/UX Features

### Design System

- Clean modern UI — Material Design inspired
- Consistent spacing — 4px base grid
- Color palette — Hospital-friendly blues and whites
- Typography — Inter font family
- Icons — Expo Vector Icons

### Components

- Custom buttons — Primary, secondary, outline variants
- Form inputs — Text, email, password, date picker
- Cards — Appointment cards, medical record cards
- Bottom sheets — Modals and action sheets
- Loading states — Skeletons and spinners
- Empty states — Friendly illustrations

### Animations

- Page transitions — Smooth slide animations
- Micro-interactions — Button press feedback
- Skeleton loaders — Content loading placeholders

## Security Features

### Token Storage

- Secure Storage — Expo SecureStore for JWT tokens
- Auto-refresh — Refresh tokens on expiry
- Logout on error — Auto-logout on 401 errors

### Data Protection

- HTTPS only — Encrypted API communication
- Input validation — Client-side validation
- Secure forms — Password masking and autocomplete disabled

### Privacy

- No sensitive data caching — Clear on logout
- Biometric authentication — Optional face/fingerprint unlock
- Session timeout — Auto-logout after inactivity

## State Management (Zustand)

```typescript
// src/store/authStore.ts
import create from 'zustand';

export const useAuthStore = create((set) => ({
  user: null,
  token: null,
  setUser: (user) => set({ user }),
  setToken: (token) => set({ token }),
  logout: () => set({ user: null, token: null })
}));
```

## API Integration

### Service Layer Pattern

```typescript
// src/services/appointmentService.ts
import api from './api';

export const appointmentService = {
  getAppointments: () => api.get('/patient-self/appointments'),
  bookAppointment: (data) => api.post('/patient-self/appointments', data),
  cancelAppointment: (id, reason) =>
    api.put(`/patient-self/appointments/${id}/cancel`, { reason })
};
```

### Axios Instance

```typescript
// src/services/api.ts
import axios from 'axios';
import { getToken } from '../utils/storage';

const api = axios.create({
  baseURL: process.env.EXPO_PUBLIC_API_URL,
  timeout: 10000
});

api.interceptors.request.use(async (config) => {
  const token = await getToken();

  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }

  return config;
});

export default api;
```

## Testing

```bash
# Run tests
npm test

# Run tests with coverage
npm run test:coverage

# Run specific test
npm test -- AppointmentScreen.test.tsx
```

## Building for Production

### Build APK (Android)

```bash
# Build APK
eas build --platform android --profile preview

# Build AAB for Play Store
eas build --platform android --profile production
```

### Build IPA (iOS)

```bash
# Build for iOS Simulator
eas build --platform ios --profile preview

# Build for App Store
eas build --platform ios --profile production
```

### Setup EAS Build

```bash
# Install EAS CLI
npm install -g eas-cli

# Login to Expo
eas login

# Configure build
eas build:configure
```

## Deployment

### Expo Updates (OTA)

```bash
# Publish update to production
eas update --branch production

# Publish update to staging
eas update --branch staging
```

### App Store Submission

1. Build IPA with `eas build --platform ios --profile production`
2. Download IPA from Expo dashboard
3. Upload to App Store Connect via Transporter
4. Submit for review

### Play Store Submission

1. Build AAB with `eas build --platform android --profile production`
2. Download AAB from Expo dashboard
3. Upload to Google Play Console
4. Submit for review

## Performance Optimizations

- FlatList optimization — `windowSize`, `maxToRenderPerBatch`
- Image optimization — Expo Image with caching
- Lazy loading — Code splitting with React.lazy
- Memoization — useMemo, useCallback for expensive operations
- Bundle size — ~25MB (Expo managed workflow)

## Tech Stack

- Framework: React Native + Expo SDK 49+
- Language: JavaScript / TypeScript
- Navigation: Expo Router (file-based routing)
- State Management: Zustand
- HTTP Client: Axios
- Storage: Expo SecureStore, AsyncStorage
- UI Components: Custom + React Native Paper (optional)
- Icons: Expo Vector Icons
- Forms: React Hook Form (optional)
- Notifications: Expo Notifications

## Scripts

```bash
npm start            # Start Expo dev server
npm run android      # Run on Android emulator
npm run ios          # Run on iOS simulator
npm test             # Run tests
npm run lint         # Lint code
npm run format       # Format code
eas build            # Build production app
eas update           # Publish OTA update
```

## Configuration

### app.json

```json
{
  "expo": {
    "name": "HMS Patient App",
    "slug": "hms-patient-app",
    "version": "1.0.0",
    "orientation": "portrait",
    "icon": "./assets/icon.png",
    "splash": {
      "image": "./assets/splash.png",
      "resizeMode": "contain",
      "backgroundColor": "#ffffff"
    },
    "assetBundlePatterns": ["**/*"],
    "ios": {
      "supportsTablet": true,
      "bundleIdentifier": "com.hospital.patientapp"
    },
    "android": {
      "adaptiveIcon": {
        "foregroundImage": "./assets/adaptive-icon.png",
        "backgroundColor": "#FFFFFF"
      },
      "package": "com.hospital.patientapp"
    }
  }
}
```

## Troubleshooting

### Metro Bundler Issues

```bash
# Clear cache
npx expo start --clear

# Reset cache completely
rm -rf node_modules

npm install

npx expo start --clear
```

### Cannot Connect to API

```bash
# Use local IP address, not localhost

# Windows
ipconfig

# Mac/Linux
ifconfig

# Update .env
EXPO_PUBLIC_API_URL=http://YOUR_LOCAL_IP:5000/api
```

### Build Errors

```bash
# Clear Expo cache
expo prebuild --clean

# Reinstall dependencies
rm -rf node_modules package-lock.json

npm install
```

## Supported Platforms

- iOS 13.0+
- Android API 21+ (Android 5.0+)

## Future Enhancements

- Telemedicine (Video consultations)
- Chat with doctors
- Prescription reminders
- Health metrics tracking (BP, glucose, etc.)
- Lab report uploads
- Multi-language support
- Offline mode
- Apple Health / Google Fit integration

## Documentation

- ../README.md
- ../ARCHITECTURE.md
- ../API_DOCUMENTATION.md
- ../backend/DATABASE_SCHEMA.md
- ../backend/README.md
- ../web-frontend/README.md

## License

MIT

## Author

**Varshith Jakkula**

GitHub:  
https://github.com/Varshith6690

Portfolio:  
https://varshith-portfolio-self.vercel.app/

LinkedIn:  
https://www.linkedin.com/in/varshith-jakkula-34145a273/

Email:  
21r21a6690@gmail.com

## Acknowledgments

Built using Expo and React Native.