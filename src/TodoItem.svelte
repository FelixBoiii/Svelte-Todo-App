<script context="module" lang="ts">
    export const TodoTypeTulip: [string, string][] = [
        ["work", "👔"],
        ["school", "🎒"],
        ["home", "🏠"],
        ["online", "🌐"],
    ];
</script>

<script lang="ts">
    import { createEventDispatcher } from "svelte";
    import { fly } from "svelte/transition";
    const dispatch = createEventDispatcher();
    export let name: string;
    export let todoType: number;
</script>

<div class="section" in:fly={{ y: 50, duration: 500 }}>
    <div class="item">
        <p>{TodoTypeTulip[todoType][1]}</p>
        <p class="itemName">{name}</p>
        <p id="delete" on:click={() => dispatch("delete", true)}>❌</p>
    </div>
</div>

<style>
    * {
        margin: 0;
    }
    .section {
        text-align: center;
        max-width: 400px;
        margin: 0 auto;
        font-size: 1.5em;
    }
    .item {
        margin: 0;
        display: grid;
        grid-template-columns: auto 300px auto;
        align-items: center;
    }
    .itemName {
        text-align: left;
    }
    #delete {
        cursor: pointer;
    }
    @media (max-width: 640px) {
        .section {
            max-width: none;
        }
    }
</style>
