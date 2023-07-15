<script>
    // Services
    export let selected_services = [];

    import { Accordion, AccordionItem, Autocomplete } from '@skeletonlabs/skeleton';
    import DynamicForm from './DynamicForm.svelte';

    let search_text = '';

    function removeService(service) {
        var index = selected_services.indexOf(service);
        if ( index > -1 ) {
            selected_services.splice(index, 1);
            selected_services = selected_services; // To tell Svelte to update the DOM
        }
        // Delete the configuration for this service
        delete configs[service]; // weird bug with this line. Should be naturally resolved when we deal with defaults.
        //configs[service] = {};

        // Slight bug by allowing the dynamic form fill in the object with a blank one: if the user adds a
        // service but then never clicks into the menu, then their service won't show up in the configs. Again, this should
        // be resolved when we deal with defaults.
    }

    // temporary until we fetch from API
    import { schemas, defaults, service_icons, available_services } from './data.js';

    let configs = {};

    function capitalize(s) {
        return s.charAt(0).toUpperCase() + s.slice(1);
    }
</script>

<div>
    <h4 class="h4">Available Services</h4>
    <input class="input pl-5 p-2 mb-3" type="search" name="service-search" bind:value={search_text} placeholder="Search..." />
    <div class="card w-full mb-2 max-h-48 p-4 overflow-y-auto" tabindex="-1">
        <Autocomplete
            bind:input={search_text}
            options={available_services}
            denylist={selected_services}
            on:selection="{e => selected_services[selected_services.length] = e.detail.value}"
            />
    </div>
    <!-- Service configuration area -->
    <h4 class="h4">Service Configuration</h4>
    {#if selected_services.length}
        <Accordion>
        {#each selected_services as s}
            <AccordionItem autocollapse>
                <svelte:fragment slot="lead">
                    {#if service_icons[s]}
                        <img src={service_icons[s]} alt="" class="service-icon m-1 w-5" />
                    {:else}
                        <i class="fa-solid fa-database m-1 fa-lg"></i>
                    {/if}
                </svelte:fragment>
                <svelte:fragment slot="summary"><h5 class="h5">{s}</h5></svelte:fragment>
                    <svelte:fragment slot="content">
                        <DynamicForm schema={schemas[s]} bind:config={configs[s]}/>
                        <button on:click={removeService(s)} class="btn bg-error-500"><i class="fa-solid fa-trash mr-2"></i>Remove Service</button>
                    </svelte:fragment>
            </AccordionItem>
        {/each}
        </Accordion>
    {:else}
        <p>No services selected</p>
    {/if}
</div>
<button class="btn bg-warning-500" on:click={() => console.log(JSON.stringify(configs, null, 2))}>Debug: Print configs</button>
