# Word2Vec

### 분산 표현의 대표적 예

Word2Vec은 2013년 구글에서 발표했으며 가장 많이 사용하고 있는 단어 임베딩 모델이다. 기존 신경망 기반의 단어 임베딩 모델에 비해 구조상 차이는 크게 없지만 계산량을 획기적으로 줄여 빠른 학습을 가능하게 하였다. Word2Vec 모델은 CBOW와 skip-gram 두 가지 모델로 제안되었다.

CBOW 모델은 맥락이라 표현되는 주변 단어들을 이용해 타깃 단어를 예측하는 신경망 모델이다. 신경망의 입력을 주변 단어들로 구성하고 출력을 타깃 단어로 설정해 학습된 가중치 데이터를 임베딩 벡터로 활용한다. SKIP-GRAM 모델은 CBOW 모델과 반대로 하나의 타깃 단어를 이용해 주변 단어들을 예측하는 신경말 모델이다.
skip-gram 모델이 CBOW 모델에 비해 예측해야 하는 맥락이 많아진다. 따라서 단어 분산 표현력이 우수해 CBOW 모델에 비해 임베딩 품질이 우수하다. 반면 CBOW 모델은 타깃 단어의 손실만 계산하면 되기 떄문에 학습 속도가 빠른 장점이 있다.

### CBOW 모델

![image](https://user-images.githubusercontent.com/66985977/222743351-98fca65d-a913-487d-af6e-d2a22986ef08.png)

### skip-gram 모델

![image](https://user-images.githubusercontent.com/66985977/222743989-fb214706-8cac-4bf6-a079-b1a34cfb2cf6.png)

각각 모델에서 타깃 단어를 예측하기 위해 앞뒤 단어를 확인하거나 또는 타깃 단어를 확인해 앞 뒤 단어 즉, 주변 단어를 예측 하려 하였다.
이때 범위를 지정 할 수 있는 이 범위를 윈도우라고 한다.

### CBOW 모델

![image](https://user-images.githubusercontent.com/66985977/222905973-74c23211-fbf2-41ed-8d63-b9dd23044d70.png)

### skip-gram 모델

![image](https://user-images.githubusercontent.com/66985977/222906107-f6af7a7f-053f-4e77-bfc0-cdeaab5c0d04.png)

<br/><br/>
Word2Vec의 단어 임베딩은 해당 단어를 밀집 벡터로 표현하며 학습을 통해 의미상 비슷한 단어들을 비슷한 벡터 공간에 위치시킨다. 또한 벡터 특성상 의미에 따라 방향성을 갖게 된다. 이때 임베딩된 벡터들 간 연산이 가능하기 때문에 사진처럼 단어 간 관계를 계산할 수 있다. 
('왕'과 '여왕'의 방향 차이만큼 '남자'와 '여자'의 방향 차이가 생긴다. 이런 특징을 이용해 자연어의 의미를 파악할 수 있다.)

![image](https://user-images.githubusercontent.com/66985977/222906611-d83262b2-0d09-4ef9-aba5-8e44a81b4739.png)

Word2vec 모델은 텐서플로나 케라스 같은 신경망 라이브러리를 이용해 직접 구현할 수도 있지만 Word2Vec을 사용 할 수 있는 오픈소스 라이브러리가 이미 존재하며,
토픽 모델링과 자연어 처리를 위한 라이브러리인 Gensim 패키지가 있고 사용법이 간단하고 임베딩 품질이 나쁘지 않아 많이 사용하고 있다.

## Word2Vec create model 

한국어 Word2Vec을 만들기 위해서는 한국어 말뭉치(corpus)를 수집해야 한다. 웹에서는 대표적으로 한국어 위키나 네이버 영화 리뷰 데이터가 있다.

네이버 영화 리뷰 데이터 다운로드 : [ratings.txt](https://github.com/StreetStudy/ChatBot/files/10888792/ratings.txt)

말뭉치 데이터는 양이 많기 때문에 모델을 학습하는 데 시간이 다소 걸린다. Word2Vec을 사용할 때마다 오랜 시간이 걸리는 모델 학습을 매번 할 수 없으므로 각 단어의 임베딩 벡터가 설정되어 있는 모델을 파일로 정한다.

```

from gensim.models import Word2Vec
from konlpy.tag import Komoran
import time


# 네이버 영화 리뷰 데이터 읽어옴
def read_review_data(filename):
    # UnicodeDecodeError 'cp949' code 에러가 발생할 수 있으므로, encoding을 함.
    with open(filename, 'r', encoding='UTF8') as f:
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
review_data = read_review_data('ratings.txt') # 말뭉치 파일 경로
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

# 모델 저장
print('4) 학습된 모델 저장 시작')
model.save('nvmc.model') # 모델 파일을 저장 할 경로
print('4) 학습된 모델 저장 완료: ', time.time() - start)

# 학습된 말뭉치 개수, 말뭉치(코퍼스) 내 전체 단어 개수
print("corpus_count : ", model.corpus_count)
print("corpus_total_words : ", model.corpus_total_words)

```

### 생성된 모델이 물리 파일로 저장된 상태.<br/>

![image](https://user-images.githubusercontent.com/66985977/222942562-1004d327-2ea7-4fac-89b4-c860e2b1360e.png)

<br/>

![image](https://user-images.githubusercontent.com/66985977/222942448-e386a420-8005-448f-8be6-1a4c11bfe90f.png)

## Word2Vec load model 

```

from gensim.models import Word2Vec

# 모델 로딩
model = Word2Vec.load('nvmc.model')
print("corpus_total_words : ", model.corpus_total_words)

```

### '사랑'이란 단어로 생성한 단어 임베딩 벡터

```

print('사랑 : ', model.wv['사랑'])

```

![image](https://user-images.githubusercontent.com/66985977/222943216-0ce99e59-ccff-4d6e-8df7-3ba08fa7f68b.png) <br/>

word2vec 모델 생성 당시 차원의 크기를 200으로 설정했기에 사진에서는 200개의 차원을 볼 수 있다.

'사랑'의 단어 임베딩 벡터를 살펴보면 모든 차원에 골고루 데이터가 분포되어 있는 것을 볼 수 있다.
