<script lang="ts">
	import { fetchAllStories, postStory, postStoryCover } from '../../server';
	import { loggedAs, stories } from '../store';
	import { onDestroy } from 'svelte';
	import { Textarea, Label, Select, Input, Fileupload } from 'flowbite-svelte';

	let selected: any;
	let dropDownCategories = [
		{ value: 'workplace', name: 'workplace' },
		{ value: 'family', name: 'family' },
		{ value: 'romance', name: 'romance' },
		{ value: 'friends', name: 'friends' },
		{ value: 'pets', name: 'pets' },
		{ value: 'general', name: 'general' }
	];

	let newStory = {
		title: '',
		username: $loggedAs,
		body: '',
		img_url: '',
		votes: 0,
		category_name: selected
	};
	let cover: File;

	let fileuploadprops = {
		id: 'story_cover'
	};


	const unsubscribe = loggedAs.subscribe((newUser) => {
		newStory.username = newUser;
	});
	onDestroy(unsubscribe);

	async function handleSubmit() {
		let coverUrl = `https://jxdsmvzqgqofgdetgfha.supabase.co/storage/v1/object/public/stories/`;
		newStory.category_name = selected;
		const urlPart = await postStoryCover(cover);
		coverUrl += urlPart;
		newStory.img_url = coverUrl;
		await postStory(newStory);
		stories.update((newStory) => [...newStory]);
		newStory.body = '';
		newStory.title = '';
		newStory.img_url = '';
		selected = '';
	}

	function handleFileSelection(e: any) {
		cover = e.target.files[0];
	}

	let renderStories: any = [];

	const unsubscribe2 = stories.subscribe(() => {
		fetchAllStories().then((data) => {
			renderStories = data!;
		});
	});

	onDestroy(unsubscribe2);
</script>

<form action="" on:submit|preventDefault={handleSubmit} class="w-auto">
	<div>
		<Label for="title" class="pb-2 my-2">Title</Label>
		<Input
			type="text"
			id="title"
			size="lg"
			placeholder="title"
			bind:value={newStory.title}
			required
		/>
	</div>
	<Label for="story" class="block mb-2 my-2">Story</Label>
	<Textarea id="story" class="w-full" bind:value={newStory.body} required />

	<Label class="pb-2">Upload file</Label>
	<Fileupload {...fileuploadprops} on:change={handleFileSelection} required/>
	<Label class="pb-2 my-2"
		>Select a Category
		<Select class="form-select mt-2" items={dropDownCategories} bind:value={selected} required />
	</Label>
	<button
		type="submit"
		class="text-white bg-gradient-to-r from-purple-400 via-purple-500 to-purple-600 hover:bg-gradient-to-br focus:ring-4 focus:outline-none focus:ring-purple-300 dark:focus:ring-purple-800 font-medium rounded-lg text-sm px-2 py-1.5 text-center mr-2 mb-2 my-3 w-full"
		>Add Story</button
	>
</form>
