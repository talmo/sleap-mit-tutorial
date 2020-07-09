# SLEAP MIT Tutorial
This repository contains slides, code and data for the [tutorial on SLEAP at MIT on July 9, 2020](http://bcs.mit.edu/news-events/events/computational-tutorial-decoding-animal-behavior-through-pose-tracking).

![SLEAP Estimates Animal Poses](https://sleap.ai/docs/_static/sleap_movie.gif)

SLEAP is a deep learning-based framework for multi-animal pose tracking. Check out installation instructions, guides and documentation at **[sleap.ai](https://sleap.ai)** or the code at **[github.com/murthylab/sleap](https://github.com/murthylab/sleap)**.


## Talk: *"Computational Tutorial: Decoding Animal Behavior Through Pose Tracking"*
**Slides:** TBA

**Recording:** TBA

**Description:** Behavioral quantification, the problem of measuring and describing how an animal interacts with the world, has been gaining increasing attention across disciplines as new computational methods emerge to automate this task and increase the expressiveness of these descriptions. In neuroscience, understanding behavior is crucial to interpretation of the structure and function of biological neural circuits, but tools to measure what an animal is doing has lagged behind the ability to record and manipulate neural activity.

In order to get a handle on how neural computations enable animals to produce complex behaviors, we turn to pose tracking in high speed videography as a means of measuring how the brain controls the body. By quantifying movement patterns of the humble fruit fly, we demonstrate how advances in computer vision and deep learning can be leveraged to describe the "body language" of freely moving animals. We further demonstrate that these techniques can be applied to a diverse range of animals, ranging from bees and flies to mice and giraffes.

This talk will describe our work in generalizing deep learning-based methods developed for human pose estimation to the domain of animals. We tackle the problems of learning with few labeled examples, dataset-tailored neural network architecture design, and multi-instance pose tracking to build a general-purpose framework for studying animal behavior. Finally, we'll explore how postural dynamics can be used in unsupervised action recognition to create interpretable descriptions of unconstrained behavior.

## Tutorial

### Training models

In this part of the tutorial, we'll walk through how to train a set of neural networks
to track the pose of a pair of interacting fruit flies.

We'll start with our pre-labeled dataset and train the models on Google Colab right in
the browser.

There are [two major approaches](https://sleap.ai/index.html#getting-started-with-sleap)
to multi-instance pose tracking. For this tutorial, we'll be using the **top-down**
approach.

First, we will train a model to predict the centroids of each animal. This neural
network will take low resolution full frame images as input and predict the location of
an anchor point (the fly's thorax).

Follow along in this notebook to train the centroid model:
<a href="https://colab.research.google.com/github/talmo/sleap-mit-tutorial/blob/master/notebooks/Interactive_training_(centroids).ipynb" target="_blank">SLEAP - Interactive training (centroids) <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>

The final predictions will look something like this:
![Centroids model predictions](https://github.com/talmo/sleap-mit-tutorial/blob/master/media/predictions-centroids.png)

Great! Next, we'll train our top-down part detection model. This neural network will
take a cropped image of an animal centered aroudn the detected centroid from the
previous model.

Follow along in this notebook to train the part detection model:
<a href="https://colab.research.google.com/github/talmo/sleap-mit-tutorial/blob/master/notebooks/Interactive_training_(topdown).ipynb" target="_blank">SLEAP - Interactive training (topdown) <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>

The final predictions will look something like this:
![Topdown model predictions](https://github.com/talmo/sleap-mit-tutorial/blob/master/media/predictions-topdown.png)

If you've run through both of these, you should have downloaded two zip files containing
the result of the training. A copy of these data can be found below.

We'll use these models in the next step to track new data from an example clip.


**Running locally:**
If you have SLEAP installed on a GPU machine, you can just clone this repository and run
the notebooks locally. To download and extract the training data:
```
curl -L --output data.zip https://www.dropbox.com/s/acnjw8amit0zmyb/data.zip?dl=1
unzip -o data.zip  # Linux
tar -xvf data.zip  # Windows
```

*Note:* You might need to adjust some paths in the notebooks depending on where your download
the files.


### Predicting on new data

Once you have trained models you will be able to predict pose tracks on new data.

To do this, we'll again use Google Colab, so follow along in this notebook:

<a href="https://colab.research.google.com/github/talmo/sleap-mit-tutorial/blob/master/notebooks/Interactive_inference.ipynb" target="_blank">SLEAP - Interactive inference <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>

Great! The result should be a `predictions.slp` file containing the SLEAP project data
that includes the predictions.

A copy of this file is included in this repository.


**Running locally:**
If you have SLEAP installed on a GPU machine, you can just clone this repository and run
the notebooks locally. To download and extract the pretrained models:
```
curl -L --output models.zip https://www.dropbox.com/s/vi13zysbzpq9c19/models.zip?dl=1
unzip -o models.zip  # Linux
tar -xvf models.zip  # Windows
```

The test video clip is already included in this repository.


### Visualizing predictions locally

Now that we have the predictions, we can load them up into our SLEAP GUI to inspect them
and proofread the tracking if needed.

*Note:* If you have not gone through the previous steps, you can just use the saved predictions
that are included here. Download or clone this repository and you'll find the results in
the `prediction` subfolder.


First, we'll install SLEAP locally. You'll need [Miniconda](https://docs.conda.io/en/latest/miniconda.html)
installed first.

Once that is ready, open up a command terminal and install SLEAP in a new environment:
```
conda install -c sleap -n sleap python=3.6 sleap=1.0.6
```

You do not need a computer with a GPU in order to install SLEAP. We recommend doing this
so that you can work with your data after the heavy lifting (training and inference) has
already been run remotely.

If you're having a hard time installing SLEAP, see the installation instructions on [our
website](https://sleap.ai/guides/installation.html).


Once you have SLEAP installed, activate the environment and open the predictions file:
```
conda activate sleap
sleap-label prediction/predictions.slp
```

You should now see the predictions in the GUI:

![SLEAP Label GUI](https://github.com/talmo/sleap-mit-tutorial/blob/master/media/sleap-label-gui.png)

Scroll around the clip, zoom in and inspect the results.