# RVC-cheat-sheet

# Retrieval-based-Voice-Conversion-WebUI

<br><br>
<br><br>

## Install
- https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/blob/main/docs/en/README.en.md
```shell
sudo apt install ffmpeg

cd ~/Projects/ai

git clone https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI.git
cd Retrieval-based-Voice-Conversion-WebUI 

pyenv local 3.10

python3 -m venv venv
source venv/bin/activate

# cuda 12
pip install torchvision torchaudio torch --extra-index-url https://download.pytorch.org/whl/cu124

# Install requirements
pip install -r requirements.txt

# Download models
python tools/download_models.py

# start ui
python infer-web.py
```

<br><br>

## Training Guide
Die besten Einstellungen für das RVC WebUI-Training hängen stark von deinem spezifischen Ziel, deiner Hardware und den Anforderungen deines Datensatzes ab. Hier sind empfohlene Einstellungen und Hinweise für optimale Ergebnisse:

---

### **Schritt 1: Experimentelle Konfiguration**
1. **Experimentname:** Wähle einen klaren, beschreibenden Namen (z. B. `singer_name_v1` oder `project_voice_v2`), um verschiedene Experimente auseinanderzuhalten.
2. **Ziel-Samplerate:** 
   - **40k:** Für Sprache geeignet, weniger anspruchsvoll für Hardware.
   - **48k:** Für hochwertigen Gesang oder professionelle Audioqualität. Erfordert mehr Ressourcen.
3. **Tonhöhensteuerung (Pitch Guidance):**
   - **True:** Für Gesang und pitch-sensible Konvertierung.
   - **False:** Für reine Sprachumwandlungen.
4. **Version:** 
   - **v2:** Moderne Architektur, oft bessere Ergebnisse und Stabilität. Standardwahl.
5. **CPU-Prozesse:** 
   - Setze hier **2–4**, je nach verfügbarer CPU-Leistung, um Balance zwischen Geschwindigkeit und Verfügbarkeit anderer Ressourcen zu schaffen.

---

### **Schritt 2a: Audioverarbeitung**
1. **Pfad des Trainingsordners:** Gib den Ordner mit deinen Audiodateien an (am besten WAV-Dateien mit mindestens 5 Minuten Gesamtlänge).
2. **Speaker/Singer ID:** Gib eine numerische ID an (z. B. `1` für den ersten Sprecher). Relevant für Modelle mit mehreren Sprechern.

---

### **Schritt 2b: Pitchen und Featureextraktion**
1. **GPU Index:** 
   - Wähle die GPU, z. B. `0` oder `0-1`, wenn du mehrere GPUs verwenden möchtest.
   - Beachte: Die GPU sollte mindestens 8 GB VRAM haben.
2. **Pitch-Algorithmus:** 
   - **pm:** Schneller, aber niedrige Qualität. Nur für schnelle Tests.
   - **harvest:** Sehr gute Qualität, langsamer. Gut für hochwertigen Gesang.
   - **dio:** Schneller, akzeptable Qualität für Sprache.
   - **rmvpe:** Beste Ergebnisse, besonders bei Gesang und weniger CPU-Belastung.
   - **rmvpe_gpu:** Noch schneller, wenn GPU verfügbar ist.
3. **Empfehlung:** 
   - Für Gesang: `rmvpe_gpu`
   - Für Sprache: `dio` (wenn Geschwindigkeit wichtig ist) oder `harvest`.

---

### **Schritt 3: Training**
1. **Save frequency:** 
   - Alle **5–10 Epochen** speichern, abhängig von der Trainingsdauer und Speicherplatz.
2. **Trainingsepochen:** 
   - **100–200 Epochen** sind ein guter Startpunkt für solide Ergebnisse.
   - Für größere Datensätze (30+ Minuten) ggf. 300–400 Epochen.
3. **Batch-Größe pro GPU:** 
   - **4–8**, abhängig von deinem VRAM. Für GPUs mit 12 GB VRAM kann Batchgröße 8 verwendet werden.
4. **Nur letzte `.ckpt` speichern:** 
   - **Ja**, wenn Speicherplatz ein Problem ist.
   - **Nein**, wenn du verschiedene Trainingsstände vergleichen möchtest.
5. **Cache alle Trainingssets:** 
   - **Ja** bei kleinen Datensätzen (<10 Minuten).
   - **Nein** bei größeren Datensätzen, um Speicherprobleme zu vermeiden.
6. **Kleines finales Modell speichern:** 
   - **Ja**, um direkt eine kompakte Version zu haben.
7. **Vortrainiertes Modell laden:** 
   - Optional, hilfreich bei ähnlichen Datensätzen. Gib den Pfad zu `.G` und `.D` an.
8. **GPU Index:** Wähle deine verfügbaren GPUs (z. B. `0`, `0-1`).

---

### **Allgemeine Tipps:**
- **Hardware:** Verwende eine NVIDIA-GPU mit mindestens 8 GB VRAM. Optimal: RTX 3060 oder besser.
- **Audiodaten:** Mindestens 5 Minuten hochwertiges, sauberes Audio (WAV-Format, Mono, konstante Lautstärke).
- **Speicherplatz:** Plane genügend Platz für Zwischenergebnisse und Modelle ein (mindestens 20 GB empfohlen).
- **Nachbearbeitung:** Überprüfe Modelle regelmäßig, um Überanpassung zu vermeiden.

Mit diesen Einstellungen kannst du ein leistungsfähiges Modell trainieren. Viel Erfolg! 🎵
