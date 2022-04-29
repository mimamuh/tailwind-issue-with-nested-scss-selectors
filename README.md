### Bug in tailwind when used with scss nested rules

##### How to start the project

1. Clone this project
2. Run `npm install`
3. Run `npm run dev` to start the webpack dev-server

##### What is the bug?

In our `./src/main.scss` we have a utility rule added to the `@components` layer using _Tailwind_ with _SASS_:

```scss
@layer components {
  .base {
    /* NOTE: These grouped and nested rules causes the 
    bug when the `base` utility is combined with a breakpoint 
    like `md:base` */
    .foo,
    .bar {
      @apply bg-rose-600;
    }
  }
}
```

As `.foo, .bar` is grouped, the generated _CSS_ of _Tailwind_ is this:

```css
@media (min-width: 768px) {
  .md\:base .foo,
  .base .bar {
    --tw-bg-opacity: 1;
    background-color: rgb(225 29 72 / var(--tw-bg-opacity));
  }
}
```

Instead of this:

```css
@media (min-width: 768px) {
  .md\:base .foo,
  .md\:base .bar {
    --tw-bg-opacity: 1;
    background-color: rgb(225 29 72 / var(--tw-bg-opacity));
  }
}
```

So basically: The breakpoint is not applied to the second nested utility (md:base -> .bar).
