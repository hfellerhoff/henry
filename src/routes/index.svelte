<script lang="ts">
	import Logo from '$lib/components/Logo.svelte';
	import MusicManager from '$lib/components/MusicManager.svelte';
	import StartButton from '$lib/components/StartButton.svelte';
	import ContinueButton from '$lib/components/ContinueButton.svelte';

	import BackgroundImage from '../lib/components/BackgroundImage.svelte';
	import Introduction from '$lib/sections/Introduction.svelte';
	import MusicStart from '$lib/sections/MusicStart.svelte';

	import * as Tone from 'tone';
	import { scrollRef } from 'svelte-scrolling';
	import LinkButton from '$lib/components/LinkButton.svelte';
	import { musicStage } from '$lib/stores/musicStore';

	let readyStates: Record<string, boolean> = {
		start: false,
		'music-start': false,
		'stage-1': false,
		'stage-2': false,
		'stage-3': false,
		'stage-4': false,
		'stage-5': false,
		'stage-6': false
	};

	const handleClick = (key: string) => () => {
		Tone.start();
		readyStates[key] = true;

		if (key.includes('stage')) {
			musicStage.set(parseInt(key.split('-')[1]));
		}
	};
</script>

<MusicManager hasStarted={readyStates.start} />

<section>
	<Logo />
	<BackgroundImage src="bloom.jpg" alt="Zoomed out photo of a Lake Erie algal bloom" />
	<h1>Silent Killer in the Great Lakes</h1>
	<p>
		Since the 1960s, algal blooms in the Great Lakes have been growing and intensifying. Now, they
		threaten the stability of the Great Lakes ecosystems and the animals that live there.
	</p>
	<StartButton onClick={handleClick('start')} />
</section>

<div class:hide={!readyStates.start}>
	<Introduction hasStarted={readyStates.start} onContinue={handleClick('music-start')} />
</div>

<div class:hide={!readyStates['music-start']}>
	<MusicStart onContinue={handleClick('stage-1')} />
</div>

<div class:hide={!readyStates['stage-1']}>
	<section use:scrollRef={'stage-1'}>
		<p>Ecosystem Stage 1/5</p>
		<ContinueButton scrollTo="stage-2" onClick={handleClick('stage-2')}>Continue</ContinueButton>
	</section>
</div>

<div class:hide={!readyStates['stage-2']}>
	<section use:scrollRef={'stage-2'}>
		<p>Ecosystem Stage 2/5</p>
		<ContinueButton scrollTo="stage-3" onClick={handleClick('stage-3')}>Continue</ContinueButton>
	</section>
</div>

<div class:hide={!readyStates['stage-3']}>
	<section use:scrollRef={'stage-3'}>
		<p>Ecosystem Stage 3/5</p>
		<ContinueButton scrollTo="stage-4" onClick={handleClick('stage-4')}>Continue</ContinueButton>
	</section>
</div>

<div class:hide={!readyStates['stage-4']}>
	<section use:scrollRef={'stage-4'}>
		<p>Ecosystem Stage 4/5</p>
		<ContinueButton scrollTo="stage-5" onClick={handleClick('stage-5')}>Continue</ContinueButton>
	</section>
</div>

<div class:hide={!readyStates['stage-5']}>
	<section use:scrollRef={'stage-5'}>
		<p>Ecosystem Stage 5/5</p>
		<ContinueButton scrollTo="stage-6" onClick={handleClick('stage-6')}>Continue</ContinueButton>
	</section>
</div>

<div class:hide={!readyStates['stage-6']}>
	<section use:scrollRef={'stage-6'}>
		<p>
			Final information about the EcoVoice Project. Amet velit Lorem amet irure duis nostrud non
			eiusmod labore ea dolor pariatur ex dolor. Qui consequat tempor occaecat mollit voluptate
			reprehenderit deserunt qui sunt cillum cillum. Laboris non aliquip proident sint commodo
			mollit incididunt eu reprehenderit aliqua consectetur ex magna commodo. Aliquip enim cillum
			laboris consequat exercitation incididunt deserunt qui ea. Exercitation enim duis incididunt
			reprehenderit fugiat non cupidatat sit cillum culpa anim culpa dolore.
		</p>
		<LinkButton href="https://www.ecovoiceproject.org">Go to the EcoVoice Project</LinkButton>
	</section>
</div>

<style>
	.hide {
		display: none;
	}

	p {
		max-width: 48rem;
		text-align: center;
	}
</style>
