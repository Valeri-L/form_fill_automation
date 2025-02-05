# Selenium Automation for Microsoft Account Login and Tenant Access

This project utilizes Selenium to automate the process of logging into Microsoft accounts and adding users to the required tenant. If the account has two-factor authentication (2FA), it logs the username and the reason for failure (2FA). Additionally, if the password is incorrect, it logs the username and this reason in the `report.csv` file.

## Requirements

- Python
- Selenium
- Chrome WebDriver

## How to Use

1. **Installation**
    - Make sure you have Python installed on your system.
    - Install Selenium by running `pip install selenium`.
    - Download the Chrome WebDriver compatible with your Chrome browser version and place it in your PATH.

2. **Configuration**
    - Update the `usernames.py` file with your Microsoft account username and password.

3. **Running the Script**
    - Execute the `main.py` script.
    - The automation will open Chrome, log in to the Microsoft account, and perform the necessary actions to add the user to the required tenant.

4. **Handling 2FA**
    - If the account has 2FA enabled, the script will log the username and the reason for failure (2FA) in the `report.csv` file.

5. **Handling Incorrect Password**
    - If the password provided is incorrect, the script will log the username and this reason in the `report.csv` file.

## File Structure

- `main.py`: Main script to run the automation.
- `usernames.py`: Configuration file containing the Microsoft account username and password.
- `report.csv`: CSV file to log usernames and reasons for failure.

## Example Usage

```python
# Import the necessary modules
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
import time

# Load the Chrome WebDriver
driver = webdriver.Chrome()

# Open the Microsoft login page
driver.get("https://login.live.com")

# Find the username input field and enter the username
username_field = driver.find_element_by_id("i0116")
username_field.send_keys("your_username")

# Find the password input field and enter the password
password_field = driver.find_element_by_id("i0118")
password_field.send_keys("your_password")

# Submit the login form
password_field.send_keys(Keys.RETURN)

# Wait for some time for the page to load
time.sleep(5)

# Perform actions to add user to the required tenant...

# Close the browser
driver.close()
