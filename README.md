<img class="img-fluid" align="center" src="https://raw.githubusercontent.com/ExtrieveTechnologies/QuickCapture/main/img/QuickCapture.png" width="30%" alt="img-verification"><img align="right" class="img-fluid" padding="10px" src="https://raw.githubusercontent.com/ExtrieveTechnologies/QuickCapture/main/img/android.png" alt="img-verification">
<!-- <a align="center" href='https://play.google.com/store/apps/details?id=com.extrieve.exScan&pcampaignid=pcampaignidMKT-Other-global-all-co-prtnr-py-PartBadge-Mar2515-1' title="Click to download android app" target="_blank" rel="noopener noreferrer"><img align="center" width="150px" alt='Get it on Google Play' src='https://play.google.com/intl/en_us/badges/static/images/badges/en_badge_web_generic.png'/></a> -->


## Document Scanning-Capture SDK ANDROID v4.0
QuickCapture Mobile Scanning SDK Specially designed for native ANDROID from [Extrieve](https://www.extrieve.com/).
Latest Release verion : 4.0.12

> It's not "**just**" a scanning SDK. It's a "**document**" 
> scanning/capture SDK evolved with **Best Quality**, **Highest Possible Compression**, **Image Optimisation**, keeping output of the document in mind.

> Control **DPI**,**Layout** & **Size** of output images and can convert them into **PDF & TIFF**

> **QR code** & **BAR Code** Scanning & Generation

> **Developer-friendly** & **Easy to integrate** SDK.

> **Works entirely offline***, locally on the device, with **no data transferred to any server or third party**.  

*For reduced build size if needed, an initial internet connection may optionally be required to fetch ML data or resource files, depending on the specific integration and features used by the consumer application*

*Choose the **right** version that suits your need* :
- [**QuickCapture v4**](https://github.com/ExtrieveTechnologies/QuickCapture_Android): Comprehensive & advanced **AI** functionalities,**QR Code & BAR Code** Scanning & Generation.
- [**QuickCapture v3**](https://github.com/ExtrieveTechnologies/QuickCapture_Android/tree/QuickCapture-V3#document-scanning-capture-sdk-android-v3): Comprehensive & advanced **AI** functionalities, **comparatively bit** larger size [~ **20 MB**].
- [**QuickCapture v2**](https://github.com/ExtrieveTechnologies/QuickCapture_Android/tree/QuickCapture-V2#document-scanning-capture-sdk-android-v2): Optimized capture functionality, designed to be as compact as possible [~ **2 MB**].

> **End of support Notice** :
> QuickCapture SDK Android **V1** deprecated by Dec. 2022.For any further updates and support, can use **V2**
> which having no major modifications.But with improved funcionalities,feature additions and fixes.
> 
> QuickCapture SDK Android **V2** deprecated by May. 2024.For any further updates and support, can use **V4** & bugfixes on **V3** 

[Refer here for **V2 documentation** and samples](https://github.com/ExtrieveTechnologies/QuickCapture_Android/tree/QuickCapture-V2#mobile-document-scanning-sdk-android-v2)
[Refer here for **V3 documentation** and samples](https://github.com/ExtrieveTechnologies/QuickCapture_Android/tree/QuickCapture-V2#mobile-document-scanning-sdk-android-v2)

### Other available platform options
- [iOS](https://github.com/ExtrieveTechnologies/QuickCapture_IOS)
- [Fultter Plugin](https://pub.dev/packages/quickcapture)
- [React-Native Plugin](https://pub.dev/packages/quickcapture)
- [Web SDK](https://github.com/ExtrieveTechnologies/QuickCapture_WEB)


Access / Download
--------
You can use this SDK in any Android project simply by using Gradle :

```java
//Add expack central repo in settings.gradle
repositories {
  google()
  mavenCentral()
  maven {url 'https://expack.extrieve.in/maven/'}
}

//Then add implementation for SDK in dependencies in build.gradle (module:<yourmodulename>)
dependencies {
  implementation 'com.extrieve.quickcapture:QCv4_PLUS:<SDK-VERSION>'
  // use latest verision : 4.0.12
}
SDK-VERSION - Need to replace with the correct v4 series.
```

Or Maven:

```xml
<dependency>
  <groupId>com.extrieve.quickcapture</groupId>
  <artifactId>QCv4_PLUS</artifactId>
  <version>SDK-VERSION</version>
</dependency>
SDK-VERSION - Need to replace with the correct v4 series
```

Or can even integrate with the **.aar** library file and manually add the file dependency to the project/app.


Compatibility
-------------
 * **JAVA 17 Support**: QuickCapture v4 requires JAVA version 17 support for the application.
 * **Minimum Android SDK**: QuickCapture v4 requires a minimum API level of 21.
 * **Target Android SDK**: QuickCapture v4 features supports **API 34**.
  * **Compiled SDK Version**: QuickCapture v4 compiled against **API 33**.Host application using this SDK should compiled against 33 or later
 

# API &  integration  Details 
Available properties and method

SDK has four core classes and supporting classes :

 1. **CameraHelper** - *Handles the  camera  related  operations. Basically, an activity.* 
 2. **ImgHelper** - *Purpose of this class is to handle all imaging related operations.*
 3. **OpticalCodeHelper** -	*Handles the  Optical code (QR CODE & BAR CODE) related activities*
 4. **HumanFaceHelper** -	*Advanced Ai based utility class handles all functionalities such as face detection, extraction,matching & related functions.*
 5. **Config**		  	-	*Holds various configurations for SDK including licensing*
 

Based on the requirement, any one or all classes can be used.And need to import those from the SDK.
```java
    import com.extrieve.quickcapture.sdk.*;
    //OR : can import only required classes as per use cases.
    import  com.extrieve.quickcapture.sdk.ImgHelper;  
    import  com.extrieve.quickcapture.sdk.CameraHelper;
    import  com.extrieve.quickcapture.sdk.OpticalCodeHelper;
    import  com.extrieve.quickcapture.sdk.Config;  
    import com.extrieve.quickcapture.sdk.HumanFaceHelper;
    import  com.extrieve.quickcapture.sdk.ImgException;
   ```
---
## 1. CameraHelper
This core class will be implemented as an activity.This class can be initialized as intent.
```java
//JAVA
CameraHelper CameraHelper = new CameraHelper();
```
```kotlin
//Kotlin
var cameraHelper: CameraHelper? = CameraHelper()
```

With an activity call, triggering the SDK for capture activity can be done.Most operations in **CameraHelper** is **activity based**.

SDK is having multiple flows as follows :
	
* **CAMERA_CAPTURE_REVIEW** - *Default flow. Capture with SDK Camera **->** review.*
* **SYSTEM_CAMERA_CAPTURE_REVIEW** - *Capture with system default camera **->** review.*
* **IMAGE_ATTACH_REVIEW** - *Attach/pass image **->** review.*
  

**1. CAMERA_CAPTURE_REVIEW** - *Default flow of the CameraHelper. Includes Capture with SDK Camera -> Review Image.*

```java
//JAVA

//Set CaptureMode as CAMERA_CAPTURE_REVIEW
Config.CaptureSupport.CaptureMode = Config.CaptureSupport.CaptureModes.CAMERA_CAPTURE_REVIEW;
//set permission for output path that set in config.
UriphotoURI = Uri.parse(Config.CaptureSupport.OutputPath);
this.grantUriPermission(this.getPackageName(),photoURI,Intent.FLAG_GRANT_WRITE_URI_PERMISSION | Intent.FLAG_GRANT_READ_URI_PERMISSION);  

//Create CameraIntent for CameraHelper activity call.
Intent CameraIntent = new Intent(this,Class.forName("com.extrieve.quickcapture.sdk.CameraHelper"));
if  (Build.VERSION.SDK_INT <= Build.VERSION_CODES.LOLLIPOP)  {
	CameraIntent.addFlags(Intent.FLAG_GRANT_WRITE_URI_PERMISSION);
}
//Call the Activity.
startActivityForResult(CameraIntent,REQUEST_CODE_FILE_RETURN);

//On activity result,recieve the captured, reviewed, cropped, optimised & compressed image collection as array.
@Override
protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data)  
{
	super.onActivityResult(requestCode,  resultCode,  data);
	if  (requestCode == REQUEST_CODE_FILE_RETURN && resultCode == Activity.RESULT_OK)
	{  
		Boolean Status = (Boolean)data.getExtras().get("STATUS");
		String Description = (String)data.getExtras().get("DESCRIPTION");  
		if(Status == false){ 
			//Failed  to  capture
		}
		finishActivity(REQUEST_CODE_FILE_RETURN); return;
	}
	FileCollection = (ArrayList<String>)data.getExtras().get("fileCollection");
	//FileCollection //: will contain all capture images path as string
	finishActivity(REQUEST_CODE_FILE_RETURN);
}
```
```kotlin
//Kotlin
try {
    /*DEV_HELP :redirecting to camera*/
    val captureIntent = Intent(this, Class.forName("com.extrieve.quickcapture.sdk.CameraHelper"))
    val photoURI = Uri.parse(Config.CaptureSupport.OutputPath)
    grantUriPermission(
	this.packageName, photoURI,
	Intent.FLAG_GRANT_WRITE_URI_PERMISSION or Intent.FLAG_GRANT_READ_URI_PERMISSION
    )
    if (Build.VERSION.SDK_INT <= Build.VERSION_CODES.LOLLIPOP) {
	captureIntent.addFlags(Intent.FLAG_GRANT_WRITE_URI_PERMISSION)
    }
    captureActivityResultLauncher!!.launch(captureIntent)
} catch (ex: Exception) {
    /*DEV_HELP : TODO : handle invalid Exception*/
    Toast.makeText(this, "Failed to open camera  -" + ex.message, Toast.LENGTH_LONG).show()
}
```

**2. SYSTEM_CAMERA_CAPTURE_REVIEW** - *If user needs to capture an image with system default camera, this can be used. It includes Capture with system default camera -> Review*.

```java
//JAVA

//Set CaptureMode as SYSTEM_CAMERA_CAPTURE_REVIEW
Config.CaptureSupport.CaptureMode = Config.CaptureSupport.CaptureModes.SYSTEM_CAMERA_CAPTURE_REVIEW;
//set permission for output path that is set in config.
UriphotoURI = Uri.parse(Config.CaptureSupport.OutputPath);
this.grantUriPermission(this.getPackageName(),photoURI,Intent.FLAG_GRANT_WRITE_URI_PERMISSION | Intent.FLAG_GRANT_READ_URI_PERMISSION);  

//Create CameraIntent for CameraHelper activity call.
Intent CameraIntent = new Intent(this,Class.forName("com.extrieve.quickcapture.sdk.CameraHelper"));
if  (Build.VERSION.SDK_INT <= Build.VERSION_CODES.LOLLIPOP)  {
	CameraIntent.addFlags(Intent.FLAG_GRANT_WRITE_URI_PERMISSION);
}
//Call the Activity.
startActivityForResult(CameraIntent,REQUEST_CODE_FILE_RETURN);

//On activity result,recieve the captured, reviewed, cropped, optimised & compressed image colletion as array.
@Override
protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data)  
{
	super.onActivityResult(requestCode,  resultCode,  data);
	if  (requestCode == REQUEST_CODE_FILE_RETURN && resultCode == Activity.RESULT_OK)
	{  
		Boolean Status = (Boolean)data.getExtras().get("STATUS");
		String Description = (String)data.getExtras().get("DESCRIPTION");  
		if(Status == false){ 
			//Failed  to  capture
		}
		finishActivity(REQUEST_CODE_FILE_RETURN); return;
	}
	FileCollection = (ArrayList<String>)data.getExtras().get("fileCollection");
	//FileCollection //: will contain all capture images path as string
	finishActivity(REQUEST_CODE_FILE_RETURN);
}
```
```kotlin
//Kotlin
try {
    /*DEV_HELP :redirecting to camera*/
    val captureIntent = Intent(this, Class.forName("com.extrieve.quickcapture.sdk.CameraHelper"))
    val photoURI = Uri.parse(Config.CaptureSupport.OutputPath)
    grantUriPermission(
	this.packageName, photoURI,
	Intent.FLAG_GRANT_WRITE_URI_PERMISSION or Intent.FLAG_GRANT_READ_URI_PERMISSION
    )
    if (Build.VERSION.SDK_INT <= Build.VERSION_CODES.LOLLIPOP) {
	captureIntent.addFlags(Intent.FLAG_GRANT_WRITE_URI_PERMISSION)
    }
    captureActivityResultLauncher!!.launch(captureIntent)
} catch (ex: Exception) {
    /*DEV_HELP : TODO : handle invalid Exception*/
    Toast.makeText(this, "Failed to open camera  -" + ex.message, Toast.LENGTH_LONG).show()
}
```

**3. IMAGE_ATTACH_REVIEW** - *This option can be used if the user needs to review an image from their device's gallery. After attaching each image, the review and all dependent functionalities become available*.

```java
//JAVA

//Set CaptureMode as IMAGE_ATTACH_REVIEW
Config.CaptureSupport.CaptureMode = Config.CaptureSupport.CaptureModes.IMAGE_ATTACH_REVIEW;
//Create/Convert/ get Image URI from image source.
Uri ImgUri = data.getData();
//Create ReviewIntent for CameraHelper activity call.
Intent ReviewIntent = new Intent(this,Class.forName("com.extrieve.quickcapture.sdk.CameraHelper"));
//Add the image URI to intent request with a key : ATTACHED_IMAGE.
ReviewIntent.putExtra("ATTACHED_IMAGE", ImUri);
//Call the Activity.
startActivityForResult(ReviewIntent,REQUEST_CODE_FILE_RETURN);

//On activity result,recieve the captured, reviewed, cropped, optimised & compressed image colletion as array.
@Override
protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data)  
{
	super.onActivityResult(requestCode,  resultCode,  data);
	if  (requestCode == REQUEST_CODE_FILE_RETURN && resultCode == Activity.RESULT_OK)
	{  
		Boolean Status = (Boolean)data.getExtras().get("STATUS");
		String Description = (String)data.getExtras().get("DESCRIPTION");  
		if(Status == false){ 
			//Failed  to  capture
		}
		finishActivity(REQUEST_CODE_FILE_RETURN); return;
	}
	FileCollection = (ArrayList<String>)data.getExtras().get("fileCollection");
	//FileCollection //: will contains all capture images path as string
	finishActivity(REQUEST_CODE_FILE_RETURN);
}
```
```kotlin
//Kotlin

try {
    /*DEV_HELP :redirecting to camera*/
    val captureIntent = Intent(this, Class.forName("com.extrieve.quickcapture.sdk.CameraHelper"))
    val photoURI = Uri.parse(Config.CaptureSupport.OutputPath)
    grantUriPermission(
	this.packageName, photoURI,
	Intent.FLAG_GRANT_WRITE_URI_PERMISSION or Intent.FLAG_GRANT_READ_URI_PERMISSION
    )
    if (Build.VERSION.SDK_INT <= Build.VERSION_CODES.LOLLIPOP) {
	captureIntent.addFlags(Intent.FLAG_GRANT_WRITE_URI_PERMISSION)
    }
    captureActivityResultLauncher!!.launch(captureIntent)
} catch (ex: Exception) {
    /*DEV_HELP : TODO : handle invalid Exception*/
    Toast.makeText(this, "Failed to open camera  -" + ex.message, Toast.LENGTH_LONG).show()
}
```
## 2. Config
The SDK includes a supporting class called for static configuration. This class holds all configurations related to the SDK. It also contains sub-configuration collection for further organization. This includes:  :

**CaptureSupport** - Contains all the Capture & review related configurations. **Config.CaptureSupport**   contains various configurations as follows:

- **BottomStampData** -  This configuration will automatically print the specified text at the bottom of the captured image with correct alignment, font size and DPI**. This also  supports placeholders, such as  `{DATETIME}`, which will be replaced with the current date and time from the device at the time of stamping.  **$** - for  new line print.
	```java
	 //JAVA
	Config.CaptureSupport.BottomStampData ="{DATETIME} , Other info $ next line info.";
	```
	```kotlin
	 //Kotlin
	Config!!.CaptureSupport!!.BottomStampData = ="{DATETIME} , Other info $ next line info.";
	```

- **OutputPath** - To set the output directory in which the captured images will be saved, base app should have rights to write to the provided path.
	```java
 	//JAVA
	Config.CaptureSupport.OutputPath = "pass output path sd string";
	```
	```kotlin
 	//Kotlin
	Config!!.CaptureSupport!!.OutputPath = "pass output path sd string";
	```
- **MaxPage** - To set the number of captures to do on each camera session, can also control whether the capture mode is single  or multi i.e :
	> if  MaxPage  <= 0 /  not  set:  means  unlimited.If  MaxPage  >= 1:
	> means  limited.
	```java
	//JAVA
	// MaxPage <= 0  : Unlimited Capture Mode  
	// MaxPage = 1   : Limited Single Capture  
	// MaxPage > 1   : Limited Multi Capture Mode  
	Config.CaptureSupport.MaxPage = 0;
	```
	```java
	//Kotlin
	// MaxPage <= 0  : Unlimited Capture Mode  
	// MaxPage = 1   : Limited Single Capture  
	// MaxPage > 1   : Limited Multi Capture Mode  
	Config!!.CaptureSupport!!.MaxPage = 0;
	```
- **ColorMode**  -  To Set the capture color mode - supporting color and grayscale.
	```java
	//JAVA
	Config.CaptureSupport.ColorMode = Config.CaptureSupport.ColorModes.RBG;
	//RBG (1) - Use capture flow in color mode.
	//GREY (2) - Use capture flow in grey scale mode.
	```
	```kotlin
	//Kotlin
	Config!!.CaptureSupport!!.ColorMode = Config!!.CaptureSupport!!.ColorModes!!.RBG;
	//RBG (1) - Use capture flow in color mode.
	//GREY (2) - Use capture flow in grey scale mode.
	```
- **EnableFlash**  -  Enable Document capture specific flash control for SDK camera.
	```java
	//JAVA
	Config.CaptureSupport.EnableFlash = true;
	```
	```kotlin
	//Kotlin
	Config!!.CaptureSupport!!.EnableFlash = true;
	```
- **CaptureSound**  -  To Enable camera capture sound.
	```java
	//JAVA
	Config.CaptureSupport.CaptureSound = true;
	```
	```kotlin
	//Kotlin
	Config!!.CaptureSupport!!.CaptureSound = true;
	```

- **CameraToggle**  -  Toggle  camera  between  front  and  back.
	```java
	//JAVA
	Config.CaptureSupport.CameraToggle = CameraToggleType.ENABLE_BACK_DEFAULT;
	//DISABLED (0) -Disable camera toggle option.
	//ENABLE_BACK_DEFAULT (1) - Enable camera toggle option with Front camera by default.
	//ENABLE_FRONT_DEFAULT (2) - Enable camera toggle option with Back camera  by default.
	```
	```kotlin
	//Kotlin
	Config!!.CaptureSupport!!.CameraToggle = CameraToggleType!!.ENABLE_BACK_DEFAULT;
	//DISABLED (0) -Disable camera toggle option.
	//ENABLE_BACK_DEFAULT (1) - Enable camera toggle option with Front camera by default.
	//ENABLE_FRONT_DEFAULT (2) - Enable camera toggle option with Back camera  by default.
	```

**Common** - Contains various configurations as follows:

- **SDKInfo**  - Contains all version related information on SDK.
	```java
	//JAVA
	Config.Common.SDKInfo;
	```
	```kotlin
	//Kotlin
	Config!!.Common!!.SDKInfo;
	```
- **DeviceInfo** - Will share all general information about the device.
	```java
	//JAVA
	Config.Common.DeviceInfo;
	```
	```kotlin
	//Kotlin
	Config!!.Common!!.DeviceInfo;
	```
**License** - Cotrolls all activities relates to licensing.
- ***Activate*** - *Method to activate the SDK license.*
	```java
 	//JAVA
	Config.License.Activate(hostApplicationContext,licenseString);
	```
 	```kotlin
  	//Kotlin
	Config!!.License!!.Activate(hostApplicationContext,licenseString)
	```
	 > **hostApplicationContext** : Application context of host/client application which is using the SDK.
	 	 > **licenseString** : Licence data in string format.
	 
	 

## 3. ImgHelper
Following are the options/methods available from class **ImgHelper** :
```java
//JAVA
ImgHelper ImageHelper = new ImgHelper(this);
```
```kotlin
//Kotlin
var ImageHelper: ImgHelper? = ImgHelper(this)
```
- ***SetImageQuality*** - *Set the Quality of the image, Document_Quality is used. If documents are used further for any automations and OCR, use Document_Quality.*
	 >*Available Image Qualities* :
		1. Photo_Quality.
		2. Document_Quality.
		3. Compressed_Document.
		
	```java
	//JAVA
	ImageHelper.SetImageQuality(ImgHelper.ImageQuality.Photo_Quality.ordinal());
	//--------------------------
	ImageHelper.SetImageQuality(1);//0,1,2 - Photo_Quality, Document_Quality, Compressed_Document
	```
 	```kotlin
  	//Kotlin
	imageHelper!!.SetImageQuality(1)
	```
- ***SetPageLayout*** - *Set the Layout for the images generated/processed by the system.*
	```java
	//JAVA
	ImageHelper.SetPageLayout(ImgHelper.LayoutType.A4.ordinal());
	//--------------------------
	ImageHelper.SetPageLayout(4);//A1-A7(1-7),PHOTO,CUSTOM,ID(8,9,10)
	```
	```kotlin
	//Kotlin
	imageHelper!!.SetPageLayout(4)
	```
	 >*Available layouts* : A1, A2, A3, **A4**, A5, A6, A7,PHOTO & CUSTOM
	 
	*A4 is the most recommended layout for document capture scenarios.*
	 
- ***SetDPI*** - *Set DPI(depth per inch) for the image.*
	```java
	//JAVA
	ImageHelper.SetDPI(ImgHelper.DPI.DPI_200.ordinal());
	//--------------------------
	ImageHelper.SetDPI(200);//int dpi_val = 150, 200, 300, 500, 600;
	```
	```kotlin
	//Kotlin
	imageHelper!!.SetDPI(200)
	```
	 >*Available DPI* : DPI_150, DPI_200, DPI_300, DPI_500, DPI_600
	 
	 *150 & 200 DPI is most used.And 200 DPI recommended for OCR and other image extraction prior to capture.*
	 
- ***GetThumbnail*** - *This method Will build thumbnail for the given image in custom width,height & AspectRatio.*
	```java
	//JAVA
	Bitmap thumb = ImageHelper.GetThumbnail(ImageBitmap, 600, 600, true);
	/*
	Bitmap GetThumbnail(
		@NonNull  Bitmap bm,
	    int reqHeight,
	    int reqWidth,
	    Boolean AspectRatio )throws ImgException.
	*/
	```
	```kotlin
	//KOTLIN
	var thumb = ImageHelper!!.GetThumbnail(ImageBitmap, 600, 600, true);
	```
- ***CompressToJPEG*** - *This method will Compress the provided bitmap image and will save to given path.\*
	```java
	//JAVA
	Boolean Iscompressed = ImageHelper.CompressToJPEG(bitmap,outputFilePath);
	/*
	Boolean CompressToJPEG(Bitmap bm,String outputFilePath)
		throws ImgException
	*/
	```
	```kotlin
	//KOTLIN
	var Iscompressed = ImageHelper!!.CompressToJPEG(bitmap, outputFilePath);
	```
	
- ***rotateBitmap*** - *This method will rotate the image to preferred orientation.*
	 ```java
	//JAVA
	Bitmap rotatedBm = ImageHelper.rotateBitmapDegree(nBm, RotationDegree);
	/*
	Bitmap rotateBitmapDegree(Bitmap bitmap,int Degree)
		throws ImgException
	*/
	```
  	```kotlin
	//KOTLIN
	var thumb = ImageHelper!!.rotateBitmapDegree(bitmap, RotationDegree);
	```
- **GetTiffForLastCapture** - Build Tiff output file from last captured set of images.
	```java
	//JAVA
	ImageHelper.GetTiffForLastCapture(outPutFileWithpath);
	//on success, will respond with string : "SUCCESS:::TiffFilePath";
	//use  ":::"  char.  key  to  split  the  response.
	//on failure,will respond with string : "FAILED:::Reason for failure";
	//use ":::" char. key to split the response.
	//on failure, error details can collect from CameraSupport.CamConfigClass.LastLogInfo
	```
	```kotlin
	//KOTLIN
	var thumb = ImageHelper!!.GetTiffForLastCapture(outPutFileWithpath);
	```
- **GetPDFForLastCapture**  -  Build  PDF  output file  from  last  captured  set  of  images.
	```java
	//JAVA
	ImageHelper.GetPDFForLastCapture(outPutFileWithpath);
	//on success, will respond with string : "SUCCESS:::PdfFilePath";
	//use  ":::"  char.  key  to  split  the  response.
	//on failure,will respond with string : "FAILED:::Reason for failure";
	//use ":::" char. key to split the response.
	//on failure, error details can collect from CameraSupport.CamConfigClass.LastLogInfo
	```
 	```kotlin
	//KOTLIN
	var thumb = ImageHelper!!.GetPDFForLastCapture(outPutFileWithpath);
	```
- **BuildTiff**  - Build tiff output file from the list  of  images shared.
	```java
	//JAVA
	ImageHelper.BuildTiff(ImageCol,OutputTiffFilePath);
	*@param "Image File path collection as ArrayList<String>".
	*@param "Output Tiff FilePath as String".
	*@return on failure = "FAILED:::REASON" || on success = "SUCCESS:::TIFF file path".
	```
	```kotlin
	//KOTLIN
	var thumb = ImageHelper!!.BuildTiff(ImageCol,OutputTiffFilePath);
	```
- **BuildPDF**  - Build PDF output file from last captured set of images.
	```java
	//JAVA
	ImageHelper.BuildPDF(ImageCol,outPutPDFFileWithpath);
	*@param  "Image File path collection as ArrayList<String>"
	*@param "Output Tiff FilePath as String".
	*@return  on failure = "FAILED:::REASON" || on success = "SUCCESS:::PDF file path".
	```
	```kotlin
	//KOTLIN
	var thumb = ImageHelper!!.BuildPDF(ImageCol,OutputTiffFilePath);
	```
## 5. OpticalCodeHelper
Following are the options/methods available from class **OpticalCodeHelper** :
```java
//JAVA
OpticalCodeHelper opticalCodeObj = new OpticalCodeHelper(this);
```
```kotlin
//Kotlin
var opticalCodeObj : OpticalCodeHelper? = OpticalCodeHelper(this)
```
- ***GenerateQRCode*** - *Method to generate QR Code*.Data need to pass in string format.Will return a bitmap of generated QR Code.
		
	```java
	//JAVA
	Bitmap qrcode = opticalCodeObj.GenerateQRCode(QRdata);
	```
	```kotlin
	//KOTLIN
	var qrcode = opticalCodeObj!!.GenerateQRCode(QRdata);
	```
- ***GenerateBarcode*** - *Method to generate BAR Code*.Data need to pass in string format.Will return a bitmap of generated BAR Code.
		
	```java
	//JAVA
	Bitmap qrcode = opticalCodeObj.GenerateBarcode(QRdata);
	```
	```kotlin
	//KOTLIN
	var qrcode = opticalCodeObj!!.GenerateBarcode(QRdata);
	```
- ***GenerateBarcodeWithText*** - *Method to generate GenerateBarcodeWithText*.Data need to pass in string format.Will return a bitmap of generated BAR Code with visible text.
		
	```java
	//JAVA
	Bitmap qrcode = opticalCodeObj.GenerateBarcode(QRdata);
	```
	```kotlin
	//KOTLIN
	var qrcode = opticalCodeObj!!.GenerateBarcode(QRdata);
	```
- **Scan / Capture OpticalCodes (QR Code / BAR Code)** - *Option to Scan or capture the **Optical Codes**.This is an activity based call.On activity result, will get the extracted data from the optical code*

	```java
	//JAVA
	//Register the activity results
	private OpticalCodeActivityResultLauncher<Intent> registerForActivityResult(new ActivityResultContracts.StartActivityForResult(), result -> handleOpticalCaptureActivityResult(result));
	
	//trigger the activity
	try {  
		Intent CameraIntent = new Intent(this, Class.forName("com.extrieve.quickcapture.sdk.OpticalCodeHelper"));  
		Uri photoURI = Uri.parse(Config.CaptureSupport.OutputPath);  
		this.grantUriPermission(this.getPackageName(), photoURI, Intent.FLAG_GRANT_WRITE_URI_PERMISSION | Intent.FLAG_GRANT_READ_URI_PERMISSION);  
		if (Build.VERSION.SDK_INT <= Build.VERSION_CODES.LOLLIPOP) {  
			CameraIntent.addFlags(Intent.FLAG_GRANT_WRITE_URI_PERMISSION);  
		}  
		captureActivityResultLauncher.launch(CameraIntent);  
	} catch (ClassNotFoundException e) {  
		Toast.makeText(this, "Failed to open camera + ", Toast.LENGTH_LONG).show();  
		e.printStackTrace();  
	}
	
	//Registered method to handle the result on callback
	private void handleOpticalCaptureActivityResult(ActivityResult result) {  
	{  
		int resultCode = result.getResultCode();  
		if (resultCode == Activity.RESULT_OK) {
			String qrData = (String) data.getExtras().get("DATA");  
			String qrType = (String) data.getExtras().get("TYPE");  
			//showToast("QR_BAR_Code Data: " + qrData, Gravity.CENTER);  
			//Here captured optical codes data can be extracted
			finishActivity(REQUEST_CODE_FILE_RETURN);  
			return;
		}
	}
	```
	```kotlin
	//Kotlin
	
	// Register the activity results
	private val opticalCaptureActivityResultLauncher = registerForActivityResult(ActivityResultContracts.StartActivityForResult()) { result ->
	   handleOpticalCaptureActivityResult(result)
	}

	// Trigger the activity
	try {
	   val cameraIntent = Intent(this, Class.forName("com.extrieve.quickcapture.sdk.OpticalCodeHelper"))
	   val photoURI: Uri = Uri.parse(Config.CaptureSupport.OutputPath)
	   this.grantUriPermission(this.packageName, photoURI, Intent.FLAG_GRANT_WRITE_URI_PERMISSION or Intent.FLAG_GRANT_READ_URI_PERMISSION)
	   if (Build.VERSION.SDK_INT <= Build.VERSION_CODES.LOLLIPOP) {
	       cameraIntent.addFlags(Intent.FLAG_GRANT_WRITE_URI_PERMISSION)
	   }
	   opticalCaptureActivityResultLauncher.launch(cameraIntent)
	} catch (e: ClassNotFoundException) {
	   Toast.makeText(this, "Failed to open camera + ", Toast.LENGTH_LONG).show()
	   e.printStackTrace()
	}

	// Registered method to handle the result on callback
	private fun handleOpticalCaptureActivityResult(result: ActivityResult) {
	   val resultCode = result.resultCode
	   val data = result.data
	   if (resultCode == Activity.RESULT_OK && data != null) {
	       val qrData = data.extras?.getString("DATA")
	       val qrType = data.extras?.getString("TYPE")
	       // showToast("QR_BAR_Code Data: $qrData", Gravity.CENTER)
	       // Here captured optical codes data can be extracted
	       finishActivity(REQUEST_CODE_FILE_RETURN)
	   }
	}
	```
# Error Handling & Exceptions 
- As a part of exceptional error handling **ImgException** class is available.
	- *Following are the possible errors and corresponding codes*:
		- CREATE_FILE_ERROR= **-100**;
		- IMAGE_ROTATION_ERROR= **-101**;
		- LOAD_TO_BUFFER_ERROR= **-102**;
		- DELETE_FILE_ERROR= **-103**;
		- GET_ROTATION_ERROR= **-104**;
		- ROTATE_BITMAP_ERROR= **-105**;
		- BITMAP_RESIZE_ERROR= **-106**;
		- CAMERA_HELPER_ERROR= **-107**;
		- LOG_CREATION_ERROR= **-108**;
- Also with **Config.CaptureSupport.LastLogInfo** last logged exception or error details can be identified.

**Extrieve** - *Your Expert in Document Management & AI Solutions.*

[© 1996 - 2024 Extrieve Technologies](https://www.extrieve.com/)
	
