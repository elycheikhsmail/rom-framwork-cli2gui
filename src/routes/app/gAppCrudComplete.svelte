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
	let appName = '';
    let controllerPrefix = '';
    let tableName = '' ;
	let appUrl = '';
	let cmd = '';
	function gComand() {
		if (!appName  || !appUrl || !controllerPrefix || !tableName ) {
			cmd = `you must fill each field`;
			isGenerated = false;
            isError = true;
		} else {
			cmd = `deno run --allow-all --unstable appCli.ts  gAppCrudComplete ${appName}  ${controllerPrefix}  ${tableName}  ${appUrl}`;
			isGenerated = true;
            isError = false;
		}
	}
</script>

<svelte:head>
	<title>generate app crud complete</title>
</svelte:head>

<div class="content"> 
	<h1>generate complete CRUD app</h1>
	<form>
		<p>
			<label for="app"> app name : </label> <br />
			<input type="text" placeholder="todo" bind:value={appName} />
		</p>
		<p>
			<label for="app"> app controller : </label> <br />
			<input type="text" placeholder="todo" bind:value={controllerPrefix} /> 
		</p>

		<p>
			<label for="app"> table name : </label> <br />
			<input type="text" placeholder="todo" bind:value={tableName} /> 
		</p>
		<p>
			<label for="app"> app url: </label> <br />
			<input type="text" placeholder="/todo" bind:value={appUrl} />
		</p>
	</form>
	<button on:click={gComand}> generate command</button>
	{#if isGenerated}
		<p>
			{cmd} <br /> <br /> <br />
			this will create app named app-{appName} <br />
			app-{appName} urls will be prefixed with {appUrl} <br />
			app-{appName} have 5 controllers  getlist getitem add delete and update <br />
            you will update sql commands in app-{appName}/config/mysql <br>
            and you will update sql in controller code and deps on this some this code <br>
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
