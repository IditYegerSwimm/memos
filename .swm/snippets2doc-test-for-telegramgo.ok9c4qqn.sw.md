---
title: Snippets2Doc test for telegram.go
---
# Introduction

This document will walk you through the implementation of the Telegram integration feature in our application. This feature allows our application to interact with Telegram, a popular messaging app, by sending and receiving messages, and handling various types of content.

We will cover:

1. How we retrieve the bot token for the Telegram integration.


2. How we handle different types of content from Telegram messages.


3. How we generate a keyboard for memo ID in Telegram.

# Retrieving the bot token

The first step in integrating with Telegram is to retrieve the bot token. This token is essential for authenticating our application with the Telegram API. We store this token as a workspace setting in our application.

<SwmSnippet path="/server/integration/telegram.go" line="36">

---

In the BotToken function in server/integration/telegram.go, we attempt to retrieve this setting. If the setting exists and there are no errors, we return the value of the setting, which is the bot token. If there is an error or the setting does not exist, we return an empty string.

```go
func (t *TelegramHandler) BotToken(ctx context.Context) string {
	if setting, err := t.store.GetWorkspaceSetting(ctx, &store.FindWorkspaceSetting{
		Name: apiv1.SystemSettingTelegramBotTokenName.String(),
	}); err == nil && setting != nil {
		return setting.Value
	}
	return ""
}
```

---

</SwmSnippet>

# Handling different types of content

Telegram messages can contain different types of content, such as text, captions, and links. We need to handle these different types of content appropriately to ensure that our application can process Telegram messages correctly.

<SwmSnippet path="/server/integration/telegram.go" line="81">

---

In server/integration/telegram.go, we check if the message contains text, a caption, or a link. If the message contains text or a caption, we convert it to Markdown format using the convertToMarkdown function. If the message contains a link, we append it to the content.

```go
	if message.Text != nil {
		create.Content = convertToMarkdown(*message.Text, message.Entities)
	}
	if message.Caption != nil {
		create.Content = convertToMarkdown(*message.Caption, message.CaptionEntities)
	}
	if message.ForwardFromChat != nil {
		create.Content += fmt.Sprintf("\n\n[Message link](%s)", message.GetMessageLink())
	}
```

---

</SwmSnippet>

# Generating a keyboard for memo ID

In Telegram, we can create a custom keyboard with buttons that users can click to perform actions. In our application, we use this feature to create a keyboard for memo ID.

<SwmSnippet path="/server/integration/telegram.go" line="216">

---

In the generateKeyboardForMemoID function in server/integration/telegram.go, we create a keyboard with buttons for each visibility level (public, protected, and private). Each button's callback data is a string that contains the visibility level and the memo ID. This allows us to handle the user's button click appropriately based on the visibility level and the memo ID.

```go
func generateKeyboardForMemoID(id int32) [][]telegram.InlineKeyboardButton {
	allVisibility := []store.Visibility{
		store.Public,
		store.Protected,
		store.Private,
	}

	buttons := make([]telegram.InlineKeyboardButton, 0, len(allVisibility))
	for _, v := range allVisibility {
		button := telegram.InlineKeyboardButton{
			Text:         v.String(),
			CallbackData: fmt.Sprintf("%s %d", v, id),
		}
		buttons = append(buttons, button)
	}

	return [][]telegram.InlineKeyboardButton{buttons}
}
```

---

</SwmSnippet>

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBbWVtb3MlM0ElM0FJZGl0WWVnZXJTd2ltbQ==" repo-name="memos"><sup>Powered by [Swimm](https://swimm-web-app.web.app/)</sup></SwmMeta>
