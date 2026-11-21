# E22P-868M30S + XIAO ESP32-S3 Plus — wariant Meshtastic

Node Meshtastic zbudowany z modułu **Ebyte E22P-868M30S** (1 W, LNA, filtr SAW) na adapterze Bikemana „E22/RA62 SMD to DIL v.2” i płytki **Seeed XIAO ESP32-S3 Plus**. Połączenia wyłącznie przez boczne otwory XIAO.

Wariant firmware: **`E22P-868M30S_XIAO-ESP32S3-PLUS_DIV`** (Meshtastic 2.8.x).

**Pomysłodawca projektu:** Rafał Cichosz (rpc)
Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>

> **EN:** Meshtastic variant for an Ebyte E22P-868M30S wired to a Seeed XIAO ESP32-S3 Plus (side through-holes only), with a measured PA gain table for correct TX power and EN control via `SX126X_POWER_EN`. Full assembly guide (Polish) in the PDF.

## Dokumentacja

Pełna instrukcja montażu ze zdjęciami, tabelą połączeń, pomiarami mocy i noise floor oraz uwagami do montażu na maszcie:
Montaż E22P-868M30S + XIAO ESP32-S3 Plus (Meshtastic).pdf

## Zawartość repozytorium

| Plik | Opis |
|---|---|
| `e22p_xiao_s3_plus_variant.patch` | wariant: `variant.h`, `pins_arduino.h`, `platformio.ini` |
| `no_chat_storage.patch` | opcjonalny: przy *unmessagable* node przekazuje tekst dalej, ale go nie zapisuje |
| `Montaz_E22P-868M30S_XIAO-ESP32-S3-Plus.pdf` | instrukcja montażu |

## Szybki start

```bash
git clone https://github.com/meshtastic/firmware.git meshtastic-firmware
cd meshtastic-firmware
git submodule update --init --recursive
git apply ../e22p_xiao_s3_plus_variant.patch
git apply --3way ../no_chat_storage.patch          # opcjonalnie
pio run -e E22P-868M30S_XIAO-ESP32S3-PLUS_DIV -t upload
```

Pierwsze wgranie na czysty XIAO: `esptool erase_flash`, potem `./device-install.sh -f firmware-...factory.bin`.

## Moc nadawania

Wariant zawiera tabelę wzmocnienia PA zmierzoną dla E22P, więc w aplikacji wpisuje się **realną moc na wyjściu modułu**. Limit EU (869,4–869,65 MHz): 27 dBm ERP. Przy antenie 5 dBi: `lora.tx_power 24`.

## Najważniejsze zasady

- Nigdy nie zasilaj radia bez anteny albo obciążenia 50 Ω.
- EN modułu stale w stanie wysokim (steruje nim firmware przez `SX126X_POWER_EN`).
- Na liniach sterujących tylko 3,3 V; 5 V wyłącznie na VCC modułu.

## Licencja

MIT
