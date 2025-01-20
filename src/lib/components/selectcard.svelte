<script lang="ts">
	import * as Card from '$lib/components/ui/card';
	import * as Select from '$lib/components/ui/select';

	export let required: boolean = false;
	export let selections: string[];
	export let title: string;
	export let desc: string;
	export let onSelect: (e: any) => void;
</script>

<Card.Root>
	<Card.Header>
		<Card.Title>{title} <span class="text-red-600">{required ? '*' : ''}</span></Card.Title>
		<Card.Description>{desc}</Card.Description>
	</Card.Header>
	<Card.Content>
		<Select.Root type="single" onValueChange={onSelect}>
			<Select.Trigger class="w-[200px]">Year</Select.Trigger>
			<Select.Content>
				{#each selections as option}
					<Select.Item value={option}>{option}</Select.Item>
				{/each}
			</Select.Content>
		</Select.Root>
	</Card.Content>
</Card.Root>
