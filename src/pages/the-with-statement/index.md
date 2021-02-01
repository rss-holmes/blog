---
title: 'The With Statement (Context Managers)'
date: '2020-11-02'
spoiler: Managing context for code blocks.
---

# The "With" Statement(Context Managers)

- used where a functionality includes a start and an exit(the period in between is called the context)

```python
# examples

# when opening and reading a file

    f = open('<filename>', w)
    try :
        f.read()
    finally:
        f.close()

# when starting a work in a thread

    some_lock = threading.acquire()
    some_lock.acquire()
    try :
        // do something
    finally:
        some_lock.release()

# using with

    with open('filename', w)
        // do something

    with threading.acquire()
        // do something

# writing a custom file open functionality which can be used with "with"

    # class based

    class ManagedFile:
        def __init__(self, name):
            self.name = name

        def __enter__(self):
            # perform all the actions required to start the context and
            # instantiate variables which will be available in the context
            self.file = open(self.name, 'w')
            return self.file

        def __exit__(self, exc_type, exc_val, exc_tb):
            # perform all actions required to exit the context in a safe
            # manner ie closing all open variables
            if self.file:
                self.file.close()

    # generator based

    from contextlib import contextmanager

    @contextmanager
    def managed_file(name):
        try:
            f = open(name, 'w')
            yield f
        finally:
            f.close()

    ###########
    # usage
    with managed_file('hello.txt') as f:
        f.write('hello, world!')
        f.write('bye now')
```