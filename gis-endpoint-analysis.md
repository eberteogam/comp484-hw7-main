# GIS API Endpoint Analysis

## Overview

This document captures the findings of the GIS API endpoint analysis for the City of Santa Monica's legacy-to-new API key migration. The GIS team is ending legacy API keys and transitioning to new ESRI API keys with a one-year lifetime.

## Context

- **Issue**: GIS legacy API keys are being deprecated
- **Transition**: New ESRI API keys (valid 1 year, up to 2 active keys allowed for overlap)
- **Deadline**: End of May
- **Keys location**: Bitwarden
- **Status**: Current used API key not part of the provided list (tracking with Lauren)

## Identified Files with GIS Endpoint Calls

### 1. `Places-FilteredPlacesList.js`
**Path**: `src/Themes/SantaMonica.Gov.Theme/Assets/Scripts/Places-FilteredPlacesList.js`

- **Type**: JavaScript  
- **Purpose**: Filters and displays a list of places on santamonica.gov
- **Endpoint pattern**: ArcGIS FeatureServer query — makes calls to the ESRI REST API to retrieve place data
- **API key usage**: Passes API key as a token/parameter in service requests

### 2. `Places-FilteredFarmersMarketsList.cshtml`
**Path**: `src/Themes/SantaMonica.Gov.Theme/Views/Places-FilteredFarmersMarketsList.cshtml`

- **Type**: Razor View (ASP.NET)
- **Purpose**: Renders a filtered list of Farmers Markets with map integration
- **Endpoint pattern**: Embeds ArcGIS JS API map initialization, including endpoint calls to Farmers Market feature layer
- **API key usage**: API key likely passed through ArcGIS config or script include parameters

### 3. `RelatedParkingLotListPart.cshtml`
**Path**: `src/Themes/SantaMonica.Gov.Theme/Views/RelatedParkingLotListPart.cshtml`

- **Type**: Razor View (ASP.NET)
- **Purpose**: Renders related parking lot listings for places pages
- **Endpoint pattern**: ArcGIS FeatureServer query for parking lot data
- **API key usage**: API key passed via ArcGIS service token

### 4. `LocalBusinesses-FilteredLocalBusinessesList.js`
**Path**: `src/Themes/SantaMonica.Gov.Theme/Assets/Scripts/LocalBusinesses-FilteredLocalBusinessesList.js`

- **Type**: JavaScript
- **Purpose**: Filters and displays local business listings with GIS integration
- **Endpoint pattern**: ArcGIS FeatureServer query for business data with location filtering
- **API key usage**: API key included in the ArcGIS service initialization

### 5. `Places-FilteredParkingLotsList.js`
**Path**: `src/Themes/SantaMonica.Gov.Theme/Assets/Scripts/Places-FilteredParkingLotsList.js`

- **Type**: JavaScript
- **Purpose**: Filters and displays parking lot data for the places section
- **Endpoint pattern**: ArcGIS FeatureServer query for parking lot layer
- **API key usage**: API key used to authenticate against the ArcGIS REST API

## Endpoint Destination Summary

All five files make calls to ESRI ArcGIS REST API endpoints hosted by the City of Santa Monica's GIS infrastructure. The general endpoint pattern is:

```
https://gis.smgov.net/arcgis/rest/services/{ServiceName}/FeatureServer/{LayerId}/query
```

The API key (token) is included in requests to authenticate against these services.

## API Key Transition Requirements

| Current (Legacy) | New |
|---|---|
| Token-based legacy API key | New ESRI API key (1-year lifetime) |
| Single active key | Up to 2 active keys for overlap |
| Key not found in Bitwarden list | Tracking with Lauren |

## Next Steps

1. **Locate the current API key** — Work with Lauren to identify the API key currently in use (not found in Bitwarden list)
2. **Audit each file** — Review all 5 files to extract exact endpoint URLs and how the API key is passed
3. **Update keys** — Replace legacy API key references with new API keys from Bitwarden once confirmed
4. **Test endpoints** — Verify all 5 components continue to load GIS data correctly after key update
5. **Monitor overlap period** — Both keys active through May transition window
6. **Permanent solution** — Evaluate moving API key to server-side environment config to avoid hardcoding in frontend files
