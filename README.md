## LOLd.js - Multimodal Lightweight Online Laughter Detection

A JavaScript API for multimodal laughter recognition via audiovisual signals.

It uses two different models:

- For video data the model used is [face-api.js](https://github.com/justadudewhohacks/face-api.js/)
- For audio data the model used is a custom one. Link will be available soon.

### Example usage

Using a live webcam feed

```
// "isOfflineVideoVersion" will predict from a video element
// which loaded an offline video as opposed to live webcam feed.

// Live webcam feed
let audioStream: MediaStream | null = null;

// Access browser's audio & video
navigator.mediaDevices
  .getUserMedia({ video: {}, audio: {} })
  .then((stream) => {
    if (!isOfflineVideoVersion) {
      videoEl.srcObject = stream;
    }
    audioStream = stream;
    return new Promise((resolve) => (videoEl.onplay = resolve));
  })
  .then(async (_loadEvent) => {
    // Create an instance of Lold.js!
    const lold = new Lold(videoEl, audioStream!, {
      predictionMode: "multimodal", // Can also access "audio" or "video" single modality
      videoSourceType: isOfflineVideoVersion ? "video" : "webcam",
    });

    // Load all models and required (from face-api.js and lold.js audio model)
    await lold.loadModels();

    // Start predicting. Predictions run in the background and can be
    // accessed with getters (e.g. getMultimodalPrediction)
    lold.startMultimodalPrediction();

    // Get the predictions
    let [audioConfidence, videoConfidence] = lold.getMultimodalPrediction();
    console.log(audioConfidence, videoConfidence);
    // Output: 0.524245215, 0.852215253

```
