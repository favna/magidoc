<script lang="ts">
  import AppList from '$lib/components/common/list/AppList.svelte'

  import {
    MarkdownToken,
    type MarkdownOptions,
    type Renderers,
  } from '@magidoc/plugin-svelte-marked'

  import { OrderedList, UnorderedList } from 'carbon-components-svelte'
  import type { marked } from 'marked'

  export let token: marked.Tokens.List
  export let options: MarkdownOptions
  export let renderers: Renderers

  let component: unknown
  $: component = token.ordered ? OrderedList : UnorderedList
</script>

<AppList ordered={token.ordered} start={token.start || 1}>
  {#each token.items as item}
    <MarkdownToken token={{ ...item }} {options} {renderers} />
  {/each}
</AppList>
