---
layout: post
title: Így használd a Gemma-t Codeium/Copilot alternatívaként IntelliJ/VS Code IDE-ben.
date: 2024-02-27 14:57 +0100
categories: [Hasznos, Trend]
seo.type: BlogPosting
image: /assets/gemma_autopilot.webp
description: itt az új LLM a Google-től, a Gemma
---

# Cél
Ha elolvasod a bejegyzést,
* megtudhatod, hogy hogyan használhatod a Google új LLM modelljét kódgenerálásra IntelliJ IDE-ben,
* megismerheted a Continue autopilot open-source eszközt,
* betekintést kapsz a Ollama LLM modell lokális futtatására szolgáló eszközre.

A GitHub-on az egyik felkapott repository a [Continue autopilot eszközé](https://github.com/continuedev/continue).
Ezt és a Gemma-t használva egy internet használata nélküli autopilot eszközt kaphatunk. 

## Ollama
A telepítés nagyon egyszerűen az [Ollama letöltési oldaláról](https://ollama.com/download) indítható.
Linux esetében ez egy egyszerű parancssori utasítás:
```shell
curl -fsSL https://ollama.com/install.sh | sh
```

Ezt követően választhatunk a támogatott modellek közül a [Ollama Model oldalán](https://ollama.com/library), hogy melyik Gemma modellt szeretnénk lokálisan futtatni:
```shell
ollama run gemma:7b
ollama run gemma:2b
```

Alap telepítés esetén az Ollama a háttérben elindít egy REST szolgáltatást, amelyet az alábbi curl hívással tudunk tesztelni:

```shell
curl --location 'http://127.0.0.1:11434/api/generate' \
--header 'Content-Type: application/json' \
--data '{
  "model": "gemma:2b",
  "prompt": "Hello",
  "stream": false
}'
```

## Intellij IDEA Continue plugin
Következő lépésként telepítsük a [Continue plugint](https://continue.dev/docs/quickstart) az IDE-be.
Ezt a File/Settings... (CTRL+ALT+S) menüpontból elérhető Plugins beállítás szekcióban tehetjük meg.

![Continue plugin](/assets/continue.webp)

A plugin telepítése után a beállításaiban (alábbi képen a fogaskerék)  

![Continue plugin](/assets/continue_config.webp)

a "models" json lista elemhez fűzzük hozzá az alábbi beállításokat.
```json
  "models": [
    {
      "title": "Gemma 2b",
      "provider": "ollama",
      "model": "gemma:2b",
      "completionOptions": {},
      "apiBase": "http://127.0.0.1:11434"
    },
    {
      "title": "Gemma 7b",
      "provider": "ollama",
      "model": "gemma:7b",
      "completionOptions": {},
      "apiBase": "http://127.0.0.1:11434"
    }
  ]
```

A Continue plugin használatáról kapott mini tutorial segítségével már ki is próbálhatjuk az új MI asszisztensünket.

![Continue plugin](/assets/continue_inwork.webp)
