<script lang="ts">
	import { type SuperForm, formFieldProxy } from 'sveltekit-superforms/client';
	import type { FormPathLeaves, FormPathType } from 'sveltekit-superforms';

	type T = $$Generic<Record<string, unknown>>;

	interface Props {
		superForm: SuperForm<T, unknown>;
		field: FormPathLeaves<T>;
	}

	let { superForm, field }: Props = $props();

	let { value, errors } = formFieldProxy(superForm, field);

	$inspect($errors);
	$inspect($value);
</script>

<div>
	<label>
		Email (proxy + oninput + value props)
		<input
			value={$value}
			oninput={(e) => {
				$value = e.currentTarget.value as FormPathType<T, FormPathLeaves<T>>;
			}}
		/>
	</label>
</div>
<div>
	<label>
		Email (proxy + value binding)
		<input bind:value={$value} />
	</label>
</div>
{#if $errors}
	<p>{$errors[0]}</p>
{/if}
