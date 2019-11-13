<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();
</script>

<style>
	.modal-background {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background: rgba(0,0,0,0.7);
	}

	.modal {
		position: fixed;
		background: #3d3f46;
		z-index: 2;
		left: 50%;
		top: 50%;
		width: calc(100vw - 4em);
		max-width: 32em;
		max-height: calc(100vh - 4em);
		overflow: auto;
		transform: translate(-50%,-50%);
		border-radius: 0.2em;
    color: white;
    border: 2px solid #dcddea;
    padding: 3em;
	}
	#closeBtn {
		display: block;
    width: 250px;
    font-family: 'Overpass Mono';
    font-weight: 400;
    margin-top: 1em;
    margin-left: auto;
    margin-right: auto;
    background: #33194d;
    color: hsla(140, 100%, 90%, 1);
    padding-top: 8px;
    cursor: pointer;
    border: 1px solid #dcdedf;
    border-radius: 6px;
    grid-column: 1/-1;
    grid-row: 3;
    transition: all 200ms;
 	}
	#closeBtn:hover {
		background: #00000000;
		color: #00ffc4;
		border: 1px solid #00ffc4;
		box-shadow: none;
	}
	hr {
		margin-top: 20px;
		margin-bottom: 20px;
	}
</style>

<div class='modal-background' on:click='{() => dispatch("close")}'></div>

<div class='modal'>
	<slot name='header'></slot>
	<hr>
	<slot></slot>
	<hr>

	<button id="closeBtn" on:click='{() => dispatch("close")}'>Close</button>
</div>