# 调度指令替代链的执行证据入仓

这是本地演练用的合成来源资料。CSV 使用 UTF-8、有首行字段名，空字段表示未提供。除非字段语义明确为零，空值没有数值含义。

## 资料契约

时间为同日 UTC，所有窗 end 不含。较小 rank 代表更高授权优先级，授权记录未覆盖的指令不形成调度有效期。显式替代不能越过更高授权优先级。撤回实际送达控制器的 at 与仓库记录的 recorded 不同。站内保护可以独立执行，其证据和调度指令分别归类。调度目标单位 MW，meter_energy 按不可分割观测片段给 MWh。动作引用 receipt_id 指实际送达凭证，claim 是人工提供待核实对应，不保证有效。

以下文件位于 `data/`。

- `authority.csv`（4 条）：`wave`、`point`、`desk`、`rank`、`start`、`end`、`rev`
- `commands.csv`（4 条）：`wave`、`command`、`point`、`desk`、`start`、`end`、`target_mw`、`replaces`
- `delivery_receipts.csv`（4 条）：`wave`、`receipt_id`、`command`、`controller`、`kind`、`at`、`recorded`
- `controller_actions.csv`（5 条）：`wave`、`action_id`、`controller`、`point`、`start`、`end`、`target_mw`、`receipt_id`、`purpose`
- `meter_energy.csv`（6 条）：`wave`、`fragment`、`controller`、`start`、`end`、`mwh`
- `operator_intake.csv`（3 条）：`wave`、`record`、`kind`、`point`、`start`、`end`、`action_id`、`receipt_id`

`manifest.json` 记录完整资料包的列名、行数、校验值和跨表引用。完整资料包可含业务冲突或同一来源的多次投递，校验器保留这些原始记录。

## 本地查看

适用 Python 3.11，无需安装第三方库或构建。

```sh
python3 inspect_inputs.py
python3 -m unittest -v test_input_pack
```

查看命令打印各原始表的记录数。单元测试校验资料结构、引用与文件完整性，可离线执行。
