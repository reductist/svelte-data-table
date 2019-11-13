*Psst* looking for a shareable component template? Go here --> [sveltejs/component-template](https://github.com/sveltejs/component-template)*

---

# Svelte Data Table

This is a set of components for a paginated, sortable, filterable data table with functionality allowing clicking on rows to bring up a modal displaying additional data associated with that row. It has no external dependencies, excluding two monospace fonts: `Fira Code` and `Overpass Mono`. 

[Svelte](https://svelte.dev) lives at https://github.com/sveltejs/template.

To create a new project based on this template using [degit](https://github.com/Rich-Harris/degit):

```bash
npx degit reductist/svelte-data-table svelte-app
cd svelte-app
```

*Note that you will need to have [Node.js](https://nodejs.org) installed.*


## Get started

Install the dependencies...

```bash
cd svelte-app
npm install
```

...then start [Rollup](https://rollupjs.org):

```bash
npm run dev
```

Navigate to [localhost:5000](http://localhost:5000). You should see your app running. Edit a component file in `src`, save it, and reload the page to see your changes.

By default, the server will only respond to requests from localhost. To allow connections from other computers, edit the `sirv` commands in package.json to include the option `--host 0.0.0.0`.


## Code Examples
This is a work-in-progress. I'm still working on genericizing/automating many of the components. Right now there are a few areas that need to be manually lined-up with your external data source.

A full working [https://svelte.dev/repl/2774f569f14b47dd9e729ffeded25ddd?version=3.14.1](example) is available on the Svelte REPL.

Until I finish automating the data identifiers, here's you can set up a table that pulls from a new data source:

*App.Svelte*

Assign your external data source:
```javascript
const url = 'https://jsonplaceholder.typicode.com/todos/';
```

Point to the object key you'd like to filter on:
```javascript
let filters = {
  title: {value: '', keys: ['title']}
  }
```

Match this key in your filter input HTML:
```javascript
<div id="filterDiv">
  <input type="text" 
        id="filterInput" 
        placeholder="Filter"
        required 
        bind:value="{filters.title.value}"/>
</div>
```

Specify key sorting identifiers in your `<thead>`:
```javascript
<thead slot="head" let:displayData="{data}">
  <Header sortKey="userId">User ID</Header>
  <Header sortKey='title' defaultSort="asc">Title</Header>
  <Header sortKey='completed'>Status</Header>
</thead>
```

Specify key sorting identifiers in your `<thead>`:
```javascript
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
```

*Rows.Svelte*

I need to move the modal HTML into either the Modal.Svelte component, or a new, dedicated component. TODO. For now, it lives where the data lives, because of reasons.

```javascript
{#if showModal}
  <Modal on:close="{() => showModal = false}">
    <h2 slot="header">
      <span class="modal-title-span">
        // modal title: specify the associated key in your data
        Item ID: {row.id}
      </span>
      <p>
        <em><span id="modal-subtitle-span">
          // modal subtitle: specify associated key
          Status: {(row.completed) ? 'Complete' : 'Incomplete'}
          </span>
        </em>
      </p>
    </h2>

    <ul class="row-details">
      // list of keyed data items - does not render if undefined (eg: if not all nested objects contain all keys, you won't see 'undefined')
      {#if row.userId != undefined}
        <li>User-ID: {row.userId}</li>
      {/if}
    </ul>
    // main text box: specify associated key
    <div class='modal-main-text'>
      {#if row.title != undefined}
        {row.title}
      {:else}
        No title available! 
        <span>{screwYouGuys}</span>
      {/if}
    </div>
  </Modal>
{/if}
```

That should get you off the ground. Let me know if I missed anything. I'll work on re-pointing most of these manual assignments to top-level variables in the `<script>`s.


## Task List

- [ ] add a task list


## Deploying to the web

### With [now](https://zeit.co/now)

Install `now` if you haven't already:

```bash
npm install -g now
```

Then, from within your project folder:

```bash
cd public
now
```

As an alternative, use the [Now desktop client](https://zeit.co/download) and simply drag the unzipped project folder to the taskbar icon.

### With [surge](https://surge.sh/)

Install `surge` if you haven't already:

```bash
npm install -g surge
```

Then, from within your project folder:

```bash
npm run build
surge public
```

## Acknowledgements
* The core of this project was shamelessly stolen from [a project by tochoromero](https://github.com/tochoromero/svelte-table) that I stumbled across while trawling for a dependency-free data table component with the functionality provided above. This is where all the tricky pagination/sorting/filtering bits using stores came from. For the most part, all I did was add the modal data dive-down and hooked up an external data source. Thanks for your hard work on this!
* [https://css-tricks.com/](CSS-Tricks) for having answers to every CSS question I Googled.
* [https://developer.mozilla.org/en-US/docs/Web/JavaScript](MDN) for the best dang Javascript documentation around (imo).
* @emarcotte for answering tons of dumb questions (thanks).