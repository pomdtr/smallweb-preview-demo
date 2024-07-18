# Preview URLs with Smallweb

All commit in this repository can be referenced by a preview URL.

Ex:

- main branch: <https://preview-demo-ab05ff2.smallweb.run>
- 1st commit: <https://preview-demo-835698d.smallweb.run>
- PR branch: <https://preview-952ab79.smallweb.run>

## How does it work ?

```jsonc
// ~/.config/smallweb/config.json
{
    "domains": {
        "preview-demo-*.smallweb.run": "~/smallweb.run/preview-demo"
    }
}
```

```ts
// ~/smallweb.run/preview-demo/main.ts

const repo = "pomdtr/smallweb-preview-url-demo";

export default {
    async fetch(req: Request) {
        const { hostname } = new URL(req.url);
        const [subdomain] = hostname.split(".");
        const ref = subdomain.split("-").pop();
        const { default: handler } = await import(
            `https://raw.githubusercontent.com/${repo}/${ref}/main.ts`
        );

        return handler.fetch(req);
    },
};
```
