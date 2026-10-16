# Qwen-Image 3.0 Pro API (`qwen-image-3.0-pro`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-hero-perfume.jpg" width="820" alt="Sample render via the APIMart image API">
</p>

> **from $0.0285712 per image** (1K $0.0285712 · 2K $0.0571432) — `https://api.apimart.ai/v1` の OpenAI 互換エンドポイント1つだけ、月額プランは不要です。*(2026-10-12 時点)*

**[APIキー](https://go.apimart.ai/k-39f078)** · **[料金](https://go.apimart.ai/k-66b456)** · **[モデルページ](https://go.apimart.ai/k-f0958c)** · [⚡ 60秒クイックスタート](#quickstart)

**チームが APIMart 経由で Qwen-Image 3.0 Pro（`qwen-image-3.0-pro`）を使う理由**

- **キー1つでカタログ全体。**同じ `https://api.apimart.ai/v1` の base URL と `Authorization` ヘッダーで Qwen-Image 3.0 Pro（`qwen-image-3.0-pro`）と画像・動画・言語モデルなど 300+ のモデルにアクセスできます。切り替えるのは `model` フィールドだけで、クライアント側の変更は不要です。
- **最低 $1 から、従量課金。**サブスクリプションも、事前に規模を見積もるプリペイドプランもありません。$1 からチャージして、呼び出した分だけ使えます。
- **課金内容はレスポンスで返ります。**完了したタスクはすべて請求額（`cost` / `credits_cost`）を報告し、`failed` になったタスクは返金されます。
- **非同期前提の設計。**送信して `task_id` を受け取り、`GET /v1/tasks/{id}` をポーリングするだけ。バッチ処理もリトライも特別な統合ではなく、普通のキュー処理です。

<!-- /conv-kit:v1 -->

**別名:** クウェン・イメージ

Qwen-Image 3.0 Pro is the higher-fidelity Qwen image tier with 1K/2K output.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Per-image | `qwen-image-3.0-pro` | per delivered image, by resolution | see the [model page](https://go.apimart.ai/k-f0958c) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/images/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Output | List price | Effective price |
| --- | --- | --- |
| default | 0.035714 | 0.0285712 |
| 1K | 0.035714 | 0.0285712 |
| 2K | 0.071429 | 0.0571432 |


<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-66b456) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

## Request parameters

| Field | Type | Default | Notes |
| --- | --- | --- | --- |
| `model` | string | required | the model id shown in the route table above |
| `prompt` | string | required | what to generate, or what to change when editing |
| `size` | string | `auto` / `1:1` | aspect ratio, or `auto` where the route supports it |
| `resolution` | string | `1K` | output resolution tier — cost scales with it |
| `n` | integer | `1` | number of images per task, where the route allows more than one |
| `image_urls` | string[] | — | reference images as public HTTP(S) URLs or base64 data URLs |
| `nsfw_check` | boolean | `false` | runs `omni-moderation-latest` before submitting |

The table lists the fields accepted by the APIMart `images/generations` endpoint. Check the [model page](https://go.apimart.ai/k-f0958c) for anything this model adds on top.

## Quickstart

```bash
curl --request POST --url https://api.apimart.ai/v1/images/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  -d '{"model":"qwen-image-3.0-pro","prompt":"A bamboo forest at sunrise, slow push-in","size":"1:1","resolution":"1K"}'
```

```python
import os, time, requests

BASE = "https://api.apimart.ai/v1"
HEADERS = {"Authorization": f"Bearer {os.environ['APIMART_API_KEY']}", "Content-Type": "application/json"}

created = requests.post(f"{BASE}/images/generations", headers=HEADERS, timeout=60, json={
    "model": "qwen-image-3.0-pro", "prompt": "A bamboo forest at sunrise, slow push-in",
}).json()
task_id = created["data"]["id"]

while True:
    task = requests.get(f"{BASE}/tasks/{task_id}", headers=HEADERS, timeout=60).json()["data"]
    if task["status"] in ("completed", "failed"):
        break
    time.sleep(5)
print(task.get("cost"), task["result"]["images"][0]["url"])
```

Runnable versions: [`examples/`](examples). The task lifecycle is `pending → processing → completed | failed`.

## FAQ

**Qwen-Image 3.0 Pro のモデル id は何ですか？**

`qwen-image-3.0-pro` です。上のルート表を参照してください。

**課金方法は？**

**from $0.0285712 per image** (1K $0.0285712 · 2K $0.0571432)。完了したタスクは `cost` フィールドに正確な金額を報告し、失敗したタスクは返金されます。

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-39f078) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, this language** — the other half of this pair: [qwen-image-3-0-pro-prompts-ja](https://github.com/cae0venx1p0/qwen-image-3-0-pro-prompts-ja)
- **Same model, other languages** — 9 more language builds: 中文 [qwen-image-3-0-pro-api-cn](https://github.com/cnc4l4x4p3ndb0/qwen-image-3-0-pro-api-cn) · Deutsch [qwen-image-3-0-pro-api-de](https://github.com/kcqkgc4m/qwen-image-3-0-pro-api-de) · Español [qwen-image-3-0-pro-api-es](https://github.com/flm74hfs6gr5/qwen-image-3-0-pro-api-es) · Français [qwen-image-3-0-pro-api-fr](https://github.com/z3phc/qwen-image-3-0-pro-api-fr) · Relay/gateway [qwen-image-3-0-pro-api-gateway](https://github.com/pzlp9carpi/qwen-image-3-0-pro-api-gateway) · Bahasa `qwen-image-3-0-pro-api-id` · 한국어 [qwen-image-3-0-pro-api-ko](https://github.com/n80bh8gycesx4t/qwen-image-3-0-pro-api-ko) · Português [qwen-image-3-0-pro-api-pt](https://github.com/c7686676/qwen-image-3-0-pro-api-pt) · Русский `qwen-image-3-0-pro-api-ru`
- **More models in this language** — [midjourney-api-ja](https://github.com/apimartt1q/midjourney-api-ja) · [gemini-2-5-flash-image-preview-api-ja](https://github.com/ic7skkh/gemini-2-5-flash-image-preview-api-ja) · [nano-banana-api-ja](https://github.com/e4ylndl3g-v/nano-banana-api-ja) · [gemini-3-1-flash-image-preview-api-ja](https://github.com/gv4ju/gemini-3-1-flash-image-preview-api-ja) · [nano-banana-2-api-ja](https://github.com/lpbxkrzbfel/nano-banana-2-api-ja) · [gemini-3-pro-image-preview-api-ja](https://github.com/zzsxs8r/gemini-3-pro-image-preview-api-ja)
- **Topic hubs** — every build of this model, all languages: [qwen-image-3-0-pro](https://github.com/topics/qwen-image-3-0-pro) · [qwen-image-3-0-pro-api](https://github.com/topics/qwen-image-3-0-pro-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->

## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-f0958c> | `apimart.ai/ja/model/qwen-image-3-0` |
| Live pricing | <https://go.apimart.ai/k-66b456> | `apimart.ai/ja/pricing` |
| Get an API key | <https://go.apimart.ai/k-39f078> | `apimart.ai/ja/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

Qwen-Image 3.0 Pro is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
