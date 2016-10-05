/*Andrew Peterson
 4/15/15
 Platformer Game
 Use Arrow keys to move, Z to jump, and X to shoot to to progress through the screens.
 Player is not able to move through groundBlocks, but player can move through platforms 
 by jumping up below them.  Enemy movement is restricted in the same way.
 The character can have three shots on the screen at a time.  Shots will disappear if
 they collide with a solid wall or an enemy.  Reach the end of the game 
 by moving from left to right.*/
 
import ddf.minim.spi.*; //import minim library
import ddf.minim.signals.*;
import ddf.minim.*;
import ddf.minim.analysis.*;
import ddf.minim.ugens.*;
import ddf.minim.effects.*;

PImage[] charRights; //two arrays for the images of the char facing left and facing right
PImage[] charLefts;

Minim minim;  //declare minim and AudioPlayer objects
AudioPlayer music;

PImage enemySprite;
PImage enemySpriteRight;
PImage groundBlockPic;
PImage decor;
PImage exitSign;
PImage titleScreen;
PImage loseScreen;
PImage winScreen;

int leftMostBlock;  //tells game how far screen has translated. Iterated by 30 every time screen moves.

int[] groundBlockY; //ground blocks all have a yValue and a boolean to tell game if it is acutally there.  
boolean[] groundNotPit; //If the block is not there, it signifies a pit.
boolean[] platWall;  //the space of a ground block can also be taken up by a wall with an opening
final static int BLOCKSIDE = 20;

float[] platX; // these are the plats above the ground blocks that the character can also jump on
float[] platY; 
int[] platW;
int platCount; //tells game which plats to show onscreen

float[] shotX; //each shot has coordinates and a boolean to tell the game if the shot is displayed
float[] shotY;
boolean[] displayShot;
int[] shotDirection;
final static int SHOTSPEED = 7;
final static int SHOTRIGHT = 0;
final static int SHOTLEFT = 1;

float charX;
float charY;
boolean charLeft; //is char moving left or right?
boolean charRight;
boolean lookLeft; //is char looking left or right
boolean lookRight;
float charYSpeed; //changes w/ gravity 
int nearestGroundY;
float nearestPlatY;
final static float MAXYSPEED = 5; 
final static int CHARXSPEED = 3;
final static float GRAVITY = .1;
final static int CHARH = 43; //character height and width
final static int CHARW = 20;
boolean charXMoving; //tells game when to animate character
int charMoveTime;  //tells game how to animate character

float[] enemyX;  //enemies have same data as char and are controlled by the procedures in the enemy procedures tab
float[] enemyY;
float[] enemyYSpeed;
boolean[] enemyLeft; //is the enemy moving right or left?
boolean[] enemyRight;
boolean[] enemyOnGround;
boolean[] enemyDefeated;
int[] enemyNearestGroundY;
int[] enemyGroundAheadY; //this matrix stores the y values of the ground in front of the enemies to prevent them from falling in pits
float[] enemyNearestPlatY;
final static int enemyXSPEED = 2;
int enemyCount; //tells game which enemies to show on screen at one time.  There are up to 8 enemies per screen, but not all of them have to be seen
final static int ENEMYH = 43;
final static int ENEMYW = 20;

boolean onGround; //tells game if char is standing on something.  If he is, he can jump.

int gameState;
final static int TITLE = 0;
final static int GAMEPLAY = 1;
final static int GAMEOVER = 2;
final static int WINSCREEN = 3;

void setup() {
  size(600, 600); 
  groundBlockY = new int[1000];
  groundNotPit = new boolean[1000];
  platWall = new boolean[1000];
  platX = new float[100];
  platY = new float[100]; 
  platW = new int[100];
  shotX = new float[3]; 
  shotY = new float[3];
  displayShot = new boolean[3];
  shotDirection = new int[3];
  
  enemyX = new float[100];  //enemies have same data as char and are controlled by the procedures in the enemy procedures tab
  enemyY = new float[100];
  enemyYSpeed = new float[100];
  enemyLeft = new boolean[100]; //is the enemy moving right or left?
  enemyRight = new boolean[100];
  enemyOnGround = new boolean[100];
  enemyDefeated = new boolean[100];
  enemyNearestGroundY = new int[100];
  enemyGroundAheadY = new int[100]; //this matrix stores the y values of the ground in front of the enemies to prevent them from falling in pits
  enemyNearestPlatY = new float[100];
  charRights = new PImage[6];
  charLefts = new PImage[6];
  minim = new Minim(this);
  music = minim.loadFile("battletoads.mp3"); //tell game what fle the music object should play
  
  charRights[0] = loadImage("Samus Standing Right.png");  //load up character images taken from google images
  charRights[1] = loadImage("Samus Running Right 1.png");
  charRights[2] = loadImage("Samus Running Right 2.png");
  charRights[3] = loadImage("Samus Running Right 3.png");
  charRights[4] = loadImage("Samus Running Right 4.png");
  charRights[5] = loadImage("Samus Running Right 5.png");
  charLefts[0] = loadImage("Samus Standing Left.png");
  charLefts[1] = loadImage("Samus Running Left 1.png");
  charLefts[2] = loadImage("Samus Running Left 2.png");
  charLefts[3] = loadImage("Samus Running Left 3.png");
  charLefts[4] = loadImage("Samus Running Left 4.png");
  charLefts[5] = loadImage("Samus Running Left 5.png");
  decor = loadImage("Super Mario World Hills.png");
  
  enemySprite = loadImage("Enemy Sprite.gif");
  enemySpriteRight = loadImage("Enemy Sprite Right.gif");
  groundBlockPic = loadImage("ground block.png"); //original images made in MS paint 
  exitSign = loadImage("exit sign.png");
  titleScreen = loadImage("Title Screen.png");
  loseScreen = loadImage("lose screen.png");
  winScreen = loadImage("win screen.png");
  charX = 50;
  charY = 300;
  charLeft = false;
  charRight = false; //start off game without moving
  lookRight = true;
  lookLeft = false; //start off game looking right
  charYSpeed = 0;
  onGround = false;
  charXMoving = false;
  charMoveTime = 0;
  leftMostBlock = 0;
  gameState = TITLE; //start at title screen

  //ground block and wall setup
  for (int i = 0; i < groundBlockY.length; i++) { //set up locations of ground blocks and walls
    groundBlockY[i] = 400; //default ground block conditions
    groundNotPit[i] = true;
    platWall[i] = false;
  }
  for (int i = 27; i < 60; i++) {
    groundBlockY[i] = 200;
    platWall[i] = true; //many walls in a row to make a cave-like area
  }
  for (int i = 60; i < 64; i++) {
    groundBlockY[i] = 200;
  }
  for (int i = 64; i < 120; i++) {
    groundNotPit[i] = false;
  }
  for (int i = 150; i < 160; i++) {
    platWall[i] = true;
    groundBlockY[i] = 400 - (i-149)*20;
  }
  for (int i = 160; i < 170; i++) {
    platWall[i] = true;
    groundBlockY[i] = 200 + (i-159)*20;
  }


  //platform setup
  platCount = 0;
  for (int i = 0; i < platX.length; i++) {
    platX[i] = 0;
    platY[i] = -100; //if you are not using a plat on a screen, it will be at this location
    platW[i] = 50; //default plat width
  }
  platX[0] = 400;
  platY[0] = 300;

  for (int i = 20; i < 23; i++) {
    platX[i] = 1200 + (i - 19)*150;
    platY[i] = 400;
  }
  for (int i = 30; i < 40; i++) {
    platX[i] = 1650 + (i - 29)*150;
    platY[i] = 300;
  }

  //shot setup
  for (int i = 0; i < shotX.length; i++) {
    shotX[i] = 0;
    shotY[i] = 0;
    displayShot[i] = false;
    shotDirection[i] = 0;
  }

  //enemy setup
  enemyCount = 0;
  for (int i = 0; i < enemyX.length; i++) {
    enemyX[i] = -100; //default enemy conditions
    enemyY[i] = 0;
    enemyYSpeed[i] = 0;
    enemyOnGround[i] = false;
    enemyDefeated[i] = false;
    enemyRight[i] = false;
    enemyLeft[i] = true;
    enemyOnGround[i] = false;
  }
  enemyX[5] = 1000;
  enemyY[5] = 100;

  for (int i = 20; i < 23; i++) {
    enemyX[i] = 2400 + (i-19)* 75;
    enemyY[i] = 350;
  }
  enemyX[25] = 3200;
  enemyY[25] = 100;
  enemyX[26] = 3400;
  enemyY[26] = 300;
}

void draw() {
  if (gameState == TITLE) {
    background(0);
    image(titleScreen,0,0);
  }
  if (gameState == GAMEPLAY) {
    music.loop();
    image(decor, -100, 0);
    pushMatrix();
    translate(-leftMostBlock*BLOCKSIDE, 0); //translate so that the leftMostBlock is on the left and multiply it by the side length of a ground block
    moveTheScreen();
    
    displayGround();
    displayPlats();
    
    moveCharX();
    moveCharY();
    displayChar();
    displayShots();
    
    displayEnemies();
    moveEnemy();
    defeatEnemy();
    
    popMatrix();

    if (charDefeat()) {  //if charDefeat returns a true, then the game is over
      gameState = GAMEOVER;
    }
    if (charX > 4200) { //if charX makes it to the end of the game, you win
      gameState = WINSCREEN;
    }
  }
  if(gameState == GAMEOVER) {
    music.pause();  //stop music on game over screen
    music.rewind();
    background(0);
    image(loseScreen,0,0);
  }
  if(gameState == WINSCREEN) {
    music.pause();  //stop music on win screen
    music.rewind();
    background(0);
    image(winScreen,0,0);
  }
}

void keyPressed() {
  if(gameState == TITLE) {
    if(key == 't') {
      gameState = GAMEPLAY; //go to gameplay state if t is pressed on title screen
    }
  }
  if (gameState == GAMEPLAY) { //keyboard controls
    if (keyCode == RIGHT) {
      charRight = true; //move right
      lookRight = true;
      lookLeft = false;
      charXMoving = true;
    } else if (keyCode == LEFT) {
      charLeft = true; //move left
      lookLeft = true;
      lookRight = false;
      charXMoving = true;
    }
    if (key == 'z' && onGround) { //if yoou press the jump button, jump
      charYSpeed = MAXYSPEED; //set ySpeed before jump so that each jump reaches the same altitude
      charYSpeed = charYSpeed*-1.1;
      onGround = false; //you aren't on the ground anymore
    }
    if (key == 'x'){
      shoot(); //unleash a shot if x is pressed
    }
  }
  if(gameState == WINSCREEN || gameState == GAMEOVER) {
    if(key == 't') {
      setup(); //restart game if you are on the win screen or on the lose screen
    }
  }
}

void keyReleased() {  //if keys are released, then make the booleans controlling movement false
  if (gameState == GAMEPLAY) {
    if (keyCode == RIGHT) {
      charRight = false;
      charXMoving = false;
    } else if (keyCode == LEFT) {
      charLeft = false;
      charXMoving = false;
    }
  }
}

void displayChar() {
  fill(0);
  int i = 0; //tells game which frame to show
  if (charXMoving) {
    charMoveTime += 1;
    charMoveTime = charMoveTime % 25; //charMoveTime repeats using mod operation
  } else {
    charMoveTime = 0;
  }
  if (!charXMoving && charMoveTime == 0 || !onGround) { // not moving
    i = 0;
  } 
  else if (onGround && charMoveTime >= 0 && charMoveTime <= 5) { //character is animated by using different frames controlled by int i
    i = 1;
  } 
  else if (onGround && charMoveTime >= 5 && charMoveTime < 10) { 
    i = 2;
  } 
  else if (onGround && charMoveTime >= 10 && charMoveTime < 15) {
    i = 3;
  } 
  else if (onGround && charMoveTime >= 15 && charMoveTime < 20) {
    i = 4;
  } 
  else if (onGround && charMoveTime >= 20 && charMoveTime < 25) { 
    i = 5;
  }
  if (lookRight) {
    image(charRights[i], charX, charY);  // draw character depending on if she is looking right or left
  } 
  else {
    image(charLefts[i], charX, charY);
  }
}

void moveCharX() {
  if (charRight) {
    charX = charX + CHARXSPEED; //move right
    lookRight = true;
    lookLeft = false;
    charXMoving = true;
  } 
  else if (charLeft) {
    charX = charX - CHARXSPEED; //move left
    lookRight = false;
    lookLeft = true;
    charXMoving = true;
  } 
  else {
    charXMoving = false;
  }

  if (charX < 5) {
    charX = 5; //don't let char go father left than this or else the game will crash because it will go out of bounds of the ground array
  }

  if (!onGround) { //if you aren't on the ground, move down 
    charY = charY + charYSpeed;
  }
  charX = groundXPush(charX, charY, CHARW, CHARH); //don't move through the groundBlocks horizontally
}

void moveCharY() {
  charYSpeed = charYSpeed + GRAVITY; //iterate charYSpeed with GRAVITY
  if (charYSpeed > MAXYSPEED) {
    charYSpeed = MAXYSPEED;
  }

  if (charYSpeed < 0) {
    charY = groundYPush(charX, charY, CHARW, CHARH); //if you are moving up because of a jump, do not move up into the groundBlocks
  }

  nearestGroundY = highestGroundBelow(charX, charY, CHARW, CHARH);  //returns nearest ground yValue below char
  nearestPlatY = highestPlatBelow(charX, charY, CHARW, CHARH); //returns nearest plat yValues below char

  if ((charY + CHARH > nearestGroundY - 5) && (charYSpeed > 0)) { //if you get really close to the closest ground below you and 
    //you have passed the TOP of your jumping arc... (shown by your positive Yspeed)
    charY = nearestGroundY - CHARH - 1;   //then you land on it
    onGround = true;
  } 
  else if ((charY + CHARH > nearestPlatY - 5) && (charYSpeed > 0)) { //this is same as above but for the plats on a screen
    charY = nearestPlatY - CHARH - 1; 
    onGround = true;
  } 
  else {
    onGround = false;
  }
}

void displayShots() { //displays and animates shots
  for (int i = 0; i < shotX.length; i++) {
    if (displayShot[i]) {
      fill(255, 0, 0);
      rect(shotX[i], shotY[i], 5, 2); //display shots
      if (shotDirection[i] == SHOTRIGHT) {
        shotX[i]+=SHOTSPEED; //move shot to the right
        if (shotX[i] > (leftMostBlock+30)*BLOCKSIDE + BLOCKSIDE) { //if the shot passes the edge of the screen, then don't display it anymore
          displayShot[i] = false;  //now this shot is ready to be used in shoot() again
        }
      } 
      else if (shotDirection[i] == SHOTLEFT) {
        shotX[i]-=SHOTSPEED; //move shot to the left
      }
      if (shotX[i] < leftMostBlock*BLOCKSIDE) {
        displayShot[i] = false; //if shot leaves screen to left, then don't display it anymore
      }
    }
    if (groundXPushed(shotX[i], shotY[i], 5, 2)) {
      displayShot[i] = false; //if shot gets "pushed" (runs into) a groundBlock, then it is not displayed anymore
    }
  }
}

void shoot() {  //shoots one shot when x is pressed.  Only 3 shots can be on the screen at one time
  boolean exit = false;
  int i = 0;
  while (!exit) {  //we don't know how many shots are already displayed, so we use a while loop
    if (!displayShot[i]) {
      displayShot[i] = true;
      shotX[i] = charX + CHARW;
      shotY[i] = charY + .28*CHARH; //set up the initial coordinates of the shot
      if (lookRight) {
        shotDirection[i] = SHOTRIGHT; //set direction of shot so that it is animated in the correct directiion
      } 
      else if (lookLeft) {
        shotDirection[i] = SHOTLEFT;
      }
      exit = true; //once one of the displayShot boolean variables goes from false to true, exit the while loop.  You have unleashed one shot.
    }
    if (i == 2) {
      exit = true; //or if all shots are already displayed, then exit the loop
    }
    i++; //you need to iterate i by yourself in a while loop
  }
}

boolean charDefeat() {
  for (int i = enemyCount; i < enemyCount + 5; i++) {
    if (!enemyDefeated[i] && (charX + CHARW > enemyX[i]) && (charX < enemyX[i] + ENEMYW ) && (charY + CHARH > enemyY[i]) && (charY < enemyY[i] + ENEMYH)) {
      return true; //if you run into an enemy, you are defeated
    } 
    else if (charY > 700) {
      return true;  //if you fall into a pit, you are defeated
    }
  }
  return false; //if neither of these conditional statements are true for all times through the loop, then return false
}

void moveTheScreen() { //procedurs translates screen when char gets to edge of screen
  if (charX + CHARW > (leftMostBlock + 30)*BLOCKSIDE) { //if you hit the edge of the screen
    leftMostBlock+=29; //then change the value that translates the screen to move (almost) one screen forward.  
    //Move by 29 blocks because it is a smoother transition
    platCount+=10; //show the next 10 plats
    enemyCount+=5; //show next  5 enemiest 
    enemyRespawn(); //bring back enemies on last screen if you had already defeated them
  } 
  else if (charX < leftMostBlock*BLOCKSIDE) {
    leftMostBlock-=29; //same as above, but for moving backward 
    platCount-=10; //show the previous 10 plats
    enemyCount-=5; 
    enemyRespawn();//bring back defeated enemies from last screen
  }
}

void displayEnemies() {
  for(int i = enemyCount; i < enemyCount + 5; i++) {
    if(!enemyDefeated[i]) { //if the enemy is not defeated, then display it
      if(enemyLeft[i]) {
        image(enemySprite,enemyX[i],enemyY[i]);
      }
      else {
        image(enemySpriteRight,enemyX[i],enemyY[i]);
      }
    }
  }
}
void moveEnemy() { 
  //enemy should only be displayed on screen if leftMostBlock signifies a certain screen that the enemy exists on
  for(int i = enemyCount; i < enemyCount + 5; i++) {
    enemyNearestGroundY[i] = highestGroundBelow(enemyX[i],enemyY[i],CHARW,CHARH); //get places where the enemy could land
    enemyNearestPlatY[i] = highestPlatBelow(enemyX[i],enemyY[i],CHARW,CHARH);
    enemyX[i] = groundXPush(enemyX[i],enemyY[i],ENEMYW,ENEMYH);
    if(enemyRight[i]) {
      enemyX[i]+=enemyXSPEED; //move right
      enemyLeft[i] = false;
    }
    else if(enemyLeft[i]) {
      enemyX[i]-=enemyXSPEED; //move left
      enemyRight[i] = false;
    }
    enemyYSpeed[i]+=GRAVITY;
    
    if(!enemyOnGround[i]) {
      enemyY[i] += enemyYSpeed[i]; //if you aren't on the ground, the move down
    }
    if((enemyY[i] + ENEMYH > enemyNearestGroundY[i] - 5) && (enemyYSpeed[i] > 0)) { //if you get close to ground, then land on it
      enemyY[i] = enemyNearestGroundY[i] - ENEMYH - 1;
      enemyOnGround[i] = true;
    }
    else if((enemyY[i] + ENEMYH > enemyNearestPlatY[i] - 5) && (enemyYSpeed[i] > 0)) { //if you get close to plat, then land of it
      enemyY[i] = enemyNearestPlatY[i] - ENEMYH - 1;
      enemyOnGround[i] = true;
    }
    else {
      enemyOnGround[i] = false;
    } 
  }
  controlEnemy(); //separate procedure for conrolling enemy movement
}

void controlEnemy() { //this prcedure controls whether the enemies are moving right or left
  for (int i = enemyCount; i < enemyCount + 5; i++) {
    if (enemyRight[i]) { //first get the y value of the ground 20 pixels in front of the enemy
      enemyGroundAheadY[i] = highestGroundBelow(enemyX[i] + 20,enemyY[i],ENEMYW,ENEMYH);
    } 
    if (enemyLeft[i]) {
      enemyGroundAheadY[i] = highestGroundBelow(enemyX[i] - 20,enemyY[i],ENEMYW,ENEMYH);
    }
    if((enemyGroundAheadY[i] == 800) && (enemyOnGround[i])) { //if the enemy is right in front of a pit, then turn it around
      enemyRight[i] = !enemyRight[i]; 
      enemyLeft[i] = !enemyLeft[i];
    }
    if (groundXPushed(enemyX[i],enemyY[i],ENEMYW,ENEMYH)) { //if the enemy runs into a groundBlock, then turn it around
      enemyRight[i] = !enemyRight[i]; 
      enemyLeft[i] = !enemyLeft[i];
    }
    if ((enemyX[i] + ENEMYW > (leftMostBlock + 30)*BLOCKSIDE) || (enemyX[i] < leftMostBlock*BLOCKSIDE)) {
      enemyRight[i] = !enemyRight[i]; //if enemy runs into edge of screen, make it turn around
      enemyLeft[i] = !enemyLeft[i];
    }
  }
}

void defeatEnemy() {  //this procedure sees whether the enemyDefeated() boolean is true or false
  for(int i = 0; i < shotX.length; i++) { //use a nested loop because we have to check the collisions of all 3 shots
    for(int j = enemyCount; j < enemyCount + 5; j++) {
      if(displayShot[i] && !enemyDefeated[j] && (shotX[i] + 5 > enemyX[j]) && 
      (shotX[i] < enemyX[j] + ENEMYW) && (shotY[i] > enemyY[j]) && (shotY[i] < enemyY[j] + ENEMYH)) {
        enemyDefeated[j] = true; //if one of the 3 shots hit the enemy, then the enemy is defeated
        displayShot[i] = false; //shot disappears after it hits an enemy
      }
    }
  }
}

void enemyRespawn() {  //restart enemy positions every time you enter a screen.
  for(int i = enemyCount; i < enemyCount + 5; i++) {
    enemyDefeated[i] = false;
  }
}

void displayGround () {
  for (int i = leftMostBlock; i < leftMostBlock + 30; i++) { //displays all the ground blocks on the screen at one time because the screen is 30 blocks wide
    if (groundNotPit[i]) { //only show the block if it is NOT a pit
      fill(100);
      image(groundBlockPic, i*BLOCKSIDE, groundBlockY[i]);
      //rect(i*20, groundBlockY[i], 20, 20);
      if (platWall[i]) {
        for (int j = groundBlockY[i]; j < 800; j += BLOCKSIDE) { 
          // if you have a wall, then draw blocks all the way to the bottom of the screen, starting at the original y value
          image(groundBlockPic, i*BLOCKSIDE, j);
        }
        for (int j = groundBlockY[i] - 7*BLOCKSIDE; j > - BLOCKSIDE; j -= BLOCKSIDE) { 
          //if you have a wall, then draw blocks all the way to the top of the screen
          image(groundBlockPic, i*BLOCKSIDE, j); //starting 3 blocks above the original y value
        }
      }
    }
  }
}

float groundXPush(float x, float y, int w, int h) { //this procedure prevents you from walking through ground blocks
  float retFloat = 0;
  boolean changed = false;
  for (int i = leftMostBlock; i < leftMostBlock + 30; i++) {
    if ((groundNotPit[i]) && (!platWall[i]) && (y + h > groundBlockY[i]) && (y < groundBlockY[i] + BLOCKSIDE)) { 
      //if char is within a block's yValues and the yBlock is not a wall...
      if ((x + w > i*BLOCKSIDE) && (x + w < i*BLOCKSIDE +BLOCKSIDE)) {
        retFloat = i*BLOCKSIDE - w; //then return a charX that prevents you from moving through it
        changed = true;
      }
      else if ((x < i*BLOCKSIDE + BLOCKSIDE) && (x > i*BLOCKSIDE)) {
        retFloat = i*BLOCKSIDE + BLOCKSIDE; //then return a charX that prevents you from moving through it
        changed = true;
      }
    }
    else if ((groundNotPit[i]) && (platWall[i])) { //if you have a wall in front of you...
      if ((y + h > groundBlockY[i]) || (y < groundBlockY[i] - BLOCKSIDE*6)) { 
        //and you are within its y coordinates that are not part of the wall's opening...
        if ((x + w > i*BLOCKSIDE) && (x + w < i*BLOCKSIDE + BLOCKSIDE)) {
          retFloat = i*BLOCKSIDE - w; //then return a charX that prevents you from moving through the wall blocks
          changed = true;
        }
        else if ((x < i*BLOCKSIDE + BLOCKSIDE) && (x > i*BLOCKSIDE)) {
          retFloat = i*BLOCKSIDE + BLOCKSIDE; //then return a charX that prevents you from moving through the wall blocks
          changed = true;
        }
      }
    }
    if (!changed) {
      retFloat = x; //if you didn't change x, then return the same x that you had before
    }
  }
  return retFloat;
}

boolean groundXPushed(float x, float y, int w, int h) { //this procedure returns a true if you have walked into a groundBlock
  boolean retBool = false;
  for (int i = leftMostBlock; i < leftMostBlock + 30; i++) {
    if (y + h > groundBlockY[i] && y < groundBlockY[i] + BLOCKSIDE) { //if char is within a block's yValues...
      if ((x + w > i*BLOCKSIDE) && (x + w < i*BLOCKSIDE +BLOCKSIDE)) {
        retBool = true; //then you ran into a block
      }
      else if ((x < i*BLOCKSIDE + BLOCKSIDE) && (x > i*BLOCKSIDE)) {
        retBool = true;
      }
    }
    else if ((groundNotPit[i]) && (platWall[i])) { //if you have a wall in front of you...
      if ((y + h > groundBlockY[i]) || (y < groundBlockY[i] - BLOCKSIDE*6)) { 
        //and you are within its y coordinates that are not part of the wall's opening...
        if ((x + w > i*BLOCKSIDE) && (x + w < i*BLOCKSIDE + BLOCKSIDE)) {
          retBool = true; //then you ran into the wall
        }
        else if ((x < i*BLOCKSIDE + BLOCKSIDE) && (x > i*BLOCKSIDE)) {
          retBool = true; 
        }
      }
    }
  }
  return retBool;
}

float groundYPush(float x, float y, int w, int h) { //prevents char from jumping up into a block
  float retFloat = y;
  boolean changed = false;
  for (int i = leftMostBlock; i < leftMostBlock + 30; i++) {
    if (!platWall[i] && (x + w > i*BLOCKSIDE) && (x < i*BLOCKSIDE + BLOCKSIDE)) { //if you are within the block's x values...
      if ((y < groundBlockY[i] + BLOCKSIDE) && (y > groundBlockY[i])) { // and you are inside the block...
        retFloat = groundBlockY[i] + BLOCKSIDE; //then make a return value that prevents you from going into it
        charYSpeed = 0; //char loses upward speed when he hits head on groundBlock
        changed = true;
      }
    }
    else if((platWall[i]) && (x + w > i*BLOCKSIDE) && (x < i*BLOCKSIDE + BLOCKSIDE)) {
      if((y < groundBlockY[i] - 6*BLOCKSIDE) && (y > groundBlockY[i] - 7*BLOCKSIDE)) { //if you are within the y values of a wall that is its opening
        retFloat = groundBlockY[i] - 6*BLOCKSIDE; //then make a return value that prevents you from going into it
        charYSpeed = 0; //char loses upward speed when he hits head on groundBlock
        changed = true;
      }
    }
  }
  if (!changed) { //if you didn't change anything, then just leave char's y-values as it was before
    retFloat = y;
  }
  return retFloat;
}

int highestGroundBelow (float x, float y, int w, int h) { //returns y value of highest ground block below player
  int [] yValues = {800, 800}; //there are 2 y values that must be compared because char can span at most 2 blocks at a time. 
  int yValueNum = 0;          //Set them up at 800 (below the screen) to suggest that you are above a pit until proven otherwise
  int retInt; 
  for (int i = leftMostBlock; i < leftMostBlock + 30; i++) { //the screen is 30 blocks wide. Find the 2 yValues of the blocks that are below the character
    if ((groundNotPit[i]) && (x + w > i*BLOCKSIDE) && (x < i*BLOCKSIDE + BLOCKSIDE) && (y + h <= groundBlockY[i])) { 
      yValues[yValueNum] = groundBlockY[i];
      yValueNum++;
    }
    else if (!groundNotPit[i]  && (x + w > i*BLOCKSIDE) && (x < i*BLOCKSIDE + BLOCKSIDE) && (y + h < groundBlockY[i])) { 
      //if the ground IS a pit and you are over it...
      yValues[yValueNum] = 800; // you would fall through the pit and lose if this were your closest ground y-value
      yValueNum++;
    }
  }
  retInt = min(yValues[0], yValues[1]); //take the higher (up the screen!) of the two yVAlues that you are over (if you are above 2)
  return retInt;
}

void displayPlats() {
  for (int i = platCount; i < platCount + 10; i++) { //have 10 plats per screen.  Not all need to acutally be seen by player.
    fill(0,255,255);
    rect(platX[i], platY[i], platW[i], 10);
  }
  if(leftMostBlock == 203) { //display exit sign
    image(exitSign,4300,100);
  }
}

float highestPlatBelow(float x, float y, int w, int h) { //this procedure returns the highest platform yValue below the player
  float retFloat;
  float[] yValues = new float [10];  
  //there are 10 platforms per screen, so all of them should considered if there are multiple platforms on top of each other
  for (int i = 0; i < yValues.length; i++) {
    yValues[i] = 800; // initialize all of the elements of the matrix below the screen
  }
  int count = 0;
  for (int i = platCount; i < platCount + 10; i++) {
    if (y + h < platY[i]) {
      if ((x + w > platX[i]) && (x < platX[i] + platW[i])) {
        yValues[count] = platY[i];  //store the y values of all of the plats that the character or enemy is above
        count++;
      }
    }
  }
  retFloat = min(yValues); //take the min of the yValues to get the highest yValue up the screen
  return retFloat; //and return that value
}
