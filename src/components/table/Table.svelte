<script lang="ts">
  import get from "lodash/get";
  import Error from "../../routes/_error.svelte";
  import type TableColumn from "./table_columns/TableColumn";

  /**
   * Array containing the headers and it's respectives
   * configurations.
   */
  export let columns: Array<TableColumn>;

  /**
   * List of elements that sould be rendered in the table.
   */
  export let elements: Array<any>;

  /**
   * Wheter the list content should expand itself
   */
  export let fluid: boolean;

  /**
   * If the component shadow should be disabled,
   * defaults to false
   */
  export let noShadow: boolean;

  /**
   * Helper tailwind classes
   * https://tailwindcss.com/docs
   */
  let clazz: string;
  export { clazz as class };

  let headers = columns.map((column) => column.header);
</script>

{#if elements.length > 0}
  <div
    class="flex rounded-md bg-black p-6 min-w-full {!fluid
      ? 'h-4/5 overflow-y-auto'
      : ''} {clazz}"
    class:shadow-lg={!noShadow}
  >
    <table class="border-collapse min-w-full">
      {#if headers != null && headers.length > 0}
        <thead class="bg-blue-950 bg-opacity-5 rounded-md">
          <tr>
            {#each headers as header}
              {#if header?.options?.override}
                <th {...header.options}>{header.title}</th>
              {:else}
                <th
                  class={`text-white uppercase font-medium text-sm text-left px-4 py-2 ${
                    header?.options?.class ?? ""
                  }`}
                >
                  {header.title}
                </th>
              {/if}
            {/each}
          </tr>
        </thead>
      {/if}
      <tbody>
        {#each elements as object, rowIndex}
          <tr class="border-b">
            {#each columns as column}
              <td class="px-4 py-8">
                {#if column.component != null}
                  <svelte:component
                    this={column.component}
                    props={column.options?.(
                      get(object, column.path, object),
                      rowIndex
                    )}
                  />
                {:else}
                  <div
                    {...column.options?.(
                      get(object, column.path, object),
                      rowIndex
                    )}
                  >
                    {column.transform?.(
                      get(object, column.path, object),
                      rowIndex
                    ) ?? get(object, column.path, object)}
                  </div>
                {/if}
              </td>
            {/each}
          </tr>
        {/each}
      </tbody>
    </table>
  </div>
{:else}
  <p class="mt-8 mb-8 text-center min-w-full text-white">Connect to wallet</p>
{/if}
