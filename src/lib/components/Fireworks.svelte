<script lang="ts">
  export let show: boolean;
</script>

{#if show}
  <div class="fireworks-container">
    <div class="firework"></div>
    <div class="firework"></div>
    <div class="firework"></div>
  </div>
{/if}

<style>
  /* Fireworks Animation */
  .fireworks-container {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    pointer-events: none;
    z-index: 1000;
  }

  .firework {
    position: absolute;
    width: 4px;
    height: 4px;
    border-radius: 50%;
    animation: firework 2s ease-out infinite;
  }

  .firework:nth-child(1) {
    left: 20%;
    animation-delay: 0s;
    background: linear-gradient(45deg, #ff6b6b, #feca57);
  }

  .firework:nth-child(2) {
    left: 50%;
    animation-delay: 0.5s;
    background: linear-gradient(45deg, #48dbfb, #0abde3);
  }

  .firework:nth-child(3) {
    left: 80%;
    animation-delay: 1s;
    background: linear-gradient(45deg, #ff9ff3, #f368e0);
  }

  @keyframes firework {
    0% {
      transform: translateY(100vh) scale(0);
      opacity: 1;
    }
    15% {
      transform: translateY(20vh) scale(1);
      opacity: 1;
    }
    20% {
      transform: translateY(20vh) scale(1);
      opacity: 1;
      box-shadow: 
        0 0 0 0px #ff6b6b,
        0 0 0 0px #feca57,
        0 0 0 0px #48dbfb,
        0 0 0 0px #ff9ff3;
    }
    25% {
      transform: translateY(20vh) scale(1);
      opacity: 1;
      box-shadow: 
        20px 0 0 4px #ff6b6b,
        -20px 0 0 4px #feca57,
        0 20px 0 4px #48dbfb,
        0 -20px 0 4px #ff9ff3,
        15px 15px 0 4px #ff6b6b,
        -15px -15px 0 4px #feca57,
        15px -15px 0 4px #48dbfb,
        -15px 15px 0 4px #ff9ff3;
    }
    30% {
      transform: translateY(20vh) scale(1);
      opacity: 0.8;
      box-shadow: 
        40px 0 0 2px #ff6b6b,
        -40px 0 0 2px #feca57,
        0 40px 0 2px #48dbfb,
        0 -40px 0 2px #ff9ff3,
        30px 30px 0 2px #ff6b6b,
        -30px -30px 0 2px #feca57,
        30px -30px 0 2px #48dbfb,
        -30px 30px 0 2px #ff9ff3;
    }
    100% {
      transform: translateY(20vh) scale(1);
      opacity: 0;
      box-shadow: 
        60px 0 0 0px #ff6b6b,
        -60px 0 0 0px #feca57,
        0 60px 0 0px #48dbfb,
        0 -60px 0 0px #ff9ff3,
        45px 45px 0 0px #ff6b6b,
        -45px -45px 0 0px #feca57,
        45px -45px 0 0px #48dbfb,
        -45px 45px 0 0px #ff9ff3;
    }
  }
</style>
