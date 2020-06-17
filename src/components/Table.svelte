<script>
  import _ from "lodash";
  export let columns = [];
  export let rows = [];

  let sortedBy = { columnAccessor: "", order: "" };

  const sortBy = column => {
    if (sortedBy.columnAccessor === "") {
      sortedBy = { ...sortedBy, order: "DESC" };
      rows = rows.sort((a, b) =>
        a[column.accessor] > b[column.accessor] ? -1 : 1
      );
    } else {
      if (sortedBy.order === "DESC") {
        rows = rows.sort((a, b) =>
          a[column.accessor] > b[column.accessor] ? 1 : -1
        );
        sortedBy = { ...sortedBy, order: "ASC" };
      } else {
        rows = rows.sort((a, b) =>
          a[column.accessor] > b[column.accessor] ? -1 : 1
        );
        sortedBy = { ...sortedBy, order: "DESC" };
      }
    }
    sortedBy = { ...sortedBy, columnAccessor: column.accessor };
  };
</script>

<style lang="scss">
  th {
    background-color: #6e7bd9;
    color: white;
    padding: 0.5rem 0.8rem;
    font-size: 1.4rem;
    border-bottom: 2px solid #b5bce9;
    position: sticky;
    top: 0;

    @media only screen and (max-width: 800px) {
      font-size: 0.8rem;
    }
  }

  tr,
  th {
    text-align: center;
  }

  table,
  th {
    border-collapse: collapse;
  }

  table {
    width: 100%;
  }
</style>

<table>
  <tr>
    {#each columns as column}
      {#if !column.hidden}
        <th
          style={`cursor:${column.disableSortBy ? 'unset' : 'pointer'};`}
          on:click={() => !column.disableSortBy && sortBy(column)}>
          {column.Header}
        </th>
      {/if}
    {/each}
  </tr>
  {#each rows as row}
    <tr>
      <slot prop={row} />
    </tr>
  {/each}
</table>
