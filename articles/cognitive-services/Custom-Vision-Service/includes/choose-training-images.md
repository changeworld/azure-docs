---
 author: PatrickFarley
 ms.service: cognitive-services
 ms.subservice: custom-vision
 ms.topic: include
 ms.date: 07/17/2019
 ms.author: pafarley
---

As a minimum, we recommend you use at least 30 images per tag in the initial training set. You'll also want to collect a few extra images to test your model once it's trained.

In order to train your model effectively, use images with visual variety. Select images that vary by:
* camera angle
* lighting
* background
* visual style
* individual/grouped subject(s)
* size
* type

Additionally, make sure all of your training images meet the following criteria:
* .jpg, .png, .bmp, or .gif format
* no greater than 6MB in size (4MB for prediction images)
* no less than 256 pixels on the shortest edge; any images shorter than this will be automatically scaled up by the Custom Vision Service

> [!NOTE]
> Do you need a broader set of images to complete your training? Trove, a Microsoft Garage project, allows you to collect and purchase sets of images for training purposes. Once you've collected your images, you can download them and then import them into your Custom Vision project in the usual way. Visit the [Trove page](https://www.microsoft.com/en-us/ai/trove?activetab=pivot1:primaryr3) to learn more.
