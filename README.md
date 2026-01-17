# 🌤️ Weather Data API Pipeline with Python & Azure Blob Storage 

## 📝 Overview
This Python-based pipeline fetches current weather data from the OpenWeatherMap API, processes it, and stores it as Blob Storage and CSV.

<img width="1397" height="685" alt="image" src="https://github.com/user-attachments/assets/4c252282-c1a7-4204-baf3-bbe8aef1f579" />

## ✨ How the script works

**1. Load environment variables from .env**
  - Loads the Azure Storage connection string and OpenWeatherMap API key from a .env file using python-dotenv, keeping credentials secure and configurable.

**2. Authenticate to Azure Blob Storage**
  - Connects to Azure Storage with BlobServiceClient and verifies access by listing all containers.

**3. Call OpenWeatherMap API for London weather data**
  - Sends an HTTP GET request using requests with latitude/longitude for London (lat=51.5080, lon=-0.1281) and the API key.
  - Checks the response; raises an error if the call fails.

**4. Add current timestamp**
  - Converts the API JSON response to a Python dictionary.
  - Adds a current_time field using datetime.now().isoformat() for tracking and filenames.

**5. Upload raw JSON to Azure Blob Storage**
  - Converts the dictionary to a string (json.dumps()) and uploads it to the open-weather-map-api-raw container, preserving the original data.

**6. Download raw JSON back for filtering**
  - Reads the raw blob from Azure and converts it from bytes → string → Python dictionary to ensure filtering is done on stored data.

**7. Filter important fields**
  - Extracts relevant data: location, latitude, longitude, temperature, weather status, description, and timestamp.
  - Converts the filtered dictionary to a JSON string for storage.

**8. Upload filtered JSON to Azure Blob Storage**
  - Uploads the filtered JSON string to the open-weather-map-api-filtered container, creating a clean dataset ready for analysis.

**9. Convert filtered JSON to a CSV file locally**
  - Converts filtered JSON to a pandas DataFrame.
  - Adds a timestamp to the filename and saves it locally for reporting or further processing.

**10. Configurable and reusable**
  - Sensitive credentials are stored in the .env file.
  - Paths and container names can be easily modified, allowing the pipeline to be reused for different locations or extended to other APIs.
<br>

## ⚙️ Prerequisites

Before running the pipeline, make sure you have
  - An Azure Storage Account with these blob containers:
      - open-weather-map-api-raw
      - open-weather-map-api-filtered
  - OpenWeatherMap API key
  - Python 3.12+ with packages installed:
      - import os
      - import json
      - import requests
      - import pandas as pd
      - from dotenv import load_dotenv
      - from pathlib import Path
      - from azure.storage.blob import BlobServiceClient
      - from datetime import datetime
<br>

## 🛠️ Setup

Clone the repository (or copy my script)
<br>

Create a .env file in your project folder with your Azure connection string and API key:
- AZURE_STORAGE_CONNECTION_STRING=your_connection_string_here
- API_KEY=your_openweathermap_api_key_here
<br>

Update folder paths in the Python script:
- dot_env_file_path = Path(r"*INSERT FOLDER PATH FOR .env FILE*")
- csv_file_path = r"*INSERT FOLDER PATH TO SAVE CSV*"
<br>

## 📄 Example Filtered JSON
{
  "location": "London",
  "lat": 51.508,
  "lon": -0.1281,
  "current_time": "2026-01-17T12-50-30",
  "temp": 5.5,
  "weather_status": "Clouds",
  "weather_description": "few clouds"
}

<br>

## 🏃 Usage

Run the script: python python_azure_api_call.py
<br>

After running:
- Raw JSON stored in open-weather-map-api-raw
- Filtered JSON stored in open-weather-map-api-filtered
- Filtered CSV saved locally in the specified folder
<br>

## 💡 Notes

API returns metric (°C) by default; use units="imperial" for Fahrenheit

CSV filenames are automatically timestamped and sanitized for Windows

Extend the pipeline to multiple locations, additional API fields, or other APIs
<br>

## 📜 License

MIT License – feel free to reuse and modify for your projects
