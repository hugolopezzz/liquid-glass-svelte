<script lang="ts">
	import toPx from 'to-px';
	import type { Snippet } from 'svelte';

	type CSSAbsoluteLength =
		| `${number}px`
		| `${number}pt`
		| `${number}pc`
		| `${number}in`
		| `${number}cm`
		| `${number}mm`
		| `${number}Q`;

	type CSSRelativeLength =
		| `${number}em`
		| `${number}ex`
		| `${number}ch`
		| `${number}ic`
		| `${number}rem`
		| `${number}lh`
		| `${number}rlh`
		| `${number}vw`
		| `${number}vh`
		| `${number}vi`
		| `${number}vb`
		| `${number}vmin`
		| `${number}vmax`
		| `${number}%`;

	type CSSLength = CSSAbsoluteLength | CSSRelativeLength | '0' | `calc(${string})`;

	interface LiquidGlassOptions {
		mainBackgroundColor: string;
		mainBlur: CSSLength;
		edgeBlur: CSSLength;
		edgeBackgroundColor: string;
		edgeWidth: CSSLength;
		edgeGradientWidth: CSSLength;
		nonEdgeGradientWidth: CSSLength;
		sheenBlur: CSSLength;
		sheenBackgroundColor: string;
		sheenWidth: CSSLength;
	}

	interface LiquidGlassProps {
		options?: Partial<LiquidGlassOptions>;
		children: Snippet;
		class?: string;
		style?: string;
		[key: string]: unknown;
	}

	const defaultOptions: LiquidGlassOptions = {
		mainBackgroundColor: 'hsla(0, 0%, 75%, 0.1)',
		mainBlur: '1vw',
		edgeBlur: '0.5vw',
		edgeBackgroundColor: 'hsla(0, 0%, 100%, 0.1)',
		edgeWidth: '1vw',
		edgeGradientWidth: '1vw',
		nonEdgeGradientWidth: '1vw',
		sheenBlur: '3vw',
		sheenBackgroundColor: 'hsla(0, 0%, 100%, 0.2)',
		sheenWidth: '0.2vw'
	};

	let { children, options: rawOptions = {}, ...pureRest }: LiquidGlassProps = $props();

	let { class: className, style, ...rest } = pureRest;

	const options: LiquidGlassOptions = { ...defaultOptions, ...rawOptions };

	try {
		const mainBlurPx = toPx(options.mainBlur) ?? 0;
		const edgeBlurPx = toPx(options.edgeBlur) ?? 0;

		if (mainBlurPx > edgeBlurPx) {
			options.nonEdgeGradientWidth = options.edgeGradientWidth;
			options.edgeGradientWidth = '0.0001px';
		} else {
			options.nonEdgeGradientWidth = '0.0001px';
		}
	} catch (error) {
		console.error('Invalid CSS value for blur size:', error);
	}

	const cssVars = `--main-background-color:${options.mainBackgroundColor};
--main-blur:${options.mainBlur};
--edge-blur:${options.edgeBlur};
--edge-background-color:${options.edgeBackgroundColor};
--edge-width:${options.edgeWidth};
--edge-gradient-width:${options.edgeGradientWidth};
--non-edge-gradient-width:${options.nonEdgeGradientWidth};
--sheen-blur:${options.sheenBlur};
--sheen-background-color:${options.sheenBackgroundColor};
--sheen-width:${options.sheenWidth};`;
</script>

<div class={`glass ${className || ''}`} style={`${cssVars} ${style || ''}`} {...rest}>
	<div class="glass-non-edge">
		<div class="glass-edge">
			<div class="glass-sheen">
				{@render children?.()}
			</div>
		</div>
	</div>
</div>

<style>
	.glass {
		position: relative;
		overflow: hidden;

		z-index: 1;

		inset: 0;
		border-radius: inherit;

		background-color: var(--main-background-color);

		:global(.glass-sheen > *) {
			position: relative;
			z-index: 1;
		}

		.glass-sheen::before {
			content: '';
			position: absolute;
			inset: 0;
			border-radius: inherit;
			backdrop-filter: blur(var(--sheen-blur));
			background-color: var(--sheen-background-color);
			pointer-events: none;
			z-index: 0;

			-webkit-mask-image:
				linear-gradient(0deg, #000, transparent var(--sheen-width)),
				linear-gradient(180deg, #000, transparent var(--sheen-width)),
				linear-gradient(90deg, #000, transparent var(--sheen-width)),
				linear-gradient(270deg, #000, transparent var(--sheen-width));
			mask-image:
				linear-gradient(0deg, #000, transparent var(--sheen-width)),
				linear-gradient(180deg, #000, transparent var(--sheen-width)),
				linear-gradient(90deg, #000, transparent var(--sheen-width)),
				linear-gradient(270deg, #000, transparent var(--sheen-width));

			mask-composite: add;
			mask-type: luminance;

			pointer-events: none;
		}

		.glass-non-edge::before {
			content: '';
			position: absolute;
			inset: 0;
			border-radius: inherit;
			backdrop-filter: blur(var(--main-blur));

			pointer-events: none;
			z-index: 0;

			--gradient:
				transparent var(--edge-width), #000 calc(var(--edge-width) + var(--non-edge-gradient-width));

			-webkit-mask-image:
				linear-gradient(0deg, var(--gradient)), linear-gradient(180deg, var(--gradient)),
				linear-gradient(90deg, var(--gradient)), linear-gradient(270deg, var(--gradient));

			mask-image:
				linear-gradient(0deg, var(--gradient)), linear-gradient(180deg, var(--gradient)),
				linear-gradient(90deg, var(--gradient)), linear-gradient(270deg, var(--gradient));

			mask-composite: intersect;

			pointer-events: none;
		}

		.glass-edge:before {
			content: '';
			position: absolute;
			inset: 0;
			border-radius: inherit;
			pointer-events: none;
			z-index: 0;

			background-color: var(--edge-background-color);
			backdrop-filter: blur(var(--edge-blur));

			--gradient:
				#000 var(--edge-width), transparent calc(var(--edge-width) + var(--edge-gradient-width));

			-webkit-mask-image:
				linear-gradient(0deg, var(--gradient)), linear-gradient(180deg, var(--gradient)),
				linear-gradient(90deg, var(--gradient)), linear-gradient(270deg, var(--gradient));
			mask-image:
				linear-gradient(0deg, var(--gradient)), linear-gradient(180deg, var(--gradient)),
				linear-gradient(90deg, var(--gradient)), linear-gradient(270deg, var(--gradient));

			mask-composite: add;
			mask-type: luminance;

			pointer-events: none;
		}
	}
</style>
