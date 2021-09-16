<script lang="ts">
  import thai5k from "../carpalx-th/data/thai5k-freq.json"
  import thaisum from "../carpalx-th/data/thaisum-full.json"
  import thaisumTestset from "../carpalx-th/data/thaisum-testset.json"
  import { wisesight } from "../carpalx-th/data/wisesight"
  import { wongnai } from "../carpalx-th/data/wongnai"
  import { thaiTweets } from "../carpalx-th/data/thai-tweets"
  import { sugreeTweets } from "../carpalx-th/data/sugree-tweets"
  import Carpalx from "../carpalx-th/src/carpalx"
  import type { Triads } from "../carpalx-th/src/carpalx"
  import { Layout } from "../carpalx-th/src/layout"
  import type { ILayout } from "../carpalx-th/src/layout"
  import { extractTriads } from "../carpalx-th/src/utils"

  const datasets = {
    thai5k,
    wisesight,
    wongnai,
    thaisumTestset,
    thaisum,
    thaiTweets,
    sugreeTweets,
  }

  let keysToSwap = []
  let effort, carpalxModel, effortBySet

  const predefinedLayouts = [
    { id: "manoonchai_v03", text: "Manoonchai v1.0" },
    { id: "manoonchai_v02b", text: "Manoonchai v0.2b" },
    { id: "manoonchai_v02", text: "Manoonchai v0.2" },
    { id: "manoonchai_v01", text: "Manoonchai v0.1" },
    { id: "kedmanee", text: "Kedmanee" },
    { id: "pattachote", text: "Pattachote" },
    { id: "ikbaeb", text: "Ikbaeb" },
  ]

  let layoutInput = new Layout({ name: "manoonchai_v03" }).matrix
    .map((row) => JSON.stringify(row).replaceAll('","', '", "'))
    .join("\n")

  let customText

  $: layoutData = layoutInput
    .trim()
    .split("\n")
    .map((line) => {
      let data = []
      try {
        data = JSON.parse(
          line.replace(/^,+|,+$/gm, "").replaceAll("'\"'", '"\\""')
        )
      } catch (e) {
        return ["", "", "", "", "", "", "", "", "", "", "", "", "", ""]
      }
      return data
    })

  $: if (layoutData.length == 8) {
    const baseLayout = new Layout({ name: "kedmanee" })
    const layout = new Layout({ name: "custom" })
    layout.matrix = layoutData as ILayout<string>

    carpalxModel = new Carpalx({ layout })
    const baseModel = new Carpalx({ layout: baseLayout })

    // Use custom text if exist, or use predefined datasets
    if (customText && customText.length >= 3) {
      const customTriads: Triads = {}

      extractTriads(customTriads, customText)

      const customDatasets = {
        custom: customTriads,
      }

      const efforts: Array<[string, number]> = Object.entries(
        customDatasets
      ).map(([name, dataset]) => {
        return [name, carpalxModel.typingEffort(dataset)]
      })

      const baseEfforts: Array<[string, number]> = Object.entries(
        customDatasets
      ).map(([name, dataset]) => {
        return [name, baseModel.typingEffort(dataset)]
      })

      effortBySet = baseEfforts.map(([name, eff], i) => {
        return `${name} : ${(100 * eff) / efforts[i][1] - 100}`
      })

      effort =
        (100 * baseEfforts.reduce((prev, [_name, eff]) => prev + eff, 0)) /
          efforts.reduce((prev, [_name, eff]) => prev + eff, 0) -
        100 +
        "%"
    } else {
      const efforts: Array<[string, number]> = Object.entries(datasets).map(
        ([name, dataset]) => {
          return [name, carpalxModel.typingEffort(dataset)]
        }
      )

      const baseEfforts: Array<[string, number]> = Object.entries(datasets).map(
        ([name, dataset]) => {
          return [name, baseModel.typingEffort(dataset)]
        }
      )

      effortBySet = baseEfforts.map(([name, eff], i) => {
        return `${name} : ${(100 * eff) / efforts[i][1] - 100}`
      })

      effort =
        (100 * baseEfforts.reduce((prev, [_name, eff]) => prev + eff, 0)) /
          efforts.reduce((prev, [_name, eff]) => prev + eff, 0) -
        100 +
        "%"
    }
  }

  $: layoutDataDisplay = Array(layoutData.length / 2)
    .fill(null)
    .map((_, i) => {
      return layoutData[i].map((x, j) => [
        x,
        layoutData[i + layoutData.length / 2][j],
      ])
    })

  function swapKeyPair(e) {
    keysToSwap = [...keysToSwap, e.target.textContent]

    if (keysToSwap.length === 2) {
      const [x, y] = keysToSwap

      layoutInput = layoutInput
        .replace('"\\""', '"""')
        .replace(`"${x}"`, "{y}")
        .replace(`"${y}"`, "{x}")
        .replace("{x}", `"${x}"`)
        .replace("{y}", `"${y}"`)
        .replace('"""', '"\\""')

      keysToSwap = []
    }
  }

  function selectLayout(e) {
    const name = e.currentTarget.value
    layoutInput = new Layout({ name }).matrix
      .map((row) => JSON.stringify(row).replaceAll('","', '", "'))
      .join("\n")
  }
</script>

<main>
  <div class="container">
    <h1>Layout Analyzer</h1>

    <a
      class="github-button"
      href="https://github.com/Manoonchai/layout-analyzer"
      data-size="large"
      data-show-count="true"
      aria-label="Star Manoonchai/layout-analyzer on GitHub">Star</a
    >

    <div class="keyboard-container">
      <div class="keyboard">
        <!-- First row -->
        <div class="key">
          <div class="key__top">~</div>
          <div class="key__bottom">`</div>
        </div>
        {#each layoutDataDisplay[0] as keypair}
          <div class="key">
            <div class="key__top swappable" on:mousedown={swapKeyPair}>
              {keypair[1]}
            </div>
            <div class="key__bottom swappable" on:mousedown={swapKeyPair}>
              {keypair[0]}
            </div>
          </div>
        {/each}
        <div class="key is-backspace">
          <div class="key__bottom">&larr;</div>
        </div>
        <!-- Second row -->
        <div class="key is-tab">
          <div class="key__bottom">&rarrb;</div>
        </div>
        {#each layoutDataDisplay[1] as keypair}
          <div class="key">
            <div class="key__top swappable" on:mousedown={swapKeyPair}>
              {keypair[1]}
            </div>
            <div class="key__bottom swappable" on:mousedown={swapKeyPair}>
              {keypair[0]}
            </div>
          </div>
        {/each}
        <!-- Third row -->
        <div class="key is-capslock is-left">
          <div class="key__bottom">ก/A</div>
        </div>
        {#each layoutDataDisplay[2] as keypair}
          <div class="key">
            <div class="key__top swappable" on:mousedown={swapKeyPair}>
              {keypair[1]}
            </div>
            <div class="key__bottom swappable" on:mousedown={swapKeyPair}>
              {keypair[0]}
            </div>
          </div>
        {/each}
        <div class="key is-enter is-right">
          <div class="key__top" />
          <div class="key__bottom">&larrhk;</div>
        </div>
        <!-- Fourth row -->
        <div class="key is-shift-left is-left">
          <div class="key__bottom">⬆</div>
        </div>
        {#each layoutDataDisplay[3] as keypair}
          <div class="key">
            <div class="key__top swappable" on:mousedown={swapKeyPair}>
              {keypair[1]}
            </div>
            <div class="key__bottom swappable" on:mousedown={swapKeyPair}>
              {keypair[0]}
            </div>
          </div>
        {/each}
        <div class="key is-shift-right is-right">
          <div class="key__bottom">⬆</div>
        </div>
        <!-- Fifth row -->
        <div class="key is-left">
          <div class="key__bottom">fn</div>
        </div>
        <div class="key is-right">
          <div class="key__bottom">control</div>
        </div>
        <div class="key is-right">
          <div class="key__top">alt</div>
          <div class="key__bottom">option</div>
        </div>
        <div class="key is-command is-right">
          <div class="key__bottom">command</div>
        </div>
        <div class="key is-space" />
        <div class="key is-command is-left">
          <div class="key__bottom">command</div>
        </div>
        <div class="key is-left">
          <div class="key__top">alt</div>
          <div class="key__bottom">option</div>
        </div>
        <div class="key is-arrow-left">&ltrif;</div>
        <div class="key is-arrow-up">&utrif;</div>
        <div class="key is-arrow-down">&dtrif;</div>
        <div class="key is-arrow-right">&rtrif;</div>
      </div>
    </div>
    {#if keysToSwap.length === 1}
      <div class="prompt">Click another character to swap</div>
    {:else}
      <div class="prompt">
        You can swap characters by clicking at the buttons
      </div>
    {/if}

    <div>
      <textarea
        id="layout-input"
        bind:value={layoutInput}
        cols="55"
        rows="10"
      />
    </div>

    <select on:change={selectLayout} on:blur={selectLayout}>
      <option disabled selected value> -- Select Layout -- </option>
      {#each predefinedLayouts as layout}
        <option value={layout.id}>
          {layout.text}
        </option>
      {/each}
    </select>

    <div>
      <center>Custom Text</center>
      <textarea
        id="custom-text"
        bind:value={customText}
        cols="55"
        rows="10"
        placeholder="Paste text to analyze (instead of default dataset)"
      />
    </div>

    <div>Effort vs Kedmanee : {effort}</div>
    <br />
    <div>Effort vs Kedmanee by set :</div>
    {#each effortBySet as set}
      <div>{set}%</div>
    {/each}
  </div>

  <script async defer src="https://buttons.github.io/buttons.js"></script>
</main>

<style lang="scss">
  @import "./styles.scss";
</style>
