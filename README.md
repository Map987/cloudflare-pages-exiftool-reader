# cloudflare-pages-exiftool-reader
使用cloudflare pages，存放查看图片exif的html页面
https://xel.pages.dev/
```
通用页面：
https://dash.cloudflare.com/?to=/:account/pages/new/upload
https://dash.cloudflare.com/用户id/pages/new/upload

https://dash.cloudflare.com/?to=/:account/:zone/dns
https://community.cloudflare.com/t/settings-cloudflare/587151/9

```

```
 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EXIF 查看</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            height: 100vh;
            background-color: #f7f7f7;
        }
        .container {
            text-align: center;
            max-width: 600px;
            margin-top: 20px;
        }
        #image-container {
            margin-bottom: 20px;
        }
        #image-output {
            max-width: 100%;
            height: auto;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        input[type="file"], input[type="text"] {
            padding: 10px;
            margin: 10px 0;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        button {
            padding: 10px 20px;
            background-color: #BE3455 - 橙;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            transition: transform 0.3s ease;
        }
        button:hover {
            background-color: #BE3455 - 橙;
            transform: scale(1.05);
        }
        .exif-info {
            margin-top: 20px;
            background-color: #fff;
            padding: 20px;
            border-radius: 4px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
        }
        .exif-info p {
            margin: 5px 0;
            font-size: 0.9rem;
            color: #333;
            width: 99%; /* Adjust width for two columns */
        }
        .exif-info::before {
            content: '';
            display: block;
            width: 100%;
            height: 1px;
            background-color: #ccc;
            margin-bottom: 10px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>EXIF Viewer</h1>
        <input type="file" id="image-file" placeholder="Select image file">
        <input type="text" id="image-url" placeholder="Or enter image URL">
        <button id="get-exif">获取 EXIF</button>
        <div id="image-container">
            <img id="image-output" src="" alt="Image will be displayed here">
        </div>
        <div class="exif-info" id="exif-output"></div>
    </div>


    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/exifreader@4.25.0/dist/exif-reader.min.js"></script>
    <script>
 
$(document).ready(function () {
    $('#get-exif').click(function () {
        var fileInput = $('#image-file')[0];
        var file = fileInput.files[0];
        var url = $('#image-url').val();

        if (file || url) {
            ExifReader.load(file || url, {async: true}).then(function (tags) {
                var exifInfo = $('#exif-output');
                exifInfo.empty(); // Clear previous EXIF data
                var count = 0;
                for (var tag in tags) {
                    var tagValue = tags[tag].value;
                    tagValue = String(tagValue);
                    exifInfo.append('<p>' + tag + ': ' + tagValue + '</p>');
                    count++;
                    if (count % 2 === 0) {
                        exifInfo.append('<div style="clear: both;"></div>'); // Clear float for every two items
                    }
                }
            }).catch(function (error) {
                console.error('Error reading EXIF data:', error);
            });
        } else {
            alert(选择图片，或者输入图片链接.');
        }
    });
});
 
    </script>
</body>
</html>

```

测试图片：
https://raw.githubusercontent.com/ianare/exif-samples/refs/heads/master/jpg/Canon_40D.jpg

需要支持cors跨域的图片链接才能读取到
参考：
1 . https://github.com/mattiasw/ExifReader
2 . 
https://syachikuai.com/2022/11/26/exif-js/

