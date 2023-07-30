# word2vec 텍스트 유사도

### 단어 유사도 계산

```

# gensim 패키지의 model.wv.similarity() 메서드를 호출하면 두 단어 단어의 유사도를 계산해준다.
print("일요일 = 월요일\t", model.wv.similarity(w1='일요일', w2='월요일'))
print("안성기 = 배우\t", model.wv.similarity(w1='안성기', w2='배우'))
print("대기업 = 삼성\t", model.wv.similarity(w1='대기업', w2='삼성'))
print("일요일 != 삼성\t", model.wv.similarity(w1='일요일', w2='삼성'))
print("히어로 != 삼성\t", model.wv.similarity(w1='히어로', w2='삼성'))

```

'일요일'과 '월요일'에 대한 유사도를 계산한 결과를 보면 0.91... 로 수치가 높다. 유사도가 1에 가까울수록 두 단어는 동일한 의미이거나 문법적으로 관계되어 있을 확률이 높다.
'안성기'와 '배우' 역시 유사도가 0.72...로 관련이 있다고 볼 수 있다.

이해하기 힘든 결과를 출력하는 경우도 있지만, 이는 주제에 맞는 말뭉치 데이터가 부족해서 생기는 현상이므로 품질 좋은 말뭉치 데이터를 학습하면 임베딩 성능이 많이 좋아질 수 있다.

![image](https://user-images.githubusercontent.com/66985977/222943278-679edff2-f47e-4b89-a1be-91cd668f350c.png) <br/>

### 가장 유사한 단어 추출

```

print("안성기 topn 5")
print(model.wv.most_similar("안성기", topn=5))
print('\n')
print("시리즈 topn 5")
print(model.wv.most_similar("시리즈", topn=5))

```

![image](https://user-images.githubusercontent.com/66985977/222943324-669bb800-e1bd-41b1-bb02-324f17b51531.png) 


## word2vec 단어 임베딩 시각화 (*참고)

### 2차원 벡터 형태로 html 파일로 생성

plotly 라이브러리 패키지를 추가로 필요로 함.
```

from gensim.models import Word2Vec
from konlpy.tag import Komoran
import time


# 네이버 영화 리뷰 데이터 읽어옴
def read_review_data(filename):
    # UnicodeDecodeError 'cp949' code 에러가 발생할 수 있으므로, encoding을 함.
    with open(filename, 'r', encoding='utf-8') as f:
        # f.read().splitlines() 한줄씩 리스트 형태로 line 변수에 담음 (for in)
        # line 변수에서 이스케이프 문자 '탭'(\t) 을 기준으로 split하여 리스트로 생성
        # 최종적으로 2차원 리스트 형태로 반환
        data = [line.split('\t') for line in f.read().splitlines()]
        data = data[1:] # 해당 파일 뭉치에서 0row에 해당하는 데이터는 헤더에 해당하기에 제거함.
    return data


# 측정 시작
# 소요되는 시간을 파악하기 위해 선언
start = time.time()

# 리뷰 파일 읽어오기
print('1) 말뭉치 데이터 읽기 시작')
review_data = read_review_data('data_model/ratings.txt') # 말뭉치 파일 경로
print(len(review_data)) # 리뷰 데이터 전체 개수
print('1) 말뭉치 데이터 읽기 완료: ', time.time() - start)

# 문장단위로 명사만 추출해 학습 입력 데이터로 만듬
print('2) 형태소에서 명사만 추출 시작')
komoran = Komoran()
docs = [komoran.nouns(sentence[1]) for sentence in review_data]
print('2) 형태소에서 명사만 추출 완료: ', time.time() - start)

# word2vec 모델 학습
print('3) word2vec 모델 학습 시작')
# sentences : Word2Vec 모델 학습에 필요한 문장 데이터, Word2Vec 모델의 입력값으로 사용됨.
# vector_size : 단어 임베딩 베겉의 차원(크기)
# window : 주변 단어 윈도우의 크기
# min_count : 단어 최소 빈도 수 제한 (설정된 min_count 빈도 수 이하의 단어들은 학습하지 않음)
# sg : 0(CBOW 모델), 1(skip-gram 모델)
model = Word2Vec(sentences=docs, vector_size=200, window=4, min_count=2, sg=1)
print('3) word2vec 모델 학습 완료: ', time.time() - start)

word_vectors = model.wv
vocabs = list(model.wv.index_to_key)
word_vectors_list = [word_vectors[v] for v in vocabs]

from sklearn.decomposition import PCA
pca = PCA(n_components=2)
xys = pca.fit_transform(word_vectors_list)
xs = xys[:,0]
ys=xys[:,1]

import matplotlib.pyplot as plt
from matplotlib import rcParams
rcParams['font.family'] = 'Malgun Gothic'

import matplotlib.pyplot as plt

import plotly
import plotly.graph_objects as go
def plot_2d_graph(vocabs, xs, ys):
    plt.figure(figsize=(15, 10))
    plt.scatter(xs, ys, marker='o')
    text = []
    for i, v in enumerate(vocabs):
        text.append(v)

    fig = go.Figure(data=go.Scatter(x=xs,
                                    y=ys,
                                    mode='markers+text',
                                    text=text))

    fig.update_layout(title='Naver Word2Vec')
    fig.show()

    plotly.offline.plot(
        fig, filename='naver_word2vec.html'
    )


plot_2d_graph(vocabs, xs, ys)

```

![image](https://user-images.githubusercontent.com/66985977/222960139-b56be960-4579-4006-bae6-c54cdf5c1b8b.png)<br/>

### 3차원 벡터 형태 

임베딩 프로젝터 : https://projector.tensorflow.org/

해당 페이지에서 3차원 벡터를 조회하기 위해서는 앞서 학습된 모델 파일을 사용하여, metadata 파일과 tensor 파일을 생성해야 한다.

```

model.wv.save_word2vec_format('model/nvmc_model') #모델저장


```

그 후 모델을 저장한 디렉터리로 이동하여 다음과 같은 스크립트를 이용해 파일을 생성한다.

```
# python -m gensim.scripts.word2vec2tensor --input [모델명] --output [모델명]
python -m gensim.scripts.word2vec2tensor --input nvmc_model --output nvmc_model

```

![image](https://user-images.githubusercontent.com/66985977/223020539-865651f8-05a3-44dd-8b9e-78fb9e4ff4a8.png) <br/>

임베딩 프로젝터 페이지 접속 후 load를 누르고

![image](https://user-images.githubusercontent.com/66985977/223020817-49aa4cb8-974d-4316-ae09-915a37c1032a.png) <br/>

step1 에는 앞서 생성한 파일중 tensor.tsv 파일을 선택해주고
step2 에는 metadata.tsv 파일을 선택한다.

![image](https://user-images.githubusercontent.com/66985977/223020995-59a30ebc-f12e-41df-9926-8bd95bb2eca9.png) <br/>

## gensim.scripts.word2vec2tensor Script Error

``` ValueError: could not convert string to float ```

에러가 나는 텍스트와 그렇지 않은 텍스트를 비교해 보면 에러가 나는 텍스트에는 공백이 포함되어 있었고
한국어 형태소에서 명사를 추출하는 과정에서 문제가 있었던 것 같다.

![image](https://user-images.githubusercontent.com/66985977/223035529-aa346593-9c70-40f6-8df5-faae3f9e12c0.png) <br/>

```

komoran = Komoran()
docs = [komoran.nouns(sentence[1]) for sentence in review_data]

```

위 코드로 했을 때 특정 문장에서 공백이 포함되는 문제가 있어서 아래처럼 조치를 했다.

```

komoran = Komoran()
docs = [komoran.nouns(sentence[1]) for sentence in review_data]
for myungsaList in docs :
    for myungsa in myungsaList :
        myungsaList[myungsaList.index(myungsa)] = myungsa.replace(" ", "")

print(docs)

```

for each 를 사용하여 명사안에 있는 공백을 모두 치환하였다.

## 정리

단어 임베딩은 의미와 문법적 정보를 이용하고, 이를 기반으로 벡터에서 단어의 위치가 선정되며<br/>
그 단어주변에 있는 다른 단어들은 의미가 유사하다고 볼 수 있는것이고 이는 텍스트 유사도 방법론중 인공 신경망을 이용한 방법중 하나이다.
