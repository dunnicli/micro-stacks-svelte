<script>
  import "../app.css";
  export let title = "Svelte Micro-Stacks";
</script>

<div class="title">
  <a href="/">
    <img src="/logo1.svg" alt="site logo" />
  </a>
  <h1 class="text-3xl font-bold underline">{title}</h1>
</div>

<style>
  .title {
    display: flex;
    align-items: center;
  }

  .title img {
    display: inline-block;
  }
  .title h1 {
    margin-left: 20px;
    color: deepskyblue;
  }
</style>
