import { useState, useRef, useEffect, useCallback } from "react";
import Link from "next/link";
import Head from "next/head";
import {
  standardPrincipalCV,
  makeStandardSTXPostCondition,
  stringAsciiCV,
  FungibleConditionCode,
} from "@stacks/transactions";
import {
  AppConfig,
  UserSession,
  showConnect,
  openContractCall,
} from "@stacks/connect";
import { StacksMocknet, StacksTestnet } from "@stacks/network";

export default function AddMinter() {
  const appConfig = new AppConfig(["publish_data"]);
  const userSession = new UserSession({ appConfig });
  const [error, setError] = useState("");
  const [loading, setLoading] = useState(true);
  const [userData, setUserData] = useState({});
  const [loggedIn, setLoggedIn] = useState(false);

  const [minter, setMinter] = useState("");
  const [notes, setNotes] = useState("");

  const handleMinterChange = (e) => {
    setMinter(e.target.value);
  };

  const handleNotesChange = (e) => {
    setNotes(e.target.value);
  };

  const network = new StacksMocknet();

  const handleSubmit = async (e) => {
    e.preventDefault();
    const clarityMinter = standardPrincipalCV(minter);
    const clarityNotes = stringAsciiCV(notes);
    const contractAddress = "ST12H4ANQQ2NGN96KB0ZYVDG02NWT99A9TPE22SP9";
    //const contractAddress = "ST3H0F71SQXP2APJX29HBQN4FAZP5H0W564KD9ZDS";

    const postConditionAddress =
      userSession.loadUserData().profile.stxAddress.testnet;
    const stxConditionCode = FungibleConditionCode.LessEqual;
    const stxConditionAmount = 100000000; // denoted in microstacks

    const postConditions = [
      makeStandardSTXPostCondition(
        postConditionAddress,
        stxConditionCode,
        stxConditionAmount
      ),
    ];
    const functionArgs = [clarityMinter, clarityNotes];

    const options = {
      contractAddress: contractAddress,
      contractName: "acatv4",
      functionName: "add-minter",
      functionArgs,
      network,
      postConditions,
      appDetails: {
        name: "acatv4",
        icon: window.location.origin + "/vercel.svg",
      },
      onFinish: (data) => {
        console.log("Stacks Transaction:", data.stacksTransaction);
        console.log("Transaction ID:", data.txId);
        console.log("Raw transaction:", data.txRaw);
      },
    };

    await openContractCall(options);
  };

  function authenticate() {
    showConnect({
      appDetails: {
        name: "Acat V4",
        icon: "",
      },
      redirectTo: "/",
      onFinish: () => {
        window.location.reload();
      },
      userSession,
    });
  }

  useEffect(() => {
    if (userSession.isSignInPending()) {
      userSession.handlePendingSignIn().then((userData) => {
        setUserData(userData);
      });
    } else if (userSession.isUserSignedIn()) {
      setLoggedIn(true);
      setUserData(userSession.loadUserData());
    }
  }, []);

  return (
    <div className="flex flex-col items-center justify-center min-h-screen py-2">
      <Head>
        <title>Add Approved Minter</title>
        <link rel="icon" href="/favicon.ico" />
      </Head>
      <main className="flex flex-col items-center justify-center w-full flex-1 px-20 text-center">
        <div className="flex flex-col w-full items-center justify-center">
          <h1 className="text-6xl font-bold mb-10">Add Approved Minter</h1>
          <p>
            <Link href="/"> Home</Link>
          </p>
          <p>&nbsp;</p>
          {loggedIn ? (
            <form
              onSubmit={handleSubmit}
              className="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4"
            >
              <p>
                <b>Minter STX Address</b>
                <br />
                <input
                  className="p-6 border rounded mx-2"
                  type="text"
                  value={minter}
                  onChange={handleMinterChange}
                  placeholder="Minters STX Address"
                />
              </p>
              <p>&nbsp;</p>
              <p>
                <b>Notes</b>
                <br />
                <input
                  className="p-6 border rounded mx-2"
                  type="text"
                  value={notes}
                  onChange={handleNotesChange}
                  placeholder="Notes here"
                />
              </p>
              <p>
                <button
                  type="submit"
                  className="p-6 bg-green-500 text-white mt-8 rounded"
                >
                  Add New Minter
                </button>
              </p>
            </form>
          ) : (
            <button
              className="bg-white-500 hover:bg-gray-300 border-black border-2 font-bold py-2 px-4 rounded mb-6"
              onClick={() => authenticate()}
            >
              Connect to Wallet
            </button>
          )}
        </div>
      </main>
    </div>
  );
}
