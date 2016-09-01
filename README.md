# xcaptcha-api-reference

--
xCaptcha: 一個類似reCaptcha的人機驗證服務, 旨在去除傳統圖形驗證碼而生

## 目錄

### API

 - /xcaptcha/api.js

用於加載驗證區域的JS代碼
使用:
```javascript
$("#input-captcha").xcaptcha({
    k:              "你的appkey",
    url:            "當前頁面URL,務必保證域名部分準確性",
    sign:           "簽名, 請參見signature生成部分",
    success:        function(e){return "迴調函數, 當驗證通過時觸發"},
    fail:           function(e){return "迴調函數, 當驗證錯誤(如網絡問題)觸發"},
    fall:           function(e){return "迴調函數, 當驗證回落圖形驗證時觸發"},
    initialized:    function(e){return "迴調函數, 完成初始化時觸發"},
    trigger:        function(e){return "迴調函數, 用戶點擊嘗試校驗後立即觸發"},
    style:          {width:"100%", border:"1px solid #ccc"} // 樣式自定義. 僅可定義frame的樣式, 不可定義區域內部樣式
});
```

 - /xcaptcha/api2/siteverify
