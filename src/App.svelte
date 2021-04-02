<script>
    import SelectGroup from "./Components/SelectGroup/SelectGroup.svelte";
    import RangeSlider from "./Components/RangeSlider/RangeSlider.svelte";
    let value = 5;
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
    }
</style>

<main>
    <div style="width: 100px; height: 100px;" />
    <SelectGroup class="selectGroup" header="Выберите сообщество" {items} />
    <div style="display: flex; justify-content: center; align-items: center; flex-direction:column;">
        <RangeSlider bind:value={value} min="0" max="10" step="0.01"></RangeSlider>
        {value}
    </div>
</main>
