<script>
  import { getAccount, getAuth } from "@micro-stacks/svelte";
  const account = getAccount();
  $: sessionStatus = !$account.stxAddress
    ? "No active session"
    : $account.stxAddress;
  const auth = getAuth();
</script>

<h1>This is a test about page.</h1>

<p>Let's load something from the session.</p>
<p>&nbsp;</p>
<h1><a href="/"><u>Home</u></a></h1>
<h2>{sessionStatus}</h2>
<h3>Connected? {$auth.isSignedIn}</h3>
