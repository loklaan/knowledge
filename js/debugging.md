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
| NDB Harness Script
|-------------------------------------------------------------------------------
| https://github.com/GoogleChromeLabs/ndb
|
| Sets up ndb to use the monorepo as the debugger's "workspace", so that files
| in all packages can be referenced in the file tree.
|
| Note: Multiple of the same file might appear in the Tabs, but it doesn't
|       affect the tools functionality.
|
*/

const path = require('path');
const exec = require('child_process').execSync;

const REST_ARGS_DENOM = '--';

function getArgValueFromArgv(arg, argv) {
  const position = argv.findIndex(v => v === arg);
  return position > -1 ? argv[position + 1] : undefined;
}

function main() {
  const workingDir = path.resolve(
    process.cwd(),
    getArgValueFromArgv('--cwd', process.argv) || process.cwd()
  );

  const restArgs = process.argv.slice(
    process.argv.findIndex(v => v === REST_ARGS_DENOM) + 1
  );
  if (!process.argv.includes(REST_ARGS_DENOM) || restArgs.length < 1)
    throw new Error(`Requires the command to be after a "${REST_ARGS_DENOM}".`);

  exec(restArgs.join(' '), { cwd: workingDir, stdio: 'pipe' });
}

try {
  main();
} catch (err) {
  console.error(err);
  process.exit(1);
}
```

</details>
