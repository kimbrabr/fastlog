# FastLogD

⚡ Высокопроизводительный systemd-совместимый logging-демон с поддержкой JSON, sampling и gzip-ротации.

## 📌 Цель

Создать минималистичный, быстрый и надёжный логгер, способный обрабатывать ≥500k сообщений/сек через Unix-socket, с минимальной нагрузкой на CPU и RAM.

## ⚙️ Особенности

- Приём логов по Unix-сокету `/tmp/fastlogd.sock`
- Структурированные JSON-логи
- Сжатие `gzip` + ротация по размеру
- Формат логов: `/var/log/fastlog/YYYYMMDD_HHMMSS.log.gz`
- `fastlogd.conf`: настройка max_size, max_files
- Поддержка сигнала `SIGUSR1` для `reopen`
- CLI-инструмент `fastlogctl`
- systemd unit: `fastlogd.service`

## 🚀 Производительность

- ≥500 000 msg/sec (`flood_test`, payload 100 B)
- CPU <1%, RAM <50MB

| Тест | RPS | RSS | CPU |
|------|-----|-----|-----|
| `flood_test` | 500k msg/sec | ~47MB | ~0.7% |

## 🧱 Архитектура

- mmap + lock-free SPSC ring-buffer
- Поток-писатель + поток-сжатия (gzip)
- Ввод: Unix-socket → очередь → gzip → файл

## 📁 Структура

