# react

## https://www.youtube.com/watch?v=mDjFfabR294

## [React Tutorial and Projects Course](https://www.udemy.com/course/react-tutorial-and-projects-course/)

### NPM Basic

npm init
creates package.json file, list dependencies

npm install packagename -- save
install locallly and add to package.json

npm install pacagename -g
install package globally sudo

npm install packagename --save-dev
use it only in development

### 15 update

.env fastrefresh=false

### 16 component

Uppercase fisrt letter means function is component.

Using self closing tag like <something/>

### 19 JSX Rules

return single element

div / section / arcticle or Fragment

use camelCase for html attribute

className instead of class

close every element

formatting

### 20 nested components and tools

implicit return

### 28 simple list

list function map()

use as this format:

names.map((name)=>{
do something
})

### 33 free hosting

<https://www.netlify.com/>

npm run build to get a production build version

### 36 install starter

npm install

### 38 useState Simple Use Case

only change value cannot rerender

### 39 useState

useState: return a array of value and function of modeify it.

### 40 general rules of hooks

the components where we invoke those hooks must start with upppercase.

the hook need be invoked in function body.

### 41 Array Example

=> is only way to binding function with args and don't tragger it when render

### 42 useState Object Example

spread operator(...person) to protect the value you don't want to change

### 43 multiple state values

console.log(person); 每次触发都会刷新的问题。

```
const UseStateObject = () => {
const [person, setPerson] = useState({
name: 'peter',
age: 24,
message: 'random message'
});
console.log(person);

    function changMessage() {
        setPerson({...person, message: 'after editing'})
    }

    return <>
        <h3>{person.name}</h3>
        <h3>{person.age}</h3>
        <h3>{person.message}</h3>
        <button className={"btn"} onClick={changMessage}>
            change message
        </button>
    </>;
};

export default UseStateObject;
```

### 45 functional update form

add 2 seconds delay for the button.

setTimeout()

multi click but change only once, because setValue() is synchronous.

setValue((preVState) => {return preVState + 1;}) is reading the correct value.

useState 2nd function, if is passing a value, will set the value.
If is passing a function, will run this function by passing the value as input parameters,
and return value for the value in useState seted.

### 47 useEffect Basics

useEffect runs after every re-render.

index.js <react.StrictMode> make the page render twice.

### 49 useEffect Dependency List

if second parameter is a blank of array, means it only run on the initial render.

if fill some parameter, it will be tragger when the para changes.

useEffect can add more than one.

### 50 useEffect cleanup Function

return a function in the useEffect function

cleanup function will run before next time use effect run.

### 51 useEffect Fetch Data

useEffect shouldn't be a async function, since the cleanup function

await async:

setUsers will trigger rerender, if setUser put in useEffect, will cause infinite loop.

### 52 multiple return basics

conditional rendering

### 54 Short-circuit Evaluation

'' is same with false in ||

Can be use like {text || 'default is text is empty'}

and {text && <h1>Show when text is not empty</h1>}

### 55 Ternary Operator

### 58 Form  Basics

submit the form and then refresh the page.

block it by e.preventDefault();

### 59 Controlled inputs

<input value={name} onChange="">

### 63 useRef

useRef is like useEffect, but preserves values between components

doesnot trigger rerender

use to target DOM nodes/elements, need ref= attribute

### 65 useReducer

functional programming. for a certain object.

### 69 prop drilling

passing some prop through many levels

### 70 Context API / useContext

solve the prop drilling problems

### 72 useFetch

custom hook need start with use.

### 78 react.memo

Block re-rendering when the value has not changed.

### 79 useCallback

block re-rendering when react.memo depends function trigger

### 80 useMemo

recalculate when the data change and rerender.

### 113 slider Autoplay

setInterval, to set a act after sometime

clearInterval, to cancel the act

### 114 slider Alternative

onclick={ function}

function= setValue((oldvalue)=>(newvalue))

### 137 getboundiingclientrect

https://www.youtube.com/watch?v=v8YENdbDv1w

### 138 navbar useRef

@media screen and (min-width: 800px)

links-container

height: auto

for auto weight change

### 192 Cocktails single cocktail page

usecallback: block re-create of function

### 220 stock photos env var

process.env for setting variable

### 222 stock photos scroll event

how to apply the function scroll down to button and refresh.

### 232 Dark Mode Local Storage

localStorage.getItem('theme')

document.documentElement.className = theme;

localStorage.setItem('theme', theme);

### 263 quiz handlechange

3:21. [variable]:value. It can use variable: (name) as name of variable: ([name] as a variable name)

### 298 Promise.allSettled()

send multi request first, wait all back and show.

### 334 Single Product Loading, Error

8:50 useHistory from react router

想改的地方：最小价格，最大价格。

搜索功能
