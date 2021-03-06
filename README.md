# Dynamsoft JavaScript Barcode SDK

Version 6.4.1.1

The repository aims to help developers get familiar with [Dynamsoft JavaScript Barcode SDK](https://www.dynamsoft.com/Products/barcode-recognition-javascript.aspx).

This SDK supports decoding **1D**, **PDF417**, **QR**, **DataMatrix**, and **Aztec**.

The supported data sources include `Blob`, `HTMLImageElement`, `HTMLVideoElement`, URL and more.

The `LocalizationResult`(like [C# LocalizationResult](https://www.dynamsoft.com/help/Barcode-Reader/class_dynamsoft_1_1_barcode_1_1_localization_result.html)) can be found in decoding results.

If you want the `nodejs` version, please visit [dbr webassembly](https://github.com/dynamsoft-dbr/webassembly).

## Online Demo

<img src="https://github.com/dynamsoft-dbr/javascript-barcode/raw/master/img/dbr-wasm-demo-scaning.jpg">

[Online Demo in Github (6.4.1.1)](https://htmlpreview.github.io/?https://github.com/dynamsoft-dbr/javascript-barcode/blob/master/examples/decodeVideoWithSettings/barcode_reader_javascript.html)

[Online Demo in Dynamsoft (6.4.1.1)](https://demo.dynamsoft.com/dbr_wasm/barcode_reader_javascript.html)

## Browser compatibility

Firefox preforms best in both desktop and mobile.

| browser | min version |
|-|-|
| Chrome | 57 |
| Firefox | 52 |
| Edge | 16 |
| Safari* | 11 |
| ie | not support |

The Webassembly compiles really slow in Safari for IOS accroding to our tests.

## Documentation

[Guide (6.4.1.1)](https://github.com/dynamsoft-dbr/javascript-barcode/blob/master/documents/guide-original.md)

[API Reference (6.4.1.1)](https://github.com/dynamsoft-dbr/javascript-barcode/blob/master/documents/api-original.md)

## Helloworld

Copy the following code into an HTML file and run it in a web browser.

```html
<!DOCTYPE html>
<html>
<body>
    <div id="divLoadInfo">loading...</div>
    <input id="uploadImage" type="file" accept="image/bmp,image/jpeg,image/png,image/gif" style="display:none">
    <script src="https://demo.dynamsoft.com/dbr_wasm/js/dbr-6.4.1.1.min.js"></script>
    <script>
        dynamsoft.dbrEnv.resourcesPath = 'https://demo.dynamsoft.com/dbr_wasm/js';
        var reader = null;
        var iptEl = document.getElementById('uploadImage');
        dynamsoft.dbrEnv.onAutoLoadWasmSuccess = function(){
            reader = new dynamsoft.BarcodeReader();
            iptEl.style.display = '';
            document.getElementById('divLoadInfo').innerHTML="load dbr wasm success.";
        };
        dynamsoft.dbrEnv.onAutoLoadWasmError = function(ex){
            document.getElementById('divLoadInfo').innerHTML="load wasm failed: "+(ex.message || ex);
        };
        
        //https://www.dynamsoft.com/CustomerPortal/Portal/TrialLicense.aspx
        dynamsoft.dbrEnv.licenseKey = "t0068MgAAAITeFdSNvIYpkFMgjUw9+ssQhJwCsd78AhMIVO6NOdYfu1TQcDLwJvtO7y5bgYrZZXrq11jkf5UVL5Y5CVpb9nU=";
        
        iptEl.addEventListener('change', function(){
            reader.decodeFileInMemory(this.files[0]).then(function(results){
                var txts = [];
                for(var i=0;i<results.length;++i){
                    txts.push(results[i].BarcodeText);
                }
                alert(txts.join("\n"));
            }).catch(ex => {
                alert('error:' + (ex.message || ex));
            });
            this.value = '';
        });
    </script>
</body>
</html>
```

## Changelog

### 6.4.1.1

Fixed a memory leak related to `mTimeout` in `RuntimeSettings`.

### 6.4.1.0

Build Dynamsoft Barcode Reader 6.4.1 to JS(webassembly) version.

Combine the normal and the mobile version into one.

### 6.3.0.2

Add built-in worker support.

```js
// The default value is false. Set it true to decode in another thread. By this way, UI would not stuck.
dynamsoft.dbrEnv.bUseWorker = true;
```

### 6.3.0.1

Set `dbr-<version>.js`(stable) as the main branch.

Add `dbr-<version>.mobile.js`(smaller, compile quicker, need less memory, but not that stable) for the mobile safari.

### 6.3.0

Build Dynamsoft Barcode Reader 6.3.0 to JS(webassembly) version.

## Contact Us

If there is any questions, please feel free to contact <support@dynamsoft.com>.
