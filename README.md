# React-Concept
*****{if codeSplitting is not used}
1. Development Server Startup
  - Vite starts development server (typically on localhost:3000)
  - Sets up hot module replacement (HMR) for live updates
  - Configures on-demand compilation pipeline

2. Browser Request & Initial Load
  - Browser requests index.html from Vite server
  - Vite serves index.html which contains:
    - Root div element
    - Script tag pointing to main.jsx

3. Module Loading & Transpilation
   - Browser requests main.jsx
  - Vite transpiles JSX → JavaScript on-demand
  - Detects App.jsx import and transpiles it
  - Recursively transpiles all imported components
  - Browser caches all transpiled JavaScript modules

4.React Router Initialization
  - React Router analyzes current URL path
  - Matches route (initially "/" for home)
  - Determines which component to render

5.Component Lifecycle (our React Flow)
  - react requests for transpiled JavaScript modules of required component

  - react's work start from here
  Mounting
     - Initial State: useState creates state with initial/dummy values
     - First Render: Component renders with dummy values creating VDOM tree
        the created VDOM is now compared to initial DOM tree which only has "root" and the difference is inserted to real DOM
  - {if any useEffect component itself re-renders}
     - Effect Execution: useEffect runs after DOM insertion
     - State Update: Data fetch completes, setState called
     - Re-render: Component re-renders with real data
     - VDOM Diff: React compares old vs new virtual DOM
     - DOM Update: Only changed elements updated in real DOM

  UnMount
   When user navigates to /about 
  1. Home component unmounts 
    useEffect(() => { return () => { 
    // Cleanup function runs 
    console.log('Component unmounting') } }, []) 
 2. React requests About component's transpiled JavaScript from browsers cache 
 3. About component mounts with same lifecycle


npm run dev → Vite Server starts → Dev server ready at localhost:5173 
→ Browser requests localhost:5173 → Vite serves index.html → HTML loads in browser
→ Browser requests main.jsx → Vite transpiles JSX to JS → Sends to browser
Browser executes main.jsx → Imports App.jsx → Vite transpiles App.jsx → Sends to browser
App.jsx imports other components → Vite transpiles each import ON-DEMAND (only when imported) → Sends to browser
→ ReactDOM.createRoot() → App component mounts → Router initializes 
→ Router detects current URL → Matches route → Loads corresponding component → Component mounts

*****{if codeSplitting is used}
- Browser requests index.html → Vite serves it
- Browser requests main.jsx → Vite transpiles and serves
- App.jsx loads but lazy components are NOT loaded yet
- Bundle splitting occurs - Vite creates separate chunks for lazy components

- When user navigates to lazy loaded components:
  - React Router detects route change
  - Tries to render lazy loaded component
  - Discovers it's a lazy component (Promise)
  - React encounters unresolved Promise
  - Suspense boundary catches it
  - Fallback UI renders: <div>Loading...</div>
  - Component remains in "suspended" state
  - Browser makes NEW network request to Vite
  - Request: localhost:3000/src/components/lazy loaded component
  - Vite transpiles lazy loaded component and its dependencies on-demand
  - Creates separate chunk
  - transfer chunk to browser
  - Browser caches the new chunk
  - Promise resolves with component definition
  - React removes Suspense fallback
  - Now react lifecycle follows
    1. Component Creation
    2. Initial State (useState with dummy values)
    3. First Render
    4. DOM Insertion  
    5. Effect Execution (useEffect runs)
    6. State Update (data fetching)
    7. Re-render with real data
    8. VDOM Diff
    9. DOM Update


***Hot module replcement
Hot Module Replacement is a development technique that enables live editing of modules in a running application. Instead of reloading the entire page when code changes, HMR surgically replaces only the updated modules while preserving application state.

- File saved → Vite detects change
- Module transpiled → New version created
- Browser receives new module
- Cache replaced → Old Home function replaced with new Home function
- HMR callback 
- Fast Refresh notified 
- Component re-render → React calls new Home function
- DOM updated


- Hmr gives:
    - Instant updates: Changes reflect immediately without full page reload
    - State preservation: Component state is maintained during updates
    - Error recovery: Better error handling and recovery {If you introduce a syntax error, Fast Refresh will automatically recover once you fix it}
    - Faster development: No need to navigate back to your current state
