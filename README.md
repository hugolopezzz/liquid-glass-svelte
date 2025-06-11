# liquid-glass-svelte

![Banner](./banner.png)

This svelte component is inspired by Apple's Liquid Glass UI.

This package will make it able to implement it.

## Installation

```bash
npm i liquid-glass-svelte
```

## How to Use
```svelte
<script lang="ts">
    import { LiquidGlass } from "liquid-glass";
</script>

<LiquidGlass>
    <h1>Content goes here</h1>
</LiquidGlass>
```

## Customization

```svelte
<LiquidGlass
    options={{
        mainBlur,
        mainBackgroundColor,
        edgeBlur,
        edgeBackgroundColor,
        edgeWidth,
        edgeGradientWidth,
        sheenBlur,
        sheenBackgroundColor,
        sheenWidth
    }}
></LiquidGlass>
```