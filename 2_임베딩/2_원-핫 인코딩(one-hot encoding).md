# 원-핫 인코딩(one-hot encoding)

### 희소 표현의 대표적 예

원-핫 인코딩은 단어를 숫자 벡터로 변환하는 가장 기본적인 방법이다. 요소들 중 단 하나의 값만 1이고 나머지 요솟값은 0인 인코딩을 의미한다.
원-핫 인코딩으로 나온 결과를 원-핫 벡터라 하며, 전체 요소 중 단 하나의 값만 1이기 때문에 희소 벡터라고 한다.

원-핫 인코딩 과정<br>
![image](https://user-images.githubusercontent.com/66985977/222443292-b314bf3e-4147-4584-9c4d-edcfdb8cfb03.png)

원-핫 인코딩을 하기 위해서는 단어 집합이라고 하는 사전을 먼저 만들어야 한다. 여기서 사전은 말뭉치에서 나오는 서로 다른 모든 단어의 집합을 의미하고, 말뭉치에 존재하는 모든 단어의 수가 원-핫 벡터의 차원을 결정한다. 예를 들어 100개의 단어가 존재한다면 원-핫 벡터의 크기는 100차원이 된다. 사전 내 단어 순서대로 고유한 인덱스 번호를 부여하고 단어의 인덱스 번호가 원-핫 인코딩에서 1의 값을 가지는 요소의 위치가 된다.

```

from konlpy.tag import Komoran
import numpy as np

komoran = Komoran()
text = "오늘 날씨는 구름이 많아요."

# 명사만 추출
nouns = komoran.nouns(text)
print(nouns)

# 단어 사전 구축 및 단어별 인덱스 부여
dics = {} # 딕셔너리 선언
for word in nouns :
    if word not in dics.keys():
        #      key = value
        dics[word] = len(dics)
print(dics)

# 원-핫 인코딩
nb_classes = len(dics)
targets = list(dics.values())

# 원-핫 벡터를 만들기 위해서는 넘파이의 eye() 메서드를 이용한다. 
# eye() 메서드는 단위행렬을 만들어주고 인자 크기대로 단위행렬을 반환하며, eye()함수 뒤에 붙은 [targets]를 이용해 생성된 단위행렬의 순서를 단어 사전의 순서에 맞게 정렬해준다.
one_hot_targets = np.eye(nb_classes)[targets]
print(one_hot_targets)

```

결과<br>
![image](https://user-images.githubusercontent.com/66985977/222447554-9097389c-fcbd-4021-b641-aa442ff5aee6.png)

원-핫 인코딩의 경우 간단한 구현 방법에 비해 좋은 성능을 가지기 때문에 많은 사람이 사용하고 있지만 원-핫 벡터의 경우 단순히 단어의 순서에 의한 인덱스값을 기반으로 인코딩된 값이기 때문에 단어의 의미나 유사한 단어와의 관계를 담고 있지 않는다. 또한 단어 사전의 크기가 커짐에 따라 원-핫 벡터의 차원도 커지는데 이때 메모리 낭비와 계산의 복잡도가 커진다.
원-핫 벡터는 대부분의 요소가 0의 값을 가지고 있으므로 비효율적이다.
