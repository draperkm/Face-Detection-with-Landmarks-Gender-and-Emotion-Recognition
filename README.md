# Face Detection with Landmarks and Emotion Recognition

I aimed at implementing JavaScript's API `face-api.js` in the browser. This API is able to `Detect` (ability to distinguish an object from the background) human faces. It is also able to do `Recognition` (ability to classify the object class such as face landmarks, human emotions, gender and age.

- **Face detection** Gives a bounding box for every face detected.
- **Face landmarks recognition** Gets the coordinates of the eyes, ears, cheeks, nose, and mouth of every face detected.
- **Emotion recognition** Determine whether a person is happy - sad - angry - disgusted - fearful - surprised.
- **Track faces across video frames** Get an identifier for each unique detected face. The identifier is consistent across invocations.
- **Process video frames in real time** Face detection is performed on the device, and is fast enough to be used in real-time applications.

https://user-images.githubusercontent.com/80494835/197230358-48047392-e3ea-4073-b3f9-22c2d3b53646.mp4

## APIs' capabilities

### Face detection

The most accurate face detector is a **SSD (Single Shot Multibox Detector)**, which is basically a CNN based on **MobileNet V1**, with some additional box prediction layers stacked on top of the network. Furthmore, face-api.js implements an optimized **Tiny Face Detector**, basically an even tinier version of **Tiny Yolo v2** utilizing depthwise seperable convolutions instead of regular convolutions, which is a much faster, but slightly less accurate face detector compared to SSD MobileNet V1. The networks return the **bounding boxes** of each face, with their corresponding scores, e.g. the probability of each bounding box showing a face. 

<img width="400" alt="Screenshot 2022-10-22 at 15 41 23" src="https://user-images.githubusercontent.com/80494835/197345637-ca5195f0-dbd0-45b3-a9a1-9e8572795712.png">

### Face Landmarks

For that purpose face-api.js implements a simple CNN, which returns the 68 point face landmarks of a given face image:

<img width="400" alt="Screenshot 2022-10-22 at 15 36 29" src="https://user-images.githubusercontent.com/80494835/197346091-a0d0255f-a2fc-4e5f-8e13-585d340fec7a.png"><img width="400" alt="Screenshot 2022-10-22 at 15 36 08" src="https://user-images.githubusercontent.com/80494835/197346099-703c6b0a-f7c7-4be9-80ef-add91cd95f79.png">


### Face expressions

The face expression recognition model is lightweight, fast and provides reasonable accuracy. The model has a size of roughly 310kb and it employs depthwise separable convolutions and densely connected blocks. It has been trained on a variety of images from publicly available datasets as well as images scraped from the web. Note, that wearing glasses might decrease the accuracy of the prediction results.

<img width="400" alt="Screenshot 2022-10-22 " src="https://user-images.githubusercontent.com/80494835/197346285-6b1b0a43-b687-4894-bc4f-c80e4796a272.png">

## Implementation

### Including the Script

First of all, get the latest build from dist/face-api.js or the minifed version from dist/face-api.min.js and include the script:

```html
<script src="face-api.js"></script>
```

### Loading the Model Data

Depending on the requirements of your application you can specifically load the models you need, but to run a full end to end example we will need to load the face detection, face landmark and face recognition model. The model files can simply be provided as static assets in your web app or you can host them somewhere else and they can be loaded by specifying the route or url to the files. Let’s say you are providing them in a models directory along with your assets under public/models

```javascript
faceapi.nets.tinyFaceDetector.loadFromUri('/models'),
faceapi.nets.faceLandmark68Net.loadFromUri('/models'),
faceapi.nets.faceExpressionNet.loadFromUri('/models')
```
### Making predictions

The neural nets accept HTML image, canvas or video elements or tensors as inputs.

```javascript
const detections = await faceapi.detectAllFaces(video, new faceapi.TinyFaceDetectorOptions()).withFaceLandmarks().withFaceExpressions()
```

### Displaying Detection Results

Preparing the overlay canvas:

```javascript
const canvas = faceapi.createCanvas(video)
    document.body.append(canvas)
    const displaySize = {width: video.width, height: video.height}
    faceapi.matchDimensions(canvas, displaySize)
```

face-api.js predefines some highlevel drawing functions, which you can utilize:

```javascript
faceapi.draw.drawDetections(canvas, resizedDetections)
faceapi.draw.drawFaceLandmarks(canvas, resizedDetections)
faceapi.draw.drawFaceExpressions(canvas, resizedDetections)
```

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

## Try it







# References

- https://www.kirupa.com/html5/accessing_your_webcam_in_html5.htm
- Some error handling: https://stackoverflow.com/questions/27120757/failed-to-execute-createobjecturl-on-url
- https://itnext.io/face-api-js-javascript-api-for-face-recognition-in-the-browser-with-tensorflow-js-bcc2a6c4cf07
- https://developers.google.com/ml-kit/vision/face-detection?hl=en

