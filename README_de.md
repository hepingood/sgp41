[English](/README.md) | [ 简体中文](/README_zh-Hans.md) | [繁體中文](/README_zh-Hant.md) | [日本語](/README_ja.md) | [Deutsch](/README_de.md) | [한국어](/README_ko.md)

<div align=center>
<img src="/doc/image/logo.svg" width="400" height="150"/>
</div>

## LibDriver SGP41

[![MISRA](https://img.shields.io/badge/misra-compliant-brightgreen.svg)](/misra/README.md) [![API](https://img.shields.io/badge/api-reference-blue.svg)](https://www.libdriver.com/docs/sgp41/index.html) [![License](https://img.shields.io/badge/license-MIT-brightgreen.svg)](/LICENSE) 

Der SGP41 ist ein digitaler Gassensor, der für die einfache Integration in Luftreiniger oder bedarfsgesteuerte Lüftungssysteme entwickelt wurde. Die CMOSens®-Technologie von Sensirion bietet ein komplettes, einfach zu bedienendes Sensorsystem auf einem einzigen Chip mit einer digitalen I 2 C-Schnittstelle und temperaturgesteuerten Mikroheizplatten, die ein VOC- und ein NO x-basiertes Raumluftqualitätssignal liefern. Sowohl das Sensorelement als auch der Gasindex-Algorithmus zeichnen sich durch eine unübertroffene Robustheit gegenüber kontaminierenden Gasen aus, die in realen Anwendungen vorkommen, was eine einzigartige Langzeitstabilität sowie eine geringe Drift ermöglicht. Das sehr kleine 3-DFN-Gehäuse von 2,44 x 2,44 x 0,85 mm ermöglicht Anwendungen auf engstem Raum. Der hochmoderne Produktionsprozess von Sensirion garantiert eine hohe Reproduzierbarkeit und Zuverlässigkeit. Die Band- und Rollenverpackung sowie die Eignung für Standard-SMD-Bestückungsprozesse machen den SGP41 prädestiniert für Anwendungen mit hohen Stückzahlen.

LibDriver SGP41 ist ein voll funktionsfähiger Treiber von SGP41, der von LibDriver eingeführt wurde. Er bietet VOC-, NOX-Messung, Temperatur- und Feuchtigkeitskorrektur und andere Funktionen. LibDriver ist MISRA-kompatibel.

### Inhaltsverzeichnis

  - [Anweisung](#Anweisung)
  - [Installieren](#Installieren)
  - [Nutzung](#Nutzung)
    - [example basic](#example-basic)
  - [Dokument](#Dokument)
  - [Beitrag](#Beitrag)
  - [Lizenz](#Lizenz)
  - [Kontaktieren Sie uns](#Kontaktieren-Sie-uns)

### Anweisung

/src enthält LibDriver SGP41-Quelldateien.

/interface enthält die plattformunabhängige Vorlage LibDriver SGP41 IIC.

/test enthält den Testcode des LibDriver SGP41-Treibers und dieser Code kann die erforderliche Funktion des Chips einfach testen.

/example enthält LibDriver SGP41-Beispielcode.

/doc enthält das LibDriver SGP41-Offlinedokument.

/Datenblatt enthält SGP41-Datenblatt.

/project enthält den allgemeinen Beispielcode für Linux- und MCU-Entwicklungsboards. Alle Projekte verwenden das Shell-Skript, um den Treiber zu debuggen, und die detaillierten Anweisungen finden Sie in der README.md jedes Projekts.

/misra enthält die Ergebnisse des LibDriver MISRA Code Scans.

### Installieren

Verweisen Sie auf eine plattformunabhängige IIC-Schnittstellenvorlage und stellen Sie Ihren Plattform-IIC-Treiber fertig.

Fügen Sie /src, /interface und /example zu Ihrem Projekt hinzu.

### Nutzung

#### example basic

```C
#include "driver_sgp41_basic.h"

uint8_t res;
uint32_t i;
uint16_t id[3];
int32_t voc_gas_index;
int32_t nox_gas_index;
uint32_t times = 3;
float rh = 50.0f;
float temp = 25.0f;

/* init */
res = sgp41_basic_init();
if (res != 0)
{
    return 1;
}

...
    
/* loop */
for (i = 0; i < times; i++)
{
    /* delay 1000ms */
    sgp41_interface_delay_ms(1000);

    /* read data */
    res = sgp41_basic_read(temp, rh, &voc_gas_index, &nox_gas_index);
    if (res != 0)
    {
        (void)sgp41_basic_deinit();

        return 1;
    }

    /* output */
    sgp41_interface_debug_print("sgp41: %d/%d.\n", (uint32_t)(i + 1), (uint32_t)times);
    sgp41_interface_debug_print("sgp41: voc gas index is %d.\n", voc_gas_index);
    sgp41_interface_debug_print("sgp41: nox gas index is %d.\n", nox_gas_index);
    
    ...
}

...

/* loop */
for (i = 0; i < times; i++)
{
    /* delay 1000ms */
    sgp41_interface_delay_ms(1000);

    /* read data */
    res = sgp41_basic_read_without_compensation(&voc_gas_index, &nox_gas_index);
    if (res != 0)
    {
        (void)sgp41_basic_deinit();

        return 1;
    }

    /* output */
    sgp41_interface_debug_print("sgp41: %d/%d.\n", (uint32_t)(i + 1), (uint32_t)times);
    sgp41_interface_debug_print("sgp41: voc gas index is %d.\n", voc_gas_index);
    sgp41_interface_debug_print("sgp41: nox gas index is %d.\n", nox_gas_index);
    
    ...
}

...
    
/* get serial id */
res = sgp41_basic_get_serial_id(id);
if (res != 0)
{
    (void)sgp41_basic_deinit();

    return 1;
}

/* output */
sgp41_interface_debug_print("sgp41: serial id 0x%04X 0x%04X 0x%04X.\n", (uint16_t)(id[0]), (uint16_t)(id[1]), (uint16_t)(id[2]));

...
    
/* deinit */
(void)sgp41_basic_deinit();

return 0;
```

### Dokument

Online-Dokumente: [https://www.libdriver.com/docs/sgp41/index.html](https://www.libdriver.com/docs/sgp41/index.html).

Offline-Dokumente: /doc/html/index.html.

### Beitrag

Bitte beachten Sie CONTRIBUTING.md.

### Lizenz

Urheberrechte © (c) 2015 - Gegenwart LibDriver Alle Rechte vorbehalten



Die MIT-Lizenz (MIT)



Hiermit wird jeder Person kostenlos die Erlaubnis erteilt, eine Kopie zu erhalten

dieser Software und zugehörigen Dokumentationsdateien (die „Software“) zu behandeln

in der Software ohne Einschränkung, einschließlich, aber nicht beschränkt auf die Rechte

zu verwenden, zu kopieren, zu modifizieren, zusammenzuführen, zu veröffentlichen, zu verteilen, unterzulizenzieren und/oder zu verkaufen

Kopien der Software und Personen, denen die Software gehört, zu gestatten

dazu eingerichtet werden, unter folgenden Bedingungen:



Der obige Urheberrechtshinweis und dieser Genehmigungshinweis müssen in allen enthalten sein

Kopien oder wesentliche Teile der Software.



DIE SOFTWARE WIRD "WIE BESEHEN" BEREITGESTELLT, OHNE JEGLICHE GEWÄHRLEISTUNG, AUSDRÜCKLICH ODER

STILLSCHWEIGEND, EINSCHLIESSLICH, ABER NICHT BESCHRÄNKT AUF DIE GEWÄHRLEISTUNG DER MARKTGÄNGIGKEIT,

EIGNUNG FÜR EINEN BESTIMMTEN ZWECK UND NICHTVERLETZUNG VON RECHTEN DRITTER. IN KEINEM FALL DARF DAS

AUTOREN ODER URHEBERRECHTSINHABER HAFTEN FÜR JEGLICHE ANSPRÜCHE, SCHÄDEN ODER ANDERE

HAFTUNG, OB AUS VERTRAG, DELIKT ODER ANDERWEITIG, ENTSTEHEND AUS,

AUS ODER IM ZUSAMMENHANG MIT DER SOFTWARE ODER DER VERWENDUNG ODER ANDEREN HANDLUNGEN MIT DER

SOFTWARE.

### Kontaktieren Sie uns

Bitte senden Sie eine E-Mail an lishifenging@outlook.com.