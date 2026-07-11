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

  function onclick() {
    if (result !== undefined) {
      numLeft = result.toString();
      numRight = "";
      op = undefined;
      result = undefined;
    }
  }

  function onDigitClick(digit: string) {
    onclick();
    if (op) {
      numRight = appendDigit(numRight, digit);
    } else {
      numLeft = appendDigit(numLeft, digit);
    }
  }

  function onClear() {
    onclick();
    numLeft = "0";
    numRight = "";
    op = undefined;
    result = undefined;
  }

  function onDelete() {
    onclick();
    if (numRight) {
      numRight = deleteDigit(numRight, false);
    } else if (op) {
      op = undefined;
    } else {
      numLeft = deleteDigit(numLeft, true);
    }
  }

  function onOpClick(opClicked: Op) {
    onclick();
    if (opClicked === "-" && !op) {
      if (numLeft === "0") {
        numLeft = "-0";
      } else if (numLeft === "-0") {
        numLeft = "0";
      } else {
        op = opClicked;
      }
    } else {
      op = opClicked;
    }
  }

  function onEqualClick() {
    onclick();
    if (op && numRight !== "") {
      result = calculate(numLeft, numRight, op);
    }
  }

  function onDotClick() {
    onclick();
    if (op) {
      if (numRight && !numRight.includes(".")) numRight += ".";
    } else {
      if (!numLeft.includes(".")) numLeft += ".";
    }
  }
</script>

<div
  style:width="544px"
  style:height="714px"
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
    <Key position="top-left" onclick={() => onDelete()}><Delete size={70} /></Key>
    <Key position="top" onclick={() => onClear()}>AC</Key>
    <Key position="top-right" onclick={() => onOpClick("+")}>+</Key>
    <Key position="left" onclick={() => onDigitClick("1")}>1</Key>
    <Key onclick={() => onDigitClick("2")}>2</Key>
    <Key position="right" onclick={() => onOpClick("-")}>-</Key>
    <Key position="left" onclick={() => onDigitClick("3")}>3</Key>
    <Key onclick={() => onDigitClick("4")}>4</Key>
    <Key position="right" onclick={() => onOpClick("*")}>*</Key>
    <Key position="left" onclick={() => onDigitClick("5")}>5</Key>
    <Key onclick={() => onDigitClick("6")}>6</Key>
    <Key position="right" onclick={() => onOpClick("/")}>/</Key>
    <Key position="left" onclick={() => onDigitClick("7")}>7</Key>
    <Key onclick={() => onDigitClick("8")}>8</Key>
    <Key position="right" onclick={() => onEqualClick()}>=</Key>
    <Key position="bottom-left" onclick={() => onDigitClick("9")}>9</Key>
    <Key position="bottom" onclick={() => onDigitClick("0")}>0</Key>
    <Key position="bottom-right" onclick={() => onDotClick()}>.</Key>
  </div>
</div>
