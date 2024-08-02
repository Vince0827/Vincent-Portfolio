# Led Snake game
This is an remake of the old snake game but on an Led board. The gist of it is to collect the apples(lights) to grow your snake longer and longer.

You should comment out all portions of your portfolio that you have not completed yet, as well as any instructions:
```HTML 
<!--- This is an HTML comment in Markdown -->
<!--- Anything between these symbols will not render on the published site -->
```

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Vincent C. | Camden Prep High School | Mechanical Engineering | Incoming Sophmore

**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Headstone Image](logo.svg)
  
# Final Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/watch/kuWQfPAjN5s" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE



# Second Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/8FxKgjxmOD0?si=ZGFPcYM6_ZfNE6to" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

For your second milestone, explain what you've worked on since your previous milestone. You can highlight:
- Technical details of what you've accomplished and how they contribute to the final goal
- What has been surprising about the project so far
- Previous challenges you faced that you overcame
- What needs to be completed before your final milestone 

# First Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/kuWQfPAJN5s?si=Z1k5iDGOU8467n9t" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

For your first milestone, describe what your project is and how you plan to build it. You can include:
-The joystick makes the led lights glow. When you push the joystick right it makes the blue led glow up, and when you push the joystick left you light up the green. But the yellow light stays on, unless you play with they jowystick a little then the green light forces it off. 
- Some progress i've made was is learning ardiuno in the last few days and im already starting to understand.
- One hard challange i had was when my little cousins unplugged and messed up my game and I had to restart from the start.
- My plan is to keep tring even if things get hard because sometimes good things dont come easy.

# Schematics 
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 

# Code
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

```c++
#include "LedControl.h"
#define joyX A0   //Joystick x richtung auf A0
#define joyY A1   //Joystick y richtung auf A1


int movewt = 500; //speed (lower = faster , higher = slower) milliseconds to move
int lengthtowin = 20; //determine the number of apples you need to win

int joybut = 9; //joystick button pin 9

int maxlength = 3; //length of the snake

int yValue;
int xValue;
int Punkte;
char direction;
int row = 4;
int col = 4;
bool neustart = true;
LedControl lc=LedControl(12,11,10,1);
bool dead = false;
int matrix [8][8]; 

int apfelPosition [8][8];
bool needapple = true;
int ax; //apple X position
int ay; //apple Y position
int atime = 200; //apple blink speed (lower = faster , higher = slower)1
bool leuchtet = false;
unsigned long lastappletime;

bool won = false;
unsigned long lasttime;
byte lachen[8]={B11111111, B11000011, B10101001, B10000101, B10000101, B10101001, B11000011, B11111111};
byte traurig[8]={B11111111, B11000011, B10100101, B10001001, B10001001, B10100101, B11000011, B11111111};
void setup() {
  delay(2000);
  pinMode(joybut, INPUT_PULLUP);
  // put your setup code here, to run once:
  Serial.begin (9600);
  Serial.println("Guten Tag");
  lc.shutdown(0,false);
  lc.setIntensity(0,8);
  lc.clearDisplay(0);
  lc.setLed(0,row,col,true);
  matrix [row][col] = 1;
}

void loop() {
  if (won == false){
  if (dead == false){
  xValue = analogRead(joyX);
  yValue = analogRead(joyY);  
  richtung();
  apfeltocollect();
  weiterfahren();
  }
  
  if (dead == true)
  {
    col = 4;
    row = 4;
    for(int x = 0; x <8 ; x ++){    
  	    for(int y = 0 ; y <8 ; y++){
          
            matrix[x][y] = 0;
            
          
        }
    }
    for(int x = 0; x <8 ; x ++){    
  	    for(int y = 0 ; y <8 ; y++){
          
            apfelPosition[x][y] = 0;
            
          
        }
    }
    maxlength = 3;
    for (int i=0; i<8; i++){
     lc.setRow(0,i,traurig[i]);
    }
    if(digitalRead(joybut)== 0){
        
        dead = false;
        lc.clearDisplay(0);
        lc.setLed(0,row,col,true);
        matrix [row][col] = 1;
      }
  }
  if (lengthtowin<=maxlength){
    won = true;
  }
  }
  if (won == true)
  {
    neustart = true;
    col = 4;
    row = 4;
    for(int x = 0; x <8 ; x ++){    
  	    for(int y = 0 ; y <8 ; y++){
          
            matrix[x][y] = 0;
            
          
        }
    }
    for(int x = 0; x <8 ; x ++){    
  	    for(int y = 0 ; y <8 ; y++){
          
            apfelPosition[x][y] = 0;
            
          
        }
    }
    maxlength = 3;
    for (int i=0; i<8; i++){
     lc.setRow(0,i,lachen[i]);
    }
    if(digitalRead(joybut)== 0){
        
        won = false;
        lc.clearDisplay(0);
        lc.setLed(0,row,col,true);
        matrix [row][col] = 1;
      }
  // put your main code here, to run repeatedly:

}}
void apfeltocollect(){
  if (needapple == true){ //generate new apple
    needapple = false;
    ay = random(0 , 7);
    ax = random(0 , 7);
    while(matrix[ax][ay] != 0){
      ay = random(0 , 7);
      ax = random(0 , 7);
    }
    apfelPosition[ax][ay] = 1;
  }
  if (ax == row && ay == col && needapple == false){ // collect apple
    needapple = true;
    for(int x = 0; x <8 ; x ++){    
  	    for(int y = 0 ; y <8 ; y++){
          
            apfelPosition[x][y] = 0;
            
          
        }
    }
    maxlength ++;

  }
  if (needapple == false){ // blinken
    if(leuchtet == false && millis()-lastappletime > atime){
      lc.setLed(0,ax,ay,true);
      lastappletime = millis();
      leuchtet = true;
      }
    else if(leuchtet == true && millis()-lastappletime > atime){
      lc.setLed(0,ax,ay,false);
      lastappletime = millis();
      leuchtet = false;
    }
  }


}
void weiterfahren(){
  
  if (millis()- lasttime > movewt){
    lasttime = millis();
  switch (direction){
    case 'N':
      col --;
      Serial.println("Nord");
      break;
    case 'O':

      Serial.println("Ost");
      row --;
      break;
    case 'S':
      Serial.println("South");
       col ++;
      break;
     
    
    case 'W':
      Serial.println("West");
      row ++;
      break;
    default:
      Serial.println("MoveJoystick");
      break;
  
  }
  if (direction == 'W' ||direction == 'O' ||direction == 'S' ||direction == 'N' ){
  if (row <0 || row >7 || col < 0 || col > 7) //ob es in den Rand gefahren ist
  {
    Serial.println("DED because of wall");
    neustart = true;
    dead = true;

  }
  else if(matrix [row][col] > 0 && matrix [row][col] != maxlength ) //if touches snake //(
   {
    Serial.println("DED because of snake");
    neustart = true;
    dead = true;
    
  }
  else{
      for(int x = 0; x <8 ; x ++){    //remove last part of snake
  	    for(int y = 0 ; y <8 ; y++){
          if (matrix[x][y] == maxlength){
            matrix[x][y] = 0;
            lc.setLed(0,x,y,false);
          }
        }

      }

     // move numbers back
     for(int l = maxlength - 1; l > 0 ; l --){
       for(int x = 0; x <8 ; x ++){    
  	    for(int y = 0 ; y <8 ; y++){
          if (matrix[x][y] == l){
            matrix[x][y] = l + 1;
            
          }
        }
      
     }
     //set new point
     matrix [row][col] = 1;
     lc.setLed(0,row,col,true);

    }
    }
   }
  }
  }
void richtung(){
  if (xValue > 700){
    if(direction != 'N'){
      direction = 'S';
  }}

  else if(xValue < 250){
    if(direction != 'S'){
      direction = 'N';
    }
  }
  
  else if (yValue > 620){
    if (direction != 'O'){
    direction = 'W';
    }
  }
  else if (yValue < 400)
  {
    if (direction != 'W'){
    direction = 'O';
  }}
  else if(neustart == true){
    neustart = false;
    direction= 'p';
  }
 // Serial.println(direction);
}

//#    #   ##       #  ######   ######   #########  #
//#   #    # #      #  #       #             #      #
//#  #     #  #     #  #      #              #      #
//# #      #   #    #  #       ##            #      #
//##       #    #   #  ######   #####        #      #
//# #      #     #  #  #            ##       #      #
//#  #     #      # #  #             #       #      #
//#   #    #       ##  #            #        #      #
//#    #   #        #  ######  #####         #      #


```

# Bill of Materials
Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. 

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Arduino uno | coding | $35.99 | <a href'https://www.amazon.com/ELEGOO-Project-Tutorial-Controller-Projects/dp/B01D8KOZF4/ref=asc_df_B01D8KOZF4/?tag=hyprod-20&linkCode=df0&hvadid=692875362841&hvpos=&hvnetw=g&hvrand=13062893050965263028&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=9003856&hvtargid=pla-2281435180258&psc=1&mcid=80a5b47d61dc377992c3c25d65d73272&hvocijid=13062893050965263028-B01D8KOZF4-&hvexpln=73&gad_source=1'> Link </a> |
| 8x8 led matrix | the snakes movement and body | $4.00 | <a href="https://www.ebay.com/itm/181860981748"> Link </a> |
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |

# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
