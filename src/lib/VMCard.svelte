<script>
    export let vmid;
</script>

<div class="vm card card-hover p-4 m-2">
    <div >
        <span>VM number {vmid}</span>
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
