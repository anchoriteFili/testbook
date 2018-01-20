---
layout: post
#标题
title:  UIImagePickerController_改变数据的剪裁窗口_转载
#时间配置
date:   2015-06-02 11:01:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

> This includes most of what you need, and takes care of all the camera orientation issues. I've added the following which will take in the editing info and use it to get the original cropping rect with this addition:

```objc
- (UIImage*)scaleImage:(UIImage*)anImage withEditingInfo:(NSDictionary*)editInfo{
    UIImage *newImage;
    UIImage *originalImage = [editInfo valueForKey:@"UIImagePickerControllerOriginalImage"];
    CGSize originalSize = CGSizeMake(originalImage.size.width, originalImage.size.height);
    CGRect originalFrame;
    originalFrame.origin = CGPointMake(0,0);
    originalFrame.size = originalSize;
    CGRect croppingRect = [[editInfo valueForKey:@"UIImagePickerControllerCropRect"] CGRectValue];
    CGSize croppingRectSize = CGSizeMake(croppingRect.size.width, croppingRect.size.height);
    CGSize croppedScaledImageSize = anImage.size;
    float scaledBarClipHeight = 80;
    CGSize scaledImageSize;
    float scale;
    if(!CGSizeEqualToSize(croppedScaledImageSize, originalSize)){
        scale = croppedScaledImageSize.width/croppingRectSize.width;
        float barClipHeight = scaledBarClipHeight/scale;
        croppingRect.origin.y -= barClipHeight;
        croppingRect.size.height += (2*barClipHeight);
        if(croppingRect.origin.y<=0){
            croppingRect.size.height += croppingRect.origin.y;
            croppingRect.origin.y=0;
        }
        if(croppingRect.size.height > (originalSize.height - croppingRect.origin.y)){
            croppingRect.size.height = (originalSize.height - croppingRect.origin.y);
        }
        scaledImageSize = croppingRect.size;
        scaledImageSize.width *= scale;
        scaledImageSize.height *= scale;
        newImage =  [self cropImage:originalImage to:croppingRect andScaleTo:scaledImageSize];
    }else{
        newImage = originalImage;
    }
    return newImage;
}
```

**I updated the call back method from the dev forums post to the following:**

```objc
- (void)imagePickerController:(UIImagePickerController *)picker didFinishPickingImage:(UIImage *)img editingInfo:(NSDictionary *)editInfo {
    [self dismissModalViewControllerAnimated:YES];
    self.myImageView.userInteractionEnabled=YES;
    CGRect imageFrame = myImageView.frame;
    CGPoint imageCenter = myImageView.center;
    UIImage *croppedImage;
    NSMutableDictionary *imageDescriptor = [editInfo mutableCopy];
    // CGFloat scaleSize = 400.0f;
    CGFloat scaleSize = 640.0f;
    switch ([picker sourceType]) {
            //done
        case UIImagePickerControllerSourceTypePhotoLibrary:
            croppedImage = [self scaleImage:img withEditingInfo:editInfo];
            [imageDescriptor setObject:croppedImage forKey:@"croppedImage"];
            break;
        case UIImagePickerControllerSourceTypeCamera: {
            UIImageOrientation originalOrientation = [[editInfo objectForKey:UIImagePickerControllerOriginalImage] imageOrientation];
            if (originalOrientation != UIImageOrientationUp) {
                NSAutoreleasePool *pool = [[NSAutoreleasePool alloc] init];
                CGRect origRect;
                [[editInfo objectForKey:UIImagePickerControllerCropRect] getValue:&origRect];
                UIImage *rotatedImage = straightenAndScaleImage([editInfo objectForKey:UIImagePickerControllerOriginalImage], scaleSize);
                CGFloat scale = scaleSize/1600.0f;
                origRect.origin.x *= scale;
                origRect.origin.y *= scale;
                origRect.size.width *= scale;
                origRect.size.height *= scale;
                croppedImage = [self cropImage:rotatedImage to:origRect andScaleTo:CGSizeMake(320, 480)];
                [imageDescriptor setObject:croppedImage forKey:@"croppedImage"];
                [pool drain];
            }
            else {
                croppedImage = [self scaleImage:img withEditingInfo:editInfo];
                [imageDescriptor setObject:croppedImage forKey:@"croppedImage"];
            }
        }
            break;
        case UIImagePickerControllerSourceTypeSavedPhotosAlbum: {
            UIImageOrientation originalOrientation = [[editInfo objectForKey:UIImagePickerControllerOriginalImage] imageOrientation];
            if (originalOrientation != UIImageOrientationUp) {
                NSAutoreleasePool *pool = [[NSAutoreleasePool alloc] init];
                CGRect origRect;
                [[editInfo objectForKey:UIImagePickerControllerCropRect] getValue:&origRect];
                UIImage *rotatedImage = straightenAndScaleImage([editInfo objectForKey:UIImagePickerControllerOriginalImage], scaleSize);
                CGFloat scale = scaleSize/640.0f;
                origRect.origin.x *= scale;
                origRect.origin.y *= scale;
                origRect.size.width *= scale;
                origRect.size.height *= scale;
                croppedImage = [self cropImage:rotatedImage to:origRect andScaleTo:CGSizeMake(320, 480)];
                [imageDescriptor setObject:croppedImage forKey:@"croppedImage"];
                [pool drain];
            }
            else {
                croppedImage = [self scaleImage:img withEditingInfo:editInfo];
                [imageDescriptor setObject:croppedImage forKey:@"croppedImage"];
            }
        }
            break;
        default:
            break;
    }
    imageFrame.size = croppedImage.size;
    myImageView.frame = imageFrame;
    myImageView.image = [imageDescriptor objectForKey:@"croppedImage"];
    myImageView.center = imageCenter;
}
```