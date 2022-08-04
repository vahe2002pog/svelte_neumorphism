<script>
    import SelectGroup from "./Components/SelectGroup/SelectGroup.svelte";
    import RangeSlider from "./Components/RangeSlider/RangeSlider.svelte";
    import PreviewBox from "./Components/PreviewBox/PreviewBox.svelte";
    import Button from "./Components/Button/Button.svelte";
    import Switch from "./Components/Switch/Switch.svelte";
    import CheckBox from "./Components/CheckBox/CheckBox.svelte";
    import Position from "./Components/Position/Position.svelte";
    import File from "./Components/File/File.svelte";

    import { mdiYoutube, mdiVideo } from "@mdi/js";

    let active = true;
    let checked = false;
    let value = 50;
    let indets = [0, 0];
    let position = 4;
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
    <!-- <Button path={mdiYoutube} size="smallasd" on:click={buttonClick} right>Youtube</Button>
        <File mini accept="image/*, .pdf"/>
        <File multiple buttonTitle="Открыть файл" fieldTitle="или перетащите файл сюда" accept="image/*, .pdf" style="width: 400px; height: 275px;"/>
        <input type="text" placeholder="Текстовое поле" style="width:250px">
        <textarea rows="4" style="width:250px" placeholder="Многострочное текстовое поле" ></textarea> -->
    <RangeSlider thumb controls bind:value={indets[0]} min="0" max="50" step="0.1" />
    <RangeSlider thumb controls bind:value={indets[1]} min="0" max="50" step="0.1" />
    ({indets[0]} - {indets[1]})
    <PreviewBox path={mdiVideo} size={value} {indets} {position} src=".\src\logo.png" vertical style="width:250px"/>
</main>
