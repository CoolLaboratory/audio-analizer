<script>
	import {onMount, tick} from 'svelte'
	export let audioCtx
	export let source

	onMount(async () => {
		await tick()
		
		var gainNode = audioCtx.createGain();
		source.connect(gainNode);
		gainNode.connect(audioCtx.destination);
	})
</script>

