<script>
    // Services
    export let selected_services = [];

    import { Accordion, AccordionItem, Autocomplete } from '@skeletonlabs/skeleton';
    import DynamicForm from './DynamicForm.svelte';

    // Figure out how to actually import this properly
    let service_icons = {
        'mysql': 'mysql.png',
        'postgresql': 'postgres.png', // https://wiki.postgresql.org/wiki/Logo
        'kubernetes': 'kubernetes.png', // https://github.com/kubernetes/kubernetes/blob/master/logo
    };

    const available_services = [
        { label: 'MySQL', value: 'mysql', keywords: 'db, database', meta: { icon: 'database' } },
        { label: 'Bind9', value: 'bind9', keywords: 'dns, bind', meta: { icon: 'database' } },
        //{ label: 'PostgreSQL', value: 'postgresql', keywords: 'db, database, psql', meta: { icon: 'database' } },
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

    const schemas = {"mysql": {"mysql_password": "string", "MySQLDebug": "boolean", "MySQLZip": "string"}, "general-linux": {"dns_server": "list", "default_users": "list", "default_admins": "list", "default_password": "string"}, "sliver-server": {"msf_install_directory": "string", "metasploit_version": "string", "sliver_gpg_key_id": "string", "sliver_version": "string"}, "bind9": {"upstream_dns_forwarders": "list", "domain": "string", "dns_a_records": "list"}, "winrm": {"WinrmConfigSSL": "int", "WinrmDebug": "boolean"}, "domaincontroller": {"domain": "string", "default_password": "string", "upstream_dns_forwards": "list", "domain_users": "list", "domain_admins": "list"}, "smb": {"SMBShareName": "string", "SMBSharePath": "string", "SMBScoreFileContent": "string", "SMBScoreFileName": "string", "SMBDebug": "boolean"}, "kubernetes-node": {}, "general-windows": {"default_users": "list", "default_password": "string"}, "kubernetes-worker": {}, "kubernetes": {}, "sliver-client": {}, "jumpbox": {}, "scoreboard": {"scoreboard_lan": "int"}, "domainjoin": {"DomainJoinDebug": "boolean", "domain": "string", "DomainJoinUsername": "string", "DomainJoinPassword": "string", "DomainJoinDNSIP": "string"}, "kubernetes-control": {"pod_network_cidr": "string"}, "ftp-bsd": {"ftp_username": "string", "ftp_password": "string", "ftp_filename": "string", "ftp_file_content": "string"}, "iis": {"IISDebug": "boolean", "IISConfigSSL": "int", "iis_web_root": "string"}, "rdp": {"RDPDebug": "boolean", "RDPUsers": "list"}, "nfs": {"nfs_export_name": "string", "nfs_export_path": "string", "nfs_file_content": "string", "nfs_file_path": "string", "nfs_debug": "boolean"}, "zencart": {"zencart_db_server": "string", "zencart_db_username": "string", "zencart_db_password": "string", "zencart_db": "string"}, "hmail": {"HmailInstaller": "string", "HmailDebug": "boolean", "domain": "string"}, "pfsense": {"default_password": "string"}, "nfs-client": {"nfs_server": "string", "nfs_share": "string", "local_mount": "string"}, "docker": {}, "vsftp": {"ftp_username": "string", "default_password": "string", "ftp_filename": "string", "ftp_file_content": "string"}};

    function capitalize(s) {
        return s.charAt(0).toUpperCase() + s.slice(1);
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
                    {#if service_icons[s]}
                        <img src={service_icons[s]} alt="" class="service-icon m-1 w-5" />
                    {:else}
                        <i class="fa-solid fa-database m-1 fa-lg"></i>
                    {/if}
                </svelte:fragment>
                <svelte:fragment slot="summary"><h5 class="h5">{s}</h5></svelte:fragment>
                    <svelte:fragment slot="content">
                        <DynamicForm schema={schemas[s]} />
                        <button on:click={removeService(s)} class="btn bg-error-500"><i class="fa-solid fa-trash mr-2"></i>Remove Service</button>
                    </svelte:fragment>
            </AccordionItem>
        {/each}
        </Accordion>
    {:else}
        <p>No services selected</p>
    {/if}
</div>
