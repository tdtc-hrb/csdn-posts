---
title: "update redux"
description: "分解redux，以理解他"
date: 2020-04-10T09:26:39+08:00
---

# 1. initial state
db.json
```js
const initialState = {
  "recipes": [
    {
      "name": "Omelette"
    },
    {
      "name": "Waffle"
    }
  ],
  "ingredients": [
    {
      "name": "Eggs",
      "quantity": 2,
      "measure": "large",
      "recipe": "Omelette"
    },
    {
      "name": "Eggs",
      "quantity": 1,
      "measure": "large",
      "recipe": "Waffle"
    },
    {
      "name": "Milk",
      "quantity": 1,
      "measure": "cups",
      "recipe": "Waffle"
    },
    {
      "name": "Sugar",
      "quantity": 2,
      "measure": "tbsp",
      "recipe": "Waffle"
    }
  ]
}
```

# 2. reducer
```js
const reducer = (state, action) => {
    switch (action.type) {
        case 'ADD_RECIPE':
            return Object.assign(
                {}, state, {
                    recipes: state.recipes.concat({name : action.name})
            });

    return state;
}
```

# 3. action

## 1) declare

```js
const addRecipe_action = (name) => ({
    type:'ADD_RECIPE',
    name
})
```

## 2) dispatch
```js
store.dispatch(addRecipe_action('Xiaobin'));
```

# 完整代码
```js
import { createStore } from 'redux';

const reducer = (state, action) => {
    switch (action.type) {
        case 'ADD_RECIPE':
            return Object.assign(
                {}, state, {
                    recipes: state.recipes.concat({name : action.name})
            });

        case 'ADD_INGREDIENT':
            return Object.assign(
                {}, state, {
                    ingredients: state.ingredients.concat({
                        name : action.name,
                        quantity: action.quantity,
                        measure: action.measure,
                        recipe: action.recipe
                        })
            });

    }
    return state;
}

// https://s3.amazonaws.com/500tech-shared/db.json
const initialState = {
  "recipes": [
    {
      "name": "Omelette"
    },
    {
      "name": "Waffle"
    }
  ],
  "ingredients": [
    {
      "name": "Eggs",
      "quantity": 2,
      "measure": "large",
      "recipe": "Omelette"
    },
    {
      "name": "Eggs",
      "quantity": 1,
      "measure": "large",
      "recipe": "Waffle"
    },
    {
      "name": "Milk",
      "quantity": 1,
      "measure": "cups",
      "recipe": "Waffle"
    },
    {
      "name": "Sugar",
      "quantity": 2,
      "measure": "tbsp",
      "recipe": "Waffle"
    }
  ]
}

const addRecipe_action = (name) => ({
    type:'ADD_RECIPE',
    name
})

const store = createStore(reducer, initialState);

console.log(store.getState());
//store.subscribe(()=>(console.log("store change")));

store.dispatch(addRecipe_action('Xiaobin'));

store.dispatch({type:'ADD_INGREDIENT', name: 'Eggs', quantity: 2, measure: 'large', recipe: 'xiaobin'});

console.log(store.getState());
```
