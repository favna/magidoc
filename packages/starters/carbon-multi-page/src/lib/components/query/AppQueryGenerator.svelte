<script type="ts">
  import Prism from '@magidoc/plugin-svelte-prismjs'
  import 'prismjs/components/prism-graphql.js'
  import 'prismjs/components/prism-json.js'

  import { Button } from 'carbon-components-svelte'
  import { graphqlQuery } from './stores'
  import { NullGenerationStrategy } from '@magidoc/plugin-query-generator'
  import { Rule, RuleFilled, Add, Copy, Subtract } from 'carbon-icons-svelte'

  export let code: string
  export let language: 'graphql' | 'json'

  let copyButtonText = 'Copy query'

  export let copy = async (text: string) => {
    try {
      copyButtonText = 'Copied!'
      await navigator.clipboard.writeText(text)
      setTimeout(() => {
        copyButtonText = 'Copy query'
      }, 1500)
    } catch (e) {
      // eslint-disable-next-line no-console
      console.error(e)
    }
  }
</script>

<div class="wrapper">
  <div class="code-section">
    <Prism source={code} {language} maxHeight={'20rem'} minHeight={'12rem'} />
  </div>
  <div class="button-bar">
    <Button
      kind="ghost"
      icon={Copy}
      iconDescription={copyButtonText}
      tooltipPosition="left"
      size="field"
      on:click={() => copy(code)}
    />
    <Button
      kind="ghost"
      icon={$graphqlQuery.nullGenerationStrategy ===
      NullGenerationStrategy.NEVER_NULL
        ? RuleFilled
        : Rule}
      iconDescription={$graphqlQuery.nullGenerationStrategy ===
      NullGenerationStrategy.NEVER_NULL
        ? 'Never null fields'
        : 'Always null fields'}
      tooltipPosition="left"
      size="field"
      on:click={() => graphqlQuery.toggleNullGenerationStrategy()}
    />
    <Button
      kind="ghost"
      icon={Add}
      iconDescription="Increase query depth"
      tooltipPosition="left"
      size="field"
      on:click={() => graphqlQuery.increaseDepth()}
    />
    <p style="text-align:center">
      {$graphqlQuery.depth}
    </p>
    <Button
      kind="ghost"
      icon={Subtract}
      iconDescription="Decrease query depth"
      tooltipPosition="left"
      size="field"
      on:click={() => graphqlQuery.decreaseDepth()}
    />
  </div>
</div>

<style>
  .wrapper {
    display: flex;
    justify-content: flex-start;
  }

  .code-section {
    width: 100%;
    overflow: hidden;
  }

  .button-bar {
    display: flex;
    flex-direction: column;
  }
</style>
