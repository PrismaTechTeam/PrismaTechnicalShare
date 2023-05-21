# Prisma Bootstrap Dashboard Template V1

## Dropdown Menu

**Step 1 :** Dropdown Menu CSS
```css
.dropdown-parent:hover  .ti-chevron-right {
	color:  rgba(93,  102,  237,  0.932);
}

.dropdown-child-panel {
	background-color:  rgb(243,  243,  243);
	width:  100%;
	display:  block;
	border-radius:  10px;
}

.dropdown-parent {
	cursor:  pointer;
}

.dropdown-child {
	width:  100%;
}

.dropdown-child  i {
	margin-left:  20px;
}

.mx-custom-dropdown-icon {
	margin-left:  80px;
	color:  rgb(109,  108,  108);
}

.rotate-icon {
	transform:  rotate(90deg);
}
```
**Step 2:** Import CSS file input html page
```html
<link  rel="stylesheet"  href="/assets/css/Custom-Dropdown.css" />
```

**Step 3:** Add your custom sidebar-item
```html
<li  class="sidebar-item dropdown-parent">
	<div
		class="sidebar-link d-flex align-items-center"
		href="#"
		aria-expanded="false"
	>
		<span>
			<i  class="ti ti-article"></i>
		</span>

		<span  class="hide-menu">Buttons </span>
			<i  class="mx-custom-dropdown-icon ti ti-chevron-right"></i>
	</div>
	<div  class="dropdown-child-panel">
		<ul>
			<li  class="sidebar-item dropdown-child">
			<a
				class="sidebar-link"
				href="./ui-card.html"
				aria-expanded="false"
				>

			<span>
				<i  class="ti ti-cards"></i>
			</span>
			<span  class="hide-menu">Card</span>
			</a>
			</li>

 			<li  class="sidebar-item dropdown-child">
			<a
				class="sidebar-link"
				href="./ui-card.html"
				aria-expanded="false"
				>

			<span>
				<i  class="ti ti-cards"></i>
			</span>
			<span  class="hide-menu">Card</span>
			</a>
			</li>
		</ul>
	</div>
</li>
```

**Step 4:** Enable the dropdown effect
```html
 <script  src="../assets/libs/jquery/dist/jquery.min.js"></script>
<script>

	$(document).ready(function () {

		$(".dropdown-child-panel").hide();
		
		// When dropdown-parent is clicked
		$(".dropdown-parent").click(function () {

			// Toggle the aria-expanded attribute

			$(this).attr("aria-expanded",  function (_,  attr) {
				return  attr  ===  "true"  ?  "false"  :  "true";
		});

		// Toggle the dropdown-child-panel visibility with slide animation
		$(this).find(".dropdown-child-panel").slideToggle();

		// Rotate the mx-custom-dropdown-icon by adding/removing a CSS class
		$(this).find(".mx-custom-dropdown-icon").toggleClass("rotate-icon");

		});

	});

</script>
 ```
