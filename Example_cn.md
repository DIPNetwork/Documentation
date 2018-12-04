# 测试目的 #
	本次测试分为两个部分：
	第一测试部署模板并且可以通过模板部署合约，合约调用正常;
	第二测试模板开发者与矿工激励分成.
# 准备材料 #
	预先准备下列数据，该合约为不带构造函数的合约。
## 合约代码： ##

	pragma solidity ^0.4.16;

	contract order{
   		 function getName() constant public returns(string){
       		 return 'sss';
   	 	}
	}
## 合约bytecode: ##

	{ "linkReferences": {}, "object": 
	"608060405234801561001057600080fd5b5061012a806100206000396000f300608060405260043610603e5763
	ffffffff7c010000000000000000000000000000000000000000000000000000000060003504166317d7de7c811
	46043575b600080fd5b348015604e57600080fd5b50605560c7565b604080516020808252835181830152835191
	9283929083019185019080838360005b83811015608d5781810151838201526020016077565b505050509050908
	10190601f16801560b95780820380516001836020036101000a031916815260200191505b509250505060405180
	910390f35b60408051808201909152600381527f737373000000000000000000000000000000000000000000000
	00000000000006020820152905600a165627a7a72305820f712e15abc7477ae7b54729eb495586b7370dcc2356a
	777651c2fdfd8bc6f72a0029",
	
	 "opcodes": "PUSH1 0x80 PUSH1 0x40 MSTORE CALLVALUE DUP1 ISZERO PUSH2 0x10 JUMPI PUSH1 0x0 
	DUP1 REVERT JUMPDEST POP PUSH2 0x12A DUP1 PUSH2 0x20 PUSH1 0x0 CODECOPY PUSH1 0x0 RETURN 
	STOP PUSH1 0x80 PUSH1 0x40 MSTORE PUSH1 0x4 CALLDATASIZE LT PUSH1 0x3E JUMPI PUSH4 
	0xFFFFFFFF PUSH29 
	
	0x100000000000000000000000000000000000000000000000000000000
	 PUSH1 0x0 CALLDATALOAD DIV AND PUSH4 0x17D7DE7C DUP2 EQ PUSH1 0x43 JUMPI JUMPDEST PUSH1 
	0x0 DUP1 REVERT JUMPDEST CALLVALUE DUP1 ISZERO PUSH1 0x4E JUMPI PUSH1 0x0 DUP1 REVERT 
	JUMPDEST POP PUSH1 0x55 PUSH1 0xC7 JUMP JUMPDEST PUSH1 0x40 DUP1 MLOAD PUSH1 0x20 DUP1 DUP3 
	MSTORE DUP4 MLOAD DUP2 DUP4 ADD MSTORE DUP4 MLOAD SWAP2 SWAP3 DUP4 SWAP3 SWAP1 DUP4 ADD 
	SWAP2 DUP6 ADD SWAP1 DUP1 DUP4 DUP4 PUSH1 0x0 JUMPDEST DUP4 DUP2 LT ISZERO PUSH1 0x8D JUMPI 
	DUP2 DUP2 ADD MLOAD DUP4 DUP3 ADD MSTORE PUSH1 0x20 ADD PUSH1 0x77 JUMP JUMPDEST POP POP 
	POP POP SWAP1 POP SWAP1 DUP2 ADD SWAP1 PUSH1 0x1F AND DUP1 ISZERO PUSH1 0xB9 JUMPI DUP1 
	DUP3 SUB DUP1 MLOAD PUSH1 0x1 DUP4 PUSH1 0x20 SUB PUSH2 0x100 EXP SUB NOT AND DUP2 MSTORE 
	PUSH1 0x20 ADD SWAP2 POP JUMPDEST POP SWAP3 POP POP POP PUSH1 0x40 MLOAD DUP1 SWAP2 SUB 
	SWAP1 RETURN JUMPDEST PUSH1 0x40 DUP1 MLOAD DUP1 DUP3 ADD SWAP1 SWAP2 MSTORE PUSH1 0x3 DUP2 
	MSTORE PUSH32 0x7373730000000000000000000000000000000000000000000000000000000000 PUSH1 0x20 
	DUP3 ADD MSTORE SWAP1 JUM

## bytecode: ##

	0x608060405234801561001057600080fd5b5061012a806100206000396000f300608060405260043610603e576
	3ffffffff7c010000000000000000000000000000000000000000000000000000000060003504166317d7de7c81
	146043575b600080fd5b348015604e57600080fd5b50605560c7565b60408051602080825283518183015283519
	19283929083019185019080838360005b83811015608d5781810151838201526020016077565b50505050905090
	810190601f16801560b95780820380516001836020036101000a031916815260200191505b50925050506040518
	0910390f35b60408051808201909152600381527f73737300000000000000000000000000000000000000000000
	000000000000006020820152905600a165627a7a72305820f712e15abc7477ae7b54729eb495586b7370dcc2356
	a777651c2fdfd8bc6f72a0029

## abi: ##
	
	[{\"constant\":true,\"inputs\":[],\"name\":\"getName\",\"outputs\":[{\"name\":\"\",\"type
	\":\"string\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"}]

# 测试步骤 #

# 常用命令 #	
		admin.peers()				//获取其他节点信息
		eth.accounts 				//获取节点账户信息 	 
		eth.getBalance(账户地址)		//获取账户余额信息 	 
		personal.newAccount("密码")		//生成账户并设置密码
		personal.unlockAccount(账户,密码)	//对账户进行解锁
		eth.sendTransaction({data:合约信息,from:交易发起者,gas:交易手续费用}) //部署模板交易
	    eth.getTransactionReceipt(交易Hash)	//获取交易信息
		eth.getCode(账户地址)			//获取账户合约代码
		miner.start()				//开启挖矿
		miner.stop()				//关闭挖矿


	测试网络节点共五个。
![avatar](https://github.com/ziYuDipnet/Documentation/blob/master/imgs/example/peers.png?raw=true)

	查看账户起始金额,可以看到账户2余额为0.
![](https://github.com/ziYuDipnet/Documentation/blob/master/imgs/example/accountBalance.png?raw=true)

	初始化变量byte(合约字节码加上coinbase地址).
![](https://github.com/ziYuDipnet/Documentation/blob/master/imgs/example/ThreadStartByte.png?raw=true)

	通过byte部署模板上链,传入参数为交易发起者、合约的字节码、以及提供的gas即可.
![](https://github.com/ziYuDipnet/Documentation/blob/master/imgs/example/DeploymentTemplate1.png?raw=true)

	部署成功以后可以通过查询模板地址获取模板合约
![](https://github.com/ziYuDipnet/Documentation/blob/master/imgs/example/getTemplateCode1.png?raw=true)

	接下来是通过模板来部署合约，传入参数交易发起者、模板地址、以及提供的GAS即可.
![](https://github.com/ziYuDipnet/Documentation/blob/master/imgs/example/DeploymentContract1.png?raw=true)

	最后发起调用该合约交易,出块完成后查看coinbase(模板开发奖励地址)的余额，可以看到确实收到了一笔资金奖励.
![](https://github.com/ziYuDipnet/Documentation/blob/master/imgs/example/feeSplitting1.png?raw=true)



# 测试目的 #
	本次测试分为两个部分：
	第一测试部署模板,并且可以通过模板加上传入的参数部署合约，合约调用正常;
	第二测试模板开发者与矿工激励分成.
# 准备材料 #
	预先准备下列数据，该合约为带构造函数的合约。
## 合约代码： ##

	pragma solidity ^0.4.16;

         contract order{
           string public name;
           function order(string na) {
              name = na;
           }
           function getName() public constant returns(string){
              return name;
           }
        }
## bytecode: ##
	0x608060405234801561001057600080fd5b5060405161032a38038061032a83398101604052805101805161003a906000906020840190610041565b5050610 

	0dc565b828054600181600116156101000203166002900490600052602060002090601f016020900481019282601f1061008257805160ff19168380011785

	556100af565b828001600101855582156100af579182015b828111156100af578251825591602001919060010190610094565b506100bb9291506100bf565
 
	b5090565b6100d991905b808211156100bb57600081556001016100c5565b90565b61023f806100eb6000396000f30060806040526004361061004b5763ff
 
	ffffff7c010000000000000000000000000000000000000000000000000000000060003504166306fdde03811461005057806317d7de7c146100da575b600
 
	080fd5b34801561005c57600080fd5b506100656100ef565b6040805160208082528351818301528351919283929083019185019080838360005b83811015
 
	61009f578181015183820152602001610087565b50505050905090810190601f1680156100cc5780820380516001836020036101000a03191681526020019
 
	1505b509250505060405180910390f35b3480156100e657600080fd5b5061006561017d565b60008054604080516020600260018516156101000260001901
 
	90941693909304601f810184900484028201840190925281815292918301828280156101755780601f1061014a57610100808354040283529160200191610
 
	175565b820191906000526020600020905b81548152906001019060200180831161015857829003601f168201915b505050505081565b6000805460408051
 
	6020601f600260001961010060018816150201909516949094049384018190048102820181019092528281526060939092909183018282801561020957806 

	01f106101de57610100808354040283529160200191610209565b820191906000526020600020905b8154815290600101906020018083116101ec57829003
 
	601f168201915b50505050509050905600a165627a7a72305820f8c91f42f37a64db26e681a0edaafbaa602fed1b209556995948fd1e1be320910029

## abi: ##
     
     [ { "constant": true, "inputs": [], "name": "name", "outputs": [ { "name": "", "type": "string" } ], "payable": false, 
     "stateMutability": "view", "type": "function" }, { "constant": true, "inputs": [], "name": "getName", "outputs": [ { 
     "name": "", "type": "string" } ], "payable": false, "stateMutability": "view", "type": "function" }, { "inputs": [ { 
     "name": "na", "type": "string" } ], "payable": false, "stateMutability": "nonpayable", "type": "constructor" } ]

## 传入参数 ##
	0x00000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000
	0033131300000000000000000000000000000000000000000000000000000000000


# 测试步骤 #

	测试网络节点共五个。
![avatar](https://github.com/ziYuDipnet/Documentation/blob/master/imgs/example/peers.png?raw=true)

	查看账户起始金额,可以看到账户3余额为0.
![](https://github.com/ziYuDipnet/Documentation/blob/master/imgs/example/InitialAmount.png?raw=true)

	初始化变量byte(合约字节码加上coinbase地址).
![](https://github.com/ziYuDipnet/Documentation/blob/master/imgs/example/byte2.png?raw=true)

	通过byte部署模板上链,传入参数为交易发起者、合约的字节码、以及提供的gas即可.
![](https://github.com/ziYuDipnet/Documentation/blob/master/imgs/example/DeploymentTemplate2.png?raw=true)

	接下来是通过模板来部署合约，传入参数交易发起者、模板地址、传入合约所需参数、以及提供的GAS即可.
![](https://github.com/ziYuDipnet/Documentation/blob/master/imgs/example/DeploymentContract2.png?raw=true)

	最后发起调用该合约交易,出块完成后查看coinbase(模板开发奖励地址)的余额，可以看到确实收到了一笔资金奖励.
![](https://github.com/ziYuDipnet/Documentation/blob/master/imgs/example/feeSplitting2.png?raw=true)


