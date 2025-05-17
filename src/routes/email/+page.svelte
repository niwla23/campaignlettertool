<script>
	import { text, mailtoUrl } from '$lib/stores';
	import { onMount } from 'svelte';

	function openMailto() {
		mailtoUrl.set(
			`mailto:praesidentin@lt.niedersachsen.de?subject=AfD-Kinderkongress&body=${$text.replaceAll('\n', '%0D%0A')}`
		);
		window.open($mailtoUrl, '_blank');
	}

	onMount(() => {
		setTimeout(openMailto, 500);
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
		<textarea value={$text} disabled class="w-full h-64"></textarea>
	</main>
</div>
