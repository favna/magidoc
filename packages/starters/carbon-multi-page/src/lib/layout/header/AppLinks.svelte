<script lang="ts">
  import { getOrDefault } from '$lib/variables'
  import {
    templates,
    type ExternalLink,
  } from '@magidoc/plugin-starter-variables'

  import {
    HeaderAction,
    HeaderPanelDivider,
    HeaderPanelLinks,
  } from 'carbon-components-svelte'
  import _ from 'lodash'
  import AppLink from './AppLink.svelte'

  const links = getOrDefault(templates.EXTERNAL_LINKS, [])

  const defaultGroups = links.filter(
    (link): link is ExternalLink => !!(link && !link.group),
  )

  const otherGroups = _.map(
    _.groupBy(
      links.filter(
        (link): link is ExternalLink & { group: string } =>
          !!(link && link.group),
      ),
      (link) => link.group,
    ),
    (value, key) => ({
      key,
      value,
    }),
  )
</script>

{#if links.length > 0}
  <HeaderAction transition={false}>
    <HeaderPanelLinks>
      <!-- Display all items without type first-->
      {#each defaultGroups as item}
        <AppLink link={item} />
      {/each}

      <!-- Display all items with type-->
      {#each otherGroups as item}
        <HeaderPanelDivider>{item.key}</HeaderPanelDivider>
        {#each item.value as link}
          <AppLink {link} />
        {/each}
      {/each}
    </HeaderPanelLinks>
  </HeaderAction>
{/if}
