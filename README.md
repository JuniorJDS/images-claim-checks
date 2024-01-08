# images-claim-checks

The goal of this project is to demonstrate an event-driven approach to image processing, where images are produced by one application and consumed by multiple applications asynchronously. The **Claim Check** pattern is employed to decouple the payload from the metadata, and the **Publish-Subscribe (Pub/Sub)** pattern is used to distribute events among the consumers.

## Components

- ### Producer

  The producer application is responsible for generating and uploading images to the event broker. It utilizes the Claim Check pattern to store image metadata separately from the actual image content.

- ### Consumers

  There are three consumer applications, each designed to perform a specific task on the processed images. These consumers subscribe to the "imagem-carregada" topic and retrieve the images for further processing.

- ### Event Broker

  The event broker handles the communication between the producer and consumers. It uses the "imagem-carregada" topic to publish image-loaded events, ensuring that consumers receive the necessary information to process the images.

- ### Blob storage
  Amazon S3 is used as a blob storage solution to store the actual image content. The Claim Check pattern allows metadata to reference the corresponding image content stored in S3.

## Workflow

![Event Architecture](/image-processing.png)

1. The producer generates an image and uploads it to the event broker.
2. The event broker publishes an "imagem-carregada" event.
3. Consumers 1, 2, and 3 receive the event and retrieve the corresponding image from Amazon S3.
4. Each consumer performs its specific task on the image asynchronously.
