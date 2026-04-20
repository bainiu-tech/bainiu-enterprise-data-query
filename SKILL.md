---
name: bainiu-enterprise-data-query
description: 白牛工商信息查询工具。用于查询企业工商信息、股东信息、高管信息等信息。当用户需要查询企业相关信息时触发此技能，包括：(1)企业基本信息查询（工商信息、注册资本、法人代表、注册地址、官方网站等）；(2)高管及股东信息查询；(3)变更信息查询。
---------------------------------------------------------------------------------------------------------------------------------------

# 白牛工商信息查询工具

查询中国大陆企业的工商信息等信息。

**重要说明**：工具列表会持续动态更新，实际可用工具以 `find_tool.js` 查询结果为准。

## **重要**：密钥配置

首次使用先向用户询问密钥并配置API密钥，如若没有API密钥，请前往[白牛商查](https://skill.bainiudata.com/home)申请获取：

1. 复制 `.env.example` 为 `.env(若没有找到.env.example文件则直接创建.env文件)`
2. 填入你的 `BAINIU_API_KEY`

详细配置说明见 [references/config.md](references/config.md)。

## 快速开始

### 1. 查找工具

根据查询意图查找匹配的工具：

```bash
node scripts/find_tool.js "查询意图描述"
```

返回JSON包含：工具ID、描述、参数列表、匹配度评分。

### 2. 调用工具

```bash
node scripts/call.js <tool-id> key1=value1 key2=value2
```

**重要**: 参数必须使用 `key=value` 格式，禁止传入JSON字符串。

## 典型工作流程

### 查询企业高管

> **注意**：以下示例中的工具ID仅供参考，实际ID以 `find_tool.js` 返回结果为准。

```bash
# 步骤1: 模糊搜索获取企业ID
node scripts/find_tool.js "企业模糊搜索"
node scripts/call.js A002 key="小米"

# 步骤2: 使用企业ID查询高管
node scripts/find_tool.js "企业高管查询"
node scripts/call.js A003_A3 entid="<企业ID>" pindex=1 psize=20
```

### 工具复用

同一会话中已调用过的工具，可直接使用其 `tool_id`，无需再次 find\_tool。

## 错误处理

当查询失败或无结果时：

1. **认证失败**：检查API密钥配置
2. **无结果**：先用模糊搜索确认企业ID是否正确
3. **数据为空**：可使用网络搜索作为回退方案

详细错误码说明见 [references/error-codes.md](references/error-codes.md)。

## 参考文档

- **[错误码与故障排除](references/error-codes.md)** - 遇到错误时查阅
- **[工具参考](references/tools-reference.md)** - 了解常用工具类型

