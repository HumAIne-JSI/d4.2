# Model Registry and Metadata

## Overview

Users can upload and store their datasets, models, and results to dedicated storage buckets designed to facilitate streamlined management and collaboration. There are 2 ways users can upload their files:

1. **Manually** through a user-friendly web interface that MinIO offers
2. **Programmatically** using API or CLI commands (through MinIO Client `mc` command line tool)

Moreover, custom APIs will be created to assist with the pilot specific needs related to the MinIO usage if needed. Concrete examples for *Deliverable 4.2* are provided below in subsection [Metadata Examples](#metadata-examples).

To maintain consistency and improve accessibility, users are required to include relevant metadata when uploading or storing files. Metadata about the AI models should describe key attributes related to datasets used, the model metadata, learning paradigm specific, performance and traceability metadata. An initial suggested format of metadata provided by the learning paradigms is presented below:

## Active Learning

- **Dataset UUID** `<string>`: UUID of the dataset in the data lake.  
- **Dataset Version** `<string>`: The version or timestamp of the dataset used. 
- **Model type** `<string>`: The type of model (e.g., neural network, SVM, random forest).  
- **Model Version** `<string>`: Version of the model or training pipeline used.  
- **Hyperparameters** `<JSON>`: All hyperparameter settings (e.g., learning rate, batch size).  
- **Training Dataset** `<[integer]>`: Subset of data used for training (indices, IDs).  
- **Validation Dataset** `<[integer]>`: Subset of data used for validation (indices, IDs). 
- **Query Strategy** `<string>`: The strategy used to select samples (e.g., uncertainty sampling, diversity sampling) and metrics or criteria for choosing the queried samples (e.g., entropy, margin of confidence).  
- **Iteration Samples** `<integer>`: The number of samples queried in each iteration. 
- **Seed Set** `<[integer]>`: The initial labelled dataset used to start active learning. 
- **Iteration Metrics** `<[iteration: integer, JSON: { "accuracy": float, ...}]>`: Accuracy, precision, recall, F1 score, and other relevant metrics at each iteration. The last record represents the final model performance metrics.
- **Timestamps*** `<JSON: [{ "action": string, "timestamp": timestamp}]>`: For each action, including dataset preparation, training, querying, and annotation.
- **Logging and Auditing*** `<string (blob)>`: Logs of decisions made by the algorithm and any manual overrides.  
- **Version Control*** `<string>`: Git hashes or other versioning identifiers for datasets, models, and code.  
- **Experiment Parameters** `[<string>]`: Unique IDs or names for each experiment run.  

## Swarm Learning

- **Participant ID** `<string>`: Unique identifier for each participant in the swarm.
- **Node Roles** `[<string>]`: Role in the swarm (e.g., leader, member).
- **Dataset UUID** `<string>`: UUID of the dataset in the data lake.
- **Dataset Version** `<string>`: The version or timestamp of the dataset used.
- **Data Sensitivity** `<string>`: Privacy and security requirements for the participant's data.
- **Model Architecture** `<onnx>` or `<JSON>`: Shared architecture agreed upon by participants.
- **Global Hyperparameters** `<JSON>`: Global settings (e.g., learning rate, batch size).
- **Global Model Version** `<string>`: Version of the globally aggregated model.
- **Aggregation Algorithm** `<string>`: Technique used for combining local models (e.g., federated averaging, weighted aggregation).
- **Weight Contributions** `<JSON>`: Proportion of each node's contribution to the global model.
- **Aggregation Frequency** `<string>`: How often aggregation occurs.

## Neurosymbolic Learning

- **Dataset UUID** `<string>`: UUID of the dataset in the MinIO data lake.
- **Dataset Version** `<string>`: The version or timestamp of the dataset used.
- **Transformation Steps** `<[string]>`: Custom labels representing the preprocessing and transformations applied to raw data.
- **Rules File** `<string>`: Path to the JSON containing a representation of the rules used for the current training run. The file name will include an ID for the training run.
- **Model Type** `<string>`: Type of neural network (e.g., CNN, RNN, Transformer).
- **Model Version** `<string>`: Versioning of model referring to the HumAIne model repository (most likely model in ONNX format).
- **Layer Details** `<[JSON]>`: List of json objects with layer index, layer types, layer configuration arguments, next layer indices.
- **Hyperparameters** `<JSON>`: Json object containing various float/string values such as learning rate, batch size, epochs, etc.
- **Optimizer** `<JSON>`: Type(e.g., Adam, SGD) and configuration, loss metric
- **Loss Metric** `<string>`: Loss metric that is minimized (i.e. name of the loss function)
- **Epoch Statistics** `<JSON>`: Contains loss metric value for specific epoch checkpoints.
- **Knowledge Representation** `<String>`: Knowledge representation for the rules to be useable by the model OR name of the converter method used to convert the rules from standard into model-specific format (e.g., Probabilistic SDD for the SPL method).
- **Satisfiability Scores** `<JSON>`: Satisfiability of ruleset at specific checkpoints during training. 
- **Seed Value** `<integer>`: seed used for training  
- **Training Indices** `<[integer]>` or `<JSON>`: data instances used for training (perhaps per cross-validation fold)
- **Validation Indices** `<[integer]>` or `<JSON>`: data instances used for validation (perhaps per cross-validation fold) 
- **Post-training Evaluation Metrics** `<JSON>`: Evaluation metrics calculated post-training on validation set(s).  
- **Training Time** `<Float>`: Time spent during training.
- **Training Time per Epoch** `<Float>`: Time spent per epoch during training.
- **Training Failure Cases** `<[Integer]>`: Indices of mislabeled training set predictions after training  
- **Validation Failure Cases** `<[Integer]>`: Indices of mislabeled validation set predictions after training

> **Note:** All the metadata per learning paradigm stated above can be enriched/modified accordingly during the integration phase, if needed.


## Metadata Examples

This section contains example metadata files for different learning components of the system, including active learning and neurosymbolic learning models. These files serve as templates as well as illustrations of metadata description of these models.

### Active Learning Model Metadata

- [active_learning_model_metadata_smart_energy.json](./active_learning_model_metadata_smart_energy.json): Metadata for an active learning model applied to a smart energy dataset.
- [active_learning_model_metadata_oncology_example.json](./active_learning_model_metadata_oncology_example.json): Metadata for an active learning model applied to an oncology dataset.
- [active_learning_model_metadata_ticketing_dispatch.json](./active_learning_model_metadata_ticketing_dispatch.json): Metadata for an active learning model applied to a ticketing and dispatch dataset.

### Neurosymbolic Learning Model Metadata

- [neurosymbolic_learning_model_metadata_example.json](./neurosymbolic_learning_model_metadata_example.json): Metadata for a neurosymbolic learning model example.