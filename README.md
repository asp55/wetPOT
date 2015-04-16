# wetPOT
Soil moisture potentiometer

An open-source hardware probe for determining soil moisture.

# Attribution
Based on work started by Mike Rice, and M.A. de Pablo in this thread: http://forum.arduino.cc/index.php?topic=37975.0

# License
CC: BY-SA

# Example Arduino Code:
```Arduino
/*  Soil Moisture measurement v0.2 20150330
 Simple circuit
 
 Started by Mike Rice, October 14, 2009
 Modified by M.A. de Pablo, October 17, 2009
 Modified by Andrew S. Parnell, March 30, 2015
 
 Circuit:
 Connect SIG pin to analog 0
 Connect other 2 pins to digital 2 & 3
 */


#define moisture_input 0
#define divider_top 2
#define divider_bottom 3

// Not calibrated at this moment
// To calibrate, get reading with probe in the air (or ideally dry soil) for dryValue, and probe in full water for wetValue
#define dryValue 0
#define wetValue 1024

int moisture; // analogical value obtained from the experiment

int SoilMoisture(){
  int reading;
  // set driver pins to outputs
  pinMode(divider_top,OUTPUT);
  pinMode(divider_bottom,OUTPUT);

  // drive a current through the divider in one direction
  digitalWrite(divider_top,LOW);
  digitalWrite(divider_bottom,HIGH);

  // wait a moment for capacitance effects to settle
  delay(1000);

  // take a reading
  reading=analogRead(moisture_input);

  // reverse the current
  digitalWrite(divider_top,HIGH);
  digitalWrite(divider_bottom,LOW);

  // give as much time in 'reverse' as in 'forward' to reduce corrosion
  delay(1000);

  // stop the current
  digitalWrite(divider_bottom,LOW);

  return reading;
}


void setup () {
  Serial.begin(9600);

}

void loop () {
  moisture=SoilMoisture(); // assign the result of SoilMoisture() to the global variable 'moisture'
  Serial.print("Soil moisture Analog Value: ");
  Serial.print(moisture); // print the analogical measurement of the experiment
  Serial.println();

  int moisturePercent = map(moisture, dryValue, wetValue, 0, 100);
  Serial.print("Soil moisture Percent Value: ");
  Serial.print(moisturePercent); // print the percent based measurement of the experiment
  Serial.println();

  delay(1000);
}
```