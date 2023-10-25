---
title: "Vocoder feature demonstrations"
author: "Matthew Winn"
---

## [***Vocoder Feature Demonstrations***]

## **Envelope low pass filtering**

Begin with a short sound (1-2 seconds long). On the startup window, choose to preserve components. Vocode a sound whose envelope has enough modulation activity for the LPF to make a difference (e.g. a sound with F0 220 Hz when your LPF is 50 Hz). Use a sinewave carrier, and choose an envelope cutoff filter of 50 Hz. Start the script. When the window pops up, choose to preserve original envelope and LPF envelope.

![](https://raw.githubusercontent.com/ListenLab/Vocoder/main/images/preserve_env_orig_and_LPF.png)

Examine a single channel (e.g. channel 7) for both the original and LPF env outputs.

The sinewave carrier makes it easy to see the effects of the filtering. Unrelated fluctuations from other carriers (noise, harmonic complex) will be inherited into the output channel, making it harder to cleanly see the effects of your envelope filtering.

![](https://raw.githubusercontent.com/ListenLab/Vocoder/main/images/combine_filtered_envelopes_to_stereo.png){width="422"}

![](https://raw.githubusercontent.com/ListenLab/Vocoder/main/images/envelope_LPFed.png){width="474"}

## **Effect of carrier type on output envelope**

Start with a short sound that has some envelope modulation (e.g. a CNC word).

On the startup window, choose to preserve components, and then choose to preserve *carrier channels*. Vocode using a sinewave carrier.

Keep all the parameters the same, but choose a different carrier {noise, LNN, harmonic complex}

Examine the full outputs, or examine the same channel (e.g. channel 7) from each iteration of the process. To do this, you want to combine those outputs to stereo, but first you need to ensure that they have the same sampling frequency, as illustrated below where they are resampled to 44100 Hz. When you have both sound objects selected, click Convert \> Resample...

![](https://raw.githubusercontent.com/ListenLab/Vocoder/main/images/resample_both.png)

With both outputs already selected, combine to stereo by clicking Combine \> Combine to stereo, as shown below:

![](https://raw.githubusercontent.com/ListenLab/Vocoder/main/images/compare_same_channel_two_carriers.png){width="408"}

Then open up the combined-to-stereo sound to view the differences in envelope. Below you can see carriers for a sinewave carrier (top) and a noise carrier (bottom).

![](https://raw.githubusercontent.com/ListenLab/Vocoder/main/images/same_channel_two_carriers_wave.png){width="600"}

## **Effect of channel bandwidth on carrier modulation depth**

Show how number of channels influences channel width, which influences envelope modulations

Preserve analysis channels and original envelopes

Vocode the same sound using a sinewave carrier, with rectangular analysis filters (analysis filter slope = 0). Allow the envelope cutoff frequency to be high (around 300 Hz). Vocode with different number of channels (e.g. once with 4 channels and once with 12 channels).

For a channel whose center frequency is about 10-15x the talker's fundamental frequency, examine that channel in both of the outputs. This will be a different number for both iterations of the vocoder process. For example, channel 2 is a 4-channel vocoder might be compared to channel 6 in a 12-channel vocoder).

You will observe how the modulation depth is greater in the channel from the vocoder with fewer channels, because the analysis filter spans a wider bandwidth, which corresponds to envelope modulations.

## **Carrier filter slopes**

Create 8-second harmonic complex with F0 of 50 Hz for dense spectral coverage (this will take a few seconds in praat). The reason for this is to create an input sound whose spectrum is flat, so that the shape of the carriers can be compared cleanly. White noise will not allow for this clean comparison unless it is a very long sound (i.e. more than 12 seconds long).

Vocode with 6 channels using a harmonic complex carrier that has a specific controlled filter rolloff. For example 12 dB per octave. Then repeat with the same parameters except change the filter rolloff to 24 dB/octave. Each time, choose to preserve the carrier channels

[*Note 1*]{.underline}*: you can also do this with a filter operating as dB/mm, but it's much harder to track that in the output*

[*Note 2*]{.underline}*: You can also do this with a noise carrier, but the results will be somewhat tainted by the inherent spectral irregularities of noise*

In the outputs, choose a specific channel (e.g. channel 3), and compare the spectra of both sounds. You will notice a difference in the steepness of the rolloff from the center of the channel.

You can also look at the spectrum of the \*entire\* vocoded sound. To do this, select the output vocoded sound in the object list, then click Analyze spectrum \> To spectrum... (and you can leave the "fast" checkbox enabled).

View the spectra of both vocoded sounds to observe the same number of channel peaks, which have different steepness.

## **Peak picking**

Start with a relatively short sentence (about 2 -- 4 seconds long)

Vocode with sinewave carrier, where number of stimulated channels is 8, and number of channels is 22.

Use 30ms as the time bin for peak picking.

Repeat the process for the same sound, using the same parameters, except this time make both of the numbers equal 22. Compare the outputs below (for the vocoded word "handstand") to observe how the 8/22 vocoder (on the left) leaves some parts of the spectrogram blank, which are filled in for the 22/22 vocoder on the right.

![](https://raw.githubusercontent.com/ListenLab/Vocoder/main/images/peak_picked_spectrogram.png)

You can also complete this process with other types of carrier (e.g. noise), but the effects are slightly more difficult to observe on the spectrogram (but they operate exactly the same across all carriers).

## **Envelope compression**

Vocode a sound with 6 channels using a sinewave carrier. These parameters are not essential, but they will make it easy to view the effect being used. It would be best to use a sound that has modulations, such as a short sentence (2-4 seconds long).

Check the "envelope compression or quantization" box.

Check the box to preserve components, and choose to preserve the LPF envelopes.

First vocode the sound while leaving the "compression_mult" value at 1. You do not need to save the components.

Then, use all the same exact settings, but change the "compression_mult" value to be 0.5 or lower.

You can compare the full vocoded sound output in each case, and for more detail, you can compare the same exact channel from both levels of compression. For example, you can ctrl+select both instances of "env_ch_4_3_compressed_LPF", and then click Combine \> Combine to stereo. Open up that stereo file to see that the envelopes have different modulation depths (you might want to disable the spectrogram in the sound editor window, to get a clearer image of the envelopes).

![Comparison of envelope compression in the full soundwave](https://raw.githubusercontent.com/ListenLab/Vocoder/main/images/envelope_compression_waveform.png){width="408"}

## **Envelope quantization**

Follow the same steps as described above for envelope compression, but leave the compression value at 1, and change the number of intensity steps to be something low like 2 or 3. Preserve the carrier channels.

(this process happens after the envelope step, so the LPF envelope is not useful for viewing quantization)

## **Interaural decorrelation**

No need to preserve individual components for inspection.

Vocode using a carrier other than sinewave or harmonic complex. Vocode again. Ctrl+select both full outputs, click Combine \> Combine to stereo. Zoom in to view how the temporal fine structure is decorrelated, even though the envelopes should be roughly the same.

For precise control over envelope fidelity, you can do this procedure with Low-noise noise carriers.

You cannot create decorrelated vocoded signals with sinewave carriers or harmonic complex carriers because the carriers will be entirely formulaic and therefore the same for each iteration of the vocoding process, and therefore perfectly correlated in stereo channels.
