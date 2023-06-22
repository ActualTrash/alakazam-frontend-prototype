<script>
    export let vm_data;
    export let vmid;

    //import { popup } from '@skeletonlabs/skeleton';
    //import type { PopupSettings } from '@skeletonlabs/skeleton';

    let service_icons = {
        'mysql': 'linux.png',
        'postgresql': 'linux.png',
        'kubernetes': 'windows.png',
    };
</script>

<div class="vm card card-hover p-4 m-2">
    <div class="mb-3 flex justify-between">
        <span><b>{vm_data.name}</b></span>
        <img src={vmid % 3 ? "linux.png" : "windows.png"} alt="" class="w-8 os-icon"/>
    </div>

    <p>IP: {vm_data.network.ip}</p>

    <div class="service-icons">
        {#each vm_data.services as s}
            <img src={service_icons[s]} alt="" class="service-icon m-1 w-6" />
        {/each}
    </div>
</div>

<style>
    .vm {
        display: inline-block;
        min-width: 12em;
        min-height: 12em;
        align-items: start;
    }
    /*.os-icon {
        float: right;
    }*/

    .service-icons {
        position: bottom;
    }

    .service-icon {
        display: inline-block;
    }
</style>
