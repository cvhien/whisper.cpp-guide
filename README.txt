# whisper.cpp-guide
# Hướng dẫn cơ bản sử dụng whisper.cpp để lấy lời thoại trong file âm thanh
1. Mã nguồn và hướng dẫn:
https://github.com/ggerganov/whisper.cpp

- Nhận diện lời thoại có độ chính xác cao và đồng bộ với thời gian
- Từ vựng chau chuốt, hợp ngữ cảnh
- Nhận diện đa ngôn ngữ.
- Dịch từ ngôn ngữ khác sang tiếng Anh
- Hiện tại chạy mất khoảng 40 phút cho một bài 1 - 2 tiếng với mô hình tốt whisper large-v2

2. Cấu hình:
RAM khoảng 8 - 16GB. Hiện tại đang cơ bản gần như chỉ tận dụng CPU.
Các mô hình nhỏ yêu cầu thấp hơn.

3. Bước cần làm:
- Tải về, build mã nguồn
- Tải về model
- Tải về và chuyển đổi file âm thanh sang wav 16bit
- Chạy whisper.cpp lấy lời

4. Chi tiết:
a, Tải về, build mã nguồn
$:git clone https://github.com/ggerganov/whisper.cpp.git
$:make

b, Tải về model
Mô hình đã chuyển đổi sang dạng ggml, dùng được luôn, large-v2 (ggml-model-large-v2.bin, 3GB):
https://drive.google.com/file/d/1NYffbFE-z3mIu8GKf8BAOVxQFQc9CBG7/view?usp=share_link
(Copy vào thư mục -> whisper.cpp/models/ggml-model-large-v2.bin)

Hoặc dùng script, tương tự như này cho model base.en
$:bash ./models/download-ggml-model.sh base.en
(Chúng nằm trong đây: https://huggingface.co/ggerganov/whisper.cpp/tree/main)

c, Tải về và chuyển đổi file âm thanh sang .wav 16bit
Cài yt-dlp:
$:pip install yt-dlp
Dùng công cụ youtube download tải về file âm thanh, cho bài https://youtu.be/ID.
$:yt-dlp -f 251 -o "%(id)s.%(ext)s" ID
(Tải về file ID.webm với tuỳ chọn định dạng 251.
Hiển thị các định dạng hỗ trợ tải về:
$:yt-dlp -F ID)
Chuyển sang định dạng được hỗ trợ, wav 16bit:
$:ffmpeg -i ID.webm -ar 16000 -ac 1 -c:a pcm_s16le ID_index34.wav
($:ffmpeg -i file.mp3 -ar 16000 -ac 1 -c:a pcm_s16le ID_index34.wav)

d, Chạy whisper.cpp lấy lời
Lấy lời thử:
$:./main -m ./models/ggml-model-large-v2.bin -f samples/jfk.wav
