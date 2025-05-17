<script lang="ts">
	import { jsPDF } from 'jspdf';

	const template = `
Sehr geehrte Landtagspräsidentin Naber, 

mein Name ist [user_name] und ich komme aus [user_city] im Wahlkreis [user_wahlkreis].
Ich schreibe Ihnen, da ich verärgert bin.  

Wenige Tage nach dem Niedersächsischen Kinderschutzkongress unter dem Motto 
“(Auf)Wachsen in Vielfalt – Zum Umgang mit Eltern und ihren verschiedenen Einstellungen 
und Meinungen” am 18. Juni 2025, veranstaltet die AfD-Landtagsfraktion am 21. Juni 2025 
ihren sogenannten “Ersten Kinderschutzkongress der AfD-Fraktion Niedersachsen” im 
Niedersächsischen Landtag.  

Dem einen Kongress, zu dem wie Ihnen sicherlich bekannt das niedersächsische 
Ministerium für Soziales, Arbeit, Gesundheit und Gleichstellung und die 
Kinderschutz-Akademie des Kinderschutzbundes Landesverband Niedersachsen einladen, 
geht es um den Abbau von Diskriminierung und ganzheitlicher Bildungsarbeit. Der andere 
Kongress der AfD dreht sich um die Diskussion um sogenannte “Frühsexualisierung”, 
“Gender-Propaganda”,  “Pädophilie-Verharmlosung” und “Lebensschutz”, wie auf der 
Webseite der Landtagsfraktion nachzulesen ist. 

Dass alle genannten Begriffe in rechtsextremistischen Kreisen - die offizielle Beurteilung der 
AfD als rechtsextrem steht aktuell unter Stillhaltezusage des Verfassungsschutzes, der 
Beurteilung als dementsprechendes Verdachtsobjekt und “in Teilen gesichert rechtsextrem” 
unterliegt diese Partei bereits jetzt - verwendet werden, um mittels der Instrumentalisierung 
des “Kinderschutzes” marginalisierte Gruppen zu kriminalisieren und öffentlich zu 
denunzieren, bedauerlich. Dass die AfD hierzu die öffentlichen Räumlichkeiten des 
Niedersächsischen Landtags nutzen und unser Landesparlament, welches als Legislative 
Niedersachsens eine besondere Bedeutung und Verantwortung hinsichtlich seiner 
humanistisch geprägten, demokratischen Werte hat, missbrauchen kann, ist erschreckend. 
Dieser Umstand macht zutiefst betrübt. Auch nach außen wirkt es fast skandalös, dass der 
Partei, dessen innerer Rädelsführer “Faschist” genannt werden darf, die Möglichkeit dieser 
eiskalten Instrumentalisierung von Kinderschutz und den Räumlichkeiten des Parlaments 
geboten ist.  

Ich bitte Sie daher, alles in Ihrer Position als Präsidentin des Niedersächsischen Landtags 
stehende zu tun, um den Kongress der AfD im Niedersächsischen Landtag zu verhindern. 
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
		window.umami.track('generate_letter');
		event.preventDefault();
		const form = event.target;
		const formData = new FormData(form);

		const data = Object.fromEntries(formData.entries());
		console.log(data);

		const text = template
			.replaceAll('[user_name]', data.user_name)
			.replaceAll('[user_city]', data.user_city)
			.replaceAll('[user_wahlkreis]', data.user_wahlkreis.toString().split(':')[1].trim());

		const doc = new jsPDF();

		// const myFont = await (await fetch('/AncizarSerif-Regular.ttf')).blob();
		// Load and convert font to base64 string
		const base64Font = await fetch('/AncizarSerif-Regular.ttf')
			.then((res) => res.blob())
			.then(
				(blob) =>
					new Promise((resolve, reject) => {
						const reader = new FileReader();
						reader.onloadend = () => resolve(reader.result.split(',')[1]); // base64 only
						reader.onerror = reject;
						reader.readAsDataURL(blob);
					})
			);

		doc.addFileToVFS('font.ttf', base64Font);
		doc.addFont('font.ttf', 'font', 'normal');
		doc.setFont('HelveticaNeue');
		doc.setFontSize(13);
		doc.text(text, 10, 30);
		doc.save('AfD_Kinderkongress_Brief.pdf');
		window.umami.track('generate_letter_saved');
	}
</script>

<div class="flex justify-center min-h-screen h-full">
	<main class="max-w-2xl w-full p-4">
		<header class="">
			<img src="/logo.png" alt="Die LINKE Heidekreis Logo" class="py-8" />
			<h1 class="font-bold text-xl pb-4">Briefgenerator: Keine Kindershitshow in Niedersachsen!</h1>
		</header>
		<form class="flex flex-col gap-4" on:submit={generateLetter}>
			<label class="flex justify-between items-center">
				<span>Dein Name</span>
				<input type="text" name="user_name" />
			</label>
			<label class="flex justify-between items-center">
				<span>Von wo kommst du? (deine Stadt)</span>
				<input type="text" name="user_city" />
			</label>
			<label class="flex justify-between items-center">
				<span>In welchem Wahlkreis gehst du wählen?</span>
				<select name="user_wahlkreis">
					{#each wahlkreise as wahlkreis (wahlkreis)}
						<option value={wahlkreis}>{wahlkreis}</option>
					{/each}
				</select>
			</label>
			<input
				type="submit"
				value="Brief generieren"
				class="bg-[red] text-white p-4 rounded-full cursor-pointer"
			/>
		</form>
	</main>
</div>
