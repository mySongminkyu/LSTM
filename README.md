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

      <img width="241" alt="image" src="https://github.com/mySongminkyu/LSTM/assets/132251519/aff3b1db-6ef2-484d-9b72-c096389500e1">
      
      
