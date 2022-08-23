-  allows user to persist values between renders.
-  returns a mutable ref object whose `.current` proerty is initialized to the passed arg(`initialValue`)
-  common use case is access a child imperatively

```js
function TextInputWithFocusButton() {
	const inputEl = useRef(null)
	const onButtonClick = () => {
		// `current` points to the mounted text input element
		inputEl.current.focus()
	}
	return (
		<>
			<input ref={inputEl} type="text" />
			<button onClick={onButtonClick}>Focus the input</button>
		</>
	)
}
```

-  If you pass a ref object to React with ` <div ref={myRef} />`, React will set its `.current` property to the corresponding DOM node whenever that node changes.
-  also handy in keeping mutable value around similar to how youd use instance fields in classes
   This works because `useRef()` creates a plain JavaScript object. The only difference between `useRef()` and creating a `{current: ...}` object yourself is that `useRef` will give you the same ref object on every render.

Keep in mind that useRef doesn’t notify you when its content changes. Mutating the `.current` property doesn’t cause a re-render.

If we tried to count how many times our application renders using the useState Hook, we would be caught in an infinite loop since this Hook itself causes a re-render.

To avoid this, we can use the useRef Hook(as it `doesnt` cause a `re-reender`).

```js
import { useState, useEffect, useRef } from "react"
import ReactDOM from "react-dom/client"

function App() {
	const [inputValue, setInputValue] = useState("")
	const count = useRef(0)

	useEffect(() => {
		count.current = count.current + 1
	})

	return (
		<>
			<input
				type="text"
				value={inputValue}
				onChange={(e) => setInputValue(e.target.value)}
			/>
			<h1>Render Count: {count.current}</h1>
		</>
	)
}

const root = ReactDOM.createRoot(document.getElementById("root"))
root.render(<App />)
```
