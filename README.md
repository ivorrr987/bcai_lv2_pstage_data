# bcai_lv2_pstage_data
NAVER BOOSTCAMP AI TECH3
LV2 PSTAGE DATA CONSTRUCTION

## GOAL
전반적인 데이터 제작 과정을 직접 경험해봄.

## STEPS
### 1. raw data 인식
<img width="715" alt="스크린샷 2022-04-19 오후 5 26 51" src="https://user-images.githubusercontent.com/79218038/163959598-e41490b3-92d4-4559-a04b-0c4da2668096.png">

우리 팀에 할당된 주제는 '요리'로 위키피디아에서 72개국에 대해 수집된 문장들이 txt파일로 주어졌다.

<img width="435" alt="스크린샷 2022-04-19 오후 5 30 30" src="https://user-images.githubusercontent.com/79218038/163960080-c5f27fbc-65cb-412a-94b7-36223d58ea78.png">

텍스트는 위와 같은 형태로 주어졌으며 '줄바꿈기호(\n)'로 구분된 문장들이고 요리, 식재료, 역사, 문화, 지리적 특성 등 다양한 정보를 포함하고 있었다.

### 2. raw data 분석 및 1차 label 정의
'정의된 라벨 개수가 10개를 초과하지 않을 것'이라는 조건이 있었다. 때문에 1차 label 정의 규칙은 다음과 같았다.
- "각 문장에서 포착한 entity쌍들을 최대한 의미있게 분류하되, 한 라벨이 다른 라벨을 대체할 수 있거나 포함 관계에 있을 시 두 라벨을 합칠 방법을 생각하자"

여기서 '의미있는' 분류를 수행하기 위해 각 국가 요리에 대한 텍스트 파일 중 비교적 문장이 많이 포함된 텍스트 파일을 추출하고 그 중 최대한 지리적, 문화적으로 멀리 있는 국가 10개를 뽑은 뒤 각 팀원이 2개씩의 텍스트 파일을 맡아 relation을 뽑아보았다.

그 후 의견을 공유하며 의미있는 분류를 수행할 수 있는 16개의 label을 만들어보았다.

<img width="483" alt="스크린샷 2022-04-19 오후 5 38 01" src="https://user-images.githubusercontent.com/79218038/163962714-70e8f55f-6854-48da-bc7c-047d00821283.png">

이후 각자 임시 label로 labeling을 수행해봤는데 나의 결과는 다음과 같았다.('모로코', '소말리아')

<img width="871" alt="mor_som_1st_labeling" src="https://user-images.githubusercontent.com/79218038/163963037-3c311ec7-d41e-41f4-8698-dbdf2ca7a766.png">

(임시 labeling을 하던 시점에 표에 오류가 있었다. 때문에 위의 결과 중 label16의 결과가 label15의 결과라고 생각하면 된다. label16은 존재하지 않는 라벨이라고 생각하면 된다.)

이후 각자의 분석을 기반으로 label 정의를 수정했다. 의미적인 통합, 매우 적은 빈도수를 갖는 label 정의 변경 등이 이루어졌는데 그 결과는 다음과 같다.
<img width="836" alt="스크린샷 2022-04-19 오후 5 48 20" src="https://user-images.githubusercontent.com/79218038/163963487-7f98e8e6-8d0f-44e3-8fa7-29ead8591080.png">

### 3. 2차 label 정의
수정된 label로 다시 labeling을 진행했다. 
모로코, 소말리아에 대한 labeling 결과는 다음과 같다.
<img width="658" alt="스크린샷 2022-04-19 오후 5 52 19" src="https://user-images.githubusercontent.com/79218038/163965988-c097e028-7cef-491f-a730-c1c4c8dc809d.png">

여기서 두가지 문제가 있었다.
1. 'no_relation'의 비중이 너무 높다
2. 지금의 6개 라벨로는 추출가능한 entity 쌍들을 충분히 커버하지 못한다.

때문에 'no_relation'으로 분류된 entity 쌍 중에서 자주 등장하거나 기존의 라벨들과의 의미적 차이가 있는 라벨을 새로 만들어야 했다.

### 3. 3차 label 정의
<img width="717" alt="스크린샷 2022-04-21 오후 8 06 39" src="https://user-images.githubusercontent.com/79218038/164445367-12ecb132-9a5c-40e7-9dfa-346f1b966e68.png">

그렇게해서 위와 같은 라벨을 정의하게 되었다. '요리'가 주제이다보니 음식에 관련한 라벨이 많을 수밖에 없었고 요리는 지리적인 영향을 많이 받기 때문에 지역에 관한 라벨을 넣었다. 지역뿐만 아니라 단체에 대한 entity 쌍도 많이 발견되었는데 label 개수의 제한이 있기 때문에 지역과 단체를 하나로 묶었다.
마찬가지 이유로 포함관계의 경우 반대 방향은 고려하지 않았다.

### 4. label 정의 완료
<img width="1168" alt="스크린샷 2022-04-21 오후 8 10 42" src="https://user-images.githubusercontent.com/79218038/164445930-fd98e17e-9a23-4a5d-85fc-debd8a3e9204.png">

'3차 label 정의'를 기초로 하여 최종 label이 완성되었다. 요리에 영향을 준 요소 중 특정 시대에 관한 entity가 있었으므로 시간적인 entity를 추가했고 출현 빈도가 너무 낮거나 의미적으로 다른 label에 합쳐질 수 있는 label은 통합했다. 

사실 labeling 하기 애매한 부분도 많았는데 label 개수의 제한 때문에 이 이상 세분화할 수는 없었다. 대신 가이드라인을 최대한 명확하고 세세하게 써서 labeling 작업이 원활히 이뤄지도록 했다.

### 4. label 및 가이드라인 평가
tagtog을 이용해서 각각 labeling을 진행했으며(각 100개의 문장에 대해 수행. 한 문장에서 여러 entity 쌍과 relation이 나올 수 있었음.) 문장 내에서 2개 이상의 관계가 나올 수 있으므로 최종적으로 900여개의 문장 set을 만들고 교차 labeling을 진행했다.

작업자 간 일치도는 Fleiss Kappa로 평가하였다. 0.7을 넘기는게 목표였는데 0.75가 나왔고 label 정의와 가이드라인 제작이 잘 이루어졌다고 생각했다.

마지막으로 900여개의 문장과 정의한 라벨을 이용해 모델을 학습시키고 f1 score를 측정했다.
8:2의 비율로 train:test dataset을 나누고(stratified), train dataset은 다시 8:2 비율로 train:valid dataset으로 나눴다.
'roberta-large' 모델로 학습을 진행했고 학습이 완료된 후 inference를 통해 모델에 대한 예측라벨이 담긴 csv파일을 만들었다.
해당 파일과 test dataset의 label을 정답으로 이용해 f1 score를 측정한 결과 0.828 정도의 값이 나왔다.
다만 한 문장에서 여러 entity 쌍을 포착하는 경우가 있어 중복되는 문장이 많았고(물론 sentence만 중복이고 entity쌍과 relation은 달랐다) 이것이 random하게 sampling된 후 test dataset으로 나뉘어지다보니 학습에 사용된 문장과 같은 문장이 test set에 포함되는 경우가 있었다. 이 때문에 f1 score가 높게 나온게 아닌가 하는 추측을 해본다. 

그럼에도 괜찮은 성능이 나왔다고 생각하며 특히 잘못 예측한 label들을 분석해봤을 때 특정 label에서 실수가 반복되거나 하지는 않는 것으로 보였다. 
<img width="414" alt="스크린샷 2022-04-21 오후 9 22 49" src="https://user-images.githubusercontent.com/79218038/164457867-c71bdd35-1af9-47bf-b811-45f35c2e54fa.png">

위의 표를 보면 예측에 실패하는 특정한 패턴이 보이지 않는다.

## REVIEW
데이터 제작의 전 과정을 경험해보는 좋은 시간이었다. model만큼이나(아니 어쩌면 model보다 더) data도 중요하다고 생각한다. 
만들어진 data를 쓰는 경우도 있겠지만 data를 직접 만들어야 하는 경우도 많이 있을텐데 그럴 때 어떤 점을 고려해야 하는지, 어떻게 가이드라인을 짜야 작업자들이 의도한대로 잘 데이터를 만들지 등을 고민해볼 수 있었다.
팀원과의 의견 합의를 이끌어내는 과정, 그 과정에서 상대를 설득하는 능력 등 배운 점도 많은 시간이었다.

## REFERENCE
제공받은 데이터는 부스트캠프 측에서 각 팀의 주제로 수집한 데이터이며 한국어 위키피디아가 그 출처이다.
본 repo에 포함된 데이터는 텍스트 파일로 제공받은 데이터를 가공한 것이다.