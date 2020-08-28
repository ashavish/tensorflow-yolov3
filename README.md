
## Yolo with rotated boxes

Based on YOLOV3 implementation by YunYang1994. And Loss function inspired by https://github.com/Molecular-Nanophotonics/YOLOTrack-1.1


1. Clone this file
```bashrc
$ git clone https://github.com/ashavish/tensorflow-yolov3.git
```
2.  You need to install some dependencies
```bashrc
$ cd tensorflow-yolov3
$ pip3 install -r ./docs/requirements.txt
```
3. Exporting loaded COCO weights as TF checkpoint(`yolov3_coco.ckpt`)【[BaiduCloud](https://pan.baidu.com/s/11mwiUy8KotjUVQXqkGGPFQ&shfl=sharepset)】
```bashrc
$ cd checkpoint
$ wget https://github.com/YunYang1994/tensorflow-yolov3/releases/download/v1.0/yolov3_coco.tar.gz
$ tar -xvf yolov3_coco.tar.gz
$ cd ..
$ python3 convert_weight.py
$ python3 freeze_graph.py
```
Then you will get some `.pb` files in the root path.

4. Data Files
<br>
Two files are required as follows:

- [`dataset.txt`](https://raw.githubusercontent.com/ashavish/tensorflow-yolov3/master/data/dataset/object_train.txt): 

```
xxx/xxx.jpg 18.19,6.32,424.13,421.83,1.57 20 323.86,2.65,640.0,421.94,1.5 20 
xxx/xxx.jpg 48,240,195,371,1.48 11 8,12,352,498,1.38 14
# image_path x_min, y_min, x_max, y_max, theta, class_id  x_min, y_min ,..., class_id 
# make sure that x_max < width and y_max < height
# theta is to be provided in radians. x_min, y_min, x_max, y_max - Should be the coordinates of the unoriented box.
# This box when it is rotated about its center for theta in radians, should be correct bounding box of the object
```



- [`class.names`](https://github.com/ashavish/tensorflow-yolov3/blob/master/data/classes/object.names):

```
person
bicycle
car
...
toothbrush
```

Then edit your ./core/config.py to make some necessary configurations

```
__C.YOLO.CLASSES                = "./data/classes/object.names"
__C.TRAIN.ANNOT_PATH            = "./data/dataset/object_train.txt"
__C.TEST.ANNOT_PATH             = "./data/dataset/object_test.txt"
```

5. Training

##### (1) train from scratch:

```bashrc
$ python3 train.py
$ tensorboard --logdir ./data
```
##### (2) train from COCO weights(recommended):

```bashrc
$ cd checkpoint
$ wget https://github.com/YunYang1994/tensorflow-yolov3/releases/download/v1.0/yolov3_coco.tar.gz
$ tar -xvf yolov3_coco.tar.gz
$ cd ..
$ python convert_weight.py --train_from_coco
$ python train.py
```
6. Predicting with an image

```bashrc
$ python3 predict_image_rotated.py
```



