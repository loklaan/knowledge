# debugging

## node.js

### With `--inspect`

* Works with already installed chrome / chromium-based browser (brave)
* Permissive blackboxing by default
* Must be executed on every run & the debugger must reconnebt

```shell
node --inspect foo.js
node --inspect-brk foo.js
```

### With `ndb`

[GoogleChromeLabs/ndb](https://github.com/GoogleChromeLabs/ndb)

* Must be installed globally or with a node project
* Restrictive blackbox & no ui for adding to the file workspace
* Has ui for managing/rerunning processes & npm scripts
* Has advanced debugging & profiling for multi-process / clusters

```
ndb foo.js
```

### With `ndb` for monorepos

#### Steps:  
1. Initialise `nwb` with the monorepo's root dir as the cwd
2. Then, execute the intended js file


<details><summary><code>Example setup...</code></summary>

_Shell:_  
```shell
ndb support/ndb-init.js --script packages/cli/index.js --script-cwd packages/cli
```

_Project structure:_  
```text
├── support
│   └── ndb-init.js
└── packages
    └── cli/index.js
```

_`support/ndb-init.js`:_  
```js
#!/usr/bin/env node

/*
|-------------------------------------------------------------------------------
| NDB Init Script
|-------------------------------------------------------------------------------
| https://github.com/GoogleChromeLabs/ndb
|
| Setup ndb to use a higher root directory as the debugger's "workspace", so
| that all the project's files can be referenced in the file tree.
|
*/

const path = require('path');
const exec = require('child_process').execSync;

function main () {
	const cwd = process.cwd();

	const scriptWorkingDir = process.argv[ process.argv.findIndex(v => v === '--script-cwd') + 1 ];
	if (!scriptWorkingDir) throw new Error('Require "--script-cwd" argument.');
	const scriptFilePath = process.argv[ process.argv.findIndex(v => v === '--script') + 1 ];
	if (!scriptFilePath) throw new Error('Require "--script" argument.');

	exec(path.resolve(cwd, scriptFilePath), { cwd: path.resolve(cwd, scriptWorkingDir) });
}

try {
  main();
} catch (err) {
  console.error(err);
  process.exit(1);
}
```

</details>
