---
description: Projedeki son değişiklikleri Github'a pushlar ve GitHub Actions aracılığıyla canlıya deploy eder.
---

# Canlıya Gönder Workflow

Bu workflow projedeki değişiklikleri commit eder, GitHub'a pushlar ve ardından GitHub Actions otomatik deployment sürecini tetikler.

## Adımlar

1. **Commit ve Push İşlemi**
   Tüm değişiklikleri `git add .` ile ekle, commit et ve `main` branch'ine pushla.
   ```bash
   // turbo
   git add . && git commit -m "chore: update Jira filter and app version" && git push origin main
   ```

2. **Sonuç Bildirimi**
   İşlem tamamlandığında kullanıcıya kodların pushlandığını ve GitHub Actions deployment sürecinin başladığını bildir. Canlı sitenin adresi: https://yafiqa.github.io/reporter/
