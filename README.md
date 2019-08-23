
# VISAR Tutorial

This project aims to train neural networks by compound-protein interactions and provides interpretation of the learned model by interactively showing transformed chemical landscape and visualized SAR for chemicals of interest.

In this notebook, we will show a typical workflow of using VISAR for training neural network QSAR models and analyzing the trained model.

## Train single task regresion model


```python
import os
from model_training_utils_v2 import ST_model_hyperparam_screen, ST_model_training
os.environ['CUDA_VISIBLE_DEVICES']='1'
```


### model setup


```python
# initialize parameters
task_names = ['T107', 'T108']
MT_dat_name = './data/MT_data_clean_June28.csv'
FP_type = 'Circular_2048'

params_dict = {
    "n_tasks": [1],

    "n_features": [2048], ## need modification given FP types
    "activation": ['relu'],
    "momentum": [.9],
    "batch_size": [128],
    "init": ['glorot_uniform'],
    "learning_rate": [0.01],
    "decay": [1e-6],
    "nb_epoch": [30],
    "dropouts": [.2, .4],
    "nb_layers": [1],
    "batchnorm": [False],
    "layer_sizes": [(1024, 512),(1024,128) ,(512, 128),(512,64),(128,64),(64,32),
                    (1024,512,128), (512,128,64), (128,64,32)],
    "penalty": [0.1]
}
```


```python
# initialize model setup
import random
import time
random_seed = random.randint(0,1000)
local_time = time.localtime(time.time())
log_path = './logs/'
RUN_KEY = 'ST_%d_%d_%d_%d' % (local_time.tm_year, local_time.tm_mon,
                              local_time.tm_mday, random_seed)
os.system('mkdir %s%s' % (log_path, RUN_KEY))
print(RUN_KEY)
```

    ST_2019_8_14_986


### hyperparameter screening


```python
# hyperparam screening using deepchem
log_output = ST_model_hyperparam_screen(MT_dat_name, task_names, FP_type, params_dict,
                                        log_path = './logs/'+RUN_KEY)
```

    ----------------------------------------------
    Extracted dataset shape: (2951, 3)
    Loading raw samples now.
    shard_size: 8192
    About to start loading CSV from ./logs/ST_2019_8_14_986/temp.csv
    Loading shard 1 of size 8192.
    Featurizing sample 0
    Featurizing sample 1000
    Featurizing sample 2000
    TIMING: featurizing shard 0 took 11.429 s
    TIMING: dataset construction took 11.677 s
    Loading dataset from disk.
    Preparing dataset for T107 of rep 0...
    Computing train/valid/test indices
    TIMING: dataset construction took 0.315 s
    Loading dataset from disk.
    TIMING: dataset construction took 0.126 s
    Loading dataset from disk.
    TIMING: dataset construction took 0.140 s
    Loading dataset from disk.
    Hyperprameter screening ...
    Fitting model 1/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (1024, 512), 'n_tasks': 1}
    computed_metrics: [0.5481104503085001]
    Model 1/18, Metric r2_score, Validation set 0: 0.548110
    	best_validation_score so far: 0.548110
    Fitting model 2/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (1024, 512), 'n_tasks': 1}
    computed_metrics: [0.6237788574566245]
    Model 2/18, Metric r2_score, Validation set 1: 0.623779
    	best_validation_score so far: 0.623779
    Fitting model 3/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (1024, 128), 'n_tasks': 1}
    computed_metrics: [0.5835653981206139]
    Model 3/18, Metric r2_score, Validation set 2: 0.583565
    	best_validation_score so far: 0.623779
    Fitting model 4/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (1024, 128), 'n_tasks': 1}
    computed_metrics: [0.6196603911294876]
    Model 4/18, Metric r2_score, Validation set 3: 0.619660
    	best_validation_score so far: 0.623779
    Fitting model 5/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (512, 128), 'n_tasks': 1}
    computed_metrics: [0.6374032122095533]
    Model 5/18, Metric r2_score, Validation set 4: 0.637403
    	best_validation_score so far: 0.637403
    Fitting model 6/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (512, 128), 'n_tasks': 1}
    computed_metrics: [0.6187663912157835]
    Model 6/18, Metric r2_score, Validation set 5: 0.618766
    	best_validation_score so far: 0.637403
    Fitting model 7/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (512, 64), 'n_tasks': 1}
    computed_metrics: [0.6298660644966039]
    Model 7/18, Metric r2_score, Validation set 6: 0.629866
    	best_validation_score so far: 0.637403
    Fitting model 8/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (512, 64), 'n_tasks': 1}
    computed_metrics: [0.6406574576550392]
    Model 8/18, Metric r2_score, Validation set 7: 0.640657
    	best_validation_score so far: 0.640657
    Fitting model 9/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (128, 64), 'n_tasks': 1}
    computed_metrics: [0.6110877363928156]
    Model 9/18, Metric r2_score, Validation set 8: 0.611088
    	best_validation_score so far: 0.640657
    Fitting model 10/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (128, 64), 'n_tasks': 1}
    computed_metrics: [0.602689374923115]
    Model 10/18, Metric r2_score, Validation set 9: 0.602689
    	best_validation_score so far: 0.640657
    Fitting model 11/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (64, 32), 'n_tasks': 1}
    computed_metrics: [0.5979003156666713]
    Model 11/18, Metric r2_score, Validation set 10: 0.597900
    	best_validation_score so far: 0.640657
    Fitting model 12/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (64, 32), 'n_tasks': 1}
    computed_metrics: [0.6163441339563296]
    Model 12/18, Metric r2_score, Validation set 11: 0.616344
    	best_validation_score so far: 0.640657
    Fitting model 13/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (1024, 512, 128), 'n_tasks': 1}
    computed_metrics: [0.5797812103668558]
    Model 13/18, Metric r2_score, Validation set 12: 0.579781
    	best_validation_score so far: 0.640657
    Fitting model 14/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (1024, 512, 128), 'n_tasks': 1}
    computed_metrics: [0.6082225807486198]
    Model 14/18, Metric r2_score, Validation set 13: 0.608223
    	best_validation_score so far: 0.640657
    Fitting model 15/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (512, 128, 64), 'n_tasks': 1}
    computed_metrics: [0.6186375458180398]
    Model 15/18, Metric r2_score, Validation set 14: 0.618638
    	best_validation_score so far: 0.640657
    Fitting model 16/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (512, 128, 64), 'n_tasks': 1}
    computed_metrics: [0.6552503861859487]
    Model 16/18, Metric r2_score, Validation set 15: 0.655250
    	best_validation_score so far: 0.655250
    Fitting model 17/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (128, 64, 32), 'n_tasks': 1}
    computed_metrics: [0.6310180282170208]
    Model 17/18, Metric r2_score, Validation set 16: 0.631018
    	best_validation_score so far: 0.655250
    Fitting model 18/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (128, 64, 32), 'n_tasks': 1}
    computed_metrics: [0.6084419728173178]
    Model 18/18, Metric r2_score, Validation set 17: 0.608442
    	best_validation_score so far: 0.655250
    computed_metrics: [0.9163785773039165]
    Best hyperparameters: ((512, 128, 64), 0.1, 'relu', 2048, 1e-06, 0.01, 0.9, 128, 1, 'glorot_uniform', 30, 0.4, 1, False)
    train_score: 0.916379
    validation_score: 0.655250
    Generate performace report ...
    Preparing dataset for T107 of rep 1...
    Computing train/valid/test indices
    TIMING: dataset construction took 0.320 s
    Loading dataset from disk.
    TIMING: dataset construction took 0.149 s
    Loading dataset from disk.
    TIMING: dataset construction took 0.121 s
    Loading dataset from disk.
    Hyperprameter screening ...
    Fitting model 1/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (1024, 512), 'n_tasks': 1}
    computed_metrics: [0.6141334216423628]
    Model 1/18, Metric r2_score, Validation set 0: 0.614133
    	best_validation_score so far: 0.614133
    Fitting model 2/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (1024, 512), 'n_tasks': 1}
    computed_metrics: [0.6456750069826567]
    Model 2/18, Metric r2_score, Validation set 1: 0.645675
    	best_validation_score so far: 0.645675
    Fitting model 3/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (1024, 128), 'n_tasks': 1}
    computed_metrics: [0.6689664106003053]
    Model 3/18, Metric r2_score, Validation set 2: 0.668966
    	best_validation_score so far: 0.668966
    Fitting model 4/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (1024, 128), 'n_tasks': 1}
    computed_metrics: [0.6880955534249764]
    Model 4/18, Metric r2_score, Validation set 3: 0.688096
    	best_validation_score so far: 0.688096
    Fitting model 5/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (512, 128), 'n_tasks': 1}
    computed_metrics: [0.6884896776847982]
    Model 5/18, Metric r2_score, Validation set 4: 0.688490
    	best_validation_score so far: 0.688490
    Fitting model 6/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (512, 128), 'n_tasks': 1}
    computed_metrics: [0.6844000694925106]
    Model 6/18, Metric r2_score, Validation set 5: 0.684400
    	best_validation_score so far: 0.688490
    Fitting model 7/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (512, 64), 'n_tasks': 1}
    computed_metrics: [0.6963359115130865]
    Model 7/18, Metric r2_score, Validation set 6: 0.696336
    	best_validation_score so far: 0.696336
    Fitting model 8/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (512, 64), 'n_tasks': 1}
    computed_metrics: [0.6850005173519169]
    Model 8/18, Metric r2_score, Validation set 7: 0.685001
    	best_validation_score so far: 0.696336
    Fitting model 9/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (128, 64), 'n_tasks': 1}
    computed_metrics: [0.6529657912138516]
    Model 9/18, Metric r2_score, Validation set 8: 0.652966
    	best_validation_score so far: 0.696336
    Fitting model 10/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (128, 64), 'n_tasks': 1}
    computed_metrics: [0.6943907300870579]
    Model 10/18, Metric r2_score, Validation set 9: 0.694391
    	best_validation_score so far: 0.696336
    Fitting model 11/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (64, 32), 'n_tasks': 1}
    computed_metrics: [0.6882941803739605]
    Model 11/18, Metric r2_score, Validation set 10: 0.688294
    	best_validation_score so far: 0.696336
    Fitting model 12/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (64, 32), 'n_tasks': 1}
    computed_metrics: [0.6688188952045332]
    Model 12/18, Metric r2_score, Validation set 11: 0.668819
    	best_validation_score so far: 0.696336
    Fitting model 13/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (1024, 512, 128), 'n_tasks': 1}
    computed_metrics: [0.663746368808691]
    Model 13/18, Metric r2_score, Validation set 12: 0.663746
    	best_validation_score so far: 0.696336
    Fitting model 14/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (1024, 512, 128), 'n_tasks': 1}
    computed_metrics: [0.6436014936686325]
    Model 14/18, Metric r2_score, Validation set 13: 0.643601
    	best_validation_score so far: 0.696336
    Fitting model 15/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (512, 128, 64), 'n_tasks': 1}
    computed_metrics: [0.7199618702370533]
    Model 15/18, Metric r2_score, Validation set 14: 0.719962
    	best_validation_score so far: 0.719962
    Fitting model 16/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (512, 128, 64), 'n_tasks': 1}
    computed_metrics: [0.7155456170754025]
    Model 16/18, Metric r2_score, Validation set 15: 0.715546
    	best_validation_score so far: 0.719962
    Fitting model 17/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (128, 64, 32), 'n_tasks': 1}
    computed_metrics: [0.6898074732971973]
    Model 17/18, Metric r2_score, Validation set 16: 0.689807
    	best_validation_score so far: 0.719962
    Fitting model 18/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (128, 64, 32), 'n_tasks': 1}
    computed_metrics: [0.6894147493435725]
    Model 18/18, Metric r2_score, Validation set 17: 0.689415
    	best_validation_score so far: 0.719962
    computed_metrics: [0.9520767295678436]
    Best hyperparameters: ((512, 128, 64), 0.1, 'relu', 2048, 1e-06, 0.01, 0.9, 128, 1, 'glorot_uniform', 30, 0.2, 1, False)
    train_score: 0.952077
    validation_score: 0.719962
    Generate performace report ...
    Preparing dataset for T107 of rep 2...
    Computing train/valid/test indices
    TIMING: dataset construction took 0.293 s
    Loading dataset from disk.
    TIMING: dataset construction took 0.115 s
    Loading dataset from disk.
    TIMING: dataset construction took 0.116 s
    Loading dataset from disk.
    Hyperprameter screening ...
    Fitting model 1/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (1024, 512), 'n_tasks': 1}
    computed_metrics: [0.4930277822893324]
    Model 1/18, Metric r2_score, Validation set 0: 0.493028
    	best_validation_score so far: 0.493028
    Fitting model 2/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (1024, 512), 'n_tasks': 1}
    computed_metrics: [0.505910666768453]
    Model 2/18, Metric r2_score, Validation set 1: 0.505911
    	best_validation_score so far: 0.505911
    Fitting model 3/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (1024, 128), 'n_tasks': 1}
    computed_metrics: [0.531299026515801]
    Model 3/18, Metric r2_score, Validation set 2: 0.531299
    	best_validation_score so far: 0.531299
    Fitting model 4/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (1024, 128), 'n_tasks': 1}
    computed_metrics: [0.5740658340870408]
    Model 4/18, Metric r2_score, Validation set 3: 0.574066
    	best_validation_score so far: 0.574066
    Fitting model 5/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (512, 128), 'n_tasks': 1}
    computed_metrics: [0.5305247291640403]
    Model 5/18, Metric r2_score, Validation set 4: 0.530525
    	best_validation_score so far: 0.574066
    Fitting model 6/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (512, 128), 'n_tasks': 1}
    computed_metrics: [0.5153835975720726]
    Model 6/18, Metric r2_score, Validation set 5: 0.515384
    	best_validation_score so far: 0.574066
    Fitting model 7/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (512, 64), 'n_tasks': 1}
    computed_metrics: [0.5228210218689161]
    Model 7/18, Metric r2_score, Validation set 6: 0.522821
    	best_validation_score so far: 0.574066
    Fitting model 8/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (512, 64), 'n_tasks': 1}
    computed_metrics: [0.5418859521801785]
    Model 8/18, Metric r2_score, Validation set 7: 0.541886
    	best_validation_score so far: 0.574066
    Fitting model 9/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (128, 64), 'n_tasks': 1}
    computed_metrics: [0.4595043813250017]
    Model 9/18, Metric r2_score, Validation set 8: 0.459504
    	best_validation_score so far: 0.574066
    Fitting model 10/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (128, 64), 'n_tasks': 1}
    computed_metrics: [0.49186165422505523]
    Model 10/18, Metric r2_score, Validation set 9: 0.491862
    	best_validation_score so far: 0.574066
    Fitting model 11/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (64, 32), 'n_tasks': 1}
    computed_metrics: [0.5167497160336099]
    Model 11/18, Metric r2_score, Validation set 10: 0.516750
    	best_validation_score so far: 0.574066
    Fitting model 12/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (64, 32), 'n_tasks': 1}
    computed_metrics: [0.5196863583860438]
    Model 12/18, Metric r2_score, Validation set 11: 0.519686
    	best_validation_score so far: 0.574066
    Fitting model 13/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (1024, 512, 128), 'n_tasks': 1}
    computed_metrics: [0.5085581237282972]
    Model 13/18, Metric r2_score, Validation set 12: 0.508558
    	best_validation_score so far: 0.574066
    Fitting model 14/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (1024, 512, 128), 'n_tasks': 1}
    computed_metrics: [0.5375331002631859]
    Model 14/18, Metric r2_score, Validation set 13: 0.537533
    	best_validation_score so far: 0.574066
    Fitting model 15/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (512, 128, 64), 'n_tasks': 1}
    computed_metrics: [0.5469272539992392]
    Model 15/18, Metric r2_score, Validation set 14: 0.546927
    	best_validation_score so far: 0.574066
    Fitting model 16/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (512, 128, 64), 'n_tasks': 1}
    computed_metrics: [0.5650936130734651]
    Model 16/18, Metric r2_score, Validation set 15: 0.565094
    	best_validation_score so far: 0.574066
    Fitting model 17/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (128, 64, 32), 'n_tasks': 1}
    computed_metrics: [0.48516907723167413]
    Model 17/18, Metric r2_score, Validation set 16: 0.485169
    	best_validation_score so far: 0.574066
    Fitting model 18/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (128, 64, 32), 'n_tasks': 1}
    computed_metrics: [0.5617374658837493]
    Model 18/18, Metric r2_score, Validation set 17: 0.561737
    	best_validation_score so far: 0.574066
    computed_metrics: [0.8890936408066037]
    Best hyperparameters: ((1024, 128), 0.1, 'relu', 2048, 1e-06, 0.01, 0.9, 128, 1, 'glorot_uniform', 30, 0.4, 1, False)
    train_score: 0.889094
    validation_score: 0.574066
    Generate performace report ...
    ----------------------------------------------
    Extracted dataset shape: (2063, 3)
    Loading raw samples now.
    shard_size: 8192
    About to start loading CSV from ./logs/ST_2019_8_14_986/temp.csv
    Loading shard 1 of size 8192.
    Featurizing sample 0
    Featurizing sample 1000
    Featurizing sample 2000
    TIMING: featurizing shard 0 took 8.051 s
    TIMING: dataset construction took 8.207 s
    Loading dataset from disk.
    Preparing dataset for T108 of rep 0...
    Computing train/valid/test indices
    TIMING: dataset construction took 0.209 s
    Loading dataset from disk.
    TIMING: dataset construction took 0.084 s
    Loading dataset from disk.
    TIMING: dataset construction took 0.084 s
    Loading dataset from disk.
    Hyperprameter screening ...
    Fitting model 1/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (1024, 512), 'n_tasks': 1}
    computed_metrics: [0.4868601404823667]
    Model 1/18, Metric r2_score, Validation set 0: 0.486860
    	best_validation_score so far: 0.486860
    Fitting model 2/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (1024, 512), 'n_tasks': 1}
    computed_metrics: [0.5342579656611066]
    Model 2/18, Metric r2_score, Validation set 1: 0.534258
    	best_validation_score so far: 0.534258
    Fitting model 3/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (1024, 128), 'n_tasks': 1}
    computed_metrics: [0.6270370260083612]
    Model 3/18, Metric r2_score, Validation set 2: 0.627037
    	best_validation_score so far: 0.627037
    Fitting model 4/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (1024, 128), 'n_tasks': 1}
    computed_metrics: [0.6428942871629781]
    Model 4/18, Metric r2_score, Validation set 3: 0.642894
    	best_validation_score so far: 0.642894
    Fitting model 5/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (512, 128), 'n_tasks': 1}
    computed_metrics: [0.6158495290980834]
    Model 5/18, Metric r2_score, Validation set 4: 0.615850
    	best_validation_score so far: 0.642894
    Fitting model 6/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (512, 128), 'n_tasks': 1}
    computed_metrics: [0.639602824852064]
    Model 6/18, Metric r2_score, Validation set 5: 0.639603
    	best_validation_score so far: 0.642894
    Fitting model 7/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (512, 64), 'n_tasks': 1}
    computed_metrics: [0.5983536776309266]
    Model 7/18, Metric r2_score, Validation set 6: 0.598354
    	best_validation_score so far: 0.642894
    Fitting model 8/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (512, 64), 'n_tasks': 1}
    computed_metrics: [0.6262399422329217]
    Model 8/18, Metric r2_score, Validation set 7: 0.626240
    	best_validation_score so far: 0.642894
    Fitting model 9/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (128, 64), 'n_tasks': 1}
    computed_metrics: [0.5898805159564117]
    Model 9/18, Metric r2_score, Validation set 8: 0.589881
    	best_validation_score so far: 0.642894
    Fitting model 10/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (128, 64), 'n_tasks': 1}
    computed_metrics: [0.6361380166768911]
    Model 10/18, Metric r2_score, Validation set 9: 0.636138
    	best_validation_score so far: 0.642894
    Fitting model 11/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (64, 32), 'n_tasks': 1}
    computed_metrics: [0.590457088062357]
    Model 11/18, Metric r2_score, Validation set 10: 0.590457
    	best_validation_score so far: 0.642894
    Fitting model 12/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (64, 32), 'n_tasks': 1}
    computed_metrics: [0.577278038754506]
    Model 12/18, Metric r2_score, Validation set 11: 0.577278
    	best_validation_score so far: 0.642894
    Fitting model 13/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (1024, 512, 128), 'n_tasks': 1}
    computed_metrics: [0.5676634412049765]
    Model 13/18, Metric r2_score, Validation set 12: 0.567663
    	best_validation_score so far: 0.642894
    Fitting model 14/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (1024, 512, 128), 'n_tasks': 1}
    computed_metrics: [0.4004630563531284]
    Model 14/18, Metric r2_score, Validation set 13: 0.400463
    	best_validation_score so far: 0.642894
    Fitting model 15/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (512, 128, 64), 'n_tasks': 1}
    computed_metrics: [0.606689529089742]
    Model 15/18, Metric r2_score, Validation set 14: 0.606690
    	best_validation_score so far: 0.642894
    Fitting model 16/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (512, 128, 64), 'n_tasks': 1}
    computed_metrics: [0.6226891620843279]
    Model 16/18, Metric r2_score, Validation set 15: 0.622689
    	best_validation_score so far: 0.642894
    Fitting model 17/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (128, 64, 32), 'n_tasks': 1}
    computed_metrics: [0.5759372539391608]
    Model 17/18, Metric r2_score, Validation set 16: 0.575937
    	best_validation_score so far: 0.642894
    Fitting model 18/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (128, 64, 32), 'n_tasks': 1}
    computed_metrics: [0.6011259388892263]
    Model 18/18, Metric r2_score, Validation set 17: 0.601126
    	best_validation_score so far: 0.642894
    computed_metrics: [0.878111896316526]
    Best hyperparameters: ((1024, 128), 0.1, 'relu', 2048, 1e-06, 0.01, 0.9, 128, 1, 'glorot_uniform', 30, 0.4, 1, False)
    train_score: 0.878112
    validation_score: 0.642894
    Generate performace report ...
    Preparing dataset for T108 of rep 1...
    Computing train/valid/test indices
    TIMING: dataset construction took 0.234 s
    Loading dataset from disk.
    TIMING: dataset construction took 0.088 s
    Loading dataset from disk.
    TIMING: dataset construction took 0.083 s
    Loading dataset from disk.
    Hyperprameter screening ...
    Fitting model 1/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (1024, 512), 'n_tasks': 1}
    computed_metrics: [0.4761562050362087]
    Model 1/18, Metric r2_score, Validation set 0: 0.476156
    	best_validation_score so far: 0.476156
    Fitting model 2/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (1024, 512), 'n_tasks': 1}
    computed_metrics: [0.5603185611901793]
    Model 2/18, Metric r2_score, Validation set 1: 0.560319
    	best_validation_score so far: 0.560319
    Fitting model 3/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (1024, 128), 'n_tasks': 1}
    computed_metrics: [0.6139116143568223]
    Model 3/18, Metric r2_score, Validation set 2: 0.613912
    	best_validation_score so far: 0.613912
    Fitting model 4/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (1024, 128), 'n_tasks': 1}
    computed_metrics: [0.5952160917993221]
    Model 4/18, Metric r2_score, Validation set 3: 0.595216
    	best_validation_score so far: 0.613912
    Fitting model 5/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (512, 128), 'n_tasks': 1}
    computed_metrics: [0.6196258627688307]
    Model 5/18, Metric r2_score, Validation set 4: 0.619626
    	best_validation_score so far: 0.619626
    Fitting model 6/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (512, 128), 'n_tasks': 1}
    computed_metrics: [0.6157923884866285]
    Model 6/18, Metric r2_score, Validation set 5: 0.615792
    	best_validation_score so far: 0.619626
    Fitting model 7/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (512, 64), 'n_tasks': 1}
    computed_metrics: [0.6619856420412034]
    Model 7/18, Metric r2_score, Validation set 6: 0.661986
    	best_validation_score so far: 0.661986
    Fitting model 8/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (512, 64), 'n_tasks': 1}
    computed_metrics: [0.6730749328874235]
    Model 8/18, Metric r2_score, Validation set 7: 0.673075
    	best_validation_score so far: 0.673075
    Fitting model 9/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (128, 64), 'n_tasks': 1}
    computed_metrics: [0.6280516778814411]
    Model 9/18, Metric r2_score, Validation set 8: 0.628052
    	best_validation_score so far: 0.673075
    Fitting model 10/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (128, 64), 'n_tasks': 1}
    computed_metrics: [0.6754045394217645]
    Model 10/18, Metric r2_score, Validation set 9: 0.675405
    	best_validation_score so far: 0.675405
    Fitting model 11/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (64, 32), 'n_tasks': 1}
    computed_metrics: [0.6099613250901108]
    Model 11/18, Metric r2_score, Validation set 10: 0.609961
    	best_validation_score so far: 0.675405
    Fitting model 12/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (64, 32), 'n_tasks': 1}
    computed_metrics: [0.6657732916243124]
    Model 12/18, Metric r2_score, Validation set 11: 0.665773
    	best_validation_score so far: 0.675405
    Fitting model 13/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (1024, 512, 128), 'n_tasks': 1}
    computed_metrics: [0.5409865108703251]
    Model 13/18, Metric r2_score, Validation set 12: 0.540987
    	best_validation_score so far: 0.675405
    Fitting model 14/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (1024, 512, 128), 'n_tasks': 1}
    computed_metrics: [0.5270717282697137]
    Model 14/18, Metric r2_score, Validation set 13: 0.527072
    	best_validation_score so far: 0.675405
    Fitting model 15/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (512, 128, 64), 'n_tasks': 1}
    computed_metrics: [0.6137628239191688]
    Model 15/18, Metric r2_score, Validation set 14: 0.613763
    	best_validation_score so far: 0.675405
    Fitting model 16/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (512, 128, 64), 'n_tasks': 1}
    computed_metrics: [0.6438303000670302]
    Model 16/18, Metric r2_score, Validation set 15: 0.643830
    	best_validation_score so far: 0.675405
    Fitting model 17/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (128, 64, 32), 'n_tasks': 1}
    computed_metrics: [0.6337577807823557]
    Model 17/18, Metric r2_score, Validation set 16: 0.633758
    	best_validation_score so far: 0.675405
    Fitting model 18/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (128, 64, 32), 'n_tasks': 1}
    computed_metrics: [0.6327424936791699]
    Model 18/18, Metric r2_score, Validation set 17: 0.632742
    	best_validation_score so far: 0.675405
    computed_metrics: [0.9318955372212835]
    Best hyperparameters: ((128, 64), 0.1, 'relu', 2048, 1e-06, 0.01, 0.9, 128, 1, 'glorot_uniform', 30, 0.4, 1, False)
    train_score: 0.931896
    validation_score: 0.675405
    Generate performace report ...
    Preparing dataset for T108 of rep 2...
    Computing train/valid/test indices
    TIMING: dataset construction took 0.202 s
    Loading dataset from disk.
    TIMING: dataset construction took 0.085 s
    Loading dataset from disk.
    TIMING: dataset construction took 0.084 s
    Loading dataset from disk.
    Hyperprameter screening ...
    Fitting model 1/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (1024, 512), 'n_tasks': 1}
    computed_metrics: [0.4635391750402559]
    Model 1/18, Metric r2_score, Validation set 0: 0.463539
    	best_validation_score so far: 0.463539
    Fitting model 2/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (1024, 512), 'n_tasks': 1}
    computed_metrics: [0.4592714074438822]
    Model 2/18, Metric r2_score, Validation set 1: 0.459271
    	best_validation_score so far: 0.463539
    Fitting model 3/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (1024, 128), 'n_tasks': 1}
    computed_metrics: [0.5656331184616337]
    Model 3/18, Metric r2_score, Validation set 2: 0.565633
    	best_validation_score so far: 0.565633
    Fitting model 4/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (1024, 128), 'n_tasks': 1}
    computed_metrics: [0.5585518225147716]
    Model 4/18, Metric r2_score, Validation set 3: 0.558552
    	best_validation_score so far: 0.565633
    Fitting model 5/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (512, 128), 'n_tasks': 1}
    computed_metrics: [0.573778460798537]
    Model 5/18, Metric r2_score, Validation set 4: 0.573778
    	best_validation_score so far: 0.573778
    Fitting model 6/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (512, 128), 'n_tasks': 1}
    computed_metrics: [0.5614257800104939]
    Model 6/18, Metric r2_score, Validation set 5: 0.561426
    	best_validation_score so far: 0.573778
    Fitting model 7/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (512, 64), 'n_tasks': 1}
    computed_metrics: [0.4895738450317698]
    Model 7/18, Metric r2_score, Validation set 6: 0.489574
    	best_validation_score so far: 0.573778
    Fitting model 8/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (512, 64), 'n_tasks': 1}
    computed_metrics: [0.5554727008272755]
    Model 8/18, Metric r2_score, Validation set 7: 0.555473
    	best_validation_score so far: 0.573778
    Fitting model 9/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (128, 64), 'n_tasks': 1}
    computed_metrics: [0.5324274902691197]
    Model 9/18, Metric r2_score, Validation set 8: 0.532427
    	best_validation_score so far: 0.573778
    Fitting model 10/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (128, 64), 'n_tasks': 1}
    computed_metrics: [0.5710144564196316]
    Model 10/18, Metric r2_score, Validation set 9: 0.571014
    	best_validation_score so far: 0.573778
    Fitting model 11/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (64, 32), 'n_tasks': 1}
    computed_metrics: [0.5176407099870516]
    Model 11/18, Metric r2_score, Validation set 10: 0.517641
    	best_validation_score so far: 0.573778
    Fitting model 12/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (64, 32), 'n_tasks': 1}
    computed_metrics: [0.5586115495820354]
    Model 12/18, Metric r2_score, Validation set 11: 0.558612
    	best_validation_score so far: 0.573778
    Fitting model 13/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (1024, 512, 128), 'n_tasks': 1}
    computed_metrics: [0.5543710979710084]
    Model 13/18, Metric r2_score, Validation set 12: 0.554371
    	best_validation_score so far: 0.573778
    Fitting model 14/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (1024, 512, 128), 'n_tasks': 1}
    computed_metrics: [0.6054202284109258]
    Model 14/18, Metric r2_score, Validation set 13: 0.605420
    	best_validation_score so far: 0.605420
    Fitting model 15/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (512, 128, 64), 'n_tasks': 1}
    computed_metrics: [0.5903675109864684]
    Model 15/18, Metric r2_score, Validation set 14: 0.590368
    	best_validation_score so far: 0.605420
    Fitting model 16/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (512, 128, 64), 'n_tasks': 1}
    computed_metrics: [0.6108050572454018]
    Model 16/18, Metric r2_score, Validation set 15: 0.610805
    	best_validation_score so far: 0.610805
    Fitting model 17/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.2, 'nb_epoch': 30, 'layer_sizes': (128, 64, 32), 'n_tasks': 1}
    computed_metrics: [0.522967609358389]
    Model 17/18, Metric r2_score, Validation set 16: 0.522968
    	best_validation_score so far: 0.610805
    Fitting model 18/18
    hyperparameters: {'penalty': 0.1, 'activation': 'relu', 'n_features': 2048, 'decay': 1e-06, 'batchnorm': False, 'learning_rate': 0.01, 'momentum': 0.9, 'batch_size': 128, 'nb_layers': 1, 'init': 'glorot_uniform', 'dropouts': 0.4, 'nb_epoch': 30, 'layer_sizes': (128, 64, 32), 'n_tasks': 1}
    computed_metrics: [0.6275553205084103]
    Model 18/18, Metric r2_score, Validation set 17: 0.627555
    	best_validation_score so far: 0.627555
    computed_metrics: [0.9142279933418767]
    Best hyperparameters: ((128, 64, 32), 0.1, 'relu', 2048, 1e-06, 0.01, 0.9, 128, 1, 'glorot_uniform', 30, 0.4, 1, False)
    train_score: 0.914228
    validation_score: 0.627555
    Generate performace report ...



```python
# manually pick the training parameters, referring to hyperparam_log saved in RUN_KEY directory
best_hyperparams = {'T107': [(512,64,1), 0.4],
                    'T108': [(512,128,1), 0.2]
                   }
```

### model training


```python
MT_dat_name = './data/MT_data_clean_Feb28.csv'
FP_type = 'Circular_2048'
RUN_KEY = 'ST_2019_8_14_986'
```


```python
# model training
output_df = ST_model_training(MT_dat_name, FP_type,
                              best_hyperparams, result_path = './logs/'+RUN_KEY)
```

    ----------------------------------------------
    Extracted dataset shape: (2063, 3)
    Loading raw samples now.
    shard_size: 8192
    About to start loading CSV from ./logs/ST_2019_8_14_986/temp.csv
    Loading shard 1 of size 8192.
    Featurizing sample 0
    Featurizing sample 1000
    Featurizing sample 2000
    TIMING: featurizing shard 0 took 8.254 s
    TIMING: dataset construction took 8.417 s
    Loading dataset from disk.
    Preparing dataset for T108 of rep 0...
    Computing train/valid/test indices
    TIMING: dataset construction took 0.211 s
    Loading dataset from disk.
    TIMING: dataset construction took 0.098 s
    Loading dataset from disk.
    Model training ...
    WARNING:tensorflow:From /root/anaconda3/envs/deepchem/lib/python3.5/site-packages/keras/backend/tensorflow_backend.py:1108: calling reduce_mean (from tensorflow.python.ops.math_ops) with keep_dims is deprecated and will be removed in a future version.
    Instructions for updating:
    keep_dims is deprecated, use keepdims instead
    Epoch 1/5
    1650/1650 [==============================] - 0s - loss: 4.5879     
    Epoch 2/5
    1650/1650 [==============================] - 0s - loss: 1.1412     
    Epoch 3/5
    1650/1650 [==============================] - 0s - loss: 0.7993     
    Epoch 4/5
    1650/1650 [==============================] - 0s - loss: 0.6275     
    Epoch 5/5
    1650/1650 [==============================] - 0s - loss: 0.5385     
    Epoch 1/5
    1650/1650 [==============================] - 0s - loss: 0.0605     
    Epoch 2/5
    1650/1650 [==============================] - 0s - loss: 0.0595     
    Epoch 3/5
    1650/1650 [==============================] - 0s - loss: 0.0688     
    Epoch 4/5
    1650/1650 [==============================] - 0s - loss: 0.0618     
    Epoch 5/5
    1650/1650 [==============================] - 0s - loss: 0.0603     
    Epoch 1/5
    1650/1650 [==============================] - 0s - loss: 0.0463     
    Epoch 2/5
    1650/1650 [==============================] - 0s - loss: 0.0485     
    Epoch 3/5
    1650/1650 [==============================] - 0s - loss: 0.0494     
    Epoch 4/5
    1650/1650 [==============================] - 0s - loss: 0.0474     
    Epoch 5/5
    1650/1650 [==============================] - 0s - loss: 0.0476     
    Epoch 1/5
    1650/1650 [==============================] - 0s - loss: 0.0399     
    Epoch 2/5
    1650/1650 [==============================] - 0s - loss: 0.0407     
    Epoch 3/5
    1650/1650 [==============================] - 0s - loss: 0.0406     
    Epoch 4/5
    1650/1650 [==============================] - 0s - loss: 0.0408     
    Epoch 5/5
    1650/1650 [==============================] - 0s - loss: 0.0365     
    Epoch 1/5
    1650/1650 [==============================] - 0s - loss: 0.0359     
    Epoch 2/5
    1650/1650 [==============================] - 0s - loss: 0.0370     
    Epoch 3/5
    1650/1650 [==============================] - 0s - loss: 0.0334     
    Epoch 4/5
    1650/1650 [==============================] - 0s - loss: 0.0368     
    Epoch 5/5
    1650/1650 [==============================] - 0s - loss: 0.0380     
    Epoch 1/5
    1650/1650 [==============================] - 0s - loss: 0.0320     
    Epoch 2/5
    1650/1650 [==============================] - 0s - loss: 0.0338     
    Epoch 3/5
    1650/1650 [==============================] - 0s - loss: 0.0335     
    Epoch 4/5
    1650/1650 [==============================] - 0s - loss: 0.0336     
    Epoch 5/5
    1650/1650 [==============================] - 0s - loss: 0.0327     
    Training baseline models ...
    Saving metrics ...
    Preparing dataset for T108 of rep 1...
    Computing train/valid/test indices
    TIMING: dataset construction took 0.187 s
    Loading dataset from disk.
    TIMING: dataset construction took 0.103 s
    Loading dataset from disk.
    Model training ...
    Epoch 1/5
    1650/1650 [==============================] - 0s - loss: 4.3973     
    Epoch 2/5
    1650/1650 [==============================] - 0s - loss: 1.1658     
    Epoch 3/5
    1650/1650 [==============================] - 0s - loss: 0.8354     
    Epoch 4/5
    1650/1650 [==============================] - 0s - loss: 0.6408     
    Epoch 5/5
    1650/1650 [==============================] - 0s - loss: 0.5431     
    Epoch 1/5
    1650/1650 [==============================] - 0s - loss: 0.0624     
    Epoch 2/5
    1650/1650 [==============================] - 0s - loss: 0.0663     
    Epoch 3/5
    1650/1650 [==============================] - 0s - loss: 0.0614     
    Epoch 4/5
    1650/1650 [==============================] - 0s - loss: 0.0589     
    Epoch 5/5
    1650/1650 [==============================] - 0s - loss: 0.0613     
    Epoch 1/5
    1650/1650 [==============================] - 0s - loss: 0.0494     
    Epoch 2/5
    1650/1650 [==============================] - 0s - loss: 0.0490     
    Epoch 3/5
    1650/1650 [==============================] - 0s - loss: 0.0460     
    Epoch 4/5
    1650/1650 [==============================] - 0s - loss: 0.0514     
    Epoch 5/5
    1650/1650 [==============================] - 0s - loss: 0.0473     
    Epoch 1/5
    1650/1650 [==============================] - 0s - loss: 0.0440     
    Epoch 2/5
    1650/1650 [==============================] - 0s - loss: 0.0419     
    Epoch 3/5
    1650/1650 [==============================] - 0s - loss: 0.0411     
    Epoch 4/5
    1650/1650 [==============================] - 0s - loss: 0.0422     
    Epoch 5/5
    1650/1650 [==============================] - 0s - loss: 0.0417     
    Epoch 1/5
    1650/1650 [==============================] - 0s - loss: 0.0348     
    Epoch 2/5
    1650/1650 [==============================] - 0s - loss: 0.0359     
    Epoch 3/5
    1650/1650 [==============================] - 0s - loss: 0.0364     
    Epoch 4/5
    1650/1650 [==============================] - 0s - loss: 0.0380     
    Epoch 5/5
    1650/1650 [==============================] - 0s - loss: 0.0353     
    Epoch 1/5
    1650/1650 [==============================] - 0s - loss: 0.0328     
    Epoch 2/5
    1650/1650 [==============================] - 0s - loss: 0.0319     
    Epoch 3/5
    1650/1650 [==============================] - 0s - loss: 0.0323     
    Epoch 4/5
    1650/1650 [==============================] - 0s - loss: 0.0348     
    Epoch 5/5
    1650/1650 [==============================] - 0s - loss: 0.0314     
    Training baseline models ...
    Saving metrics ...
    Preparing dataset for T108 of rep 2...
    Computing train/valid/test indices
    TIMING: dataset construction took 0.195 s
    Loading dataset from disk.
    TIMING: dataset construction took 0.098 s
    Loading dataset from disk.
    Model training ...
    Epoch 1/5
    1650/1650 [==============================] - 0s - loss: 6.0135     
    Epoch 2/5
    1650/1650 [==============================] - 0s - loss: 1.3239     
    Epoch 3/5
    1650/1650 [==============================] - 0s - loss: 0.8725     
    Epoch 4/5
    1650/1650 [==============================] - 0s - loss: 0.7057     
    Epoch 5/5
    1650/1650 [==============================] - 0s - loss: 0.5775     
    Epoch 1/5
    1650/1650 [==============================] - 0s - loss: 0.0595     
    Epoch 2/5
    1650/1650 [==============================] - 0s - loss: 0.0606     
    Epoch 3/5
    1650/1650 [==============================] - 0s - loss: 0.0582     
    Epoch 4/5
    1650/1650 [==============================] - 0s - loss: 0.0632     
    Epoch 5/5
    1650/1650 [==============================] - 0s - loss: 0.0626     
    Epoch 1/5
    1650/1650 [==============================] - 0s - loss: 0.0482     
    Epoch 2/5
    1650/1650 [==============================] - 0s - loss: 0.0468     
    Epoch 3/5
    1650/1650 [==============================] - 0s - loss: 0.0476     
    Epoch 4/5
    1650/1650 [==============================] - 0s - loss: 0.0470     
    Epoch 5/5
    1650/1650 [==============================] - 0s - loss: 0.0477     
    Epoch 1/5
    1650/1650 [==============================] - 0s - loss: 0.0349     
    Epoch 2/5
    1650/1650 [==============================] - 0s - loss: 0.0360     
    Epoch 3/5
    1650/1650 [==============================] - 0s - loss: 0.0362     
    Epoch 4/5
    1650/1650 [==============================] - 0s - loss: 0.0352     
    Epoch 5/5
    1650/1650 [==============================] - 0s - loss: 0.0357     
    Epoch 1/5
    1650/1650 [==============================] - 0s - loss: 0.0301     
    Epoch 2/5
    1650/1650 [==============================] - 0s - loss: 0.0302     
    Epoch 3/5
    1650/1650 [==============================] - 0s - loss: 0.0336     
    Epoch 4/5
    1650/1650 [==============================] - 0s - loss: 0.0299     
    Epoch 5/5
    1650/1650 [==============================] - 0s - loss: 0.0300     
    Epoch 1/5
    1650/1650 [==============================] - 0s - loss: 0.0278     
    Epoch 2/5
    1650/1650 [==============================] - 0s - loss: 0.0263     
    Epoch 3/5
    1650/1650 [==============================] - 0s - loss: 0.0283     
    Epoch 4/5
    1650/1650 [==============================] - 0s - loss: 0.0284     
    Epoch 5/5
    1650/1650 [==============================] - 0s - loss: 0.0278     
    Training baseline models ...
    Saving metrics ...
    Generate performace report ...
    ----------------------------------------------
    Extracted dataset shape: (2951, 3)
    Loading raw samples now.
    shard_size: 8192
    About to start loading CSV from ./logs/ST_2019_8_14_986/temp.csv
    Loading shard 1 of size 8192.
    Featurizing sample 0
    Featurizing sample 1000
    Featurizing sample 2000
    TIMING: featurizing shard 0 took 11.116 s
    TIMING: dataset construction took 11.333 s
    Loading dataset from disk.
    Preparing dataset for T107 of rep 0...
    Computing train/valid/test indices
    TIMING: dataset construction took 0.281 s
    Loading dataset from disk.
    TIMING: dataset construction took 0.151 s
    Loading dataset from disk.
    Model training ...
    Epoch 1/5
    2360/2360 [==============================] - 1s - loss: 5.0688     
    Epoch 2/5
    2360/2360 [==============================] - 0s - loss: 1.6524     
    Epoch 3/5
    2360/2360 [==============================] - 0s - loss: 1.2350     
    Epoch 4/5
    2360/2360 [==============================] - 0s - loss: 1.0929     
    Epoch 5/5
    2360/2360 [==============================] - 0s - loss: 0.9332     
    Epoch 1/5
    2360/2360 [==============================] - 0s - loss: 0.1423     
    Epoch 2/5
    2360/2360 [==============================] - 0s - loss: 0.1396     
    Epoch 3/5
    2360/2360 [==============================] - 0s - loss: 0.1356     
    Epoch 4/5
    2360/2360 [==============================] - 0s - loss: 0.1390     
    Epoch 5/5
    2360/2360 [==============================] - 0s - loss: 0.1316     
    Epoch 1/5
    2360/2360 [==============================] - 0s - loss: 0.0837     
    Epoch 2/5
    2360/2360 [==============================] - 0s - loss: 0.0798     
    Epoch 3/5
    2360/2360 [==============================] - 0s - loss: 0.0770     
    Epoch 4/5
    2360/2360 [==============================] - 0s - loss: 0.0780     
    Epoch 5/5
    2360/2360 [==============================] - 0s - loss: 0.0785     
    Epoch 1/5
    2360/2360 [==============================] - 0s - loss: 0.0667     
    Epoch 2/5
    2360/2360 [==============================] - 0s - loss: 0.0675     
    Epoch 3/5
    2360/2360 [==============================] - 0s - loss: 0.0642     
    Epoch 4/5
    2360/2360 [==============================] - 0s - loss: 0.0671     
    Epoch 5/5
    2360/2360 [==============================] - 0s - loss: 0.0677     
    Epoch 1/5
    2360/2360 [==============================] - 0s - loss: 0.0647     
    Epoch 2/5
    2360/2360 [==============================] - 0s - loss: 0.0652     
    Epoch 3/5
    2360/2360 [==============================] - 0s - loss: 0.0613     
    Epoch 4/5
    2360/2360 [==============================] - 0s - loss: 0.0630     
    Epoch 5/5
    2360/2360 [==============================] - 0s - loss: 0.0589     
    Epoch 1/5
    2360/2360 [==============================] - 0s - loss: 0.0618     
    Epoch 2/5
    2360/2360 [==============================] - 0s - loss: 0.0586     
    Epoch 3/5
    2360/2360 [==============================] - 0s - loss: 0.0642     
    Epoch 4/5
    2360/2360 [==============================] - 0s - loss: 0.0614     
    Epoch 5/5
    2360/2360 [==============================] - 0s - loss: 0.0619     
    Training baseline models ...
    Saving metrics ...
    Preparing dataset for T107 of rep 1...
    Computing train/valid/test indices
    TIMING: dataset construction took 0.276 s
    Loading dataset from disk.
    TIMING: dataset construction took 0.139 s
    Loading dataset from disk.
    Model training ...
    Epoch 1/5
    2360/2360 [==============================] - 1s - loss: 5.2055     
    Epoch 2/5
    2360/2360 [==============================] - 0s - loss: 1.5749     
    Epoch 3/5
    2360/2360 [==============================] - 0s - loss: 1.1795     
    Epoch 4/5
    2360/2360 [==============================] - 0s - loss: 1.0271     
    Epoch 5/5
    2360/2360 [==============================] - 0s - loss: 0.9604     
    Epoch 1/5
    2360/2360 [==============================] - 0s - loss: 0.1215     
    Epoch 2/5
    2360/2360 [==============================] - 0s - loss: 0.1220     
    Epoch 3/5
    2360/2360 [==============================] - 0s - loss: 0.1236     
    Epoch 4/5
    2360/2360 [==============================] - 0s - loss: 0.1186     
    Epoch 5/5
    2360/2360 [==============================] - 0s - loss: 0.1264     
    Epoch 1/5
    2360/2360 [==============================] - 0s - loss: 0.0737     
    Epoch 2/5
    2360/2360 [==============================] - 0s - loss: 0.0689     
    Epoch 3/5
    2360/2360 [==============================] - 0s - loss: 0.0699     
    Epoch 4/5
    2360/2360 [==============================] - 0s - loss: 0.0697     
    Epoch 5/5
    2360/2360 [==============================] - 0s - loss: 0.0697     
    Epoch 1/5
    2360/2360 [==============================] - 0s - loss: 0.0600     
    Epoch 2/5
    2360/2360 [==============================] - 0s - loss: 0.0597     
    Epoch 3/5
    2360/2360 [==============================] - 0s - loss: 0.0590     
    Epoch 4/5
    2360/2360 [==============================] - 0s - loss: 0.0643     
    Epoch 5/5
    2360/2360 [==============================] - 0s - loss: 0.0590     
    Epoch 1/5
    2360/2360 [==============================] - 0s - loss: 0.0592     
    Epoch 2/5
    2360/2360 [==============================] - 0s - loss: 0.0580     
    Epoch 3/5
    2360/2360 [==============================] - 0s - loss: 0.0579     
    Epoch 4/5
    2360/2360 [==============================] - 0s - loss: 0.0554     
    Epoch 5/5
    2360/2360 [==============================] - 0s - loss: 0.0595     
    Epoch 1/5
    2360/2360 [==============================] - 0s - loss: 0.0566     
    Epoch 2/5
    2360/2360 [==============================] - 0s - loss: 0.0574     
    Epoch 3/5
    2360/2360 [==============================] - 0s - loss: 0.0563     
    Epoch 4/5
    2360/2360 [==============================] - 0s - loss: 0.0582     
    Epoch 5/5
    2360/2360 [==============================] - 0s - loss: 0.0548     
    Training baseline models ...
    Saving metrics ...
    Preparing dataset for T107 of rep 2...
    Computing train/valid/test indices
    TIMING: dataset construction took 0.274 s
    Loading dataset from disk.
    TIMING: dataset construction took 0.140 s
    Loading dataset from disk.
    Model training ...
    Epoch 1/5
    2360/2360 [==============================] - 1s - loss: 5.5089     
    Epoch 2/5
    2360/2360 [==============================] - 0s - loss: 1.7023     
    Epoch 3/5
    2360/2360 [==============================] - 0s - loss: 1.3171     
    Epoch 4/5
    2360/2360 [==============================] - 0s - loss: 1.1166     
    Epoch 5/5
    2360/2360 [==============================] - 0s - loss: 0.9882     
    Epoch 1/5
    2360/2360 [==============================] - 0s - loss: 0.1386     
    Epoch 2/5
    2360/2360 [==============================] - 0s - loss: 0.1187     
    Epoch 3/5
    2360/2360 [==============================] - 0s - loss: 0.1288     
    Epoch 4/5
    2360/2360 [==============================] - 0s - loss: 0.1254     
    Epoch 5/5
    2360/2360 [==============================] - 0s - loss: 0.1222     
    Epoch 1/5
    2360/2360 [==============================] - 0s - loss: 0.0751     
    Epoch 2/5
    2360/2360 [==============================] - 0s - loss: 0.0730     
    Epoch 3/5
    2360/2360 [==============================] - 0s - loss: 0.0685     
    Epoch 4/5
    2360/2360 [==============================] - 0s - loss: 0.0707     
    Epoch 5/5
    2360/2360 [==============================] - 0s - loss: 0.0743     
    Epoch 1/5
    2360/2360 [==============================] - 0s - loss: 0.0622     
    Epoch 2/5
    2360/2360 [==============================] - 0s - loss: 0.0587     
    Epoch 3/5
    2360/2360 [==============================] - 0s - loss: 0.0625     
    Epoch 4/5
    2360/2360 [==============================] - 0s - loss: 0.0604     
    Epoch 5/5
    2360/2360 [==============================] - 0s - loss: 0.0575     
    Epoch 1/5
    2360/2360 [==============================] - 0s - loss: 0.0586     
    Epoch 2/5
    2360/2360 [==============================] - 0s - loss: 0.0577     
    Epoch 3/5
    2360/2360 [==============================] - 0s - loss: 0.0571     
    Epoch 4/5
    2360/2360 [==============================] - 0s - loss: 0.0587     
    Epoch 5/5
    2360/2360 [==============================] - 0s - loss: 0.0589     
    Epoch 1/5
    2360/2360 [==============================] - 0s - loss: 0.0543     
    Epoch 2/5
    2360/2360 [==============================] - 0s - loss: 0.0565     
    Epoch 3/5
    2360/2360 [==============================] - 0s - loss: 0.0528     
    Epoch 4/5
    2360/2360 [==============================] - 0s - loss: 0.0548     
    Epoch 5/5
    2360/2360 [==============================] - 0s - loss: 0.0576     
    Training baseline models ...
    Saving metrics ...
    Generate performace report ...



```python
from VISAR_model_utils_v2 import generate_performance_plot_ST
import seaborn as sns
plot_df = generate_performance_plot_ST('./logs/ST_2019_8_14_986/performance_metrics.csv')
```

```python
g = sns.catplot(x = 'task', y = 'value', hue = 'method',
                col = 'tt', row = 'performance',
                data = plot_df, kind = 'bar')
```


![png](output_11_0.png)


### process trained results for VISAR analysis


```python
from VISAR_model_utils_v2 import generate_RUNKEY_dataframe_ST
RUN_KEY = 'ST_2019_8_14_986'
log_path = './logs/'
prev_model = log_path + RUN_KEY + '/T107_rep2_50.hdf5'
output_prefix = 'T107_rep2_50_'
task_list = ['T107']
add_features = None
dataset_file = log_path + RUN_KEY + '/temp.csv'
FP_type = 'Circular_2048'
```


```python
generate_RUNKEY_dataframe_ST(prev_model, output_prefix, task_list, dataset_file, FP_type, add_features,
                             n_layer = 1)
```

    ------------- Loading dataset --------------------
    Extracted dataset shape: (2951, 3)
    Loading raw samples now.
    shard_size: 8192
    About to start loading CSV from ./logs/ST_2019_8_14_986/temp.csv
    Loading shard 1 of size 8192.
    Featurizing sample 0
    Featurizing sample 1000
    Featurizing sample 2000
    TIMING: featurizing shard 0 took 11.564 s
    TIMING: dataset construction took 11.787 s
    Loading dataset from disk.
    ------------- Loading previous trained models ------------------
    WARNING:tensorflow:From /root/anaconda3/envs/deepchem/lib/python3.5/site-packages/keras/backend/tensorflow_backend.py:1108: calling reduce_mean (from tensorflow.python.ops.math_ops) with keep_dims is deprecated and will be removed in a future version.
    Instructions for updating:
    keep_dims is deprecated, use keepdims instead
    ------------- Prepare information for chemicals ------------------
    ------------- Prepare information for minibatches ------------------
    ------------- Prepare information for tasks ------------------
    ------- Generate color labels with default K of 5 --------
    -------------- Saving datasets ----------------



```python
import pandas as pd
import numpy as np

n_tasks = 1
batch_df = pd.read_csv('T107_rep2_50_batch_df.csv')
X = np.asarray(np.matrix(batch_df)[:,0:n_tasks]).reshape(-1)
order_x = np.asarray(np.argsort(X)).reshape(-1)
fit_data = X[order_x].T
[min_value, max_value] = [fit_data.min(), fit_data.max()]
rows_new = ['T107']
cols = batch_df['Label_id'].tolist()
cols_new = [str(cols[idx]) for idx in order_x]
```


```python
plot_dat = pd.DataFrame(np.asmatrix(fit_data))
plot_dat['task'] = rows_new
plot_dat = plot_dat.set_index('task')
plot_dat.columns = cols_new
plot_dat.columns.name = 'label'
plot_df = pd.DataFrame(plot_dat.stack(), columns = ['value']).reset_index()
```


```python
plot_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>task</th>
      <th>label</th>
      <th>value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>T107</td>
      <td>15</td>
      <td>0.0265525</td>
    </tr>
    <tr>
      <th>1</th>
      <td>T107</td>
      <td>77</td>
      <td>0.644419</td>
    </tr>
    <tr>
      <th>2</th>
      <td>T107</td>
      <td>14</td>
      <td>0.792068</td>
    </tr>
    <tr>
      <th>3</th>
      <td>T107</td>
      <td>47</td>
      <td>0.978062</td>
    </tr>
    <tr>
      <th>4</th>
      <td>T107</td>
      <td>94</td>
      <td>0.987944</td>
    </tr>
  </tbody>
</table>
</div>




```python
batch_selected = 5
plot_df.index[plot_df['label'] == str(batch_selected)].tolist()[0]
```




    [95]



Next:
- copy output files (including output_compound_df, output_batch_df, output_task_df) to a data directory, and clear the VISAR_webapp static directory if neccessary;
- start the app in prompt window by 'bokeh serve --show VISAR_webapp'


## Train robust multitask regressor model


```python
import os
import deepchem as dc
import numpy as np
import pandas as pd
import tensorflow as tf
os.environ['CUDA_VISIBLE_DEVICES']='1'
```

### model setup


```python
from model_training_utils_v2 import prepare_dataset
import os
import pandas as pd

RUN_KEY = 'Serotonin_Aug14'

log_path = './logs/' + RUN_KEY
os.system('mkdir %s' % log_path)
dataset_file = '%s/raw_data.csv' % (log_path)
MT_dat_name = './data/MT_data_clean_June28.csv'
FP_type = 'Circular_2048'
task_list = ['T51', 'T106','T107','T227', 'T108'] # 5HT-1a/1b/2a/2b/2c
#add_features = ['MW', 'logP', 'BertzCT', 'TPSA']

n_features = 2048
layer_sizes = [512, 64]
bypass_layer_sizes=[128]
bypass_dropouts = [.5]
dropout = 0.5
lr = 0.0005
```

### model training


```python
from keras.layers import Dense, Input
from keras.layers.core import Dropout
from keras.models import Model
from keras.optimizers import Adam
from keras.callbacks import ModelCheckpoint

from model_training_utils_v2 import prepare_dataset

def RobustMT_model_training(MT_dat_name, FP_type, task_list, log_path, epoch_num = 10,
                            n_features, layer_sizes, bypass_layer_sizes, bypass_dropouts, dropout, lr,
                            N_test = 500.0, add_features = None, n_epoch = 40):

    dataset_file = '%s/raw_data.csv' % (log_path)
    if len(task_list) > 1:
        model_flag = 'MT'
    else:
        model_flag = 'ST'
    dataset, df = prepare_dataset(MT_dat_name, task_list, dataset_file, FP_type,
                              smiles_field = 'canonical_smiles',
                              add_features = add_features,
                              id_field = 'chembl_id', model_flag = model_flag)

    # calculate the ratio of missing values in the multitask setting
    weights = dataset.w
    true_cnt = sum(sum(weights))
    missing_ratio = 1 - (true_cnt / (weights.shape[0] * weights.shape[1]))
    print('Missing ratio of the dataset is %.2f' % missing_ratio)
    print('Number of valid samples is %d' % int(true_cnt))

    # split dataset
    frac_train = 1 - N_test / dataset.X.shape[0]
    splitter = dc.splits.RandomSplitter(dataset_file)
    train_dataset, test_dataset = splitter.train_test_split(dataset, frac_train = frac_train)

    metric = dc.metrics.Metric(
        dc.metrics.r2_score, np.mean, mode = 'regression')

    # model training
    n_tasks = len(task_list)
    model = dc.models.RobustMultitaskRegressor(n_tasks = n_tasks, n_features = n_features, layer_sizes = layers_sizes,
                                               bypass_layer_sizes=bypass_layer_sizes, bypass_dropouts = bypass_dropouts,
                                               dropout = dropout, learning_rate = lr)
    model.save_file = log_path + '/model'

    train_evaluation = []
    test_evaluation = []
    for iteration in range(epoch_num):
        model.fit(train_dataset, n_epoch = n_epoch, max_checkpoints_to_keep = 1, checkpoint_interval=20)
        print('======== Iteration %d ======' % iteration)
        print("Evaluating model")
        train_scores = model.evaluate(train_dataset, [metric], [], per_task_metrics=metric)
        train_evaluation.append(train_scores[1]["mean-r2_score"])
        print("Training R2 score: %f" % train_scores[0]["mean-r2_score"])
        test_scores = model.evaluate(test_dataset, [metric], [], per_task_metrics=metric)
        test_evaluation.append(test_scores[1]["mean-r2_score"])
        print("Test R2 score: %f" % test_scores[0]["mean-r2_score"])

        # save evaluation scores (need test!)
        train_df = pd.DataFrame(np.array(train_evaluation))
        test_df = pd.DataFrame(np.array(test_evaluation))
        train_df.to_csv(model.save_file + '_train_log.csv')
        test_df.to_csv(model.save_file + '_test_log.csv')
    return
```


```python
RobustMT_model_training(MT_dat_name, FP_type, task_list, log_path, epoch_num = 10, N_test = 500.0, add_features = None, n_epoch = 40)
```
    Extracted dataset shape: (6451, 7)
    Loading raw samples now.
    shard_size: 8192
    About to start loading CSV from ./logs/Serotonin_Aug14/raw_data.csv
    Loading shard 1 of size 8192.
    Featurizing sample 0
    Featurizing sample 1000
    Featurizing sample 2000
    Featurizing sample 3000
    Featurizing sample 4000
    Featurizing sample 5000
    Featurizing sample 6000
    TIMING: featurizing shard 0 took 25.922 s
    TIMING: dataset construction took 26.403 s
    Loading dataset from disk.
    Missing ratio of the dataset is 0.69
    Number of valid samples is 10088
    Computing train/valid/test indices
    TIMING: dataset construction took 0.634 s
    Loading dataset from disk.
    TIMING: dataset construction took 0.258 s
    Loading dataset from disk.
    ======== Iteration 0 ======
    Evaluating model
    computed_metrics: [0.759661163147638, 0.8487385832755133, 0.7927909892421446, 0.7862606713724265, 0.7956715188464687]
    Training R2 score: 0.796625
    computed_metrics: [0.5752939701171875, 0.6945875534170977, 0.6066652533645858, 0.37844448486926474, 0.6625924044094114]
    Test R2 score: 0.583517
    ======== Iteration 1 ======
    Evaluating model
    computed_metrics: [0.8158505208724209, 0.8933779909691956, 0.861604485570545, 0.863322781629866, 0.8612575332340614]
    Training R2 score: 0.859083
    computed_metrics: [0.6115972878457685, 0.679947886147258, 0.6161329373560849, 0.34145438760506797, 0.6974210306560764]
    Test R2 score: 0.589311
    ======== Iteration 2 ======
    Evaluating model
    computed_metrics: [0.8548260530951476, 0.9267161642164854, 0.8862006766204017, 0.9050664020898176, 0.9002187486642536]
    Training R2 score: 0.894606
    computed_metrics: [0.6032906919973067, 0.6598824576132678, 0.618635857697012, 0.3220733219525531, 0.7186496734891725]
    Test R2 score: 0.584506
    ======== Iteration 3 ======
    Evaluating model
    computed_metrics: [0.864321517262086, 0.9397616613245016, 0.9095721444843354, 0.9238929926683908, 0.9167479937892477]
    Training R2 score: 0.910859
    computed_metrics: [0.6027677773255737, 0.6413089451338263, 0.5980630288323545, 0.32247924099147907, 0.7210531590609532]
    Test R2 score: 0.577134
    ======== Iteration 4 ======
    Evaluating model
    computed_metrics: [0.8953533936281479, 0.9534278679065692, 0.9215748228809671, 0.9290959487772327, 0.9301747706191565]
    Training R2 score: 0.925925
    computed_metrics: [0.6155427222670634, 0.6820633227742317, 0.6073417748208587, 0.3246368070900857, 0.7268722175042581]
    Test R2 score: 0.591291
    ======== Iteration 5 ======
    Evaluating model
    computed_metrics: [0.9029854417257765, 0.9604575943054542, 0.9331062337742813, 0.9284163299055174, 0.9388175501780647]
    Training R2 score: 0.932757
    computed_metrics: [0.6069236074254009, 0.714621862394654, 0.5836234677856831, 0.28948690011193723, 0.7211811441306561]
    Test R2 score: 0.583167
    ======== Iteration 6 ======
    Evaluating model
    computed_metrics: [0.9171476271850806, 0.9653620693031328, 0.9429793121516797, 0.9437126330935732, 0.943749722922384]
    Training R2 score: 0.942590
    computed_metrics: [0.6105522209362191, 0.7095486611172115, 0.5890714681689975, 0.31531457007535124, 0.709942904059075]
    Test R2 score: 0.586886
    ======== Iteration 7 ======
    Evaluating model
    computed_metrics: [0.926968619508657, 0.9638583261828477, 0.9480784696063547, 0.9463554566177619, 0.9519935893626453]
    Training R2 score: 0.947451
    computed_metrics: [0.6206203893849938, 0.7097037146131558, 0.5916103154555746, 0.2890225797452647, 0.7278600725600175]
    Test R2 score: 0.587763
    ======== Iteration 8 ======
    Evaluating model
    computed_metrics: [0.9344494765668044, 0.9707340451387145, 0.95448494227031, 0.9407190704283092, 0.9540568289914396]
    Training R2 score: 0.950889
    computed_metrics: [0.6148857396669338, 0.7325904076146383, 0.5878479022455009, 0.2916763885710445, 0.7200587355168194]
    Test R2 score: 0.589412
    ======== Iteration 9 ======
    Evaluating model
    computed_metrics: [0.9366193912320537, 0.9579353393749691, 0.9569519113034609, 0.9525346894456956, 0.9535840932986404]
    Training R2 score: 0.951525
    computed_metrics: [0.6153497824407556, 0.6753012257001534, 0.5834952261590962, 0.3384884190099773, 0.7063542992615486]
    Test R2 score: 0.583798



```python
# visualize the evaluation scores along the training process
import seaborn as sns
import pandas as pd
from VISAR_model_utils_v2 import generate_performance_plot_RobustMT

plot_df = generate_performance_plot_RobustMT(train_file = './logs/Serotonin_Aug14/model_train_log.csv',
                                             test_file = './logs/Serotonin_Aug14/model_test_log.csv',
                                             task_list = ['T51', 'T106','T107','T227', 'T108'])
```


```python
import matplotlib.pyplot as plt
g = sns.FacetGrid(plot_df, col = 'tt', hue = 'tasks')
g = (g.map(plt.plot, 'step', 'R2', marker = '.')).add_legend()
```


![png](output_7_0.png)


### process trained results for VISAR analysis


```python
from VISAR_model_utils_v2 import generate_RUNKEY_dataframe_RobustMT
prev_model = './logs/Serotonin_Aug14/model-1200'
RUNKEY = './logs/Serotonin_Aug14/'

task_list = ['T51', 'T106','T107','T227', 'T108'] # 5HT-1a/1b/2a/2b/2c
#add_features = ['MW','logP','BertzCT','TPSA']
dataset_file = '%s/raw_data.csv' % (RUNKEY)
MT_dat_name = './data/MT_data_clean_June28.csv'
FP_type = 'Circular_2048'
model_flag = 'MT'

n_features = 2048
layer_sizes = [512, 64]
bypass_layer_sizes=[128]
bypass_dropouts = [.5]
dropout = 0.5
learning_rate = 0.001
n_layer = 2
n_bypass = 2
add_features = None

output_prefix = RUNKEY + '/RobustMT_serotonin_output_'
```


```python
generate_RUNKEY_dataframe_RobustMT(prev_model, output_prefix, task_list, dataset_file, FP_type, add_features,
                                   n_features, layer_sizes, bypass_layer_sizes, model_flag, n_bypass, n_layer = n_layer)
```

    ------------- Loading dataset --------------------
    Extracted dataset shape: (6451, 7)
    Loading raw samples now.
    shard_size: 8192
    About to start loading CSV from ./logs/Serotonin_Aug14//raw_data.csv
    Loading shard 1 of size 8192.
    Featurizing sample 0
    Featurizing sample 1000
    Featurizing sample 2000
    Featurizing sample 3000
    Featurizing sample 4000
    Featurizing sample 5000
    Featurizing sample 6000
    TIMING: featurizing shard 0 took 24.339 s
    TIMING: dataset construction took 24.805 s
    Loading dataset from disk.
    ------------- Loading previous trained models ------------------
    INFO:tensorflow:Restoring parameters from ./logs/Serotonin_Aug14/model-1200
    ------------- Prepare information for chemicals ------------------
    INFO:tensorflow:Restoring parameters from ./logs/Serotonin_Aug14/model-1200
    WARNING:tensorflow:From /root/anaconda3/envs/deepchem/lib/python3.5/site-packages/keras/backend/tensorflow_backend.py:1108: calling reduce_mean (from tensorflow.python.ops.math_ops) with keep_dims is deprecated and will be removed in a future version.
    Instructions for updating:
    keep_dims is deprecated, use keepdims instead
    ------------- Prepare information for minibatches ------------------
    ------------- Prepare information for tasks ------------------
    INFO:tensorflow:Restoring parameters from ./logs/Serotonin_Aug14/model-1200
    INFO:tensorflow:Restoring parameters from ./logs/Serotonin_Aug14/model-1200
    INFO:tensorflow:Restoring parameters from ./logs/Serotonin_Aug14/model-1200
    INFO:tensorflow:Restoring parameters from ./logs/Serotonin_Aug14/model-1200
    INFO:tensorflow:Restoring parameters from ./logs/Serotonin_Aug14/model-1200
    INFO:tensorflow:Restoring parameters from ./logs/Serotonin_Aug14/model-1200
    ------- Generate color labels with default K of 5 --------
    -------------- Saving datasets ----------------


Next:
- copy output files (including output_compound_df, output_batch_df, output_task_df) to VISAR_webapp data directory, and clear the static directory if neccessary;
- start the app in prompt window by 'bokeh serve --show VISAR_webapp'


## pharmacophore model analysis for selected batches


```python
import os
import numpy as np
import pandas as pd
```

### generate sdf file of selected batches


```python
compound_df = pd.read_csv('T107_rep2_50_compound_df.csv')

selected_batch = 0
select_df = compound_df.loc[compound_df['label'] == selected_batch]
select_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>T107</th>
      <th>chembl_id</th>
      <th>canonical_smiles</th>
      <th>x</th>
      <th>y</th>
      <th>pred_T107</th>
      <th>label</th>
      <th>batch_label_color</th>
      <th>batch_label</th>
      <th>label_color</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1800</th>
      <td>3.753245</td>
      <td>CHEMBL3664821</td>
      <td>O=C(CC1CC1)N[C@@H]2CC[C@@H](CCN3CCC(CC3)c4cccc...</td>
      <td>-39.537960</td>
      <td>-6.644773</td>
      <td>3.695881</td>
      <td>0</td>
      <td>#1f77b4</td>
      <td>2</td>
      <td>#393b79</td>
    </tr>
    <tr>
      <th>1852</th>
      <td>3.662531</td>
      <td>CHEMBL3664872</td>
      <td>O=C(N[C@@H]1CC[C@@H](CCN2CCC(CC2)c3cccc4OCOc34...</td>
      <td>-39.001064</td>
      <td>-1.747338</td>
      <td>3.654126</td>
      <td>0</td>
      <td>#1f77b4</td>
      <td>2</td>
      <td>#393b79</td>
    </tr>
    <tr>
      <th>2306</th>
      <td>3.787004</td>
      <td>CHEMBL3697951</td>
      <td>CC(=O)N[C@@H]1CC[C@@H](CCN2CCN(CC2)c3cccc4OCOc...</td>
      <td>-39.745975</td>
      <td>2.005099</td>
      <td>3.765032</td>
      <td>0</td>
      <td>#1f77b4</td>
      <td>2</td>
      <td>#393b79</td>
    </tr>
    <tr>
      <th>2307</th>
      <td>3.673449</td>
      <td>CHEMBL3697952</td>
      <td>COCCC(=O)N[C@@H]1CC[C@@H](CCN2CCN(CC2)c3cccc4O...</td>
      <td>-37.736317</td>
      <td>2.435640</td>
      <td>3.635353</td>
      <td>0</td>
      <td>#1f77b4</td>
      <td>2</td>
      <td>#393b79</td>
    </tr>
    <tr>
      <th>2311</th>
      <td>3.780144</td>
      <td>CHEMBL3697956</td>
      <td>O=C(CC1CCCO1)N[C@@H]2CC[C@@H](CCN3CCN(CC3)c4cc...</td>
      <td>-40.699070</td>
      <td>0.664541</td>
      <td>3.700971</td>
      <td>0</td>
      <td>#1f77b4</td>
      <td>2</td>
      <td>#393b79</td>
    </tr>
  </tbody>
</table>
</div>




```python
from rdkit import Chem
from rdkit.Chem import PandasTools

def df2sdf(df, output_sdf_name,
           smiles_field = 'canonical_smiles', id_field = 'chembl_id',
           selected_batch = None):
    '''
    pack pd.DataFrame to sdf_file
    '''
    if not selected_batch is None:
        df = df.loc[df['label'] == selected_batch]
    PandasTools.AddMoleculeColumnToFrame(df,smiles_field,'ROMol')
    PandasTools.WriteSDF(df, output_sdf_name, idName=id_field, properties=df.columns)

    return
```


```python
df2sdf(select_df, 'T107_rep2_50_batch0.sdf', selected_batch = 0)
```

### Building pharmacophore models using [Align-it](http://silicos-it.be.s3-website-eu-west-1.amazonaws.com/software/align-it/1.0.4/align-it.html)

#### prepare 3D coordinates for ligands


```python
from rdkit import Chem
from rdkit.Chem import AllChem
from rdkit.Chem import Draw
raw_sdf_file = 'T107_rep2_50_batch0.sdf'
ms = [x for x in Chem.SDMolSupplier(raw_sdf_file)]
```


```python
n_conf = 5
w = Chem.SDWriter('T107_rep2_50_batch0_rdkit_conf.sdf')
for i in range(n_conf):
    ms_addH = [Chem.AddHs(m) for m in ms]
    for m in ms_addH:
        AllChem.EmbedMolecule(m)
        AllChem.MMFFOptimizeMoleculeConfs(m)
        w.write(m)
```

#### from prepared 3D ligands to representative pharmacophores


```python
from align_it_utils import proceed_pharmacophore
import os
```


```python
home_dir = './data/'
result_dir = home_dir + 'Label13_rdkit_phars/'
sdf_file = '/Users/dingqy14/Desktop/writing/P2_data_Dec27/code_utils_v1/data/Label13_rdkit_conf.sdf' # absolute path is prefered!
output_name = 'Cluster13_'
```


```python
proceed_pharmacophore(home_dir, sdf_file, result_dir, output_name)
```

You can also building pharmacophore models using [TeachOpenCADD platform](https://github.com/volkamerlab/TeachOpenCADD), or analyze the selected ligands with other informatic tools, eg. [DataWarrior](http://www.openmolecules.org/datawarrior/), [Schrodinger](https://www.schrodinger.com/drug-discovery)
