How To Get Duration (Years Months Days Hours Minutes Seconds) Between Two Dates In JavaScript

```js
// output: x years, x months, x days, x hrs, x min, x s
export default function dateDiff(firstDate, secondDate) {
    firstDate = new Date(firstDate);
    secondDate = new Date(secondDate);
    let duration = Math.floor((secondDate - firstDate)/1000);
    let results = {};
    let seconds = {
        years: 31536000,
        months: 2592000,
        // weeks: 604800,
        days: 86400,
        hrs: 3600,
        min: 60,
        s: 1
    };

    Object.keys(seconds).forEach(function(key){
        results[key] = Math.floor(duration / seconds[key]);
        duration -= results[key] * seconds[key];
    });

    return Object.entries(results)
    .filter(([key, value]) => value > 0)
    .map(([key, value]) => value + ' ' + key)
    .join(', ');
}
```
