---
cssclasses:
  - img-grid
  - img-zoom
---
## 이미지 변환하기

```python
import fitz  # PyMuPDF

def convert_pdf_to_images():
	# PDF 파일 열기
	document = fitz.open('files\Sample.pdf')

	# PDF 0페이지 불러오기
	page = document.load_page(0)

	# 이미지로 변환하기
	dpi = 72
	zoom_x = dpi / 72
	zoom_y = dpi / 72
	matrix = fitz.Matrix(zoom_x, zoom_y)
	pixmap = page.get_pixmap(matrix=matrix)

	# 이미지 저장
	pixmap.save('images\page_0.png')
```

1. `import fitz`: PyMuPDF 라이브러리를 불러옵니다.
2. PDF 파일 다루기:
	- `document = fitz.open('files\Sample.pdf')`: PDF 파일을 엽니다.
	- `page = document.load_page(0)`: PDF의 첫 번째 페이지를 불러옵니다 (0부터 시작)
3. 이미지 변환 설정:
	- `dpi = 72`: 변환할 이미지의 해상도를 지정합니다.
	- `zoom_x = dpi / 72`: 가로 방향 확대/축소 비율을 계산합니다.
	- `zoom_y = dpi / 72`: 세로 방향 확대/축소 비율을 계산합니다
	- `matrix = fitz.Matrix(zoom_x, zoom_y)`: 변환 매트릭스를 생성합니다.
4. 이미지 변환 및 저장:
	- `pixmap = page.get_pixmap(matrix=matrix)`: PDF 페이지를 이미지로 변환합니다.
	- `pixmap.save('images\page_0.png')`: 변환된 이미지를 PNG 파일로 저장합니다.

### DPI(Dot Per Inch)란?

DPI는 가로와 세로가 1인치(2.54cm)인 도화지에 들어가는 점의 개수를 의미합니다. 예를 들어 72 DPI는 가로와 세로가 1인치인 도화지에 점이 72개씩 들어있다고 생각하면 됩니다.

고품질 인쇄용이 아닌, 웹 사이트에 간단히 디스플레이할 용도라면 72 DPI로 변환하는 것으로 충분합니다. DPI 값이 높을수록 이미지 파일의 용량도 커지므로, 변환된 이미지의 품질을 확인하면서 적절한 DPI 값을 찾아가면 됩니다.

![pdf_72dpi](assets/images/pdf_72dpi.png)
![pdf_300dpi](assets/images/pdf_300dpi.png)

- 좌측: 72DPI, 50.1KB
- 우측: 300DPI, 255KB

### Zoom 값 계산하기

본 문서에서는 PDF 파일의 DPI 값을 72로 생각하고 zoom 값을 `원하는 DPI / 72`로 계산했으며, 다음과 같은 예시를 들 수 있습니다:

- 72 DPI: zoom = 72/72 = 1.0 (원본 크기)
- 150 DPI: zoom = 150/72 ≈ 2.08 (약 2배 확대)
- 300 DPI: zoom = 300/72 ≈ 4.17 (약 4배 확대)

PDF 파일의 실제 DPI가 72가 아닌 경우(예: 300 DPI), `원하는 DPI / 300`으로 계산하는 것이 좋습니다.

## 성능 고려사항

DPI 값이 높아질수록:

- 이미지 파일의 크기가 커집니다.
- 메모리 사용량이 증가합니다.
- PDF를 이미지로 변환하는 시간이 늘어납니다.