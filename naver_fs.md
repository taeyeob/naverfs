## 목차
1. 네이버금융에서 기업실적분석 표를 가져오기 위한 모듈 다운로드
2. 소스코드
3. 유의사항

## 모듈 다운로드
1. 명령 프롬프트(cmd)를 켠다.
2. **pip install pandas** 를 입력하여 pandas 모듈을 다운로드한다.

## 소스코드
1. 원하는 폴더에 **naver_fs.py** 라는 파일을 생성한다.
2. 다음 코드를 작성한다.
```Python
import pandas as pd

print("종목코드를 입력하세요")
enter = input().split()

writer = pd.ExcelWriter('naver_fs.xlsx', engine='xlsxwriter')
for i in range(len(enter)):
    url_tmpl = 'https://finance.naver.com/item/main.nhn?code=%s'
    url = url_tmpl %(enter[i])
    tables = pd.read_html(url, encoding='euc-kr')
    df = tables[3]
    df.to_excel(writer, sheet_name=enter[i])

writer.save()
```

## 유의사항
1. 원하는 기업의 실적분석표를 가져오기 위해서는 기업별 종목코드(ex: 삼성전자의 경우 005930)를 입력하여야 한다. 기업명을 입력해서는 안 된다.
2. 빠른 시일 내에 더욱 효율적으로 많은 기업의 정보를 가져올 수 있도록 수정할 예정.