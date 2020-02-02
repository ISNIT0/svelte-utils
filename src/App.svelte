<script>
  import { Button, Form, Spinner, Sprite } from "./export.js";

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
  setInterval(() => spritePlaying && (spriteIndex = (spriteIndex + 1) % 32), 100);
</script>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 500px;
    margin: 0 auto;
    overflow-y: auto;
  }
</style>

<main>
  <h2>Sprite</h2>
  <Sprite
    spritesheetUrl="https://i.stack.imgur.com/wFCJb.png"
    spriteWidth="32"
    spriteHeight="32"
    width="50"
    height="50"
    rowLength="6"
    index={spriteIndex} />
    <button on:click={() => spritePlaying = !spritePlaying}>Play/Pause Sprite</button>
  <h2>Spinner</h2>
  <div style="display:flex;justify-content:center;">
    <Spinner black={true} size="50px" />
  </div>
  <h2>Buttons</h2>
  <h3>Normal</h3>
  <Button onClick={exampleButtonHandler}>Confirm & Pledge</Button>
  <h3>Disabled</h3>
  <Button onClick={exampleButtonHandler} disabled={true}>
    Confirm & Pledge
  </Button>
  <h3>Error</h3>
  <Button onClick={exampleButtonHandler} error={true}>Confirm & Pledge</Button>
  <h3>Pending</h3>
  <Button onClick={exampleButtonHandler} pending={true}>
    Confirm & Pledge
  </Button>
  <h3>Icon</h3>
  <Button onClick={exampleButtonHandler} icon="/lock.svg">
    Confirm & Pledge
  </Button>
  <h2>Form</h2>
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
</main>
