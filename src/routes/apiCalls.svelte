<script context="module">
  import { geNetwork } from "@micro-stacks/svelte";
</script>

<script>
  import { callReadOnlyFunction } from "micro-stacks/api";

  import { uintCV } from "micro-stacks/clarity";

  let result = "";
  let tid = 0;
  let myresult = 0;
  let myuri = "";
  const network = geNetwork();

  async function getMetadata() {
    const contractAddress = "ST12H4ANQQ2NGN96KB0ZYVDG02NWT99A9TPE22SP9";
    const contractName = "acatv4";
    const functionName = "get-metaUri";
    //const functionArgs = [uintCV(tid)];
    const senderAddress = "ST12H4ANQQ2NGN96KB0ZYVDG02NWT99A9TPE22SP9";

    const options = {
      contractAddress: contractAddress,
      contractName: contractName,
      functionName: functionName,
      functionArgs: [uintCV(tid)],
      network: $network.setNetwork("testnet"),
      senderAddress: senderAddress,
    };

    // @ts-ignore
    const result = await callReadOnlyFunction(options);
    console.log("Result: ", result);
    // @ts-ignore
    myuri = result.value.data;
  }

  async function getTokenId() {
    const contractAddress = "ST12H4ANQQ2NGN96KB0ZYVDG02NWT99A9TPE22SP9";
    const contractName = "acatv4";
    const functionName = "get-last-token-id";
    const senderAddress = "ST12H4ANQQ2NGN96KB0ZYVDG02NWT99A9TPE22SP9";

    const options = {
      contractAddress: contractAddress,
      contractName: contractName,
      functionName: functionName,
      functionArgs: [],
      network: $network.setNetwork("testnet"),
      senderAddress: senderAddress,
    };

    // @ts-ignore
    const result = await callReadOnlyFunction(options);
    console.log("Result: ", result);
    // @ts-ignore
    console.log("Next TID: ", parseInt(result.value.value) + 1);

    // @ts-ignore
    console.log("More Result: ", result.value.value);
    // @ts-ignore
    myresult = result.value.value;
  }
</script>

<svelte:head>
  <title>API Calls</title>
</svelte:head>

<h1>API Calls</h1>

<div class="flex p-4">
  <div class="w-1/2 flex flex-col pb-12">
    <p>&nbsp;</p>
    <p>&nbsp;</p>
    <button
      class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
      on:click={getTokenId}
    >
      Get Last Token ID
    </button>

    <p>&nbsp;</p>
    <p>&nbsp;</p>
    <p>Result: {myresult}</p>
    <p>
      Next TID: {parseInt(
        // @ts-ignore
        myresult
      ) + 1}
    </p>
    <p>&nbsp;</p>
    <p>&nbsp;</p>
    <hr class="border-4" />
    <p>&nbsp;</p>
    <p>&nbsp;</p>
    <h1>Get Token Metadata URI</h1>
    <p>&nbsp;</p>
    Token ID<br />
    <input
      type="number"
      class="p-2 border border-gray-300"
      name="tid"
      placeholder="Token ID"
      bind:value={tid}
    />
    <p>&nbsp;</p>
    <button
      class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
      on:click={getMetadata}
    >
      Get Metadata URI
    </button>

    <p>&nbsp;</p>
    <p>&nbsp;</p>
    <p>
      URL:
      <u><a href={myuri}>{myuri}</a></u>
    </p>
    <p>&nbsp;</p>
    <p>&nbsp;</p>
  </div>
</div>
