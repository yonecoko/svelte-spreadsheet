
<script lang="ts">
    import Cell from './Cell.svelte';
    import {onMount} from 'svelte'

    let cellContents:(string|number)[][] = [
        [1, 2, 3,],
        [4, 5, 6,],
        [7, 8, 9,],
    ];

    
    const persist = () => {
        fetch('/api/cells', {
            method: 'POST',
            headers: {'Content-type': 'application/json'},
            body: JSON.stringify({cells: cellContents})
        });
    }

    onMount(() => {
        fetch('/api/cells').then(res => res.text().then(s => {
            const data = JSON.parse(s);
            if(data.cells) {
                cellContents = data.cells;
            }
        }));
    });

    </script>


<h1>スプレッドシート</h1>
<table>
    <tr>
        <th></th>
        {#each cellContents[0] as _, i}
        <th>{String.fromCharCode(65 + i)}</th>
        {/each}
    </tr>
    {#each cellContents as row, i}
    <tr>
        <th>{i + 1}</th>
        {#each row as cell, j}
        <Cell content={cell} onChange={(updated) => cellContents[i][j] = updated}/>
        {/each}
    </tr>
    {/each}
</table>
<button on:click={() => console.log(cellContents)}>Inspect</button>
<button on:click={() => cellContents = [...cellContents, Array(cellContents[0].length).fill(0)]}>+ row</button>
<button on:click={() => cellContents = cellContents.slice(0, -1)}>- row</button>
<br>
<button on:click={() => cellContents = cellContents.map(row => [...row, 0])}>+ column</button>
<button on:click={() => cellContents = cellContents.map(row => row.slice(0, -1))}>- column</button>

<br>
<button on:click={persist}>Save</button>

<style>
    table{
        width: 100%;
        /* border: solid 1px #000000; */
        text-align: center;
    }

    th{
        border: solid 1px;
    }
</style>
