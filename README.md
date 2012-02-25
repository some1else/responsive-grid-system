# Responsive Grid System

A *responsive* grid system in `SASS`. Lets you define **contexts** and refer to them when specifying sizes for elements.

## Usage

This is how basic usage looks in `.sass`. Of course, only the syntax changes for `.scss`.

	@import responsive_grid_system

	@media (max-width: 480px)
		+uses_grid(small, 100%, 12, 2%)
		// context name, grid width, number of columns, gutter width

	@media (min-width: 480px) and (max-width: 960px)
		+uses_grid(medium, 100%, 12, 5%)

	@media (min-width: 960px) and (max-width: 1200px)
		+uses_grid(screen, 960px, 12, 20px)

	@media (min-width: 1200px)
		+uses_grid(huge, 1140px, 12, 40px)

	body
		+container
		h1
			+col(12) // in all contexts
			+hide(handheld) // only hidden in handheld context
		p
			+col(6, medium, screen) // half the layout size in a few contexts
			+col(4, huge) // a third of the layout size in a specific context
			+col(12, handheld) // only full width in handheld context

Check out the source for the public API

## Implementation details

Responsive Grid System uses `@extend` instead of `@mixin` internally, which compacts the code by concatenating selectors to only define a given style once:

Input:

	.element
		+col(2)

	.another
		+col(2)

	.andanother
		+col(4)

Becomes:

	.element, .another, .andanother {
		float: left;
		display: inline-block;
	}
	.element, .another {
		width: 200px;
	}
	.andanother {
		width: 400px;
	} 

While there have been many sound arguments for defining style outside of the HTML (unlike most grid systems, which require adding class names to the DOM), the generated stylesheets were mostly inflated due to constant repetition of the grid styles. Use of `@extend` successfully mitigates the impact of generated code on file size.


## Status

* Working on simplifying @media exceptions anywhere in the selector hierarchy
* Perparing test, demo, docs
* This is currently a proof of concept, not production-quality code. It has not been tested in old browsers.