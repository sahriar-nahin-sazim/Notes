
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



`Ts/Eslint/Tailwind/src/App router/ @`
### Form
0. zod stuff
```ts schema
import {z} from 'zod'

export const userNameValidation = z.string().min(n, msg)
// .length

export const abcSchema = z.object({
	userName: userNameValidation
})
```


1. create form type
```ts
type FormFields = {
	email: string;
	password: string;
} // zod.infer<typeof schema>
```
2. create onSubmit
```ts
const onSubmit: SubmitHandler<FormFields> = (data)=>{
	// do stuff
} 
```
3.  
```ts
const { register,
		handleSubmit,
		setError, // has a root option for whole form
		formState: { errors, isSubmitting }
	} = useForm<FormFields>({
		defaultValues: {
			"email": "dfa"
		},
		resolver: zodResolver(schema) // 
	})
```
4. prepare onSubmit
```ts
<form onSubmit={handleSubmit(onSubmit)}
```
5. register input fields
```ts
<input {...register(name)} type="text" placeholder="inp"/>
```
6. access error message
```ts
{ errors.field && errors.field.message }
```


- [ ] Give a login form
	- [ ] User enters credentials
	- [ ] Server sends back response
		- [ ] sets a http only cookie (refresh token from some user data)
		- [ ] generate an *access* token from *refresh* token (with low expiration)
			- [ ] access token stored in memory (not local storage)
			- [ ] if expired get a new one

Access token
Refresh token

### Authentication
