<script>
	import { browser } from '$app/env';

	let outlookVersions = [
		{ id: 1, text: '2013', keyValue: '15.0' },
		{ id: 2, text: '2016', keyValue: '16.0' }
	];
	let selected;
	let output;

	const header = `Windows Registry Editor Version 5.00`;
	const pathBase = `[HKEY_CURRENT_USER\\Software\\Policies\\Microsoft\\Office\\`;
	const pathEnd = `\\Outlook\\AutoDiscover]`;
	const keys = `"PreferLocalXML"=dword:00000000 ; don't prefer a local XML file
"ExcludeHttpRedirect"=dword:00000000; check for redirection at autodiscover.domain.com
"ExcludeHttpsAutodiscoverDomain"=dword:00000001 ; don't check autodiscover.domain.com/autodiscover
"ExcludeHttpsRootDomain"=dword:00000001 ; don't check domain.com/autodiscover
"ExcludeScpLookup"=dword:00000001 ; no SCP (local via ActiveDirectory)
"ExcludeSrvRecord"=dword:00000001 ; no Srv
"ExcludeExplicitO365Endpoint"=dword:00000000 ;Allow 365
"ExcludeLastKnownGoodURL"=dword:00000001 ; don't prefer the 'last known good' aka the old server information
"Timeout"=dword:00002710 ;10 second timeout, min possible
"ShowCertErrors"=dword:00000001 ;show cert errors`;

	const handleSubmit = () => {
		output = makeRegistryOutput(selected.keyValue);
	};

	const handleDownload = (data) => {
		let blob = new Blob([data], {
			type: 'text/plain',
			endings: 'native'
		});
		let toDownload = URL.createObjectURL(blob);
		let filename = `OutlookAutodiscoverReg${selected.text}.reg`;
		downloadFile(toDownload, filename, '#download');
	};

	const makeRegistryOutput = (keyVersion) => {
		const keyPath = pathBase + keyVersion + pathEnd;
		return header + '\n\n' + keyPath + '\n' + keys;
	};

	const downloadFile = (toDownload, filename, elementSelector) => {
		let downloadButton = document.querySelector(elementSelector);
		downloadButton.setAttribute('href', toDownload);
		downloadButton.setAttribute('download', filename);
	};
</script>

<label>
	Outlook Version:
	<select bind:value={selected} on:blur={handleSubmit} on:change={handleSubmit}>
		{#each outlookVersions as value}
			<option {value}>
				{value.text}
			</option>
		{/each}
	</select>
</label>
<form on:submit|preventDefault={handleSubmit}>
	<label>
		Select Outlook Version:
		<select bind:value={selected}>
			{#each outlookVersions as value}
				<option {value}>
					{value.text}
				</option>
			{/each}
		</select>
	</label>
	<button type="submit"> Submit </button>
	<br />
</form>
{#if output}
	<div id="infoDiv">
		<a id="download" on:click={handleDownload(output)}>Download</a>
		<div class="div">
			<code>{output}</code>
		</div>
	</div>
{/if}

<style>
	form {
		padding: 2em;
		display: flex;
		justify-content: center;
		align-content: center;
	}
	code {
		color: darkgreen;
		padding: 5px;
		font-size: 105%;
	}
	a {
		-webkit-border-radius: 4px;
		-moz-border-radius: 4px;
		border-radius: 4px;
		border: solid 1px darkslategray;
		background: darkslategray;
		padding: 8px 12px;
		text-decoration: none;
		max-width: fit-content;
	}
	#infoDiv {
		display: flex;
		justify-content: center;
		flex-direction: column;
		align-content: center;
		align-items: center;
	}
</style>
