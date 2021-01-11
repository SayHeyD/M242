# IoTKitV3

# Erstes Programm auf Mikroprozessor anwenden

Als erstes Programm habe ich mich für [Programmnamen]()

## Eigener Service

### Error-States

Unten sind die möglichen Error-States gelistet.

**Zur erklärung:**
* Die Nummern der LEDs sind von Links nach rechts durchnummeriert.
* Die Spalte Nachricht zeigt, welche Nachricht auf dem OLED display ausgegeben wird.

### No Error (Normal Operation)

| Errorcode | LED | Nachricht |
| --------- | --- | --------- |
| 0         | -   | -         |

Die Observierte Temperatur liegt im festgelegten Normalbereich.

### Temperature too high

| Errorcode | LED | Nachricht                        |
| --------- | --- | -------------------------------- |
| 1         | 1   | Temperature is above the limits! |

Die Observierte Temperatur liegt oberhalb des festgelegten Normalbereichs.

### Temperature too low

| Errorcode | LED | Nachricht                        |
| --------- | --- | -------------------------------- |
| 2         | 2   | Temperature is below the limits! |

Die Observierte Temperatur liegt unterhalb des festgelegten Normalbereichs.