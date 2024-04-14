# LSTM

-LONG SHORT-TERM MEMORY(LSTM)

  - Vanilla RNN의 한계점

    - Time Interval이 큰 data에 대한 지식을 잘 저장하지 못하였는데 이러한 한계점은 back propagation 과정이 정보를 충분히 전달하지 못하기 때문이고, 많은 layer를 지나며 gradient vanishing 문제가 발생하기 때문이다.
      
      본 논문에서는 이러한 문제점을 해결할 수 있는 method인 LSTM을 제안하는데 이는 특정 정보가 gradient에 안 좋은 영향을 끼치지 않는 이상, 약 1000번의 time step 이상의 insterval에도 정보 손실이 없다고 한다.

  - LSTM cell structure
    - LSTM은 long-term dependencies problem을 해결하기 위해 명시적으로 설계되었다. 기본적인 RNN model은 neural network module을 반복하는 chain과 같은 형태를 띄고 있는데, vanilla RNN에서는 hidden layer로
      tanh을 이용하여 hidden state를 추출하는 단순한 구조를 가지고 있다.

      LSTM도 큰 구조 자체는 다르지 않지만, 각 반복 module은 다른 구조를 가지고 있다. 단순한 neural network layer 한 층 대신에, 4개의 layer가 서로 정보를 주고 받도록 설계되어 있다.

      <img width="652" alt="image" src="https://github.com/mySongminkyu/LSTM/assets/132251519/0d7f4876-4f06-4175-b0ec-16a38f87a11e">

      기존에 tanh module만 있던 RNN과는 확실히 다른 모습인데 그림에 있는 노드와 아크들은 각각의 기능이 있다.

      <img width="611" alt="image" src="https://github.com/mySongminkyu/LSTM/assets/132251519/291f7946-eb7d-4e93-b533-d44ece58e6b8">

      위 그림에서 각 line들은 한 node의 output을 다른 node의 input으로 vector 전체를 보내는 흐름을 나타낸다. Pointwise operation은 vector의 합이나 곱을 나타낸다.

  - LSTM
    - LSTM의 핵심은 cell state이다.
   
      <img width="307" alt="image" src="https://github.com/mySongminkyu/LSTM/assets/132251519/f06b48b2-efb3-45aa-9c88-aaaa37f23ebe">

      이 부분인데 cell state은 컨베이어 벨트와 같은 기능을 해서, 작은 linear interaction만을 적용하며 전체 chain을 계속 작동하게 한다. 정보 손실 없이 그대로 흐르게 하는 것을 쉽게 할 수 있다.

      또한 이 cell state에 gate를 이용하여 뭔가를 더하거나 없애는 것도 가능하다.
      gate는 정보가 전달될 수 있는 추가적인 method로, sigmoid layer와 pointwise 곱으로 이루어져 있다.

      - Forget Gate

      <img width="597" alt="image" src="https://github.com/mySongminkyu/LSTM/assets/132251519/79937c75-0931-49d2-9aeb-229ca84de7ef">
      
      전의 cell에서 넘어온 정보들을 sigmoid function을 이용하여 불필요하다 여겨지는 data들에 낮은 가중치를 부여한다.(Sigmoid function을 통해 얻어진 0~1 사이의 weight를 곱해주며 학습을 해서 상대적으로 중요한 정보에는
      높은 가중치를, gradient update에 안 좋은 영향을 주는 정보에는 낮은 가중치를 부여한다.)

      즉 network를 학습하여 얻은 $W_f$와 $B_f$를 이용하여 0에 가까운 값이 나오면 반영을 적게하고 아예 0이 되면 삭제, 반대로 1에 가까운 값일 수록 반영을 많이하며 1이 나오면 정보손실 없이 그대로 전달하는 역할을 함으로써
      cell을 지나면서도 계속 유의미한 data들이 cell state flow에 보존되도록 한다.

      - Input Gate
      
      <img width="625" alt="image" src="https://github.com/mySongminkyu/LSTM/assets/132251519/09fb2f90-1386-4310-8fa3-eaf67eacb0d6">

      입력값 $x_t$와 이전 hidden state를 사용하여 현재 cell의 local cell state를 얻어내고 이를 global cell state에 얼마나 반영할지 결정하는 gate.

      forget gate와 비슷하게 sigmoid 함수로 현재 시점에서 얻은 정보의 값을 계산하여 weight를 설정하는 기능을 한다.

      - Cell state update
      
      <img width="541" alt="image" src="https://github.com/mySongminkyu/LSTM/assets/132251519/6aa32c98-08de-4ba9-9de2-2b896d9d9a83">

      이후, 전시점 hidden state가 forget gate를 지난 cell state와 현재 cell에서 Input gate를 지나 얻은 cell state를 더해서 최종 global cell state를 update 한다.

      - Output gate

      <img width="586" alt="image" src="https://github.com/mySongminkyu/LSTM/assets/132251519/841ad6bf-c91b-4780-83f0-48be29561bf1">

      최종 global cell state 값에서 어느정도를 hidden state로 전달할 것인지 정하는 마지막 gate이다.
      여기서 최종적으로 얻은 $h_t$를 기반으로 다음 cell에서는 $C$ _t+1 을 구하게 된다.


        
      
