# Face Detection with Landmarks and Emotion Recognition

I aimed at implementing JavaScript's API `face-api.js` in the browser. This API is able to `Detect` (ability to distinguish an object from the background) human faces. It is also able to do `Recognition` (ability to classify the object class such as face landmarks, human emotions, gender and age.

- **Face detection** Get the contours of detected faces and their eyes, eyebrows, lips, and nose.
- **Face landmarks recognition** Get the coordinates of the eyes, ears, cheeks, nose, and mouth of every face detected.
- **Emotion recognition** Determine whether a person is smiling or has their eyes closed.
- **Track faces across video frames** Get an identifier for each unique detected face. The identifier is consistent across invocations, so you can perform image manipulation on a particular person in a video stream
- **Process video frames in real time** Face detection is performed on the device, and is fast enough to be used in real-time applications, such as video manipulation.

https://user-images.githubusercontent.com/80494835/197230358-48047392-e3ea-4073-b3f9-22c2d3b53646.mp4

## API's capabilities: building the canvas function



### Face detection

The most accurate face detector is a SSD (Single Shot Multibox Detector), which is basically a CNN based on MobileNet V1, with some additional box prediction layers stacked on top of the network.

Furthmore, face-api.js implements an optimized Tiny Face Detector, basically an even tinier version of Tiny Yolo v2 utilizing depthwise seperable convolutions instead of regular convolutions, which is a much faster, but slightly less accurate face detector compared to SSD MobileNet V1.

The networks return the bounding boxes of each face, with their corresponding scores, e.g. the probability of each bounding box showing a face. 



### Face Landmarks

For that purpose face-api.js implements a simple CNN, which returns the 68 point face landmarks of a given face image:

### Face expressions


## Implementation

### Including the Script

First of all, get the latest build from dist/face-api.js or the minifed version from dist/face-api.min.js and include the script:

```html
<script src="face-api.js"></script>
```

### Loading the Model Data



## Some challenges

### Accessing Your Webcam in HTML

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

### Uncaught Reference Error: faceapi is not defined

https://stackoverflow.com/questions/71617220/uncaught-reference-error-faceapi-is-not-defined

https://github.com/WebDevSimplified/Face-Detection-JavaScript/issues/8

### Uncaught TypeError: Cannot read property 'loadFromUri' of undefined 

https://github.com/justadudewhohacks/face-api.js/issues/735


## Try it







# References

- https://www.kirupa.com/html5/accessing_your_webcam_in_html5.htm
- Some error handling: https://stackoverflow.com/questions/27120757/failed-to-execute-createobjecturl-on-url
- https://itnext.io/face-api-js-javascript-api-for-face-recognition-in-the-browser-with-tensorflow-js-bcc2a6c4cf07
- https://developers.google.com/ml-kit/vision/face-detection?hl=en

