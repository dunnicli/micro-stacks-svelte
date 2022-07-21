<script lang="ts">
  import { getOpenContractCall } from "@micro-stacks/svelte";
  import { getAccount, getAuth } from "@micro-stacks/svelte";
  import {
    uintCV,
    intCV,
    bufferCV,
    stringAsciiCV,
    stringUtf8CV,
    standardPrincipalCV,
    trueCV,
  } from "micro-stacks/clarity";

  import {
    makeStandardSTXPostCondition,
    makeStandardNonFungiblePostCondition,
    makeContractNonFungiblePostCondition,
    NonFungibleConditionCode,
    createAssetInfo,
    FungibleConditionCode,
  } from "micro-stacks/transactions";

  $: sessionStatus = !$account.stxAddress
    ? "No active session"
    : $account.stxAddress;
  const auth = getAuth();

  const contractCall = getOpenContractCall();
  const account = getAccount();

  let recipient = "ST12H4ANQQ2NGN96KB0ZYVDG02NWT99A9TPE22SP9";
  let metadataUri = "";

  const clarityRecipient = standardPrincipalCV(recipient);
  const clarityUri = stringAsciiCV(metadataUri);

  const functionArgs = [clarityRecipient, clarityUri];

  // contract address for the asset contract
  const assetAddress = "ST12H4ANQQ2NGN96KB0ZYVDG02NWT99A9TPE22SP9";
  const postConditionAddress = $account.stxAddress!;
  const nftPostConditionCode = NonFungibleConditionCode.Owns;
  const assetContractName = "acatv4";
  const assetName = "acat";
  const tokenAssetName = stringAsciiCV("acat");

  const nonFungibleAssetInfo = createAssetInfo(
    assetAddress,
    assetContractName,
    assetName
  );

  const stxConditionCode = FungibleConditionCode.LessEqual;
  const stxConditionAmount = 100000000; // denoted in microstacks

  const postConditions = [
    makeStandardNonFungiblePostCondition(
      postConditionAddress,
      nftPostConditionCode,
      nonFungibleAssetInfo,
      tokenAssetName
    ),
    makeStandardSTXPostCondition(
      postConditionAddress,
      stxConditionCode,
      stxConditionAmount
    ),
  ];

  /**
  const postConditions = [
    makeStandardSTXPostCondition(
      $account.stxAddress!,
      FungibleConditionCode.LessEqual,
      "100"
    ),
  ];
  */

  const onClick = async () => {
    await $contractCall.openContractCall({
      contractAddress: assetAddress,
      contractName: "acatv4",
      functionName: "mint",
      functionArgs,
      postConditions,
      //attachment: "This is an attachment",
      onFinish: (data) => {
        console.log("finished contract call!", data);
        console.log("txId: ", data.txId);
      },
      onCancel: () => {
        console.log("popup closed!");
      },
    });
  };
</script>

<svelte:head>
  <title>NFT Mint Test</title>
</svelte:head>

<h1>Mint NFT Test</h1>

<div class="flex p-4">
  <div class="w-1/2 flex flex-col pb-12">
    <p>&nbsp;</p>
    <p>&nbsp;</p>
    Recipient<br />
    <input
      type="text"
      class="w-full p-2 border border-gray-300"
      name="recipient"
      placeholder="Recipient"
      bind:value={recipient}
    />
    <p>&nbsp;</p>
    Metadata URI<br />
    <input
      type="text"
      class="w-full p-2 border border-gray-300"
      name="metadataUri"
      placeholder="Metadata URI"
      bind:value={metadataUri}
    />
    <p>&nbsp;</p>

    <br />
    <button
      {onClick}
      class="font-bold mt-4 bg-green-500 text-white rounded p-4 shadow-lg"
    >
      Just Mint Create Stx NFT
    </button>
    <p>&nbsp;</p>
    <p>&nbsp;</p>
    <h1>TXID:</h1>
    <p>&nbsp;</p>
    <p>&nbsp;</p>
    <p>&nbsp;</p>
  </div>
</div>

<hr />
<p>&nbsp;</p>
<h2>{sessionStatus}</h2>
<h3>Wallet Connected? {$auth.isSignedIn}</h3>
