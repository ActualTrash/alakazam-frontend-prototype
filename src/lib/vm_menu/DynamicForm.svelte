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
    export let config = {};

    import { SlideToggle } from '@skeletonlabs/skeleton';
    import { slide } from 'svelte/transition';

    let uid = 0; // uid to uniquely identify list elements. Incremented for each element
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
            <!-- Look into https://svelte.dev/tutorial/group-inputs -->
            {#if config[name]}
                {#each config[name] as l,i (l.id) }
                    <div class="flex" transition:slide={{ duration: 100 }}>
                        <input bind:value={l.value} class="input pl-4 p-2" type="text" placeholder="default string">
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
                    config[name] = [...config[name], {id: uid++, value: 'default string'}]; // id used for list removal
                }
            }><i class="fa-solid fa-plus"></i>Add</button>
        {:else}
            <p>Error: Unrecognized type {type}</p>
        {/if}
    {/each}
{:else}
    <h5 class="h5">This service has no configuration! 🎉</h5>
{/if}

<button class="btn bg-primary-500" on:click={() => console.log(JSON.stringify(config, null, 2))}>Debug: Print config</button>
