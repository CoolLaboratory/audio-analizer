<script>
	import {onMount, tick} from 'svelte'
	
	/**@type {AudioContext} */
	export let audioCtx;
	export let source;
	export let visual = 'sinewave'
	
	let canvas
	let canvasCtx
	let drawVisual
	
	const analyser = audioCtx.createAnalyser();
	analyser.minDecibels = -90;
	analyser.maxDecibels = -10;
	analyser.smoothingTimeConstant = 0.85;
	
	$: visualSetting = visual
	
	onMount(async () => {
		await tick()
		canvasCtx = canvas.getContext("2d");
		source.connect(analyser);
		
		source.mediaElement.onplay = (e) => {
			console.log('isPlaying')
			visualize()
		}
		
		source.mediaElement.onpause = (e) => {
			console.log('isPaused')
			window.cancelAnimationFrame(drawVisual)
		}

		console.log(source)
		
	})
	
	function visualize() {
		const WIDTH = canvas.width = window.innerWidth;
		const HEIGHT = canvas.height = window.innerHeight;
		
		const options = { 
			WIDTH, 
			HEIGHT 
		}


		if(visualSetting == "sinewave") {
			buildSineWave(options);
		} 
		else if(visualSetting == "frequencybars") {
			buildFrequencyBars(options)
		} 
		else if(visualSetting == "off") {
			canvasCtx.clearRect(0, 0, WIDTH, HEIGHT);
			canvasCtx.fillStyle = "red";
			canvasCtx.fillRect(0, 0, WIDTH, HEIGHT);
		}
	}

	function buildSineWave({
		WIDTH, 
		HEIGHT
	}) {
		analyser.fftSize = 1024;
    var bufferLength = analyser.fftSize;
    console.log(bufferLength);
    var dataArray = new Float32Array(bufferLength);

    canvasCtx.clearRect(0, 0, WIDTH, HEIGHT);

    function draw() {

      drawVisual = requestAnimationFrame(draw);

      analyser.getFloatTimeDomainData(dataArray);

      canvasCtx.fillStyle = 'rgb(0, 0, 0)';
      canvasCtx.fillRect(0, 0, WIDTH, HEIGHT);

      canvasCtx.lineWidth = 2;
      canvasCtx.strokeStyle = 'rgb(0, 200, 200)';

      canvasCtx.beginPath();

      var sliceWidth = WIDTH * 1.0 / bufferLength;
      var x = 0;

      for(var i = 0; i < bufferLength; i++) {
   
        var v = dataArray[i] * 200.0;
        var y = HEIGHT/2 + v;

        if(i === 0) {
          canvasCtx.moveTo(x, y);
        } else {
          canvasCtx.lineTo(x, y);
        }

        x += sliceWidth;
      }

      canvasCtx.lineTo(canvas.width, canvas.height/2);
      canvasCtx.stroke();
    };

    draw();
	}
	
	function buildFrequencyBars({
		WIDTH, 
		HEIGHT
	}) {
		analyser.fftSize = 256;
    var bufferLength = analyser.frequencyBinCount;
    console.log(bufferLength);
    var dataArray = new Float32Array(bufferLength);

    canvasCtx.clearRect(0, 0, WIDTH, HEIGHT);

    function draw() {
      drawVisual = requestAnimationFrame(draw);

      analyser.getFloatFrequencyData(dataArray);

      canvasCtx.fillStyle = 'rgb(0, 0, 0)';
      canvasCtx.fillRect(0, 0, WIDTH, HEIGHT);

      var barWidth = (WIDTH / bufferLength) * 2.5;
      var barHeight;
      var x = 0;

      for(var i = 0; i < bufferLength; i++) {
        barHeight = (dataArray[i] + 140)*3;
        
        canvasCtx.fillStyle = 'rgb(' + Math.floor(barHeight+100) + ',50,50)';
        canvasCtx.fillRect(x,HEIGHT-barHeight/2,barWidth,barHeight/2);

        x += barWidth + 1;
      }
    };

    draw();
	}
	
	function onresize(e) {
		console.log('window resized.')

		window.cancelAnimationFrame(drawVisual);
  	visualize();
	}
</script>

<svelte:window on:resize={onresize} />
<canvas bind:this="{canvas}"></canvas>

<style>
	canvas {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		z-index: -1;
	}
</style>