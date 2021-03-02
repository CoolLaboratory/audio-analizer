<script>

	/**@type {AudioContext} */
	const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
</script>

<slot {audioCtx} />