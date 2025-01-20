<script lang="ts">
	import { goto } from '$app/navigation';
	import Checkboxcard from '$lib/components/checkboxcard.svelte';
	import Introcard from '$lib/components/introcard.svelte';
	import Textcard from '$lib/components/textcard.svelte';
	import Button from '$lib/components/ui/button/button.svelte';

	let isLoading = false;
	const sheetId = '1Cp3m9Ldt0w_Ox_bkdek_desndGIrKGNLTKQSwo-6R8c';

	let name: string;
	let email: string;
	let phone: string;
	let agreed: boolean;
	let department: string;
	let registration: string;

	$: disabled = isLoading || !name || !phone || !email || !agreed;

	function addEntry() {
		if (disabled) return;
		isLoading = true;

		const data: Entry = {
			name: name || 'None',
			email: email || 'None',
			phone: phone || 'None'
		};

		fetch('/api/add', {
			method: 'POST',
			body: JSON.stringify({ spreadsheetId: sheetId, entry: data })
		})
			.then((res) => res.json())
			.then((res) => {
				goto('/success');
			})
			.catch((e) => console.log(e))
			.finally(() => {
				isLoading = false;
			});
	}

	function createForm() {
		fetch('/api/create', {
			method: 'POST'
		})
			.then((res) => res.json())
			.then((res) => {
				// goto('/success');
				console.log(res);
			})
			.catch((e) => console.log(e))
			.finally(() => {
				isLoading = false;
			});
	}
	function addPermission() {
		fetch('/api/add-permission', {
			method: 'POST',
			body: JSON.stringify({ email: '', spreadsheetId: sheetId })
		})
			.then((res) => res.json())
			.then((res) => {
				// goto('/success');
				console.log(res);
			})
			.catch((e) => console.log(e))
			.finally(() => {
				isLoading = false;
			});
	}
</script>

<div class="relative flex h-dvh w-full p-4">
	<div class="mx-auto flex w-full max-w-xl flex-col gap-4">
		<Introcard />
		<Textcard bind:text={name} title={'Name'} desc="Enter your Fullname" required={true} />
		<Textcard bind:text={email} title={'Email'} desc="Enter your Email" required={true} />
		<Textcard bind:text={phone} title={'Phone'} desc="Enter your Mobile number" required={true} />
		<Textcard
			bind:text={registration}
			title={'Registration'}
			desc="Enter your Registration number"
			required={true}
		/>
		<Textcard
			bind:text={department}
			title={'Department'}
			desc="Enter your Department"
			required={true}
		/>

		<Checkboxcard bind:checked={agreed} />
		<div>
			<Button
				onclick={addEntry}
				class="mb-4 w-full border border-transparent bg-gradient-to-r from-blue-950 via-blue-500 to-blue-950  disabled:opacity-100"
				{disabled}>{isLoading ? 'Please wait...' : 'Submit'}</Button
			>
		</div>
		<Button
			onclick={addPermission}
			class="mb-4 w-full border bg-gradient-to-r from-purple-800 via-indigo-600 to-blue-800  disabled:opacity-100"
			>{isLoading ? 'Please wait...' : 'Create'}</Button
		>
	</div>
</div>
