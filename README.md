 Spaceship bob = new Spaceship ();
star[] nightsky = new star [200];
Asteroid[] asteroids = new Asteroid[10];
public void setup() {
  size(500, 500);
  for ( int i = 0; i < nightsky.length; i++)
  {
    nightsky[i] = new star();
}
for (int i = 0; i < asteroids.length; i++) {
    asteroids[i] = new Asteroid();
  }
}
public void draw(){
  background(0); 
  for ( int i = 0; i < nightsky.length; i++) {
    nightsky[i].show();
  }
  bob.show();
  bob.move(); 
  for (Asteroid asteroid : asteroids) {
    asteroid.show();
    asteroid.move();
      if (bob.hit(asteroid)) {
      bob.resetPosition2();
      bob.stop();
      asteroid.setActive(false);
      
    }
  }
      
}
void keyPressed(){
  if (key == 'h' || key == 'H') {
    bob.stop(); 
    bob.resetPosition(); 
    bob.setRandomDirection();
  }
  if (key == 'w' || key == 'W') {
    bob.accelerate(1);
  }
  if (key == 'a' || key == 'A') {
    bob.turn(-25);
  }
  if (key == 'd' || key == 'D') {
    bob.turn(25);
  }
  if (key == 's' || key == 'S') { 
    bob.accelerate(-1);
  }
}


class Asteroid extends Floater {
  boolean active;
  public Asteroid() {
    active = true; 
    myCenterX = random(width);
    myCenterY = random(height);
    corners = 6;
    xCorners = new int[corners];
    yCorners = new int[corners];

    xCorners[0] = -11; yCorners[0] = -8;
    xCorners[1] = 7; yCorners[1] = -8;
    xCorners[2] = 13; yCorners[2] = 0;
    xCorners[3] = 6; yCorners[3] = 10;
    xCorners[4] = -11; yCorners[4] = 8;
    xCorners[5] = -5; yCorners[5] = 0;
    myColor = color(150, 150, 150);
    myXspeed = random(-1, 1);
    myYspeed = random(-1, 1);
    myPointDirection = random(360);
  }

  public void move() {
    super.move();
    handleScreenWrapping();
  }

  private void handleScreenWrapping() {
    if (myCenterX > width) {
      myCenterX = 0;
    } else if (myCenterX < 0) {
      myCenterX = width;
    }
    if (myCenterY > height) {
      myCenterY = 0;
    } else if (myCenterY < 0) {
      myCenterY = height;
    }
  }
  public boolean isActive() {
    return active;
  }

  public void setActive(boolean isActive) {
    active = isActive;
  }
}


class Spaceship extends Floater {
  public Spaceship() {
    corners = 4;
    xCorners = new int[corners];
    yCorners = new int[corners];
  
    xCorners[0] = -8; yCorners[0] = -8;
    xCorners[1] = 16; yCorners[1] = 0;
    xCorners[2] = -8; yCorners[2] = 8;
    xCorners[3] = -2; yCorners[3] = 0;
    myColor = 206;
    myCenterX = 250;
    myCenterY = 250;
    myXspeed = 0;
    myYspeed = 0;
    myPointDirection = 0;
  }

  public void accelerate(double amount) {
    double acceleration = 1;
    super.accelerate(acceleration * amount);
  }

  public void stop() {
    myXspeed = 0;
    myYspeed = 0;
  }

  public void resetPosition() {
    myCenterX = Math.random() * width;
    myCenterY = Math.random() * height;
  }

  public void setRandomDirection() {
    myPointDirection = Math.random() * 360;
  }
  private static final double COLLISION_THRESHOLD = 20;

public boolean hit(Asteroid asteroid) {
  double distanceX = myCenterX - asteroid.myCenterX;
  double distanceY = myCenterY - asteroid.myCenterY;
  double distance = Math.sqrt(distanceX * distanceX + distanceY * distanceY);
  if (distance < COLLISION_THRESHOLD) {
    return true;
  } else {
    return false;
  }
    
  }
  
  
  


  public void resetPosition2() {
    myCenterX = width / 2;
    myCenterY = height / 2;
    myXspeed = 0;
    myYspeed = 0;
  }
}

class star {
  int myX;
  int myY;

  public star() {
    myX = (int) (Math.random() * 500);
    myY = (int) (Math.random() * 500);
  }

  void show() {
    fill(255);
    ellipse(myX, myY, 2, 2);
  }
}

class Floater //Do NOT modify the Floater class! Make changes in the Spaceship class 
{   
  protected int corners;  //the number of corners, a triangular floater has 3   
  protected int[] xCorners;   
  protected int[] yCorners;   
  protected int myColor;
  protected double myCenterX, myCenterY; //holds center coordinates   
  protected double myXspeed, myYspeed; //holds the speed of travel in the x and y directions   
  protected double myPointDirection; //holds current direction the ship is pointing in degrees    

  //Accelerates the floater in the direction it is pointing (myPointDirection)   
  public void accelerate (double dAmount)   
  {          
    //convert the current direction the floater is pointing to radians    
    double dRadians =myPointDirection*(Math.PI/180);     
    //change coordinates of direction of travel    
    myXspeed += ((dAmount) * Math.cos(dRadians));    
    myYspeed += ((dAmount) * Math.sin(dRadians));       
  }   
  public void turn (double degreesOfRotation)   
  {     
    //rotates the floater by a given number of degrees    
    myPointDirection+=degreesOfRotation;   
  }   
  public void move ()   //move the floater in the current direction of travel
  {      
    //change the x and y coordinates by myXspeed and myYspeed       
    myCenterX += myXspeed;    
    myCenterY += myYspeed;     

  
    if(myCenterX >width)
    {     
      myCenterX = 0;    
    }    
    else if (myCenterX<0)
    {     
      myCenterX = width;    
    }    
    if(myCenterY >height)
    {    
      myCenterY = 0;    
    } 
    
    else if (myCenterY < 0)
    {     
      myCenterY = height;    
    }   
  }   
  public void show ()  //Draws the floater at the current position  
  {             
    fill(myColor);   
    stroke(myColor);    
    
    //translate the (x,y) center of the ship to the correct position
    translate((float)myCenterX, (float)myCenterY);

    //convert degrees to radians for rotate()     
    float dRadians = (float)(myPointDirection*(Math.PI/180));
    
    //rotate so that the polygon will be drawn in the correct direction
    rotate(dRadians);
    
    //draw the polygon
    beginShape();
    for (int nI = 0; nI < corners; nI++)
    {
      vertex(xCorners[nI], yCorners[nI]);
    }
    endShape(CLOSE);

    //"unrotate" and "untranslate" in reverse order
    rotate(-1*dRadians);
    translate(-1*(float)myCenterX, -1*(float)myCenterY);
  }   
} 


}
