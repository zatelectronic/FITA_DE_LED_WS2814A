# FITA DE LED WS2814A — Revisão R01 — 2026-11-12
Zat Electronic · KiCad 10.0 · padrão Zat (doc 11)

## Arquivos
- WS2814A.* -> FITA_DE_LED_WS2814A.* (nome da pasta, sem espaços). .kicad_dru só com "(version 1)".
- Removidos: .history (também a de dentro de packages3D), ~WS2814A.kicad_pro.lck, ERC.rpt, DRC.rpt.

## Esquema
- Folha A3 plana -> capa A2 + 3 folhas: 02 PIXELS 1–5, 03 PIXELS 6–10 (A3), 04 CONEXÕES (A4).
- Cada pixel num bloco com moldura (WS2814C + LED + R + C).
- Referências: IC1–IC10 -> U1–U10, L1–L10 (LED) -> D1–D10 (L é prefixo de indutor). R, C, J, FID iguais.
- Labels: só DIN_1, DOUT_5 e DOUT_10 cruzam folhas (global, input/output). RED/GREEN/BLUE/WHITE_n e os outros DOUT_n viraram labels locais.
- Símbolos e footprints:

  | Ref | Antes | Agora |
  |-----|-------|-------|
  | U1–U10 | WS2814C:WS2814C + ws2814c:ws2814c (libs do projeto) | ZAT_LED:WS2814C + ZAT_LED:ws2814c (global, 3D WS2814C) |
  | D1–D10 | RGBW:QBLP679-RGBCW + RGBW:QBLP679RGBCW | ZAT_LED:QBLP679-RGBCW + ZAT_LED:QBLP679RGBCW (global, 3D RGBW) |
  | R1–R10 | PCM_4ms_Resistor:100_0603 + R_0603 (PCM não instalado) | Device:R + Resistor_SMD:R_0603_1608Metric, 100R |
  | C1–C10 | PCM_SparkFun 0.1uF + PCM_4ms C_0603 | Device:C + Capacitor_SMD:C_0603_1608Metric, 100nF |
  | J1/J2 | Connector_Generic:Conn_01x03 + PinHeader_1x03 Horizontal | ZAT_Connector:Conn_01x03 + ZAT_Connector:PinHeader_1x03_P2.54mm_Horizontal_EdgeMount |
  | FID1–FID3 | Fiducial_1mm_Mask2mm (versão antiga) | Fiducial_1mm_Mask2mm do KiCad 10 |

- PWR_FLAG em GND e +5V (vêm do J1) e no nó VDD de cada WS2814C (alimentado só pelo R de 100 R). Resolve os 12 power_pin_not_driven.
- Campos: Value, Footprint, Description em PT, Stock Code, Package Group. Os campos de fabricante (Yageo RC0603FR-07100RL, JLCPCB C22775 etc.) foram mantidos.

## PCB
- Cobre alterado (autorizado pelo Kenneth):
  - C1–C10: 0,32 mm para a esquerda. O courtyard invadia o do LED (10 courtyards_overlap).
  - J1 +0,10 mm e J2 −0,12 mm para dentro. Os pads ficavam a 0,40 mm da borda e a regra é 0,50 mm (6 copper_edge_clearance).
  - As pontas das trilhas presas aos pads acompanharam.
- Footprints 0603 do KiCad 10: pads 0,025 mm (R) e 0,075 mm (C) mais para dentro. As trilhas continuam ligadas.
- J1/J2: footprint EdgeMount (silk só dentro da placa, corpo em balanço). Resolve os 12 silk_edge_clearance.
- Referências visíveis em F.SilkS 1,0 mm.
- Textos 5V / DIN / DOUT / GND: 0,65 -> 0,8 mm (mínimo da placa), verticais e alinhados aos pinos. Resolve os 26 text_height.
- Bibliotecas PCM/RGBW/ws2814c não são mais usadas. Resolve os 40 lib_footprint_issues e os 5 lib_footprint_mismatch.
- Zonas GND (F.Cu e B.Cu) sem preenchimento salvo: preencher com B. Na simulação, todo o GND fica num só grupo.

## Biblioteca global (já gravada no PC, md5 conferido)
- symbols/ZAT_Connector.kicad_sym: + Conn_01x03 (cópia do KiCad com filtro ampliado para ZAT_Connector).
- footprints/ZAT_Connector.pretty: + PinHeader_1x03_P2.54mm_Horizontal_EdgeMount (3D do KiCad 10).

## README
- Conferido contra o circuito:
  - os 100 R + 100 nF são filtro do VDD de cada WS2814C, não "proteção de dados";
  - o diagrama do pixel foi refeito;
  - o LED tem 4 canais, não 5;
  - BOM com U/D;
  - J1 é pino 1 GND / 2 DIN / 3 5V (estava invertido);
  - consumo ~0,66 A em +5V.
- Exemplo: NeoPixel com NEO_RGBW (o WS2814 recebe R G B W). O exemplo FastLED saiu, porque a API clássica manda só 24 bits (RGB).

## Pontos de projeto
- OK: WS2814C VDD 3,7–5,3 V com 5 V − queda no 100 R. Saídas de corrente constante (~16,5 mA) nos catodos, anodos em +5V: folga suficiente para o LED branco/azul (~3 V).
- MELHORIA: 10 pixels em branco total puxam ~0,66 A por J1. Em cadeia longa, alimentar cada barra (o README já avisa).
- MELHORIA: não há capacitor bulk (ex.: 10 µF) na entrada de +5V.

## Após abrir no KiCad
1. Reiniciar o KiCad (biblioteca ZAT_Connector atualizada).
2. ERC no esquema.
3. PCB: B (preencher zonas) -> F8 (deve vir sem alterações) -> DRC.
