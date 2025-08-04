<script lang="ts">
  import { onMount } from 'svelte';
  import { writable } from 'svelte/store';
  import GameGrid from '$lib/components/GameGrid.svelte';
  import Keyboard from '$lib/components/Keyboard.svelte';
  import Fireworks from '$lib/components/Fireworks.svelte';
  import { GAME_WORDS, GAME_CONFIG } from '$lib/constants';

  // Game state
  let targetWord: string = '';
  let guesses = writable<string[]>([]);
  let currentGuess: string = '';
  let gameOver: boolean = false;
  let win: boolean = false;
  let winStreak = writable<number>(0);
  let lossStreak = writable<number>(0);
  let message: string = '';
  let showFireworks: boolean = false;
  let shakeRow: number = -1; // Track which row should shake

  interface GameState {
    targetWord: string;
    guesses: string[];
    currentGuess: string;
    gameOver: boolean;
    win: boolean;
    message: string;
  }

  // Load game state and streaks from local storage
  onMount(() => {
    const savedWinStreak = localStorage.getItem('winStreak');
    const savedLossStreak = localStorage.getItem('lossStreak');
    const savedGameState = localStorage.getItem('currentGame');
    
    if (savedWinStreak) winStreak.set(parseInt(savedWinStreak));
    if (savedLossStreak) lossStreak.set(parseInt(savedLossStreak));
    
    if (savedGameState) {
      const gameState: GameState = JSON.parse(savedGameState);
      targetWord = gameState.targetWord;
      guesses.set(gameState.guesses);
      currentGuess = gameState.currentGuess;
      gameOver = gameState.gameOver;
      win = gameState.win;
      message = gameState.message;
    } else {
      resetGame();
    }
  });

  // Save game state to localStorage
  function saveGameState(): void {
    const gameState: GameState = {
      targetWord,
      guesses: $guesses,
      currentGuess,
      gameOver,
      win,
      message
    };
    localStorage.setItem('currentGame', JSON.stringify(gameState));
  }

  // Reset game and start new round
  function resetGame() {
    targetWord = GAME_WORDS[Math.floor(Math.random() * GAME_WORDS.length)];
    guesses.set([]);
    currentGuess = '';
    gameOver = false;
    win = false;
    message = '';
    showFireworks = false;
    shakeRow = -1;
  }

  // Handle keyboard input
  function handleKeyPress(key: string): void {
    // If game is over, only allow Enter to start new game
    if (gameOver) {
      if (key === 'Enter') {
        startNewGame();
      }
      return;
    }

    if (key === 'Enter') {
      if (currentGuess.length === GAME_CONFIG.WORD_LENGTH) {
        const upperGuess = currentGuess.toUpperCase();
        if (!GAME_WORDS.includes(upperGuess)) {
          message = 'Not a valid word!';
          setTimeout(() => (message = ''), GAME_CONFIG.MESSAGE_DISPLAY_DURATION);
          return;
        }
        
        guesses.update(g => [...g, upperGuess]);
        console.log('Current guesses:', $guesses); // Debug log
        console.log("Target word:", targetWord); // Debug log
        if (upperGuess === targetWord) {
          // WIN: Game over, show fireworks
          win = true;
          gameOver = true;
          showFireworks = true;
          winStreak.update(n => {
            const newStreak = n + 1;
            localStorage.setItem('winStreak', newStreak.toString());
            localStorage.setItem('lossStreak', '0');
            lossStreak.set(0);
            return newStreak;
          });
          message = `🎉 You won! The word was ${targetWord}`;
          
        } else if ($guesses.length >= GAME_CONFIG.MAX_ATTEMPTS) {
          // LOSE: Game over after max attempts
          gameOver = true;
          lossStreak.update(n => {
            const newStreak = n + 1;
            localStorage.setItem('lossStreak', newStreak.toString());
            localStorage.setItem('winStreak', '0');
            winStreak.set(0);
            return newStreak;
          });
          message = `💀 Game over! The word was ${targetWord}`;
          
        } else {
          // Wrong guess: shake row and continue
          shakeRow = $guesses.length - 1;
          setTimeout(() => {
            shakeRow = -1;
          }, GAME_CONFIG.SHAKE_DURATION);
        }
        currentGuess = '';
        saveGameState();
      } else {
        message = `Guess must be ${GAME_CONFIG.WORD_LENGTH} letters!`;
        setTimeout(() => (message = ''), GAME_CONFIG.MESSAGE_DISPLAY_DURATION);
      }
    } else if (key === 'Backspace') {
      currentGuess = currentGuess.slice(0, -1);
      saveGameState();
    } else if (/^[a-zA-Z]$/.test(key) && currentGuess.length < GAME_CONFIG.WORD_LENGTH) {
      currentGuess += key.toUpperCase();
      saveGameState();
    }
  }

  // Manual new game function for button
  function startNewGame(): void {
    resetGame();
    message = '';
    showFireworks = false;
    saveGameState(); // This was missing! Need to save state after reset
  }
</script>

<svelte:window on:keydown={e => handleKeyPress(e.key)} />

<Fireworks show={showFireworks} />

<div class="flex flex-col items-center justify-center min-h-screen bg-gray-900 px-4 text-white">
  <h1 class="text-4xl font-bold mb-6 text-white">WORDIVIRI</h1>
  <div class="mb-6 text-center">
    <p class="text-lg font-semibold">Win Streak: <span class="text-green-400">{$winStreak}</span></p>
    <p class="text-lg font-semibold">Loss Streak: <span class="text-red-400">{$lossStreak}</span></p>
  </div>
  
  <!-- Fixed height message container to prevent layout shift -->
  <div class="message-container mb-4">
    {#if message}
      <p class="text-yellow-400 text-lg font-semibold">{message}</p>
    {/if}
  </div>
  
  <!-- Game grid component -->
  <div class="mb-6">
    <GameGrid 
      guesses={$guesses} 
      currentGuess={currentGuess} 
      targetWord={targetWord}
      shakeRow={shakeRow}
    />
  </div>
  
  <!-- Keyboard component -->
  <div class="pt-6">
    <Keyboard 
      guesses={$guesses} 
      targetWord={targetWord} 
      onKeyPress={handleKeyPress}
    />
  </div>
  
  <!-- Fixed position for new game button -->
  <div class="play-again-container">
    {#if gameOver}
      <button
        class="btn bg-purple-500 text-white px-4 py-2 rounded mr-4"
        on:click={startNewGame}
      >
        New Game
      </button>
      <p class="text-gray-400 text-sm">Press Enter to start new game</p>
    {:else}
      <button
        class="btn bg-purple-500 text-white px-4 py-2 rounded"
        on:click={startNewGame}
      >
        New Game? 🤔
      </button>
    {/if}
  </div>
</div>

<style>
  @import 'https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css';

  /* Fixed height message container to prevent layout shift */
  .message-container {
    height: 32px;
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
  }

  /* Play Again Button Container */
  .play-again-container {
    height: 60px;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-top: 20px;
    gap: 16px;
  }
</style>
