# Exercise 5 : Security
1. XSS Attack: Happens in the "Notes:" input box of New/Edit person. Typing
 ```html
<html>
	<script>
		alert('xss')
	</script>
</html>
```
 and submit will immediately call the alert on submission.

 This is potentially dangerous because these unescaped code of HTML will be stored on the server, and may disrupt other users who are browsing
the same content.

2. Permissions violation: A user can view any user's contact list by visiting URI `/security/friends/<target user>/`. This eliminates any access control.

	In addition, any user can create a contact for any user provided he's logged in. As long as query parameters match, simply changing `owner_id` can save the contact to another account.

3. Inappropriately trusted input: Putting characters such as `'!@#^@^#%^#'` in the *first name* field while leaving the *last name* blank will trigger a Django exception. This exception page exposes information that attackers may take interest, such as whether `SSL` information. The same goes for leaving both name fields blank. Essentially, no validation was done on these two fields.

	If you try to enter empty strings to already existing users, you'll notice that the webapp allows it.

4. CSRF possibility: The `csrftoken` is exposed in cookie data and was never changed throughout the user session. Potentially, someone can create an endpoint that feeds the token from the victim's cookie and take actions on his behalf. Each individual request should require a different token.

	1. One of the actions is adding new contact. Potentially, somebody can fake a request on the logged-in user's behalf and populate garbage data into his contact list.

	2. The other is contact sharing. Somebody can fake a request to make the victim share its contacts with the attacker.

5. Other General Deficiency: Django's Debug mode is on (Discovered through Inappropriately trusted input). This potentially leaks information.

	The application is also not running under HTTPS protocol. Attackers can spy on the user's network and collect everything that user inputs.
