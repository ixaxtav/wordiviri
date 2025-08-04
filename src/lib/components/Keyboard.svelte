<script lang="ts">
  export let guesses: string[];
  export let targetWord: string;
  export let onKeyPress: (key: string) => void;

  // Get keyboard letter color
  function getKeyboardLetterColor(letter: string): string {
    for (let guess of guesses) {
      for (let i = 0; i < guess.length; i++) {
        if (guess[i] === letter) {
          if (guess[i] === targetWord[i]) return 'bg-green-500';
          if (targetWord.includes(letter)) return 'bg-yellow-500';
          return 'bg-gray-500';
        }
      }
    }
    return 'bg-gray-200';
  }
</script>

<div class="keyboard">
  {#each ['QWERTYUIOP', 'ASDFGHJKL', 'ZXCVBNM'] as row}
    <div class="keyboard-row">
      {#if row === 'ZXCVBNM'}
        {#each row.split('') as letter}
          <button
            class="key {getKeyboardLetterColor(letter)}"
            on:click={() => onKeyPress(letter)}
          >
            {letter}
          </button>
        {/each}
        <button
          class="key key-special bg-gray-200"
          on:click={() => onKeyPress('Backspace')}
        >
          Back
        </button>
      {:else}
        {#each row.split('') as letter}
          <button
            class="key {getKeyboardLetterColor(letter)}"
            on:click={() => onKeyPress(letter)}
          >
            {letter}
          </button>
        {/each}
        {#if row === 'ASDFGHJKL'}
          <button
            class="key key-special bg-gray-200"
            on:click={() => onKeyPress('Enter')}
          >
            Enter
          </button>
        {/if}
      {/if}
    </div>
  {/each}
</div>

<style>
  /* Keyboard Styles */
  .keyboard {
    display: flex;
    flex-direction: column;
    gap: 8px;
    max-width: 500px;
    margin: 0 auto;
  }

  .keyboard-row {
    display: flex;
    gap: 6px;
    justify-content: center;
    align-items: center;
  }

  .key {
    min-width: 40px;
    height: 58px;
    display: flex;
    align-items: center;
    justify-content: center;
    border: none;
    border-radius: 4px;
    font-size: 14px;
    font-weight: bold;
    text-transform: uppercase;
    cursor: pointer;
    transition: all 0.2s ease;
    color: #e5e7eb;
    background-color: #374151;
  }

  .key:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
    background-color: #4b5563;
  }

  .key:active {
    transform: translateY(0);
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  }

  .key-special {
    min-width: 65px;
    font-size: 12px;
  }

  /* Color overrides for keys */
  .bg-green-500 {
    background-color: #10b981 !important;
    color: white !important;
  }

  .bg-yellow-500 {
    background-color: #f59e0b !important;
    color: white !important;
  }

  .bg-gray-500 {
    background-color: #6b7280 !important;
    color: white !important;
  }

  .bg-gray-200 {
    background-color: #4b5563 !important;
    color: #e5e7eb !important;
  }

  .bg-gray-200:hover {
    background-color: #6b7280 !important;
  }

  /* Responsive Design */
  @media (max-width: 640px) {
    .key {
      min-width: 35px;
      height: 50px;
      font-size: 12px;
    }
    
    .key-special {
      min-width: 55px;
      font-size: 10px;
    }
  }
</style>
