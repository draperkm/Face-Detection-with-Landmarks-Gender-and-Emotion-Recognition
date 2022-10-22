# Face Detection with Landmarks and Emotion Recognition

I aimed at implementing JavaScript's API `face-api.js`, as a web application. This API is able to `Detect` (ability to distinguish an object from the background) human faces. It is also able to do `Recognition` (ability to classify the object class such as face landmarks, human emotions, gender and age.

https://user-images.githubusercontent.com/80494835/197230358-48047392-e3ea-4073-b3f9-22c2d3b53646.mp4



## Some challenges

### Accessing Your Webcam in HTML

https://www.kirupa.com/html5/accessing_your_webcam_in_html5.htm

We can communicate with our webcam and access its video stream from our browser with just some JavaScript code. We only need a browser that supports the `getUserMedia` function.

There are two components that do all the heavy lifting in getting data from your webcam displayed on your screen. They are the HTML video element and the JavaScript getUserMedia function:
 
 The video element is pretty straightforward in what it does. It is responsible for taking the video stream from your webcam and actually displaying it on the screen.

The interesting piece is the getUserMedia function. This function allows you to do three things:

Specify whether you want to get video data from the webcam, audio data from a microphone, or both.
If the user grants permission to access the webcam, specify a success function to call where you can process the webcam data further.
If the user does not grant permission to access the webcam or your webcam runs into some other kind of error, specify a error function to handle the error conditions.

For what we are trying to do, we call the getUserMedia function and tell it to only retrieve the video from the webcam.

I will cover the microphone in the future! Once we retrieve the video, we tell our success function to send the video data to our video element for display on our screen.

Adding the code:

In this section, let's go ahead and display our webcam data to the screen. First, let's add the HTML and CSS:

```
```

In a new document, go ahead and add all of the HTML and CSS that you see above. The important thing to note in this snippet is the video tag:

```
```

Our video tag has an id value of videoElement, and its autoplay attribute is set to true. By setting the autoplay attribute to true, we ensure that our video starts to display automatically once we have our webcam video stream.

Yes, this looks pretty plain and boring now. That is because we haven't added the JavaScript that ties together our video element with your webcam. We'll do that next!

Inside your script tag, add the following code:

```
```

Once you've added this code, save your HTML document and preview your page. Provided you are on a supported browser, you should see your webcam video stream after you've given your browser permission to access it.

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



