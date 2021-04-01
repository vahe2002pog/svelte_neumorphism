<script>
    import SelectGroup from "./SelectGroup.svelte";
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
    <!-- <SelectGroup class="selectGroup" header="Выберите сообщество" {items} /> -->
    <div style="display: flex; justify-content: center; align-items: center;">
        <!-- <input type="text" placeholder="ABOBA" /> -->
        <!-- <textarea placeholder="ABOBA" rows="1"/> -->
        <input type="range" style="margin: 10px">
    </div>
</main>
