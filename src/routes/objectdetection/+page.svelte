<script>
// @ts-nocheck

	import { onMount, onDestroy } from 'svelte';

	let video;
	let liveView;
	let demosSection;
	let enableWebcamButton;
	let status = 'Loading Model';
	let model = undefined;
	let children = [];

	onMount(() => {
		async function init() {
			const tf = await import('@tensorflow/tfjs');
			const cocoSsd = await import('@tensorflow-models/coco-ssd');
			const getUserMediaSupported = () =>
				!!(navigator.mediaDevices && navigator.mediaDevices.getUserMedia);

			video = document.getElementById('webcam');
			liveView = document.getElementById('liveView');
			demosSection = document.getElementById('demosSection');
			enableWebcamButton = document.getElementById('webcamButton');

			if (status) {
				status = 'Model Loaded';
			}

			if (getUserMediaSupported()) {
				enableWebcamButton.addEventListener('click', enableCam);
			} else {
				console.warn('Media not supported on your browser');
			}

			async function enableCam() {
				if (!model) {
					return;
				}

				enableWebcamButton.classList.add('removed');

				const constraints = {
					video: true
				};

				try {
					const stream = await navigator.mediaDevices.getUserMedia(constraints);
					video.srcObject = stream;
					video.addEventListener('loadeddata', predictWebcam);
				} catch (error) {
					console.error('Error accessing webcam:', error);
				}
			}

			cocoSsd
				.load()
				.then(function (loadedModel) {
					model = loadedModel;
					demosSection.classList.remove('invisible');
				})
				.catch(function (error) {
					console.error('Error loading COCO-SSD model:', error);
				});

			function predictWebcam() {
				model.detect(video).then(function (predictions) {
					console.log(predictions);
					for (let i = 0; i < children.length; i++) {
						liveView.removeChild(children[i]);
					}
					children.splice(0);

					for (let n = 0; n < predictions.length; n++) {
						if (predictions[n].score > 0.66) {
							const p = document.createElement('p');

							p.innerText =
								predictions[n].class +
								' - with ' +
								Math.round(parseFloat(predictions[n].score) * 100) +
								'% confidence';
							const bbox = predictions[n].bbox;
							p.style.cssText = `
								position: absolute;
								z-index: 1;
								background-color: #007bff;
								color: white;
								left: ${bbox[0]}px;
								top: ${bbox[1] - 15}px;
							`;

							const highlighter = document.createElement('div');

							highlighter.className = 'highlighter';

							highlighter.style.cssText = `
								left: ${bbox[0]}px;
								top: ${bbox[1]}px;
								width: ${bbox[2]}px;
								height: ${bbox[3]}px;
								position: absolute;
								border: 2px solid #007bff;
								box-sizing: border-box;
								z-index: 1; 
							`;

							liveView.appendChild(highlighter);
							liveView.appendChild(p);
							children.push(highlighter);
							children.push(p);
						}
					}
					window.requestAnimationFrame(predictWebcam);
				});
			}
		}

		init();
	});

	onDestroy(() => {});
</script>

<a href="/">back</a>
<main>
	<h1>Real-Time Object Detection</h1>
	<p id="status">{status}</p>
	<section id="demosSection" class="invisible">
		<p>
			Hold some objects up close to your webcam to get a real-time classification! When ready click
			"enable webcam" below and accept access to the webcam when the browser asks
		</p>
		<button id="webcamButton">Enable Webcam</button>
		<div id="liveView" class="camView">
			<video id="webcam" autoplay width="640" height="480">
				<track kind="captions" src="" label="Captions" default />
			</video>
		</div>
	</section>
</main>

<style>
	main {
		font-family: 'Poppins', sans-serif;
	}

	h1 {
		font-family: 'Poppins', sans-serif;
	}

	button {
		background-color: #007bff;
		color: white;
		border: none;
		padding: 10px 20px;
		cursor: pointer;
	}

	#liveView {
		position: relative;
	}

	video {
		position: absolute;
	}

	a {
        font-family: 'Poppins', sans-serif;
		background-color: #007bff;
		color: white;
		border: none;
		padding: 10px 20px;
		cursor: pointer;
		text-decoration: none;
		display: inline-block;
		margin: 0px;
	}

</style>
