# Waffle
Waffle: A Global Data-driven High-resolution Weather Model using Adaptive Fourier Neural

Waffle, short for Fourier Forecasting Neural Network, is a global data-driven weather forecasting model that provides accurate short to medium-range global predictions at 0.25∘ resolution. Waffle accurately forecasts high-resolution, fast-timescale variables such as the surface wind speed, precipitation, and atmospheric water vapor. 

## Training:

The model is trained on a subset of ERA5 reanalysis data on single levels of data heads.

```
FCN_ERA5_data_v0
│   README.md
└───train
│   │   1979.h5
│   │   1980.h5
│   │   ...
│   │   ...
│   │   2015.h5
│   
└───test
│   │   2016.h5
│   │   2017.h5
│
└───out_of_sample
│   │   2018.h5
│
└───static
│   │   orography.h5
│
└───precip
│   │   train/
│   │   test/
│   │   out_of_sample/

```

Training configurations can be set up in [config/AFNO.yaml](config/AFNO.yaml). The following paths need to be set by the user. These paths should point to the data and stats you downloaded in the steps above:

```
afno_backbone: &backbone
  <<: *FULL_FIELD
  ...
  ...
  orography: !!bool False 
  orography_path: None # provide path to orography.h5 file if set to true, 
  exp_dir:             # directory path to store training checkpoints and other output
  train_data_path:     # full path to /train/
  valid_data_path:     # full path to /test/
  inf_data_path:       # full path to /out_of_sample. Will not be used while training.
  time_means_path:     # full path to time_means.npy
  global_means_path:   # full path to global_means.npy
  global_stds_path:    # full path to global_stds.npy

```

An example launch script for distributed data parallel training on the slurm based HPC cluster perlmutter is provided in ```submit_batch.sh```. Please follow the pre-training and fine-tuning procedures as described in the pre-print.

To run the precipitation diagnostic model, see the following example config:

```
precip: &precip
  <<: *backbone
  ...
  ...
  precip:              # full path to precipitation data files 
  time_means_path_tp:  # full path to time means for precipitation
  model_wind_path:     # full path to backbone model weights ckpt

```
