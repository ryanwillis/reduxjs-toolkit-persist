## Hot Module Replacement

Hot Module Replacement (HMR) is a wonderful feature that is really useful in development environment. This allows you to update the code of your application without reloading the app and resetting the redux state.

The key modification for using HMR with reduxjs-toolkit-persist, is the incoming hot reducer needs to be re-persisted via `persistReducer`.

**configureStore.js**
```js
import { persistReducer } from 'reduxjs-toolkit-persist'
import rootReducer from './path/to/reducer'

export default () => {
  // create store and persistor per normal...

  if (module.hot) {
    module.hot.accept('./path/to/reducer', () => {
      // This fetch the new state of the above reducers.
      const nextRootReducer = require('./path/to/reducer').default
      store.replaceReducer(
        persistReducer(persistConfig, nextRootReducer)
      )
    })
  }

  return { store, persistor }
}
```
