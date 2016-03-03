imapfw is a framework. As such, its internals are a bit uncommon for most users. There's nothing much complex: we load the rascal as a python module and use its content (almost) like any other module.

# A brief introduction on imapfw's internals

Internally, there are some implementation concepts that worth to be aware of. The most important are:
* **concurrency**
* **workers**

## Concurrency

The **concurrency** is a module to use a single and very simple API whatever the Python backend in use (`multiprocessing` or `threading`). If you want to hack on this, bear in mind that while they expose very similar APIs, they come with subtle differences it's better to avoid introducing in the long run. This is a reason why we force using only a small subset of their API.

Because the concurrency backend is easily switchable, **worker** is a simple term to refer to either a process (`multiprocessing`) or a thread (`threading`).
