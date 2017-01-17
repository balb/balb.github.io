---
layout: post
title:  "OAuth 2.0 server-to-server authentication"
---

I was tasked with adding [OAuth 2.0](https://tools.ietf.org/) authentication to an ASP.NET Web API service.

OAuth 2.0 provides a bunch of different grant types. After a bit of googling it looked like
the [Client Credentials Grant](https://tools.ietf.org/html/rfc6749#section-4.4) (sometimes called "two-legged OAuth") was the way to go.
[Google use it](https://developers.google.com/identity/protocols/OAuth2ServiceAccount) for their APIs.

There is a [OWIN OAuth 2.0 Authorization Server](https://www.asp.net/aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server) tutorial 
that has some examples but looked to be full of stuff I didn't need.

This is the minimal setup I needed based on the tutorial for an authorization server and to add authentication to Web API controllers via [Authorize] attributes.

{% highlight C# %}

public void Configuration(IAppBuilder app)
{
	// Setup Authorization Server
	app.UseOAuthAuthorizationServer(new OAuthAuthorizationServerOptions
	{
		TokenEndpointPath = new PathString(Paths.TokenPath),
#if DEBUG
		AllowInsecureHttp = true,
#endif
		// Authorization server provider which controls the lifecycle of Authorization Server
		Provider = new OAuthAuthorizationServerProvider
		{
			OnValidateClientAuthentication = ValidateClientAuthentication,
			OnGrantClientCredentials = GrantClientCredetails
		}
	});

	// Setup Resource Server i.e. the bit that checks the access token header when requesting web methods with the Authorize attribute
	app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions());
}

private Task ValidateClientAuthentication(OAuthValidateClientAuthenticationContext context)
{
	string clientId;
	string clientSecret;
	if (context.TryGetBasicCredentials(out clientId, out clientSecret) ||
		context.TryGetFormCredentials(out clientId, out clientSecret))
	{
		if (clientId == "MyClientId" && clientSecret == "MySecret")
		{
			context.Validated();
		}
	}

	return Task.FromResult(0);
}

private Task GrantClientCredetails(OAuthGrantClientCredentialsContext context)
{
	var identity = new ClaimsIdentity(new GenericIdentity(context.ClientId, OAuthDefaults.AuthenticationType),
		context.Scope.Select(x => new Claim("urn:oauth:scope", x)));

	context.Validated(identity);

	return Task.FromResult(0);
}

{% endhighlight %}