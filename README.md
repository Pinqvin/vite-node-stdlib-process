# Test repo for polyfilling Node process / Buffer

Some frontend libraries (at least older versions we are using in an old CRA project) tend to access Node Buffer / process by not importing it directly. While in CRA (webpack versions older than 5) will happily polyfill this for you, Vite will not.

To fix this in this example repo, I'm trying to use [vite-plugin-node-stdlib-browser](https://github.com/sodatea/vite-plugin-node-stdlib-browser). But this plugin just makes Buffer and process importable, but doesn't fix any modules that expect them to be globally available.

A naive solution one could try is to add process (and/or Buffer) to window:

```js
<script type="module">
  import process from "process";
  window.process = process;
</script>
```

And this works... sort of. It does break the dev server, so if you try running the application with `npm run dev`, you'll see an error like this:

```
Uncaught TypeError: (intermediate value).default.env is undefined
```

which gets thrown from React while trying to figure out if we should load the development or the production bundle. Clearly our solution is somehow interfering with Vite trying to make env variables available.