# Anaconda

download : https://www.anaconda.com/products/distribution/start-coding-immediately

자주 사용하는 커맨드 : https://wooni-research.tistory.com/entry/%EC%95%84%EB%82%98%EC%BD%98%EB%8B%A4-CMD-%EC%9E%90%EC%A3%BC%EC%93%B0%EB%8A%94-%EB%AA%85%EB%A0%B9%EC%96%B4-%EB%AA%A8%EC%9D%8C

한 컴퓨터에서 여러 프로젝트를 진행하다 보면 여러 라이브러리, 패키지를 사용하게 되는데 이때 충돌이 일어날 수 있다. 아나콘다를 사용하면 프로젝트 별로 가상환경을 구성하여 독립적인 환경에서 개발할 수 있다.

<details>
<summary>시작하기전 아나콘다 설치 <접기/펼치기></summary>
<div markdown="1">

설치<br/>
![image](https://user-images.githubusercontent.com/66985977/222026227-98920b94-e7b1-4a3a-b0f0-ed63c3543807.png)<br/>
<br/>
라이센스<br/>
![image](https://user-images.githubusercontent.com/66985977/222026319-ffc4fdbf-3a0c-4f5f-95fd-2974641e53ee.png)<br/>
<br/>
설치 타입 [다중 사용자 환경을 고려한 설정값인듯 하며, Just Me로 설정]<br/>
![image](https://user-images.githubusercontent.com/66985977/222026715-5aaeaa4d-66da-47d3-90de-7fe7f442db36.png)<br/>
<br/>
설치 폴더<br/>
![image](https://user-images.githubusercontent.com/66985977/222026852-d46469aa-079b-4d65-a928-0c459844b6ea.png)<br/>
<br/>
고급 옵션 [PATH를 자동으로 등록할지 여부를 체크함.]<br/>
![image](https://user-images.githubusercontent.com/66985977/222027446-63c0d037-c92e-4ea1-833f-0fe91dad357a.png)<br/>
 <br/>
설치 중<br/>
![image](https://user-images.githubusercontent.com/66985977/222027820-034ae56d-1ab1-499a-95b3-5b043cbde415.png)<br/>
<br/>
</div>
</details>

## 실행

![image](https://user-images.githubusercontent.com/66985977/222030085-81a96310-8aac-419f-9049-3fc6e70cbcec.png)<br/>

## 가상환경 생성 [CUI(CLI)]

### 실행<br/>
![image](https://user-images.githubusercontent.com/66985977/222041257-be6c5b87-e03c-4cd7-8541-04feee9824e5.png)<br/>

### 환경생성(파이썬 버전은 임의로 3.7로 설정 하였음)<br/>
 
 ```
 // conda create -n <가상 환경명> python=<파이썬 버전>
 conda create -n chatbot python=3.7
 ```
 
![image](https://user-images.githubusercontent.com/66985977/222041626-d0dc601c-f761-4aa6-8119-553d1c4cc98b.png)<br/>

### 진행 의사확인<br/>
 
```
y
```
 
![image](https://user-images.githubusercontent.com/66985977/222041892-3e801129-b42b-43d9-a115-08ddda4a2e78.png)<br/>

### 생성완료<br/>
![image](https://user-images.githubusercontent.com/66985977/222042056-6f7b1c2b-4787-4dc8-acbf-f09e395e45bd.png)<br/>

### 환경활성화<br/>
 
 ```
 conda activate <가상 환경명>  
 ```
 
![image](https://user-images.githubusercontent.com/66985977/222042386-fdbf1d82-c6f9-4b82-bea5-ff97c829d184.png)<br/>

### Tensorflow 2.1(딥러닝 모델) 설치<br/>
 
 ```
 pip install tensorflow==2.1  
 ```
 
![image](https://user-images.githubusercontent.com/66985977/222042666-d524042b-af80-4c2c-9039-e9d38e3afc95.png)<br/>

### Java SE Runtime Environment 8(Komoran 형태소 분석기 사용을 위해) 설치<br/>
[ download : https://www.oracle.com/java/technologies/downloads/#java8 ]<br/>
![image](https://user-images.githubusercontent.com/66985977/222044618-1b16356d-d883-4212-a7cd-f2bd16ea0946.png)<br/>
### KoNLPy 패키저와 코모란 형태소 분석기 설치<br/>
 
```
pip install konlpy
pip install PyKomoran
```
 

![image](https://user-images.githubusercontent.com/66985977/222045179-2f535def-d83d-4827-8940-dce271868dca.png)<br/>
### Gensim 패키지 설치 (Word2Vee 임베딩을 사용하기 위해)<br/>
 
```
pip install gensim 
```
 

![image](https://user-images.githubusercontent.com/66985977/222045358-69920245-5c02-4a1e-8ac4-8d1a0864dfef.png)<br/>
### sklearn(사이킷런) 패키지 설치 (머신러닝에 필요한 도구를 제공하는 라이브러리)<br/>
 
```
pip install sklearn 
```
 

![image](https://user-images.githubusercontent.com/66985977/222045469-6931d5df-2855-4cc2-aa48-ab9011cade61.png)<br/>
### Seqeval 패키지 설치 (시퀀스 레이블 점수 평가에 사용하는 파이썬 프레임워크)<br/>
 
```
pip install seqeval 
```
 
![image](https://user-images.githubusercontent.com/66985977/222045615-b3a4587e-439d-45e1-ac86-8d48a1f1827b.png)<br/>
### PyMySQl 패키지 설치 (파이썬에서 MySQL을 사용하기 위해)<br/>
 
```
pip install PyMySQL 
```
 
![image](https://user-images.githubusercontent.com/66985977/222045779-60eb6b2c-52c0-4ed0-9d3e-74e271295377.png)<br/>
### OpenPyXl 패키지 설치 (파이썬에서 엑셀 파일을 제어하기 위해 설치)<br/>
 
```
pip install openpyxl
```
 
![image](https://user-images.githubusercontent.com/66985977/222045927-881c488c-6d86-4f80-8fd9-a870e5dd5540.png)<br/>
### Pandas, xlrd 패키지 설치 (데이터 분석 및 처리를 위한 라이브러리)<br/>
 
```
pip install pandas xlrd
```
 
![image](https://user-images.githubusercontent.com/66985977/222046390-0a456a7b-066f-465e-aa38-6a4f5116c5f3.png)<br/>
### Matplotlib 패키지 설치 (데이터를 시각화하는데 필요한 도구)<br/>
 
```
pip install matplotlib
```
 
![image](https://user-images.githubusercontent.com/66985977/222046584-394d19ff-825a-4b16-b678-bc04d9b71171.png)
### Flask 웹 프레임워크와 requests 패키지 설치
 
```
pip install flask
pip install requests
```
 
![image](https://user-images.githubusercontent.com/66985977/222046726-ac4b5cd2-9c8a-4378-a281-f84a4ba7dd29.png)
