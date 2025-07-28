Angular Change Detection 
      Angular change detection will happen two times in the dev why ?

Great question! It's awesome that you're diving into **Angular's Change Detection** — understanding this sets you apart from the average Angular developer.

---

## ✅ Quick Answer:

In **development mode**, Angular runs **change detection twice intentionally** to help catch side effects and coding mistakes.

---

## 🧠 Why Does It Run Twice?

This is due to **Angular's development-only check called `ngDevMode`**, which invokes **`checkNoChanges()`** after the normal change detection pass.

### Here's what happens:

1. **First pass** – Angular runs the normal change detection to update the DOM based on the latest state.
2. **Second pass (`checkNoChanges`)** – Angular runs another check to ensure **nothing changed** during that first pass.

If anything changes during the second pass (i.e., DOM, bindings, etc.), it throws an error.

---

### 🔍 Purpose of `checkNoChanges()`:

* It helps **catch unexpected side effects** during rendering.
* Forces you to write **pure, predictable component logic**.
* Encourages avoiding things like:

  * Changing variables inside `getters`
  * Updating inputs during `ngOnInit()` or `ngAfterViewInit()` without guarding it properly
  * Making HTTP calls or emitting events that mutate state during rendering

---

## ⚠️ In Production

Angular **does NOT run change detection twice** in production mode.

* `checkNoChanges()` is skipped for performance.
* Only one change detection cycle runs.

---

## ✅ Example

```ts
get myValue() {
  console.log('Getter called');
  this.count++; // ⚠️ Side effect!
  return this.count;
}
```

In development mode:

* First change detection runs → `count++`
* Second check (`checkNoChanges`) runs → `count++` again → now the value is different from the first run → Angular throws error

---

## 🧪 How to Confirm This?

Try this in dev mode:

```ts
ngOnInit() {
  console.log('Init');
}

ngDoCheck() {
  console.log('Change detection triggered');
}
```

You’ll see `ngDoCheck` run twice for a single event in dev mode.

But when you build with:

```bash
ng build --configuration=production
```

Or:

```bash
ng serve --configuration=production
```

→ Only one `ngDoCheck()` runs per event.

---

## 💡 Summary

| Mode        | Runs Twice? | Purpose                   |
| ----------- | ----------- | ------------------------- |
| Development | ✅ Yes       | Catch side-effects & bugs |
| Production  | ❌ No        | Max performance           |

---

Let me know if you want a real-world example of a bug this has caught or how to write cleaner code that avoids issues here.
