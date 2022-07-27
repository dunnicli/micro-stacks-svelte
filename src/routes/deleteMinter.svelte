<script>
  //import { callReadOnlyFunction } from "micro-stacks/api";
  import { getOpenContractCall, geNetwork } from "@micro-stacks/svelte";

  import { stringAsciiCV, standardPrincipalCV } from "micro-stacks/clarity";

  import {
    makeStandardSTXPostCondition,
    //createAssetInfo,
    FungibleConditionCode,
  } from "micro-stacks/transactions";

  //let result = "";
  let stxAddress = "";
  //let minterNotes = "";
  //let myresult = "";
  const network = geNetwork();

  //const auth = getAuth();

  const contractCall = getOpenContractCall();
  //const account = getAccount();

  async function deleteMinter() {
    const contractAddress = "ST12H4ANQQ2NGN96KB0ZYVDG02NWT99A9TPE22SP9";
    const senderAddress = "ST12H4ANQQ2NGN96KB0ZYVDG02NWT99A9TPE22SP9";
    const clarityMinter = standardPrincipalCV(stxAddress);
    //const clarityNotes = stringAsciiCV(minterNotes);

    const postConditionAddress = senderAddress;
    const stxConditionCode = FungibleConditionCode.LessEqual;
    const stxConditionAmount = 100000000; // denoted in microstacks

    const postConditions = [
      makeStandardSTXPostCondition(
        postConditionAddress,
        stxConditionCode,
        stxConditionAmount
      ),
    ];
    //const functionArgs = [clarityMinter, clarityNotes];

    /**
      const options = {
        contractAddress: contractAddress,
        contractName: contractName,
        functionName: functionName,
        functionArgs: functionArgs,
        network: $network.setNetwork("testnet"),
        senderAddress: senderAddress,
      };
      */

    const options = {
      contractAddress: contractAddress,
      contractName: "acatv4",
      functionName: "delete-minter",
      functionArgs: [clarityMinter],
      network: $network.setNetwork("testnet"),
      postConditions,
      //appDetails: {
      //name: "acatv4",
      //icon: window.location.origin + "/vercel.svg",
      //},
      // @ts-ignore
      onFinish: (data) => {
        console.log("Stacks Transaction:", data.stacksTransaction);
        console.log("Transaction ID:", data.txId);
        console.log("Raw transaction:", data.txRaw);
      },
    };

    await $contractCall.openContractCall(options);
  }
</script>

<svelte:head>
  <title>Delete Minter</title>
</svelte:head>

<h1>Delete Minter</h1>

<div class="flex p-4">
  <div class="w-1/2 flex flex-col pb-12">
    <p>&nbsp;</p>
    <p>&nbsp;</p>

    <p>&nbsp;</p>
    <p>&nbsp;</p>
    <hr class="border-4" />
    <p>&nbsp;</p>
    <p>&nbsp;</p>
    <h1>Delete Minter</h1>
    <p>&nbsp;</p>
    Minter STX Address<br />
    <input
      type="text"
      class="p-2 border border-gray-300"
      name="tid"
      placeholder="Minter STX Address"
      bind:value={stxAddress}
    />
    <p>&nbsp;</p>
    <p>&nbsp;</p>
    <p>&nbsp;</p>

    <button
      class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
      on:click={deleteMinter}
    >
      Delete Minter
    </button>

    <p>&nbsp;</p>
    <p>&nbsp;</p>
    <p>Response:</p>
    <p>&nbsp;</p>
    <p>&nbsp;</p>
  </div>
</div>
