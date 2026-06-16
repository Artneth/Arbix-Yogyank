# Audit Memo: Yogyank Entitlement Score Model

## 1. What was dangerous in the original script?
The original script contained several critical flaws that made the reported validation score entirely untrustworthy:
*    The feature 'defaulted_in_next_12_months' was included in the training set. This is future information that would definitively not be available at the time of scoring. 
*    The script modified target 'target_entitlement_score' directly during training by subtracting 150 points. This prevents the ML model from learning the underlying data patterns.
*    LabelEncoder was fitted on the entire dataset before the train-test split. This causes information from the test set to leak into the training process. Also we should use One hot Encoder for Nominal data.
*    Using Random shuffling on time-dependent data allows the model to train on future data to predict past events. 
*    The script only saved the XGBoost model. It dropped the encoder and failed to save a pipeline.
