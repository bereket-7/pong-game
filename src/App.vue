
<template>
    <div class="pong-game" @keydown="handleKeyDown" tabindex="0">
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
  methods: {
    handleKeyDown(event) {
      const keyCode = event.keyCode;
      if (keyCode === 87) {
        this.paddleLeftY -= 10;
      } else if (keyCode === 83) { 
        this.paddleLeftY += 10;
      }
      if (keyCode === 38) { 
        this.paddleRightY -= 10;
      } else if (keyCode === 40) { 
        this.paddleRightY += 10;
      }
    },
    moveBall() {
      this.ballX += this.ballSpeedX;
      this.ballY += this.ballSpeedY;
    },
    movePaddle(event) {
      const mouseY = event.clientY;
      this.paddleLeftY = mouseY - this.paddleHeight / 2;
    },
    checkCollision() {
      if (this.ballX + this.ballSize >= this.canvasWidth) {
        this.ballSpeedX = -this.ballSpeedX;
      } else if (this.ballX <= 0) {
        this.ballSpeedX = -this.ballSpeedX;
      }

      if (this.ballY + this.ballSize >= this.canvasHeight || this.ballY <= 0) {
        this.ballSpeedY = -this.ballSpeedY;
      }

      if (
        this.ballX <= this.paddleWidth &&
        this.ballY + this.ballSize >= this.paddleLeftY &&
        this.ballY <= this.paddleLeftY + this.paddleHeight
      ) {
        this.ballSpeedX = -this.ballSpeedX;
      }
      if (
        this.ballX + this.ballSize >= this.canvasWidth - this.paddleWidth &&
        this.ballY + this.ballSize >= this.paddleRightY &&
        this.ballY <= this.paddleRightY + this.paddleHeight
      ) {
        this.ballSpeedX = -this.ballSpeedX;
      }
    },
    gameLoop() {
      this.moveBall();
      this.checkCollision();
      requestAnimationFrame(this.gameLoop);
    }
  },
  mounted() {
        // Start the game loop when the component is mounted
        this.gameLoop();

// Listen to mousemove event for paddle control
document.addEventListener("mousemove", this.movePaddle);
},
beforeUnmount() {
document.removeEventListener("mousemove", this.movePaddle);
    // Call moveBall(), movePaddle(), and checkCollision() methods
    // using setInterval or requestAnimationFrame for game loop
  },
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
