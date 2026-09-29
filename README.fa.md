# 🦭 PromptSeal Action

**اکشن گیت‌هابی که وقتی رفتار پرامپت/مدل رگرسیون می‌کنه، بیلد رو fail می‌کنه.**

این اکشن [`promptseal ci`](https://github.com/ArsinShaabani/promptseal) رو اجرا می‌کنه:
همه‌ی کیس‌ها رو اجرا می‌کنه، با baseline قفل‌شده مقایسه می‌کنه، گزارش markdown رو توی
خلاصه‌ی PR می‌ذاره و هنگام رگرسیون غیرصفر خارج می‌شه.

## شروع سریع

```yaml
name: PromptSeal
on: [pull_request]

jobs:
  seal:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: ArsinShaabani/promptseal-action@v1
        with:
          provider: openai:gpt-4o-mini
        env:
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
```

ریپوت باید `promptseal.yaml` + پوشه‌ی `cases/` و یک baseline قفل‌شده داشته باشه
(آموزش کامل: [TUTORIAL.fa.md](https://github.com/ArsinShaabani/promptseal/blob/main/TUTORIAL.fa.md)).

## ورودی‌ها (Inputs)

| ورودی | پیش‌فرض | توضیح |
|---|---|---|
| `provider` | *(پیش‌فرض کانفیگ)* | مثلاً `openai:gpt-4o` یا `ollama:llama3.1:8b` |
| `min_pass_rate` | *(کانفیگ)* | مثل `0.95` — نرخ پاس کمتر از این، بیلد رو fail می‌کنه |
| `install_from` | `git` | بعد از انتشار PyPI می‌تونی `pypi` بذاری |
| `python_version` | `3.12` | نسخه‌ی پایتون رانر |
| `working_directory` | `.` | محل فایل `promptseal.yaml` (منوریپوها) |

کلید provider رو از طریق `env` پاس بده (`OPENAI_API_KEY`، `OPENROUTER_API_KEY` و...).
برای Ollama/vLLM سلف-هاست کلید لازم نیست.

## استراتژی baseline

بعد از هر تغییر بررسی‌شده، لوکال روی `main` قفل کن:

```bash
promptseal seal -p openai:gpt-4o
```

از این به بعد PR ها نسبت به همین رفتار سنجیده می‌شن.

## لایسنس

MIT © [Arsin Shaabani](https://github.com/ArsinShaabani)
