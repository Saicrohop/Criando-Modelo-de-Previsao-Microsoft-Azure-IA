# Criando-Modelo-de-Previsao-Microsoft-Azure-IA
Este guia descreve como criar, treinar e implantar um modelo de previsão diretamente pelo Portal do Azure utilizando Azure Machine Learning, realizado como exercício parte do DIO Bootcamp Microsoft IA-900.
# Configuração do Workspace do Azure ML e Treinamento de Modelo de Predição

Este guia descreve como configurar um workspace no Azure Machine Learning, criar um experimento de aprendizado de máquina automatizado, avaliar o modelo, implantá-lo e testá-lo.

---

## 1. Configurar o Workspace do Azure ML

### Acesse o Portal do Azure:
1. Faça login no [Portal do Azure](https://portal.azure.com/).

### Crie um Workspace do Azure ML:
1. No menu esquerdo, clique em **Criar um recurso**.
2. Pesquise por **Machine Learning** e selecione **Workspace Machine Learning**.
3. Selecione a **Assinatura**.
4. Crie um **Grupo de Recursos**.
5. Preencha os detalhes:
   - **Nome**: `workspace-ml-predicao`
   - **Grupo de Recursos**: Crie um novo ou escolha um existente.
   - **Região**: Escolha a mesma região dos seus dados.
6. Clique em **Revisar + Criar** e confirme.

---

## 2. Criar e Configurar um Experimento

### Abra o Workspace:
1. Após a criação, acesse o **workspace** e clique em **Azure Machine Learning Studio** para abrir o estúdio no navegador.

### Prepare os Dados:
1. Vá para a aba **Dados** e clique em **+ Criar**.
2. Insira **Nome** e **Descrição** nos respectivos campos e escolha o tipo de dados (ex: `urifile`, `mltable`, `Tabular`, `Arquivo`, `JSON`, `CSV`).
3. Clique em **Avançar**.
4. Faça o **upload** do arquivo de entrada (o JSON fornecido), conecte a um armazenamento existente ou forneça um link da Web.
5. Verifique e registre o dado com um nome, como `bike-rentals`.

### Use Aprendizado de Máquina Automatizado:
1. No **Azure Machine Learning Studio**, vá para a página **ML Automatizado** (em **Criação**).
2. Crie um novo trabalho de ML Automatizado com as seguintes configurações:

#### Configurações básicas:
- **Nome do trabalho**: Nome gerado automaticamente.
- **Novo nome do experimento**: `mslearn-bike-rental`
- **Descrição**: Aprendizado de máquina automatizado para previsão de aluguel de bicicletas.
- **Tags**: Nenhuma.

#### Tipo de Tarefa e Dados:
1. **Selecione o tipo de tarefa**: **Regressão**.
2. **Criar um novo conjunto de dados** com as configurações:
   - **Nome**: `bike-rentals`
   - **Descrição**: Histórico de aluguel de bicicletas.
   - **Tipo**: Tabela (`mltable`).
   - **Fonte de dados**: **De arquivos locais**.
   - **Tipo de armazenamento de destino**: **Azure Blob Storage**.
   - **Nome do armazenamento**: `workspaceblobstore`.
   - **Seleção de MLTable**:
     - Baixe e descompacte a pasta que contém os arquivos.
   - **Selecione Criar**.
3. Após a criação do conjunto de dados, selecione **bike-rentals** para continuar o envio do trabalho de ML automatizado.

---

## 3. Avaliar o Modelo
1. Na guia **Visão Geral** do trabalho de aprendizado de máquina automatizado, observe o **resumo do melhor modelo**.
2. Selecione o nome do algoritmo para visualizar seus detalhes.
3. Na guia **Métricas**, selecione os gráficos **residuais** e **prediction_true** para avaliar o desempenho do modelo.

---

## 4. Implantar o Modelo

### Criar um Endpoint:
1. Na aba **Implantações**, clique em **+ Criar Ponto de Extremidade**.
2. Escolha **Ação de Inferência em Tempo Real**.
3. Preencha os detalhes:
   - **Máquina virtual**: `Standard_DS3_v2`
   - **Contagem de instâncias**: `3`
   - **Ponto final**: Novo.
   - **Nome do ponto de extremidade**: Mantenha o padrão ou escolha um nome único.
   - **Nome da implantação**: Mantenha o padrão.
   - **Inferência de coleta de dados**: **Desativado**.
   - **Modelo de pacote**: **Desativado**.

### Implantação:
1. Aguarde até que o status **Deploy** mude para `Succeeded`. Esse processo pode levar de 5 a 10 minutos.

---

## 5. Testar o Endpoint

1. No **Azure Machine Learning Studio**, no menu à esquerda, selecione **Endpoints** e abra o endpoint **predict-rentals**.
2. Na página do endpoint em tempo real do `predict-rentals`, visualize a guia **Teste**.
3. No painel **Dados de entrada**, substitua o JSON do modelo pelos seguintes dados de entrada:

```json
{
  "input_data": {
    "columns": [
      "day",
      "mnth",
      "year",
      "season",
      "holiday",
      "weekday",
      "workingday",
      "weathersit",
      "temp",
      "atemp",
      "hum",
      "windspeed"
    ],
    "index": [0],
    "data": [[1,1,2022,2,0,1,1,2,0.3,0.3,0.3,0.3]]
  }
}
```
## Verifique o resultado esperado, que será algo como:
```json
{
[361.41500445154400]
}
```
