# IoT-based-Alcohol-Monitoring-System-using-GPS-and-GSM-modules
IoT-Based Alcohol Detection &amp; Vehicle Immobilization System An IoT solution to improve road safety by detecting alcohol on the driver’s breath. Built with Arduino, MQ-3 sensor, GPS, and GSM, it immobilizes the vehicle and sends location alerts via SMS if alcohol levels are high. Aimed to reduce accidents and support responsible driving practices.
## Project Title:
IoT based Alcohol Monitoring System using GPS and GSM modules
## Project Description:
An IoT-powered alcohol detection system that enhances road safety by detecting alcohol in the driver’s breath, immobilizing the vehicle, and sending location alerts via SMS using GPS and GSM modules. Built with Arduino and compatible sensors, it’s designed for public and private vehicles to reduce alcohol-related accidents.

## Features:

1.Real-time alcohol detection with MQ-3 sensor.

2.Vehicle immobilization if alcohol is detected.

3.Location alerts via SMS with exact GPS coordinates.

4.Instant system status display on 16x2 LCD screen.

## Hardware Requirements:

1.Arduino Uno

2.MQ-3 Alcohol Sensor

3.NEO-6M GPS Module

4.SIM800l GSM Module

5.16x2 LCD Display

6.Lithium-ion battery and DC geared motors

7.Plywood chassis and wiring materials

## Software Requirements:

1.Arduino IDE (for coding and uploading the script)

2.Libraries for SIM800l, NEO-6M GPS, and LCD 

## System Architecture:

![image](https://github.com/user-attachments/assets/3cf27fca-8908-4549-bb91-d81e9eafbdb9)
![image](https://github.com/user-attachments/assets/607d20e0-1de6-4b25-9dc4-26aa085dda63)


## Setup and Installation:

Follow these steps to set up the IoT-based Alcohol Detection and Vehicle Immobilization System:

### 1. Clone the Repository Start by cloning this repository to your local machine:

### 2.Prepare the Hardware:

Arduino Uno: Connect it to your computer via USB for programming.

MQ-3 Alcohol Sensor: Connect the sensor’s output pin to an analog input pin on the Arduino, the VCC pin to 5V, and the GND pin to ground.

NEO-6M GPS Module: Connect the TX pin to Arduino’s RX pin and the RX pin to Arduino’s TX pin.

SIM800l GSM Module: Attach the TX and RX pins to the Arduino’s RX and TX pins respectively, and ensure it has a reliable power supply.

16x2 LCD Display: Connect the LCD according to the specified pins in your code, using a 10k potentiometer for contrast adjustment.

### 3.Install Required Libraries:

Open the Arduino IDE and go to Sketch > Include Library > Manage Libraries...

Install the following libraries (if not already installed):

LiquidCrystal for the LCD display

TinyGPS++ for GPS data processing

SoftwareSerial (for handling serial communication with GSM)

### 4.Upload the Code

Open main_code.ino in the Arduino IDE.

Select the correct board (Arduino Uno) and port under Tools.

Verify the code and upload it to your Arduino Uno.

```````
// Include necessary libraries
#include <LiquidCrystal.h> 
// Library for LCD display management 
// PinDefinitions 
const int sensorPin = A0; 
// Analog pin connected to the alcohol sensor 
const int ledPin = 13;
 // Digital pin for an LED indicator 
const int buzzerPin = 12; 
// Digital pin for a buzzer alert const int buttonPin = 2;
 // Pin for a button to reset the alert 
// LCD setup: Define the pins for the LCD 
LiquidCrystal lcd(7, 8, 9, 10, 11, 12); 
// Initialize the LCD with the specified pins 
// Threshold values 
const int defaultThreshold = 400; 
// Default threshold for alcohol detection (calibrated for the sensor) 
const int buttonDebounce = 200;
 // Debounce time for button press in milliseconds 
// Variables to hold sensor readings and states 
int sensorValue = 0; 
// Variable to store the alcohol sensor reading 
int alcoholThreshold = defaultThreshold; 
// Variable to store the current alcohol threshold 
bool alertActive = false; 
// Flag to indicate whether the alert is active
 unsigned long lastDebounceTime = 0; 
// Time for button debounce mechanism 
bool buttonState = HIGH; 
// Current state of the button (using pull-up resistor) 
// Variables for logging alcohol levels
 const int logSize = 10;
 // Size of the alcohol level log
 int alcoholLog[logSize]; 
// Array to store last alcohol readings
 int logIndex = 0; 
// Index for logging 
// Function prototypes
 void activateAlert(); 
void deactivateAlert(); 
void initializeSystem(); 
void readSensorValue();
 void handleButtonPress(); 
void displayLog(); 
void calibrateThreshold(); 
void setup() 
{ 
// Initialize the system and LCD 
initializeSystem();
 } 
void loop() 
{ 
// Continuously read the alcohol sensor value
 readSensorValue(); 
// Display the current alcohol level on the LCD 
lcd.setCursor(0, 0); 
lcd.print("Alcohol Level:");
 lcd.setCursor(0, 1); 
lcd.print(sensorValue);
 // Store the sensor value in the log 
alcoholLog[logIndex] = sensorValue; 
// Log the current alcohol level 
logIndex = (logIndex + 1) % logSize; 
// Update the log index in a circular manner 
// Check if the alcohol level exceeds the threshold
 if (sensorValue > alcoholThreshold) 
{ 
activateAlert(); 
// Activate alert if threshold is exceeded
 } 
else 
{ 
deactivateAlert();
 // Deactivate alert if threshold is not exceeded 
} 
// Handle button press for resetting alerts and calibrating 
handleButtonPress(); }
 // Function to activate the alert when alcohol level exceeds threshold 
void activateAlert()
 { 
if (!alertActive) 
{ 
// Check if the alert is not already active 
alertActive = true; 
// Set the alert flag 
digitalWrite(ledPin, HIGH); 
// Turn on LED to indicate alert 
tone(buzzerPin, 1000); 
// Sound the buzzer at 1kHz frequency for alert 
lcd.clear(); 
// Clear the previous display 
lcd.print("ALERT! Alcohol!"); 
// Show alert message on LCD 
delay(2000);
 // Display alert for 2 seconds for visibility
 }
 } 
// Function to deactivate the alert when alcohol level is safe 
void deactivateAlert() 
{ 
alertActive = false; 
// Reset the alert flag
 digitalWrite(ledPin, LOW); 
// Turn off LED 
noTone(buzzerPin); 
// Stop buzzer sound 
lcd.clear(); 
// Clear the LCD 
lcd.print("Monitoring..."); 
// Show monitoring status on LCD 
} 
// Function to initialize the system components 
void initializeSystem()
 { 
// Initialize the LCD 
lcd.begin(16, 2); 
// Set up the LCD dimensions (16 columns, 2 rows)
 lcd.print("Alcohol Monitor"); 
// Display the system name on the LCD 
delay(2000); 
// Pause for 2 seconds to allow users to read the message 
// Set up pins for output
 pinMode(ledPin, OUTPUT);
 // Set LED pin as output 
pinMode(buzzerPin, OUTPUT); 
// Set buzzer pin as output
 pinMode(buttonPin, INPUT_PULLUP); 
// Configure button pin with internal pull-up resistor 
// Initialize the alcohol log 
for (int i = 0; i < logSize; i++) 
{ 
alcoholLog[i] = 0; 

// Set initial log values to zero
 } 
// Display initial message 
lcd.clear(); 

// Clear the display 
lcd.print("Initializing...");
 // Show initializing message 
delay(1000);
 // Allow time for message to be read 
lcd.clear(); 
// Clear the display for normal operation 
} 
// Function to read the sensor value from the alcohol sensor 
void readSensorValue()
 { 
sensorValue = analogRead(sensorPin); 
// Read the analog value from the sensor 
} 
// Function to handle button press and reset the alert or calibrate the threshold 
void handleButtonPress() 
{
 int reading = digitalRead(buttonPin); 
// Read the current state of the button
 if (reading == LOW && (millis() - lastDebounceTime) > buttonDebounce) 
{ 
lastDebounceTime = millis(); 
// Update the last debounce time 
// Check for a long press (more than 2 seconds) 
if (millis() - lastDebounceTime > 2000) 
{
 calibrateThreshold(); 
// Enter calibration mode 
} 
else
 {
 alertActive = false; 
// Reset the alert status
 deactivateAlert(); 
// Call to deactivate the alert
 } 
} 
} 
// Function to calibrate the alcohol threshold dynamically 
void calibrateThreshold()
 { 
lcd.clear(); 
// Clear the display
 lcd.print("Calibrating..."); 
// Show calibration message
 delay(2000); 
// Wait for 2 seconds for visibility
 lcd.clear(); 
// Clear the display for threshold input 
// Simulating threshold adjustment: In practice, you would capture a specific sensor value here alcoholThreshold = analogRead(sensorPin); 
// Read current value for calibration
 lcd.print("New Threshold:");
 lcd.setCursor(0, 1); 
lcd.print(alcoholThreshold);
 // Display the new threshold 
delay(2000); 
// Show the new threshold for 2 seconds 
} 
// Function to display the alcohol level log on the LCD 
void displayLog() 
{ 
lcd.clear(); 
// Clear the display 
lcd.print("Last Readings:"); 
for (int i = 0; i < logSize; i++) 
{
 lcd.setCursor(0, (i + 1) % 2); 
// Update cursor position 
lcd.print(alcoholLog[i]); 
// Display each log value 
delay(500); 
// Short delay for visibility
 }
 }
```````````````
### 4.Assemble the System

Place all components securely on the plywood chassis.

Connect the DC geared motors for vehicle movement.

Power the system using lithium-ion batteries, ensuring appropriate voltage levels for each component.

### 5.Power On and Test the System

After assembly, turn on the power.

The LCD should display the initial system status.

Test the system by introducing alcohol near the sensor; if the threshold is met, the buzzer should activate, the vehicle should immobilize, and an SMS with GPS coordinates will be sent.

## Usage:
Once all components are assembled and the code is uploaded, follow these steps to use the IoT-based Alcohol Detection and Vehicle Immobilization System:

### Power On the Device:

Connect the system to the power supply using lithium-ion batteries or an external power source. Ensure all connections are stable and the voltage is correct for each component.
Once powered on, the Arduino initializes, and the LCD screen will display a welcome message or the current status of the system, including "System Ready" or similar information.

### Monitoring System Status:

The LCD will continue to display the real-time system status. It will update to reflect "Monitoring..." when the alcohol detection sensor is actively scanning.
The MQ-3 sensor constantly monitors the driver’s breath for alcohol levels and provides feedback via the LCD, such as "Safe to Drive" if levels are below the threshold.

### Alcohol Detection Response:

If the sensor detects alcohol levels above the set threshold, the system will respond with several immediate actions to ensure driver and public safety:
Buzzer Activation: A buzzer will sound immediately to alert the driver and others nearby of intoxicated conditions.
Vehicle Immobilization: The system will trigger an immobilization mechanism, cutting off the vehicle’s engine to prevent any further movement.
SMS Alert with GPS Coordinates: The SIM800l GSM module will automatically send an SMS alert to pre-defined contacts. This alert includes the exact GPS coordinates of the vehicle, allowing authorities or designated contacts to take appropriate actions swiftly.

### Real-Time Location Tracking:

The NEO-6M GPS module continuously updates the vehicle’s position and sends current coordinates in the SMS notification. This allows emergency contacts or authorities to track the vehicle's exact location if needed.

### System Reset and Restart:

Once the vehicle has been safely secured and authorities have been notified, the system can be reset either by removing power or pressing a designated reset button (if included in the design).
After resetting, the system will resume monitoring once powered on again, ready to scan for alcohol levels and ensure safe driving conditions.
## Project Demo:
https://www.youtube.com/watch?v=rdVbVAqx6hk

## Output :
![image](https://github.com/user-attachments/assets/949e1174-1e49-4cf2-90b2-8f7b684abcb2)
![image](https://github.com/user-attachments/assets/cd496e0b-0d29-4bec-bb63-bb2154d197a5)

## Results and Outcome :
The alcohol detection system developed with an Arduino board demonstrates effective real-time monitoring of alcohol levels, specifically tailored for vehicle safety applications. During testing, the system reliably activated alerts through an LED indicator and buzzer when alcohol levels exceeded the preset threshold. This immediate response provides a visual and auditory warning, helping deter intoxicated driving.
The MQ-3 alcohol sensor proved consistent in detecting alcohol concentration in breath samples, with calibration adjustments allowing for accurate performance in different conditions. This adaptability enhances the system's reliability over time, even with environmental changes or sensor aging.
The system's LCD interface displays real-time alcohol readings, making it user-friendly, while the button-based reset feature allows for easy handling. This low-cost, efficient solution holds potential for broader applications, such as workplace safety, where sober operation is essential. Future enhancements could include connectivity with GPS or GSM modules for remote alerts, making it suitable for fleet management systems. Overall, this system effectively contributes to safety by helping to prevent alcohol-related incidents.

## Conclusion:

The IoT-Based Alcohol Detection and Vehicle Immobilization System represents a significant step forward in enhancing road safety and preventing alcohol-related accidents. By integrating advanced technologies like alcohol sensors, GPS, and GSM modules, this project provides a comprehensive solution to tackle the dangers of impaired driving. The system's ability to immediately immobilize a vehicle upon detecting alcohol levels and notify designated contacts ensures swift action can be taken to protect both the driver and the public.

As a proactive measure, this innovative solution is ideal for use in personal vehicles and public transportation, aiming to foster responsible driving practices and promote safer roads for everyone. With the increasing prevalence of alcohol-related incidents, the deployment of such systems can have a profound impact on public safety, ultimately saving lives and reducing the burden on emergency services.

By harnessing the power of the Internet of Things, this project not only showcases the potential of modern technology but also emphasizes our collective responsibility to prioritize safety on the roads. Together, we can pave the way for a future where responsible driving becomes the norm, ensuring a safer environment for all.

## Articles published / References :
[1] Choi, Y., Han, S.I., Kong, S.-H., Ko, H.: Driver status monitoring systems for smart vehicles using physiological sensors—a safety enhancement system from automobile manufacturers. IEEE Mag. Sig. Process. Smart Veh. Technol. (2016)

[2] Dhivya, M., Kathiravan, S.: Hybrid driver safety, vigilance and security system for vehicle. In: IEEE Sponsored 2nd International Conference on Innovations in Information Embedded and Communication Systems ICIIECS’15 (2015)

[3] Pughazendi, N., Sathishkumar, R., Balaji, S., Sathyavenkateshwaren, S., Subash Chander, S., Surendar, V.: Heart attack and alcohol detection sensor monitoring in smart transportation system using internet of things. In: IEEE International Conference on Energy, Communication, Data Analytics and Soft Computing (ICECDS-2017) (2017)

[4] Malathi, M., Sujitha, R., Revathi, M.R.: Alcohol detection and seat belt control system using Arduino. In: IEEE International Conference on Innovations in Information Embedded and Communication Systems (ICIIECS) (2017)

[5] Parakkal, P.G., Sajith Variyar, V.V.: GPS based navigation system for autonomous car. In: IEEE International Conference on Advances in Computing, Communications and Informatics (2017)

[6] Kodire, V., Bhaskaran, S., Vishwas, H.N.: GPS and ZigBee based traffic signal preemption. In: IEEE International Conference on Inventive Computational Technologies (2016)

[7] Hu, J., Xu, L., He, X., Meng, W.: Abnormal driving detection based on normalized driving behaviour. IEEE Trans. Veh. Technol. 66(8) (2017)

[8] Vishal, D., Afaque, H.S., Bhardawaj, H., Ramesh, T.K.: IoT-driven road safety system. In: International Conference on Electrical, Electronics, Communication, Computer and Optimization Techniques (2017)

[9] Chowdhury, A., Shankaran, R., Kavakli, M., Haque, M.M.: Sensor applications and physiological features in drivers’ drowsiness detection: a review. IEEE Sens. J. 18(8) (2018)

[10] Sandeep, K., Kumar, P.R., Ranjith, S.: Novel drunken driving detection and prevention models using Internet of Things. In: International Conference on Recent Trends in Electrical, Electronics and Computing Technologies (2017)

[11] Charniya, N.N., Nair, V.R.: Drunk driving and drowsiness detection. In: 2017 IEEE International Conference on Intelligent Computing and Control (I2C2) (2017)
