# 🚀 Bright Data の Scraping Browser

*Puppeteer、Selenium、Playwright を用いた動的Webスクレイピング向けの、完全自動化されたヘッドレスブラウザソリューションです。Scraping Browser は Bright Data のインフラ上で GUI として開きます。*  

[![Promo](https://github.com/luminati-io/LinkedIn-Scraper/raw/main/Proxies%20and%20scrapers%20GitHub%20bonus%20banner.png)](https://brightdata.jp/products/scraping-browser) 

🔗 **[無料で始める](https://brightdata.jp/products/scraping-browser)** | 📖 **[公式ドキュメント](https://docs.brightdata.com/scraping-automation/scraping-browser/introduction)**  

---

## 🔹 概要  
Scraping Browser は、**ブラウザベースのスクレイピングソリューション**としてフルホストで提供され、**組み込みのプロキシ管理**、**CAPTCHA 解決**、**高度なサイトのブロック解除**により、**マルチステップのデータ収集**を自動化します。**Puppeteer、Playwright、Selenium** をサポートし、**無制限のスケール**で容易にWeb自動化を実現できます。  

## ✅ Scraping Browser を使う理由  
- **インフラ運用のオーバーヘッドなし** – ブラウザインフラを維持することなく、API でブラウザセッションを実行・スケールできます。  
- **組み込みのアンロック機能** – **CAPTCHA、フィンガープリント、リトライ、JS レンダリング**を内部で自動処理します。  
- **マルチステップのナビゲーション** – クリック、スクロール、フォーム送信、ホバー操作を自動化します。  
- **無制限のスケーリング** – レート制限なしで **数千の同時ブラウザセッション**を起動できます。  
- **グローバルなジオアクセス** – [**195か国にわたる 72M+ のレジデンシャルIP**](https://brightdata.jp/proxy-types/residential-proxies)でローカライズされたコンテンツをアンロックできます。  
- **シームレスなデバッグ** – **Chrome DevTools 連携**により、セッションをリアルタイムで監視できます。  

---

## 🚀 はじめに  

### 依存関係のインストール  
お使いの好みのWeb自動化ライブラリと併せて、**Node.js**、**Python**、または **C#** がインストールされていることを確認してください。  

```sh
# Install Puppeteer
npm install puppeteer-core

# Install Playwright
npm install playwright

# Install Selenium
pip install selenium
```

## 🔧 使用例

### Puppeteer 例（JavaScript）

```js
const puppeteer = require('puppeteer-core');

const SBR_WS_ENDPOINT = 'wss://brd-customer-<YOUR-USERNAME>-zone-<YOUR-ZONE-NAME>:<YOUR-PASSWORD>@brd.superproxy.io:9222';

async function main() {
    console.log('Connecting to Scraping Browser...');
    const browser = await puppeteer.connect({ browserWSEndpoint: SBR_WS_ENDPOINT });
    try {
        const page = await browser.newPage();
        console.log('Connected! Navigating to example.com...');
        await page.goto('https://example.com');
        console.log('Scraping page content...');
        const html = await page.content();
        console.log(html);
    } finally {
        await browser.close();
    }
}

main().catch(err => console.error(err.stack || err));
```

> **💡 [Puppeteer を使ったWebスクレイピング](https://brightdata.jp/blog/how-tos/web-scraping-puppeteer)について詳しく見る**

### Playwright 例（Python）

```python
import asyncio
from playwright.async_api import async_playwright

SBR_WS_CDP = 'wss://brd-customer-<YOUR-USERNAME>-zone-<YOUR-ZONE-NAME>:<YOUR-PASSWORD>@brd.superproxy.io:9222'

async def run(pw):
    print('Connecting to Scraping Browser...')
    browser = await pw.chromium.connect_over_cdp(SBR_WS_CDP)
    try:
        page = await browser.new_page()
        print('Connected! Navigating to example.com...')
        await page.goto('https://example.com')
        print('Scraping page content...')
        html = await page.content()
        print(html)
    finally:
        await browser.close()

async def main():
    async with async_playwright() as playwright:
        await run(playwright)

if __name__ == '__main__':
    asyncio.run(main())
```

> **💡 [Playwright を使ったWebスクレイピング](https://brightdata.jp/blog/how-tos/playwright-web-scraping)について詳しく見る**

### Selenium 例（JavaScript）

```js
const { Builder, Browser } = require('selenium-webdriver');

const SBR_WEBDRIVER = 'https://brd-customer-<YOUR-USERNAME>-zone-<YOUR-ZONE-NAME>:<YOUR-PASSWORD>@brd.superproxy.io:9515';

async function main() {
    console.log('Connecting to Scraping Browser...');
    const driver = await new Builder().forBrowser(Browser.CHROME).usingServer(SBR_WEBDRIVER).build();
    try {
        console.log('Connected! Navigating to example.com...');
        await driver.get('https://example.com');
        console.log('Scraping page content...');
        const html = await driver.getPageSource();
        console.log(html);
    } finally {
        driver.quit();
    }
}

main().catch(err => console.error(err.stack || err));
```

> **💡 [Selenium を使ったWebスクレイピング](https://brightdata.jp/blog/how-tos/using-selenium-for-web-scraping)について詳しく見る**

## 🔥 高度な機能

### Chrome DevTools でのデバッグ

ブラウザセッションをリアルタイムで監視します。

```js
const { exec } = require('child_process');

const chromeExecutable = 'google-chrome';
const openDevtools = async (page, client) => {
    const frameId = page.mainFrame()._id;
    const { url: inspectUrl } = await client.send('Page.inspect', { frameId });
    exec(`"${chromeExecutable}" "${inspectUrl}"`, error => {
        if (error) throw new Error('Unable to open devtools: ' + error);
    });
};
```

### CAPTCHA 解決

```js
const client = await page.target().createCDPSession();
const { status } = await client.send('Captcha.solve', { detectTimeout: 30000 });
```

> **🤖 当社の [CAPTCHA Solver](https://github.com/luminati-io/Captcha-solver) について詳しく見る。**

## 🔄 自動 IP ローテーション & アンロック  
Scraping Browser は、統合された [ローテーティングプロキシ](https://brightdata.jp/solutions/rotating-proxies) によりIPを自動でローテーションし、シームレスなデータ収集のためにリトライを処理します。 

## 💰 料金  

### 柔軟なプラン  
- **従量課金（Pay-As-You-Go）:** $8.40/GB – コミットメント不要です。  
- **Growth Plan:** $7.14/GB – チームに最適です。  
- **Business Plan:** $6.30/GB – 運用のスケールに向いています。  
- **Enterprise:** 大量利用のニーズに向けたカスタム料金です。  

**今すぐ登録して、初回入金が最大 $500 まで同額マッチされます！**  

[料金を見る](https://brightdata.jp/pricing/scraping-browser)  

## ❓ よくある質問  

### Scraping Browser は標準的なヘッドレスブラウザと何が違いますか？  
Scraping Browser は、Bright Data のインフラ上で動作するフルマネージドの GUI ベースブラウザであり、最も保護されたサイトであっても自動的にアンロックします。  

### Scraping Browser はボット検知をどのように処理しますか？  
フィンガープリント、CAPTCHA 解決、リトライを自動化し、実ユーザーの挙動を模倣して検知を回避します。  

### Scraping Browser は Puppeteer、Playwright、Selenium に対応していますか？  
はい。主要なWeb自動化ツールすべてにシームレスに統合できます。  

### プロキシではなく Scraping Browser を使うべきなのはいつですか？  
JavaScript レンダリング、インタラクティブな操作（クリック、スクロール）、およびマルチステップのナビゲーションが必要な場合は Scraping Browser を使用してください。