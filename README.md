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

