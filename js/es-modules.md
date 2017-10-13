# javascript modules

## best practice

_Avoid using `export default`_

* named exports are easier to track
* implicit behaviours around `default` for export chaining are confusing
* requiring es module default into cjs is jarring

## es module syntax

**import**
```js
import "module-name";
import * as name from "module-name";
import { member } from "module-name";
import { member as alias } from "module-name";
import { member1 , member2 } from "module-name";
import { member1 , member2 as alias } from "module-name";

import defaultMember from "module-name";
import defaultMember, * as name from "module-name";
import defaultMember, { member } from "module-name";
```

**export**

```js
export { name };
export { value as name };
export const name = value;
export * from 'module-name';
export { member } from  'module-name';
export { member as name } from  'module-name';

export default expression;
export default function (…) { … }
export { name as default };
```
