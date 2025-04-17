# 🔁 Excel to YAML API Test Automation Framework

This project enables automated testing of RESTful APIs by converting test case data from an Excel file to YAML, then executing tests using **Java**, **RestAssured**, and **JUnit 5**. Results are reported using **Allure** for easy visualization.

---

## 📦 Tech Stack

| Layer           | Technology                        |
|----------------|------------------------------------|
| Language        | Java 17+                          |
| Test Framework  | JUnit 5                           |
| API Testing     | RestAssured                       |
| Excel Parser    | Apache POI                        |
| YAML Parser     | SnakeYAML                         |
| Reporting       | Allure                            |
| Build Tool      | Maven                             |
| Framework       | Spring Boot (for Excel to YAML)   |

---

## 🧾 Excel Test Case Format (`testData.xlsx`)

| Column    | Description |
|-----------|-------------|
| `name`    | Test case name |
| `method`  | HTTP method (GET, POST, PUT, DELETE) |
| `endpoint`| API endpoint path |
| `headers` | Headers in format `key:value;key2:value2` |
| `body`    | Raw JSON request body (as string) |
| `status`  | Expected HTTP status code (e.g. `200`) |

### ✅ Example (inside Excel):
| name              | method | endpoint          | headers                         | body                                                | status |
|-------------------|--------|-------------------|----------------------------------|-----------------------------------------------------|--------|
| create user       | POST   | /api/users        | Content-Type:application/json    | {"name":"morpheus", "job":"leader"}                 | 201    |
| fetch user        | GET    | /api/users?page=2 | Content-Type:application/json    |                                                     | 200    |
| update user       | PUT    | /api/users/2      | Content-Type:application/json    | {"name":"neo", "job":"zion leader"}                 | 200    |
| delete user       | DELETE | /api/users/2      | Content-Type:application/json    |                                                     | 204    |


## 🧬 YAML Output Format (`testcases.yaml`)

Automatically generated after Excel conversion:

```yaml
- name: create user
  method: POST
  endpoint: /api/users
  headers:
    Content-Type: application/json
  body: '{"name":"morpheus", "job":"leader"}'
  status: 201
Project structure
├── src/
│   ├── main/java/org/hbn/flowcheck/utils/
│   │   ├── Exceltoyaml.java       # Converts Excel to YAML
│   │   └── Integration.java       # Spring Boot entry point
│   ├── test/java/org/hbn/flowcheck/
│   │   └── ApiTest.java           # API tests using JUnit & RestAssured
│   ├── main/resources/testData.xlsx      # Input Excel file
│   └── test/resources/testcases.yaml     # Output YAML file
├── target/allure-results/

Work flow:

 Excel (testData.xlsx)
       ⬇
 [Step 1] Convert Excel to YAML
 → Run `Exceltoyaml.java` via Spring Boot
 → Output: `testcases.yaml`

       ⬇
 [Step 2] API Test Execution
 → Run `ApiTest.java` with JUnit 5 & RestAssured
 → Reads test cases from `testcases.yaml`
 → Executes HTTP requests
 → Validates responses

       ⬇
 [Step 3] Generate Allure Report
 → Run: `allure serve target/allure-results`
 → View report in browser

View Allure Report
allure serve target/allure-results


     
