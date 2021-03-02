<script>
	import AudioContext from './AudioContext.svelte'
	import AudioSource from './AudioSource.svelte'
	import Analizer from './Analizer.svelte'
	import Speakers from './Speakers.svelte'

	let visual = 'sinewave'
	let src
	
	
	let tracks = [
		{
			name: 'DJ Shadow - High Noon', 
			src: 'https://CoolLaboratory.github.io/assets/DJ%20Shadow%20-%20High%20Noon.mp3'
		},
		{
			name: 'Moloko - Sing it Back', 
			src: "https://CoolLaboratory.github.io/assets/Moloko%20-%20Sing%20it%20Back%20-%20Can%207%201930's%20Mix%20(HQ%20192kb).mp3"
		},
		{
			name: 'Nina Simone - Feeling Good', 
			src: 'https://CoolLaboratory.github.io/assets/Nina%20Simone%20-%20Feeling%20Good%20(Bassnectar%20Remix).mp3'
		},
		{
			name: 'Rodes - Katathliptikia', 
			src: 'https://CoolLaboratory.github.io/assets/Rodes%20-%20Katathliptikia.mp3'
		},
		{
			name: 'The Echocentrics - Don Alejo', 
			src: 'https://CoolLaboratory.github.io/assets/The%20Echocentrics%20-%20%20Don%20Alejo.mp3'
		},
		{
			name: 'The Black Keys - Too Afraid To Love You', 
			src: 'https://CoolLaboratory.github.io/assets/The%20Black%20Keys%20-%20Too%20Afraid%20To%20Love%20You.mp3'
		},
		{
			name: 'Kako Synapantima Band - Apagoreuesai', 
			src: 'https://CoolLaboratory.github.io/assets/kako%20synapantima%20band%20-%20Apagoreuesai.mp3'
		}
	]
</script>

<select bind:value="{visual}">
	<option value='sinewave'>Sinewave</option>
	<option value='frequencybars'>Frequency Bars</option>
	<option value="off">None</option>
</select>

<select bind:value={src}>
	{#each tracks as track}
	<option value={track.src}>{track.name}</option>
	{/each}
</select>
<br >

<AudioContext let:audioCtx>
	<AudioSource {audioCtx} let:source {src}>
  	<Analizer {audioCtx} {source} {visual} />
	  <Speakers {audioCtx} {source} />
	</AudioSource>
</AudioContext>