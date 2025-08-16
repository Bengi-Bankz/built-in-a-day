<script lang="ts" module>
	import { Rectangle, type RectangleProps } from 'pixi-svelte';

	export type Props = RectangleProps;
</script>

<script lang="ts">
	const props: Props = $props();
</script>

<Rectangle
	borderRadius={50}
	backgroundColor={0xadd8e6}
	backgroundAlpha={0.75}
	borderColor={0x000000}
	borderWidth={4}
	{...props}
/>