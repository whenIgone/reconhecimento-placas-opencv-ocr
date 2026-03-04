# Reconhecimento e Leitura de Placas com OpenCV + OCR

**Pipeline de Visão Computacional clássico + OCR para detecção e leitura de placas veiculares brasileiras**  
Projeto acadêmico desenvolvido no Google Colab explorando técnicas de pré-processamento, detecção por múltiplas estratégias e validação com duplo OCR.

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![OpenCV](https://img.shields.io/badge/OpenCV-4.x-green.svg)](https://opencv.org/)
[![Tesseract](https://img.shields.io/badge/Tesseract-OCR-orange.svg)](https://github.com/tesseract-ocr/tesseract)
[![EasyOCR](https://img.shields.io/badge/EasyOCR-1.7+-yellow.svg)](https://github.com/JaidedAI/EasyOCR)

---

## 📋 Sobre o Projeto

Este projeto implementa um **sistema de ALPR (Automatic License Plate Recognition)** educacional focado em:
- Detectar regiões de placas veiculares em imagens usando **múltiplas estratégias de Visão Computacional clássica**
- Extrair texto das placas usando **dois motores de OCR** (Tesseract + EasyOCR)
- Validar resultados com **heurísticas específicas para placas brasileiras** (formato ABC1234 e Mercosul ABC1D23)

**Contexto acadêmico**: Protótipo desenvolvido para aprendizado de técnicas de pré-processamento de imagens, detecção de ROI e OCR. **Não é uma solução production-ready** — os resultados variam com qualidade da imagem, ângulo, iluminação e resolução.

---

## 🎯 Objetivos de Aprendizado

- Implementar pipeline completo de **Visão Computacional** (pré-processamento → detecção → validação)
- Explorar **múltiplas estratégias de detecção** (contornos, bordas, componentes conectados)
- Comparar **motores de OCR** (Tesseract vs EasyOCR) com pós-processamento
- Aplicar **filtros heurísticos rigorosos** para reduzir falsos positivos
- Entender **trade-offs** entre sensibilidade e precisão em detecção de objetos

---

## 🛠️ Stack Tecnológica

| Componente | Tecnologia | Propósito |
|------------|------------|-----------|
| **Processamento de Imagem** | OpenCV 4.x | Pré-processamento, detecção de contornos/bordas, morfologia |
| **OCR** | Tesseract + EasyOCR | Extração de texto com configuração específica para placas |
| **Ambiente** | Google Colab | Desenvolvimento e testes |
| **Linguagem** | Python 3.8+ | Implementação do pipeline |
| **Visualização** | Matplotlib | Análise de resultados e debug |

## 🚀 Como Executar

### Pré-requisitos
```bash
# Python 3.8+
pip install opencv-python opencv-contrib-python
pip install easyocr pytesseract
pip install matplotlib numpy

# Tesseract OCR (sistema)
# Ubuntu/Debian:
sudo apt-get install tesseract-ocr tesseract-ocr-por libtesseract-dev

# macOS:
brew install tesseract

# Windows: baixar instalador em https://github.com/UB-Mannheim/tesseract/wiki
```
### Executar no Google Colab
- Abra reconhecimento_placas.ipynb no Google Colab
- Monte o Google Drive: drive.mount('/content/drive')
- Crie pasta /content/drive/MyDrive/placas_veiculos/
- Faça upload de imagens de teste (com placas visíveis)
- Execute todas as células

### Executar localmente
```bash
git clone https://github.com/seu-usuario/reconhecimento-placas-opencv-ocr.git
cd reconhecimento-placas-opencv-ocr
pip install -r requirements.txt

# Edite reconhecimento_placas.py:
# - Mude pasta_imagens para seu diretório local
# - Comente linhas do Google Drive (drive.mount, etc.)

python reconhecimento_placas.py
```

## 📚 Estrutura do Código
Classe Principal: SistemaReconhecimentoPlacasMelhorado
### Métodos públicos:
processar_imagem(caminho) → Processa imagem completa e retorna dict com resultados

### Métodos internos:
- filtrar_regiao_interesse() → ROI mask
- preprocessar_para_placas() → Pipeline de pré-processamento
- detectar_placas_melhorado() → Orquestra 3 estratégias de detecção
- _detectar_por_contornos/bordas/componentes() → Detectores específicos
- _tem_caracteristicas_texto() → Validação de candidatos
- _calcular_score_placa() → Scoring de qualidade
- _filtrar_placas_candidatas() → Deduplicação por IoU
- _validar_com_ocr_preliminar() → OCR rápido para validação
- _ocr_tesseract_completo/easyocr_completo() → OCR detalhado
- _pos_processar_texto() → Limpeza e correções

## 🎓 Aprendizados Técnicos

### O que funcionou bem
- Morfologia horizontal (kernel 3×1) é eficaz para conectar caracteres antes da detecção
- Threshold adaptativo inverso captura placas escuras que Otsu perderia
- Múltiplas estratégias de detecção aumentam recall (captura mais placas verdadeiras)
- IoU para deduplicação remove 90% dos candidatos duplicados
- Validação por OCR preliminar reduz processamento desnecessário

### O que precisa melhorar
- Detecção de ângulo e correção de perspectiva (warpPerspective) antes do OCR
- Segmentação adaptativa de caracteres (não depender 100% de OCR black-box)
- Ensemble de OCR com votação ponderada por confiança
- Modelo de linguagem para corrigir sequências improváveis (ex: "0O0O" → "0000")
- Fine-tuning de Tesseract com dataset brasileiro de placas

## 🔄 Próximos Passos
- Implementar correção de perspectiva (detecção de 4 cantos da placa)
- Adicionar segmentação de caracteres individuais
- Treinar modelo YOLO/Faster R-CNN para detecção robusta
- Fine-tuning de OCR com dataset brasileiro
- Pipeline de ensemble OCR com votação
- Suporte para vídeo (rastreamento temporal para aumentar acurácia)
- API REST para inferência
- Benchmark quantitativo com dataset público (OpenALPR, etc.)

## ⚖️ Privacidade e Uso Ético
- ⚠️ Dataset de teste não incluído: imagens de placas não foram consentidas, portanto não estão no repositório
- ⚠️ Uso educacional: este código é para aprendizado de técnicas de CV/OCR, não para vigilância ou uso comercial
- ⚠️ Conformidade legal: reconhecimento de placas pode ser regulado por leis de privacidade (LGPD, GDPR) — use responsavelmente

## 📖 Referências
- [OpenCV Documentation - Contour Detection](https://docs.opencv.org/4.x/d4/d73/tutorial_py_contours_begin.html)
- [Tesseract OCR - Improving Output Quality](https://tesseract-ocr.github.io/tessdoc/ImproveQuality.html)
- [EasyOCR GitHub](https://github.com/JaidedAI/EasyOCR)
- [Morphological Transformations - OpenCV](https://docs.opencv.org/4.x/d9/d61/tutorial_py_morphological_ops.html)
- [Adaptive Thresholding](https://docs.opencv.org/4.x/d7/d4d/tutorial_py_thresholding.html)

### 👤 Autor
Lucas Rodrigues Ferreira

📧 lucas.rferreira@gmail.com
💼 [LinkedIn](https://www.linkedin.com/in/lucasrodriguesti2026/)

### 📄 Licença
Este projeto está sob a licença MIT. Veja o arquivo LICENSE para mais detalhes.

### 🙏 Agradecimentos
- Projeto desenvolvido com Rhyan Morais de Azevedo e Kayky Paulino Alves dos Santos
- Google Colab por fornecer ambiente GPU para testes
- Comunidades OpenCV e Tesseract OCR
