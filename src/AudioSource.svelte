<script>
	import {onMount} from 'svelte'

	/**@type {AudioContext}*/
	export let audioCtx
	
	/**@type {string}*/
	export let src
	
	/**@type {MediaElementAudioSourceNode}*/
	let source

	/**@type {HTMLAudioElement}*/
	let audioEl
	
	onMount(() => {
		source = audioCtx.createMediaElementSource(audioEl)
	})
</script>


<audio {src} controls bind:this={audioEl} crossorigin="anonymous" autoplay>
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