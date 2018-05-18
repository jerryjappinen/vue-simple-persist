# Vue Persist

A mixin for storing component state in localStorage and reload it automatically upon remount. Useful for persisting input values when navigating away from a view, providing extra convenience to users.

Remember: users can flush their localStorage at any time, and other other users won't have the same data there if they load the same URL, so localStorage is no match for URL-based persistence.

## Installation

```sh
npm install vue-simple-persist
```

## Usage

```js
// 1. Import dependency
import VueSimplePersist from 'vue-simple-persist'

export default {
	name: 'my-component',

	// 2. Load the mixin in your component
	mixins: [
		VueSimplePersist
	],

	data () {
		return {
			myValue: ''
		}
	}

	computed: {

		// 3. Add a writable computed
		persist: {

			// This defines which component data is collected and saved
			get () {
				return {
					myValue: this.myValue
				}
			},

			// This is called after component is loaded, if data is found on localStorage
			set (loadedValues) {

				// I can now decide what to do with the data
				if (loadedValues.myValue) {
					this.myValue = loadedValues.myValue
				}

			}

		}

	}

}
```

## Reusable components with separate data

By default, Vue Persist uses the component name as the key to store data. In other words, every time the same component is mounted, it will get the latest data from localStorage, regardless of where it was used or if the data was stored when the user was using another part of the application with the same component.

This works well for e.g. page components that you might use only once, and also for e.g. a feedback form that you want to show in multiple places.

However, you can also reuse the same component without sharing the stored data by specifying a `persistKey` value in your component. Any components with the same `persistKey` value will share their data.

```js
{
	name: 'comment-form',

	props: [
		articleId
	]

	computed: {

		persistKey () {
			return this.$options.name + '-for-article-' + this.articleId
		},

		persist: {

			// ...

		}

	}

}
```
