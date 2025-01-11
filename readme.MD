## 为什么？

Gemini API 是[免费的](https://ai.google.dev/pricing "limits applied!"),
而且许多工具专门与 OpenAI API 配合使用。

该项目免费提供个人 OpenAI 兼容端点。

## 没有服务器怎么办？

因为它运行在云端，而且不需要服务器维护。它都可以轻松地免费部署到各种提供商（！无法翻译！机器翻译后：具有适合个人使用的慷慨限制）。

> [!TIP]
> ！无法翻译！机器翻译后: 在本地运行代理端点也是一种选择，
> 尽管它更适合开发使用。


## 如何开始呢？

访问google获取 [API key](https://makersuite.google.com/app/apikey).

> [!IMPORTANT]
> 即使您位于其他国家请查看[支持的区域](https://ai.google.dev/gemini-api/docs/available-regions#available_regions),
> 仍然可以使用 VPN 获取一个。（无法提供VPN地址）

使用以下项目部署到提供商之一。您需要在那里登录/创建一个帐户。

如果您选择“按钮部署”，系统将引导您首先完成分叉存储库的过程，这是持续集成 (CI) 所必需的。


### 使用 Vercel 进行部署

 [![Vercel部署](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/PublicAffairs/openai-gemini&repository-name=my-openai-gemini)
-或者可以部署到 [命令行](https://vercel.com/docs/cli):
  `vercel 部署`
- Serve locally: `vercel dev`
- Vercel _Functions_ [limitations](https://vercel.com/docs/functions/limitations) (with _Edge_ runtime)


### Deploy to Netlify

[![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/PublicAffairs/openai-gemini&integrationName=integrationName&integrationSlug=integrationSlug&integrationDescription=integrationDescription)
- Alternatively can be deployed with [cli](https://docs.netlify.com/cli/get-started/):
  `netlify deploy`
- Serve locally: `netlify dev`
- Two different api bases provided:
  - `/v1` (e.g. `/v1/chat/completions` endpoint)  
    _Functions_ [limits](https://docs.netlify.com/functions/get-started/?fn-language=js#synchronous-function-2)
  - `/edge/v1`  
    _Edge functions_ [limits](https://docs.netlify.com/edge-functions/limits/)


### Deploy to Cloudflare

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/PublicAffairs/openai-gemini)
- Alternatively can be deployed manually pasting content of [`src/worker.mjs`](src/worker.mjs)
  to https://workers.cloudflare.com/playground (see there `Deploy` button).
- Alternatively can be deployed with [cli](https://developers.cloudflare.com/workers/wrangler/):
  `wrangler deploy`
- Serve locally: `wrangler dev`
- _Worker_ [limits](https://developers.cloudflare.com/workers/platform/limits/#worker-limits)


### Deploy to Deno

See details [here](https://github.com/PublicAffairs/openai-gemini/discussions/19).


### Serve locally - with Node, Deno, Bun

Only for Node: `npm install`.

Then `npm run start` / `npm run start:deno` / `npm run start:bun`.


#### Dev mode (watch source changes)

Only for Node: `npm install --include=dev`

Then: `npm run dev` / `npm run dev:deno` / `npm run dev:bun`.


## How to use
If you open your newly-deployed site in a browser, you will only see a `404 Not Found` message. This is expected, as the API is not designed for direct browser access.
To utilize it, you should enter your API address and your Gemini API key into the corresponding fields in your software settings.

> [!NOTE]
> Not all software tools allow overriding the OpenAI endpoint, but many do
> (however these settings can sometimes be deeply hidden).

Typically, you should specify the API base in this format:  
`https://my-super-proxy.vercel.app/v1`

The relevant field may be labeled as "_OpenAI proxy_".
You might need to look under "_Advanced settings_" or similar sections.
Alternatively, it could be in some config file (check the relevant documentation for details).

For some command-line tools, you may need to set an environment variable, _e.g._:
```sh
OPENAI_BASE_URL="https://my-super-proxy.vercel.app/v1"
```
_..or_:
```sh
OPENAI_API_BASE="https://my-super-proxy.vercel.app/v1"
```


## Models

Requests use the specified [model] if its name starts with "gemini-", "learnlm-", 
or "models/". Otherwise, these defaults apply:

- `chat/completions`: `gemini-1.5-pro-latest`
- `embeddings`: `text-embedding-004`

[model]: https://ai.google.dev/gemini-api/docs/models/gemini


## Media

[Vision] and [audio] input supported as per OpenAI [specs].
Implemented via [`inlineData`](https://ai.google.dev/api/caching#Part).

[vision]: https://platform.openai.com/docs/guides/vision
[audio]: https://platform.openai.com/docs/guides/audio?audio-generation-quickstart-example=audio-in
[specs]: https://platform.openai.com/docs/api-reference/chat/create

---

## Supported API endpoints and applicable parameters

- [x] `chat/completions`

  Currently, most of the parameters that are applicable to both APIs have been implemented,
  with the exception of function calls.
  <details>

  - [x] `messages`
      - [x] `content`
      - [x] `role`
          - [x] "system" (=>`system_instruction`)
          - [x] "user"
          - [x] "assistant"
          - [ ] "tool" (v1beta)
      - [ ] `name`
      - [ ] `tool_calls`
  - [x] `model`
  - [x] `frequency_penalty`
  - [ ] `logit_bias`
  - [ ] `logprobs`
  - [ ] `top_logprobs`
  - [x] `max_tokens`, `max_completion_tokens`
  - [x] `n` (`candidateCount` <8, not for streaming)
  - [x] `presence_penalty`
  - [x] `response_format`
      - [x] "json_object"
      - [x] "json_schema" (a select subset of an OpenAPI 3.0 schema object)
      - [x] "text"
  - [ ] `seed`
  - [x] `stop`: string|array (`stopSequences` [1,5])
  - [x] `stream`
  - [x] `stream_options`
      - [x] `include_usage`
  - [x] `temperature` (0.0..2.0 for OpenAI, but Gemini supports up to infinity)
  - [x] `top_p`
  - [ ] `tools` (v1beta)
  - [ ] `tool_choice` (v1beta)
  - [ ] `parallel_tool_calls`

  </details>
- [ ] `completions`
- [x] `embeddings`
- [x] `models`
