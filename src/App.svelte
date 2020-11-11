<script>
  import Nested from "./Nested.svelte";
  export let name;

  let count = 0;
  $: doubled = count * 2;
  $: console.log(`the count is ${count}`);
  $: if (count >= 10) {
    alert(`count is dangerously high!`);
    count = 9;
  }

  const handleClick = () => {
    count += 1;
  };

  let numbers = [1, 2, 3, 4];

  const addNumber = () => {
    numbers[numbers.length] = numbers.length + 1;
  };

  $: sum = numbers.reduce((t, n) => t + n, 0);
</script>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>

<main>
  <h1>Hello {name}!</h1>
  <p>
    Visit the
    <a href="https://svelte.dev/tutorial">Svelte tutorial</a>
    to learn how to build Svelte apps.
  </p>
  <button on:click={handleClick}>
    Clicked
    {count}
    {count === 1 ? 'time' : 'times'}
  </button>
  <p>{count} doubled is {doubled}</p>

  <p>{numbers.join(' + ')} = {sum}</p>
  <button on:click={addNumber}>Add a number</button>

  <Nested answer={42} />
  <Nested />
</main>
