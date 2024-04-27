<script lang=ts>
	let username = ''
	let fav_tv_show = ''
	let response_data: any
	let get_api_data = async () => {
		let outputStr = fav_tv_show 
			.split('') 
			.map(char => (char === ' ' ? '%20' : char)) 
			.join('');
		console.log(outputStr);
		console.log(username, fav_tv_show)

		let fetch_url = `http://127.0.0.1:5000/fget_anime_recs/${username}?fav_tv_show=${outputStr}`		
		console.log(fetch_url)
		
		let fetch_response = await fetch(fetch_url)
		console.log(fetch_response)
		
		let fetch_response_json = await fetch_response.json()
		console.log(fetch_response_json)

		response_data = fetch_response_json['data']
	}

	let nums = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
</script>

<style>
	main-wrapper {
		display: grid;
		height: 100vh;

		grid-template:
		"head" 1fr
		"body" 5fr
		"output" 10fr
		;
	}

	main-header {
		grid-area: head;
		height: 100%;
		
		display: grid;
		justify-content: center;
		align-items: center;
		height: 100%;
		width: 100%;
	}

	main-body {
		grid-area: body;
		height: 100%;

		display: grid;
		grid-template: 
		"username" 3fr
		"anime" 3fr
		"submit" 1fr
		;
	}

	body-username {
		grid-area: username;
	}

	button {
		grid-area: submit;
	}

	main-output {
		width: 100%;
		display: grid;
		grid-template-rows: repeat(5, 1fr);
		grid-template-columns: repeat(2, 1fr);
		grid-row-gap: .5em;
	}

	p {
		margin: 0px;
		padding: 0px;	
	}

	my-rec {
		display: grid;
		justify-content: center;
		align-self: center;
	}

	my-rec > p {
		justify-self: center;
		align-items: center;
	}


</style>

<main-wrapper>
	<main-header>
		<h1>AniLyst</h1>
	</main-header>
	<main-body>
		<body-username>
			<p>Username:</p>
			<input type="text" bind:value={username}>
		</body-username>
		<body-anime>
			<p>Anime you'd like recommendations for:</p>
			<input type="text" bind:value={fav_tv_show}>
		</body-anime>
		<button on:click={get_api_data}>Submit</button>
	</main-body>
	<main-output>
		{#if response_data}
			{#each nums as num}
				<my-rec>
					<p>Rank: {num+1}</p>
					<p>{response_data[num]['english_name']}</p>
					<p>Rating: {response_data[num]['score']}</p>
				</my-rec>
			{/each}
		{:else}
			Waiting...
		{/if}
	</main-output>
</main-wrapper>