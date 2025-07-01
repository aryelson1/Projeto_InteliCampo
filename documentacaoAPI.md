# Documentação da API do Intelicampo

A API do Intelicampo fornece uma interface programática para gerenciamento de informações em uma fazenda, incluindo dados sobre animais, pastos, lotes e muito mais. Esta documentação descreve cada um dos módulos disponíveis, incluindo os endpoints, os DTOs utilizados e exemplos de chamadas e respostas.

## Índice

- [Módulo Account](#módulo-account)
- [Módulo Animal](#módulo-animal)
- [Módulo Balance](#módulo-balance)
- [Módulo Farm](#módulo-farm)
- [Módulo Pasture](#módulo-pasture)
- [Módulo Batch](#módulo-batch)
- [Módulo Picket](#módulo-picket)

### Estrutura de Arquivos

- **dto/**
  - **create.dto.ts**: Define os campos para criar uma nova conta.
  - **select.dto.ts**: Define os campos para consulta de contas.
  - **update.dto.ts**: Define os campos que podem ser atualizados em uma conta existente.

- **entities/**
  - **account.entity.ts**: Define a tabela de contas.

- **account.controller.ts**: Gerencia as requisições HTTP relacionadas a contas.

- **account.module.ts**: Configura as dependências do módulo Account.

- **account.service.ts**: Implementa a lógica de negócios para operações de contas.

## Módulo Account
## AccountController

O `AccountController` gerencia as operações de CRUD (Create, Read, Update, Delete) para contas na API. Ele utiliza o `AccountService` para interagir com o banco de dados e os DTOs para validação e estruturação dos dados de entrada e saída. Este controlador é decorado com `@ApiTags('Account')` para organização na documentação Swagger.

## Endpoints

### POST /account

- **Descrição**: Cria uma nova conta.
- **Corpo da Requisição**: Recebe os dados através de `CreateAccountDto`, que valida e estrutura a entrada de dados para a criação de uma conta.
- **Comportamento**: Invoca `accountService.create()` passando os dados validados para criar uma nova conta no banco de dados.

### GET /account

- **Descrição**: Retorna uma lista paginada de todas as contas.
- **Query Params**: Aceita parâmetros de paginação através de `PaginationOptionsQuery`, que incluem `limit` e `page`.
- **Comportamento**: Utiliza `accountService.findAll()` com opções de paginação para buscar e retornar contas. O resultado é paginado usando o padrão definido por `nestjs-typeorm-paginate`.

### GET /account/:id

- **Descrição**: Busca uma conta específica pelo ID.
- **Parâmetros**: `id` - o identificador único da conta.
- **Comportamento**: Chama `accountService.findOne(id)`, retornando os detalhes da conta correspondente ao ID fornecido.

### PATCH /account/:id

- **Descrição**: Atualiza uma conta existente.
- **Parâmetros**: `id` - o identificador único da conta.
- **Corpo da Requisição**: Recebe os dados através de `UpdateAccountDto`, que valida e estrutura os dados de entrada para atualização.
- **Comportamento**: Executa `accountService.update(id, updateAccountDto)` para atualizar a conta especificada com os novos dados fornecidos.

### DELETE /account/:id

- **Descrição**: Remove uma conta pelo ID.
- **Parâmetros**: `id` - o identificador único da conta a ser removida.
- **Comportamento**: Invoca `accountService.remove(id)`, que exclui a conta do banco de dados.

## Decoradores e Utilitários

- `@ApiPaginatedResponse`: Um decorador customizado que automatiza a documentação da resposta paginada para o endpoint GET /account.
- `@ApiResponse`: Usado no método GET /account/:id para especificar o tipo de resposta esperada, facilitando a geração da documentação automática com Swagger.


# Módulo Animal
## AnimalController

O `AnimalController` gerencia as operações relacionadas aos animais na API, utilizando o serviço `AnimalService`. Este controlador está anotado com `@ApiTags('Animal')` para categorização na documentação Swagger.

## Endpoints

### POST /animal

- **Descrição**: Cria um novo animal.
- **Corpo da Requisição**: Recebe os dados através de `CreateAnimalDto`, que valida e estrutura a entrada de dados para a criação de um animal.
- **Comportamento**: Invoca `animalService.create(createAnimalDto)` para adicionar um novo animal no banco de dados.

### GET /animal

- **Descrição**: Retorna uma lista paginada de animais, filtrada por critérios específicos.
- **Query Params**:
  - `AnimalFilter`: Filtra a busca por atributos específicos do animal.
  - `PaginationOptionsQuery`: Parâmetros de paginação, incluindo `limit` e `page`.
- **Comportamento**: Utiliza `animalService.findAll(animalFilter, options)` para buscar e retornar animais conforme os filtros e opções de paginação fornecidos. A resposta é configurada pelo `paginationDTOResponse(SelectAnimalDto)`.

### GET /animal/:id

- **Descrição**: Busca um animal específico pelo ID.
- **Parâmetros**: `id` - o identificador único do animal.
- **Comportamento**: Chama `animalService.findOne(id)`, retornando os detalhes do animal correspondente ao ID fornecido.

### GET /animal/rfid/:rfid

- **Descrição**: Busca um animal pelo seu RFID.
- **Parâmetros**: `rfid` - o código RFID do animal.
- **Comportamento**: Executa `animalService.findOneByRfid(rfid)`, que retorna os detalhes do animal que possui o RFID especificado.

### PATCH /animal/:id

- **Descrição**: Atualiza um animal existente.
- **Parâmetros**: `id` - o identificador único do animal.
- **Corpo da Requisição**: Recebe os dados através de `UpdateAnimalDto`, que valida e estrutura os dados de entrada para atualização.
- **Comportamento**: Invoca `animalService.update(id, updateAnimalDto)` para atualizar o animal especificado com os novos dados fornecidos.

### DELETE /animal/:id

- **Descrição**: Remove um animal pelo ID.
- **Parâmetros**: `id` - o identificador único do animal a ser removido.
- **Comportamento**: Utiliza `animalService.remove(id)` para excluir o animal do banco de dados.

## Decoradores e Utilitários

- `@ApiResponse`: Usado para descrever as respostas esperadas dos endpoints, particularmente útil no endpoint `GET /animal`, onde especifica o tipo e estrutura da resposta paginada.
- `paginationDTOResponse`: Um utilitário que ajuda a definir o tipo de resposta esperada para listagens paginadas, facilitando a integração com o Swagger para documentação automática.

# Módulo Balance
## BalanceController

O `BalanceController` gerencia as operações relacionadas a saldos financeiros na API, usando o serviço `BalanceService`. Este controlador é categorizado com `@ApiTags('Balance')` para fins de organização na documentação Swagger.

## Endpoints

### POST /balance

- **Descrição**: Cria um novo saldo.
- **Corpo da Requisição**: Usa `CreateBalanceDto` para validar e estruturar a entrada de dados.
- **Comportamento**: Chama `balanceService.create(createBalanceDto)` para adicionar um novo registro de saldo no banco de dados.

### GET /balance

- **Descrição**: Retorna uma lista paginada de saldos, filtrada por critérios específicos.
- **Query Params**:
  - `BalanceFilter`: Permite a filtragem de saldos por critérios definidos.
  - `PaginationOptionsQuery`: Parâmetros de paginação, incluindo `limit` e `page`.
- **Comportamento**: Utiliza `balanceService.findAll(balanceFilter, options)` para buscar e retornar saldos conforme os filtros e opções de paginação. Decorado com `@ApiPaginatedResponse(SelectBalanceDto)` para documentação da resposta paginada.

### GET /balance/:id

- **Descrição**: Busca um saldo específico pelo ID.
- **Parâmetros**: `id` - identificador único do saldo.
- **Comportamento**: Invoca `balanceService.findOne(id)`, retornando os detalhes do saldo especificado.

### PATCH /balance/:id

- **Descrição**: Atualiza um saldo existente.
- **Parâmetros**: `id` - identificador único do saldo.
- **Corpo da Requisição**: Usa `UpdateBalanceDto` para validar e estruturar a entrada de dados para atualização.
- **Comportamento**: Chama `balanceService.update(id, updateBalanceDto)` para atualizar o saldo com os novos dados fornecidos.

### DELETE /balance/:id

- **Descrição**: Remove um saldo pelo ID.
- **Parâmetros**: `id` - identificador único do saldo a ser removido.
- **Comportamento**: Utiliza `balanceService.remove(id)` para excluir o saldo do banco de dados.

### POST /balance/:id/ping

- **Descrição**: Envia um "ping" para um saldo específico, geralmente para verificar a conexão ou o status.
- **Parâmetros**: `id` - identificador único do saldo.
- **Corpo da Requisição**: Usa `CreatePingBalanceDto` para validar e estruturar a entrada de dados específicos para o ping.
- **Comportamento**: Executa `balanceService.ping(id, createPingBalanceDto)`, que pode ser usado para realizar verificações de integridade ou atualizações de status relacionadas ao saldo.

## Decoradores e Utilitários

- `@ApiPaginatedResponse`: Decorador customizado que automatiza a documentação de respostas paginadas, especificamente usado no endpoint `GET /balance` para definir o tipo de resposta esperada.
- `@ApiResponse`: Usado no método `GET /balance/:id` para especificar a estrutura da resposta esperada, facilitando a geração de documentação Swagger precisa.


# Módulo Farm
## FarmController

O `FarmController` gerencia as operações relativas a fazendas na API, utilizando o serviço `FarmService`. Este controlador está categorizado com `@ApiTags('Farm')` para facilitar a organização na documentação Swagger.

## Endpoints

### POST /farm

- **Descrição**: Cria uma nova fazenda.
- **Corpo da Requisição**: Utiliza `CreateFarmDto` para validar e estruturar a entrada de dados.
- **Comportamento**: Invoca `farmService.create(createFarmDto)` para adicionar um novo registro de fazenda no banco de dados.

### GET /farm

- **Descrição**: Retorna uma lista paginada de fazendas, opcionalmente filtrada por um ID de conta.
- **Query Params**:
  - `PaginationOptionsQuery`: Parâmetros de paginação, incluindo `limit` e `page`.
  - `accountId` (opcional): Filtra as fazendas associadas a uma conta específica.
- **Comportamento**: Utiliza `farmService.findAll(options, accountId)` para buscar e retornar fazendas conforme os critérios especificados. A resposta é configurada pelo `@ApiPaginatedResponse(SelectFarmDto)` para documentação da resposta paginada.

### GET /farm/:id

- **Descrição**: Busca uma fazenda específica pelo ID.
- **Parâmetros**: `id` - identificador único da fazenda.
- **Comportamento**: Chama `farmService.findOne(id)`, retornando os detalhes da fazenda especificada.

### GET /farm/:id/address

- **Descrição**: Retorna o endereço associado a uma fazenda específica, identificada pelo ID.
- **Parâmetros**: `id` - identificador da fazenda.
- **Comportamento**: Utiliza `farmService.findAddressByFarmId(id)` para buscar e retornar o endereço da fazenda especificada.

### GET /farm/:id/weight

- **Descrição**: Calcula e retorna o peso total de todos os animais em uma fazenda específica, baseado em filtros adicionais.
- **Parâmetros**: `id` - identificador da fazenda.
- **Query Params**: `FarmWeightFilter` - filtra os animais por critérios específicos para cálculo do peso.
- **Comportamento**: Executa `farmService.getTotalWeight(options, id)`, que retorna o peso total dos animais conforme os critérios de filtro aplicados.

### PATCH /farm/:id

- **Descrição**: Atualiza uma fazenda existente.
- **Parâmetros**: `id` - identificador único da fazenda.
- **Corpo da Requisição**: Usa `UpdateFarmDto` para validar e estruturar a entrada de dados para atualização.
- **Comportamento**: Invoca `farmService.update(id, updateFarmDto)` para atualizar a fazenda com os novos dados fornecidos.

### DELETE /farm/:id

- **Descrição**: Remove uma fazenda pelo ID.
- **Parâmetros**: `id` - identificador único da fazenda a ser removida.
- **Comportamento**: Utiliza `farmService.remove(id)` para excluir a fazenda do banco de dados.

## Decoradores e Utilitários

- `@ApiPaginatedResponse`: Decorador customizado que automatiza a documentação de respostas paginadas, especialmente usado no endpoint `GET /farm` para definir o tipo de resposta esperada.
- `@ApiResponse`: Usado no método `GET /farm/:id` para especificar a estrutura da resposta esperada, facilitando a geração de documentação Swagger precisa.


# Módulo Pasture
## PastureController

O `PastureController` gerencia as operações relacionadas a pastagens, utilizando o serviço `PastureService`. Este controlador é categorizado com `@ApiTags('Pasture')` para organização e documentação automatizada através do Swagger.

## Endpoints

### POST /pasture

- **Descrição**: Cria uma nova pastagem.
- **Corpo da Requisição**: Utiliza `CreatePastureDto` para validar e estruturar a entrada de dados.
- **Comportamento**: Chama `pastureService.create(createPastureDto)` para adicionar um novo registro de pastagem no banco de dados.

### GET /pasture

- **Descrição**: Retorna uma lista paginada de pastagens, opcionalmente filtrada por um ID de piquete.
- **Query Params**:
  - `PaginationOptionsQuery`: Parâmetros de paginação, incluindo `limit` e `page`.
  - `picketId` (opcional): Filtra as pastagens associadas a um piquete específico.
- **Comportamento**: Utiliza `pastureService.findAll(options, picketId)` para buscar e retornar pastagens conforme os critérios especificados. A resposta é configurada pelo `@ApiPaginatedResponse(SelectPastureDto)` para documentação da resposta paginada.

### GET /pasture/:id

- **Descrição**: Busca uma pastagem específica pelo ID.
- **Parâmetros**: `id` - identificador único da pastagem.
- **Comportamento**: Chama `pastureService.findOne(id)`, retornando os detalhes da pastagem especificada.

### PATCH /pasture/:id

- **Descrição**: Atualiza uma pastagem existente.
- **Parâmetros**: `id` - identificador único da pastagem.
- **Corpo da Requisição**: Usa `UpdatePastureDto` para validar e estruturar a entrada de dados para atualização.
- **Comportamento**: Invoca `pastureService.update(id, updatePastureDto)` para atualizar a pastagem com os novos dados fornecidos.

### DELETE /pasture/:id

- **Descrição**: Remove uma pastagem pelo ID.
- **Parâmetros**: `id` - identificador único da pastagem a ser removida.
- **Comportamento**: Utiliza `pastureService.remove(id)` para excluir a pastagem do banco de dados.

## Decoradores e Utilitários

- `@ApiPaginatedResponse`: Decorador customizado que automatiza a documentação de respostas paginadas, especialmente usado no endpoint `GET /pasture` para definir o tipo de resposta esperada.
- `@ApiResponse`: Usado no método `GET /pasture/:id` para especificar a estrutura da resposta esperada, facilitando a geração de documentação Swagger precisa.


# Módulo Batch
## BatchController

O `BatchController` gerencia operações relacionadas a lotes (batches), usando o serviço `BatchService`. Ele é decorado com `@ApiTags('Batch')` para categorização na documentação Swagger.

## Endpoints

### POST /batch

- **Descrição**: Cria um novo lote.
- **Corpo da Requisição**: Utiliza `CreateBatchDto` para validar e estruturar a entrada de dados.
- **Comportamento**: Chama `batchService.create(createBatchDto)` para adicionar um novo lote no banco de dados.

### GET /batch

- **Descrição**: Retorna uma lista paginada de lotes, com filtros aplicáveis.
- **Query Params**:
  - `BatchFilter`: Filtra os lotes por critérios especificados.
  - `PaginationOptionsQuery`: Parâmetros de paginação, incluindo `limit` e `page`.
- **Comportamento**: Utiliza `batchService.findAll(BatchFilter, options)` para buscar e retornar lotes conforme os critérios de filtro e opções de paginação fornecidos. A resposta é configurada pelo `@ApiPaginatedResponse(SelectBatchDto)` para documentação da resposta paginada.

### GET /batch/:id

- **Descrição**: Busca um lote específico pelo ID.
- **Parâmetros**: `id` - identificador único do lote.
- **Comportamento**: Chama `batchService.findOne(id)`, retornando os detalhes do lote especificado.

### PATCH /batch/:id

- **Descrição**: Atualiza um lote existente.
- **Parâmetros**: `id` - identificador único do lote.
- **Corpo da Requisição**: Usa `UpdateBatchDto` para validar e estruturar a entrada de dados para atualização.
- **Comportamento**: Invoca `batchService.update(id, updateBatchDto)` para atualizar o lote com os novos dados fornecidos.

### GET /batch/:id/weight

- **Descrição**: Calcula e retorna o peso total dos itens de um lote, usando filtros adicionais.
- **Parâmetros**: `id` - identificador do lote.
- **Query Params**: `FarmWeightFilter` - aplica filtros de peso específicos para o cálculo.
- **Comportamento**: Primeiro obtém os detalhes do lote chamando `batchService.findOne(id)`, e então utiliza `batchService.getTotalWeightBatch(dataFilters, filters)` para calcular o peso total com base nos critérios fornecidos.

### DELETE /batch/:id

- **Descrição**: Remove um lote pelo ID.
- **Parâmetros**: `id` - identificador único do lote a ser removido.
- **Comportamento**: Utiliza `batchService.remove(id)` para excluir o lote do banco de dados.

## Decoradores e Utilitários

- `@ApiPaginatedResponse`: Decorador customizado que automatiza a documentação de respostas paginadas, usado no endpoint `GET /batch`.
- `@ApiResponse`: Usado no método `GET /batch/:id` para especificar a estrutura da resposta esperada, facilitando a geração de documentação Swagger precisa.

# Módulo Picket
## PicketController

O `PicketController` gerencia operações relacionadas a piquetes, utilizando o serviço `PicketService`. Ele é decorado com `@ApiTags('Picket')` para categorização na documentação Swagger.

## Endpoints

### POST /picket

- **Descrição**: Cria um novo piquete.
- **Corpo da Requisição**: Utiliza `CreatePicketDto` para validar e estruturar a entrada de dados.
- **Comportamento**: Chama `picketService.create(createPicketDto)` para adicionar um novo piquete no banco de dados.

### GET /picket

- **Descrição**: Retorna uma lista paginada de piquetes, com filtros aplicáveis.
- **Query Params**:
  - `PaginationOptionsQuery`: Parâmetros de paginação, incluindo `limit` e `page`.
  - `PicketFilter`: Filtra os piquetes por critérios especificados.
- **Comportamento**: Utiliza `picketService.findAll(options, picketFilter)` para buscar e retornar piquetes conforme os critérios de filtro e opções de paginação fornecidos. A resposta é configurada pelo `@ApiPaginatedResponse(SelectPicketDto)` para documentação da resposta paginada.

### GET /picket/:id

- **Descrição**: Busca um piquete específico pelo ID.
- **Parâmetros**: `id` - identificador único do piquete.
- **Comportamento**: Chama `picketService.findOne(id)`, retornando os detalhes do piquete especificado.

### GET /picket/:id/weight

- **Descrição**: Calcula e retorna o peso total dos itens de um piquete, usando filtros adicionais.
- **Parâmetros**: `id` - identificador do piquete.
- **Query Params**: `FarmWeightFilter` - aplica filtros de peso específicos para o cálculo.
- **Comportamento**: Primeiro obtém os detalhes do piquete chamando `picketService.findOne(id)`, e então utiliza `picketService.getTotalWeight(dataFilters, {id, farmId})` para calcular o peso total com base nos critérios fornecidos.

### PATCH /picket/:id

- **Descrição**: Atualiza um piquete existente.
- **Parâmetros**: `id` - identificador único do piquete.
- **Corpo da Requisição**: Usa `UpdatePicketDto` para validar e estruturar a entrada de dados para atualização.
- **Comportamento**: Invoca `picketService.update(id, updatePicketDto)` para atualizar o piquete com os novos dados fornecidos.

### DELETE /picket/:id

- **Descrição**: Remove um piquete pelo ID.
- **Parâmetros**: `id` - identificador único do piquete a ser removido.
- **Comportamento**: Utiliza `picketService.remove(id)` para excluir o piquete do banco de dados.

### PUT /picket/animals

- **Descrição**: Atualiza os animais associados a um piquete.
- **Corpo da Requisição**: Utiliza `UpdatePicketAnimalDto` para validar e estruturar a entrada de dados.
- **Comportamento**: Chama `picketService.updatePicketAnimals(updatePicketAnimalDto)` para atualizar a lista de animais associados ao piquete.

## Decoradores e Utilitários

- `@ApiPaginatedResponse`: Decorador customizado que automatiza a documentação de respostas paginadas, usado no endpoint `GET /picket`.
- `@ApiResponse`: Usado no método `GET /picket/:id` para especificar a estrutura da resposta esperada, facilitando a geração de documentação Swagger precisa.
