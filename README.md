# 百度贴吧 MCP | Baidu Tieba MCP

LobeHub 用户请使用 [LobeHub setup / 接入说明](LOBEHUB.md)，包含专属官网入口、认证和来源配置。

SocialDataX / 社媒数据助手 provides a hosted, read-only MCP service for public Baidu Tieba data. This repository contains connection metadata and client examples.

## Connection

- Endpoint: `https://mcp.socialdatax.com/baidu-tieba/mcp`
- Transport: `streamable-http`
- API Key: set `SOCIALDATAX_API_KEY` and send `Authorization: Bearer <SOCIALDATAX_API_KEY>`.
- Website and API Key access: <https://socialdatax.com/ai?from=github>
- Registry name: `com.socialdatax/baidu-tieba`
- Local metadata: `0.1.1`, 19 tools including the shared points balance tool.
- Hosted discovery: `0.1.1`, 19 tools, verified on 2026-10-16.
- Official Registry: [`0.1.1`](https://registry.modelcontextprotocol.io/v0.1/servers/com.socialdatax%2Fbaidu-tieba/versions/0.1.1), verified active/latest on 2026-10-16. Public repository and third-party directory status are verified separately.

## Capabilities

Search public threads and users, get search suggestions, read thread details, comments and nested replies, and explore creator profiles, posts, followed forums and forum threads.

The service does not provide account login, posting, editing, liking, following or writing comments.

## Tools

| Tool | Purpose |
| --- | --- |
| `baidu_tieba_get_forum_posts` | 按准确吧名浏览普通帖和置顶帖，无需关键词，支持分页，支持排序或仅查看精华帖。  精华帖仅支持默认顺序，不提供分类选项。按关键词查找吧内帖子使用 baidu_tieba_search_posts_in_forum； 只查询贴吧简介和统计使用 baidu_tieba_get_forum_detail。摘要和媒体可能不完整。 |
| `baidu_tieba_get_forum_detail` | 按准确吧名获取贴吧简介、标语、建吧日期、关注人数、帖子统计和友情链接贴吧。  不知道准确吧名时，先用 baidu_tieba_search_suggestions 获取 forum_name； 按关键词查找吧内帖子时，使用 baidu_tieba_search_posts_in_forum；无需关键词浏览时使用 baidu_tieba_get_forum_posts。 |
| `baidu_tieba_search_suggestions` | 获取贴吧搜索框的吧名联想词和相关贴吧信息，不搜索帖子或用户，不支持分页。  items 与 forums 为两组独立结果，不保证一一对应；可将 forums 中的 forum_name 传给 baidu_tieba_get_forum_detail 查询贴吧详情，或传给 baidu_tieba_search_posts_in_forum 按关键词搜索吧内帖子。 |
| `baidu_tieba_search_users` | 按关键词查找百度贴吧用户，返回精确匹配和相关用户的单页结果，不支持分页。  结果中的 profile_url 可用于读取用户资料或用户发帖；已有用户主页链接时，直接使用相应的用户资料或发帖工具。 |
| `baidu_tieba_search_general` | 按关键词获取百度贴吧综合搜索中的帖子，保留帖子顺序，支持分页。  不返回吧、用户、话题或搜索建议。需要最新、最早、相关排序时使用帖子搜索； 指定帖子链接或 ID 使用详情工具。当前页没有帖子时，仍以 next_page_token 判断能否续页。 图片、视频仅来自搜索结果，可能不完整，无法据此判断媒体总数。 |
| `baidu_tieba_search_posts_in_forum` | 按准确贴吧名称和关键词搜索该吧主题帖，支持最新或相关排序及分页，不搜索评论。  不限定贴吧时使用全站帖子搜索；返回摘要及可能不完整的图片和视频。 |
| `baidu_tieba_get_user_info_by_profile_url` | 通过百度贴吧用户主页链接读取资料、签名、粉丝数、关注数、累计获赞、吧龄和 IP 属地。  已有 profile_url 时优先使用本工具；链接可取搜索、帖子详情或评论作者的同名字段。 仅有数字用户 ID 时使用 baidu_tieba_get_user_info_by_user_id；不返回发帖或回复列表。 |
| `baidu_tieba_get_user_info_by_user_id` | 仅有百度贴吧数字用户 ID 时读取用户资料及主页统计；已有 profile_url 时优先使用 baidu_tieba_get_user_info_by_profile_url。 |
| `baidu_tieba_get_user_posts_by_user_id` | 按数字用户 ID 读取该用户发布的主题帖，支持分页，不包含回复活动，不提供排序。  已有主页链接时优先使用 baidu_tieba_get_user_posts_by_profile_url；列表及媒体不保证完整，空令牌表示本次可读取列表结束。 |
| `baidu_tieba_get_user_posts_by_profile_url` | 按主页链接读取该用户发布的主题帖，支持分页，不包含回复活动，不提供排序。  已有 profile_url 时优先使用本工具；仅有数字用户 ID 时使用 baidu_tieba_get_user_posts_by_user_id。 列表及媒体不保证完整，空令牌表示本次可读取列表结束。 |
| `baidu_tieba_get_user_followed_forums_by_user_id` | 按数字用户 ID 读取用户公开可读取的关注贴吧列表，支持分页，返回贴吧 ID、名称、头像、链接和用户在吧内的等级。  已有 profile_url 时优先使用 baidu_tieba_get_user_followed_forums_by_profile_url；不查询关注的用户或用户发帖，未提供列表时返回错误。 |
| `baidu_tieba_get_user_followed_forums_by_profile_url` | 按主页链接读取用户公开可读取的关注贴吧列表，支持分页，返回贴吧 ID、名称、头像、链接和用户在吧内的等级。  已有 profile_url 时优先使用本工具；仅有数字用户 ID 时使用 baidu_tieba_get_user_followed_forums_by_user_id。 不查询关注的用户或用户发帖，未提供列表时返回错误。 |
| `baidu_tieba_search_posts` | 按搜索词搜索百度贴吧公开帖子，支持分页令牌和排序；需要综合搜索中的帖子顺序时使用综合搜索，指定帖子链接使用详情工具；需要限定贴吧时使用 baidu_tieba_search_posts_in_forum。图片、视频仅来自搜索结果，可能不完整。 |
| `baidu_tieba_get_post_detail_by_thread_id` | 按主题帖数字 ID 读取百度贴吧详情；不包含完整回复列表。 |
| `baidu_tieba_get_post_detail_by_url` | 按百度贴吧主题帖链接读取详情；不包含完整回复列表。 |
| `baidu_tieba_get_post_comments_by_thread_id` | 按主题帖数字 ID 读取百度贴吧一级评论，支持排序和分页，不包含楼中楼回复。 |
| `baidu_tieba_get_post_comments_by_url` | 按百度贴吧主题帖链接读取一级评论，支持排序和分页，不包含楼中楼回复。 |
| `baidu_tieba_get_post_comment_replies` | 按主题帖 ID 和一级评论 ID 读取楼中楼回复，支持分页，不提供排序。  comment_id 取一级评论列表的 comment_id；读取帖子一级评论使用评论列表工具。 |
| `socialdatax_get_points_balance` | Query the current API Key account's SocialDataX points balance. |

For tools whose schema includes `page_token`, leave it empty on the first request and pass the returned `next_page_token` unchanged on the next request. Keep the same target, search terms and filters. Search suggestions do not return posts or user details. Use IDs and URLs returned by the service; do not derive them from display names.

## Client examples

- [Streamable HTTP](examples/streamable_http_config.json)
- [Codex](examples/codex_config.toml)
- [Cursor](examples/cursor_mcp.json)
- [Claude Desktop fallback](examples/claude_desktop_config.json)

The command/stdio fallback in [mcp.json](mcp.json) uses `mcp-remote`. Direct HTTP clients should use the hosted endpoint.

## Verification scope

2026-10-16 authenticated production checks returned 9 search items, a successful thread detail and 10 items on the next search page. Comment retrieval for the sampled thread returned service_failure; that call is not counted as a successful check. Comment replies and full history coverage were not verified.

## Metadata

[server-card.json](server-card.json) contains the tool descriptions and input schemas. The implementation is privately hosted. License: MIT for the public documentation and connection examples.
