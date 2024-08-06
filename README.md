# DIO NFT Pokémon
 Criando o seu NFT de Pokémon com Blockchain

 ## Objetivo
 Estudar os métodos de criação de NFT através de um smart contract na rede Ethereum.
 Vai ser criado uma coleção de bicinhos da séries Pokémon para viabilizar os estudos.

 ## Tecnologias utilizadas
 * Ide Remix https://remix.ethereum.org/
 * Truffle https://archive.trufflesuite.com/
 * Ganache https://archive.trufflesuite.com/ganache/
 * Metamask https://metamask.io/

## Smart contract
```solidity
 // SPDX-License-Identifier: GPL-3.0

pragma solidity ^0.8.0;

/* 
 * @title DIO_NFT_Pokemon
 * Estudo de criação e implementação de um NFT na rede Ethereum
 * Implementado com base no repo https://github.com/digitalinnovationone/formacao-blockchain-dio
 * dev https://www.linkedin.com/in/jcbdeoliveira/
 * 06/08/2024
 */

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";

contract PokeDIO is ERC721{

    struct Pokemon{
        string name;
        uint level;
        string img;
    }

    Pokemon[] public pokemons;
    address public gameOwner;

    constructor () ERC721 ("PokeDIO", "PKD"){

        gameOwner = msg.sender;

    } 

    modifier onlyOwnerOf(uint _monsterId) {

        require(ownerOf(_monsterId) == msg.sender,"Apenas o dono pode batalhar com este Pokemon");
        _;

    }

    function battle(uint _attackingPokemon, uint _defendingPokemon) public onlyOwnerOf(_attackingPokemon){
        Pokemon storage attacker = pokemons[_attackingPokemon];
        Pokemon storage defender = pokemons[_defendingPokemon];

         if (attacker.level >= defender.level) {
            attacker.level += 2;
            defender.level += 1;
        }else{
            attacker.level += 1;
            defender.level += 2;
        }
    }

    function createNewPokemon(string memory _name, address _to, string memory _img) public {
        require(msg.sender == gameOwner, "Apenas o dono do jogo pode criar novos Pokemons");
        uint id = pokemons.length;
        pokemons.push(Pokemon(_name, 1,_img));
        _safeMint(_to, id);
    }
}
```
## Resultado do deploy
![localImage](./img/01.png)

## Operações (fucntions) do contrato
![localImage](./img/02.png)

## Painel do ganache
![localImage](./img/03.png)
