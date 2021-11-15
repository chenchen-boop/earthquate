# earthquate
#include <Servo.h>
#include "Timer.h" //?臬?賢?摨?
#include "pitches.h"
Servo myservo;  // 撱箇?隡箸?擐祇??批
Timer T;
  
//const int tone_Pin = 12;
const int LED_Pin = 12;
const int servoPin = 14;


int pos = 90; //閫漲???其葉??閮剖???0摨?
bool  btn_Pressed = false;  //蝝????????
bool danger=true,firstrot=true;

long presentTime=0;





int earthquate = 1;
int before = 0;
int now = 0;
const int btnPin = 13;



//int melody[] = {
//  NOTE_C4, NOTE_D4, NOTE_E4, NOTE_F4, NOTE_G4,
//};
//int noteDurations[] = {
//  4, 4,8,4,4,
//};

//void music(){
//  
//  for (int thisNote = 0; thisNote < 5; thisNote++) {
//      int noteDuration = 1000 / noteDurations[thisNote];
//      ledcWriteTone(channel, melody[thisNote]);//voice
//      delay(noteDuration);
//      ledcWriteTone(channel, 0);//reset
//      int pauseBetweenNotes = noteDuration * 1.30;
//      delay(pauseBetweenNotes);
//    }
//
//
//
//    
//}





void rotate(){
  if(earthquate==true){
    firstrot=false;
    //Serial.println("rotatei");
    int r=random(0,4);
    switch (r) {
      
      case 0:pos+=20;
      break;
      case 1:pos-=20 ;
      break;
      case 2:pos+=10;
      break;
      case 3:pos-=10 ;
      break;
      
      
    }
  
    if(pos>=180||pos<=0){
      pos=90;
    }
    myservo.write(pos); 
    delay(300);
    Serial.println(pos);
  }


  else{
      digitalWrite(LED_Pin,LOW);
  }
}


void setup() {
  Serial.begin(115200);
  
  myservo.attach(servoPin);  
  myservo.write(90);  

  pinMode(btnPin,INPUT);
  pinMode(27,INPUT);//紅外線感測
  pinMode(LED_Pin,OUTPUT);
  
  digitalWrite(LED_Pin,LOW);
  T.every(200,rotate);
  


  //delay(5000)

  
   
}




void loop() {

T.update();



now=digitalRead(btnPin);

if(now==0&&before==1 ){
  earthquate=-earthquate;
}
Serial.println(earthquate);
delay(100);
before=now;


if(earthquate){ 
  
  if(digitalRead(27)==HIGH){//紅外線感測danger
    danger=true;
    digitalWrite(LED_Pin,HIGH);
    delay(300);
    //Serial.println("danger");
    //music();
  }
  

}
else if(earthquate==-1){
  digitalWrite(LED_Pin,LOW);
  pos=90;
  myservo.write(pos); 
  delay(300);
  
}





  





}
