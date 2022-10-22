# Face Detection with Landmarks and Emotion Recognition

I aimed at implementing JavaScript's API `face-api.js`, as a web application. This API is able to `Detect` (ability to distinguish an object from the background) human faces. It is also able to do `Recognition` (ability to classify the object class such as face landmarks, human emotions, gender and age.

https://user-images.githubusercontent.com/80494835/197230358-48047392-e3ea-4073-b3f9-22c2d3b53646.mp4



## Some challenges

### Accessing Your Webcam in HTML

https://www.kirupa.com/html5/accessing_your_webcam_in_html5.htm

We can communicate with our webcam and access its video stream from our browser with just some JavaScript code. We only need a browser that supports the `getUserMedia` function.

Two components are essentials in getting data from our webcam displayed on our screen. 

- The HTML `video` element 
- The JavaScript `getUserMedia` function
 
The video element is pretty straightforward in what it does. It is responsible for taking the video stream from your webcam and actually displaying it on the screen.

```html
<video id="video" width="720" height="560" autoplay="true"></video>
```

By setting the autoplay attribute to true, we ensure that our video starts to display automatically once we have our webcam video stream.

That is because we haven't added the JavaScript that ties together our video element with your webcam. We'll do that next! The getUserMedia function allows us to do three things:

- Specify whether we want to get video data from the webcam, audio data from a microphone, or both.
- If the user grants permission to access the webcam, specify a success function to call where you can process the webcam data further.
- If the user does not grant permission to access the webcam or your webcam runs into some other kind of error, specify a error function to handle the error conditions.

```javascript
function startVideo() {
    navigator.getMedia = navigator.getUserMedia ||
                         navigator.webkitGetUserMedia ||
                         navigator.mozGetUserMedia ||
                         navigator.msGetUserMedia;
    // Capture video
    navigator.getMedia({
        video: true,
        audio: false
    }, function(stream) {
        video.srcObject=stream;
        video.play();
    }, function(error){
        // an error occoured
    });
}
```

For what we are trying to do, we call the getUserMedia function and tell it to only retrieve the video from the webcam. Once we retrieve the video, we tell our success function to send the video data to our video element for display on our screen.

**Some error handling:**

This error is caused because the function createObjectURL no longer accepts media stream object for Google Chrome

I changed this:

```javascript
video.src=vendorUrl.createObjectURL(stream);
video.play();
```

to this:

```javascript
video.srcObject=stream;
video.play();
```


https://stackoverflow.com/questions/27120757/failed-to-execute-createobjecturl-on-url

### Uncaught Reference Error: faceapi is not defined

https://stackoverflow.com/questions/71617220/uncaught-reference-error-faceapi-is-not-defined

https://github.com/WebDevSimplified/Face-Detection-JavaScript/issues/8

### Uncaught TypeError: Cannot read property 'loadFromUri' of undefined 

https://github.com/justadudewhohacks/face-api.js/issues/735



## API's capabilities

### Multiple faces detection




### Face Landmarks


### Face expressions



### High level API

https://github.com/justadudewhohacks/face-api.js#getting-started-loading-models


```
console.log(faceapi.nets)
// ageGenderNet
// faceExpressionNet
// faceLandmark68Net
// faceLandmark68TinyNet
// faceRecognitionNet
// ssdMobilenetv1
// tinyFaceDetector
// tinyYolov2
```






## Try it



