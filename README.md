# Gemini & Cluster Itinerary Planner API

This project implements a Flask-based web API that generates personalized tourist itineraries for Singapore and performs clustering on location data using **DBSCAN**. The API combines LLM-powered suggestions via the Gemini API with geospatial clustering for optimized multi-day itineraries, providing both JSON responses and interactive maps.

## Features

* **LLM-Powered Itinerary Generation**: Uses Google's Gemini API to fetch recommended tourist attractions.
* **Location Clustering**: Groups attractions by geographic coordinates using DBSCAN.
* **Clustering Solutions**:

  1. **Solution 1**: Assign locations to a user-defined number of days.
  2. **Solution 2**: Distribute locations across the optimal minimum number of days.
* **Priority-Based Planning**: Attractions are sorted by priority and distributed across days according to maximum hours per day.
* **Map Visualizations**: Generates HTML maps for each day with clusters and location markers.
* **Dynamic API Response**: Returns itinerary and clustering results in JSON format, including location details (latitude, longitude, priority, suggested visit hours, cluster IDs).

## Requirements

* Python 3.9+
* Flask
* numpy
* scikit-learn
* folium
* google-genai (Gemini API client)

Install dependencies via pip:

```bash
pip install Flask numpy scikit-learn folium google-genai
```

## Installation

1. **Clone the Repository**:

   ```bash
   git clone https://github.com/yourusername/gemini_cluster_itinerary.git
   cd gemini_cluster_itinerary
   ```

2. **Set API Key**:

   * Replace `API_KEY` in `app.py` with your Gemini API key.

3. **Run the API**:

   ```bash
   python app.py
   ```

   The server will start on `http://127.0.0.1:5000/`.

## API Endpoints

### 1. `/plan_itinerary_llm`

**Method**: `POST`

**Request Body**:

```json
{
  "user_stay_days": 3,
  "max_hours_per_day": 8
}
```

**Response**:

```json
{
  "solution": "solution1",
  "days": [
    [
      {
        "cluster": 0,
        "locations": [
          {
            "name": "Hotel/Start",
            "latitude": 1.3000,
            "longitude": 103.8300,
            "priority": 1,
            "suggested_visit_hours": 0
          }
        ]
      }
    ]
  ]
}
```

### 2. `/get_clusters`

**Method**: `POST`

**Request Body**:

```json
{
  "locations_sorted": [
    [1.3000, 103.8300, 1, 0],
    [1.2827, 103.865, 1, 2],
    [1.286, 103.852, 2, 3]
  ],
  "requested_days": 2,
  "max_hours_per_day": 10
}
```

**Response**:

```json
{
  "solution1": {
    "day1": [
      {
        "latitude": 1.2827,
        "longitude": 103.865,
        "priority": 1,
        "stay_hours": 2,
        "cluster_id": 0
      }
    ],
    "rejected": []
  },
  "solution2": [
    {
      "day": 1,
      "locations": [
        {
          "latitude": 1.2827,
          "longitude": 103.865,
          "priority": 1,
          "stay_hours": 2,
          "cluster_id": 0
        }
      ]
    }
  ]
}
```

## Visualization

* Generates HTML maps for each day showing clusters and attraction markers.
* Example output file: `day_1.html`.

## Notes

* Ensure the Gemini API key is valid.
* Adjust `eps` in DBSCAN for clustering granularity.
* Designed specifically for Singapore attractions, but can be adapted for other locations.


