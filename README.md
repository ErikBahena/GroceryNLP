<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://img.shields.io/badge/grocery-nlp-white?style=for-the-badge&labelColor=111827&color=374151">
  <img alt="grocerynlp" src="https://img.shields.io/badge/grocery-nlp-black?style=for-the-badge&labelColor=111827&color=374151">
</picture>

### Voice to groceries. On-device. Instant.

A **0.63 MB** neural network that turns natural speech into structured grocery data — items, quantities, stores, and categories. Runs entirely in the browser. No server. No API keys. No data leaves your device.

**[Try the live demo &rarr;](https://erikbahena.github.io/GroceryNLP/)**

---

## Performance

| Metric | Value |
|:---|:---|
| Model size | **0.63 MB** |
| Inference | **~5 ms** |
| Intent accuracy | **99.9%** |
| Entity extraction F1 | **98.7%** |
| Category accuracy | **96.3%** |

<br>

## How it works

```
"add 2 gallons of milk and a loaf of bread from Costco"
```

```json
{
  "intent": "add",
  "items": [
    { "name": "milk",  "quantity": "2 gallons", "category": "Dairy",  "store": "Costco" },
    { "name": "bread", "quantity": "1 loaf",    "category": "Bakery", "store": "Costco" }
  ]
}
```

One model handles everything — intent classification, entity extraction, and category prediction — in a single forward pass.

<br>

## Capabilities

**6 intents** &mdash; add, remove, check, uncheck, clear, update

**Entity extraction** &mdash; item names, quantities, units, store names

**11 categories** &mdash; Fruits & Vegetables, Meats, Dairy, Bakery, Canned & Packaged, Frozen, Beverages, Deli, Snacks, Pantry Staples, Other

**Multi-item parsing** &mdash; handles compound requests like *"eggs, cheese, and butter"*

**Natural phrasing** &mdash; understands *"put bananas on the list"*, *"we need orange juice"*, *"pick up some chicken"*

<br>

## Architecture

```
Speech ──► Web Speech API ──► Text ──► Char Tokenizer ──► ONNX Model ──► Structured Output
                                              │
                                    ┌─────────┴──────────┐
                                    │  Char-level         │
                                    │  Transformer        │
                                    │  Encoder            │
                                    ├─────────────────────┤
                                    │  Intent Head (6)    │
                                    │  Entity Head (32×11)│
                                    │  Category Head (5×11)│
                                    └─────────────────────┘
```

- **Encoder** &mdash; Character-level transformer, no external tokenizer dependency
- **Entity extraction** &mdash; Word-level BIO tagging (max 32 words per input)
- **Category prediction** &mdash; Per-item classification from entity-aware pooling
- **Runtime** &mdash; ONNX via WebAssembly ([onnxruntime-web](https://github.com/microsoft/onnxruntime))
- **Training** &mdash; PyTorch, 120k synthetic examples, CRF-assisted, label smoothing

<br>

## Run locally

```bash
git clone https://github.com/ErikBahena/GroceryNLP.git
cd GroceryNLP
python3 -m http.server 8000
```

Open [localhost:8000](http://localhost:8000). That's it.

<br>

## Stack

| Layer | Technology |
|:---|:---|
| Model | PyTorch &rarr; ONNX (INT8 quantized) |
| Browser inference | [onnxruntime-web](https://github.com/microsoft/onnxruntime) (WASM) |
| Voice input | [Web Speech API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API) |
| UI | Single HTML file, [Tailwind CSS](https://tailwindcss.com) |
| Mobile app | Flutter + [onnxruntime_flutter](https://pub.dev/packages/onnxruntime_flutter) |

<br>

## Files

```
├── index.html                    # Complete demo (22 KB)
├── grocery_nlp_quantized.onnx    # Quantized model (0.63 MB)
└── README.md
```

<br>

---

<sub>Built by [@ErikBahena](https://github.com/ErikBahena)</sub>
