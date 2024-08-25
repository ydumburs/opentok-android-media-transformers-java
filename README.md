Overview
======================
This project is based on OpenTok's [Media-Transformers-Java](https://github.com/opentok/opentok-android-sdk-samples/tree/main/Media-Transformers-Java) Android sample application.

Changes Made
======================
* Implemented a toggle feature to adjust the background blur strength
* Added a new image for the background image to switch from the original image upon a second button click.
* Updated the button text to reflect the current state.

How It Works
======================
* Clicking the `BLUR` button applies a `High (10px)` strength blur. Clicking it increases the blur with a custom radius of `30px`.
* Clicking the `VIRTUAL` button applies a background image. Clicking it again will change the background to another image.

How to Launch the App
======================
Open the project folder in Android Studio. Set a session credentials in the config file and run the app. For more details, please visit [Basic-Video-Chat-Java](https://github.com/opentok/opentok-android-sdk-samples/tree/main/Basic-Video-Chat-Java)

App Screenshots
======================
<img width="200" alt="Screenshot 2024-08-23 at 3 05 20 PM" src="https://github.com/user-attachments/assets/74b1eb9f-66f4-4459-952f-a19379abba78">

Added Changes Details
======================  
## **Blur button functionality**  
Use a boolean flag `isBlurHigh` to toggle between 2 blur levels. If `isBlurHigh` is true, the blur level is set to "10" (=preset level `High`); if false, it’s set to `30`. It also toggles the text on a button id `setbackgroundblur` between `Blur` and `Blur2`. For comparison, see the original code: [MainActivity.java#L389](https://github.com/opentok/opentok-android-sdk-samples/blob/0f23c71d191fc29af233789f8d55f242896457e5/Media-Transformers-Java/app/src/main/java/com/tokbox/sample/mediatransformers/MainActivity.java#L389))
```
// in MainActivity.java
private boolean isBlurHigh = true;
    public void SetBackgroundBlurAndCustomTransformers(View view) {
        videoTransformers.clear();
        String blurLevel = isBlurHigh ? "10" : "30";
        String blurText = isBlurHigh ? "Blur" : "Blur2";
        Button blurToggle = findViewById(R.id.setbackgroundblur);
        blurToggle.setText(blurText);
        PublisherKit.VideoTransformer backgroundBlur = publisher.new VideoTransformer("BackgroundBlur", "{\"radius\":\"Custom\",\"custom_radius\":\"" + blurLevel + "\"}");
        PublisherKit.VideoTransformer myCustomTransformer = publisher.new VideoTransformer("myTransformer", watermarkTransformer);
        videoTransformers.add(backgroundBlur);
        videoTransformers.add(myCustomTransformer);
        publisher.setVideoTransformers(videoTransformers);
        isBlurHigh = !isBlurHigh;
        buttonBlur.setBackgroundColor(Color.GREEN);
        buttonVirtualBackground.setBackgroundColor(Color.RED);
    }
```
## **Virtual button functionality**  
Use a boolean flag `isBeachImage` to switch between two background images. If `isBeachImage` is true, it uses `beachImageFile`; if false, it uses `meetingImageFile`. It also updates the text of a button with ID `setbackgroundreplacement` to `Virtual` if `isBeachImage` is true, or `Virtual2` if false. For comparison, see the original code: [MainActivity.java#L400](https://github.com/opentok/opentok-android-sdk-samples/blob/0f23c71d191fc29af233789f8d55f242896457e5/Media-Transformers-Java/app/src/main/java/com/tokbox/sample/mediatransformers/MainActivity.java#L400).
```
// in MainActivity.java
private boolean isBeachImage = true;
    public void SetBackgroundReplacementTransformers(View view) {
        videoTransformers.clear();
        String virtualText = isBeachImage ? "Virtual" : "Virtual2";
        Button virtualToggle = findViewById(R.id.setbackgroundreplacement);
        virtualToggle.setText(virtualText);

        PublisherKit.VideoTransformer BackgroundReplacement;
        if (isBeachImage) {
            BackgroundReplacement = publisher.new VideoTransformer("BackgroundReplacement", "{\"image_file_path\":\"" + beachImageFile.getAbsolutePath() + "\"}");
        } else {
            BackgroundReplacement = publisher.new VideoTransformer("BackgroundReplacement", "{\"image_file_path\":\"" + meetingImageFile.getAbsolutePath() + "\"}");
        }
            videoTransformers.add(BackgroundReplacement);
        publisher.setVideoTransformers(videoTransformers);
        isBeachImage = !isBeachImage;
        buttonVirtualBackground.setBackgroundColor(Color.GREEN);
        buttonBlur.setBackgroundColor(Color.RED);
    }
```
Define another Bitmap for the newly added image file. Loads a drawable image resource into a Bitmap, then saves this bitmap as a PNG file in the app's internal storage directory, handling any potential I/O errors.
```
Bitmap bitmap2 = BitmapFactory.decodeResource(resources, R.drawable.meeting); 
        meetingImageFile = new File(context.getFilesDir(), resourceName + ".png");

        try (FileOutputStream outputStream = new FileOutputStream(meetingImageFile)) {
            bitmap2.compress(Bitmap.CompressFormat.PNG, 100, outputStream);
            outputStream.flush();
        } catch (IOException e) {
            e.printStackTrace();
            return;
        }
```


