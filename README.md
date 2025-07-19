# SkyCast - Find My Weather
## Date:19.07.2025
## Objective:
To build a responsive single-page application using React that allows users to enter a city name and retrieve real-time weather information using the OpenWeatherMap API. This project demonstrates the use of Axios for API calls, React Router for navigation, React Hooks for state management, controlled components with validation, and basic styling with CSS.
## Tasks:

#### 1. Project Setup
Initialize React app.

Install necessary dependencies: npm install axios react-router-dom

#### 2. Routing
Set up BrowserRouter in App.js.

Create two routes:

/ – Home page with input form.

/weather – Page to display weather results.

#### 3. Home Page (City Input)
Create a controlled input field for the city name.

Add validation to ensure the input is not empty.

On valid form submission, navigate to /weather and store the city name.

#### 4. Weather Page (API Integration)
Use Axios to fetch data from the OpenWeatherMap API using the city name.

Show temperature, humidity, wind speed, and weather condition.

Convert and display temperature in both Celsius and Fahrenheit using useMemo.

#### 5. React Hooks
Use useState for managing city, weather data, and loading state.

Use useEffect to trigger the Axios call on page load.

Use useCallback to optimize form submit handler.

Use useMemo for temperature conversion logic.

#### 6. UI Styling (CSS)
Create a responsive and clean layout using CSS.

Style form, buttons, weather display cards, and navigation links.

## Programs:
## Home.jsx:
```
import React, { useState, useCallback } from 'react';
import { useNavigate } from 'react-router-dom';

function Home({ city, setCity }) {
  const [input, setInput] = useState(city);
  const [error, setError] = useState('');
  const navigate = useNavigate();

  const handleSubmit = useCallback((e) => {
    e.preventDefault();
    if (input.trim() === '') {
      setError('City name cannot be empty.');
      return;
    }
    setError('');
    setCity(input);
    navigate('/weather');
  }, [input, navigate, setCity]);

  return (
    <div className="container">
      <h1>Weather App</h1>
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          placeholder="Enter city name"
          value={input}
          onChange={(e) => setInput(e.target.value)}
        />
        <button type="submit">Get Weather</button>
      </form>
      {error && <p className="error">{error}</p>}
    </div>
  );
}

export default Home;
```
## Weather.jsx:
```
import React, { useEffect, useState, useMemo } from 'react';
import axios from 'axios';
import { useNavigate } from 'react-router-dom';
function Weather({ city }) {
  const [weather, setWeather] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState('');
  const navigate = useNavigate();
  useEffect(() => {
    if (!city) {
      console.log("No city found, redirecting to home");
      navigate('/');
      return;
    }
    const apiKey = 'bd5e378503939ddaee76f12ad7a97608'; 
    const url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&units=metric`;
    console.log("Fetching weather data for:", city);
    axios
      .get(url)
      .then((response) => {
        console.log("API Success:", response.data);
        setWeather(response.data);
        setError('');
      })
      .catch((err) => {
        console.error("API Error:", err);
        setError('Failed to fetch weather data');
      })
      .finally(() => {
        setLoading(false);
      });
  }, [city, navigate]);

  const fahrenheit = useMemo(() => {
    if (!weather) return null;
    return (weather.main.temp * 9) / 5 + 32;
  }, [weather]);

  return (
    <div className="container">
      <h1>Weather in {city}</h1>
      {loading ? (
        <p>Loading weather...</p>
      ) : error ? (
        <p className="error">{error}</p>
      ) : weather ? (
        <div className="card">
          <p><strong>Temperature:</strong> {weather.main.temp}°C / {fahrenheit.toFixed(1)}°F</p>
          <p><strong>Humidity:</strong> {weather.main.humidity}%</p>
          <p><strong>Wind Speed:</strong> {weather.wind.speed} m/s</p>
          <p><strong>Condition:</strong> {weather.weather[0].main}</p>
        </div>
      ) : (
        <p>No weather data available.</p>
      )}
      <button onClick={() => navigate('/')}>Search Another City</button>
    </div>
  );
}

export default Weather;
```
## App.jsx:
```
import React, { useState } from 'react';
import { Routes, Route } from 'react-router-dom';
import Home from './Home';
import Weather from './Weather';
function App() {
  const [city, setCity] = useState('');
  return (
    <Routes>
      <Route path="/" element={<Home city={city} setCity={setCity} />} />
      <Route path="/weather" element={<Weather city={city} />} />
    </Routes>
  );
}
export default App;
```
## App.css:
```
body {
  font-family: sans-serif;
  background: lightblue;
  margin: 0;
  padding: 0;
}

.container {
  max-width: 500px;
  margin: 50px auto;
  padding: 20px;
  background:lightpink;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0,0,0,0.1);
  text-align: center;
}

input {
  padding: 10px;
  width: 80%;
  font-size: 16px;
  margin-bottom: 10px;
}

button {
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
}

.error {
  color: red;
}

.card {
  margin-top: 20px;
  padding: 15px;
  background:lightcyan;
  border-radius: 8px;
}
```
## Output:
<img width="1919" height="1079" alt="Screenshot 2025-07-19 201455" src="https://github.com/user-attachments/assets/7d1cc6c5-6d6b-4e4e-bf49-7933646c83c0" />
<img width="1919" height="1079" alt="Screenshot 2025-07-19 201511" src="https://github.com/user-attachments/assets/b72fc86d-e5cb-4090-9b07-388e693f5be0" />


## Result:
A responsive single-page application using React that allows users to enter a city name and retrieve real-time weather information using the OpenWeatherMap API has been built successfully. 
