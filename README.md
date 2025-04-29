## CloudflareBypasser

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
![Python Version](https://img.shields.io/badge/python-3.7%2B-blue)

A powerful tool to bypass Cloudflare's bot protection for web scraping projects. Solve the common "Checking your browser before accessing" loop that traditional web drivers get stuck in.

## How It Works

Traditional Selenium-based automation gets detected and blocked by Cloudflare's protection, leaving you stuck in the "Checking your browser before accessing" page - even after clicking "I'm not a robot."

**CloudflareBypasser** solves this by using DrissionPage, which controls the browser at a deeper level. The browser isn't detected as a webdriver, successfully bypassing Cloudflare protection.

## Features

- **Full Cloudflare Bypass**: Get past the protection that blocks traditional Selenium webdrivers
- **Simple Integration**: Add just a few lines to your existing code
- **Server Mode**: Deploy as a service to handle Cloudflare challenges remotely
- **Docker Support**: Run the service in a containerized environment
- **Proxy Support**: Configure with proxies for distributed access

## Requirements

- Python 3.7+
- Chrome/Chromium browser

## Installation

Install the required packages:

```bash
pip install -r requirements.txt
```

For server mode:

```bash
pip install -r server_requirements.txt
```

## Usage

### Basic Usage

```python
from CloudflareBypasser import CloudflareBypasser
from DrissionPage import ChromiumPage

# Initialize a browser
driver = ChromiumPage()
driver.get('https://nopecha.com/demo/cloudflare')

# Create and use the bypasser
cf_bypasser = CloudflareBypasser(driver)
cf_bypasser.bypass()

# Now you can continue with your scraping
```

### Testing

Run the included test script to see the bypasser in action:

```bash
python test.py
```

## Server Mode

Server Mode allows you to bypass Cloudflare protection remotely. Get either cookies or complete HTML content from protected sites.

### Starting the Server

```bash
python server.py
```

### API Endpoints

The server provides two main endpoints:

#### 1. Get Cookies

```
GET /cookies?url=<URL>&retries=<NUMBER>&proxy=<PROXY>
```

Returns Cloudflare cookies that can be used in other requests.

**Example request:**
```bash
curl http://localhost:8000/cookies?url=https://nopecha.com/demo/cloudflare
```

**Example response:**
```json
{
  "cookies": {
    "cf_clearance": "SJHuYhHrTZpXDUe8iMuzEUpJxocmOW8ougQVS0.aK5g-1723665177-1.0.1.1-5_NOoP19LQZw4TQ4BLwJmtrXBoX8JbKF5ZqsAOxRNOnW2rmDUwv4hQ7BztnsOfB9DQ06xR5hR_hsg3n8xteUCw"
  },
  "user_agent": "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/125.0.0.0 Safari/537.36"
}
```

#### 2. Get HTML Content

```
GET /html?url=<URL>&retries=<NUMBER>&proxy=<PROXY>
```

Returns the complete HTML content of the protected website.

### Parameters

- `url`: (Required) URL of the website to bypass
- `retries`: (Optional) Number of retry attempts if bypass fails
- `proxy`: (Optional) Proxy server to use for the request

## Docker Deployment

### Build from Source

```bash
docker build -t cloudflare-bypass .
```

### Run the Container

```bash
docker run -p 8000:8000 cloudflare-bypass
```

### Use Pre-built Image

Skip the build step and use my pre-built image:

```bash
docker run -p 8000:8000 ghcr.io/zebbern/captcha-bypass:latest
```

## Troubleshooting

- **Chrome/Chromium Version**: Ensure your Chrome version is compatible with the DrissionPage version
- **Timeout Issues**: Increase timeout values if you encounter timeout errors
- **IP Blocking**: If your IP gets blocked, consider using proxy rotation

## Limitations

- Cloudflare continuously updates its protection mechanisms; this tool may require updates
- Some high-security sites may implement additional protection measures
- Performance depends on your network connection and the target website's response time
