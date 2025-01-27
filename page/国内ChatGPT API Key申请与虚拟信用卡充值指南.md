## 一、ChatGPT API简介

最近，OpenAI推出了一项付费计划，允许 ChatGPT 和 Whisper API 的使用。前者使企业能够在其应用、网站和服务中集成 ChatGPT 的功能，而 Whisper API 则提供语音转文本的服务。

ChatGPT API 的用途非常广泛，简而言之，它可以让企业或个人将 ChatGPT 的功能直接嵌入到自己的产品中。以每千个 tokens 仅需 $0.002 的价格，显得非常有吸引力。

ChatGPT API 的推出，有点像当年乔布斯推出Apple应用商店，让全球的开发者可以轻松接入人工智能接口，连接与 ChatGPT 平台。

## 二、申请ChatGPT账号及充值步骤

本文假设你已经注册了 OpenAI 的账号。如果尚未注册，请参考这篇 [《Chat GPT官方推荐新手教程》](https://chatgpt-plus.github.io/chatgpt-plus/) 中的第1步，申请一个账号。注册时需借助接码平台和 Depay 虚拟信用卡，成本在 $1 以下，并支持支付宝。

充值时，登录 OpenAI 网站，点击右上角的账号信息 -> Billing，设置付款方式，并绑定刚申请的虚拟信用卡信息以进行 ChatGPT API 的充值。

## 三、开始使用ChatGPT API

注册完成后，登录界面将会显示相关选项。点击右上角的 "View API keys" -> "Create new secret key"，生成您自己的 API 密钥。请注意，由于安全原因，这个 API 密钥只会显示一次，请在关闭对话框前将其复制并妥善保存。

您还可以在左侧的 Usage 栏中查看 token 使用情况，数据每5分钟更新一次。每位新用户注册后可获赠 $18 的免费 token 使用额，但需注意，免费额度有时间限制，一旦过期将无法使用。

使用 ChatGPT API 非常简便，您可以尝试用简单的 curl 命令进行测试，记得将 $OPENAI_API_KEY 替换为您自己的 API 密钥。

OpenAI 还提供了各类编程语言的实现代码，例如 Python，只需导入 openai 包，使用您申请的 API 密钥和最新的模型 gpt-3.5-turbo 即可。

## 四、ChatGPT API使用常见问题解析

### 1. GPT-3.5-turbo模型

GPT-3.5 是 OpenAI 提供的最强文本生成模型，代号“turbo”标识其优化版本，响应速度更快。GPT-4 计划于今年发布，并预计将更强大。

### 2. ChatGPT API速度

使用反馈显示，ChatGPT API 的响应速度较快，明显改善了用户体验。

### 3. 充值方式

如前所述，国内用户需通过 Depay 虚拟信用卡进行充值，具体操作可参考第二章的官方指导文档。

### 4. Token收费标准

官方的收费标准为 $0.002/1K tokens，使用时需注意，发送响应文本会消耗较多的 tokens，尤其在连续会话中。

### 5. Token的连续会话能力

ChatGPT API 能够支持连续对话，但每次请求历史对话内容时，会增加 token 的费用。

### 6. 使用地区限制

有反馈指出国内用户访问API可能会遇到不稳定情况，建议考虑使用国际IP。

### 7. 关于套壳APP和网站

随着 API 的发布，市场上可能会出现许多套壳应用，需谨慎分辨，确保选择官方渠道。

### 8. Whisper API

除了 ChatGPT API，OpenAI 还推出了 Whisper API，用于语音转文字，按需收费，价格为每分钟 $0.006。

## 五、ChatGPT Plus是否还需续费？

不少用户疑惑 ChatGPT API 发布后是否还需续费 ChatGPT Plus。实际上，ChatGPT API 的使用更偏向于开发者，其使用门槛较高。而且，API 的 token 费用实际上并不便宜，因此对于普通用户，继续使用 Plus 可能更为划算。

## 六、如何升级到ChatGPT Plus？

👉 [野卡 | 一分钟注册，轻松订阅海外线上服务](https://bit.ly/bewildcard)