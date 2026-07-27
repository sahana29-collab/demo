# LPG LEAKAGE CONTROL AND ALERT SYSTEM


#Project Overview:

  This project is an Arduino-based gas leakage detection and safety system. It continuously monitors LPG gas leakage using     an MQ2 gas sensor. When gas is detected above a predefined threshold, the system automatically activates a buzzer and an     exhaust fan, rotates a servo motor to turn OFF the gas regulator, and sends an SMS alert to the owner's mobile phone using   the SIM800L GSM module. The system helps reduce the risk of fire accidents and improves household safety.


#Features:-

  1.Detects LPG gas leakage using the MQ2 gas sensor. 

  2.Activates a buzzer to alert nearby people.

  3.Turns ON an exhaust fan to remove leaked gas.

  4.Automatically rotates the gas regulator using a servo motor.

  5.Sends an SMS alert to the owner's mobile phone using the SIM800L GSM module.

  6.Low-cost and easy to build.


#Components Used:-

  1.Arduino UNO

  2.MQ2 Gas Sensor

  3.SIM800L GSM Module

  4.Servo Motor (180°)

  5.DC Fan

  6.Buzzer

  7.Jumper Wires

  8.Breadboard

  9.Power Supply

  10.LPG Gas Regulator Model


#Working Principle:-

  1.The MQ2 sensor continuously monitors the surrounding air.

  2.When gas concentration exceeds the threshold value, the Arduino detects the leakage.

  3.The buzzer turns ON to provide an audible alert.

  4.The exhaust fan starts to remove the leaked gas.

  5.The servo motor rotates to close the gas regulator.

  6.The SIM800L GSM module sends an SMS notification to the registered mobile number.

  7.Once the gas level returns to normal, the fan and buzzer turn OFF.


#Software Used:-

  1.Arduino IDE

  2.Embedded C (Arduino Programming Language)


#Applications:-

  1.Home LPG safety systems

  2.Restaurants and hotels

  3.Laboratories

  4.Industrial gas monitoring

  5.Kitchens


#Future Improvements:-

  1.Mobile application integration.

  2.IoT monitoring using Wi-Fi.

  3.Fire detection sensor 

  4.Automatic emergency call feature.

  5.Cloud-based monitoring and alerts.

#Code:-

  #include <Servo.h>
  #include <SoftwareSerial.h>

  Servo myServo;

  // GSM Module Pins
  SoftwareSerial gsm(10, 11); // RX, TX

  // Component Pins
  int gasSensor = A0;
  int fan = 7;
  int buzzer = 8;

  // Threshold
  int threshold = 50;

  // SMS flag
  bool alertSent = false;

  void setup() {
  
    pinMode(fan, OUTPUT);
    pinMode(buzzer, OUTPUT);

    digitalWrite(fan, LOW);
    digitalWrite(buzzer, LOW);

    Serial.begin(9600);

    gsm.begin(9600);

    // Servo setup
    myServo.attach(9);

    // Stable startup position
    myServo.write(90);

    delay(1000);

    // Prevent automatic movement
    myServo.detach();

    Serial.println("System Ready");
  }

  void loop() {

    // Read MQ2 value
    int gasValue = analogRead(gasSensor);

    Serial.println(gasValue);

    // GAS DETECTED
    if (gasValue > threshold) {

      // Fan ON
      digitalWrite(fan, HIGH);

      // Buzzer ON
      digitalWrite(buzzer, HIGH);

      // Servo rotate
      myServo.attach(9);

      myServo.write(0);

      // Send SMS only once
      if (!alertSent) {

        sendSMS();

        alertSent = true;
      }
    }

    // NORMAL CONDITION
    else {

      // Fan OFF
      digitalWrite(fan, LOW);

      // Buzzer OFF
      digitalWrite(buzzer, LOW);

      // Stop servo signal
      myServo.detach();

      alertSent = false;
    }

    delay(500);
  }

  // SMS Function
  void sendSMS() {

    gsm.println("AT+CMGF=1");

    delay(1000);

    // Replace with your mobile number
    gsm.println("AT+CMGS=\"+91XXXXXXXXXX\"");

    delay(1000);

    gsm.println("Gas Leakage Detected!");

    delay(1000);

    // Send SMS
    gsm.write(26);

    delay(3000);
  }


<img width="470" height="754" alt="Screenshot 2026-07-27 093632" src="https://github.com/user-attachments/assets/b5e65aa1-d4ad-4f5c-9b68-4c35b9f49666" />



<img width="1131" height="755" alt="Screenshot 2026-07-27 093411" src="https://github.com/user-attachments/assets/88d193fd-7ce2-45b5-ab49-f95c03304eb1" />

