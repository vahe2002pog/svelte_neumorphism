<script>
    import Icon from "mdi-svelte";
    import { randString } from "../../scripts/functions";
    import { getEventsAction } from "../../scripts/utils.js";
    import { current_component } from "svelte/internal";
    import Position from "../Position/Position.svelte";
    const events = getEventsAction(current_component);

    export let path = null;
    export let style = null;
    export let src = null;
    export let disabled = false;
    export let size = 50;
    export let indets = [1, 1];
    export let position = 5;
    export let opacity = 50;
    let flexParams = ["start", "center", "end"];
    let watermarkBlockStyle;
    let watermarkStyle;
    let flexParamsIndex;
    let watermarkBoxWidth;
    let watermarkBoxHeight;
    let watermarkWidth;
    let watermarkHeight;
    let imgElement;

    let classes = "";
    let localClass;
    while (true) {
        let tempClass = "previewBox-" + randString(5);
        if (document.getElementsByClassName(tempClass).length === 0) {
            localClass = tempClass;
            break;
        }
    }
    initClasses();
    function initClasses() {
        let classesArray = [];
        if ($$props.class) {
            classesArray.push($$props.class);
        }
        if (disabled) {
            classesArray.push("disabled");
        }
        classes = classesArray.join(" ").trim();
    }

    $: watermarkWidth = imgElement?.width;
    $: watermarkHeight = imgElement?.height;

    $: watermarkStyle = `
        max-width: ${size}%;
        max-height: ${size}%;
        opacity: ${opacity / 100};
        margin: ${
            (watermarkBoxHeight / 100) * indets[0] -
            (imgElement?.height / 100) * indets[0]
        }px ${
            (watermarkBoxWidth / 100) * indets[1] -
            (imgElement?.width / 100) * indets[1]}px;`;

    $: flexParamsIndex = position.toString(3).padStart(2, "0").slice("");
    $: watermarkBlockStyle = `
        align-items: ${flexParams[flexParamsIndex[0]]};
        justify-content: ${flexParams[flexParamsIndex[1]]};
        `;

    function getIndets() {}
</script>

<div
    class="{localClass} {classes}"
    bind:clientWidth={watermarkBoxWidth}
    bind:clientHeight={watermarkBoxHeight}
    use:events
    {style}
>
    <div class="watermark-block" style={watermarkBlockStyle}>
        <img
            {src}
            bind:this={imgElement}
            style={watermarkStyle}
            alt=""
            class="watermark"
        />
    </div>
    {#if path}
        <div class="icon">
            <Icon {path} />
        </div>
    {/if}
</div>

<style lang="less" global>
    @import "PreviewBox.less";
</style>
