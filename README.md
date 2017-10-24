# Bivrost [![Build Status](https://travis-ci.org/gnosis/bivrost-swift.svg?branch=master)](https://travis-ci.org/gnosis/bivrost-swift)

🔥 🌈 Bridge between Solidity Contracts and Swift

[![CI Status](http://img.shields.io/travis/gnosis/bivrost-swift.svg?style=flat)](https://travis-ci.org/gnosis/bivrost-swift)

Bivrost is in very early development. Do not use this for anything important.

## Description

`bivrost` generates Swift classes for Solidity contracts that know how to encode and decode their function calls. Use this to generate transactions that can then be sent to the blockchain.

You can use this as a pre-compile script phase for your Swift project. Generate the needed files, then add them to your project.

`bivrost` creates an output folder that contains auxiliary code for encoding and decoding as well as generated contract classes.

## Usage

### [Mint](https://github.com/yonaskolb/mint) 🌱

    $ mint run gnosis/bivrost-swift "bivrost --input './contracts/*.json' --output '../Sources/Solidity'"

### SPM

    $ git clone https://github.com/gnosis/bivrost-swift
    $ cd bivrost-swift
    $ swift run bivrost --input './contracts/*.json' --output '../Sources/Solidity'

Using `BivrostKit` from code is currently not supported, but is on the roadmap.

## Options

- `--input` [default: ./abi/*.json] - Input file pattern specifying which json files should be parsed. Needs to be escaped with quotes to prevent the shell from expanding it.
- `--output` [default: ./solidity] - Output folder which will contain all necessary Solidity files (generated and auxiliary).
- `--force` [default: false] - If set, deletes all contents of the output folder before recreating it. Make sure there is nothing else in there.

Run `$ mint run gnosis/bivrost-swift "bivrost --help"` for current help.

## Input files

`bivrost` parses the Solidity contract JSON files generated by the [truffle compiler](https://github.com/trufflesuite/truffle) for code generation. They should have the following format:

    {
        "contract_name": "TestContract",
        "abi": [
          {
            "constant": false,
            "inputs": [
              {
                "name": "inputVariable",
                "type": "uint16"
              },
              {
                "name": "anotherInputVariable",
                "type": "uint128"
              }
            ],
            "name": "aTestFunction",
            "outputs": [
              {
                "name": "outputVariable",
                "type": "bool"
              }
            ],
            "payable": false,
            "type": "function"
          },
          [ more function definitions... ]
        ]
    }

## Output

`bivrost` generates following folder structure in the output folder:

    <output folder>
      - BivrostError.swift
      - Coding
        - BaseEncoder.swift
        - BaseDecoder.swift
      - Extensions
        - BigIntExtension.swift
        - DataExtension.swift
        - StringExtension.swift
      - Generated Contracts
        - ContractName1.swift
        - ContractName2.swift
      - Generated Types
        - ArrayX-Generated.swift
        - BytesX-Generated.swift
        - IntX-Generated.swift
        - UIntX-Generated.swift
      - Protocols
        - DynamicType.swift
        - SolidityCodable.swift
        - SolidityFunction.swift
        - StaticType.swift
      - Types
        - Address.swift
        - ArrayX.swift
        - Bool.swift
        - Bytes.swift
        - BytesX.swift
        - Function.swift
        - IntX.swift
        - Solidity.swift
        - String.swift
        - UIntX.swift
        - VariableArray.swift

The generated code depends on [BigInt](https://github.com/attaswift/BigInt) (`import BigInt`) so make sure you make that available for the generated code.

## License

Bivrost is available under the MIT license. See the LICENSE file for more info.

## Issues

Tests currently do not work when generating an Xcode project via `swift package generate-xcodeproj`. This issue is described at the bottom of <https://github.com/Quick/Quick/issues/707>. Drop to the CMD and use `swift test` for testing.