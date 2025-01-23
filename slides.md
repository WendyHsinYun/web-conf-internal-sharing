---
# You can also start simply with 'default'
theme: default
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
---

#  2024 WebConf 內部分享
2025/1/23 AP 張馨云

---
transition: slide-left
---

# 參加了兩天的 WebConf...

<div class="w-65% h-auto mx-auto overflow-hidden" style="border-radius: 16px; margin-top:5%">
  <img 
    src="/webconf.png" 
    class="w-full object-cover" 
    style="margin-top: -10%"
  />
</div>

---
transition: slide-left
---

# 收穫

* 從略有耳聞到實際感受 (ex: RxJS 狀態管理、Zeabur 雲端部署、Vue Vapor Mode...)
* 了解他人在工作現場遇到的實務問題、如何思考與解決 (ex: 勞保局 e 化系統、TonyQ 專案管理實務)
* 「做簡報」跟「公開演講」原來可以這麼精彩 (ex: PJ、Mosky、沅霖、AntFu...)
* 現場參與的可貴: 跟同事交流、促成討論，形成共有基底

---
class: text-center text-center flex flex-col justify-center

transition: slide-left
---

# 趁這個機會來聊聊單元測試

---
transition: slide-left
---

# 「單元測試的藝術」第 3 版（第1章）
第1版 .NET, 第2版C#

<div class="w-40% h-auto mx-auto" style="border-radius: 16px; margin-top:0%">
  <img 
    src="/unit-testing.png" 
    class="w-full object-cover" 
  />
</div>

---
transition: slide-left
---

# 什麼是手動測試

<br>
人工測試情境：

Ｂ有更動時，通常會需要也回去測試Ａ <br>
當加了Ｃ功能之後，可能要回來測試 Ａ，Ｂ<br>
通常只能測到目前正在修改的功能，無法確定是否有打破原本正常的其他功能

<v-click>
<br>
如果用程式碼？
</v-click>

---
transition: slide-left
---

# 寫測試，很浪費時間？

<br>
好處可能有：

- 釐清 code 夠不夠乾淨，有助於日後維護
- 測試很難寫，有可能代表這段程式碼寫得不好
- 提高交付 code 的信心值
- crash down 的時候，可以比較快找到哪一塊發生問題，更快 debug
- 有助於**提升資安**，因為 code 夠乾淨，漏洞會很好抓
- 自動化 CICD 需要測試

<!--
為何有助於資安：

1. 防止未經處理的輸入輸出

單元測試可驗證輸入和輸出是否符合安全要求：

    測試輸入驗證
    確保所有輸入都經過檢查和清理（防止 SQL 注入、XSS 等）。
        測試範例：輸入惡意腳本 <script>alert(1)</script>，確認輸入被攔截或過濾。
    測試輸出編碼
    確保輸出到前端的數據正確編碼，防止 XSS 攻擊。

2. 模擬邊界條件與異常情境

單元測試可驗證在極端或異常情境下的系統反應：

    測試過長的輸入或非預期類型的數據（例如，數字欄位中輸入字串）。
    模擬數據溢出、空指標或未處理的異常，確保程式不暴露敏感信息。

3. 加強認證與授權檢查

透過單元測試檢查認證與授權邏輯是否正確：

    測試授權檢查是否涵蓋所有功能，防止未授權用戶執行敏感操作。
    測試是否正確處理會話管理（例如，過期 Token 是否無法使用）。
-->

---
transition: slide-left
---

# 什麼時候可能不需要寫測試

* 拋棄式的接案類型，不用考慮長期的維護性
* 個人小專案

---
transition: slide-left
---

# 什麼是單元測試
<br>
Kent Beck 在 1970s 年代就在 SmallTalk 這個語言中開始做單元測試的工具。<br>
他也知名於 TDD、極限開發、架構設計等領域。

<div class="w-65% h-auto mx-auto overflow-hidden" style="border-radius: 16px; margin-top:5%">
  <img 
    src="/kent-beck.jpg" 
    class="w-full object-cover" 
    style="margin-top: 0%"
  />
</div>

---

# 單元測試的「單元」?

<br>
維基百科有他的定義，而作者的定義是系統中的一個 “unit of work” (工作單元) 或一個 “use case” (使用案例)

Unit of Work 是指從一個 entry point 觸發到產生 exit point 的整個操作過程。

<v-click>
<br> 
舉例：


```javascript
function calculate(x, y) { // 入口點 (entry point)
    const total = x + y;  
    return total; // 出口點 (exit point)
}
```
</v-click>

<v-click>
<br> 
可能牽涉不同 functions, modules, components, 不僅是一個 function 而已。
</v-click>

---
transition: slide-left
---

# Entry Point & Exit Point

<br>
每一個單元都會有一個從「外部」（透過 production code 或 test）觸發的 entry point，

一至多個「有用的」 exit point。

---
transition: slide-left
---

# Exit Point 的類型分三種

<div style="display: flex; width: 100%; height: auto;">
<!-- 左邊區域 -->
<div style="width: 50%; padding: 10px;">
<li> 返回值 </li>
<li> 狀態改變  </li>
<li> 呼叫第三方依賴  </li>
</div>

<!-- 右邊區域 -->
<div class="w-40% h-auto mx-auto overflow-hidden" style="border-radius: 16px;">
  <img 
    src="/exit-point.png" 
    class="w-full object-cover" 
  />
</div>
</div>

---
transition: slide-left
---

# 1. 返回值：最簡單好寫

例如：計算16進位字串並轉換為 byte


<div style="display: flex; width: 100%; height: auto;">
<!-- 左邊區域 -->
<v-click>
<div style="width: 50%; padding: 10px; ">
Production Code
```javascript
const hexStringToByte = (hexString) => {
  if (!memoryHexString) return 0;
  return memoryHexString.length / 2;
};
```
</div>
</v-click>

<!-- 右邊區域 -->
<v-click>
<div style="width: 50%; padding: 10px; box-sizing: border-box;">
Test Code

```javascript
import { describe, it, expect } from 'vitest';
import { hexStringToByte } from '@/utils/memory;

describe('hexStringToByte', () => {
  it('should return 0 for an empty string', () => {
    const result = hexStringToByte('');
    expect(result).toBe(0);
  });

  it('should calculate the correct length for a valid hex string', () => {
    const result = hexStringToByte('A1'); // 2 characters
    expect(result).toBe(1); // 2 / 2 = 1
  })
});

```
</div>
</v-click>

</div>

---
transition: slide-left
---

# 2. 狀態改變：稍微麻煩，因為 output 較間接

例如： [清除memory](https://192.168.1.188/product/modify-subproduct/9ce3e21e-50ab-4acf-9768-de688703e769)

<div style="display: flex; width: 100%; height: auto;">
<!-- 左邊區域 -->
<v-click>
<div style="width: 50%; padding: 10px; ">
Production Code
```javascript
const memoryTableData = ref([]);

const handleRemoveMemory = () => {
    memoryTableData.value = [];
};
```
</div>
</v-click>

<!-- 右邊區域 -->
<v-click>
<div style="width: 50%; padding: 10px; box-sizing: border-box;">
Test Code

```javascript
​​​​import { ref } from 'vue';
​​​​import { describe, it, expect } from 'vitest';
​​​​import { handleRemoveMemory } from '@/hooks/product.ts';

​​​​describe('handleRemoveMemory', () => {
​​​​  it('should clear memoryTableData', () => {
​​​​    // 準備初始測試資料
​​​​    const memoryTableData = ref([
​​​​      { from: 'A', to: 'B', length: 10 }
​​​​    ]);

​​​​    // 執行目標函數
​​​​    handleRemoveMemory(memoryTableData);

​​​​    // 驗證資料已被清空
​​​​    expect(memoryTableData.value).toEqual([]);
​​​​  });
```

</div>
</v-click>

</div>

---
transition: slide-left
---

# 3. 呼叫第三方依賴: 最麻煩（通常會用 mock 處理）

例如： [route change](https://192.168.1.188/product/product-list)

<div style="display: flex; width: 100%; height: auto;">
<!-- 左邊區域 -->
<v-click>
<div style="width: 50%; padding: 10px; ">
Production Code
```javascript
const handleAddProductClick = () => {
    routerPushByKey('product_add-product');
};
```
</div>
</v-click>

<!-- 右邊區域 -->
<v-click>
<div style="width: 50%; padding: 10px; box-sizing: border-box;">
Test Code

```javascript
import { vi } from 'vitest';
import { routerPushByKey } from 'router-module';
import { handleAddProductClick } from './composables.js';

vi.mock('router-module', () => ({
  routerPushByKey: vi.fn(),
}));

test('should call routerPushByKey with correct key', () => {
  handleAddProductClick();
  expect(routerPushByKey).toHaveBeenCalledWith('product_add-product');
});
```
</div>
</v-click>

</div>

---
transition: slide-left
---

# 單元測試跟整合測試的差別

只要測試需要連網、用到真實的 API、真實的系統時間、真實的檔案系統、真實的 db...就是整合測試的範疇。


<div class="w-60% h-auto mx-auto overflow-hidden" style="border-radius: 16px;">
  <img 
    src="/integration-test.png" 
    class="w-full object-cover" 
  />
</div>

<br>
想像成一台車，一個子系統壞了，車子就跑不動，當車子跑得動，就代表全部都沒事。
<br>
故障點很多，出錯時，無法第一時間抓到錯誤在哪。

---
transition: slide-left
---

# 好單元測試 checklist

1. 能不能正常運行兩週或一個月以前寫的測試？

<v-click>
<div style="margin-left: 20px; color: gray;">
如果你在破壞某件物品後的 60 秒內就知道自己破壞了它，那不是很棒嗎？
</div>
</v-click>

<br>
<v-click>
2. 其他同事也可以把之前的測試跑起來嗎？
</v-click>

<v-click>
<div style="margin-left: 20px; color: gray;">
有好的測試安全網，就可以減少 legacy code 的恐懼感
</div>
</v-click>

<br>
<v-click>
3. 我可以把我寫過的測試在幾秒內跑起來嗎？
</v-click>

<v-click>
<div style="margin-left: 20px; color: gray;">
好的單元測試應該要可以很快跑起來。如果不行，代表它可能有依賴到外部系統，且會使開發者更少跑測試，製造 bugs 未爆彈。
</div>
</v-click>

<br>
<v-click>
4. 可以一鍵就把測試跑起來嗎？
</v-click>

<v-click>
<div style="margin-left: 20px; color: gray;">
跑測試不應該要去設定額外的環境 (Docker, 資料庫)，不應該有過多人工的介入，且是自動化的。
</div>
</v-click>

---
transition: slide-left
---

5. 可否在幾分鐘內簡單快速寫完一個測試？

<v-click>
<div style="margin-left: 20px; color: gray;">
如果寫測試變得很困難，代表你可能關注在「重要的大事情」，那就會變成整合測試，但整合測試是不夠的，單元測試是用很多很小而簡單的片段，去確保邏輯的正確性。錯誤常發生在這些看似不重要的細節裡。
</div>
</v-click>

<br>
<v-click>
6. 當同事的 code 壞了，我的測試還能通過嗎？我的測試在不同的 server 能獲得同樣結果嗎？我的測試會因為沒有 db、網路或是部署到某個環境就不能跑嗎？
</v-click>

<v-click>
<div style="margin-left: 20px; color: gray;">
一樣，隔絕外部依賴。我們要對測試的 code 有完全的掌握度。那些外部依賴都可以變成我們創造的 Stub（假依賴），讓我們能夠控制輸入。
</div>
</v-click>

<br>
<v-click>
7. 如果有一個測試被移除了，那其他測試能夠維持不被影響嗎？
</v-click>

<v-click>
<div style="margin-left: 20px; color: gray;">
整合測試可能會有共享的狀態，但單元測試不會。舉例來說，整合測試的測試有可能必須按照特定順序來跑，否則就會出錯。
</div>
</v-click>

<br>
<br>
<br>
<v-click>
<div style="margin-left: 20px; color: orange;">
如果有任何一個答案是 NO，就代表這不是單元測試，而可能是整合測試。
</div>
</v-click>

---
transition: slide-left
---

# 傳統的單元測試撰寫方法

先寫好扣、「如果有時間」再寫測試

<div class="w-90% h-auto mx-auto overflow-hidden" style="border-radius: 16px; margin-top:5%">
  <img 
    src="/traditional.png" 
    class="w-full object-cover" 
    style="margin-top: 0%"
  />
</div>

---
transition: slide-left
---

# TDD 方法的單元測試

寫測試來證明某一段 code 還不存在（許願） → 跑測試，測試失敗 → 撰寫最小單位的 code → 跑測試，測試通過 → 重構測試或是 code → 跑測試，測試通過

<div class="w-40% h-auto mx-auto overflow-hidden" style="border-radius: 16px; margin-top:0%">
  <img 
    src="/tdd.png" 
    class="w-full object-cover" 
    style="margin-top: 0%"
  />
</div>

<!--
在作者的觀點中，用 TDD 還有一個好處，就是「測試你的測試」。例如當我們預期測試應該失敗卻通過時，代表你的測試寫的有問題。還有，當你測試修好了，你以為會通過結果沒有，可能也代表測試有問題。所以可以在反覆看到測試通過或失敗之間，把測試寫的更好。該成功時就會成功，該失敗就會失敗。而如果實踐 TDD，通常花在 debug 的時間也會變少。
-->

---
transition: slide-left
---

# 如何撰寫具彈性的測試程式 | 2024 WebConf Taiwan

[簡報](https://hackmd.io/@webconf/BJ2N6ksMke/%2FmIdCScKdTgSQuwjNcyrXTg)
- 段落一：怎麼寫好維護的測試
   - 選取元素的方式太過鬆散、嚴謹或意義不夠明確
   - 過多的模擬
   - 好好拆分元件，舉例
      - 將商業邏輯與資料狀態封裝在 custom hook。
      - 將資料運算放在 utils。
      - 元件只留下畫面顯示用的部份。

- 段落二：設計前端測試架構

---
transition: slide-left
---

# 困難點

ref: <a href="https://teddy-chen-tw.blogspot.com/2013/01/blog-post_16.html" target="_blank">
搞笑談軟工：需求變來變去的情況下需要寫單元測試嗎？
</a>

<div class="w-40% h-auto mx-auto overflow-hidden" style="border-radius: 16px; margin-top:0%">
  <img 
    src="/teddy.png" 
    class="w-full object-cover" 
    style="margin-top: 0%"
  />
</div>

---
transition: slide-left
---

# 2025 新年新期待

- 持續學習
- TDD ?