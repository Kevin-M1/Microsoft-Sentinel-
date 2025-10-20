# Entra ID (Azure) Authentication Success Geo-Map — Sentinel Workbook + KQL

This repo documents exactly what I did:
- Built a **Microsoft Sentinel Workbook** to visualize **successful Entra ID (Azure AD) authentications** on a **world map**.
- Wrote a **KQL** query over `SigninLogs` to count successful logins per **Identity** and **geolocation** (City/Country/Latitude/Longitude).
- Displayed results both as a **map** and a **table**.

## Screenshots
- **Workbook world map**
  <img width="1861" height="890" alt="image" src="https://github.com/user-attachments/assets/9fb6bfeb-b344-40c3-908a-468f3710f874" />


- **KQL results table**
<img width="2134" height="1131" alt="image" src="https://github.com/user-attachments/assets/22c9e34d-4bd5-4ef4-b07b-cca84e30b610" />


## Environment
- **Platform**: Microsoft Sentinel (Azure)
- **Data source**: `SigninLogs`
- **Workbook**: *Entra ID (Azure) Authentication Success*
- **Time range**: *Last 24 hours*

## KQL Used
```kusto
SigninLogs
| where ResultType == 0                                   // Success
| summarize LoginCount = count() by
    Identity,
    Latitude  = tostring(LocationDetails["geoCoordinates"]["latitude"]),
    Longitude = tostring(LocationDetails["geoCoordinates"]["longitude"]),
    City      = tostring(LocationDetails["city"]),
    Country   = tostring(LocationDetails["countryOrRegion"])
| project Identity, Latitude, Longitude, City, Country, LoginCount,
         friendly_label = strcat(Identity, " - ", City, ", ", Country)
```

## Workbook Map Settings
- **Latitude**: `Latitude`
- **Longitude**: `Longitude`
- **Metric/Size**: `LoginCount`
- **Label**: `friendly_label` (or `Identity`)
- **Row limit**: first 100 rows (for quick testing)

