<script>
    import { randString } from "../../scripts/functions";
    import { getEventsAction } from "../../scripts/utils.js";
    import { current_component } from "svelte/internal";
    import { createEventDispatcher } from "svelte";

    const dispatch = createEventDispatcher();
    const events = getEventsAction(current_component);

    export let active;
    
    let localClass;
    while (true) {
        let tempClass = "file-" + randString(5);
        if (document.getElementsByClassName(tempClass).length === 0) {
            localClass = tempClass;
            break;
        }
    }

    function toggle(){
        active = !active;
        dispatch('change', {active, "class": localClass});
    }

</script>

<style lang="less" global>
    @import "File.less";
</style>
<div class="{$$props.class ? $$props.class : ''} {localClass}" use:events>
    <input type="file">
</div>
