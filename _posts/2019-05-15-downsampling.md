---
title: "Downsampling and Lowpass Filter in MATLAB"
header:
  teaser: /assets/images/downsampling.png
categories:
  - Digital Signal Processing
tags:
  - Downsampling and Filters
toc: true
---

# Introduction
In this small amount of MATLAB code we look into downsampling a music track as well as applying a simple lowpass filter defined as h0 to the track to make it difficult to guess what the original song was. Downsampling is a common technique used to reduce the file size of a music track and some samples are generally not needed and by downsampling and removing those samples space is saved. Filtering is also a good technique to reduce the size of a music track as less space is used when not all frequencies are recorded. We see this in the first project where to hear dolphins we need a large sampling frequency which takes up a lot of space, but as most people are not listening to dolphins it makes sense to not have those high frequencies recorded.

## Main Project

1. **Introduction** 
We will use two MATLAB functions: downsampling.m and extractSound.m in order to demonstrate how downsampling works. extractSound.m creates a signal we can feed into downsampling.m as well as let us choose how many seconds we want our signal to be. downsampling.m then takes this signal and applies a low-pass filter and downsamples the song by using the conv function in MATLAB. This is then saved as a .wav file and can be listened to by anyone including your friends if you want them to guess what song you've downsampled.  

2. **MATLAB functions** 

extractSound.m

```
function [ soundExtract,p ] = extractSound( filename, time)
%extractSound Extracts time (in seconds) from the middle of the song
%   Write a MATLAB function that extract T seconds of music from a
%   given track. You will use the MATLAB function audioread to
%   read a track and the function play to listen to the track.
narginchk(1, 2);
if(nargin == 1)
    time = 24;
end
info = audioinfo(filename);
song=audioread(filename);
if time >= info.Duration
    soundExtract=song;
    if(nargout == 2)
        p=audioplayer(soundExtract,info.SampleRate);
    end
    return;
elseif time<= 1/info.SampleRate
    error('Too small of a time to sample');
end
samples=time*info.SampleRate;

soundExtract=song(floor(info.TotalSamples/2)-floor(samples/2):1: ...
    floor(info.TotalSamples/2)+floor(samples/2));
if(nargout == 2)
    p=audioplayer(soundExtract,info.SampleRate);
end
end
```

downsampling.m

```
function[downsampledSong] = downsampling(filename, time)
%This function turns your favorite music tracks into a downsampled and filtered song
%filename must have structure 'track.wav or track.mp3'
%time determines how long you want your downsampled track to be
%also must have extractSound function for this to work

h0 = [-1 2 -1];
[song, fs] = audioread(filename);
songL = song(:,1);
audiowrite('asstrack1.wav', songL, fs);
extractedSong = extractSound('asstrack1.wav', time);
downsampledTrack1 = extractedSong(1:10:length(extractedSong));
downsampledTrack2 = conv(h0, downsampledTrack1);
downsampledSong = audioplayer(downsampledTrack2, fs/10);
audiowrite('downsampledtrack.wav', downsampledTrack2, fs/10);
play(downsampledSong);
```
	







