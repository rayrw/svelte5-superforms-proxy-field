<script lang="ts">
	import { z } from 'zod';
	import { superForm, defaults } from 'sveltekit-superforms/client';
	import { zod } from 'sveltekit-superforms/adapters';
	import Input from './Input.svelte';

	const schema = z.object({
		email: z.string().email().min(1)
	});

	const _superForm = superForm(defaults(zod(schema)), {
		validators: zod(schema),
		SPA: true,
		validationMethod: 'onblur'
	});
	const { enhance, errors, form } = _superForm;
	$inspect($errors);
</script>

<h1>Email form</h1>

<form use:enhance>
	<div>
		<label>
			Email (oninput + value props)
			<input
				value={$form.email}
				oninput={(e) => {
					$form.email = e.currentTarget.value;
				}}
			/>
		</label>

		{#if $errors.email}
			<p>{$errors.email[0]}</p>
		{/if}
	</div>

	<div>
		<label>
			Email (value binding)
			<input bind:value={$form.email} />
		</label>

		{#if $errors.email}
			<p>{$errors.email[0]}</p>
		{/if}
	</div>

	<Input superForm={_superForm} field="email" />
</form>
