# Smart DNS 去广告规则转换
## 介绍
将部分去广告规则转换为 Smart DNS 适用的版本。
本仓库不定期更新。如果你想自行转换，可以参考下列的转换指南。已适配规则来源：[AdGuard](https://adguardteam.github.io/AdGuardSDNSFilter/Filters/filter.txt)  [AdAway](https://adaway.org/hosts.txt)

## 使用 Visual Studio Code 进行规则转换
本教程涉及正则表达式。如果你想自定义替换规则，请先学习正则表达式。请在操作前将正则表达式搜索打开。
### AdGuard Home 规则
即简单的 AdBlock 规则，包含注释、拦截规则与放行规则，此处只保留拦截规则。

| 查找内容   | 替换内容 | 说明 |
| :--------: | :------: | :--: |
| `^[^\|].*\n` | 无 | 删除所有不支持的标记方式 |
| `^\n` | 无 | 删除所有的空行 |
| `.*\*.*\n` | 无 | 删除所有包含通配符的规则 |
| `\|\|` | `address /` | 替换原规则前缀 |
| `\^` | `/#` | 替换原规则后缀 |
| `([^#]$)` | `$1/#` | 为不以 `^` 结尾的规则添加后缀 |

检查：查找 `^(?!address\s\/).*(?<!\/#)$` 。若无匹配，或只有空行匹配，则转换完成。

### Host 规则

包含 IP 地址与其对应域名。在替换前，请手动删除这两行：

```
127.0.0.1  localhost
::1  localhost
```

|      查找内容      | 替换内容 |           说明           |
| :----------------: | :------: | :----------------------: |
| `^(?!127.0.0.1).*` |    无    | 删除所有不支持的标记方式 |
|       `^\n`        |    无    |      删除所有的空行      |
|   `127.0.0.1\s`    |    `address /`    |      替换原规则前缀      |
|        `\n`        |  `/#\n`  |      替换原规则后缀      |

检查：查找 `^(?!address\s\/).*(?<!\/#)$` 。若无匹配，或只有空行匹配，则转换完成。
