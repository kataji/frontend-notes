# Code Splitting

Code Splitting是在打包代码的时候防止一个bundle过大的。把代码打成好几个bundle，按需lazy load，这样就不会在一开始有太长的沉默。

和tree-shaking还是有不一样的，shake之后减少了不必要的代码，总代码体积减小。而split之后代码总体积不变，只是由一大块变成了几小块，结合按需加载优化体验。

### import\(\)

> **Before:**
>
> ```jsx
> import { add } from './math';
>
> console.log(add(16, 26));
> ```
>
> **After:**
>
> ```jsx
> import("./math").then(math => {
>   console.log(math.add(16, 26));
> });
> ```

### React.lazy\(\)

> **Before:**
>
> ```jsx
> import OtherComponent from './OtherComponent';
> ```
>
> **After:**
>
> ```jsx
> import React, { Suspense } from 'react';
>
> const OtherComponent = React.lazy(() => import('./OtherComponent'));
> const AnotherComponent = React.lazy(() => import('./AnotherComponent'));
>
> function MyComponent() {
>   return (
>     <div>
>       <Suspense fallback={<div>Loading...</div>}> // fallback: any react element
>         <section>
>           <OtherComponent /> // render lazy component inside suspense component
>           <AnotherComponent /> // can have multiple lazy components
>         </section>
>       </Suspense>
>     </div>
>   );
> }
> ```

SSR： [Loadable Components](https://github.com/gregberge/loadable-components)

### Route-Based

> You want to make sure you choose places that will split bundles evenly, but won’t disrupt the user experience.

```jsx
import React, { Suspense, lazy } from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';

const Home = lazy(() => import('./routes/Home'));
const About = lazy(() => import('./routes/About'));

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/" component={Home}/>
        <Route path="/about" component={About}/>
      </Switch>
    </Suspense>
  </Router>
);
```

