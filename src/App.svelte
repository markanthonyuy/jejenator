<script>
  import LETTER_MAP from "./CHAR_MAP.js";
  import { slide, fade } from "svelte/transition";

  let str = "";
  let jejemon = "";
  let copy = false;
  let bottomInfo = true;

  function mapLetter(letter) {
    return LETTER_MAP[letter]
      ? LETTER_MAP[letter][
          Math.floor(Math.random() * LETTER_MAP[letter].length)
        ]
      : letter;
  }

  function handleClick() {
    let text = str
      .toLowerCase()
      .trim()
      .split("");
    jejemon = text.map(l => mapLetter(l)).join("");
  }

  function handleClear() {
    str = "";
    jejemon = "";
  }

  function handleKeydown(e) {
    if (e.keyCode === 13) handleClick();
  }

  function handleKeyup(e) {
    if (e.target.value.length === 0) jejemon = "";
  }

  function handleCopy(e) {
    e.target.select();
    e.target.setSelectionRange(0, 99999);
    document.execCommand("copy");
    copy = true;
    setTimeout(() => (copy = false), 1000);
  }

  function toggleBottomInfo() {
    bottomInfo = !bottomInfo;
  }
</script>

<style>
  main {
    width: 100%;
    height: 100vh;
    margin: 0 auto;
    padding: 30px;
    text-align: center;
  }

  @media (max-width: 600px) {
    main {
      padding: 30px 5px;
    }
  }

  h1 {
    color: var(--main-color);
    text-transform: uppercase;
    font-size: 4rem;
    font-weight: 100;
  }

  @media (max-width: 600px) {
    h1 {
      font-size: 2rem;
    }
  }

  .tagname {
    margin-bottom: 20px;
    font-size: 1.4rem;
    line-height: 2rem;
    color: #a3a3a3;
  }

  .author {
    font-size: 0.8rem;
    color: #a3a3a3;
  }

  @media (max-width: 600px) {
    .tagname {
      font-size: 1rem;
    }
  }

  p {
    line-height: 1.5rem;
  }

  .form {
    position: relative;
    max-width: 400px;
    margin: 0 auto;
    padding: 20px;
    display: flex;
    justify-content: space-between;
  }

  @media (max-width: 600px) {
    .form {
      padding: 20px 5px;
    }
  }

  .form > input {
    padding: 10px 40px 10px 10px;
    width: calc(100% - 85px);
  }

  @media (max-width: 600px) {
    .form > input {
      padding-right: 40px;
    }
  }

  .form > button {
    padding: 10px;
    background: var(--main-color);
    color: #fff;
  }

  .form > .clear {
    position: absolute;
    top: 35px;
    right: 110px;
    color: #ccc;
    font-size: 0.7rem;
    cursor: pointer;
    text-transform: uppercase;
  }

  @media (max-width: 600px) {
    .form > .clear {
      right: 95px;
    }
  }

  .jejemon-wrap {
    position: relative;
    max-width: 500px;
    margin: 0 auto;
  }

  @media (max-width: 600px) {
    .jejemon-wrap {
      width: 100%;
    }
  }

  .copied {
    position: absolute;
    top: 5px;
    right: 5px;
    color: var(--main-color);
    font-size: 0.9rem;
    text-transform: uppercase;
  }

  .jejemon {
    min-width: 300px;
    max-width: 500px;
    margin: 0 auto 5px;
    padding: 10px;
    font-size: 1.8rem;
    line-height: 2.5rem;
    background: #f9f9f9;
    border: 1px dashed #ccc;
    cursor: pointer;
    outline: none;
  }

  @media (max-width: 600px) {
    .jejemon {
      width: 100%;
    }
  }

  .copy {
    font-size: 0.8rem;
    line-height: 1rem;
    color: var(--main-color);
    text-transform: uppercase;
  }

  .toggle {
    text-transform: uppercase;
    font-size: 13px;
  }

  .toggle > span {
    cursor: pointer;
  }

  .info {
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100%;
    max-height: 30vh;
    padding: 10px;
    background: #f9f9f9;
    text-align: left;
    font-size: 1rem;
    overflow-y: auto;
  }

  .box {
    padding: 5px 0;
    display: flex;
    justify-content: flex-start;
    align-items: top;
    border-bottom: 1px solid #e6e6e6;
  }

  .box:last-child {
    border-bottom: none;
  }

  .box > .question {
    width: 30%;
    color: var(--main-color);
    text-align: right;
  }

  .box > .answer {
    width: 70%;
    margin: 4px 0 0 10px;
    color: #7a7a7a;
    font-size: 0.9rem;
    line-height: 1.2rem;
  }
</style>

<main transition:fade={{ duration: 300 }}>
  <h1>Jejenator!</h1>
  <p class="tagname">The Jejemon Generator.</p>

  <p class="author">
    Created by
    <a href="https://markanthonyuy.com/">mAarK 4nH+HOnHY UyH</a>
    | Built using
    <a href="https://svelte.dev/">Svelte.js</a>
  </p>

  <div class="form">
    <input
      type="text"
      bind:value={str}
      on:keydown={handleKeydown}
      on:keyup={handleKeyup}
      placeholder="Enter text you want to jejenize." />
    {#if str.length}
      <span
        class="clear"
        on:click={handleClear}
        transition:fade={{ duration: 500 }}>
        Clear
      </span>
    {/if}
    <button type="button" on:click={handleClick}>Jejenize!</button>
  </div>

  {#if jejemon.length}
    <div class="jejemon-wrap" transition:slide={{ duration: 200 }}>
      {#if copy}
        <span class="copied" transition:fade={{ duration: 300 }}>Copied!</span>
      {/if}

      <!-- prettier-ignore-start -->
      <textarea
        type="text"
        class="jejemon"
        readonly
        on:click={handleCopy}
        rows="5"
        value={jejemon} />
      <!-- prettier-ignore-end -->
    </div>
    <span class="copy">Click box to copy to the clipboard</span>
  {/if}

  <p class="toggle">
    <span on:click={toggleBottomInfo}>
      {bottomInfo ? 'Close' : 'Open'} Info
    </span>
  </p>

  {#if bottomInfo}
    <div class="info" transition:slide={{ duration: 200 }}>
      <div class="box">
        <p class="question">What is Jejemon?</p>
        <p class="answer">
          A Jejemon is a type of person in the Philippines who makes the English
          language hard to read.
          <sup>
            <a href="https://simple.wikipedia.org/wiki/Jejemon">[wikipedia]</a>
          </sup>
        </p>
      </div>
      <div class="box">
        <p class="question">How is this helpful?</p>
        <p class="answer">
          Short answer,
          <b>Password Generation!</b>
          Being able to literally see this on my text messages in the early 2000
          makes my eyes bleed. Good luck trying to remember your
          <b>Jejemon</b>
          password. Jejeje!
        </p>
      </div>
      <div class="box">
        <p class="question">Why did you build this?</p>
        <p class="answer">It's fun! And I really want to try out Svelte.js</p>
      </div>
    </div>
  {/if}
</main>
