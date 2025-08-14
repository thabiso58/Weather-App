import requests

import os


def get_weather(api_key, location):


    try:

        base_url = f"http://api.openweathermap.org/data/2.5/weather?q={location}&appid={api_key}&units=metric"

        response = requests.get(base_url)

        response.raise_for_status()

        weather_data = response.json()

        return weather_data

    except requests. exceptions.RequestException as e:

        print(f"Error fetching weather data: {e}")

        return None


def display_weather(weather_data):

    if weather_data is not None:

        print(f"Weather in {weather_data['name']}:")

        print(f"Temperature: {weather_data['main']['temp']}°C")

        print(f"Humidity: {weather_data['main']['humidity']}%")

        print(f"Wind Speed: {weather_data['wind']['speed']} m/s")

        print(f"Weather Condition: {weather_data['weather'][0]['description']}")

    else:

        print("Failed to retrieve weather data.")


def main():

    api_key = os.getenv("OPENWEATHER_API_KEY")

    if not api_key:

        print("Error: Please set your OPENWEATHER_API_KEY in the Secrets tab")

        return

    while True:

        location = input("Enter the location (or 'exit' to quit): ")

        if location.lower() == 'exit':

            break

        weather_data = get_weather(api_key, location)

        display_weather(weather_data)


if __name__ == "__main__":

    main()
