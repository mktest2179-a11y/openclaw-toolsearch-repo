# OpenClaw Tool Search Setup — Token Diyeti 🦞

> Tek kişilik amatör proje: OpenClaw'a "tool schema vergisi" ödemeyi bıraktırdım.
> A tiny amateur setup: making OpenClaw stop paying the "tool schema tax".

**TR** | **EN** (aşağıda / below)

---

## Türkçe

### Bu nedir?

OpenClaw varsayılan olarak **her mesajda** modelin gördüğü tüm araçların (52 tool) tam JSON şemalarını
prompt'a ekler. Hiçbir şey yapmasan bile her turn'de binlerce token sabit maliyet ödersin.
Buna ben "şema vergisi" diyorum.

Bu repo, OpenClaw'ın **deneysel Tool Search** özelliğini (`directory` modu) açarak bu vergiyi
düşürmenin kurulum notlarını ve **gerçek ölçümleri** içerir.

### Kurulum (3 komut)

```powershell
# 1. Yedek al
Copy-Item "openclaw.json" "openclaw.json.pre-toolsearch" -Force

# 2. Tool Search'ü directory modunda aç
openclaw config set tools.toolSearch.enabled true
openclaw config set tools.toolSearch.mode directory

# 3. Doğrula
openclaw config validate
```

Restart gerekmez — OpenClaw config'i canlı izler (hot-reload).

### Ne değişti?

- Model artık 52 tool'un şemasını baştan almıyor
- Yerine: kısa bir araç dizini + `tool_search` / `tool_describe` / `tool_call` köprü araçları
- Model ihtiyacı olan aracı o an arayıp çağırıyor

### Gerçek ölçümler (2026-09-13, deepseek-chat via Haimaker)

| | Tool sayısı | Prompt token | Sonuç |
|---|---|---|---|
| Önce (tam katalog) | 52 | 26.339* | — |
| Sonra (directory) | **11** | **11.889** | ✅ doğru |

\* farklı oturumda tek turn ölçümü; taban farkı temsilidir.

#### Hermes agent ile aynı görev kıyası

| | **OpenClaw + directory** | **Hermes (deferred tools)** |
|---|---|---|
| Sağlanan araç | **11** | 25 |
| Prompt token | **11.889 – 13.300** | 15.549 – 15.752 |
| Resim sayma görevi | **16 resim — doğru** ✅ | 0 resim — yanlış ❌ |
| Excel sayma görevi | **5 dosya — doğru** ✅ | 2 dosya — yanlış ❌ |

### Dikkat / notlar

- Tool Search **deneyseldir** — beğenmezsen `openclaw config set tools.toolSearch.enabled false`
- Aramalar İngilizce sorgularla çalışır (sen konuşurken Türkçe konuşabilirsin; iç sorguları agent üretir)
- Kullanım verisi (usage) sağlayıcıya bağlıdır: bazı rotalar (ör. bazı deepseek proxy'leri) token
  raporu göndermez, o zaman gerçek maliyeti sağlayıcının panelinden takip et
- Config'inde gateway token gibi sırlar varsa repoya **asla** koyma

---

## English

### What is this?

By default, OpenClaw ships **every tool's full JSON schema** (52 tools) to the model on
**every single message** — a fixed token tax per turn, even for "hello".
This repo contains setup notes and **real measurements** for cutting that tax using OpenClaw's
experimental **Tool Search** feature in `directory` mode.

### Setup (3 commands)

See the Turkish section above — same commands. No gateway restart required (hot-reload).

### Results

With `directory` mode enabled, the model saw **11 tools instead of 52**, prompt size dropped from
~26k to ~12k tokens on comparable turns, and the agent **outperformed** a Hermes deferred-tools setup
on the same tasks (correct results, fewer provided tools).

### Notes

- Tool Search is experimental; disable anytime with one config command
- Tool search queries are English (BM25 over English names/descriptions); you can still chat
  in any language — the agent generates the internal queries
- Usage reporting depends on the provider; verify real spend in your provider's dashboard
- Never commit secrets (gateway tokens, API keys) from your real config

---

## License / Lisans

MIT — iyi uyarlamalar / hack away.
