<script lang="ts">
    // Props
    /** Exposes parent props to this component. */
    export let parent: any;
    export let onSubmit;
    export let vmid: int;

    import { modalStore, RadioGroup, RadioItem, ListBox, ListBoxItem, SlideToggle, Accordion, AccordionItem, InputChip, Autocomplete } from '@skeletonlabs/skeleton';
    import type { AutocompleteOption } from '@skeletonlabs/skeleton';

    // Figure out how to actually import this properly
    let service_icons = {
        'mysql': 'linux.png',
        'postgresql': 'linux.png',
        'kubernetes': 'windows.png',
    };

    const formData = {};

    // We've created a custom submit function to pass the response and close the modal.
    function onFormSubmit(): void {
            // Maybe we should be using this to pass the response rather than a callback...
            if ($modalStore[0].response) $modalStore[0].response(formData);
            modalStore.close();

            const vm_data = {
                'name': vm_name,
                'os': os_template,
                'network': network_data,
                'services': selected_services,
            };

            onSubmit(vm_data);
    }

    // Base Classes
    const cBase = 'card p-4 w-modal shadow-xl space-y-4';
    const cHeader = 'text-2xl font-bold';
    const cFormSection = 'border border-surface-500 p-4 space-y-4 rounded-container-token';

    // Information
    let vm_name = 'VM-' + vmid; // make this vmid by default
    let n_copies = 1;

    // OS flavors
    const os_flavors = {
        'linux': {
            displayName: 'Linux',
            templates: [
                {templateName: 'ubuntuserver-jammy', displayName: 'Ubuntu Server (Jammy)'},
                {templateName: 'debian-bullseye', displayName: 'Debian (Bullseye)'},
                {templateName: 'debian-buster', displayName: 'Debian (Buster)'},
                {templateName: 'debian-stretch', displayName: 'Debian (Stretch)'},
                {templateName: 'kali', displayName: 'Kali Linux'},
            ]
        },
        'windows': {
            displayName: 'Windows',
            templates: [
                {templateName: 'windows-2012', displayName: 'Windows Server 2012'},
            ]
        },
        'firewall': {
            displayName: 'Firewall',
            templates: [],
        },
        'hypervisor': {
            name: 'hypervisor',
            displayName: 'Hypervisor',
            templates: [],
        },
    };

    let os_flavor = Object.entries(os_flavors)[0][0]; // Get key of first entry in the object
    $: os_template = os_flavors[os_flavor].templates.length ? os_flavors[os_flavor].templates[0].templateName : undefined;

    // Networking
    let automatic_networking = true;

    let network_data = {
        'ip': '127.0.0.1/8',
        'dns_server': '127.0.0.1',
        'gateway': '127.0.0.1',
    }


    // Services
    let selected_services = [];

    const available_services: AutocompleteOption[] = [
            { label: 'MySQL', value: 'mysql', keywords: 'db, database', meta: { icon: 'database' } },
            { label: 'PostgreSQL', value: 'postgresql', keywords: 'db, database, psql', meta: { icon: 'database' } },
            { label: 'Kubernetes', value: 'kubernetes', keywords: 'containers', meta: { icon: 'kubernetes' } },
    ];
				

    let inputChip = '';

    function onInputChipSelect(e: any): void {
    }
</script>

<!-- @component This example creates a simple form modal. -->

{#if $modalStore[0]}
    <div class="modal-example-form {cBase}">
        <header class={cHeader}>Create VM</header>
        <article>Select VM options</article>
        <form form="modal-form">
            <Accordion>
                <!-- General information -->
                <AccordionItem open autocollapse>
                    <svelte:fragment slot="lead">📕</svelte:fragment>
                    <svelte:fragment slot="summary">General</svelte:fragment>
                    <svelte:fragment slot="content">
                        <section class="mt-5 {cFormSection}">
                            <label class="label">
                                <span>Virtual Machine Name</span>
                                <input bind:value={vm_name} class="input pl-4 p-1" type="text" placeholder="a-really-awesome-name">
                            </label>
                            <label class="label">
                                <p>Number of copies to make</p>
                                <span><small>Names will be appended with numbers starting from 0</small></span>
                                <input disabled bind:value={n_copies} class="input pl-4 p-1" type="text" placeholder="a-really-awesome-name">
                            </label>
                        </section>
                    </svelte:fragment>
                </AccordionItem>

                <!-- OS Selection -->
                <AccordionItem autocollapse>
                    <svelte:fragment slot="lead">🖥️</svelte:fragment>
                    <svelte:fragment slot="summary">Operating System</svelte:fragment>
                    <svelte:fragment slot="content">
                        <section class={cFormSection}>

                            <!-- Flavor selection -->
                            <h4 class="h4">OS Flavor</h4>
                            <RadioGroup active="variant-ghost-primary">
                                {#each Object.entries(os_flavors) as [name, flavor]}
                                    <RadioItem bind:group={os_flavor} value={name}>{flavor.displayName}</RadioItem>
                                {/each}
                            </RadioGroup>

                            <!-- Template selection -->
                            <h4 class="h4">OS Template</h4>
                            {#if os_flavors[os_flavor].templates.length }
                                <ListBox active="variant-ghost-primary">
                                    {#each os_flavors[os_flavor].templates as templ}
                                        <ListBoxItem bind:group={os_template} value={templ.templateName}>{templ.displayName}</ListBoxItem>
                                    {/each}
                                </ListBox>
                            {:else}
                                <p>No templates available for this OS flavor yet 😕</p>
                            {/if}
                        </section>
                    </svelte:fragment>
                </AccordionItem>


                <!-- Networking selection -->
                <AccordionItem autocollapse>
                    <svelte:fragment slot="lead">🌐</svelte:fragment>
                    <svelte:fragment slot="summary">Networking</svelte:fragment>
                    <svelte:fragment slot="content">
                        <section class="mt-5 {cFormSection}">
                            <h4 class="h4">Networking Information</h4>
                            <SlideToggle active="bg-primary-500" bind:checked={automatic_networking}>
                                Automatically generate networking information
                            </SlideToggle>

                            <label class="label">
                                <span>IP address</span>
                                <input bind:value={network_data.ip} disabled={automatic_networking} class="input pl-4 p-1" type="text" placeholder="192.168.1.1/24">
                            </label>
                            <label class="label">
                                <span>Default Gateway</span>
                                <input bind:value={network_data.gateway} disabled={automatic_networking} class="input pl-4 p-1" type="text" placeholder="192.168.1.1">
                            </label>
                            <label class="label">
                                <span>DNS Server</span>
                                <input bind:value={network_data.dns_server} disabled={automatic_networking} class="input pl-4 p-1" type="text" placeholder="192.168.1.1">
                            </label>

                        </section>
                    </svelte:fragment>
                </AccordionItem>

                <!-- Serivces selection -->
                <AccordionItem autocollapse>
                    <svelte:fragment slot="lead">🚀</svelte:fragment>
                    <svelte:fragment slot="summary">Services</svelte:fragment>
                    <svelte:fragment slot="content">
                        <section class="mt-5 {cFormSection}">
                            <h4 class="h4">Available Services</h4>
                            <InputChip bind:input={inputChip} whitelist="{available_services.map(a => a.value)}" bind:value={selected_services} name="chips" />
                            <div class="card w-full max-h-48 p-4 overflow-y-auto" tabindex="-1">
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
                                                <img src={service_icons[s]} alt="" class="service-icon m-1 w-4" />
                                            </svelte:fragment>
                                            <svelte:fragment slot="summary">{s}</svelte:fragment>
                                            <svelte:fragment slot="content">
                                                Configuration stuff for {s} goes here
                                            </svelte:fragment>
                                        </AccordionItem>
                                    {/each}
                                </Accordion>
                            {:else}
                                <p>No services selected</p>
                            {/if}

                        </section>
                    </svelte:fragment>
                </AccordionItem>
            </Accordion>
        </form>

        <footer class="modal-footer {parent.regionFooter}">
            <button class="btn {parent.buttonNeutral}" on:click={parent.onClose}>{parent.buttonTextCancel}</button>
            <button class="btn bg-primary-500 {parent.buttonPositive}" on:click={onFormSubmit}>Create</button>
        </footer>
    </div>
{/if}
