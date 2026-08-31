# Documentação técnica

## 1. Arquitetura e arquivos

O projeto é um notebook Jupyter de classificação binária com PyTorch. Não há servidor web nem etapa de deploy: a execução ocorre no kernel local do notebook.

- `Classificacao_Gato_vs_Cachorro_CNN.ipynb`: configuração, seleção de imagens, DataLoaders, CNN, treinamento e gráficos.
- `README.md`: apresentação e roteiro de primeira execução.
- `.gitignore`: exclui ambientes locais, dataset, splits e pesos do versionamento.
- `PetImages/Cat/` e `PetImages/Dog/`: imagens fornecidas separadamente pelo usuário.
- `output_5173/`: diretório gerado; contém `treino/`, `teste/`, `melhor_modelo.pth`, `modelo_ra5173.pth` e `grafico_RA5173156.png` quando produzidos pela execução.

## 2. Fluxo e configuração

Execute as células em ordem. A primeira inicializa imports, constantes e sementes; depois o notebook prepara os splits, verifica contagens, cria datasets e modelo, treina ou carrega pesos e apresenta os gráficos.

As dependências importadas são PyTorch, torchvision, Pillow, Matplotlib, Pandas e IPython, além da biblioteca padrão. É necessário um ambiente que execute notebooks Jupyter. O repositório não fixa versões dessas dependências.

A primeira célula concentra os campos editáveis: `RECRIAR_SPLITS`, `TREINAR`, `OUTPUT_DIR`, `IMG_SIZE`, `BATCH_SIZE`, `NUM_EPOCHS`, `LR`, `WEIGHT_DECAY`, `DROPOUT` e `PATIENCE`.

O RA e `RA_4DIG` são constantes independentes: alterar `RA` não recalcula automaticamente `RA_4DIG`, o nome da pasta nem os textos dos gráficos. Os valores atuais usam 5173 para treino e `ceil(5173 * 0.30) = 1552` para teste.

A seleção de dispositivo usa CUDA quando `torch.cuda.is_available()` retorna verdadeiro; caso contrário, usa CPU. No Windows, os DataLoaders usam zero workers.

## 3. Diagnóstico

| Sintoma | Verificação e ação |
| --- | --- |
| `ModuleNotFoundError` | Confira se as dependências estão instaladas no mesmo ambiente Python do kernel selecionado. |
| `PetImages incompleto` | Confira `Cat/` e `Dog/` dentro de `./PetImages` ou `../PetImages`, relativos ao diretório de trabalho do kernel. O código também tenta o caminho Windows da máquina original. |
| `Splits nao encontrados` | Na primeira célula, configure `RECRIAR_SPLITS = True`, execute-a novamente e depois execute a preparação. Essa ação substitui as pastas de treino e teste dentro da saída configurada. |
| `Apenas ... imagens validas` | A contagem de arquivos não garante imagens válidas. Confira a integridade e a quantidade das imagens; a coleta usa Pillow para verificá-las. |
| `Nenhum modelo salvo` | Configure `TREINAR = True` na primeira célula e execute as células em ordem para produzir os pesos. |
| Gráficos de uma execução anterior | Sem treinamento na sessão, o notebook tenta exibir o PNG salvo. Pesos carregados não restauram as listas de métricas. |
| Erro ao carregar pesos após mudar a arquitetura | Use pesos compatíveis com a arquitetura atual ou realize novo treinamento. Preserve os pesos antigos separadamente se precisar deles. |

## 4. Limitações dos resultados

O split chamado `teste` é usado como validação para o scheduler, o early stopping e a escolha do melhor modelo. Ele não constitui uma avaliação final independente.

As chamadas de `criar_split` sorteiam imagens de treino e teste separadamente a partir da mesma origem, sem excluir as já escolhidas. Portanto, pode haver sobreposição entre os conjuntos. Antes de interpretar a acurácia como generalização, verifique essa interseção e implemente uma separação sem sobreposição. Esta documentação não altera o algoritmo.

Quando não há métricas de treinamento na sessão, a análise final imprime valores históricos fixos no código. Esses números não são uma medição do modelo carregado na execução atual.

## 5. Segurança e alterações rápidas

- Mantenha datasets e checkpoints locais; as regras existentes do Git ignoram esses artefatos.
- Use apenas checkpoints de origem confiável. O carregamento atual utiliza `weights_only=True`.
- Não armazene arquivos pessoais em `OUTPUT_DIR/treino` ou `OUTPUT_DIR/teste`: recriar splits remove essas pastas.
- Para mudar o destino dos resultados, ajuste `OUTPUT_DIR` antes de executar a primeira célula.
- Para reutilizar splits e pesos, deixe ambas as flags falsas após uma execução bem-sucedida.

## 6. Histórico

- Corrigido no README o significado de 5173 e documentada a primeira execução.
- Adicionado este guia com estrutura, configuração, diagnóstico e limites de interpretação, conferidos diretamente no código do notebook.
