<script lang="ts">
	import { onMount } from 'svelte';

	export let document = '';

	let pdfjsLib;
	let canvasRef,
		errorMessage = '';

	let m = { x: 0, y: 0 };
	let key = 'waiting...',
		keyCode = '';
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

	const handleKeydown = (event) => {
		key = event.key;
		keyCode = event.keyCode;
	};

	const debugMessage = (msg: string) => {
		if (errorMessage.length > 250) errorMessage = '';
		errorMessage += msg;
	};

	onMount(() => {
		pdfjsLib = window['pdfjs-dist/build/pdf'];
		pdfjsLib.GlobalWorkerOptions.workerSrc = '//mozilla.github.io/pdf.js/build/pdf.worker.js';

		pdfjsLib
			.getDocument(document)
			.promise.then((doc) => doc.getPage(1))
			.then((page) => {
				const scale = 1.5;
				const viewport = page.getViewport({ scale });

				const context = canvasRef.getContext('2d');
				canvasRef.height = viewport.height;
				canvasRef.width = viewport.width;

				const renderContext = {
					canvasContext: context,
					viewport: viewport
				};

				const renderTask = page.render(renderContext);
				renderTask.promise.then(() => {
					debugMessage('renderTask completed');
				});
			});
	});
</script>

<svelte:head>
	<script src="//mozilla.github.io/pdf.js/build/pdf.js"></script>
</svelte:head>

<svelte:window
	bind:innerHeight
	bind:outerHeight
	bind:innerWidth
	bind:outerWidth
	on:keydown={handleKeydown}
/>

<h1>{document}</h1>

<canvas bind:this={canvasRef} on:mousemove={(e) => (m = { x: e.clientX, y: e.clientY })} />

<section>
	<div class="fixed">
		<strong> Mouse x y: </strong>{m.x}, {m.y}<br />
		<strong> Key down: </strong>{key}, {keyCode}<br />
		<strong> Inner height: </strong>{innerHeight} <br />
		<strong> Inner width: </strong>{innerWidth} <br />
		<strong> Outer height: </strong>{outerHeight} <br />
		<strong> Outer width: </strong>{outerWidth} <br /><br />
		<strong> Debug Messages: </strong>{errorMessage}<br />
	</div>
</section>

<style>
	div.fixed {
		padding: 1em;
		position: fixed;
		width: 50%;
		bottom: 25px;
		border: 2px solid #e9252f;
	}
</style>
