<script lang="ts">
	import * as Card from '$lib/components/ui/card/index.js';
	import * as m from '$lib/paraglide/messages.js';

	let { open = false, onClose = () => {}, muteSound = $bindable(false) } = $props();

	const handleClose = () => {
		onClose();
	};

	const handleKeydown = (e: KeyboardEvent) => {
		if (e.key === 'Escape') {
			handleClose();
		}
	};

	$effect(() => {
		if (open) {
			document.addEventListener('keydown', handleKeydown);
			return () => {
				document.removeEventListener('keydown', handleKeydown);
			};
		}
	});
</script>

{#if open}
	<div
		class="fixed inset-0 z-50 flex items-center justify-center bg-black/50"
		onclick={handleClose}
	>
		<div
			class="mx-4 w-full max-w-md rounded-lg border bg-background shadow-lg"
			onclick={(e) => e.stopPropagation()}
		>
			<Card.Root class="border-0 shadow-none">
				<Card.Header class="flex flex-row items-center justify-between space-y-0 pb-2">
					<Card.Title class="text-lg">{m.advanced_settings()}</Card.Title>
					<button
						onclick={handleClose}
						class="rounded-sm opacity-70 ring-offset-background transition-opacity hover:opacity-100 focus:ring-2 focus:ring-ring focus:ring-offset-2 focus:outline-none disabled:pointer-events-none data-[state=open]:bg-accent data-[state=open]:text-muted-foreground"
					>
						<svg
							xmlns="http://www.w3.org/2000/svg"
							width="24"
							height="24"
							viewBox="0 0 24 24"
							fill="none"
							stroke="currentColor"
							stroke-width="2"
							stroke-linecap="round"
							stroke-linejoin="round"
							class="h-4 w-4"
						>
							<path d="M18 6L6 18"></path>
							<path d="M6 6l12 12"></path>
						</svg>
						<span class="sr-only">Close</span>
					</button>
				</Card.Header>
				<Card.Content class="space-y-4">
					<div class="flex items-center space-x-2">
						<input
							type="checkbox"
							id="mute-sound"
							bind:checked={muteSound}
							class="h-4 w-4 rounded border-gray-300 text-primary focus:ring-primary"
						/>
						<div class="grid gap-1.5 leading-none">
							<label
								for="mute-sound"
								class="text-sm leading-none font-medium peer-disabled:cursor-not-allowed peer-disabled:opacity-70"
							>
								{m.mute_sound()}
							</label>
							<p class="text-xs text-muted-foreground">{m.mute_sound_description()}</p>
						</div>
					</div>
				</Card.Content>
			</Card.Root>
		</div>
	</div>
{/if}
