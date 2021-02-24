# Error Boundaries

[https://reactjs.org/docs/error-boundaries.html](https://reactjs.org/docs/error-boundaries.html)

特别的组件，像是组件版的try/catch，可以捕捉子孙组件里的error并做fallback处理。

> A class component becomes an error boundary if it defines either \(or both\) of the lifecycle methods [`static getDerivedStateFromError()`](https://reactjs.org/docs/react-component.html#static-getderivedstatefromerror) or [`componentDidCatch()`](https://reactjs.org/docs/react-component.html#componentdidcatch).

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // Update state so the next render will show the fallback UI.    
    return { hasError: true };  
  }
  
  componentDidCatch(error, errorInfo) {    
    // You can also log the error to an error reporting service    
    logErrorToMyService(error, errorInfo);  
  }
  
  render() {
    if (this.state.hasError) {      
      // You can render any custom fallback UI      
      return <h1>Something went wrong.</h1>;    
    }
    
    return this.props.children; 
  }
}

<ErrorBoundary>
  <MyWidget />
</ErrorBoundary>
```

> Note
>
> Error boundaries do **not** catch errors for:
>
> * Event handlers
> * Asynchronous code \(e.g. `setTimeout` or `requestAnimationFrame` callbacks\)
> * Server side rendering
> * Errors thrown in the error boundary itself \(rather than its children\)

