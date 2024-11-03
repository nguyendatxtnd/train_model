
1. Chuẩn bị dataset 

    - đánh nhãn dữ liệu với roboflow rồi dowload dataset với yolov5 pytorch (nếu không biết có thể xem các hướng dẫn trên youtube)
    - giải nén file và tải lên gg drive

2. Train yolov5 với gg Colab 
    - lưu ý: sử dụng GPU trên colab để quá trình train nhanh hơn (edit -> notebook setting -> Hardware accelerator -> T4 GPU) 
    - kết nối với gg drive để lấy data vừa tải lên:

            from google.colab import drive
            drive.mount('/content/drive/', force_remount=True)

    - tải model và cài đặt các requirements:

            %cd /content/drive/MyDrive
            !git clone https://github.com/ultralytics/yolov5  

            %cd /content/drive/MyDrive/yolov5
            %pip install -qr requirements.txt

            import torch
            from drive.MyDrive.yolov5 import utils
            display = utils.notebook_init()

    - chỉnh sửa file .yaml - xóa phần test, và thay các đường dẫn của train và val để đúng với đường dẫn đến folder chứa ảnh (images) (sử dụng copy path cho đúng) như ví dụ sau:  

            train: /content/drive/MyDrive/data_train/train/images
            val: /content/drive/MyDrive/data_train/valid/images

            nc: 2
            names: ['hinh tron', 'hinh vuong']

            roboflow:
            workspace: src-hsvk4
            project: detect_yolov5
            version: 2
            license: CC BY 4.0
            url: https://universe.roboflow.com/src-hsvk4/detect_yolov5/dataset/2

    - bắt đầu train:

            !python train.py --img 640 --batch 16 --epochs 100 --data /content/drive/MyDrive/data_train/data.yaml --weights yolov5n.pt

        - train.py: script huấn luyện của YOLOv5, thường nằm trong thư mục chính của dự án YOLOv5.
        - --img 640: Kích thước ảnh đầu vào được đặt là 640x640 pixel, Ảnh càng lớn thì mô hình càng chi tiết, nhưng tốn nhiều tài nguyên hơn.
        - --batch 16:  Số lượng ảnh được đưa vào mô hình trong một lần (batch size)
        - --epochs 100: số vòng lặp qua toàn bộ dữ liệu trong quá trình huấn luyện
        - --data /content/drive/MyDrive/data_train/data.yaml: Đường dẫn đến file cấu hình dữ liệu data.yaml (file sửa ở trên)
        - --weights yolov5n.pt: Đường dẫn tới file trọng số ban đầu để bắt đầu huấn luyện, ở đây dùng phiển bản yolov5 Nano phiển bản nhẹ và nhanh như độ chính xác thấp hơn các phiên bản khác (có thể sử dụng các phiên bản khác như: yolov5s, yolov5m, và yolov5l).
        ("Trọng số" (weights) trong mô hình học máy và mạng nơ-ron là các giá trị số mà mô hình học được trong quá trình huấn luyện, giúp mô hình nhận diện hoặc dự đoán tốt hơn)

3. test mô hình

    - thử mô hình với các hình ảnh sau khi train:

            !python detect.py --weights /content/drive/MyDrive/yolov5/runs/train/exp3/weights/best.pt --source <path_to_image_you_want_to_test> --img 640 --conf 0.25

        - detect.py: Script để chạy mô hình YOLOv5 cho nhiệm vụ phát hiện đối tượng trên ảnh hoặc video
        - --weights: Đường dẫn đến tệp trọng số (best.pt). Đây là file trọng số đã huấn luyện (tốt nhất) từ quá trình huấn luyện trước đó
        - --source: Đường dẫn đến ảnh muốn phát hiện đối tượng (có thể sử dụng các ảnh trong folder test của data tải lên lúc đầu)
        - --img 640: Kích thước ảnh đầu vào sẽ được resize về 640x640 pixel trước khi đưa vào mô hình
        - --conf 0.25: Mức ngưỡng cho độ tin cậy (confidence threshold) ( Những dự đoán có độ tin cậy dưới 0.25 sẽ bị bỏ qua)

    - hình ảnh sau khi test sẽ được lưu trong runs/detect/exp (đương dẫn trả về sau khi chạy xong đoạn mã trên)









# train_yolo
