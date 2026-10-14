# Reliable Importer

Лабораторна робота №5 з курсу «Професійний Python».
Варіант №1 — «Імпорт та експорт даних системи обліку студентів».

## Description

Надійний модуль імпорту/експорту та конфігурації:
streaming CSV → validation → filtering → JSON export з atomic write.
Має власну exception hierarchy, logging, context manager,
strict/tolerant режими, статистику valid/invalid.

## Features

- streaming CSV import (не завантажує весь файл у пам'ять);
- validation + custom `RecordValidationError` з exception chaining;
- YAML configuration з fail-fast валідацією;
- strict / tolerant режими обробки невалідних записів;
- statistics: total / valid / invalid / exported;
- JSON export через atomic write (`.tmp` → `.replace`);
- власний context manager `logged_operation` для логування часу;
- logging у файл + консоль (UTF-8).

## Requirements

Python 3.11+, PyYAML

## Installation

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -e ".[dev]"
Run
bash
python -m reliable_importer.main
Project structure
text
reliable_importer/
├── exceptions.py         — власна ієрархія винятків
├── models.py             — Student + config dataclasses
├── config.py             — YAML loader + fail-fast валідація
├── logging_config.py     — налаштування logging
├── context_managers.py   — logged_operation (context manager)
├── readers.py            — streaming CSV reader
├── validators.py         — parse_student + RecordValidationError
├── exporters.py          — atomic JSON export
├── services.py           — streaming import + statistics
└── main.py               — точка входу
Author
Student: Anastasiia Brativnyk
Group: ФеП-32
Lab: №5, Variant 1

text

---

## 🐍 Крок 17. Встанови та запусти

```bash
cd ~/Desktop/Brativnyk_Anastasiia_Lab05
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -e ".[dev]"
python -m reliable_importer.main
Очікуваний вивід у терміналі:

text
2026-10-14 12:00:00 | INFO | reliable_importer.context_managers | Student import/export started
2026-10-14 12:00:00 | WARNING | reliable_importer.services | Invalid record at line 5: line 5: Student name is empty
2026-10-14 12:00:00 | WARNING | reliable_importer.services | Invalid record at line 6: line 6: Grade must be between 0 and 100
2026-10-14 12:00:00 | WARNING | reliable_importer.services | Invalid record at line 7: line 7: Invalid student ID
2026-10-14 12:00:00 | INFO  | reliable_importer.context_managers | Student import/export finished in 0.0007 s
2026-10-14 12:00:00 | INFO  | reliable_importer.main | Total=7, valid=4, invalid=3, exported=4