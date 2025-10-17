---
title: "rootredux"
description: "分解redux，以理解他"
date: 2020-04-10T08:26:39+08:00
---

# 1. split
```js
const reducer = (state, action) => {
    switch (action.type) {
        case 'ADD_RECIPE':
            return Object.assign(
                {}, state, {
                    recipes: state.recipes.concat({name : action.name})
            });

        case 'ADD_INGREDIENT':
            const myIngredients = {
                        name : action.name,
                        quantity: action.quantity,
                        measure: action.measure,
                        recipe: action.recipe
                        };
            return Object.assign(
                {}, state, {
                    ingredients: state.ingredients.concat(myIngredients)
            });

    }
    return state;
}
```

## recipeReducer
```js
export const recipeReducer = (stat, action) => {
    switch (action.type) {
        case 'ADD_RECIPE':
            return
Object.assign(
                {}, state, {
                    ingredients: state.ingredients.concat(myIngredients)
            });
} return recipes; }
```

## ingredientReducer
```js
const ingredientReducer = (state, action) => {
    switch (action.type) {
        case 'ADD_INGREDIENT':
            const myIngredients = {
                        name : action.name,
                        quantity: action.quantity,
                        measure: action.measure,
                        recipe: action.recipe
                        };
            return Object.assign(
                {}, state, {
                    ingredients: state.ingredients.concat(myIngredients)
            });

    }
    return state;
}
```
