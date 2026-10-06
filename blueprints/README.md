# Daikin: Raumtemperatur vom Wandthermostat

Blueprint: [`automation/daikin_raumtemperatur_wandthermostat.yaml`](automation/daikin_raumtemperatur_wandthermostat.yaml)

Die Daikin misst die Temperatur im Innengerät unter der Decke und liegt dadurch
oft mehrere Grad neben der echten Raumtemperatur. Die Daikin-Integration von
Home Assistant kann keinen externen Fühler übergeben. Deshalb korrigiert die
Automation den Sollwert der Daikin:

```
Sollwert Daikin = Wunschtemperatur + (Temperatur Daikin − Temperatur Wandthermostat)
```

Beispiel Heizen: Wunsch 21 °C, Wandthermostat misst 20 °C, Daikin misst 24 °C,
also bekommt die Daikin 25 °C. Sie heizt, bis ihr eigener Fühler 25 °C zeigt,
und das ist der Fall, wenn das Wandthermostat 21 °C misst. Das funktioniert
genauso beim Kühlen und im Automatikbetrieb.

## Einrichten

1. In Home Assistant **Einstellungen → Automationen & Szenen → Blueprints →
   Blueprint importieren** öffnen und diese URL eintragen:
   `https://github.com/T3flon/spypoint-home-assistant/blob/ccr-7b39d5cb-5ek82m/blueprints/automation/daikin_raumtemperatur_wandthermostat.yaml`
2. Auf **Automation erstellen** klicken und auswählen:
   - **Klimaanlage**: die Daikin (`climate.…`)
   - **Raumtemperatur**: das Wandthermostat Wohnzimmer (oder sein Temperatursensor)
   - **Wunschtemperatur**: ebenfalls das Wandthermostat. Dann folgt die Daikin
     dessen Solltemperatur und Zeitplan. Alternativ geht ein eigener
     Zahl-Helfer (siehe unten).
3. Speichern. Die Feineinstellungen passen in der Regel so, wie sie sind.

## Wichtig zu wissen

- **Solltemperatur nur noch am Wandthermostat bzw. am Helfer ändern**, nicht an
  der Daikin oder ihrer Fernbedienung. Manuelle Änderungen an der Daikin
  überschreibt die Automation beim nächsten Durchlauf. In der Daikin-Karte steht
  deshalb der korrigierte Wert (z. B. 25 °C), nicht die Wunschtemperatur.
- **Kühlen**: Wenn das Wandthermostat nachts auf Absenktemperatur (z. B. 17 °C)
  geht, würde die Daikin ebenfalls darauf herunterkühlen. Wer im Sommer kühlt,
  nimmt besser einen eigenen Helfer als Wunschtemperatur: **Einstellungen →
  Geräte & Dienste → Helfer → Helfer erstellen → Zahl** (z. B. 16–30 °C,
  Schrittweite 0,5).
- Reagiert sofort, wenn die Wunschtemperatur geändert oder die Daikin
  eingeschaltet bzw. umgeschaltet wird. Danach wird alle 10 Minuten nachgeführt
  (einstellbar). Die Automation sendet nur, wenn sich der Wert wirklich ändert.
  Wenn das Piepen der Daikin stört, die **Mindeständerung** auf 1 K oder das
  **Prüfintervall** auf 15 bis 30 Minuten erhöhen.
- Ist die Daikin aus, im Entfeuchten- oder Lüftermodus, oder ist das
  Wandthermostat nicht erreichbar, macht die Automation nichts.
- Sicherheitsgrenzen: höchstens ±5 K Korrektur, Sollwert zwischen 16 und
  30 °C, beim Kühlen und im Automatikbetrieb mindestens 18 °C.
- Ist mit „Wandthermostat“ eine kabelgebundene Daikin-Fernbedienung gemeint
  (z. B. Madoka BRC1H), braucht es das alles nicht: Dort lässt sich in den
  Installateur-Einstellungen direkt der Fühler der Fernbedienung als
  Raumfühler wählen.
