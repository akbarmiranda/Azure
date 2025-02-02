# Azure
# Modelo de Previsão de Aluguel de Bicicletas com Azure ML

## Visão Geral do Projeto
Este projeto implementa uma solução de machine learning automatizado utilizando o Azure Machine Learning para prever a demanda de aluguel de bicicletas. O modelo utiliza dados históricos de aluguel para prever volumes esperados de locação com base em várias características ambientais e temporais, demonstrando a aplicação prática de machine learning em previsão de demanda e otimização de recursos.

## Arquitetura Técnica
- **Plataforma**: Azure Machine Learning
- **Tipo de Modelo**: Regressão
- **Algoritmos**: RandomForest e LightGBM
- **Implantação**: Endpoint de inferência em tempo real via Azure Container Instance
- **Métrica de Desempenho**: Erro Quadrático Médio da Raiz Normalizada (NRMSE)

## Processo de Implementação

### 1. Configuração do Ambiente
1. Criação do workspace do Azure Machine Learning
2. Configuração dos recursos necessários:
   - Conta de Armazenamento
   - Key Vault
   - Application Insights
   - Registro de Container

### 2. Preparação dos Dados
- Utilização do dataset histórico de aluguel de bicicletas com as seguintes características:
  - Temporais: dia, mês, ano, estação
  - Ambientais: temperatura, umidade, velocidade do vento
  - Categóricas: feriado, dia da semana, dia útil
  - Variável Alvo: quantidade de aluguéis

### 3. Desenvolvimento do Modelo
Implementação do job de ML Automatizado com as seguintes especificações:

```yaml
Tipo de Tarefa: Regressão
Métrica Principal: ErroQuadráticoMédioRaizNormalizado
Algoritmos: 
  - RandomForest
  - LightGBM
Computação: 
  Tipo: Serverless
  VM: Standard_DS3_V2
Parâmetros de Treinamento:
  - Máximo de trials: 3
  - Máximo de trials concorrentes: 3
  - Máximo de nodes: 3
  - Divisão de Validação: 10%
```

### 4. Implantação do Modelo
- Implantação do melhor modelo como endpoint em tempo real
- Configuração:
  - Tipo de Endpoint: Inferência em tempo real
  - Infraestrutura: Azure Container Instance
  - Escalonamento: 3 instâncias para alta disponibilidade
  - Monitoramento: Application Insights habilitado

### 5. Testes e Validação
Teste bem-sucedido do endpoint com dados de exemplo:
```json
{
  "input_data": {
    "columns": ["day", "mnth", "year", "season", "holiday", "weekday", "workingday", "weathersit", "temp", "atemp", "hum", "windspeed"],
    "data": [[1,1,2022,2,0,1,1,2,0.3,0.3,0.3,0.3]]
  }
}
```

## Valor para o Negócio e Aplicações
Este modelo de previsão oferece valor significativo para empresas de compartilhamento de bicicletas:
- **Previsão de Demanda**: Previsão precisa da demanda de aluguel permitindo gestão otimizada da frota
- **Otimização de Recursos**: Melhor alocação de bicicletas em diferentes locais
- **Redução de Custos**: Minimização de custos operacionais através da distribuição eficiente de recursos
- **Satisfação do Cliente**: Melhor disponibilidade de serviço durante períodos de pico de demanda

## Insights Técnicos

### Seleção do Modelo
O processo de ML automatizado avaliou múltiplos algoritmos, focando em RandomForest e LightGBM por suas características:
- Tratamento robusto de features numéricas e categóricas
- Excelente performance em dados tabulares
- Capacidades nativas de importância de features
- Forte resistência a overfitting

### Otimização de Performance
- Implementação de early stopping com threshold de pontuação métrica de 0.085 NRMSE
- Utilização de divisão treino-validação para avaliação robusta do modelo
- Configuração de trials concorrentes para utilização eficiente de recursos

## Melhorias Futuras
1. Integração com API de clima para dados meteorológicos em tempo real
2. Implementação de framework de testes A/B
3. Adição de pipeline de engenharia de features
4. Expansão do modelo para incluir componentes espaciais

## Gestão de Recursos
Para manter a eficiência de custos:
- Exclusão do endpoint quando não em uso
- Gestão adequada do grupo de recursos
- Monitoramento da utilização de recursos computacionais

## Conclusão
Esta implementação demonstra a aplicação prática do Azure ML na resolução de problemas reais de negócios através de machine learning automatizado e soluções de implantação escaláveis. A arquitetura do modelo garante tanto precisão quanto eficiência operacional, tornando-o uma ferramenta valiosa para otimização de serviços de compartilhamento de bicicletas.