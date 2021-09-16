# NinjaMan_Challenge
In this project I worked on a simple game called Ninjaman, its quite similar to Pacman except that its 10X COOLER :D

https://user-images.githubusercontent.com/85634099/133632437-8832d79d-e849-48fc-8b4a-9d0a40377d5d.mp4


This game is built with VS Code using JS,HTML,CSS and it runs on the browser, the main charechter is a cool ninja running around eating sushi and onigiri while escaping the ghosts whith his super speed !


# To Start
This is a pre-built game so I got the code from [Coding Dojo](https://github.com/keephopealive/academy-ninja/archive/master.zip) and I added the following :
* Add Onigiri as an alternative food to eat (Onigiri Score : 5 Points)
* Keep Score of how many Sushi's & Onigiri's NinjaMan eats (Onigiri Score : 5 Points), (Sushi Score : 10 Points)
* Random world generated when loading the page
* Adding 3 Ghosts & randomly change their places in new worlds
* Give NinjaMan 3 lives where Game Over occurs when a ghost hits NinjaMan 3 times 
* Take 5 Points and 1 live when NinjaMan is attaked by a ghost
* Moving to a new world when NinjaMan Wins by eating all the food in the world



## Add Onigiri To The Game's World
Add Onigiri image to your project files then go to the world's dictionary and add a new key & value to resemble the Onigiri (ex: 3 :'Onigiri').
```
var worldDict={
			0:'blank',
			1:'wall',
			2:'sushi',
			3:'Onigiri',
		}
```

## Total Score
To determine if the user won or not we need to check the total score which is based on the sum of all the Sushi's & Onigiri's the Ninjaman ate. I defined 2 variables as follows:

1) First one is 'Score' to track the player's current score by checking with each key stroke if the Ninjaman passed through a Sushi or Onigiri, and if so , add the corresponding score to the player's score variable.
```
var score = 0;
document.onkeydown = function(e){
if(world[ninjaman.y][ninjaman.x]==2){//IF THE NINJA ATE A SUSHI ADD 10 POINTS TO THE TOTAL SCORE
		score+=10;
	}
	if(world[ninjaman.y][ninjaman.x]==3){//IF THE NINJA ATE A ONIGIRI ADD 5 POINTS TO THE TOTAL SCORE
		score+=5;
	} }
```
2) Second variable is 'total_score', and to caluculate it we need to know how many Sushi's and Onigiri's are in the world and to do so I used a loop to go theough every spot in the world and check its' value, if it found an Onigiri, add 5 points to the total, and if it finds a Sushi then add 10 points.
```
var total_score = 0;

for(var row = 0; row < world.length; row++){//CALCULATE THE TOTAL SCORE FOR THIS LEVEL TO END 
				for(var x = 0; x < world[row].length; x++){
					if(world[row][x]==2){total_score+=10;}
					if(world[row][x]==3){total_score+=5;}
			}}
	} 
```
### Note: 
Ghost attacks takes 5 Points from the total score per attack, compare the Ninjaman (x,y) coordinates with the ghosts coordinates to check if the Ninjaman is being attacked or not.
```
if(ninjaman.y== ghost1.y && ninjaman.x== ghost1.x ||ninjaman.y== ghost2.y && ninjaman.x == ghost2.x ||ninjaman.y == ghost3.y && ninjaman.x== ghost3.x){
		score-=5;// TAKE 5 POINTS OFF THE SCORE
		total_score-=5;// TAKE 5 POINTS OFF THE TOTAL SCORE
    }
```
## Random world
Simply, use Random function from Math object which will return a number between 0 & 1, as I have keys from 0 to 3 defined in 'worldDict', I multiplied the Random number in 3 so the map can inclued all the diffrent elements. Note: Random values changes when page is refreshed, so a new world will be created with every page reload.

And this is the final random world:
```
var world=[//USE RANDOM TO CREATE A NEW WORLD WHEN THE PAGE IS RELOADED
			[1,1,1,1,1,1,1,1,1,1],
			[1,0,2,2,Math.floor(Math.random() * 3),2,3,Math.floor(Math.random() * 3),3,1],
			[1,3,2,2,Math.floor(Math.random() * 3),3,2,2,3,1],
			[1,Math.floor(Math.random() * 3),2,2,Math.floor(Math.random() * 3),3,2,2,2,1],
			[1,Math.floor(Math.random() * 3),2,2,Math.floor(Math.random() * 3),2,2,Math.floor(Math.random() * 3),Math.floor(Math.random() * 3),1],
			[1,2,3,2,Math.floor(Math.random() * 3),2,Math.floor(Math.random() * 3),3,Math.floor(Math.random() * 3),1],
			[1,Math.floor(Math.random() * 3),2,Math.floor(Math.random() * 3),2,2,2,3,2,1],
			[1,Math.floor(Math.random() * 3),2,Math.floor(Math.random() * 3),3,Math.floor(Math.random() * 3),2,2,Math.floor(Math.random() * 3),1],
			[1,Math.floor(Math.random() * 3),3,Math.floor(Math.random() * 3),2,Math.floor(Math.random() * 3),2,3,Math.floor(Math.random() * 3),1],
			[1,2,3,Math.floor(Math.random() * 3),2,Math.floor(Math.random() * 3),2,3,2,1],
			[1,1,1,1,1,1,1,1,1,1],
			
		];
```
## Adding 3 Ghosts 
In the Body section add 3 divs, each representing a unique ghost with a diffrent gif & location assigned with the ID to apply the corresponding CSS:
```
<body>
  <div id="ghost1"></div>
	<div id="ghost2"></div>
	<div id="ghost3"></div> </body>
```
EX: The CSS for ghost 1 (do the same stucture with other ghosts just with few changes):
```
#ghost1{
				background-color: black;
				height: 40px;
				width: 40px;
				display: inline-block;
				background-image: url("img/pinky.gif");
				background-size: contain;
				position: absolute;
				top: 40px;
				left: 320px;
			}
```
Now these ghosts need to move and to start they need their own (x,y) coordinates created randomly by Random function. Note: There is a possibility that the ghost may land on a wall, to prevent that I used a do-while to check that the ghost won't land on a wall:
```
do{
	var ghost1={
	x:Math.floor(Math.random() * 2)+1,
	y:Math.floor(Math.random() * 2)+2}
  
}while(worldDict[world[ghost1.y][ghost1.x]]==1);
```
And to make them move I created 4 functions to go in 4 directions(UP,RIGHT,DOWN,LEFT), to make the motion visible I trapped the code in a Timeout function with 0.5 sec delay:
```
function ghost_right(){ // RIGHT
	 setTimeout(function(){
		if(world[ghost1.y][ghost1.x+1]!=1 ){//IF GHOST DIDN'T REACH THE WALL YET, KEEP GOING
			ghost1.x++;
			update();//UPDATE THE GHOST COORDINATES
			result();
			ghost_right()
		}else{
    ghost_left()//IF THE GHOST REACHED THE RIGHT WALL, GO LEFT
    } if(world[ghost1.y][ghost1.x+1]==1 && world[ghost1.y][ghost1.x-1] ==1){ghost_up()}//IF THE GHOST IS TRAPPED BETWEEN 2 WALLS, GO UP & DOWN
	},500)
	
}
```
NOTE: for other direction do the same taking in consideration (y) for vertical movement & (x) for horizontal.

In case you noticed the previous code lines has a function named Update which is mainly for updating the (x,y) coordinates for all the ghosts as follows:
```
function update(){
//each spot is 40px wide so multiply the coordinate by 40
	document.getElementById("ghost1").style.left = ghost1.x*40+"px";
	document.getElementById("ghost1").style.top = ghost1.y*40+"px";
	document.getElementById("ghost2").style.left = ghost2.x*40+"px";
	document.getElementById("ghost2").style.top = ghost2.y*40+"px";
	document.getElementById("ghost3").style.left = ghost3.x*40+"px";
	document.getElementById("ghost3").style.top = ghost3.y*40+"px";

	
}
```

## Give NinjaMan 3 lives
Similar steps to adding a ghost, start by adding a div with unique CSS and keep track of the lives remaining using a counter, also displaying the lives and the score for the player, here how it goes:
* HTML:
```
<p id="live" style="margin-left:140px;"></p>
	<p style="display:inline ;margin-left:140px;"><b>Score</b>: <div style="display:inline; margin-left:10px;" id="score">0</div></p>
	</body>
```
* CSS
```
.live{
				height: 15px;
				width: 15px;
				display: inline-block;
				background-image: url("img/ninja.gif");
				background-size: contain;
				position: absolute;
				top: 460px;
			}
```
* JAVASCRIPT
1) Diaplay Lives:
```
var life="<b><br>Lives:</b> ";
for(var x = 0; x < count; x++){//DISPLAY THE LIVES ON SCREEN
					life+="<div class = 'live'  style='left: "+margin+"px;'></div>";
					margin+=19;
			}
		
		document.getElementById('live').innerHTML= life;
```
2) Count Remaining Lives:
```
var count=3;//NUMBER OF DEFAULT LIVES
//IF THE NINJA RAN INTO A GHOST SUBTRACT 10 POINTS FROM THE SCORE AND 1 FROM LIVES 
if(ninjaman.y== ghost1.y && ninjaman.x== ghost1.x ||ninjaman.y== ghost2.y && ninjaman.x == ghost2.x ||ninjaman.y == ghost3.y && ninjaman.x== ghost3.x){
score-=5;// TAKE 5 POINTS OFF THE SCORE
total_score-=5;// TAKE 5 POINTS OFF THE TOTAL SCORE
count-=1;//LOSES 1 LIVE
life=life.replace("<div class = 'live'  style='left: "+margin+"px;'></div>","");//REMOVE THE LIVE FROM THE SCREEN
document.getElementById('live').innerHTML= life;//UPDATE THE LIVE COUNTER
}
```

## Player Wins
when the Ninjaman eats all the Sushi's & Onigiri's in the world and still have 1 live at least, alert message displays celebrating the player win and moving into a new world.
```
if(score==total_score){//IF THE USER WINS
		document.getElementById('live').innerHTML="<b><br> LEVEL COMPLETED !! :D </b>";//END THE GAME IF NINJA ATE ALL THE FOOD
		document.getElementById('live').style.color = '#00ff00'//CHANGE TEXT COLOR TO Green
		setTimeout(function() {alert("LEVEL COMPLETED !! :D   YOUR SCORE IS: "+score); },10)//ALERT THE USER 'LEVEL COMPLETED'
		setTimeout(function() {alert("NEXT CHALLENGE IS UP .. LETS GO NINJA !! "); },10)//ALERT THE USER TO PLAY AGAIN
		location.reload(true);//RELOAD THE PAGE
		
	}
  ```
## Player Loses
when the Ninjaman loses all the lives before eating all the Sushi's & Onigiri's in the world, alert message displays declaring the Game is over and moving into a new world
```
if(count==0){//IF THE USER LOSES
		document.getElementById('live').innerHTML="<b><br> GAME OVER ! x_x </b>";//END THE GAME IF NINJA ENCOUNTERED 3 GHOSTS
		document.getElementById('live').style.color = '#d00'//CHANGE TEXT COLOR TO RED
		setTimeout(function() {alert("GAME OVER ! x_x"); },10)//ALERT THE USER 'GAME OVER'
		setTimeout(function() {alert("Lets Try Again .. Ninja !! "); },10)//ALERT THE USER TO PLAY AGAIN
		location.reload(true);//RELOAD THE PAGE
		
	}
```
## Final Game:
Here is the completed game showing both cases when a player wins and loses , Enjoy :)

https://user-images.githubusercontent.com/85634099/133634292-fa0ffb56-9fa2-4662-9166-8e0ad8ba7d86.mp4


