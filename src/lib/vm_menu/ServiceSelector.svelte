<script>
    // Services
    export let selected_services = [];

    import { Accordion, AccordionItem, Autocomplete } from '@skeletonlabs/skeleton';

    // Figure out how to actually import this properly
    let service_icons = {
        'mysql': 'mysql.png',
        'postgresql': 'postgres.png', // https://wiki.postgresql.org/wiki/Logo
        'kubernetes': 'kubernetes.png', // https://github.com/kubernetes/kubernetes/blob/master/logo
    };

    const available_services = [
        { label: 'MySQL', value: 'mysql', keywords: 'db, database', meta: { icon: 'database' } },
        { label: 'PostgreSQL', value: 'postgresql', keywords: 'db, database, psql', meta: { icon: 'database' } },
        { label: 'Kubernetes', value: 'kubernetes', keywords: 'containers', meta: { icon: 'kubernetes' } },
    ];

    let inputChip = '';

    function removeService(service) {
        var index = selected_services.indexOf(service);
        if ( index > -1 ) {
            selected_services.splice(index, 1);
            selected_services = selected_services; // To tell Svelte to update the DOM
        }
    }
</script>

<div>
    <h4 class="h4">Available Services</h4>
    <input class="input pl-5 p-2 mb-3" type="search" name="service-search" bind:value={inputChip} placeholder="Search..." />
    <div class="card w-full mb-2 max-h-48 p-4 overflow-y-auto" tabindex="-1">
        <Autocomplete
            bind:input={inputChip}
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
                    <img src={service_icons[s]} alt="" class="service-icon m-1 w-5" />
                </svelte:fragment>
                <svelte:fragment slot="summary">{s}</svelte:fragment>
                    <svelte:fragment slot="content">
                    <p>Configuration stuff for {s} goes here</p>
                    <button on:click={removeService(s)} class="btn bg-error-500"><i class="fa-solid fa-trash mr-2"></i>Remove Service</button>
                    </svelte:fragment>
            </AccordionItem>
        {/each}
        </Accordion>
    {:else}
        <p>No services selected</p>
    {/if}
</div>
