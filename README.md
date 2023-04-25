# GoogleCodeScanner sample

[ProAndroidDev: Google Code Scanner makes code scanning easy on Android](https://proandroiddev.com/google-code-scanner-makes-code-scanning-easy-android-3fba3d2edafc)

## Implementation

Add dependency
`
dependencies {
  implementation 'com.google.android.gms:play-services-code-scanner:16.0.0'
}
`

Add metadata to download the scanner module automatically
`
<application ...>
  ...
  <meta-data
      android:name="com.google.mlkit.vision.DEPENDENCIES"
      android:value="barcode_ui"/>
  ...
</application>
`

## Scan a code

Configure barcode scanner options — GmsBarcodeScannerOptions [Optional]

`
val options = GmsBarcodeScannerOptions.Builder()
    .setBarcodeFormats(
        Barcode.FORMAT_QR_CODE,
        Barcode.FORMAT_AZTEC)
    .build()
` 

## Get an instance of GmsBarcodeScanner

`
val scanner = GmsBarcodeScanning.getClient(this)

// Or with a configured options
val options = GmsBarcodeScannerOptions.Builder()
    .setBarcodeFormats(
        Barcode.FORMAT_QR_CODE,
        Barcode.FORMAT_AZTEC)
    .build()

val scanner = GmsBarcodeScanning.getClient(this, options)
`

## Initiate the scan ║▌║█║▌│║▌║▌█

`
private fun initiateScanner(
    gmsBarcodeScanner: GmsBarcodeScanner,
    onSuccess: (Barcode) -> Unit,
    onCancel: () -> Unit,
    onFailure: (Exception) -> Unit
) {
    gmsBarcodeScanner.startScan()
        .addOnSuccessListener { barcode ->
            // Task completed successfully
            val result = barcode.rawValue
            Log.d(TAG, "initiateScanner: $result")
            // check the value - URL, TEXT, etc.
            when (barcode.valueType) {
                Barcode.TYPE_URL -> {
                    Log.d(TAG, "initiateScanner: ${barcode.valueType}")
                }
                else -> {
                    Log.d(TAG, "initiateScanner: ${barcode.valueType}")
                }
            }
            // Display valu
            Log.d(TAG, "initiateScanner: Display value: ${barcode.displayValue}")
            // Formate - FORMAT_AZTEC, etc.
            Log.d(TAG, "initiateScanner: Format: ${barcode.format}")
            onSuccess(barcode)
        }
        .addOnCanceledListener {
            // Task canceled by the user
            onCancel()
        }
        .addOnFailureListener { e ->
            // Task failed with an exception
            onFailure(e)
        }
}
`
