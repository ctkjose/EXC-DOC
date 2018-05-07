

JavaScript

When displaying or hidding elements update aria-expanded / aria-hidden state attributes as required.

aria-haspopup
aria-control="id"


```html
<li role="presentation" tabindex="0">
			<a href="item-1.html" role="menuitem" id="nav-item-1" aria-haspopup="true">Item 1</a>
			<ul role="menu" aria-expanded="false" aria-hidden="true" aria-labelledby="nav-item-1">
				<li role="presentation"><a href="item-1-1.html" role="menuitem">Item 1.1</a></li>
				<li role="presentation"><a href="item-1-2.html" role="menuitem">Item 1.2</a></li>
				<li role="presentation"><a href="item-1-3.html" role="menuitem">Item 1.3</a></li>
			</ul>
		</li>
```


## References ##

[Steve Faulkner](http://html5doctor.com/on-html-belts-and-aria-braces/)

[BUTTONS](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques/Using_the_button_role)
