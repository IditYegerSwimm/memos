---
title: The first doc
---
This is the first doc in this repo. It says nothing at all. I'll add a few snippet and tokens now.

<SwmSnippet path="/plugin/idp/oauth2/oauth2.go" line="18">

---

This code defines a struct called `IdentityProvider` that represents an OAuth2 Identity Provider. It contains a `config` field of type `*store.IdentityProviderOAuth2Config` which holds the configuration details for the identity provider.

```go
// IdentityProvider represents an OAuth2 Identity Provider.
type IdentityProvider struct {
	config *store.IdentityProviderOAuth2Config
}
```

---

</SwmSnippet>

&nbsp;

![](/.swm/images/Screen%20Shot%202023-04-11%20at%2016.44.41-2024-1-5-13-14-47-30.jpg)These firemen are <SwmToken path="/plugin/idp/oauth2/oauth2.go" pos="95:16:16" line-data="	if v, ok := claims[p.config.FieldMapping.Identifier].(string); ok {">`FieldMapping`</SwmToken>. Good job guys!

That's it, 5 minutes has passed.

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBbWVtb3MlM0ElM0FJZGl0WWVnZXJTd2ltbQ==" repo-name="memos"><sup>Powered by [Swimm](https://swimm-web-app.web.app/)</sup></SwmMeta>
