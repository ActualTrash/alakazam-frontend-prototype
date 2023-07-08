<script lang="ts">
    // Props
    /** Exposes parent props to this component. */
    export let parent: any;
    export let onSubmit;
    export let vmid: int;

    // pass in data and have the sub variables be updated
    // changes to sub-vars should update vm_data

    import { modalStore, Accordion, AccordionItem } from '@skeletonlabs/skeleton';
    import ServiceSelector from './ServiceSelector.svelte';
    import OperatingSystemSelector from './OperatingSystemSelector.svelte';
    import NetworkingSelector from './NetworkingSelector.svelte';


    const formData = {};

    // We've created a custom submit function to pass the response and close the modal.
    function onFormSubmit(): void {
            // Maybe we should be using this to pass the response rather than a callback...
            if ($modalStore[0].response) $modalStore[0].response(formData);
            modalStore.close();

            onSubmit(vm_data_new);
    }

    // Base Classes
    const cBase = 'card p-4 w-modal shadow-xl space-y-4';
    const cHeader = 'text-2xl font-bold';
    const cFormSection = 'border border-surface-500 p-4 space-y-4 rounded-container-token';

    // Information
    let vm_name = 'VM-' + vmid; // make this vmid by default
    let n_copies = 1;

    // OS
    let os_flavor = 'linux';
    let os_template = 'ubuntuserver-jammy';

    // Networking
    let network_data = {
        'ip': '127.0.0.1/8',
        'dns_server': '127.0.0.1',
        'gateway': '127.0.0.1',
    };

    // Services
    let sel_services = [];

    $: vm_data_new = {
        'name': vm_name,
        'os': {
            'flavor': os_flavor,
            'template': os_template,
        },
        'network': network_data,
        'services': sel_services,
    };
</script>

<!-- @component Modal menu to create a VM -->

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
                            <OperatingSystemSelector bind:os_flavor={os_flavor} bind:os_template={os_template}/>
                        </section>
                    </svelte:fragment>
                </AccordionItem>

                <!-- Networking selection -->
                <AccordionItem autocollapse>
                    <svelte:fragment slot="lead">🌐</svelte:fragment>
                    <svelte:fragment slot="summary">Networking</svelte:fragment>
                    <svelte:fragment slot="content">
                        <section class="mt-5 {cFormSection}">
                            <NetworkingSelector bind:network_data={network_data} />
                        </section>
                    </svelte:fragment>
                </AccordionItem>

                <!-- Serivces selection -->
                <AccordionItem autocollapse>
                    <svelte:fragment slot="lead">🚀</svelte:fragment>
                    <svelte:fragment slot="summary">Services</svelte:fragment>
                    <svelte:fragment slot="content">
                        <section class="mt-5 {cFormSection}">
                            <ServiceSelector bind:selected_services={sel_services} />
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
