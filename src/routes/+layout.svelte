<script lang="ts">
	import '../app.css';
	let { children } = $props();
</script>

<div class="p-10 flex flex-col gap-20 text-white">

    {@render children()}
</div>
