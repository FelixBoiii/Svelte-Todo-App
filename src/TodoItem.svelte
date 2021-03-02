<script context="module" lang="ts">
    export const TodoTypeTulip: [string, string][] = [
        ["work", "👔"],
        ["home", "🏠"],
        ["school", "🎓"],
        ["self", "💪"],
        ["other", "🗿"],
    ];
</script>

<script lang="ts">
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();
    export let name: string;
    export let todoType: number;
</script>

<div class="section">
    <div class="item">
        <p>{TodoTypeTulip[todoType][1]}</p>
        <p class="itemName">{name}</p>
        <p id="delete" on:click={() => dispatch("delete", true)}>❌</p>
    </div>
</div>

<style>
    .section {
        text-align: center;
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
        .item {
            grid-template-columns: auto 50% auto;
        }
    }
</style>
