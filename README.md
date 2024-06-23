


markdown
# Patent Registry Contract

Este projeto é um contrato inteligente Ethereum escrito em Solidity para registrar patentes como tokens não fungíveis (NFTs) utilizando o padrão ERC-721. O contrato também armazena informações sobre o proprietário inicial e seu herdeiro.

## Descrição

O contrato permite:
- Registrar patentes com um nome e uma descrição.
- Transferir a propriedade das patentes registradas.
- Consultar informações sobre as patentes e o proprietário.

## Proprietário e Herdeiro

- **Proprietário Inicial**: Lucas Januario do Nascimento
  - Data de Nascimento: 12/07/1999
  - CPF: 48761013789
  - Localização: São Paulo, Brasil
- **Herdeiro**: Gabriela Januário do Nascimento

## Contrato Solidity

### PatentRegistry.sol

solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract PatentRegistry is ERC721, Ownable {
    uint256 public patentCount = 0;

    struct Patent {
        uint256 id;
        string name;
        string description;
        address owner;
    }

    struct OwnerInfo {
        string name;
        string birthDate;
        string cpf;
        string location;
        string heirName;
    }

    mapping(uint256 => Patent) public patents;
    OwnerInfo public ownerInfo;

    event PatentRegistered(
        uint256 indexed id,
        string name,
        string description,
        address indexed owner
    );

    // Constructor setting the deployer as the owner
    constructor(
        address _owner,
        string memory _name,
        string memory _birthDate,
        string memory _cpf,
        string memory _location,
        string memory _heirName
    ) ERC721("PatentRegistry", "PATENT") {
        transferOwnership(_owner);
        ownerInfo = OwnerInfo({
            name: _name,
            birthDate: _birthDate,
            cpf: _cpf,
            location: _location,
            heirName: _heirName
        });
    }

    function registerPatent(string memory _name, string memory _description) public onlyOwner {
        patentCount++;
        patents[patentCount] = Patent(patentCount, _name, _description, msg.sender);
        _mint(msg.sender, patentCount);
        emit PatentRegistered(patentCount, _name, _description, msg.sender);
    }

    function transferPatent(uint256 _patentId, address _newOwner) public {
        require(ownerOf(_patentId) == msg.sender, "You must be the owner of the patent to transfer it.");
        _transfer(msg.sender, _newOwner);
        patents[_patentId].owner = _newOwner;
    }

    function getPatent(uint256 _patentId) public view returns (Patent memory) {
        return patents[_patentId];
    }

    function getOwnerInfo() public view returns (OwnerInfo memory) {
        return ownerInfo;
    }
}


## Implantação do Contrato

1. **Compile o Contrato**:
   - Use o Remix ou sua ferramenta de escolha para compilar o contrato `PatentRegistry.sol`.

2. **Implante o Contrato**:
   - Na seção de deploy do Remix, forneça os argumentos necessários:
     - `_owner`: "0xYourEthereumAddress" (substitua pelo seu endereço Ethereum real)
     - `_name`: "Lucas Januario do Nascimento"
     - `_birthDate`: "12/07/1999"
     - `_cpf`: "48761013789"
     - `_location`: "São Paulo, Brasil"
     - `_heirName`: "Gabriela Januário do Nascimento"

3. **Interaja com o Contrato**:
   - Use a função `registerPatent` para adicionar novas patentes.
   - Use a função `transferPatent` para transferir patentes.
   - Use a função `getPatent` para consultar detalhes de uma patente.
   - Use a função `getOwnerInfo` para obter informações sobre o proprietário e o herdeiro.

## Patentes Registradas

- **rubyxERC-98**
- **ERC-299**
- **BANCO BOSCO JONES**
- **BR1 2077**

## Licença

Este projeto é licenciado sob a licença MIT. Veja o arquivo `LICENSE` para mais detalhes.


### Nota Adicional

Este README.md fornece uma visão geral do contrato, detalhes sobre o proprietário e o herdeiro, e instruções sobre como implantar e usar o contrato. Certifique-se de revisar e ajustar as informações conforme necessário antes de publicar ou compartilhar o projeto.
