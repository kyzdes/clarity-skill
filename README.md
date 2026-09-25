# clarity — чёткая и ёмкая речь

Плагин для Claude Code и Codex: правка любых рабочих текстов на русском
(статьи, посты, письма, доки, спичи) до ясности и плотности. Лечит канцелярит
и машинно-ровный слог — и объясняет, какие паттерны мешают тексту звучать
по-человечески.

## Что внутри

- **`skills/clarity/SKILL.md`** — операционное ядро: процедура из 10 проходов, 14 законов в сжатой форме, две ручки настройки, быстрые словари, формат ответа.
- **`references/method.md`** — полная методика: законы с примерами до/после и исключениями, русский слой, чек-лист перед сдачей.
- **`references/linter.md`** — спецификация линтера: L1 (словари/regex, правила с ID), L2 (смысловые проверки агентом), L3 (метрики текста), словари A.1–A.9.
- **`references/ai-writing-signals-wikipedia.md`** — отдельный проход по
  признакам машинно-ровного письма: ложная значительность, поверхностные
  толкования, одинаковые секции, тройки, жирные ярлыки и реплики ассистента.
  Это редакторские сигналы, а не детектор авторства.
- **`references/verdicts.md`** — 15 разрешённых споров школ (пассив, стоп-слова, длина, сжатие…), мифы и чёрный список опровергнутых «классических» цитат.
- **`references/demo.md`** — один текст до/после с полным разбором (121 слово AI-стиля → 89 слов человеческой прозы, 36 флагов → 0).

## Откуда это взялось

Методика синтезирована из шести deep-research исследований школ ясности: Joseph Williams («Style: Toward Clarity and Grace»), английский канон (Оруэлл, Strunk & White, Zinsser, plain language), Пинкер и классический стиль, русская классика (Чуковский, Нора Галь, Мильчин, Розенталь), инфостиль (Ильяхов, Главред, редполитики) и психолингвистика как арбитр. Каждый факт прошёл adversarial-верификацию тремя независимыми проверяющими; то, что верификацию не пережило (включая хрестоматийные примеры вроде «покойника» Чуковского и плеоназмов «§141»), вынесено в чёрный список и в методике не используется.

Ключевые «не как у всех» решения:
- **Не «выжигатель стоп-слов»:** селективное правило (Ильяхов + Пинкер + Уильямс сходятся) — убирается тик, модальность остаётся.
- **Вложенность вместо длины:** порога «N слов в предложении» наука не даёт; меряются вложенные придаточные и цепочки родительных.
- **Пассив не запрещён** — флагуется только спрятанный ответственный.
- **Граница упрощения:** объяснимая сложность (термины по делу) читателем не штрафуется — это измерено.
- **Балл линтера ≠ качество** — позиция самих создателей Главреда, встроена в архитектуру.

## Установка

```
/plugin marketplace add kyzdes/claude-skills
/plugin install clarity@claude-skills
```

## Использование

Просто попроси: «упакуй этот текст», «убери канцелярит», «почему это звучит как ИИ?», «сделай чётко и ёмко», «напиши анонс, чтобы не звучал как нейросеть». Скилл триггерится сам; для оценки без правки — «оцени текст по clarity».

## Plugin update policy

When Claude's native auto-update is enabled for `claude-skills`, the host owns
updates and this plugin's fallback updater does no work. Native auto-update is
an independent user setting; hook environment flags do not disable it.

With native auto-update off, the fallback hook updates **only this plugin**,
in the background, at most once per four hours after a successful update.
Updates share an OS lock, have bounded command timeouts, and retry failed work
without starting a four-hour success cooldown. Set `KKZ_NO_AUTOUPDATE=1` to
disable the fallback; `KKZ_AUTO_UPDATE_INTERVAL_SEC` sets its cooldown.
Keys Keeper's fallback additionally requires `KEYS_KEEPER_ENABLE_MUTABLE_AUTOUPDATE=1`
and respects `KEYS_KEEPER_NO_AUTOUPDATE`. It never updates another plugin.
To prohibit every automatic update, disable native auto-update as well.
Logs contain operation names and exit codes only, under
`~/.cache/kyzdes-claude-skills/v2/<config-id>/`, isolated by Claude configuration.
Python 3.9 or newer is required for the fallback.
