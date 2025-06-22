# Hugo网站搭建4：内容


## 图片网页制作 

图片上传到资源库后，需要制作展示图片的网页。  
只需要图片网址即可展示图片。在图片很多的情况下，可通过脚本来制作展示图片的网页。

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*- #

import os
import sys
import re
import datetime
import random
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

OVERWRITE = False


def get_key_word():
    str_words = 'keyword'
    return str_words

def get_description():
    return 'description'

def get_title():
    return 'Beauty Illustrated Book'

def check_url(url):
    p = re.compile(r"/v\d+/")
    return p.sub(r"/", url)

def make_md_file(out_folder):
    begin = datetime.datetime(2021, 8, 1)
    start_id = 1

    while True:
        str_date = begin + datetime.timedelta(days=start_id-1)

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
            # print(img_folder)
            md_file = out_folder + "{id}.md".format(id=start_id)

            if not OVERWRITE:
                if os.path.exists(md_file):
                    print(md_file + " is exists, passed")
                    start_id = start_id + 1
                    continue

            img_list = []
            thumb_img = ""
            not_found = 0
            for pid in range(1, 100):
                sid = img_folder + "/{:0>4d}".format(pid)
                try:
                    res = explicit(
                        sid,
                        type='upload'
                    )
                except Exception as ex:
                    print(ex)
                    not_found = not_found + 1
                else:
                    url = check_url(res['url'])
                    print(url)
                    img_list.append(url)
                    if pid == 1:
                        thumb_img = cloudinary.CloudinaryImage(res['public_id']) \
                            .build_url(width=300, height=150, gravity="faces", radius='10',  # effect='cartoonify',
                                       crop="fill")
                        thumb_cartoon_img = cloudinary.CloudinaryImage(res['public_id']) \
                            .build_url(width=300, height=150, gravity="faces", radius='10', effect='cartoonify',
                                       crop="fill")

                pid = pid + 1
                if not_found > 3:
                    break

            if len(img_list) > 1:
                with open(md_file, 'w', encoding='utf-8') as f:
                    f.write('---\n')
                    s_date = str_date.strftime('%Y-%m-%d %H:%M:%S')
                    s_pub_date = str_date.strftime('%Y-%m-%d')
                    f.write('date: {0}\n'.format(s_date))
                    f.write('publishdate: {0}\n'.format(s_pub_date))
                    f.write('title: \"{0}\"\n'.format(get_title()))
                    f.write('keywords: \"{0}\"\n'.format(get_key_word()))
                    f.write('description: \"{0}\"\n'.format(get_description()))
                    f.write('weight: {0}\n'.format(start_id))
                    f.write('thumb: \"{0}\"\n'.format(thumb_img))
                    f.write('thumbCartoon: \"{0}\"\n'.format(thumb_cartoon_img))
                    f.write('---\n\n')

                    for img_url in img_list:
                        f.write('![]({url})\n'.format(url=img_url))
                        if img_url != img_list[-1]:
                            f.write('\n---\n\n')

            start_id = start_id + 1


if __name__ == '__main__':
    out_folder = r'./content/pages/'
    make_md_file(out_folder)

```

通过本脚本，可以制作Markdown文件放入content文件夹下。再通过hugo编译生成展示图片的网页。


---

> Author:   
> URL: https://yfeier.github.io/tech/build-hugo-site4/  

