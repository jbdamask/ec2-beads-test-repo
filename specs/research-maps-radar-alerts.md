# Weather Underground Maps, Radar, and Alerts Research

## Overview

This document provides comprehensive research on Weather Underground's (wunderground.com) maps, radar, and severe weather alert features. Weather Underground, owned by The Weather Company (an IBM Business), offers one of the most comprehensive consumer weather platforms with extensive mapping and alert capabilities.

**Note:** This research is based on documented features and patterns from Weather Underground's platform. For the most current feature set, direct API documentation review is recommended.

---

## 1. Interactive Weather Maps

### 1.1 Map Types Available

Weather Underground provides multiple map categories accessible through their maps interface:

#### Radar Maps
- **Current Radar** (`/maps/radar/current`) - Real-time precipitation radar
- **Regional Radar** - Zoomed views for specific regions
- **National Radar Mosaic** - Combined NEXRAD radar imagery
- **Future Radar** - Predictive radar showing expected precipitation movement

#### Satellite Maps
- **Visible Satellite** - Daytime cloud imagery
- **Infrared Satellite** - Temperature-based cloud detection (day/night)
- **Water Vapor** - Atmospheric moisture visualization
- **Enhanced Satellite** - Color-enhanced cloud imagery

#### Temperature Maps
- **Current Temperature** - Real-time temperature readings
- **Feels Like Temperature** - Heat index/wind chill adjusted
- **Temperature Anomaly** - Deviation from normal
- **High/Low Temperature Forecasts** - Daily extremes

#### Precipitation Maps
- **Current Precipitation** - Real-time rain/snow
- **Precipitation Forecast** - Expected accumulation
- **Probability of Precipitation** - Chance percentages
- **Precipitation Type** - Rain vs. snow vs. mix

#### Wind Maps
- **Current Wind Speed** - Real-time wind readings
- **Wind Gusts** - Peak wind speeds
- **Wind Direction** - Animated wind flow visualization
- **Jet Stream** - Upper-level wind patterns

#### Specialty Maps
- **Air Quality Index (AQI)** - Pollution levels
- **UV Index** - Solar radiation intensity
- **Humidity** - Relative humidity levels
- **Dew Point** - Moisture comfort indicator
- **Pressure** - Barometric pressure patterns
- **Cloud Cover** - Percentage cloud coverage

### 1.2 WunderMap Features

The WunderMap (`/wundermap`) is Weather Underground's flagship interactive mapping tool:

#### Available Layers (Toggleable)
- Radar (multiple intensity options)
- Satellite imagery
- Temperature overlay
- Weather stations (PWS network)
- Webcams
- Severe weather alerts
- Hurricane/tropical tracks
- Lightning strikes
- Snow depth
- Air quality
- Traffic conditions
- Wildfire hotspots

#### Data Sources
- NEXRAD radar network (NOAA)
- GOES satellite imagery
- Personal Weather Stations (PWS) network
- NWS weather alerts
- Third-party data providers

---

## 2. Radar Features

### 2.1 Core Radar Capabilities

#### Animation Controls
- **Play/Pause** - Start/stop radar loop animation
- **Frame-by-frame** - Step through individual radar scans
- **Speed Control** - Adjust animation playback speed
- **Loop Length** - Configurable time range (1-6 hours typical)
- **Future Forecast** - Predictive radar (1-2 hours ahead)

#### Time Controls
- **Historical Playback** - View past radar data
- **Time Slider** - Scrub through radar timeline
- **Timestamp Display** - Current frame time indicator
- **Auto-refresh** - Automatic data updates (typically 5-10 minutes)

#### Zoom Levels
- **National View** - Full CONUS radar mosaic
- **Regional View** - Multi-state coverage
- **Local View** - County-level detail
- **Street Level** - Maximum zoom for precise location
- **Pinch-to-zoom** - Touch device support
- **Mouse wheel zoom** - Desktop interaction

### 2.2 Radar Products

#### Base Reflectivity
- Standard precipitation intensity display
- Color-coded by dBZ (decibel relative to Z)
- Range: -30 to 75+ dBZ
- Color scale: Light blue (light) to purple/white (extreme)

#### Composite Reflectivity
- Maximum reflectivity from all elevation angles
- Better detection of elevated precipitation

#### Velocity Data
- Doppler velocity showing storm motion
- Red/green color scheme (toward/away from radar)
- Rotation detection for severe weather

#### Precipitation Classification
- Rain, snow, mixed, freezing rain indicators
- Algorithm-based precipitation type

### 2.3 Enhanced Radar Features

#### High-Resolution Radar
- Enhanced resolution in select markets
- More detailed precipitation structure
- Better urban area coverage

#### Dual-Pol Radar Data
- Correlation coefficient
- Differential reflectivity
- Specific differential phase
- Better precipitation type detection

#### Storm Tracking
- Cell identification
- Storm motion vectors
- Projected storm paths
- Hail indicators
- Tornado vortex signatures (TVS)

---

## 3. Severe Weather Alerts

### 3.1 Alert Types

Weather Underground displays all National Weather Service (NWS) alert products:

#### Watch Categories
- **Tornado Watch** - Conditions favorable for tornadoes
- **Severe Thunderstorm Watch** - Conditions favorable for severe storms
- **Flash Flood Watch** - Conditions favorable for flash flooding
- **Winter Storm Watch** - Significant winter weather possible
- **Hurricane Watch** - Hurricane conditions possible within 48 hours
- **Fire Weather Watch** - Critical fire weather conditions possible

#### Warning Categories
- **Tornado Warning** - Tornado detected or imminent
- **Severe Thunderstorm Warning** - Severe storm detected
- **Flash Flood Warning** - Flash flooding occurring or imminent
- **Flood Warning** - Flooding occurring or expected
- **Winter Storm Warning** - Significant winter weather expected
- **Blizzard Warning** - Blizzard conditions expected
- **Ice Storm Warning** - Significant icing expected
- **Hurricane Warning** - Hurricane conditions expected within 36 hours
- **Tropical Storm Warning** - Tropical storm conditions expected
- **Extreme Wind Warning** - Extreme sustained winds
- **Storm Surge Warning** - Life-threatening storm surge

#### Advisory Categories
- **Wind Advisory** - Sustained winds 25-39 mph
- **Heat Advisory** - Heat index 100-104F
- **Frost Advisory** - Temperatures 33-36F expected
- **Dense Fog Advisory** - Visibility below 1/4 mile
- **Winter Weather Advisory** - Light winter precipitation
- **Coastal Flood Advisory** - Minor coastal flooding

#### Special Statements
- **Special Weather Statement** - Noteworthy weather not requiring watch/warning
- **Severe Weather Statement** - Updates on ongoing severe weather
- **Short Term Forecast** - Detailed 6-hour forecast

### 3.2 Alert Display Features

#### Visual Presentation
- **Color-coded polygons** - Alert areas shown on maps
- **County/zone highlighting** - Affected areas shaded
- **Alert severity indicators** - Warning (red), Watch (yellow), Advisory (blue)
- **Alert list panel** - Sortable active alerts list
- **Alert detail overlay** - Full text on click/tap

#### Alert Information Displayed
- Alert type and severity
- Affected areas (counties/zones)
- Issuance time
- Expiration time
- Issuing NWS office
- Full alert text
- Hazards expected
- Recommended actions
- Update history

#### Alert Filtering
- Filter by alert type
- Filter by severity level
- Filter by geographic area
- Show/hide expired alerts
- Custom alert preferences

### 3.3 Alert Notification Systems

#### Web Notifications
- Browser push notifications
- Desktop notifications (with permission)
- Alert banners on site

#### Mobile Notifications
- Push notifications to app
- Customizable alert types
- Location-based alerts
- Multiple location monitoring

#### Email Alerts
- Daily forecast emails
- Severe weather email alerts
- Customizable thresholds

---

## 4. Hurricane/Tropical Storm Tracker

### 4.1 Hurricane Tracking Features

#### Active Storm Display
- Current storm position
- Storm category (Saffir-Simpson scale)
- Maximum sustained winds
- Central pressure
- Storm motion (speed and direction)
- Size (radius of tropical storm/hurricane force winds)

#### Track Visualization
- **Official Track** - NHC forecast track
- **Track History** - Past positions (6-hour intervals)
- **Cone of Uncertainty** - Probable track area
- **Wind Speed Probabilities** - Percentage chance of specific wind speeds
- **Storm Surge Zones** - Potential surge inundation areas
- **Watch/Warning Areas** - Coastal alert zones

### 4.2 Hurricane Data Products

#### Forecast Information
- 5-day track forecast
- Intensity forecast
- Size forecast
- Rainfall forecast
- Storm surge forecast
- Timing of impacts

#### Historical Data
- Historical storm tracks
- Storm archive by year
- Comparison to similar past storms
- Climate statistics

#### Model Guidance
- Spaghetti models (multiple model tracks)
- Ensemble spread visualization
- Model comparison
- Intensity model guidance

### 4.3 Tropical Outlook

- 7-day tropical weather outlook
- Invest areas (potential development)
- Formation probability percentages
- Basin-wide view (Atlantic, Eastern Pacific, Central Pacific)

---

## 5. Winter Storm Tracking

### 5.1 Winter Weather Maps

#### Snow Maps
- **Current Snow Depth** - Ground snow coverage
- **Snowfall Forecast** - Expected accumulation
- **Snowfall Totals** - Storm total accumulation
- **Snow Water Equivalent** - Water content of snow

#### Ice Maps
- **Ice Accumulation Forecast** - Expected ice thickness
- **Freezing Rain Zones** - Areas of freezing precipitation

#### Temperature Maps
- **Surface Temperature** - Current readings
- **Road Temperature** - Pavement conditions
- **Wind Chill** - Feels-like temperature

### 5.2 Winter Storm Features

#### Storm Tracking
- Winter storm position and movement
- Precipitation type boundaries
- Deformation zone identification
- Lake effect snow bands

#### Forecast Products
- Snowfall accumulation maps
- Hour-by-hour snowfall rates
- Snow level elevation
- Timing of precipitation changes

#### Impact Assessment
- Travel impact ratings
- School closure likelihood
- Power outage risk zones

---

## 6. Lightning Detection

### 6.1 Lightning Map Features

#### Real-Time Display
- Live lightning strike locations
- Cloud-to-ground vs. cloud-to-cloud
- Strike density visualization
- Time-decay display (fading older strikes)

#### Historical Data
- Lightning strike totals by area
- Storm cell lightning rates
- Lightning trend analysis

### 6.2 Lightning Data

#### Strike Information
- Timestamp
- Location (lat/long)
- Polarity (+/-)
- Peak current
- Multiplicity (stroke count)

#### Lightning Alerts
- Lightning proximity alerts
- Strike rate thresholds
- Custom radius settings

---

## 7. Wildfire and Smoke Maps

### 7.1 Fire Detection

#### Active Fire Display
- Satellite-detected hotspots
- Fire perimeter mapping
- Fire intensity indicators
- Containment status

#### Fire Weather
- Red flag warnings display
- Fire danger ratings
- Wind/humidity conditions

### 7.2 Smoke and Air Quality

#### Smoke Visualization
- Satellite smoke detection
- Smoke forecast models
- Particulate matter concentration

#### Air Quality Mapping
- AQI index values
- PM2.5 concentration
- Ozone levels
- Health impact categories

---

## 8. Map Controls and Customization

### 8.1 Interactive Controls

#### Navigation
- **Pan** - Click and drag to move map
- **Zoom** - Mouse wheel, buttons, pinch gesture
- **Zoom to Location** - Enter address or coordinates
- **Current Location** - GPS/IP-based positioning
- **Full Screen** - Expanded view mode

#### Layer Controls
- **Layer Toggle** - Show/hide individual layers
- **Opacity Slider** - Adjust layer transparency
- **Layer Order** - Stack order customization
- **Legend Display** - Color scale reference

### 8.2 Map Settings

#### Display Options
- **Base Map Style** - Street, satellite, terrain, dark mode
- **Units** - Imperial/metric toggle
- **Time Zone** - Local vs. UTC display
- **Animation Speed** - Playback rate adjustment
- **Auto-refresh** - Enable/disable automatic updates

#### Location Settings
- **Default Location** - Home location setting
- **Saved Locations** - Multiple favorite locations
- **Recent Locations** - Location history

### 8.3 Sharing Features

- **Direct Link** - Shareable URL with current view
- **Embed Code** - Website embedding
- **Social Sharing** - Facebook, Twitter integration
- **Screenshot/Export** - Image download capability

---

## 9. Technical Patterns and API Insights

### 9.1 Data Update Frequencies

| Data Type | Update Interval |
|-----------|-----------------|
| Radar | 5-10 minutes |
| Satellite | 15-30 minutes |
| Surface Observations | 5-15 minutes |
| Forecasts | 1-6 hours |
| Alerts | Real-time (minutes) |
| Lightning | Real-time (seconds) |
| Air Quality | 1 hour |

### 9.2 Observed URL Patterns

```
/maps/radar/current          - Current radar view
/maps/radar/regional/{region} - Regional radar
/maps/satellite/{type}       - Satellite imagery
/maps/temperature/current    - Temperature map
/maps/precipitation/forecast - Precipitation forecast
/severe                      - Severe weather center
/severe/us/{state}           - State severe weather
/hurricane                   - Hurricane center
/hurricane/{basin}           - Basin-specific (atlantic, pacific)
/hurricane/{year}/{storm}    - Specific storm tracking
/wundermap                   - Interactive WunderMap
```

### 9.3 API/Data Patterns

#### Tile-Based Mapping
- Uses standard map tile servers (z/x/y format)
- Multiple tile layers for different data types
- WebGL rendering for smooth animations

#### Data Formats Observed
- GeoJSON for geographic features
- PNG/WebP for raster tiles
- JSON for station data and metadata

#### Authentication Patterns
- API key for external access
- Session-based for web interface
- Rate limiting on public endpoints

### 9.4 Third-Party Integrations

- Google Maps/Mapbox for base maps
- ESRI for GIS functionality
- Various tile servers for weather data

---

## 10. Feature Summary for Implementation

### 10.1 Core Map Features to Implement

#### Must-Have (P0)
1. Radar maps with animation
2. Current conditions map
3. Severe weather alerts display
4. Hurricane/tropical tracker (seasonal)
5. Basic layer controls

#### Should-Have (P1)
1. Multiple radar products
2. Satellite imagery
3. Temperature/precipitation maps
4. Alert notifications
5. Location search and saving

#### Nice-to-Have (P2)
1. Lightning detection
2. Air quality/smoke maps
3. Winter storm specific maps
4. Model comparison (spaghetti plots)
5. Historical data access

### 10.2 Key User Interactions

1. **View radar animation** - Play/pause, speed control
2. **Toggle layers** - Show/hide data types
3. **Check alerts** - View active warnings for location
4. **Track storms** - Follow severe weather cells
5. **Track hurricanes** - Monitor tropical systems
6. **Set notifications** - Configure alert preferences
7. **Share maps** - Generate shareable links

### 10.3 Data Requirements

#### Real-Time Data Feeds
- NEXRAD radar data
- GOES satellite imagery
- NWS alert feed (CAP format)
- Lightning detection network
- Surface observation network
- PWS network data (optional)

#### Forecast Data
- NWP model output
- NHC tropical products
- WPC precipitation forecasts
- SPC severe weather outlooks

### 10.4 Technical Recommendations

1. **Use tile-based architecture** for scalable map rendering
2. **Implement caching layer** for radar/satellite tiles
3. **WebSocket connections** for real-time alerts
4. **Progressive loading** for large datasets
5. **Offline support** for basic functionality
6. **Responsive design** for all device sizes

---

## 11. Competitive Features Analysis

### 11.1 Unique Weather Underground Features

1. **Personal Weather Station Network** - Largest PWS network globally
2. **Hyperlocal data** - Block-level conditions from PWS
3. **Historical data depth** - Extensive climate archives
4. **Community features** - User photos, reports
5. **Educational content** - Weather explanations

### 11.2 Areas for Differentiation

1. Better mobile experience
2. Faster load times
3. More intuitive layer controls
4. Improved alert customization
5. Enhanced accessibility features
6. Offline functionality
7. Better international coverage

---

## Appendix A: Alert Color Codes

| Alert Type | Color | Hex Code |
|------------|-------|----------|
| Tornado Warning | Red | #FF0000 |
| Severe Thunderstorm Warning | Orange | #FFA500 |
| Flash Flood Warning | Dark Red | #8B0000 |
| Tornado Watch | Yellow | #FFFF00 |
| Severe Thunderstorm Watch | Blue | #00BFFF |
| Winter Storm Warning | Pink | #FF69B4 |
| Blizzard Warning | Red-Orange | #FF4500 |
| Hurricane Warning | Red | #DC143C |
| Hurricane Watch | Magenta | #FF00FF |
| Tropical Storm Warning | Blue | #00CED1 |
| Heat Advisory | Orange | #FF7F50 |
| Wind Advisory | Tan | #D2B48C |

---

## Appendix B: Saffir-Simpson Hurricane Scale Reference

| Category | Wind Speed (mph) | Storm Surge (ft) | Damage |
|----------|------------------|------------------|--------|
| TD | < 39 | - | Minimal |
| TS | 39-73 | - | Minimal |
| 1 | 74-95 | 4-5 | Minimal |
| 2 | 96-110 | 6-8 | Moderate |
| 3 | 111-129 | 9-12 | Extensive |
| 4 | 130-156 | 13-18 | Extreme |
| 5 | > 157 | > 18 | Catastrophic |

---

## Appendix C: Radar Color Scale (dBZ)

| dBZ Range | Color | Precipitation Type |
|-----------|-------|-------------------|
| < 5 | Light Gray | Very light/none |
| 5-10 | Light Blue | Light mist |
| 10-20 | Green | Light rain |
| 20-30 | Yellow | Moderate rain |
| 30-40 | Orange | Heavy rain |
| 40-50 | Red | Very heavy rain |
| 50-60 | Purple | Extreme rain/hail |
| > 60 | White/Pink | Large hail possible |

---

*Document compiled for weather application development reference.*
*Last updated: January 2026*
*Source: Weather Underground (wunderground.com) feature analysis*
