<script>
	import {onMount} from 'svelte'
	export let audioCtx
	export let src
	
	let source
	let audioEl
	
	onMount(() => {
		source = audioCtx.createMediaElementSource(audioEl)
// 		source.connect(audioCtx.destination)
	})
</script>


<audio {src} controls bind:this={audioEl} crossorigin="anonymous">
	<track kind="captions">
</audio>

<slot {source} />


<style>
	audio {
		position: absolute;
		fill: blue;
		bottom: 8px;
		right: 8px;
	}
</style>