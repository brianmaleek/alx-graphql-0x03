# ReactGuard: Mastering Error Handling

## Overview

This project implements an Error Boundary component in a Next.js application to gracefully handle JavaScript errors that occur during rendering. The solution includes creating an ErrorBoundary class component, integrating it with the application, testing it with an error-prone component, and adding error monitoring with Sentry.

## Learning Objectives

- Understand how React Error Boundaries work
- Implement a class component in TypeScript
- Handle runtime errors gracefully in a Next.js application
- Integrate third-party error monitoring services
- Test error handling components effectively

## Key Concepts

- **Error Boundaries**: Special React components that catch JavaScript errors anywhere in their child component tree
- **Component Lifecycle Methods**: Using `getDerivedStateFromError` and `componentDidCatch` to handle errors
- **Error Monitoring**: Integrating services like Sentry for production error tracking
- **Fallback UI**: Providing user-friendly error messages when components fail
- **Error Recovery**: Implementing “Try again” functionality for users

## Tools and Libraries

- **React**: JavaScript library for building user interfaces
- **TypeScript**: Typed superset of JavaScript
- **Next.js**: React framework for server-rendered applications
- **Sentry**: Error monitoring and tracking service
- **Node.js/npm**: JavaScript runtime and package manager

## Real-World Use Case

Error boundaries are essential in production applications to: 1. Prevent entire application crashes from single component failures 2. Provide meaningful error messages to users instead of blank screens 3. Track and monitor errors in production environments 4. Allow users to recover from non-critical errors 5. Maintain application stability and improve user experience

This implementation pattern is used by major web applications to ensure reliability and maintainability. The Sentry integration provides valuable insights for debugging and fixing issues that occur in production.

## Implementation Summary

1. Created an ErrorBoundary class component with proper TypeScript interfaces
2. Wrapped the main application component with the ErrorBoundary
3. Developed a test component that intentionally throws errors
4. Integrated Sentry for error monitoring and logging
5. Implemented a fallback UI with error recovery option
This solution follows React best practices for error handling while demonstrating modern web development techniques with TypeScript and Next.js.

## 📝 Project Assessment (Hybrid)

Your project will be evaluated primarily through manual reviews. To ensure you receive your full score, please:

✅ Complete your project on time
📄 Submit all required files
🔗 Generate your review link
👥 Have your peers review your work

An auto-check will also be in place to verify the presence of core files needed for manual review.

## ⏰ Important Note

If the deadline passes, you won’t be able to generate your review link—so be sure to submit on time!

## Tasks

### 0. Create the ErrorBoundary Component

**mandatory**

**Objective**: Implement an ErrorBoundary class component in TypeScript that can catch and handle JavaScript errors in your application.

**Instructions**:

- Duplicate `alx-graphql-0x02` to `alx-graphql-0x03`
- Change directory to alx-rick-and-morty-app
- Create a new file named `ErrorBoundary.tsx` in the `components` directory.
- Implement the ErrorBoundary class by extending `React.Component`. Use the following structure:

```typescript
interface State {
  hasError: boolean;
}

interface ErrorBoundaryProps {
  children: ReactNode;
}


class ErrorBoundary extends React.Component<ErrorBoundaryProps , State> {
  constructor(props: ErrorBoundaryProps) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error: Error): State {
    return { hasError: true };
  }

  componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
    console.log({ error, errorInfo });
  }

  render() {
    if (this.state.hasError) {
      return (
        <div>
          <h2>Oops, there is an error!</h2>
          <button onClick={() => this.setState({ hasError: false })}>
            Try again?
          </button>
        </div>
      );
    }

    return this.props.children;
  }
}

export default ErrorBoundary;
```

- Save and close your files
- Run `npm run dev` from the terminal
- From a tab in your browser type `http://localhost:3000` to see the changes made.

**Repo**:

- **GitHub repository**: **alx-graphql-0x03**
- **Directory**: **alx-rick-and-morty-app**
- **File**: [README.md](./alx-rick-and-morty-app/README.md), [components/ErrorBoundary.tsx](./alx-rick-and-morty-app/components/ErrorBoundary.tsx)

### 1. Wrap the Application with ErrorBoundary

**mandatory**

**Objective**: Integrate the ErrorBoundary component into your Next.js application by wrapping the main component.

**Instructions**:

- Open `thepages/_app.tsx` file.

- Import the `ErrorBoundary` component.

- Wrap the `Component` prop in the `ErrorBoundary`:

```typescript
import ErrorBoundary from '@/components/ErrorBoundary';
import type { AppProps } from "next/app";



function MyApp({ Component, pageProps }: AppProps) {
  return (
    <ErrorBoundary>
      <Component {...pageProps} />
    </ErrorBoundary>
  );
}

export default MyApp;
```

- Save and close your files
- Run `npm run dev` from the terminal
- From a tab in your browser type `http://localhost:3000` to see the changes made.

**Repo**:

- **GitHub repository**: **alx-graphql-0x03**
- **Directory**: **alx-rick-and-morty-app**
- **File**: [README.md](./alx-rick-and-morty-app/README.md), [pages/_app.tsx](./alx-rick-and-morty-app/pages/_app.tsx)

### 2. Create a Component to Test ErrorBoundary

**mandatory**

**Objective**: Develop a simple component that intentionally throws an error to test the ErrorBoundary functionality.

**Instructions**:

- Create a new file named `ErrorProneComponent.tsx` in the `components` directory.
- Implement a functional component that throws an error when rendered:

```typescript
const ErrorProneComponent: React.FC = () => {
  throw new Error('This is a test error!');
};

export default ErrorProneComponent;
```

In any of your pages (e.g., `pages/index.tsx`), import and use the `ErrorProneComponent` within the `ErrorBoundary` to trigger the error:

```typescript
import ErrorBoundary from '@/components/ErrorBoundary';
import ErrorProneComponent from '@/components/ErrorProneComponent';

const Home: React.FC = () => {
  return (
    <ErrorBoundary>
      <ErrorProneComponent />
    </ErrorBoundary>
  );
};

export default Home;
```

- Save and close your files
- Run `npm run dev` from the terminal
- From a tab in your browser type `http://localhost:3000` to see the changes made.

**Repo**:

- **GitHub repository**: **alx-graphql-0x03**
- **Directory**: **alx-rick-and-morty-app**
- **File**: [README.md](./alx-rick-and-morty-app/README.md), [components/ErrorProneComponent.tsx](./alx-rick-and-morty-app/components/ErrorProneComponent.tsx)

### 3. Monitor and Log Errors

**mandatory**

**Objective**: Integrate an error monitoring service (e.g., Sentry) into the ErrorBoundary to log errors.

**Instructions**:

- Choose an error monitoring service and install its SDK. For example, for Sentry, run:

```bash
npm install @sentry/react @sentry/nextjs
```

- Configure the monitoring service in your `ErrorBoundary` component. For Sentry, modify `componentDidCatch` to report errors:

```typescript
import * as Sentry from '@sentry/react';

componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
  Sentry.captureException(error, { extra: errorInfo });
}
```

- Ensure that the service is correctly set up in your application (follow their documentation for initialization).

- Test your application by throwing an error in `ErrorProneComponent` and check if the error is logged in the monitoring service dashboard.

- Submit your updated `ErrorBoundary.tsx` file and provide a screenshot of the logged error in your monitoring service.

- Save and close your files

- Run `npm run dev` from the terminal

- From a tab in your browser type `http://localhost:3000` to see the changes made.

**Repo**:

- **GitHub repository**: **alx-graphql-0x03**
- **Directory**: **alx-rick-and-morty-app**
- **File**: [package.json](./alx-rick-and-morty-app/package.json), [components/ErrorBoundary.tsx](./alx-rick-and-morty-app/components/ErrorBoundary.tsx)
