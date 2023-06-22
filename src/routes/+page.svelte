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

    function createVM(vm_data) {
        vms[vms.length] = vm_data;
        console.log(vms);
    }
</script>

<div class="container mx-auto p-8 space-y-8">
    <h1 class="h1">Machines</h1>
    <div class="flex">
    {#each vms as v, i}
        <VMCard vm_data={v} vmid={i + 1} />
    {/each}
    <button on:click={add_vm} type="button" class="vm-add-btn btn variant-ghost p-4 m-2">
        Add VM
    </button>
    </div>
</div>

<style>
    .vm-add-btn {
        min-width: 12em;
        min-height: 12em;
        align-items: start;
    }
</style>
