# exhebition1

import processing.io.*;

import ddf.minim.*;
import ddf.minim.signals.*;

AudioOutput out;
Oscillator  osc;


Minim minim;
AudioPlayer player;

int input1 = 0;
int input2 = 0;
int input3 = 0;
int input4 = 0;

int total = -50;
boolean music = false;
boolean standing1 = false;
boolean standing2 = false;
boolean standing3 = false;
boolean standing4 = false;

// GPIO numbers refer to different phyiscal pins on various boards
// On the Raspberry Pi GPIO 4 is physical pin 7 on the header
// see setup.png in the sketch folder for wiring details

void setup() {
  size(600, 400);
  // INPUT_PULLUP enables the built-in pull-up resistor for this pin
  // left alone, the pin will read as HIGH
  // connected to ground (via e.g. a button or switch) it will read LOW
  GPIO.pinMode(27, GPIO.INPUT_PULLUP);
  GPIO.pinMode(22, GPIO.INPUT_PULLUP);
  GPIO.pinMode(4, GPIO.INPUT_PULLUP);
  GPIO.pinMode(17, GPIO.INPUT_PULLUP); 





  minim = new Minim(this);
  player = minim.loadFile("voice2.mp3", 2048);
  //total = -60;
}

void draw() {
  //total = (input1 + input2+input3+input4)/4;
  background(255);


  if (GPIO.digitalRead(27) == GPIO.HIGH  && standing1 == false  ) {
    // button is pressed
    fill(0);
    if (music == false) {
      player.play();
    }
    standing1 = true;
    for (int i = 0; i<20; i++) {
      total = total+1;
      player.setGain(total);
      delay(200);
    }
    music = true;
  } else if (GPIO.digitalRead(27) == GPIO.LOW  && standing1 == true)
  {
    println("down");
     for (int i = 20; i>0; i--) {
      total = total -1;
      player.setGain(total);
      println(i);
      delay(200);
    }
    standing1 = false;
  }

  if (GPIO.digitalRead(22) == GPIO.HIGH && standing2 == false ) {
    // button is pressed
    fill(255, 0, 0);
    if (music == false) {
      player.play();
    }
    standing2 = true;
    for (int i = 0; i<20; i++) {
      total = total+1;
      player.setGain(total);
      delay(200);
    }
    music = true;
  } else if (GPIO.digitalRead(22) == GPIO.LOW  && standing2 == true)
  {
    println("down");
    for (int i = 20; i>0; i--) {
      total = total -1;
      println(i);
      player.setGain(total);
      delay(200);
    }
    standing2 = false;
  }


  if (GPIO.digitalRead(17) == GPIO.HIGH && standing3 == false   ) {
    // button is pressed
    fill(0);
    if (music == false) {
      player.play();
    }
    standing3 = true;
    for (int i = 0; i<20; i++) {
      total = total+1;
      player.setGain(total);
      delay(200);
    }
    music = true;
  } if (GPIO.digitalRead(17) == GPIO.LOW  && standing3 == true)
  {
    println("down");
    for (int i = 20; i>0; i--) {
      total = total-1;
      println(i);
      player.setGain(total);
      delay(200);
    }
    standing3 = false;
  }


  if (GPIO.digitalRead(4) == GPIO.HIGH && standing4 == false ) {
    // button is pressed
    fill(255, 0, 0);
    if (music == false) {
      player.play();
    }
    standing4 = true;
    for (int i = 0; i<20; i++) {
      total = total+1;
      player.setGain(total);
      delay(200);
    }
    music = true;
  } 
  if (GPIO.digitalRead(4) == GPIO.LOW  && standing4 == true){
    println("down");
   for (int i = 20; i>0; i--) {
      total = total -1;
      println(i);
      player.setGain(total);
      delay(200);
    }
    standing4 = false;
  }


  print(GPIO.digitalRead(17)+"  "+GPIO.digitalRead(4)+"  ");
   println(GPIO.digitalRead(27)+"  "+GPIO.digitalRead(22));
  player.setGain(total);
  if (music == true) {
    player.play();
  }
  if ( player.position() == player.length() ) {
    player.rewind();
    music = false;
  }

  if (total == -50 && music == true) {
   
    
      player.pause(); 
      player.rewind();
      music = false;
  }
  stroke(255); textSize(50);fill(0);textAlign(CENTER);
    text(total,width/2,height/2);
}
