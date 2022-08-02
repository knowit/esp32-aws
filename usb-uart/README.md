# Installer driver for ESP32

ESP32 snakker med datamaskinen over seriell.

Utviklingskortet har en chip som oversetter seriell til USB, og denne krever en driver.

Last ned driveren fra https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers

NB! For Linux (testet på Ubuntu 16):

Driveren er inkludert fra før av og trenger ikke installeres

NB! For macOS:

Når denne har blitt installert må den "godkjennes" under Systemvalg > Sikkerhet og Personvern > General for at den faktisk skal starte.
