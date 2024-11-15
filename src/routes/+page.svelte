<script>
	import Listitem from '$lib/fonts/components/Listitem.svelte';
	export let data;
</script>

<h1>Welcome to the <br /><strong class="text-primary">Fronten Quiz!</strong></h1>
<p>Pick a subject to get started.</p>
<br />
<div class="flex flex-col gap-4">
	{#each data.quizzes as quiz, index (quiz.title)}
		<a href="/{quiz.title.toLowerCase()}"> <Listitem title={quiz.title} {index} /><br /></a>
	{/each}
</div>
