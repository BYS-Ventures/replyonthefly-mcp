# ReplyOnTheFly MCP server

Writes replies to customer reviews on Google, Yelp, Facebook, TripAdvisor and Trustpilot, and short messages asking a customer for a Google review. It is a hosted remote server: nothing to install, no account, no API key.

| | |
|---|---|
| URL | `https://www.replyonthefly.com/mcp` |
| Transport | Streamable HTTP |
| Authentication | None |
| MCP Registry name | `com.replyonthefly/replyonthefly` |

This repository holds the documentation. The server itself is hosted and run by [ReplyOnTheFly](https://www.replyonthefly.com).

## Configure

### VS Code (GitHub Copilot)

Add the server to `.vscode/mcp.json` in your workspace, or to your user `mcp.json`:

```json
{
  "servers": {
    "replyonthefly": {
      "type": "http",
      "url": "https://www.replyonthefly.com/mcp"
    }
  }
}
```

Then open Copilot Chat in agent mode and make sure the ReplyOnTheFly tools are enabled.

### Claude Code

```sh
claude mcp add --transport http replyonthefly https://www.replyonthefly.com/mcp
```

### Gemini

On [gemini.google.com](https://gemini.google.com), open **Settings > Connected Apps** and click **+ Custom**. Paste the URL, click **Next**, accept the notice and click **Connect**. In a chat, type **@ReplyOnTheFly**, pick it, and paste the review. Google limits custom apps to personal Google accounts in the US, aged 18 and over, in English, with Keep Activity on. You add them on the web; they also work in the Gemini mobile app.

### Other MCP clients

Add a remote MCP server with the URL `https://www.replyonthefly.com/mcp`, transport Streamable HTTP, and no authentication. Setup steps for each app are also at [replyonthefly.com/ai-connector](https://www.replyonthefly.com/ai-connector).

## Use it

Paste a review into the chat and ask for a reply. For example:

- Write a reply to this 1-star Google review of my restaurant: …
- Help me respond to an angry customer review without sounding defensive
- Make it shorter and sign it from Maria
- Write a text I can send my customer Dana asking for a Google review

Example output. Prompt:

> Help me respond to this 2-star review of Oak & Pine Furniture without sounding defensive: "Nobody answered the phone all week and my order still isn't here."

Reply:

> I'm really sorry your experience with us was frustrating and disappointing. We take responsibility for this and would like to sort it out, please reach out to us directly so we can look into your order and make things right. I hope we can resolve this for you soon.

## Tools

Both tools are read-only: they return text and change nothing anywhere.

### `write_review_reply`

Writes a reply that fits the star rating: a thank-you for happy customers, and for unhappy ones a calm reply that acknowledges the problem without arguing and offers to make it right. Healthcare replies never confirm that the reviewer is a patient.

| Input | Required | Description |
|---|---|---|
| `business_name` | yes | Name of the business being reviewed |
| `star_rating` | yes | Stars the customer gave, 1 to 5 |
| `review_text` | | The review as the customer wrote it (up to 2,000 characters) |
| `reviewer_name` | | The reviewer's display name, if shown |
| `business_type` | | For example "dentist", "pizza restaurant", "plumber" |
| `tone` | | `friendly` (default), `professional`, `casual` or `enthusiastic` |
| `length` | | `concise` (1 to 2 sentences), `standard` (2 to 3, default) or `detailed` (3 to 4) |
| `platform` | | `google` (default), `yelp`, `facebook`, `tripadvisor` or `trustpilot` |
| `owner_notes` | | Up to 5 facts or wishes from the owner, such as "we fixed the AC last week" or "sign off as Maria" |
| `alternate` | | `true` for a differently worded version of a reply already written |

Returns the reply as text, plus structured content with `reply`, `star_rating`, `business_name` and `platform`.

### `write_review_request_message`

Writes up to three short texts or emails asking a customer for a Google review, worded for the type of business, with the review link at the end. They never offer a discount or reward, because Google does not allow incentives for reviews.

| Input | Required | Description |
|---|---|---|
| `business_name` | yes | Name of the business asking for the review |
| `business_type` | | For example "plumber", "hair salon", "restaurant" |
| `customer_first_name` | | The customer's first name |
| `channel` | | `text` (default) or `email` |
| `review_link` | | The business's Google review link (`https://...`). Without it, the messages use a placeholder and explain where to find the link. |

## What it does not do

- Post anything. You copy the reply and paste it under the review yourself.
- Send texts or emails. You send the review request yourself.
- Write reviews, fake or otherwise.
- Connect to your Google Business Profile or need a ReplyOnTheFly account.

## Privacy

The server receives only the tool inputs for each request. It does not store the review, the names, the review link or the reply. Replies are generated with OpenAI's API; review request messages come from templates and are not sent to OpenAI. Full details: [replyonthefly.com/privacy#ai-apps](https://www.replyonthefly.com/privacy#ai-apps).

## Support

Email [support@replyonthefly.com](mailto:support@replyonthefly.com).

ReplyOnTheFly is operated by BYS Ventures LLC.
