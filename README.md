## SvelteKit and Micro-Stacks

I created this app to prototype the incorporation of micro-stacks into a Sveltekit app.

It works I'm happy to say. I started with this repo from fungible-systems:

https://github.com/fungible-systems/micro-stacks-examples/tree/main/examples/with-sveltekit

Our main app is an online art gallery (real paintings). The artists have asked if they could
create nfts and so here we are. Only the artist can create nfts.

Everything here works. You will have to deploy your own contract to testnet. Feel free to copy and use anything.

Also, I'm not a typescript fan and avoid it whenever possible. However, the repo I built this from is typescript so I have to live with it in this context.

# create-svelte

Everything you need to build a Svelte project, powered by [`create-svelte`](https://github.com/sveltejs/kit/tree/master/packages/create-svelte).

## Creating a project

If you're seeing this, you've probably already done this step. Congrats!

```bash
# create a new project in the current directory
npm init svelte

# create a new project in my-app
npm init svelte my-app
```

## Developing

Once you've created a project and installed dependencies with `npm install` (or `pnpm install` or `yarn`), start a development server:

```bash
npm run dev

# or start the server and open the app in a new browser tab
npm run dev -- --open
```

## Building

To create a production version of your app:

```bash
npm run build
```

You can preview the production build with `npm run preview`.

> To deploy your app, you may need to install an [adapter](https://kit.svelte.dev/docs/adapters) for your target environment.
