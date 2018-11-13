<html>

<canvas id="gameCanvas" width="1000" height="700"></canvas>

<script>
var canvas;
var canvasContext;
// Positions for all 3 balls
var ballX, ballX2, ballX3;
var ballY, ballY2, ballY3;
ballX = ballX2 = ballX3 = 500;
ballY = ballY2 = ballY3 = 410;
// Speed for all 3 balls on X-axis (low value = slow balls)
var ballSpeedX, ballSpeedX2, ballSpeedX3;
ballSpeedX = ballSpeedX2 = ballSpeedX3 = 20;
// Speed values on Y-axis for each ball
var ballSpeedY = 6;
var ballSpeedY2 = 7;
var ballSpeedY3 = 8;
var player1Score = 0;
var player2Score = 0;
const winningScore = 3;
var paddle1Y = 250;
var paddle2Y = 250;
const paddleHeight = 150;
function calculateMousePos(evt) 
{
	// Mouse code for player paddle
	
	var rect = canvas.getBoundingClientRect();
	var root = document.documentElement;
	var mouseX = evt.clientX - rect.left - root.scrollLeft;
	var mouseY = evt.clientY - rect.top - root.scrollTop;
	return {
		x:mouseX,
		y:mouseY
	};
}
window.onload = function() 
{
	canvas = document.getElementById('gameCanvas');
	canvasContext = canvas.getContext('2d');
	var framesPerSecond = 30;
	setInterval(function() {
		moveEverything();
		drawEverything();
	}, 1000/framesPerSecond);
	canvas.addEventListener('mousemove',
		function(evt)
		{
			var mousePos = calculateMousePos(evt);
			paddle1Y = mousePos.y;
		}); 
} 
function ballReset() 
{
	// Once a player reaches past the winning score, reset score to 0
	
	if(player1Score >= winningScore ||
	   player2Score >= winningScore) {
		player1Score = 0;
		player2Score = 0;
	}
	ballSpeedX = -ballSpeedX;
	ballX = canvas.width/2;
	ballY = canvas.height/2;
	ballSpeedX2 = -ballSpeedX2;
	ballX2 = canvas.width/2;
	ballY2 = canvas.height/2;
	ballSpeedX3 = -ballSpeedX3;
	ballX3 = canvas.width/2;
	ballY3 = canvas.height/2;
}
function computerMovement()
{
	var paddle2YCenter = paddle2Y +	(paddleHeight/2);
	if(paddle2YCenter < ballY && ballY2 && ballY3)
	{
		paddle2Y += 6;
	}
	else
	{
 		paddle2Y -= 6;
	}
}
function moveEverything()
{
	computerMovement();
	ballX += ballSpeedX;
	ballY -= ballSpeedY;
	ballX2 += ballSpeedX2;
	ballY2 -= ballSpeedY2;
	ballX3 += ballSpeedX3;
	ballY3 -= ballSpeedY3;
	//Flipping horizontal level of the balls (X axis)
	if(ballX && ballX2 && ballX3 < 0) 
	{
		if(ballY && ballY2 && ballY3 > paddle1Y &&
			ballY && ballY2 && ballY3 < paddle1Y + paddleHeight) {
			ballSpeedX = -ballSpeedX
			ballSpeedX2 = -ballSpeedX2
			ballSpeedX3 = -ballSpeedX3
		} 
		else 
		{
			player2Score++;	
			ballReset();
		}
	}
	if(ballX && ballX2 && ballX3 > canvas.width) 
	{
		if(ballY && ballY2 && ballY3 > paddle2Y &&
			ballY && ballY2 && ballY3 < paddle2Y + paddleHeight) {
			ballSpeedX = -ballSpeedX
			ballSpeedX2 = -ballSpeedX2
			ballSpeedX3 = -ballSpeedX3;
		} 
		else 
		{
			player1Score++;	
			ballReset();	
		}
	}
	//Flipping vertical level of the balls (Y axis)
	
	if(ballY && ballY2 && ballY3 < 0) 
	{
		ballSpeedY = -ballSpeedY
		ballSpeedY2 = -ballSpeedY2
		ballSpeedY3 = -ballSpeedY3;
	}
	if(ballY && ballY2 && ballY3 > canvas.height) 
	{
		ballSpeedY = -ballSpeedY
		ballSpeedY2 = -ballSpeedY2
		ballSpeedY3 = -ballSpeedY3;
	}	
} 
function drawEverything() 
{
	// Canvas/board
	canvasContext.fillStyle = 'black';
	canvasContext.fillRect(0, 0, canvas.width, canvas.height);
	// Left/player paddle
	canvasContext.fillStyle = 'yellow';
	canvasContext.fillRect(0, paddle1Y, 10, paddleHeight, 100);
	// Right/enemy paddle
	canvasContext.fillStyle = 'yellow';
	canvasContext.fillRect(canvas.width-10, paddle2Y, 10, paddleHeight, 100);
	
	// White ball (X-axis, Y-axis, rectangle width, rectangle height)
	canvasContext.fillStyle = 'white';
	canvasContext.fillRect(ballX, ballY, 100, 11);
		
	// Blue ball (X-axis, Y-axis, rectangle width, rectangle height)
	canvasContext.fillStyle = 'blue';
	canvasContext.fillRect(ballX2, ballY2, 100, 11);
	// Red ball (X-axis, Y-axis, rectangle width, rectangle height)
	canvasContext.fillStyle = 'red';
	canvasContext.fillRect(ballX3, ballY3, 100, 11);
	// Score text
	canvasContext.fillStyle = 'yellow';
	canvasContext.fillText(player1Score, 100, 100);
	canvasContext.fillText(player2Score, canvas.width-100, 100);
	// Middle text (the text means Russian Pong, very clever I know)
	canvasContext.font = "25px Arial";
	canvasContext.fillStyle = 'red';
	canvasContext.fillText("Русский понг", 430, 100);
}
</script>

</html>
