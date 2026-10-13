# QJ OCR · Dịch màn hình

Ứng dụng Windows x64 khoanh vùng màn hình, chạy RapidOcrNet/ONNX OCR tại máy, rồi đưa text có thể sửa vào Firefox Translations. UI: Việt, English, 简体中文, 한국어, 日本語.

## Tải và chạy

Dùng [gói Windows x64](dist/QJOcrTranslator-Windows-x64.zip), giải nén **toàn bộ** thư mục và chạy `QJOcrTranslator.exe`. Bản nhẹ framework-dependent cần [.NET 8 Desktop Runtime x64](https://dotnet.microsoft.com/download/dotnet/8.0). Ứng dụng không upload ảnh OCR.

1. Chọn ngôn ngữ giao diện ở góc phải trên.
2. Chọn profile OCR phù hợp với chữ nguồn.
3. Nhấn **Khoanh vùng & OCR** hoặc `Ctrl+Alt+O`; kéo vùng cần quét, nhấn `Esc` để hủy.
4. Sửa text nếu cần rồi nhấn **Dịch văn bản hiện tại**. Đích dịch đặt là tiếng Việt qua `about:translations#src=detect&trg=vi`.

Lựa chọn tự dán/clipboard được lưu trong `%LOCALAPPDATA%\QJOcrTranslator\settings.json`. Firefox cần cài mô hình dịch riêng ở lần đầu nếu chưa có.

## Chọn model theo ngôn ngữ

- **PP-OCRv6 Medium**: lựa chọn lớn hơn để thử với Trung giản thể/phồn thể, Anh, Nhật và chữ thuộc 46 ngôn ngữ Latin; nạp lâu và dùng nhiều RAM hơn v5 mobile. Tài liệu PaddleOCR **không liệt kê tiếng Hàn** trong 50 ngôn ngữ này. Bạn vẫn có thể thử v6 với chữ Hàn khó, nhưng không nên kỳ vọng được hỗ trợ/độ chính xác chính thức.
- **PP-OCRv5 Mobile Korean**: profile chuyên dụng cho tiếng Hàn/Hangul; dùng làm phương án dự phòng nếu v6 không nhận tốt.
- **PP-OCRv5 Mobile Chinese/Japanese**: Trung giản thể/phồn thể, Nhật và Anh; nhẹ hơn.
- **PP-OCRv5 Mobile Latin**: Việt/Anh; nhẹ.
- **PP-OCRv5 Mobile East Slavic**: Nga/Cyrillic.

Bạn ưu tiên nguồn Trung/Nhật/Anh để dịch sang tiếng Việt nên việc v6 dictionary thiếu một số dạng chữ tiếng Việt **không cản trở luồng đó**. Nhưng nếu OCR chính văn bản tiếng Việt có dấu, hãy dùng v5 Latin: PaddleOCR issue [#18254](https://github.com/PaddlePaddle/PaddleOCR/issues/18254) ghi nhận v6 dictionary thiếu 88/196 ký tự đơn tiếng Việt. Đừng tự nối/chỉnh dictionary, vì thứ tự ký tự phải khớp chính xác với class index của model.

Ứng dụng giới hạn ONNX ở một luồng CPU và chỉ nạp một profile tại một thời điểm. Model Medium lớn hơn có thể giúp thử các cảnh phức tạp, nhưng kích thước lớn **không tự đảm bảo** nhận tốt mọi ngôn ngữ.

## PP-OCRv6: vì sao báo thiếu dictionary?

`PP-OCRv6_rec_medium.onnx` chỉ là **recognizer**, chưa đủ một pipeline OCR. App cần cả:

1. `PP-OCRv6_rec_medium.onnx` — recognizer.
2. `PP-OCRv6_det_medium.onnx` — detector tìm vùng chữ.
3. **Dictionary bắt buộc** — file nguồn chính thức tên `ppocrv6_dict.txt`; RapidOcrNet preset yêu cầu tên local `ppocrv6_medium_dict.txt`. App tải file nguồn rồi lưu đúng alias này. Dictionary ánh xạ output ID về ký tự, không phải gói ngôn ngữ tùy chọn, nên không thể bỏ qua kể cả khi không quét tiếng Việt.
4. `ch_PP-LCNet_x0_25_textline_ori_cls_mobile.onnx` — classifier dùng chung, đã kèm với profile v5 trong app.

Mở **Quản lý model**:

- Nếu đã tải recognizer: nhấn **Nhập file Rec đã tải** và chọn `PP-OCRv6_rec_medium.onnx`; app tự lấy các file đồng hành còn thiếu.
- Nếu Rec đã nằm trong thư mục model nhưng detector/dictionary còn thiếu: nhấn **Tải file còn thiếu**. Nút này chỉ tải detector/dictionary và **không tải lại Rec đã có**.
- **Tải trọn bộ v6 Medium** chỉ tải asset nào còn thiếu; nếu Rec đã có thì không tải lại Rec.
- **Mở thư mục model** để kiểm tra các file dưới `%LOCALAPPDATA%\QJOcrTranslator\models\v6`.

Nguồn dictionary: [ModelScope `ppocrv6_dict.txt`](https://www.modelscope.cn/models/RapidAI/RapidOCR/resolve/v3.10.0/paddle/PP-OCRv6/rec/PP-OCRv6_rec_medium/ppocrv6_dict.txt). Model Manager đối chiếu SHA-256 với bản RapidOCR đã pin; file lấy từ nguồn khác có thể hiện cảnh báo.

## Tái dùng Firefox, không mở tab mới mỗi lần

Khi Firefox đã chạy, app đưa cửa sổ hiện có lên trước, nhập URL dịch vào thanh địa chỉ (`Ctrl+L`) rồi Enter; app không gọi lệnh mở URL để sinh thêm một tab mới mỗi lần quét. Nếu Firefox đang có tab `about:translations#src=detect&trg=vi` và gợi ý tab đang bật, Firefox có thể chuyển sang tab đó. Nếu không tìm thấy tab khớp, app sẽ dùng tab đang hoạt động. Muốn giữ nguyên trang khác, hãy mở sẵn tab dịch một lần trước khi dùng. Tự dán vào giao diện dịch đặc biệt của Firefox vẫn là best-effort, cần thử trên phiên bản Firefox/Windows của bạn.

App không bật Remote Agent/remote-debugging của Firefox. Cơ chế đó cần cấu hình riêng, chỉ có thể có sẵn trong một số bản Nightly, và không phù hợp để mở cổng điều khiển trình duyệt cho ứng dụng đơn giản này.

## Build từ source trên Windows

Yêu cầu .NET 8 SDK x64 và internet để restore NuGet/tải các model đã chọn.

```powershell
cd QJOcrTranslator
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\Get-LanguageModels.ps1
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\Build-Windows.ps1
```

Kết quả nằm ở `QJOcrTranslator\publish\QJOcrTranslator.exe` và `models\v5`. Model v6 chỉ được tải khi bạn chủ động cài trong Model Manager. GitHub Actions có workflow build Windows và lưu ZIP artifact.

## Chia sẻ dự án trên GitHub

Mã nguồn gốc của dự án được cấp phép MIT; xem file [`LICENSE`](LICENSE). MIT cho phép sử dụng, chỉnh sửa và phân phối lại, kể cả mục đích thương mại, với điều kiện giữ thông báo bản quyền và license. Không commit model weights/dictionary hoặc output build; `.gitignore` đã loại chúng. Model và dependencies bên thứ ba có điều khoản riêng trong `ThirdPartyLicenses` và [model-assets.md](docs/model-assets.md).

## Tệp chính

- `QJOcrTranslator/MainForm.cs` — cửa sổ, chọn vùng, OCR, clipboard, Firefox.
- `QJOcrTranslator/ModelManagerForm.cs` — nhập model, tải file thiếu, xác minh hash.
- `QJOcrTranslator/ModelCatalog.cs` — profile, file assets, checksum.
- `QJOcrTranslator/UiText.cs` — UI 5 ngôn ngữ.
- `QJOcrTranslator/SettingsStore.cs` — lưu lựa chọn cục bộ.
- `QJOcrTranslator/Get-LanguageModels.ps1`, `Build-Windows.ps1` — tải asset v5/build.
- `.github/workflows/build-windows.yml` — build tự động.

## Nguồn

- [RapidOcrNet](https://github.com/BobLd/RapidOcrNet)
- [RapidOCR model config](https://github.com/RapidAI/RapidOCR/blob/main/python/rapidocr/default_models.yaml)
- [PaddleOCR PP-OCRv6: model tiers và coverage](https://github.com/PaddlePaddle/PaddleOCR/blob/main/docs/version3.x/algorithm/PP-OCRv6/PP-OCRv6.en.md)
- [PaddleOCR issue #18254: dictionary tiếng Việt](https://github.com/PaddlePaddle/PaddleOCR/issues/18254)
- [Mozilla: Firefox address bar and open-tab suggestions](https://support.mozilla.org/en-US/kb/search-firefox-address-bar)
