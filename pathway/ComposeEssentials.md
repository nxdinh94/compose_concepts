# Starter tutorial
# Thinking in Compose
### A simple composable function
- The function doesn't return anything. Compose functions that emit UI do not need to return anything, because they describe the desired screen state instead of constructing UI widgets.


- This function is fast, idempotent, and free of side-effects.

    - The function behaves the same way when called multiple times with the same argument, and it does not use other values such as global variables or calls to random().
    - The function describes the UI without any side-effects, such as modifying properties or global variables.

### The declarative paradigm shift
- You update the UI by calling the same composable function with different arguments.


### Dynamic content
### Recomposition
- Recomposition is the process of calling your composable functions again when inputs change. 
- The function depend on changed value will be redraw, other functions don't.


- **Example:** When the user **interacts** with the UI, the UI raises events such as `onClick`. Those events should `notify` the app logic, which can then `change` the `app's state`. When the state changes, the composable functions are **automatically** called again with `the new data`. This causes the UI elements to be `redrawn`.