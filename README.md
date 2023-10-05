# TensorFlow.js in SvelteKit

This web application showcases the integration of TensorFlow.js, a powerful JavaScript library for machine learning and deep learning, within the SvelteKit framework. With TensorFlow.js, I've implemented two machine learning features at the moment: **Image Classification** and **Real-Time Object Detection**.

- **Image Classification:** This application uses a pre-trained MobileNet model to classify images. It can recognize a wide range of objects and provide classification results with high accuracy.

- **Real-Time Object Detection:** Utilizing the COCO-SSD (Common Objects in Context - Single Shot MultiBox Detector) model, our app can detect and locate objects within a live video stream from your webcam. It overlays bounding boxes on detected objects for a visually engaging experience.

## Demo

Check out the live demo: [TensorFlow.js in SvelteKit](https://tfjs-sveltekit.vercel.app/)

![Screenshot 2023-10-05 204008](https://github.com/fadhlakmal/tfjs-sveltekit-app/assets/120249194/50133cfb-5418-41b5-8afb-84c90ecc7e33)
![Screenshot 2023-10-05 204203](https://github.com/fadhlakmal/tfjs-sveltekit-app/assets/120249194/f69fac84-ad54-415a-8e62-8a75900061cc)


## Technologies Used

- **SvelteKit:** This web application is built on top of the SvelteKit framework, which provides a fast and efficient development environment for building modern web applications.

- **TensorFlow.js:** Enable machine learning capabilities directly within your web browser using TensorFlow.js.

- **MobileNet:** The [MobileNet](https://www.npmjs.com/package/@tensorflow-models/mobilenet) model is used for image classification. It's a versatile, lightweight model that excels at real-time tasks.

- **COCO-SSD:** For real-time object detection, I've integrated the [COCO-SSD](https://www.npmjs.com/package/@tensorflow-models/coco-ssd) model. It can identify a wide range of objects within images and video streams.

## Getting Started

1. **Installation:** Clone this repository to your local machine and install the necessary dependencies:

   ```bash
   git clone https://github.com/fadhlakmal/tfjs-sveltekit-app.git
   cd tfjs-sveltekit-app
   npm install
   ```
2. **Run and Explore the App:** Start the SvelteKit development server and run the app locally:
   ```bash
   npm run dev -- --open
   ```

## Usage
Feel free to modify and expand upon this project to suit your specific needs. Customize the machine learning models, enhance the user interface, or add new features to create a unique web application that leverages the power of TensorFlow.js and SvelteKit.

## Contributing
Contributions to this project are welcome! If you have ideas for improvements or new features, feel free to reach out through GitHub issues or by [email](mailto:akmal.madany1@gmail.com).

## License
This project is licensed under the MIT License.
