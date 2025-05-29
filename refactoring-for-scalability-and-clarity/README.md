<h1>
  <span class="headline">Selenium: Data-Driven Testing</span>
  <span class="subhead">Refactoring for Scalability and Clarity</span>
</h1>

**Learning objective:** Refactor tests to separate logic from data for scalability and clarity.

A well-structured, scalable test suite is like a carefully organized supply chain in an international business. If data and processes are mixed together with every shipment, finding and fixing issues slows down as the company grows. By clearly separating the _what_ (your test logic) from the _with what_ (your test data), you streamline maintenance, boost coverage, and ensure anyone on your team can contribute new cases or spot issues quickly.

## Is it time to refactor?

As you examine your own test suite, keep an eye out for these “code smells”:

- Test data and logic appear together in every test function.
- Adding new users or edge cases means duplicating the entire test process.
- When requirements change or new fields are added, you have to hunt through multiple files to update.
- Frequent copy-paste patterns, and little confidence that every scenario is covered consistently.

## Principles for smart separation

The core principle is to ensure your **test logic** (the steps—actions, assertions) and **test data** (the variable values—usernames, passwords, scenarios) are managed independently.

- **Scalability:** Easily add new scenarios for new regions, languages, or requirements.
- **Clarity:** Find and update test steps in one clear place; keep data easy to browse and expand.
- **Reduced duplication:** Fix test bugs or process updates once, not in every function.
- **Team efficiency:** Anyone can add or review new cases without digging into logic-heavy scripts.

## Review : How do we safely separate logic from data in Python with Selenium?

Automation frameworks like `pytest` support:

- **Data-provider functions**: Load, parse, and prepare test case data from CSV, JSON, or APIs, ready for use in your logic.
- **Fixtures**: Provide reusable setup (ex: opening a browser, selecting a test data file).

By parameterizing your tests with these helpers, you keep logic clean and adaptable.

## Step 1: Identify hardcoded logic that could benefit from a refactor

```python
def test_register_ayesha(driver):
    driver.get("http://example.com/register.html")
    driver.find_element(By.ID, "username").send_keys("ayesha_khan")
    driver.find_element(By.ID, "email").send_keys("ayesha@edu.pk")
    driver.find_element(By.ID, "password").send_keys("mypassword1!")
    driver.find_element(By.ID, "submit").click()
    assert "Registration successful" in driver.page_source
```

## Step 2: Move your data into an external file

```plaintext
username,email,password
ayesha_khan,ayesha@edu.pk,mypassword1!
kofi_mensah,kofi@study.gh,Pa$$w0rd9!
mariana.rodriguez,mariana@email.cl,ClaveSegura2@
よしだ, yoshida@メール.jp,初めてのパスワード
```

- You can now represent users from different continents and scripts by simply adding rows.

## Step 3: Load your data with a reusable function

```python
import csv

def load_registration_data():
    with open("testdata/registration.csv", newline='') as csvfile:
        reader = csv.reader(csvfile)
        next(reader)  # Skip header
        return [tuple(row) for row in reader]
```

- This function returns a list of tuples containing all your scenarios.

## Step 4: Refactor the test logic to use parameterization

```python
import pytest

@pytest.mark.parametrize("username,email,password", load_registration_data())
def test_register_data_driven(driver, username, email, password):
    driver.get("http://localhost:8000/register.html")
    driver.find_element(By.ID, "username").clear()
    driver.find_element(By.ID, "username").send_keys(username)
    driver.find_element(By.ID, "email").clear()
    driver.find_element(By.ID, "email").send_keys(email)
    driver.find_element(By.ID, "password").clear()
    driver.find_element(By.ID, "password").send_keys(password)
    driver.find_element(By.ID, "submit").click()
    assert "Registration successful" in driver.page_source
```

- Each row in the CSV is now a test case.
- Add or remove users without touching the test logic.

> _Tip_: To handle errors where registration fails, add a column for the expected result or error message in your data file.

## Step 5: (Optional) Simplify setup with fixtures

```python
import pytest

@pytest.fixture
def registration_data():
    return load_registration_data()
```

- Fixtures allow you to switch between data sources or provide additional setup for specific test cases.

## Knowledge check

❓ What are two key advantages of separating test logic from data? Give an example showing how this approach makes it easier to adapt your tests as requirements change.
