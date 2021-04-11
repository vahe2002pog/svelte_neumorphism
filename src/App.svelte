<script>
    import SelectGroup from "./Components/SelectGroup/SelectGroup.svelte";
    import RangeSlider from "./Components/RangeSlider/RangeSlider.svelte";
    import Button from "./Components/Button/Button.svelte";
    import Switch from "./Components/Switch/Switch.svelte";
    import CheckBox from "./Components/CheckBox/CheckBox.svelte";
    import Position from "./Components/Position/Position.svelte";
    import File from "./Components/File/File.svelte";

    import { mdiYoutube } from "@mdi/js";

    let active = true;
    let checked = false;
    let value = 50;
    let position = 1;
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

    function buttonClick(){
        window.open("https://youtube.com");
    }

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
    <RangeSlider thumb controls bind:value min="0" max="100" step="0.1" />
    {active? value: "Значение слайдера не будет отображаться!"}<br>

    <Switch bind:active/> 
    <CheckBox bind:checked> Показать позицию </CheckBox>
    <Position bind:position/>
    {checked? position : "Позиция не будет показываться"}<br><br>
    <Button path={mdiYoutube} size="normal" on:click={buttonClick} right>Youtube</Button>
    <File mini accept="image/*, .pdf"/>
    <File multiple buttonTitle="Открыть файл" fieldTitle="или перетащите файл сюда" accept="image/*, .pdf" style="width: 400px; height: 275px;"/>
    <input type="text" placeholder="Текстовое поле" style="width:250px">
    <textarea rows="4" style="width:250px" placeholder="Многострочное текстовое поле" ></textarea>
</main>
