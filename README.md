# dtw-sofar

[![License](https://img.shields.io/github/license/egeozguroglu/dtw-sofar.svg)](https://github.com/egeozguroglu/dtw-sofar)
![GitHub issues](https://img.shields.io/github/issues/egeozguroglu/dtw-sofar) [![Build Status](https://github.com/egeozguroglu/dtw-sofar/workflows/Build%20Status/badge.svg?branch=main)](https://github.com/egeozguroglu/dtw-sofar/actions?query=workflow%3A%22Build+Status%22) [![codecov](https://codecov.io/gh/egeozguroglu/dtw-sofar/branch/main/graph/badge.svg)](https://codecov.io/gh/egeozguroglu/dtw-sofar)


# Overview

This library implements the "Dynamic Time Warping Algorithm" for multimodal research in a way it can warp the time series received "so far." In other words, it modifies the dynamic time warping algorithm to be compatible with 'iterative stimuli' (when future points in the time series are received one by one). 

One motivating application is aligning natural language annotations and video frames for research on Visual Language Models (e.g. CLIP), when the latter's embeddings are received frame by frame (e.g. we're making observations and don't have access to future frames). With "dtw-sofar," we can predict optimally matching annotations to new video frames on the fly - by relying on temporal information available "so far."

# Development Details
This library uses a `Makefile` as a command registry, with the following commands. 

- `make`: list available commands
- `make develop`: install and build this library and its dependencies using `pip`
- `make build`: build the library using `setuptools`
- `make lint`: perform static analysis of this library with `flake8` and `black`
- `make format`: autoformat this library using `black`
- `make annotate`: run type checking using `mypy`
- `make test`: run automated tests with `pytest`
- `make coverage`: run automated tests with `pytest` and collect coverage information
- `make dist`: package library for distribution

# Installation: 
To install this library, you may execute `pip install dtw-sofar`

# Quick Start Example:
Below is a use case to perform dynamic time warping so-far on image and natural language embeddings from Open AI's CLIP Model, so as to align them:
    from dtw_sofar import dtw_cost, get_initial_matrices, dtw_sofar
    import numpy as np

    video_features = np.load('CLIP_video_embeddings_path')
    text_features = np.load('CLIP_text_embeddings_path')
    cost_matrix, backpointers = get_initial_matrices(video_features, text_features) 

    # iterate over each RGB frame's CLIP embeddings:
    for i in range(len(video_features)):
        path_sofar, cost_matrix, backpointers, current_predicted_idx = dtw_sofar.dtw_sofar(
            frame_features[i], text_features, i, cost_matrix, backpointers, dtw_cost
        )
