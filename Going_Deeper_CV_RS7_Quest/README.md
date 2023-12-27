# AIFFEL Campus Online Code Peer Review Templete
- 코더 : 이승제
- 리뷰어 : 강다은


# PRT(Peer Review Template)
- [x]  **1. 주어진 문제를 해결하는 완성된 코드가 제출되었나요?**
    - 문제에서 요구하는 최종 결과물이 첨부되었는지 확인
    - 문제를 해결하는 완성된 코드란 프로젝트 루브릭 3개 중 2개, 
    퀘스트 문제 요구조건 등을 지칭
        - 해당 조건을 만족하는 코드를 캡쳐해 근거로 첨부
     
      <br/>

      >요구사항대로 layer block 함수 및 해당 함수를 이용한 model 함수를 작성하였으며, 학습 모델 속성에 따른 ablation study 를 성공적으로 수행하였다.

      ```
      def residual_block(input_layer, filters, stride = 1, resnet_layers = 34):
          ...
          return x

      def build_resnet(input_shape, num_classes, resnet_layers=34, is_plain=False):
          ...
          return Model(inputs=input_layer,outputs=outputs)
      ```
      <img width="900" alt="image" src="https://github.com/DiANA-KANG/AIFFEL7_Quest_LSJ/assets/149550222/fb5c130e-c964-46ff-bdc2-598da83eb06d">

      <br/>
      <br/>
    
- [x]  **2. 전체 코드에서 가장 핵심적이거나 가장 복잡하고 이해하기 어려운 부분에 작성된 
주석 또는 doc string을 보고 해당 코드가 잘 이해되었나요?**
    - 해당 코드 블럭에 doc string/annotation이 달려 있는지 확인
    - 해당 코드가 무슨 기능을 하는지, 왜 그렇게 짜여진건지, 작동 메커니즘이 뭔지 기술.
    - 주석을 보고 코드 이해가 잘 되었는지 확인
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.
      
      <br/>

      >주석을 통해 함수 내 코드의 작동 흐름을 설명하고, 해당 함수를 어떻게 사용하고 변형 가능한지 가이드를 제시하여 후속 연구 또한 원활하게 진행할 수 있도록 하였다.

      ```
      def build_resnet(input_shape, num_classes, resnet_layers=34, is_plain=False):
            # residual_block 반복 횟수 이론상 추가 가능하게 설계
            residual_layers_18  = [2, 2, 2, 2]
            residual_layers_34  = [3, 4, 6, 3]
            residual_layers_50  = [3, 4, 6, 3]
        
            # input에서 고를 수 있게 dic처리
            residual_layers = {18: residual_layers_18, 34:residual_layers_34, 50:residual_layers_50}
        
            # 이값은 고정
            channel_list = [64, 128, 256, 512]
        
            input_layer = Input(shape=input_shape)
        
            x = input_layer
        
            # conv1 에서는 7*7,64 채널 stride2 가 고정이다.
            x = Conv2D(64, 7, strides=2, padding='same')(x)
            x = BatchNormalization()(x)
            x = Activation('relu')(x)
        
            # conv2 에서는 3*3 maxpool, stride2 로 시작하고 반복 횟수에 따라서 다르다. 18=2,34-3,50~152 = 3
            x = MaxPool2D(pool_size=(3, 3), strides=2, padding='same')(x) # 고정
        
            for i, v in enumerate(residual_layers[resnet_layers]): # i = 0은 conv2
              for j in range(v): # residual_block 반복 횟수
                # resnet_layers 이 친구랑 index값 사용해서 처리
                if i != 0 and j == 0: # conv2가 아니면서 첫 번째 residual_block일 경우
                  x = residual_block(x, channel_list[i], 2, resnet_layers) # stride 2고정
                else:
                  x = residual_block(x, channel_list[i], 1, resnet_layers) # 나머지는 1
        
            x = GlobalAveragePooling2D()(x)
            outputs = Dense(num_classes, activation="sigmoid")(x)
        
            return Model(
              inputs=input_layer,
              outputs=outputs
            )
      ```

      <br/>



- [x]  **3. 에러가 난 부분을 디버깅하여 문제를 “해결한 기록을 남겼거나” 
”새로운 시도 또는 추가 실험을 수행”해봤나요?**
    - 문제 원인 및 해결 과정을 잘 기록하였는지 확인
    - 문제에서 요구하는 조건에 더해 추가적으로 수행한 나만의 시도, 
    실험이 기록되어 있는지 확인
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.
          
      <br/>

      >요구사항인 ResNet-34, ResNet-50 뿐만 아니라, ResNet-18 또한 생성할 수 있도록 프로그래밍 하였으며, 후속 연구를 통해 또 다른 ResNet 모델 생성으로도 확장할 수 있도록 코드 구조를 유연하게 설계하였다.

      ```
      def build_resnet(input_shape, num_classes, resnet_layers=34, is_plain=False):
          # residual_block 반복 횟수 이론상 추가 가능하게 설계
          residual_layers_18  = [2, 2, 2, 2]
          residual_layers_34  = [3, 4, 6, 3]
          residual_layers_50  = [3, 4, 6, 3]
        
          # input에서 고를 수 있게 dic처리
          residual_layers = {18: residual_layers_18, 34:residual_layers_34, 50:residual_layers_50}
    
          ...
    
          return Model(inputs=input_layer,outputs=outputs)
      ```

      <br/>


        
- [x]  **4. 회고를 잘 작성했나요?**
    - 주어진 문제를 해결하는 완성된 코드 내지 프로젝트 결과물에 대해
    배운점과 아쉬운점, 느낀점 등이 기록되어 있는지 확인
    - 전체 코드 실행 플로우를 그래프로 그려서 이해를 돕고 있는지 확인
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.
      
      <br/>

      >회고를 통해 프로그래밍을 수행하면서 겪었던 실수와 반성, 후속 연구에 대한 아이디어 등에 관하여 언급하였다.

      ```
      우선 주어진 그래프를 보고 resnet모델을 만들었는데 변수로 18layer에서 152layer까지 생성이 가능하게 만들어 보았다. 시간이 된다면 18~152까지 다 돌려보고 비교를 해봐야겠다.  
      너무 그래프만 보고 만들었더니 처음에 short cut을 더해주는 과정을 빼먹어서 accuracy가 낮게 계속 나오고 있었는데 뭔가 이상해서 돌려보던 중 알게 되어 추가했더니 나름 우상향 하는 그래프가 나와서 뿌듯했다.
      ```

      <br/>


        
- [x]  **5. 코드가 간결하고 효율적인가요?**
    - 파이썬 스타일 가이드 (PEP8) 를 준수하였는지 확인
    - 하드코딩을 하지않고 함수화, 모듈화가 가능한 부분은 함수를 만들거나 클래스로 짰는지
    - 코드 중복을 최소화하고 범용적으로 사용할 수 있도록 함수화했는지
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.
      
      <br/>

      > ResNet의 기본 구성 요소인 residual block에 관한 함수를 정의하고, ResNet 종류에 따라 residual block의 배치를 결정하는 함수를 정의함으로써, 최소한의 코드만으로 유사하지만 다양한 ResNet 모델을 생성하도록 프로그래밍을 수행하였다.

      ```
      def residual_block(input_layer, filters, stride = 1, resnet_layers = 34):
          ...
          return x

      def build_resnet(input_shape, num_classes, resnet_layers=34, is_plain=False):
          # residual_block 반복 횟수 이론상 추가 가능하게 설계
          residual_layers_18  = [2, 2, 2, 2]
          residual_layers_34  = [3, 4, 6, 3]
          residual_layers_50  = [3, 4, 6, 3]
        
          # input에서 고를 수 있게 dic처리
          residual_layers = {18: residual_layers_18, 34:residual_layers_34, 50:residual_layers_50}
    
          ...
    
          return Model(inputs=input_layer,outputs=outputs)
        ```

<br/>
<br/>
<br/>

# 참고 링크 및 코드 개선
```
# 코드 리뷰 시 참고한 링크가 있다면 링크와 간략한 설명을 첨부합니다.
# 코드 리뷰를 통해 개선한 코드가 있다면 코드와 간략한 설명을 첨부합니다.
```
**항상 멋진 코드를 보여주시는 승제님🐻💛**  
다양한 ResNet을 한 번에 만들어낼 수 있는 코드가 매우 인상적이였습니다.  
또 한 번 퀘스트를 통해 좋은 코드를 리뷰할 기회를 주셔서 감사합니다 😄😄😄
