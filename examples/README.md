# Browser Automation Examples

Example n8n workflows demonstrating Puppeteer and Playwright usage in Code nodes.

## Importing Workflows

1. Open your n8n instance
2. Go to **Workflows** > **Add Workflow** > **Import from File**
3. Select any `.json` file from this folder

## Available Examples

### Puppeteer

| File | Description |
|------|-------------|
| `puppeteer-screenshot.json` | Take a screenshot of a webpage and return as binary |
| `puppeteer-scrape.json` | Scrape Hacker News top stories |
| `puppeteer-stealth.json` | Use stealth plugin to avoid bot detection |

### Playwright

| File | Description |
|------|-------------|
| `playwright-screenshot.json` | Take a screenshot of a webpage and return as binary |
| `playwright-scrape.json` | Scrape Hacker News top stories using Playwright locators |
| `playwright-pdf.json` | Generate a PDF from a webpage |
| `playwright-stealth.json` | Use stealth plugin to avoid bot detection |

## Key Differences

### Puppeteer (Basic)
```javascript
const puppeteer = require('puppeteer-core');

const browser = await puppeteer.launch({
  executablePath: '/usr/bin/chromium-browser',
  headless: true,
  args: ['--no-sandbox', '--disable-setuid-sandbox']
});
```

### Puppeteer (Stealth)
```javascript
const puppeteer = require('puppeteer-extra');
const StealthPlugin = require('puppeteer-extra-plugin-stealth');

puppeteer.use(StealthPlugin());

const browser = await puppeteer.launch({
  executablePath: '/usr/bin/chromium-browser',
  headless: true,
  args: ['--no-sandbox', '--disable-setuid-sandbox']
});
```

### Playwright (Basic)
```javascript
const { chromium } = require('playwright-core');

const browser = await chromium.launch({
  executablePath: '/usr/bin/chromium-browser',
  headless: true,
  args: ['--no-sandbox', '--disable-setuid-sandbox']
});
```

### Playwright (Stealth)
```javascript
const { chromium } = require('playwright-extra');
const StealthPlugin = require('puppeteer-extra-plugin-stealth');

chromium.use(StealthPlugin());

const browser = await chromium.launch({
  executablePath: '/usr/bin/chromium-browser',
  headless: true,
  args: ['--no-sandbox', '--disable-setuid-sandbox']
});
```

## Stealth Mode

The stealth plugins help avoid bot detection by:
- Hiding webdriver flags
- Spoofing plugin enumeration
- Fixing iframe contentWindow
- Mocking chrome.runtime
- Spoofing navigator properties
- And many more evasions

Test your stealth setup at: https://bot.sannysoft.com

## Required Browser Args

Always include these args for Docker environments:
- `--no-sandbox` - Required for running as root
- `--disable-setuid-sandbox` - Required for containerized environments
- `--disable-dev-shm-usage` - Prevents /dev/shm issues in Docker
- `--disable-gpu` - Not needed in headless mode

## Tips

- Always call `browser.close()` to free resources
- Use `waitUntil: 'networkidle'` (Playwright) or `waitUntil: 'networkidle0'` (Puppeteer) for pages with async content
- Use stealth mode when scraping sites with bot detection
- Return binary data using the `binary` property in the output object
