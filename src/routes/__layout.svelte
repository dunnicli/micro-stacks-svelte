<script context="module">
  export async function load({ session }) {
    const dehydratedState = session.dehydratedState;
    return {
      props: { dehydratedState },
    };
  }
</script>

<script lang="ts">
  import "../app.css";
  import Title from "$lib/title.svelte";
  import { mountClient } from "@micro-stacks/svelte";
  import { onPersistState, onSignOut } from "../common/fetchers";

  export let dehydratedState: string | null = null;
  mountClient({
    appName: "svelte",
    appIconUrl: "/",
    onPersistState,
    onSignOut,
    dehydratedState,
  });
</script>

<header>
  <nav>
    <Title title="Svelte Micro-Stacks " />
    <div class="links">
      <a href="/">Home</a>
      <a href="/mintNft">Mint NFT</a>
      <a href="/apiCalls">API Calls</a>
      <a href="/checkMinter">Verify Minter</a>
      <a href="/addMinter">Add Minter</a>
      <a href="/deleteMinter">Delete Minter</a>
      <a href="/about">About</a>
    </div>
  </nav>
</header>

<main class="main">
  <slot />
</main>

<footer>
  <!--  Footer Content -->
  Copyright &copy; 2022 JTD MX
</footer>

<style>
  header {
    text-align: center;
    justify-content: center;
    background: #fff;
    padding: 10px;
    border: 1px solid #eee;
    margin: 20px auto;
  }
  links {
    margin: left auto;
  }
  a {
    margin-left: 10px;
  }
  nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
    max-width: 960px;
    margin: 0 auto;
  }
  main {
    max-width: 960px;
    padding: 20px;
    border: 1px solid deepskyblue;
    margin: 20px auto;
  }
  footer {
    text-align: center;
    background: #fafafa;
    padding: 20px;
    border: 1px solid #eee;
    margin: 20px auto;
  }
</style>
