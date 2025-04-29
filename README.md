# Cloudflare Captcha Bypass

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
![Python Version](https://img.shields.io/badge/python-3.7%2B-blue)
![Build Status](https://img.shields.io/badge/build-passing-brightgreen)
![Last Commit](https://img.shields.io/badge/last%20commit-April%202025-brightgreen)

An advanced tool to bypass Cloudflare and other protection systems (including CAPTCHA, Turnstile, JavaScript challenges) for web scraping projects. Solve the common "Checking your browser before accessing" loop that traditional web drivers get stuck in.

## Features

- **Multi-Protection Support**: Bypasses Cloudflare, reCAPTCHA, hCaptcha, Imperva, Akamai, and more
- **Browser Profile Management**: Save and reuse sessions for better success rates
- **Proxy Rotation**: Automatically cycle through proxies to avoid IP blocking
- **Fingerprint Randomization**: Avoid detection with realistic browser fingerprints
- **Headless Operation**: Run in headless mode for server environments
- **API Server Mode**: Deploy as a service with RESTful API endpoints
- **Multi-Threaded Processing**: Handle multiple requests simultaneously
- **Integration Examples**: Ready-to-use code samples for popular scraping libraries
- **Command-Line Interface**: Easy-to-use CLI for quick operations
- **User-Friendly Interface**: Interactive console menu for all operations
- **Docker Support**: Containerized deployment for easy scaling
- **Comprehensive Logging**: Detailed logs for debugging and monitoring
- **Automated Driver Management**: Auto-download compatible Chrome drivers

## How It Works

Traditional Selenium-based automation gets detected and blocked by Cloudflare's protection, leaving you stuck in the "Checking your browser before accessing" page - even after clicking "I'm not a robot."

This tool solves this by using a specialized approach:

1. **Deep Browser Integration**: Controls the browser at a lower level than traditional webdrivers
2. **Shadow DOM Penetration**: Accesses elements within shadow DOM structures
3. **Fingerprint Management**: Randomizes or maintains consistent fingerprints as needed
4. **Protection Detection**: Automatically identifies and handles various protection systems
5. **Smart Waiting**: Uses intelligent timing for challenge resolution
6. **Cookie Management**: Properly stores and reuses authentication cookies

The result is a reliable tool that can bypass protection systems that block traditional scrapers.

## Installation

### Quick Install (Recommended)

Run the installer script for a guided setup:

```bash
python install.py --all
```

### Manual Installation

Clone the repository:

```bash
git clone https://github.com/zebbern/captcha-bypass.git
cd captcha-bypass
```

Install the required packages:

```bash
pip install -r functions/requirements.txt
pip install -r functions/server_requirements.txt  # For server mode
```

### Docker Installation

```bash
docker build -t cloudflare-bypass .
docker run -p 8000:8000 cloudflare-bypass
```

## Quick Start

### Basic Usage

Run the main script for an interactive menu:

```bash
python main.py
```

Or use the command-line interface for direct operations:

```bash
# Bypass protection and get cookies
python captcha-bypass.py bypass https://example.com --output cookies.json

# Start API server
python main.py --server --port 8000
```

### Code Integration

```python
from functions.CloudflareBypasser import CloudflareBypasser
from DrissionPage import ChromiumPage

# Initialize a browser
driver = ChromiumPage()
driver.get('https://example.com')

# Create and use the bypasser
cf_bypasser = CloudflareBypasser(driver)
cf_bypasser.bypass()

# Continue with your scraping
print(f"Page title: {driver.title}")
```

## Components

The project consists of several components that work together:

- **CloudflareBypasser**: Core component for bypassing protection
- **API Server**: RESTful API for remote access
- **Browser Profile Manager**: Manages browser profiles and sessions
- **Proxy Manager**: Handles proxy rotation and testing
- **Fingerprint Generator**: Creates and applies realistic browser fingerprints
- **WebDriver Manager**: Automatically downloads compatible Chrome drivers
- **User Agent Rotator**: Generates and rotates realistic user agents
- **Protection Detector**: Identifies various protection systems

## Advanced Features

### Browser Profiles

Browser profiles store cookies and settings for specific domains, improving bypass success rates:

```python
# Create a profile for a domain
python main.py profiles create --domain example.com

# Use a specific profile for bypassing
python captcha-bypass.py bypass https://example.com --profile example_profile
```

### Proxy Rotation

Rotate through multiple proxies to avoid IP blocking:

```python
# Add proxies to the pool
python main.py proxies add http://user:pass@host:port

# Use proxy rotation in bypass operations
python captcha-bypass.py bypass https://example.com --proxy-rotate
```

### Fingerprint Randomization

Generate realistic browser fingerprints to avoid detection:

```python
# Generate a random fingerprint
from functions.fingerprint_generator import fingerprint_generator
fingerprint = fingerprint_generator.generate()

# Apply fingerprint to browser
fingerprint_generator.apply_to_page(driver, fingerprint)
```

## API Server

The API server provides RESTful endpoints for remotely bypassing protection systems.

### Starting the Server

```bash
python main.py --server --port 8000
```

### API Endpoints

#### 1. Get Cookies

```
GET /cookies?url=<URL>&retries=<NUMBER>&proxy=<PROXY>
```

Returns Cloudflare cookies that can be used in other requests.

#### 2. Get HTML Content

```
GET /html?url=<URL>&retries=<NUMBER>&proxy=<PROXY>
```

Returns the complete HTML content of the protected website.

#### 3. Advanced Bypass

```
POST /bypass
```

Full-featured endpoint with more options:

```json
{
  "url": "https://example.com",
  "retries": 5,
  "proxy": "http://user:pass@host:port",
  "profile_name": "example_profile",
  "headless": true,
  "return_html": true
}
```

## Dashboard

Access the web dashboard for monitoring and control:

```bash
# Start server with dashboard enabled
python main.py --server --dashboard
```

Then open your browser at: http://localhost:8000/dashboard

## Library Integration

The tool can be integrated with popular scraping libraries:

### BeautifulSoup

```python
# See examples/integration_example.py for complete code
from functions.CloudflareBypasser import CloudflareBypasser
import requests
from bs4 import BeautifulSoup

# Get cookies using the bypasser
cf_bypasser = CloudflareBypasser(driver)
cf_bypasser.bypass()
cookies = {cookie.get("name", ""): cookie.get("value", "") for cookie in driver.cookies()}

# Use the cookies with requests and BeautifulSoup
response = requests.get(url, cookies=cookies, headers={"User-Agent": driver.user_agent})
soup = BeautifulSoup(response.text, 'html.parser')
```

### Scrapy

```python
# See examples/integration_example.py for Scrapy middleware implementation
class CloudflareBypassMiddleware:
    """Scrapy middleware for bypassing Cloudflare protection."""
    
    async def process_request(self, request, spider):
        # Bypass Cloudflare and return response
        # ...
```

## Performance Tips

- **Use Browser Profiles**: Reuse profiles for better success rates
- **Proxy Rotation**: Rotate proxies to avoid IP blocking
- **Headless Mode**: Use headless mode for better performance
- **Server Mode**: Deploy as a service for handling multiple requests
- **Optimize Timeouts**: Adjust timeouts based on your needs
- **Cache Results**: Cache results to avoid unnecessary bypassing

## Troubleshooting

- **Chrome/Chromium Version**: Ensure your Chrome version is compatible with the DrissionPage version
- **Timeout Issues**: Adjust timeout values in settings.py if you encounter timeout errors
- **IP Blocking**: If your IP gets blocked, use proxy rotation
- **Import Errors**: Make sure you're running from the project root directory
- **Permission Issues**: In Linux environments, check executable permissions for Chrome
- **Driver Compatibility**: Use the built-in WebDriver Manager to download compatible drivers

> [!Warning]
This tool is for educational and research purposes only. The author is not responsible for any misuse of this software. Always respect website terms of service and robots.txt directives.
