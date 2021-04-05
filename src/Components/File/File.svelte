<script>
    import { randString } from "../../scripts/functions";
    import { getEventsAction } from "../../scripts/utils.js";
    import { current_component } from "svelte/internal";
    import { createEventDispatcher } from "svelte";
    import Button from "../Button/Button.svelte";

    const dispatch = createEventDispatcher();
    const events = getEventsAction(current_component);
    
    export let buttonTitle = "Открыть";
    export let fieldTitle = "Выберите файл";
    export let style = null;


    let localClass;
    while (true) {
        let tempClass = "file-" + randString(5);
        if (document.getElementsByClassName(tempClass).length === 0) {
            localClass = tempClass;
            break;
        }
    }

</script>

<style lang="less" global>
    @import "File.less";
</style>
<div {style} class="{$$props.class ? $$props.class : ''} {localClass}">
    <input id={localClass} type="file" use:events>
    <label for={localClass} style="z-index: 1;">
        <Button style="border-top-left-radius: 36px; border-bottom-left-radius: 36px; margin: 0;">{buttonTitle}</Button>
    </label>
    <div class="field">
        <div>
            {fieldTitle}
        </div>
    </div>
</div>
