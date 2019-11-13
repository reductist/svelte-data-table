<script>
  import Table from './Table.svelte'
  import Header from './Header.svelte'
  import Rows from './Rows.svelte'
  import Pagination from './Pagination.svelte'
  import { onMount } from 'svelte';

  const url = 'https://jsonplaceholder.typicode.com/todos/';
  let data = [];

  onMount(async function() {
    const response = await fetch(url);
    const json = await response.json();
    data = json;
  });
  
  let filters = {
    title: {value: '', keys: ['title']}
  }
  
  let pageSize = 10;
  let totalPages = 0;
  let currentPage = 1;
  
  function totalPagesChanged (e) {
    totalPages = e.detail;
  }
</script>

<style>
  :root {
    background:#3d3f46;
    margin: 0 auto;
    overflow-x: hidden;
    width: 95%;
  }
  #wrapper {
    font-family: 'Overpass Mono', sans-serif;
    font-size: 14px;
    margin-top: 2vh;
    height: fit-content;
  }
  #table-wrapper {
    width: 100%;
  }
  td {
    font-family: 'Fira Code';
    font-size: 12px;
    color: #001122;
    border: 0.5px solid #7e8e9f;
    border-collapse: collapse;
    padding-left: 10px;
    padding-top: 5px;
    padding-bottom: 5px;
    height: 30px;
    font-weight: 100;
    line-height: 1rem;
  }
  td:nth-child(1) {
    text-align: center;
  }
  td:hover {
    background: #66686a !important;
    color: #00ffc4 !important;
    border: 1px solid #00ffc4 !important;
    border-collapse: collapse !important;
    font-weight: 400 !important;
  }
  #filterInput {
    color: #00ffc4;
    border: 1px solid #00ffc4;
    border-top-left-radius: 10px;
    background: #2d2f3a;
    padding-top: 8px;
    padding-bottom: 8px;
    text-indent: 8px;
    transition: all 350ms;
    transition-timing-function: ease-out;
    margin-bottom: 0;
  }
  #filterInput:focus	{
    border-radius: 2em;
    border: 2px solid #00d5ff;
    background: #fff;
    color: #052223;
    outline: none;
    margin-bottom: 1em;
  }
  #filterInput:not(:placeholder-shown) {
    border-radius: 2em;
    border: 2px solid #00d5ff;
    background: #fff;
    color: #052223;
    outline: none;
    margin-bottom: 1em;
  }
</style>

<div id='wrapper'>

  <div id="filterDiv">
    <input type="text" 
           id="filterInput" 
           placeholder="Filter"
           required 
           bind:value="{filters.title.value}"/>
  </div>

  <div id="table-wrapper">
    <Table {data} {filters} {pageSize} {currentPage}
            class="table"
            selectedClass="table-info"
            on:totalPagesChanged="{totalPagesChanged}"
    >

      <thead slot="head" let:displayData="{data}">
        <Header sortKey="userId">User ID</Header>
        <Header sortKey='title' defaultSort="asc">Title</Header>
        <Header sortKey='completed'>Status</Header>
      </thead>

      <tbody slot="body" let:displayData="{displayData}">
        {#each displayData as row (row.title)}
          <Rows {row}>
            <td>{row.userId}</td>
            {#if row.title === undefined}
              <td>-</td>
            {:else}	
              <td>{row.title}</td>
            {/if}
            {#if row.completed === undefined}
              <td>-</td>
            {:else}
              <td>{(row.completed) ? 'Complete' : 'Incomplete'}</td>
            {/if}
          </Rows>
        {/each}
      </tbody>
    </Table>

    <Pagination {totalPages} bind:currentPage="{currentPage}">
    </Pagination>

  </div>
</div>