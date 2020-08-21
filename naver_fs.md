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
    if i == 0:
        url_tmpl = 'https://finance.naver.com/item/main.nhn?code=%s'
        url = url_tmpl %(enter[i])
        tables = pd.read_html(url, encoding='euc-kr')
        df = tables[3]
        df = df['최근 연간 실적']
        df = df['2019.12']
        df.rename(columns = {'IFRS연결':enter[i]}, inplace = True)
        df.index = ['매출액(억원)', '영업이익(억원)', '당기순이익(억원)', '영업이익률(%)', '순이익률(%)', 'ROE(%)', '부채비율(%)', '당좌비율(%)', '유보율(%)', 'EPS(원)', 'PER(배)', 'BPS(원)', 'PBR(배)', '주당배당금(원)', '시가배당율(%)', '배당성향(%)']
    else:
        url_tmpl = 'https://finance.naver.com/item/main.nhn?code=%s'
        url = url_tmpl %(enter[i])
        tables = pd.read_html(url,encoding='euc-kr')
        df_i = tables[3]
        df_i = df_i['최근 연간 실적']
        df_i = df_i['2019.12']
        df_i.rename(columns = {'IFRS연결':enter[i]}, inplace = True)
        df_i.index = ['매출액(억원)', '영업이익(억원)', '당기순이익(억원)', '영업이익률(%)', '순이익률(%)', 'ROE(%)', '부채비율(%)', '당좌비율(%)', '유보율(%)', 'EPS(원)', 'PER(배)', 'BPS(원)', 'PBR(배)', '주당배당금(원)', '시가배당율(%)', '배당성향(%)']
        df = pd.merge(df, df_i, left_index=True, right_index=True)
    df.to_excel(writer)

writer.save()
```

## 팁
1. 여러 기업의 재무상황을 한 눈에 볼 수 있도록 수정한 것.
2. 첫행은 기업별 종목코드로 나타나도록 하였으나 처음 커맨드창에 입력한 순서 그대로 출력되므로 헷갈리지 않을 것이다.