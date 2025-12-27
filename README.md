## useTransition 

Imagine that a user is typing so fast and we have these heavy updates :

```js
const searchHandler = (e:{ target:{ value:searchValue }}) => {
    setSearchValue(searchValue) // ! Urgent update 
    setProductList(
        filter(searchValue) // ! Heavy process
    ) // ! Urgent update
}
```

updating a heavy array along with updating input value might block the re-renders and commit process and makes UI feels slow because React see both queued setStates as urgent.
The hook ``useTransaction()`` allows you to prioritize setStates : 

```js
const [pending,startTransition] = useTransition()
const searchHandler = (e:{ target:{ value:searchValue }}) => {
    setInputValue(searchValue)// ! Urgent update 
    startTransition(()=>{
        setProductList(filter(searchValue)) // ! Heavy process
    })
    
}
```

``startTransition`` marks a state update as **low-priority**.
Urgent updates (like typing in an input) are committed immediately to keep the UI responsive.
Transition updates may render in the background, but if a newer urgent update happens, React discards the previous transition and only commits the latest one when there is no urgent updates so the UI is still responsive even if we have heavy setStates.

## Memorizing
- Use React-Compiler to make automatic and easier Memorizing process so it dose not memorize unnecessary values and avoid memory lake which improves performance.

## Separating responsibilities 

- if anything dos not relate to React we can move it outside of a React Component like data fetching, complex state or any logical code.

## Server Components 

- There is no javascript for client, no re-render, no hydration just a pies of html sent to the client which is called stream.


