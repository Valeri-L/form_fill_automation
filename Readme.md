# Selenium Automation for Microsoft Account Login and Tenant Access

This project uses **Python** and **Selenium** to automate Microsoft account login and complete a tenant / partnership access flow.

The script loops through a list of usernames, attempts to sign in using configured passwords, handles common Microsoft login screens, checks for login errors, handles security / 2FA-related blocks, completes the required tenant access confirmation steps, and writes the result of each account to a CSV report.

> This project should only be used with accounts and tenants you own or are authorized to manage.

---

## Features

* Automates Microsoft login using Selenium and Chrome WebDriver.
* Supports multiple usernames from `usernames.py`.
* Supports multiple passwords from `password_list`.
* Detects invalid usernames.
* Detects invalid passwords.
* Detects security / 2FA-related blocks.
* Handles Microsoft “Stay signed in?” and security defaults screens when possible.
* Completes the tenant / partnership authorization form.
* Writes success and failure results to `report.csv`.

---

## Requirements

* Python 3
* Google Chrome
* ChromeDriver
* Selenium

Install Selenium:

```bash
pip install selenium
```

---

## Project Structure

```txt
.
├── main.py              # Main automation script
├── usernames.py         # Username and password lists
├── settings.py          # ChromeDriver path and destination URL
├── report_csv.py        # CSV reporting logic
└── report.csv           # Generated report file
```

---

## Configuration

### `settings.py`

Create or update `settings.py` with the ChromeDriver path and the destination URL:

```python
CHROME_DRIVER_PATH = "path/to/chromedriver"
DESTINATION_URL = "https://your-destination-url.com"
```

### `usernames.py`

Create or update `usernames.py` with the users and passwords you want to process:

```python
username_list = [
    "user1@example.com",
    "user2@example.com",
]

password_list = [
    "password1",
    "password2",
    "password3",
    "password4",
]
```

> Do not commit real usernames, passwords, or sensitive tenant URLs to GitHub. Add files containing secrets to `.gitignore`.

---

## How It Works

The script runs the automation in this order:

1. Opens Chrome using the configured ChromeDriver path.
2. Navigates to `DESTINATION_URL`.
3. Enters the username.
4. Checks whether the username is valid.
5. Tries the configured passwords.
6. Logs failure if all password attempts fail.
7. Checks whether Microsoft security defaults or 2FA blocks the flow.
8. Handles the “Stay signed in?” screen if it appears.
9. Completes the partnership / tenant access confirmation form.
10. Writes the final result to `report.csv`.

---

## Running the Script

Run:

```bash
python main.py
```

The script will loop through every username in `username_list`:

```python
for username in username_list:
    form = AutomatedFormFill(DESTINATION_URL, username)
    form.activate()
```

---

## CSV Report

The script writes results using `ReportToCsv`.

Possible statuses:

```python
{
    "1": "Added",
    "0": "Not Added"
}
```

Example report outcomes:

```txt
username@example.com,Added,account added successfully
username2@example.com,Not Added,usernameError
username3@example.com,Not Added,passwordError
username4@example.com,Not Added,needed 2FA to proceed
```

---

## Main Files

### `main.py`

Contains the `AutomatedFormFill` class.

Main methods:

| Method                        | Purpose                                                             |
| ----------------------------- | ------------------------------------------------------------------- |
| `credential_validation()`     | Checks Microsoft login error messages for username/password errors. |
| `password_all()`              | Enters a password and submits the login form.                       |
| `credentials_fill()`          | Handles username and password input.                                |
| `security_defaults_windows()` | Handles Microsoft security defaults / 2FA-related screens.          |
| `check_auth_partnership()`    | Completes the final tenant / partnership form.                      |
| `activate()`                  | Runs the full automation flow.                                      |

---

## Important Notes

* This automation depends on Microsoft page element IDs and class names. If Microsoft changes the login UI, some selectors may need to be updated.
* The script uses `sleep()` in multiple places. For better reliability, `WebDriverWait` can be used instead.
* The script currently catches broad exceptions in several places. This prevents crashes, but it can also hide bugs during debugging.
* If you use Selenium 4, you may need to update the ChromeDriver initialization to use `Service`.

Example Selenium 4 style:

```python
from selenium.webdriver.chrome.service import Service

service = Service(CHROME_DRIVER_PATH)
driver = webdriver.Chrome(service=service)
```

---

## Security Warning

This tool is intended only for authorized automation on accounts and tenants you are allowed to manage.

Do not use this project for unauthorized login attempts, account testing, password guessing, or access to systems you do not own or administer.
