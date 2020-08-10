# curves

<h2>figure eight / infinity<sub></h2>


<p align="left"><img src="https://i.stack.imgur.com/nOPMx.gif" /></p>

[Lemniscate of Bernoulli](http://en.wikipedia.org/wiki/Lemniscate_of_Bernoulli) \[ [source](https://gamedev.stackexchange.com/questions/43691/how-can-i-move-an-object-in-an-infinity-or-figure-8-trajectory) \]

```js
function eightCurve(t = 0) {
  const scale = 2 / (3 - Math.cos(2*t));
  const x = scale * Math.cos(t);
  const y = scale * Math.sin(2*t) / 2;

  return [x, y]
}
```
