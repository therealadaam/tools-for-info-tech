<script>
	import { test } from '@m-lab/ndt7';
	let uploadSpeed, downloadSpeed;

	const speedTest = async () => {
		const results = await test(
			{
				userAcceptedDataPolicy: true,
				downloadworkerfile: './ndt7-download-worker.min.js',
				uploadworkerfile: './ndt7-upload-worker.min.js'
			},
			{
				serverChosen: function (server) {
					console.log('Testing to:', {
						machine: server.machine,
						locations: server.location
					});
				},
				downloadComplete: function (data) {
					// (bytes/second) * (bits/byte) / (megabits/bit) = Mbps
					console.log(data.LastServerMeasurement);
					const serverBw = (data.LastServerMeasurement.BBRInfo.BW * 8) / 1000000;
					const clientGoodput = data.LastClientMeasurement.MeanClientMbps;
					console.log(
						`Download test is complete:
						Instantaneous server bottleneck bandwidth estimate: ${serverBw} Mbps
						Mean client goodput: ${clientGoodput} Mbps`
					);
					downloadSpeed = clientGoodput;
				},
				uploadComplete: function (data) {
					// TODO: used actual upload duration for rate calculation.
					// bytes * (bits/byte() * (megabits/bit) * (1/seconds) = Mbps
					const serverBw = (data.LastServerMeasurement.TCPInfo.BytesReceived * 8) / 1000000 / 10;
					const clientGoodput = data.LastClientMeasurement.MeanClientMbps;
					console.log(
						`Upload test is complete:
						Mean server throughput: ${serverBw} Mbps
						Mean client goodput: ${clientGoodput} Mbps`
					);
					uploadSpeed = clientGoodput;
				}
			}
		);
		const done = await results.exitcode;
	};
</script>

<!-- TODO: 
    fix this as it's trying to DL part of the NPM package to the 
    client browser to translate the data from the test.

	FIX:
	The fix is to adjust the npm module or get SvelteKit to server the 
	JS file. There's an option called config.downloadworkerfile and one for
	config.uploadworkerfile that can point to the proper files.
-->
<button on:click={speedTest}>TestSpeed!</button>
<!-- {#if downloadSpeed && uploadSpeed} -->
<div class="div">
	<p>Download: {downloadSpeed}</p>
	<p>Upload: {uploadSpeed}</p>
</div>

<!-- {/if} -->
<style>
	p {
		width: auto;
		color: darkolivegreen;
		/* font-size: 15; */
	}
</style>
