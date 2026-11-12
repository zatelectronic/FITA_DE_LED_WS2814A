<div align="center">

<img src="imagem_1.png" alt="WS2814A — Barra de LEDs RGBW Endereçáveis" width="800"/>

# WS2814A

**Barra de LEDs endereçáveis RGBW com 10 pixels WS2814C — protocolo de 1 fio, encadeável, totalmente SMD, alimentação 5V**

[![KiCad](https://img.shields.io/badge/KiCad-10-blue?logo=kicad&logoColor=white)](https://www.kicad.org/)
[![LEDs](https://img.shields.io/badge/LEDs-10%20pixels%20RGBW-yellow)](.)
[![Protocolo](https://img.shields.io/badge/Protocolo-1--Wire%20WS2814-orange)](.)
[![Tensão](https://img.shields.io/badge/Alimenta%C3%A7%C3%A3o-5V-green)](.)
[![Montagem](https://img.shields.io/badge/Montagem-SMD-blueviolet)](.)
[![ZAT ELECTRONIC](https://img.shields.io/badge/Feito%20por-ZAT%20ELECTRONIC-black)](.)
[![Made in Brazil](https://img.shields.io/badge/Made%20in-Brazil%20%F0%9F%87%A7%F0%9F%87%B7-009c3b)](.)

</div>

---

## 📋 Índice

- [WS2814A](#ws2814a)
  - [📋 Índice](#-índice)
  - [Visão Geral](#visão-geral)
  - [Renders 3D](#renders-3d)
  - [Funcionalidades](#funcionalidades)
  - [Arquitetura do Pixel](#arquitetura-do-pixel)
  - [Especificações Técnicas](#especificações-técnicas)
  - [Lista de Materiais](#lista-de-materiais)
  - [Pinagem dos Conectores](#pinagem-dos-conectores)
    - [J1 — Entrada (lado esquerdo)](#j1--entrada-lado-esquerdo)
    - [J2 — Saída (lado direito)](#j2--saída-lado-direito)
  - [Encadeamento de Barras](#encadeamento-de-barras)
  - [Aplicações](#aplicações)
  - [Estrutura do Repositório](#estrutura-do-repositório)
  - [Como Usar](#como-usar)
    - [Conexão básica com Arduino / ESP32](#conexão-básica-com-arduino--esp32)
    - [Exemplo com Adafruit NeoPixel](#exemplo-com-adafruit-neopixel)
  - [Sobre](#sobre)

---

## Visão Geral

**WS2814A** é uma barra de LEDs endereçáveis **RGBW** com **10 pixels** individuais, projetada pela **ZAT ELECTRONIC** utilizando **KiCad 10**. Cada pixel é composto por um controlador **WS2814C** (SMD) e um LED **QBLP679-RGBCW** (RGBW — 4 canais: Vermelho, Verde, Azul e Branco), permitindo controle independente de cor e intensidade por pixel.

A comunicação utiliza o protocolo **single-wire (1 fio)** compatível com a família WS28xx, suportado por bibliotecas com modo RGBW, como a **Adafruit NeoPixel**. A barra possui conector de **entrada (DIN)** e **saída (DOUT)** em ambas as extremidades, permitindo **encadeamento de múltiplas barras** em série com um único pino de dados do microcontrolador.

> 💡 O canal **branco dedicado (W)** do LED QBLP679-RGBCW proporciona iluminação branca de alta qualidade sem mistura de cores RGB, ideal para aplicações de iluminação ambiente e fotografia.

---

## Renders 3D

<div align="center">

<img src="imagem_1.png" alt="Vista Superior" width="700"/>

*Vista superior — 10 pixels RGBW em série, controladores WS2814C e capacitores de desacoplamento*

<br/>

<img src="imagem_2.png" alt="Vista Isométrica" width="700"/>

*Vista isométrica — layout compacto SMD com conectores de entrada e saída nas extremidades*

</div>

---

## Funcionalidades

- ✅ **10 pixels RGBW endereçáveis** — controle individual de cor e brilho por pixel
- ✅ **Controlador WS2814C** (SMD) por pixel — protocolo single-wire compatível com WS28xx
- ✅ **LED QBLP679-RGBCW** por pixel — RGBW com canal branco dedicado (4 canais)
- ✅ **Protocolo 1 fio** — família WS28xx, quadro de 32 bits por pixel (R, G, B, W); compatível com Adafruit NeoPixel (NEO_RGBW)
- ✅ **Encadeável (DIN → DOUT)** — múltiplas barras em série com 1 único pino de dados
- ✅ **Conector de entrada J1** (5V / DIN / GND) — lado esquerdo da barra
- ✅ **Conector de saída J2** (5V / DOUT / GND) — lado direito para encadeamento
- ✅ **Filtro RC no VDD de cada WS2814C** — R1–R10 100 Ω + C1–C10 100 nF (0603 SMD)
- ✅ **3 pontos fiduciais** (FID1–FID3) — referência para montagem SMD automatizada
- ✅ **Alimentação 5VDC** — compatível com fontes USB e reguladores padrão
- ✅ **Layout totalmente SMD** — compacto e adequado para fabricação automatizada

---

## Arquitetura do Pixel

Cada um dos 10 pixels da barra é composto por:

```
   +5V ──[R_n 100Ω]──┬──► VDD  WS2814C (U_n)            +5V
                     │                                    │
               [C_n 100nF]          OUTR ──► K_R ┐        │
                     │              OUTG ──► K_G │ D_n    ├── anodos A_R / A_G / A_B / A_W
                    GND             OUTB ──► K_B │ RGBW   │
                                    OUTW ──► K_W ┘        │
DIN_n ─────────────────────────────► DIN
                                     DOUT ──► DIN do próximo pixel
```

Os pixels são encadeados internamente em série, do U1/D1 ao U10/D10, com o sinal de dados propagando de pixel em pixel até chegar ao conector de saída DOUT.

---

## Especificações Técnicas

| Parâmetro | Valor |
|-----------|-------|
| **Quantidade de pixels** | 10 |
| **Controlador por pixel** | WS2814C (SMD) |
| **LED por pixel** | QBLP679-RGBCW (RGBW — 4 canais) |
| **Canais de cor** | Vermelho, Verde, Azul, Branco |
| **Resolução por canal** | 8 bits (0–255) por cor |
| **Protocolo de dados** | Single-wire — compatível WS28xx |
| **Tensão de alimentação** | 5VDC (WS2814C: 3,7 V a 5,3 V no VDD) |
| **Corrente por canal** | ~16,5 mA (corrente constante do WS2814C) |
| **Consumo máximo** | ~0,66 A em +5V (10 pixels × 4 canais em 100%) |
| **Conector de entrada** | J1 — 3 pinos: GND / DIN / 5V (pino 1 = GND) |
| **Conector de saída** | J2 — 3 pinos: 5V / DOUT / GND |
| **Capacitores** | 100nF / 0603 SMD (1 por pixel, VDD do WS2814C) |
| **Resistores** | 100Ω / 0603 SMD (1 por pixel, série no VDD do WS2814C) |
| **Fiduciais** | 3x (FID1, FID2, FID3) |
| **Ferramenta de Projeto** | KiCad 10 |
| **Tipo de montagem** | SMD — fabricação automatizada (J1/J2 PTH na borda) |
| **Placa** | 150 × 15 mm, 2 camadas |

---

## Lista de Materiais

| Ref | Componente | Valor / Parte | Encapsulamento |
|-----|-----------|--------------|----------------|
| U1–U10 | Controlador LED | WS2814C | SOP-8 (ZAT_LED:ws2814c) |
| D1–D10 | LED RGBW | QBLP679-RGBCW | PLCC-8 (ZAT_LED:QBLP679RGBCW) |
| C1–C10 | Capacitor de desacoplamento | 100nF | 0603 SMD |
| R1–R10 | Resistor do filtro RC do VDD | 100Ω | 0603 SMD |
| J1 | Conector de Entrada | GND / DIN / 5V | Barra de pinos 1x3 90° 2,54 mm, na borda |
| J2 | Conector de Saída | 5V / DOUT / GND | Barra de pinos 1x3 90° 2,54 mm, na borda |
| FID1–FID3 | Fiducial de montagem | — | Fiducial 1mm / Mask 2mm |

---

## Pinagem dos Conectores

### J1 — Entrada (lado esquerdo)

| Pino | Sinal | Descrição |
|------|-------|-----------|
| 1 | GND | Terra |
| 2 | DIN | Entrada de dados serial |
| 3 | 5V | Alimentação positiva |

> J1 e J2 são espelhados: na placa, a linha de cima é sempre 5V e a de baixo GND, para encaixar uma barra na outra.

### J2 — Saída (lado direito)

| Pino | Sinal | Descrição |
|------|-------|-----------|
| 1 | 5V | Alimentação (passthrough) |
| 2 | DOUT | Saída de dados para próxima barra |
| 3 | GND | Terra |

---

## Encadeamento de Barras

A barra suporta encadeamento em série para criar arranjos maiores com um único pino de dados:

```
Microcontrolador
      │
   Pino DIN
      │
  ┌───▼───────────┐      ┌───────────────┐      ┌───────────────┐
  │  WS2814A #1   │      │  WS2814A #2   │      │  WS2814A #3   │
  │  10 pixels    ├─────►│  10 pixels    ├─────►│  10 pixels    │
  │  J1: DIN      │DOUT  │  J1: DIN      │DOUT  │  J1: DIN      │
  └───────────────┘      └───────────────┘      └───────────────┘
   pixels 1–10            pixels 11–20            pixels 21–30

Fonte 5V ──► J1(5V) de cada barra (alimentação independente por barra)
```

> ⚠️ Para encadeamentos longos, conecte a fonte de alimentação 5V diretamente em cada barra — não apenas na primeira. Isso evita quedas de tensão e variações de cor nos pixels mais distantes.

---

## Aplicações

- 💡 **Iluminação ambiente RGBW** — branco de alta qualidade com canal W dedicado
- 🎨 **Painéis e displays de cor** — efeitos visuais com 10 pixels individualmente controláveis
- 📷 **Iluminação para fotografia e vídeo** — temperatura de cor ajustável via R+G+B+W
- 🏠 **Automação residencial** — integração com Home Assistant, ESPHome, MQTT
- 🤖 **Projetos com Arduino e ESP32** — biblioteca Adafruit NeoPixel (modo RGBW)
- 🎭 **Cenografia e decoração** — efeitos de luz programáveis para eventos
- 🔬 **Laboratório e prototipagem** — módulo de iluminação endereçável para experimentos

---

## Estrutura do Repositório

```
FITA_DE_LED_WS2814A/
├── FITA_DE_LED_WS2814A.kicad_pro   # Projeto KiCad 10
├── FITA_DE_LED_WS2814A.kicad_sch   # Esquemático: capa A2 (índice das folhas)
├── PIXELS_1_5.kicad_sch            # Folha 02: pixels 1 a 5
├── PIXELS_6_10.kicad_sch           # Folha 03: pixels 6 a 10
├── CONN.kicad_sch                  # Folha 04: J1 / J2, fiduciais, PWR_FLAG
├── FITA_DE_LED_WS2814A.kicad_pcb   # Layout da PCB (KiCad 10)
├── FITA_DE_LED_WS2814A.kicad_prl   # Configurações locais
├── FITA_DE_LED_WS2814A.kicad_dru   # Regras customizadas (vazio: version 1)
├── Zat_TitleBlock.kicad_wks        # Title block ZAT ELECTRONIC
├── fabrication-toolkit-options.json  # Configurações de fabricação
├── packages3D/                     # Modelos 3D arquivados (referência)
├── imagem_1.png                    # Render 3D superior
├── imagem_2.png                    # Render 3D isométrico
├── LEIA-ME_REVISAO_R01.md          # O que mudou na revisão R01
└── README.md
```

---

## Como Usar

### Conexão básica com Arduino / ESP32

```
Arduino / ESP32        WS2814A — J1
───────────────        ─────────────
GND          ────────► GND
5V           ────────► 5V
Pino digital ────────► DIN
```

### Biblioteca

O WS2814C recebe **32 bits por pixel** (R, G, B, W). A API clássica da FastLED envia 24 bits (RGB), por isso o exemplo usa a **Adafruit NeoPixel** no modo `NEO_RGBW`.

### Exemplo com Adafruit NeoPixel

```cpp
#include <Adafruit_NeoPixel.h>

#define PIN        6
#define NUMPIXELS 10

Adafruit_NeoPixel strip(NUMPIXELS, PIN, NEO_RGBW + NEO_KHZ800);   // WS2814: ordem R G B W

void setup() {
  strip.begin();
  strip.setBrightness(100);
}

void loop() {
  // Canal branco puro (W)
  for (int i = 0; i < NUMPIXELS; i++) {
    strip.setPixelColor(i, strip.Color(0, 0, 0, 255));
  }
  strip.show();
  delay(1000);

  // Vermelho
  for (int i = 0; i < NUMPIXELS; i++) {
    strip.setPixelColor(i, strip.Color(255, 0, 0, 0));
  }
  strip.show();
  delay(1000);
}
```
## Sobre

<div align="center">

Projetado por **ZAT ELECTRONIC**

*WS2814A — Made in Brazil 🇧🇷*

[![KiCad](https://img.shields.io/badge/Projetado%20com-KiCad%2010-blue)](https://www.kicad.org/)
[![Open Source Hardware](https://img.shields.io/badge/Open%20Source-Hardware-brightgreen)](https://www.oshwa.org/)

</div>