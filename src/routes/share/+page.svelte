<script lang="ts">
	import { goto } from '$app/navigation';
	import { text, mailtoUrl } from '$lib/stores';
	import { onMount } from 'svelte';

	const template = `Keine Kindershitshow in Niedersachsen! 🤚⛔
Die AfD Niedersachsen plant im Juni ihren sogenannten "Kinderschutzkongress". Nicht nur geht es der AfD dabei um alles Andere als den Schutz unserer Jugend, diese Kindershitshow soll auch im Gebäude des Niedersächsischen Landtags stattfinden. 🏛💔

Da haben wir etwas gegen – und wir brauchen deine Hilfe!

Nutze unseren Musterbrief über unseren E-Mail-Generator und verteidige mit uns die Würde des höchsten Hauses in Niedersachsen!

Kein öffentlicher Raum für rechtsextremes Geschwurbel! 🗣

[url]`;

	function copyLink() {
		navigator.clipboard.writeText(
			template.replaceAll('[url]', `${window.location.origin}?utm_source=sharelink`)
		);

		alert('Link kopiert!');
	}

	async function share() {
		try {
			await navigator.share({
				title: 'Keine Kindershitshow in Niedersachsen! 🤚⛔',
				text: template.replaceAll('[url]', `${window.location.origin}?utm_source=sharesheet`)
			});
		} catch {
			copyLink();
		}
	}
</script>

<div class="flex justify-center min-h-screen h-full">
	<main class="max-w-2xl w-full p-4">
		<header class="">
			<img src="/logo.png" alt="Die LINKE Heidekreis Logo" class="py-8" />
			<h1 class="font-bold text-xl pb-4">Teile die Kampagne</h1>
		</header>
		<div>
			Je mehr Menschen mitmachen je besser! Tippe auf Link kopieren oder Teilen und sende ihn an ein
			paar Freunde.
		</div>

		<div class="flex gap-2 pt-4">
			<button
				on:click={copyLink}
				class="flex-1 border-[red] border-solid border-2 text-[red] p-4 rounded-full cursor-pointer active:border-red-400 transition-all transform active:scale-95"
				>Link kopieren</button
			>
			<button
				on:click={share}
				class="flex-1 bg-[red] text-white p-4 rounded-full cursor-pointer transform transition-all active:scale-95"
				>Teilen</button
			>
		</div>
	</main>
</div>
