# admin
this is repository for my website tradesmart
// Assuming this is a React project deployed on Netlify
// To create an admin panel, we'll add routing and a basic admin dashboard component.

import React from 'react';
import { BrowserRouter as Router, Route, Switch, Redirect } from 'react-router-dom';

// Components
import Home from './components/Home'; // Your existing Home page
import AdminDashboard from './components/AdminDashboard';
import Login from './components/Login'; // Login page for admin authentication

// Fake authentication function (replace with real authentication logic)
const isAuthenticated = () => {
  return localStorage.getItem('adminAuth') === 'true';
};

const ProtectedRoute = ({ component: Component, ...rest }) => (
  <Route
    {...rest}
    render={(props) =>
      isAuthenticated() ? (
        <Component {...props} />
      ) : (
        <Redirect to="/login" />
      )
    }
  />
);

const App = () => {
  return (
    <Router>
      <Switch>
        <Route path="/" exact component={Home} />
        <Route path="/login" component={Login} />
        <ProtectedRoute path="/admin" component={AdminDashboard} />
      </Switch>
    </Router>
  );
};

export default App;

// AdminDashboard Component (components/AdminDashboard.js)
import React from 'react';

const AdminDashboard = () => {
  const handleLogout = () => {
    localStorage.removeItem('adminAuth');
    window.location.href = '/';
  };

  return (
    <div>
      <h1>Admin Dashboard</h1>
      <p>Welcome to the admin panel!</p>
      <button onClick={handleLogout}>Logout</button>
    </div>
  );
};

export default AdminDashboard;

// Login Component (components/Login.js)
import React, { useState } from 'react';

const Login = () => {
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');
  const [error, setError] = useState('');

  const handleLogin = (e) => {
    e.preventDefault();
    if (username === 'admin' && password === 'password') { // Replace with real credentials
      localStorage.setItem('adminAuth', 'true');
      window.location.href = '/admin';
    } else {
      setError('Invalid credentials');
    }
  };

  return (
    <div>
      <h1>Admin Login</h1>
      <form onSubmit={handleLogin}>
        <input
          type="text"
          placeholder="Username"
          value={username}
          onChange={(e) => setUsername(e.target.value)}
        />
        <input
          type="password"
          placeholder="Password"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
        />
        <button type="submit">Login</button>
      </form>
      {error && <p style={{ color: 'red' }}>{error}</p>}
    </div>
  );
};

export default Login;
