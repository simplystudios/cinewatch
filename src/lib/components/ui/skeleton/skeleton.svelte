<script>
	import { cn } from "$lib/utils.js.ts";
	let className = undefined;
	export { className as class };
</script>

<div class={cn("bg-muted animate-pulse rounded-md", className)} {...$$restProps}></div>
