<script lang="ts">
  export let guesses: string[];
  export let currentGuess: string;
  export let targetWord: string;
  export let shakeRow: number;

  // Get letter color for feedback
  function getLetterColor(guess: string, letter: string, index: number): string {
    if (guess[index] === targetWord[index]) return 'bg-green-500';
    if (targetWord.includes(letter)) return 'bg-yellow-500';
    return 'bg-gray-500';
  }
</script>

<div class="game-grid">
  {#each Array(6) as _, row}
    <div class="grid-row {row === shakeRow ? 'shake-animation' : ''}">
      {#each Array(5) as _, col}
        <div
          class="game-tile {row < guesses.length ? getLetterColor(guesses[row], guesses[row][col], col) : ''} 
          {row === guesses.length && col < currentGuess.length ? 'typing-animation' : ''}
          {row < guesses.length ? 'reveal-animation' : ''}"
          style="animation-delay: {col * 0.1}s"
        >
          {row < guesses.length ? guesses[row][col] || '' : row === guesses.length && col < currentGuess.length ? currentGuess[col] : ''}
        </div>
      {/each}
    </div>
  {/each}
</div>

<style>
  /* Game Grid Styles */
  .game-grid {
    display: flex;
    flex-direction: column;
    gap: 8px;
    max-width: 350px;
    margin: 0 auto;
  }

  .grid-row {
    display: flex;
    gap: 8px;
    justify-content: center;
  }

  /* Shake animation for wrong guesses */
  .shake-animation {
    animation: shake 0.5s ease-in-out;
  }

  @keyframes shake {
    0%, 100% { transform: translateX(0); }
    25%, 75% { transform: translateX(-3px); }
    50% { transform: translateX(3px); }
  }

  .game-tile {
    width: 60px;
    height: 60px;
    display: flex;
    align-items: center;
    justify-content: center;
    border: 2px solid #4b5563;
    font-size: 24px;
    font-weight: bold;
    text-transform: uppercase;
    transition: all 0.3s ease;
    border-radius: 4px;
    background-color: #1f2937;
    color: #e5e7eb;
  }

  .game-tile:hover {
    border-color: #6b7280;
    transform: scale(1.05);
  }

  /* Typing animation */
  .typing-animation {
    animation: bounce 0.3s ease;
    border-color: #9ca3af !important;
    background-color: #374151 !important;
  }

  @keyframes bounce {
    0%, 100% { transform: scale(1); }
    50% { transform: scale(1.1); }
  }

  /* Reveal animation for completed words */
  .reveal-animation {
    animation: flip 0.6s ease forwards;
  }

  @keyframes flip {
    0% { transform: rotateX(0); }
    50% { transform: rotateX(90deg); }
    100% { transform: rotateX(0); }
  }

  /* Color overrides for tiles */
  .bg-green-500 {
    background-color: #10b981 !important;
    color: white !important;
    border-color: #10b981 !important;
  }

  .bg-yellow-500 {
    background-color: #f59e0b !important;
    color: white !important;
    border-color: #f59e0b !important;
  }

  .bg-gray-500 {
    background-color: #6b7280 !important;
    color: white !important;
    border-color: #6b7280 !important;
  }

  /* Responsive Design */
  @media (max-width: 640px) {
    .game-tile {
      width: 50px;
      height: 50px;
      font-size: 20px;
    }
  }
</style>
