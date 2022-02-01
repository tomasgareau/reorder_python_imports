# reorder-python-imports

You're probably looking for [asottile/reorder_python_imports](https://www.github.com/asottile/reorder_python_imports). This is just a fork that provides a flag to allow "implicit" duplicates (while still removing explicit duplicates) and I'm not planning to maintain it super heavily.

For example, the original `reorder_python_imports` would turn this:

```python
import asyncio
import zmq
import zmq.asyncio
import asyncio
```

into this:

```python
import asyncio

import zmq.asyncio
```

whereas this fork, with the `--allow-implicit-duplicates` flag, will produce:

```python
import asyncio

import zmq
import zmq.asyncio
```

## Use-case
I was bumping into this using both Pyright & reorder-python-imports at the same time.

When reorder-python-imports removed the implicity duplicated `zmq` import, Pyright would warn me that symbols from `zmq` (e.g., `zmq.PUSH`) were undefined. This is intended behaviour from Pyright:

> You shouldn't rely on cross-module temporal ordering. If you want to access symbols from module, you should include an import module statement in your code. If you want to access symbols from a submodule, you should either import that submodule or the top-level module should re-export the symbol. As a static type checker, pyright enforces that. The Python runtime is very "loose" in this respect, and that allows developers to (often inadvertently) rely on cross-module implicit initialization, but this leads to code fragility. Explicit is better than implicit.

but reorder-python-imports prefers to stick to Python's current behaviour:

> sorry no -- pyright is not doing the correct thing here. they should make their checker compliant with python

I personally agree with Pyright's approach here, hence this fork.
