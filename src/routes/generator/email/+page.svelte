<script lang="ts">
	import { goto } from '$app/navigation';
	import { text, mailtoUrl } from '$lib/stores';
	import { onMount } from 'svelte';

	const subjectOptions = [
		'Bitte verhindern Sie den AfD-Kinderschutzkongress im Landtag',
		'AfD-Kongress zum Kinderschutz im Landtag',
		'Kinderschutz darf nicht durch AfD im Landtag missbraucht werden',
		'Appell an Sie: Stoppen Sie den AfD-Kongress im Namen des Kinderschutzes',
		'Kein Raum für AfD-Kinderschutzkongress im Niedersächsischen Landtag',
		'Landtag als Bühne für AfD-Ideologie unter dem Deckmantel Kinderschutz?',
		'Klares Zeichen gegen den AfD-Kinderschutzkongress im Parlament nötig',
		'AfD-Kongress im Landtag gefährdet demokratische Werte – bitte handeln Sie',
		'Verhindern Sie die Instrumentalisierung des Kinderschutzes durch die AfD',
		'Der Landtag darf kein Ort für den AfD-Kinderschutzkongress sein',
		'Missbrauch des Kinderschutzes durch AfD im Landtag verhindern',
		'AfD nutzt Kinderschutz zur Hetze – nicht im Landtag!',
		'Landtagsräume sind kein Ort für AfD-Kinderschutzpropaganda',
		'AfD-Kinderschutzkongress widerspricht dem Auftrag des Landtags',
		'Ich bitte Sie: Keine Bühne für den AfD-Kinderschutzkongress',
		'Kinderschutz als Vorwand für AfD-Ideologie im Landtag? Bitte handeln Sie',
		'Der AfD-Kongress zum Kinderschutz gehört nicht in unser Parlament',
		'Kinderschutzkongress der AfD ist ein Angriff auf Demokratie im Landtag',
		'Keine Legitimation für AfD-Kinderschutzkongress durch Landtagsräume',
		'Verhindern Sie den AfD-Kongress zum Kinderschutz im Landtag',
		'Der Landtag darf kein Sprachrohr für AfD-Ideologie zum Kinderschutz sein',
		'AfD-Kinderschutzkongress untergräbt demokratische Grundwerte',
		'Nutzung des Landtags für AfD-Kinder-Kongress ist inakzeptabel',
		'Kinderschutz im Deckmantel rechter Hetze? Nicht im Landtag!',
		'Bitte setzen Sie sich gegen den AfD-Kongress zum Kinderschutz ein',
		'Der Landtag darf kein Ort für rechtsextreme Kinderschutzagenda sein',
		'Kinderschutz ja – AfD-Kongress im Parlament nein!',
		'AfD-Kinderschutzkongress bedroht Vielfalt und Demokratie – bitte handeln Sie',
		'Öffentliche Räume des Landtags sind kein Ort für den AfD-Kinderschutzgipfel'
	];

	function openMailto(alternative?: boolean) {
		const subject = subjectOptions[Math.floor(Math.random() * subjectOptions.length)];

		mailtoUrl.set(
			`mailto:praesidentin@lt.niedersachsen.de?subject=${subject}&body=${$text.replaceAll('\n', '%0D%0A')}`
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

		<a
			href="/share?from=email"
			class="w-full block text-center bg-[red] text-white p-4 rounded-full cursor-pointer transform transition-all active:scale-95"
			>Kampagne Teilen</a
		>
		<div class="pt-4">
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
