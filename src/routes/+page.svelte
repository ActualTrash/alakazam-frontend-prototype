<script>
    import VMCard from '../lib/VMCard.svelte';
    import CreateVMMenu from '../lib/CreateVMMenu.svelte';

    import { Modal, modalStore } from '@skeletonlabs/skeleton';

    let vms = [];
    function add_vm() {
        const c = {
            ref: CreateVMMenu,
            props: {
                onSubmit: createVM,
                vmid: vms.length, // race condition?
            },
        };

        const modal = {
            type: 'component',
            component: c,
        };

        modalStore.trigger(modal);
    }

    function createVM(name) {
        vms[vms.length] = name;
        console.log(vms);
    }
</script>

<div class="container mx-auto p-8 space-y-8">
    <h1 class="h1">Machines</h1>
    {#each vms as v, i}
        <VMCard vmname={v} vmid={i + 1} />
    {/each}
    <button on:click={add_vm} type="button" class="vm-add-btn btn variant-ghost p-4 m-2">
        Add VM
    </button>
</div>

<style>
    .vm-add-btn {
        display: inline-flex;
        min-width: 12em;
        min-height: 12em;
        align-items: start;
    }
</style>
