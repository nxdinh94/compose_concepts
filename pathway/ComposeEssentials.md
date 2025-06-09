# 1. Starter tutorial
# 2. Thinking in Compose
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

### Recomposition skips as much as possible
Example: 
```kotlin
/**
 * Display a list of names the user can click with a header
 */
@Composable
fun NamePicker(
    header: String,
    names: List<String>,
    onNameClicked: (String) -> Unit
) {
    Column {
        // this will recompose when [header] changes, but not when [names] changes
        Text(header, style = MaterialTheme.typography.bodyLarge)
        HorizontalDivider()

        // LazyColumn is the Compose version of a RecyclerView.
        // The lambda passed to items() is similar to a RecyclerView.ViewHolder.
        LazyColumn {
            items(names) { name ->
                // When an item's [name] updates, the adapter for that item
                // will recompose. This will not recompose when [header] changes
                NamePickerItem(name, onNameClicked)
            }
        }
    }
}

/**
 * Display a single name the user can click.
 */
@Composable
private fun NamePickerItem(name: String, onClicked: (String) -> Unit) {
    Text(name, Modifier.clickable(onClick = { onClicked(name) }))
}
```
Compose might skip to the `Column` lambda without executing any of its parents when the `header` changes. And when executing `Column`, Compose might choose to skip the `LazyColumn`'s items if `names` didn't change.
>[!NOTE]
> When you need to perform a side-effect, trigger it from a callback.

### [Recomposition is optimistic](https://developer.android.com/develop/ui/compose/mental-model#optimistic)

### [Composable functions could be run in parallel](https://developer.android.com/develop/ui/compose/mental-model#parallel)

### Composable functions can be executed in any orders
- When you call more than a composable function at a time, it dont gurantee that thoses are run in order. Cause, Compose have ability to set priority to composable function, and then redraw them first.
# 3. Composable functions
# 4. Write your first Compose app
## 1. Before you begin
## 2. Start a new Compose project
>[!WARNING]
> The app theme used inside setContent depends on how your project is named. This codelab assumes that the project is called BasicsCodelab. If you copy-paste code from the codelab, don't forget to update BasicsCodelabTheme with the name of your theme available in the ui/Theme.kt file. We'll get into theming later in the codelab.
## 3.   Getting started with compose
### Composable functions
- `Composable functions` are `Kotlin functions` that are marked with the `@Composable` annotation, as you can see in the code snippet above.

### Compose in an Android app
- MainActivity file is launched when the user open the app. We can modify them is `AndroidManifest.xml`.
- `setContent` use to define your UI.
- `@Preview` annotation is assigned to composable function in order to see preview instead of show up entire app on device or emulator
## 4. Tweaking the UI
## 5. Reusing composables
- As a best practice, your function should include a Modifier parameter that is assigned an empty Modifier by default.
```kotlin
@Composable
fun MyApp(modifier: Modifier = Modifier) {
    Surface(
        modifier = modifier,
        color = MaterialTheme.colorScheme.background
    ) {
        Greeting("Android")
    }
}
```
## 6. Creating columns and rows
## 7. State in Compose

``` kotlin
// Don't copy over
@Composable
fun Greeting(name: String) {
    var expanded = false // Don't do this!

    Surface(
        color = MaterialTheme.colorScheme.primary,
        modifier = Modifier.padding(vertical = 4.dp, horizontal = 8.dp)
    ...
    )
}
```
The reason why mutating this variable does not trigger recompositions is that** it's not being tracked by Compose**. Also, each time `Greeting` is called, the variable will be **reset** to `false`.

To add internal state to a composable, you can use the `mutableStateOf` function, which makes `Compose` recompose `functions` that read that `State`.

>[!NOTE]
> `State` and `MutableState` are interfaces that hold some value and trigger UI updates (recompositions) whenever that value changes.
> To preserve state across recompositions, remember the mutable state using `remember`.

```kotlin
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
// ...

@Composable
fun Greeting(...) {
    val expanded = remember { mutableStateOf(false) }
    // ...
}
```

## 8. State hoisting
In Composable functions, state that is `read` or `modified` by multiple functions should live in a common ancestor—this process is called `state hoisting`. To hoist means to lift or elevate.

In Compose you don't hide UI elements. Instead, you simply don't add them to the composition,
## 9. Creating a performant lazy list
To display a scrollable column we use a LazyColumn.

LazyColumn renders only the visible items on screen, allowing performance gains when rendering a big list.
>[!NOTE]
>Note: LazyColumn and LazyRow are equivalent to RecyclerView in Android Views.

## 10. Persisting state
- The `remember` function works only as long as the composable is kept in the Composition.
- When you rotate, the whole activity is restarted so all state is lost. 
- Instead of using `remember` you can use `rememberSaveable`. This will save each state surviving configuration changes (such as rotations) and process death.
- Run, rotate, change to dark mode or kill the process. The onboarding screen is not shown unless you have previously exited the app.
## 11. Animating your list
## 12. Styling and theming your app
# 5. The Compose UI toolkit
# 6. Implement real world design
## 1. Introduction
## 2. 