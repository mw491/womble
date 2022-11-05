<script>
  export let getWord;

  const ENTER_KEY = 13; // key code for the enter key
  let currentInput = ""; // the current text in the input
  let inputRef;
  let correctGuess = false; // if user enters correct guess
  let entered = false; // if user has submitted their guess with enter key
  let showWord = false; // weather to show the word or not
  let word, shuffledWord;
  let message = "";

  function setWord() {
    message = "";
    entered = false;
    showWord = false;
    correctGuess = false;
    currentInput = "";
    var words = getWord();
    word = words[0];
    shuffledWord = words[1];
  }

  function resetWord() {
    setWord();
    inputRef.disabled = false;
    inputRef.focus();
  }

  setWord();

  let guess = ""; // will be set to user's guess
  function setGuess(event) {
    if (event.which === ENTER_KEY && event.target.value.length === 8) {
      guess = event.target.value; // set guess to user guess
      correctGuess = guess === word ? true : false; // check if guess is correct
      entered = true;
      showWord = true;
      chances_used += 1;
    }
  }
</script>

<main
  class="bg-neutral-900 text-white grid items-center justify-center text-3xl"
>
  <h1 class="text-4xl normal-font">
    Womble &nbsp;&nbsp;&nbsp;&nbsp;<button title="Replay" class="text-5xl" on:click={resetWord}>⟳</button>
  </h1>
  <div>
    <p class="uppercase tracking-[1rem] text-3xl mb-3">{showWord ? word : shuffledWord}</p>
    <!-- svelte-ignore a11y-autofocus -->
    <input
      type="text"
      disabled="{entered}"
      class="bg-transparent outline-none w-[17rem] tracking-[1rem] uppercase text-start"
      class:caret-transparent={currentInput.length === 8}
      class:text-white={!entered}
      class:text-red-500={entered && !correctGuess}
      class:text-green-500={entered && correctGuess}
      on:keydown={setGuess}
      bind:value={currentInput}
      bind:this={inputRef}
      maxlength="8"
      autofocus
    />
  </div>
  <p class="opacity-50 text-base normal-font">
    © <a class="underline" href="https://github.com/mw491">mw491</a> 2022
  </p>
</main>

<style>
  main {
    height: 100vh;
    width: 100vw;
    font-family: "Roboto Mono";
  }
  .normal-font {
    font-family: "Bebas Neue";
  }
</style>
