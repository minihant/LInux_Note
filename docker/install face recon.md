# 如何在 TensorFlow 使用深度學習建立臉部辨識 (二)
[web ](https://blog.gcp.expert/tensorflow-facial-recognition-2/)

## 使用 Dlib 和 Docker 預先處理數據
1. 透過 LFW (Labeled Faces in the Wild) 數據集作為訓練數據
> # Download lfw dataset
>curl -O http://vis-www.cs.umass.edu/lfw/lfw.tgz # 177MB
>tar -xzvf lfw.tgz
>
>> Directory Structure
>> ├── Tyra_Banks
>> │ ├── Tyra_Banks_0001.jpg
>> │ └── Tyra_Banks_0002.jpg
>> ├── Tyron_Garner
>> │ ├── Tyron_Garner_0001.jpg
>> │ └── Tyron_Garner_0002.jpg

## 預先處理
> 你將使用 docker 安裝 tensorflow、opencv 和 Dlib。Dlib 提供一個可以用於臉部檢測和對齊的資料庫 (libraries)。這些資料庫可能有點難以安裝，使用 docker 進行安裝更為方便。
[requirement.txt](https://gist.github.com/ColeMurray/382f28997a568871c380d33ea4d11ee1#file-requirements-txt)
[Dockerfile](https://gist.github.com/ColeMurray/574c2a2d22e39b513052718b66de9c5d#file-dockerfile)

- 使用下面的指令來建構 docker 映像檔(build docker image)：
> docker build -t colemurray/medium-facenet-tutorial -f Dockerfile .
>
> This can take several minutes depending on your hardware
> On MBP, ~ 25mins
> Image can be pulled from dockerhub below
```
 docker pull colemurray/medium-facenet-tutorial
 ```

## 檢測, 裁剪和與 Dlib 對齊
- 首先下載 dlib 的臉部座標預測軟體 (dlib’s face landmark predictor)。
> curl -O http://dlib.net/files/shape_predictor_68_face_landmarks.dat.bz2
> bzip2 -d shape_predictor_68_facelandmarks.dat.bz2
[align_dlib.py：](https://gist.github.com/ColeMurray/ece4c62baafb4a8117bd93a5a4b13088#file-align_dlib-py)

- 接下來，你可以為你的數據集創建一個預處理器。這個文件將讀取每個圖像到記憶體中，嘗試找到最大的臉，中心對齊，並輸出執行結果。如果在圖像中找不到臉部，日誌記錄會顯示文件名，以方便控管。
[preprocess.py](https://gist.github.com/ColeMurray/b124c4bd0082e4a38a829b69cb5cb19c#file-preprocess-py)

## 觀看目前的成果
- 現在你已經創建了一個 pipeline，是時候看結果了！由於我們的 script 用多個 CPU 可以有效提升執行效能。您需要在 docker 環境中跑行預處理器才能使用已安裝的資料庫 (libraries)。
以下的指令是將你的專案目錄以 volume 的形式掛載進入 docker 容器中，並對輸入數據執行預處理腳本(preprocessing script)。結果將寫入使用命令參數指定的目錄。
- 
> docker run -v $PWD:/medium-facenet-tutorial \
>  -e PYTHONPATH=$PYTHONPATH:/medium-facenet-tutorial \
>  -it colemurray/medium-facenet-tutorial python3 /medium-facenet-tutorial/medium_facenet_tutorial/preprocess.py \
>  --input-dir /medium-facenet-tutorial/data \
>  --output-dir /medium-facenet-tutorial/output/intermediate \
>  --crop-dim 180

- Code up to this point can be found [here](https://github.com/ColeMurray/medium-facenet-tutorial/tree/add_alignment)

現在我們了解了人臉辨識的概念，並完成了初步的實作，下一篇最終章將完整您對臉部辨識的了解。
- 上一篇：如何在 TensorFlow [使用深度學習建立臉部辨識1](https://blog.gcp.expert/tensorflow-facial-recognition-1/)