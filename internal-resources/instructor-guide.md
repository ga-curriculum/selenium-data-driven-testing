<h1>
  <span class="headline">Selenium: Data-Driven Testing</span>
  <span class="subhead">Instructor Guide</span>
</h1>

## Introduction to data-driven testing

### Delivery guidance

- Begin by introducing the practical problems learners may have experienced: repetitive scripts, duplicated code, and maintenance challenges when automating similar UI flows with different inputs.
- Use the provided analogy (invitations for a global event) to help learners relate to the challenge of scaling test coverage with minimal code changes.
- Highlight the inclusion and diversity in sample test data (use names from various regions/cultures).
- Encourage learners to share real-world examples from their own or their team's work, and to draw connections to how better test coverage can aid quality and team efficiency.
- During the activity, move between groups or individuals to offer support in updating both the data files and test scripts, and assist with interpreting test results.
- Use the discussion prompts to encourage sharing of strategies for scaling tests and handling complex or changing requirements.

### Answer to knowledge check

**Sample answer:** When password rules change (such as requiring symbols), you can simply update your data file to include both compliant and non-compliant passwords. You do not need to rewrite your test functions, just add or adjust the data source. This ensures that your tests adapt quickly to new requirements, and you can check multiple edge cases with minimal effort.

### Suggested solution to the activity

- The revised test should:
  - Import and use _csv.DictReader_ or equivalent to loop over external data.
  - Replace hard-coded inputs with references to the data row fields.
  - Allow seamless addition or removal of scenarios by editing the data file.
  - Optionally, use test automation best practices (isolating tests as needed, appropriate assertions for expected outcomes).
- Example:

  data_driven_example.py

  ```python
  import csv

  def test_scenario_data_driven(driver):
      with open("testdata.csv", newline='') as csvfile:
          datareader = csv.DictReader(csvfile)
          for row in datareader:
              driver.get("http://localhost:8000/page.html")
              driver.find_element(By.ID, "input").send_keys(row['input_value'])
              driver.find_element(By.ID, "submit").click()
              # Add relevant assertions for expected result
  ```

---

## Working with External Data Sources

### Delivery guidance

- Begin with a relatable challenge, such as testing a login feature for a global audience or an application with rapidly changing requirements.
- Emphasize how separating data from logic directly builds on previous concepts and unlocks more sophisticated, maintainable tests.
- Use the narratives and globally inclusive examples to illustrate why CSV and JSON formats are chosen in real projects.
- During the activity, encourage learners to modify the provided data files—add new users, new languages, or extra fields—to observe how easily the code adapts.
- Facilitate the discussion prompt to invite learners to connect data structures to their own test cases and to anticipate future scalability needs.
- For remote groups, prompt learners to share sample test data that fits their regional context or specific project requirements.

### Knowledge check answer

**Sample answer:**  
JSON is preferable. CSV files work well for simple, flat data, but representing users with multiple groups and unique permissions requires nesting and arrays, which JSON supports. JSON handles complex, hierarchical structures more naturally than CSV.

_Ensure asset suggestions are shared with the asset design team for creation. All code and file names follow modular style guidelines and use globally relevant, inclusive data._

---

## Parameterizing Selenium Tests

### Delivery guidance

- Use the analogy of filling out event forms for many global participants to introduce the value of parameterized tests.
- Invite learners to share countries or regions they want to represent in their test data, fostering a globally inclusive mindset.
- Walk through both pytest-based and manual for-loop approaches with clear coding demonstrations.
- During guided practice, circulate to support correct CSV formatting, encourage the use of global data variations, and troubleshoot errors related to skipping headers or field clearing.
- In knowledge checks and discussions, prompt learners to critically evaluate the trade-offs between different parameterization approaches, connecting back to real project needs.
- Encourage small groups or pairs to brainstorm and propose challenging test scenarios from diverse linguistic or cultural backgrounds before expanding their data files.

### Answers

**Knowledge check sample answers:**
_pytest.mark.parametrize_ is best when you need each test run reported separately—making it ideal for regression suites, continuous integration, and large, collaborative projects. Manual for-loops are better for exploratory testing, complex flows, or when you need custom handling in each test cycle.

**Sample solution for the registration activity:**

- Create a <code class="filepath">registration.csv</code> file in the <code class="filepath">/code> directory.
- Use _csv.reader_ (with _next(reader)_ to skip header) or _csv.DictReader_ in your test logic.
- Implement the pytest parameterization as shown above.
- Add one or more diverse, global edge case scenarios as new CSV rows.
- Run pytest to confirm a unique test for each data row. All field values should be driven by the data source, with the logic itself untouched.

---

## Integrating API Data in Selenium Tests

### Delivery guidance

- Begin by connecting the importance of dynamic, real-world data to situations learners may recognize: displaying the current temperature, stock prices, or exchange rates in an app.
- Emphasize inclusivity through examples that are globally relevant, such as choosing weather for Paris, Tokyo, or Nairobi, rather than defaulting to a single locale.
- Remind learners how this builds naturally on previous work with CSV and JSON, highlighting the move from static to truly dynamic input.
- During the activity, clarify any points of confusion about the _requests_ library and how to inspect API JSON structure before extracting fields.
- Encourage learners to experiment with changing the city parameter in the API or adjusting the widget value to see test pass/fail behavior.
- Facilitate the interaction by prompting discussion about where dynamic data increases test effectiveness versus where it may introduce flakiness or false failures.
- Suggest learners propose API integration points from their real projects—such as checking news headlines, price feeds, or localization elements.

### Knowledge check answer

**Sample answer:**

1. **Network or API reliability:** If the API is down or slow, your test may fail for reasons unrelated to your app. Solution: Add error handling to check response status, and fail with clear messages when the API does not return data.
2. **Live data changing during tests:** If your app and the API update at different times, small mismatches can cause false failures. Solution: Allow for a small tolerance in values (for example, ±1 for temperature) and use clear assertions to explain any difference.

### Suggested solution to the activity

- The test should fetch live temperature from the API using _requests_.
- The Selenium test should grab the displayed value, normalize it (remove _°C_, spaces), and compare it to the API value.
- The assertion should allow for a 1-degree difference to avoid failures due to data lag.
- If the stretch goal is attempted, a helper function should accept a city as an argument and use it in both the API call and the UI check.

Example code snippets:

```python
import requests

def get_live_temperature(city):
    url = f"https://wttr.in/{city}?format=j1"
    response = requests.get(url)
    if response.status_code == 200:
        return response.json()['current_condition'][0]['temp_C']
    else:
        return None

def test_temperature_widget(driver):
    city = "Paris"
    live_temp = get_live_temperature(city)
    assert live_temp is not None, "Failed to fetch live temperature"

    driver.get("http://localhost:8000/temperature_widget.html")
    widget_value = driver.find_element(By.ID, "temp-widget").text
    widget_temp = widget_value.strip().replace("°C", "")

    assert abs(int(widget_temp) - int(live_temp)) <= 1
```

---

## Refactoring for Scalability and Clarity

### Delivery guidance

- Begin by inviting learners to share frustrations with maintaining duplicated tests for many user or input scenarios.
- Use real-world analogies—organizing warehouses, managing an international classroom, or updating logistics across global locations—to anchor the lesson in something concrete.
- Reinforce that refactoring for clarity and scalability builds on earlier learning with data sources and parameterization.
- Encourage inclusive thinking: prompt learners to expand data files with globally diverse cases—names, languages, and formats not common in just one region.
- Walk through each step—extracting data, loading it, and refactoring logic—by modeling both the code and the rationale behind each move.
- In group interactions, support learners in reflecting on where tangled logic and data have impeded their testing agility and team collaboration.
- For remote groups, use collaborative documents for mapping out bottlenecks, pain points, and refactoring strategies.
- When troubleshooting, help learners confirm they are correctly parsing CSV headers and resetting state in parameterized tests.

### Knowledge check answer

**Sample answer:**  
Two key advantages are (1) scalability—adding new scenarios is as simple as adding rows to a data file, with no code changes needed, and (2) clarity—test logic stays clean and easy to read, while data files are the single source for inputs. For example, when a product manager adds a requirement for passwords to include a symbol, you simply add new rows to your CSV file with symbol-containing passwords. The same test logic checks these cases automatically, so updates are both faster and less prone to errors.

---

🏗️ **Under Construction**

We are constantly working to improve our resources for instructors and students.

**Have something to contribute to this Instructor Guide?** [Let us know](https://pages.git.generalassemb.ly/modular-curriculum-all-courses/universal-resources-internal/module-feedback.html).
