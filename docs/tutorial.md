Here is a Google Doc with screenshots included: https://docs.google.com/document/d/1MMeoEpYuQkLMnbxZE6RehQQHry-196USr4hQUBv4D-I/edit#. 

# Overview

The topic of this project is ARCore on the Android platform. The goal of the project was to build an entertainment app inspired by Tamagotchis. Using ARCore, a 3d model can be placed on a surface and react to the user’s actions. To support ARCore, Sceneform is extremely helpful as it provides a variety of functions, such as model rendering and animations. The app supports simple actions, which are limited to changing the models, moving the models, and animating the models to dance. The user can choose between two models - a halloween-themed pumpkin man and a tiger - and the user can also choose to animate the models. Currently, the app is constrained to only allow two models to be present at the same time, but there is an easy way to allow for more models in one session. 

ARCore supports augmented reality features for Android Studio. Using only ARCore, developers can create apps to place virtual 3D models into the physical world; however, there are other platform features that support and enhance ARCore, notably Sceneform. Sceneform supports ARCore through animations and allows new developers to bypass OpenGL for Android, which can be challenging to learn. 

The base code for this app was provided by Scene View Open Community on Github (https://github.com/SceneView). Sceneform SDK for android has been archived with version 1.16.0. The Scene View Open Community is maintaining support for Sceneform until Google releases an alternative. While the changes made to the initial repository are in Kotlin, many of the original files in Sceneform were written in Java. 

# Getting Started
IDE Version
Android Studio 2020.3.1 (or updated)
Minimum SDK
27 - Android 8.1 (Oreo)
Dependencies 
com.google.ar:core:1.28.0


When running the project using an Android device, developers may need to disable antivirus software as it can affect Android Studio’s exports. An example is MalwareBytes, which blocks Android Studio from installing the app on an Android device. 

# Coding Instructions
The app can be run using either the Android Studio emulator with an AR compatible phone, or with an AR compatible Android. A complete list of compatible Android devices can be found at https://developers.google.com/ar/devices.

Using the forked Sceneform repository, download the core, sceneform, and ux folders as they are necessary to support ARCore. 
To run this project, clone https://github.com/daviswygvsu/CIS357ARCore. Open in Android Studio, and using a compatible device, run the project from the samples/ar-model-viewer folder.

# Core
## Google
This folder contains the original sceneform folders and files that were archived by Google. The original sceneform supported animations, collisions, rendering, and placement. The classes and helper classes within the folder were written in Java, not Kotlin.

## Gorisse.thomas.sceneform

The core folder contains the maintained support for sceneform. In the ‘gorisse.thomas.sceneform’ folder, the dependencies for environment, light, materials, scenes, and ARCore are maintained together. This folder contains the files that allow the virtual models to interact with the real world by recognizing the environment and helping to anchor them.  

# Sceneform
This folder contains a Manifest file listing the project package and a BuildConfig file listing the project package again. This folder is necessary as it contains a global debugger and allows access to the other files supporting sceneform. 

# Ux 
The “ux” folder for this project contains some unnecessary components as it was also meant to support other features of ARCore such as face recognition and rendering. The focus for this project was 3D models. The ux folder contains relevant support for ArFragments, Gestures, and Nodes for placement. The features in this folder help make it intuitive for users to place objects by providing them with a grid where they can place models.

# Application Files
## Manifest
The Manifest file has the default activities and intents, but it also includes some features and permissions which are essential for ARCore. The following permissions are for using the camera to place objects and recognize the surroundings, the Internet to load models, and for OpenGLES to function properly.
[Manifest Screenshot](https://ibb.co/9gTbPd3)

## Activity
The Activity file contains the opening scene which gives ARCore time to load the actual surroundings and start recognizing surfaces. Its main purpose is to serve as a buffer. 

## MainFragment
The MainFragment file holds the actionable code. It holds the renderers, the view, and the activity. 
These are the imports for the MainFragment file. Except for ‘com.google.ar.core.Anchor,’ all of the imports were included in the supported Scene View repository. 
[Main Fragment Code](https://ibb.co/h98Yxx9)


## Private variables
[Code Screenshot](https://ibb.co/gwkPyxy)

### OnViewCreated()
[Code Screenshot](https://ibb.co/fNjWpfK)


In the OnViewCreated method, the main fragment, arFragment, is created. This fragment contains the scene where the models will be placed. Two buttons, newModel and animateModel, are also declared and referenced in the code. These buttons use two private methods that will allow the user to switch the models being used or to animate the models. 
### Actions
[Code Screenshot](https://ibb.co/KK55Ktv)

These two functions are used to switch the models and animate them.
### loadModels()
[Code Screenshot](https://ibb.co/cQVpJQz)


This loadModels() function creates two models - the tiger model and the halloween model. After creating the models, the function sets the modelInUse variable as model1 to be used outside of the function. To add a different model, create a var of type Renderable? outside of the function and then set it equal to ModelRenderable.builder(). Set the model source by either entering the hyperlink where the glb file can be downloaded or by calling it from the models file. 
### onTapPlane()
[Code Screenshot](https://ibb.co/4P4sgWh)

Inside of the last function, onTapPlane, the function checks to make sure that a model is available and that it has been rendered successfully. Once the function finds the model and view, an anchor is created and that anchor is added to a list of anchors. Anchors are used to ground the models so that they are in a fixed location. The Anchor class had to be imported into the repository in order to access the list of anchors, which was necessary to limit the amount of anchors. 

Inside the first addChild method, an anchor is created and then an if statement is used to limit the number of anchors, and models by extension. If the limit is exceeded, then a least recently used (LRU) policy is enacted by removing the oldest instance of an anchor. Inside the second addChild method, the model is set using the Renderable class. Afterwards, a relative position is created for the model. Because of this position, when the user walks closer or farther away from the model, the model will have a corresponding adjustment in size. 

### Models
Sceneform will support animations with .gfb, .gltf, and .fbk extensions. In this project, .gfb files were used. The first .gfb was imported into the project, while the second was loaded from Google’s apis. If the .gfb file is corrupted, then it will not render correctly when it is loaded into the app. Online converters, such as AnyConv.com, will not accurately convert OBJ files to .gfb.
The models folder should hold all of the .gfb files used in the project. These files are then referenced and used in the MainActivity code when loading models.

### Further Discussion/Conclusions
While we based our app on a forked repository, another approach to AR for Android is OpenGL. Based on our research, OpenGL is harder to implement than Sceneform as it is less intuitive for new developers. It seems that OpenGL would include more customization than was included in this project. Given more time, we would have liked to have been able to compare OpenGL functionality with the archived Sceneform.

Earlier in the project, we were focusing on implementing InstantPlacement and the Depth API. However, the phone we used to test the app, a motorola z4, was not capable of running the Depth API. While InstantPlacement was useful in our first iteration, we did not include it as the Anchor class and subclasses were sufficient for our demo app. 

To run our project, a user can clone our repository and run it using an Android compatible phone. This tutorial was based on the Scene View repository and the major changes we made included (1) adding new models, (2) disabling and enabling animations, (3) limiting the amount of models in a session, and (4) allowing the user to switch between models in the same session. 

While we are currently not planning on updating our project, our completed project can be found at https://github.com/daviswygvsu/CIS357ARCore.
