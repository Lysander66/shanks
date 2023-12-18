## Building

To create a production version of your app:

```bash
npm run build
```

You can preview the production build with `npm run preview`.

> To deploy your app, you may need to install an [adapter](https://kit.svelte.dev/docs/adapters) for your target environment.

## Preview

```sh
pnpm run build
rsync -avz .svelte-kit hk3:/var/www/shanks
scp vite.config.js svelte.config.js hk3:/var/www/shanks
```

> ssh hk3

package.json

```json
{
	"name": "shanks",
	"version": "0.0.1",
	"scripts": {
		"preview": "vite preview --host 0.0.0.0"
	},
	"devDependencies": {
		"@sveltejs/adapter-auto": "^2.0.0",
		"@sveltejs/kit": "^1.20.4",
		"vite": "^4.4.2"
	},
	"type": "module"
}
```
