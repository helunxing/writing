# js study

##### <https://www.bilibili.com/video/BV1WP4y187Tu>

async programming: don't wait, keep on exec[word trans.md](word%20trans.md)ute following code

callback: notice when the async finish. setTimeout().

callback hell: multi async operate nesting, like loop nesting.

promise: `.then()` to apply callback function. `.catch()` to catch error in any callback function. `.finally()`

async function: return a promise.

await key word: waiting a async function, then return the result.

`await Promise.all()` to wait multi promise

`.foreach` and `.map` will not wait, even inside have `await`.

`for` will wait all async function.

`for await` will wait and concurrency operate them.
