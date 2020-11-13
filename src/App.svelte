<script>
  export let name;

  import marked from "marked";
  import Nested from "./Nested.svelte";
  import Info from "./Info.svelte";
  import Thing from "./Thing.svelte";
  import Outer from "./Outer.svelte";
  import CustomButton from "./CustomButton.svelte";

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

  const handleDOMClick = () => {
    alert("clicked");
  };

  // Component Events
  const handleMessage = (event) => {
    alert(event.detail.text);
  };

  // Forward DOM Events
  const handleCustomButtonClick = () => {
    alert("custom button clicked");
  };

  // Text Input Binding
  let inputName = "Jason Wang";

  // Numeric Inputs, bind:value auto converts to appropriate type aka int.
  let a = 1;
  let b = 2;

  // Checkbox Inputs, bound to checked boolean
  let yes = false;

  // Group Inputs
  let scoops = 1;
  let flavours = [];

  let menu = ["Cookies and cream", "Mint choc chip", "Raspberry ripple"];

  const join = (flavours) => {
    if (flavours.length === 1) return flavours[0];
    return `${flavours.slice(0, -1).join(", ")} and ${
      flavours[flavours.length - 1]
    }`;
  };

  // Text Area
  let value = `Some words are *italic*, some are **bold**`;

  // Select Bindings
  let questions = [
    { id: 1, text: `Where did you go to school?` },
    { id: 2, text: `What is your mother's name?` },
    {
      id: 3,
      text: `What is another personal fact that an attacker could easily find with Google?`,
    },
  ];

  let selected;

  let answer = "";

  const handleSubmit = () => {
    alert(
      `answered question ${selected.id} (${selected.text}) with "${answer}"`
    );
  };

  // Contenteditable Divs
  let html = "<h3>Write some text!</h3>";

  // Each Block Bindings
  let todos = [
    { done: false, text: "finish Svelte tutorial" },
    { done: false, text: "build an app" },
    { done: false, text: "world domination" },
  ];

  const add = () => {
    todos = todos.concat({ done: false, text: "" });
  };

  const clear = () => {
    todos = todos.filter((t) => !t.done);
  };

  $: remaining = todos.filter((t) => !t.done).length;
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

  textarea {
    width: 100%;
    height: 200px;
  }

  [contenteditable] {
    padding: 0.5em;
    border: 1px solid #eee;
    border-radius: 4px;
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

  .done {
    opacity: 0.4;
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

    <div
      id="DOMEvents2"
      on:mousemove={(e) => (m = { x: e.clientX, y: e.clientY })}>
      The mouse position is
      {m.x}
      x
      {m.y}, calculated inline!
    </div>

    <div id="DOMEventModifier">
      <button on:click|preventDefault|stopPropagation={handleDOMClick}>Click me</button>
    </div>

    <div id="eventDispatchForward">
      <Outer on:message={handleMessage} />
    </div>

    <div id="DOMEventDispatchForward">
      <CustomButton on:click={handleCustomButtonClick} />
    </div>

    <div id="inputBindings">
      <input type="text" bind:value={inputName} />
      <h1>Hello {inputName}!</h1>
    </div>

    <div id="numericInputs">
      <label>
        <input type="number" bind:value={a} min="0" max="10" />
        <input type="range" bind:value={a} min="0" max="10" />
      </label>

      <label>
        <input type="number" bind:value={b} min="0" max="10" />
        <input type="range" bind:value={b} min="0" max="10" />
      </label>

      <p>{a} + {b} = {a + b}</p>
    </div>

    <div id="checkboxInputs">
      <label>
        <input type="checkbox" bind:checked={yes} />
        Yes! Send me regular email spam
      </label>

      {#if yes}
        <p>
          Thank you. We will bombard your inbox and sell your personal details.
        </p>
      {:else}
        <p>
          You must opt in to continue. If you're not paying, you're the product.
        </p>
      {/if}

      <button disabled={!yes}> Subscribe </button>
    </div>

    <div id="groupedInputs">
      <h2>Size</h2>

      <label>
        <input type="radio" bind:group={scoops} value={1} />
        One scoop
      </label>

      <label>
        <input type="radio" bind:group={scoops} value={2} />
        Two scoops
      </label>

      <label>
        <input type="radio" bind:group={scoops} value={3} />
        Three scoops
      </label>

      <h2>Flavours</h2>

      {#each menu as flavour}
        <label>
          <input type="checkbox" bind:group={flavours} value={flavour} />
          {flavour}
        </label>
      {/each}

      <select multiple bind:value={flavours}>
        {#each menu as flavour}
          <option value={flavour}>{flavour}</option>
        {/each}
      </select>

      {#if flavours.length === 0}
        <p>Please select at least one flavour</p>
      {:else if flavours.length > scoops}
        <p>Can't order more flavours than scoops!</p>
      {:else}
        <p>
          You ordered
          {scoops}
          {scoops === 1 ? 'scoop' : 'scoops'}
          of
          {join(flavours)}
        </p>
      {/if}
    </div>

    <div id="textArea">
      <textarea bind:value />
      {@html marked(value)}
    </div>

    <div id="selectBindings">
      <h2>Insecurity questions</h2>

      <form on:submit|preventDefault={handleSubmit}>
        <select bind:value={selected} on:blur={() => (answer = '')}>
          {#each questions as question}
            <option value={question}>{question.text}</option>
          {/each}
        </select>

        <input bind:value={answer} />

        <button disabled={!answer} type="submit"> Submit </button>
      </form>

      <p>selected question {selected ? selected.id : '[waiting...]'}</p>
    </div>

    <div id="contenteditableDivs">
      <div contenteditable="true" bind:innerHTML={html} />
      <pre>{html}</pre>
    </div>

    <div id="bindInEach">
      <h1>Todos</h1>

      <!-- Note that binding in this way mutates the data array. to avoid, we would use event handlers instead. -->
      {#each todos as todo}
        <div class:done={todo.done}>
          <input type="checkbox" bind:checked={todo.done} />

          <input placeholder="What needs to be done?" bind:value={todo.text} />
        </div>
      {/each}

      <p>{remaining} remaining</p>

      <button on:click={add}> Add new </button>

      <button on:click={clear}> Clear completed </button>
    </div>
  </div>
</main>
