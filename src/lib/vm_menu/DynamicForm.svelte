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

    let config = {};
</script>

{#if Object.entries(schema).length }
    {#each Object.entries(schema) as [name, type]}
        {#if type == 'int'}
            <label class="label">
                <p>{name}</p>
                <input bind:value={config[name]} class="input pl-4 p-1" type="number" placeholder="0">
            </label>
        {:else if type == 'string'}
            <label class="label">
                <p>{name}</p>
                <input bind:value={config[name]} class="input pl-4 p-1" type="text" placeholder="default string">
            </label>
        {:else if type == 'boolean'}
            <SlideToggle name="auto-networking-toggle" active="bg-primary-500" bind:checked={config[name]}>
                {name}
            </SlideToggle>
        {:else if type == 'list'}
            <p>{name}</p>
            {#if config[name]}
                {#each config[name] as l,i}
                    <div class="flex">
                        <input bind:value={l} class="input pl-4 p-1" type="text" placeholder="default string">
                        <button class="btn bg-primary-500 ml-2" on:click={ () => {config[name].splice(i, 1); config = config} }>
                            <i class="fa-solid fa-minus">
                            <!--<i class="fa-solid fa-trash">-->
                        </button>
                    </div>
                {/each}
            {/if}
            <button class="btn bg-primary-500" on:click={
                () => {
                    if (!config[name]) {
                        config[name] = [];
                    }
                    config[name] = [...config[name], 'default string'];
                }
            }><i class="fa-solid fa-plus"></i>Add</button>
        {:else}
            <p>Error: Unrecognized type {type}</p>
        {/if}
    {/each}
{:else}
    <h5 class="h5">This service has no configuration! 🎉</h5>
{/if}

<button class="btn bg-primary-500" on:click={() => console.log(config)}>Debug: Print config</button>
