# DeepForcedAligner

With this tool you can create accurate text-audio alignments given a bunch of audio files and their transcription. The alignments can for example be used to train text-to-speech models such as 
[FastSpeech](https://arxiv.org/abs/1905.09263?utm_source=feedburner&utm_medium=feed&utm_campaign=Feed%253A+arxiv%252FQSXk+%2528ExcitingAds%2521+cs+updates+on+arXiv.org%2529). In comparison to other forced alignment tools this repo has following advantages:

- **Polyglot:** By design, the DFA is language-agnostic and can align charater or phonemes.
- **Robustness:** Alignment has a high tolerance for text errors and special (silent) characters.
- **Convenience:** Easy installation with no extra dependencies. You can provide your own data in the standard LJSpeech format with no extra preprocessing (such as phoneme mappings etc.).
## Installation

Running on Python >=3.6

```
pip install -r requirements.txt
```

## Example Data Prep and Model Training 
(1) Download the [LJSpeech](https://keithito.com/LJ-Speech-Dataset/) dataset, set paths in config.yaml:
```
  dataset_dir: LJSpeech
  metadata_path: LJSpeech/metadata.csv
```
(2) Preprocess the data and train aligner:
```
  python preprocess.py
  python train.py
```
(3) Extract durations with latest model checkpoint (60k steps should be sufficient):
```
  python extract_durations.py
```
By default durations are put as numpy files into: 
```
  output/durations 
```
Each character duration correspons to one mel time step, which translates to hop_length / sample_rate seconds in the wav file.

## Tensorboard
You can monitor the training with 
```
  tensorboard dfa_checkpoints
```

## Using Preprocessed Mel Spectrograms
You can provide your own mel spectrogams by setting in the config.yaml:
```
  precomputed_mels: /path/to/mels
```
Make sure that the mel names match the ids in the metafile, e.g. 
```
  00001.mel ---> 00001|'First sample text'
```
