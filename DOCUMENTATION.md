# LadGEO Documentation Guide

LadGEO is a high-efficiency logistics and delivery route optimization platform built with React, Vite, and Tailwind CSS. It leverages real-world road data, multi-vehicle capacity constraints, and advanced heuristic algorithms to minimize travel time and maximize driver productivity.

## 🚀 Quick Start Guide

1.  **Set the Hub**: The "Central Hub" is your starting and ending point. Use the search bar or GPS icon to set your depot's coordinates.
2.  **Define Vehicles**: Add your fleet members. You can specify:
    *   **Capacity (kg)**: Maximum weight the vehicle can carry.
    *   **Speed (km/h)**: Average travel speed for ETA calculations.
    *   **Color**: Used to distinguish the vehicle's route on the map.
3.  **Add Stops**: Enter delivery locations. Use the **Autocomplete Search** for precise addressing or the **GPS button** if you are currently at the delivery site.
    *   *Weight*: Input the parcel weight for capacity checking.
    *   *Priority*: High-priority stops are prioritized for earlier time slots.
4.  **Optimize**: Click **"Analyze & Optimize Routes"**. The system will distribute stops among vehicles and calculate the fastest path for each using road-network data.

---

## 🛠 Technical Architecture

### 1. Routing & Optimization Logic
*   **Capacity Constraints**: The app uses a "Best-Fit Decreasing" bin-packing approach to assign stops to vehicles based on weight limits and priority.
*   **TSP Solver**: 
    *   For routes with $\le 11$ stops, an exact **Held-Karp** (Dynamic Programming) approach is used.
    *   For routes with $> 11$ stops, a **2-Opt Heuristic** is applied to iteratively improve the path.
*   **Real Road Data**: Integration with the **OSRM (Open Source Routing Machine)** API fetches actual street geometry and road distances rather than simple "as-the-crow-flies" calculations.

### 2. Map & Geolocation
*   **Leaflet.js**: Used for rendering the interactive map layer.
*   **OpenStreetMap (Nominatim)**: Powers the location search and reverse-geocoding (GPS) features.
*   **Dynamic Polylines**: Renders distinct, color-coded paths for each vehicle with directional indicators.

### 3. State Management & Components
*   **`App.tsx`**: Orchestrates the global state (Stops, Vehicles, Optimized Routes).
*   **`RouteMap.tsx`**: High-performance map component with auto-bounding.
*   **`LocationSearch.tsx`**: A debounced search component for address resolution.
*   **`routing.ts`**: The mathematical core containing distance matrices and optimization algorithms.

---

## 📊 Understanding Metrics

*   **Total Distance**: Sum of all road segments for all active routes.
*   **ETA (Estimated Time of Arrival)**: Calculated based on road distance and vehicle speed, including a simulated 5-minute service time per stop.
*   **Capacity Utilization**: Shown as a percentage bar for each vehicle. If a vehicle is over-capacity, stops will remain unassigned.
*   **Optimization Score**: A comparison between a randomized route and the computed optimized path.

---

## 🔒 Limitations & Best Practices

*   **API Rate Limits**: The geocoding and OSRM services are public instances. For high-volume commercial use, consider a private OSRM server or a Google Maps API key.
*   **Coordinate Accuracy**: Always verify the address suggestion in the dropdown to ensure the latitude/longitude is precise.
*   **Browser Permissions**: Location services require "Allow" permissions in the browser to use the GPS icon feature.

---

## 🧪 Demo Data
Use the **"Load Demo Data"** button in the dashboard to instantly populate the app with a sample fleet and 5 delivery locations to see the optimization engine in action.
