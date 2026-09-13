# Kanıtlar / Evidence — 2026-09-13

Gerçek ölçümler: Haimaker dashboard trace'leri + OpenClaw state DB. / Real measurements: Haimaker dashboard traces + OpenClaw state DB. Model: deepseek/deepseek-chat (aynı model iki sistemde / same model on both systems).

## 1. Şema vergisi / Schema tax

| Ölçüm / Metric | Değer / Value |
|---|---|
| Tools provided (directory OFF, tam katalog / full catalog) | 52 |
| Tools provided (directory ON) | **11** |
| Prompt (tam katalog / full catalog turn) | 26.339 tok |
| Prompt (directory turn) | **11.889 – 14.034 tok** |
| Cost per turn (directory) | $0.0033 – $0.0044 |
| Cache Read Tokens (provider-level) | 13.568 |

Kaynak / Source: Haimaker dashboard trace gen-1789304963, gen-1789308863, DB usage kayıtları.

## 2. Aynı görev kıyası / Same-task comparison: OpenClaw vs Hermes

| Görev / Task | OpenClaw (+directory) | Hermes (deferred) |
|---|---|---|
| Resim sayma / count images (16 dosya) | **16 — doğru/correct** ✅ | 0 — yanlış/wrong ❌ |
| Excel sayma / count Excel (5 dosya) | **5 — doğru/correct** ✅ | 2 — yanlış/wrong ❌ |
| Prompt token | 11.889 – 13.475 | 15.549 – 15.785 |
| Tools provided | **11** | 25 |
| Makro görevi / macro task | statik analize kaçtı → SOUL.md persona ile çözüldü / fixed via SOUL.md persona | **26 VBA makroyu çalıştırıp raporladı** ✅ |

Not: Makro görevindeki fark model değil **kişilik dosyasıydı** — Hermes'te SOUL.md
(action-oriented persona) var, OpenClaw'da yoktu. `SOUL.md` eklenerek giderildi.

## 3. Web sitesi görevi / Website task

| | OpenClaw | Hermes |
|---|---|---|
| Süre / time | **41 sn** | döngüye girdi / looped (dotnet) |
| Server durumu / server status | **HTTP 200 — canlı/alive** ✅ | ölü/dead (onay kapısı process'i bekletti) |
| Çözüm / approach | python http.server (basit) | dotnet new web (ağır) |

## 4. Sonuç / Conclusion

1. Tool Search `directory` mode: şema yükünü ~%50 düşürür, tool sayısıyla ölçeklenmez.
2. Persona dosyası (SOUL.md) davranışı belirler — aynı model (deepseek-chat) persona
   olmadan kaçınıyor, persona ile çalışıyor.
3. Basit görevlerde basit çözüm (http.server) ağır framework'ten (dotnet) daha güvenilir.
