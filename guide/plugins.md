# Plugins

Add or customize features through plugins.

Currently, plugins can only be manually added at build time and they are not dynamically imported. In the future, this will change.

## Layout

Always a `package.json`, `tsconfig.json`. And, an `_isomorphic.ts` file, along with a `_client.ts` and/or a `_server.ts`.

With contents like:

```jsonc
// package.json
{
	"name": "@quasipanacea/plugin-<pluginFamily>-<id>",
	"dependencies": {
		"@quasipanacea/common": "workspace:^",
		"@quasipanacea/plugin-utility": "workspace:^"
	},
	"devDependencies": {
		"vue": "^3.0.0",
		"vue-router": "^4.0.0"
	},
	"peerDependencies": {
		"vue": "^3.0.0",
		"vue-router": "^4.0.0"
	}
}
```

```jsonc
// tsconfig.json
{
	"extends": "@quasipanacea/common/tsconfig.json"
}
```

```ts
// client.ts
import { plugin } from '@quasipanacea/common/server/index.ts'

import { metadata } from './_isomorphic.ts'
import { default as component } from './<PluginFamily><Id>.vue'

export async function init() {
	plugin.register({
		metadata,
		component,
	})
}
```

```ts
// server.ts
import { plugin } from '@quasipanacea/common/server/index.ts'

import { metadata } from './_isomorphic.ts'
import * as exports from './<pluginFamily><Id>.ts'

export async function init() {
	plugin.register({
		metadata,
		...exports,
	})
}
```