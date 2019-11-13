<script>
  import { getContext } from 'svelte'
  import Modal from './Modal.svelte';

  export let row;
  let tr;
  let showModal = false;

  const store = getContext('store');
  let customSelection = store.customSelection;
  let selectedRows = store.selectedRows;
  let selectedClass = store.selectedClass;

  $: isSelected = $selectedRows.find(it => it === row) !== undefined;

  $: rowClass = (() => {
    let classes = [];
    if (isSelected) {
      classes.push($selectedClass);
    }
    if (!$customSelection) {
      classes.push('selectable');
    }
    return classes.join(' ');
  })()

  function handleRowSelected(event) {
    if ($customSelection) {
      return;
    }
    let source = event.target || event.srcElement
    if (source.tagName.toLowerCase() === 'td') {
      if (isSelected) {
        store.deselectRow(row);
        showModal = true;
      } else {
        store.selectRow(row);
        showModal = true;
      }
    }
  }

  const screwYouGuys = '\\_(-_-)_/';
</script>

<style>
  tr.selectable {
    cursor: pointer;
  }

  tr.table-info {
    background: #99ffe7;
  }

  tr {
    height: 15px;
    padding: 10px;
  }

  .modal-main-text {
    background: #efeffa;
    color: #12141a;
    padding: 10px;
    border-radius: 10px;
    border: 1px solid #6f6f7a;
    font-size: 12px;
    line-height: 1rem;
    font-family: 'Fira Code';
    text-align: justify;
  }

  .row-details {
    padding-left: 10px;
    font-size: 0.9rem;
    font-weight: 100;
    font-family: 'Overpass Mono', sans-serif;
    line-height: 1.5rem;
  }

  .modal-subtitle-span {
    font-family: 'Fira Code';
    font-size: 1rem;
    font-weight: 500;
    margin-top: 8px;
    color: #e4e4fc;
  }

  .modal-title-span {
    font-weight: 100;
  }
</style>

<tr bind:this="{tr}" class="{rowClass + ' ' + $$props.class}" on:click="{handleRowSelected}">
  <slot></slot>
</tr>

{#if showModal}
  <Modal on:close="{() => showModal = false}">
    <h2 slot="header">
      <span class="modal-title-span">
        Item ID: {row.id}
      </span>
      <p>
        <em><span id="modal-subtitle-span">
          Status: {(row.completed) ? 'Complete' : 'Incomplete'}
          </span>
        </em>
      </p>
    </h2>

    <ul class="row-details">
      {#if row.userId != undefined}
        <li>User-ID: {row.userId}</li>
      {/if}
    </ul>
    <div class='modal-main-text'>
      {#if row.title != undefined}
        {row.title}
      {:else}
        No title available! <span style="color:yellow; background:black; padding:0.5em; border-radius:15%; font-weight:900;">{screwYouGuys}</span>
      {/if}
    </div>
  </Modal>
{/if}