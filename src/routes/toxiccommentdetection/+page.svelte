<script>
	// @ts-nocheck
	import { onMount } from 'svelte';

	let tf;
	let toxicity;
	let model;
	let predictHistory = [];

	const threshold = 0.7;
	const labels = [
		'identity_attack',
		'insult',
		'obscene',
		'severe_toxicity',
		'sexual_explicit',
		'threat',
		'toxicity'
	];

	onMount(async () => {
		tf = await import('@tensorflow/tfjs');
		toxicity = await import('@tensorflow-models/toxicity');
		model = await toxicity.load(threshold, labels);
	});

	let sentence = '';
	let predictions = [];

	function handleInput(event) {
		sentence = event.target.value;
	}
    
	async function predictText() {
		const sentences = [sentence];
		predictions = await model.classify(sentences);
		console.log(predictions);
		predictHistory = [...predictHistory, { sentence, predictions }];
	}
</script>

<a href="/">back</a>
<main>
	<h1>Toxic Comment Detection</h1>
	<form>
		<input type="text" placeholder="i.e 'you suck'" bind:this={sentence} on:input={handleInput} />
		<button on:click={predictText}>Predict</button>
	</form>

	<h2>Predictions</h2>
	<table border="1" class="predictions-table">
		<tr>
			<th>sentence</th>
			{#each labels as label}
				<th>{label}</th>
			{/each}
		</tr>
		{#if predictHistory.length > 0}
			{#each predictHistory as { sentence, predictions }}
				<tr>
					<td>{sentence}</td>
					{#each predictions as { label, results }}
						<td>{results[0].match ? 'True' : 'False'}</td>
					{/each}
				</tr>
			{/each}
		{/if}
	</table>
</main>

<style>
	main {
		text-align: center;
		font-family: 'Poppins', sans-serif;
		margin: 0 auto;
		padding: 20px;
		max-width: 600px;
		display: flex;
		flex-direction: column;
		align-items: center;
		text-align: center;
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

	button {
		background-color: #007bff;
		color: white;
		border: none;
		padding: 10px 20px;
		cursor: pointer;
		width: fit-content;
	}

	form {
		display: flex;
		flex-direction: row;
		align-items: center;
		text-align: center;
		margin-bottom: 50px;
	}

	input {
		height: 30px;
		margin: 10px;
		width: 600px;
		padding-left: 10px;
	}

	.predictions-table {
		border-collapse: collapse;
		width: 100%;
	}

	th,
	td {
		border: 1px solid #ddd;
		padding: 8px;
		text-align: left;
	}
    
	th {
		background-color: #007bff;
		color: white;
	}
</style>
