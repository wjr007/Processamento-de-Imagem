# Classificação Gato vs Cachorro com CNN

**Aluno:** Walteir Luiz de Morais Junior  
**RA:** 5173156  
**Disciplina:** Python Aplicado à Inteligência Artificial

---

## Descrição do Projeto

Este trabalho implementa uma **Rede Neural Convolucional (CNN)** para classificação binária de imagens: **gatos** vs **cachorros**. O projeto utiliza o dataset PetImages e segue uma arquitetura simples e eficiente, com técnicas modernas de regularização e otimização.

---

## Arquitetura da Rede

### CNN_Simples
- **Entrada:** Imagens 128×128 RGB
- **Camadas Convolucionais:** 3 blocos (Conv → BatchNorm → ReLU → MaxPool)
  - Conv1: 3 → 32 filtros
  - Conv2: 32 → 64 filtros
  - Conv3: 64 → 128 filtros
- **Fully Connected Layers:** 128×16×16 → 512 → 2 (Cat/Dog)
- **Regularização:** Dropout (40%), BatchNorm, Weight Decay (L2)

---

## Dataset e Splits

### Dimensionamento Automático via RA
- **RA:** 5173156
- **4 primeiros dígitos:** 5173
- **N_TRAIN:** 5173 imagens (metade de cada classe)
- **N_TEST:** 1552 imagens (30% arredondado para cima)
- **Distribuição:** Balanceada entre gatos e cachorros

### Organização
```
output_5173/
├── treino/
│   ├── Cat/ (2586 imagens)
│   └── Dog/ (2587 imagens)
└── teste/
    ├── Cat/ (776 imagens)
    └── Dog/ (776 imagens)
```

---

## Técnicas de Treinamento

### 1. **Data Augmentation**
- Flip horizontal aleatório (50% chance)
- Rotação (±15°)
- Variação de brilho e contraste

### 2. **Otimização**
- **Otimizador:** Adam (LR=1e-3, Weight Decay=1e-4)
- **Scheduler:** ReduceLROnPlateau (reduz LR quando validação estagna)
- **Early Stopping:** Patience=5 épocas

### 3. **Regulação**
- **Dropout:** 40% após fully connected
- **BatchNorm:** Após cada convolução
- **Weight Decay:** L2 regularization

---

## Hiperparâmetros

| Parâmetro | Valor | Justificativa |
|-----------|-------|---------------|
| IMG_SIZE | 128 px | Preserva detalhes com ~5k imagens |
| BATCH_SIZE | 32 | Estável para este volume de dados |
| NUM_EPOCHS | 20 | Dataset maior converge mais devagar |
| DROPOUT | 0.40 | Regularização contra overfitting |
| WEIGHT_DECAY | 1e-4 | Penalização L2 dos pesos |
| LR Scheduler | ReduceOnPlateau | Reduz taxa de aprendizado dinamicamente |
| Early Stopping | patience=5 | Evita treinar além do ponto ótimo |

---

## Resultados Esperados

### Métricas
- **Acurácia Treino:** ~78-85%
- **Acurácia Validação:** ~80-86%
- **Diferença (T-V):** < 10% (indica bom treinamento)

### Gráficos
- **Acurácia por Época:** Mostra convergência de treino e validação
- **Perda por Época:** Demonstra redução de loss ao longo do treinamento
- Título com RA e estatísticas do dataset

---

## Como Usar

### 1. Preparação

Abra `Classificacao_Gato_vs_Cachorro_CNN.ipynb` em um ambiente Jupyter com as bibliotecas listadas em **Tecnologias** instaladas. Inicie o notebook a partir da pasta do projeto.

O dataset não acompanha o repositório. Descompacte `PetImages` na raiz do projeto (ou na pasta imediatamente acima), preservando as subpastas `Cat/` e `Dog/`. O caminho Windows abaixo é uma alternativa específica da máquina original:

```python
# Descompacte o dataset
# C:\Users\jrdev\Downloads\PetImages\
#   ├── Cat/
#   └── Dog/
```

### 2. Flags de Configuração
```python
RECRIAR_SPLITS = True    # Primeira execução: cria treino/ e teste/
TREINAR = True           # Primeira execução: treina e salva os pesos
```

### 3. Execução
- Ajuste as flags na primeira célula **antes** de executá-la.
- Execute as células em ordem, da inicialização até a análise final.
- Confira as contagens: 5173 imagens de treino e 1552 de teste.
- O treinamento salva pesos e gráficos em `output_5173/`; o tempo depende do hardware.
- Nas próximas sessões, use `RECRIAR_SPLITS = False` para reutilizar as imagens já separadas e `TREINAR = False` para carregar `melhor_modelo.pth`, se ele existir.

**Atenção:** `RECRIAR_SPLITS = True` apaga e recria as pastas `treino/` e `teste/` dentro de `OUTPUT_DIR`. Não guarde arquivos pessoais nessas pastas. Carregar pesos não recupera o histórico de métricas por época; o notebook pode exibir o gráfico salvo anteriormente.

---

## Estrutura de Branches

- **master** — Código principal estável
- **backup** — Versão de backup

---

## Requisitos Atendidos

✓ **Req 1:** DataLoaders com shuffle e augmentation  
✓ **Req 2:** Modelo CNN com convoluções e pooling  
✓ **Req 3:** Treinamento com otimização e validação  
✓ **Req 4:** Tabela de parâmetros alterados  
✓ **Req 5:** Prints de treino/teste  
✓ **Req 6:** Gráficos com título = RA  
✓ **Req 7:** Análise de resultados  
✓ **Req 8:** Conclusão sobre generalização  

---

## Tecnologias

- **Python 3.x**
- **PyTorch** — Deep Learning framework
- **torchvision** — Transformações e utilitários
- **PIL/Pillow** — Processamento de imagens
- **Matplotlib** — Visualização
- **Pandas** — Tabelas e análise

---

## Autor

**Walteir Luiz de Morais Junior**  
RA: 5173156

---

## Licença

Este projeto é de uso acadêmico apenas.
