<script>
  import { fade } from "svelte/transition";
  import Spinner from "./Spinner.svelte";
  export let disabled;
  export let pending;
  export let success;
  export let error;
  export let onClick;
  export let backgroundColor = '#1fd186';
  export let icon = "";
</script>

<style>
  button {
    width: 100%;
    color: white;
    border: none;
    border-radius: 4px;
    padding: 10px;
    box-shadow: 0px 2px 8px rgba(0, 0, 0, 0.12);
    font-style: normal;
    font-weight: normal;
    font-size: 17px;
    line-height: 22px;

    transition: 0.3s;

    width: 100%;
    height: 56px;
    position: relative;
  }

  button:not(.success):not(.pending):not(:disabled) {
    cursor: pointer;
  }

  button.pending,
  button.success {
    opacity: 0.9;
    box-shadow: 0px 4px 12px rgba(0, 0, 0, 0.12);
  }

  button:not(:disabled):active {
    opacity: 0.9;
    box-shadow: 0px 4px 12px rgba(0, 0, 0, 0.12);
  }

  button.disabled {
    background: #dddddd !important;
  }

  .extra-wrapper {
    position: absolute;
    top: 0;
    right: 10px;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  .text-cont {
    position: absolute;
    width: 100%;
    height: 100%;
    left: 0;
    top: 0;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }
</style>

<button
  style={backgroundColor ? `background-color:${backgroundColor};` : ''}
  disabled={disabled || pending || success}
  class:disabled
  class:pending
  class:success
  class:error={!!error}
  on:click={async () => {
    if (onClick) {
      try {
        pending = true;
        await onClick();
        success = true;
        pending = false;
      } catch (err) {
        error = true;
        setTimeout(() => (error = false), 2000);
        pending = false;
        throw err;
      }
    }
  }}
  class="svelte-utils-theme-background">
  <span class="text-cont">
    <span class="text svelte-utils-theme-color">
      <slot />
    </span>
  </span>
  {#if success}
    <div class="extra-wrapper" transition:fade={{ duration: 100 }}>
      <span>✅</span>
    </div>
  {:else if pending}
    <div class="extra-wrapper" transition:fade={{ duration: 100 }}>
      <Spinner size="20px" />
    </div>
  {:else if error}
    <div class="extra-wrapper" transition:fade={{ duration: 100 }}>
      <span>❌</span>
    </div>
  {:else if icon}
    <div class="extra-wrapper" transition:fade={{ duration: 100 }}>
      <img src={icon} alt="" height="20px" />
    </div>
  {/if}
</button>
