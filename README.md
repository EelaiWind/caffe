# Caffe

[![Build Status](https://travis-ci.org/BVLC/caffe.svg?branch=master)](https://travis-ci.org/BVLC/caffe)
[![License](https://img.shields.io/badge/license-BSD-blue.svg)](LICENSE)

Caffe is a deep learning framework made with expression, speed, and modularity in mind.
It is developed by the Berkeley Vision and Learning Center ([BVLC](http://bvlc.eecs.berkeley.edu)) and community contributors.

Check out the [project site](http://caffe.berkeleyvision.org) for all the details like

- [DIY Deep Learning for Vision with Caffe](https://docs.google.com/presentation/d/1UeKXVgRvvxg9OUdh_UiC5G71UMscNPlvArsWER41PsU/edit#slide=id.p)
- [Tutorial Documentation](http://caffe.berkeleyvision.org/tutorial/)
- [BVLC reference models](http://caffe.berkeleyvision.org/model_zoo.html) and the [community model zoo](https://github.com/BVLC/caffe/wiki/Model-Zoo)
- [Installation instructions](http://caffe.berkeleyvision.org/installation.html)

and step-by-step examples.

[![Join the chat at https://gitter.im/BVLC/caffe](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/BVLC/caffe?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Please join the [caffe-users group](https://groups.google.com/forum/#!forum/caffe-users) or [gitter chat](https://gitter.im/BVLC/caffe) to ask questions and talk about methods and models.
Framework development discussions and thorough bug reports are collected on [Issues](https://github.com/BVLC/caffe/issues).

Happy brewing!

## License and Citation

Caffe is released under the [BSD 2-Clause license](https://github.com/BVLC/caffe/blob/master/LICENSE).
The BVLC reference models are released for unrestricted use.

Please cite Caffe in your publications if it helps your research:

    @article{jia2014caffe,
      Author = {Jia, Yangqing and Shelhamer, Evan and Donahue, Jeff and Karayev, Sergey and Long, Jonathan and Girshick, Ross and Guadarrama, Sergio and Darrell, Trevor},
      Journal = {arXiv preprint arXiv:1408.5093},
      Title = {Caffe: Convolutional Architecture for Fast Feature Embedding},
      Year = {2014}
    }

# Caffe Augmentation Extension
This is a recent caffe version (2016/05/25, 4bf4b18607) with additional transformation options in ImageData layer. The following transformations have been added to support:

* min_side - resize and crop preserving aspect ratio, default 0 (disabled);
* max_rotation_angle - max angle for an image rotation, default 0;
* contrast_brightness_adjustment - enable/disable contrast adjustment, default false;
* smooth_filtering - enable/disable smooth filterion, default false;
* min_contrast - min contrast multiplier (min [alpha](http://docs.opencv.org/2.4/doc/tutorials/core/basic_linear_transform/basic_linear_transform.html)), default 0.8;
* max_contrast - min contrast multiplier (max [alpha](http://docs.opencv.org/2.4/doc/tutorials/core/basic_linear_transform/basic_linear_transform.html)), default 1.2;
* max_brightness_shift - max brightness shift in positive and negative directions ([beta](http://docs.opencv.org/2.4/doc/tutorials/core/basic_linear_transform/basic_linear_transform.html)), default 5;
* max_smooth - max smooth multiplier, default 6;
* apply_probability - how often every transformation should be applied, default 0.5;
* debug_params - enable/disable printing tranformation parameters, default false;

## How to use
You could specify your network prototxt as:

    layer {
    name: "data"
    type: "ImageData"
    top: "data"
    top: "label"
    include {
      phase: TRAIN
    }
    transform_param {
        mirror: false
        contrast_brightness_adjustment: true
        smooth_filtering: true
        max_rotation_angle: 10
        min_side: 256
        crop_size: 224
        mean_file: "/home/your/imagenet_mean.binaryproto"
        min_contrast: 0.8
        max_contrast: 1.2
        max_smooth: 6
        apply_probability: 0.5
        debug_params: false
    }
    image_data_param {
      source: "/home/your/image/list.txt"
      batch_size: 32
      shuffle: true
    }
    }


## Setup caffe-augmentation
There are two options:

1. Pull and run docker container:

    ```$ docker pull kostyaev/caffe-gpu```

    ```$ nvidia-docker run -it kostyaev/caffe-gpu /bin/bash```

2. Install from source code:
Clone this repo, adjust Makefile.config and simply run the following commands:

    ```$ make all -j8```

    ```$ make test -j8```

    ```$ make runtest -j8```

For a faster build, compile in parallel by doing `make all -j8` where 8 is the number of parallel threads for compilation (a good choice for the number of threads is the number of cores in your machine).


## Acknowledgment
This project is based upon
[@kevinlin311tw](https://github.com/kevinlin311tw)'s [caffe-augmentation](https://github.com/kevinlin311tw/caffe-augmentation),
[@ChenlongChen](https://github.com/ChenglongChen)'s [caffe-windows](https://github.com/ChenglongChen/caffe-windows), [@ShaharKatz](https://github.com/ShaharKatz)'s [Caffe-Data-Augmentation](https://github.com/ShaharKatz/Caffe-Data-Augmentation), and [@senecaur](https://github.com/senecaur)'s [caffe-rta](https://github.com/senecaur/caffe-rta).
