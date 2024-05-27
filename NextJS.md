
### 14
`public` - static asset
`.next` - application served from here
`src`
	- `app` - app router
		- `folder`
			- `page.tsx`
			- `layout.tsx`  
		- `[id]/page.tsx`
			- `PageComponent({params})`
			- `notFound()` - finds and show `not-found.tsx` file
	- `layout.tsx` - when app runs (`RootLayout`) - children is the path 
	- `page.tsx` - next js root, render with layout
	- `loading.tsx` - to view loading
	- `error.tsx` - client component, all hooks are available. (for error handling)
	- `not-found.tsx`

Data Fetching: 



`<Link href="">`
	- prefetch
### 13
`src`
	`pages`
	`post`
			`[id]`
				`router = useRouter()`
				`router.query`
	`styles`
		`globals.css`
		`Home.module.css`
	
	
	

`npm i @reduxjs/toolkit react-redux`

make a folder in `app/src` -> `GlobalRedux` ->`provider.tsx` ,`store.ts`
`GlobalRedux` ->`Features/FeatureName` ->`FeatureSlice.ts`
(all files has to be 'use client')
```ts store.ts
'use client'
// configureStore -> toolkit
// import reducer

export const store = configureStore({
	reducer: {
		counter: counterReducer
	}
})


export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch
```

```ts provider
'use client'
import { Provider } from 'react-redux';
import { store } from './store'

export function Providers({children}) {
	return <Provider store={store}>{children}</Provider>
}
```


```ts Slice
export interface CounterSlice {
	value: number
}

const initalState: CounterState = {
	value: 0
}

export const counterSlice = createSlice({
	name: 'counter',
	initialState,
	reducers: reducers
})

export const {...reducers} = counterSlice.actions;

export default counterSlice.reducer
```


```ts
'use client'
export default function Home(){
	const count = useSelector(state=> state.counter.value)
	// state.counter (slice name/store name) gives initial value obj
	const dispatch = useDispatch()
	}
```