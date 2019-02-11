# Lantern
Green Lantern game using javaScript
var gunLoaded = true;
var enemyLeft = false;
var gameStarted = false;
var enemiesLeft = 4;
var startTime;
var elapsedTime;
var greenX = 0;
var greenY = 180;
var distance = 12;
var KillCounter = 0;

hideElement("image_shot");
onEvent("shootBtn", "click", function(){
  showElement("image_shot");
  fire();
});
onEvent("downBtn", "mousedown", function() {
  greenY = greenY + distance;
  setPosition("greenlantern", greenX, greenY);
});
onEvent("upBtn", "click", function() {
 greenY = greenY- distance; 
 setPosition("greenlantern", greenX, greenY);
});

onEvent("screen_game", "keydown", function(event){
  if(event.key == "Up")
  {greenY = greenY- distance;
  setPosition("greenlantern", greenX, greenY);
  }
  if (event.key == "Down")
  {greenY = greenY + distance;
  
  }
  if (event.key == "1")
  {showElement("image_shot");
    fire();
    
  }
  setPosition("greenlantern", greenX, greenY);
wrap("greenlantern");
});

function wrap(Enemy)
{var EnemyX = getXPosition(Enemy);
var EnemyY = getYPosition(Enemy);
var EnemyHeight = getProperty(Enemy, "height");

if (EnemyY < 0 - (EnemyHeight))
{
  EnemyY = 450 + (EnemyHeight);
}
else if (EnemyY > 450 - (EnemyHeight))
 {EnemyY = 0 - (EnemyHeight); 
 }
 setPosition(Enemy, EnemyX, EnemyY);
}

function fire()
{while (gunLoaded) 
{gunLoaded = false;
var greenX = getXPosition("greenlantern");
var greenY = getYPosition("greenlantern");
var greenWidth = getProperty("greenlantern", "width");
var shotX = greenX + greenWidth;
var shotY = greenY;


setPosition("image_shot", shotX, shotY);
}}
onEvent("startBtn", "click", function(){
  setScreen("screen_game");
  if(!gameStarted)
  {
    gameStarted = true;
    
    startTime = getTime();
    setPosition("image_shot", 400,100);
    timedLoop(50, function(){
      updateTime();
  moveShot();
    
moveEnemy("enemy_1");
moveEnemy("enemy_2");
moveEnemy("enemy_3");
moveEnemy("enemy_4");
hitDetection("enemy_1");
hitDetection("enemy_2");
hitDetection("enemy_3");
hitDetection("enemy_4");
});
}});
onEvent("Start_Screen", "keydown", function(event) {
if(event.key === "Enter"){
KillCounter = 0;
 setScreen("screen_game");
  if(!gameStarted)
  {
gameStarted = true;
startTime = getTime();
setPosition("image_shot", 400,100);
timedLoop(50, function(){
updateTime();
moveShot();
moveEnemy("enemy_1");
moveEnemy("enemy_2");
moveEnemy("enemy_3");
moveEnemy("enemy_4");
hitDetection("enemy_1");
hitDetection("enemy_2");
hitDetection("enemy_3");
hitDetection("enemy_4");
});
}}});
function updateTime()
{
  var currentTime = getTime();
  elapsedTime = currentTime - startTime;
  elapsedTime = (elapsedTime/1000).toFixed(2);
  setNumber("timeLbl", elapsedTime);
}

function moveShot()
{
 var shotX = getXPosition("image_shot");
  var shotY = getYPosition("image_shot");
  var shotSpeed = 30;
 
  shotX = shotX + shotSpeed;
  setPosition("image_shot", shotX, shotY);
  if (shotX > 400)
  gunLoaded = true;
}

function moveEnemy(Enemy)
{
  var enemyX = getXPosition(Enemy);
  var enemyY = getYPosition(Enemy);
  
  var minX = 100;
  var maxX = 300;
  var horizontal = randomNumber(1, 15);
  while (!(enemyX >3001)&&(enemyX< 0))
  setPosition(Enemy, (getXPosition(Enemy)+20));
  enemyY = enemyY + 20;
  if(enemyX > maxX)
  enemyLeft = true;
  
  if (enemyX < minX)
  enemyLeft = false;
  
  if  (enemyLeft === true)
  enemyX = enemyX - horizontal;
  else
  enemyX = enemyX + horizontal;
  setPosition(Enemy, enemyX, enemyY);
  wrap(Enemy);
}
function hitDetection(Enemy)
{
  var shotX = getXPosition("image_shot");
  var shotY = getYPosition("image_shot");
  var EnemyX = getXPosition(Enemy);
  var EnemyY = getYPosition(Enemy);
  
  var shotWidth = getProperty("image_shot", "width");
  var shotHeight = getProperty("image_shot", "height");
  var EnemyWidth = getProperty(Enemy, "width");
  var EnemyHeight = getProperty(Enemy, "height");
  
  if (shotX + shotWidth >= EnemyX && shotX <= EnemyX + EnemyWidth)
{
if  (shotY + shotHeight >= EnemyY && shotY <= EnemyY + EnemyHeight)
{
  if(!getProperty(Enemy, "hidden"))
  {hideElement(Enemy);
  enemiesLeft--;
  KillCounter++;
  setText("KillCounter", "Kills: " + KillCounter);
  if (enemiesLeft <= 0)
  {stopTimedLoop();
  hideElement("image_shot");

  setScreen("GameOverScreen");
  setText("ScoreLbl", "It took you "+ elapsedTime +" seconds! Play again to beat your time!");
  
}}}}}

onEvent("playAgainBtn", "click", function() {
 showElement("RulesLbl");
  showElement("title_lbl");
  setScreen("Start_Screen");
  showElement("enemy_1");
   showElement("enemy_2");
    showElement("enemy_3");
     showElement("enemy_4");
  enemiesLeft = enemiesLeft + 4;
 gunLoaded = true;

 setNumber("timeLbl", 0.0);
 gameStarted = false;
KillCounter = 0;
setText ("KillCounter", "Kills: 0");

  setPosition("enemy_1", 230, 15);
   setPosition("enemy_2", 230, 125);
    setPosition("enemy_3", 230, 235);
     setPosition("enemy_4", 230, 345);
});


