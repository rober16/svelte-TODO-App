<script>

const contacts = (async () => {
		const response = await fetch('https://randomuser.me/api/')
    return await response.json()
	})()
</script>

<style>
	.card {
		position: relative;
		margin: 0.5em;
		padding: 0.5em 0.5em 0.5em 6em;
		border: 1px solid #eee;
		border-radius: 4px;
		box-shadow: 2px 2px 4px rgba(0,0,0,0.1);
		min-height: 5em;
	}
    .card::after {
		clear: both;
		display: block;
	}

	.avatar {
		position: absolute;
		width: 5em;
		height: 5em;
		left: 0.5em;
		top: 0.5em;
		border-radius: 50%;
		background: no-repeat 50% 50%;
		background-size: cover;
	}
    h3 {
		margin: 0 0 0.5em 0;
		font-size: 16px;
	}

	p {
		margin: 0;
		font-size: 14px;
	}
</style>


<div class="container">

    {#await contacts}
    <p class="loading">loading...</p>
    
    {:then data}
	<h2>Mis contactos</h2>
    <div class='card'>
        <span class='avatar' style='background: url({data.results['0'].picture.large})'></span>
        <h3>{data.results['0'].name.first}</h3>
        <p>{data.results['0'].email}</p>
        <p>{data.results['0'].phone}</p>
    </div>

    <div class='card'>
        <span class='avatar' style='background: url({data.results['0'].picture.large})'></span>
        <h3>{data.results['0'].name.first}</h3>
        <p>{data.results['0'].email}</p>
        <p>{data.results['0'].phone}</p>
    </div>

    <div class='card'>
        <span class='avatar' style='background: url({data.results['0'].picture.large})'></span>
        <h3>{data.results['0'].name.first}</h3>
        <p>{data.results['0'].email}</p>
        <p>{data.results['0'].phone}</p>
    </div>

    {:catch error}

	<p>An error occurred!</p>
    {/await}

</div>




