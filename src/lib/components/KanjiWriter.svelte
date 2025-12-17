<script lang="ts">
	import { onMount, onDestroy } from 'svelte';
	import HanziWriter from 'hanzi-writer';

	let {
		character,
		size = 280,
		showOutline = true,
		showCharacter = false,
		strokeAnimationSpeed = 1,
		delayBetweenStrokes = 300,
		onComplete = () => {},
		onCorrectStroke = () => {},
		onMistake = () => {}
	}: {
		character: string;
		size?: number;
		showOutline?: boolean;
		showCharacter?: boolean;
		strokeAnimationSpeed?: number;
		delayBetweenStrokes?: number;
		onComplete?: () => void;
		onCorrectStroke?: (data: any) => void;
		onMistake?: (data: any) => void;
	} = $props();

	let container: HTMLDivElement;
	let writer: HanziWriter | null = null;
	let quizActive = $state(false);
	let mounted = false;
	let currentCharacter = '';

	onMount(() => {
		console.log('[KanjiWriter] onMount, character:', character);
		mounted = true;
		if (container && character) {
			createWriter();
		}
	});

	onDestroy(() => {
		if (writer) {
			writer.cancelQuiz();
		}
	});

	function createWriter() {
		console.log('[KanjiWriter] createWriter, mounted:', mounted);
		if (!mounted || !container || !character) return;

		if (writer) {
			writer.cancelQuiz();
		}

		container.innerHTML = '';

		writer = HanziWriter.create(container, character, {
			width: size,
			height: size,
			padding: 5,
			showOutline,
			showCharacter,
			strokeAnimationSpeed,
			delayBetweenStrokes,
			strokeColor: '#333',
			outlineColor: '#ddd',
			drawingColor: '#333',
			drawingWidth: 8,
			showHintAfterMisses: 2,
			highlightOnComplete: true,
			charDataLoader: async (char: string) => {
				const encoded = encodeURIComponent(char); const url = 'https://cdn.jsdelivr.net/npm/hanzi-writer-data@2.0/' + encoded + '.json'; console.log('[KanjiWriter] Fetching:', url);
				const res = await fetch(url); return res.json();
			}
		});
		console.log('[KanjiWriter] writer created:', !!writer);
	}

	$effect(() => {
		if (character === currentCharacter) return;
		currentCharacter = character;
		if (mounted && container && character) {
			createWriter();
			quizActive = false;
		}
	});

	export function animateStroke(strokeNum: number) {
		writer?.animateStroke(strokeNum);
	}

	export function animateCharacter() {
		console.log('[KanjiWriter] animateCharacter');
		return new Promise<void>((resolve) => {
			writer?.animateCharacter({
				onComplete: () => resolve()
			});
		});
	}

	export function startQuiz() {
		console.log('[KanjiWriter] startQuiz, writer:', !!writer);
		if (!writer) {
			console.error('[KanjiWriter] writer is null!');
			return;
		}
		quizActive = true;
		writer.quiz({
			onCorrectStroke: (data) => {
				console.log('[KanjiWriter] correct stroke');
				onCorrectStroke(data);
			},
			onMistake: (data) => {
				console.log('[KanjiWriter] mistake');
				onMistake(data);
			},
			onComplete: (data) => {
				console.log('[KanjiWriter] complete');
				quizActive = false;
				onComplete();
			}
		});
	}

	export function cancelQuiz() {
		quizActive = false;
		writer?.cancelQuiz();
	}

	export function showHint() {
		writer?.showHint();
	}

	export function hideCharacter() {
		writer?.hideCharacter();
	}

	export function showCharacterFn() {
		writer?.showCharacter();
	}

	export function isQuizActive(): boolean {
		return quizActive;
	}
</script>

<div
	bind:this={container}
	class="kanji-writer rounded-2xl border-4 border-amber-200 bg-white"
	style="width: {size}px; height: {size}px;"
></div>

<style>
	.kanji-writer {
		display: flex;
		align-items: center;
		justify-content: center;
	}
	.kanji-writer :global(svg) {
		display: block;
	}
</style>
