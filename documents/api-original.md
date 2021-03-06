﻿# API

- [Guide](guide-original.md)
<!-- TOC -->

- [API](#api)
    - [`object` dynamsoft.dbrEnv](#object-dynamsoftdbrenv)
    - [`function` dynamsoft.BarcodeReader.loadWasm()](#function-dynamsoftbarcodereaderloadwasm)
    - [`constructor` dynamsoft.BarcodeReader()](#constructor-dynamsoftbarcodereader)
    - [`function` .deleteInstance()](#function-deleteinstance)
    - [`function` .decodeFileInMemory()](#function-decodefileinmemory)
    - [`function` .decodeVideo()](#function-decodevideo)
    - [`function` .decodeBase64String()](#function-decodebase64string)
    - [`function` .decodeBuffer()](#function-decodebuffer)
    - [`function` .getRuntimeSettings()](#function-getruntimesettings)
    - [`function` .updateRuntimeSettings()](#function-updateruntimesettings)
    - [`function` .resetRuntimeSettings()](#function-resetruntimesettings)
    - [`function` .getAllLocalizationResults()](#function-getalllocalizationresults)
    - [`Error` dynamsoft.BarcodeReader.BarcodeReaderException](#error-dynamsoftbarcodereaderbarcodereaderexception)
    - [`enum` dynamsoft.BarcodeReader.EnumImagePixelFormat](#enum-dynamsoftbarcodereaderenumimagepixelformat)
    - [`enum` dynamsoft.BarcodeReader.EnumBarcodeFormat](#enum-dynamsoftbarcodereaderenumbarcodeformat)
    - [`enum` dynamsoft.BarcodeReader.EnumErrorCode](#enum-dynamsoftbarcodereaderenumerrorcode)
    - [`enum` dynamsoft.BarcodeReader.EnumResultType](#enum-dynamsoftbarcodereaderenumresulttype)
    - [`enum` dynamsoft.BarcodeReader.EnumTerminateStage](#enum-dynamsoftbarcodereaderenumterminatestage)

<!-- /TOC -->

<br /><br />

---
## `object` dynamsoft.dbrEnv

*example:*
```js
// All the settings are optional, even dynamsoft and dynamsoft.dbrEnv.
dynamsoft = self.dynamsoft || {};
dynamsoft.dbrEnv = dynamsoft.dbrEnv || {};
// https://www.dynamsoft.com/CustomerPortal/Portal/TrialLicense.aspx
dynamsoft.dbrEnv.licenseKey = "<a license key>",
// The default value is true. It will load the wasm files automatically.
// If you want to load the file manually, please set it to false
// and call dynamsoft.BarcodeReader.loadWasm when needed.
dynamsoft.dbrEnv.bAutoLoadWasm = true;
// The default value is false. Set it true to decode in another thread. By this way, UI would not stuck.
dynamsoft.dbrEnv.bUseWorker = false;
// By default, js will load `dbr-<version>.min.js` & `dbr-<version>.wasm` in the same folder as the context.
// `dbr-<version>.min.js` & `dbr-<version>.wasm` should always put in same folder.
// Modify this setting when you put `dbr-<version>.min.js` & `dbr-<version>.wasm` somewhere else.
// e.g. Set this as 'js' when you place `dbr-<version>.min.js` & `dbr-<version>.wasm`` at 'js/'.
dynamsoft.dbrEnv.resourcesPath = 'js';
dynamsoft.dbrEnv.onAutoLoadWasmSuccess = function(){
    console.log("success");
};
dynamsoft.dbrEnv.onAutoLoadWasmError = function(status){
    console.log("error");
};
```

<br /><br />

---
## `function` dynamsoft.BarcodeReader.loadWasm()

*Syntax:* `dynamsoft.BarcodeReader.loadWasm()`

Only need to call this manually when you set `dynamsoft.dbrEnv.bAutoLoadWasm` as `false`. 

You can use `decodeXXX`(no worker) in main thread after `loadWasm` success.

| parameter | type | Description |
| --- | --- | --- | 
| *(Return value)* | `Promise(resolve(), reject(ex))` |

<br /><br />

---
## `constructor` dynamsoft.BarcodeReader()

*Syntax:* `new dynamsoft.BarcodeReader( [licenceKeys] )`

New an instance of BarcodeReader.
Don't forget to delete the instance using `deleteInstance()` when it will not be used again.

| parameter | type | Description |
| --- | --- | --- |
| *(Return value)* | `dynamsoft.BarcodeReader` |
| licenceKeys *(optional)* | `String` | If not set, the default value is `dynamsoft.dbrEnv.licenseKey`. |

*example:*
```js
var reader = new dynamsoft.BarcodeReader();
```

<br /><br />

---
## `function` .deleteInstance()

`BarcodeReader` needs to be released when it will not be used again.

*example:*
```js
reader.deleteInstance();
```

<br /><br />

---
## `function` .decodeFileInMemory()

*Syntax:* `.decodeFileInMemory(source)`

| parameter | type | Description |
| --- | --- | --- | 
| *(Return value)* | `Promise(resolve(array), reject(ex))` | The array element is like [C# TextResult](https://www.dynamsoft.com/help/Barcode-Reader/api%20reference%20document/class_dynamsoft_1_1_barcode_1_1_text_result.html). |
| source | `Blob` `ArrayBuffer` `Uint8Array` `Uint8ClampedArray` `HTMLImageElement` `HTMLCanvasElement` `HTMLVideoElement` `String`*(base64 with mime)* `String`*(url)* | Support formats that can be rendered in `HTMLImageElement` of browser. |

*example:*
```js
reader.decodeFileInMemory('./imgs/example.png').then(results=>{
    for(var i = 0; i < results.length; ++i){
        console.log(results[i].BarcodeText);
        // If Confidence >= 30, the barcode results are reliable
        console.log(results[i].LocalizationResult.ExtendedResultArray[0].Confidence);
    }
})
```

<br /><br />

---
## `function` .decodeVideo()

A useful function when you want to decode video. It uses built in [drawImage](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/drawImage) before decoding.

*Syntax:* `.decodeVideo(video)`

| parameter | type | Description |
| --- | --- | --- | 
| *(Return value)* | `Promise(resolve(array), reject(ex))` | The array element is like [C# TextResult](https://www.dynamsoft.com/help/Barcode-Reader/api%20reference%20document/class_dynamsoft_1_1_barcode_1_1_text_result.html). |
| video | `HTMLVideoElement` | |

*example:*
```js
reader.decodeVideo(video).then(results=>{
    for(var i = 0; i < results.length; ++i){
        console.log(results[i].BarcodeText);
        // If Confidence >= 30, the barcode results are reliable
        console.log(results[i].LocalizationResult.ExtendedResultArray[0].Confidence);
    }
})
```

<br />

*Syntax:* `.decodeVideo(video, dWidth, dHeight)`

*(`dWidth`, `dHeight` also see: [drawImage](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/drawImage))*

| parameter | type | Description |
| --- | --- | --- | 
| *(Return value)* | `Promise(resolve(array), reject(ex))` | The array element is like [C# TextResult](https://www.dynamsoft.com/help/Barcode-Reader/api%20reference%20document/class_dynamsoft_1_1_barcode_1_1_text_result.html). |
| video | `HTMLVideoElement` |  |
| dWidth | `number` |  |
| dHeight | `number` |  |

*example:*
```js
// resize the video if the size is bigger than 512x512 to speed up decoding
// the success rate may decrease because of the smaller size
reader.decodeVideo(video, Math.min(video.videoWidth, 512), Math.min(video.videoHeight, 512)).then(results=>{
    for(var i = 0; i < results.length; ++i){
        console.log(results[i].BarcodeText);
        // If Confidence >= 30, the barcode results are reliable
        console.log(results[i].LocalizationResult.ExtendedResultArray[0].Confidence);
    }
})
```

<br />

*Syntax:* `.decodeVideo(video, sx, sy, sWidth, sHeight, dWidth, dHeight)`

*(`sx`, `sy`, `sWidth`, `sHeight`, `dWidth`, `dHeight` also see: [drawImage](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/drawImage))*

| parameter | type | Description |
| --- | --- | --- | 
| *(Return value)* | `Promise(resolve(array), reject(ex))` | The array element is like [C# TextResult](https://www.dynamsoft.com/help/Barcode-Reader/api%20reference%20document/class_dynamsoft_1_1_barcode_1_1_text_result.html). |
| video | `HTMLVideoElement` |  |
| sx | `number` |  |
| sy | `number` |  |
| sWidth | `number` |  |
| sHeight | `number` |  |
| dWidth | `number` |  |
| dHeight | `number` |  |

*example:*
```js
// decode a region in video to speed up decoding
var vw = video.videoWidth;
var vh = video.videoHeight;
var vw_2 = Math.round(vw * 0.2);
var vh_2 = Math.round(vh * 0.2);
var vw_6 = Math.round(vw * 0.6);
var vh_6 = Math.round(vh * 0.6);
reader.decodeVideo(video, vw_2, vh_2, vw_6, vh_6, vw_6, vh_6).then(results=>{
    for(var i = 0; i < results.length; ++i){
        console.log(results[i].BarcodeText);
    }
})
```

<br /><br />

---
## `function` .decodeBase64String()

*Syntax:* `.decodeBase64String(base64Str)`

| parameter | type | Description |
| --- | --- | --- | 
| *(Return value)* | `Promise(resolve(array), reject(ex))` | The array element is like [C# TextResult](https://www.dynamsoft.com/help/Barcode-Reader/api%20reference%20document/class_dynamsoft_1_1_barcode_1_1_text_result.html). |
| base64Str | `String`*(base64 with or without mime)* |  |

*example:*
```js
var base64str = 'data:image/png;base64,xxxxxxx';
//with mime
reader.decodeBase64String(base64str).then(results=>{
    for(var i = 0; i < results.length; ++i){
        console.log(results[i].BarcodeText);
    }
});
//without mime
reader.decodeBase64String(base64str.substring(base64str.indexOf(',') + 1)).then(results=>{
    for(var i = 0; i < results.length; ++i){
        console.log(results[i].BarcodeText);
    }
});
```

<br /><br />

---
## `function` .decodeBuffer()

*Syntax:* `.decodeBuffer(source, width, height, stride, enumImagePixelFormat)`

| parameter | type | Description |
| --- | --- | --- | 
| *(Return value)* | `Promise(resolve(array), reject(ex))` | The array element is like [C# TextResult](https://www.dynamsoft.com/help/Barcode-Reader/api%20reference%20document/class_dynamsoft_1_1_barcode_1_1_text_result.html). |
| source | `Blob` `ArrayBuffer` `Uint8Array` `Uint8ClampedArray` | The image raw buffer. |
| width | `number` | The width of the image buffer. |
| height | `number` | The height of the image buffer. |
| stride | `number` | The stride width (also called scan width) of the image buffer. |
| enumImagePixelFormat | `number`*([dynamsoft.BarcodeReader.EnumImagePixelFormat](#enum-dynamsoftbarcodereaderenumimagepixelformat))* | The pixel format of the image buffer. |

*example:*
```js
var rawImgData = new Blob(['xxxxxxx']);
var width = 100;
var height = 200;
reader.decodeBuffer(rawImgData, width, height, width * 4, dynamsoft.BarcodeReader.EnumImagePixelFormat.IPF_ARGB_8888).then(results=>{
    for(var i = 0; i < results.length; ++i){
        console.log(results[i].BarcodeText);
    }
})
```

<br /><br />

---
## `function` .getRuntimeSettings()

*Syntax:* `(BarcodeReader).getRuntimeSettings()`

*reference:* 
* [`function` .updateRuntimeSettings()](#function-updateruntimesettings)
* [`function` .resetRuntimeSettings()](#function-resetruntimesettings)

| parameter | type | Description |
| --- | --- | --- | 
| *(Return value)* | `PlainObject` | *reference: [PublicRuntimeSettings](https://www.dynamsoft.com/help/Barcode-Reader/api%20reference%20document/struct_dynamsoft_1_1_barcode_1_1_public_runtime_settings.html)* |

<br /><br />

---
## `function` .updateRuntimeSettings()

*Syntax:* `(ThreadBarcodeReader).updateRuntimeSettings(settings)`

*reference:* 
* [`function` .getRuntimeSettings()](#function-getruntimesettings)
* [`function` .resetRuntimeSettings()](#function-resetruntimesettings)

| parameter | type | Description |
| --- | --- | --- | 
| *(Return value)* | `Promise(resolve(), reject(ex))` | |
| settings | `String`*(json)* `PlainObject` | |

*example:*
```js
//get the settings
var settings = reader.getRuntimeSettings();
//modify it
settings.mExpectedBarcodesCount = 3;
//update the settings
reader.updateRuntimeSettings(settings).then(()=>{
    return reader.decodeFileInMemory('./imgs/example.png');
}).then(results=>{
    for(var i = 0; i < results.length; ++i){
        console.log(results[i].BarcodeText);
        // If Confidence >= 30, the barcode results are reliable
        console.log(results[i].LocalizationResult.ExtendedResultArray[0].Confidence);
    }
    reader.resetRuntimeSettings();
});
```

<br /><br />

---
## `function` .resetRuntimeSettings()

*Syntax:* `(BarcodeReader).resetRuntimeSettings()`

*reference:* 
* [`function` .getRuntimeSettings()](#function-getruntimesettings)
* [`function` .updateRuntimeSettings()](#function-updateruntimesettings)

| parameter | type | Description |
| --- | --- | --- | 
| *(Return value)* | `undefined` |  |

<br /><br />

---
## `function` .getAllLocalizationResults()

*Syntax:* `.getAllLocalizationResults()`

| parameter | type | Description |
| --- | --- | --- | 
| *(Return value)* | `array` | The array element is like [C# LocalizationResult](https://www.dynamsoft.com/help/Barcode-Reader/API/DotNet/classLocalizationResult.html). |

*example:*
```js
reader.decodeFileInMemory('./imgs/example.png').then(results=>{
    console.log(reader.getAllLocalizationResults());
})
```

<br /><br />

---
## `Error` dynamsoft.BarcodeReader.BarcodeReaderException

| feild | type | Description |
| --- | --- | --- | 
| code | `number`*([dynamsoft.BarcodeReader.EnumErrorCode](#enum-dynamsoftbarcodereaderenumerrorcode))* |  |
| message | `String` |  |

<br /><br />

---
## `enum` dynamsoft.BarcodeReader.EnumImagePixelFormat

*source code:*
```js
f.IPF_Binary = 0;
f.IPF_BinaryInverted = 1;
f.IPF_GrayScaled = 2;
f.IPF_NV21 = 3;
f.IPF_RGB_565 = 4;
f.IPF_RGB_555 = 5;
f.IPF_RGB_888 = 6;
f.IPF_ARGB_8888 = 7;
```

<br /><br />

---
## `enum` dynamsoft.BarcodeReader.EnumBarcodeFormat

*source code:*
```js
f.All = 0x1e0003ff;
f.OneD = 0x3FF;
f.CODE_39 = 0x1;
f.CODE_128 = 0x2;
f.CODE_93 = 0x4;
f.CODABAR = 0x8;
f.ITF = 0x10;
f.EAN_13 = 0x20;
f.EAN_8 = 0x40;
f.UPC_A = 0x80;
f.UPC_E = 0x100;
f.INDUSTRIAL_25 = 0x200;
f.PDF417 = 0x2000000;
f.QR_CODE = 0x4000000;
f.DATAMATRIX = 0x8000000;
f.AZTEC = 0x10000000;
```

*example:*
```js
var settings = reader.getRuntimeSettings();
var EnumBarcodeFormat = dynamsoft.BarcodeReader.EnumBarcodeFormat;
settings.mBarcodeFormatIds = EnumBarcodeFormat.OneD | EnumBarcodeFormat.QR_CODE;
reader.updateRuntimeSettings(settings);
```

<br /><br />

---
## `enum` dynamsoft.BarcodeReader.EnumErrorCode

*source code:*
```js
e.DBR_SYSTEM_EXCEPTION = 1;
e.DBR_SUCCESS = 0;
e.DBR_UNKNOWN = -10000;
e.DBR_NO_MEMORY = -10001;
e.DBR_NULL_REFERENCE = -10002;
e.DBR_LICENSE_INVALID = -10003;
e.DBR_LICENSE_EXPIRED = -10004;
e.DBR_FILE_NOT_FOUND = -10005;
e.DBR_FILETYPE_NOT_SUPPORTED = -10006;
e.DBR_BPP_NOT_SUPPORTED = -10007;
e.DBR_INDEX_INVALID = -10008;
e.DBR_BARCODE_FORMAT_INVALID = -10009;
e.DBR_CUSTOM_REGION_INVALID = -10010;
e.DBR_MAX_BARCODE_NUMBER_INVALID = -10011;
e.DBR_IMAGE_READ_FAILED = -10012;
e.DBR_TIFF_READ_FAILED = -10013;
e.DBR_QR_LICENSE_INVALID = -10016;
e.DBR_1D_LICENSE_INVALID = -10017;
e.DBR_DIB_BUFFER_INVALID = -10018;
e.DBR_PDF417_LICENSE_INVALID = 10019;
e.DBR_DATAMATRIX_LICENSE_INVALID = -10020;
e.DBR_PDF_READ_FAILED = -10021;
e.DBR_PDF_DLL_MISSING = -10022;
e.DBR_PAGE_NUMBER_INVALID = -10023;
e.DBR_CUSTOM_SIZE_INVALID = -10024;
e.DBR_CUSTOM_MODULESIZE_INVALID = -10025;
e.DBR_RECOGNITION_TIMEOUT = -10026;
e.DBR_JSON_PARSE_FAILED = -10030;
e.DBR_JSON_TYPE_INVALID = -10031;
e.DBR_JSON_KEY_INVALID = -10032;
e.DBR_JSON_VALUE_INVALID = -10033;
e.DBR_JSON_NAME_KEY_MISSING = -10034;
e.DBR_JSON_NAME_VALUE_DUPLICATED = -10035;
e.DBR_TEMPLATE_NAME_INVALID = -10036;
e.DBR_JSON_NAME_REFERENCE_INVALID = -10037;
e.DBR_PARAMETER_VALUE_INVALID = 10038;
e.DBR_DOMAIN_NOT_MATCHED = -10039;
e.DBR_RESERVEDINFO_NOT_MATCHED = -10040;
e.DBR_DBRERR_AZTEC_LICENSE_INVALID = -10041;
```

<br /><br />

---
## `enum` dynamsoft.BarcodeReader.EnumResultType

*source code:*
```js
t.EDT_StandardText = 0;
t.EDT_RawText = 1;
t.EDT_CandidateText = 2;
t.EDT_PartialText = 3;
```

<br /><br />

---
## `enum` dynamsoft.BarcodeReader.EnumTerminateStage

*source code:*
```js
t.ETS_Prelocalized = 0;
t.ETS_Localized = 1;
t.ETS_Recognized = 2;
```
