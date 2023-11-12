# Computer Vision in SvelteKit

This web application integrates computer vision using TensorFlow.js into the SvelteKit framework. The project showcases two machine learning features: **Image Classification** and **Real-Time Object Detection**.

- **Image Classification:** Utilizing a pre-trained MobileNet model, the application accurately classifies images, recognizing a wide range of objects.

- **Real-Time Object Detection:** The app leverages the COCO-SSD (Common Objects in Context - Single Shot MultiBox Detector) model to detect and locate objects in a live webcam stream, overlaying bounding boxes for an interactive experience.

## Demo

Explore the live demo: [Computer Vision in SvelteKit](https://cv-sveltekit-app.vercel.app/)

![Screenshot 2023-10-05 204008](https://github.com/fadhlakmal/tfjs-sveltekit-app/assets/120249194/50133cfb-5418-41b5-8afb-84c90ecc7e33)
![Screenshot 2023-10-05 204203](https://github.com/fadhlakmal/tfjs-sveltekit-app/assets/120249194/f69fac84-ad54-415a-8e62-8a75900061cc)

## Technologies Used

- **SvelteKit:** A framework providing a fast and efficient development environment for modern web applications.

- **TensorFlow.js:** Empowering machine learning capabilities directly within web browsers.

- **MobileNet:** The [MobileNet](https://www.npmjs.com/package/@tensorflow-models/mobilenet) model excels at real-time image classification.

- **COCO-SSD:** For real-time object detection, the [COCO-SSD](https://www.npmjs.com/package/@tensorflow-models/coco-ssd) model identifies various objects within images and video streams.

## Getting Started

1. **Installation:** Clone this repository to your local machine and install the necessary dependencies:

   ```bash
   git clone https://github.com/fadhlakmal/tfjs-sveltekit-app.git
   cd tfjs-sveltekit-app
   npm install

   ```
2. **Run and Explore the App:** Start the SvelteKit development server to run the app locally:
   ```bash
   npm run dev -- --open
   ```

## Usage
Feel free to modify and expand upon this project to suit your specific needs.

## Contributing
Contributions to this project are welcome! If you have ideas for improvements or new features, feel free to reach out through GitHub issues or by [email](mailto:akmal.madany1@gmail.com).

## License
This project is licensed under the MIT License.
