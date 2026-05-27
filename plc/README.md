# Modbus client — котлы 5 и 6

## Что добавить в TIA Portal

1. **DB `modbus_data`** — массивы для котла 6 (по аналогии с котлом 5):
   - `Boiler_6` — 4 word (holding 40001)
   - `Boiler_coil_6` — 24 word (coils)

2. **IP котла 6** — в Instance DB блока `modbus_client` для `boiler_server_6`:
   - `InterfaceId`, `ID`, `ConnectionType`, `ActiveEstablished`
   - `RemoteAddress.ADDR[1..4]` — адрес котла 6 (у котла 5 свой набор)

3. **Переименование** — если в проекте осталось имя `server_5_query`, замените на `server_query` (диапазон 1..4).

## Цикл опроса

| `server_query` | Устройство | Данные        | CONNECT           |
|----------------|------------|---------------|-------------------|
| 1              | Котёл 5    | HR 40001, 4   | `boiler_server_5` |
| 2              | Котёл 5    | Coils 1, 24   | `boiler_server_5` |
| 3              | Котёл 6    | HR 40001, 4   | `boiler_server_6` |
| 4              | Котёл 6    | Coils 1, 24   | `boiler_server_6` |

Один экземпляр `MB_CLIENT` — запросы выполняются последовательно; при смене котла меняется только `CONNECT`.
