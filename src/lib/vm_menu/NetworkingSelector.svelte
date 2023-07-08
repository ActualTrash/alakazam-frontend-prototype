<script>
    export let network_data;
    import { SlideToggle } from '@skeletonlabs/skeleton';

    let automatic_networking = true;
</script>

<h4 class="h4">Networking Information</h4>
<SlideToggle name="auto-networking-toggle" active="bg-primary-500" bind:checked={automatic_networking}>
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
