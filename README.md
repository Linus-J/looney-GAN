
## looney-GAN

### General description
This is a repository for a GAN model that generates cartoon backgrounds similar to those produced by the [animators](https://looneytunes.fandom.com/wiki/Category:Cartoons_by_background_artist) of [Looney Tunes (1930-69)](https://en.wikipedia.org/wiki/Looney_Tunes).

The model was trained using the open-source [stylegan3](https://github.com/NVlabs/stylegan3) network infrastructure provided by [NVIDIA](https://github.com/NVlabs). [(Paper here)](https://nvlabs-fi-cdn.nvidia.com/stylegan3/stylegan3-paper.pdf).
### Early training results.

![Original](https://github.com/Linus-J/repo-images/blob/main/looney-GAN/early.png)

### Intermediary training results.

![Original](https://github.com/Linus-J/repo-images/blob/main/looney-GAN/intermediary.png)

### Fake image generation improvement during training video.

![Video](https://raw.githubusercontent.com/Linus-J/repo-images/main/looney-GAN/fakes.mp4)

### Dataset description
The dataset used to train the model was curated using source images collected from [@looneytunesbackgrounds](https://www.instagram.com/looneytunesbackgrounds/), [animation.backgrounds](https://www.instagram.com/animation.backgrounds/) and [@cartoonarchitecture](https://www.instagram.com/cartoonarchitecture/) on Instagram. 

Black and white images, as well as unsuitable images such as those with animals/humans in the picture or real-life sketches, were excluded from the dataset.

All images were processed using [ImageMagick](https://imagemagick.org/)  to batch normalise all images to be 256x256 centred PNGs, where non-square images were scaled to remove black bars whilst maintaining their source aspect ratios. Using higher resolution images such as 1024x1024 requires significantly more training time and resources.

After the data was normalised, the dataset was mirrored on the x-axis to double the amount of data used to train and test the network resulting in a set of 3634 images.

#### Commands to normalise dataset:
Whilst within the directory containing all the images:

##### Batch convert all JPGs to PNGs:
`mogrify -format png *.jpg`
##### Remove old JPG files:
`find -type f -name '*.jpg' -delete`
##### Batch resize new PNG to desired size and maintain source aspect ratios:
`mogrify -format png -resize '256x256^' -gravity center -extent 256x256`

### Generator installation
#### Linux:
Clone both this and the [stylegan3](https://github.com/NVlabs/stylegan3) repository to the same directory.
##### Download:

`git clone https://github.com/Linus-J/looney-GAN.git`

`git clone https://github.com/NVlabs/stylegan3.git`

Since the stylegan3 repo requires an older version of PyTorch, I would recommend working in a new [python virtual environment](https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/) before installing the requirements.
##### Set up python virtual environment:
`python3 -m pip install --user --upgrade pip`

`python3 -m pip install --user virtualenv`

`python3 -m venv env`

`source env/bin/activate`

##### Install prerequisite python packages:
`pip3 install -r looney-GAN/requirements.txt`

##### Download the trained model:
`cd looney-GAN`

`gdown https://drive.google.com/u/0/uc?id=1Yzmygxt6XxZducs7VSRHND3LT7TuTIO_&export=download`

`cd ..`

##### (Optional) Download the dataset (512x512):
`gdown https://drive.google.com/u/0/uc?id=1m3WhwDsxmHPxigVqWmSFz-V7hOsAdttH&export=download`

##### Generate images:
This example will generate 4 images which can be viewed in the created `out` directory. 

`python stylegan3/gen_images.py --outdir=out --trunc=1 --seeds=85,265,297,849 --network=./looney-GAN/final.pkl`
    
The `--outdir` is a required option is used to determine the output directory, `--trunc` is an optional argument that determines the variability of the generated images where 1 is untruncated and values towards zero result in full truncation, `--seeds` is an optional argument to seed singular or multiple image generations and `--network` is a required option that points towards the pre-trained final.pkl model.

Edit these options to generate your desried backgrounds.
