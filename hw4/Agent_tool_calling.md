# Tool Calling 作業示範：查詢 Ubuntu PackageKit 用途與 CVE 漏洞

## 任務目標

讓 Agent 自動使用工具完成以下任務：

> 查詢 Ubuntu 上 `packagekit` 套件的用途，以及是否存在已知 CVE 安全漏洞。

---

# Agent 執行完整紀錄

## 1. 任務分析

```text
[2026/5/7 下午10:51] Adam_agent:
[ANALYSIS]
此問題需要查詢 Ubuntu 上 packagekit 套件的用途，
並檢索其已知的 CVE 漏洞資訊。

我將先透過 apt show 指令了解套件功能，
然後使用 web_search 查詢相關的 CVE 報告。

[SOLUTION]
首先查詢 packagekit 套件的詳細資訊。
```

---

## 2. 呼叫 Terminal Tool

Agent 自動執行 Linux 指令：

```bash
apt show packagekit
```

工具呼叫紀錄：

```text
[2026/5/7 下午10:51] Adam_agent:
💻 terminal: "apt show packagekit"
```

---

## 3. Tool 呼叫後回傳空結果

```text
[2026/5/7 下午10:52] Adam_agent:
⚠️ Model returned empty after tool calls — nudging to continue
```

Agent 發現 `web_search` 不存在，因此自動調整策略：

```text
[2026/5/7 下午10:52] Adam_agent:
[ANALYSIS]
我嘗試使用 web_search 工具，
但它並不在我的可用工具集中。

我應該使用 browser_navigate 和 browser_snapshot
來執行網路搜尋。

我將修正這個問題，
並使用 browser 工具來查詢 PackageKit 的 CVE 漏洞。

[SOLUTION]
我將使用 browser_navigate 和 browser_snapshot
來搜尋 PackageKit 的 CVE 漏洞。

首先，我會導航到一個常用的 CVE 搜尋網站，
例如 NVD (National Vulnerability Database)。
```

---

## 4. Browser Tool 開始搜尋

```text
[2026/5/7 下午10:52] Adam_agent:
🌐 browser_navigate:
"https://nvd.nist.gov/vuln/search"
```

---

## 5. 網站超時

```text
[2026/5/7 下午10:53] Adam_agent:
[ANALYSIS]
browser_navigate 指令超時了，
可能是網路問題或網站載入緩慢。

我會再次嘗試導航到 NVD 搜尋頁面。

[SOLUTION]
重新嘗試導航到 NVD 網站。
```

再次執行：

```text
[2026/5/7 下午10:53] Adam_agent:
🌐 browser_navigate:
"https://nvd.nist.gov/vuln/search"
```

---

## 6. Agent 改變搜尋策略

```text
[2026/5/7 下午10:54] Adam_agent:
🌐 browser_navigate:
"https://www.google.com"
```

```text
[2026/5/7 下午10:54] Adam_agent:
[ANALYSIS]
browser_navigate 指令連續兩次超時，

這表示直接導航到 NVD 網站可能存在問題
（例如網路或網站本身）。

我需要改變策略。

我將使用一個通用的搜尋引擎來查詢
「PackageKit CVE」，

然後從搜尋結果中選擇相關連結，
而不是直接導航到 NVD。

[SOLUTION]
我將導航到 Google 搜尋頁面，
然後搜尋「PackageKit CVE vulnerabilities」。
```

---

## 7. 自動輸入搜尋內容

```text
[2026/5/7 下午10:54] Adam_agent:
[ANALYSIS]
我已成功導航到 Google 搜尋頁面。

現在，我需要找到搜尋框並輸入：
「PackageKit CVE vulnerabilities」

根據提供的快照，
搜尋框 ref 是 e13，
搜尋按鈕 ref 是 e16。

[SOLUTION]
在搜尋框 e13 中輸入：
「PackageKit CVE vulnerabilities」。
```

Agent 自動輸入：

```text
[2026/5/7 下午10:54] Adam_agent:
⌨️ browser_type:
"PackageKit CVE vulnerabilities"
```

---

## 8. 自動點擊搜尋按鈕

```text
[2026/5/7 下午10:54] Adam_agent:
👆 browser_click: "e16"
```

```text
[2026/5/7 下午10:54] Adam_agent:
[ANALYSIS]
我已經點擊了搜尋按鈕。

現在我應該會看到搜尋結果頁面。

我需要取得頁面快照來查看結果，
並尋找相關的 CVE 資訊。

[SOLUTION]
取得搜尋結果頁面的快照。
```

---

## 9. 擷取搜尋結果頁面

```text
[2026/5/7 下午10:54] Adam_agent:
📸 browser_snapshot...
```

```text
[2026/5/7 下午10:54] Adam_agent:
⏳ Still working...
(3 min elapsed — iteration 10/90,
receiving stream response)
```

再次擷取：

```text
[2026/5/7 下午10:54] Adam_agent:
📸 browser_snapshot...
```

---

## 10. 遇到 reCAPTCHA

```text
[2026/5/7 下午10:54] Adam_agent:
[ANALYSIS]
目前的快照只顯示了一個連結
「為什麼會發生這種情況？」

這不是我期望的搜尋結果。

這可能表示頁面尚未完全載入，
或者我點擊的按鈕觸發了不同的行為。

我將嘗試重新取得完整頁面快照。
```

接著：

```text
[2026/5/7 下午10:54] Adam_agent:
[ANALYSIS]
看來我被 Google 的 reCAPTCHA 阻擋了。

由於我無法直接與 reCAPTCHA 互動，
因此無法繼續透過 Google 搜尋。

我將改用已知 CVE 網站，
例如 MITRE CVE Search。
```

---

## 11. 直接導航 MITRE CVE 網站

```text
[2026/5/7 下午10:54] Adam_agent:
🌐 browser_navigate:
"https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=PackageKit"
```

---

## 12. 成功取得 CVE 結果

```text
[2026/5/7 下午10:55] Adam_agent:
[ANALYSIS]
我已成功導航到 MITRE 的 CVE 搜尋頁面。

搜尋結果顯示：
共有 9 個與 PackageKit 相關的 CVE 漏洞。

頁面中已列出：
- CVE ID
- CNA
- 漏洞描述
```

---

# 最終分析結果

## PackageKit 用途

根據 `apt show packagekit`：

> PackageKit 是 Linux/Ubuntu 上的套件管理 D-Bus 抽象層，
> 用來提供跨發行版、跨架構的統一套件管理 API。

用途包括：

- 安裝套件
- 更新軟體
- 管理 repositories
- 提供 GUI 軟體中心後端
- 系統套件管理整合

---

# 已知 CVE 漏洞

| CVE | 說明 |
|---|---|
| CVE-2026-41651 | PackageKit D-Bus 安全漏洞 |
| CVE-2024-0217 | use-after-free 記憶體漏洞 |
| CVE-2022-0987 | 本地檔案資訊洩漏 |
| CVE-2020-16122 | local deb 被錯誤信任 |
| CVE-2020-16121 | 詳細錯誤訊息資訊洩漏 |
| CVE-2018-1106 | 權限繞過漏洞 |
| CVE-2013-1764 | 套件降級漏洞 |
| CVE-2011-2515 | 未簽章 RPM 被視為可信任 |
| CVE-2010-4013 | Apple PackageKit 格式字串漏洞 |

---

# Security Note

PackageKit 屬於系統級套件管理服務。

若漏洞被利用，可能造成：

- 權限提升
- 任意程式碼執行
- 系統被植入惡意套件
- 資訊洩漏
- 套件信任繞過

---

# 建議緩解措施

1. 定期更新 Ubuntu 系統
2. 維持 PackageKit 最新版本
3. 限制 D-Bus 權限
4. 監控 package management logs
5. 避免安裝不可信來源套件

---

# Tool Calling 成功證明

本案例中 Agent 自動使用了以下工具：

| Tool | 用途 |
|---|---|
| terminal | 查詢 apt 套件資訊 |
| browser_navigate | 開啟 CVE 網站 |
| browser_type | 輸入搜尋關鍵字 |
| browser_click | 點擊搜尋 |
| browser_snapshot | 擷取搜尋結果 |

---

# 作業成功標準

✅ Bot 自動選擇工具  
✅ Bot 自動切換搜尋策略  
✅ Bot 自動處理錯誤  
✅ Bot 回傳實際查詢結果  
✅ Bot 非單純文字說明  
✅ 完整展示 Tool Calling 流程