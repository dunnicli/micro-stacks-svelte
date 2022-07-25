<script context="module">
  import { geNetwork } from "@micro-stacks/svelte";
</script>

<script>
  import { callReadOnlyFunction } from "micro-stacks/api";

  import { standardPrincipalCV } from "micro-stacks/clarity";

  const network = geNetwork();

  export let thatMinter = false;
  export let resultMinter = "";
  export let theMinter = "";

  async function getTheMinter() {
    const contractAddress = "ST12H4ANQQ2NGN96KB0ZYVDG02NWT99A9TPE22SP9";
    const contractName = "acatv4";
    const functionName = "get-minter";
    const buffer = standardPrincipalCV(theMinter);
    const senderAddress = "ST12H4ANQQ2NGN96KB0ZYVDG02NWT99A9TPE22SP9";

    const options = {
      contractAddress: contractAddress,
      contractName: contractName,
      functionName: functionName,
      functionArgs: [buffer],
      network: $network.setNetwork("testnet"),
      senderAddress: senderAddress,
    };

    // @ts-ignore
    const result = await callReadOnlyFunction(options);
    console.log("Result: ", result);

    console.log("More Result: ", JSON.stringify(result));

    // @ts-ignore
    if (result.type == 10) {
      // @ts-ignore
      resultMinter = result.value.data;
      thatMinter = true;
      console.log("Minter: ", resultMinter);
    }

    if (result.type == 9) {
      // @ts-ignore
      console.log("No Minter Found!!: ");
      thatMinter = false;
    }
  }
</script>

<svelte:head>
  <title>Verify Minter Status</title>
</svelte:head>

<h1>Verify Minter Status</h1>

<div class="flex p-4">
  <div class="w-1/2 flex flex-col pb-12">
    <p>&nbsp;</p>
    <p>&nbsp;</p>
    Minter Address<br />
    <input
      type="text"
      class="w-full p-2 border border-gray-300"
      name="theMinter"
      placeholder="Minter Address"
      bind:value={theMinter}
    />
    <p>&nbsp;</p>

    <p>&nbsp;</p>

    <br />
    <button
      class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
      on:click={getTheMinter}
    >
      Verify Minter Status
    </button>

    <p>&nbsp;</p>
    {#if thatMinter === true}
      <h1 class="text-3xl font-semibold">Confirmed: Is A Minter</h1>
      <h1>{resultMinter}</h1>
    {:else}
      <h1 class="text-3xl font-semibold">Not A Minter</h1>
    {/if}
    <p>&nbsp;</p>
  </div>
</div>
