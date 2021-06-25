<script lang="ts">

import Cell from "./cell.svelte";

export let width: number = 10;
export let height: number = 10;

type Cell = [x: number, y: number, color: string];
type Range = [start: Cell, end: Cell];

const range = (start: number, end: number) => [...Array(end - start).keys()].map(n => start + n)

let cells: Cell[][];
let selections: Range[];
let dragging = false;
let dragStart = null;
let dragEnd = null;

$: cells = generateCells(dragging, dragStart, dragEnd);

function inSelection(x, y, [sx, sy], [ex, ey]) {
    if (x < Math.min(sx, ex) || x > Math.max(sx, ex)) return false;
    if (y < Math.min(sy, ey) || y > Math.max(sy, ey)) return false;
    return true;
}

function generateCells(dragging, start, end): Cell[][] {
    return range(0, height).map(y => range(0, width).map(x => {
        let color;
        if (start && end && inSelection(x, y, start, end)) {
            color = 'pink';
        } else {
            color = 'white';
        }
        return [x, y, color];
    }));
}

function handleBegin(ev) {
    const [x, y] = ev.detail;
    console.log('begin', x, y);
    dragging = true;
    dragStart = dragEnd = [x, y];
}

function handleEnter(ev) {
    const [x, y] = ev.detail;
    if (dragging) {
        console.log('enter', x, y);
        dragEnd = [x, y];
    }
}

function handleUp(ev) {
    console.log(ev);
    dragging = false;
    dragStart = null;
    dragEnd = null;
}

</script>

<div class="board">
    {#each cells as row}
        <div class="row">
            {#each row as [x, y, color] (x + y * width)}
                <Cell {x} {y} {color}
                    on:begin={handleBegin}
                    on:enter={handleEnter}
                />
            {/each}
        </div>
    {/each}
</div>

<svelte:window on:mouseup={handleUp}/>

<style>

.board {
    display: inline-flex;
    flex-direction: column;
    margin: 0 auto;
    border: solid 1px darkgrey;
}

.row {
    display: flex;
}
</style>
