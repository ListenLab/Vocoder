---
title: "Praat Vocoder intro"
author: "Matthew Winn"
format: html
---

# Praat Vocoder

This script allows you to vocode speech using the free open-source software Praat. Vocoding typically involves some sort of spectral and/or temporal degradation of the speech signal, and is commonly used in experiments relating to cochlear implants. Although vocoded speech is not a perfect simulation of the sound of a cochlear implant, it can help us simulate some aspects of CI processing to see how they affect auditory perception.

## Basic features

With this script you can:

-   Change carrier type (sinewave, noise, low-noise noise, harmonic complex)

-   Change number of channels

-   Use channel peak-picking (i.e. n-of-m)

-   Control spread of excitation for each carrier channel

-   Control interaction of analysis channels (very useful for sinewave carriers)

-   Change temporal envelope fidelity (low-pass filtering, quantization, compression)

-   Simulate specific device channel-frequency allocation

-   Spectral shift (i.e. simulate shallow device insertion)

-   preserve components that are assembled along the way, for offline inspection, analysis, and plotting

# How to call up and run the script

the script is available for download here:

https://github.com/ListenLab/Vocoder/blob/main/praat_vocoder.txt

You can call up the script in two ways.

1\) save the contents of that page to a text file and use Praat to call it up by clicking Praat \> Open Praat script... and locate the file on your computer

2\) In Praat, click Praat \> New praat script, and paste the script into the script window

then, in the script window, click Run \> Run (or ctrl-R on a PC) to start the script.

# The startup window

![Praat vocoder startup window](https://raw.githubusercontent.com/ListenLab/Vocoder/main/images/startup_window.png){width="453px"}

### Some parameters that deserve explanation:

#### Number stimulated

the number of channels activated at any particular time

#### Number of channels

the number of channels analyzed in the signal

USUALLY you will have these numbers be the same. If you want a 12-channel vocoder, then you will have 12 in each of these boxes.

##### Peak-picking

If you want a peak-picking vocoder, then the number of stimulated channels will be lower than the number of channels. For example, if you want 22 analysis channels, but want to activate only 8 channels at any moment, you will have 8 and then 22. If you do this, then you'll get a follow-up window that looks like this:

![peak-picking time bin window](https://raw.githubusercontent.com/ListenLab/Vocoder/main/images/peak_picking_time_bin_window.png){width="429px"}

This setting allows to determine how often the channels will be compared for the purpose of peak-picking. A value of "30" means that the channels will be compared in successive 30-millisecond time windows. This is probably about as short as you want to make it, since spoken language rarely contrasts things that are less than 30 ms apart in a meaningful way (although note that important structured and idiosyncratic variation does occur). A value of "300" means that everything within a 150ms window will be used to pick channels, and this will have a severely damaging effect on your ability to understand the speech, since everything within that long time window will be blurred.

#### Analysis Filter Slope

When the original speech signal is divided into frequency bands, you can think of those bands as being contiguous rectangles, or sloped triangle-ish shapes that partially overlap. Rectangle-shaped analysis channels mean that each channel is independent form its neighbors, and this is the one that is easiest to visualize and conceptualize. For rectangular analysis filters, keep this setting at 0.

If you want triangle-ish shaped analysis filters, then you set this to be a number that reflects the steepness of the slope away from the center of the channel. The center of the channel always maintains full spectral energy from the original sound. A number of 6 means that one octave away from the center, the gain of the filter drops by 6 dB. This is a very shallow filter that would cause interaction between adjacent channels. A sharper filter (say, 36 dB/octave) would keep the channels more independent.

The way to think about these overlapping filters is that each channel's envelope reflects input from a range of frequencies, and if the range for channel 7 overlaps with the range for channel 8, then those channels are said to "interact". More channel interaction means poorer spectral resolution - not because you have a smaller number of output channels or because you have channels that have wide area of excitation - but because each channel reflects inputs that spread across the input frequency range. See Oxenham & Kreft (2014) for more details.

[@Oxenham2014]Oxenham,A.J., & Kreft, H.A. (2014). Speech perception in tones and noise via cochlear implants. reveals influence of spectral resolution on temporal processing. Trends in Hearing, 18, 1--14.

Why would we use this? in cases where you have a sinewave carrier (where the spectral bandwidth of the carrier is rather narrow), but wish to impose some channel interaction because you know that is an important aspect of listening with a cochlear implant.

#### Carrier Type

The type of sound that ultimately represents the speech in the output. This can be sinewave, noise, low-noise noise, or harmonic complex.

Within this drop-down menu, there are further options specific for each type

![carrier drop-down menu](https://raw.githubusercontent.com/ListenLab/Vocoder/main/images/carrier_dropdown.png)

For carriers that are naturally broadband in nature, you have the option of shaping the output spectrum to be rectangular (i.e. contiguous), or sloped in a way that reflects spread of excitation.

#### Harmonic Complex F0

This field only matters if you have selected a harmonic complex as your carrier. The harmonic complex is generated with in-phase sinewaves that share a common fundamental, and you set that fundamental here. A lower fundamental (e.g. 50 Hz or below) results in a harmonic every 50 Hz, which is advantageous for dense spectral coverage (you can shape it very specifically), but disadvantageous in that it results in a perceptibly low pitch and slow rate of "pulsing" in the output. A fundamental between 100 and 200 Hz results in an output that sounds like a monotone robotic voice; this range will typically ensure 'adequate' spectral coverage. Fundamentals 300 Hz and higher will typically result in spectral components that are too dense to control very rigorously. For example, if your components exist at 400, 800, 1200, 1600, etc., then there is no way of carefully controlling the exact spectral shape in terms of peaked rolloff.

#### Peaked carrier rolloff (dB per oct or mm)

This field only matters if you have chosen a carrier that has a "controlled filter rolloff". The number you enter here will be the steepness fo your filter as dB/octave or dB/millimeter of cochlear space (the distinction between those two will be done automatically based on your choice of carrier).

For example: Suppose one of your carrier channels has a center frequency of 2000 Hz and your rolloff is 12 dB/octave. This means that if the center of the channel is 72 dB, then the energy for this channel at 1000 Hz and at 4000 Hz will be 60 dB. The energy at 500 Hz and at 8000 Hz will be 48 dB. This means that the channel centered at 2000 Hz will almost certainly have energy that overlaps with adjacent channels. The higher your rolloff rate, the steeper your carrier channel will be (i.e. the less it will interact with adjacent channels). But there is a point at which the channel cannot get much steeper. Consult the literature for values that make sense for your experiment.

#### Basal shift (mm)

This shifts all the frequencies in the carrier by a specified amount of space along the basilar membrane in the cochlea.

Note: this is not a perfect simulation of shallow insertion depth for a cochlear implant, since the electrode placements are not specifically programmed into this vocoder, and the mapping fo stimulation site to perceived frequency operates according to the spiral ganglion rather than the basilar membrane. Still, this is the method that people generally use to simulate the well-known effect of upward spectral shifting in a cochlear implant.

#### Envelope cutoff filter (Hz)

The highest rate of envelope fluctuations that are preserved in the carrier channels. Setting this at a low value like 50 will remove any cues for periodicity (pitch). Setting this at a value like 300 Hz can ensure that is above the expected F0 of your speech signal, so long as their F0 is below 300 Hz.

Note 1: Envelope fluctuations result in spectral sidebands. Suppose you have a sinewave carrier channel that is centered on 1000 Hz, and you impose an envelope from a talker whose F0 is 100 Hz. The spectrum of that carrier channel will have components at 900, 1000, and 1100 Hz. Keep this in mind as you think about the relative spacing between your channels.

Note 2: For noise carriers, the envelope will contain random fluctuations that are unrelated to your input speech signal. This means that the output will not reflect the original speech envelope very faithfully, and in fact you might not be able to perceive the periodicity at all.

Note 3: For rectangular-shaped carriers, the rate of envelope modulations (in Hz) cannot exceed the linear spectral bandwidth of the channel (in Hz). So if you have a channel that spans from 500 to 615 Hz, it cannot modulate at 200 Hz. You are more likely to see those periodicity-related modulations 1) in the higher channels, which tend to have wider analysis bands, or 2) in carriers of vocoders with a small number of channels, which also tend to have wide frequency analysis bands.

Note 4: In general, it's difficult to hear envelope modulations faster than 30 Hz or so. So this parameter becomes less relevant at higher values.

#### Envelope compression or quantization

Check this box to enable a follow-up window where you can set special envelope parameters

![extra envelope parameters menu](https://raw.githubusercontent.com/ListenLab/Vocoder/main/images/envelope_extra_menu.png){width="502px"}

This window allows you to impose some extra parameters over the envelope, as described below.

##### Envelope Compression

Compression refers to changing the modulation depth of the amplitude envelope. This parameter can vary between 0 and 1. Keeping this setting at 1 means that the envelope modulation depth is unchanged. Setting this parameter to 0.5 means that the depth (in dB) of each modulation is cu tin half. So if there is a 40 dB modulation, it is now 20 dB. Here is an illustration:

![Comparison of envelope compression settings](https://raw.githubusercontent.com/ListenLab/Vocoder/main/images/envelope_compression_comparison.png){width="476px"}

On the full sound waveform, you can see some more examples here:

![Comparison of envelope compression in the full soundwave](https://raw.githubusercontent.com/ListenLab/Vocoder/main/images/envelope_compression_waveform.png){width="408"}

##### Envelope Quantization

Quantization allows you to determine the number of unique values that the sound can take, in the intensity domain. This can be used for example to simulate a small number of discriminable intensities, which is known to be a feature of electric hearing. However, when you implement this in the vocoder, any number of steps beyond 3 has virtually no effect on perception.

![Envelope quantization](https://raw.githubusercontent.com/ListenLab/Vocoder/main/images/px_envelope_quantization.png){width="496px"}

#### Output name suffix

If "output name suffix" is left blank, then the script will automatically generate an output name that reflects some of the core features of the vocoder that you used. The output name will begin with the original sound name, with an additional suffix that reflects some of the features that were used. In some cases a feature is omitted because it is at a "standard" level (for example, envelope compression is only indicated in the output name if it is altered from a value of 1).

If you type anything into this blank field, the thing that you type will override the automatic name generation.

Example: if you have a sound object called "Sentence", and you type *voc_8* into this field, your output will be *sentence_voc_8.* If you type *voc_8* into this field (without the leading underscore), the output will be *sentencevoc_8.* Special characters are NOT recommended here, as we can't guarantee how they will be handled by praat.

[Example of automatic name generation:]{.underline} if you have a sound object called "Sentence", and you use the following settings:

-   12 channel sinewave vocoder with 150 Hz envelope filter

    -   output name is *sentence_voc_12_of_12ch\_\_sine_env_150*

-   12-channel sinewave vocoder with 2mm basal shift

    -   output name is *sentence_voc_12_of_12ch\_\_sine_env_150_sh_200_mm*

    -   note: 200 instead of 2, to gracefully handle cases where you want non-integer values (e.g. 2.5mm shift ends inf 250_mm)

-   noise vocoder with 8 peak-picked channels out of 22 analysis channels with 12 dB/octave rolloff, 150 Hz envelope filter, 2mm shift

    -   output name is *sentence_voc_8_of_22ch_12_dBoct_env_150_sh_200_mm*

-   12-channel harmonic complex vocoder with fundamental frequency of 200 Hz. The carrier channels roll off at a rate of 6 dB/mm of cochlear space, with envelope filtered at 200 Hz

    -   output name is *sentence_voc_12_of_12ch_06_dBmm_HarmComp_200_env_150*

-   Same as above, but the envelope is compressed to multiply all modulation depth by 0.5

```         
-   sentence_voc_12_of_12ch_06_dBmm_HarmComp_200_env_150_comp_50
```

Process entire folder

If you check this box, you will see a follow-up window allowing you to identify an entire folder of sounds. The script will process all of the .wav files in the folder with the same settings that you determine in the main startup window. You will enter the name of a new sub-folder where all of the outputs will be saved.

![window to process entire folder](https://raw.githubusercontent.com/ListenLab/Vocoder/main/images/process_entire_folder_window.png){width="406px"}

If you're not sure exactly how to type in your original folder path, you can choose the option to select it via pop-up window.

There's an extra checkbox here to remove the naming suffix for the outputs. This is useful for example in cases where you have an experiment that uses a single stimulus list but where the type of speech output (vocoded, non-vocoded) can be altered simply by changing the folder whose those stimuli are drawn.

### Preserve Components for Inspection

By checking this box, you are able to keep some components in the praat object list that are usually cleared away during the vocoding process.

![preserve components menu](https://raw.githubusercontent.com/ListenLab/Vocoder/main/images/preserve_components_menu.png){width="350px"}

Clicking any of these checkboxes means that the selected component will be saved for ALL channels of the vocoder. The exception is the last box "intercept_to_save..." which is a single object to be described later.

If you elect to preserve the [analysis]{.underline} channels for a 12-channel vocoder, then you will have access to the 12 filtered portions of the *original* sound that was analyzed.

If you elect to preserve the [carrier]{.underline} channels for a 12-channel vocoder, then you will have access to the 12 output channels (e.g. 12 sinewaves in a sinewave vocoder, or 12 bands of noise in a noise vocoder). This can be useful for inspecting the spectral shape of a channel.

If you elect to preserve the original envelopes for a 12-channel vocoder, then you will have access to the envelopes for the 12 bands of the *original* signal before the envelope was filtered with your specific vocoder settings.

If you elect to preserve the compressed envelopes, then you will have the original envelopes where the modulation depth is altered by the settings you chose in the compression/quantization form. This checkbox will not give you anything useful unless you actually engaged that extra form in the startup menu.

If you elect to preserve the LPF envelopes, then you will have the low-pass filtered envelopes that were used to modulate your vocoder carriers. These filtered envelopes reflect the processing done on the original sound and DO NOT reflect the true output envelope of your carrier because the true output envelope also includes whatever modulations were already part of the carrier. In the case of a sinewave carrier, the output fo the LPF envelope is an accurate reflection of the carrier of your vocoder channel. But if you have a noise carrier, then the actual carrier also contains a bunch of random noise fluctuations that were never part of the original sound envelope.

To determine the real prevailing envelope of your non-sinewave carrier channels, you will want to preserve the carrier channels and conduct a separate analysis of the envelopes on your own.

If you preserve any of these components described above, you will be given an option to batch-save all of those objects as sound files on a folder on your computer.

By electing to "intercept to save channel matrix", you are able to view the timebin-by-timebin analysis of channel intensities that are used for peak-picking. For example, you can see for any time point the intensities of all 22 analysis channels and therefore determine which 8 channels were picked for stimulation. This option will not produce anything useful unless you actually engaged a peak-picking vocoder.

NOTE: If you want to preserve any components, you can only do it for ONE sound at a time. The reason for this is that the object names are generic (i.e. carrier_channel_10) and therefore will not distinguish between different original sounds. Also, depending on your number of analyzed sounds and your number of channels, you could end up with a huge number of objects remaining in the list, and that would be difficult to work through.
