<<<<<<< HEAD
<div class="link-holder">
    <a href="/imageclassification">image classification</a>
    <a href="/objectdetection">object detection</a>
	<a href="/toxiccommentdetection">toxic comment detection</a>
</div>
=======
<script>
	import { onMount } from 'svelte';
	import * as tf from '@tensorflow/tfjs';
	import * as mobilenet from '@tensorflow-models/mobilenet';

	let isLoadingModel = true;
	let model = null;
	let imageUrl = null;
	let identificationResults = [];
	let imageHistory = [];

	let imageElement = null;
	let urlInput = null;
	let fileInput = null;

	const loadModel = async () => {
		await tf.setBackend('cpu');
		try {
			model = await mobilenet.load();
			isLoadingModel = false;
		} catch (error) {
			console.log(error);
			isLoadingModel = false;
		}
	};

	const uploadImage = (e) => {
		const { files } = e.target;
		if (files.length > 0) {
			const url = URL.createObjectURL(files[0]);
			imageUrl = url;
			imageHistory = [imageUrl, ...imageHistory];
		} else {
			imageUrl = null;
		}
	};

	const identifyImage = async () => {
		urlInput.value = '';
		const results = await model.classify(imageElement);
		identificationResults = results;
	};

	const handleUrlChange = (e) => {
		imageUrl = e.target.value;
		identificationResults = [];
	};

	const triggerFileUpload = () => {
		fileInput.click();
	};

	onMount(() => {
		loadModel();
	});
</script>

{#if isLoadingModel}
	<h2 class="loadingModel">Loading Model...</h2>
{:else}
	<div class="App">
		<h1 class="header">Image Identification</h1>

		<div class="inputHolder">
			<input
				type="file"
				accept="image/*"
				capture="camera"
				class="uploadInput"
				on:change={uploadImage}
				bind:this={fileInput}
			/>
			<button class="uploadImage" on:click={triggerFileUpload}>Upload Image</button>
			<br />
			<span>or</span>
			<br />
			<input
				type="text"
				placeholder="Paste image URL"
				bind:this={urlInput}
				on:change={handleUrlChange}
			/>
		</div>

		<div class="mainWrapper">
			<div class="mainContent">
				<div class="imageHolder">
					{#if imageUrl}
						<img
							src={imageUrl}
							alt="Upload Preview"
							crossOrigin="anonymous"
							bind:this={imageElement}
						/>
					{/if}
				</div>
				{#if identificationResults.length > 0}
					<div class="resultsHolder">
						{#each identificationResults as result, index}
							<div class="result" key={result.className}>
								<span class="name">{result.className}</span>
								<span class="confidence">
									Confidence level: {(result.probability * 100).toFixed(2)}% {#if index === 0}
										<span class="bestGuess"> Best Guess</span>
									{/if}
								</span>
							</div>
						{/each}
					</div>
				{/if}
			</div>

			{#if imageUrl}
				<button class="button" on:click={identifyImage}>Identify Image</button>
			{/if}
		</div>

		{#if imageHistory.length > 0}
			<div class="recentPredictions">
				<h2>Recent Images</h2>
				<div class="recentImages">
					{#each imageHistory as image, index}
						<div class="recentPrediction" key={`${image}${index}`}>
							<img src={image} alt="Recent Prediction" on:click={() => (imageUrl = image)} />
						</div>
					{/each}
				</div>
			</div>
		{/if}
	</div>
{/if}
>>>>>>> parent of 4fe6326 (added navigation screen and object detection)

<style>
	.loadingModel {
		display: flex;
		align-items: center;
		justify-content: center;
		height: 100vh;
	}

	.App {
		text-align: center;
		font-family: 'Poppins', sans-serif;
		margin: 0 auto;
		padding: 20px;
		max-width: 600px;
	}

	.header {
		margin-bottom: 20px;
	}

	.inputHolder {
		margin-bottom: 20px;
	}

	.uploadInput {
		display: none;
	}

	.uploadImage {
		background-color: #007bff;
		color: white;
		border: none;
		padding: 10px 20px;
		cursor: pointer;
	}

	.or {
		margin: 10px 0;
	}

	.mainWrapper {
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	.imageHolder img {
		max-width: 100%;
		margin: 20px 0;
	}

	.resultsHolder {
		margin-top: 20px;
	}

	.result {
		margin-bottom: 10px;
	}

	.button {
		background-color: #007bff;
		color: white;
		border: none;
		padding: 10px 20px;
		cursor: pointer;
	}

	.recentImages {
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
	}

	.recentPrediction {
		margin: 10px;
	}

	.recentPrediction img {
		max-width: 100px;
		cursor: pointer;
	}
</style>
