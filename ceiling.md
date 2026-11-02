# My ceiling — person detection on the ESP32-S3

| Image size | My prediction | Measured | FPS | Accuracy |
|------------|---------------|----------|-----|----------|
| 96 × 96 | Guess: not recorded | **4362 ms** | **0.2** | **80.7%**|
| 160 × 160 | 96 px time × 2.8 = **12,213.6 ms** | **12,078 ms** | **0.1** | **83.2%** |
| 48 × 48 | 96 px time ÷ 4 = **1,090.5 ms** | **1,022 ms** | **1** | **76.5%** |

## Which budget breaks first — time or memory?

1. A doorbell: decide "person or not" within 1 second of the button press, at 96 × 96.

   → **Time.** The measured inference time is 4362 ms (4.36 seconds), which is much longer than the 1-second requirement.

2. A people counter: keeps one 320 × 240 colour photo in memory and checks it once a minute.

   → **Memory.** A 320 × 240 RGB image requires 230,400 bytes (225 KiB). The image needs to fit in available memory. Since it only checks once per minute, processing speed is less critical.

3. Your team's project: which budget is tighter for your model, and why?

   → **Time.** My 96 × 96 model takes 4362 ms per image (approximately 0.2 FPS). The ESP32-S3 has approximately 8 MB PSRAM and 367,424 bytes of free internal RAM at startup, so processing speed is currently the bigger limitation.
