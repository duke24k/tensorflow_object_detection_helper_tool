# **Object Detection with Tensorflow Helper Tool**

### Summary

This is Helper Tool for Google Tensorflow Object Detection API.

주요 기능:
> 1. Create tfrecord file
>2. Re-training Automation
>3. Active Learning Assistant ( Not yet )

### Compatibility

파이썬 2.x , 3.x 호환됩니다.

### 1. tfrecord Generator

이미지 파일과 이미지 파일에 대응되는 영역위치 좌표가 지정된 xml을 가지고, tfrecord 파일을 생성합니다.  
정해진 비율대로 train.record , validate.record 가 생성됩니다. ( Random 모듈의 Shuffle 함수로 전부 Shuffling 됩니다. )

##### 사전 준비 작업

[Google image download](https://github.com/hardikvasa/google-images-download) 를 사용하여 필요한 이미지를 수집합니다.

[labelimg](https://github.com/tzutalin/labelImg) 를 사용하여 원본 이미지와 오브젝트 영역을 저장한 xml 을 하나의 폴더에 위치 시킵니다.  
( 기본 폴더는 ./images 이지만, argument로 변경 가능합니다.)

##### Usage - Using Command Line Interface

    python tfgenerator.py [Arguments...]

##### Arguments

| Argument            | Short hand | Default                | Description                                                                                                                                                                                                                                                  |
|---------------------|------------|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| max num classes     | m          | 90                     | 최대 클래수 갯수   ( 90이 넘을 경우 수정하십시오 )                                                                                                                                                                                                           |
| input_folder        | i          | ./images/              | 이미지와 xml이 위치한 폴더입니다.                                                                                                                                                                                                                            |
| label_file          | l          | ./label_map.pbtxt      | 레이블 파일이 존재하는 위치입니다.                                                                                                                                                                                                                           |
| train_csv_output    | tc         | ./dataset/train.csv    | train cvs 파일이 생성되는 위치입니다.<br>   train.csv 파일은 내용 확인 용도 입니다.                                                                                                                                                                              |
| validate_csv_output | vc         | ./dataset/validate.csv | validate cvs 파일이 생성되는 위치입니다.<br>   validate.csv 파일은 내용 확인 용도 입니다.                                                                                                                                                                        |
| split_rate          | sr         | 8                      | 분할 비율을 설정합니다.<br>   기본값은 train 80% : validate 20% 입니다.<br>   6으로 설정할 경우, train 60% : validate 40% 입니다.<br> 2로 설정할 경우, train 20% : validate 80% 입니다.                                                                                  |
| log_level           | lv         | INFO                   | 로그 레벨을 지정합니다.<br>   로그 레벨의 종류는 다음과 같습니다.<br> [ DEBUG , INFO , WARNING , ERROR , CRITICAL ]<br>   현재는 INFO 레벨의 로그밖에 존재하지 않습니다.<br>  로그는 process.log 파일에서 확인할 수 있습니다.<br> 로그는 Re-training Automation 툴과 공유합니다. |

##### Example

### 2. Re-training Automation Tool

원하는 Pre training 된 모델을 다운로드하고, 자동으로 Transfer learning , Export 를 진행합니다.  

##### Usage - Using Command Line Interface

    python main.py [Arguments...]

##### Model zoo

*현재는 mask model 은 지원하지 않습니다.*  

Model name  | Speed (ms) | COCO mAP[^1] | Outputs |
| ------------ | :--------------: | :--------------: | :-------------: |
| [ssd_mobilenet_v1_coco](http://download.tensorflow.org/models/object_detection/ssd_mobilenet_v1_coco_2017_11_17.tar.gz) | 30 | 21 | Boxes |
| [ssd_mobilenet_v2_coco](http://download.tensorflow.org/models/object_detection/ssd_mobilenet_v2_coco_2018_03_29.tar.gz) | 31 | 22 | Boxes |
| [ssd_inception_v2_coco](http://download.tensorflow.org/models/object_detection/ssd_inception_v2_coco_2017_11_17.tar.gz) | 42 | 24 | Boxes |
| [faster_rcnn_inception_v2_coco](http://download.tensorflow.org/models/object_detection/faster_rcnn_inception_v2_coco_2018_01_28.tar.gz) | 58 | 28 | Boxes |
| [faster_rcnn_resnet50_coco](http://download.tensorflow.org/models/object_detection/faster_rcnn_resnet50_coco_2018_01_28.tar.gz) | 89 | 30 | Boxes |
| [faster_rcnn_resnet50_lowproposals_coco](http://download.tensorflow.org/models/object_detection/faster_rcnn_resnet50_lowproposals_coco_2018_01_28.tar.gz) | 64 |  | Boxes |
| [rfcn_resnet101_coco](http://download.tensorflow.org/models/object_detection/rfcn_resnet101_coco_2018_01_28.tar.gz)  | 92 | 30 | Boxes |
| [faster_rcnn_resnet101_coco](http://download.tensorflow.org/models/object_detection/faster_rcnn_resnet101_coco_2018_01_28.tar.gz) | 106 | 32 | Boxes |
| [faster_rcnn_resnet101_lowproposals_coco](http://download.tensorflow.org/models/object_detection/faster_rcnn_resnet101_lowproposals_coco_2018_01_28.tar.gz) | 82 |  | Boxes |
| [faster_rcnn_inception_resnet_v2_atrous_coco](http://download.tensorflow.org/models/object_detection/faster_rcnn_inception_resnet_v2_atrous_coco_2018_01_28.tar.gz) | 620 | 37 | Boxes |
| [faster_rcnn_inception_resnet_v2_atrous_lowproposals_coco](http://download.tensorflow.org/models/object_detection/faster_rcnn_inception_resnet_v2_atrous_lowproposals_coco_2018_01_28.tar.gz) | 241 |  | Boxes |
| [faster_rcnn_nas](http://download.tensorflow.org/models/object_detection/faster_rcnn_nas_coco_2018_01_28.tar.gz) | 1833 | 43 | Boxes |
| [faster_rcnn_nas_lowproposals_coco](http://download.tensorflow.org/models/object_detection/faster_rcnn_nas_lowproposals_coco_2018_01_28.tar.gz) | 540 |  | Boxes |
 
 ##### Arguments

| Argument            | Short hand | Default                | Description                                                                                                                                                                                                                                                  |
|---------------------|------------|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| label_file          | l          | ./label_map.pbtxt      | 레이블 파일이 존재하는 위치입니다.                                                                                                                                                                                                                           |
| log_level           | lv         | INFO                   | 로그 레벨을 지정합니다.<br>   로그 레벨의 종류는 다음과 같습니다.<br> [ DEBUG , INFO , WARNING , ERROR , CRITICAL ]<br>   현재는 INFO 레벨의 로그밖에 존재하지 않습니다.<br>  로그는 process.log 파일에서 확인할 수 있습니다.<br> 로그는 Re-training Automation 툴과 공유합니다. |
| reset               | r          | False                  | 리셋 여부를 설정 합니다.<br>   기본적으로 learning 은 기존 러닝과 이어서 계속 진행이 됩니다.<br><br> step 이 10000 에서 종료가 되었다면, 20000으로 learning 을 다시 시작하면, 10001 부터 시작합니다.<br>  새로운 데이터셋을 교육시키려면 reset 를 True로 설정하십시오. <br> |

##### Example

