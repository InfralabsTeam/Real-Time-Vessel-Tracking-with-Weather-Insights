# **Real-Time Vessel Tracking with Weather Insights**  

## **Background**
Maritime operations rely on accurate vessel tracking and weather data to ensure safe and efficient navigation. However, existing solutions often require separate systems for position tracking and weather monitoring, leading to inefficiencies. This application aims to seamlessly integrate both data sources in a sequential operation, ensuring that each vessel’s real-time position is always paired with its corresponding weather conditions.

## **Objective**  
Develop a **C# console application** that:  
- **Fetches** real-time vessel positions from **[aisstream.io](https://aisstream.io/)**.  
- **Immediately retrieves** weather data for each vessel position from **[OpenWeather API](https://openweathermap.org/)**.  
- **Stores both** vessel position and weather data in an **SQLite database**.  
- **Allows retrieval of location history** with weather conditions.  

---

## **Key Features**  

1. **Real-time Vessel Tracking (AIS Data)**  
   - Fetch real-time **vessel positions** using **WebSockets** from aisstream.io.  
   - Extract **MMSI, latitude, longitude, speed, course**.  

2. **Weather Data Fetching (Sequential)**  
   - For **each new vessel position**, immediately query **OpenWeather API**.  
   - Store **temperature, wind speed, and wave height** along with vessel data.  

3. **Data Storage (SQLite)**  
   - Save **each vessel position** and **its weather conditions** in **one transaction**.  

4. **Historical Data & Reporting**  
   - Retrieve past **positions with weather conditions**.  
   - Export **data to CSV** for analysis.  

---

## **Database Schema (SQLite)**  

### **Vessels Table**  
| Column     | Type    | Description |  
|------------|--------|-------------|  
| Id         | INTEGER (PK, AUTOINCREMENT) | Unique vessel ID |  
| MMSI       | TEXT (UNIQUE, NOT NULL) | Vessel's unique identifier |  
| Name       | TEXT | Vessel name |  
| Type       | TEXT | Vessel type |  

### **Positions Table**  
| Column     | Type    | Description |  
|------------|--------|-------------|  
| Id         | INTEGER (PK, AUTOINCREMENT) | Unique position ID |  
| VesselId   | INTEGER (FK, Vessels.Id, NOT NULL) | Vessel reference |  
| Latitude   | DECIMAL(10,6) | Vessel latitude |  
| Longitude  | DECIMAL(10,6) | Vessel longitude |  
| Speed      | DECIMAL(10,2) | Vessel speed in knots |  
| Course     | DECIMAL(10,2) | Vessel heading |  
| Temperature | DECIMAL(5,2) | Temperature at location |  
| WindSpeed  | DECIMAL(5,2) | Wind speed at location |  
| WaveHeight | DECIMAL(5,2) | Wave height at location |  
| Timestamp  | DATETIME (DEFAULT CURRENT_TIMESTAMP) | Time of data capture |  

---

## **Implementation Plan**  

### **1. Real-time Data Collection (AIS & Weather Sequentially)**  
**Step 1:** Connect to **aisstream.io** WebSocket to receive **live vessel data**.  
**Step 2:** Extract **MMSI, latitude, longitude, speed, and course**.  
**Step 3:** **Immediately call OpenWeather API** with the **latitude/longitude**.  
**Step 4:** Extract **temperature, wind speed, and wave height**.  
**Step 5:** Store **both vessel position and weather conditions** in **SQLite**.  
**Step 6:** Continue doing the operation for **3 mins** then ask the user if he wants to continue or to choose onother option.  

### **2. Reporting & Visualization**  
**Step 7:** Retrieve **historical positions and weather conditions** for any vessel.  
**Step 8:** Display **past weather conditions** at a given vessel's locations.  
**Step 9:** Export **data to CSV**.  

---

## **Example User Flow**  

```
Real-Time Vessel Tracking with Weather Data  

[1] Start Tracking Live Vessels for 3 mins 
[2] View Vessel History  
[3] Export Data to CSV  
[4] Exit  

Enter your choice: 1  

Connecting to aisstream.io...  
Receiving live vessel data...  

Vessel: Ocean Explorer, MMSI: 123456789  
   Latitude: 34.0522, Longitude: -118.2437, Speed: 14.2 knots  
   Fetching weather data...  
   Temperature: 21.4°C, Wind Speed: 10.5 knots, Wave Height: 1.0 meters  
Position and weather stored in database.  

Vessel: Sea Voyager, MMSI: 987654321  
   Latitude: 40.7128, Longitude: -74.0060, Speed: 12.7 knots  
   Fetching weather data...  
   Temperature: 25.3°C, Wind Speed: 8.2 knots, Wave Height: 0.8 meters  
Position and weather stored in database.  

-----------------------------------------------  
[1] Continue Tracking  
[2] View Vessel History  
[3] Export Data to CSV  
[4] Exit  

Enter your choice: 2  

View Vessel History  
Enter MMSI: 123456789  

History for Ocean Explorer  
Time: 2024-07-18 10:00  
   Latitude: 34.0522, Longitude: -118.2437, Speed: 14.2 knots  
   Temperature: 21.4°C, Wind Speed: 10.5 knots, Wave Height: 1.0 meters  

Time: 2024-07-18 11:00  
   Latitude: 35.0000, Longitude: -119.0000, Speed: 13.8 knots  
   Temperature: 22.0°C, Wind Speed: 9.8 knots, Wave Height: 1.2 meters  

-----------------------------------------------  
[1] Go Back  
[2] Export Data to CSV  
[3] Exit  

Enter your choice: 2  

Exporting data to CSV... Done!  
File saved as vessel_tracking_2024-07-18.csv  

-----------------------------------------------  
[1] Start Over  
[2] Exit  

Enter your choice: 2  
Exiting... 
```

---

## **Technology Stack**  
- **C# (.NET 8) Console App**  
- **SQLite** (for local storage)  
- **ADO.NET** provider for SQLite (`Microsoft.Data.Sqlite`).  
- **WebSockets** (AIS real-time data from aisstream.io)  
- **HttpClient** (REST API calls to OpenWeather)  

## **Submission Requirements**

- **Source code** in a GitHub repository (include the SQLite database "`data.db`" file).
- **README.md** with setup instructions (including database creation scripts).
- Brief documentation on how to run the application.
