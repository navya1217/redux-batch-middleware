> Enhance your Redux store to support batched actions

## Installation

```
$> npm install --save @manaflair/redux-batch
```

## Usage

```js
import { reduxBatch }  from '@manaflair/redux-batch';
import { createStore } from 'redux';

let store = createStore(reducer, reduxBatch);

store.dispatch([

  // Store listeners will only be called once all of these actions
  // have been dispatched to the store.

  { type: `INCREMENT_COUNTER` },
  { type: `INCREMENT_COUNTER` },

]);
```

## Usage w/ extra middlewares

```js
import { reduxBatch }                            from '@manaflair/redux-batch';
import { put, takeEvery }                        from 'redux-saga/effects';
import createSagaMiddleware                      from 'redux-saga';
import { applyMiddleware, compose, createStore } from 'redux';

let sagaMiddleware = createSagaMiddleware();
let store = createStore(reducer, compose(reduxBatch, applyMiddleware(sagaMiddleware), reduxBatch));

sagaMiddleware.run(function* () {
  yield takeEvery(`*`, function* (action) {

    // Duplicate any event dispatched, and once again, store
    // listeners will only be fired after both actions have
    // been resolved/

    yield put([ action, action ]);

  });
});
```
