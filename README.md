# php image to webp convert
php convert images to webp format

 

## php function convert image to webp 

```php
/** 
 * Generate Webp image format
 *  https://www.jclabs.co.uk/generate-webp-images-in-php-using-and-gd-or-imagick/
 * Uses either Imagick or imagewebp to generate webp image
 * 
 * @param string $file Path to image being converted.
 * @param int $compression_quality Quality ranges from 0 (worst quality, smaller file) to 100 (best quality, biggest file).
 * 
 * @return false|string Returns path to generated webp image, otherwise returns false.
 */
function webpConvert($file, $compression_quality = 80)
{
    // check if file exists
    if (!file_exists($file)) {
        return false;
    }
    $file_type = strtolower(pathinfo($file, PATHINFO_EXTENSION));
    $output_file =  $file . '.webp';
    if (file_exists($output_file)) {
        return $output_file;
    }
    if (function_exists('imagewebp')) {
        switch ($file_type) {
            case 'jpeg':
            case 'jpg':
                $image = imagecreatefromjpeg($file);
                break;
            //(PHP 8 >= 8.1.0)
            // case 'avif':
            //     $image = imagecreatefromavif($file);
            //     break;
            case 'bmp':
                $image = imagecreatefrombmp($file);
                break;
            case 'xbm':
                $image = imagecreatefromxbm($file);
                break;
            case 'png':
                $image = imagecreatefrompng($file);
                imagepalettetotruecolor($image);
                imagealphablending($image, true);
                imagesavealpha($image, true);
                break;
            case 'gif':
                $image = imagecreatefromgif($file);
                break;
            default:
                return false;
        }
        // Save the image
        $result = imagewebp($image, $output_file, $compression_quality);
        if (false === $result) {
            return false;
        }
        // Free up memory
        imagedestroy($image);
        return $output_file;
    } elseif (class_exists('Imagick')) {
        $image = new Imagick();
        $image->readImage($file);
        if ($file_type === 'png') {
            $image->setImageFormat('webp');
            $image->setImageCompressionQuality($compression_quality);
            $image->setOption('webp:lossless', 'true');
        }
        $image->writeImage($output_file);
        return $output_file;
    }
    return false;
}
```



### use code php convert image to webp :

in your php 

```bash
  $file ="images.jpg";
    $newfile ="images.jpg.webp";
// nếu file ko tồn tại và file name nhỏ hơn 0
if(!file_exists($newfile) ||  filesize($newfile)==0){
     $newfile = webpConvert($file);
     header('Content-type:image/webp');
     header('Cache-Control:public, max-age=315360000');
     echo file_get_contents($newfile);
    die;
}else{
    echo "file invalid format. bla bla...";
    die;
}
```

