<h1>
  <span class="headline">Selenium: Data-Driven Testing</span>
  <span class="subhead">Setup</span>
</h1>

## Setup

### 1: Create Your Project Folder

In this lesson, you’ll start fresh with a new project just for testing waits, alerts and dynamic uis.

Open a terminal and run:

```bash
mkdir selenium-data-driven-testing
cd selenium-data-driven-testing
```

### 2: Set Up a Virtual Environment

Create and activate a virtual environment inside this new folder:

```bash
python -m venv venv
```

Activate it:

**macOS/Linux:**

```bash
source venv/bin/activate
```

**Windows:**

```bash
venv\Scripts\activate
```

You should now see (venv) at the start of your terminal prompt.

### 3: Install Dependencies

With the virtual environment active, install `Selenium` and `pytest` using pip:

```bash
pip install selenium pytest
```

Check that both installed correctly:

```bash
pip show selenium
pip show pytest
```

> ✅ If installed correctly, you’ll see output with its version and location.

### 4: Create Your Project Files

Inside your `selenium-data-driven-testing` folder, create the following files:

- `search.html`
- `login.html`
- `registration.html`
- `temperature_widget.html`

## 5. Add your starter code

For this lesson we will be using local `html` files as mock sites for testing.

### Add the following starter code to `search.html`:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Travel Info Search</title>
    <style>
      body {
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        background-color: #f5f9fc;
        color: #333;
        padding: 2rem;
        max-width: 600px;
        margin: auto;
      }

      h1 {
        color: #0077b6;
        text-align: center;
        margin-bottom: 2rem;
      }

      form {
        display: flex;
        gap: 0.5rem;
        justify-content: center;
      }

      input[type='text'] {
        flex: 1;
        padding: 0.5rem;
        font-size: 1rem;
        border: 1px solid #ccc;
        border-radius: 4px;
      }

      button {
        background-color: #00a896;
        color: white;
        border: none;
        padding: 0.5rem 1rem;
        font-size: 1rem;
        border-radius: 4px;
        cursor: pointer;
      }

      button:hover {
        background-color: #028d7e;
      }

      #results {
        margin-top: 2rem;
        font-weight: bold;
        text-align: center;
        color: #007f5f;
      }
    </style>
  </head>
  <body>
    <h1>Search Travel Help Topics</h1>
    <form id="search-form">
      <input
        type="text"
        id="search-box"
        placeholder="e.g., flights to Tokyo, baggage policy..."
      />
      <button id="submit" type="submit">Search</button>
    </form>
    <div id="results"></div>

    <script>
      document
        .getElementById('search-form')
        .addEventListener('submit', function (e) {
          e.preventDefault();
          const query = document.getElementById('search-box').value;
          document.getElementById('results').innerText =
            'Showing results for: ' + query;
        });
    </script>
  </body>
</html>
```

### Add the following starter code to `login.html`:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Login Form</title>
    <style>
      body {
        background-color: #1e1e1e;
        color: #f5f5f5;
        font-family: sans-serif;
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        margin: 0;
      }

      .login-container {
        background: #2c2c2c;
        padding: 2rem;
        border-radius: 6px;
        width: 300px;
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.4);
      }

      h1 {
        text-align: center;
        margin-bottom: 1.5rem;
        font-size: 1.5rem;
      }

      input,
      button {
        display: block;
        width: 100%;
        padding: 0.75rem;
        margin-bottom: 1rem;
        border: none;
        border-radius: 4px;
        font-size: 1rem;
        box-sizing: border-box;
      }

      input {
        background: #444;
        color: #f5f5f5;
      }

      button {
        background: #4caf50;
        color: white;
        cursor: pointer;
      }

      #welcome-message {
        text-align: center;
        margin-top: 1rem;
        color: #90ee90;
        display: none;
      }
    </style>
  </head>
  <body>
    <div class="login-container">
      <h1>Sign In</h1>
      <form id="login-form">
        <input type="text" id="username" placeholder="Username" required />
        <input type="password" id="password" placeholder="Password" required />
        <button id="submit" type="submit">Sign In</button>
      </form>
      <div id="welcome-message"></div>
    </div>

    <script>
      document
        .getElementById('login-form')
        .addEventListener('submit', function (e) {
          e.preventDefault();
          const username = document.getElementById('username').value;
          const password = document.getElementById('password').value;
          if (username && password) {
            document.getElementById(
              'welcome-message'
            ).textContent = `Welcome, ${username}!`;
            document.getElementById('welcome-message').style.display = 'block';
          }
        });
    </script>
  </body>
</html>
```

### Add the following starter code to `registration.html`:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Global Registration Form</title>
    <style>
      body {
        background-color: #1e1e1e;
        color: #f5f5f5;
        font-family: Arial, sans-serif;
        display: flex;
        flex-direction: column;
        align-items: center;
        padding: 2rem;
      }

      h1 {
        color: #00bcd4;
      }

      .form-container {
        background-color: #2e2e2e;
        padding: 2rem;
        border-radius: 8px;
        max-width: 400px;
        width: 100%;
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
      }

      input,
      button {
        width: 100%;
        box-sizing: border-box;
        margin: 0.5rem 0;
        padding: 0.75rem;
        border: none;
        border-radius: 4px;
        font-size: 1rem;
      }

      input {
        background-color: #444;
        color: #fff;
      }

      button {
        background-color: #00bcd4;
        color: #000;
        font-weight: bold;
        cursor: pointer;
      }

      #message {
        margin-top: 1rem;
        color: #81c784;
      }
    </style>
  </head>
  <body>
    <h1>Register for Your Travel Account</h1>

    <div class="form-container">
      <form id="registration-form">
        <input id="username" placeholder="Username" required />
        <input id="email" type="email" placeholder="Email" required />
        <input id="password" type="password" placeholder="Password" required />
        <button id="submit" type="submit">Sign Up</button>
      </form>

      <div id="message" style="display: none;">Registration successful!</div>
    </div>

    <script>
      document
        .getElementById('registration-form')
        .addEventListener('submit', function (e) {
          e.preventDefault();
          document.getElementById('message').style.display = 'block';
        });
    </script>
  </body>
</html>
```

### Add the following starter code to `temperature_widget.html`:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Weather Widget</title>
    <style>
      body {
        background-color: #1e1e1e;
        color: #f1f1f1;
        font-family: sans-serif;
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        margin: 0;
      }

      .widget {
        background-color: #2d2d2d;
        border-radius: 10px;
        padding: 2rem;
        text-align: center;
        width: 260px;
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.4);
      }

      .emoji {
        font-size: 2.5rem;
      }

      #temp-widget {
        font-size: 2rem;
        font-weight: bold;
        margin-top: 0.5rem;
      }

      .source {
        margin-top: 1rem;
        font-size: 0.85rem;
      }

      .source a {
        color: #60a5fa;
        text-decoration: none;
      }

      .source a:hover {
        text-decoration: underline;
      }
    </style>
  </head>
  <body>
    <div class="widget">
      <div class="emoji">🌤️</div>
      <h2>Paris Temperature</h2>
      <div id="temp-widget">18°C</div>
      <div class="source">
        Powered by
        <a href="https://wttr.in" target="_blank" rel="noopener noreferrer"
          >wttr.in</a
        >
      </div>
    </div>
  </body>
</html>
```
