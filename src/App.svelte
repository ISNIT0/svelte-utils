<script>
  import ExampleRoute1 from "./exampleRoutes/ExampleRoute1.svelte";
  import ExampleRoute2 from "./exampleRoutes/ExampleRoute2.svelte";
  import { Button, Form, Spinner, Sprite, StackRouter } from "./export.js";

  function exampleButtonHandler() {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        if (Math.random() < 0.5) {
          resolve();
        } else {
          reject();
        }
      }, 5000);
    });
  }

  let spriteIndex = 0;
  let spritePlaying = false;
  setInterval(
    () => spritePlaying && (spriteIndex = (spriteIndex + 1) % 32),
    100
  );

  let routerStack;
  let router;
  function routerReady(_router) {
    router = _router;
  }

  const routes = [
    {
      component: ExampleRoute1,
      props: { name: "Fred" }
    },
    {
      component: ExampleRoute2,
      props: { name: "Joe" }
    },
    {
      component: ExampleRoute1,
      props: { name: "Max" }
    },
    {
      component: ExampleRoute2,
      props: { name: "Joe" }
    }
  ];
</script>

<main>
  <h1>Svelte Utils Demos</h1>
  <h3>
    <a href="https://github.com/isnit0/svelte-utils">View on GitHub</a>
  </h3>
  <hr />
  <h2>Buttons</h2>
  <div class="cont">
    <h3>Normal</h3>
    <Button onClick={exampleButtonHandler}>Confirm</Button>
    <h3>Disabled</h3>
    <Button onClick={exampleButtonHandler} disabled={true}>Confirm</Button>
    <h3>Error</h3>
    <Button onClick={exampleButtonHandler} error={true}>Confirm</Button>
    <h3>Pending</h3>
    <Button onClick={exampleButtonHandler} pending={true}>Confirm</Button>
    <h3>Icon & Color</h3>
    <Button
      onClick={exampleButtonHandler}
      icon="/lock.svg"
      backgroundColor="red">
      Confirm
    </Button>
  </div>

  <h2>Form</h2>
  <div class="cont">
    <div class="form-cont">
      <Form
        onDone={async function() {}}
        buttonIcon="/lock.svg"
        handleSubmit={async function(data, ev) {
          return new Promise((resolve, reject) => {
            setTimeout(() => {
              if (Math.random() < 0.5) {
                resolve();
              } else {
                reject({ message: 'Some Error Message' });
              }
            }, 3000);
          });
        }}
        buttonText="Submit Form">
        <div class="title">This is a form title</div>
        <div class="description">It looks like you're new here</div>
        <div class="input">
          <label for="name">Name</label>
          <input
            type="text"
            id="name"
            name="name"
            placeholder="Joe Bloggs"
            required />
        </div>
      </Form>
    </div>
  </div>

  <h2>Spinner</h2>
  <div class="cont">
    <div style="display:flex;justify-content:center;">
      <Spinner black={true} size="50px" />
    </div>
  </div>

  <h2>Sprite</h2>
  <div class="cont">
    <div style="display:flex;justify-content:center;">
      <Sprite
        spritesheetUrl="https://i.stack.imgur.com/wFCJb.png"
        spriteWidth="32"
        spriteHeight="32"
        width="50"
        height="50"
        rowLength="6"
        index={spriteIndex} />
    </div>
    <button on:click={() => (spritePlaying = !spritePlaying)}>
      Play/Pause Sprite
    </button>
  </div>

  <h2>Stack Router</h2>
  <div class="cont">
    <div style="background:#f2f2f2;">
      <StackRouter
        onReady={routerReady}
        defaultRoute={routes[0]}
        bind:stack={routerStack} />
    </div>
    <small>Stack Length: {routerStack && routerStack.length}</small>
    <div>
      <button on:click={() => router.pushRoute(routes[routerStack.length % 3])}>
        Push To Stack
      </button>
      <button on:click={() => router.popRoute()}>Pop From Stack</button>
    </div>
  </div>
  <h3>
    <a href="https://github.com/isnit0/svelte-utils">Star on GitHub</a>
  </h3>
</main>
