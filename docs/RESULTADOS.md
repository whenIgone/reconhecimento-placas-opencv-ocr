## 📊 Resultados e Limitações

### ✅ Sucessos
- Detecção funciona bem em imagens com boa iluminação, placa frontal e alta resolução.
- Filtros heurísticos reduzem falsos positivos (carros inteiros, janelas, rodas).
- Duplo OCR melhora robustez (EasyOCR para fontes distorcidas, Tesseract para texto limpo).
- Pré-processamento adaptativo captura placas claras e escuras.

### ⚠️ Limitações Conhecidas
- Dependência de qualidade da imagem: placas pequenas (<80px largura), muito anguladas ou desfocadas falham.
- Erros de OCR persistentes: confusões entre caracteres similares (O/0, I/1, B/8) mesmo com correções.
- Sem deep learning: CNN/YOLO teria melhor generalização e robustez a variações.
- Threshold fixo: parâmetros de binarização/morfologia não se adaptam automaticamente.
- Sem tratamento de movimento: imagens com motion blur sofrem degradação.

### 📈 Taxa de Sucesso Observada
Em testes controlados (imagens do Google Drive, sem movimento, iluminação razoável):
- Detecção de ROI: ~70–80% das placas identificadas como candidatos.
- Leitura correta completa: ~40–60% (varia com qualidade e ângulo).
- Leitura parcial útil: ~70% (pelo menos 5 de 7 caracteres corretos).

**Principais causas de falha:**
- Placa muito pequena na imagem (<100px).
- Ângulo oblíquo severo (>45°).
- Reflexo/brilho saturando caracteres.
- Baixa resolução geral da imagem.
