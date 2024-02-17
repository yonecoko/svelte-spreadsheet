<script lang="ts">
    export let content:string|number = '';
    export let onChange = (updatedContent:string|number) => {};

    let editing = false;

    const onKeyDown = (event:any) => {
        if(['Enter', 'Escape'].includes(event.key)) {
            editing = false;
            if(event.key === 'Enter') {
                content = (event.target as HTMLInputElement).value;
                onChange(content)
            }
        }
    }

	const evaluateFormula = (exp:string) => {
	const sanitized = exp.slice(1).replace(/[^\=\%\+\*\/\/\(\)\/0-9]/g,'');
	return eval(sanitized)
	}

  </script>

  <td on:click={() => {editing = !editing}}>
    {#if editing}
    <input value={content} on:click|stopPropagation={() => {null}} on:keydown= {onKeyDown}>
    {:else}
	{#if content.toString().startsWith('=')}
	{evaluateFormula(content.toString())}
	{:else}
    {content}
	{/if}
{/if}
</td>

  <style>
    td{
        border: solid 1px;
    }
</style>