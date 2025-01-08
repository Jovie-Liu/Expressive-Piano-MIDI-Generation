# Expressive Piano MIDI Generation

## Multi-Arguments I/O

Five arguments are extracted from the MIDI files as sufficient statistics for expressive piano performances. The five fields are:

- Note value ($n$): a MIDI note number in range $[21,108]$.
- Time shift ($t$): the onset difference between two subsequent notes in miliseconds, $t = 0$ gives perfectly simultaneous notes.
- Duration ($d$): duration of the note in miliseconds.
- Velocity ($v$): a number in range $[0,127]$, the MIDI default velocity representation.
- Sustain pedal ($p$): the status of the sustain pedal, with binary value on/off (1/0).

## Listening-based Data Processing

- *Abandonment of fixed grid.* Use time-shift events and duration measured in miliseconds to generate expressive timing.

- *A homogeneous treatment of monophony and polyphony.* We claim that there is no real simultaneity of notes. For any two notes that are played by a human performer, there is always a time discrepancy between them, no matter how unnoticeable it is. It means that, since there are no simultaneous events, **we can always place the notes in sequential order, by their time onsets.**

- *Not only the notes matter.* **The control events in MIDI may play a crucial role in musical expressivity** (eg. sustain pedal in piano generation). Please listen to "Original.MID" and "no_sustain.mid" for comparison.

- *Mel quantization of auditory features.* Instead of equal division, like the Mel spectrogram, we divide the ranges into uneven chunks to better reflect the perceptual truth. We refer to **Weber’s law** for just noticeable differences as our theoretical foundation for the divisions, where the noticeable difference is proportional to the current value.

<img src="Pictures/division_new.png" style="width:800px">
<caption><center> Figure 1. The categorical distributions for given input features. The divisions obey Weber's law where the perceptual changes are proportional to the values. </center></caption>

## Multi-arguments Sequential Model

The temporal feature of the performance is captured by the sequential model, and the multi-arguments are inherently interdependent. To model their interdependencies, we decompose the temporal predictor into 5 separate LSTMs with inputs conditioned on previous outputs.

<img src="Pictures/LSTM_5.jpg" style="width:800px">
<caption><center> Figure 2. A way to Capture Interdependency among Arguments in a Multi-argument Sequential Model.</center></caption>

## Generation Selection with Entropy Sequence

<img src="Pictures/statistics.png" style="width:900px">
<caption><center> Figure 3. Statistics of Data and Generation Entropy Sequences of 5 LSTM Models.</center></caption>


We propose several ways to select high quality generations from all generated samples. The main idea is to select generations with small entropy values regularized by the data statistics (making selected generation's entropy sequence close to data entropy sequences).

## Generated Samples

https://github.com/user-attachments/assets/4567784d-dbed-4083-a266-879ed4c73ce6
https://github.com/user-attachments/assets/c7a6df22-aad3-4052-bd87-f8f899910df0
https://github.com/user-attachments/assets/766fc974-a133-4043-bd2e-5e3eec520e81
