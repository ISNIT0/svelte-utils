<script>
  export let spritesheetUrl;
  export let spriteWidth;
  export let spriteHeight;
  export let width = spriteWidth;
  export let height = spriteHeight;
  export let index = 0;
  export let rowLength = Infinity;
  $: bgWidthScale = Number(width) / Number(spriteWidth);
  $: bgHeightScale = Number(height) / Number(spriteHeight);
  $: pos = getSpritePos(index);

  export let getSpritePos = function(spriteIndex) {
    const a = {
      x: ((spriteIndex % Number(rowLength)) * Number(spriteWidth) * bgWidthScale) + "px",
      y: (Math.floor(spriteIndex / Number(rowLength)) * Number(spriteHeight) * bgHeightScale) + "px"
    };
    return a;
  };
  export let onLoaded = null;
  let loaded;

  let spritesheetWidth = 0;
  let spritesheetHeight = 0;

  const img = new Image();
  img.onload = function(ev) {
    const { width, height } = img;
    spritesheetWidth = width;
    spritesheetHeight = height;
    loaded = true;
    onLoaded && onLoaded();
  };
  img.src = spritesheetUrl;
</script>

{#if loaded}
  <div
    style={`width:${width}px;height:${height}px;background-position:-${pos.x} -${pos.y};background-image:url(${spritesheetUrl}); background-size:${spritesheetWidth * bgWidthScale}px ${spritesheetHeight * bgHeightScale}px;`} />
{/if}
