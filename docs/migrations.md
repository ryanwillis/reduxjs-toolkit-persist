# Redux Persist Migration Example

### Example with createMigrate
```js
import { createMigrate, persistReducer, persistStore } from 'reduxjs-toolkit-persist'
import storage from 'reduxjs-toolkit-persist/es/storage'

const migrations = {
  0: (state) => {
    // migration clear out device state
    return {
      ...state,
      device: undefined   
    }
  },
  1: (state) => {
    // migration to keep only device state
    return {
      device: state.device
    }
  }
}

const persistConfig = {
  key: 'primary',
  version: 1,
  storage,
  migrate: createMigrate(migrations, { debug: false }),
}

const finalReducer = persistReducer(persistConfig, reducer)

export default function configureStore() {
  let store = createStore(finalReducer)
  let persistor = persistStore(store)
  return { store, persistor }
}
```

### Alternative
The migrate method can be any function with which returns a promise of new state. 
```js
const persistConfig = {
  key: 'primary',
  version: 1,
  storage,
  migrate: (state) => {
    console.log('Migration Running!')
    return Promise.resolve(state)
  }
}
