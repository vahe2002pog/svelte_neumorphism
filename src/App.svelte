<script>
    import SelectGroup from "./Components/SelectGroup/SelectGroup.svelte";
    import RangeSlider from "./Components/RangeSlider/RangeSlider.svelte";
    import Button from "./Components/Button/Button.svelte";
    import Switch from "./Components/Switch/Switch.svelte";
    import { mdiInstagram } from "@mdi/js";

    let checked = true;
    let value = 50;
    let items = [
        {
            imageURL: "https://picsum.photos/200",
            name: "Group1",
            groupURL: "https://music.youtube.com/",
        },
        {
            imageURL: "https://picsum.photos/200?1",
            name: "Group2",
            groupURL: "https://www.reddit.com/",
        },
    ];
</script>

<style lang="less">
    main {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }
</style>

<main>
    <SelectGroup class="selectGroup" header="Выберите сообщество" {items} />
    <RangeSlider thumb controls bind:value min="0" max="100" step="1" />
    {checked? value: "Значение слайдера не будет отображаться!"}
    <Switch bind:checked/> <br>
    <Button path={mdiInstagram} size="x-small" right>ABOBA</Button>
    <Button path={mdiInstagram} size="small" right>ABOBA</Button>
    <Button path={mdiInstagram} size="normal" right>ABOBA</Button>
    <Button path={mdiInstagram} size="large" right>ABOBA</Button>
    <Button path={mdiInstagram} size="x-large" right>ABOBA</Button>
    <input type="text" placeholder="Текстовое поле" style="width:250px">
    <textarea rows="4" style="width:250px" placeholder="Многострочное текстовое поле" ></textarea>
</main>
