## audrino code:

//sensor1
#define echoPin1 12// 
#define trigPin1 11 //

//sensor2
#define echoPin2 10 // 
#define trigPin2 9 //

//sensor3
#define echoPin3 6 // 
#define trigPin3 5 //

//sensor4
#define echoPin4 4// 
#define trigPin4 3 //


long duration1, duration2, duration3, duration4, distance1, distance2,  distance3, distance4 ; // Duration used to calculate distance



void setup() {
  // initialize the serial communication:
  Serial.begin(9600);
  
  pinMode(trigPin1, OUTPUT);//sensor1
  pinMode(echoPin1, INPUT);

  pinMode(trigPin2, OUTPUT);//sensor2
  pinMode(echoPin2, INPUT);

  pinMode(trigPin3, OUTPUT);//sensor3
  pinMode(echoPin3, INPUT);

  pinMode(trigPin4, OUTPUT);//sensor4
  pinMode(echoPin4, INPUT);
}

void loop() {

  digitalWrite(trigPin1, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin1, HIGH);
  delayMicroseconds(2);
  digitalWrite(trigPin1, LOW);
  duration1 = pulseIn(echoPin1, HIGH);
  //Calculate the distance (in cm) based on the speed of sound.
  distance1 = (duration1 / 2) / 29.1;

  digitalWrite(trigPin2, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin2, HIGH);
  delayMicroseconds(2);
  digitalWrite(trigPin2, LOW);
  duration2 = pulseIn(echoPin2, HIGH);
  //Calculate the distance (in cm) based on the speed of sound.
  distance2 = (duration2 / 2) / 29.1;

  digitalWrite(trigPin3, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin3, HIGH);
  delayMicroseconds(2);
  digitalWrite(trigPin3, LOW);
  duration3 = pulseIn(echoPin3, HIGH);
  //Calculate the distance (in cm) based on the speed of sound.
  distance3 = (duration3 / 2) / 29.1;

  digitalWrite(trigPin4, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin4, HIGH);
  delayMicroseconds(2);
  digitalWrite(trigPin2, LOW);
  duration4 = pulseIn(echoPin4, HIGH);
  //Calculate the distance (in cm) based on the speed of sound.
  distance4 = (duration4 / 2) / 29.1;
//
// if (distance1 < 200 && distance2 < 200 && distance3 < 200 && distance4 < 200) {
    Serial.print(distance1);//print the data that less than 2 meters from sensor1-4
    Serial.print(',');
    Serial.print(distance2);
     Serial.print(',');
    Serial.print(distance3);
     Serial.print(',');
    Serial.println(distance4);

// }
  }


## processing code:
#### part1 :

//original visual code by Raven Kwok aka Guo, Ruiwen, Jerome Herr, p5art.tumblr.com, modified by Wendy Di Wang


Serial myPort;        // The serial port
int xPos = 100;         // horizontal position of the graph
float inByte = 0;

float freq;           // sine wave frequency 
float distance1;
float distance2;
float distance3;
float distance4;

Minim minim;
AudioOutput out;
ArrayList<Particle> pts;
boolean onPressed;
color[] palette = { 
  #001ABF, #0095FF, #0017A6,  #EF00DE,   #A200FF, #5700B0, #007EF5, #FF00FF, #00E7FF, #FF00BC, #3700BC,  #8400ff, #5400ff, #0060ff
}; // set up colors of thoes floating dots

void setup() {
  fullScreen();
  frameRate(30);

  pts = new ArrayList<Particle>();
  
// get data from arduino
  myPort = new Serial(this, Serial.list()[1], 9600);
  myPort.bufferUntil('\n');
  minim = new Minim(this);
  out = minim.getLineOut(Minim.STEREO);
}

void draw() {
  println(distance1); // print the distance from each sensor
  println(distance2);
  println(distance3);
  println(distance4);
  background(0);

  if (distance1>0 && distance1<15)// when something is in front of the sensor 1 from 0 to 15 cms, paly song1
  {
    print("Song1\t"); // show song1 is paly in the monitor
   Song1();

    smooth();
  }


  if (distance1>15 && distance1<30)// when something is in front of the sensor 1 from 15 to 30 cms, paly song2
  {
    print("Song2\t");
   Song2();

    smooth();
  }

  if (distance1>30 && distance1<45)
  {
    print("Song3\t");
    Song3();

    smooth();
  }
  
   if (distance1>45 && distance1<60)
  {
    print("Song4\t");
    Song4();

    smooth();
  }



  if (distance2>0 && distance2<15)
  {
    print("Rhythm1\t");
    Rhythm1();

    smooth();
  }
  
   if (distance2>15 && distance2<30)
  {
    print("Rhythm2\t");
    Rhythm2();

    smooth();
  }
  
   if (distance2>30 && distance2<45)
  {
    print("Rhythm3\t");
    Rhythm3();

    smooth();
  }
  
   if (distance2>45 && distance2<60)
  {
    print("Rhythm4\t");
    Rhythm4();

    smooth();
  }

 if (distance2>30 && distance2<40 && distance3>30 && distance3<40)
  {
    print("R1\t");
    R1();

    smooth();
  }
  
   if (distance3>0 && distance3<15)
  {
    print("CPR3\t");
    CPR3();

    smooth();
  }
  
   
   if (distance3>15 && distance3<30)
  {
    print("CPR2\t");
    CPR2();

    smooth();
  }
  
   
   if (distance3>30 && distance3<45)
  {
    print("CPR1\t");
    CPR1();

    smooth();
  }

  if (distance4>0 && distance4<45)
  {
    print("CPL1\t");
    CPL1();

    smooth();
  }
  if (distance3>30 && distance3<40 && distance4>30 && distance4<40)
  {
    print("CT1\t");
    CT1();

    smooth();
  }
  
  
  // active the flating dots when something is in front of either of thoes sensor less than 2.8 meters.
  if (distance1>0 && distance1<280 || distance2>0 && distance2<280 || distance3>0 && distance3<280 || distance4>0 && distance4<280)
  {
    onPressed = true;
  } else {
    onPressed = false;
  }

  println();
  shine();
}

//floating dots setup
void shine() {
  if (onPressed) {
    for (int i=0; i<10; i++) {
      Particle newP = new Particle(random(0, 1960), random(0, 1080), i+pts.size(), i+pts.size());
      pts.add(newP);
    }
  }

  for (int i=0; i<pts.size (); i++) {
    Particle p = pts.get(i);
    p.update();
    p.display();
  }

  for (int i=pts.size ()-1; i>-1; i--) {
    Particle p = pts.get(i);
    if (p.dead) {
      pts.remove(i);
    }
  }
}

// song1 setup
void Song1() {
  
  out.playNote( 0.0, 1.2, new SineInstrument( Frequency.ofPitch( "D6" ).asHz() ) );
  out.playNote( 0.0, 1.2, new SineInstrument( Frequency.ofPitch( "D5" ).asHz() ) );

  out.playNote( 1.0, 0.5, new SineInstrument( Frequency.ofPitch( "C6" ).asHz() ) );
  out.playNote( 1.0, 0.5, new SineInstrument( Frequency.ofPitch( "C5" ).asHz() ) );

  out.playNote( 1.3, 1.0, new SineInstrument( Frequency.ofPitch( "B5" ).asHz() ) );
  out.playNote( 1.3, 1.0, new SineInstrument( Frequency.ofPitch( "B4" ).asHz() ) );


  out.playNote( 2.0, 1.0, new SineInstrument( Frequency.ofPitch( "A5" ).asHz() ) );
  out.playNote( 2.0, 1.0, new SineInstrument( Frequency.ofPitch( "A4" ).asHz() ) );
  // resume time after a bunch of notes are added at once
  
  delay(500);
}

void Song2() {
  out.playNote( 0.0, 1.2, new SineInstrument( Frequency.ofPitch( "D6" ).asHz() ) );
  out.playNote( 0.0, 1.2, new SineInstrument( Frequency.ofPitch( "D5" ).asHz() ) );


  out.playNote( 1.0, 0.3, new SineInstrument( Frequency.ofPitch( "E6" ).asHz() ) );
  out.playNote( 1.2, 0.3, new SineInstrument( Frequency.ofPitch( "D6" ).asHz() ) );
  out.playNote( 1.4, 0.3, new SineInstrument( Frequency.ofPitch( "C6" ).asHz() ) );
  out.playNote( 1.6, 0.3, new SineInstrument( Frequency.ofPitch( "D6" ).asHz() ) );
  out.playNote( 1.8, 0.3, new SineInstrument( Frequency.ofPitch( "C6" ).asHz() ) );


  out.playNote( 2.0, 0.9, new SineInstrument( Frequency.ofPitch( "B5" ).asHz() ) );
  out.playNote( 2.0, 0.9, new SineInstrument( Frequency.ofPitch( "B4" ).asHz() ) );


  out.playNote( 2.7, 1.2, new SineInstrument( Frequency.ofPitch( "A5" ).asHz() ) );
  out.playNote( 2.7, 1.2, new SineInstrument( Frequency.ofPitch( "A4" ).asHz() ) );
  delay(500);
}

void Song3() {

  out.playNote( 0.0, 0.9, new SineInstrument( Frequency.ofPitch( "C6" ).asHz() ) );
  out.playNote( 0.0, 0.9, new SineInstrument( Frequency.ofPitch( "C5" ).asHz() ) );


  out.playNote( 0.9, 0.9, new SineInstrument( Frequency.ofPitch( "B5" ).asHz() ) );
  out.playNote( 0.9, 0.9, new SineInstrument( Frequency.ofPitch( "B4" ).asHz() ) );

  out.playNote( 1.3, 1.2, new SineInstrument( Frequency.ofPitch( "E5" ).asHz() ) );
  out.playNote( 1.3, 1.2, new SineInstrument( Frequency.ofPitch( "E4" ).asHz() ) );
  delay(500);
}

void Song4() {
  out.playNote(0.0, 1.0, new SineInstrument( Frequency.ofPitch( "C6" ).asHz() ) );
  out.playNote( 0.0, 1.0, new SineInstrument( Frequency.ofPitch( "C5" ).asHz() ) );

  out.playNote( 0.8, 0.3, new SineInstrument( Frequency.ofPitch( "D6" ).asHz() ) );
  out.playNote( 1.0, 0.3, new SineInstrument( Frequency.ofPitch( "C6" ).asHz() ) );
  out.playNote( 1.2, 0.3, new SineInstrument( Frequency.ofPitch( "B5" ).asHz() ) );
  out.playNote( 1.4, 0.3, new SineInstrument( Frequency.ofPitch( "C6" ).asHz() ) );
  out.playNote( 1.6, 0.3, new SineInstrument( Frequency.ofPitch( "B5" ).asHz() ) );

  out.playNote( 1.8, 0.9, new SineInstrument( Frequency.ofPitch( "E5" ).asHz() ) );
  out.playNote( 1.9, 0.9, new SineInstrument( Frequency.ofPitch( "E6" ).asHz() ) );
  out.playNote( 2.0, 0.9, new SineInstrument( Frequency.ofPitch( "E4" ).asHz() ) );
  delay(500);
}



void Rhythm1() {
  out.playNote( 0.0, 1.2, new SineInstrument( Frequency.ofPitch( "D4" ).asHz() ) );

  out.playNote( 0.7, 1.5, new SineInstrument( Frequency.ofPitch( "C4" ).asHz() ) );
  out.playNote( 0.7, 1.5, new SineInstrument( Frequency.ofPitch( "E4" ).asHz() ) );
  out.playNote(0.7, 1.5, new SineInstrument( Frequency.ofPitch( "A4" ).asHz() ) );
  delay(500);
}

void Rhythm2() {

  out.playNote( 0.0, 1.2, new SineInstrument( Frequency.ofPitch( "A3" ).asHz() ) );

  out.playNote( 0.9, 1.5, new SineInstrument( Frequency.ofPitch( "C4" ).asHz() ) );
  out.playNote( 0.9, 1.5, new SineInstrument( Frequency.ofPitch( "E4" ).asHz() ) );
  out.playNote( 0.9, 1.5, new SineInstrument( Frequency.ofPitch( "A4" ).asHz() ) );
  delay(500);
}

void Rhythm3() {
  out.playNote( 0.0, 0.9, new SineInstrument( Frequency.ofPitch( "F4" ).asHz() ) );

  out.playNote( 0.7, 0.9, new SineInstrument( Frequency.ofPitch( "A4" ).asHz() ) );
  out.playNote( 0.7, 0.9, new SineInstrument( Frequency.ofPitch( "C5" ).asHz() ) );
  delay(500);
}

void Rhythm4() {
  out.playNote( 0.0, 1.2, new SineInstrument( Frequency.ofPitch( "C4" ).asHz() ) );

  out.playNote( 0.7, 1.2, new SineInstrument( Frequency.ofPitch( "E4" ).asHz() ) );
  out.playNote( 0.7, 1.2, new SineInstrument( Frequency.ofPitch( "G4" ).asHz() ) );
  out.playNote( 0.7, 1.2, new SineInstrument( Frequency.ofPitch( "C5" ).asHz() ) );
  delay(500);
}

void CPL1(){

  out.playNote( 0.0, 2.0, new SineInstrument(  Frequency.ofPitch( "C3" ).asHz() ) );
  out.playNote( 0.0, 2.0, new SineInstrument(  Frequency.ofPitch( "C4" ).asHz() ) );
   out.playNote( 0.0, 2.0, new SineInstrument(  Frequency.ofPitch( "E3" ).asHz() ) );
  out.playNote( 0.0, 2.0, new SineInstrument(  Frequency.ofPitch( "E4" ).asHz() ) );

  out.playNote( 1.9, 2.0, new SineInstrument(  Frequency.ofPitch( "A3" ).asHz() ) );
  out.playNote( 1.9, 2.0, new SineInstrument(  Frequency.ofPitch( "A2" ).asHz() ) );

  out.playNote( 3.9, 2.0, new SineInstrument(  Frequency.ofPitch( "B3" ).asHz() ) );
  out.playNote( 3.9, 2.0, new SineInstrument(  Frequency.ofPitch( "B4" ).asHz() ) );
  out.playNote( 3.9, 2.0, new SineInstrument(  Frequency.ofPitch( "E3" ).asHz() ) );
  out.playNote( 3.9, 2.0, new SineInstrument(  Frequency.ofPitch( "E4" ).asHz() ) );
  
  out.playNote(  5.9, 2.0, new SineInstrument(  Frequency.ofPitch( "G3" ).asHz() ) );
  out.playNote( 5.9, 2.0, new SineInstrument(  Frequency.ofPitch( "G2" ).asHz() ) );

  out.playNote(  7.9, 2.0, new SineInstrument(  Frequency.ofPitch( "A3" ).asHz() ) );
  out.playNote(  7.9, 2.0, new SineInstrument(  Frequency.ofPitch( "A2" ).asHz() ) );
    out.playNote(  7.9, 2.0, new SineInstrument(  Frequency.ofPitch( "C3" ).asHz() ) );
  out.playNote(  7.9, 2.0, new SineInstrument(  Frequency.ofPitch( "C4" ).asHz() ) );
    out.playNote(  7.9, 2.0, new SineInstrument(  Frequency.ofPitch( "E3" ).asHz() ) );
  out.playNote(  7.9, 2.0, new SineInstrument(  Frequency.ofPitch( "E4" ).asHz() ) );

  out.playNote( 9.9, 2.0, new SineInstrument(  Frequency.ofPitch( "E3" ).asHz() ) );
  out.playNote( 9.9, 2.0, new SineInstrument(  Frequency.ofPitch( "E2" ).asHz() ) );
  
  out.playNote(  11.9, 4.0, new SineInstrument(  Frequency.ofPitch( "F3" ).asHz() ) );
  out.playNote(  11.9, 4.0, new SineInstrument(  Frequency.ofPitch( "F2" ).asHz() ) );
    out.playNote(  11.9, 2.0, new SineInstrument(  Frequency.ofPitch( "E3" ).asHz() ) );
  out.playNote(  11.9, 2.0, new SineInstrument(  Frequency.ofPitch( "E3" ).asHz() ) );

  out.playNote( 13.9, 2.0, new SineInstrument(  Frequency.ofPitch( "D4" ).asHz() ) );
  out.playNote( 13.9, 2.0, new SineInstrument(  Frequency.ofPitch( "D3" ).asHz() ) );

  delay(1500);
}

void CPR1() {
  out.playNote(0.0, 1.0, new SineInstrument(  Frequency.ofPitch( "E5" ).asHz() ) );
  out.playNote(0.0, 1.0, new SineInstrument(  Frequency.ofPitch( "E4" ).asHz() ) );
  out.playNote( 0.0, 1.0, new SineInstrument(  Frequency.ofPitch( "G5" ).asHz() ) );
  out.playNote( 0.0, 1.0, new SineInstrument(  Frequency.ofPitch( "G4" ).asHz() ) );


  out.playNote( 0.9, 2.0, new SineInstrument(  Frequency.ofPitch( "C6" ).asHz() ) );
  out.playNote( 0.9, 2.0, new SineInstrument(  Frequency.ofPitch( "C5" ).asHz() ) );

  out.playNote( 2.7, 1.0, new SineInstrument(  Frequency.ofPitch( "G6" ).asHz() ) );
  out.playNote( 2.7, 1.0, new SineInstrument(  Frequency.ofPitch( "G5" ).asHz() ) );

  out.playNote( 3.6, 1.0, new SineInstrument(  Frequency.ofPitch( "G6" ).asHz() ) );
  out.playNote( 3.6, 1.0, new SineInstrument(  Frequency.ofPitch( "G5" ).asHz() ) );

  out.playNote(  4.5, 2.0, new SineInstrument(  Frequency.ofPitch( "B5" ).asHz() ) );
  out.playNote( 4.5, 2.0, new SineInstrument(  Frequency.ofPitch( "B4" ).asHz() ) );

  out.playNote( 6.3, 1.0, new SineInstrument(  Frequency.ofPitch( "G6" ).asHz() ) );
  out.playNote( 6.3, 1.0, new SineInstrument(  Frequency.ofPitch( "G5" ).asHz() ) );

  out.playNote( 7.2, 1.0, new SineInstrument(  Frequency.ofPitch( "G6" ).asHz() ) );
  out.playNote( 7.2, 1.0, new SineInstrument(  Frequency.ofPitch( "G5" ).asHz() ) );

  out.playNote(  8.1, 2.0, new SineInstrument(  Frequency.ofPitch( "C5" ).asHz() ) );
  out.playNote(  8.1, 2.0, new SineInstrument(  Frequency.ofPitch( "C4" ).asHz() ) );

  out.playNote( 10.0, 1.0, new SineInstrument(  Frequency.ofPitch( "G6" ).asHz() ) );
  out.playNote( 10.0, 1.0, new SineInstrument(  Frequency.ofPitch( "G5" ).asHz() ) );

  out.playNote( 10.9, 1.0, new SineInstrument(  Frequency.ofPitch( "G6" ).asHz() ) );
  out.playNote( 10.9, 1.0, new SineInstrument(  Frequency.ofPitch( "G5" ).asHz() ) );

  out.playNote(  11.8, 2.0, new SineInstrument(  Frequency.ofPitch( "C5" ).asHz() ) );
  out.playNote(  11.8, 2.0, new SineInstrument(  Frequency.ofPitch( "C4" ).asHz() ) );
  
  out.playNote(  13.6, 1.0, new SineInstrument(  Frequency.ofPitch( "A5" ).asHz() ) );
  out.playNote(  13.6, 1.0, new SineInstrument(  Frequency.ofPitch( "A4" ).asHz() ) );

  delay(1500);
}

void CPR2() {
  out.playNote(0.0, 1.0, new SineInstrument(  Frequency.ofPitch( "A5" ).asHz() ) );
  out.playNote(0.0, 1.0, new SineInstrument(  Frequency.ofPitch( "A6" ).asHz() ) );

  out.playNote( 0.9, 1.0, new SineInstrument(  Frequency.ofPitch( "G5" ).asHz() ) );
  out.playNote( 0.9, 1.0, new SineInstrument(  Frequency.ofPitch( "G6" ).asHz() ) );

  out.playNote( 1.7, 1.0, new SineInstrument(  Frequency.ofPitch( "D6" ).asHz() ) );
  out.playNote( 1.7, 1.0, new SineInstrument(  Frequency.ofPitch( "D7" ).asHz() ) );

  out.playNote( 2.6, 1.0, new SineInstrument(  Frequency.ofPitch( "C6" ).asHz() ) );
  out.playNote( 2.6, 1.0, new SineInstrument(  Frequency.ofPitch( "C7" ).asHz() ) );

  out.playNote(  3.5, 1.0, new SineInstrument(  Frequency.ofPitch( "B5" ).asHz() ) );
  out.playNote( 3.5, 1.0, new SineInstrument(  Frequency.ofPitch( "B6" ).asHz() ) );

  out.playNote( 4.4, 2.0, new SineInstrument(  Frequency.ofPitch( "G5" ).asHz() ) );
  out.playNote( 4.4, 2.0, new SineInstrument(  Frequency.ofPitch( "G6" ).asHz() ) );

  out.playNote( 6.2, 1.0, new SineInstrument(  Frequency.ofPitch( "C6" ).asHz() ) );
  out.playNote( 6.2, 1.0, new SineInstrument(  Frequency.ofPitch( "C7" ).asHz() ) );

  out.playNote(  7.1, 1.0, new SineInstrument(  Frequency.ofPitch( "B5" ).asHz() ) );
  out.playNote(  7.1, 1.0, new SineInstrument(  Frequency.ofPitch( "B6" ).asHz() ) );

  out.playNote( 8.0, 2.0, new SineInstrument(  Frequency.ofPitch( "A6" ).asHz() ) );
  out.playNote( 8.0, 2.0, new SineInstrument(  Frequency.ofPitch( "A5" ).asHz() ) );

  out.playNote( 9.9, 1.0, new SineInstrument(  Frequency.ofPitch( "D7" ).asHz() ) );
  out.playNote( 9.9, 1.0, new SineInstrument(  Frequency.ofPitch( "D6" ).asHz() ) );

  out.playNote(  10.8, 1.0, new SineInstrument(  Frequency.ofPitch( "C7" ).asHz() ) );
  out.playNote(  10.8, 1.0, new SineInstrument(  Frequency.ofPitch( "C6" ).asHz() ) );
  
  out.playNote(  11.7, 2.0, new SineInstrument(  Frequency.ofPitch( "A5" ).asHz() ) );
  out.playNote(  11.7, 2.0, new SineInstrument(  Frequency.ofPitch( "A6" ).asHz() ) );

  delay(1500);
}


void CPR3(){

  out.playNote( 0.9, 1.0, new SineInstrument(  Frequency.ofPitch( "G5" ).asHz() ) );
  out.playNote( 0.9, 1.0, new SineInstrument(  Frequency.ofPitch( "G4" ).asHz() ) );

  out.playNote( 1.7, 1.0, new SineInstrument(  Frequency.ofPitch( "D6" ).asHz() ) );
  out.playNote( 1.7, 1.0, new SineInstrument(  Frequency.ofPitch( "D5" ).asHz() ) );

  out.playNote( 2.6, 1.0, new SineInstrument(  Frequency.ofPitch( "C6" ).asHz() ) );
  out.playNote( 2.6, 1.0, new SineInstrument(  Frequency.ofPitch( "C5" ).asHz() ) );

  out.playNote(  3.5, 1.0, new SineInstrument(  Frequency.ofPitch( "B5" ).asHz() ) );
  out.playNote( 3.5, 1.0, new SineInstrument(  Frequency.ofPitch( "B4" ).asHz() ) );

  out.playNote(  4.4, 2.0, new SineInstrument(  Frequency.ofPitch( "B5" ).asHz() ) );
  out.playNote(  4.4, 2.0, new SineInstrument(  Frequency.ofPitch( "B4" ).asHz() ) );

  out.playNote( 6.2, 2.0, new SineInstrument(  Frequency.ofPitch( "A4" ).asHz() ) );
  out.playNote( 6.2, 2.0, new SineInstrument(  Frequency.ofPitch( "A5" ).asHz() ) );

  delay(1500);
}

void R1() {
  out.playNote( 0.0, 2.0, new RInstrument(  Frequency.ofPitch( "A2" ).asHz() ) );
  out.playNote( 0.0, 2.0, new RInstrument(  Frequency.ofPitch( "C3" ).asHz() ) );
  out.playNote(0.0, 2.0, new RInstrument(  Frequency.ofPitch( "E3" ).asHz() ) );
  out.playNote( 0.0, 2.0, new RInstrument(  Frequency.ofPitch( "A3" ).asHz() ) );


  out.playNote( 1.0, 2.0, new RInstrument(  Frequency.ofPitch( "A2" ).asHz() ) );
  out.playNote( 1.0, 2.0, new RInstrument(  Frequency.ofPitch( "C3" ).asHz() ) );
  out.playNote(1.0, 2.0, new RInstrument(  Frequency.ofPitch( "E3" ).asHz() ) );
  out.playNote( 1.0, 2.0, new RInstrument(  Frequency.ofPitch( "A3" ).asHz() ) );


  out.playNote( 2.0, 2.0, new RInstrument(  Frequency.ofPitch( "A2" ).asHz() ) );
  out.playNote( 2.0, 2.0, new RInstrument(  Frequency.ofPitch( "C3" ).asHz() ) );
  out.playNote( 2.0, 2.0, new RInstrument(  Frequency.ofPitch( "E3" ).asHz() ) );
  out.playNote( 2.0, 2.0, new RInstrument(  Frequency.ofPitch( "A3" ).asHz() ) );

  out.playNote(  3.0, 2.0, new RInstrument(  Frequency.ofPitch( "A2" ).asHz() ) );
  out.playNote(  3.0, 2.0, new RInstrument(  Frequency.ofPitch( "C3" ).asHz() ) );
  out.playNote(  3.0, 2.0, new RInstrument(  Frequency.ofPitch( "E3" ).asHz() ) );
  out.playNote( 3.0, 2.0, new RInstrument(  Frequency.ofPitch( "A3" ).asHz() ) );
  
   out.playNote( 4.0, 2.0, new RInstrument(  Frequency.ofPitch( "A2" ).asHz() ) );
  out.playNote( 4.0, 2.0, new RInstrument(  Frequency.ofPitch( "C3" ).asHz() ) );
  out.playNote(4.0, 2.0, new RInstrument(  Frequency.ofPitch( "E3" ).asHz() ) );
  out.playNote( 4.0, 2.0, new RInstrument(  Frequency.ofPitch( "A3" ).asHz() ) );


  out.playNote( 5.0, 2.0, new RInstrument(  Frequency.ofPitch( "A2" ).asHz() ) );
  out.playNote( 5.0, 2.0, new RInstrument(  Frequency.ofPitch( "C3" ).asHz() ) );
  out.playNote(5.0, 2.0, new RInstrument(  Frequency.ofPitch( "E3" ).asHz() ) );
  out.playNote( 5.0, 2.0, new RInstrument(  Frequency.ofPitch( "A3" ).asHz() ) );


  out.playNote( 6.0, 2.0, new RInstrument(  Frequency.ofPitch( "A2" ).asHz() ) );
  out.playNote( 6.0, 2.0, new RInstrument(  Frequency.ofPitch( "C3" ).asHz() ) );
  out.playNote( 6.0, 2.0, new RInstrument(  Frequency.ofPitch( "E3" ).asHz() ) );
  out.playNote( 6.0, 2.0, new RInstrument(  Frequency.ofPitch( "A3" ).asHz() ) );

  out.playNote(  7.0, 2.0, new RInstrument(  Frequency.ofPitch( "A2" ).asHz() ) );
  out.playNote(  7.0, 2.0, new RInstrument(  Frequency.ofPitch( "C3" ).asHz() ) );
  out.playNote(  7.0, 2.0, new RInstrument(  Frequency.ofPitch( "E3" ).asHz() ) );
  out.playNote( 7.0, 2.0, new RInstrument(  Frequency.ofPitch( "A3" ).asHz() ) );
  
  out.playNote( 8.0, 2.0, new RInstrument(  Frequency.ofPitch( "G2" ).asHz() ) );
  out.playNote( 8.0, 2.0, new RInstrument(  Frequency.ofPitch( "B2" ).asHz() ) );  
  out.playNote(8.0, 2.0, new RInstrument(  Frequency.ofPitch( "D3" ).asHz() ) );
  out.playNote( 8.0, 2.0, new RInstrument(  Frequency.ofPitch( "G3" ).asHz() ) );


  out.playNote( 9.0, 2.0, new RInstrument(  Frequency.ofPitch( "G2" ).asHz() ) );
  out.playNote( 9.0, 2.0, new RInstrument(  Frequency.ofPitch( "B2" ).asHz() ) );
  out.playNote(9.0, 2.0, new RInstrument(  Frequency.ofPitch( "D3" ).asHz() ) );
  out.playNote( 9.0, 2.0, new RInstrument(  Frequency.ofPitch( "G3" ).asHz() ) );


  out.playNote( 10.0, 2.0, new RInstrument(  Frequency.ofPitch( "G2" ).asHz() ) );
  out.playNote( 10.0, 2.0, new RInstrument(  Frequency.ofPitch( "B2" ).asHz() ) );
  out.playNote( 10.0, 2.0, new RInstrument(  Frequency.ofPitch( "D3" ).asHz() ) );
  out.playNote( 10.0, 2.0, new RInstrument(  Frequency.ofPitch( "G3" ).asHz() ) );

  out.playNote(  11.0, 2.0, new RInstrument(  Frequency.ofPitch( "G2" ).asHz() ) );
  out.playNote(  11.0, 2.0, new RInstrument(  Frequency.ofPitch( "B2" ).asHz() ) );
  out.playNote(  11.0, 2.0, new RInstrument(  Frequency.ofPitch( "D3" ).asHz() ) );
  out.playNote( 11.0, 2.0, new SineInstrument(  Frequency.ofPitch( "G3" ).asHz() ) );
  
   out.playNote( 12.0, 2.0, new RInstrument(  Frequency.ofPitch( "G2" ).asHz() ) );
  out.playNote( 12.0, 2.0, new RInstrument(  Frequency.ofPitch( "B2" ).asHz() ) );  
  out.playNote(12.0, 2.0, new RInstrument(  Frequency.ofPitch( "D3" ).asHz() ) );
  out.playNote( 12.0, 2.0, new RInstrument(  Frequency.ofPitch( "G3" ).asHz() ) );


  out.playNote( 13.0, 2.0, new RInstrument(  Frequency.ofPitch( "G2" ).asHz() ) );
  out.playNote( 13.0, 2.0, new RInstrument(  Frequency.ofPitch( "B2" ).asHz() ) );
  out.playNote(13.0, 2.0, new RInstrument(  Frequency.ofPitch( "D3" ).asHz() ) );
  out.playNote( 13.0, 2.0, new RInstrument(  Frequency.ofPitch( "G3" ).asHz() ) );


  out.playNote( 14.0, 2.0, new RInstrument(  Frequency.ofPitch( "G2" ).asHz() ) );
  out.playNote( 14.0, 2.0, new RInstrument(  Frequency.ofPitch( "B2" ).asHz() ) );
  out.playNote( 14.0, 2.0, new RInstrument(  Frequency.ofPitch( "D3" ).asHz() ) );
  out.playNote( 14.0, 2.0, new RInstrument(  Frequency.ofPitch( "G3" ).asHz() ) );

  out.playNote(  15.0, 2.0, new RInstrument(  Frequency.ofPitch( "G2" ).asHz() ) );
  out.playNote(  15.0, 2.0, new RInstrument(  Frequency.ofPitch( "B2" ).asHz() ) );
  out.playNote(  15.0, 2.0, new RInstrument(  Frequency.ofPitch( "D3" ).asHz() ) );
  out.playNote( 15.0, 2.0, new SineInstrument(  Frequency.ofPitch( "G3" ).asHz() ) );

  delay(1000);
}

void CT1() {
  out.playNote(0.0, 1.0, new TubeInstrument(  Frequency.ofPitch( "E3" ).asHz() ) );
  out.playNote(0.0, 1.0, new TubeInstrument(  Frequency.ofPitch( "E4" ).asHz() ) );
  out.playNote( 0.0, 1.0, new TubeInstrument(  Frequency.ofPitch( "G3" ).asHz() ) );
  out.playNote( 0.0, 1.0, new TubeInstrument(  Frequency.ofPitch( "G4" ).asHz() ) );


  out.playNote( 0.9, 2.0, new TubeInstrument(  Frequency.ofPitch( "C4" ).asHz() ) );
  out.playNote( 0.9, 2.0, new TubeInstrument(  Frequency.ofPitch( "C5" ).asHz() ) );

  out.playNote( 2.7, 1.0, new TubeInstrument(  Frequency.ofPitch( "G4" ).asHz() ) );
  out.playNote( 2.7, 1.0, new TubeInstrument(  Frequency.ofPitch( "G5" ).asHz() ) );

  out.playNote( 3.6, 1.0, new TubeInstrument(  Frequency.ofPitch( "G4" ).asHz() ) );
  out.playNote( 3.6, 1.0, new TubeInstrument(  Frequency.ofPitch( "G5" ).asHz() ) );

  out.playNote(  4.7, 2.0, new TubeInstrument(  Frequency.ofPitch( "B3" ).asHz() ) );
  out.playNote( 4.7, 2.0, new TubeInstrument(  Frequency.ofPitch( "B4" ).asHz() ) );

  out.playNote( 6.7, 1.0, new TubeInstrument(  Frequency.ofPitch( "G4" ).asHz() ) );
  out.playNote( 6.7, 1.0, new TubeInstrument(  Frequency.ofPitch( "G5" ).asHz() ) );

  out.playNote( 7.5, 1.0, new TubeInstrument(  Frequency.ofPitch( "G4" ).asHz() ) );
  out.playNote( 7.5, 1.0, new TubeInstrument(  Frequency.ofPitch( "G5" ).asHz() ) );

  out.playNote(  8.4, 2.0, new TubeInstrument(  Frequency.ofPitch( "C3" ).asHz() ) );
  out.playNote(  8.4, 2.0, new TubeInstrument(  Frequency.ofPitch( "C4" ).asHz() ) );

  out.playNote( 10.5, 1.0, new TubeInstrument(  Frequency.ofPitch( "G4" ).asHz() ) );
  out.playNote( 10.5, 1.0, new TubeInstrument(  Frequency.ofPitch( "G5" ).asHz() ) );

  out.playNote( 11.4, 1.0, new TubeInstrument(  Frequency.ofPitch( "G4" ).asHz() ) );
  out.playNote( 11.4, 1.0, new TubeInstrument(  Frequency.ofPitch( "G5" ).asHz() ) );

  out.playNote(  12.4, 2.0, new TubeInstrument(  Frequency.ofPitch( "C3" ).asHz() ) );
  out.playNote(  12.4, 2.0, new TubeInstrument(  Frequency.ofPitch( "C4" ).asHz() ) );
  
  out.playNote(  14.4, 1.0, new TubeInstrument(  Frequency.ofPitch( "A3" ).asHz() ) );
  out.playNote(  14.4, 1.0, new TubeInstrument(  Frequency.ofPitch( "A4" ).asHz() ) );

  delay(1000);
}

void CT2() {
  out.playNote(0.0, 1.0, new TubeInstrument(  Frequency.ofPitch( "A5" ).asHz() ) );
  out.playNote(0.0, 1.0, new TubeInstrument(  Frequency.ofPitch( "A6" ).asHz() ) );

  out.playNote( 0.9, 1.0, new TubeInstrument(  Frequency.ofPitch( "G5" ).asHz() ) );
  out.playNote( 0.9, 1.0, new TubeInstrument(  Frequency.ofPitch( "G6" ).asHz() ) );

  out.playNote( 1.7, 1.0, new TubeInstrument(  Frequency.ofPitch( "D6" ).asHz() ) );
  out.playNote( 1.7, 1.0, new TubeInstrument(  Frequency.ofPitch( "D7" ).asHz() ) );

  out.playNote( 2.6, 1.0, new TubeInstrument(  Frequency.ofPitch( "C6" ).asHz() ) );
  out.playNote( 2.6, 1.0, new TubeInstrument(  Frequency.ofPitch( "C7" ).asHz() ) );

  out.playNote(  3.5, 1.0, new TubeInstrument(  Frequency.ofPitch( "B5" ).asHz() ) );
  out.playNote( 3.5, 1.0, new TubeInstrument(  Frequency.ofPitch( "B6" ).asHz() ) );

  out.playNote( 4.4, 2.0, new TubeInstrument(  Frequency.ofPitch( "G5" ).asHz() ) );
  out.playNote( 4.4, 2.0, new TubeInstrument(  Frequency.ofPitch( "G6" ).asHz() ) );

  out.playNote( 6.2, 1.0, new TubeInstrument(  Frequency.ofPitch( "C6" ).asHz() ) );
  out.playNote( 6.2, 1.0, new TubeInstrument(  Frequency.ofPitch( "C7" ).asHz() ) );

  out.playNote(  7.1, 1.0, new TubeInstrument(  Frequency.ofPitch( "B5" ).asHz() ) );
  out.playNote(  7.1, 1.0, new TubeInstrument(  Frequency.ofPitch( "B6" ).asHz() ) );

  out.playNote( 8.0, 2.0, new TubeInstrument(  Frequency.ofPitch( "A6" ).asHz() ) );
  out.playNote( 8.0, 2.0, new TubeInstrument(  Frequency.ofPitch( "A5" ).asHz() ) );

  out.playNote( 9.9, 1.0, new TubeInstrument(  Frequency.ofPitch( "D7" ).asHz() ) );
  out.playNote( 9.9, 1.0, new TubeInstrument(  Frequency.ofPitch( "D6" ).asHz() ) );

  out.playNote(  10.8, 1.0, new TubeInstrument(  Frequency.ofPitch( "C7" ).asHz() ) );
  out.playNote(  10.8, 1.0, new TubeInstrument(  Frequency.ofPitch( "C6" ).asHz() ) );
  
  out.playNote(  11.7, 2.0, new TubeInstrument(  Frequency.ofPitch( "A5" ).asHz() ) );
  out.playNote(  11.7, 2.0, new TubeInstrument(  Frequency.ofPitch( "A6" ).asHz() ) );

  delay(1000);
}

 

####part2
####classes:


import processing.serial.*;
import ddf.minim.*;
import ddf.minim.ugens.*;
import ddf.minim.signals.*;
import processing.sound.*;


//the class of piano effects:
class SineInstrument implements Instrument
{
  Oscil wave;
  Line  ampEnv;

  SineInstrument( float frequency )
  {
    wave   = new Oscil( frequency, 0, Waves.SINE );
    ampEnv = new Line();
    ampEnv.patch( wave.amplitude );
  }

  void noteOn( float duration )
  {

    ampEnv.activate( duration, 0.15f, 0 );
    wave.patch( out );
  }

  void noteOff()
  {
    wave.unpatch( out );
  }
}

class RInstrument implements Instrument
{
  Oscil wave;
  Line  ampEnv;

  RInstrument( float frequency )
  {
    wave   = new Oscil( frequency, 0, Waves.SINE );
    ampEnv = new Line();
    ampEnv.patch( wave.amplitude );
  }

  void noteOn( float duration )
  {

    ampEnv.activate( duration, 0.10f, 0 );
    wave.patch( out );
  }

  void noteOff()
  {
    wave.unpatch( out );
  }
}


//the class of tube instrument effects:
class TubeInstrument implements Instrument
{
  Oscil wave;
  Line  ampEnv;

  TubeInstrument( float frequency )
  {
    wave   = new Oscil( frequency, 15, Waves.SINE );
    ampEnv = new Line();
    ampEnv.patch( wave.amplitude );
  }

  void noteOn( float duration )
  {

    ampEnv.activate( duration, 0.05f, 0.1 );
    wave.patch( out );
  }

  void noteOff()
  {
    wave.unpatch( out );
  }
}

void serialEvent (Serial myPort) {
  String inString = myPort.readStringUntil('\n');
  if (inString != null) {
    inString = trim(inString);
    int sensors[] = int(split(inString, ','));
    if (sensors.length == 4) {
      distance1 =sensors[0];
      distance2 =sensors[1];
      distance3 =sensors[2];
      distance4 =sensors[3];
      //distance1 = map(sensors[0], 0, 950, 10, 200);
      //distance2 = map(sensors[1], 0, 950, 10, 200);
    }
  }
}


//the class of floating dots effects:

class Particle {
  PVector loc, vel, acc;
  int lifeSpan, passedLife;
  boolean dead;
  float alpha, weight, weightRange, decay, xOffset, yOffset;
  int c;
//location 
  Particle(float x, float y, float _xOffset, float _yOffset) {
    loc = new PVector(x, y);

    float randDegrees = random(360);
    vel = new PVector(cos(radians(randDegrees)), sin(radians(randDegrees)));
    vel.mult(random(5));

    acc = new PVector(0, 0);
    lifeSpan = int(random(10, 100));
    decay = random(0.85, 0.95);
    weightRange = random(3, 50);

    c = (int) random(palette.length);

    xOffset = _xOffset;
    yOffset = _yOffset;
  }

  void update() {
    if (passedLife>=lifeSpan) {
      dead = true;
    } else {
      passedLife++;
    }

    alpha = float(lifeSpan-passedLife)/lifeSpan * 70+50;
    weight = float(lifeSpan-passedLife)/lifeSpan * weightRange;

    acc.set(0, 0);

    float rn = (noise((loc.x+frameCount+xOffset)*0.01, (loc.y+frameCount+yOffset)*0.01)-0.5)*4*PI;
    float mag = noise((loc.y+frameCount)*0.01, (loc.x+frameCount)*0.01);
    PVector dir = new PVector(cos(rn), sin(rn));
    acc.add(dir);
    acc.mult(mag);

    float randDegrees = random(360);// where does those dots float to
    PVector randV = new PVector(cos(radians(randDegrees)), sin(radians(randDegrees)));
    randV.mult(0.001);
    acc.add(randV);

    vel.add(acc);
    vel.mult(decay);
    vel.limit(2);
    loc.add(vel);
  }

  void display() {

    strokeWeight(weight+1.5+5);// size of the glow
    stroke(palette[c], alpha/5);//color and transparency of the dots
    point(loc.x, loc.y);

    strokeWeight(weight*.5+2);
    //stroke(#6FFF00);
    stroke(palette[c]);
    point(loc.x, loc.y);
  }
}
