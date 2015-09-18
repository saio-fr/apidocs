# SAIO Javascript API

## Getting started

With the SAIO Javascript API you can easily add specific behaviors and customize your visitors' experience of the SAIO web application.

You can choose when and where your visitors can see the SAIO app, register visitor informations or trigger custom behaviours.

You need the latest SAIO snippet code to use the Javascript API. You can find it under http://lily.saio.fr/config (you have to be logged in as an admin to get there)

For `saio.config` API calls, place them just before `saio.load` in the snippet code:

``` js
// ⟶ insert saio.config calls here
saio.load('548053c9a18e2');
```

## Support

If you have a problem with an API call, such as an incorrect return value or an intended action not performing, email contact@saio.fr. (If you can provide a link with your code, it will be easier for us to test the functionality and help you in the best delays)

## API Functions

### Widget and box behaviour

#### Show the Widget `saio.api('widget.show')`

Shows the widget.

Note: The widget is shown by default on every page that has the snippet code, but if it was hidden (for example using the `widget.hide` method, you can show it with `widget.show`.

#### Hide the Widget `saio.api('widget.hide')`

Hides the widget. The widget won't display on that page. Can be undone using `widget.show`.
You can invoke it before or after page load.

example:

``` js
// Hide the widget on the home page
if (window.location.href.indexOf('home')) {
	saio.api('widget.hide');
}
```

#### Open the app `saio.api('box.expand')`

Opens the app. This has the same effect as clicking on the widget.
You can use it to create your own *click to open* button.

The app will open in its starting state (the starting state depends on how you configured the app in the admin config section at http://lily.saio.fr/config) if it is openned for the first time. Otherwise it will take the state it was in before being closed.

example (assuming you use jQuery, but can also be done using vanilla javascript):

``` html
<button class="saio-click-to-open"></button>

<script>
$('.saio-click-to-open').on('click', function() {
	saio.api('box.expand');
});
</script>
```

#### Close the app `saio.api('box.shrink')`

Closes the app. Will show the widget (the widget is the default clickable button for openning the app. If the app is closed, we show the widget).

Similarly you can make your own *click to close* button:


``` html
<button class="saio-click-to-close"></button>

<script>
$('.saio-click-to-close').on('click', function() {
	saio.api('box.shrink');
});
</script>
```

#### On widget show `saio.api('widget.onShow', function)`

Will call a given callback function whenever the widget is shown:

``` js
saio.api('widget.onShow', function() {
	// your callback function
	alert('the widget was shown');
});
```

#### On widget hide `saio.api('widget.onHide', function)`

Will call a given callback function whenever the widget is hidden:

``` js
saio.api('widget.onHide', function() {
	// your callback function
	alert('the widget was hidden');
});
```

#### On app expand `saio.api('box.onExpand', function)`

Will call a given callback function whenever the app expands:

``` js
saio.api('box.onExpand', function() {
	// your callback function
	alert('the app was oppenned');
});
```

#### On app shrink `saio.api('box.onShrink', function)`

Will call a given callback function whenever the app shrinks:

``` js
saio.api('box.onShrink', function() {
	// your callback function
	alert('the app was closed');
});
```

### App configuration

#### Set operator group `saio.config('chat.setOperatorGroup', string)`

Sets the preferred operator group for a chat conversation on this page. If an operator member of this group is available to chat, incomming visitor chats on this page will be affected to him.

Pass the group id as a parametter.

Note: the group id can be found by clicking on a group in the group configuration page at http://lily.saio.fr/users#groups (you have to be logged in as an admin to get there)

#### Identify user `saio.config('identify', object)`

If you have access to your end user's name and email on the page ( for example you have an authentification system and the user is logged in), you can use `identify` to pass user details into your SAIO account.

This helps SAIO give you more detailed and contextual informations about your users, and allows your chat operators to access this information while chatting with an identified user.

Required fields: `email`, `name`

Supported fields:
- `externalId` (**string**): a unique Id for that user in your system
- `custom`: (**object**) an object with custom properties you want to associate with that user (ex: `location`, `lang`)

example:

``` js
saio.config('identify', {
	name: 'John Doe',
	email: 'john@doe.com',
	externalId: '123456'
});
```
