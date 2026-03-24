# 3D-shooter-game-#include <iostream>
#include <chrono>
#include <iomanip>
#include <vector>
#include <ctime>

struct TimeZone {
    std::string name;
    int offset; // UTC offset in hours
};

class DigitalClock {
private:
    std::vector<TimeZone> timeZones;

public:
    DigitalClock() {
        // Initialize common time zones (UTC offset in hours)
        timeZones = {
            {"UTC", 0},
            {"EST (US Eastern)", -5},
            {"CST (US Central)", -6},
            {"MST (US Mountain)", -7},
            {"PST (US Pacific)", -8},
            {"GMT (UK)", 0},
            {"CET (Central Europe)", 1},
            {"IST (India)", 5.5},
            {"JST (Japan)", 9},
            {"AEST (Australia)", 10}
        };
    }

    void displayClock() {
        while (true) {
            system("clear"); // Use "cls" on Windows
            
            auto now = std::chrono::system_clock::now();
            auto time = std::chrono::system_clock::to_time_t(now);
            
            std::cout << "╔════════════════════════════════════════╗\n";
            std::cout << "║     DIGITAL CLOCK - WORLD TIME ZONES     ║\n";
            std::cout << "╚════════════════════════════════════════╝\n\n";
            
            for (const auto& tz : timeZones) {
                displayTimeInZone(time, tz);
            }
            
            std::cout << "\n(Updates every second. Press Ctrl+C to exit)\n";
            std::this_thread::sleep_for(std::chrono::seconds(1));
        }
    }

private:
    void displayTimeInZone(std::time_t time, const TimeZone& tz) {
        // Get UTC time
        struct tm* utc_time = std::gmtime(&time);
        
        // Calculate seconds offset
        long seconds_offset = static_cast<long>(tz.offset * 3600);
        time_t adjusted_time = time + seconds_offset;
        
        struct tm* local_time = std::gmtime(&adjusted_time);
        
        std::cout << std::left << std::setw(25) << tz.name 
                  << ": ";
        
        // Display in HH:MM:SS format
        std::cout << std::setfill('0')
                  << std::setw(2) << local_time->tm_hour << ":"
                  << std::setw(2) << local_time->tm_min << ":"
                  << std::setw(2) << local_time->tm_sec
                  << " UTC" << std::showpos << std::setw(+3) 
                  << static_cast<int>(tz.offset) << "\n";
        std::cout << std::noshowpos;
    }
};

int main() {
    DigitalClock clock;
    clock.displayClock();
    return 0;
}
