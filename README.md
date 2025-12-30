# PuppeteerでCAPTCHAをバイパスする

[![Promo](https://github.com/bright-jp/LinkedIn-Scraper/blob/main/Proxies%20and%20scrapers%20GitHub%20bonus%20banner.png)](https://brightdata.jp/products/web-unlocker) 

Puppeteerで人間の挙動を模倣してCAPTCHAをバイパスするためのクイックガイドです。ガイドをスキップする場合は、Bright Dataにサインアップして[Web Unlocker API](https://brightdata.jp/products/web-unlocker)をオプトインしてください。 

## Step 1: プロジェクトのセットアップ

```bash
mkdir bypass_captcha_puppeteer
cd bypass_captcha_puppeteer
npm init -y
npm install puppeteer
```

構成:

```bash
bypass_captcha_puppeteer/
├── index.js
└── package.json
```

```package.json```に```"type"```: ```"module"```を含めます:

```json
{
  "name": "bypass_captcha_puppeteer",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "type": "module",
  "scripts": {
    "start": "node index.js"
  },
  "dependencies": {
    "puppeteer": "^23.10.4"
  }
}
```

## Step 2: Puppeteerをテスト（ステルスなし）

```javascript
import puppeteer from 'puppeteer';
const visitBotAnalyzerPage = async () => {
  try {
    const browser = await puppeteer.launch();
    const page = await browser.newPage();
    const url = 'https://bot.sannysoft.com/';
    console.log(`Navigating to ${url}...`);
    await page.goto(url, { waitUntil: 'networkidle2' });
    console.log('Taking full-page screenshot...');
    await page.screenshot({ path: 'anti-bot-analysis.png', fullPage: true });
    console.log('Screenshot taken');
    await browser.close();
    console.log('Browser closed');
  } catch (error) {
    console.error('An error occurred:', error);
  }
};
// run the script
visitBotAnalyzerPage();
```

実行:

```bash
node index.js
```

一部のボットチェックに失敗し、CAPTCHAが表示される可能性があります。

## Step 3: ステルスプラグインをインストール

```bash
npm install puppeteer-extra puppeteer-extra-plugin-stealth
```

```puppeteer```のimportを```puppeteer-extra```に置き換え、プラグインを追加します:

```javascript
import puppeteer from 'puppeteer-extra';
import StealthPlugin from 'puppeteer-extra-plugin-stealth';

// Add the stealth plugin
puppeteer.use(StealthPlugin());

const visitBotAnalyzerPage = async () => {
  try {
    const browser = await puppeteer.launch();
    console.log('Launching browser in stealth mode...');
    const page = await browser.newPage();
    const url = 'https://bot.sannysoft.com/';
    console.log(`Navigating to ${url}...`);
    await page.goto(url, { waitUntil: 'networkidle2' });
    console.log('Taking full-page screenshot...');
    await page.screenshot({ path: 'anti-bot-analysis.png', fullPage: true });
    console.log(`Screenshot taken`);
    await browser.close();
    console.log('Browser closed. Script completed successfully');
  } catch (error) {
    console.error('Error occurred:', error);
  }
};
// run the script
visitBotAnalyzerPage();
```

再度実行:

```bash
node index.js
```

ステルスによりボット検知とCAPTCHAが減少します。

## ステルスだけでは不十分な場合

高度なWAFやボット検知に対しては、追加プラグイン（例: ```puppeteer-extra-plugin-anonymize-ua```）または専用ツールを試してください。[Bright Data’s Web Unlocker API](https://brightdata.jp/products/web-unlocker)のようなツールは、reCAPTCHA、hCaptcha、その他多くの要素に対応します。