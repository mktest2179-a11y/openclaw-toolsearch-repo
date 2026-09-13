# 🦞 OpenClaw Token Diet — 52 Tools → 11. Real Benchmarks.

**Cut your OpenClaw tool-schema tax by ~50% and prove it with numbers.** Works with any model (tested on deepseek-chat). No forks, no patches — one config flag + one persona file.

[![Tools provided](https://img.shields.io/badge/tools%20provided-52%20%E2%86%92%2011-blue)](#-kanıtlar--evidence) [![Prompt size](https://img.shields.io/badge/prompt-26k%20%E2%86%92%2012k%20tokens-green)](#-kanıtlar--evidence) [![Cost/turn](https://img.shields.io/badge/cost%2Fturn-%240.003--0.004-success)](#-kanıtlar--evidence) [![Status](https://img.shields.io/badge/status-tested%20%2B%20reproducible-orange)](#%EF%B8%8F-test-edin--test-it)

Türkçe | English (aşağıda / below)

## ⚡ TL;DR

OpenClaw her mesajda modelin gördüğü **tüm araç şemalarını** gönderir — 52 tool = ~26.000 token **sabit vergi**. `tools.toolSearch: "directory"` + kısa bir persona dosyası ile:

- Araç sayısı: **52 → 11**
- Prompt: **26k → 12k token**
- Turn maliyeti: **$0.0033 – $0.0044** (gerçek panel ölçümü)
- Görev doğruluğu: korundu (16/16 resim, 5/5 Excel, canlı web sunucusu)

Hepsini **Hermes Agent ile aynı modelde (deepseek-chat) kıyasladık** — tablolar aşağıda.

## 📊 Kanıtlar / Evidence

| Task | OpenClaw (+directory) | Hermes (deferred tools) |
|---|---|---|
| Count images (16 files) | **16 — correct** ✅ | 0 — wrong ❌ |
| Count Excel (5 files) | **5 — correct** ✅ | 2 — wrong ❌ |
| Tools provided | **11** | 25 |
| Prompt tokens | **11.9k – 13.5k** | 15.5k – 15.8k |
| Live web server task | **HTTP 200, 41s** ✅ | dead server ❌ |
| Macro analysis | refused → **fixed via SOUL.md** ✅ | ran 26 VBA macros ✅ |

Tam veri / full data: [EVIDENCE.md](EVIDENCE.md)

## 🔧 Reproduce it (3 komut / commands)

```powershell
Copy-Item openclaw.json openclaw.json.pre-toolsearch -Force
openclaw config set tools.toolSearch.enabled true
openclaw config set tools.toolSearch.mode directory
```

Restart gerekmez / no restart needed. Doğrula / verify: `openclaw config validate`

**Opsiyonel / optional** — agent'a "her görevi çalıştır" kişiliği ver (refusal'ları bitirir):
[SOUL.md](SOUL.md) dosyasını workspace root'una koy.

## 🧠 Nasıl çalışıyor / How it works

`directory` mode: model tüm şemaları değil, **sınırlı bir araç dizini + 3 köprü aracı**
(`tool_search` / `tool_describe` / `tool_call`) görür. İhtiyaç duyduğu aracı o an
arar, şemasını çeker, çağırır. Tool sayısı artsa da prompt sabit kalır.

## 🌍 Test edin / Test it

- Farklı modellerde deneyin (Mini/Pro/Ultra free'ler) — sonucu issue açın
- Kendi trace'inizi Haimaker/OpenRouter panelinden alıp EVIDENCE'a PR gönderin
- Yıldızlayın ki deneysel özellik kanıtlanmış örneklerle büyüsün ⭐

## 📁 Files

| File | What |
|---|---|
| [EVIDENCE.md](EVIDENCE.md) | Gerçek ölçümler + Hermes kıyası / real benchmarks |
| [SOUL.md](SOUL.md) | Executor persona — "çalıştır denildiğinde çalıştır" |
| LICENSE | MIT |

## ⚠️ Notes

- Tool Search is **experimental** — disable: `openclaw config set tools.toolSearch.enabled false`
- Search queries are English internally (chat in any language)
- Usage reporting depends on provider; verify real spend in provider dashboard
- Never commit secrets. Never.

## English TL;DR

OpenClaw pays a **fixed tool-schema tax (~26k tokens for 52 tools) on every message**.
This repo documents enabling the experimental **Tool Search (directory mode)** and the
results: **11 tools provided instead of 52**, prompt size halved, per-turn cost $0.003–0.004,
verified against a Hermes deferred-tools setup running the **same model** — with correct
results on every task. Setup is 3 commands, no restart, no fork. See [EVIDENCE.md](EVIDENCE.md).

## License

MIT
