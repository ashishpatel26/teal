# Teal - Audio Processing Layers for TensorFlow

Create TensorFlow layers and models for audio preprocessing and audio data augmentation:

:heavy_check_mark: No dependency other than TensorFlow

:heavy_check_mark: Can utilize GPU

:heavy_check_mark: Online preprocessing and data augmentation

:heavy_check_mark: Deploy preprocessing logic in production with the saved model

__Teal__ is in very early stage and a _lot_ of work is to be done. Please feel free to reach out if you'd like to help out!! :smile:

## Getting Started

Install would be using `pip`:

`pip install --user git+https://github.com/am1tyadav/teal.git`

### Preprocessing Model - Log Mel Spectrogram

```python
import tensorflow as tf
import teal

NUM_SAMPLES = 44100
SAMPLE_RATE = 22050
N_FFT = 2048
HOP_LEN = 512
N_MELS = 64

log_mel_model = tf.keras.models.Sequential([
    tf.keras.layers.Input(shape=(NUM_SAMPLES, )),
    teal.MelSpectrogram(SAMPLE_RATE, N_FFT, HOP_LEN, N_MELS),
    teal.PowerToDb()
])

# Save it as a Keras model or TF saved model
log_mel_model.save("log_mel.h5")
```

### Audio Data Augmentation Model

```python
import tensorflow as tf
import teal

NUM_SAMPLES = 44100

audio_augmentation_model = tf.keras.models.Sequential([
    tf.keras.layers.Input(shape=(NUM_SAMPLES, )),
    teal.InversePolarity(0.5),
    teal.RandomNoise(0.2),
    teal.RandomGain(0.5)
])
```

For a detailed example, please take a look at [this notebook](examples/Audio%20Classifier.ipynb)

## Layers

### Preprocessing layers

* STFT - Computes Short Time Fourier Transform
* STFTToSpectrogram - Computes power spectrum from STFT - _untested_
* STFTToPhase - Computes phase from STFT - _untested_
* Spectrogram - Computes power spectrum from audio
* MelSpectrogram - Computes mel spectrogram from audio
* SpectrogramToMel - Computes mel spectrogram from power spectrum - _untested_
* PowerToDb - Scales the power spectrum to db range
* NormalizeAudio - Scales audio to a range of (-1, 1)
* NormalizeSpectrum - Scales spectrogram to a range of (-1, 1)

### Postprocessing Layers

* DbToPower - Scales the db magnitude to power spectrum - _untested_
* MelToSTFT - Converts mel power spectrogram to STFT magnitude - _WIP_
* GriffinLim - Converts STFT magnitude to audio using Griffin Lim algorithm - _WIP_

### Data augmentation layers

* InversePolarity - Inverts polarity of input audio
* RandomGain - Applies different random gain to different examples in a batch
* RandomNoise - Applies random noise to audio samples - _untested_
* NoiseBank - Applies noise from user given 16-bit WAV file - _untested_
* PitchShift - Applies random shift to the pitch of input audio - _untested_
* Reverb - _WIP_
