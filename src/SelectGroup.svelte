<script>
    import Icon from "mdi-svelte";
    import { clickOutside } from "./scripts/clickOutside.js";
    import { mdiChevronDown } from "@mdi/js";
    export let header = "";
    export let items = [];
    let chosenItem = undefined;
    let opened = false;

    function selectArrowClick(e, close = false) {
        if (close) {
            opened = true;
        } else {
            opened = !opened;
        }
        let options = document.querySelector(".options");
        if (opened) {
            options.style.height = "min-content";
            let height = options.offsetHeight + "px";
            options.style.height = "0px";
            setTimeout(() => {
                options.style.height = height;
            }, 0);
        } else {
            options.style.height = "0px";
        }
    }

    function optionClick(item) {
        chosenItem = item;
        selectArrowClick((close = true));
    }
</script>

<style lang="less">
    @import "../public/less/varibles";
    @import "../public/global";
    .select-container {
        position: relative;
        & *{
            .unselectable;
        }
        & .select {
            position: absolute;
            background-color: @background-color;
            overflow: hidden;
            .neumorphism(@flat: "true");
            & .header {
                display: flex;
                padding: 2px;
                justify-content: space-between;
                align-items: center;
                height: 30px;
                flex-wrap: nowrap;

                & .header-text {
                    flex-grow: 1;
                    margin-left: 15px;
                    font-size: small;
                }

                & .icon {
                    margin-left: 10px;
                    margin-right: 5px;
                    transition: 0.3s;
                    transform: rotate(0deg);
                    &.rotate {
                        transform: rotate(180deg);
                    }
                }
            }
            & .options {
                overflow: hidden;
                transition: 0.3s;
                height: 0px;
                & .item {
                    display: flex;
                    align-items: center;
                    padding: 5px 10px;
                    height: 32px;
                    &:hover {
                        background-color: darken(@background-color, 3%);
                    }
                    &:active {
                        background-color: darken(@background-color, 5%);
                    }
                    & img {
                        height: 100%;
                        margin-right: 10px;
                        border-radius: 16px;
                        object-fit: cover;
                    }
                }
            }
        }
    }
</style>

<div class="select-container">
    <div
        class="select {$$props.class}"
        use:clickOutside
        on:click_outside={opened ? () => selectArrowClick((close = true)) : () => {}}>
        <div class="header">
            <div class="header-text">
                {#if chosenItem}
                    <a href={chosenItem.groupURL} target="_blank">
                        {chosenItem.name}
                    </a>
                {:else}
                    <div>{header}</div>
                {/if}
            </div>
            <div
                class={(opened ? 'rotate ' : '') + 'icon pointer'}
                on:click={selectArrowClick}>
                <Icon path={mdiChevronDown} />
            </div>
        </div>
        <div class="options">
            {#each items as item}
                <hr size="5" />
                <div
                    class="item pointer"
                    on:click={() => optionClick(item)}>
                    <img src={item.imageURL} alt="" />
                    <div>{item.name}</div>
                </div>
            {/each}
        </div>
    </div>
</div>
