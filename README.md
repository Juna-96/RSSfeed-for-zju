# RSSfeed-for-zju
# 浙大门户RSS聚合器

该项目提供一个可配置的脚本，用于模拟登录浙江大学多个门户网站，抓取最新通知并生成 RSS 订阅源，方便导入 Folo 等聚合工具。

## 功能概览

- 通过配置文件描述各个站点的登录信息与抓取规则
- 自动维护会话、模拟登录并抓取受保护页面
- 使用 CSS 选择器提取标题、链接、摘要与发布时间
- 生成符合 RSS 2.0 标准的订阅源，可直接导入 Folo

## 快速开始

1. 安装依赖：

   ```bash
   pip install -r requirements.txt
   ```

2. 根据 `config.example.yaml` 创建并修改自己的配置文件，填入浙大各个系统的域名、账号密码以及页面结构的 CSS 选择器。

3. 运行命令生成 RSS（需保证 `src/` 在 `PYTHONPATH` 中）：

   ```bash
   PYTHONPATH=src python -m rssfeed.cli config.yaml --output zju-feed.xml
   ```

   若不指定 `--output`，脚本会将 XML 输出到标准输出，可直接通过管道交给其他工具。

## 配置说明

配置文件使用 YAML 格式，每个站点的关键字段如下：

- `login_url`：登录请求地址
- `content_url`：需要抓取的列表页地址
- `username` / `password`：登录凭证
- `username_field` / `password_field`：登录表单字段名，默认分别为 `username`、`password`
- `extra_login_fields`：登录表单额外字段（如隐藏的 token）
- `success_indicator`：登录成功页面中应包含的关键字，用于提升登录结果判断的准确性
- `selectors`：使用 CSS 选择器描述页面结构，`item` 表示每条通知的容器，其余字段为容器内的元素选择器
- `base_url`：当通知链接为相对路径时用于拼接完整 URL
- `max_items`：每个站点最多抓取的条目数

## 注意事项

- 由于部分站点可能包含验证码或复杂的 SSO 逻辑，脚本仅提供基础登录能力，必要时可通过 `extra_login_fields` 或自定义拓展来适配。
- 请妥善保护账号密码，仅在受信任的环境中运行脚本，并将配置文件设置为合适的权限。
- 如需调度定时生成 RSS，可结合 `cron` 或其他任务调度系统。

## 许可证

本项目以 MIT License 发布，可自由修改与使用。
