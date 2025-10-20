# Entra ID (Azure) Authentication Success Geo-Map — Sentinel Workbook + KQL

This repo documents exactly what I did:
- Built a **Microsoft Sentinel Workbook** to visualize **successful Entra ID (Azure AD) authentications** on a **world map**.
- Wrote a **KQL** query over `SigninLogs` to count successful logins per **Identity** and **geolocation** (City/Country/Latitude/Longitude).
- Displayed results both as a **map** and a **table**.

## Screenshots
- **Workbook world map**
  ![Entra ID Authentication Success Geo Map](images/entra-workbook-map.png)

- **KQL results table**
  ![KQL table output](images/kql-table.png)

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

