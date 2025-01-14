---
layout: default-layout
needAutoGenerateSidebar: true
title: Dynamic Web TWAIN SDK API Reference - Uploader APIs
keywords: Dynamic Web TWAIN, Documentation, API Reference, Uploader APIs
breadcrumbText: File Uploader
description: Dynamic Web TWAIN SDK Documentation API Reference Uploader APIs Page
permalink: /info/api/Dynamsoft_FileUploader.html
---

# `Dynamsoft.FileUploader`

**Methods**

|                       |
| :-------------------- | :-------------------------------------- | --------------------------------------------------------- |
| [`Init()`](#init)     | [`CreateJob()`](#createjob)             | [`Run()`](#run)                                           |
| [`Cancel()`](#cancel) | [`CancelAllUpload()`](#cancelallupload) | [`GenerateURLForUploadData()`](#generateurlforuploaddata) |

**Properties**

|                           |
| :------------------------ | :-------------------------- | ----------------------------- |
| [`ServerUrl`](#serverurl) | [`HttpHeader`](#httpheader) | [`SourceValue`](#sourcevalue) |
| [`FormField`](#formfield) |

**Properties**

|                                                             |
| :---------------------------------------------------------- | :------------------------------ | ------------------------------- |
| [`OnUploadTransferPercentage`](#onuploadtransferpercentage) | [`OnRunSuccess`](#onrunsuccess) | [`OnRunFailure`](#onrunfailure) |

---

## Init

Initialize and create a FileUploader instance.

**Syntax**

```typescript
Init(
    URL: string,
    successCallback: (uploadManager: UploadManager) => void,
    failureCallback: (errorCode: number, errorString: string) => void
): void;
```

**Parameters**

`URL`: Specify a path to retrieve the FileUploader library.

`successCallback`: A callback function that is executed if the request succeeds.
- `uploadManager`: A FileUploader instance.

`failureCallback`: A callback function that is executed if the request fails.
- `errorCode`: The error code.
- `errorString`: The error string.

**Availability**

<div class="availability"></div>

|:-|:-|
|ActiveX|H5(Windows)|H5(macOS/TWAIN)|H5(macOS/ICA)|H5(Linux)|
| not supported | v17.2+ | v17.2+ | v17.2+ | v17.2+ |

**Usage notes**

The FileUploader library is installed with Dynamsoft Service by default, therefore, `URL` can be left empty "".

---

## CreateJob

Create an upload job.

**Syntax**

```typescript
CreateJob(): Job;

interface Job {
    /**
     * Specify the block size (in bytes). By default, it's 10240.
     */
    BlockSize: number;
    /**
     * Specify the file name.
     */
    FileName: string;
    /**
     * Specify the fields in the HTTP Post Form.
     */
    FormField: FormField;
    /**
     * Specify custom HTTP Post request headers.
     * Example: job.HttpHeader["Content-Type"] = "text/plain";
     */
    HttpHeader: object;
    /**
     * Return the Http version.
     */
    readonly HttpVersion: string;
    /**
     * A callback triggered when the job succeeds.
     * @argument job Specify the job.
     * @argument errorCode The error code.
     * @argument errorString The error string.
     */
    OnRunFailure: (
        job: Job,
        errorCode: number,
        errorString: string
    ) => void;
    /**
     * A callback triggered when the job succeeds.
     * @argument job Specify the job.
     */
    OnRunSuccess: (job: Job) => void;
    /**
     * A callback triggered multiple times during the upload.
     * @argument job Specify the job.
     * @argument percentage Return the percentage.
     */
    OnUploadTransferPercentage: (
        job: Job,
        percentage: number
    ) => void;
    /**
     * Specify the URL of the script to receive the upload.
     */
    ServerUrl: string;
    /**
     * Specify the main content of the job, i.e. the file(s).
     */
    SourceValue: SourceValue;
    /**
     * Specify the number of threads (<=4) for the upload.
     */
    ThreadNum: number;
    /**
     * Return the version of the job.
     */
    readonly Version: number;
}
interface SourceValue {
    /**
     * Specify the block size. By default, it's 10240.
     * @param source A URL to specify the content of the file.
     * Normally it's generated by {GenerateURLForUploadData()}
     * @param name Specify the name of the file.
     * @param key Specify the key of the file in the request. This key can be used to retrieve the file content in server-side scripts.
     */
    Add: (
        source: string,
        name: string,
        key ? : string
    ) => void;
}
interface FormField {
    /**
     * Specify the block size. By default, it's 10240.
     * @param key Specify the key of the field.
     * @param value Sepcify the value of the field.
     */
    Add: (
        key: string,
        value: string
    ) => void;
}
```

**Availability**

<div class="availability"></div>

|:-|:-|
|ActiveX|H5(Windows)|H5(macOS/TWAIN)|H5(macOS/ICA)|H5(Linux)|
| not supported | v17.2+ | v17.2+ | v17.2+ | v17.2+ |

---

## Run

Start uploading (processing the specified job).

**Syntax**

```typescript
Run(job: Job): boolean;
```

**Parameters**

`job`: Specify the job.

**Availability**

<div class="availability"></div>

|:-|:-|
|ActiveX|H5(Windows)|H5(macOS/TWAIN)|H5(macOS/ICA)|H5(Linux)|
| not supported | v17.2+ | v17.2+ | v17.2+ | v17.2+ |

---

## Cancel

Cancel a job.

**Syntax**

```typescript
Cancel(job: Job): boolean;
```

**Parameters**

`job`: Specify the job.

**Availability**

<div class="availability"></div>

|:-|:-|
|ActiveX|H5(Windows)|H5(macOS/TWAIN)|H5(macOS/ICA)|H5(Linux)|
| not supported | v17.2+ | v17.2+ | v17.2+ | v17.2+ |

---

## CancelAllupload

Cancel all jobs.

**Syntax**

```typescript
CancelAllUpload(): boolean;
```

**Availability**

<div class="availability"></div>

|:-|:-|
|ActiveX|H5(Windows)|H5(macOS/TWAIN)|H5(macOS/ICA)|H5(Linux)|
| not supported | v17.2+ | v17.2+ | v17.2+ | v17.2+ |

**Usage notes**

`Cancel()` or `CancleAllUpload()` should be called in the event [OnUploadTransferPercentage](#onuploadtransferpercentage).

**Example**

```javascript
var dsUploadManager;
Dynamsoft.FileUploader.Init(
  "",
  function (obj) {
    dsUploadManager = obj;
    var job = dsUploadManager.CreateJob();
    job.OnUploadTransferPercentage = FileUpload_OnUploadTransferPercentage;
    dsUploadManager.Run(job);

    function FileUpload_OnUploadTransferPercentage(job, iPercentage) {
      console.log("job cancelled!");
      dsUploadManager.Cancel(job);
    }
  },
  function () {}
);
```

## GenerateURLForUploadData

Generates a URL that will be used by the upload module to fetch the file/data to upload.

**Syntax**

```typescript
GenerateURLForUploadData(
    Indexes: Number[],
    EnumDWT_ImageType: Dynamsoft.DWT.EnumDWT_ImageType | number,
    successCallback: (resultURL: resultURL) => void,
    failureCallback: (errorCode: number, errorString: string) => void
): void;
```

**Parameters**

`Indexes`: The indices of the images in the buffer. The index is 0-based.

`EnumDWT_ImageType`: The format in which you'd like the images to be uploaded. Please refer to [EnumDWT_ImageType]({{site.info}}api/Dynamsoft_Enum.html#dynamsoftdwtenumdwt_imagetype).

`successCallback`: A callback function triggered when the operation succeeds. This function will return the result URL.
- `resultURL`: The result URL.

`failureCallback`: A callback function triggered when the operation fails.
- `errorCode`: The error code.
- `errorString`: The error string.

**Availability**

<div class="availability"></div>

|:-|:-|
|ActiveX|H5(Windows)|H5(macOS/TWAIN)|H5(macOS/ICA)|H5(Linux)|
| not supported | v17.2+ | v17.2+ | v17.2+ | v17.2+ |

**Example**

```javascript
Dynamsoft.FileUploader.Init(
  "",
  function (obj) {
    dsUploadManager = obj;
  },
  function () {}
);
DWObject.GenerateURLForUploadData(
  [0, 1],
  EnumDWT_ImageType.IT_PDF,
  function (resultURL, newIndices, enumImageType) {
    var serverUrl = "https://yoursite/yourserverurl.aspx";
    var jobtemp = dsUploadManager.CreateJob();
    jobtemp.ServerUrl = serverUrl;
    jobtemp.SourceValue.Add(resultURL, "uploadedFile.pdf");
    dsUploadManager.Run(jobtemp);
  },
  function (
    erroCode,
    errorString,
    httpResponseString,
    newIndices,
    enumImageType
  ) {}
);
```

---

## ServerUrl

Specifies the target of the HTTP Post Request of the upload job. This typically is a file on the server. For example: `job.ServerUrl = 'http://www.dynamsoft.com/ScanAndUpload/Actions/SaveToFile.aspx';`

**Syntax**

```typescript
ServerUrl: string;
```

**Availability**

<div class="availability"></div>

|:-|:-|
|ActiveX|H5(Windows)|H5(macOS/TWAIN)|H5(macOS/ICA)|H5(Linux)|
| not supported | v17.2+ | v17.2+ | v17.2+ | v17.2+ |

---

## HttpHeader

Specifies headers in the the HTTP Post Request of the upload job. For example: `job.HttpHeader["Content-Type"] = "text/plain";`

**Syntax**

```typescript
HttpHeader: object;
```

**Availability**

<div class="availability"></div>

|:-|:-|
|ActiveX|H5(Windows)|H5(macOS/TWAIN)|H5(macOS/ICA)|H5(Linux)|
| not supported | v17.2+ | v17.2+ | v17.2+ | v17.2+ |

**Usage notes**

By default, HttpHeader is an empty object. If left as it is, default headers are used. Otherwise, the headers set by this property will be added to the HTTP Post Request or replace existing ones with the same names.

---

## SourceValue

Specifies the files to be uploaded and the name for it. The files are specified by URLs which can be created with the method GenerateURLForUploadData. This object has a method Add to add file to the job.

**Syntax**

```typescript
SourceValue: object;
```

**Availability**

<div class="availability"></div>

|:-|:-|
|ActiveX|H5(Windows)|H5(macOS/TWAIN)|H5(macOS/ICA)|H5(Linux)|
| not supported | v17.2+ | v17.2+ | v17.2+ | v17.2+ |

**Usage notes**

Use the Add(string urltoFetchFileData, string fileName) method of the Object to add data for uploading.

**Example**

```javascript
job.SourceValue.Add(url, fileName);
```

---

## FormField

Specifies extra fields to be uploaded in the same HTTP post.

**Syntax**

```typescript
FormField: object;
```

**Availability**

<div class="availability"></div>

|:-|:-|
|ActiveX|H5(Windows)|H5(macOS/TWAIN)|H5(macOS/ICA)|H5(Linux)|
| not supported | v17.2+ | v17.2+ | v17.2+ | v17.2+ |

**Usage notes**

Use the Add(string fieldName, string fieldValue) method of the Object to add fields for uploading, check out the sample code for more information.

**Example**

```javascript
job.FormField.Add("customField", "FormFieldValue");
```

---

## OnUploadTransferPercentage

The event is triggered during the execution of an upload job. It has a parameter which specifies the percentage of the completion of the job.

**Syntax**

```typescript
.OnUploadTransferPercentage = function(obj: Object , sPercentage: number){};
```

**Parameters**

`obj`: A job object.

`sPercentage`: The percentage of the completion of the job.

**Availability**

<div class="availability"></div>

|:-|:-|
|ActiveX|H5(Windows)|H5(macOS/TWAIN)|H5(macOS/ICA)|H5(Linux)|
| not supported | v17.2+ | v17.2+ | v17.2+ | v17.2+ |

**Example**

```javascript
job.OnUploadTransferPercentage = FileUpload_OnUploadTransferPercentage;
function FileUpload_ OnUploadTransferPercentage (obj, sPercentage){
    console.log(sPercentage);
}
```

---

## OnRunSuccess

The event is triggered when an upload job completes successfully.

**Syntax**

```typescript
.OnRunSuccess = function(obj: Object){};
```

**Parameters**

`obj`: A job object.

**Availability**

<div class="availability"></div>

|:-|:-|
|ActiveX|H5(Windows)|H5(macOS/TWAIN)|H5(macOS/ICA)|H5(Linux)|
| not supported | v17.2+ | v17.2+ | v17.2+ | v17.2+ |

**Example**

```javascript
job.OnRunSuccess = FileUpload_OnRunSuccess;
function FileUpload_OnRunSuccess(obj) {
  alert(" upload completed ");
}
```

---

## OnRunFailure

The event is triggered when an upload job completes successfully.

**Syntax**

```typescript
.OnRunFailure = function(
    obj: Object,
    errorCode: number,
    errorString: string
){};
```

**Parameters**

`obj`: A job object.

`errorCode`: The error code.

`errorString`: The error string.

**Availability**

<div class="availability"></div>

|:-|:-|
|ActiveX|H5(Windows)|H5(macOS/TWAIN)|H5(macOS/ICA)|H5(Linux)|
| not supported | v17.2+ | v17.2+ | v17.2+ | v17.2+ |

**Example**

```javascript
job.OnRunFailure = FileUpload_OnRunFailure;
function FileUpload_OnRunFailure(obj, errorCode, errorString) {
  alert(errorString);
}
```