## HTTP cookies

- Cookie is a small peice of data that server sends to a user's web browser.
- Browser may store the cookie and later send it back to the same server with later requests.
- Cookies are used to remember stateful information for stateless HTTP protocol.
- Cookies server mainly three purposes:
	1. Session Management
	2. Personalization
	3. Tracking

### Creating Cookies
- Server can send one or more `Set-Cookie` headers with the response.
- Browser usually store the cookie and send it back to the server inside `Cookie` HTTP header.
- You can sepcify an expiration date or time period after which the cookie shouldn't be sent.
- You can also set additional restrictions regarding domain and path.

### `Set-Cookie` Header
- Response header
- Syntax: `Set-Cookie: <cookie-name>=<cookie-value>`
- Example: `Set-Cookie: yummy_cookie=choco`

### `Cookie` Header
- Request header
- Syntax: `Cookie: <cookie-name>=<cookie-value>; <cookie-name>=<cookie-value>`
- Example: `Cookie: yummy_cookie=choco; tasty_cookie=strawberry`
	
### Lifetime of a Cookie
- Can be defined in two ways:
	1. Session cookies are deleted when the current session ends.
	2. Permanent cookies are deleted at a date specified by the `Expires` attribute, or after a period of time specified by the `Max-Age` attribute.
- Example: `Set-Cookie: id=a3fWa; Expires=Thu, 31 Oct 2021 07:28:00 GMT;`
- *NOTE*: `Expires` date and time is relative to client, not server
	
- If your site authenticates users, it should regenerate and resend session cookies, even ones that already exist, whenever a user authenticates.

### Restrict access to cookies
- You can ensure that cookies are sent securely and aren't accessed by unintended parties or scripts in one of two ways:
	1. `Secure` attribute -- only sent over HTTPS -- never sent on HTTP (except localhost) -- help mitigate man-in-the-middle attack
	2. `HttpOnly` attribute -- a cookie with this attribute set, is inaccessible to JavaScript's `Document.cookie` API -- help mitigate XSS attacks
- Example: `Set-Cookie: id=a3fWa; Expires=Thu, 21 Oct 2021 07:28:00 GMT; Secure; HttpOnly`

### Define where cookies are sent
- `Domain` and `Path` attributes are used for this purpose
- The Domain attribute specifies which hosts can receive a cookie. If the server does not specify a `Domain`, the browser defaults the domain to the same host that set the cookie, excluding subdomains. If `Domain` is specified, then subdomains are always included.
	- For example, if you set `Domain=mozilla.org`, cookies are available on subdomains like`developer.mozilla.org`.
- The Path attribute indicates a URL path that must exist in the requested URL in order to send the Cookie header.
	- For example, if you set `Path=/docs`, these request paths match:
		- `/docs`
		- `/docs/`
		- `/docs/Web/`
		- `/docs/Web/HTTP`
	- But these request paths don't:
		- `/`
		- `/docsets`
		- `/fr/docs`
		
### `SameSite` attribute
- It has three options: Strict, Lax and None
- Default is Lax
- Strict --> the browser only sends the cookie with requests from the cookie's origin site.
- Lax --> is similar to Strict, except the browser also sends the cookie when the user navigates to the cookie's origin site (even if the user is coming from a different site).
- None --> cookies are sent on both originating and cross-site requests, but only in secure contexts i.e. Cookies with SameSite=None must also specify the Secure attribute (they require a secure context).

### Cookie prefixes
- `__Host-` --> If a cookie name has this prefix, it's accepted only if it's also marked with the `Secure` attribute -- was sent from a secure origin -- does not include a `Domain` attribute -- and has a `Path` set to `/`.
- `__Secure-` --> If a cookie name has this prefix, it's accepted only id it's marked with the `Secure` attribute -- was sent from a secure origin.

### First-Party vs Third-Party Cookies
- If the cookie domain and scheme match the current page, the cookie is considered to be from the same site as the page, and is referred to as a first-party cookie.
- If the domain and scheme are different, the cookie is not considered to be from the same site, and is referred to as a third-party cookie.