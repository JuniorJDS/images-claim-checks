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

1. The Client uploads the image to the system through the **image-receiver-api**;
2. The **image-receiver-api** conducts essential validations and proceeds to store the uploaded image;
3. The **image-receiver-api** publishes an event containing the image metadata and URL at the topic **uploaded-image**;
4. The Aplications **gen-thumbnail**, **resize-mobile** and **resize-web** subscribe to the event **uploaded-image**;
5. Aplications **gen-thumbnail**, **resize-mobile** and **resize-web** search for the image based on its URL;
6. Asynchronous processing begins for each consumer:

   - **gen-thumbnail** - generates a thumbnail version of the image;
   - **resize-mobile** - resizes the image for optimal display on mobile device;
   - **resize-web** - resizes the image for optimal display on the web.

7. Each aplication uploads the resulting processed image to the designated bucket.
