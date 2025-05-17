<script lang="ts">
	import { goto } from '$app/navigation';
	import { text, mailtoUrl } from '$lib/stores';
	import { onMount } from 'svelte';

	function openMailto(alternative?: boolean) {
		mailtoUrl.set(
			`mailto:praesidentin@lt.niedersachsen.de?subject=AfD-Kinderkongress&body=${$text.replaceAll('\n', '%0D%0A')}`
		);
		if (alternative) {
			window.location.href = $mailtoUrl;
		} else {
			window.open($mailtoUrl, '_blank');
		}
	}

	onMount(() => {
		if ($text == '') {
			goto('/');
			return;
		}
		setTimeout(openMailto, 1500);
	});
</script>

<div class="flex justify-center min-h-screen h-full">
	<main class="max-w-2xl w-full p-4">
		<header class="">
			<img src="/logo.png" alt="Die LINKE Heidekreis Logo" class="py-8" />
			<h1 class="font-bold text-xl pb-4">Briefgenerator: Keine Kindershitshow in Niedersachsen!</h1>
		</header>
		<div>
			Wir werden versuchen dein E-Mail Programm zu öffnen. Wenn das nicht funktioniert, kannst du
			den Text hier kopieren.
		</div>
		<div class="py-2">An: <b>praesidentin@lt.niedersachsen.de</b></div>
		<textarea value={$text} disabled class="w-full h-64"></textarea>

		<div class="flex gap-2">
			<button
				on:click={() => navigator.clipboard.writeText($text)}
				class="flex-1 border-[red] border-solid border-2 text-[red] p-4 rounded-full cursor-pointer"
				>Alles kopieren</button
			>
			<button
				on:click={openMailto(true)}
				class="flex-1 bg-[red] text-white p-4 rounded-full cursor-pointer"
			>
				Automatisch versuchen
			</button>
		</div>
	</main>
</div>
