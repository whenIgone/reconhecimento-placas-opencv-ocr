## 📐 Arquitetura do Pipeline

## 1️⃣ **Filtro de Região de Interesse (ROI)**
- Restringe busca para **40-90% da altura da imagem** (onde placas tipicamente aparecem)
- Reduz falsos positivos e custo computacional

## 2️⃣ **Pré-processamento Multi-Branch**

Original → Bilateral Filter (suavização preservando bordas)
         → Grayscale
         → Threshold Adaptativo (normal + inverso para placas escuras)
         → Otsu Binarization
         → Canny Edge Detection
         → Morfologia Horizontal (conectar caracteres) + Opening (limpar ruído)

## 3️⃣ Detecção de Candidatos (Múltiplas Estratégias)

### Estratégia A — Por Contornos
- `cv2.findContours` na imagem binarizada após morfologia.
- Filtros: área (800–800k px), dimensões (80–400 × 20–120 px), aspect ratio (2.5–5.5).

### Estratégia B — Por Bordas
- Dilatar bordas Canny para formar regiões conexas.
- Mesmos filtros dimensionais.

### Estratégia C — Por Componentes Conectados
- `cv2.connectedComponentsWithStats` para análise estatística de componentes.
- Mesmos filtros + verificação de características de texto.

## 4️⃣ Validação de Características de Texto
Para cada candidato:
- Densidade de pixels brancos (15–85% para evitar regiões vazias/saturadas).
- Perfil horizontal de projeção (mínimo 3 picos indicando caracteres).

## 5️⃣ Scoring e Ranking
Cada candidato recebe score baseado em:
- Proximidade de aspect ratio ideal (~3.5:1): 30%.
- Proximidade de área ideal (~10k px): 20%.
- Densidade de texto ideal (30–70%): 30%.
- Variância normalizada do perfil horizontal: 20%.

## 6️⃣ Deduplicação e Filtragem
- Remoção de candidatos sobrepostos via IoU > 0.5.
- Mantém candidato com maior score.
- Ordenação decrescente por qualidade.

## 7️⃣ OCR Preliminar (Validação)
Para candidatos top-ranqueados:
- OCR rápido com Tesseract (config: PSM 8, whitelist alfanumérica).
- OCR rápido com EasyOCR (threshold confiança 0.3).
- Validação de formato:
  - 5–8 caracteres (7 ideal).
  - Mínimo 2 letras + 2 números.
  - Padrão BR: ABC1234 ou ABC1D23 (Mercosul).

## 8️⃣ OCR Completo (Placas Validadas)
Para placas que passaram validação preliminar:
- Tesseract: 3 estratégias (original, resize 2x, denoise).
- EasyOCR: configuração otimizada (width_ths, height_ths).
- Retorna texto mais longo.

## 9️⃣ Pós-processamento
- Whitelist: `ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789`.
- Correções comuns de OCR:
  - Confusões letra/número: O↔0, I↔1, L↔1, Z↔2, S↔5, B↔8, etc.
- Formatação: `ABC1234` → `ABC-1234`.

