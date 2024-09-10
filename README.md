# DEP
Basic level:
1) LOCATION.H:


#include "LocationManager.h"
#include <iostream>
#include <algorithm>

void LocationManager::addLocation(const Location& location) {
    locations.push_back(location);
}

void LocationManager::removeLocation(const std::string& name) {
    locations.erase(std::remove_if(locations.begin(), locations.end(),
                    [&name](const Location& loc) { return loc.getName() == name; }),
                    locations.end());
}

void LocationManager::listLocations() const {
    for (const auto& loc : locations) {
        std::cout << "Name: " << loc.getName() << ", Latitude: " << loc.getLatitude()
                  << ", Longitude: " << loc.getLongitude() << std::endl;
    }
}

2) LOCATION.CPP:

   
#include "Location.h"

Location::Location(const std::string& name, double latitude, double longitude)
    : name(name), latitude(latitude), longitude(longitude) {}

std::string Location::getName() const {
    return name;
}

double Location::getLatitude() const {
    return latitude;
}

double Location::getLongitude() const {
    return longitude;
}


3)LOCATIONMANAGER.H:


#ifndef LOCATIONMANAGER_H
#define LOCATIONMANAGER_H

#include "Location.h"
#include <vector>

class LocationManager {
public:
    void addLocation(const Location& location);
    void removeLocation(const std::string& name);
    void listLocations() const;

private:
    std::vector<Location> locations;
};

#endif // LOCATIONMANAGER_H


4) LOCATIONMANAGER.CPP:

   
#include "LocationManager.h"
#include <iostream>
#include <algorithm>

void LocationManager::addLocation(const Location& location) {
    locations.push_back(location);
}

void LocationManager::removeLocation(const std::string& name) {
    locations.erase(std::remove_if(locations.begin(), locations.end(),
                    [&name](const Location& loc) { return loc.getName() == name; }),
                    locations.end());
}

void LocationManager::listLocations() const {
    for (const auto& loc : locations) {
        std::cout << "Name: " << loc.getName() << ", Latitude: " << loc.getLatitude()
                  << ", Longitude: " << loc.getLongitude() << std::endl;
    }
}

5) WEATHERVARIABLE CLASS:
   #include <map>

class WeatherVariable {
public:
    void setVariable(const std::string& name, double value) {
        variables[name] = value;
    }

    double getVariable(const std::string& name) const {
        auto it = variables.find(name);
        if (it != variables.end()) {
            return it->second;
        }
        return 0.0;
    }

    void listVariables() const {
        for (const auto& var : variables) {
            std::cout << var.first << ": " << var.second << std::endl;
        }
    }

private:
    std::map<std::string, double> variables;
};


6) WEATHER FORECASTING SYSTEM CLASS


   #include <iostream>
#include <string>
#include <curl/curl.h> // Ensure libcurl is installed and linked

class WeatherForecastingSystem {
public:
    std::string fetchWeatherData(const std::string& location) {
        // Placeholder for API URL
        std::string apiUrl = "https://api.example.com/weather?location=" + location;

        // Initialize CURL
        CURL* curl = curl_easy_init();
        if (!curl) {
            return "CURL initialization failed";
        }

        // Set options
        curl_easy_setopt(curl, CURLOPT_URL, apiUrl.c_str());

        // Write callback function to handle response
        std::string response;
        curl_easy_setopt(curl, CURLOPT_WRITEFUNCTION, WriteCallback);
        curl_easy_setopt(curl, CURLOPT_WRITEDATA, &response);

        // Perform the request
        CURLcode res = curl_easy_perform(curl);
        curl_easy_cleanup(curl);

        if (res != CURLE_OK) {
            return "CURL request failed";
        }

        return response;
    }

private:
    static size_t WriteCallback(void* contents, size_t size, size_t nmemb, void* userp) {
        ((std::string*)userp)->append((char*)contents, size * nmemb);
        return size * nmemb;
    }
};


7) HISTORICAL WEATHER FORECASTING SYSTEM:


   class HistoricalWeatherSystem {
public:
    std::string fetchHistoricalData(const std::string& location, const std::string& date) {
        // Placeholder for API URL
        std::string apiUrl = "https://api.example.com/historical?location=" + location + "&date=" + date;

        // Initialize CURL
        CURL* curl = curl_easy_init();
        if (!curl) {
            return "CURL initialization failed";
        }

        // Set options
        curl_easy_setopt(curl, CURLOPT_URL, apiUrl.c_str());

        // Write callback function to handle response
        std::string response;
        curl_easy_setopt(curl, CURLOPT_WRITEFUNCTION, WriteCallback);
        curl_easy_setopt(curl, CURLOPT_WRITEDATA, &response);

        // Perform the request
        CURLcode res = curl_easy_perform(curl);
        curl_easy_cleanup(curl);

        if (res != CURLE_OK) {
            return "CURL request failed";
        }

        return response;
    }

private:
    static size_t WriteCallback(void* contents, size_t size, size_t nmemb, void* userp) {
        ((std::string*)userp)->append((char*)contents, size * nmemb);
        return size * nmemb;
    }
};


8) EXPORT TO CSV:


   #include <fstream>

void exportToCSV(const std::vector<Location>& locations) {
    std::ofstream file("locations.csv");
    file << "Name,Latitude,Longitude\n";
    for (const auto& loc : locations) {
        file << loc.getName() << ","
             << loc.getLatitude() << ","
             << loc.getLongitude() << "\n";
    }
}


9) EXPORT TO JSON:


    #include <nlohmann/json.hpp>
#include <fstream>

void exportToJSON(const std::vector<Location>& locations) {
    nlohmann::json jsonArray;
    for (const auto& loc : locations) {
        nlohmann::json jsonObject;
        jsonObject["Name"] = loc.getName();
        jsonObject["Latitude"] = loc.getLatitude();
        jsonObject["Longitude"] = loc.getLongitude();
        jsonArray.push_back(jsonObject);
    }

    std::ofstream file("locations.json");
    file << jsonArray.dump(4); // Pretty-print with an indent of 4 spaces
}

ADVANCED LEVEL:

#include <string>
#include <vector>
#include <iostream>

// Base class for weather and air quality systems
class DataSystem {
public:
    virtual std::string fetchData(const std::string& location) = 0;
    virtual void displayData(const std::string& data) const = 0;
};




#include <curl/curl.h>

class AirQualityForecastingSystem : public DataSystem {
public:
    std::string fetchData(const std::string& location) override {
        std::string apiUrl = "https://api.example.com/airquality?location=" + location;
        CURL* curl = curl_easy_init();
        if (!curl) return "CURL initialization failed";

        std::string response;
        curl_easy_setopt(curl, CURLOPT_URL, apiUrl.c_str());
        curl_easy_setopt(curl, CURLOPT_WRITEFUNCTION, WriteCallback);
        curl_easy_setopt(curl, CURLOPT_WRITEDATA, &response);

        CURLcode res = curl_easy_perform(curl);
        curl_easy_cleanup(curl);

        return res == CURLE_OK ? response : "CURL request failed";
    }

    void displayData(const std::string& data) const override {
        std::cout << "Air Quality Data: " << data << std::endl;
    }

private:
    static size_t WriteCallback(void* contents, size_t size, size_t nmemb, void* userp) {
        ((std::string*)userp)->append((char*)contents, size * nmemb);
        return size * nmemb;
    }
};



#include <fstream>

class OfflineStorage {
public:
    void saveData(const std::string& data, const std::string& filename) {
        std::ofstream file(filename);
        file << data;
        file.close();
    }

    std::string loadData(const std::string& filename) {
        std::ifstream file(filename);
        std::string data((std::istreambuf_iterator<char>(file)), std::istreambuf_iterator<char>());
        return data;
    }
};



#include <firebase/app.h>
#include <firebase/firestore.h>

class CloudStorage {
public:
    CloudStorage() {
        // Initialize Firebase
        firebase::AppOptions options;
        app_ = firebase::App::Create(options);
        firestore_ = firebase::firestore::Firestore::GetInstance(app_);
    }

    void saveData(const std::string& collection, const std::string& document, const std::string& data) {
        auto document_ref = firestore_->Collection(collection).Document(document);
        document_ref.Set({{"data", data}});
    }

    std::string loadData(const std::string& collection, const std::string& document) {
        auto document_ref = firestore_->Collection(collection).Document(document);
        auto result = document_ref.Get();
        if (result.status().ok()) {
            return result.result().Get("data").string_value();
        }
        return "Error fetching data";
    }

private:
    firebase::App* app_;
    firebase::firestore::Firestore* firestore_;
};



class UserInterface {
public:
    void run() {
        // Example interaction
        std::string location;
        std::cout << "Enter location: ";
        std::cin >> location;

        // Fetch and display weather and air quality data
        WeatherForecastingSystem weatherSystem;
        std::string weatherData = weatherSystem.fetchData(location);
        weatherSystem.displayData(weatherData);

        AirQualityForecastingSystem airQualitySystem;
        std::string airQualityData = airQualitySystem.fetchData(location);
        airQualitySystem.displayData(airQualityData);
    }
};



#include <nlohmann/json.hpp>
#include <fstream>

void exportToCSV(const std::vector<std::string>& data, const std::string& filename) {
    std::ofstream file(filename);
    for (const auto& entry : data) {
        file << entry << "\n";
    }
    file.close();
}

void exportToJSON(const std::vector<std::string>& data, const std::string& filename) {
    nlohmann::json jsonArray = data;
    std::ofstream file(filename);
    file << jsonArray.dump(4); // Pretty-print with indent of 4 spaces
    file.close();
}


#include <gtest/gtest.h>

TEST(LocationTest, Creation) {
    Location loc("Test", 12.34, 56.78);
    EXPECT_EQ(loc.getName(), "Test");
    EXPECT_DOUBLE_EQ(loc.getLatitude(), 12.34);
    EXPECT_DOUBLE_EQ(loc.getLongitude(), 56.78);
}



   


