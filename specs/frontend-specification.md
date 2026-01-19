# Weather Application - Mobile Front-End Specification (React Native)

## 1. Technology Stack

### 1.1 Core Technologies

| Technology | Version | Purpose |
|------------|---------|---------|
| React Native | 0.73.x | Cross-platform mobile framework |
| TypeScript | 5.x | Type safety |
| Expo | SDK 50 | Development platform & services |
| React Navigation | 6.x | Navigation library |
| React Query | 5.x | Server state management |
| Zustand | 4.x | Client state management |

### 1.2 UI Libraries

| Library | Purpose |
|---------|---------|
| React Native Reanimated | Animations |
| React Native Gesture Handler | Touch interactions |
| React Native Maps | Map integration |
| React Native SVG | Icon rendering |
| NativeWind | Tailwind-style styling |

### 1.3 Platform Services

| Service | Purpose |
|---------|---------|
| Expo Location | Geolocation |
| Expo Notifications | Push notifications |
| Expo SecureStore | Secure credential storage |
| AsyncStorage | Local data persistence |

### 1.4 Development Tools

| Tool | Purpose |
|------|---------|
| ESLint | Code linting |
| Prettier | Code formatting |
| Jest | Unit testing |
| Detox | E2E testing |
| Flipper | Debugging |

---

## 2. Application Architecture

### 2.1 Directory Structure

```
src/
├── app/                    # Expo Router screens
│   ├── (tabs)/            # Tab navigation screens
│   │   ├── index.tsx      # Weather dashboard
│   │   ├── hourly.tsx     # Hourly forecast
│   │   ├── daily.tsx      # 10-day forecast
│   │   ├── maps.tsx       # Radar/maps
│   │   └── settings.tsx   # Settings
│   ├── location/          # Location screens
│   ├── alerts/            # Alert screens
│   └── _layout.tsx        # Root layout
├── components/
│   ├── common/            # Shared components
│   ├── weather/           # Weather components
│   ├── maps/              # Map components
│   └── charts/            # Chart components
├── hooks/                  # Custom React hooks
├── lib/                    # Utility libraries
├── services/              # API service layer
├── stores/                # Zustand stores
├── types/                 # TypeScript types
└── constants/             # App constants
```

### 2.2 Navigation Structure

```
App Navigation:

TabNavigator (Bottom Tabs)
├── WeatherStack
│   ├── Dashboard (Home)
│   ├── HourlyDetail
│   ├── DailyDetail
│   └── DayDetail
├── MapsStack
│   ├── RadarMap
│   ├── SatelliteMap
│   └── AlertsMap
├── AlertsStack
│   ├── AlertsList
│   └── AlertDetail
└── SettingsStack
    ├── Settings
    ├── Locations
    ├── Notifications
    ├── Units
    └── Account

Modal Screens (Overlay):
├── LocationSearch
├── LocationPicker
└── AlertDetail
```

---

## 3. Screen Specifications

### 3.1 Weather Dashboard (Home)

**Tab:** Home
**Route:** `/(tabs)/`

**Components:**
- `LocationHeader` - Current location with search button
- `AlertBanner` - Active weather alerts
- `CurrentConditionsCard` - Primary weather display
- `HourlyPreview` - Horizontal scroll of next 12 hours
- `DailyPreview` - Next 5 days compact view
- `WeatherDetailsGrid` - UV, humidity, wind, pressure
- `AirQualityCard` - AQI display
- `SunMoonCard` - Sunrise/sunset, moon phase

**Layout:**
```
┌─────────────────────────────────────┐
│  San Francisco, CA        [Search]  │
├─────────────────────────────────────┤
│ ⚠️ Winter Storm Warning      [View] │
├─────────────────────────────────────┤
│                                     │
│         58°F                        │
│      ☁️ Partly Cloudy              │
│      H: 62° L: 48°                 │
│                                     │
├─────────────────────────────────────┤
│ Hourly                     See All >│
│ ┌────┬────┬────┬────┬────┬────┐    │
│ │Now │1PM │2PM │3PM │4PM │5PM │    │
│ │58° │60° │61° │60° │58° │56° │    │
│ └────┴────┴────┴────┴────┴────┘    │
├─────────────────────────────────────┤
│ 10-Day Forecast            See All >│
│ Today      ☀️  62°/48°    10%      │
│ Tomorrow   🌧️  58°/45°    60%      │
│ Wednesday  ⛅  55°/42°    20%      │
├─────────────────────────────────────┤
│ ┌─────────┬─────────┬─────────┐    │
│ │ UV: 3   │ Humid   │ Wind    │    │
│ │Moderate │  68%    │ 12 mph  │    │
│ └─────────┴─────────┴─────────┘    │
├─────────────────────────────────────┤
│ Air Quality: Good (42)              │
├─────────────────────────────────────┤
│ ☀️ 7:15 AM    🌙 5:25 PM           │
└─────────────────────────────────────┘
│  🏠    📊    🗺️    ⚠️    ⚙️  │
└─────────────────────────────────────┘
```

### 3.2 Hourly Forecast Screen

**Route:** `/(tabs)/hourly`

**Components:**
- `HourlyList` - Full 48-hour list
- `TemperatureChart` - Line chart of temps
- `PrecipitationBars` - Precipitation probability
- `HourDetailSheet` - Bottom sheet for hour details

**Features:**
- Pull-to-refresh
- Sticky header with current hour highlighted
- Tap hour for detailed bottom sheet
- Toggle between list and chart view

### 3.3 10-Day Forecast Screen

**Route:** `/(tabs)/daily`

**Components:**
- `DailyList` - Full 10-day expandable list
- `TemperatureTrendChart` - 10-day temp range graph
- `DayDetailSheet` - Bottom sheet for day details

**Features:**
- Expandable day cards (tap to expand)
- Visual temp range bars
- Precipitation probability indicators
- Day/night forecasts

### 3.4 Radar/Maps Screen

**Route:** `/(tabs)/maps`

**Components:**
- `MapView` - Full-screen map (react-native-maps)
- `RadarOverlay` - Animated radar tiles
- `LayerSelector` - Bottom sheet layer controls
- `AnimationControls` - Play/pause, timeline scrubber
- `LocationButton` - Center on user location

**Map Layers:**
- Radar (default)
- Satellite
- Temperature
- Alerts
- Wind

**Features:**
- Pinch to zoom
- Double-tap to zoom in
- Two-finger tap to zoom out
- Radar animation loop (2 hours)
- Play/pause controls
- Speed control (0.5x, 1x, 2x)

### 3.5 Alerts Screen

**Route:** `/alerts`

**Components:**
- `AlertsList` - Active alerts for location
- `AlertCard` - Alert summary card
- `AlertDetailScreen` - Full alert details
- `AlertMap` - Map with alert polygons

### 3.6 Settings Screen

**Route:** `/(tabs)/settings`

**Sections:**
- **Locations** - Manage saved locations
- **Units** - Temperature, wind, pressure, precipitation
- **Notifications** - Alert preferences
- **Appearance** - Theme (light/dark/system)
- **Account** - Login/signup/profile
- **About** - Version, licenses, feedback

---

## 4. Component Specifications

### 4.1 CurrentConditionsCard

```typescript
interface CurrentConditionsProps {
  temperature: number;
  feelsLike: number;
  conditions: string;
  icon: WeatherIconType;
  high: number;
  low: number;
  humidity: number;
  windSpeed: number;
  windDirection: string;
  pressure: number;
  uvIndex: number;
  lastUpdated: Date;
}
```

**Visual Specs:**
- Temperature: 72pt, bold
- Icon: 100x100pt
- Conditions: 20pt, medium
- High/Low: 16pt, regular

### 4.2 HourlyForecastItem

```typescript
interface HourlyItemProps {
  time: Date;
  temperature: number;
  icon: WeatherIconType;
  precipChance: number;
  isNow?: boolean;
}
```

**Visual Specs:**
- Item width: 64pt
- Time: 14pt
- Temperature: 18pt, semibold
- Icon: 32x32pt
- Precip: 12pt, blue

### 4.3 DailyForecastItem

```typescript
interface DailyItemProps {
  date: Date;
  dayName: string;
  icon: WeatherIconType;
  high: number;
  low: number;
  precipChance: number;
  expanded?: boolean;
  onPress?: () => void;
}
```

### 4.4 AlertBanner

```typescript
interface AlertBannerProps {
  alerts: WeatherAlert[];
  onPress: () => void;
}
```

**Color Mapping:**
- Warning: Red (#DC2626)
- Watch: Orange (#F97316)
- Advisory: Yellow (#EAB308)

---

## 5. Design System

### 5.1 Color Palette

```typescript
const colors = {
  // Primary
  primary: {
    50: '#EEF2FF',
    500: '#6366F1',
    600: '#4F46E5',
  },

  // Temperature gradient
  temp: {
    freezing: '#60A5FA',  // < 32°F
    cold: '#38BDF8',      // 32-50°F
    cool: '#34D399',      // 50-65°F
    mild: '#FCD34D',      // 65-75°F
    warm: '#FB923C',      // 75-85°F
    hot: '#F87171',       // 85-95°F
    extreme: '#DC2626',   // > 95°F
  },

  // Alerts
  alert: {
    warning: '#DC2626',
    watch: '#F97316',
    advisory: '#EAB308',
  },

  // AQI
  aqi: {
    good: '#22C55E',
    moderate: '#EAB308',
    sensitive: '#F97316',
    unhealthy: '#EF4444',
    veryUnhealthy: '#8B5CF6',
    hazardous: '#7F1D1D',
  },

  // Neutral
  gray: {
    50: '#F9FAFB',
    100: '#F3F4F6',
    500: '#6B7280',
    900: '#111827',
  },
};
```

### 5.2 Typography

```typescript
const typography = {
  // Headings
  h1: { fontSize: 32, fontWeight: '700', lineHeight: 40 },
  h2: { fontSize: 24, fontWeight: '600', lineHeight: 32 },
  h3: { fontSize: 20, fontWeight: '600', lineHeight: 28 },

  // Body
  bodyLarge: { fontSize: 18, fontWeight: '400', lineHeight: 28 },
  body: { fontSize: 16, fontWeight: '400', lineHeight: 24 },
  bodySmall: { fontSize: 14, fontWeight: '400', lineHeight: 20 },

  // Temperature display
  tempLarge: { fontSize: 72, fontWeight: '300', lineHeight: 80 },
  tempMedium: { fontSize: 24, fontWeight: '600', lineHeight: 32 },
  tempSmall: { fontSize: 18, fontWeight: '500', lineHeight: 24 },
};
```

### 5.3 Spacing

```typescript
const spacing = {
  xs: 4,
  sm: 8,
  md: 12,
  lg: 16,
  xl: 20,
  '2xl': 24,
  '3xl': 32,
  '4xl': 40,
};
```

### 5.4 Border Radius

```typescript
const borderRadius = {
  sm: 4,
  md: 8,
  lg: 12,
  xl: 16,
  full: 9999,
};
```

---

## 6. Weather Icons

### 6.1 Icon Set (SF Symbols / Custom SVG)

| Condition | Icon Name | SF Symbol |
|-----------|-----------|-----------|
| Clear Day | sun.max.fill | ☀️ |
| Clear Night | moon.fill | 🌙 |
| Partly Cloudy Day | cloud.sun.fill | ⛅ |
| Partly Cloudy Night | cloud.moon.fill | 🌙☁️ |
| Cloudy | cloud.fill | ☁️ |
| Light Rain | cloud.drizzle.fill | 🌧️ |
| Rain | cloud.rain.fill | 🌧️ |
| Heavy Rain | cloud.heavyrain.fill | 🌧️ |
| Thunderstorm | cloud.bolt.fill | ⛈️ |
| Snow | snowflake | ❄️ |
| Fog | cloud.fog.fill | 🌫️ |
| Wind | wind | 💨 |

### 6.2 Icon Sizes

| Context | Size (pt) |
|---------|-----------|
| Current Conditions | 100 |
| Daily Forecast | 40 |
| Hourly Forecast | 32 |
| Compact | 24 |

---

## 7. Gestures and Interactions

### 7.1 Standard Gestures

| Gesture | Action |
|---------|--------|
| Pull down | Refresh data |
| Swipe left/right | Navigate hourly items |
| Tap | Select/expand item |
| Long press | Show options menu |
| Pinch | Zoom map |
| Double tap | Zoom in map |

### 7.2 Haptic Feedback

| Event | Feedback Type |
|-------|---------------|
| Refresh complete | Light impact |
| Alert received | Warning notification |
| Location selected | Selection changed |
| Error | Error notification |

### 7.3 Animations

```typescript
// Standard durations
const animations = {
  fast: 150,      // Quick feedback
  normal: 250,    // Standard transitions
  slow: 350,      // Complex animations
};

// Common animations
- Card expand/collapse: spring animation
- Screen transitions: slide from right
- Modal: slide from bottom
- Refresh spinner: continuous rotation
- Radar loop: frame-based animation
```

---

## 8. Offline Support

### 8.1 Cached Data

| Data Type | Cache Duration | Storage |
|-----------|----------------|---------|
| Current conditions | 30 min | AsyncStorage |
| Hourly forecast | 1 hour | AsyncStorage |
| Daily forecast | 6 hours | AsyncStorage |
| Radar tiles | 15 min | File system |
| Saved locations | Permanent | AsyncStorage |
| User preferences | Permanent | SecureStore |

### 8.2 Offline UI States

```
Online:
  - Normal display
  - Background refresh

Offline with cached data:
  - Show cached data
  - "Offline - showing cached data" banner
  - Disable refresh (gray out)
  - Show last updated time

Offline without cached data:
  - "No internet connection" screen
  - Retry button
  - Show saved locations only
```

---

## 9. Push Notifications

### 9.1 Notification Types

| Type | Priority | Sound |
|------|----------|-------|
| Tornado Warning | Critical | Alert |
| Severe Thunderstorm Warning | High | Alert |
| Flash Flood Warning | High | Alert |
| Winter Storm Warning | Default | Default |
| Heat Advisory | Default | Default |
| Daily Forecast | Low | None |

### 9.2 Notification Payload

```typescript
interface WeatherNotification {
  type: 'alert' | 'forecast' | 'pws';
  title: string;
  body: string;
  data: {
    alertId?: string;
    locationId?: string;
    action?: string;
  };
}
```

---

## 10. Performance Targets

### 10.1 Startup Performance

| Metric | Target |
|--------|--------|
| Cold start | < 2 seconds |
| Warm start | < 500ms |
| Time to interactive | < 3 seconds |

### 10.2 Runtime Performance

| Metric | Target |
|--------|--------|
| Frame rate | 60 fps |
| JS thread | < 16ms per frame |
| Memory usage | < 150MB |
| Map pan/zoom | Smooth 60fps |

### 10.3 Network Performance

| Metric | Target |
|--------|--------|
| API response display | < 1 second |
| Radar tile load | < 500ms per tile |
| Image load | Progressive |

---

## 11. Accessibility

### 11.1 Requirements

- VoiceOver (iOS) / TalkBack (Android) support
- Dynamic type support (iOS)
- Minimum touch target: 44x44pt
- Color contrast: 4.5:1 minimum
- Reduce motion support
- Screen reader announcements for weather updates

### 11.2 Accessibility Labels

```typescript
// Example accessibility props
<CurrentConditionsCard
  accessible={true}
  accessibilityLabel={`Current temperature ${temp} degrees, ${conditions}. High of ${high}, low of ${low}.`}
  accessibilityRole="summary"
/>
```

---

## 12. Platform-Specific Considerations

### 12.1 iOS Specific

- Support iOS 14+
- Home screen widgets (WidgetKit via Expo)
- Apple Watch complication (future)
- Live Activities for alerts (iOS 16.1+)
- Dynamic Island support (iPhone 14 Pro+)

### 12.2 Android Specific

- Support Android 10+ (API 29)
- Home screen widgets
- Notification channels for alert types
- Adaptive icons
- Material You theming (Android 12+)

---

## 13. App Store Requirements

### 13.1 iOS App Store

- App screenshots (6.7", 6.5", 5.5")
- Privacy policy URL
- App privacy details (data collection)
- Location usage description
- Notification usage description

### 13.2 Google Play Store

- Feature graphic (1024x500)
- Screenshots (phone, tablet)
- Privacy policy URL
- Data safety section
- Target API level 34+

---

*Document Version: 2.0 (Mobile)*
*Last Updated: January 2026*
