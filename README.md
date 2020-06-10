# Pose Animator

Pose Animator takes a 2D vector illustration and animates its containing curves in real-time based on the recognition result from PoseNet and FaceMesh. It borrows the idea of skeleton-based animation from computer graphics and applies it to vector characters.

This is running in the browser in realtime using [TensorFlow.js](https://www.tensorflow.org/js). Check out more cool TF.js demos [here](https://www.tensorflow.org/js/demos).

*This is not an officially supported Google product.*

<img src="/resources/gifs/avatar-new-1.gif?raw=true" alt="cameraDemo" style="width: 250px;"/>

<img src="/resources/gifs/avatar-new-full-body.gif?raw=true" alt="cameraDemo" style="width: 250px;"/>

In skeletal animation a character is represented in two parts:
1. a surface used to draw the character, and 
1. a hierarchical set of interconnected bones used to animate the surface. 

In Pose Animator, the surface is defined by the 2D vector paths in the input SVG files. For the bone structure, Pose Animator provides a predefined rig (bone hierarchy) representation, designed based on the keypoints from PoseNet and FaceMesh. This bone structure’s initial pose is specified in the input SVG file, along with the character illustration, while the real time bone positions are updated by the recognition result from ML models.

<img src="https://firebasestorage.googleapis.com/v0/b/pose-animator-demo.appspot.com/o/ml-keypoints.png?alt=media" style="width:250px;"/>

<img src="/resources/gifs/avatar-new-bezier-1.gif?raw=true" alt="cameraDemo" style="width: 250px;"/>

// TODO: Add blog post link.
For more details on its technical design please check out this blog post.

### Demo 1: [Camera feed](https://pose-animator-demo.firebaseapp.com/camera.html)

The camera demo animates a 2D avatar in real-time from a webcam video stream.


### Demo 2: [Static image](https://pose-animator-demo.firebaseapp.com/static_image.html)

The static image demo shows the avatar positioned from a single image.


## Build And Run on Ubuntu

First install yarn:

```
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt update
sudo apt install yarn
```

then replace the whole content of  `package.json `with:

```
{
  "name": "tfjs-models",
  "version": "0.0.1",
  "description": "",
  "main": "index.js",
  "license": "Apache-2.0",
  "private": true,
  "engines": {
    "node": ">=8.9.0"
  },
  "dependencies": {
    "@tensorflow-models/facemesh": "^0.0.1",
    "@tensorflow-models/posenet": "^2.2.1",
    "@tensorflow/tfjs": "^1.7.0",
    "@tensorflow/tfjs-converter": "^1.7.0",
    "@tensorflow/tfjs-core": "^1.7.0",
    "face-api.js": "^0.22.1",
    "paper": "^0.12.1",    
    "stats.js": "^0.17.0"
  },
  "scripts": {
    "watch": "cross-env NODE_ENV=development parcel index.html --no-source-maps --no-hmr --open ",
    "build": "cross-env NODE_ENV=production parcel build index.html --no-source-maps --public-url ./",
    "build-camera": "cross-env NODE_ENV=production parcel build camera.html --public-url ./",
    "lint": "eslint .",
    "link-local": "yalc link"
  },
  "browser": {
    "crypto": false
  },
  "devDependencies": {
    "babel-core": "^6.26.3",
    "babel-plugin-transform-runtime": "~6.23.0",
    "babel-polyfill": "~6.26.0",
    "babel-preset-env": "~1.6.1",
    "babel-preset-es2017": "^6.24.1",
    "clang-format": "~1.2.2",
    "cross-env": "^5.2.0",
    "dat.gui": "^0.7.2",
    "eslint": "^4.19.1",
    "eslint-config-google": "^0.9.1",
    "parcel-bundler": "~1.12.4",
    "yalc": "~1.0.0-pre.27"
  },
  "eslintConfig": {
    "extends": "google",
    "rules": {
      "require-jsdoc": 0,
      "valid-jsdoc": 0
    },
    "env": {
      "es6": true
    },
    "parserOptions": {
      "ecmaVersion": 8,
      "sourceType": "module"
    }
  },
  "eslintIgnore": [
    "dist/"
  ]
}
```



then:

`yarn`

it will download and install some stuff.
when finished type:

`yarn watch`

your browser should pop-up and pose-animator is running.

## Build And Run

Install dependencies and prepare the build directory:

```sh
yarn
```

To watch files for changes, and launch a dev server:

```sh
yarn watch
```

## Platform support

Demos are supported on Desktop Chrome and iOS Safari.

It should also run on Chrome on Android and potentially more Android mobile browsers though support has not been tested yet.

# Animate your own design

1. Download the [sample skeleton SVG here](/resources/samples/skeleton.svg).
1. Create a new file in your vector graphics editor of choice. Copy the group named ‘skeleton’ from the above file into your working file. Note: 
	* Do not add, remove or rename the joints (circles) in this group. Pose Animator relies on these named paths to read the skeleton’s initial position. Missing joints will cause errors.
	* However you can move the joints around to embed them into your illustration. See step 4.
1. Create a new group and name it ‘illustration’, next to the ‘skeleton’ group. This is the group where you can put all the paths for your illustration.
    * Flatten all subgroups so that ‘illustration’ only contains path elements.
    * Composite paths are not supported at the moment.
    * The working file structure should look like this:
	```
        [Layer 1]
        |---- skeleton
        |---- illustration
              |---- path 1
              |---- path 2
              |---- path 3
	```
1. Embed the sample skeleton in ‘skeleton’ group into your illustration by moving the joints around.
1. Export the file as an SVG file.
1. Open [Pose Animator camera demo](https://pose-animator-demo.firebaseapp.com/camera.html). Once everything loads, drop your SVG file into the browser tab. You should be able to see it come to life :D
