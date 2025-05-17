<script lang="ts">
	import { goto } from '$app/navigation';
	import { mailtoUrl, text } from '$lib/stores';
	import { jsPDF } from 'jspdf';

	const template = `
Sehr geehrte Frau Landtagspräsidentin Naber,

mein Name ist [user_name] und ich komme aus [user_city] im Wahlkreis [user_wahlkreis].
Ich schreibe Ihnen, da ich verärgert bin.

Wenige Tage nach dem niedersächsischen Kinderschutzkongress unter dem Motto
„(Auf)Wachsen in Vielfalt – Zum Umgang mit Eltern und ihren verschiedenen Einstellungen
und Meinungen“ am 18. Juni 2025, veranstaltet die AfD-Landtagsfraktion am 21. Juni 2025
ihren sogenannten „Ersten Kinderschutzkongress der AfD-Fraktion Niedersachsen“ im
Niedersächsischen Landtag.

Dem einen Kongress, zu dem, wie Ihnen sicherlich bekannt, das niedersächsische
Ministerium für Soziales, Arbeit, Gesundheit und Gleichstellung und die Kinderschutz-Akademie
des Kinderschutzbundes Landesverband Niedersachsen einladen, geht es um den Abbau
von Diskriminierung und ganzheitlicher Bildungsarbeit. Der andere Kongress der AfD
dreht sich um die Diskussion um sogenannte „Frühsexualisierung“, „Gender-Propaganda“,
„Pädophilie-Verharmlosung“ und „Lebensschutz“, wie auf der Webseite
der Landtagsfraktion nachzulesen ist.

Dass alle genannten Begriffe in rechtsextremistischen Kreisen (die offizielle Beurteilung
der AfD als rechtsextrem steht bekanntlich aktuell unter Stillhaltezusage
des Verfassungsschutzes) verwendet werden, um mittels der Instrumentalisierung
des „Kinderschutzes“ marginalisierte Gruppen zu kriminalisieren und öffentlich
zu denunzieren, ist bedauerlich.

Dass die AfD hierzu die öffentlichen Räumlichkeiten des niedersächsischen Landtags nutzen
und unser Landesparlament, welches als Legislative Niedersachsens eine besondere
Bedeutung und Verantwortung hinsichtlich seiner humanistisch geprägten, demokratischen
Werte hat, missbrauchen kann, ist erschreckend. Dieser Umstand macht zutiefst betrübt.
Auch nach außen wirkt es fast skandalös, dass der Partei, dessen innerer Rädelsführer
„Faschist“ genannt werden darf, die Möglichkeit dieser eiskalten Instrumentalisierung
von Kinderschutz und den Räumlichkeiten des Parlaments geboten ist.

Ich bitte Sie daher, alles in Ihrer Position als Präsidentin des niedersächsischen Landtags
stehende zu tun, um den Kongress der AfD im niedersächsischen Landtag zu verhindern.
Bitte setzen Sie sich mit all Ihren Mitteln ein, um diesen Schaden von unserem Parlament
abzuwenden.


Mit freundlichen Grüßen
[user_name]

  `;

	const wahlkreise = [
		'024: Aurich – Emden',
		'025: Unterems',
		'026: Friesland – Wilhelmshaven – Wittmund',
		'027: Oldenburg – Ammerland',
		'028: Delmenhorst – Wesermarsch – Oldenburg-Land',
		'029: Cuxhaven – Stade II',
		'030: Stade I – Rotenburg II',
		'031: Mittelems',
		'032: Cloppenburg – Vechta',
		'033: Diepholz – Nienburg I',
		'034: Osterholz – Verden',
		'035: Rotenburg I – Heidekreis',
		'036: Harburg',
		'037: Lüchow-Dannenberg – Lüneburg',
		'038: Osnabrück-Land',
		'039: Stadt Osnabrück',
		'040: Nienburg II – Schaumburg',
		'041: Stadt Hannover I',
		'042: Stadt Hannover II',
		'043: Hannover-Land I',
		'044: Celle – Uelzen',
		'045: Gifhorn – Peine',
		'046: Hameln-Pyrmont – Holzminden',
		'047: Hannover-Land II',
		'048: Hildesheim',
		'049: Salzgitter – Wolfenbüttel',
		'050: Braunschweig',
		'051: Helmstedt – Wolfsburg',
		'052: Goslar – Northeim – Göttingen II',
		'053: Göttingen I'
	];

	async function generateLetter(event: SubmitEvent) {
		event.preventDefault();

		const isEmail = event.submitter.id == 'btn_email';

		const form = event.target;
		const formData = new FormData(form);

		const data = Object.fromEntries(formData.entries());
		console.log(data);

		if (!import.meta.env.DEV) {
			window.umami.track('generate_letter', {
				wahlkreis: data.user_wahlkreis,
				city: data.user_city,
				isEmail
			});
		}

		text.set(
			template
				.replaceAll('[user_name]', data.user_name)
				.replaceAll('[user_city]', data.user_city)
				.replaceAll('[user_wahlkreis]', data.user_wahlkreis.toString().split(':')[1].trim())
		);

		if (isEmail) {
			// const url = new URL('mailto:praesidentin@lt.niedersachsen.de');
			// url.searchParams.append('subject', 'AfD-Kinderkongress');
			// url.searchParams.append('body', text);
			goto('/generator/email');
			return;
		}

		const doc = new jsPDF();
		console.log(doc.getFontList());

		doc.setFont('times');
		doc.setFontSize(13);
		doc.text($text, 15, 40);
		doc.save('AfD_Kinderkongress_Brief.pdf');
	}
</script>

<div class="flex justify-center min-h-screen h-full">
	<main class="max-w-2xl w-full p-4">
		<header class="">
			<img src="/logo.png" alt="Die LINKE Heidekreis Logo" class="py-8" />
			<h1 class="font-bold text-xl pb-4">
				Briefgenerator: Keine Kindershitshow in Niedersachsen! 🤚⛔
			</h1>
		</header>
		<form class="flex flex-col gap-4" on:submit={generateLetter}>
			<label class="flex flex-col lg:flex-row justify-between">
				<span>Dein Name</span>
				<input type="text" required name="user_name" />
			</label>
			<label class="flex flex-col lg:flex-row justify-between">
				<span>Von wo kommst du? (deine Stadt)</span>
				<input required type="text" name="user_city" />
			</label>
			<label class="flex flex-col lg:flex-row justify-between">
				<span>In welchem Wahlkreis gehst du wählen?</span>
				<select required name="user_wahlkreis">
					<option value={null} disabled selected>Bitte wählen</option>
					{#each wahlkreise as wahlkreis (wahlkreis)}
						<option value={wahlkreis}>{wahlkreis}</option>
					{/each}
				</select>
			</label>
			<div class="flex gap-2">
				<!-- <div class="text-center -my-2 font-bold">ODER</div> -->
				<input
					type="submit"
					id="btn_letter"
					value="Brief drucken (PDF)"
					class="bg-[red] text-white p-4 rounded-full cursor-pointer flex-1"
				/>
				<input
					type="submit"
					id="btn_email"
					value="E-Mail verschicken"
					class="bg-[red] text-white p-4 rounded-full cursor-pointer flex-1"
				/>
			</div>
		</form>
		<footer class="text-center pt-8 text-gray-400">
			<a
				href="https://www.dielinke-heidekreis.de/service/impressum/"
				class="underline text-center w-full">Impressum</a
			>
		</footer>
	</main>
</div>
