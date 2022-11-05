<script>
  import { wordStore, shuffledWordStore } from "../stores.js";

  const ENTER_KEY = 13;
  var currentInput = "";
  var correctGuess = false;
  var entered = false;

  var word;
  wordStore.subscribe((value) => {
    word = value;
  });

  var shuffledWord = word
    .split("")
    .sort(function () {
      return 0.5 - Math.random();
    })
    .join("");
  shuffledWordStore.update((_) => shuffledWord);

  var guess = "";
  function setGuess(event) {
    if (event.which === ENTER_KEY && event.target.value.length === 8) {
      guess = event.target.value;
      event.target.disabled = true;
      correctGuess = guess === word ? true : false;
      entered = true;
    }
  }
</script>

<main
  class="bg-neutral-900 text-white grid items-center justify-center text-3xl"
>
  <h1 class="text-4xl normal-font">
    Womble &nbsp;&nbsp;&nbsp;&nbsp;<a class="text-5xl" href="/">⟳</a>
  </h1>
  <div>
    <p class="uppercase tracking-[1rem] text-3xl mb-3">{shuffledWord}</p>
    <!-- svelte-ignore a11y-autofocus -->
    <input
      type="text"
      class="bg-transparent outline-none w-[17rem] tracking-[1rem] uppercase text-start"
      class:caret-transparent={currentInput.length === 8}
      class:text-white={!entered}
      class:text-red-500={entered && !correctGuess}
      class:text-green-500={entered && correctGuess}
      on:keydown={setGuess}
      bind:value={currentInput}
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
