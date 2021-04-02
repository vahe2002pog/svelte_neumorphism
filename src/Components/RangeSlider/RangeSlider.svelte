<script>
    import { randString } from "../../scripts/functions";

    let localClass;
    export let max;
    export let min;
    export let step;
    export let value;
    let fixSize;
    let valPercent;
    normalizeTypes();
    $: valPercent = (value / (max - min)) * 100;
    let isCaught = false;

    while (true) {
        let tempClass = "range-" + randString(5);
        if (document.getElementsByClassName(tempClass).length === 0) {
            localClass = tempClass;
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
        calcValue(event.x);
    }

    function thumbDragging(event) {
        if (event.buttons === 1 && isCaught) {
            calcValue(event.x);
        }
    }

    function mathMod(a, b) {
        const mul = Math.pow(10, fixSize);
        a = mul * a;
        b = mul * b;
        return (a % b) / mul;
    }

    function calcValue(x) {
        let element = document.querySelector(`.${localClass}`);
        let containerX = element.getBoundingClientRect().x;
        let width = element.offsetWidth;
        if(x < containerX){
            return value = min;
        }
        if(x > containerX + width){
            return value = max;
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
            value = max;
        } else if (tempValue < min) {
            tempValue = min;
        } else {
            value = tempValue;
        }
    }

    document.addEventListener("mouseup", thumbRelease);
    document.addEventListener("mousemove", thumbDragging);
</script>

<style lang="less" global>
    @import "RangeSlider.less";
</style>

<div style="margin:10px;">
    <div
        class="range-container {localClass} {$$props.class ? $$props.class : ''}">
        <div class="track" on:mousedown={thumbCaught}>
            <div class="progress" style="width: {valPercent}%" />
            <div
                class="thumb"
                style="margin-left: {valPercent}%; transform: translate(-50%, 0);">
                <div />
            </div>
        </div>
    </div>
</div>
