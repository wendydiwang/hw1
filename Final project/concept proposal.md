## This is my concept proposal for the final project.

I am planning to build a prototype of the interactive lighting system at a city on a small scale. 
see the project here: http://www.wendydiwang.com/new-page


The lights will change the brightness when it detects a moving thing coming by. the closer the brighter, the farter the daker.
I need to buld a small scale of the city with lights that contraled by  light resistor, distance sensor and interactived with mini cars.
Ideally I will also need a display to show the data that from those sensors. For the data, I need to design the visualization to reflect the traffic.The scale of the city will be based on CCA SF campus in 5 blocks, included freeways, living neighborhood and working neighborhood.

Showing here: 
   <img width="989" alt="screen shot 2017-03-15 at 22 42 37" src="https://cloud.githubusercontent.com/assets/22774491/23984408/a630db2a-09d6-11e7-9ff0-5139b348945b.png">
   
   ### the effect:
   <img width="1010" alt="1" src="https://cloud.githubusercontent.com/assets/22774491/24017658/16344a84-0a4e-11e7-9c69-18a5b427df2e.png">
   ### the effect:
   <img width="1057" alt="2" src="https://cloud.githubusercontent.com/assets/22774491/24017673/2a961584-0a4e-11e7-9339-17a71cf25c65.png">


 I have 6 weeks and 5 classes for this project before it's due.(April 27)
 the next week 
 
 ### Thursday, March 16, 2017
#### Start working: Research, mock-ups, test ideas and concepts.
 
#### Research:
 1.Find the code for arduino that make multiple tasks.
 2.Related sensors and order it.
 (I have 5 distance sensors and 10 light resistors, if every block need 4 lights I will need 100 sensors. if every sensor control 4 lights I will need 20 sensors....so expensive! ). 
 3.The material I may use. 
 
#### Mock-ups:
 test with 10 lights on 5 senses, see how is the effect.
 
 Decide the size of this prototype. 
 
 
   #### similar project:
   
   #### Future cityes lab
   ![20150521-img_7598-peterprato](https://cloud.githubusercontent.com/assets/22774491/24017830/cdfa01c2-0a4e-11e7-8d07-e7d7f8ca2a59.jpg)
   http://www.future-cities-lab.net/murmurwall
 
 ![street-lights-project-web](https://cloud.githubusercontent.com/assets/22774491/23984647/d9a99f72-09d7-11e7-9391-2676abcadb9d.jpg)
(http://sigmatechnology.se/news/new-iot-traffic-light-system-will-save-energy-and-increase-traffic-awareness-on-the-swedish-roads/)
 

   #### and this:
   ![img_5748](https://cloud.githubusercontent.com/assets/22774491/23984676/09266dc0-09d8-11e7-924b-8d67c1c95c69.jpg)
   (I forgot the link of this  image ... it's not mine anyway.)

 
 

 Spring Break: Thursday, March 22, 2017
 Research, build mock-ups, test ideas and concepts
 
 Thursday, March 30, 
### test, repair, iterate. make working mock-up
 
## test code
int currentBrightness = 55;
int newBrightness = currentBrightness;
int fadeSpeed = 1;


void setup() {
  // put your setup code here, to run once:

  Serial.begin(9600);

  analogWrite(9, currentBrightness);

}

void loop() {
  // put your main code here, to run repeatedly:

  // Read the sensor valure
  int sensorVal = analogRead(0);

  Serial.println(sensorVal);

  // Cheching the distance and adjusting the new value to fade to
  if (sensorVal > 350) {
    newBrightness = 255;
  }
  else {
    newBrightness = 55;
  }



  //adjusting the current brightness based on what direction it should go in
  if (newBrightness > currentBrightness) {
    currentBrightness = currentBrightness + fadeSpeed;
  }
  else if (newBrightness < currentBrightness){
    currentBrightness = currentBrightness - fadeSpeed;
  }


  // Writes the brightness value to the LED
  analogWrite(9, currentBrightness);


  //delay(10);
}

 ### Thursday, April 6, 2017
 Debug and refine mockup, start on prototype
 
 ### Thursday, April 13, 2017 
 Passover: work week

 ### Thursday, April 21, 2017
 The last class to refine it.
