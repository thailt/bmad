# BMAD_AUDIO_WORKFLOW

## Tên workflow

- `audio`

## Mục tiêu

Biến một yêu cầu text thành audio, sau đó đưa artifact audio vào repo `BMad` để lưu trữ, version hóa, và chia sẻ qua Git.

## Outcome

Sau khi workflow chạy xong, phải có đủ các đầu ra sau:

- file audio được generate từ text
- file audio được copy sang repo `BMad`
- commit mới trong nhánh làm việc
- commit đã được push lên remote

## Input

Người dùng cung cấp:

- nội dung text cần convert thành audio
- nếu cần: style/giọng đọc mong muốn
- nếu cần: lựa chọn `denoise` hoặc không
- nếu cần: tên file output mong muốn

## Repos / paths

### TTS workspace

- repo: `/Users/admin/Documents/DDD/voxcpm`
- dùng để chạy `VoxCPM` và generate file audio

### Target repo

- repo: `/Users/admin/Documents/DDD/BMad`
- dùng để lưu artifact audio và push lên GitHub

## Naming convention

### Trong `voxcpm/outputs`

- ưu tiên tên file ngắn, mô tả đúng nội dung
- ví dụ:
  - `bmad-intro-vi.wav`
  - `bmad-intro-vi-optimized-denoised.wav`
  - `system-decomposition-vi.wav`

### Trong `BMad`

- copy giữ nguyên tên file nếu tên đã rõ nghĩa
- tránh tên mơ hồ như `out.wav`, `test.wav`, `audio.wav`

## Flow chuẩn

### 1. Nhận yêu cầu

Xác định rõ:

- text đầu vào là gì
- có cần `denoise` không
- có cần `optimize` không
- tên file output là gì

Lưu ý thực tế hiện tại:

- trên máy Apple Silicon chạy `mps`, `optimize` không thực sự hoạt động như trên CUDA
- vì vậy nếu người dùng yêu cầu `optimize`, nên nói rõ là runtime có thể không optimize thật trên MPS

### 2. Generate audio ở repo `voxcpm`

Chạy từ repo:

- `/Users/admin/Documents/DDD/voxcpm`

Mẫu lệnh cơ bản:

```bash
./.venv/bin/python -m voxcpm.cli design \
  --device mps \
  --no-optimize \
  --no-denoiser \
  --text "<TEXT>" \
  --output outputs/<FILE>.wav
```

Mẫu lệnh khi bật denoise:

```bash
./.venv/bin/python -m voxcpm.cli design \
  --device mps \
  --denoise \
  --text "<TEXT>" \
  --output outputs/<FILE>.wav
```

## 3. Verify output

Kiểm tra tối thiểu:

- file có tồn tại không
- tên file đúng không
- duration có hợp lý không
- format là WAV hay không

Ví dụ:

```bash
ls -lh outputs/<FILE>.wav
file outputs/<FILE>.wav
```

## 4. Copy sang repo `BMad`

```bash
cp /Users/admin/Documents/DDD/voxcpm/outputs/<FILE>.wav /Users/admin/Documents/DDD/BMad/
```

## 5. Commit vào repo `BMad`

Nguyên tắc:

- chỉ add đúng file audio cần commit
- không commit `.idea/` hay file IDE
- commit message phải mô tả nội dung audio

Ví dụ:

```bash
git -C /Users/admin/Documents/DDD/BMad add <FILE>.wav
git -C /Users/admin/Documents/DDD/BMad commit -m "Add <short-description> audio"
```

## 6. Push lên remote

```bash
git -C /Users/admin/Documents/DDD/BMad push origin media
```

## Guardrail quan trọng

### 1. Không chạy commit và push song song

Bài học thực tế trong repo này:

- nếu `commit` và `push` chạy song song, `push` có thể đi trước commit mới
- kết quả là local có commit mới nhưng remote chưa có

Vì vậy flow đúng là:

1. commit xong
2. kiểm tra HEAD
3. push sau
4. verify remote sau khi push

### 2. Luôn verify remote sau push

Sau khi push, nên check:

```bash
git -C /Users/admin/Documents/DDD/BMad rev-parse HEAD origin/media
```

Nếu 2 commit hash khác nhau:

- nghĩa là remote chưa ăn commit mới
- phải push lại riêng một lần nữa

### 3. Không commit file rác

Không add các file như:

- `.idea/`
- file cache
- file output tạm không có ý nghĩa

## Recommended step-by-step checklist

- [ ] xác nhận text đầu vào
- [ ] xác nhận chế độ `denoise` / non-denoise
- [ ] generate audio ở `voxcpm`
- [ ] verify file output
- [ ] copy sang `BMad`
- [ ] `git add` đúng file
- [ ] `git commit`
- [ ] `git push origin media`
- [ ] verify `HEAD == origin/media`
- [ ] báo lại cho người dùng: file, commit, branch

## Suggested reply format sau khi xong

- file audio: `<FILE>.wav`
- repo: `/Users/admin/Documents/DDD/BMad`
- branch: `media`
- commit: `<SHA>`
- trạng thái push: `đã push`

## Workflow summary

`audio` =

1. nhận text
2. generate bằng `voxcpm`
3. kiểm tra output
4. copy sang `BMad`
5. commit
6. push
7. verify remote

Đây là workflow artifact để tái sử dụng cho mọi request kiểu “biến text thành audio rồi đưa vào repo BMad”.
