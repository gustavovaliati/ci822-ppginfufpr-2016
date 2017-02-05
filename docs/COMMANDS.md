Regex utilizado para substituir final de linha nos arquivos positives.txt e negatives.txt

```
%s/\n/\r/g
%s/\n/\r\r/g
```

Comandos para gerar a lista de referência das amostras
```
find positive_images -iname "*.jpg" > positives.txt
find negative_images -iname "*.jpg" > negatives.txt

find positive_convert_50 -iname "*.jpg" > positives.txt
find negative_convert_200 -iname "*.jpg" > negatives.txt

find 50pos_inria_png -iname "*.png" > positives.txt ;
find 200neg_inria_png -iname "*.png" > negatives.txt

find 50pos_inria_jpg -iname "*.jpg" > positives.txt ;
find 200neg_inria_jpg -iname "*.jpg" > negatives.txt

find 50pos_inria_original -iname "*.png" > positives.txt ;
find 200neg_inria_original -iname "*.png" > negatives.txt
```
Comandos para criação de novas amostras
```
perl bin/createsamples.pl positives.txt negatives.txt samples 1500\
  "opencv_createsamples -bgcolor 0 -bgthresh 0 -maxxangle 1.1\
  -maxyangle 1.1 maxzangle 0.5 -maxidev 40 -w 19 -h 19"

perl bin/createsamples.pl positives.txt negatives.txt samples 1500\
  "opencv_createsamples -bgcolor 0 -bgthresh 0 -maxxangle 1.1\
  -maxyangle 1.1 maxzangle 0.5 -maxidev 40 -w 20 -h 33"

perl bin/createsamples.pl positives.txt negatives.txt samples 1500\
  "opencv_createsamples -bgcolor 0 -bgthresh 0 -maxxangle 1.1\
  -maxyangle 1.1 maxzangle 0.5 -maxidev 40 -w 96 -h 160"
```

Mesclagem de amostras

```
python ./tools/mergevec.py -v samples/ -o samples.vec
```

Treinamento haar
```
opencv_traincascade -data classifier -vec samples.vec -bg negatives.txt\
  -numStages 20 -minHitRate 0.9 -maxFalseAlarmRate 0.5 -numPos 40\
  -numNeg 180 -w 19 -h 19 -mode ALL -precalcValBufSize 1024\
  -precalcIdxBufSize 1024

opencv_traincascade -data classifier -vec samples.vec -bg negatives.txt\
  -numStages 20 -minHitRate 0.9 -maxFalseAlarmRate 0.5 -numPos 40\
  -numNeg 180 -w 20 -h 33 -mode ALL -precalcValBufSize 1024\
  -precalcIdxBufSize 1024

opencv_traincascade -data classifier -vec samples.vec -bg negatives.txt\
  -numStages 20 -minHitRate 0.9 -maxFalseAlarmRate 0.5 -numPos 40\
  -numNeg 180 -w 96 -h 160 -mode ALL -precalcValBufSize 1024\
  -precalcIdxBufSize 1024
  
  ```
