<script lang="ts">
import { createEventDispatcher } from "svelte";



export let x: number;
export let y: number;
export let color: string = 'white';

const dispatch = createEventDispatcher();

</script>

<div
    class=cell style="background-color: {color};"
    on:mousedown|preventDefault={() => dispatch('begin', [x, y])}
    on:mouseenter={() => dispatch('enter', [x, y])}
>

</div>

<style>

.cell {
    display: inline-block;
    width: 40px;
    height: 40px;
    border: solid 1px darkgrey;
    color: lightblue;
    cursor: pointer;
    transition: background-color 0.25s ease;
}

</style>
