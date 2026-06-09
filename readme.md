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