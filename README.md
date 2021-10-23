#### SecVision

This uses ssd-mobilenet-v2 that comes with the 

Technologies used
 
 - Jetson Nano 
 - Jetson Inference - Detectnet
 - TensorRT - Yolov4
 - HikVision DVR

### Goals

 - Use still frames from HTTP GET from DVR to analyze zones(cameras)
 - Detected persons
   - HTTP PUT to HikVision DVR
   - Use DVR output connected to input to trigger recording on DVR
 
 ### Usage

   Setup username and password and channel count
   
   Uses basic auth for HTTP request to DVR
   
      pip3 install aiofiles aiohttp asyncio

   ### jetson inference stack 

      python3 detect.py

   Takes approximately 1.1 seconds to do its thing for 8x2MP images, sometimes a little longer at 1.5s when writing files
   
   ### Tensort stack with yolov4, needs symlinks to tensorrt_demo project

      python3 detect_yolo.py

   Takes approximately 2.6 seconds to do its thing for 8x2MP images and is very accurate

   ### Automatic / Continuous Operation
   
   Change User in detect.service file to suite your environment then 

      $ sudo ./install.sh
    
   Change User in detect_yolo.service file to suite your environment then 

      $ sudo ./install_yolo.sh

   Check status of service jetson.utils using ssd-mobilenet-v2

      $ sudo service detect status

   **Or** yolov4-416 with openCV

      $ sudo service detect_yolo status
     
   The log output can be viewed by running, this will be further implemented at a later stage...

      $ sudo journalctl -u detect.service -f -n
      $ sudo journalctl -u detect_yolo.service -f -n

   To stop the service, simply run:

      $ sudo service detect stop
      $ sudo service detect_yolo stop

   Symlinks for running yolov4 version

      $ sudo ln -s /home/jetsonman/tensorrt_demos/utils/ ./utils
      $ sudo ln -s /home/jetsonman/tensorrt_demos/plugins/ ./plugins
      $ sudo ln -s /home/jetsonman/tensorrt_demos/yolo/ ./yolo