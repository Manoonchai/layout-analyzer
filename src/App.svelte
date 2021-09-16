<script lang="ts">
  const layoutDataDisplay = [
    [
      ["1", "!", "๑"],
      ["2", "@", "๒"],
      ["3", "#", "๓"],
      ["4", "$", "๔"],
      ["5", "%", "๕"],
      ["6", "^", "๖"],
      ["7", "&", "๗"],
      ["8", "*", "๘"],
      ["9", "(", "๙"],
      ["0", ")", "๐"],
      ["-", "_", "÷"],
      ["=", "+", "×"],
    ],
    [
      ["ใ", "ฒ", " "],
      ["ต", "ฏ", " "],
      ["ห", "ซ", " "],
      ["ล", "ญ", " "],
      ["ส", "ฟ", " "],
      ["ป", "ฉ", " "],
      ["ั", "ึ", "ฺ"],
      ["ก", "ธ", " "],
      ["ิ", "ฐ", " "],
      ["บ", "ฎ", " "],
      ["็", "ฆ", "["],
      ["ฬ", "ฑ", "]"],
      ["ฯ", "ฌ", "\\"],
    ],
    [
      ["ง", "ษ", "◌"],
      ["เ", "ถ", "๏"],
      ["ร", "แ", "๛"],
      ["น", "ช", "฿"],
      ["ม", "พ", " "],
      ["อ", "ผ", "ํ"],
      ["า", "ำ", "ๅ"],
      ["่", "ข", "ฃ"],
      ["้", "โ", " "],
      ["ว", "ภ", ";"],
      ["ื", '"', "'"],
    ],
    [
      ["ุ", "ฤ", "ฦ"],
      ["ไ", "ฝ", " "],
      ["ท", "ๆ", "๚"],
      ["ย", "ณ", " "],
      ["จ", "๊", " "],
      ["ค", "๋", "ฅ"],
      ["ี", "์", "๎"],
      ["ด", "ศ", ","],
      ["ะ", "ฮ", "."],
      ["ู", "?", "/"],
    ],
  ]
</script>

<main>
  <div class="container">
    <div class="keyboard-container">
      <div class="keyboard">
        <!-- First row -->
        <div class="key non-alphabets">
          <div class="key__top swappable">~</div>
          <div class="key__bottom swappable">`</div>
        </div>
        {#each layoutDataDisplay[0] as keypair}
          <div class="key">
            <div class="key__top swappable">
              {keypair[1]}
            </div>
            <div class="key__right swappable">
              {keypair[2]}
            </div>
            <div class="key__bottom swappable">
              {keypair[0]}
            </div>
          </div>
        {/each}
        <div class="key non-alphabets is-backspace">
          <div class="key__bottom">&larr;</div>
        </div>
        <!-- Second row -->
        <div class="key non-alphabets is-tab">
          <div class="key__bottom">&rarrb;</div>
        </div>
        {#each layoutDataDisplay[1] as keypair}
          <div class="key">
            <div class="key__top swappable">
              {keypair[1]}
            </div>
            <div class="key__right swappable">
              {keypair[2]}
            </div>
            <div class="key__bottom swappable">
              {keypair[0]}
            </div>
          </div>
        {/each}
        <!-- Third row -->
        <div class="key non-alphabets is-capslock is-left">
          <div class="key__bottom">ก/A</div>
        </div>
        {#each layoutDataDisplay[2] as keypair}
          <div
            class="key {'งเรนา่้ว'.split('').includes(keypair[0])
              ? 'home-key'
              : ''}"
          >
            <div class="key__top swappable">
              {keypair[1]}
            </div>
            <div class="key__right swappable">
              {keypair[2]}
            </div>
            <div class="key__bottom swappable">
              {keypair[0]}
            </div>
          </div>
        {/each}
        <div class="key non-alphabets is-enter is-right">
          <div class="key__top" />
          <div class="key__bottom">&larrhk;</div>
        </div>
        <!-- Fourth row -->
        <div class="key non-alphabets is-shift-left is-left">
          <div class="key__bottom">⬆</div>
        </div>
        {#each layoutDataDisplay[3] as keypair}
          <div class="key">
            <div class="key__top swappable">
              {keypair[1]}
            </div>
            <div class="key__right swappable">
              {keypair[2]}
            </div>
            <div class="key__bottom swappable">
              {keypair[0]}
            </div>
          </div>
        {/each}
        <div class="key non-alphabets is-shift-right is-right">
          <div class="key__bottom">⬆</div>
        </div>
        <!-- Fifth row -->
        <div class="key non-alphabets is-left">
          <div class="key__bottom">fn</div>
        </div>
        <div class="key non-alphabets is-right">
          <div class="key__bottom">control</div>
        </div>
        <div class="key non-alphabets is-right">
          <div class="key__top" />
          <div class="key__bottom" />
        </div>
        <div class="key non-alphabets is-command is-right">
          <div class="key__bottom">alt</div>
        </div>
        <div class="key non-alphabets is-space" />
        <div class="key non-alphabets is-command is-left">
          <div class="key__bottom">alt</div>
        </div>
        <div class="key non-alphabets is-left">
          <div class="key__top" />
          <div class="key__bottom" />
        </div>
        <div class="key non-alphabets is-arrow-left">&ltrif;</div>
        <div class="key non-alphabets is-arrow-up">&utrif;</div>
        <div class="key non-alphabets is-arrow-down">&dtrif;</div>
        <div class="key non-alphabets is-arrow-right">&rtrif;</div>
      </div>
    </div>
  </div>
</main>

<style lang="scss">
  @import "./styles.scss";
</style>
