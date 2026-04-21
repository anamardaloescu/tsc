Diagrama bloc
este in diagrama.png

Descrierea hardware
Microcontroller principal – nRF52840 (U1)
	SoC Bluetooth 5 / 802.15.4, ARM Cortex-M4F @ 64 MHz
	1 MB Flash, 256 KB RAM
	Gestioneaza logica aplicației, senzori, display si conectivitatea wireless
	Antenă 2.4 GHz 2450AT18B100E (ANT1) plasata la marginea PCB, cu keepout de cupru
	Cristal de 32 MHz (X2) pentru ceas RF
	Retea de adaptare RF (L1, L2, L3, C3, C4, C9 etc.)

Incarcator baterie – BQ25180YBGR (IC1)
	Incarcator LiPo single-cell (BGA 1.6 × 1.1 mm)
	Comunicare I2C cu MCU
	Intrare alimentare: USB-C (VUSB)
	Ieșire SYS pentru alimentarea sistemului
	Conectare baterie prin test pad-uri (TP_BAT_GND, TP_VBAT)
	Condensatoare de decuplare 1 µF (C37, C38, C40, C41, C44, C45) plasate aproape de pini

Fuel Gauge – MAX17048G+T10 (U2)
	Monitorizare nivel baterie (SOC)
	Consum redus (~3 µA în repaus)
	Comunicare I2C
	Conectat in paralel cu bateria

Regulator buck-boost – RT6160AWSC (IC9)
	Regulator DC-DC controlat prin I2C
	Genereaza tensiune stabila pentru display e-paper
	Functioneaza din tensiunea bateriei
	Inductor L4 (68 µH) + pasive pentru comutie

Display e-paper (EPD) – conector FPC (J3)
	Conector Molex 503480-2400 (24 pini, pas 0.5 mm)
	Control prin SPI + GPIO (BUSY, RST, DC, CS)
	Circuit dedicat de alimentare (RT6160, diode, tranzistoare si pasive)

Accelerometru / IMU – BMA423 (IC3)
	Accelerometru triaxial
	Functii: pasi, activitate, detectare inclinare, wake-up
	Comunicare I2C
	Linie de intrerupere catre GPIO

Driver haptic – DRV2605YZFR (IC4)
	Suport pentru motoare ERM și LRA
	Biblioteca interna de efecte haptice
	Comunicare I2C
	Pin EN controlat de MCU

Protecție ESD – USBLC6-2SC6Y (D8)
	Protectie pentru liniile USB D+ / D−
	Plasata imediat după conectorul USB-C

Butoane
	3 butoane tactile SMD Panasonic EVP-AKE31A
	SW_UP, SW_DN, SW_ENT
	Pull-up extern: 10k la 3.3V (R1, R6, R10)
	Pozitionare dictata de carcasa

Conector debug – TC2030-IDC (J1)
	Interfată SWD (fără conector montat)
	Conectare prin pogo pins
	Semnale: SWDIO, SWDCLK, GND, VCC, RESET

Conector USB-C – KH-TYPE-C-16P (J5)
	16 pini
	VBUS → incarcator BQ25180
	D+ / D− → ESD → MCU
	CC1 / CC2 cu rezistente Rd 5.1k (mod device USB)

Utilizarea pinilor nRF52840

| Pin nRF52840 | Net / Semnal | Conectat la | Interfata |
| P0.00 / XL1 (D2) | P0.00/XL1 | Cristal 32 MHz (X2, pin 2) | OSC |
| P0.01 / XL2 (F2) | P0.01/XL2 | Cristal 32 MHz (X2, pin 1) | OSC |
| P0.02 / AIN0 (A12) | SCK | Conector FPC EPD J3 (pin 13) | SPI CLK |
| P0.03 / AIN1 (B13) | MOSI | Conector FPC EPD J3 (pin 14) | SPI MOSI |
| P0.04 / AIN2 (J1) | P0.04 | Neconectat (liber) | - |
| P0.05 / AIN3 (K2) | EPD_CS | Conector FPC EPD J3 (pin 12) | GPIO (CS EPD) |
| P0.06 (L1) | SDA | Bus I2C (IC1, IC9, IC3, IC4, U2) + TP_SDA + R18 | I2C SDA |
| P0.07 (M2) | SCL | Bus I2C (IC1, IC9, IC3, IC4, U2) + TP_SCL + R17 | I2C SCL |
| P0.08 (N1) | IMU_INT1 | BMA423 INT1 | GPIO (intrerupere IMU) |
| P0.09 / NFC1 (L24) | P0.09 | Neconectat (liber) | - |
| P0.10 / NFC2 (J24) | ALERT | Semnal ALERT (fuel gauge MAX17048) | GPIO |
| P0.11 (T2) | PMIC_INT | BQ25180 pin !INT (intrerupere charger) | GPIO |
| P0.12 (U1) | HAPTIC_EN | DRV2605 pin EN | GPIO (enable haptic) |
| P0.13 (AD8) | P0.13 | Buton SW_UP (prin R1 + C41) | GPIO (buton UP) |
| P0.14 (AC9) | P0.14 | Buton SW_ENT (prin R6 + C44) | GPIO (buton ENTER) |
| P0.15 (AD10) | EPD_DC | Conector FPC EPD J3 (pin 11) | GPIO (DC EPD) |
| P0.16 (AC11) | EPD_RST | Conector FPC EPD J3 (pin 10) | GPIO (RST EPD) |
| P0.17 (AD12) | EPD_BUSY | Conector FPC EPD J3 (pin 9) | GPIO (BUSY EPD) |
| P0.18 / RESET (AC13) | RESET | TC2030-IDC J1 pin 6 + TP5 | RESET |
| P0.19 (AC15) | P0.19 | Neconectat (liber) | - |
| P0.20 (AD16) | P0.20 | Neconectat (liber) | - |
| P0.21 (AC17) | P0.21 | Neconectat (liber) | - |
| P0.22 (AD18) | P0.22 | Neconectat (liber) | - |
| P0.23 (AC19) | P0.23 | Neconectat (liber) | - |
| P0.24 (AD20) | P0.24 | Neconectat (liber) | - |
| P0.25 (AC21) | P0.25 | Neconectat (liber) | - |
| P0.26 (G1) | P0.26 | Neconectat (liber) | - |
| P0.27 (H2) | P0.27 | Neconectat (liber) | - |
| P0.28 / AIN4 (B11) | P0.28 | Neconectat (liber) | - |
| P0.29 / AIN5 (A10) | P0.29 | Neconectat (liber) | - |
| P0.30 / AIN6 (B9) | P0.30 | Neconectat (liber) | - |
| P0.31 / AIN7 (A8) | P0.31 | Neconectat (liber) | - |
| P1.00 (AD22) | SWO | TP4 (test pad SWO) | SWD SWO |
| P1.01 (Y23) | P1.01 | Comanda Q4 (MOSFET alimentare EPD) | GPIO (EPD PWR EN) |
| P1.02 (W24) | P1.02 | Buton SW_DN (prin R10 + C45) | GPIO (buton DOWN) |
| P1.03 (V23) | P1.03 | Neconectat (liber) | - |
| P1.04 (U24) | P1.04 | Neconectat (liber) | - |
| P1.05 (T23) | P1.05 | Neconectat (liber) | - |
| P1.06 (R24) | P1.06 | Neconectat (liber) | - |
| P1.07 (P23) | P1.07 | Neconectat (liber) | - |
| P1.08 (P2) | IMU_INT2 | BMA423 INT2 | GPIO (intrerupere IMU 2) |
| P1.09 (R1) | P1.09 | Neconectat (liber) | - |
| P1.10 (A20) | P1.10 | Neconectat (liber) | - |
| P1.11 (B19) | P1.11 | Neconectat (liber) | - |
| P1.12 (B17) | P1.12 | Neconectat (liber) | - |
| P1.13 (A16) | P1.13 | Neconectat (liber) | - |
| P1.14 (B15) | P1.14 | Neconectat (liber) | - |
| P1.15 (A14) | P1.15 | Neconectat (liber) | - |
| VBUS (AD2) | VBUS | Tensiune USB-C (prin C21) | USB power |
| D+ (AD6) | D+ | USBLC6-2SC6Y pin I/O1_2 -> USB-C | USB data |
| D- (AD4) | D- | USBLC6-2SC6Y pin I/O2_2 -> USB-C | USB data |
| SWDIO (AC24) | SWDIO | TC2030-IDC J1 pin 2 + TP3 | SWD |
| SWDCLK (AA24) | SWDCLK | TC2030-IDC J1 pin 4 + TP6 | SWD |
| ANT (H23) | N$4 | Retea de adaptare RF (L1, C3) -> ANT1 | RF 2.4 GHz |
| XC1 (B24) | N$75 | Cristal 32 MHz extern (U$1, pin 1) + C1 | OSC RF |
| XC2 (A23) | N$74 | Cristal 32 MHz extern (U$1, pin 3) + C2 | OSC RF |
| DCC (B3) | N$57 | Inductanta L2 (filtru DC-DC intern) | DC-DC int |
| DCCH (AB2) | - | Neconectat in retea externa | - |
| DEC3V3 (AC5) | N$38 | Condensator C20 (decuplare 3V3 intern) | Decuplare |
| DEC1 (C1) | DEC1 | Condensator C5 (decuplare) | Decuplare |
| DEC2 (A18) | N$71 | Condensator C13 (decuplare) | Decuplare |
| DEC3 (D23) | DEC3 | Condensator C11 (decuplare) | Decuplare |
| DEC4 (B5) | DEC4_6 | Condensatoare C15, C16, C10 (decuplare) | Decuplare |
| DEC5 (N24) | N$10 | Condensator C9 (decuplare) | Decuplare |
| DEC6 (E24) | DEC4_6 | Condensatoare C15, C16, C10 (decuplare) | Decuplare |
| VDD x5 | 3V3 | Alimentare 3.3V | Alimentare |
| VDDH (Y2) | 3V3 | Alimentare 3.3V | Alimentare |
| VSS (B7) | GND | Masa | Masa |
| VSS_PA (F23) | GND | Masa (RF PA) | Masa |
| VSS_PAD | GND | Thermal pad masa | Masa |

Lista de materiale (BOM)

| Cant. | Referinta | Componenta | Capsula | Descriere | Link achizitie |
| 1 | U1 | nRF52840 | QFN-73 | SoC principal BT5 + 802.15.4 | [Nordic / Mouser](https://www.mouser.com/ProductDetail/Nordic-Semiconductor/nRF52840-QIAA-R7) |
| 1 | IC1 | BQ25180YBGR | BGA-8 | Incarcator LiPo | [Mouser](https://www.mouser.co.uk/ProductDetail/Texas-Instruments/BQ25180YBGR) |
| 1 | U2 | MAX17048G+T10 | TDFN-8 | Fuel gauge baterie | [SnapEDA](https://www.snapeda.com/parts/MAX17048G+T10/Analog+Devices/view-part/) |
| 1 | IC9 | RT6160AWSC | WL-CSP-15 | Regulator buck-boost I2C | [Mouser](https://www.mouser.co.uk/ProductDetail/Richtek/RT6160AWSC) |
| 1 | IC3 | BMA423 | LGA-12 | Accelerometru 3 axe | [Mouser](https://www.mouser.co.uk/ProductDetail/Bosch-Sensortec/BMA423) |
| 1 | IC4 | DRV2605YZFR | BGA-9 | Driver haptic ERM/LRA | [Arrow](https://www.arrow.com/en/products/drv2605yzfr/texas-instruments) |
| 1 | D8 | USBLC6-2SC6Y | SOT-23-6 | Protectie ESD USB | [SnapEDA](https://www.snapeda.com/parts/USBLC6-2SC6Y/STMicroelectronics/view-part/) |
| 3 | D1, D6, D7 | MBR0530 | SOD-123 | Dioda Schottky 30V 0.5A | [SnapEDA](https://www.snapeda.com/parts/MBR0530/Onsemi/view-part/) |
| 1 | ANT1 | 2450AT18B100E | 3216 | Antena chip 2.4 GHz | [Mouser](https://www.mouser.co.uk/ProductDetail/Johanson-Technology/2450AT18B100E) |
| 1 | J3 | 503480-2400 | FPC-24P | Conector FPC 0.5mm 24 pini | [Mouser](https://www.mouser.co.uk/ProductDetail/Molex/503480-2400) |
| 1 | J5 | KH-TYPE-C-16P | USB-C 16P | Receptacul USB-C | [SnapEDA](https://www.snapeda.com/parts/KH-TYPE-C-16P/Kinghelm/view-part/) |
| 1 | J1 | TC2030-IDC | TC2030 | Conector debug SWD | [Tag-Connect](https://www.tag-connect.com/product/tc2030-idc) |
| 3 | SW_UP, SW_DN, SW_ENT | EVP-AKE31A | SMD | Buton tactil | [DigiKey P123437TR-ND](https://www.digikey.com/en/products/detail/panasonic-electronic-components/EVP-AKE31A/123437) |
| 1 | Q2 | SI1308EDL-T1-GE3 | SC-70-3 | MOSFET N-ch 30V 1.5A | [SnapEDA](https://www.snapeda.com/parts/SI1308EDL-T1-GE3/Vishay+Siliconix/view-part/) |
| 1 | Q4 | DMG2305UX | SOT-23-3 | MOSFET P-ch 20V 4.2A | - |
| 1 | L7 | FTC252012SR47MBCA | 2016 | Inductanta 0.47 uH | [JLCPCB C5832368](https://jlcpcb.com/partdetail/6763488-FTC252012SR47MBCA/C5832368) |
| 1 | L4 | XFL4020 68uH | XFL4020 | Inductanta de putere 68 uH | - |
| 1 | L2 | 10 uH | 0805-WIDE | Inductanta filtru RF | - |
| 1 | L3 | 15 nH | 0805-WIDE | Inductanta retea RF | - |
| 1 | L1 | 3.9 nH | 0805-WIDE | Inductanta retea RF | - |
| 1 | X2 | 32 MHz | 2.0x1.6 mm | Cristal oscilator | - |
| 1 | SJ1 | Jumper lipire | SMD | Jumper de configurare | - |
| 14 | TP1-TP8... | Test pad-uri | TP20R | Puncte de test (vezi silkscreen) | - |
| ~40+ | C*, R* | Pasive diverse | 0201 / 0402 | Condensatoare de decuplare si rezistente | [JLCPCB Parts](https://jlcpcb.com/parts) |

Decizii de design si observatii

Grosimea PCB
	1.0 mm (nu 1.6 mm standard)
	obligatorie pentru a incapea in carcasa InkTime
Stratificare
	placa cu 2 straturi
		planuri de masa pe TOP si BOTTOM
via stitching intre planurile de masa, mai ales in zona RF
Plasarea antenei
	ANT1 la marginea PCB-ului
	zona de excludere a cuprului (keepout) pe toate straturile
	fara plan de masa sau trasee sub antena
Strategie de decuplare
	condensatoare de 100 nF plasate foarte aproape de pinii de alimentare
	la BGA: rutare cu latime redusa intre bile
Rutare
	fara unghiuri drepte
	doar trasee la 45 de grade
Latimea traseelor
	alimentare (VCC / VUSB / VBAT / 3V3): minim 0.3 mm
	exceptie sub BGA unde este necesar
	semnale de date: minim 0.15 mm
Conectarea bateriei
	conectare directa la TP_VBAT si TP_BAT_GND
	fara conector JST (economisire spatiu)
Butoane
	pozitionare dictata de carcasa
	carcasa nemodificata
Silkscreen
	doar designatori de referinta
	fara valori componente
	test pad-uri etichetate clar (MISO, MOSI, SCL, SDA, GND, VBAT etc.)
ERC / DRC
	ERC: un singur warning acceptat
	"Only INPUT pins on NET ID" (benign)
	DRC: conform regulilor JLCPCB pentru 2 straturi
Plasare componente
	toate componentele pe TOP
Erori acceptate
	erori DRC de tip Dimension
	cauzate de butoane si USB-C
	acceptate conform specificatiilor proiectului
