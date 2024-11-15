<script>
	import { page } from '$app/stores';
	import Listitem from '$lib/fonts/components/Listitem.svelte';
	import { goto } from '$app/navigation';
	let { data } = $props();
	let currentQuestion = $state(0);
	let selectedAnswer = $state('');
	let score = $state(0);
	let submitted = $state(false);

	function handleSubmit() {
		submitted = true;
		data.questions[currentQuestion].selectedAnswer = selectedAnswer;
		if (selectedAnswer === data.questions[currentQuestion].answer) {
			score++;
		}
	}

	function nextQuestion() {
		submitted = false;
		currentQuestion++;
		selectedAnswer = '';
	}

	function navigateToMenu() {
		goto('/');
	}
</script>

{#if currentQuestion < data.questions.length}
	{#each data.questions as question, index (question.question)}
		{#if index === currentQuestion}
			<p class="my-3">Question {currentQuestion + 1} of {data.questions.length}</p>
			<progress
				class="progress progress-primary w-full"
				value={currentQuestion + 1}
				max={data.questions.length}
			></progress>
			<h1 class="my-3">{question.question}</h1>
			{#each question.options as option, index (option)}
				<div class="my-3">
					{#key submitted}
						<Listitem
							title={option}
							{index}
							correctAnswer={question.selectedAnswer && question.answer === option}
							inncorrectlySelected={question.selectedAnswer === option &&
								question.answer !== option}
							focusAction={() => (selectedAnswer = option)}
							disabled={submitted}
						></Listitem>
					{/key}
				</div>
			{/each}
		{/if}
	{/each}

	{#if !submitted}
		<button class="btn btn-primary" onclick={handleSubmit} disabled={!selectedAnswer}>Submit</button
		>
	{:else}
		<button class="btn btn-primary" onclick={nextQuestion}>Next Question</button>
	{/if}
{:else}
	<h2>Game Over!</h2>
	<p>You scored {score} out of {data.questions.length} points!</p>
	<br />
	<button class="btn btn-primary" onclick={window.location.reload()}> Play again!</button>
	<br /><br />
	<button class="btn btn-primary" onclick={navigateToMenu}>Menu</button>
{/if}
