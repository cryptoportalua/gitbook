# Gaianet

## Скрипт

```bash
source <(curl -s https://raw.githubusercontent.com/cryptoportalua/scripts/refs/heads/main/gaianet)
```

## Ручне встановлення

```bash
# Install node
curl -sSfL 'https://github.com/GaiaNet-AI/gaianet-node/releases/latest/download/install.sh' | bash
```

Зберігаємо собі `device-id` та `адресу гаманця`

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

<details>

<summary>Міняємо стандартну модель на Qwen2 0.5B щоб фармити більше поінтів</summary>

```bash
# Вписуєм адресу ноди в перемінну
ADDRESS=""
```

```bash
sudo tee ~/gaianet/config.json > /dev/null <<EOF
{
  "address": "$ADDRESS",
  "chat": "https://huggingface.co/gaianet/Qwen2-0.5B-Instruct-GGUF/resolve/main/Qwen2-0.5B-Instruct-Q5_K_M.gguf",
  "chat_batch_size": "32",
  "chat_ctx_size": "131072",
  "description": "This GaiaNet node config with a Qwen2 0.5B model.",
  "domain": "gaia.domains",
  "embedding": "https://huggingface.co/gaianet/Nomic-embed-text-v1.5-Embedding-GGUF/resolve/main/nomic-embed-text-v1.5.f16.gguf",
  "embedding_batch_size": "8192",
  "embedding_collection_name": "default",
  "embedding_ctx_size": "8192",
  "llamaedge_port": "8082",
  "prompt_template": "chatml",
  "qdrant_limit": "1",
  "qdrant_score_threshold": "0.5",
  "rag_policy": "system-message",
  "rag_prompt": "Use the following pieces of context to answer the user's question.\nIf you don't know the answer, just say that you don't know, don't try to make up an answer.\n----------------\n",
  "reverse_prompt": "",
  "snapshot": "",
  "system_prompt": "You are a helpful, respectful, and honest assistant. Always answer accurately, while being safe."
}
EOF
```



</details>

```bash
source ~/.bashrc && gaianet init
```

```bash
gaianet start
```

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Переходимо у браузер за посиланням з терміналу

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

Тиснемо старт і спілкуємось із чатом, за кожен запит нараховуються поінти
