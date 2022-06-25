# Object Detection

## Overview



A detection describes the object's position at one moment in time with a set of skeletal landmark points. Type of object depends on the model and dataset used to train it.



![Demo](https://media.giphy.com/media/7MTXnXM6UuTRXU0HAj/giphy.gif "Demo")







## Install SDK

We provide packages for iOS and Android



### iOS



To install latest sdk version add following line in your Podfile :



```ruby
pod 'EdgeStoreEngine'

```


#### Android



Edit your app gradle file and add the following lines:



```groovy
dependencies {
    // other project dependencies ...
    implementation "ai.edgestore:engine:0.1.0-alpha01"
}


```


### Running the Model



Download the [Efficient Detector](https://store.edgestore.ai/home/model?model=efficientdet-d4-lite) or you can use  any object detection model from the [EdgeStore](store.edgestore.ai).



Place the **edgem** file in your project or assets and run the following code:



#### Swift



```swift
let modelPath:String = Bundle.main.path(forResource: "efficientdet-d4-lite", ofType: "edgem")!
let model =  try EdgeModel(modelPath: modelPath)

// Preparing data for model
let image = // your input image

// Run the model on prepared inputs and get recognitions as output
let recognitions: Recognitions = try model.run(inputs:[image])

// Draw the recognitions on your Image or UI
let drawing = recognitions.draw(image:image)

// Use the recognitions in your app:
for detection in recognitions {
	let id = detection.id!
	let displayName = detection.displayName!
	let confidence = detection.confidence!
	let location = detection.location!
	let colorLegend = detection.colorLegend!
}

```


#### Kotlin



```kotlin
val model = EdgeModel.fromAsset(context, "efficientdet-d4-lite.edgem")

// Preparing data for model
val image = // your input image

// Run the model on prepared inputs and get recognitions as output
val recognitions: Recognitions = model.run(listOf(image))

// Draw the recognitions on your Image or UI
val drawing: Bitmap = recognitions.drawOnBitmap(image)

// Use the recognitions in your app:
for (detection in recognitions) {
	val id = detection.id!!
	val displayName = detection.displayName!!
	val confidence = detection.confidence!!
	val location = detection.location!!
	val colorLegend = detection.colorLegend!!
}

```


#### Java



```java
EdgeModel model = EdgeModel.fromAsset(context, "efficientdet-d4-lite.edgem");

// Preparing data for model
Bitmap image = // your input image

// Run the model on prepared inputs and get recognitions as output
Recognitions recognitions = model.run(Arrays.asList(image));

// Draw the recognitions on your Image or UI
Bitmap drawing = recognitions.drawOnBitmap(image);

// Use the recognitions in your app:
for (Recognition detection : recognitions) {
	var id = detection.id;
	var displayName = detection.displayName;
	var confidence = detection.confidence;
	var location = detection.location;
	var colorLegend = detection.colorLegend;
}

```




We support following input formats for images:

- CvPixelBuffer  (Recommended, iOS)

- UIImage (Slow, iOS)

- Bitmap (Android)









### Interpreting and Using Results



The output *results* variable has type Recognitions that is a type of List. All the useful

information the model provides is compacted into this variable. Not matter your use case the output of model will always be Recognitions object. You can use location (inside the for loop) data to get coordinates of each object.

### Coordinate System



Our coordinate system is same as used in UIKit. i.e.  our origin is at top left corner of image, with x axis to the right and y axis to the bottom.

![Coordinates Demo](https://files.seeedstudio.com/wiki/Wio-Terminal/img/grids.jpg)



The recognitions object returned after model execution provides coordinates in image domain. i.e. 0 < x < Input Image Width and 0 < y < Input Image Height. 



You can directly apply transformations on Recognitions object by using the Recognitions.map function in Android or Recogntitions.applying in iOS