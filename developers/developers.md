imapfw is a framework. As such, its internals are a bit uncommon for most users. There's nothing much complex: we load the rascal as a python module and use its content (almost) like any other module.

# A brief introduction about imapfw's internals

Internally, there are other implementation concepts that some of you might like to be aware of. The most important are the **concurrency**, the **workers** and the **managers**.

The **concurrency** is a module to use a single and very simple API whatever the Python backend in use (`multiprocessing` or `threading`). Yes, I'm aware they are already very similar but they still have some subtil differences it's better to avoid introducing in the long run. We force using a subset of them.

Because the conccurency backend is easily switchable, **worker** is a simple term to refer to either a process (`multiprocessing`) or a thread (`threading`).

The **managers** are objects to nicely handle concurrency. It takes inspiration from the managers of the `multiprocessing` library. They allow to have an intuitive usage for the "passing by message" design.
