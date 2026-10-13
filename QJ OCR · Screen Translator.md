# QJ OCR · Screen Translator

> **AI credit / Ghi công AI:** This project’s initial prototype and documentation were created with help from **Manus AI (Free plan)**. / Bản thử nghiệm và tài liệu ban đầu của dự án được tạo với sự hỗ trợ của **Manus AI (gói Free)**.
>
> The idea and project direction came from QJ, a non-programmer. This is an independent community project, not an official Manus, Mozilla, Microsoft, or RapidAI product. / Ý tưởng và định hướng dự án đến từ QJ, một người không chuyên lập trình. Đây là dự án cộng đồng độc lập, không phải sản phẩm chính thức của Manus, Mozilla, Microsoft hay RapidAI.

## The idea · Ý tưởng dự án

### English

I am not a programmer and do not know a programming language. I found that setting up separate offline OCR and translation tools could take time, use a lot of disk space, and be difficult on lower-spec computers. I wondered whether a small Windows app could select text from the screen, recognize it locally, and hand the result to Firefox Translations.

With help from Manus AI’s Free plan, I was able to turn that idea into an early C# prototype. I could not take it much further on my own, so I am sharing the project in the hope that people around the world—especially fellow Vietnamese—might find it interesting and help make it better.

If you work with C#, Windows, OCR, Firefox, testing, documentation, or translation, your feedback and improvements are welcome. You do not need to be an expert to help: trying it on a different computer, reporting a bug, suggesting clearer instructions, or improving a translation can all make a difference.

### Tiếng Việt

Tôi không biết lập trình và cũng chưa biết ngôn ngữ lập trình nào. Tôi thấy việc cài riêng các công cụ OCR và dịch offline đôi khi mất thời gian, tốn dung lượng, và gây khó cho những máy cấu hình thấp. Tôi nghĩ liệu có thể làm một ứng dụng Windows nhỏ để khoanh vùng chữ trên màn hình, nhận dạng ngay trên máy, rồi chuyển nội dung đó sang Firefox Translations hay không.

Với sự hỗ trợ của Manus AI gói Free, tôi đã biến ý tưởng ấy thành một bản thử nghiệm C#. Tôi chưa thể tự phát triển xa hơn, nên chia sẻ dự án này với hy vọng mọi người trên thế giới—đặc biệt là anh em Việt Nam—thấy nó thú vị và có thể cùng làm cho nó tốt hơn.

Nếu bạn biết C#, Windows, OCR, Firefox, kiểm thử, viết tài liệu hoặc dịch thuật, mọi góp ý và cải tiến đều được hoan nghênh. Bạn không cần phải là chuyên gia mới giúp được: thử trên máy khác, báo lỗi, góp ý hướng dẫn dễ hiểu hơn, hay sửa bản dịch đều có ích.

## What it does · Công cụ làm gì?

- Select a screen region with a shortcut or button, then run local OCR with RapidOcrNet/ONNX. / Khoanh vùng màn hình bằng phím tắt hoặc nút bấm, rồi chạy OCR local bằng RapidOcrNet/ONNX.
- Edit or copy the recognized text and send it to Firefox Translations. / Sửa hoặc sao chép văn bản đã nhận dạng rồi chuyển sang Firefox Translations.
- Choose an OCR profile and a UI language: Vietnamese, English, Simplified Chinese, Korean, or Japanese. / Chọn profile OCR và ngôn ngữ giao diện: Việt, Anh, Trung giản thể, Hàn hoặc Nhật.
- Keep OCR on the computer; the app does not upload screenshots to an OCR server. The recognized text is handed to Firefox, whose translation behavior depends on its own version and downloaded language models. / Ảnh chụp màn hình được OCR ngay trên máy; ứng dụng không tải ảnh lên máy chủ OCR. Văn bản nhận dạng được chuyển cho Firefox; cách dịch phụ thuộc vào phiên bản Firefox và mô hình ngôn ngữ mà Firefox đã tải.

## Current limitations · Giới hạn hiện tại

This is an early community project, not a polished or fully tested product. The Windows interface and Firefox handoff still need testing on different PCs and Firefox versions. Firefox tab reuse and automatic pasting are best-effort.

PP-OCRv6 Medium is optional and downloaded/imported by the user; it is not included in the source archive. PaddleOCR’s published v6 coverage lists Chinese, English, Japanese, and Latin-script languages, but not Korean. For Korean, use the dedicated PP-OCRv5 Korean profile if v6 does not recognize it well. See [docs/model-assets.md](docs/model-assets.md) for the v6 dictionary filename and model details.

Đây là dự án cộng đồng ở giai đoạn đầu, chưa phải sản phẩm hoàn thiện hoặc đã được kiểm thử đầy đủ. Giao diện Windows và thao tác chuyển sang Firefox cần được thử trên nhiều máy và phiên bản Firefox. Tái sử dụng tab và tự dán văn bản chỉ là best-effort.

PP-OCRv6 Medium là tùy chọn, do người dùng tự nhập/tải; source archive không kèm model v6. Tài liệu PaddleOCR công bố v6 hỗ trợ tiếng Trung, Anh, Nhật và các ngôn ngữ dùng chữ Latin, nhưng không liệt kê tiếng Hàn. Nếu v6 nhận dạng tiếng Hàn chưa tốt, hãy dùng profile PP-OCRv5 Korean. Xem [docs/model-assets.md](docs/model-assets.md) để biết tên dictionary và chi tiết model.

## Try it · Dùng thử

The Windows release ZIP contains the app and PP-OCRv5 model files. It requires the .NET 8 Desktop Runtime x64. Extract the entire ZIP before running `QJOcrTranslator.exe`; do not separate the executable from its `models` folder. For detailed instructions, see [README-VI.md](README-VI.md). Build steps are in [QJOcrTranslator/README-BUILD.md](QJOcrTranslator/README-BUILD.md).

Gói Windows ZIP có ứng dụng và các model PP-OCRv5. Máy cần .NET 8 Desktop Runtime x64. Giải nén toàn bộ ZIP trước khi chạy `QJOcrTranslator.exe`; không tách EXE khỏi thư mục `models`. Hướng dẫn chi tiết bằng tiếng Việt ở [README-VI.md](README-VI.md). Cách build nằm tại [QJOcrTranslator/README-BUILD.md](QJOcrTranslator/README-BUILD.md).

## Help improve it · Cùng cải thiện dự án

You can start by opening an **Issue** to report a problem or suggest an improvement. If you are comfortable editing code or documentation, you can fork the repository and submit a **Pull Request**. Please describe the change and, for OCR changes, include the language/script and a sample that you have permission to share.

Bạn có thể bắt đầu bằng cách mở **Issue** để báo lỗi hoặc đề xuất ý tưởng. Nếu muốn sửa code hay tài liệu, bạn có thể fork repository rồi gửi **Pull Request**. Hãy mô tả thay đổi; nếu góp ý về OCR, vui lòng nêu ngôn ngữ/chữ viết và chỉ gửi mẫu mà bạn có quyền chia sẻ.

## Project license · Giấy phép dự án

**The original project code is licensed under the MIT License; see [LICENSE](LICENSE).** MIT permits use, modification, and redistribution, including commercial use, provided the copyright and license notice are retained. This project license does not replace the separate terms for third-party libraries and OCR models; see [ThirdPartyLicenses](ThirdPartyLicenses/) and the model documentation.

**Mã nguồn gốc của dự án được cấp phép theo MIT; xem file [LICENSE](LICENSE).** MIT cho phép sử dụng, chỉnh sửa và phân phối lại, kể cả mục đích thương mại, với điều kiện giữ lại thông báo bản quyền và license. License của dự án không thay thế điều khoản riêng của thư viện và model bên thứ ba; xem [ThirdPartyLicenses](ThirdPartyLicenses/) và tài liệu model.
