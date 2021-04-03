<script>
    import { randString } from "../../scripts/functions";
    import Button from "../Button/Button.svelte";
    import { mdiPlus, mdiMinus } from "@mdi/js";

    let localClass;
    export let max;
    export let min;
    export let step;
    export let value;
    export let controls = false;
    export let thumb;
    let fixSize;
    let valPercent;
    normalizeTypes();
    $: valPercent = (value / (max - min)) * 100;
    let isCaught = false;
    let container;
    let interval;
    let timeout;
    let thumbHeight = 0;

    while (true) {
        let tempClass = "range-" + randString(5);
        if (document.getElementsByClassName(tempClass).length === 0) {
            localClass = tempClass;
            setTimeout(() => {
                container = document.querySelector(`.${localClass}`);
                if(thumb){
                    thumbHeight = container.getElementsByClassName("thumb")[0].offsetHeight;
                }
            }, 0);
            break;
        }
    }

    function normalizeTypes() {
        max = parseFloat(max) || 10;
        min = parseFloat(min) || 0;
        step = parseFloat(step) || 1;
        value = parseFloat(value) || 0;
        if (max <= min + step) {
            max = 10;
            min = 0;
            step = 1;
            value = 0;
        }
        fixSize = step.toString().split(".")[1]?.length || 0;
    }

    function thumbRelease(event) {
        isCaught = false;
    }

    function thumbCaught(event) {
        isCaught = true;
        value = calcValue(event.x);
    }

    function thumbDragging(event) {
        if (event.buttons === 1 && isCaught) {
            value = calcValue(event.x);
        }
    }

    function mathMod(a, b) {
        const mul = Math.pow(10, fixSize);
        a = mul * a;
        b = mul * b;
        return (a % b) / mul;
    }

    function calcValue(x) {
        let containerX = container.getBoundingClientRect().x + (thumbHeight / 2);
        let width = container.offsetWidth - thumbHeight;
        if (x < containerX) {
            return min;
        }
        if (x > containerX + width) {
            return max;
        }
        let percent = (x - containerX) / width;
        let floatValue = percent * (max - min);
        let mod = mathMod(floatValue, step);
        let tempValue;
        if (mod < step / 2) {
            tempValue = Math.floor(floatValue / step) * step;
        } else {
            tempValue = (Math.floor(floatValue / step) + 1) * step;
        }

        tempValue = parseFloat(tempValue.toFixed(fixSize));

        if (tempValue > max) {
            return max;
        } else if (tempValue < min) {
            return min;
        } else {
            return tempValue;
        }
    }

    function valueUp() {
        let tempValue = value;
        tempValue += step;
        tempValue = parseFloat(tempValue.toFixed(fixSize));
        if (tempValue > max) {
            value = max;
            mouseUp();
        } else {
            value = tempValue;
        }
    }

    function valueDown() {
        let tempValue = value;
        tempValue -= step;
        tempValue = parseFloat(tempValue.toFixed(fixSize));
        if (tempValue < min) {
            value = min;
            mouseUp();
        } else {
            value = tempValue;
        }
    }

    function mouseDown(act) {
        clearTimeout(timeout);
        timeout = setTimeout(() => {
            if (act === "+") {
                interval = setInterval(valueUp, 40);
            } else {
                interval = setInterval(valueDown, 40);
            }
        }, 300);
    }

    function mouseUp() {
        clearTimeout(timeout);
        clearInterval(interval);
    }

    document.addEventListener("mouseup", () => {
        thumbRelease();
        mouseUp();
    });
    document.addEventListener("mousemove", thumbDragging);
</script>

<style lang="less" global>
    @import "RangeSlider.less";
</style>

<div class="range-block">
    {#if controls}
        <div>
            <Button
                path={mdiMinus}
                on:click={valueDown}
                on:mousedown={() => mouseDown('-')}
                rounded
                size="x-small" />
        </div>
    {/if}
    <div
        class="range-container {localClass} {$$props.class ? $$props.class : ''}">
    <div class="track{thumb ?"": " none-thumb"}" on:mousedown={thumbCaught}>
            <div class="progress" style="width: calc({valPercent}% + {thumbHeight / 2 - (thumbHeight * valPercent / 100)}px)" />
            {#if thumb}
                <div
                    class="thumb"
                    style="margin-left: {valPercent}%; transform: translate({ -valPercent }% , 0);">
                    <div />
                </div>
            {/if}
        </div>
    </div>
    {#if controls}
        <div>
            <Button
                path={mdiPlus}
                on:click={valueUp}
                on:mousedown={() => mouseDown('+')}
                rounded
                size="x-small" />
        </div>
    {/if}
</div>
