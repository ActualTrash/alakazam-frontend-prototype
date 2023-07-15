<script>
    export let os_flavor;
    export let os_template;

    // OS flavors
    import { RadioGroup, RadioItem, ListBox, ListBoxItem, SlideToggle, Accordion, AccordionItem, InputChip, Autocomplete } from '@skeletonlabs/skeleton';
    import { fade, slide, scale } from 'svelte/transition';
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

    // Defaults
    os_flavor = Object.entries(os_flavors)[0][0]; // Get key of first entry in the object
    $: os_template = os_flavors[os_flavor].templates.length ? os_flavors[os_flavor].templates[0].templateName : null;
</script>

<!-- Flavor selection -->
<h4 class="h4">OS Flavor</h4>
<RadioGroup active="variant-ghost-primary">
    {#each Object.entries(os_flavors) as [name, flavor]}
        <RadioItem bind:group={os_flavor} name="flavor_selector" value={name}>{flavor.displayName}</RadioItem>
    {/each}
</RadioGroup>

<!-- Template selection -->
<h4 class="h4">OS Template</h4>
{#if os_flavors[os_flavor].templates.length }
    <ListBox active="variant-ghost-primary" >
        {#each os_flavors[os_flavor].templates as templ}
            <div transition:slide={{ duration: 100 }}> <!-- This is kind of buggy. Especially when opening the accordian and switching to a flavor with no templates available yet -->
                <ListBoxItem bind:group={os_template} name= "template_selector" value={templ.templateName}>{templ.displayName}</ListBoxItem>
            </div>
        {/each}
    </ListBox>
{:else}
    <p>No templates available for this OS flavor yet 😕</p>
{/if}
