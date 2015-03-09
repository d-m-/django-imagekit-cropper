# django-imagekit-cropper
A library to enhance django-imagekit which allows you to specify your image variant crops:

![CKEditor Dialog](https://raw.github.com/ninapavlich/ckeditor-imagekit-cropper/master/docs/screenshots/crop_screenshot.png)

**InstanceSpecField** This is a class that extends ImageSpecField which passes the model instance to 
the processors so that you can access other model fields to process the image.

**ImageCropField** This is a custom model field for setting the image crop. This uses a custom
widget to allow admins to visually crop the image.

**PositionCrop** This a custom processor which recieves the model instance and crops the image
using the image source and the value of the image crop field.


## Example Usage

```python
    #models.py

    from ckeditor_imagekit_cropper.fields import ImageCropField, InstanceSpecField  
    from ckeditor_imagekit_croper.processors import PositionCrop 


    image = models.ImageField(blank=True, null=True)
    square_150_crop_properties = {
        'source':'image',
        'crop_field':'square_150_crop', 
        'resize_method':'fill',
        'width':150,
        'height':150, 
        'upscale':False
    }
    square_150_crop = ImageCropField(null=True, blank=True, 
        properties=square_150_crop_properties)
    square_150 = InstanceSpecField( source=square_150_crop_properties['source'], 
        id='media:profile:cropspec', format='PNG', options={'quality': 85}, 
        hash_key_values=['square_150_crop'],
        instance_processors=[PositionCrop(square_150_crop_properties)])

```  

```python
    #admin.py

    from .forms import ImageAdminForm
    
    class ImageAdmin(models.ModelAdmin):
        form = ImageAdminForm

```        

```python
    #forms.py

    from django import forms
    from ckeditor_imagekit_cropper.widgets import ImageCropWidget    
    from .models import Image

    
    class ImageAdminForm(forms.ModelForm):
        square_150_crop = forms.CharField(widget=ImageCropWidget(properties=Image.square_150_crop_properties))

        class Meta:
            model = Image

``` 


