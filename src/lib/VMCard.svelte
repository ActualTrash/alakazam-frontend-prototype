<script>
    export let vm_data;
    export let vmid;

    import { popup } from '@skeletonlabs/skeleton';

    let service_icons = {
        'mysql': 'mysql.png',
        'postgresql': 'postgres.png', // https://wiki.postgresql.org/wiki/Logo
        'kubernetes': 'kubernetes.png', // https://github.com/kubernetes/kubernetes/blob/master/logo
    };


    const popupHover = {
	event: 'hover',
	target: 'os-flavor-' + vmid,
	placement: 'top'
    };

    function capitalize(s) {
        return s.charAt(0).toUpperCase() + s.slice(1);
    }
</script>

<div class="vm card card-hover p-4 m-2">
    <div class="mb-3 flex justify-between">
        <span><b>{vm_data.name}</b></span>
        <img src={vm_data.os.flavor == 'linux' ? "linux.png" : "windows.png"} alt="" class="w-8 os-icon [&>*]:pointer-events-none" use:popup={popupHover}/>
        <div class="card p-4 variant-filled-surface instant-popup" data-popup="os-flavor-{vmid}">
            <p>{capitalize(vm_data.os.flavor)}</p>
            <div class="arrow variant-filled-surface" />
        </div>
    </div>

    <p>OS: {vm_data.os.template}</p>
    <p>IP: {vm_data.network.ip}</p>

    <br>
    <br>

    <!-- Service icon tray -->
    <div class="service-icons">
        {#each vm_data.services as s, i}
            <img src={service_icons[s]} alt="" class="service-icon m-1 w-6 [&>*]:pointer-events-none" use:popup={{
                    event: 'hover',
                    target: 'service-icon-' + vmid + '-' + i,
                    placement: 'top'
                }
            } />
            <!-- hover popup -->
            <div class="card p-4 variant-filled-surface instant-popup" data-popup="service-icon-{vmid}-{i}">
                <p>{capitalize(s)}</p>
                <div class="arrow variant-filled-surface" />
            </div>
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
        /*display: inline-block;
        align-self: flex-end;
        /*position: absolute;
        bottom: 0;
        left: 0;*/
    }

    .service-icon {
        display: inline-block;
    }

    .instant-popup {
        transition-duration: 0ms;
    }
</style>
