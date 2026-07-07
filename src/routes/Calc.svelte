<script lang="ts">
  import Delete from "./Delete.svelte";
  import Key from "./Key.svelte";
</script>

<div style:flex-direction="column" style:align-items="end" style:gap="10px">
  <div style:font-size="42pt">123 + 456</div>
  <div style:font-size="64pt">579</div>
  <div
    style:display="grid"
    style:grid-template-columns="repeat(3, auto)"
    style:grid-template-rows="repeat(6, auto)"
    style:justify-items="center"
    style:align-items="center"
    style:grid-gap="10px"
  >
    <Key><Delete size={70} /></Key>
    <Key>AC</Key>
    <Key>+</Key>
    <Key>1</Key>
    <Key>2</Key>
    <Key>-</Key>
    <Key>3</Key>
    <Key>4</Key>
    <Key>*</Key>
    <Key>5</Key>
    <Key>6</Key>
    <Key>/</Key>
    <Key>7</Key>
    <Key>8</Key>
    <Key>=</Key>
    <Key>9</Key>
    <Key>0</Key>
    <Key>.</Key>
  </div>
</div>
