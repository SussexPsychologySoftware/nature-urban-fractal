generating csvs from folders:

inside /stimuli
`
ls /path/to/directory/*.JPG | xargs -I{} basename {} .JPG | awk 'BEGIN{print "image_name"} {print}' > output.csv
`

```
ls /Users/mel29/Code/nature-urban-fractal/stimuli/images/nature/*.JPG | xargs -I{} basename {} .JPG | awk 'BEGIN{print "image_name"} {print}' > nature.csv
ls /Users/mel29/Code/nature-urban-fractal/stimuli/images/nature_fractal/*.JPG | xargs -I{} basename {} .JPG | awk 'BEGIN{print "image_name"} {print}' > nature_fractal.csv
ls /Users/mel29/Code/nature-urban-fractal/stimuli/images/urban/*.JPG | xargs -I{} basename {} .JPG | awk 'BEGIN{print "image_name"} {print}' > urban.csv
ls /Users/mel29/Code/nature-urban-fractal/stimuli/images/urban_fractal/*.JPG | xargs -I{} basename {} .JPG | awk 'BEGIN{print "image_name"} {print}' > urban_fractal.csv
```

Generating placeholder video:
```
ffmpeg -f lavfi -i color=c=black:s=320x240:r=1 -t 3 -c:v libx264 -crf 28 stimuli/videos/parent.mp4
```

Converting fixation cross to .mov (original, causes issues):
```
ffmpeg -i stimuli/videos/Spiral.avi -c:v libx264 -g 15 -pix_fmt yuv420p -c:a pcm_s16le stimuli/videos/fixation.mov
```

Fixation stimulus for the experiment routines — split into separate video + audio files.
```
# video only: copy the validated frames byte-for-byte, drop audio, move moov to front (faststart)
ffmpeg -y -i stimuli/videos/fixation.mov -map 0:v:0 -c:v copy -an -movflags +faststart stimuli/videos/fixation_video.mp4
# audio only: extract the original track unchanged (22050 Hz mono)
ffmpeg -y -i stimuli/videos/fixation.mov -map 0:a:0 -c:a pcm_s16le -ar 22050 -ac 1 stimuli/videos/fixation_audio.wav
```

`fixation_faststart.mov` fixes AV issue with original file
```
# FIX AV issue: keeps PCM audio, container stays .mov
ffmpeg -y -i stimuli/videos/fixation.mov -c copy -movflags +faststart stimuli/videos/fixation_faststart.mov
# OR in .mp4
ffmpeg -y -i stimuli/videos/fixation.mov -c:v copy -c:a aac -movflags +faststart stimuli/videos/fixation_faststart.mp4
```