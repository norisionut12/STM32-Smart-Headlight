# STM32-Smart-Headlight
Sistem Auto de Stabilizare și Iluminare Automată

Acest proiect reprezintă un prototip pentru un sistem de faruri inteligente. Dispozitivul menține farurile la nivel orizontal indiferent de înclinația mașinii și aprinde automat faza scurtă atunci când detectează întuneric.

### Video Proiect
[![Proiect Videoclip](https://img.youtube.com/vi/xtVIIzk4ZnM/maxresdefault.jpg)](https://www.youtube.com/shorts/xtVIIzk4ZnM)

__Cum funcționează?__
Stabilizarea: Senzorul de înclinație măsoară unghiul mașinii. 

Microcontroller-ul procesează aceste date și comandă servomotorului să rotească farul în sens opus înclinației, pentru a păstra lumina pe drum.

Iluminarea: O fotorezistență măsoară lumina ambientală. Când se face întuneric, sistemul aprinde automat un LED de mare putere.

__Componente folosite__
- Nucleo-L476RG: Microcontroller-ul care coordonează tot sistemul.

- MPU6050 (Accelerometru): Măsoară înclinația (unghiul de Pitch).

- Servomotor SG90: Mișcă fizic suportul farului.

- OLED SSD1306: Afișează în timp real unghiul în grade și starea farului (ON/OFF).

- Fotorezistență + LM358N (OpAmp): Detectează lumina. Am folosit OpAmp-ul ca Buffer pentru a avea o citire stabilă și precisă.

- LED + Tranzistor 2N2222: Farul propriu-zis. Tranzistorul protejează placa Nucleo de curentul mare cerut de LED ( in cazul in care doresc un LED de o putere mai mare ).

__Configurare STM32CubeMX__
- I2C1: Comunicare cu senzorul MPU6050 și ecranul OLED.

- ADC1: Citirea valorii de la senzorul de lumină (Canalul 5).

- TIM1 (PWM): Generarea semnalului pentru controlul precis al servomotorului.

- GPIO Output: Controlul tranzistorului pentru aprinderea LED-ului.

- Frecvență ceas: 80 MHz pentru un timp de răspuns rapid.

__Logica Codului__
- Codul rulează într-o buclă infinită care face următoarele:

- Filtrare: Citește unghiul de la accelerometru și aplică un mic filtru (medie între valoarea nouă și cea veche) ca să nu tremure servo-ul.

- Control: Calculează poziția servo-ului în funcție de unghi și o actualizează prin PWM.

- Decizie: Verifică valoarea ADC. Dacă lumina scade sub pragul stabilit, aprinde LED-ul.

- Feedback: Trimite toate datele pe ecranul OLED pentru a vedea ce se întâmplă "sub capotă"


__Structura Fișierelor__

Core/Src/main.c: Logica principală a aplicației

Core/Src/ssd1306.c: Driverul pentru afișajul OLED

Far_var1.ioc: Fișierul de configurare grafică pentru pini și periferice
