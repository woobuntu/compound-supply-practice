[https://medium.com/compound-finance/supplying-assets-to-the-compound-protocol-ec2cf5df5aa](https://medium.com/compound-finance/supplying-assets-to-the-compound-protocol-ec2cf5df5aa)

Compound 프로토콜은 이더리움 블록체인 상에서 운영되는 금리 시장(rate market)
Compound 프로토콜에 가상자산을 예치한 순간부터, 변동 금리(variable interest rate)를 얻기 시작한다.
이자는 이더리움 블록마다 축적되고, 사용자들은 원리금(원금 + 이자)을 언제든지 인출할 수 있다.
(이더리움 블록은 약 13초에 하나씩 생성된다)

사용자들이 유동성 풀에 예치한 자산은 또 다른 사용자에게 대출되고, 예치자들은 대출자들이 지불한 이자를 나눠가진다?

컴파운드에 자산을 예치하면 사용자는 cToken을 수령한다.
cToken은 예치한 자산에 대한 일종의 영수증으로, 제시하면 언제든지 예치한 자산을 상환받을 수 있다.
예치한 자산에 대한 이자가 축적됨에 따라, cToken의 가치는 상승한다.
(원금에 해당하는 가상자산이 10COMP고 이자로 쌓인 가상자산이 1COMP라고 할 때, 원래 1cToken으로 10COMP를 수령할 수 있었다가 11COMP를 수령할 수 있게 된 것을 표현한 것 같다)

non-technical user는 Argent, Coinbase Wallet, 또는 app.compound.finance 등의 interface를 통해서 컴파운드 프로토콜과 상호작용할 수 있다.
개발자는 컴파운드의 스마트 컨트랙트와 상호작용할 자체 애플리케이션을 제작할 수 있다.

컴파운드 프로토콜이 이더리움 블록체인 상에서 운영되기 때문에 운용되는 자산은 ETH와 ERC-20토큰으로 한정된다.
따라서 ETH에 대한 supply method와 ERC-20에 대한 supply method를 구분할 필요가 있다.

컴파운드 프로토콜에 Ether를 공급(예치)할 때, 애플리케이션은 cEther 컨트랙트의 payable mint function으로 ETH를 직접 전송할 수 있다.
이 mint함수의 실행 결과로 해당 함수를 호출한 지갑(EOA)이나 컨트랙트(CA)로 cEther가 발행(mint)된다.
주의할 것은 이 mint함수를 호출하는 스마트 컨트랙트는 payable 함수를 필요로 한다.
이는 cToken(cEther아닌가)을 상환할 때 ETH를 돌려받기 위함이다.

cERC20토큰을 minting할 때는, 해당 mint함수를 호출하는 지갑(EOA)이나 컨트랙트(CA)는 우선 기반이 되는 토큰의 컨트랙트의 approve함수를 호출해야 한다.
(ex: cUNI를 mint할 때는, UNI컨트랙트의 approve함수를 먼저 호출)
모든 ERC20토큰의 컨트랙트는 approve함수를 가지고 있다.

apporve함수는 상응하는 cToken 컨트랙트가 sender address로부터 지정된 금액을 가져갈 수 있음을 허용한다.
이어서 mint함수가 호출되면 cToken 컨트랙트는 앞선 approve함수의 호출에 기반하여 sender address에서 지정된 양의 (기초)토큰이 존재하는지 확인한다.

[https://medium.com/compound-finance/borrowing-assets-from-compound-quick-start-guide-f5e69af4b8f4](https://medium.com/compound-finance/borrowing-assets-from-compound-quick-start-guide-f5e69af4b8f4)
