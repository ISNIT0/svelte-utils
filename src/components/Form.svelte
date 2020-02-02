<script>
  import { fade } from "svelte/transition";
  import Button from "./Button.svelte";
  import Spinner from "./Spinner.svelte";

  export let afterButtonText = "";
  export let buttonText = "Next";
  export let handleSubmit;
  export let onDone;
  export let buttonIcon = "";
  export let disabled = false;
  let pending = false;
  let success = false;
  let error = false;
  let errorMessage;

  async function onSubmit(ev) {
    ev.preventDefault();
    pending = true;

    try {
      const $form = ev.target;
      const inputs = Array.from(
        $form.querySelectorAll("input[name],textarea[name]")
      );
      const data = inputs.reduce((acc, input) => {
        if (input.type === "checkbox") {
          if (input.checked) {
            acc[input.name] = (acc[input.name] || []).concat(input.value);
          }
        } else {
          acc[input.name] = input.value;
        }
        return acc;
      }, {});

      const doneData = await handleSubmit(data, ev);
      pending = false;
      success = true;

      setTimeout(() => {
        onDone(doneData);
      }, 700);
    } catch (err) {
      pending = false;
      success = false;
      error = err;
    }
  }
</script>

<style>
  form {
    text-align: left;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    margin-bottom: 24px;
  }

  form :global(.title) {
    font-size: 22px;
    line-height: 24px;
    margin-bottom: 8px;
    text-align: left;
    font-weight: 600;
  }

  form :global(label) {
    font-weight: 600;
    font-size: 12px;
    line-height: 16px;
    color: #4f4f4f;
    margin: 4px 0;
    display: block;
    transition: 0.3s;
  }

  form :global(label.info) {
    opacity: 0.5;
    font-weight: 300;
  }

  form :global(label.error) {
    opacity: 0.5;
    font-weight: 300;
    color: red;
  }

  form :global(label.optional:after) {
    content: " optional";
    opacity: 0.5;
    font-size: 80%;
    font-weight: 300;
  }

  form :global(input, textarea) {
    background: #ffffff;
    border: 1px solid #dddddd;
    box-sizing: border-box;
    box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.08);
    border-radius: 4px;
    width: 100%;
    font-size: 17px;
    line-height: 22px;
    padding: 13px;
    display: block;
  }

  form :global(div.input) {
    margin-bottom: 24px;
  }

  form .form-body {
    display: flex;
    flex-direction: column;
    height: 100%;
    overflow-y:auto;
  }

  form :global(input[type="checkbox"]) {
    width: auto;
    display: inline-block;
  }
  form :global(input[type="checkbox"] + label) {
    display: inline-block;
    font-weight: normal;
  }
</style>

<form action="#" on:submit={onSubmit}>
  <div class="form-body">
    <slot />
  </div>
  <div>

    {#if error && !error.hidden}
      <label class="error" style="text-align:center;">
        {#if error.message}{error.message}{:else}Something went wrong...{/if}
      </label>
    {/if}
    <Button icon={buttonIcon} {disabled} {pending} {success} {error}>
      {buttonText}
    </Button>
    <label style="text-align:center;" class="info">{afterButtonText}</label>
  </div>
</form>
