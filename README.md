# Check Face

 > Putting a face to a hash

`Winner of Facebook Hack Melbourne 2019`


Facebook's [Hackathon](https://www.facebook.com/hackathon/) at [Facebook Hack Melbourne 2019](https://www.facebook.com/events/408587233300017/)

Who uses checksums? We all know we should.

A range of unused tools exist for verifying file integrity that suffer from poor adoption, are difficult to use and aren't human-friendly.
Humans are inherently good at remembering interesting information, be it stories, people and generally benefit from context. Most humans also have the ability to remember faces extremely well, with many of us experiencing false-positives or pareidolia - seeing faces as a part of inanimate objects.

With the advent of hyper-realistic Style transfer GAN's like [Nvidia's StyleGAN](https://github.com/NVlabs/stylegan), we can generate something that our brains believe is a real person, and make use of that human-hardware accelerated memorisation and let people compare between hashes they've seen, potentially even weeks apart, with only a few quick glances.


![CheckFace Face](/docs/assets/images/face.jpg)  
*This generated face is an example of what you could expect to see next to your file's checksum or your git commit sha.*

## How to use CheckFace

First, use the [Chrome Extension](https://chrome.google.com/webstore/detail/check-face/pbfneacmjmcjdbeonggmoanjpaklahfk) to generate the face for the hash in a web environment as is shown  
![Download Checkface](/docs/assets/images/screenshots/download.gif)

Once downloaded, verify the CheckFace by using the [Context-Menu Extension](https://github.com/check-face/checkface/releases) to generate another checkface as shown below  
![Download Checkface](/docs/assets/images/screenshots/verify.gif)

You should already know if they're the same! EASY

## Our Stack
   - [Nvidia StyleGAN](https://stylegan.xyz/code)
     - Tensorflow
   - Docker
     - Nvidia Docker runtime
   - Flask
   - GitHub Pages
   - Chrome Web Extension
   - Winforms Application
   - CloudFlare


## Quickstart

 - **Chrome Extension** Context Menu
 - **Electron App** Context Menu
 - **Backend API** running a Dockerized Nvidia Stylegan on Flask
 - **Project Webpage**

### Chrome Extension

The `/src/extension` directory holding the manifest file can be added as an extension in developer mode in its current state.

Open the Extension Management page by navigating to [chrome://extensions](chrome://extensions).
The Extension Management page can also be opened by clicking on the Chrome menu, hovering over More Tools then selecting Extensions.
Enable Developer Mode by clicking the toggle switch next to Developer mode.
Click the LOAD UNPACKED button and select the extension directory.

![How to load extension in chrome with developer mode](https://developer.chrome.com/static/images/get_started/load_extension.png)


#### Load Extension

Ta-da! The extension has been successfully installed. Because no icons were included in the manifest, a generic toolbar icon will be created for the extension.

(Sourced: [Chrome Developer](https://developer.chrome.com/extensions/getstarted))

### Windows Explorer File Context Menu

Download and install the latest release.
Right click any file and choose from a number of hash algorithms to see its checkface.
We recommend using SHA256.

![using explorer file context menu](/docs/assets/images/screenshots/explorer-context-menu.jpg)
![windows desktop app](/docs/assets/images/screenshots/checkface-dotnet-example.jpg)

### Electron App

Build and run from source only at the moment.

### Backend API

Request images at [api.checkface.ml/api/face?value=example&dim=300](https://api.checkface.ml/api/face?value=example&dim=300).

Prerequisites to run the backend server

  - GPU with sufficient VRAM to hold the model
  - Nvidia Docker runtime (only supported on Linux, until HyperV adds GPU passthrough support)

For running a backend we have used an AWS p3 instance on ECS, or g3s.xlarge via docker-machine for testing.

### Project Webpage

Simple pure Javascript based bootstrap webpage. Upload to anything that serves static files

## Development

### Chrome Extension

TODO

### Windows Desktop Application

Open `src/dotnet-windows/checkface-dotnet.sln` in Visual Studio.

To use as explorer shell extension, you will need to [sign the assembly](https://docs.microsoft.com/en-us/visualstudio/ide/managing-assembly-and-manifest-signing?view=vs-2019#how-to-sign-an-assembly-in-visual-studio).

Use [SharpShell ServerManager](https://github.com/dwmkerr/sharpshell/releases) to load the project output `checkface-dotnet.dll` in a test shell.

### Electron App

```console
cd ./src/electron
yarn install
yarn run dev ./README.md
```

Build installer using

```console
yarn run build
```

Help needed to set up auto updating and registering in file context menu.

### Backend API

We rely on Nvlabs StyleGAN to run our inference, using the default model. First ensure you have 

  - [cuDNN](https://developer.nvidia.com/cudnn) 7.3.1
  - [CUDA toolkit 9.0](https://developer.nvidia.com/cuda-90-download-archive)
  - NVIDIA driver 391.35

Instructions for installing the 

Best practice is to first create a virtualenv, followed by installing the requirements

  1. Run `virtualenv venv` in the project directory
  2. Activate the venv `./venv/Scripts/activate.bat`
  3. Install the requirements `pip install -r requirements.txt`
  4. Run the backend with `python src/server/checkface.py`

#### System requirements

All you really need is a CUDA GPU with enough VRAM to load the inference model, which has been tested to work on a GTX 1080 with 8GB of VRAM, with NVIDIA driver 391.35.

## License

Our work is based on a combination of original content and work adapted from [Nvidia Labs StyleGAN](https://stylegan.xyz/code) under the [Creative Commons Attribution-NonCommercial 4.0 International License](https://creativecommons.org/licenses/by-nc/4.0/). Anything outside of the `src/server` dir is original work, and a diff can be used to show the use of the dnnlib and StyleGAN model inside of this directory.

The inference model was trained by Nvidia Labs on the FFHQ dataset, please refer to the [Flickr-Faces-HQ repository](http://stylegan.xyz/ffhq).
