# -Weather-Forecast-App
BHARAT INTERN

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';

void main() {
  runApp(WeatherApp());
}

class WeatherApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Weather App',
      home: WeatherScreen(),
    );
  }
}

class WeatherScreen extends StatefulWidget {
  @override
  _WeatherScreenState createState() => _WeatherScreenState();
}

class _WeatherScreenState extends State<WeatherScreen> {
  final TextEditingController locationController = TextEditingController();
  WeatherData weatherData;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Weather App'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            TextField(
              controller: locationController,
              decoration: InputDecoration(
                labelText: 'Enter Location',
              ),
            ),
            SizedBox(height: 20.0),
            ElevatedButton(
              onPressed: () {
                fetchWeatherData(locationController.text);
              },
              child: Text('Get Weather'),
            ),
            SizedBox(height: 20.0),
            if (weatherData != null)
              Column(
                children: [
                  Text('Temperature: ${weatherData.temperature}°C'),
                  Text('Description: ${weatherData.description}'),
                  Text('Humidity: ${weatherData.humidity}%'),
                ],
              ),
          ],
        ),
      ),
    );
  }

  Future<void> fetchWeatherData(String location) async {
    // Replace with your actual API endpoint and key
    final String apiKey = 'YOUR_API_KEY';
    final String apiUrl = 'https://api.example.com/weather?location=$location&apiKey=$apiKey';

    try {
      final response = await http.get(Uri.parse(apiUrl));

      if (response.statusCode == 200) {
        final Map<String, dynamic> data = json.decode(response.body);
        setState(() {
          weatherData = WeatherData.fromMap(data);
        });
      } else {
        throw Exception('Failed to fetch weather data');
      }
    } catch (error) {
      print(error);
    }
  }
}

class WeatherData {
  final double temperature;
  final String description;
  final int humidity;

  WeatherData({required this.temperature, required this.description, required this.humidity});

  factory WeatherData.fromMap(Map<String, dynamic> map) {
    return WeatherData(
      temperature: map['temperature'],
      description: map['description'],
      humidity: map['humidity'],
    );
  }
}
