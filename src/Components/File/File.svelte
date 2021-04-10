<script>
    import { randString } from "../../scripts/functions";
    import { FileListItems } from "../../scripts/FileListItems";
    import { isValid } from "../../scripts/FileValidate";
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
    export let files = null;
    export let accept = "*.*";

    let localClass;
    while (true) {
        let tempClass = "file-" + randString(5);
        if (document.getElementsByClassName(tempClass).length === 0) {
            localClass = tempClass;
            break;
        }
    }

    function getFiles(items) {
        let tempFiles = [];
        for (let i = 0; i < items.length; i++) {
            let item = items[i];
            if (item.kind === "file") {
                if (isValid(accept, item.type)) {
                    tempFiles.push(item.getAsFile());
                    if (!multiple) {
                        break;
                    }
                }
            }
        }
        return tempFiles;
    }

    function dropHandler(e) {
        e.preventDefault();
        let items = Object.values(e?.dataTransfer?.items);
        files = new FileListItems(getFiles(items));
        setTitle();
    }
    
    function dragOverHandler(e) {
        e.preventDefault();
        let items = Object.values(e?.dataTransfer?.items);
        if (getFiles(items).length > 0) {
            e.dataTransfer.dropEffect = "move";
        } else {
            e.dataTransfer.dropEffect = "none";
            e.stopPropagation();
        }
    }
    
    function fileInputChange(e) {
        files = e.target.files;
        setTitle();
    }

    function setTitle(){
        if(multiple){
            fieldTitle = "Выбрано файлов: " + files.length;
        }
        else{
            fieldTitle = files[0].name;
        }
    }
</script>

<style lang="less" global>
    @import "File.less";
</style>

<div
    {style}
    class="{$$props.class ? $$props.class : ''} {localClass}"
    use:events>
    <input
        id={localClass}
        {multiple}
        type="file"
        {accept}
        on:change={fileInputChange} />
    <div class="field" on:dragover={dragOverHandler} on:drop={dropHandler}>
        <label for={localClass} style="z-index: 1;">
            <Button
                style="border-top-left-radius: 36px; border-bottom-left-radius: 36px; margin: 0; flex:1;">
                {buttonTitle}
            </Button>
        </label>
        <div>{fieldTitle}</div>
    </div>
</div>
