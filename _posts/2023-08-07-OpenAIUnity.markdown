---
title: "OpenAI in Unity"
categories:
  - AI
  - VR
tags:
  - VR
  - OpenAI
  - DallE
---
With the increase in popularity in AI and the amazing things it can do, I decided to explore potential ways that it can be used in games and or XR expereiences. 

I looked into ChatGPT one of the more popular language models and tried to find ways that I can use their API to potentially create something cool.

Because of the way that the OpenAI C# API runs with NuGet, integrating it into Unity wasn't very straightforward. Luckly, there is already a Unity package and Youtube playlist to help me get right into it: [OpenAI in Unity](https://www.youtube.com/playlist?list=PLrE-FZIEEls1-c7QifZYzeq50Id08FcJo)

## ChatGPT, Whisper and DallE

With access to the OpenAI API, I gained access to all three of these models. I wanted to find a clever way to potentially use all 3 of these in a single experience. Whisper, being a transctiption and translation tool, was easy to use with either DallE or ChatGPT.

The experience I ended up deciding on was one that allows the user to speak a small description of a texture and has DallE generate an image which is then skinned onto an object.

## Unity's Microphone Input and Whisper

With Unity's basic microphone input, one of the mandatory fields is a length, in seconds, that the recording will last for. I thought that by using Microphone.End(), the audio clip would simply be cut short and the length of the clip resized to when the clip was stopped. I needed to make sure that the base length of the clip was long enough for the user to be able to say anything and everything, so I set it to 60 seconds.

{% highlight ruby %}
AudioClip clip;
clip = Microphone.Start(dropdown.options[index].text, false, 60, 44100);
{% endhighlight %}

Testing this, I learned that using the Microphone.End() function doesn't cut the sound clip and every sound clip I was creating was 60 seconds long weather or not I actually talked for that long. I was pushing minute long wav files through to whisper which both cost me money and made the translation come back slower. It also gave me transcriptions that were completely wrong and made zero sense.

Unfortunately, Unity had no innate way of cutting audio clips and thus, I had to find a way to cut the clips myself. After researching online, I found that audio clips contain a "Data" field that is an array of all the samples that were taken during the recording. The last field of the Microphone.Start() function is how many samples the recording takes each second. Using this, I can determine how many total samples were taken by multiplying the samples per second with the amount of time that the recording lasted. I can then gather the samples exclusivly from the 0th sample to the total sample and create a new audio recording with that array. Then, push that recording through to Whisper.

{% highlight ruby %}
Microphone.End(null);

float lengthL = clip.length;
float samplesL = clip.samples;
float samplesPerSec = (float)samplesL / lengthL;
float[] samples = new float[(int)(samplesPerSec * time)];
clip.GetData(samples, 0);

clip = AudioClip.Create("RecordedSound", (int)(time * samplesPerSec), 1, 44100, false);

clip.SetData(samples, 0);

byte[] data = SaveWav.Save(fileName, clip);
{% endhighlight %}

This was a fun find and helped me understand audio in Unity. Most importantly, it saved me whole cents $$.


## Demo Videos

I have three demo videos in this playlist in the order that I implemented features:

[Demo Videos](https://www.youtube.com/playlist?list=PLzFymnp51SV-kBJ1uIoxA8MP92sXtNwRu)

The first video is converting the image into a material and using that to skin the space ship.

The second video is the VR implementation and making the UI into world space so it can be used in VR space.

The 3rd video is the working Whisper implementation that lets the user speak a description.


## Other Issues

If you do end up working with the OpenAI API and Unity, be extra careful to not put any API calls in the update function, I made a mistake and my program called the API 50 times before it got cut off for too many requests. Thankfully there was a cut off so I was only charged $1 for my mistake.

One thing I could've done to completely prevent this is using Unity's input system. The way I coded to check if the button is pressed:

{% highlight ruby %}
void Update()
    {
        //If recording isnt started and button isn't pressed, open panel and start recording
        if (OVRInput.Get(OVRInput.Button.One) && !whisper.isRecording)
        {
            whisper.StartRecording();
        }

        //If recording hasnt stopped and button is released, stop recording
        if (!OVRInput.Get(OVRInput.Button.One) && whisper.isRecording)
        {
            whisper.EndRecording();
        }
    }
{% endhighlight %}

Could've been changed to the event call "action.triggered" which would remove the possibility of accidently calling the API multiple times.