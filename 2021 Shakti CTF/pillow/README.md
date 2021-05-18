The name of the problem is a hint to the Python Imaging Library (PIL) also known simply as Pillow.

We are provided 3000 pixels in a zip file named `60x50`. The pixel files are numbered from 1 to 3000 and are all JPGs. We can write a python script to stitch the pixels together and display the resulting image:

```py 
from PIL import Image

new_im = Image.new('RGB', (600,500))

for i in range(1,61):
    for j in range(1,51):
        file_num = j + (i-1) * 50
        file_name = str(file_num) + '.jpg'
        im = Image.open(file_name)
        new_im.paste(im, ((i-1)*10,(j-1)*10))
new_im.show()
```

Running the script yields the following JPG:

<img src="pillow.png" width=400>
