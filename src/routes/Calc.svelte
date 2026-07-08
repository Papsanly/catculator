<script lang="ts">
  import Delete from "./Delete.svelte";
  import Key from "./Key.svelte";

  const PRECISION = 7;

  type Op = "+" | "-" | "*" | "/";

  let op: Op | undefined = $state();
  let numLeft: string = $state("0");
  let numRight: string = $state("");
  let result: number | undefined = $state();

  function calculate(numLeftStr: string, numRightStr: string, op: Op) {
    let numLeft = Number(numLeftStr);
    let numRight = Number(numRightStr);
    let result = 0;
    if (op === "+") {
      result = numLeft + numRight;
    } else if (op === "-") {
      result = numLeft - numRight;
    } else if (op === "*") {
      result = numLeft * numRight;
    } else if (op === "/") {
      result = numLeft / numRight;
    }
    return Math.round(result * 10 ** PRECISION) / 10 ** PRECISION;
  }

  function appendDigit(num: string, digit: string) {
    if (num === "0") {
      return digit;
    } else if (num === "-0") {
      return "-" + digit;
    } else {
      return num + digit;
    }
  }

  function deleteDigit(num: string, isNumLeft: boolean) {
    let str = num.slice(0, -1);
    if (isNumLeft && str === "") {
      return "0";
    } else if (str === "-") {
      return "-0";
    } else {
      return str;
    }
  }

  function onclick(action: string) {
    if (result !== undefined) {
      numLeft = result.toString();
      numRight = "";
      op = undefined;
      result = undefined;
    }
    if (["0", "1", "2", "3", "4", "5", "6", "7", "8", "9"].includes(action)) {
      if (op) {
        numRight = appendDigit(numRight, action);
      } else {
        numLeft = appendDigit(numLeft, action);
      }
    } else if (action === "delete") {
      if (numRight) {
        numRight = deleteDigit(numRight, false);
      } else if (op) {
        op = undefined;
      } else {
        numLeft = deleteDigit(numLeft, true);
      }
    } else if (action === "clear") {
      numLeft = "0";
      numRight = "";
      op = undefined;
      result = undefined;
    } else if (["+", "-", "*", "/"].includes(action)) {
      if (action === "-" && !op) {
        if (numLeft === "0") {
          numLeft = "-0";
        } else if (numLeft === "-0") {
          numLeft = "0";
        } else {
          op = action as Op;
        }
      } else {
        op = action as Op;
      }
    } else if (action === "=") {
      if (op && numRight !== "") {
        result = calculate(numLeft, numRight, op);
      }
    } else if (action === ".") {
      if (op) {
        if (numRight && !numRight.includes(".")) numRight += ".";
      } else {
        if (!numLeft.includes(".")) numLeft += ".";
      }
    }
  }
</script>

<div
  style:width="544px"
  style:height="773px"
  style:flex-direction="column"
  style:align-items="end"
  style:justify-content="end"
  style:gap="10px"
>
  <div style:font-size="42pt" style:white-space="nowrap">
    {#if result !== undefined}
      {numLeft} {op} {numRight}
    {/if}
  </div>
  <div style:font-size="64pt" style:white-space="nowrap">
    {#if result !== undefined}
      {result}
    {:else}
      {numLeft}
      {op}
      {#if numRight}{numRight}{/if}
    {/if}
  </div>
  <div
    style:display="grid"
    style:grid-template-columns="repeat(3, auto)"
    style:grid-template-rows="repeat(6, auto)"
    style:justify-items="center"
    style:align-items="center"
  >
    <Key position="top-left" onclick={() => onclick("delete")}><Delete size={70} /></Key>
    <Key position="top" onclick={() => onclick("clear")}>AC</Key>
    <Key position="top-right" onclick={() => onclick("+")}>+</Key>
    <Key position="left" onclick={() => onclick("1")}>1</Key>
    <Key onclick={() => onclick("2")}>2</Key>
    <Key position="right" onclick={() => onclick("-")}>-</Key>
    <Key position="left" onclick={() => onclick("3")}>3</Key>
    <Key onclick={() => onclick("4")}>4</Key>
    <Key position="right" onclick={() => onclick("*")}>*</Key>
    <Key position="left" onclick={() => onclick("5")}>5</Key>
    <Key onclick={() => onclick("6")}>6</Key>
    <Key position="right" onclick={() => onclick("/")}>/</Key>
    <Key position="left" onclick={() => onclick("7")}>7</Key>
    <Key onclick={() => onclick("8")}>8</Key>
    <Key position="right" onclick={() => onclick("=")}>=</Key>
    <Key position="bottom-left" onclick={() => onclick("9")}>9</Key>
    <Key position="bottom" onclick={() => onclick("0")}>0</Key>
    <Key position="bottom-right" onclick={() => onclick(".")}>.</Key>
  </div>
</div>
