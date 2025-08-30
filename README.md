Here's a simple README template you can use for your project based on the code you've shared:

---

# Cluster API Project

This project implements an API for clustering location data using the **DBSCAN** algorithm, built with **Flask** for easy deployment. The API processes location data, performs clustering, and provides two possible solutions for grouping locations based on priorities and time constraints. The results are visualized on interactive maps generated using **Folium**.

## Features

* **Location Clustering**: Using DBSCAN, this API clusters a list of location data based on geographical coordinates.
* **Clustering Solutions**:

  1. **Solution 1**: Assigns locations to a single day based on maximum hours.
  2. **Solution 2**: Distributes locations across multiple days based on a set maximum hours per day.
* **Map Visualizations**: Generates HTML files with visual maps for each day in Solution 2, showing the clustered locations.
* **Dynamic API Response**: Returns clustering results in a JSON format that includes location details (latitude, longitude, priority, etc.).

## Requirements

* Python 3.x
* Flask
* Scikit-learn
* Folium
* Numpy

You can install the required dependencies using `pip`:

```bash
pip install Flask scikit-learn folium numpy
```

## Installation

1. **Clone the Repository**:

   ```bash
   git clone https://github.com/yourusername/repository-name.git
   cd repository-name
   ```

2. **Install Dependencies**:
   Make sure you have Python 3.x installed and use the following command to install the required packages:

   ```bash
   pip install -r requirements.txt
   ```

3. **Run the API**:
   To start the Flask API, simply run:

   ```bash
   python app.py
   ```

   The server will start running on `http://127.0.0.1:5000/`.

## API Usage

### Endpoint: `/get_clusters`

**Method**: `POST`

**Request Body** (JSON):

```json
{
  "locations_sorted": [
    ["start_lat", "start_lon", 1, 2],  // Example of location data: [latitude, longitude, priority, stay_hours]
    ["lat1", "lon1", 2, 3],
    ["lat2", "lon2", 1, 1]
  ],
  "requested_days": 2,
  "max_hours_per_day": 10
}
```

* `locations_sorted`: A sorted list of locations with attributes: latitude, longitude, priority, and stay hours.
* `requested_days`: The number of days the locations should be spread across.
* `max_hours_per_day`: The maximum number of hours that can be allocated for visits each day.

### Example Response:

```json
{
  "solution1": {
    "day1": [
      {
        "latitude": "lat1",
        "longitude": "lon1",
        "priority": 1,
        "stay_hours": 3,
        "cluster_id": 1
      },
      {
        "latitude": "lat2",
        "longitude": "lon2",
        "priority": 2,
        "stay_hours": 4,
        "cluster_id": 1
      }
    ],
    "rejected": []
  },
  "solution2": [
    {
      "day": 1,
      "locations": [
        {
          "latitude": "lat1",
          "longitude": "lon1",
          "priority": 2,
          "stay_hours": 4,
          "cluster_id": 1
        }
      ]
    },
    {
      "day": 2,
      "locations": [
        {
          "latitude": "lat2",
          "longitude": "lon2",
          "priority": 1,
          "stay_hours": 3,
          "cluster_id": 1
        }
      ]
    }
  ]
}
```

### Visualization

The API generates visual maps for each day in the `solution2` response, where the clusters are plotted with markers indicating their geographic locations.

### Example Output Map:

Each day in the solution results in an HTML map file (e.g., `day_1.html`), which opens automatically in your browser.


