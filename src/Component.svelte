<script>
  import { getContext } from "svelte"

  export let data;

  const { styleable, Provider  } = getContext("sdk")
  const component = getContext("component")

  let filesURL = [];
    $: filesURL = data? JSON.parse(data).map((e) => e.link): [];
</script>

<div use:styleable={$component.styles}>
  {#each filesURL as url}
        <Provider data={{fileURL:url}}>
        <slot />
      </Provider>
      {/each}
</div>
