# physical springs

An alternative to easing functions, for animations, is spring-based interpolations.

## comparison

**Easings**

* Takes a _duration_ and an _easing function_
* Determines the position of a value between `a` & `b`, given the duration (from `0`)

**Springs**

* Takes a configuration of spring properties (`mass`, `tension`, `friction`, `velocity`)
* Determines the next position of a value as well as the spring's velocity, based on the current position & current velocity
* The "stopping" accuracy of the spring (aka it's amplitude) can be tuned with a 5th spring property; `precision`

## quick maf

So imagining that have a value (`A`), and you want to figure out what it will be _next_; you're intend to do this every frame, inside of a `requestAnimationFrame` or something.

To get this insight, you'll need to determine a couple things, then use them together:

1. The spring force (using [Hooke's Law](https://en.wikipedia.org/wiki/Hooke's_law))
2. The friction force (using [Newtons second law of motion](https://en.wikipedia.org/wiki/Friction#Static_friction))
3. The acceleration of the value

Let's break this all down in code:

```js
// Config for a standard nice feeling spring
const config = {
  mass: 1,
  tension: 170,
  friction: 26,
}

// A function that does all the things mentioned above, and returns the next position & velocity
function interpolateWithSpring (currentPosition, toPosition, currentVelocity, config) {
  const tensionForce = -config.tension * (currentPosition - toPosition)
  const frictionForce = -config.friction * currentVelocity
  const acceleration = (tensionForce + frictionForce) / config.mass
  const nextVelocity = currentVelocity + acceleration
  const nextPosition = currentPosition + nextVelocity

  return [ nextPosition, nextVelocity ]
}

// The interpolation values (mutative)
const values = {
  fromPosition: 0,
  toPosition: 1,
  velocity: 0,
}
var [ nextPosition, nextVelocity ] = interpolateWithSpring(
  values.fromPosition,
  values.toPosition,
  values.velocity,
  config
)

// Now, when it comes time to put this in a RAF, it's a key requirement to sample
// the interpolation for each millsecond passed...
let lastTs = null
loop()
function loop (ts) {
  if (lastTs == null) lastTs = ts
  const samples = Math.floor(ts - lastTs) // Here we determine how many samples to run
  
  for (let i = 0; i < samples; i++) { // Then we run the samples
    const [ nextPosition, nextVelocity ] = interpolateWithSpring(
      values.fromPosition,
      values.toPosition,
      values.velocity,
      config
    )

    // We carry each sample, by persisting it somewhere
    values.fromPosition = nextPosition
    values.velocity = nextVelocity
  }


  // It's good to test the precision of the velocity to stop the spring early
  const isVelocityBelowPrecision = Math.abs(values.velocity) <= config.precision;
  const isDisplacementBelowPrecision = config.tension !== 0
    ? Math.abs(values.fromPosition - values.toPosition) <= config.precision
    : true;
  if (isVelocityBelowPrecision && isDisplacementBelowPrecision) return;
  else requestAnimationFrame(loop) // Finally, rerun the RAF
}
```
