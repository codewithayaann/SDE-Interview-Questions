## SDE-Interview-Questions

### Reference YouTube Videos: [Watch Here](https://www.youtube.com/watch?v=RFF16z8yyFQ&list=PLe5vjs4_PbIIXnqoanq2O8DG-bR0GlOCR)

### Q1 : You have an array of async functions (or promises). You need to execute them in batches where each batch runs in parallel but batches themselves are processed in series

### Solution:

```javascript
const createPromise = (time) => {
    return new Promise((res) => {
        setTimeout(async () => {
            const url =
                `https://jsonplaceholder.typicode.com/todos/${id}`;
            await fetch(url)
            res(time)
        }, time)
    })
}

const p1 = createPromise(100)
const p2 = createPromise(200)
const p3 = createPromise(300)
const p4 = createPromise(400)
const p5 = createPromise(500)
const p6 = createPromise(600)

const promises = [
    () => p1.then(console.log),
    () => p2.then(console.log),
    () => p3.then(console.log),
    () => p4.then(console.log),
    () => p5.then(console.log),
    () => p6.then(console.log)
]

const promisesBatch = async (promises, batchCount) => {
    for (let i = 0; i < promises.length; i += batchCount) {
        const batch = promises.slice(i, i + batchCount)
        // [p1, p2] [p3, p4] [p5, p6]
        await Promise.all(batch.map(fn => fn()))
        await createPromise(2000)
    }
}

promisesBatch(promises, 2)
```


##
### Q2. Implement a function memoize(fn) that caches the result of function calls and returns cached results when the same inputs occur.

### Solution: 

```javascript
const delay = (time) =>
    new Promise((res) => setTimeout(res, time))

async function getData(id = 1) {
    const url =
        `https://jsonplaceholder.typicode.com/todos/${id}`;

    const response = await fetch(url);
    const json = await response.json()
    await delay(4000)
    return json.title;
}

const cacheFunction = (fn) => {
    const map = new Map()
    return async function (...args) {
        const now = performance.now()
        const key = args.toString()
        if (map.has(key)) {
            const response = map.get(key)
            const later = performance.now()
            return `cache res: ${await response} time: ${later - now} `;
        }
        const response = fn(...args)
        map.set(key, response);
        const later = performance.now()
        return `res: ${await response} time: ${later - now} `;
    }
}

const memoizeGetData = cacheFunction(getData);

memoizeGetData(100).then(console.log);
memoizeGetData(100).then(console.log);
memoizeGetData(100).then(console.log);
memoizeGetData(100).then(console.log);
memoizeGetData(100).then(console.log);

```
