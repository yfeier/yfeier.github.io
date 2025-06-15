# Hugo网站搭建3：图片


## 图片存储


[本网站](https://avgle.top/pages/)主要展示图片，因此需要将图片存储到网络上。  
对于个人网站，最好挑选一个免费的图片空间。  
经过挑选，这里使用[Cloudinary](https://cloudinary.com/invites/lpov9zyyucivvxsnalc5/fxh9oezpff5hjdmadvzq?t=default)网站。  
该网站提供25Credit免费使用量。如果访问量大，也可升级收费版本。  
```
1 Credit =
 1,000 Transformations OR
 1 GB Storage OR
 1 GB Image Bandwidth OR
 2 GB Video Bandwidth
```
如需申请，请点击[Cloudinary](https://cloudinary.com/invites/lpov9zyyucivvxsnalc5/fxh9oezpff5hjdmadvzq?t=default)


## 图片上传


通过Cloudinary网站可以直接上传图片，但如果需要上传的图片量很大的时候，手动上传就很麻烦。
由于Cloudinary提供了API接口，因此可通过脚本工具进行上传。

这里使用python编写上传脚本，需要安装cloudinary插件。

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*- #

import os
import re
import sys
import datetime
import cloudinary
from cloudinary.api import delete_resources_by_tag, resources_by_tag
from cloudinary.uploader import upload, explicit
from cloudinary.utils import cloudinary_url

# 数值Cloudinary网站提供
cloudinary.config(
    cloud_name="******",
    api_key="******",
    api_secret="******"
)

def get_current_dir(path):
    list_dir = os.listdir(path)
    dirs = []
    for _dir in list_dir:
        if _dir.isdigit():
            dirs.append(_dir)
    dirs.sort(key=int)

    return dirs

def get_all_files(file_dir):
    file_list = []
    ext_list = ['.jpeg', '.jpg', '.png', '.JPG', '.bmp']
    for root, dirs, files in os.walk(file_dir):
        for file in files:
            if os.path.splitext(file)[1] in ext_list:
                file_list.append(os.path.join(root, file))

    return file_list

def dump_response(response):
    print("Upload response:")
    for key in sorted(response.keys()):
        print("  %s: %s" % (key, response[key]))

def upload_files(img, pid, img_folder):
    print("--- Upload a local file")
    response = upload(
        img,
        public_id=pid,
        folder=img_folder
    )
    dump_response(response)

    _filepath, _filename = os.path.split(img)
    if not re.match(r'\d+\.', _filename):
        os.rename(img, '{0}/{1}.JPG'.format(_filepath, pid))

def get_start_id(begin):
    start_id = 1
    while True:
        str_date = begin + datetime.timedelta(days=start_id - 1)

        img_folder = "my_blog/pages/" + str_date.strftime('%Y%m%d')
        test_img = img_folder + "/{:0>4d}".format(1)
        try:
            res = explicit(
                test_img,
                type='upload'
            )
        except Exception as ex:
            break
        else:
            start_id = start_id + 1

    return start_id


NEED_CHECK_UPDATE = True
LIMIT_SIZE = 30

if __name__ == '__main__':
    begin = datetime.datetime(2021, 8, 1)
    start_id = get_start_id(begin)
    up_num = 0

    dir_list = get_current_dir(r'./')
    for cur_dir in dir_list:
        # print(cur_dir)
        if NEED_CHECK_UPDATE:
            test_file = os.path.join(cur_dir, '0001.JPG')
            if os.path.exists(test_file):
                continue

        files = get_all_files(cur_dir)
        if len(files) == 0:
            continue
        print(cur_dir)

        str_date = begin + datetime.timedelta(days=start_id - 1)
        img_folder = "my_blog/pages/" + str_date.strftime('%Y%m%d')

        files = get_all_files(cur_dir)
        pid = 1
        for f in files:
            file_size = os.path.getsize(f)
            file_size = file_size / float(1024 * 1024)
            if file_size > 10:   # 10M
                continue

            try:
                upload_files(f, "{:0>4d}".format(pid), img_folder)
            except Exception as ex:
                print(ex)
            else:
                pid = pid + 1

        start_id = start_id + 1

        up_num = up_num + 1
        if 0 < LIMIT_SIZE < up_num:
            break

    pass

```

上传后，在Cloudinary网站的Media Library中可以查看上传的图片。

---

> Author:   
> URL: http://example.org/tech/build-hugo-site3/  

