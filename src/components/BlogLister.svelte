<script>
	import Error from '$components/Error.svelte';
	let getPosts = fetch('/posts.json').then((resp) => resp.json());
</script>

<div class="grid grid-cols-1 grid-rows-4 sm:grid-rows-2 sm:grid-cols-2 gap-8">
	<!-- We're using the await block here instead of a page endpoint to make the blog post loading asyncronous (won't hang up the entire page render) -->
	{#await getPosts}
		{#each Array(4) as item}
			<div class="bg-gray-300 dark:bg-gray-800 rounded-lg animate-pulse h-72 lg:h-48 w-full" />
		{/each}
	{:then data}
		{#each data.posts as post}
			<a href={post.link} target="_blank" rel="noopener">
				<div class="flex justify-center h-full">
					<div
						class="h-full flex flex-col lg:flex-row justify-start transition-all bg-gray-200 rounded-lg shadow-md lg:max-h-48 hover:bg-gray-100 dark:border-gray-700 dark:bg-gray-800 dark:hover:bg-gray-700"
					>
						<img
							class="w-full max-h-36 object-cover lg:w-48 lg:max-h-full rounded-t-lg lg:rounded-none lg:rounded-l-lg dark:brightness-75 aspect-square"
							src={post.uagb_featured_image_src.medium[0]}
							alt=""
						/>
						<div class="p-4 flex flex-col justify-start">
							<p
								class="mb-2 text-lg md:text-xl lg:text-2xl font-bold font-display text-gray-900 dark:text-white capitalize"
							>
								{@html post.title.rendered} <i class="fa-solid fa-arrow-right" />
							</p>
							<p class="mb-3 font-normal text-gray-700 dark:text-gray-400 overflow-hidden">
								{@html post.uagb_excerpt}
							</p>
						</div>
					</div>
				</div>
			</a>
		{/each}
	{:catch error}
		<Error message="{error}}" />
	{/await}
</div>
