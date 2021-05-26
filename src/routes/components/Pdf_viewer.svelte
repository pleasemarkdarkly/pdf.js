<script lang="ts">
	import { onMount } from 'svelte';

	export let document;

	let pdfjsLib;
	let context, canvasRef;

	let innerWidth, innerHeight, outerWidth, outerHeight;

	const data = atob(
		'JVBERi0xLjcKCjEgMCBvYmogICUgZW50cnkgcG9pbnQKPDwKICAvVHlwZSAvQ2F0YWxvZwog' +
			'IC9QYWdlcyAyIDAgUgo+PgplbmRvYmoKCjIgMCBvYmoKPDwKICAvVHlwZSAvUGFnZXMKICAv' +
			'TWVkaWFCb3ggWyAwIDAgMjAwIDIwMCBdCiAgL0NvdW50IDEKICAvS2lkcyBbIDMgMCBSIF0K' +
			'Pj4KZW5kb2JqCgozIDAgb2JqCjw8CiAgL1R5cGUgL1BhZ2UKICAvUGFyZW50IDIgMCBSCiAg' +
			'L1Jlc291cmNlcyA8PAogICAgL0ZvbnQgPDwKICAgICAgL0YxIDQgMCBSIAogICAgPj4KICA+' +
			'PgogIC9Db250ZW50cyA1IDAgUgo+PgplbmRvYmoKCjQgMCBvYmoKPDwKICAvVHlwZSAvRm9u' +
			'dAogIC9TdWJ0eXBlIC9UeXBlMQogIC9CYXNlRm9udCAvVGltZXMtUm9tYW4KPj4KZW5kb2Jq' +
			'Cgo1IDAgb2JqICAlIHBhZ2UgY29udGVudAo8PAogIC9MZW5ndGggNDQKPj4Kc3RyZWFtCkJU' +
			'CjcwIDUwIFRECi9GMSAxMiBUZgooSGVsbG8sIHdvcmxkISkgVGoKRVQKZW5kc3RyZWFtCmVu' +
			'ZG9iagoKeHJlZgowIDYKMDAwMDAwMDAwMCA2NTUzNSBmIAowMDAwMDAwMDEwIDAwMDAwIG4g' +
			'CjAwMDAwMDAwNzkgMDAwMDAgbiAKMDAwMDAwMDE3MyAwMDAwMCBuIAowMDAwMDAwMzAxIDAw' +
			'MDAwIG4gCjAwMDAwMDAzODAgMDAwMDAgbiAKdHJhaWxlcgo8PAogIC9TaXplIDYKICAvUm9v' +
			'dCAxIDAgUgo+PgpzdGFydHhyZWYKNDkyCiUlRU9G'
	);
</script>

<svelte:head>
	<script src="//mozilla.github.io/pdf.js/build/pdf.js"></script>
</svelte:head>

<svelte:window bind:innerHeight bind:outerHeight bind:innerWidth bind:outerWidth />

<h1>{document}</h1>

<div class="fixed">
	<strong> Inner height: </strong>{innerHeight}, <br />
	<strong> Inner width: </strong>{innerWidth}, <br />
	<strong> Outer height: </strong>{outerHeight}, <br />
	<strong> Outer width: </strong>{outerWidth}, <br />
</div>

<canvas bind:this={canvasRef} />

<style>
	div.fixed {
		position: fixed;
		width: 50%;
		bottom: 40px;
		border: 3px solid #e9252f;
	}
</style>
