//Comment
<script lang="ts">
	import Intro from '$lib/Intro.svelte';
	import { Label, Input, Range, Radio, Checkbox } from 'flowbite-svelte';
	import { onMount } from 'svelte';

	export let data;
	
</script>

<!-- <Intro heading={data.meta.title} description={data.meta.description} />

<svelte:window bind:innerWidth bind:innerHeight /> -->

<section class="*">
	<div class="*">
		<h1> Hello , World </h1>
	</div>
</section>

//comment from  1 to 21 line.
<script>
	import { onMount, onDestroy } from "svelte";
	// @ts-ignore
	import Quagga from "quagga";
  
	let scannerInitialized = false;
	/**
	 * @type {null}
	 */
	let scannedData = null;
  
	function initScanner() {
	  if (!scannerInitialized) {
		Quagga.init({
		  inputStream: {
			name: "Live",
			type: "LiveStream",
			target: document.querySelector('#scanner-preview'),
			constraints: {
			  width: 350,
			  height: 350,
			  facingMode: "environment" // Use the rear camera for mobile devices
			},
		  },
		  locator: {
			patchSize: "medium",
			halfSample: true,
		  },
		  numOfWorkers: 4,
		  decoder: {
			readers: ["code_128_reader", "ean_reader", "ean_8_reader", "code_39_reader", "code_39_vin_reader", "codabar_reader", "upc_reader", "upc_e_reader", "i2of5_reader", "2of5_reader", "code_93_reader"],
		  },
		  locate: true,
		}, function (/** @type {any} */ err) {
		  if (err) {
			console.error(err);
			return;
		  }
		  Quagga.start();
		  scannerInitialized = true;
		});
  
		Quagga.onDetected(function (/** @type {{ codeResult: { code: string; }; }} */ result) {
      console.log("Barcode detected and processed : [" + result.codeResult.code + "]", result);
      // You can handle the scanned QR code data here, for example, by setting it to a variable.
      // You can also stop the scanner if needed: Quagga.stop();
    });
  
	  }
	}
  
	onMount(() => {
	  initScanner();
	});
  
	onDestroy(() => {
	  if (scannerInitialized) {
		Quagga.stop();
		scannerInitialized = false;
	  }
	});
  </script>
  
  <main>
	<h1 style="color: white;" >QR Code Scanner</h1>
	<div id="scanner-preview"></div>
	{#if scannedData}
	  <p >Scanned QR Code: {scannedData}</p>
	{/if}
  </main>
  
  <style>
	main {
	  text-align: center;
	  padding: 1em;
	}
  
	#scanner-preview {
	  width: 100%;
	  height: 300px;
	  border: 1px solid #ccc;
	}
  </style>
  
