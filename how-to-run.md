# downloading and converting the models
## 1. Tensorflow SSD Mobilenet V2
        Use the following steps:
        1. Download model
            command: `wget http://download.tensorflow.org/models/object_detection/ssd_mobilenet_v2_coco_2018_03_29.tar.gz`
        2. Unpack the downloaded file
            command: `tar -xvf ssd_mobilenet_v2_coco_2018_03_29.tar.gz`
        3. change directory to the extracted folder
        4. Convert model using following command:
            `python /opt/intel/openvino/deployment_tools/model_optimizer/mo.py --input_model frozen_inference_graph.pb --tensorflow_object_detection_api_pipeline_config pipeline.config --reverse_input_channels --tensorflow_use_custom_operations_config /opt/intel/openvino/deployment_tools/model_optimizer/extensions/front/tf/ssd_v2_support.json`
        5. Run model [check port and model path]:
            `python main.py -i resources/Pedestrian_Detect_2_1_1.mp4 -m models/inference_model/frozen_inference_graph.xml -l /opt/intel/openvino/deployment_tools/inference_engine/lib/intel64/libcpu_extension_sse4.so -d CPU -pt 0.6 | ffmpeg -v warning -f rawvideo -pixel_format bgr24 -video_size 768x432 -framerate 24 -i - http://0.0.0.0:3000/fac.ffm`
        6. Save the output to file
            `python main.py -i resources/Pedestrian_Detect_2_1_1.mp4 -m models/inference_model/frozen_inference_graph.xml -l /opt/intel/openvino/deployment_tools/inference_engine/lib/intel64/libcpu_extension_sse4.so -d CPU -pt 0.6 | ffmpeg -v warning -f rawvideo -pixel_format bgr24 -video_size 768x432 -framerate 24 -i - out.avi`

## 2. Tensorflow SSD ResNet V2
Use the following steps:
        1. Download model
            command: `http://download.tensorflow.org/models/object_detection/ssd_resnet50_v1_fpn_shared_box_predictor_640x640_coco14_sync_2018_07_03.tar.gz`
        2. Unpack the downloaded file
            command: `tar -xvf ssd_resnet50_v1_fpn_shared_box_predictor_640x640_coco14_sync_2018_07_03.tar.gz`
        3. change directory to the extracted folder
        4. Convert model using following command:
            `python /opt/intel/openvino/deployment_tools/model_optimizer/mo.py --input_model frozen_inference_graph.pb --tensorflow_object_detection_api_pipeline_config pipeline.config --reverse_input_channels --tensorflow_use_custom_operations_config /opt/intel/openvino/deployment_tools/model_optimizer/extensions/front/tf/ssd_v2_support.json`
        5. Run model [check port and model path]:
            `python main.py -i resources/Pedestrian_Detect_2_1_1.mp4 -m models/inference_model/frozen_inference_graph.xml -l /opt/intel/openvino/deployment_tools/inference_engine/lib/intel64/libcpu_extension_sse4.so -d CPU -pt 0.6 | ffmpeg -v warning -f rawvideo -pixel_format bgr24 -video_size 768x432 -framerate 24 -i - http://0.0.0.0:3000/fac.ffm`
        6. Save the output to file
            `python main.py -i resources/Pedestrian_Detect_2_1_1.mp4 -m models/inference_model/frozen_inference_graph.xml -l /opt/intel/openvino/deployment_tools/inference_engine/lib/intel64/libcpu_extension_sse4.so -d CPU -pt 0.6 | ffmpeg -v warning -f rawvideo -pixel_format bgr24 -video_size 768x432 -framerate 24 -i - out-resnet.avi`









