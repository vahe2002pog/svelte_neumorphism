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
    export let multiple = false;
    export let style = null;

    let localClass;
    while (true) {
        let tempClass = "file-" + randString(5);
        if (document.getElementsByClassName(tempClass).length === 0) {
            localClass = tempClass;
            break;
        }
    }

    function dropHandler(e) {
        let fileInput = document.getElementById(localClass);
        e.preventDefault();
        if (e.dataTransfer != null) {
            let files = [];
            for (let i = 0; i < e.dataTransfer.items.length; i++) {
                if (e.dataTransfer.items[i].kind === "file") {
                    let fileType = e.dataTransfer.items[i].type.split("/")[1];
                    if (fileInput.accept.includes(fileType) || fileInput.accept.length === 0) {
                        console.log(e.dataTransfer.items[i].getAsFile())
                        // if (i == 0) {
                        //     $(fileInput).val("");
                        // }
                        // let file = e.dataTransfer.items[i].getAsFile();
                        // files.push(file);
                        // if (!fileInput.multiple) {
                        //     break;
                        // }

                    }
                }
            }
            // fileInput.files = new FileListItems(files);
        }
    }

    function dragOverHandler(e) {
        e.preventDefault();
    }
</script>

<div {style} class="{$$props.class ? $$props.class : ''} {localClass}">
    <input id={localClass} {multiple} type="file" use:events />
    <label for={localClass} style="z-index: 1;">
        <Button
            style="border-top-left-radius: 36px; border-bottom-left-radius: 36px; margin: 0;"
            >{buttonTitle}</Button
        >
    </label>
    <div class="field" on:dragover={dragOverHandler} on:drop={dropHandler}>
        <div>
            {fieldTitle}
        </div>
    </div>
</div>

<style lang="less" global>
    @import "File.less";
</style>
