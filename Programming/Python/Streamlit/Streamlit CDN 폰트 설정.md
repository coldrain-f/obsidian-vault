# CDN 폰트 적용

```python
import streamlit as st

# 전체 폰트 설정
st.markdown(
    """
            <style>
				@import url(".../dist/web/static/pretendard.min.css");
	            * {
	                font-family: 'Pretendard' !important
	            }
            </style>
            """,
    unsafe_allow_html=True,
)
```

`st.markdown()`를 이용해 CSS(Cascading Style Sheets) 문법으로 CDN 폰트를 Import해주고 폰트를 설정해 주면 간단하게 적용 할 수 있습니다.
