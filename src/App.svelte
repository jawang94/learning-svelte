<script>
  import Nested from "./Nested.svelte";
  import Info from "./Info.svelte";
  import Thing from "./Thing.svelte";

  export let name;

  // Counter
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

  // Sum an array
  let numbers = [1, 2, 3, 4];

  const addNumber = () => {
    numbers[numbers.length] = numbers.length + 1;
  };

  $: sum = numbers.reduce((t, n) => t + n, 0);

  // Spreading prop object
  const pkg = {
    name: "svelte",
    version: 3,
    speed: "blazing",
    website: "https://svelte.dev",
  };

  // Log in and Log Out
  let user = { loggedIn: false };

  const toggle = () => {
    user.loggedIn = !user.loggedIn;
  };

  // If else if block
  let x = 7;

  // Loop over list of data
  let cats = [
    { id: "J---aiyznGQ", name: "Keyboard Cat" },
    { id: "z_AbfPXTKms", name: "Maru" },
    { id: "OUtn3pvWmpg", name: "Henri The Existential Cat" },
  ];

  let things = [
    { id: 1, color: "#0d0887" },
    { id: 2, color: "#6a00a8" },
    { id: 3, color: "#b12a90" },
    { id: 4, color: "#e16462" },
    { id: 5, color: "#fca636" },
  ];

  const handleThingClick = () => {
    things = things.slice(1);
  };

  // Await blocks
  const getRandomNumber = async () => {
    const res = await fetch(
      `https://www.random.org/integers/?num=1&min=1&max=6&col=1&base=10&format=plain&rnd=new`
    );
    const text = await res.text();

    if (res.ok) {
      return text;
    } else {
      throw new Error(text);
    }
  };

  let promise = getRandomNumber();

  const handlePromiseClick = () => {
    promise = getRandomNumber();
  };

  // DOM Events
  let m = { x: 0, y: 0 };

  const handleMousemove = (event) => {
    m.x = event.clientX;
    m.y = event.clientY;
  };
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

  .container {
    width: 100%;
    height: 100%;
  }
</style>

<main>
  <div class="container" on:mousemove={handleMousemove}>
    <div id="intro">
      <h1>Hello {name}!</h1>
      <p>Messing around w/ Svelte. This is kind of cool.</p>
    </div>

    <div id="counter">
      <button on:click={handleClick}>
        Clicked
        {count}
        {count === 1 ? 'time' : 'times'}
      </button>
    </div>

    <div id="reactivity">
      <p>{count} doubled is {doubled}</p>
    </div>

    <div id="sumAnArrayReactively">
      <p>{numbers.join(' + ')} = {sum}</p>
      <button on:click={addNumber}>Add a number</button>
    </div>

    <div id="passingPropsAndDefaultProps">
      <Nested answer={42} />
      <Nested />
    </div>

    <div id="spreadingProps">
      <Info {...pkg} />
    </div>

    <div id="inlineLogic">
      {#if user.loggedIn}<button on:click={toggle}>Log out</button>{/if}
      {#if !user.loggedIn}<button on:click={toggle}>Log in</button>{/if}
    </div>

    <div id="ifElseBlock">
      {#if user.loggedIn}
        <button on:click={toggle}>Log out</button>
      {:else}<button on:click={toggle}>Log in</button>{/if}
    </div>

    <div id="ifElseIfBlock">
      {#if x > 10}
        <p>{x} is greater than 10</p>
      {:else if x < 5}
        <p>{x} is less than 5</p>
      {:else}
        <p>{x} is between 5 and 10</p>
      {/if}
    </div>

    <div id="loopingOverData">
      <ul>
        {#each cats as { id, name }, i}
          <li>
            <a
              target="_blank"
              href="https://www.youtube.com/watch?v={id}">{i + 1}:
              {name}</a>
          </li>
        {/each}
      </ul>
    </div>

    <div id="keyedEachBlocks">
      <button on:click={handleThingClick}> Remove first thing </button>

      {#each things as thing (thing.id)}
        <Thing current={thing.color} />
      {/each}
    </div>

    <div id="asyncBlock">
      <button on:click={handlePromiseClick}>Generate a random number</button>

      {#await promise}
        <p>...waiting</p>
      {:then number}
        <p>The number is {number}</p>
      {:catch error}
        <p style={{ color: 'red' }}>{error.message}</p>
      {/await}
    </div>

    <div id="DOMEvents">The mouse position is {m.x} x {m.y}</div>

    <div on:mousemove={(e) => (m = { x: e.clientX, y: e.clientY })}>
      The mouse position is
      {m.x}
      x
      {m.y}, calculated inline!
    </div>
  </div>
</main>
