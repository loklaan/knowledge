# effect

Learnings / notes on using the [Effect](https://effect.website/) library & ecosystem.

## running effects

Inheriting `FiberRefs` / `Context` between `Runtime`

- **Explanation**: A fresh `Runtime` does not inherit FiberRefs or Context, which means any already run Config or Layers are not propogated down into the fresh Runtime.  
  Sometimes this isn't intentional, but can happen when integrating 3rd-parties between effects.
- **Fix**: Store the FiberRefs and Context from the parent fiber, and in the/a child Effect, just use `Effect.inheritFiberRefs(parentFiberRefs)` and `Effect.provide(parentContext)`.


`RunMain` from `@effect/platform/Runtime`

> `Effect<void, unknown, unknown>` is not assignable to parameter of type `Effect<void, unknown, never>`
- **Explanation**: The `RunMain` expects there to be no services left to provide in an rffect; the `unknown` in the 3rd channel of the effect does not satisfy this expectation, as it's essentially "this effect requires some unknown service(s)".
- **Fix**: Cast the effect so the 3rd channel is `never`.
   1. `Effect.context()` defaults to `unknown` for the type argument; provide it an explicit type like `Effect.context<MyServices>` instead.
   2. Just use:  
      ```ts
      effect as Effect<
        Effect.Success<typeof effect>,
        Effect.Error<typeof effect>,
        never // <- the
      >
      ```


## layers (dependency injection)

- A `Layer<Effect<A, E, R>, O>` can be flatted with `Layer.unwrapEffect` to become a `Layer<A, O>`.
