---
layout: post
title: A Google bemutatta az új state-of-the-art nyílt forráskódú nagyméretű nyelvi
  modelljét a Gemma-t!
date: 2024-02-22 07:00 +0100
categories: [Hasznos, Trend]
seo.type: BlogPosting
image: /assets/ideogram_hello_gemma.webp
description: itt az új LLM a Google-től, a Gemma
---

# Cél
Ha elolvasod a bejegyzést,
* megtudhatod, hogy hogyan használhatod a Google új LLM modelljét,
* megismerheted a Gemma főbb tulajdonságait

A Google leleplezte a Gemma-t, egy olyan könnyűsúlyú, csúcstechnológiás, nyílt forráskódú modellcsomagot, amit a Gemini modellek létrehozásánál alkalmazott kutatásokra és technológiára alapoztak.

## Gemma kulcsfontosságú tulajdonságai:

- Modellvariációk: A Gemma 2 és 7 milliárd (2B és 7B) paraméter számossággal is elérhető, mindkettő elérhető utasításra (instruction) és alap (base) formátumban.
- Kereskedelmi felhasználás: Teljes körűen engedélyezett kereskedelmi célokra, ami új lehetőségeket nyit a vállalkozások előtt.
- Kontextusablak: Mindkét modell támogatja a 8192 tokenes kontextusablakot.
- Teljesítmény: A 7B modell jelentősen lepipálja a versenytársakat, például a Mistral AI 7B-t és az LLaMa 2-t a Human Eval és MMLU teszteken, az MMLU-n 64,56-os eredményt elérve.

## A Gemma architektúrája számos jelentős fejlesztést tartalmaz a hagyományos transzformátor alapú modellekhez képest. Az innovációk közé tartoznak:

- Multi-Query Attention és RoPE: Továbbfejlesztett figyelemmechanizmusok és pozicionális kódolás a hatékonyabb tanulás érdekében.
- GeGLU aktivációs függvények: A szabványos ReLU helyettesítése a dinamikusabb modellválaszokért.
- Normalizálási technikák: Az RMSNorm használata a jobb stabilitás érdekében a rétegek között.

A Google hozzáférhetővé tette a Gemmát olyan platformokon, mint a Hugging Face, a Kaggle és a Vertex AI.

Amennyiben érdekes a számodra, már ki is próbálhatod a modell képességeit a [HuggingFace-en](https://huggingface.co/chat/settings/google/gemma-7b-it).

## Példa prompotok a Gemma bemutatására:

1. Kreatív írás:

   Írj egy 500 szavas novellát egy varázslatos erdőben élő tündérről, aki egy elátkozott herceggel találkozik. Használj élénk leírásokat és izgalmas cselekményt.

2. Kódolás:

   Írj egy Python kódot, amely egy weboldalt épít fel a kedvenc hobbiddal kapcsolatban. Használj API-kat és adatbázisokat a funkcionalitás bővítéséhez.


![huggingface](/assets/huggingface.webp)
![huggingface](/assets/usage.webp)

## Felhasználás

Érdekel hogy tudod saját gépről host-olni? Programozáshoz segítségül hívnád?
Olvass bele a [Így használd a Gemma-t Codeium/Copilot alternatívaként IntelliJ/VS Code IDE-ben.](/hasznos/trend/2024/02/27/így-használd-a-gemma-t-codeium-copilot-alternatívaként-intellij-vs-code-ide-ben.html) cikkbe!

## Linkek
- [Gemma Google Dev](https://ai.google.dev/gemma/?utm_source=keyword&utm_medium=referral&utm_campaign=gemma_cta&utm_content)
- [HuggingFace Gemma Chat fine tuned](https://huggingface.co/chat/settings/google/gemma-7b-it)