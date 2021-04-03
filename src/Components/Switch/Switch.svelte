<script>
    import { randString } from "../../scripts/functions";

    export let checked;
    
    let localClass;
    while (true) {
        let tempClass = "switch-" + randString(5);
        if (document.getElementsByClassName(tempClass).length === 0) {
            localClass = tempClass;
            break;
        }
    }

    function toggle(){
        checked = !checked;
    }

</script>

<style lang="less" global>
    @import "Switch.less";
</style>
<div class="switch {$$props.class ? $$props.class : ''} {localClass}{checked ? " active": ""}" on:click={toggle}>
    <div class="track">
        <div class="thumb"></div>
    </div>
</div>
