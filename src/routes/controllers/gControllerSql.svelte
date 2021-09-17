<script context="module">
	import { browser, dev } from '$app/env';

	// we don't need any JS on this page, though we'll load
	// it in dev so that we get hot module replacement...
	export const hydrate = dev;

	// ...but if the client-side router is already loaded
	// (i.e. we came here from elsewhere in the app), use it
	export const router = browser;

	// since there's no dynamic data here, we can prerender
	// it so that it gets served as a static asset in prod
	export const prerender = true;
</script>

<script lang="ts">
	let isGenerated = false;
    let isError = false;
	let appName = 'todo';
	let ctrname = 'todo';
	let methode = ''
	let ctrUrl = '/todo';
	let cmd = '';
	function gComand() {
		if (!appName || !ctrname || !ctrUrl) {
			cmd = `you must give app neme and url`;
			isGenerated = false;
            isError = true;
		} else {
			// deno run --allow-all --unstable appCli.ts  gController foo index  get  /
			cmd = `deno run --allow-all --unstable appCli.ts  gControllerSql   ${appName} ${ctrname}  ${ctrUrl} ${methode} `;
			isGenerated = true;
            isError = false;
		}
	}
</script>

<svelte:head>
	<title>generate controller </title>
</svelte:head>

<div class="content">
	<h1>generate sql controller</h1>
	<form>
		<p>
			<label for="app"> app name : </label> <br />
			<input type="text" placeholder="todo" bind:value={appName} />
		</p>
		<p>
			<label for="app"> controller name : </label> <br />
			<input type="text" placeholder="todo" bind:value={ctrname} />
		</p>
		<p>
			<label for="app"> methode name : </label> <br />
			<!-- <input type="text" placeholder="get" bind:value={methode} /> -->
			<select  bind:value={methode}>
				<option value="getlist" selected>getlist</option>
				<option value="getbyid">getbyid</option>
				<option value="add">add</option>
				<option value="update">update</option>
			</select>
		</p>

		<p>
			<label for="app"> controller url  : </label> <br />
			<input type="text" placeholder="/list" bind:value={ctrUrl} />
		</p>
	</form>
	<button on:click={gComand}> generate command</button>
    
	{#if isGenerated}
		<p>
			{cmd} <br /> <br /> <br />
			this will create sql controller   named {ctrname}Ctr inside app-{appName} <br />
			{ctrname}Ctr  url will be prefixed with {ctrUrl} <br /> 
			by default this ctr will return un html message as response with needed headers
		</p>
	{/if}
    {#if isError}
    <p>{cmd}</p>
    {/if}
</div>
 

<style>
	.content {
		width: 100%;
		max-width: var(--column-width);
		margin: var(--column-margin-top) auto 0 auto;
	}
</style>
