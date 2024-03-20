pragma solidity ^0.8.0;

interface IPolymer {
    function sendUniversalPacket(address destination, bytes memory data) external;
}

contract CrossChainQuery {
    IPolymer public polymer;
    address public baseContractAddress = 0x528f7971cE3FF4198c3e6314AA223C83C7755bf7;

    string public secretMessage;

    constructor(address _polymerAddress) {
        polymer = IPolymer(_polymerAddress);
    }

    function sendQuery() public {
        string memory query = "crossChainQuery";
        polymer.sendUniversalPacket(baseContractAddress, abi.encode(msg.sender, query));
    }

    function onAcknowledgementUniversalPacket(bytes32 channelId, bytes memory data) external {
        (string memory ackData) = abi.decode(data, (string));
        secretMessage = ackData;
        emit SecretMessageReceived(secretMessage);
    }

    event SecretMessageReceived(string secretMessage);
}
