<template>
  <div class="pong-game">
    <div class="game-board">
      <div class="paddle paddle-left" :style="{ top: paddleLeftY + 'px' }"></div>
      <div class="paddle paddle-right" :style="{ top: paddleRightY + 'px' }"></div>
      <div class="ball" :style="{ top: ballY + 'px', left: ballX + 'px' }"></div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      paddleLeftY: 0,
      paddleRightY: 0,
      ballX: 0,
      ballY: 0,
      ballSpeedX: 5,
      ballSpeedY: 5,
      paddleHeight: 80,
      paddleWidth: 10,
      ballSize: 10,
      canvasWidth: 600,
      canvasHeight: 400
    };
  },
  mounted() {
    this.startGame();
  },
  methods: {
    startGame() {
      this.gameLoop = setInterval(() => {
        this.updateGame();
      }, 16.67);
    },
    updateGame() {
      this.movePaddle();
      this.moveBall();
      this.checkCollision();
    },
    movePaddle(event) {
      // Logic for moving the paddles based on user input
      // Update paddleLeftY and paddleRightY values accordingly
      const mouseY = event.clientY;
      this.paddleLeftY = mouseY - this.paddleHeight / 2;
    },
    moveBall() {
      // Logic for moving the ball
      // Update ballX and ballY values based on ballSpeedX and ballSpeedY
      this.ballX += this.ballSpeedX;
      this.ballY += this.ballSpeedY;
    },
    checkCollision() {
      // Logic for checking collisions
      // Adjust ballSpeedX and ballSpeedY if the ball hits a paddle or wall
      if (this.ballX + this.ballSize >= this.canvasWidth) {
        // Ball hits the right wall
        this.ballSpeedX = -this.ballSpeedX;
      } else if (this.ballX <= 0) {
        // Ball hits the left wall
        this.ballSpeedX = -this.ballSpeedX;
      }

      if (this.ballY + this.ballSize >= this.canvasHeight || this.ballY <= 0) {
        // Ball hits the top or bottom wall
        this.ballSpeedY = -this.ballSpeedY;
      }

      // Check collision with left paddle
      if (
        this.ballX <= this.paddleWidth &&
        this.ballY + this.ballSize >= this.paddleLeftY &&
        this.ballY <= this.paddleLeftY + this.paddleHeight
      ) {
        this.ballSpeedX = -this.ballSpeedX;
      }

      // Check collision with right paddle
      if (
        this.ballX + this.ballSize >= this.canvasWidth - this.paddleWidth &&
        this.ballY + this.ballSize >= this.paddleRightY &&
        this.ballY <= this.paddleRightY + this.paddleHeight
      ) {
        this.ballSpeedX = -this.ballSpeedX;
      }
    },
    gameLoop() {
      // The main game loop
      this.moveBall();
      this.checkCollision();

      // Call gameLoop() recursively to keep the game running
      requestAnimationFrame(this.gameLoop);
    }
  },
  beforeUnmount() {
    clearInterval(this.gameLoop);
  }
};
</script>

<style scoped>
.pong-game {
  position: relative;
  width: 600px;
  height: 400px;
  background-color: #000;
}

.game-board {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.paddle {
  position: absolute;
  width: 10px;
  height: 60px;
  background-color: #fff;
}

.paddle-left {
  left: 10px;
}

.paddle-right {
  right: 10px;
}

.ball {
  position: absolute;
  width: 10px;
  height: 10px;
  background-color: #fff;
}
</style>
