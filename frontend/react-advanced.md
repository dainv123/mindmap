# ⚛️ React Advanced - Complete Knowledge Map

## 🧠 Mindmap Overview
```
React Advanced
├── 🎣 Hooks Deep Dive
│   ├── Custom Hooks → Reusable logic, composition
│   ├── useCallback → Memoize functions
│   ├── useMemo → Memoize values
│   └── useReducer → Complex state management
├── 🏗️ Advanced Patterns
│   ├── Render Props → Component composition
│   ├── Higher-Order Components → Function composition
│   ├── Compound Components → Flexible APIs
│   └── Context API → Global state management
├── ⚡ Performance Optimization
│   ├── React.memo → Prevent unnecessary re-renders
│   ├── Code Splitting → Lazy loading, dynamic imports
│   ├── Virtualization → Large lists optimization
│   └── Bundle Optimization → Tree shaking, compression
├── 🧪 Testing
│   ├── Jest → Unit testing framework
│   ├── React Testing Library → Component testing
│   ├── Cypress → E2E testing
│   └── MSW → API mocking
└── 🚀 State Management
    ├── Redux Toolkit → Modern Redux
    ├── Zustand → Lightweight state
    ├── React Query → Server state
    └── Recoil → Atomic state
```

## 📋 Table of Contents
- [Hooks Deep Dive](#hooks-deep-dive)
- [Advanced Patterns](#advanced-patterns)
- [Performance Optimization](#performance-optimization)
- [Testing](#testing)
- [State Management](#state-management)
- [Common Interview Questions](#common-interview-questions)

## 🎣 Hooks Deep Dive

### Custom Hooks
```javascript
// Custom hook for API calls
const useApi = (url) => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        setLoading(true);
        const response = await fetch(url);
        const result = await response.json();
        setData(result);
      } catch (err) {
        setError(err);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, [url]);

  return { data, loading, error };
};

// Custom hook for form handling
const useForm = (initialValues) => {
  const [values, setValues] = useState(initialValues);
  const [errors, setErrors] = useState({});

  const handleChange = (e) => {
    const { name, value } = e.target;
    setValues(prev => ({
      ...prev,
      [name]: value
    }));
  };

  const handleSubmit = (callback) => (e) => {
    e.preventDefault();
    callback(values);
  };

  return { values, errors, handleChange, handleSubmit };
};

// Usage
const UserForm = () => {
  const { values, handleChange, handleSubmit } = useForm({
    name: '',
    email: ''
  });

  const onSubmit = (formData) => {
    console.log('Form submitted:', formData);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input
        name="name"
        value={values.name}
        onChange={handleChange}
        placeholder="Name"
      />
      <input
        name="email"
        value={values.email}
        onChange={handleChange}
        placeholder="Email"
      />
      <button type="submit">Submit</button>
    </form>
  );
};
```

### useCallback & useMemo
```javascript
// useCallback - Memoize functions
const ExpensiveComponent = ({ onItemClick, items }) => {
  const handleItemClick = useCallback((itemId) => {
    onItemClick(itemId);
  }, [onItemClick]);

  return (
    <div>
      {items.map(item => (
        <Item
          key={item.id}
          item={item}
          onClick={handleItemClick}
        />
      ))}
    </div>
  );
};

// useMemo - Memoize values
const ExpensiveCalculation = ({ data, filter }) => {
  const processedData = useMemo(() => {
    console.log('Processing data...');
    return data
      .filter(item => item.category === filter)
      .map(item => ({
        ...item,
        processed: item.value * 2
      }));
  }, [data, filter]);

  return (
    <div>
      {processedData.map(item => (
        <div key={item.id}>{item.processed}</div>
      ))}
    </div>
  );
};

// Performance optimization example
const UserList = ({ users, searchTerm }) => {
  const filteredUsers = useMemo(() => {
    return users.filter(user =>
      user.name.toLowerCase().includes(searchTerm.toLowerCase())
    );
  }, [users, searchTerm]);

  const handleUserClick = useCallback((userId) => {
    console.log('User clicked:', userId);
  }, []);

  return (
    <div>
      {filteredUsers.map(user => (
        <UserItem
          key={user.id}
          user={user}
          onClick={handleUserClick}
        />
      ))}
    </div>
  );
};
```

### useReducer
```javascript
// Complex state management with useReducer
const initialState = {
  users: [],
  loading: false,
  error: null,
  selectedUser: null
};

const userReducer = (state, action) => {
  switch (action.type) {
    case 'FETCH_USERS_START':
      return {
        ...state,
        loading: true,
        error: null
      };
    
    case 'FETCH_USERS_SUCCESS':
      return {
        ...state,
        loading: false,
        users: action.payload
      };
    
    case 'FETCH_USERS_ERROR':
      return {
        ...state,
        loading: false,
        error: action.payload
      };
    
    case 'SELECT_USER':
      return {
        ...state,
        selectedUser: action.payload
      };
    
    case 'ADD_USER':
      return {
        ...state,
        users: [...state.users, action.payload]
      };
    
    case 'UPDATE_USER':
      return {
        ...state,
        users: state.users.map(user =>
          user.id === action.payload.id ? action.payload : user
        )
      };
    
    case 'DELETE_USER':
      return {
        ...state,
        users: state.users.filter(user => user.id !== action.payload)
      };
    
    default:
      return state;
  }
};

const UserManager = () => {
  const [state, dispatch] = useReducer(userReducer, initialState);

  const fetchUsers = async () => {
    dispatch({ type: 'FETCH_USERS_START' });
    try {
      const response = await fetch('/api/users');
      const users = await response.json();
      dispatch({ type: 'FETCH_USERS_SUCCESS', payload: users });
    } catch (error) {
      dispatch({ type: 'FETCH_USERS_ERROR', payload: error.message });
    }
  };

  const addUser = (user) => {
    dispatch({ type: 'ADD_USER', payload: user });
  };

  const selectUser = (user) => {
    dispatch({ type: 'SELECT_USER', payload: user });
  };

  return (
    <div>
      {state.loading && <div>Loading...</div>}
      {state.error && <div>Error: {state.error}</div>}
      
      <UserList
        users={state.users}
        onUserSelect={selectUser}
        selectedUser={state.selectedUser}
      />
      
      <UserForm onSubmit={addUser} />
    </div>
  );
};
```

## 🏗️ Advanced Patterns

### Render Props
```javascript
// Render props pattern
const DataFetcher = ({ url, render }) => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        setLoading(true);
        const response = await fetch(url);
        const result = await response.json();
        setData(result);
      } catch (err) {
        setError(err);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, [url]);

  return render({ data, loading, error });
};

// Usage
const UserList = () => {
  return (
    <DataFetcher
      url="/api/users"
      render={({ data, loading, error }) => {
        if (loading) return <div>Loading...</div>;
        if (error) return <div>Error: {error.message}</div>;
        if (!data) return <div>No data</div>;

        return (
          <div>
            {data.map(user => (
              <div key={user.id}>{user.name}</div>
            ))}
          </div>
        );
      }}
    />
  );
};
```

### Higher-Order Components (HOC)
```javascript
// HOC for authentication
const withAuth = (WrappedComponent) => {
  return function AuthenticatedComponent(props) {
    const [isAuthenticated, setIsAuthenticated] = useState(false);
    const [user, setUser] = useState(null);

    useEffect(() => {
      // Check authentication status
      const checkAuth = async () => {
        try {
          const response = await fetch('/api/auth/me');
          if (response.ok) {
            const userData = await response.json();
            setUser(userData);
            setIsAuthenticated(true);
          }
        } catch (error) {
          setIsAuthenticated(false);
        }
      };

      checkAuth();
    }, []);

    if (!isAuthenticated) {
      return <div>Please log in to continue</div>;
    }

    return <WrappedComponent {...props} user={user} />;
  };
};

// HOC for error boundary
const withErrorBoundary = (WrappedComponent) => {
  return class ErrorBoundary extends React.Component {
    constructor(props) {
      super(props);
      this.state = { hasError: false, error: null };
    }

    static getDerivedStateFromError(error) {
      return { hasError: true, error };
    }

    componentDidCatch(error, errorInfo) {
      console.error('Error caught by boundary:', error, errorInfo);
    }

    render() {
      if (this.state.hasError) {
        return (
          <div>
            <h2>Something went wrong.</h2>
            <details>
              <summary>Error details</summary>
              <pre>{this.state.error?.toString()}</pre>
            </details>
          </div>
        );
      }

      return <WrappedComponent {...this.props} />;
    }
  };
};

// Usage
const ProtectedUserProfile = withAuth(withErrorBoundary(UserProfile));
```

### Compound Components
```javascript
// Compound components pattern
const Tabs = ({ children, defaultIndex = 0 }) => {
  const [activeIndex, setActiveIndex] = useState(defaultIndex);

  return (
    <TabsContext.Provider value={{ activeIndex, setActiveIndex }}>
      {children}
    </TabsContext.Provider>
  );
};

const TabsList = ({ children }) => {
  return <div className="tabs-list">{children}</div>;
};

const Tab = ({ index, children }) => {
  const { activeIndex, setActiveIndex } = useContext(TabsContext);
  const isActive = index === activeIndex;

  return (
    <button
      className={`tab ${isActive ? 'active' : ''}`}
      onClick={() => setActiveIndex(index)}
    >
      {children}
    </button>
  );
};

const TabPanels = ({ children }) => {
  return <div className="tab-panels">{children}</div>;
};

const TabPanel = ({ index, children }) => {
  const { activeIndex } = useContext(TabsContext);
  
  if (index !== activeIndex) return null;
  
  return <div className="tab-panel">{children}</div>;
};

// Usage
const App = () => {
  return (
    <Tabs defaultIndex={0}>
      <TabsList>
        <Tab index={0}>Users</Tab>
        <Tab index={1}>Settings</Tab>
        <Tab index={2}>Profile</Tab>
      </TabsList>
      
      <TabPanels>
        <TabPanel index={0}>
          <UserList />
        </TabPanel>
        <TabPanel index={1}>
          <Settings />
        </TabPanel>
        <TabPanel index={2}>
          <Profile />
        </TabPanel>
      </TabPanels>
    </Tabs>
  );
};
```

### Context API
```javascript
// Context for theme
const ThemeContext = createContext();

const ThemeProvider = ({ children }) => {
  const [theme, setTheme] = useState('light');

  const toggleTheme = () => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light');
  };

  const value = {
    theme,
    toggleTheme
  };

  return (
    <ThemeContext.Provider value={value}>
      {children}
    </ThemeContext.Provider>
  );
};

// Custom hook for theme
const useTheme = () => {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error('useTheme must be used within ThemeProvider');
  }
  return context;
};

// Usage
const App = () => {
  return (
    <ThemeProvider>
      <Header />
      <Main />
      <Footer />
    </ThemeProvider>
  );
};

const Header = () => {
  const { theme, toggleTheme } = useTheme();
  
  return (
    <header className={`header ${theme}`}>
      <h1>My App</h1>
      <button onClick={toggleTheme}>
        Switch to {theme === 'light' ? 'dark' : 'light'} mode
      </button>
    </header>
  );
};
```

## ⚡ Performance Optimization

### React.memo
```javascript
// Prevent unnecessary re-renders
const UserItem = React.memo(({ user, onSelect, isSelected }) => {
  console.log(`Rendering UserItem: ${user.name}`);
  
  return (
    <div
      className={`user-item ${isSelected ? 'selected' : ''}`}
      onClick={() => onSelect(user.id)}
    >
      <h3>{user.name}</h3>
      <p>{user.email}</p>
    </div>
  );
});

// Custom comparison function
const UserItemWithCustomComparison = React.memo(
  ({ user, onSelect, isSelected }) => {
    return (
      <div
        className={`user-item ${isSelected ? 'selected' : ''}`}
        onClick={() => onSelect(user.id)}
      >
        <h3>{user.name}</h3>
        <p>{user.email}</p>
      </div>
    );
  },
  (prevProps, nextProps) => {
    // Custom comparison logic
    return (
      prevProps.user.id === nextProps.user.id &&
      prevProps.isSelected === nextProps.isSelected
    );
  }
);
```

### Code Splitting
```javascript
// Lazy loading with React.lazy
const LazyUserList = lazy(() => import('./UserList'));
const LazySettings = lazy(() => import('./Settings'));
const LazyProfile = lazy(() => import('./Profile'));

const App = () => {
  const [currentPage, setCurrentPage] = useState('users');

  return (
    <div>
      <nav>
        <button onClick={() => setCurrentPage('users')}>Users</button>
        <button onClick={() => setCurrentPage('settings')}>Settings</button>
        <button onClick={() => setCurrentPage('profile')}>Profile</button>
      </nav>

      <Suspense fallback={<div>Loading...</div>}>
        {currentPage === 'users' && <LazyUserList />}
        {currentPage === 'settings' && <LazySettings />}
        {currentPage === 'profile' && <LazyProfile />}
      </Suspense>
    </div>
  );
};

// Dynamic imports
const loadComponent = async (componentName) => {
  const module = await import(`./components/${componentName}`);
  return module.default;
};

const DynamicComponent = ({ componentName, ...props }) => {
  const [Component, setComponent] = useState(null);

  useEffect(() => {
    loadComponent(componentName).then(setComponent);
  }, [componentName]);

  if (!Component) return <div>Loading...</div>;

  return <Component {...props} />;
};
```

### Virtualization
```javascript
// Virtualized list for large datasets
import { FixedSizeList as List } from 'react-window';

const VirtualizedUserList = ({ users }) => {
  const Row = ({ index, style }) => {
    const user = users[index];
    
    return (
      <div style={style} className="user-row">
        <h3>{user.name}</h3>
        <p>{user.email}</p>
      </div>
    );
  };

  return (
    <List
      height={400}
      itemCount={users.length}
      itemSize={80}
      width="100%"
    >
      {Row}
    </List>
  );
};

// Virtualized grid
import { FixedSizeGrid as Grid } from 'react-window';

const VirtualizedGrid = ({ items, columns = 3 }) => {
  const Cell = ({ columnIndex, rowIndex, style }) => {
    const index = rowIndex * columns + columnIndex;
    const item = items[index];
    
    if (!item) return null;
    
    return (
      <div style={style} className="grid-cell">
        <h3>{item.name}</h3>
        <p>{item.description}</p>
      </div>
    );
  };

  const rows = Math.ceil(items.length / columns);

  return (
    <Grid
      columnCount={columns}
      columnWidth={200}
      height={400}
      rowCount={rows}
      rowHeight={150}
      width={columns * 200}
    >
      {Cell}
    </Grid>
  );
};
```

## 🧪 Testing

### Jest & React Testing Library
```javascript
// Component test
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import UserForm from './UserForm';

describe('UserForm', () => {
  test('renders form fields', () => {
    render(<UserForm />);
    
    expect(screen.getByPlaceholderText('Name')).toBeInTheDocument();
    expect(screen.getByPlaceholderText('Email')).toBeInTheDocument();
    expect(screen.getByRole('button', { name: 'Submit' })).toBeInTheDocument();
  });

  test('submits form with user data', async () => {
    const mockOnSubmit = jest.fn();
    render(<UserForm onSubmit={mockOnSubmit} />);
    
    const nameInput = screen.getByPlaceholderText('Name');
    const emailInput = screen.getByPlaceholderText('Email');
    const submitButton = screen.getByRole('button', { name: 'Submit' });
    
    await userEvent.type(nameInput, 'John Doe');
    await userEvent.type(emailInput, 'john@example.com');
    fireEvent.click(submitButton);
    
    expect(mockOnSubmit).toHaveBeenCalledWith({
      name: 'John Doe',
      email: 'john@example.com'
    });
  });

  test('validates required fields', async () => {
    render(<UserForm />);
    
    const submitButton = screen.getByRole('button', { name: 'Submit' });
    fireEvent.click(submitButton);
    
    await waitFor(() => {
      expect(screen.getByText('Name is required')).toBeInTheDocument();
      expect(screen.getByText('Email is required')).toBeInTheDocument();
    });
  });
});

// Custom hook test
import { renderHook, act } from '@testing-library/react';
import { useCounter } from './useCounter';

describe('useCounter', () => {
  test('initializes with default value', () => {
    const { result } = renderHook(() => useCounter());
    expect(result.current.count).toBe(0);
  });

  test('initializes with custom value', () => {
    const { result } = renderHook(() => useCounter(10));
    expect(result.current.count).toBe(10);
  });

  test('increments counter', () => {
    const { result } = renderHook(() => useCounter());
    
    act(() => {
      result.current.increment();
    });
    
    expect(result.current.count).toBe(1);
  });

  test('decrements counter', () => {
    const { result } = renderHook(() => useCounter(5));
    
    act(() => {
      result.current.decrement();
    });
    
    expect(result.current.count).toBe(4);
  });
});

// API test with MSW
import { rest } from 'msw';
import { setupServer } from 'msw/node';
import { render, screen, waitFor } from '@testing-library/react';
import UserList from './UserList';

const server = setupServer(
  rest.get('/api/users', (req, res, ctx) => {
    return res(
      ctx.json([
        { id: 1, name: 'John Doe', email: 'john@example.com' },
        { id: 2, name: 'Jane Smith', email: 'jane@example.com' }
      ])
    );
  })
);

beforeAll(() => server.listen());
afterEach(() => server.resetHandlers());
afterAll(() => server.close());

describe('UserList', () => {
  test('renders users from API', async () => {
    render(<UserList />);
    
    await waitFor(() => {
      expect(screen.getByText('John Doe')).toBeInTheDocument();
      expect(screen.getByText('Jane Smith')).toBeInTheDocument();
    });
  });

  test('handles API error', async () => {
    server.use(
      rest.get('/api/users', (req, res, ctx) => {
        return res(ctx.status(500));
      })
    );

    render(<UserList />);
    
    await waitFor(() => {
      expect(screen.getByText('Error loading users')).toBeInTheDocument();
    });
  });
});
```

## 📚 Resources
- [React Documentation](https://react.dev/)
- [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/)
- [React Performance](https://react.dev/learn/render-and-commit)
- [Custom Hooks](https://react.dev/learn/reusing-logic-with-custom-hooks)
- [React Patterns](https://reactpatterns.com/) 