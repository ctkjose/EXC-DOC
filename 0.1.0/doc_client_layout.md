

## Headers ##

The CSS class ```.exc-app-header``` creates a header in the app layout.

```html
<div class='exc-app-layout'>
	<header class='exc-app-header'>

	</header>
</div>
```

The following attributes allow you to modify the look and behavior of your header.

| Attributes | Type | Description |
| -- | -- | -- |
| is-fixed | CSS Class | Attaches header to top of screen. |
| with-shadow | CSS Class | Adds a shadow under a ```fixed``` header. |


## A container ##

Use the container to separate parts of your layout that will include other layout sections.

The CSS class ```exc-app-container``` defines a container.


The following example uses the ```exc-app-container``` to place a **drawer** and a content **panel** side by side.

```html
<div class='exc-app-layout'>
	<header class='exc-app-header'></header>
	<div class='exc-app-container'>
		<section class='exc-app-drawer'></section>
		<section class='exc-app-panel'></section>
	</div>
</div>
```

By default the parts inside an ```exc-app-container``` are arranged in columns side-by-side, but you can add the modifier class ```with-rows``` to arrange its content one under the other.


## Menus ##

```html
<body class="exc-app-layout drawer exc-theme-light">
	<header class="exc-app-header is-fixed with-shadow">
		A header
	</header>
	<div class="exc-app-container" data-view="body">
		<section class="exc-app-drawer" data-view="drawer" role="navigation">
			<nav class='exc-app-menu with-border' role="menubar" data-view="menu" aria-label="Applications Menu Bar">
				<div role='menu' arial-label="A menu header">Menu Header</div>
				<a role='menuitem' title="Test Menu 1"><i class="la la-search"></i>Test Menu 1</a>
				<a role='menuitem' title="Test Menu 2">Test Menu 2</a>
				<a role='menuitem' title="Test Menu 3">Test Menu 3 <i class='exc-arrow'></i></a>
				<a role='menuitem' class="is-expanded" title="Test Menu 3">Test Menu 3 <i class='exc-arrow'></i></a>
			</nav>
		</section>
		<section class="exc-app-panel" data-view="main">
			Panel...
		</section>
	</div>
</body>
```

The following attributes allow you to modify the look and behavior of your menubar.

| Attributes | Type | Description |
| -- | -- | -- |
| with-border | CSS Class | A menu bar has borders. |


The following attributes allow you to modify the look and behavior of a menu item.

| Attributes | Type | Description |
| -- | -- | -- |
| is-disabled | CSS Class | A menu item is disabled |
| is-selected | CSS Class | A menu item is in the selected state. |
