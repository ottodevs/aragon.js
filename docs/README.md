![](/docs/assets/brand/aragonjs.png)

A JavaScript implementation of aragonAPI, used to interact with aragonOS by handling transaction pathing, upgradeability and state of the contracts.

### Docs

- [App API](/docs/APP.md)
  - [React API](/packages/aragon-api-react/README.md)
- [Aragon client implementations (wrapper)](/docs/WRAPPER.md)
- [Providers](/docs/PROVIDERS.md)
- [Architecture of Aragon apps and their communication channels](/docs/ARCHITECTURE.md)

### Guides

- [Background Scripts](/docs/BACKGROUND_SCRIPTS.md)

## Quick Start for apps

```sh
npm i @aragon/api
```

```js
const Aragon = require('@aragon/api')

// Set up app
const app = new Aragon()

// Set the app identifier since multiple instances of this can be installed
// (e.g. for our token manager, we use the ticker of the token it manages)
app.identify('Employee counter')

// Listen to events and build app state
const state$ = app.store((state, event) => {
  // Initial state
  if (state === null) state = 0

  // Build state
  if (event.event === 'Decrement') {
    // Calculate the next state
    state--
    // Send notification
    app.notify('Counter decremented', `The counter was decremented to ${state}`)
  }
  if (event.event === 'Increment') {
    state++
    app.notify('Counter incremented', `The counter was incremented to ${state}`)
  }

  return state
})

// Log out the state
state$.subscribe(console.log)

// Send an intent to the wrapper
app
  .increment()
  .subscribe(
    txHash => console.log(`Success! Incremented in tx ${txHash}`),
    err => console.log(`Could not increment: ${err}`)
  )
```
