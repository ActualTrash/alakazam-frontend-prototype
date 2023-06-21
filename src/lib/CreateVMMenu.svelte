<script lang="ts">
    // Props
    /** Exposes parent props to this component. */
    export let parent: any;
    export let onSubmit;
    export let vmid: int;

    import { modalStore, RadioGroup, RadioItem, ListBox, ListBoxItem, SlideToggle, Accordion, AccordionItem } from '@skeletonlabs/skeleton';

    const formData = {};

    // We've created a custom submit function to pass the response and close the modal.
    function onFormSubmit(): void {
            if ($modalStore[0].response) $modalStore[0].response(formData);
            modalStore.close();
            onSubmit(vm_name);
    }

    // Base Classes
    const cBase = 'card p-4 w-modal shadow-xl space-y-4';
    const cHeader = 'text-2xl font-bold';
    const cFormSection = 'border border-surface-500 p-4 space-y-4 rounded-container-token';

    // Information
    let vm_name = 'VM-' + vmid; // make this vmid by default

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

    let automatic_networking = true;
    let ip = '127.0.0.1/8';
    let dns_server = '127.0.0.1';
    let gateway = '127.0.0.1';
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
                                <p>No templates available for this OS flavor yet :(</p>
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
                            <p>Automatically generate networking information</p>
                            <SlideToggle active="bg-primary-500" bind:checked={automatic_networking} />

                            <label class="label">
                                <span>IP address</span>
                                <input bind:value={ip} disabled={automatic_networking} class="input pl-4 p-1" type="text" placeholder="192.168.1.1/24">
                            </label>
                            <label class="label">
                                <span>Default Gateway</span>
                                <input bind:value={gateway} disabled={automatic_networking} class="input pl-4 p-1" type="text" placeholder="192.168.1.1">
                            </label>
                            <label class="label">
                                <span>DNS Server</span>
                                <input bind:value={dns_server} disabled={automatic_networking} class="input pl-4 p-1" type="text" placeholder="192.168.1.1">
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
                            <h4 class="h4">Services</h4>
                        </section>
                    </svelte:fragment>
                </AccordionItem>
            </Accordion>
        </form>

        <footer class="modal-footer {parent.regionFooter}">
            <button class="btn {parent.buttonNeutral}" on:click={parent.onClose}>{parent.buttonTextCancel}</button>
            <button class="btn {parent.buttonPositive}" on:click={onFormSubmit}>Create</button>
        </footer>
    </div>
{/if}
