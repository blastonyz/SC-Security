'forge clean' running (wd: /mnt/c/Users/Blas/Desktop/Proyectos/Cyfrin/Security/puppy-raffle/4-puppy-raffle-audit)
'forge config --json' running
'forge build --build-info --skip ./test/** ./script/** --force' running (wd: /mnt/c/Users/Blas/Desktop/Proyectos/Cyfrin/Security/puppy-raffle/4-puppy-raffle-audit)
INFO:Detectors:
Detector: arbitrary-send-eth
PuppyRaffle.withdrawFees() (src/PuppyRaffle.sol#178-186) sends eth to arbitrary user
	Dangerous calls:
	- (success,None) = feeAddress.call{value: feesToWithdraw}() (src/PuppyRaffle.sol#185)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#functions-that-send-ether-to-arbitrary-destinations
INFO:Detectors:
Detector: weak-prng
PuppyRaffle.selectWinner() (src/PuppyRaffle.sol#137-177) uses a weak PRNG: "winnerIndex = uint256(keccak256(bytes)(abi.encodePacked(msg.sender,block.timestamp,block.difficulty))) % players.length (src/PuppyRaffle.sol#144-145)" 
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#weak-PRNG
INFO:Detectors:
Detector: incorrect-equality
PuppyRaffle.withdrawFees() (src/PuppyRaffle.sol#178-186) uses a dangerous strict equality:
	- require(bool,string)(address(this).balance == uint256(totalFees),PuppyRaffle: There are currently players active!) (src/PuppyRaffle.sol#179-183)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#dangerous-strict-equalities
INFO:Detectors:
Detector: reentrancy-no-eth
Reentrancy in PuppyRaffle.refund(uint256) (src/PuppyRaffle.sol#104-112):
	External calls:
	- address(msg.sender).sendValue(entranceFee) (src/PuppyRaffle.sol#110-111)
	State variables written after the call(s):
	- players[playerIndex] = address(0) (src/PuppyRaffle.sol#111)
	PuppyRaffle.players (src/PuppyRaffle.sol#25-27) can be used in cross function reentrancies:
	- PuppyRaffle.enterRaffle(address[]) (src/PuppyRaffle.sol#83-97)
	- PuppyRaffle.getActivePlayerIndex(address) (src/PuppyRaffle.sol#120-129)
	- PuppyRaffle.players (src/PuppyRaffle.sol#25-27)
	- PuppyRaffle.refund(uint256) (src/PuppyRaffle.sol#104-112)
	- PuppyRaffle.selectWinner() (src/PuppyRaffle.sol#137-177)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#reentrancy-vulnerabilities-2
INFO:Detectors:
Detector: missing-zero-check
PuppyRaffle.constructor(uint256,address,uint256)._feeAddress (src/PuppyRaffle.sol#63) lacks a zero-check on :
		- feeAddress = _feeAddress (src/PuppyRaffle.sol#65-66)
PuppyRaffle.changeFeeAddress(address).newFeeAddress (src/PuppyRaffle.sol#189-190) lacks a zero-check on :
		- feeAddress = newFeeAddress (src/PuppyRaffle.sol#192)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#missing-zero-address-validation
INFO:Detectors:
Detector: reentrancy-events
Reentrancy in PuppyRaffle.refund(uint256) (src/PuppyRaffle.sol#104-112):
	External calls:
	- address(msg.sender).sendValue(entranceFee) (src/PuppyRaffle.sol#110-111)
	Event emitted after the call(s):
	- RaffleRefunded(playerAddress) (src/PuppyRaffle.sol#111-112)
Reentrancy in PuppyRaffle.selectWinner() (src/PuppyRaffle.sol#137-177):
	External calls:
	- (success,None) = winner.call{value: prizePool}() (src/PuppyRaffle.sol#173-174)
	- _safeMint(winner,tokenId) (src/PuppyRaffle.sol#177)
		- returndata = to.functionCall(abi.encodeWithSelector(IERC721Receiver(to).onERC721Received.selector,_msgSender(),from,tokenId,_data),ERC721: transfer to non ERC721Receiver implementer) (lib/openzeppelin-contracts/contracts/token/ERC721/ERC721.sol#441-447)
		- (success,returndata) = target.call{value: value}(data) (lib/openzeppelin-contracts/contracts/utils/Address.sol#119)
	External calls sending eth:
	- (success,None) = winner.call{value: prizePool}() (src/PuppyRaffle.sol#173-174)
	- _safeMint(winner,tokenId) (src/PuppyRaffle.sol#177)
		- (success,returndata) = target.call{value: value}(data) (lib/openzeppelin-contracts/contracts/utils/Address.sol#119)
	Event emitted after the call(s):
	- Transfer(address(0),to,tokenId) (lib/openzeppelin-contracts/contracts/token/ERC721/ERC721.sol#343)
		- _safeMint(winner,tokenId) (src/PuppyRaffle.sol#177)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#reentrancy-vulnerabilities-4
INFO:Detectors:
Detector: timestamp
PuppyRaffle.selectWinner() (src/PuppyRaffle.sol#137-177) uses timestamp for comparisons
	Dangerous comparisons:
	- require(bool,string)(block.timestamp >= raffleStartTime + raffleDuration,PuppyRaffle: Raffle not over) (src/PuppyRaffle.sol#139-142)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#block-timestamp
INFO:Detectors:
Detector: pragma
4 different versions of Solidity are used:
	- Version constraint >=0.6.0 is used by:
		->=0.6.0 (lib/base64/base64.sol#3)
	- Version constraint >=0.6.0<0.8.0 is used by:
		->=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/access/Ownable.sol#3)
		->=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/introspection/ERC165.sol#3)
		->=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/introspection/IERC165.sol#3)
		->=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/math/SafeMath.sol#3)
		->=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/token/ERC721/ERC721.sol#3)
		->=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/token/ERC721/IERC721Receiver.sol#3)
		->=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/utils/Context.sol#3)
		->=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/utils/EnumerableMap.sol#3)
		->=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/utils/EnumerableSet.sol#3)
		->=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/utils/Strings.sol#3)
	- Version constraint >=0.6.2<0.8.0 is used by:
		->=0.6.2<0.8.0 (lib/openzeppelin-contracts/contracts/token/ERC721/IERC721.sol#3)
		->=0.6.2<0.8.0 (lib/openzeppelin-contracts/contracts/token/ERC721/IERC721Enumerable.sol#3)
		->=0.6.2<0.8.0 (lib/openzeppelin-contracts/contracts/token/ERC721/IERC721Metadata.sol#3)
		->=0.6.2<0.8.0 (lib/openzeppelin-contracts/contracts/utils/Address.sol#3)
	- Version constraint 0.7.6 is used by:
		-0.7.6 (src/PuppyRaffle.sol#1-2)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#different-pragma-directives-are-used
INFO:Detectors:
Detector: dead-code
PuppyRaffle._isActivePlayer() (src/PuppyRaffle.sol#194-200) is never used and should be removed
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#dead-code
INFO:Detectors:
Detector: solc-version
Version constraint 0.7.6 contains known severe issues (https://solidity.readthedocs.io/en/latest/bugs.html)
	- FullInlinerNonExpressionSplitArgumentEvaluationOrder
	- MissingSideEffectsOnSelectorAccess
	- AbiReencodingHeadOverflowWithStaticArrayCleanup
	- DirtyBytesArrayToStorage
	- DataLocationChangeInInternalOverride
	- NestedCalldataArrayAbiReencodingSizeValidation
	- SignedImmutables
	- ABIDecodeTwoDimensionalArrayMemory
	- KeccakCaching.
It is used by:
	- 0.7.6 (src/PuppyRaffle.sol#1-2)
solc-0.7.6 is an outdated solc version. Use a more recent version (at least 0.8.0), if possible.
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#incorrect-versions-of-solidity
INFO:Detectors:
Detector: low-level-calls
Low level call in PuppyRaffle.selectWinner() (src/PuppyRaffle.sol#137-177):
	- (success,None) = winner.call{value: prizePool}() (src/PuppyRaffle.sol#173-174)
Low level call in PuppyRaffle.withdrawFees() (src/PuppyRaffle.sol#178-186):
	- (success,None) = feeAddress.call{value: feesToWithdraw}() (src/PuppyRaffle.sol#185)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#low-level-calls
INFO:Detectors:
Detector: unindexed-event-address
Event PuppyRaffle.RaffleRefunded(address) (src/PuppyRaffle.sol#56-57) has address parameters but no indexed parameters
Event PuppyRaffle.FeeAddressChanged(address) (src/PuppyRaffle.sol#57-58) has address parameters but no indexed parameters
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#unindexed-event-address-parameters
INFO:Detectors:
Detector: cache-array-length
Loop condition i < players.length (src/PuppyRaffle.sol#196-197) should use cached array length instead of referencing `length` member of the storage array.
 Loop condition j < players.length (src/PuppyRaffle.sol#95) should use cached array length instead of referencing `length` member of the storage array.
 Loop condition i < players.length (src/PuppyRaffle.sol#122-123) should use cached array length instead of referencing `length` member of the storage array.
 Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#cache-array-length
INFO:Detectors:
Detector: constable-states
PuppyRaffle.commonImageUri (src/PuppyRaffle.sol#41-42) should be constant 
PuppyRaffle.legendaryImageUri (src/PuppyRaffle.sol#51-52) should be constant 
PuppyRaffle.rareImageUri (src/PuppyRaffle.sol#46-47) should be constant 
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#state-variables-that-could-be-declared-constant
INFO:Detectors:
Detector: immutable-states
PuppyRaffle.raffleDuration (src/PuppyRaffle.sol#27-28) should be immutable 
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#state-variables-that-could-be-declared-immutable
INFO:Slither:. analyzed (16 contracts with 100 detectors), 24 result(s) found
