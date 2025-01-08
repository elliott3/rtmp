notes:



# ffmpeg and vlc are working locally

kubectl apply -f pv-pvc.yaml
kubectl apply -f ffmpeg-deployment.yaml\
kubectl apply -f nginx-deployment.yaml\
kubectl apply -f ffmpeg-service.yaml\
kubectl apply -f nginx-service.yaml

# terminal
      ffmpeg -i rtmp://liteavapp.qcloud.com/live/liteavdemoplayerstreamid
      -c:v libx264 -preset veryfast -crf 28
      -c:a aac -strict experimental -b:a 128k
      -f hls -hls_time 4 -hls_list_size 10 -hls_flags delete_segments
      /shared/index.m3u8

# terminal 2
cd ./live
python3 -m http.server 8080

# vlc open
http://192.168.1.100:8080/index.m3u8 
