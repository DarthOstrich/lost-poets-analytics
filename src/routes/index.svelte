<script>
	let totalPages = 0;
	let totalPoets = 0;
  let allOrigins = [];
  let allPoets = [];
	let totalOrigins = 0;
  let originsWithTheirPoets = [];
	let loadingOrigins = true;
	let loadingPoets = true;

	const options = { method: 'GET' };

  function getPoet(tokenID) {
		fetch(`https://lostpoets.api.manifoldxyz.dev/metadata/${tokenID}`, options)
			.then((response) => response.json())
      .then((response) => {
        return response
      })
  }
  async function getData() {
    getOrigins(1);
    getPoets(1025);
  }

	function getOrigins(tokenID) {
		fetch(`https://lostpoets.api.manifoldxyz.dev/metadata/${tokenID}`, options)
			.then((response) => response.json())
			.then((response) => {
        totalOrigins++;
        const originTrait = response.name.split('#')[1]
				console.log(`Poet ${tokenID}:`, originTrait);
        allOrigins.push(response)
        /* originsWithTheirPoets.push({originTrait}) */
				/* console.log(`Origins List:`, allOrigins); */
        tokenID++
        if(tokenID === 10) {
          loadingOrigins = false;
          return
        }
        setTimeout(getOrigins(tokenID), 500)
			})
      .catch((err) => {
        console.error(err)
        loadingOrigins = false;
        return;
      });
	}
	function getPoets(tokenID) {
		fetch(`https://lostpoets.api.manifoldxyz.dev/metadata/${tokenID}`, options)
			.then((response) => response.json())
			.then((response) => {
        totalPoets++;
				console.log(`Poet ${tokenID}:`, response.name);
        allPoets.push(response)
        tokenID++
        if(tokenID === 1030) {
          loadingPoets = false;
          return
        }
        setTimeout(getPoets(tokenID), 1000)
			})
      .catch((err) => {
        console.error(err)
        loadingPoets = false;
        return;
      });
	}

  /* getPoets(1025); */

	/* function getPoets(offset) { */
	/* 	fetch( */
	/* 		`https://api.opensea.io/api/v1/assets?asset_contract_address=0x4b3406a41399c7fd2ba65cbc93697ad9e7ea61e5&order_direction=desc&offset=${offset}&limit=50`, */
	/* 		options */
	/* 	) */
	/* 		.then((response) => response.json()) */
	/* 		.then((response) => { */
	/* 			console.log(`${offset} Response:`, response); */
	/* 			const { assets } = response; */
	/*       totalPoets = totalPoets + response.assets.length */
	/* 			allPoets.push(...assets); */
	/* 			if (response.assets.length <= 50) { */
	/* 				setTimeout(getPoets(offset + 50), 500) */
	/* 			} else { */
	/* 				return; */
	/* 			} */
	/* 		}) */
	/* 		.catch((err) => console.error(err)); */
	/* } */
	/* getPoets(0) */

	/* function getOrigins() { */
	/* 	fetch( */
	/* 		'https://api.opensea.io/api/v1/assets?asset_contract_address=0x4b3406a41399c7fd2ba65cbc93697ad9e7ea61e5&order_direction=asc&offset=0&limit=50', */
	/* 		options */
	/* 	) */
	/* 		.then((response) => response.json()) */
	/* 		.then((response) => { */
	/* 			console.log('Origins:', response); */
	/* 		}) */
	/* 		.catch((err) => console.error(err)); */
	/* } */
	/* getOrigins(); */
</script>

<h1>Welcome to SvelteKit</h1>
{#await getData()}
  <p>Loading Origins...</p>
  <p>Loading Poets...</p>
{:then}
  <p>Total Origins: {totalOrigins}</p>
  <p>Total Poets: {totalPoets}</p>
{/await}

{#if loadingOrigins}
  <p>Loading...</p>
  {:else}
    <ul>
    {#each allOrigins as { name }}
      <li>{name}</li>
    {/each}
    </ul>

{/if}
