# Svelte Utils
> Handy Svelte components for the whole family

TL;DR:
- MIT Licensed Simple Svelte Components
- Live demo of components: https://svelte-utils.netlify.com/
- Example code: [./src/App.svelte](./src/App.svelte)

Included:
- Button
- Form
- Spinner
- Sprite
- StackRouter

To override all Svelte Utils default colors, set the following with CSS:
```css
.svelte-utils-theme-background {
    background-color: red;
}
.svelte-utils-theme-color {
    color: white;
}
```

## Button
```html
<Button
    onClick={buttonHandler}
    icon="/lock.svg"
    backgroundColor="red">
    Confirm
</Button>
```
Common use parameters
- onClick: `(event) => Promise`
- icon?: `path to image`
- backgroundColor?: `hex, rgba, etc.`

Control state with
- disabled?: `boolean`
- pending?: `boolean`
- success?: `boolean`
- error?: `boolean`

## Form
```jsx
<Form
    onDone={async function() {}}
    buttonIcon="/lock.svg"
    handleSubmit={async function(data, ev) {
        console.log(data); // { name: 'Joe Blogs' }
        return postJSON(`/api/save`, data);
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
```
Automatic styling will be applied for:
- `Form > .title`
- `Form > .description`
- `Form > label`
- `Form > div.input *`

Common use parameters
- handleSubmit: `(data, ev) => Promise`
- onDone: `(handleSubmitRes) => Promise`
- buttonText?: `string`
- `<slot />`

> The first argument for `handleSubmit` will be a Key-Value-Store (object) with keys based on the input `name` attribute.
> 
> For `input[type=checkbox]` elements, the value is an array of checked values

> Rejected promises should contain a `message` property which will be shown on the form

Other parameters
- buttonIcon?: `string`
- afterButtonText?: `string`
- disabled?: `boolean`


## Spinner
```html
<Spinner black={true} size="50px" />
```
Parameters
- black?: `boolean` (default is `false` - will display a white spinner)
- size?: `px value`

## Sprite
```html
<Sprite
    spritesheetUrl="https://i.stack.imgur.com/wFCJb.png"
    spriteWidth="32"
    spriteHeight="32"
    width="50"
    height="50"
    rowLength="6"
    index={spriteIndex} />
```
Parameters
- spritesheetUrl: `string`
- index?: `number` (sprite index)
- spriteWidth: `number` (pixel width of sprite on spritesheet)
- spriteHeight: `number` (pixel height of sprite on spritesheet)
- width?: `number` (width to scale sprite to)
- height?: `number` (height to scale sprite to)
- rowLength?: `number` (number of sprites per row on spritesheet)

Advanced Parameters
- getSpritePos?: `(spriteIndex) => { x: string, y: string }`

## StackRouter
```html
<StackRouter
    onReady={routerReady}
    defaultRoute={routes[0]}
    bind:stack={routerStack} />
```
Parameters
- onReady: `(router: { pushRoute, popRoute, replaceRoute }) => void`
- defaultRoute: `Route` (`{ component: SvelteComponent, props: { name: 'Joe' }}`)
- stack?: `Route[]`


# Contributing
Make a PR, I won't bite 💖

# Also see
- **Svelte London Meetup** - https://meetup.com/svelte
- News Hackathons - https://HackThePress.org
- Political Hackathons - https://ToryTechs.uk
- Joe Reeve - https://SimmsReeve.com

# License - MIT
[./LICENSE](./LICENSE)