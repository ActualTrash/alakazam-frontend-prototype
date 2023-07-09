<script>
    /* An object containing a form schema in this format:
    {
        "field_one": "int",
        "field_two": "string",
        ... etc.
    }

    This will change in the future to support labeling, defaults, and if we need to support heterogenous types
    */
    export let schema;
    import { SlideToggle } from '@skeletonlabs/skeleton';

    let dummy;
</script>

{#if Object.entries(schema).length }
    {#each Object.entries(schema) as [name, type]}
        {#if type == 'int'}
            <label class="label">
                <p>{name}</p>
                <input bind:value={dummy} class="input pl-4 p-1" type="number" placeholder="0">
            </label>
        {:else if type == 'string'}
            <label class="label">
                <p>{name}</p>
                <input bind:value={dummy} class="input pl-4 p-1" type="text" placeholder="default string">
            </label>
        {:else if type == 'boolean'}
            <SlideToggle name="auto-networking-toggle" active="bg-primary-500" bind:checked={dummy}>
                {name}
            </SlideToggle>
        {:else}
            <p>Error: Unrecognized type {type}</p>
        {/if}
    {/each}
{:else}
    <h5 class="h5">This service has no configuration! 🎉</h5>
{/if}
