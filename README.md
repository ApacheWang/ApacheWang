### Systems Engineer — LLM Inference Runtimes, Tiered KV Storage & Wire Diagnostics

8+ years building C++ and Python infrastructure across constrained-memory distributed LLM serving, NVMe/columnar storage engines, and fault-tolerant runtimes.

Focused on three areas in open source:
- **C++ Inference & Tokenization**: GGUF model-loading contracts, BPE/Metaspace tokenization, activation extraction, and stateful KV slot persistence (`ggml-org/llama.cpp`).
- **Tiered KV Cache & Distributed Storage**: Zero-copy buffer lifecycle, placement modeling, and `O_DIRECT` / `io_uring` storage paths (`deepseek-ai/3FS`, `kvcache-ai/Mooncake`, `LMCache`).
- **Wire-Level Streaming Forensics**: Catching silent `HTTP 200 OK` protocol breaches across inference engines and OpenAI-compatible relays.

### Flagship Project

- **[ApacheWang/inferdiff](https://github.com/ApacheWang/inferdiff)** — Zero-dependency Python 3.11+ CLI ("Wireshark & protocol bill-checker for streaming LLMs"). Runs 5-second live canary probes (`inferdiff doctor`) and chunk-jitter-tolerant frame diffs (`inferdiff diff`) across `llama-server`, `vLLM`, `SGLang`, `Ollama`, and API relays, exporting `<80 KB` airgapped SVG frame waterfalls and `<=25`-line upstream GitHub issue reproducers.

### Selected Upstream Contributions

| Project | Patch | Status | Technical Summary |
| :--- | :--- | :--- | :--- |
| **ggml-org/llama.cpp** | [#33032](https://github.com/ggml-org/llama.cpp/pull/33032) | **Merged** (`v1.1.0`) | Consolidated 4 optional YaRN GGUF RoPE scaling parameters into `llama_model_base::load_hparams` (`+7/-13`) while preserving legacy `grok1` defaults (`0.00e+00` drift across 6 architectures). |
| **ggml-org/llama.cpp** | [#33038](https://github.com/ggml-org/llama.cpp/pull/33038) | **Merged** (`v1.1.0`) | Generalized BPE `metaspace.prepend_scheme = "always"` handling with backward-compatible fallback for legacy mmBERT GGUF vocabularies (`+8/-5`). |
| **ggml-org/llama.cpp** | [#33074](https://github.com/ggml-org/llama.cpp/pull/33074) | Open | Fixed `llama-cvector-generator` `l_out-<il>` block alignment, `ne[1] == 1` tail-layer trimming (`qwen35moe`/`gemma4`), variable-length prompt indexing, and zero-norm / 1-sample PCA guards. |
| **deepseek-ai/3FS** | [#459](https://github.com/deepseek-ai/3FS/pull/459) | Open | Reduced `DataPlacementModel.get_peer_traffic()` complexity from `O(v^2 b)` to `O(vb + bk^2 + v^2)` (`75.7x` speedup at `v=50, b=450, k=3`) with HiGHS MIP relaxation counterexample benchmarks. |
| **kvcache-ai/Mooncake** | [#5837](https://github.com/kvcache-ai/Mooncake/pull/5837) | Open | Fixed silent NumPy string-to-`uint8` corruption and `int16` failure on `BundleStore` DataProto fallback reads via context-managed `memoryview.cast("B")` and bounded single-block reuse (`5 -> 3` GETs). |
| **sgl-project/sglang** | [#53119](https://github.com/sgl-project/sglang/pull/53119) | Open | Parsed textless trailing SSE `meta_info` frames in native `/generate` benchmarks to surface silent short-generation completions (`8 / 8000` tokens under `ignore_eos=True`). |
| **ray-project/ray** | [#68555](https://github.com/ray-project/ray/pull/68555), [#68556](https://github.com/ray-project/ray/pull/68556) | Open | Hardened job supervisor state convergence (`RUNNING -> FAILED`) on GCS RPC timeouts and added bounded `GetAllJobInfo` retry for uninterrupted loopback WebSocket log streaming. |
