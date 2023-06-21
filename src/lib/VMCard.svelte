<script>
    export let vmid;
    export let vmname;
</script>

<div class="vm card card-hover p-4 m-2">
    <div >
        <span>{vmname}</span>
        <img src={vmid % 3 ? "linux.png" : "windows.png"} class="w-8 os-icon"/>
    </div>
</div>

<style>
    .vm {
        display: inline-block;
        min-width: 12em;
        min-height: 12em;
        align-items: start;
    }
    .os-icon {
        float: right;
    }
</style>
