# liquid-glass-svelte

![Banner](./banner.png)

`liquid-glass-svelte` provides a Svelte component that reproduces Apple's Liquid Glass effect.

## Installation

```bash
npm i liquid-glass-svelte
```

## How to Use

```svelte
<script lang="ts">
    import { LiquidGlass } from "liquid-glass-svelte";
</script>

<LiquidGlass>
    <h1>Content goes here</h1>
</LiquidGlass>
```

## Customization

Pass a partial `options` object to tune the effect. Any field you omit falls back to its default value.

```svelte
<LiquidGlass
    options={{
        mainBackgroundColor: 'hsla(0, 0%, 75%, 0.1)',
        mainBlur: '1vw',
        edgeBlur: '0.5vw',
        edgeBackgroundColor: 'hsla(0, 0%, 100%, 0.1)',
        edgeWidth: '1vw',
        edgeGradientWidth: '1vw',
        sheenBlur: '3vw',
        sheenBackgroundColor: 'hsla(0, 0%, 100%, 0.2)',
        sheenWidth: '0.2vw'
    }}
>
    <p>Your content</p>
</LiquidGlass>
```

![Explanation](./explanation.png)

---

Since the component forwards additional attributes (`...rest`), you can attach classes or other props:

```svelte
<LiquidGlass class="hero">
    <!-- content -->
</LiquidGlass>
```
