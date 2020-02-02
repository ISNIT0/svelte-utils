<script>
  export let defaultRoute = null;
  export let stack = defaultRoute ? [defaultRoute] : null;
  export let onReady;

  $: currentRoute = stack[stack.length - 1];

  function pushRoute(route) {
    if (!route.component) {
      throw new Error(`Invalid route, property "component" is required`);
    }
    stack[stack.length] = route;
  }
  function popRoute() {
    if (stack.length > 1) {
      stack.pop();
      stack = stack;
    } else {
      throw new Error(`Cannot pop last item from stack`);
    }
  }
  function replaceRoute(route) {
    stack[stack.length - 1] = route;
  }

  const router = {
    pushRoute,
    popRoute,
    replaceRoute,
    stack
  };

  onReady && onReady(router);
</script>

<svelte:component
  this={currentRoute.component}
  {...currentRoute.props || {}}
  {router} />
