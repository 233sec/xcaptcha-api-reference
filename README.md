# xcaptcha-api-reference

--
xCaptcha: 一個類似reCaptcha的人機驗證服務, 旨在去除傳統圖形驗證碼而生

## 目錄

### API

#### /xcaptcha/api.js

用於加載驗證區域的JS代碼
使用:
```html
<!-- 本服務依賴於jQuery 1.8+, 另Zepto未測試, 請注意儘量使用jQuery -->
<form>
<input type="hidden" id="input-captcha" name="challenge-response">
</form>
<script type="text/javascript" src="//www.233sec.com/xcaptcha/api.js"></script>
<script>
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
</script>
```

 #### /xcaptcha/api2/siteverify
 
 用於站長業務方驗證用戶傳入的challenge response是否合法的API接口
 使用:
 ```php
<?php
function verify(){
    $appkey     = '';
    $appsecret  = '';
    $challenge_response = $_POST['challenge-response'];

    $param = [
        'k'         => $appkey,
        'response'  => $$challenge_response,
        'remoteip'  => $_SERVER['remote_addr']    // 如用CDN之類, 請使用七層方法獲取訪客IP. 若條件限制也可不傳
    ];
    ksort($param);
    $param['sign']  = md5( $appkey.'|'. http_build_query($param) . '|'.$appsecret);

    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, 'https://www.233sec.com/xcaptcha/api2/siteverify?'.http_build_query($param) );
    curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, true);
    curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, 2);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1 );
    curl_setopt($ch, CURLOPT_HEADER, 0 );
    $result = curl_exec( $ch );

    $result = json_decode($result);
    if(json_last_error() !== JSON_ERROR_NONE){
        // JSON解析出錯 邏輯處理
        return;
    }

    if ($result->head->statusCode !== 0) {
        // 驗證失敗, $result->head->note是原因
        return;
    }

    // 驗證通過
}
```
