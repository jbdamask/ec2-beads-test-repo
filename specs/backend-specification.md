# Weather Application - Back-End Specification

## 1. Technology Stack

### 1.1 Core Technologies

| Technology | Version | Purpose |
|------------|---------|---------|
| Node.js | 20.x LTS | Runtime environment |
| TypeScript | 5.x | Type safety |
| Fastify | 4.x | HTTP framework |
| PostgreSQL | 16.x | Primary database |
| Redis | 7.x | Caching layer |
| TimescaleDB | Extension | Time-series data |

### 1.2 Infrastructure

| Service | Purpose |
|---------|---------|
| AWS/GCP/Azure | Cloud hosting |
| Docker | Containerization |
| Kubernetes | Orchestration |
| CloudFront/CDN | Static asset delivery |
| S3/GCS | Object storage |

### 1.3 Message Queue

| Technology | Purpose |
|------------|---------|
| Redis Streams | Real-time alert processing |
| Bull | Job queue for background tasks |

---

## 2. System Architecture

### 2.1 High-Level Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                      Load Balancer                           │
└─────────────────────────┬───────────────────────────────────┘
                          │
┌─────────────────────────▼───────────────────────────────────┐
│                     API Gateway                              │
│    (Rate Limiting, Auth, Request Routing)                    │
└─────────────────────────┬───────────────────────────────────┘
                          │
        ┌─────────────────┼─────────────────┐
        │                 │                 │
┌───────▼───────┐ ┌───────▼───────┐ ┌───────▼───────┐
│   Weather     │ │    Maps       │ │   Alerts      │
│   Service     │ │   Service     │ │   Service     │
└───────┬───────┘ └───────┬───────┘ └───────┬───────┘
        │                 │                 │
        └────────┬────────┴────────┬────────┘
                 │                 │
         ┌───────▼───────┐ ┌───────▼───────┐
         │    Redis      │ │  PostgreSQL   │
         │   (Cache)     │ │  (Primary DB) │
         └───────────────┘ └───────────────┘
                 │
         ┌───────▼───────┐
         │  External     │
         │  Weather APIs │
         └───────────────┘
```

### 2.2 Service Breakdown

| Service | Responsibility |
|---------|----------------|
| Weather Service | Current conditions, forecasts |
| Maps Service | Radar tiles, map data |
| Alerts Service | Weather alerts, notifications |
| User Service | Authentication, preferences |
| PWS Service | Personal weather station data |
| Location Service | Geocoding, location search |

---

## 3. API Specification

### 3.1 Base URL Structure

```
Production: https://api.clearsky.weather/v1
Staging: https://api.staging.clearsky.weather/v1
```

### 3.2 Authentication

```
Header: Authorization: Bearer <jwt_token>
API Key: X-API-Key: <api_key>
```

### 3.3 Weather Endpoints

#### Get Current Conditions

```
GET /weather/current
```

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| lat | number | Yes* | Latitude |
| lon | number | Yes* | Longitude |
| location | string | Yes* | Location query (city, ZIP) |
| units | string | No | imperial/metric (default: imperial) |

*Either lat/lon or location required

**Response:**
```json
{
  "location": {
    "name": "San Francisco",
    "state": "CA",
    "country": "US",
    "lat": 37.7749,
    "lon": -122.4194,
    "timezone": "America/Los_Angeles"
  },
  "current": {
    "observationTime": "2026-01-19T12:00:00Z",
    "temperature": 58,
    "feelsLike": 55,
    "humidity": 72,
    "dewPoint": 49,
    "pressure": 30.12,
    "pressureTrend": "rising",
    "windSpeed": 12,
    "windGust": 18,
    "windDirection": 270,
    "windDirectionCardinal": "W",
    "visibility": 10,
    "uvIndex": 3,
    "uvDescription": "Moderate",
    "cloudCover": 45,
    "conditions": "Partly Cloudy",
    "icon": "partly-cloudy-day"
  },
  "source": {
    "type": "official",
    "station": "KSFO",
    "distance": 2.5
  }
}
```

#### Get Hourly Forecast

```
GET /weather/hourly
```

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| lat | number | Yes* | Latitude |
| lon | number | Yes* | Longitude |
| location | string | Yes* | Location query |
| hours | number | No | Hours to return (default: 48, max: 168) |
| units | string | No | imperial/metric |

**Response:**
```json
{
  "location": { /* ... */ },
  "hourly": [
    {
      "time": "2026-01-19T13:00:00Z",
      "temperature": 60,
      "feelsLike": 58,
      "humidity": 68,
      "precipChance": 20,
      "precipAmount": 0,
      "precipType": null,
      "windSpeed": 10,
      "windDirection": 280,
      "windDirectionCardinal": "W",
      "uvIndex": 4,
      "cloudCover": 40,
      "conditions": "Partly Cloudy",
      "icon": "partly-cloudy-day"
    }
    // ... more hours
  ]
}
```

#### Get Daily Forecast

```
GET /weather/daily
```

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| lat | number | Yes* | Latitude |
| lon | number | Yes* | Longitude |
| location | string | Yes* | Location query |
| days | number | No | Days to return (default: 10, max: 15) |
| units | string | No | imperial/metric |

**Response:**
```json
{
  "location": { /* ... */ },
  "daily": [
    {
      "date": "2026-01-19",
      "sunrise": "07:15:00",
      "sunset": "17:25:00",
      "temperatureHigh": 62,
      "temperatureLow": 48,
      "humidity": 70,
      "precipChance": 30,
      "precipAmount": 0.1,
      "precipType": "rain",
      "windSpeed": 12,
      "windDirection": 270,
      "uvIndexMax": 5,
      "conditions": "Chance of Rain",
      "conditionsDay": "Partly cloudy with a chance of afternoon showers",
      "conditionsNight": "Mostly cloudy, lows in the upper 40s",
      "icon": "chance-rain"
    }
    // ... more days
  ]
}
```

### 3.4 Alert Endpoints

#### Get Active Alerts

```
GET /alerts/active
```

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| lat | number | Yes* | Latitude |
| lon | number | Yes* | Longitude |
| location | string | Yes* | Location query |
| radius | number | No | Search radius in miles (default: 25) |
| severity | string | No | Filter: warning,watch,advisory |
| type | string | No | Filter by alert type |

**Response:**
```json
{
  "location": { /* ... */ },
  "alerts": [
    {
      "id": "NWS-IDP-PROD-5234567",
      "type": "Winter Storm Warning",
      "severity": "warning",
      "certainty": "likely",
      "urgency": "expected",
      "headline": "Winter Storm Warning in effect until Monday",
      "description": "A winter storm will bring 6-10 inches of snow...",
      "instruction": "Travel should be restricted to emergencies only...",
      "areas": ["San Francisco County", "Marin County"],
      "polygon": [[lat, lon], ...],
      "onset": "2026-01-19T18:00:00Z",
      "expires": "2026-01-20T12:00:00Z",
      "sender": "NWS San Francisco",
      "references": []
    }
  ]
}
```

### 3.5 Maps Endpoints

#### Get Radar Tile

```
GET /maps/radar/{z}/{x}/{y}.png
```

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| time | string | No | ISO timestamp (default: latest) |
| product | string | No | base/composite (default: base) |

#### Get Radar Timestamps

```
GET /maps/radar/timestamps
```

**Response:**
```json
{
  "timestamps": [
    "2026-01-19T12:00:00Z",
    "2026-01-19T11:55:00Z",
    "2026-01-19T11:50:00Z"
    // ... last 2-3 hours
  ],
  "interval": 300,
  "latestTime": "2026-01-19T12:00:00Z"
}
```

### 3.6 Location Endpoints

#### Search Locations

```
GET /locations/search
```

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| q | string | Yes | Search query |
| limit | number | No | Max results (default: 10) |

**Response:**
```json
{
  "results": [
    {
      "id": "us-ca-san_francisco",
      "name": "San Francisco",
      "state": "CA",
      "country": "US",
      "lat": 37.7749,
      "lon": -122.4194,
      "type": "city"
    }
  ]
}
```

#### Geocode Coordinates

```
GET /locations/geocode
```

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| lat | number | Yes | Latitude |
| lon | number | Yes | Longitude |

### 3.7 Air Quality Endpoint

```
GET /airquality/current
```

**Response:**
```json
{
  "location": { /* ... */ },
  "aqi": {
    "value": 42,
    "category": "Good",
    "color": "#00E400",
    "primaryPollutant": "PM2.5",
    "pollutants": {
      "pm25": { "value": 10.2, "unit": "ug/m3", "aqi": 42 },
      "pm10": { "value": 18.5, "unit": "ug/m3", "aqi": 17 },
      "o3": { "value": 28, "unit": "ppb", "aqi": 23 },
      "no2": { "value": 12, "unit": "ppb", "aqi": 11 },
      "co": { "value": 0.3, "unit": "ppm", "aqi": 3 }
    },
    "healthRecommendation": "Air quality is satisfactory."
  }
}
```

### 3.8 Astronomy Endpoint

```
GET /astronomy
```

**Response:**
```json
{
  "location": { /* ... */ },
  "sun": {
    "sunrise": "07:15:00",
    "sunset": "17:25:00",
    "solarNoon": "12:20:00",
    "dayLength": "10:10:00",
    "civilTwilightBegin": "06:48:00",
    "civilTwilightEnd": "17:52:00"
  },
  "moon": {
    "moonrise": "10:30:00",
    "moonset": "22:15:00",
    "phase": "Waxing Gibbous",
    "illumination": 78,
    "age": 10.5
  }
}
```

---

## 4. Database Schema

### 4.1 PostgreSQL Tables

#### users

```sql
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255),
    name VARCHAR(255),
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW(),
    email_verified BOOLEAN DEFAULT FALSE,
    last_login TIMESTAMPTZ
);
```

#### user_preferences

```sql
CREATE TABLE user_preferences (
    user_id UUID PRIMARY KEY REFERENCES users(id),
    temperature_unit VARCHAR(10) DEFAULT 'fahrenheit',
    wind_unit VARCHAR(10) DEFAULT 'mph',
    pressure_unit VARCHAR(10) DEFAULT 'inHg',
    precipitation_unit VARCHAR(10) DEFAULT 'in',
    time_format VARCHAR(10) DEFAULT '12h',
    theme VARCHAR(20) DEFAULT 'system',
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW()
);
```

#### saved_locations

```sql
CREATE TABLE saved_locations (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id),
    name VARCHAR(255) NOT NULL,
    lat DECIMAL(10, 7) NOT NULL,
    lon DECIMAL(10, 7) NOT NULL,
    city VARCHAR(255),
    state VARCHAR(100),
    country VARCHAR(100),
    is_default BOOLEAN DEFAULT FALSE,
    sort_order INTEGER DEFAULT 0,
    created_at TIMESTAMPTZ DEFAULT NOW()
);
```

#### alert_subscriptions

```sql
CREATE TABLE alert_subscriptions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id),
    location_id UUID REFERENCES saved_locations(id),
    alert_types TEXT[],
    min_severity VARCHAR(20) DEFAULT 'advisory',
    push_enabled BOOLEAN DEFAULT TRUE,
    email_enabled BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMPTZ DEFAULT NOW()
);
```

### 4.2 TimescaleDB Hypertables (PWS Data)

#### pws_observations

```sql
CREATE TABLE pws_observations (
    time TIMESTAMPTZ NOT NULL,
    station_id VARCHAR(50) NOT NULL,
    temperature DECIMAL(5, 2),
    humidity DECIMAL(5, 2),
    pressure DECIMAL(7, 2),
    wind_speed DECIMAL(5, 2),
    wind_direction INTEGER,
    wind_gust DECIMAL(5, 2),
    rain_rate DECIMAL(7, 4),
    daily_rain DECIMAL(7, 4),
    uv_index DECIMAL(4, 2),
    solar_radiation DECIMAL(7, 2)
);

SELECT create_hypertable('pws_observations', 'time');

-- Compression policy
SELECT add_compression_policy('pws_observations', INTERVAL '7 days');

-- Retention policy
SELECT add_retention_policy('pws_observations', INTERVAL '2 years');
```

#### pws_stations

```sql
CREATE TABLE pws_stations (
    id VARCHAR(50) PRIMARY KEY,
    user_id UUID REFERENCES users(id),
    name VARCHAR(255),
    lat DECIMAL(10, 7) NOT NULL,
    lon DECIMAL(10, 7) NOT NULL,
    elevation DECIMAL(7, 2),
    hardware_type VARCHAR(100),
    software_type VARCHAR(100),
    is_active BOOLEAN DEFAULT TRUE,
    is_public BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    last_observation TIMESTAMPTZ
);
```

---

## 5. Caching Strategy

### 5.1 Redis Cache Patterns

| Data Type | TTL | Key Pattern |
|-----------|-----|-------------|
| Current conditions | 5 min | `weather:current:{lat}:{lon}` |
| Hourly forecast | 1 hr | `weather:hourly:{lat}:{lon}` |
| Daily forecast | 6 hr | `weather:daily:{lat}:{lon}` |
| Alerts | 1 min | `alerts:{lat}:{lon}:{radius}` |
| Radar timestamps | 1 min | `radar:timestamps` |
| Location search | 24 hr | `location:search:{query}` |
| Geocode | 30 days | `location:geocode:{lat}:{lon}` |

### 5.2 Cache Invalidation

```typescript
// Event-driven invalidation
interface CacheInvalidationEvent {
  type: 'weather_update' | 'alert_update' | 'radar_update';
  region?: GeoRegion;
  timestamp: Date;
}

// Invalidation patterns
redis.del(`weather:current:${lat}:${lon}`);
redis.keys('alerts:*').then(keys => redis.del(keys));
```

---

## 6. External Data Sources

### 6.1 Primary Weather Data

| Source | Data Type | Update Frequency |
|--------|-----------|------------------|
| National Weather Service (NWS) | Forecasts, Alerts | 1-6 hours |
| NOAA NDFD | Grid forecasts | 1 hour |
| METAR/TAF | Aviation weather | 1 hour |
| NWS CAP | Alerts | Real-time |

### 6.2 Radar Data

| Source | Data Type | Update Frequency |
|--------|-----------|------------------|
| NEXRAD (AWS) | Radar tiles | 5-10 minutes |
| MRMS | Multi-radar mosaic | 2 minutes |

### 6.3 Additional Data

| Source | Data Type | Update Frequency |
|--------|-----------|------------------|
| EPA AirNow | Air quality | 1 hour |
| USNO | Astronomy | Daily |
| NHC | Tropical | 6 hours |

### 6.4 Data Fetching Architecture

```typescript
// Data source adapter interface
interface WeatherDataSource {
  fetchCurrentConditions(lat: number, lon: number): Promise<CurrentConditions>;
  fetchHourlyForecast(lat: number, lon: number, hours: number): Promise<HourlyForecast[]>;
  fetchDailyForecast(lat: number, lon: number, days: number): Promise<DailyForecast[]>;
}

// Implement adapters for each source
class NWSDataSource implements WeatherDataSource { }
class OpenWeatherDataSource implements WeatherDataSource { }
```

---

## 7. Background Jobs

### 7.1 Job Types

| Job | Schedule | Description |
|-----|----------|-------------|
| Radar Tile Fetch | Every 5 min | Download latest radar |
| Alert Sync | Every 1 min | Sync NWS alerts |
| PWS Data Cleanup | Daily | Archive old PWS data |
| Cache Warmup | Every 5 min | Pre-cache popular locations |
| Email Digest | Daily 6am | Send daily forecasts |

### 7.2 Job Queue Configuration

```typescript
// Bull queue configuration
const weatherQueue = new Queue('weather', {
  redis: { host: 'redis', port: 6379 },
  defaultJobOptions: {
    removeOnComplete: 100,
    removeOnFail: 50,
    attempts: 3,
    backoff: { type: 'exponential', delay: 1000 }
  }
});

// Scheduled jobs
weatherQueue.add('fetch-radar', {}, { repeat: { cron: '*/5 * * * *' } });
weatherQueue.add('sync-alerts', {}, { repeat: { cron: '* * * * *' } });
```

---

## 8. Real-Time Features

### 8.1 WebSocket Events

```typescript
// Server-to-client events
interface ServerEvents {
  'alert:new': (alert: WeatherAlert) => void;
  'alert:update': (alert: WeatherAlert) => void;
  'alert:expire': (alertId: string) => void;
  'weather:update': (data: WeatherUpdate) => void;
  'radar:frame': (timestamp: string) => void;
}

// Client-to-server events
interface ClientEvents {
  'subscribe:location': (lat: number, lon: number) => void;
  'unsubscribe:location': (lat: number, lon: number) => void;
  'subscribe:alerts': (region: GeoRegion) => void;
}
```

### 8.2 Socket.IO Configuration

```typescript
const io = new Server(server, {
  cors: { origin: process.env.FRONTEND_URL },
  pingTimeout: 60000,
  pingInterval: 25000
});

// Room-based subscriptions
socket.join(`location:${lat}:${lon}`);
socket.join(`alerts:${state}`);
```

---

## 9. Error Handling

### 9.1 Error Response Format

```json
{
  "error": {
    "code": "LOCATION_NOT_FOUND",
    "message": "The specified location could not be found",
    "details": {
      "query": "asdfasdf"
    },
    "requestId": "req_abc123"
  }
}
```

### 9.2 Error Codes

| Code | HTTP Status | Description |
|------|-------------|-------------|
| VALIDATION_ERROR | 400 | Invalid request parameters |
| UNAUTHORIZED | 401 | Missing/invalid auth |
| FORBIDDEN | 403 | Insufficient permissions |
| LOCATION_NOT_FOUND | 404 | Location not found |
| RATE_LIMITED | 429 | Too many requests |
| UPSTREAM_ERROR | 502 | Weather API error |
| SERVICE_UNAVAILABLE | 503 | Service temporarily down |

---

## 10. Rate Limiting

### 10.1 Rate Limit Tiers

| Tier | Requests/min | Requests/day |
|------|--------------|--------------|
| Anonymous | 30 | 1,000 |
| Authenticated | 60 | 10,000 |
| Premium | 120 | 50,000 |
| API Developer | 300 | 100,000 |

### 10.2 Rate Limit Headers

```
X-RateLimit-Limit: 60
X-RateLimit-Remaining: 45
X-RateLimit-Reset: 1705671600
```

---

## 11. Security

### 11.1 Authentication

- JWT tokens with 1-hour expiry
- Refresh tokens with 30-day expiry
- OAuth2 support (Google, Apple)
- API key authentication for integrations

### 11.2 Security Headers

```
Strict-Transport-Security: max-age=31536000
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
Content-Security-Policy: default-src 'self'
```

### 11.3 Input Validation

```typescript
// Zod schema validation
const locationQuerySchema = z.object({
  lat: z.number().min(-90).max(90).optional(),
  lon: z.number().min(-180).max(180).optional(),
  location: z.string().max(200).optional(),
  units: z.enum(['imperial', 'metric']).optional()
}).refine(data => (data.lat && data.lon) || data.location);
```

---

## 12. Monitoring & Observability

### 12.1 Metrics

| Metric | Type | Description |
|--------|------|-------------|
| http_requests_total | Counter | Total HTTP requests |
| http_request_duration_seconds | Histogram | Request latency |
| cache_hit_ratio | Gauge | Cache hit percentage |
| external_api_latency | Histogram | Weather API latency |
| active_websocket_connections | Gauge | Current WS connections |

### 12.2 Logging

```typescript
// Structured logging format
{
  "level": "info",
  "message": "Weather data fetched",
  "timestamp": "2026-01-19T12:00:00Z",
  "requestId": "req_abc123",
  "service": "weather-service",
  "lat": 37.7749,
  "lon": -122.4194,
  "duration": 145
}
```

### 12.3 Health Checks

```
GET /health
GET /health/ready
GET /health/live
```

---

## 13. Deployment

### 13.1 Environment Variables

```bash
# Required
DATABASE_URL=postgresql://user:pass@host:5432/weather
REDIS_URL=redis://host:6379
NWS_API_KEY=xxx
JWT_SECRET=xxx

# Optional
LOG_LEVEL=info
PORT=3000
CORS_ORIGIN=https://clearsky.weather
```

### 13.2 Scaling Configuration

| Service | Min Replicas | Max Replicas | CPU Threshold |
|---------|--------------|--------------|---------------|
| API Gateway | 2 | 10 | 70% |
| Weather Service | 3 | 20 | 70% |
| Maps Service | 2 | 10 | 80% |
| Alerts Service | 2 | 5 | 70% |

---

*Document Version: 1.0*
*Last Updated: January 2026*
